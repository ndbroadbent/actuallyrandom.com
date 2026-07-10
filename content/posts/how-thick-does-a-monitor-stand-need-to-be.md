---
title: "How thick does a monitor stand need to be?"
date: 2026-07-10T21:30:00+12:00
draft: false
description: "I built a 1000 mm monitor stand for a 49 inch ultrawide and wanted to know if it would bow. The answer is a formula with a cube in it, and the cube changes how you should design the thing."
slug: "how-thick-does-a-monitor-stand-need-to-be"
tags:
  - woodworking
  - engineering
  - monitors
  - DIY
categories:
  - Tech
keywords:
  - how thick does a monitor stand need to be
  - monitor riser stand
  - how much weight can a shelf hold
  - shelf sag calculator
  - diy monitor stand sag
---

I wanted a monitor stand to lift a Samsung Odyssey Neo G9 off my desk. The monitor is 49 inches of curved glass and weighs about 14.5 kilograms with its stand attached. My stand would be 1000 mm wide, 380 mm deep, 170 mm tall, cut from a single pine panel.

One question mattered more than any of the joinery: would a metre of pine bow in the middle under all that?

It would not. But working out why taught me something about shelves that I had entirely backwards, and it turns out the most important decision in the whole build was made in the aisle at Bunnings, before I picked up a saw.

## The formula, and the exponent that matters

A loaded shelf is a beam. Put the weight in the middle, support the ends, and the sag at the centre is

```
delta = W L^3 / (48 E I)
```

where W is the load, L is the span, E is the stiffness of the material, and I is the second moment of area of the cross-section. For a rectangular board of width b and thickness t, that last term is

```
I = b t^3 / 12
```

Substitute one into the other and stare at the exponents. Sag grows with the cube of the span, and shrinks with the cube of the thickness. Everything else, the load and the material, is merely linear.

This is the whole subject. Doubling the load doubles the sag. Doubling the thickness divides the sag by eight.

## The number the shop does not tell you

To use the formula I needed E, the modulus of elasticity of the panel.

