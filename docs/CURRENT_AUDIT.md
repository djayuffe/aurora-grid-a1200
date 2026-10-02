# Current audit

## Summary

The repository was audited for naming consistency, AGA/OCS documentation drift, generated assets, source safety assumptions, validation coverage and obvious stale references.

## Changes made

- Renamed visible project identity to **AMIGA NAMETAMPLE A1200**.
- Updated runtime AGA refusal message and scroller text.
- Renamed build output to `build/amiga_nametample_a1200`.
- Regenerated the logo and MOD assets from the updated generators.
- Improved generated colour ramps for logo, plasma and floor-bar effects.
- Replaced outdated OCS/ECS documentation with AGA/A1200-focused docs.
- Added this current audit note so old historical audit material is not mistaken for the current target.

## Findings

- The source already contains a proper AGA gate before hardware takeover.
- DMA-visible data is copied to verified chip RAM and pointer slots are rebased.
- The frame loop separates time-critical Copper/list updates from blitter drawing.
- The repo still requires VASM for a full executable build; host-only validation does not assemble the binary.
- FS-UAE 3.2.35 A1200/internal-ROM visual smoke test booted from a temporary directory hard drive and displayed the renamed logo, plasma, wireframe object, sprite balls, floor bars and scroller.
- Real hardware validation is still external/manual.

## Remaining release gates

- Run on real PAL A1200/A4000 hardware before claiming hardware-complete status.
