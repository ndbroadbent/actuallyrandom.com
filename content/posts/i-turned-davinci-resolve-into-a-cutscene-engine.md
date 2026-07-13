---
title: "I turned DaVinci Resolve project files into a cutscene engine"
date: 2026-07-12T21:00:00+12:00
draft: false
description: "I needed a 36 second intro cutscene for a Godot game. Instead of rendering a video, I edited it in DaVinci Resolve, exported the project file, and found out it was a zip full of XML I could compile straight into the game."
slug: "i-turned-davinci-resolve-into-a-cutscene-engine"
tags:
  - game dev
  - Godot
  - DaVinci Resolve
  - cutscenes
categories:
  - Tech
keywords:
  - davinci resolve project file format
  - drt file format
  - godot cutscene
  - resolve timeline xml
  - ken burns effect godot
  - non-linear editor as authoring tool
---

I was building the intro for a match-3 game called MatchCraft, made in Godot. Thirty-six seconds: eight painted slides that slowly push in and cross-dissolve into each other, a narrator over the top, ambient sound underneath, subtitles fading in and out on the beat of the voice.

The obvious way to ship that is to render a video and play it. I did not want a video. So I ended up using DaVinci Resolve, a professional video editor, as the authoring tool for a cutscene that never becomes a video at all. The game reads the edit out of Resolve's project file and replays it live.

Here is how that works.

## Why not just render a video

It is one resolution. A phone and a Retina iPad are wildly different pixel counts, and a video baked for one looks soft or letterboxed on the other. The native version loads the full-resolution stills and lets Godot scale them to whatever the device actually is, with mipmapped linear filtering, re-laying out the whole screen when you rotate the device. The intro is sharp on hardware I have never tested it on.

Its text is burned into the pixels. I wanted subtitles to be real `Label` nodes, so they stay crisp at any size, and so a future me can translate them without re-rendering anything.

And a video is a black box. I wanted a skip button that fades out cleanly, audio that ducks properly when you leave, a clock the rest of the game can pause. All of that is awkward around a video player and trivial around native nodes.

I should be honest about the one thing this does not buy me, because it is the thing people assume. It did not save space. The raw stills and the field recordings come to about 56 MB, and a compressed 36 second video would have been smaller. I did this for fidelity and control, not for bytes.

## The part nobody wants to do

The catch is that once you rule out a video, you have to time the whole thing yourself. This slide starts pushing in now. Cross-dissolve to the next one over 1.08 seconds. The narrator's second line comes in at 6.8 seconds and runs for 2.8. An owl call at 10.2. A subtitle at 1.4, gone by 4.

Doing that by editing numbers in a script and re-launching the game is miserable. You cannot see the audio waveform, so you are guessing where the narrator pauses. You cannot scrub. You change a number, wait for the game to boot, watch, decide it was 200 milliseconds late, change it again. I have built timed sequences that way before and it feels like tuning a radio with oven mitts on.

Then it occurred to me that there is an entire category of software built to do exactly this, and it is very good at it.

## A video editor is a timing tool

A non-linear editor is a machine for placing media on a timeline and getting the timing right by feel. That is the whole job. Resolve gives me a video track to drop the stills onto, keyframed transforms for the slow push-in on each one, a dissolve I can drag to any length between two clips, an audio track where I can see the narrator's waveform and razor it into pieces exactly on the pauses, and more tracks underneath for the ambient beds. I can scrub the playhead across all of it in real time and feel whether a cut lands.

So I edited the cutscene in Resolve as if I were cutting a short film. Nine AI-painted stills on the video track, a slow zoom on each, dissolves between them. The narrator, an ElevenLabs voice I named Raven, on one audio track, cut into six lines. Ambient recordings underneath: wind in trees, a village afternoon, a tawny owl, a fire, a stream.

Then, instead of hitting render, I saved the project and exported the timeline.

## What a .drt file actually is

A DaVinci Resolve Timeline exports as a `.drt` file. Open one in a text editor and it looks like binary garbage. It is not. It is a zip archive. The first two bytes are `PK`, the signature Phil Katz put on every zip file in 1989.

Unzip it and the edit is right there in plain XML:

```
project.xml
MediaPool/Master/MpFolder.xml
SeqContainer/cec4de82-6f1b-4f24-891d-ad8038209ffe.xml
```

That `SeqContainer` file is the timeline. Every clip is an element with a handful of fields, and only four of them matter to me:

```xml
<Start>86593</Start>
<Duration>170</Duration>
<In>...</In>
<Position>1</Position>
<MediaFilePath>.../evening-sound-effect-in-village.mp3</MediaFilePath>
```

`Start` is where the clip sits on the timeline. `In` is the in-point into its source media, so a razor cut becomes an offset. `Duration` is how long it plays. `Position` is which track it is on. Transitions are their own elements, `Sm2TiTransition`. Audio clips are `Sm2TiAudioClip`. That is the entire edit, as text, that I can read with any XML parser.

## The one hour that is not there

Here is the detail that made me grin. My cutscene is 36 seconds long, but every `Start` value in the file is a number around 86,400.

Resolve starts its timelines at one hour: `01:00:00:00`. This is a broadcast convention older than the software, from the tape era, when you left an hour of leader at the head of a program for pre-roll and bars and tone. Nobody threads tape anymore, but the hour of empty space at the front of the timeline survived into the file format.

At 24 frames per second, one hour is 3,600 times 24, which is 86,400 frames. So to turn a Resolve frame number into "seconds since my cutscene actually starts," you subtract 86,400 and divide by the frame rate. That subtraction is sitting in my game code, exactly as literal as it sounds:

