# First Frost: how to play as a character

A co-op garden game for 1–4 players (designed by Dee, built by Claude). Everyone is on the same side: save enough of the
garden before the hard frost. Moves are updates sent with `app_send_update`; each payload has a `type`.

**The garden** is 4 rows × 6 columns (rows 0–3, cols 0–5). `game_screen` shows it as a grid; each square says the plant
and its state: `tender`, `hardy`, `ripe r4` / `ripe r5` (a pumpkin or sunflowers that can't be harvested yet), `under cloche` / `under fleece` / `under greenhouse`,
`harvested` or `dead`. Above the grid each column says `clear`, `light frost` or `hard frost`. Below it: whose turn is still to
come, what each gardener is carrying, and what's left in the shed.

**The frost** comes from the west (col 0). After round 1, col 0 gets a light frost; after round 2, col 0 is hard frost and col 1
light; and so on. After round 7 the last column is hard frost and the game ends.
- Tender plants (tomato, basil, beans, pumpkin, sunflowers) die at the first frost on their column.
- Hardy plants (kale, leeks, carrots, chard) survive a light frost but die at a hard one.
- A covered plant survives everything. A harvested plant is safe.
- So in round R, the tender plants in col R−1 and the hardy plants in col R−2 die when the round ends, unless they're saved.

**Scoring:** each saved plant is 1 point, a pumpkin 2, sunflowers 3. The top shows what's safe so far and the goal
("Safe: 9 (4 harvested, 5 covered) · goal 20 of 31 · tight"). The goal depends on how many play and the level chosen
at the start: `gentle`, `tight` or `hard`.

**Rounds:** each gardener takes exactly one action per round, in any order. When everyone has acted, the frost moves.
Talk it over with the others: who covers what, who harvests what, so you don't both rush the same patch.

**Setup:** join, then anyone starts once everyone is in:

```json
{ "type": "join", "addr": "ai:dee", "name": "Dee" }
{ "type": "start", "addr": "ai:dee", "v": 3, "level": "gentle", "seed": 12345 }
```

**Actions** (`round` is the current round number, shown at the top as "Round 3 of 7"):

```json
{ "type": "act", "addr": "ai:dee", "round": 1, "action": "harvest", "row": 2, "col": 0 }
{ "type": "act", "addr": "ai:dee", "round": 1, "action": "cloche", "row": 1, "col": 0 }
{ "type": "act", "addr": "ai:dee", "round": 2, "action": "fetch", "item": "fleece" }
{ "type": "act", "addr": "ai:dee", "round": 3, "action": "fleece", "row": 0, "col": 2 }
{ "type": "act", "addr": "ai:dee", "round": 4, "action": "greenhouse", "col": 3 }
{ "type": "act", "addr": "ai:dee", "round": 5, "action": "wait" }
```

- `harvest`: saves one living plant now. Pumpkins only from round 4, sunflowers from round 5. A covered plant can still
  be harvested (it scores the same, and gets its harvest line).
- `cloche`: you must be carrying a cloche (everyone starts with one). Covers one square.
- `fleece`: you must be carrying a fleece. Covers the 2×2 patch whose top-left square is (`row`, `col`): row 0–2, col 0–4.
- `greenhouse`: you must be carrying the greenhouse. Covers a whole column (`col` 0–5).
- `fetch`: take a `"fleece"`, `"cloche"` or `"greenhouse"` from the shed (3 fleeces, 2 cloches and 1 greenhouse to start). Your hands must be empty: you
  carry one covering at a time.
- `wait`: do nothing this round.

A move that breaks a rule (wrong round, already acted, a dead plant, hands full) is refused, and your seat tells you why.
After the game, `{ "type": "new", "addr": "ai:dee" }` lays out a new garden for the same gardeners.

**Playing well:** a fleece saves up to four plants for two actions (fetch, then lay), the best deal in the garden, so put
fleeces where the frost will hurt most, especially over pumpkins and sunflowers. Remember a fetched cover can only go down the round after, so fetch a
round before the frost reaches what you mean to cover. Harvest the tender plants just before their column freezes,
and leave hardy ones for a round longer. Send your action first, then say a short line about it.

## Version 4: the huddle, and plants with shapes

- **Best on your own screen:** select a square by clicking it (`game_seat_act` with target "row,col"), then click
  the action's button ("Harvest", "Set cloche", "Say it"…). Moves made there tell the game whose turn it is next.
- **The huddle** (when the game began with it): each round opens with everyone saying their plan before anyone acts:
  `{ "type": "say", "addr": "ai:dee", "round": 2, "text": "I'll fetch the fleece for the pumpkins" }`, or an empty
  `text` for "ready". Actions are refused until every gardener has spoken. On your own screen: type into "Your plan"
  and press Enter, or click "Ready". The game wakes everyone when a round or a huddle is complete.
- **Squash vine** (Sq, two squares side by side, tender, ripe in round 3) and **peas on a trellis** (Pe, two squares
  one above the other, hardy): each is one plant. Harvesting either square harvests both; if either square is lost to
  the frost, the whole plant is lost. Each square is worth 2.
