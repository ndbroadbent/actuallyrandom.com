---
title: "What do foxes eat? About fifteen rodents a day"
date: 2026-07-10T21:56:00+12:00
draft: false
description: "Every answer to this question is a list. The useful answer is a number: a red fox burns about 2,300 kJ a day, a vole is worth 159, and that quota explains everything else about how foxes live."
slug: "what-do-foxes-eat"
tags:
  - biology
  - ecology
  - foxes
  - energy
categories:
  - Science
keywords:
  - what do foxes eat
  - how often do foxes eat
  - how many mice does a fox eat a day
  - fox energy requirements
  - fox hunting success rate
---

Search this and you get a list. Rodents, rabbits, birds, insects, earthworms, berries, carrion, and whatever fell out of your bin. Every source has the same list because the list is correct.

It is also useless, in the way that a list of things a car can run into is useless if what you wanted to know was how fast it goes.

I wanted the number. How much food does a fox actually need, in a day, and what does that imply about how it spends its time? The answer turns out to explain the list.

## A fox is a 2,300 kilojoule per day animal

Somebody has measured this properly. Winstanley, Buttemer and Saunders used doubly labelled water, which tracks the isotopes an animal breathes out and so measures real energy expenditure in a wild animal going about its life, rather than a captive one lying in a box.

In autumn, male red foxes averaging 5.6 kilograms burned 2,328 kilojoules a day. Females at 5.4 kilograms burned 1,681. Across spring and summer the figure sat near 326 kilojoules per kilogram per day for males and 318 for females.

Call it 1,700 to 2,300 kilojoules a day. That is what the fox has to find, before it does anything else, every single day, forever.

## What is a vole worth?

Now the other half, which was harder to source than I expected. Nobody seems to publish "energy content of a vole" as a tidy figure.

But a 2024 study of red fox nutritional ecology gives the macronutrient composition of the things foxes eat, on a wet weight basis. Its entry for small mammals is 19.6 percent protein and 9.8 percent lipid, with no carbohydrate. The same paper gives the Atwater coefficients it uses to turn that into energy: 14.64 kilojoules per gram of protein, 35.56 for lipids.

Multiply it out.

```
0.196 x 14.64  +  0.098 x 35.56  =  6.35 kJ per gram
```

So a small mammal is worth about 6.35 kilojoules for every gram of it. Usefully, Atwater coefficients already describe metabolisable energy, the part the animal actually gets, so there is no digestion discount still to apply.

A field vole of 25 grams is therefore a 159 kilojoule meal. A 20 gram mouse is 127.

## Divide

| Fox | Energy needed | Rodents per day, at 20 g / 25 g / 30 g each |
| --- | --- | --- |
| Male, autumn (5.6 kg) | 2,328 kJ | 18.3 / 14.7 / 12.2 |
| Female, autumn (5.4 kg) | 1,681 kJ | 13.2 / 10.6 / 8.8 |
| Male, spring and summer | 1,826 kJ | 14.4 / 11.5 / 9.6 |
| Female, spring and summer | 1,717 kJ | 13.5 / 10.8 / 9.0 |

Between about nine and eighteen rodents a day, depending on who the fox is, what season it is, and how fat the voles are. Call it fifteen for a male fox in autumn.

Fifteen successful hunts. Every day. That is the quota, and everything else follows from it.

## And that is why the diet is a list

Fifteen rodents a day is a punishing target, and voles do not cooperate. They have population cycles. They vanish under ice. Sometimes there simply are not fifteen.

A specialist starves in those weeks. So the fox is not a specialist. It takes rabbits when it can catch them, and beetles when it cannot, and earthworms on wet nights, and blackberries in September, and it caches surplus, and it works through your rubbish at three in the morning with no dignity whatsoever.

The list is not a description of what a fox likes. It is a description of what a 2,300 kilojoule daily deficit does to an animal's standards.

This is the same move that makes [a sun-powered human impossible]({{< ref "why-cant-humans-photosynthesize.md" >}}). Work out the energy budget first, and the biology stops being a collection of facts and starts being a consequence.

## What I am glossing over

The metabolic rates come from foxes in Australia and the hunting statistics from foxes in the Czech Republic, and those animals eat different things in different climates. I am treating them as one fox because I want an order of magnitude, not a thesis.

"Small mammals" in that composition table is an aggregate, not a vole. Real voles vary in fat content across the year, quite a lot, and fat carries nearly two and a half times the energy of protein.

The under-18-percent figure is for jumps in the disfavoured directions, and Červený's team could only cleanly determine the outcome of 200 of the 592 jumps. The overall success rate across all pounces is not reported, so my eighty-pounce figure is a worst case, not an average day.

None of that moves the conclusion. A fox needs something in the region of a dozen rodents a day, and it has maybe a few dozen pounces in it, and the arithmetic is tight enough that a magnetic compass earns its keep.

## The short version

A red fox burns 1,700 to 2,300 kilojoules a day. A small mammal is worth about 6.35 kilojoules per gram, so a vole is 159 kilojoules. That is nine to eighteen rodents daily, depending on the fox and the season.

Pouncing north-northeast succeeds 72.5 percent of the time. Pouncing any other way succeeds less than 18 percent of the time. Fifteen meals a day is twenty pounces if you are aligned and eighty if you are not.

Foxes eat everything because fifteen is a lot of voles.

## Sources

- Winstanley, R. K., Buttemer, W. A. and Saunders, G. (2003). [Field metabolic rate and body water turnover of the red fox *Vulpes vulpes* in Australia](https://doi.org/10.1046/j.1365-2907.2003.00015.x). *Mammal Review* 33. Doubly labelled water measurements: 2,328 kJ per day for autumn males, 1,681 for females.
- Balestrieri, A., Gigliotti, A., Caniglia, R. et al. (2024). [Nutritional ecology of a prototypical generalist predator, the red fox (*Vulpes vulpes*)](https://doi.org/10.1038/s41598-024-58711-6). *Scientific Reports* 14. Small mammals at 19.6 percent protein and 9.8 percent lipid by wet weight, and the Atwater coefficients used to convert them.
- Červený, J., Begall, S., Koubek, P., Nováková, P. and Burda, H. (2011). [Directional preference may enhance hunting accuracy in foraging foxes](https://doi.org/10.1098/rsbl.2010.1145). *Biology Letters* 7. 592 mousing jumps by 84 foxes; 72.5 percent success to the north-northeast, under 18 percent elsewhere. Free to read [via PubMed Central](https://pmc.ncbi.nlm.nih.gov/articles/PMC3097881/).
