# Design Document: Guild System

## Overview

Guilds are organizations based in specific cities that offer services to the player.
When visiting a city with a guild, a popup panel appears in the bottom quarter of the screen
(separate from the sidebar).

---

## Guild Types

Different guilds offer different services. Available guilds:

| Guild | Focus |
|-------|-------|
| Caravan Guild | Travel bonuses, route info |
| Explorers Guild | Map reveals, item discounts |
| Merchants Guild | Trade prices, buy/sell bonuses |
| Smugglers Guild | Black market, risky deals |

---

## Guild Data Structure

```
Guild:
  - id: unique identifier
  - name: display name
  - type: caravan / explorers / merchants / smugglers
  - city: which city it belongs to
  - reputation: player's standing (-100 to +100 - starts at 0)
  - services: list of available services (just the name of the service)
```

---

## Guild Services

Things guilds can offer (unlocked by reputation):

(note: some thing are use-once, some things can be used repeatedly).

**Caravan Guild:**
- Route information (reveal path wear on all visible tiles)
- Camel blessing (next 20 moves cost 1 less food per move, min 1)

**Explorers Guild:**
- Reveal terrain (show all hexes within 5 tiles of the city for rest of game)
- Discount on explorer items (if used, from now on you get 50% off buying compass, map scrolls, boat anywhere)
- Oasis map (marks all oases on your map permanently)

**Merchants Guild:**
- Price report (view prices at 3 other random cities, including their distance)
- Market manipulation (instantly increase price of a good with 2, at this city)

**Smugglers Guild:**
- Contraband tip (reveals which city has highest price - and what it is - for a random good)
- Bribe guards (increase reputation with another guild with 20, only usable once per 7 days)

---

## Reputation System

Each guild tracks player reputation separately (default is 10):

| Level | Reputation | Benefits |
|-------|------------|----------|
| Shunned | -100 to -50 | No services rendered, scared message shown instead! |
| Suspect | -50 to 0 | No services rendered, snide message shown instead! |
| Stranger | 0 to 19 | Basic services only |
| Known | 20 to 49 | Basic services only |
| Trusted | 50 to 79 | Discounted services |
| Honored | 80 to 100 | Top-tier services! |

Reputation gained by:
- Purchasing the guild's services. Each services increases the reputation with a set rate.
- Donating dirhams (each donation is 20 dirhams; this increases reputation with 1-10. Same every time,
but randomized per-guild upon the start of the game. So first game it might be 20 dirhams = +5 rep,
another game might be 20 dirhams = +1 rep).

Reputation lost by:
- Not visiting for many days (decay: -1 per day, down to 10. Also applies to negative reputation, e.g. -10
  reputation becomes 10 after 20 days).

---

## UI: Guild Panel

**Location:** Bottom 1/4 of screen, overlays the map area (not sidebar)

**Trigger:** Automatically appears when entering a city with a guild

**Layout:**
```
┌─────────────────────────────────────────────────┐
│ [Guild Name]                              [X]   │
│ Your Standing: Known (+47)                      │
├─────────────────────────────────────────────────┤
│ Guild services:                                 │
│  • Reveal terrain (15 dirhams)                  │
│  • Discount on explorer items (20 dirhams)      │
│  • [Locked] Oasis map (40 dirhams) (need higher reputation)  │
│                                                 │
└─────────────────────────────────────────────────┘
```

---

## Questions, with answer

1. **How many guilds?** One per city, or only some cities have guilds?

A: Each guild is unique. Distribute randomly among the cities. Each city has none or one guild.

2. **Guild rivalry:** Do guilds conflict? (Helping one hurts another?)

No, not for now.

5. **Membership fee:** Pay to join, or open to all?

You can't join guilds (yet!), they just provide services.

6. **Tie to existing reputation?** Use the unused `state.reputation` or separate per-guild?

Per-guild reputation. Keep state.reputation (it will be used for global reputation later, "become infamous").

---

## Files to Modify

- `/home/ran/src/html/silk_road_caravan/silk_road_caravan.html`
