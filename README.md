# AMIGA NAMETAMPLE A1200

A self-contained PAL Amiga AGA demo in 68000 assembly. It targets the Amiga 1200/4000 chipset family, checks for Lisa/AGA on startup, and exits safely with a message on non-AGA machines.

The project was audited and renamed from Aurora Grid to **AMIGA NAMETAMPLE A1200**. The current identity, generated logo, ProTracker title, source comments, build artifact and documentation now use that name.

## Features

- **AGA 24-bit Copper colour:** raster plasma and floor bars use AGA high/low colour writes through `BPLCON3`/`LOCT`, producing smooth gradients beyond OCS/ECS limits.
- **Procedural logo asset:** `tools/logo_art.py` generates a 320×64 four-plane logo for AMIGA NAMETAMPLE A1200 with a brighter cyan/gold/pink AGA-inspired palette.
- **Smoother plasma palette:** `tools/gen_tables.py` now eases the aurora palette with smoothstep interpolation and a richer blue/cyan/magenta/gold ramp.
- **Wireframe tunnel and objects:** cube, octahedron and cuboctahedron scenes are transformed, projected and drawn with the blitter line engine.
- **Layered motion:** starfield, tunnel, bouncing shapes, logo wave, glint, plasma flash, floor bars and scroller all run in the frame loop.
- **Hardware sprite ball ring:** eight sprite balls use prebuilt shaded pages and depth ordering.
- **Four-channel Paula music:** generated ProTracker module in D minor with drums, bass, lead and arpeggio/pad material.
- **Safe OS takeover/restore:** the demo saves system state, allocates and verifies chip RAM, owns the blitter, installs its Copper list, then restores display/DMA/interrupt state on exit.

## Build

```sh
make validate
make verify-repro
make
```

`make` needs `vasmm68k_mot` on `PATH` or at `tools/bin/vasmm68k_mot`. The executable output is:

```text
build/amiga_nametample_a1200
```

Left mouse exits the running demo.

## Assets and generated data

```sh
make assets
python3 tools/generate_assets.py
python3 tools/gen_tables.py plasma_pal > /tmp/plasma_pal.s
python3 tools/gen_tables.py copper > /tmp/copper.s
```

Checked-in assets:

- `assets/logo.raw` — planar 4-bitplane logo bitmap.
- `assets/logo_preview.png` — preview of the generated logo.
- `assets/font.raw` — scroller font.
- `assets/aurora.mod` — generated ProTracker module, retitled AMIGA NAMETAMPLE.

## Compatibility

- PAL timing is assumed.
- AGA is required. The program checks Lisa before taking over hardware.
- CPU code is assembled as 68000-compatible (`-m68000`) even though the intended machine is A1200/A4000.
- OCS/ECS machines are intentionally rejected.
- NTSC is not a primary target; the wait logic avoids hanging, but timing/visual acceptance is PAL-focused.

## Documentation

- `docs/ARCHITECTURE.md` — startup, chip-RAM relocation, frame loop, DMA ownership.
- `docs/GFX.md` — display bands, bitplanes, AGA colour effects and rendering pipeline.
- `docs/BUILD_AND_TEST.md` — validation matrix and emulator/real-hardware checklist.
- `docs/CURRENT_AUDIT.md` — current audit findings and remaining external requirements.

## Validation status

Local static validation and deterministic asset checks are expected to pass without the assembler. Full executable build requires VASM. Real A1200 hardware acceptance is still a manual release gate.
