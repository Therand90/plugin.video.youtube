# Therand preview playback checkpoint — 2026-08-26

Device: Kodi 21.3 / LibreELEC 12.2.1.

This document records the preview-specific playback changes validated during AutoTrailer testing. It is intentionally a checkpoint before source cleanup/version bump.

## Required final behaviour

For URLs carrying `therand_preview=true` only:

1. keep `therand_preview` through YouTube's parameter parser;
2. reject AV1 adaptive representations before MPD generation;
3. try a genuine progressive audio+video stream first;
4. if no progressive stream is available, retain the normal DASH/InputStream Adaptive fallback (with AV1 still rejected for previews);
5. leave normal YouTube playback unchanged.

## Device evidence

The problematic `Orang-outan` preview (`91epnzIb-4I`) originally selected AV1 and fell back to software `dav1d`, causing high CPU, fan ramp and stalls.

After the parser + AV1 filter changes, logs showed all `av01` representations being skipped and playback using H.264. The old high-level `yt_play.py` experiment that forced `use_mpd=False` did not reliably prevent MPD generation and must not remain in the final source state.

A later player-client-level progressive-first path made the Orang-outan preview play normally on the device.

## Cleanup requirement

Remove the obsolete `THERAND_PREVIEW_PROGRESSIVE_V1` block from `yt_play.py` while retaining:

- `THERAND_PREVIEW_PARAM_V1` in `abstract_context.py`;
- `THERAND_PREVIEW_NO_AV1_V1` in `player_client.py`;
- `THERAND_PREVIEW_PROGRESSIVE_V2` in `player_client.py`.

After device verification, port those three behaviours cleanly into the fork, bump the Therand fork version, run syntax/tests and create a validation ZIP. Do not change normal YouTube playback policy.
