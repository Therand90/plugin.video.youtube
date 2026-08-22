# Therand YouTube baseline 7.4.4+therand.1.0.4

Validated on 2026-08-22 with the inline trailer stack.

## Functional delta from 1.0.3

Only two files changed:

- `addon.xml` — version bump from `7.4.4+therand.1.0.3` to `7.4.4+therand.1.0.4`.
- `resources/lib/youtube_plugin/youtube/helper/yt_play.py` — force the existing local MPD/DASH path for every request carrying `therand_preview=true`.

Normal YouTube playback is intentionally unchanged.

## Why DASH is forced for Therand previews

Several trailers reproducibly lost audio through the HLS path (including Mutiny, Insidious and Obsession). A targeted Minions DASH experiment restored audio, then 1.0.4 generalized the same transport only to Therand inline preview requests. Manual validation confirmed audio on the previously failing trailers without regression on previously working trailers.

## Canonical artifact

```text
plugin.video.youtube-7.4.4+therand.1.0.4-preview-dash.zip
SHA256 912e4a3d164fb75634d029559227feab3ab69958ddc3319d72f98be0813cad2b
```

The exact source delta from the validated 1.0.3 artifact is stored in `baseline/1.0.3-to-1.0.4.patch`.

## Companion validated versions

- Skin: `3.19.11+25widgets.89.5-develop`
- AutoTrailer: `0.27.2`
- JackTook: `1.18.0.4` with local boot-time compatibility patch
- YouTube setting `refresh after watched`: OFF

This branch is a restoration/reference snapshot. Future experiments should not be developed directly on it.
