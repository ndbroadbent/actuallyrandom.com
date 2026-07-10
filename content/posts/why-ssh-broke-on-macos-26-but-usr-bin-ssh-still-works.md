---
title: "Why ssh stopped working on macOS 26, but /usr/bin/ssh still works"
date: 2026-07-10T18:00:00+12:00
draft: false
description: "Homebrew's ssh could not reach my home server and returned No route to host. DNS was fine, the server was fine, and the real cause was macOS Local Network privacy blocking one binary but not another."
slug: "why-ssh-broke-on-macos-26-but-usr-bin-ssh-still-works"
tags:
  - macOS
  - ssh
  - Homebrew
  - networking
  - debugging
categories:
  - Tech
keywords:
  - ssh No route to host macOS
  - macOS local network permission ssh
  - homebrew ssh no route to host
  - macOS 26 local network privacy
  - EHOSTUNREACH local network macOS
---

I have a home server called `reef`. I have been connecting to it with `ssh reef` for a long time. Today it stopped.

```
❯ ssh root@10.5.7.35
ssh: connect to host 10.5.7.35 port 22: No route to host
```

Then, in the same terminal, one second later:

```
❯ /usr/bin/ssh root@10.5.7.35
Welcome to Ubuntu 22.04 LTS (GNU/Linux 6.8.12-10-pve x86_64)
root@reef:~#
```

Same host. Same shell. Same second. One works and one does not. I have never seen anything like it, and it took a while to believe what I was looking at.

## Everything that looked broken was fine

`No route to host` sounds like a networking failure, so I started where anyone would.

DNS was healthy. Both of my internal resolvers answered correctly, and `reef.hs` resolved to `10.5.7.35` from each of them. Public names resolved too. Nothing had changed in my DNS configuration, and nothing needed to.

The server was healthy. It answered ICMP in under three milliseconds, its ARP entry was present, and I could pull its SSH banner straight off the wire with a tool that was never blocked:

```
❯ curl -s telnet://10.5.7.35:22
SSH-2.0-OpenSSH_8.9p1 Ubuntu-3
```

So `sshd` was listening, reachable, and introducing itself politely, at the exact moment `ssh` insisted there was no route to it.

The rest of my network was fine as well. I had convinced myself that Proxmox was down, because `https://proxmox.home.ndbroadbent.com/` would not load in Chrome. From the command line it returned HTTP 200 immediately. Chrome was the thing that was broken, not Proxmox.

At this point I had spent a while suspecting my router, my DNS server and the Terraform repository that manages my home infrastructure. All three were innocent.

## The pattern

The clue was that `curl` worked, `ping` worked, and `ssh` did not. Those are different programs. So I stopped testing hosts and started testing binaries.

| Binary | Internet | My LAN |
| --- | --- | --- |
| `/usr/bin/ssh` | works | works |
| `/usr/bin/curl` | works | works |
| `/usr/bin/nc` | works | works |
| `/bin/bash` (`/dev/tcp`) | works | works |
| `/opt/homebrew/bin/ssh` | works | **No route to host** |
| `/opt/homebrew/bin/bash` | works | **No route to host** |
| `/opt/homebrew/opt/curl/bin/curl` | works | **fails** |
| `/opt/homebrew/bin/node` | works | **EHOSTUNREACH** |

Homebrew's `ssh` will happily authenticate to GitHub over the public internet. It cannot open a TCP connection to a machine on my own network. Apple's `ssh` does both.

That is not a routing problem. Routes do not know which binary is asking.

## macOS decides this per executable

macOS gained a Local Network privacy control in Sequoia, the one that asks whether an app may "find and connect to devices on your local network". I assumed, reasonably I thought, that this was a per-application setting, and that my terminal either had it or did not. iTerm had it. That did not help.

To test whether the permission really is granted per binary, I wrote a short C program that opens a TCP connection to an address and prints the result. I compiled it, checked that it was ad-hoc signed like any freshly built local binary, and ran it:

```
  10.5.7.10:8006 -> No route to host
  140.82.121.4:22 -> CONNECTED
```

