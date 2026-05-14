# Luxury Hotel Map — Project Context for Claude Code

## Project Overview
A single-file interactive luxury hotel map (`index.html`) built with Leaflet.js + CartoDB tiles. It plots ~1,011 luxury hotels worldwide, color-coded by brand group. The file is self-contained — all data, styles, and logic live in one HTML file.

---

## File Structure
- **`index.html`** — the entire app (was originally named `four-seasons-map.html`, now at 1,155 hotels)
- **`CLAUDE.md`** — this file

---

## Tech Stack
- **Leaflet.js** (map rendering, pins, popups)
- **CartoDB** tile layer (dark/light basemap)
- **Vanilla JS** — no build step, no npm, no frameworks
- Single HTML file with embedded CSS and JS

---

## Data Format
All hotels live in a JS array called `FS` (legacy name, ignore it):

```javascript
const FS = [
  {
    name: "Hotel Name",
    city: "City, Country",
    region: "North America",        // see Regions below
    lat: 40.7614,
    lng: -73.9743,
    tags: ["Beach", "Spa", "Golf", "Ski"],  // subset of valid tags
    brand: "Marriott",              // see Brand Values below
    rooms: 238,                     // integer or null
    opened: 1904,                   // year integer or null
    renovated: null,                // year integer or null
    stars: null,                    // not currently used
    url: "https://..."              // direct hotel URL
  },
  ...
];
```

### Valid Regions
```
"North America"
"Central & South America"
"Europe"
"Middle East & Africa"
"Asia & Pacific"
```

### Valid Tags
```
"Beach"
"Spa"
"Golf"
"Ski"
```

### Valid Brand Values
```
"Four Seasons"
"Hyatt"
"Hilton"
"Marriott"
"IHG"
"Accor"
"Boutique"
```

---

## Brand Colors (Pin Colors)
```javascript
const BRAND_COLOR = {
  'Four Seasons': { pin: '#c8a96e', border: '#a08050' },  // gold
  'Hyatt':        { pin: '#4B77A9', border: '#3a5f8a' },  // metro blue
  'Hilton':       { pin: '#39FF14', border: '#28cc0f' },  // neon green
  'Marriott':     { pin: '#B41F3A', border: '#8f1830' },  // deep red
  'IHG':          { pin: '#E15F27', border: '#b54a1e' },  // orange
  'Accor':        { pin: '#FFCB05', border: '#cc9f00' },  // yellow/gold
  'Boutique':     { pin: '#a9916e', border: '#8a7350' },  // tan
};
```

---

## Brands Currently Mapped (1,011 hotels total)

### Four Seasons (144 pins — gold)
- Four Seasons Hotels & Resorts — full global portfolio

### Hyatt (285 pins — metro blue)
All use `brand: "Hyatt"`. Sub-brands identified by name:
- **The Standard** (12) — name starts with "The Standard"
- **Thompson Hotels** (20) — name starts with "Thompson"
- **Alila** (19) — name starts with "Alila"
- **Andaz** (34) — name starts with "Andaz"
- **Park Hyatt** (52) — name starts with "Park Hyatt" or "Palacio Duhau Park Hyatt" or "Hyatt Hotel Canberra"
- **Unbound Collection** (67) — name ends with "- Unbound"
- **Grand Hyatt** (75) — name starts with "Grand Hyatt"

### Hilton (320 pins — neon green)
All use `brand: "Hilton"`. Sub-brands by name:
- **Signia by Hilton** (8) — name starts with "Signia by Hilton"
- **LXR Hotels & Resorts** (20) — name contains "LXR Hotels & Resorts"
- **Waldorf Astoria** (44) — name starts with "Waldorf Astoria" or contains "A Waldorf Astoria"
- **Conrad** (55) — name starts with "Conrad"
- **Canopy by Hilton** (52) — name starts with "Canopy by Hilton"
- **Curio Collection** (144) — name ends with "- Curio"

### Marriott (87 pins — deep red)
All use `brand: "Marriott"`. Sub-brands by name:
- **EDITION** (21) — name ends with "EDITION"
- **St. Regis** (61) — name starts with "The St. Regis" or "St. Regis"