I bought a Specrite 2200 x 600 x 26 mm Multi Use Pine Panel, which is finger-jointed and laminated radiata pine. The listing gives its dimensions, its price, and a photograph. I could not find a published stiffness figure for it anywhere, from the retailer or the manufacturer. This is [exactly what happened when I went looking for my router's base plate geometry]({{< ref "bosch-pof-1200-ae-dimensions-and-specs.md" >}}): the number you need to actually calculate anything is the one number nobody prints.

The Sagulator, a shelf calculator that woodworkers have used for twenty years, carries a table of species values. It lists radiata pine at 1.48 million psi, which is 10.2 GPa.

I do not entirely trust that for a finger-jointed panel, since laminating scraps of mixed-age timber should land you below a clear-wood figure. So I ran the whole calculation across a range instead of pretending I knew.

| Assumed stiffness | Sag, 26 mm pine, 1000 mm span, 20 kg | With long-term creep |
| --- | --- | --- |
| 7.0 GPa | 1.05 mm | 1.57 mm |
| 8.0 GPa | 0.92 mm | 1.38 mm |
| 10.2 GPa | 0.72 mm | 1.08 mm |
| 11.0 GPa | 0.67 mm | 1.00 mm |

Across every plausible value of a number I could not look up, the answer stays the same. That is the useful thing to know about a sensitivity analysis. You do not always need the input.

## What counts as too much sag

Woodworkers have a rule, and the Sagulator states it plainly: the target is "0.02 in per foot (1.7 mm per m) or less."

So a 1000 mm span may sag about 1.67 mm before anyone objects. My worst case above, at the lowest plausible stiffness and a load heavier than my actual monitor, comes to 1.57 mm. It squeaks in.

The rule has a second half that matters more, and it is the part I nearly missed. Wood does not just deflect and stop. It keeps creeping under sustained load for years. The Sagulator puts it as an engineering rule of thumb: "wood beams/shelves will sag an additional 50% over time beyond the initial deflection induced by the load."

A monitor is the definition of a sustained load. It sits there for a decade. Every elastic deflection you calculate should be multiplied by 1.5 before you decide you are happy, which is what the third column above does.

## The design decision that was worth eight times its weight

My stand has two legs at the ends and a third leg in the middle at the back. I added it on the theory that the monitor's foot sits centrally, so a support underneath it seemed sensible.

Look again at the exponents. Halving the span divides sag by two cubed, which is eight. Doubling the thickness divides sag by two cubed, which is also eight.

They are the same lever. A centre leg cut from an offcut of the panel I already owned bought me precisely as much stiffness as replacing the 26 mm top with a 52 mm slab. One of those is free and takes ten minutes. The other one is a tree.

With the leg in place the span becomes 500 mm and the calculated sag drops to 0.09 mm, about the thickness of a sheet of paper. The stand is now absurdly overbuilt, and I am fine with that.

If you take one thing from any of this: when a shelf sags, do not buy a thicker board. Shorten the span. The maths rewards it identically and your wallet notices the difference.

## What the AI got right

I put the design to ChatGPT while I was planning it, and I have since checked its arithmetic properly.

It told me an 18 mm panel under a 20 kg point load over 1000 mm would deflect "~2.5 mm". I calculate 2.17 mm. It told me that with the centre leg, 26 mm pine under 20 kg "deflects ≈0.1 mm". I calculate 0.090 mm. It called 26 mm "over-spec", which it is.

That is a good showing, and I want to be clear about it, because I have spent [several]({{< ref "bosch-pof-1200-ae-dimensions-and-specs.md" >}}) [posts]({{< ref "does-earths-rotation-affect-flight-time.md" >}}) documenting cases where a confident answer fell apart. This one held.

It missed two things, though, and both are the kind of omission that only shows up years later.

It never mentioned creep. Every figure it gave was instantaneous elastic deflection, which is the number that matters on the day you build the thing and the wrong number for a shelf that holds a monitor until 2036. Add 50 percent to all of them.

And it never asked what the monitor weighed. I said "~20kg" in my question and it took the number, when the Neo G9 is 14.5 kg. The guess was conservative, so nothing broke. It happened to fail safe.

## The real danger was the panel next to it

Here is the thing I did not appreciate at the time. The sag question was never really about pine.

Next to the Specrite panels at every hardware store sits a stack of MDF, cheaper, flatter, smoother, and extremely tempting for a project that is going to be painted anyway. The Sagulator rates medium density MDF at 0.35 million psi, or 2.41 GPa, less than a quarter of pine's stiffness.

Run 18 mm MDF across the full 1000 mm span under 20 kg and you get 9.2 mm of elastic sag, and 13.8 mm once it has crept. That is not a shelf, that is a hammock. Your monitor would visibly sit in a valley, and because creep is permanent, it would stay in that valley after you took the monitor off.

Pine at 26 mm passes comfortably. Pine at 18 mm fails the limit at full span. MDF at 18 mm fails it by a factor of eight.

The joinery I spent most of that conversation worrying about, the dowels and the dovetails and the pocket screws, was never going to be what determined whether this thing sagged. It was decided the moment I picked up the pine instead of the MDF, and I picked up the pine for aesthetic reasons.

I got the right answer for the wrong reason, which is most of woodworking.

## The short version

Sag goes as the cube of the span and the inverse cube of the thickness. A centre leg halves the span and therefore divides sag by eight, which is exactly what doubling the board thickness would do, and it costs you an offcut.

For a 1000 mm monitor stand carrying a 15 kilogram ultrawide, 26 mm pine is fine even without a centre leg. 18 mm pine is marginal. 18 mm MDF would sag about 14 mm and stay there.

Multiply every deflection you calculate by 1.5, because wood keeps sagging for years after you stop looking.

## Sources

- [The Sagulator](https://woodbin.com/calcs/sagulator/), WoodBin. Target sag of "0.02 in per foot (1.7 mm per m) or less", the 50 percent creep rule of thumb, and the species stiffness table (radiata pine 1.48 Mpsi, medium density MDF 0.35 Mpsi).
- Specrite 2200 x 600 x 26 mm Timber Multi Use Pine Panel, sold by Bunnings. Finger-jointed laminated radiata pine. I have deliberately not linked it, because the product listing is rendered by JavaScript and I could not verify its contents from a plain fetch, which felt like the wrong way to source a post about checking things.
- [Samsung Odyssey Neo G9 (LS49AG952NN) specifications](https://www.displayspecifications.com/en/model/16c32616), DisplaySpecifications. 14.5 kg with stand.
