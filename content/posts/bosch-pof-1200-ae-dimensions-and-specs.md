---
title: "Bosch POF 1200 AE dimensions and specs, and the one measurement nobody publishes"
date: 2026-07-10T20:00:00+12:00
draft: false
description: "The official specs for the Bosch POF 1200 AE, straight from the manual. Plus the base plate hole pattern, which Bosch has never published, and which every source online gets differently."
slug: "bosch-pof-1200-ae-dimensions-and-specs"
tags:
  - routers
  - woodworking
  - 3D printing
  - Bosch
  - fact check
categories:
  - Tech
keywords:
  - bosch pof 1200 ae dimensions
  - bosch pof 1200 ae specs
  - bosch pof 1200 ae collet size
  - bosch pof 1200 ae shank size
  - bosch pof 1200 ae base plate
---

I wanted to 3D print an adapter so my Bosch POF 1200 AE router would run along a Makita guide rail. To do that I needed the base plate geometry: the diameter of the centre hole, and where the three mounting screws sit.

I assumed this would take about four minutes. Bosch sells the router, Bosch sells a guide rail adapter for it, so Bosch must publish the plate dimensions somewhere.

Bosch does not. As far as I can tell, nobody does. And the numbers floating around the internet do not agree with each other.

## The specifications that are actually published

These come from the original instructions for the POF 1200 AE and POF 1400 ACE, which Bosch publishes as a single manual. Check the article number against the type plate on your machine before trusting any of it, because Bosch reuses trade names across models and voltages.

| | POF 1200 AE | POF 1400 ACE |
| --- | --- | --- |
| Article number | 3 603 B6A 0.1 | 3 603 B6C 7.1 |
| Rated power input | 1200 W | 1400 W |
| No-load speed | 11,000 to 28,000 rpm | 11,000 to 28,000 rpm |
| Tool holder | 6 mm / 8 mm, and 1/4 inch | 6 mm / 8 mm, and 1/4 inch |
| Plunge depth | 55 mm | 55 mm |
| Weight (EPTA 01/2003) | 3.4 kg | 3.5 kg |
| Protection class | II | II |
| Speed preselection | yes | yes |
| Constant electronic control | no | yes |

The manual notes that these values hold for a nominal voltage of 230 V and can vary for other voltages and country-specific models.

That last row is the honest difference between the two routers. The 1400 ACE holds its speed under load. The 1200 AE lets the speed sag when the bit bites. If you are choosing between them and you cut hardwood, that matters more than the 200 watts.

## Collet size, shank size, bit size

This is what most people are really asking, and the answer is muddier than it should be.

The manual gives the tool holder as 6 mm and 8 mm, and separately as 1/4 inch. The machine accepts all three, because the collet is a removable part. What actually ships in the box depends on where you bought it. UK retailers routinely list this router as a 1/4 inch machine. The manual's own table leads with the metric sizes.

So "what shank size does the POF 1200 AE take" has no single answer. It takes whatever collet is currently fitted, and you need to look.

This is worth being careful about, because 1/4 inch is 6.35 mm. That is only 0.35 mm larger than a 6 mm collet, which is close enough to feel like it might work and far enough to be dangerous. The manual is blunt on the point: routing bits that do not fit precisely in the tool holder "rotate irregularly" and lead to imbalance. A 1/4 inch bit does not belong in a 6 mm collet, and a 6 mm bit rattling inside a 1/4 inch collet is worse.

The other number people miss: the manual requires the shank to be immersed at least 20 mm into the collet. Short-shank bits and deep plunges are how people find that out the hard way.

## The base plate: what Bosch documents

The manual labels the base plate as part 6. It labels the router compass and guide rail adapter as part 35, marked with an asterisk to show it is an accessory rather than something in the box.

It gives no dimension for either. There is no drawing of the plate, no bolt circle, no centre opening diameter, no screw spacing. The only geometric fact the manual volunteers about the plate is a warning that some router bits will not fit through it.

Bosch sells you a plate, sells you an adapter that bolts to the plate, and tells you nothing about the plate.

## Three numbers that cannot all be true

I asked ChatGPT for the base plate dimensions. Twice, on the same day, a few hours apart, in two separate conversations. I got two confident answers, each laid out in a tidy table with citations.

