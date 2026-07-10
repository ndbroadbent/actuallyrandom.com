---
title: "Can an AI guess what it knows?"
date: 2026-07-10T21:45:00+12:00
draft: false
description: "I gave a language model 60 New Zealand schools and asked it to guess which ones have Wikipedia articles. It could rank them. It could not put a number on them, and it would have scored better by ignoring everything it knew."
slug: "can-an-ai-guess-what-it-knows"
tags:
  - AI
  - calibration
  - Wikipedia
  - experiments
categories:
  - Artificial intelligence
keywords:
  - do llms know what they don't know
  - llm calibration
  - does chatgpt know when it is wrong
  - ai hallucination confidence
  - brier score language model
---

Last New Year's Eve I asked ChatGPT a question that has been rattling around my head:

> based purely on your training data, if you had to guess (without looking anything up), is "Wentworth Valley Campground" notable enough to have a wikipedia page?

That is not really a question about a campground. It is a question about whether a model can look inward and estimate the density of its own knowledge. Every hallucinated citation I have ever been handed is a failure of exactly this skill.

So I stopped speculating and measured it.

## The setup

To test whether a model knows what it knows, you need a set of things where the answer is genuinely uncertain. Ask about Napoleon and it says yes, correctly. Ask about my neighbour and it says no, correctly. Neither tells you anything.

I needed entities sitting right on the notability line. Wikidata gave me one: New Zealand schools. There are 143 of them with English labels, and exactly half have an English Wikipedia article. A base rate of 50 percent is the hardest possible test, because a coin does well.

Then the important part, the blinding. A script queried Wikidata, wrote the ground-truth labels to `labels.json`, wrote a shuffled list of 60 school names to `names.txt`, and printed nothing but the aggregate base rate. I read only the names. I assigned a probability to each one, from memory, with no lookups, and saved them. Only then did the scoring script join the two files.

I want to be blunt about the conflict of interest here. The model doing the guessing was Claude, the same assistant I use to help write this blog. It is grading its own homework, and I designed the exam. What saves it from being worthless is that the labels were on disk, unread, before a single probability was written down.

## What happened

Sixty schools. Thirty-one have articles, twenty-nine do not, so the base rate is 52 percent.

The model's average predicted probability was 33.5 percent.

That gap is the whole story, but here is the scoring anyway.

| Measure | Model | A coin that always says "52 percent" |
| --- | --- | --- |
| Brier score (lower is better) | 0.2706 | 0.2497 |
| Expected calibration error | 0.192 | 0 |
| Accuracy at a 0.5 threshold | 53 percent | 52 percent |
| AUC (ranking ability) | 0.640 | 0.500 |

Read the first row twice. The model's carefully considered, per-school probability estimates produced a worse Brier score than a rock with "52 percent" painted on it. Bootstrapping the difference gives a 95 percent confidence interval of -0.031 to +0.095, so I cannot claim it is reliably worse. I certainly cannot claim it is better. All that knowledge, and it fails to beat knowing nothing except the base rate.

And yet the last row says something is working. An AUC of 0.640 means that if you hand the model one school with an article and one without, it puts them in the right order 64 percent of the time. A permutation test puts that at p = 0.028. Weak, but real.

So the model can rank. It cannot calibrate.

## The reliability table, which is where it gets embarrassing

Group the predictions by confidence and compare each group's average prediction against how often those schools actually had articles.

| The model said | Number of schools | Actually had articles |
| --- | --- | --- |
| about 12 percent | 14 | 29 percent |
| about 31 percent | 32 | 56 percent |
| about 48 percent | 8 | 50 percent |
| about 72 percent | 2 | 100 percent |
| about 82 percent | 4 | 75 percent |

Every low-confidence bin is wrong in the same direction. When the model said "probably not," it was wrong about twice as often as it expected. Its single largest bin, thirty-two schools it rated around 31 percent, contained a majority of schools that do have articles.

It is not noisily miscalibrated. It is systematically, uniformly too pessimistic, by about 18 points.

## It answered a different question

Once you look at which ones it missed, the reason is obvious and slightly humbling.

The confident errors run in one direction. It gave Hamilton Seventh-day Adventist School a 10 percent chance, Cornerstone Christian School 12 percent, Rangiora New Life School 15 percent, Te Kura Kaupapa Māori o Te Koutu 15 percent. All four have Wikipedia articles.

