# The Warriors 60FPS Patch

A PCSX2 patch that runs the USA release of *The Warriors* (PlayStation 2) at
60 fps: 60 distinct simulation states every second, at the correct world speed.
It is not frame doubling or interpolation. The game itself updates twice as
often, and everything the engine counts per frame has been retimed so it lasts
as long as it does in the original. That covers cutscenes and cameras, the
HUD, minigames, button timing, effects and sounds.

It is a single `.pnach` file that PCSX2 applies by itself. There is no
companion program, and nothing on your disc image or memory card is modified.

## Requirements

- *The Warriors*, USA release: serial `SLUS-21215`, version `1.03`, CRC
  `B99A75DE`. The patch applies to nothing else.
- PCSX2 on desktop. Tested on 2.8.2 (stable) and on the 2.9.150 nightly.

## Installing

1. Copy `SLUS-21215_B99A75DE_60fps.pnach` into the `patches` folder of your
   PCSX2 data directory. On a default Windows install that is
   `Documents\PCSX2\patches`; in a portable install, the `patches` folder next
   to `pcsx2-qt.exe`. Keep the file name: PCSX2 matches it to the game by the
   `SLUS-21215_B99A75DE` prefix.
2. In the game list, right-click *The Warriors* → **Properties** → **Patches**
   and tick **The Warriors 60FPS Patch**.
3. Start the game. You can also tick the patch while the game is running.

The Patches tab shows **Applied: Unknown** for this patch. This is expected:
PCSX2 shows "Unknown" for any patch that has lines applied once at startup as
well as lines applied every frame.

## Input lag: STOCK and OFF

At 60 fps the game reacts to a button press sooner than the original does,
because the engine counts some of its delays in frames. By default the patch
keeps the original game's response.

Hold **Select + L2 + R2** for one second to switch between:

| Mode | Response |
| --- | --- |
| `Input lag: STOCK` (default) | The same input timing as the original game. |
| `Input lag: OFF` | The fastest response the game can give at 60 fps. |

The mode is shown for a couple of seconds at the top of the screen. It goes
back to STOCK every time the game boots, and a save state keeps the mode it
was made with.

## Turning it off

Untick the patch in **Properties → Patches**. The running game goes back to
the original 30 fps behaviour within a couple of frames, with no restart.
Tick it again to return to 60.

## Save states

- **A save state made without the patch** can be loaded with it on. The
  game's clock is corrected automatically on the first frame.
- **A save state made with the patch** contains the patch's code. If you load
  one with the patch unticked, the game goes back to stock by itself, exactly
  as if you had unticked it.

Memory card saves are not affected in either direction.

## Performance

The emulated PS2 does twice the work per second, so in a few levels a slower
PC may not keep 60. Because the world advances a fixed step per frame, a drop
looks like slow motion rather than stutter. If that bothers you, open the
game's **Properties → Emulation** and raise **EE Cycle Rate**: +1 helps a lot
and +2 usually solves it. Set it for this game rather than globally. It costs
more CPU on the PC and does not change the game's clock. EE Cycle Skipping does
the opposite and should stay off.

## Compatibility

PCSX2's own patches for this game (**Widescreen 16:9** and **480p Mode**, in
the same Patches tab) write to different memory addresses than this patch.

## Known limitations

- **Two-player play is only partly covered.** Most of the timing was measured
  with player 1, so some player 2 behaviour may still differ from the original
  game.

## A note on the Hi-Hats introduction

With the game's widescreen mode, the Hi-Hats gang introduction card does not
appear. That happens without this patch too: in widescreen the card sits
behind the camera's near plane. Switch the game to 4:3 to see it.

## How it works

The original game presents each frame twice and runs its world 30 times a
second. Sections 1 to 6 of the `.pnach` change the display divider, remove the
frame-parity gate and halve the simulation timestep, which gives 60 distinct
frames at the correct world speed.

That alone leaves a large number of things running at double speed. Anything
the engine counts in frames rather than in seconds is affected: camera
damping, cutscene animation, AI scheduling, HUD blinks, minigames, the
two-button chord windows, effects and sounds. Each of the remaining sections
fixes one of these, usually by hooking the routine involved and giving it
back its original cadence. Every hook is guarded so that it only acts at
60 fps. The last section watches for the patch being unticked and then writes
the game's original code back.

Every section is documented in the comments of the `.pnach`, with its cause,
its guards and the disassembled routine.

## License

Copyright (C) 2026 mostdak1ng.

Licensed under the MIT License. See `LICENSE`.