```gdscript
const FPS := 24.0
const TIMELINE_START_FRAME := 86400.0

const INTRO_SFX_EVENING_START := (86593.0 - TIMELINE_START_FRAME) / FPS
const INTRO_SFX_EVENING_SOURCE_START := 215.0 / FPS
const INTRO_SFX_EVENING_DURATION := 170.0 / FPS
```

That evening-village ambience starts at frame 86593, which is 193 frames past the hour mark, which is 8.04 seconds into the intro. Every cue in the cutscene is a line like that: a start, an in-point, a duration, each one a frame count from the XML divided by 24.

## Compiling a timeline into code

So the pipeline is: edit in Resolve, export the `.drt`, unzip it, read the `SeqContainer` XML, and for every clip pull its three numbers. Divide by the frame rate. Emit them as constants.

The runtime side is deliberately small. It is a clock, a pile of timers, and a two-slot crossfade. At each start time it fires a callback: dissolve to the next slide, play a narration line, trigger an ambient bed, show a subtitle. There is a comment in the file that says exactly what these numbers are:

```gdscript
func _schedule_sound_effect_cues() -> void:
	# These timings are read directly from the Resolve timeline XML.
	...
```

Playing an audio segment is just seeking into the source file at the in-point and stopping after the duration, which maps one-to-one onto Resolve's `In` and `Duration`:

```gdscript
func _on_play_audio_segment(player, source_start_seconds, duration_seconds):
	player.play(source_start_seconds)
	_schedule_timer(duration_seconds, _on_stop_audio_segment.bind(player))
```

The nice thing is how much fell out of the format for free.

The narrator is a single audio file that I razored into six lines in Resolve. In the game that is one audio player and six `play(from_seconds)` calls at six different in-points. The cuts I made in the editor became source offsets in the code without my doing anything.

The ambient reuse works the same way. One "wind in trees" recording appears three times across the timeline, at three different in-points, so it never sounds like the same loop twice. In the engine that is one loaded stream scheduled three times. Resolve reusing a clip is the engine reusing a stream.

The cross-dissolve becomes the oldest trick in film editing. Two clips overlap on the track with a dissolve between them, and I replay that as an A-B roll: prepare the hidden slot with the next image, start its slow zoom, fade the old slot out and the new one in together, swap which slot is live. Ten lines of tween.

And the push-in that continues across a cut, so a zoom carries through a dissolve instead of resetting, was a keyframed transform in Resolve. In code it is one slide whose zoom starts where the previous one ended, rather than at 1.0.

## It is compiled, not played

I want to be precise about what the game ships, because "reads the project file" could imply the wrong thing.

The game does not contain a Resolve runtime and does not parse `.drt` files when it launches. The timeline is a source file, the way a `.psd` or a `.blend` is a source file. It lives in the repo at 44 KB as the editable master. The shipped app contains the baked constants and the raw slides and audio, and nothing else. Resolve is the level editor, the `.drt` is the source format, and the build step turns one into the other.

I will also be honest that the build step is not yet a clean one-click importer. I parsed the XML and derived the constants with a script rather than wiring a proper importer into the Godot editor. The obvious next move is to write that as an `EditorImportPlugin` so a `.drt` dropped into the project round-trips to cues automatically, and re-cutting the intro means re-exporting from Resolve and nothing else. Right now re-cutting means re-exporting and re-running the parse. That is a fair limitation. It has still been worth it, because the hard part, the timing, happens in the tool built for timing.

## The short version

If you need a scripted, timed multimedia sequence, a cutscene, an onboarding flow, a kiosk loop, and you are hand-timing it by editing numbers in code, stop and reach for a video editor instead. Timing is a visual, physical act in an NLE and a guessing game in a text file.

You do not have to render to a video to get the benefit. Author the sequence in the editor, export the project, and read the timeline out of it. The project file is almost always more open than it looks. Resolve's is a zip of XML, and the edit inside it is four fields per clip: where it starts, where it starts inside its source, how long it runs, and which track it is on. That is enough to rebuild the whole thing natively, at full resolution, with live text and a working skip button.

Subtract the hour first.

## Sources and notes

- A DaVinci Resolve `.drt` (Timeline) export is a zip archive. Unzipping mine gives `project.xml`, `MediaPool/Master/MpFolder.xml`, and a `SeqContainer/<uuid>.xml` that holds the timeline, with per-clip `<Start>`, `<In>`, `<Duration>`, `<Position>` and `<MediaFilePath>` fields, plus `<Sm2TiTransition>` and `<Sm2TiAudioClip>` elements. These field names are from my own file; Blackmagic does not publish a schema for the format.
- The `01:00:00:00` timeline start is a broadcast convention from the tape era, carried into most NLEs. At 24 fps it is frame 86,400, which is why every `Start` in a 36 second timeline is a five-figure number. The subtraction `(86593.0 - 86400.0) / 24.0` in the intro code converts a timeline frame back to elapsed seconds.
- The slides were generated with Google Gemini, the narration with ElevenLabs, and the ambiences are field recordings. The narrator is one file cut into six lines; the shipped intro reuses single ambient recordings at multiple in-points, exactly as they were placed in the Resolve timeline.
- The engine side is Godot 4 GDScript. The cutscene runs on a `GameClock` abstraction so it can be paused and tested headlessly, which is the same reason the timing is expressed as constants rather than magic numbers scattered through the code.

