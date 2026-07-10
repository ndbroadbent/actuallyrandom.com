---
title: "How to make AI sound less like AI"
date: 2026-07-10T17:05:00+12:00
draft: false
description: "I use AI to help write ActuallyRandom.com. The open source Humanizer skill removes the habits that make useful AI writing sound like ChatGPT."
slug: "how-to-make-ai-sound-less-like-ai"
tags:
  - AI writing
  - Humanizer
  - Claude Code
  - agent skills
  - blogging
categories:
  - AI
keywords:
  - how to make AI sound less like AI
  - Humanizer skill
  - remove AI writing patterns
  - humanize AI writing
  - Claude Code Humanizer
---

I use AI to help write ActuallyRandom.com. Quite a lot, in fact.

I am not ashamed of this. The whole point of the site is to give me somewhere to put random ideas and experiment with AI, SEO, ads and whatever else catches my attention. The bat article began with something my wife heard on Russian TikTok. The Adrien Brody article began because she was watching *Chapelwaite* after we ate Thai food. Those ideas came from us. AI helped me research them and turn them into articles.

I still read every article before publishing it. I check the sources, remove things I do not agree with and change sentences that sound wrong. I do not publish a post unless I am relatively happy with it.

There was still a problem. The writing sounded like AI.

## Once you notice AI writing, you see it everywhere

The em dashes were the most obvious sign. ChatGPT loves them. So do Claude and most other writing models. A sentence begins normally, takes a dramatic sideways turn, and then returns with suspiciously perfect punctuation.

There were other habits too. Every article had a neat introduction, several evenly sized sections and a conclusion that restated the point. Ideas arrived in groups of three. Ordinary facts became "important reminders" of something bigger. Sentences used words such as *pivotal*, *landscape*, *showcase* and *underscore* even when *is* would have worked.

Then there are the phrases that are hard to unsee:

> It is not just about X. It is about Y.

> This serves as a powerful reminder that...

> In today's rapidly evolving landscape...

> The result? A more seamless, efficient and intuitive experience.

None of these constructions is inherently wrong. Humans use them. The problem is the cluster. Put enough of them into one article and the author starts to sound like the same cheerful corporate chatbot that wrote half of LinkedIn.