The first conversation told me the plate is a D-shape measuring 161 mm by 140 mm, with a 53 mm centre hole, two 8 mm guide rod bores 84 mm apart, and three M4 countersunk screws.

The second conversation told me the plate is a round shoe with a 155 mm outside diameter, a 44 mm centre opening, and three M4 screws on a 56 mm pitch circle.

Those are not two descriptions of one router. The centre hole cannot be both 53 mm and 44 mm. The plate cannot be both a 161 mm D and a 155 mm circle.

It gets better. The first answer described the screw pattern three different ways in one block of text:

- the two screws on the flat edge are about 75 mm apart
- each screw sits about 47 mm from the spindle centre
- which "gives an equilateral triangle of about 94 mm side length"

For three screws equally spaced around the spindle, the bolt circle radius is the triangle's side divided by the square root of three. So those three claims imply three different plates:

```
side 75 mm  ->  radius 43.3 mm
side 94 mm  ->  radius 54.3 mm
radius 47 mm ->  side 81.4 mm
```

No two of them agree. The answer contradicted itself inside a single bullet list, and I did not notice at the time, because it was formatted like a spec sheet and spec sheets are things I believe.

Both answers cited sources. When I pulled these conversations back out of my ChatGPT archive to write this post, the citations had been reduced to placeholder tokens like `citeturn12view0`. Not one of them survived as a URL. I could not check a single source, because the sources were never really there for me to check.

## So measure it

The correct answer to "what is the Bosch POF 1200 AE base plate hole pattern" is that you have to go and find out, and it takes ten minutes.

Unplug the router. Take the black plastic shoe off, which is held by the countersunk screws you are trying to measure. Put digital calipers across two screw holes, centre to centre. If the three holes are equally spaced, divide that distance by 1.732 to get the bolt circle radius. Measure the centre opening directly.

Then, before you cut anything expensive, print your CAD sketch at full scale on paper, lay it over the original plate, and hold it up to a light. Misalignment that is invisible in Fusion is obvious on paper.

This is not a satisfying answer and it is the only reliable one. Green-line Bosch routers have varied across production years, so even a correct measurement from someone else's machine is a starting point rather than a specification.

GrabCAD hosts user-uploaded STEP models of this router. They are useful for getting the rough shape into CAD quickly. They are also uploaded by strangers with no obligation to be accurate, and at least one of the contradictory answers above was probably derived from one of them.

## If you are printing the adapter

A few things I learned that are not in any manual.

Model the rail as a solid and subtract it from your sled, rather than trying to draw the slot by hand. Then grow the cavity with an offset rather than redoing the boolean every time you want to adjust the fit.

For PETG I settled on 0.15 mm of offset per wall, so 0.30 mm across the slot. PETG contracts a few tenths of a percent as it cools, and most printers need somewhere between 0.10 and 0.15 mm per side before a sliding fit stops binding. At 0.10 mm per side mine dragged. At 0.20 mm it had a perceptible wobble, which on a router sled means a wandering cut.

Print a 50 mm test coupon of just the rail slot before committing to the whole part. Filament is cheap and a failed six hour print at 2am is not.

Lay the sled flat so the layer lines run along the direction of travel. The tabs that hook under the rail lip are the parts most likely to delaminate, and they last much longer when the layers are not trying to peel apart along the sliding axis.

## The short version

The published specs are the ones in the table above, and they are reliable because Bosch printed them. The plunge depth is 55 mm, the collet is 6 mm or 8 mm or 1/4 inch depending on which one is fitted, and the shank needs 20 mm of engagement.

The base plate hole pattern is not published by Bosch, is not reliably documented anywhere else, and is reported inconsistently by every secondary source I found, including a language model that managed to contradict itself twice in one answer. The calipers are in the drawer. Go and get them.

## Sources

- [Bosch POF 1200 AE and POF 1400 ACE original instructions](https://www.tooled-up.com/artwork/ProdPDF/Bosch-POF1200AE_manual.pdf) (PDF). Technical data table, article number 3 603 B6A 0.1, tool holder sizes, 55 mm plunge depth, the 20 mm minimum shank engagement, and the warning about bits that do not fit the collet precisely.
- [Bosch POF 1200 AE user-uploaded STEP model](https://grabcad.com/library/bosch-pof-1200-ae-1), GrabCAD. Useful for rough geometry, not a specification.
