# Architecture

## Target

AMIGA NAMETAMPLE A1200 is a PAL AGA demo for Amiga 1200/4000-class machines. The code is kept 68000-compatible, but AGA colour hardware is required and checked at startup.

## Startup and ownership

1. Save CPU registers.
2. Open `graphics.library`, save the old view and system Copper pointer.
3. Detect AGA/Lisa. Non-AGA systems print a refusal message and exit before custom-chip takeover.
4. Allocate one contiguous chip-RAM payload block and verify it with `TypeOfMem`.
5. Copy the chip data payload, then rebase the Copper/list/asset pointers listed in `reloc_table`.
6. Blank the system view, wait for safe frames, own the blitter, snapshot DMA/interrupt/audio state.
7. Disable interrupts/tasks, stop DMA, initialise screen, assets, stars, Copper, MOD state and scene state.
8. Install the Copper list and enable master, bitplane, Copper, sprite and blitter DMA. Paula audio DMA is enabled by the replay only when a channel has valid sample data.

On exit the demo waits for the blitter, stops Paula, masks custom interrupts/DMA, restores the old view/system Copper, restores saved custom-chip masks where safe, releases the blitter, frees chip memory and closes libraries.

## Chip-RAM relocation

The executable may be loaded outside chip RAM, so all DMA-visible data is copied into an allocated chip block:

- Copper list and Copper patch points
- screen bitplanes and wireframe buffers
- sprite data
- logo/font assets
- silence word and MOD data

Runtime code reaches those through rebased pointer slots. Direct accidental references to DMA-visible link-time labels are rejected by `tools/validate.py`.

## Frame loop

The main loop waits for PAL frame sync, then runs:

1. swap in the previous frame's completed wireframe buffer;
2. update logo wave, raster bar, AGA plasma and floor bars;
3. update/erase/redraw stars;
4. update scroller;
5. tick the ProTracker replay;
6. clear and redraw the hidden wireframe/tunnel buffer with the blitter;
7. check left mouse exit.

The wireframe is double-buffered because it is the longest visible drawing task. Copper rows, stars and scroller remain single-buffered but are scheduled before the beam reaches their bands.

## Audio

The MOD player is a small row/tick ProTracker core for the generated four-channel module. Audio DMA is not blindly restored on exit because channel registers have changed; the demo exits with audio DMA stopped to avoid noise.

## Constraints

- No OS calls occur inside the running display loop.
- No interrupt handler is installed.
- The blitter is owned while the demo runs.
- PAL timing is the acceptance target.
- VASM with classic hunk output is the supported assembler path.