I searched for a way to edit these habits out and found [Humanizer](https://github.com/blader/humanizer), an open source skill created by `blader`.

## What the Humanizer skill does

Humanizer is a Markdown file containing detailed editing instructions for an AI agent. It does not run a special model or send text through a separate rewriting service. You give the skill to a compatible agent, then ask that agent to humanize some writing.

The current version checks for 33 patterns. These include inflated claims of significance, vague attributions, fake analysis added with dangling "-ing" phrases, promotional language, synonym cycling, forced groups of three and generic positive conclusions.

It also looks at presentation. Humanizer dislikes mechanical bold text, title case on every heading, decorative emoji and lists in which every bullet begins with a bold label. It removes chatbot leftovers such as "I hope this helps" and "Would you like me to continue?"

The skill is based on [Wikipedia's guide to signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing), which grew out of the work of editors cleaning AI-generated text from Wikipedia. The repository uses the MIT licence and has attracted more than 28,000 GitHub stars.

This is an editing checklist, not an AI detector. The skill itself warns that good human writers may use many of these patterns. Perfect grammar does not prove that a model wrote something. Neither does formal vocabulary or a single em dash. It recommends looking for clusters of habits instead.

## The em dash rule is deliberately extreme

Humanizer's strongest rule is simple: the final version must contain no em dashes or en dashes.

That is stricter than its own detection advice. Elsewhere, the skill correctly says that an em dash by itself proves nothing. Human journalists and novelists have used the punctuation mark for a very long time.

But AI has saturated the internet with em dashes. I now notice them immediately, especially when they appear several times in a short post. I have stopped using them in my own writing. A full stop, comma, colon or pair of parentheses almost always works instead.

Is that fair to the em dash? Probably not. The em dash will cope.

## How to install Humanizer

The simplest installation method uses the cross-agent Skills CLI:

```bash
npx skills add blader/humanizer
```

To install it for every supported agent on your computer:

```bash
npx skills add blader/humanizer --agent '*'
```

You can update it later with:

```bash
npx skills update humanizer
```

Claude Code users can instead install it as a plugin:

```text
/plugin marketplace add blader/humanizer
/plugin install humanizer@humanizer
```

Then invoke it with `/humanizer:humanizer` and provide the text you want edited. Because the skill is plain Markdown, it can also work in other agent systems that support skill-style instructions.

Here is the sort of sentence an AI writing assistant produces without much direction:

> In today's rapidly evolving digital landscape, AI writing tools are not merely a convenience. They represent a pivotal shift in how creators bring ideas to life, fostering efficiency, creativity and meaningful connection.

Humanizer would turn that into something closer to this:

> I use AI because it gets a rough article onto the page quickly. The first draft is often useful. It is also full of sentences I would never say.

The second version loses information if the original author truly intended to make a grand claim about creativity and connection. Usually they did not. The model supplied those abstractions because they fit many possible articles. Humanizer prefers a concrete statement that someone might be willing to defend.

The best part of the skill is its editing loop. It produces a draft, asks itself what still makes the result sound obviously AI-generated, then revises it again. That extra audit catches cases where the model merely swaps a few words and leaves the same rhythm underneath.

## What it cannot do

Running a draft through Humanizer does not make the writing yours. It can remove common model habits, but it does not know what happened at lunch or why you cared about an obscure question in the first place.

Specific details do most of the work. "My wife was watching *Chapelwaite* while we ate tom yum soup, chicken fried rice and panang curry" sounds human because it is a strange thing that happened to two particular people. A model would be more likely to sand it down to "a casual conversation over lunch."

The same applies to opinions. Humanizer can add a little uncertainty or humour, but it should not invent a reaction and attribute it to me. I still need to decide whether I believe the argument, whether a joke is funny and whether the article deserves to exist. It can also overdo the personality pass and produce cute asides that were never mine. Those have to go too.

It also does not check facts. A confident falsehood written in a natural voice is still a falsehood. That is arguably worse, because better prose makes it easier to trust. I still open the sources and make sure they support the claims in the article.

## Does Google care if a blog uses AI?

[Google's current guidance](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content) says that appropriate use of generative AI is not automatically against its rules. The problem is publishing large numbers of pages without adding value, especially when the goal is to manipulate search rankings. That may violate Google's policy against scaled content abuse.

Google recommends useful, original content written for people. It also suggests explaining how automation was used when that context would help readers.

That seems reasonable to me. I am happy to say that AI helped write this site. Hiding it would be particularly silly in an article about the tool I use to make AI writing less obvious.

Humanizer should improve the reading experience, not help fill the web with disguised sludge. If I publish 10,000 generic articles that answer questions nobody asked, removing the em dashes will not rescue them. The useful part comes from choosing a real question, researching it properly and adding some personal reason for caring about the answer.

## My current process

For now, this is how I am writing posts for ActuallyRandom.com:

1. I start with a question or observation that genuinely occurred to me.
2. I use AI to research it and produce a first draft with links to the sources.
3. I run the article through Humanizer.
4. I read the result, check the claims and change anything that does not sound like me.
5. I publish it only if I would be comfortable putting my name on it.

This article went through that process. AI did much of the research and drafted a large portion of the prose. Humanizer helped edit it. I read the result and approved what you are reading now.

The finished article is AI-assisted. I am comfortable putting my name on it, and I hope it is less annoying to read.

## Sources

- [blader/humanizer](https://github.com/blader/humanizer) on GitHub.
- [Humanizer SKILL.md](https://github.com/blader/humanizer/blob/main/SKILL.md), version 2.8.2.
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing).
- [Google Search's guidance on using generative AI content](https://developers.google.com/search/docs/fundamentals/using-gen-ai-content).
- [Google Search's guidance about AI-generated content](https://developers.google.com/search/blog/2023/02/google-search-and-ai-content).
