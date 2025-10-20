# Act II – Spec Audit Summary

## Audit results
| Spec item | Status | Notes |
| --- | --- | --- |
| WorldTimeAPI primary with device fallback, drift guard, 1s polling | Patched | Added `TimeProvider` with API strategy, device fallback, drift detection, and optional mock override for tests. |
| On-load past segment queueing with 60s spacing | Patched | Introduced queue manager that schedules overdue segments, adds 60-second spacing (scalable in harness), and exposes state for the harness. |
| One-hour moon phase before on-time delivery | Patched | `MoonController` tracks long-phase progress and shows full glow on availability. |
| Short 10s final-stage moon for queued segments | Patched | Past segments trigger a 10-second finale before auto-delivery, scaled for harness tests. |
| Message sequence (3s glyphs, 0.4s shake, reveal, 2m auto-close, 5m reread) | Patched | Delivery flow enforces animation order, click gating, and duration scaling for tests. |
| Click-to-close logic after reveal | Patched | Overlay ignores clicks until reveal completes, then supports manual close and rerun access. |
| Blood Moon window 06:00–06:30 SGT, no retro-activation | Patched | Time-gated activation/deactivation with shooting rings and automatic reset. |
| Segment 4 gated at exactly 07:00 SGT, blog mode persists | Patched | Segment 4 only unlocks in the exact minute, triggers irreversible blog mode stored in `localStorage`. |
| Z-index layers (stars 0, ritual 1, header 100, overlay 500) | Patched | Updated overlay z-index to 500 and kept prior layer ordering. |
| Visual reset after each segment | Patched | Moon and ring revert to idle after delivery; queued segments use finale flag. |
| Automated harness scenarios | Added | `tests/time-harness.html` runs scripted scenarios with a mock time provider and scaled durations. |

## Implementation notes
- `index.html` now exposes a lightweight debug surface (`window.__ACT2_DEBUG__`) for the harness to drive ticks and observe state.
- Global flags (`__ACT2_TIME_PROVIDER__`, `__ACT2_DISABLE_AUTOTICK__`, `__ACT2_TIME_SCALE__`) allow deterministic testing without affecting production behaviour.
- The moon controller stores progress so the harness can confirm the one-hour lead-in animation.

## Running the harness
1. Serve the repository through a static file server (e.g. `python -m http.server`).
2. Open `tests/time-harness.html` in a browser.
3. Click any scenario button (or “Run all scenarios”) to replay the corresponding spec edge case with console-style logs.
