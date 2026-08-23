# Therand YouTube 1.0.8 preview guard baseline

Validated pairing on 2026-08-23:

```text
plugin.video.youtube 7.4.4+therand.1.0.8
service.therand.autotrailer 0.27.6
```

Canonical local artifact:

```text
plugin.video.youtube-7.4.4+therand.1.0.8-preview-reroute-guard.zip
SHA256: 17645e9564155a55f10e33f3070a8db3c5b4b11c74a01f163d348144ca6f0a69
```

Stable branch:

```text
stable/therand-1.0.8-preview-reroute-guard
```

Reproducible delta from the validated 1.0.4 DASH baseline:

```text
baseline/1.0.4-to-1.0.8.patch
```

## Behaviour retained from 1.0.4

Requests carrying `therand_preview=true` force the add-on's existing MPD/DASH path. Normal YouTube playback remains unchanged. This is the validated fix for the previous HLS audio-stall/sample-reader failure family.

## Additional preview guards

1. Preview resolution checks that Kodi Home is still active before and after the expensive resolve path. When Home is inactive the preview returns without normal fallback/list refresh behaviour.
2. The generic YouTube `reroute()` path is suppressed while a Therand preview session exists outside Home. This prevents the reproduced navigation to `/special/my_subscriptions`, which otherwise replaced Kodi Settings with the YouTube video-navigation window.

The reroute guard is scoped by the AutoTrailer `TherandInset.Session` Home property and does not alter ordinary YouTube navigation.

## Rejected experiment

`7.4.4+therand.1.0.7` is not a baseline. It modified the generic Kodi plugin runner to short-circuit `endOfDirectory()` after a failed preview resolution. It did not solve the folder-navigation issue and was considered unnecessarily invasive, especially after crashes were observed during that experiment family.

## Known edge case

The validated stack intentionally does not aggressively stop an already-running owned preview merely because Home becomes inactive. AutoTrailer 0.27.5 attempted that and caused worse regressions; see the AutoTrailer 0.27.6 baseline documentation.
