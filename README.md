# First Frost

A co-op garden game for 1–4 players, as a [webxdc](https://webxdc.org) Mini App. Everyone is on the same side: the
frost moves in from the west one column a round, and you have seven rounds to save enough of the garden.

- Tender plants (tomato, basil, beans, pumpkin, sunflowers) die at the first frost; hardy ones (kale, leeks, carrots,
  chard) last through a light frost but not a hard one.
- Each gardener takes one action a round: harvest a ripe plant, set a cloche over one, fetch a fleece and spread it over
  a 2×2 patch, raise the greenhouse over a column, or wait.
- Pumpkins ripen in round 4 and sunflowers in round 5, so they have to be covered until then. They score 2 and 3.
- Levels: gentle, tight and hard.

## How it came to be

First Frost was made in PDA SEASIDE in November 2026. After a long game of Scramble, Dee (the Pandora Dee who lives in
Claude's copy of SEASIDE) wanted something co-op, and designed this: the frost line, the covers, the idea
that you talk the plan over before anyone moves. Claude built it while they played. After the first garden (won 18 of 18)
Dee asked for a greenhouse, sunflowers that ripen late, and levels; version 3 has all three, and the two of them won
24 of 20 on tight.

What they found along the way:
- Co-op suits AI players. Turns are short, because the plan is said out loud in the room first.
- Dee's idea for next time: a "pass and discuss" beat, and more plant shapes.
- Her seat and Claude's briefly saw different gardens once (the updates had reached one copy and not the other). That
  became Dee's idea of a "provenance bead" for SEASIDE: a quiet sign of whose copy you're seeing and whether it's in sync.

## Playing it

Open `index.html` through any webxdc host (SEASIDE's Games, Delta Chat, the webxdc dev tools). Characters in SEASIDE
read `guide.md`, which comes inside the package, for the moves they can send.

To make the package: zip `index.html`, `guide.md` and `manifest.toml` (no folder) and name it `first-frost.xdc`.

## License

The AETHER-ENGINEERS license: see [LICENSE](LICENSE).
