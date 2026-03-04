# Family Tree Development Practices

Rolling log of decisions for the family wheel visualization.

---

## 2024-XX-XX - Text Orientation & Positioning

### Decision: Blood relative positioning
- **Blood relative is ALWAYS radially closest to the previous generation ring (toward center)**
- **Spouse is ALWAYS radially furthest from center (toward outer edge)**
- "m." sits between blood relative and spouse
- Text rotation (180° flip on bottom half) handles readability but does NOT change radial positioning

### Radial positioning (consistent across all quadrants):
```
[Center] ←── Blood Relative ←── m. ←── Spouse ←── [Outer Edge]
```

The text is rotated for readability (so you don't read upside-down), but the blood relative is always physically closer to the center circle regardless of which quadrant.

---

---

## Data Structure Decisions

### Spouse data structure
Changed from single `spouse` string to `spouses` array of objects:
```javascript
spouses: [
  { name: "Clarence Burton", order: 1 },
  { name: "Jim Carruthers", order: 2 }
]
```

**Rationale:** Gen 1 has two people with remarriages (Coombs Richardson and Emily Richardson). Array preserves marriage order and is extensible for future needs (e.g., adding children-per-marriage linkage).

**Display:** Currently showing only first spouse (`spouses[0].name`). Future enhancement could show remarriages with different styling.

### Two Emily Richardsons
The family tree contains TWO people named Emily Richardson:
1. **Emily Richardson (ID 5)** - Gen 1, child of Annis & James
   - Married: Clarence Burton (1st), Jim Carruthers (2nd)
2. **Emily Richardson (ID 16)** - Gen 2, daughter of Roland Richardson & Althea Ford
   - Married: Joe Boughton (ancestor of the Boughton family)

### Data source
Primary data source: **`data/family_tree.json`** (extracted from RootsMagic database)
- Clean, readable JSON with Gen 0, 1, and 2 fully populated
- IDs match PersonID in original `Richardson Family Circle Data.rmgc` SQLite database
- For Gen 3+, extract more data from the .rmgc file using sqlite3 queries

Original source: `data/Richardson Family Circle Data.rmgc` (SQLite database)
- More reliable than PDF for relationship data
- Contains proper parent-child linkages and marriage records

---

## Color Ramp (for generation coloring)

**Current implementation:** Light pastels at 60% opacity with dark text (#1a2e1e)
```javascript
const genColors = [
  "rgba(135, 206, 235, 0.6)", // Gen 0 (center) - Sky blue
  "rgba(255, 215, 100, 0.6)", // Gen 1 - Gold
  "rgba(255, 100, 100, 0.6)", // Gen 2 - Light red
  "rgba(152, 251, 152, 0.6)", // Gen 3 - Pale green
  "rgba(200, 162, 200, 0.6)", // Gen 4 - Lavender
  "rgba(255, 160, 122, 0.6)", // Gen 5 - Light coral
  "rgba(127, 255, 212, 0.6)", // Gen 6 - Aquamarine
  "rgba(240, 230, 140, 0.6)"  // Gen 7+ - Khaki/pale yellow
];
```

**Rejected alternatives (too dark/aggressive):**
- Forest depth: Deep forest → emerald → teal → sage → seafoam → mint
- Earth to sky: Deep brown → terracotta → amber → sage → sky blue → lavender
- Cool to warm: Deep navy → indigo → purple → magenta → coral → gold
- Categorical (Tableau10): Each generation completely different hue

---

## Questions to Clarify

1. **Divorcees/Remarriages:** How should we handle Coombs (divorced Henry LaBoiteaux, remarried)? Show both spouses? Show only current? Special notation?

2. **Deceased indicators:** Should we mark deceased family members differently? (e.g., italics, dates, cross symbol)

3. **Generation 2+ spacing:** As we add more generations, wedges get narrower. At what point do we abbreviate names or use initials?

4. **Click/hover behavior:** What should happen when someone clicks or hovers on a wedge? Show photo? Bio? Dates?

5. **Color coding:** Should different branches (Roland's, Emily's, Coombs', Adelaide's) have different background colors?
