# CDBN Tiled Extension

A [Tiled](https://www.mapeditor.org/) map editor extension for **Cataclysm: Bright Nights (BN)**. Import BN tilesets, stamp tiles onto maps, edit mapgen properties, and export JSON that BN can load.

Forked from solerante's [CDDA Tiled extension](https://github.com/solerante/cdda-tiled-extension) and adapted for Bright Nights.

## Features

- **Import BN tilesets** — pick any tileset from your BN `gfx/` folder; every BN id gets its own tile for easy stamping
  - Find a tile by its BN id with `Ctrl+F`
  - Save tiles to a "favorites" tileset
  - Use custom palettes
- **Create and edit maps** — stamp tiles, draw shapes, and edit BN properties (`density`, `repeat`, etc.) as custom properties
- **Import and export BN maps** — export mapgen JSON that should load in BN (always verify in-game!)
- **Full BN mapgen support** — all `place_*` entity arrays (`place_terrain`, `place_furniture`, `place_monster`, `place_npcs`, `place_nested`, …), symbol-based layers (`toilets`, `vendingmachines`, `liquids`, `fields`, `graffiti`, `computers`, `signs`, `items`), and BN's `mapping` key (one symbol → multiple layer types)

## Requirements

- Tiled **1.10.0 or later**
- A local checkout of [Cataclysm: Bright Nights](https://github.com/cataclysmbnteam/Cataclysm-BN)

## Installation

1. Copy `BN_Map_extension.mjs` and the `bn_map_extension_extras/` folder into your Tiled extensions folder:
   - **Windows:** `%APPDATA%\Tiled\extensions\`
   - **macOS:** `~/Library/Preferences/Tiled/extensions/`
   - **Linux:** `~/.config/tiled/extensions/`
2. In Tiled, go to **Edit → Preferences → Plugins** and enable the extension.
3. See the [Tiled scripting docs](https://doc.mapeditor.org/en/stable/reference/scripting/#script-extensions) for details.

## Usage

1. Set the path to your BN folder when prompted (the extension looks for `cataclysm-bn` / `cataclysm-bn-tiles` binaries, `data/cataicon.ico`, or `gfx/tile_config_template.json`).
2. **Import a tileset** via the extension's tileset import action — by default it uses `MSX++UnDeadPeopleEdition`, BN's default tileset.
3. Create or open a map, stamp tiles from the imported tileset, and edit BN ids/layers via each tile's custom properties (`bn_id`, `bn_layer`).
4. **Export** to BN mapgen JSON and test it in-game.

## Configuration

Settings are stored in `bn_tiled_extension_config.json` (created next to the extension on first run).

## Differences from upstream (CDDA version)

| Area | Upstream (CDDA) | This fork (BN) |
|---|---|---|
| Default tileset | `ChibiUltica` | `MSX++UnDeadPeopleEdition` |
| Game path detection | `data/cataicon.ico` + `gfx/tile_config_template.json` | Same, plus `cataclysm-bn` / `cataclysm-bn-tiles` binaries |
| Extension file | `CDDA_Map_extension.mjs` | `BN_Map_extension.mjs` |
| Config file | `cdda_tiled_extension_config.json` | `bn_tiled_extension_config.json` |
| Extras folder | `cdda_map_extension_extras/` | `bn_map_extension_extras/` |
| Tile custom properties | `cdda_id`, `cdda_layer`, `cdda_mapfile`, `cdda_palette_*` | `bn_id`, `bn_layer`, `bn_mapfile`, `bn_palette_*` |
| Mapgen keys | CDDA layer set | BN layer set + `mapping` key support |

## License

Licensed under the [Apache License 2.0](LICENSE). Based on the original CDDA Tiled extension by [solerante](https://github.com/solerante/cdda-tiled-extension).
