# CLAUDE.md

This file provides guidance for AI assistants working on the Silk Road Caravan codebase.

## Project Overview

Single-file browser game (`silk_road_caravan.html`) implementing a Silk Road trading simulation with hexagonal grid navigation.

## Architecture

Everything is contained in one HTML file with embedded CSS and JavaScript:

```
silk_road_caravan.html
├── <style> - CSS styling for sidebar, buttons, map container
├── <body> - HTML structure (canvas + sidebar)
└── <script> - All game logic (~670 lines)
```

## Key Data Structures

### State Object (line 180-192)
```javascript
state = {
  funds: 20,              // Currency (dirhams)
  supplies: { food: 25 }, // Consumables
  reputation: 0,          // Unused, for future features
  cargo: { silk, spices, pottery }, // Tradeable goods
  pos: { q, r },          // Current hex position (axial coords)
  prices: {},             // Current city prices
  pathWear: {},           // Tracks travel frequency per hex
  visitedCities: Set,     // Cities discovered
  zoom: 1|2,              // Map zoom level
  items: Set,             // Purchased item IDs
  cityItems: {}           // Random items available per city
}
```

### Map Data
- `coords` (line 86-91): Array of all valid hex coordinates
- `cities` (line 125-135): City definitions with position and metadata
- `oases` (line 137-144): Oasis rest stops
- `gameItems` (line 150-156): Purchasable upgrade items

## Core Functions

### Hex Grid Math
- `hexToPixel(q, r)`: Convert axial coords to canvas pixels (line 93)
- `pixelToHex(x, y)`: Convert click position to axial coords (line 102)
- `hexDistance(a, b)`: Manhattan distance between hexes (line 99)
- `axialRound(q, r)`: Round fractional coords to nearest hex (line 109)

### Game Logic
- `moveToHex(q, r)`: Handle movement, costs, path wear (line 608)
- `getMovementCost(q, r)`: Calculate food cost based on terrain/wear (line 216)
- `generatePrices()`: Set commodity prices at current city (line 229)
- `marketFluctuation()`: Random price changes (line 242)
- `updatePathWear()`: Decay path wear over time (line 207)

### Trading
- `buyOneItem/buyAllItem(item)`: Purchase commodities (line 521, 504)
- `sellOneItem/sellAllItem(item)`: Sell commodities (line 540, 552)
- `buyItem(itemId)`: Purchase upgrade items (line 413)

### UI
- `updateUI()`: Refresh sidebar display (line 432)
- `drawMap()`: Render hex grid on canvas (line 278)
- `showToast(msg)`: Temporary notification (line 65)
- `addLog(msg)`: Append to journey log (line 73)

## Coordinate System

Uses axial hex coordinates (q, r):
- Origin (0, 0) is Chang'an at center
- Grid radius is 6 hexes
- Third coordinate s = -q - r (implicit)

## Path Wear System

Hexes track travel frequency in `state.pathWear`:
- Each traversal adds +20 wear
- Wear decays by 1 each move
- Higher wear (max 200) = lower movement cost
- Visual brown tinting shows worn paths

## Development Notes

### Adding New Cities
Add to `cities` array (line 125) with:
```javascript
{ q: X, r: Y, name: 'CityName', emoji: '🏛️' }
```

### Adding New Items
Add to `gameItems` array (line 150) with:
```javascript
{ id: 'item_id', name: 'Display Name', price: N, description: 'Effect' }
```
Then implement the effect check using `state.items.has('item_id')`.

### Adding New Commodities
1. Add to `state.cargo` initial values (line 184)
2. Add price generation in `generatePrices()` (line 229)
3. UI automatically picks up from `state.prices`

### Testing Changes
Simply refresh the browser - no build step required.

## Potential Improvements

- Save/load game state to localStorage
- Add more random events during travel
- Implement the reputation system
- Add bandit encounters or other hazards
- Multiple caravan routes or difficulty levels
