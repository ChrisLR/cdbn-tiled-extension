# BN Tiled Extension — Work Notes

## What this is
Fork of solerante's CDDA Tiled extension, adapted for **Cataclysm: Bright Nights**.
Source extension file renamed: `CDDA_Map_extension.mjs` → `BN_Map_extension.mjs`.

## BN project path
`/mnt/8e269cce-cf09-45c9-8a83-18504f3c7034/Projects/C++/catabn/Cataclysm-BN`

## Key incompatibilities found & fixed

### 1. Default tileset
- CDDA default was `ChibiUltica` — doesn't exist in BN.
- Changed to `MSX++UnDeadPeopleEdition` (BN's default).
- Other BN tilesets: ASCIITileset, BrownLikeBears, ChestHole16Tileset, HitButton_iso, HoderTileset, RetroDays+Tileset, RetroDaysTileset, UltimateCataclysm.

### 2. Path validation
- Original checked `data/cataicon.ico` + `gfx/tile_config_template.json` (both exist in BN, so OK).
- Added fallback: `cataclysm-bn` or `cataclysm-bn-tiles` binary in root.

### 3. Layer types expanded
- `mapLayerTypes` (symbol-based): added `items`, `toilets`, `vendingmachines`, `liquids`, `fields`, `graffiti`, `computers`, `signs`
- `entityLayerTypes` (place_* arrays): added `place_monster`, `place_terrain`, `place_furniture`, `place_traps`, `place_toilets`, `place_npcs`, `place_signs`, `place_liquids`, `place_vendingmachines`, `place_gaspumps`, `place_graffiti`, `place_rubble`, `place_cave_moss`, `place_computers`, `place_nested`, `place_beyond_bounds`, `place_ter_furn_transforms`, `place_corpses`
- New `mappingLayerTypes` constant for BN's `mapping` key

### 4. BN `mapping` key
- BN mapgen uses `"mapping": { "X": { "terrain": "t_dirt", "traps": "tr_pit", ... } }` — one symbol maps to multiple layer types.
- Added handling in: both `getMapfileCustomPalette` functions, main import symbol loop, `getTilesForSymbolLocations`, `fillTileDict`, `prepareTileDict`.

### 5. Generic resolvers (replaced hardcoded if-chains)
- `chooseIdFromSymbolObject` — now recursive, handles strings/arrays/objects with any known key (`param`, `ter`, `furn`, `trap`, `field`, `item`, `vehicle`, `monster`, `liquid`, `type`, `class`, `group`, `item_group`, `switch`).
- Entity name resolution in `prepareTiledLayer` (3 copies) — generic property lookup instead of per-layer if-chain.
- Export `bn_id_name` resolution — `propMap` lookup for all `place_*` types.

### 6. `no_id_objects` expanded
Added: `mapping`, `mapgensize`, `flags`, `weight`, `om_terrain`, `nested_mapgen_id`, `predecessor_mapgen`, `distribution`, `place_nested`, `parameters`, `sealed_item`, `zones`

### 7. Export template (`BNMapEntry` class)
Added all BN symbol layers and `place_*` arrays.

### 8. Renamed all DDA → BN references
- File: `CDDA_Map_extension.mjs` → `BN_Map_extension.mjs`
- Config: `cdda_tiled_extension_config.json` → `bn_tiled_extension_config.json`
- Extras folder: `cdda_map_extension_extras` → `bn_map_extension_extras`
- Extension-invented property names (NOT BN data keys — verified 0 occurrences in BN's `data/` and `gfx/`):
  - `cdda_id` → `bn_id` (105 occurrences)
  - `cdda_layer` → `bn_layer` (18)
  - `cdda_mapfile` → `bn_mapfile` (1)
  - `cdda_palette_*` → `bn_palette_*` (13)
- Action IDs: `cte_CDDA_*` → `cte_BN_*`, `cte_cdda_*` → `cte_bn_*`
- Class names: `CDDAMapFormat` → `BNMapFormat`, `CDDAMapEntry` → `BNMapEntry`, `CDDAMapEntryImport` → `BNMapEntryImport`
- Variables: `path_to_cdda` → `path_to_bn`, `cddaId` → `bnId`, etc.

## BN data format notes
- Tileset configs (`gfx/<tileset>/tile_config.json`) still use `tiles-new` (same as CDDA).
- Top-level `gfx/tile_config.json` uses `tiles` (not `tiles-new`) — but extension reads per-tileset configs, so OK.
- Mapgen palettes at `data/json/mapgen_palettes/` — same path as CDDA.
- Mapgen files at `data/json/mapgen/` — same structure, plus new keys listed above.

## File uses CRLF line endings
Edits must preserve `\r\n`. Used `perl -i -pe` and `sed` for safe replacements.

## Syntax check
Brace diff = 1 (same as original — from a brace inside a string/regex). Parens balanced.
