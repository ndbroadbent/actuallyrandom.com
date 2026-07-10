---
title: "Setting up and publishing a new Hugo blog in about 10 minutes using Claude Code"
date: 2026-07-10T17:30:00+12:00
draft: false
description: "I gave Claude Code an empty directory and a domain name. About ten minutes of agent work later, a Hugo blog was live on GitHub Pages with Cloudflare DNS, HTTPS and Google Analytics."
slug: "setting-up-and-publishing-a-new-hugo-blog-in-about-10-minutes-using-claude-code"
tags:
  - Hugo
  - Claude Code
  - GitHub Pages
  - Cloudflare
  - AI coding
categories:
  - AI
  - Web development
keywords:
  - set up Hugo blog with Claude Code
  - Hugo GitHub Pages custom domain
  - publish Hugo blog GitHub Actions
  - Claude Code website setup
  - Hugo PaperMod setup
---

At the start of this project, `~/code/actually_random` was an empty directory. I had bought ActuallyRandom.com, but there was no Hugo site, no theme, no GitHub repository and no DNS configuration.

I opened Claude Code and described what I wanted. Roughly ten minutes of active agent work later, [actuallyrandom.com](https://actuallyrandom.com/) was serving a Hugo blog over HTTPS. It had a theme, search, archives, RSS, Google Analytics and an automatic GitHub Pages deployment on every push to `main`.

I did not spend those ten minutes copying YAML from documentation or working out which GitHub Actions needed which permissions. I mostly watched Claude do it.

That needs a few qualifications. I already owned the domain, had Claude Code installed and was logged into GitHub, Cloudflare and 1Password on my computer. I also had a Google Analytics measurement ID ready. Buying the domain and setting up those accounts are not included in the ten minutes.

## The prompt

This was the initial request, lightly shortened:

> I just bought ActuallyRandom.com. I want to publish a new Hugo blog on this domain via GitHub Pages. This is a brand new empty directory. Please install the latest version of Hugo, set it up with an appropriate theme, configure the custom domain and tell me how to name the GitHub repository and push the initial version. I will also be setting up Google Analytics.

I then pasted in the Google Analytics tag.

That was enough for Claude Code to inspect my Mac, find the installed tools, upgrade Hugo and start building. This kind of task suits a coding agent well because it crosses several boundaries. Claude needed to edit files, run Hugo, use Git, call the GitHub API, change DNS records and inspect the final website. A chat response could explain all of that. Claude Code could actually do it.

## What Claude chose

Claude upgraded Hugo from 0.151.2 to 0.164.0 Extended, which was the latest release. It created the site and installed [PaperMod](https://github.com/adityatelange/hugo-PaperMod) as a Git submodule.

I had not asked for PaperMod specifically. Claude chose it because it is a mature blog theme with responsive layouts, automatic dark mode, client-side search and useful SEO defaults such as Open Graph, Twitter Cards and Schema.org metadata. It also has no JavaScript build dependency. That felt like the right amount of infrastructure for a blog whose main design requirement was "please let me publish random things."

Claude created or configured these files:

```text
hugo.yaml
.github/workflows/hugo.yml
.gitignore
static/CNAME
archetypes/posts.md
content/archives.md
content/search.md
content/posts/hello-world.md
themes/PaperMod/          # Git submodule
```

The Hugo configuration set the real base URL, enabled `robots.txt`, added HTML, RSS and JSON home outputs, and configured PaperMod's search and navigation. It also enabled reading times, breadcrumbs, previous and next post links, code-copy buttons and a table of contents.

Claude wired the GA4 measurement ID into Hugo's Google Analytics service, built the site, and checked the generated HTML to confirm that the tag was present. It told me that PaperMod only emits the tag on production builds, so `hugo server` would not pollute my analytics with my own local page views. It wrote that claim into a comment in `hugo.yaml` as well.

The claim was wrong, and it was wrong because of something Claude had done a few lines further down the same file.

PaperMod decides whether to emit analytics with this condition:

```go-html-template
{{- if hugo.IsProduction | or (eq site.Params.env "production") }}
{{- partial "google_analytics.html" . }}
```

`hugo.IsProduction` is false under `hugo server`, which is the mechanism that is supposed to keep development traffic out of the stats. But Claude had also set `env: production` under `params` in `hugo.yaml`. That is a common line to see in PaperMod configs, and it short-circuits the check. With it present, the right-hand side of that `or` is always true, so the tag fired on every local preview.

I only found this while fact-checking a later article, when I asked Claude to prove the production-only claim rather than repeat it. Starting `hugo server` and grepping the response found the measurement ID sitting there twice. Deleting the `env: production` line fixed it. A development server now emits the tag zero times, a production build emits it once, and Open Graph and Schema.org metadata still render in production, because those sit behind the same condition.

It is a small bug with an annoying property: nothing looks broken. The site builds, the tag appears where you check for it, and the only symptom is quietly inflated analytics.

The first local build produced 17 pages in about 90 milliseconds. Static site generators are slightly ridiculous in the best possible way.

## Publishing through GitHub Actions

Hugo generates the site into `public/`, but that directory is ignored by Git. GitHub Actions builds a fresh copy instead. This follows the deployment method in the [official Hugo GitHub Pages guide](https://gohugo.io/host-and-deploy/host-on-github-pages/).

The workflow checks out the repository and its theme submodule, installs the required Hugo version, runs a minified production build, uploads `public/` as a Pages artifact and deploys it. It has the `pages: write` and `id-token: write` permissions required by GitHub's [`deploy-pages` action](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).

Claude suggested naming the repository `actuallyrandom.com`. The name is descriptive, but it is not required. An Actions-based project site with a custom domain can use any repository name. I already had `ndbroadbent.github.io` hosting another blog, so I briefly wondered whether that would conflict.

It does not. GitHub can host a user site and multiple project sites at the same time. A request for `www.actuallyrandom.com` reaches GitHub Pages, then GitHub uses the requested hostname to select the repository that owns that custom domain. My existing site at `ndbroadbent.github.io` remains separate.

I created the public [`ndbroadbent/actuallyrandom.com`](https://github.com/ndbroadbent/actuallyrandom.com) repository in GitHub and gave Claude the SSH URL. Claude pushed the initial commit and watched the workflow.

It failed.

## The first deployment failed

The workflow attempted to enable GitHub Pages automatically with `enablement: true`. The repository's workflow token did not have the repository-admin permission needed to create the Pages site, so the first run stopped after 12 seconds.

This was the most useful part of the whole exercise. Claude did not hand me a red workflow and suggest five things to try. It opened the failed run, found the permission problem, created the Pages configuration using my authenticated `gh` credentials, selected the workflow build type and ran the deployment again.

The second run succeeded. Future pushes do not need admin credentials because the Pages site now exists and the workflow only has to build and deploy it.

The official instructions make the prerequisite explicit: select GitHub Actions as the Pages source before the first deployment. Trying to hide that setup inside the workflow saved no time in this case. It created one failed run and a good paragraph for this article.

## Configuring Cloudflare without opening the dashboard

I had bought the domain through Cloudflare. I gave Claude the zone and account identifiers and pointed it at a Cloudflare API token stored in 1Password.

The first credential lookup failed because two 1Password accounts were connected and there were similarly named Cloudflare items. Claude listed matching item titles without reading or printing their secret fields, selected the intended personal account and tried again. The working token never appeared in the transcript.

Claude then created the DNS records through the Cloudflare API:

| Type | Name | Destination |
| --- | --- | --- |
| A | `@` | GitHub Pages' four IPv4 addresses |
| AAAA | `@` | GitHub Pages' four IPv6 addresses |
| CNAME | `www` | `ndbroadbent.github.io` |

The exact A and AAAA values are listed in [GitHub's custom-domain documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site). GitHub recommends configuring both the apex domain and `www`; it can then redirect one to the other automatically.

The records were left in Cloudflare's DNS-only mode. That lets DNS return GitHub's addresses directly and leaves GitHub in charge of HTTPS. Cloudflare's proxy can be useful, but this site did not need a second CDN, cache or TLS layer in front of GitHub Pages.

Claude attached `actuallyrandom.com` to the Pages site, waited for GitHub to issue a certificate, enabled HTTPS enforcement and tested both hostnames. `www.actuallyrandom.com` redirected to the apex domain, and HTTP redirected to HTTPS.

One small correction to the generated repository: Claude created `static/CNAME`, which causes Hugo to copy a `CNAME` file into the published site. GitHub's current documentation says that an existing `CNAME` file is ignored when a site is published through a custom Actions workflow. It is harmless, but the repository setting and DNS records are doing the real work.

## Verifying the domain

The blog was already live, but I later added GitHub's domain-verification TXT record through Cloudflare. GitHub recommends doing this before attaching a custom domain, although doing it afterward still protects the domain.

[Domain verification](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages) prevents another GitHub user from claiming the domain for their own Pages site if my repository is deleted or Pages becomes disconnected while the DNS records remain. The verification applies to the apex domain and its immediate subdomains. GitHub says to leave the TXT record in place permanently.

This step was not part of getting the site online, but it took very little extra work and is worth doing for any custom domain that will remain pointed at GitHub Pages.

## What the ten minutes produced

By the end, I had:

- a working Hugo site using PaperMod;
- a custom domain with apex and `www` routing;
- HTTPS with automatic certificate management;
- deployment on every push to `main`;
- search, archives, tags, RSS, sitemap and `robots.txt`;
- a production-only Google Analytics tag;
- a reusable post archetype and local preview workflow;
- a verified GitHub Pages domain.

The main configuration and deployment work took roughly ten minutes of active Claude time according to the timing lines in the transcript: 4 minutes 19 seconds for the local setup, then about 6 minutes to push, configure DNS, recover from the failed workflow and verify the live site. There were pauses between prompts while I created the repository and supplied account details.

So no, this was not ten minutes from "I have never heard of Hugo" to a live site with no accounts or domain. It was ten minutes from an empty, prepared project directory to a deployed site. That is still a task that would normally consume a decent part of an afternoon, especially once DNS and GitHub Actions became involved.

## Writing posts now

The day-to-day workflow is pleasantly boring:

```bash
hugo new content posts/my-random-idea.md
hugo server -D
```

The first command creates a draft using the post archetype. The second starts a local server and includes draft content. When the post is ready, I set `draft: false`, commit it and push to `main`. GitHub normally publishes the update within about a minute.

There is one Hugo trap worth remembering. This site has `buildFuture: false`, so a post dated even a few minutes ahead of the current time silently disappears from the production build. I managed to hit this with a later article by setting its time to 18:00 when it was only 17:07. The build succeeded, but the post was not there. If a new Hugo post refuses to appear, check its date before debugging anything more exotic.

## What Claude Code changed about the job

None of the individual steps was difficult. Hugo has good documentation. GitHub publishes an official Actions workflow. Cloudflare has an API. The annoying part is joining them together, carrying values between systems and checking that the final result works.

Claude Code handled that connective work. It could read the repository, change several files, run builds, use authenticated command-line tools, watch a workflow, inspect DNS and request the live page. When the first approach failed, it still had enough context to diagnose the error and finish the job.

I would not let it publish blindly. The unnecessary `CNAME` file is a mild example. The analytics bug is a better one, and it is worth being precise about what went wrong there. Claude did not hallucinate an API or write code that failed to compile. It wrote a plausible, conventional line of configuration, then described the behaviour that line was supposed to produce instead of the behaviour it actually produced. Every individual piece was reasonable. The result was a site quietly counting me as a visitor.

The fix came from asking for evidence rather than a restatement. Starting the development server and grepping for the measurement ID takes about fifteen seconds and settles the question completely. An agent will happily check that sort of thing when asked, and will happily not check it when not asked.

More importantly, an agent with access to GitHub, DNS and stored credentials can make real changes very quickly. I read the proposed DNS records, approved the public repository and checked the final site.

But I did not need to remember the current GitHub Pages workflow format, manually create nine DNS records or spend twenty minutes wondering why the first deployment token lacked permission. I described the outcome and stayed around to make decisions.

Ten minutes later, I had a blog.

## Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview).
- [Hugo: Host on GitHub Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/).
- [GitHub: Using custom workflows with GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages).
- [GitHub: Managing a custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).
- [GitHub: Verifying a custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages).
- [PaperMod on GitHub](https://github.com/adityatelange/hugo-PaperMod).
- [The Actually Random source repository](https://github.com/ndbroadbent/actuallyrandom.com).
