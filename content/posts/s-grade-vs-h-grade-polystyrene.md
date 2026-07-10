---
title: "S grade vs H grade polystyrene: what is actually different?"
date: 2026-07-10T19:00:00+12:00
draft: false
description: "S grade EPS is 16 kg/m3 and H grade is 24 kg/m3. Here are the densities, R-values, compressive strengths and the colour code that tells them apart, and why the manufacturer's numbers beat the standard's."
slug: "s-grade-vs-h-grade-polystyrene"
tags:
  - polystyrene
  - insulation
  - building
  - New Zealand
categories:
  - Tech
keywords:
  - s grade vs h grade polystyrene
  - expol thermaslab s grade h grade
  - EPS polystyrene grades nz
  - polystyrene density 16 kg/m3 24 kg/m3
  - AS 1366.3 polystyrene grades
---

I was buying a sheet of polystyrene to sit between a pool table and a plywood workbench top, which is a sentence that requires no further explanation if you have ever owned both a pool table and an idea. The listing said "EXPOL 2400 x 1200 x 40mm S Grade Polystyrene ThermaSlab", and I wondered what the S meant, and whether I should have bought the H instead.

The short answer, if that is all you came for:

| | S grade | H grade |
| --- | --- | --- |
| Density | 16 kg/m³ | 24 kg/m³ |
| Nominal thermal conductivity | 0.041 W/m °C | 0.038 W/m °C |
| Nominal R-value at 50 mm | about 1.2 m² °C/W | about 1.3 m² °C/W |
| Colour code on the sheet edge | brown | green |
| Relative weight | a third lighter | 50% heavier |

H grade is denser, stronger, slightly more insulating and more expensive. S grade is the standard board and it is what most people want.

That is the answer. The interesting part is where those numbers come from, because two respectable sources disagree about them, and the disagreement is the whole story.

## What the grades actually are

Rigid polystyrene foam comes in two forms. Extruded polystyrene (XPS) is usually coloured, with a uniform texture. Expanded polystyrene (EPS) is the common white stuff with visible beads, and it is what ThermaSlab and its competitors are made of. XPS costs more for the same thermal resistance but takes less space: roughly 30 mm of XPS matches 50 mm of EPS.

Within EPS, the grades are set by an Australian and New Zealand standard, AS 1366.3, which classifies the material with the wonderfully bureaucratic name "rigid cellular polystyrene, moulded". The grades are graded by density. EXPOL sells five of them:

| Grade | Density |
| --- | --- |
| SL | 12 kg/m³ |
| S | 16 kg/m³ |
| M | 20 kg/m³ |
| H | 24 kg/m³ |
| VH | 28 kg/m³ |

S and H are the two you will actually meet in a hardware store. Everything else follows from that density. Denser foam has more polystyrene and less trapped air per cubic metre, so it holds more weight and conducts slightly more heat per unit of thickness, which is why the R-value per millimetre goes up only a little while the compressive strength goes up a lot.

On EXPOL's figures, compressive strength at 10% deformation ranges from 84 kPa for S grade up to 189 kPa for VH. Cross-breaking strength runs from 135 kPa for SL to 320 kPa for VH.

## The colour code nobody mentions

AS 1366.3 requires the sheets to be colour coded along the side. Brown for S grade. Green for H.

I did not know this, and I suspect most people buying a sheet do not either. It is genuinely useful, because once a sheet is out of its wrapper the two grades are almost impossible to tell apart. As BRANZ put it, "in practice it is difficult to detect the difference by simply feeling the material."

There is a catch. The same source notes that "unfortunately not all the manufacturers comply".

## Where the numbers disagree

Here is where it gets interesting.

The best independent source I could find is an article by Ian Cox-Smith, a building physicist at BRANZ, published in *Build* magazine. It gives the nominal figures from the standard: S grade has a nominal thermal conductivity of 0.041 W/m °C and H grade 0.038, which works out to an R-value at 50 mm of about 1.2 for S and 1.3 for H.

