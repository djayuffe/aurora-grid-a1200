# Build and test

## Host checks

| Check | Command | Expected result |
| --- | --- | --- |
| Static source/assets | `make validate` | passes |
| Reproducible assets | `make verify-repro` | passes |
| Full executable | `make` | requires `vasmm68k_mot`; emits `build/amiga_nametample_a1200` |
| Manifest | `python3 tools/make_manifest.py` | refreshes `MANIFEST.sha256` |

`make` uses `-m68000 -kick1hunks -Fhunkexe` for classic Kickstart-compatible output.

## Emulator checklist

Use an A1200/AGA PAL configuration:

- [ ] non-AGA configuration refuses to start cleanly;
- [ ] logo is stable, unclipped and visibly renamed to AMIGA NAMETAMPLE A1200;
- [ ] logo wave and glint move smoothly with no row tearing;
- [ ] 24-bit plasma has no obvious stepping or corrupt colour writes;
- [ ] stars fly outward without trails;
- [ ] tunnel and objects draw solid lines with no half-frame buffer exposure;
- [ ] sprite ball ring remains stable and depth-sorted;
- [ ] floor bars glide smoothly;
- [ ] scroller wraps and inserts columns cleanly;
- [ ] all four Paula channels sound correct;
- [ ] left mouse exits and the Workbench/Shell display returns;
- [ ] repeated runs return memory.

## Real hardware checklist

For a public release, also run on a real PAL A1200 or A4000:

- [ ] cold boot, run from Shell, exit by left mouse;
- [ ] repeat at least 10 times and verify chip/fast memory is returned;
- [ ] verify no audio noise remains after exit;
- [ ] verify no Copper/sprite corruption on CRT or scandoubler output;
- [ ] verify timing on stock 68020 and any accelerated test machine separately.

## Scope

This is an AGA PAL demo, not a portable application or a general-purpose ProTracker replay library. `vamos` is useful for some host-side sanity checks but is not a substitute for custom-chip emulation.
