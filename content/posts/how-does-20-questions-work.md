---
title: "How does 20 questions work? Twenty is not an arbitrary number"
date: 2026-07-10T22:00:00+12:00
draft: false
description: "Twenty yes-or-no questions is exactly twenty bits, enough to pick one thing out of 1,048,576. I played against ChatGPT, measured every question it asked, and watched it throw away eight of its twenty bits."
slug: "how-does-20-questions-work"
tags:
  - information theory
  - AI
  - games
  - mathematics
categories:
  - Science
keywords:
  - how does 20 questions work
  - 20 questions strategy
  - why 20 questions
  - information theory yes no questions
  - entropy bits game
---

Twenty questions. Yes or no answers only. Somebody thinks of a thing, and you have to find it.

The number twenty looks like a folk choice, the sort of round figure a Victorian parlour settles on. It is not. Twenty is very close to exactly the right number, and you can derive it.

## Every question is worth one bit, at best

A yes-or-no answer can carry at most one bit of information. One bit halves the space of possibilities. Ask a perfect question and whatever the answer, you throw away half the world.

Do that twenty times and you have divided the world by two, twenty times over. Which means twenty questions can single out one thing from

```
2^20 = 1,048,576
```

possibilities. A little over a million.

Now ask how many things a person might reasonably pick when you say "think of something." Not every noun and every human who ever lived, but the things that occur to a person in a game: animals, objects around the house, famous people, foods, countries, fictional characters.

A hundred thousand needs 16.6 questions. A million needs 19.9. Ten million needs 23.3.

Twenty questions covers about a million things, which is roughly the size of the space a person actually draws from. The game is calibrated. Whoever settled on twenty had good instincts, and Shannon could have told them why.

## But only if you split the space in half

The one-bit figure is a ceiling, not a promise. You only collect the full bit if the answer is a coin flip, if before you asked, "yes" and "no" were equally likely.

The information a question carries is its entropy, and for a question that comes back "yes" with probability p, that is

```
H(p) = -p log2(p) - (1-p) log2(1-p)
```

Which produces a brutal little table.

| The question splits the space | Information you get | Questions needed to cover 20 bits |
| --- | --- | --- |
| 50 / 50 | 1.000 bits | 20.0 |
| 40 / 60 | 0.971 bits | 20.6 |
| 30 / 70 | 0.881 bits | 22.7 |
| 20 / 80 | 0.722 bits | 27.7 |
| 10 / 90 | 0.469 bits | 42.6 |
| 5 / 95 | 0.286 bits | 69.8 |

An imperfect question is not slightly worse. A question that only eliminates one possibility in ten costs you less than half a bit, and you will need forty-three of them.

This is why "is it an animal?" is a good opening and "is it a badger?" is a terrible one. Both are legal. One of them is worth twenty times the other.

## Guessing a name is almost worthless

Which brings us to the trap that every human player falls into, and that I watched a language model fall into with unusual precision.

Suppose you have narrowed things down to sixty-four candidates, and you use a question to guess one of them by name. What is that worth?

If your guess is right, wonderful, you win and the information was worth everything. But it will be right one time in sixty-four. Put p = 1/64 into the entropy formula and the expected value of the question is 0.116 bits.

You have a turn worth one bit and you are spending it on a tenth of a bit.

| Candidates remaining | Value of guessing one by name | Proportion of the turn wasted |
| --- | --- | --- |
| 64 | 0.116 bits | 88 percent |
| 200 | 0.045 bits | 95 percent |
| 500 | 0.021 bits | 98 percent |
| 1000 | 0.011 bits | 99 percent |

Naming a candidate feels like progress because it is the shape of the answer. It is nearly always the worst legal move on the board.

## So I went and looked at the transcript

Two years ago I played twenty questions against ChatGPT. It asked, I answered. I dug the conversation out of my archive and counted what it did.

Forty-one questions. Thirty-two of them about attributes, nine of them naming a specific person.

The first fourteen are a masterclass. Living thing, animal, plant, human, famous, entertainer, actor, known for movies, male, currently active, action movies, twentieth century, American, starred in a famous action series. Every one of those cuts a real fraction off the space. That is what a binary search looks like.

Fourteen clean questions take 1,048,576 down to 64. And at that point it had six questions left in the budget, and six bits is precisely enough to identify one thing out of sixty-four. It had exactly the information it needed and exactly the turns required to spend it.

Instead it started guessing names.

Arnold Schwarzenegger. Sylvester Stallone. Bruce Willis. Eddie Murphy. Jackie Chan. Robert Downey Jr. Wesley Snipes. Ryan Reynolds. Will Smith.

Nine guesses, worth about 1.05 bits between them, in place of nine questions worth 9.00. It threw away eight bits, which is to say it discarded the ability to distinguish 256 things, and it took forty-one questions to finish a job that twenty would have done.

Here is the part I like. If your questions average a 10/90 split, the table above says you need 42.6 of them. The model needed 41.

## It also broke its own logic and did not notice

Look again at the opening.

> Is it an animal?
> No.

> Is it a human?
> Yeah.

A human is an animal. I answered the first question wrong, in the loose way people do, and the model simply carried on. It never flagged the contradiction, never revisited the branch it had already pruned, never asked which of my two answers I meant.

A search algorithm maintains a set of live candidates. If the answers are inconsistent, that set becomes empty and something has to give. The model was not maintaining a set. It was producing questions that sounded like the next question a person would ask.

## The answer was Will Smith

And it got there in the end, on the forty-first question. In fairness to the machine, I had picked a genuine curveball, and it is worth seeing why.

Somewhere around question thirty it asked whether the person had starred in a superhero movie, and I said yes, because of *Hancock*. Then it asked whether he played a character in the Marvel Cinematic Universe, and I said no. Both answers are true. Together they are close to unfalsifiable, because almost every famous superhero-movie actor is in Marvel or DC, and *Hancock* belongs to neither.

I had, without meaning to, chosen a person who sits in a tiny pocket of the space where the model's questions could not reach. That is what a good answer looks like in this game: not obscure, just badly indexed.

## The lesson, which is the same lesson

The model asked plausible questions. Questions that sound like the ones a person asks while playing twenty questions. What it did not do was ask which question would carve the space most evenly, which is the only thing the game rewards.

I keep finding this. When I [asked a model to guess which New Zealand schools have Wikipedia articles]({{< ref "can-an-ai-guess-what-it-knows.md" >}}), it answered a question about which ones deserved articles. Here, asked to gather information, it produced the aesthetics of an interrogation.

Plausibility is a very good proxy for correctness, most of the time, which is exactly why it is so hard to see when it comes apart.

## How to actually win

Ask questions that half your remaining candidates would answer yes to. Not the interesting question. Not the question that would be thrilling if the answer were yes. The boring one, the one that cleaves the field.

Never name a specific candidate until fewer than about three remain, because until then you are handing back most of a bit for a lottery ticket.

And if you are the one thinking of something, pick a thing that is famous but sits between the categories anyone would think to ask about. Pick *Hancock*.

## Sources and workings

- The entropy of a binary question is `H(p) = -p log2(p) - (1-p) log2(1-p)`, from Shannon's 1948 [A Mathematical Theory of Communication](https://doi.org/10.1002/j.1538-7305.1948.tb01338.x).
- The transcript is my own, from July 2024. I counted 41 assistant turns, of which 9 proposed a named individual, by matching the pattern "Is this person X?" against the conversation.
- All the arithmetic above is reproducible in three lines of Python, which is really the point of information theory.
