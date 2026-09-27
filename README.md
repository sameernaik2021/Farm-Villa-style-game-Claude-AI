# 🌾 Wheatgrove Farm

A self-contained, single-file **Farm Villa–style** farming/management game, built in plain HTML, CSS, and JavaScript — no build tools, no dependencies, no server required.

Based on the game design breakdown in `Farm_Villa_style_game.txt`, this implements the **MVP feature set** described in that document (Section 54): a farm grid, crops, animals/production buildings, inventory with storage limits, coins & XP progression, customer orders, land expansion, and save/load.

## How to run

Just open `farm-villa.html` in any modern web browser (Chrome, Firefox, Safari, Edge). That's it — everything (styles, game logic, and assets) is inlined in the one file.

Your progress saves automatically:
- Every 15 seconds
- Whenever you close/refresh the tab
- Whenever you click **💾 Save**

Progress is stored in your browser's `localStorage`, under the key `wheatgrove_save`, so it persists between visits on the same browser/device.

## Gameplay overview

| System | Description |
|---|---|
| **Farm Grid** | 24 plots, starting with 8 unlocked. Unlock more land for 🪙200 each. |
| **Crops** | Wheat, Corn, and Tomato — each with a seed cost, grow time, sell/harvest yield, and XP reward. Higher-tier crops unlock at higher player levels. |
| **Growing** | Click an empty plot to plant your selected seed. Crops visibly progress (🌱 → 🌿 → ready icon) with a live countdown bar. Click a ready plot to harvest. |
| **Buildings** | Chicken Coop, Windmill, and Bakery — each consumes raw ingredients (e.g. wheat) and produces higher-value goods (egg, flour, bread) over time. Unlock by player level, buy in the Shop. |
| **Storehouse** | All harvested/produced goods count against a shared storage cap. Upgrade capacity in the Shop when it fills up. |
| **Customer Orders** | A rotating board of 3 orders requesting specific goods, paying coins + XP. Refresh the board for 🪙20 if you don't like the current requests. |
| **Progression** | Earning XP (from harvesting, producing, and fulfilling orders) levels you up, which gates access to new crops, buildings, and land. |

## Project structure

```
farm-villa.html   → the entire game (HTML + CSS + JavaScript, single file)
README.md         → this file
```

Inside `farm-villa.html`, the JavaScript is organized into clear sections:

```
DATA          → crop, building, and item definitions (easy to tune/extend)
STATE         → save/load, fresh-game state shape
HELPERS       → inventory math, XP/leveling, time formatting
FARM GRID     → planting, growing, harvesting, land unlocking
BUILDINGS     → production start/collect logic
SHOP          → buying buildings and storage upgrades
ORDERS        → generating and fulfilling customer orders
INVENTORY/HUD → rendering coins, XP bar, storehouse
RENDER ALL    → single re-render entry point after any state change
TABS          → Farm / Buildings / Orders / Shop navigation
INIT & LOOP   → load save, render, and a 1s tick for timers
```

## Extending the game

The design doc outlines a much larger scope (Section 1–57) than this MVP. Straightforward next additions, following the same patterns already in the code:

- **More crops/buildings**: add entries to the `CROPS` or `BUILDING_DEFS` objects — the UI (seed bar, shop, buildings panel) renders from these automatically.
- **Quests**: a `QUESTS` array + a progress tracker, similar to how `orders` works.
- **Decorations**: a new tab plus a `decorations` array in state, placed on the grid like buildings.
- **Sound/animation**: hook into the existing render points (e.g. on harvest, on order fulfilled) to trigger audio/CSS transitions.
- **Server-authoritative timers / backend**: the doc's Section 6 and 44–45 explain why production timers (`startedAt`/`finishAt`) should ultimately be validated server-side rather than trusted from the client, for any real multiplayer/live version.

## Known limitations (by design, per the MVP scope)

- No multiplayer, social features, or leaderboards
- No decorations or ambient animation/sound
- All game logic runs client-side — fine for a single-player local game, but not suitable as-is for a real-money or competitive online game (see Section 44 of the design doc on server validation)
- Balancing (costs, timers, rewards) is a first pass and can be tuned in the `DATA` section