### IHG (158 pins — orange)
All use `brand: "IHG"`. Sub-brands by name:
- **Regent** (16) — name starts with "Regent" or "Carlton Cannes"
- **Vignette Collection** (27) — name ends with "Vignette Collection"
- **Six Senses** (27) — name starts with "Six Senses"
- **Kimpton** (88) — name starts with "Kimpton" or "Onyria Quinta"

### Accor (167 pins — yellow/gold)
All use `brand: "Accor"`. Sub-brands by name:
- **Raffles** (24) — name starts with "Raffles" or "Le Royal Monceau"
- **Banyan Tree** (40) — name starts with "Banyan Tree" or "Mandai Rainforest" or "Mamula Island" or "Ubuyu"
- **Emblems Collection** (8) — name ends with "Emblems Collection"
- **Fairmont** (94) — name starts with "Fairmont" or "The Savoy" or "The Plaza Hotel" or "Makkah Clock Royal Tower" or "Rome Cavalieri" or "Grand Wailea" or "The Roosevelt New Orleans" or "Boca Raton Beach Club"

---

## UI Features
- **Top bar**: Logo, search box, nav links (Blog, Brands, Hotel of the Day), Log In button, dark mode toggle
- **Region chips** (below top bar): All regions + individual regions — not yet wired to filter logic
- **Left sidebar filters**:
  - Brand (checkbox per brand group)
  - Hotel Collections (Edit, FHR, PHR, LHW, SLH, R&C) — not yet wired
  - Size (<50, 50–150, 151–400, 400+ rooms)
  - Stars (5, 4.5, 4) — not yet wired
  - Amenities (Beach, Golf, Spa, Ski)
- **Right panel**: Hotel list cards showing name, city·region, amenity tags, Rooms | Opened | Renovated
- **Map pins**: Colored circles, click to open popup with hotel details and link
- **Dark mode**: Fully styled, toggled via ☽/○ button

---

## Key Conventions & Rules
1. **Always include Coming Soon properties** — if a brand page lists upcoming hotels, add them with `opened: null` or the expected year
2. **No duplicates** — check existing entries before adding; deduplication matters especially for China-heavy brands
3. **Unbound Collection suffix** — all Unbound Collection hotels must have `" - Unbound"` appended to their name
4. **Vignette Collection suffix** — all Vignette Collection hotels must have `", Vignette Collection"` appended to their name
5. **Coordinates** — use accurate lat/lng; for island/resort properties be precise, not just city center
6. **URL format** — use the direct hotel page URL, not a search results page
7. **Null vs omit** — always include all fields; use `null` for unknown values, never omit a field
8. **JS comments** — do NOT add `//` comments inside the FS array; they break JSON-adjacent parsing in some contexts
9. **Array integrity** — the array must close with `};` on the last entry followed by `\n];` — always verify after edits

---

## How to Add a New Brand
1. Add a new color entry to `BRAND_COLOR` in the JS (or reuse an existing `brand` value)
2. Fetch the full hotel list from the brand's official website
3. Include all open + coming soon properties
4. Append entries to the `FS` array before the closing `];`
5. Verify total count matches expected brand portfolio size
6. Commit and push

---

## Deployment
- Hosted on GitHub Pages
- Single file — just replace `index.html` and push; site updates in ~60 seconds
- No build step, no dependencies to install

---

## Session History Summary
This project was built over multiple chat sessions. The data was populated brand by brand:
- Four Seasons (full global)
- Hyatt: Standard, Thompson, Alila, Andaz, Park Hyatt, Unbound Collection, Grand Hyatt
- Hilton: Signia, LXR, Waldorf Astoria, Conrad, Canopy
- Marriott: EDITION, St. Regis
- IHG: Regent, Vignette, Six Senses, Kimpton
- Accor: Raffles, Banyan Tree, Emblems Collection, Fairmont

## Brands Still To Add (potential roadmap)
- Marriott: Ritz-Carlton, Luxury Collection, W Hotels, JW Marriott
- Hyatt: Hyatt Regency (luxury tier), Alila (remaining)
- IHG: InterContinental
- Accor: Sofitel Legend, Orient Express Hotels
- Independent: Aman Resorts, Rosewood Hotels, Belmond, Oetker Collection, Capella Hotels