EXPOL's own published table says a 50 mm sheet gives R1.32 for S grade and R1.39 for H grade.

Both cannot be describing the same thing. Work backwards from the R-value and you can see what EXPOL is claiming, because R is just thickness divided by conductivity:

```
EXPOL S grade:  0.050 / 1.32  =  0.0379 W/m.K
EXPOL H grade:  0.050 / 1.39  =  0.0360 W/m.K

AS 1366.3 nominal S grade:  0.041 W/m.K
AS 1366.3 nominal H grade:  0.038 W/m.K
```

EXPOL's S grade claims a thermal conductivity of 0.0379, which is very slightly better than the standard's nominal value for H grade. Its declared R1.32 for a 50 mm S sheet is, to two decimal places, exactly the nominal R-value of an H sheet.

Neither party is lying. The standard sets a floor, not a description. A grade is awarded when a material meets or exceeds every specification, so real material routinely performs better than nominal. Cox-Smith says so directly, and in 2004 he predicted precisely this situation:

> A nominal S-grade material could, in practice, have almost as low a thermal conductivity as the nominal H grade.

The trap he warns about is what a designer is allowed to assume. If a manufacturer states only the grade and not a measured conductivity, the designer must use the nominal value for that grade. And he is blunt about why measured values deserve some suspicion:

> Because equipment for measuring thermal conductivity is relatively expensive, manufacturers usually do not have the facilities for monitoring it during production. They regularly check the other properties of the material, such as compressive stress and cross-breaking strength, and simply assume that the thermal conductivity specifications are being met.

He also notes that advertising and technical literature have "erroneously quoted the thermal conductivity specifications from AS 1366.3" as though meeting one specification implied meeting them all.

So: if you are doing something where the number matters, use 1.2 and 1.3. If you are insulating a garden shed, EXPOL's numbers are probably closer to the truth of the sheet in your hands, and the difference between the grades is a rounding error either way.

## Which one should you buy?

For thermal performance alone, the honest answer is that it barely matters. The gap between S and H at the same thickness is 0.1 in R-value on the standard's figures, which is a tenth of the insulation you would get from adding 4 mm of extra foam. Buying a slightly thicker S sheet is usually cheaper than upgrading the grade.

The gap that actually matters is compressive strength. H grade is for loads: under a concrete floor slab, under something that will be driven on, anywhere the foam has to hold its thickness while a building presses on it. S grade is the general-purpose board for walls, roofs, retaining walls and low-load work.

Cox-Smith raises one case where the small thermal difference does matter. In 2004 he pointed out that 0.1 of R-value "may be the difference between complying and not complying with the New Zealand Building Code". If you are building to a compliance number rather than to taste, the grade on the docket is not a detail.

For my workbench, sitting between a pool table and a sheet of plywood, carrying some tools and the occasional elbow, S grade was obviously fine. I would have needed H grade only if I intended to park something on it.

## A note on the sources

The best available explanation of the difference between S and H grade polystyrene, the one that actually cites the standard and explains the colour coding, was written in the June/July 2004 issue of a New Zealand trade magazine. It is twenty-two years old. Its web server returns 403 to anything that is not a browser, so I read it through the Internet Archive.

Everything else on the first page of search results is a product page. There is no shortage of information about EPS grades. There is just nobody putting it in one place, in a form that answers the question a person types into Google while standing in a hardware aisle.

That is why this post exists.

## Sources

- Cox-Smith, I. (2004). Insulating with expanded polystyrene foam. *Build* 82, pages 20 to 21. BRANZ. [Archived copy](https://web.archive.org/web/20240802222221/https://www.buildmagazine.org.nz/assets/PDF/B82-20.pdf).
- [EXPOL ThermaSlab polystyrene sheet](https://www.expol.co.nz/expol-thermaslab-polystyrene-sheet/), grade densities, R-values and compressive strengths.
- AS 1366.3, *Rigid cellular plastics sheets for thermal insulation: rigid cellular polystyrene, moulded (RC/PS-M)*, as described in the BRANZ article above.
