# Palette Forge — User Guide & How-To

Palette Forge is a single, self-contained HTML tool for creating and batch-baking Ragnarok Online sprite palettes (`.spr` / `.pal`). Everything runs locally in your browser — there is no install, no server, and no internet connection is required. Just open the `.html` file.

## Contents

1. [Quick start](#1-quick-start)
2. [Key concepts you should know](#2-key-concepts-you-should-know)
3. [The toolbar (top bar)](#3-the-toolbar-top-bar)
4. [Tab: Palette](#4-tab-palette)
5. [Tab: Areas](#5-tab-areas)
6. [Tab: Adjust](#6-tab-adjust)
7. [Tab: Generate](#7-tab-generate)
8. [Tab: Remap](#8-tab-remap)
9. [Tab: Mount](#9-tab-mount)
10. [Tab: Schemes](#10-tab-schemes)
11. [Tab: Macro (groups, masters, scheme sets)](#11-tab-macro-groups-masters-scheme-sets)
12. [Tab: Export](#12-tab-export)
13. [Tab: Info](#13-tab-info)
14. [The colour picker](#14-the-colour-picker)
15. [Adding palettes from ColorHunt](#15-adding-palettes-from-colorhunt)
16. [Where your data is saved](#16-where-your-data-is-saved)
17. [Common workflows (recipes)](#17-common-workflows-recipes)
18. [Tips & gotchas](#18-tips--gotchas)

---

## 1. Quick start

1. Open `Palette_Forge.html` in a modern browser (Chrome/Edge/Firefox).
2. Click **Open SPR** and choose a class sprite (`.spr`). The sprite preview and its 256-colour palette load on the right.
3. Pick a tab depending on what you want:
   - Tweak one colour or one body part → **Palette / Areas**
   - Make many recoloured variants → **Generate / Schemes**
   - Bake a whole pack for a class → **Macro**
4. When you're happy, use **Export** (single `.pal`) or the **Schemes/Macro** tabs (batch `.zip`).

---

## 2. Key concepts you should know

**Palette (`.pal`)**
A list of 256 colours (indexed). Each pixel in the sprite stores an index (0–255), not a colour, and looks up its colour in the palette. Recolouring a sprite means changing the palette, never the pixels.

**Index 0 / magenta transparency**
Index 0 is transparent. The client also treats pure magenta (`FF00FF`) as transparent (a "chroma key"). Palette Forge never recolours index 0 or any magenta slot, so your transparency stays intact. If a custom palette has a stray near-magenta or blank/desaturated slot in the middle of a colour ramp, it can show up as an unexpected grey/transparent patch — worth checking your source scheme if you see that.

**Areas**
An "area" is a group of palette indices that belong to the same body part (hair, cape, armour, boots, ...). Palette Forge can auto-detect these so you can recolour or lock a whole part at once instead of clicking 30 swatches.

**Shading is preserved**
When you recolour an area or apply a scheme, the tool keeps each index's relative brightness (the shading ramp) and only changes the hue/saturation. That's why recolours still look "painted" and not flat.

**Baked-in vs Custom layout**
Different palette sources order their colours differently. Gravity's embedded layout ("Baked-in") and community/Kamish-style layouts ("Custom") don't match. Palette Forge detects the layout on load, works internally in the sprite's native order, and converts back to whichever layout you choose on export. The "Baked-in / Custom" toggle (toolbar and Export tab) controls the order of exported files. If a baked palette shows up scrambled in-game, flip this.

**Gender suffix**
RO palette filenames end in a gender tag. Palette Forge uses the on-disk byte form: Male = `_³²`, Female = `_¿©` (these are the EUC-KR Korean tags shown as "mojibake" in a Latin-1 view). Set this in the Export tab; it is applied to every exported filename.

**Scheme**
A reusable recipe that assigns a hue/treatment to each area. Roll a batch of them on the Schemes tab and save them as a library (`.json`). Schemes drive the Generate, Schemes, and Macro batch exports.

**Master (Macro tab)**
A reusable "shade + lock" preset: Saturation / Brightness / Contrast plus a per-area treatment (Free / Lock / Black / White / Desat). A master carries no scheme of its own — you apply it to one or more scheme sets.

**Mount**
A second sprite (Peco, dragon, etc.) whose rider shares the character's body palette. Palette Forge keeps the rider in sync with the character automatically and lets you theme the beast separately.

---

## 3. The toolbar (top bar)

| Control | Description |
|---|---|
| Open SPR | Load a class sprite (`.spr`). The usual starting point; gives the tool the pixels (for the preview) and the baked palette. |
| Open PAL | Load just a `.pal` file (256 colours) onto the current sprite. |
| Save PAL | Save the current palette as a `.pal` (quick single save). |
| Reset | Revert the palette to the sprite's original baked colours. |
| Baked-in / Custom | Layout toggle (see "Baked-in vs Custom" above). Affects the order colours are written on export. |
| 🌙 | Light / dark theme. |
| ▶ / − / + | Sprite preview: play animation, zoom out, zoom in. |
| Tab buttons | Palette, Areas, Adjust, Generate, Remap, Mount, Schemes, Macro, Export, Info. |

---

## 4. Tab: Palette

The raw 256-colour grid plus a single-colour editor.

- **Colors (256)** — The full palette grid. Click any swatch to select it.
- **Edit selected color** — R / G / B fields and a "Pick" swatch button (opens the colour picker — see section 14) to set the exact colour of the selected index.
- **Area** — Selects the whole area that the chosen index belongs to (handy to jump from one swatch to its whole body part).
- **Lock** — Lock the selected index so generators/adjusters never touch it.

**How to: change one exact colour**
1. Click the swatch in the grid.
2. Click "Pick" and choose a colour (or type R/G/B), then Save PAL/Export.

---

## 5. Tab: Areas

Work on whole body parts at once.

- **Auto-detect** — Scans the palette and groups indices into areas. There is a granularity control (coarser = fewer, bigger areas; finer = more, smaller areas).
- **Detected areas** — The list of areas. Select one to act on it.
- **Select on sprite** — Flashes the selected area's pixels on the preview so you can see exactly what it covers.
- **Recolor (keep shading)** — Recolour the selected area to a chosen colour while preserving its light-to-dark shading. Pick the target colour with the swatch (colour picker).
- **Lock area** — Lock every index in the area.
- **Tune selected area** — Live Saturation / Brightness / Contrast sliders that affect only the selected area. Great for nudging one part without disturbing the rest.

**How to: recolour just the cape to teal**
1. Auto-detect areas.
2. Click the cape area (use "Select on sprite" to confirm).
3. Open the recolour swatch, pick teal, click "Recolor (keep shading)".

---

## 6. Tab: Adjust

Whole-palette tone shifting.

- **Hue / Sat / Bri / Con** — Shift the entire palette's hue, saturation, brightness and contrast at once (locked + transparent slots are left alone).
- **Reset sliders** — Return sliders to neutral.
- **Bake in** — Commit the current slider result into the palette so it becomes the new baseline.

Use this for quick global mood changes (e.g. "make the whole thing cooler/darker") before exporting or before generating variants.

---

## 7. Tab: Generate

Create recoloured palettes from colour theory or a curated palette.

- **Color scheme** — The harmony rule (e.g. monochromatic, complementary, analogous, ...). Used when no curated palette is picked.
- **Base color** — The seed colour (opens the colour picker). Important: the generator respects the base colour's saturation and lightness, not only its hue — so a brown base makes muted browns, a near-white base makes light greys, a near-black base makes dark greys, and a vivid base stays vivid.
- **Randomize base each roll** — Picks a fresh random base colour on every roll.
- **Curated palette** — A fixed set of colours (Sunset Glow, Ocean Breeze, ...) distributed across the areas. Picking one overrides the colour-scheme rule above. "Color theory (off)" turns the curated palette back off. You can add your own here (see section 15). The colours are used as a hue family spread over the areas — not a literal 4-swatch fill.
- **Color balance** — How hues are distributed across areas (e.g. "Free — one hue per area").
- **Spread** — Hue jitter: high = hues spaced evenly/separated, low = clustered together.
- **Modifiers (Sat/Bri/Con)** — Global tweaks layered on top of the generated result.
- **Respect areas** — One hue per detected area (keeps parts visually distinct).
- **Roll preview** — Generate a candidate and preview it on the sprite.
- **Apply** — Commit the previewed generation to the palette.

**How to: generate a muted brown outfit**
1. Open the Base color picker, dial in a brown (low-ish saturation, mid-dark).
2. Choose a scheme (monochromatic works well) or leave color theory on.
3. Roll preview until you like it, then Apply.

---

## 8. Tab: Remap

For working across the Baked-in ↔ Custom layout boundary.

- **Build color map** — Builds the mapping between the baked-in index order and a custom (e.g. Kamish) order, by matching nearest colours.
- **Lock map** — Freezes the current mapping so later edits use it consistently.
- **Export index structure** — Controls how indices are written out.

Most users only need this if their palettes come from a creator whose layout differs from Gravity's, and they see colours landing in the wrong slots.

---

## 9. Tab: Mount

Add mounts so a recolour produces matching character + mount palettes.

- **Add mount (.spr)** — Load a mount sprite. The tool finds the "rider" indices (the part of the mount palette that mirrors the character body) and the "beast" indices (the mount itself).

**How mounts behave:**
- The rider automatically follows the character's recolour, so a character's skin always matches its mounted version — you never theme it twice.
- The beast areas are themed by the scheme and can be locked/treated separately.
- You can give a mount aliases (alternate base filenames) so one bake writes the palette under several mount names at once.

Mounts are picked up automatically by the Schemes and Macro batch exports when "include mounts" is ticked.

---

## 10. Tab: Schemes

Build a library of schemes (the building blocks for batch exports).

- **Roll #** — How many schemes to roll at once.
- **Roll** — Generate that many schemes into the library.
- **Save current** — Save the current on-screen palette as a scheme.
- **Variation** — Hue separation between rolled schemes (higher = the schemes differ from each other more).
- **Tone variety** — How much tone/shade varies between rolled schemes.
- **Scheme library** — The saved schemes.
- **Clear all** — Empty the library.
- **Import .json / Export .json** — Load or save the whole library as a `.json` file. This `.json` is exactly what the Macro tab imports as a "scheme set".
- **Adjust before export (Sat/Bri/Con)** — Tone tweak applied to every scheme at bake time.
- **Reset to neutral** — Zero those adjust sliders.
- **Start #** — The first palette number used when baking.
- **Also create mount palettes** — Include mount palettes in the bake.
- **Export all as .zip** — Bake every scheme in the library into `.pal` files for the current class (numbered from Start #), zipped.

**How to: make a 20-variant pack for one class**
1. Load the class sprite, auto-detect areas.
2. Set Roll # = 20, tune Variation/Tone variety, click Roll.
3. Set Start #, tick mounts if needed, click "Export all as .zip".

---

## 11. Tab: Macro (the big batch planner)

The Macro plans a whole pack — many colours × many shade/lock presets × mounts — in one configuration, then exports it all with neat, contiguous file numbers. Three building blocks:

**Scheme sets**
Named scheme libraries (the `.json` you export from the Schemes tab). Import one per colour family, e.g. "red", "green", "blue".
- Name it, click "Import set .json", choose the file.
- Duplicate filenames are auto-suffixed so importing several never silently overwrites.

**Groups**
A group selects which scheme sets (colours) it covers, and in what order. Groups run in sequence, so you can stage blocks — e.g. a "Basics" group before a "Colourful" group — while each block keeps its own internal order.

**Masters**
A master is a reusable Sat/Bri/Con + per-area treatment preset. Each master has "Apply to" checkboxes for the group's colours, so one master runs against several colours at once. Per-area treatments:
- **Free** = recoloured by the scheme (normal)
- **Lock** = keep the sprite's original colour for that area
- **Black** = fixed neutral dark ramp
- **White** = fixed neutral light ramp
- **Desat** = greyscale at original brightness

Click a master's "Areas" row to open the per-area editor (click an area name to flash it on the sprite). Black/White/Desat are fixed: they're computed from the original baked sprite and are not affected by the master's Sat/Bri/Con or by the scheme, so locked-to-neutral parts stay identical across every colour and shade.

**Export order**
`group → colour (scheme set) → master → scheme`
So every palette of one colour stays contiguous (all reds, then all greens, ...), groups append in order, and file numbers run flat with no gaps. The breakdown under the export button shows each pass (group × colour × master, id range, S/B/C, lock summary) so you can verify before exporting.

Bakes a full, structured pack (colours × shades × locks) for a class in one export.

**Panel**
Shows the full plan (scheme set, master, id range, S/B/C, lock summary) so you can verify before exporting.

**Controls**
- **Add group / Add master / the ⧉ icons** — duplicate a master (inserts after) or a whole group. ✕ removes.
- **Start #** — first file number.
- **Include mount palettes / Export full pack .zip** — bakes character + all loaded mounts for every pass.
- **Import macro .json / Export macro .json** — save or share the entire plan, including every master's Sat/Bri/Con and per-area locks.

**Persistence / reuse across characters**
The whole plan — groups, masters, and their per-area locks — is saved on the device and is no longer cleared when you load a new sprite. Because area treatments are keyed by area position, the same class's other characters share the same layout, so your locks carry straight over: load the next character, export again. (Different class? Areas differ, so re-check or re-import a suitable macro.)

**How to: bake red/green/blue × light/dark/locked in order**
1. Schemes tab: roll & Export `.json` three libraries; import them on the Macro tab as sets "red", "green", "blue".
2. Add a group, tick red/green/blue.
3. Add three masters (e.g. "Light", "Dark", "Locked"), set each one's Sat/Bri/Con and Areas, and tick which colours each applies to.
4. Set Start #, tick mounts if wanted, "Export full pack .zip".

Output: all reds (× each master), then all greens, then all blues.

---

## 12. Tab: Export

Single-file and batch export, plus filename setup.

- **Output structure** — Baked-in or Custom layout for the written files.
- **Class base name** — The filename stem (Korean as-on-disk, or whatever your files use).
- **Gender suffix** — Female (`_¿©`) / Male (`_³²`) buttons — applied to every exported filename.
- **Palette #** — The number for a single save.
- **Save .pal** — Save the current palette as one `.pal`.
- **Batch (generated variants)** — Start # / Count / "Each variant uses" / Vary — produce a numbered run of variants and "Export batch as .zip".

The zip writer flags filenames as UTF-8 so Korean names extract correctly.

---

## 13. Tab: Info

Shows details about the currently loaded sprite (dimensions, version/encoding, palette info). Load a sprite to populate it.

---

## 14. The colour picker

Wherever you set a colour (Palette tab "Pick", Areas recolour, Generate base colour) you get a full HSV picker, like the one in ActEditor:

- A 2D Saturation × Value field (drag the circle).
- A vertical Hue strip (drag up/down).
- Editable H / S / V, R / G / B, and HEX fields — type into any of them.
- A live preview chip. It updates as you drag and closes when you click away.

This is what lets you dial in brown, black, white, and muted shades precisely — which the old RGB-only input made awkward.

---

## 15. Adding palettes from ColorHunt

Under the curated palettes (Generate tab) there is a paste field and a "+ Add" button.

- Paste a ColorHunt link, e.g. `https://colorhunt.co/palette/f5f5f5f2ead3dfd7bf3f2305` and press Add (or Enter).
- The colours are read straight out of the link text, so it works completely offline (no network call, nothing to be blocked).
- It also accepts a generic `.../palette/<hex>` URL, or raw hex codes such as `#FF6B6B, #FFD93D, #FF9F45` or `f5f5f5 f2ead3 dfd7bf`.
- The new palette appears as a chip, is auto-selected, and behaves exactly like a built-in curated palette — including being rollable into a scheme set for the Macro.
- Custom palettes are saved on this device. Hover a custom chip to get a small ✕ to remove it (built-ins can't be removed).

**Note:** a curated palette feeds the generator as a hue family spread across the areas (following the sprite's shading), not a literal one-colour-per-swatch fill.

---

## 16. Where your data is saved

Palette Forge stores a little data in your browser's local storage (per device, per browser):

- Custom curated palettes (your ColorHunt imports).
- The Macro plan: scheme sets, groups, masters, and all per-area locks.

It is **not** synced between machines. To move work between computers, use the Import/Export `.json` buttons:

- Schemes tab **Export .json** → your scheme libraries (a.k.a. macro "sets").
- Macro tab **Export macro .json** → the whole groups/masters plan.

Clearing your browser data will remove the locally-saved palettes and macro plan, so export anything you want to keep.

---

## 17. Common workflows (recipes)

**A) Recolour one body part**
Open SPR → Areas → Auto-detect → select area → pick colour → Recolor (keep shading) → Export tab → set name/gender/# → Save .pal.

**B) A pack of N random-but-tasteful variants for one class**
Open SPR → Areas: Auto-detect → Schemes tab → set Roll # → Roll → set Start # (+ mounts) → Export all as .zip.

**C) A full structured pack (colours × shades × locks) for a class**
Build scheme libraries on the Schemes tab (Export .json per colour) → Macro tab → import them as sets → add a group, tick the colours → add masters (shade + Areas locks), set "Apply to" per master → Start # + mounts → Export full pack .zip. Reuse for the next character of the same class: just load it and export again.

**D) Match a mount to the character**
Open SPR (character) → Mount tab → Add mount (.spr) [+ aliases] → recolour/generate as usual → tick "include mounts" when you export.

**E) Use a palette you found on ColorHunt**
Generate tab → paste the link in the curated field → Add → it's selected → Roll preview / Apply, or roll a batch on Schemes and feed the Macro.

---

## 18. Tips & gotchas

- Always Auto-detect areas after loading a class if you'll recolour parts or use the Macro/Schemes batch — areas are what locks and per-area treatments key off.
- Grey/washed-out result from a "monochromatic" set? Check the source scheme for a stray desaturated or blank index sitting in the colour ramp; one bad slot tints a whole area. Re-roll or fix that scheme.
- Brown/white/black bases now work as expected in Generate because the base colour's saturation and lightness are used, not just its hue.
- Black / White / Desat in the Macro are intentionally fixed — they ignore the master's Sat/Bri/Con and the scheme, so neutral-locked parts stay identical across the whole pack.
- If exported palettes look scrambled in-game, flip the Baked-in / Custom layout toggle before exporting.
- Transparency (index 0) and magenta slots are always protected; the tool will not recolour them.
- Keep backups: use the Schemes and Macro Export .json buttons. Browser data can be cleared, and that takes the locally-saved plan/palettes with it.