Its most confident mistake in the other direction is the most revealing of the lot. It gave Wellington Technical College an 80 percent chance of having an article. It has no article of its own. Type the name into Wikipedia and you are redirected to "Wellington High School, New Zealand," which is what the institution became.

Look at what the model was actually doing. Wellington Technical College is historically important, so it deserves an article, so it probably has one. Kaikohe Christian School is a small religious school in a small town, so it does not deserve an article, so it probably has none. That reasoning is about merit.

But whether a Wikipedia article exists is not a fact about merit. It is a fact about whether a New Zealander with a spare evening in 2007 decided to write a stub. Wikipedia's own notability guideline is one input to that decision, and a weak one. The model has a theory of what deserves to exist and no theory at all of who does the work.

Asked to predict the world, it evaluated the world instead.

## The redirect problem, and a bug of my own

I nearly missed something. Wellington Technical College counts as "no article" in my data because Wikidata records no English Wikipedia sitelink for it. But you can still reach a page by typing its name.

So I checked all 29 of the schools with no article, to see how many were reachable anyway. The script came back and said three.

That number was wrong, and I knew it was wrong, because I had already confirmed by hand that Wellington Technical College redirects to Wellington High School, and it was not in the list of three. My script was catching exceptions from the Wikipedia API and silently treating a network hiccup as "does not exist." I added retries and ran it again.

The real answer is nineteen out of twenty-nine. Sixty-six percent of the schools with "no Wikipedia article" will still land you on a real Wikipedia page, usually the article for the town they sit in. Hastings Christian School redirects to the suburb of Akina. Braemar House redirects to Columba College. Dunedin School of Art redirects to King Edward Technical College, which is the very entity the model rated at 85 percent.

I have written before about [a metric that reported everything was fine because it was quietly failing]({{< ref "mining-2286-chatgpt-conversations-for-blog-posts-people-actually-search-for.md" >}}), and here I am doing it again in a post about measurement error. The only reason I caught it is that I happened to have verified one case by hand, and the machine disagreed with me.

Under the looser definition, where a redirect counts, the base rate leaps from 52 percent to 83 percent, and the model's underprediction goes from 18 points to 50.

## Why this is the hallucination problem

This is the same failure that produces fake citations, and running the experiment made me see it properly.

When a model invents a plausible reference, it is not lying, and it is not confused about the facts. It is answering the question it can answer. "What would a reference to this claim look like?" is a question about plausibility, and models are superb at plausibility. "Does that reference exist?" is a question about the contents of a database, and requires knowing the boundaries of your own memory.

When I chased [the dimensions of a router base plate]({{< ref "bosch-pof-1200-ae-dimensions-and-specs.md" >}}), I got two confident, contradictory tables, each decorated with citations that had decayed into placeholder tokens. Nothing about those numbers was random. They were the numbers such a table ought to contain.

A model asked whether Wentworth Valley Campground has a Wikipedia page answers by asking itself whether it should. That is the wrong question, and it is wrong in a direction that produces confident, articulate, well-formatted fiction.

## Caveats, because sixty is not many

This is one entity type, in one country, with one model, and sixty data points. The confidence intervals are wide enough to drive a bus through. Wikidata's coverage is itself entangled with Wikipedia, since many items exist because an article did, which will distort the base rate in ways I have not untangled.

What I would not bet against is the direction. A model that has never met a New Zealand school is going to reason from notability, because notability is the only thing it can reason from.

## The campground

I should finish the question I started with.

Wentworth Valley Campground has no Wikipedia article. Wentworth Valley does.

The model would have got that one right, for entirely the wrong reason, and it would not have been able to tell me how confident to be.

## Method and data

- Population: Wikidata items with `instance of: school` and `country: New Zealand`, 143 with English labels, of which 72 have an English Wikipedia sitelink.
- Sample: 60, shuffled with a fixed seed, labels written to disk and unread until after predictions were recorded.
- Ground truth spot-checked against live Wikidata sitelinks, and the redirect analysis run against the live English Wikipedia API.
- Scores: Brier, expected calibration error over 5 bins, AUC by the Mann-Whitney statistic, a 20,000-draw permutation test for AUC and a 20,000-draw bootstrap for the Brier difference.
