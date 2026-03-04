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
Primary data source: `data/Richardson Family Circle Data.rmgc` (SQLite database extracted from `family_wheel.rmgb`)
- More reliable than PDF for relationship data
- Contains proper parent-child linkages and marriage records

---

## Questions to Clarify

1. **Divorcees/Remarriages:** How should we handle Coombs (divorced Henry LaBoiteaux, remarried)? Show both spouses? Show only current? Special notation?

2. **Deceased indicators:** Should we mark deceased family members differently? (e.g., italics, dates, cross symbol)

3. **Generation 2+ spacing:** As we add more generations, wedges get narrower. At what point do we abbreviate names or use initials?

4. **Click/hover behavior:** What should happen when someone clicks or hovers on a wedge? Show photo? Bio? Dates?

5. **Color coding:** Should different branches (Roland's, Emily's, Coombs', Adelaide's) have different background colors?
