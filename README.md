# Silk Road Caravan

A browser-based trading game where you lead a caravan along the ancient Silk Road, buying and selling goods across cities from Chang'an to Antioch.

## How to Play

Open `silk_road_caravan.html` in any modern web browser.

### Objective

Travel between cities, trade goods for profit, and visit all 9 cities along the Silk Road while managing your supplies and funds.

### Core Mechanics

**Movement**
- Click adjacent hexes to move your caravan
- Desert tiles cost 1-5 food depending on path wear (well-traveled paths cost less)
- Cities and oases cost 1 food (or 0 with Camel Shoes)
- You can only see 2 hexes around you (3 with Map Scrolls)

**Trading**
- Buy and sell silk, spices, and pottery at cities
- Prices vary by location and fluctuate randomly over time
- Buy low in one city, sell high in another

**Resources**
- **Funds (dirhams)**: Currency for trading and resupplying
- **Food**: Consumed when moving; run out and you're stranded
- **Cargo**: Limited capacity (20 items, or 30 with Silk Bags)

**Resting**
- Rest at cities or oases for 5 dirhams to restore food
- Oases provide bonus food restoration
- Random events may occur while resting

### Items

Purchase special items at cities for permanent bonuses:

| Item | Price | Effect |
|------|-------|--------|
| Compass | 30 | Reduces desert movement cost by 1 |
| Food Preserves | 24 | Increases max food to 35 |
| Silk Bags | 40 | Increases cargo capacity to 30 |
| Map Scrolls | 36 | Increases view distance to 3 hexes |
| Camel Shoes | 20 | Cities/oases cost no supplies |

### Cities

- Chang'an (starting city)
- Samarkand
- Antioch
- Kashgar
- Delhi
- Tashkent
- Bukhara
- Baghdad
- Merv

### Tips

- Worn paths (brown trails) reduce travel costs - stick to routes you've used before
- Check the "Cities Visited" checklist to track your progress
- Market prices fluctuate - watch for opportunities
- Always keep enough funds to resupply at the next city or oasis

## Controls

- **Click hex**: Move to adjacent tile
- **Zoom button (top-right)**: Toggle 1x/2x zoom
- **Trade buttons**: Buy/sell goods at cities
- **Rest button**: Restore supplies at cities/oases

## Technical Details

Single-file HTML application with no external dependencies. Uses:
- HTML5 Canvas for hex grid rendering
- Vanilla JavaScript for game logic
- CSS for sidebar UI styling
