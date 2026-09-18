# Roadmap

Phase 1 and Phase 2 (this repository's current state) are described in the
main [README.md](README.md). The phases below are not yet implemented;
they're recorded here so scope is explicit and nothing is silently claimed
as done before it is.

## Phase 3 — Advanced music production

- Step-sequencer drum machine + basic sample playback engine, reusing the
  existing clip-scheduling machinery in `AlmusAudioEngine` with very short,
  looping clips.
- Piano roll / MIDI-style note grid for programming melodic patterns.
- Track volume automation (the `EffectSettings`/track model would grow an
  automation lane type; the mixer would interpolate gain per-buffer instead
  of reading a single atomic).
- Offline vocal pitch correction: monophonic pitch detection (e.g. an
  autocorrelation or YIN-based estimator) on a recorded/imported vocal clip,
  snapping to a chosen scale, with adjustable correction speed/strength and
  a natural/hard-tune mode. This ships as **offline processing of a clip
  first** per the project's own requirement — real-time monitoring with
  pitch correction is a harder, later goal, not promised here.
- MIDI file import/export for patterns.
- Remaining effects not in Phase 2's fixed-slot chain: limiter, noise gate,
  chorus, flanger, distortion, saturation, pitch shifting, time stretching.

## Phase 4 — Professional refinement

- Cache effect filter coefficients (recompute only when a parameter actually
  changes, instead of every process() call) -- a real but minor CPU-usage
  optimization, not a correctness fix; see `track_effects.h`.
- A dedicated, independent export playhead (removing the current "briefly
  borrow the live playhead" approach documented in `audio_engine.cpp`) so
  exporting no longer risks a few frames of jitter on live playback.
- Anti-aliased resampling (the Phase 2 resampler is linear-interpolation
  only, adequate but not broadcast-quality — see `wav_file.h`).
- Replace the fixed 10ms UI polling loop with an event-driven meter update
  where practical, to reduce battery use.
- Project autosave/recovery after a crash or forced app kill.
- Broader device testing (matrix of sample rates, framesPerBurst values,
  Bluetooth vs. wired monitoring latency).
- Undo/redo across track and clip edits.
- Drag-based clip trim/move in the UI (Phase 2 ships split/delete/fade via a
  dialog, not direct-manipulation gestures on the waveform).

