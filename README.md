# Palette Forge

A single, self-contained HTML tool for creating and batch-baking Ragnarok Online sprite palettes (`.spr` / `.pal`). Everything runs locally in your browser — no install, no server, no internet connection required. Just open the `.html` file.

## Features

- **Canonical palette layout model** — auto-detects Baked-in vs Custom palette layout and converts between them, so you're never fighting inconsistent index orders between files.
- **Area auto-detection** — automatically groups palette indices into body-part areas (hair, cape, armor, boots, etc.) so you can recolor or lock a whole part at once.
- **Shading-preserving recolor** — recolors keep each index's relative brightness, so results stay "painted" instead of flat.
- **Mount support** — detects mount/rider palette regions and keeps the rider synced with the character automatically, with a magenta-exclusion guard to avoid false positives.
- **Schemes & Macro system** — build reusable recolor recipes and shade/lock presets, then batch-bake full packs (colors × shades × locks) across a class and its mounts in one export.
- **Flexible export** — character alias and gender suffix support, single `.pal` saves, or batch `.zip` export with UTF-8-safe filenames.
- **Offline ColorHunt import** — paste a ColorHunt link or raw hex codes to add custom curated palettes, no network call needed.

## Quick Start

1. Open `palette-forge.html` in a modern browser (Chrome/Edge/Firefox).
2. Click **Open SPR** and choose a class sprite. The sprite preview and its palette load on the right.
3. Pick a tab depending on what you want:
   - Tweak one color or one body part → **Palette** / **Areas**
   - Make many recolored variants → **Generate** / **Schemes**
   - Bake a whole pack for a class → **Macro**
4. Export a single `.pal` (**Export** tab) or a batch `.zip` (**Schemes** / **Macro** tabs).

For the full tab-by-tab reference, workflows, and troubleshooting tips, see **[GUIDE.md](./GUIDE.md)**.

## License

Free to use, modify, and redistribute. Not for resale. Credit back is appreciated but not required. See [LICENSE](./LICENSE) (CC BY-NC 4.0).

## Feedback

Bug reports, feature suggestions, and pull requests are all welcome.
