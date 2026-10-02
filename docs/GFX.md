# Graphics

Aurora Grid A1200 uses a PAL low-resolution 320×256 display with four playfield bitplanes for bitmap layers and AGA 24-bit Copper colour for the plasma/floor gradients.

## Screen layout

| Rows | Band | Main content |
| --- | --- | --- |
| 8–71 | Logo | generated Aurora Grid A1200 logo, row wave and glint |
| 72–75 | Raster bar | animated highlight colour slot |
| 76–195 | Middle | starfield, 3D objects, tunnel, sprite balls, AGA plasma |
| 200–215 | Scroller | 16-line text strip with frame lines |
| 222–255 | Floor | AGA Copper bars, no bitmap drawing |

## Colour and smoothness improvements

- The logo generator now uses a brighter 16-colour ramp with cyan, gold, ice white, pink and dark-blue shadow tones.
- The plasma generator uses smoothstep interpolation across a wider aurora ramp, reducing visible banding in generated tables.
- Floor bars have softer eight-row profiles and higher contrast cyan/magenta/gold highlights over a darker base.
- The checked-in logo preview and raw logo asset are regenerated from the Aurora Grid A1200 artwork.

## Bitmap layers

The chip block contains four plane-sized buffers: logo/text/star plane, two alternating wireframe buffers, and the far/near star plane. Plane 1 is double-buffered for the blitter-drawn wireframe so the display never shows a partially drawn object.

The Copper changes palette entries by screen band: the same bitmap bit can be gold in the logo, blue-white in the scroller, or a star shade in the middle band.

## AGA Copper effects

The AGA effects write both high and low colour nibbles through `BPLCON3` `LOCT`, effectively giving 24-bit colour values for raster plasma and floor gradients. The source still uses a compact four-bitplane bitmap layout for compatibility and speed; the AGA win is concentrated in colour precision and smooth raster motion.

## Logo wave

Each logo row has a Copper entry that updates `BPLCON1`. `UpdateWave` rewrites those scroll values every frame from a sine table, producing a horizontal ripple across the logo. A separate glint blends over `logo_face` colours.

## Stars, tunnel and objects

Stars use reciprocal-table projection and three brightness classes. The wireframe/tunnel path rotates and projects vertices, then the blitter draws edges into the hidden wireframe buffer. Objects and scene parameters ease through an eight-scene table in `src/main.s`.

## Scroller

The scroller shifts planar rows left using big-endian longword rotation and inserts one glyph column at the right. It is updated before the beam reaches the strip.

## Verification

`tools/validate.py` checks asset sizes, static source rules, relocation assumptions and hardware constants. Full visual verification still needs an emulator or real AGA machine.