A program I had written thirty seconds earlier could reach GitHub and could not reach a machine in the next room. There was no prompt, no dialog, and no entry appeared in System Settings. It simply failed.

That is the whole mechanism. Local network access is evaluated for the executable making the call. Binaries that ship with macOS are signed by Apple as platform binaries and are exempt. Anything else is denied unless it has been granted access, and a command line tool has no way to display the approval prompt, so it does not get asked. The syscall returns `EHOSTUNREACH`, and `EHOSTUNREACH` is printed as `No route to host`.

Which is a genuinely unfortunate error string, because it names the one thing that is not wrong.

## The part I cannot explain

Homebrew's `python3` reaches my LAN without complaint. I checked that it was really connecting and not being redirected somewhere harmless:

```
local: ('10.5.7.199', 65012)   peer: ('10.5.7.10', 8006)
```

That is a genuine socket to a genuine machine. Meanwhile `python3.10` and `python3.11`, installed by the same Homebrew, in the same directory, both fail with `[Errno 65] No route to host`.

So `python3.14` is holding a grant that its siblings do not have. Permissions really are stored per binary, and at least one of mine was approved at some point without my remembering it. I could not find where that grant lives. It is not in `TCC.db`:

```
❯ tccutil reset LocalNetwork
tccutil: Failed to reset LocalNetwork

❯ sudo tccutil reset LocalNetwork
tccutil: Failed to reset LocalNetwork
```

That is not a permissions failure, it is a category error. I dumped the list of services in the TCC database and `kTCCServiceLocalNetwork` is not among them. Local network permission is handled by `nehelper`, somewhere I have not located, and `tccutil` has no idea it exists. So the standard advice for resetting a macOS privacy prompt does not apply here at all.

If you know where these grants are stored, I would like to hear about it.

## Every diagnostic tool lies to you

The reason this took so long is that the failure is invisible to the tools you reach for first.

`ping` works, because ICMP is not gated. DNS works, because your queries are answered by `mDNSResponder`, which is Apple's. `curl` works, because it is Apple's. `nc` works, because it is Apple's. Each of those tells you the network is fine, which is true, and none of them is the program you are trying to run.

Worse, the tools can lie in the other direction. Early on I probed my LAN with a shell loop using `/dev/tcp`, and my shell is Homebrew's `bash`. Every host came back dead. I briefly believed that my Proxmox server, my reverse proxy and my home server had all failed simultaneously. They were all running perfectly. I was holding a blocked binary and asking it whether the network was up.

If you take one thing from this, take that: when you are debugging local network access on a recent macOS, run your probes with `/bin/bash` and `/usr/bin/curl`, or you will be reading fiction.

## What to do about it

The quickest fix is to stop using the blocked binary. Apple ships a perfectly good OpenSSH, and it is exempt:

```bash
alias ssh='/usr/bin/ssh'
```

If Homebrew's `openssh` arrived as an incidental dependency rather than a deliberate choice, `brew uninstall openssh` makes `ssh` resolve to `/usr/bin/ssh` and the problem disappears for good.

Before that, open System Settings, go to Privacy and Security, then Local Network, and scroll past the applications. Individual command line tools can appear in that list. If `ssh` or `bash` is there with the switch off, turning it on is the real fix rather than a workaround. My `python3` almost certainly got its grant this way.

And if a machine on your network appears to be down, check it with `curl` before you check it with anything else. Mine was up the entire time.

## Postscript

None of this was a bug in my server, my DNS, my router or my infrastructure code. I spent a while inspecting all four. The actual change was that a privacy feature quietly denied a permission to a binary that could not ask for it, and reported the denial using the error message for a completely different failure.

I do not know exactly when it broke. When I finally logged in with Apple's `ssh`, the server told me my last successful login from this laptop was on 22 June. Something between then and now moved: an operating system update, or a Homebrew rebuild of `openssh` that gave the binary a fresh ad-hoc signature, or both. I am not going to pretend I can distinguish them after the fact.

What I can say is that nothing announced it. It simply stopped, on an ordinary Friday, in the middle of trying to do something else.
