# Changelog

## 1.0.0 - 2026-11-08

- First release, as a single `.pnach` for the PCSX2 Patches tab.
- 60 distinct simulation states per second at the correct world speed, with
  the camera turning at the original rate.
- Cutscene and on-rails cameras at 60 Hz, with camera cuts, relocations and
  character poses matching the original on the extra frames.
- Original timing restored for button holds and taps, two-button chords,
  minigames (uncuff, lockpick, tag, radio, mug, arrest, workouts, chases),
  HUD blinks and prompts, AI scheduling, flames, sprays, camera shakes,
  doors, credits and menu input.
- `Input lag: STOCK` (default) and `Input lag: OFF`, switched with
  Select + L2 + R2 held for one second.
- Unticking the patch returns a running game to the original 30 fps.
- Save states made without the patch load at the correct world speed.
