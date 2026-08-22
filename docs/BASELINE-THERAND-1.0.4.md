# YouTube 7.4.4+therand.1.0.4 — baseline validée

Date : **22 août 2026**.

## But

Le fork doit distinguer strictement une lecture YouTube normale d'une preview technique lancée par `service.therand.autotrailer`.

AutoTrailer ajoute le paramètre :

```text
therand_preview=true
```

Pour ce mode uniquement, le fork sélectionne le chemin **DASH / MPD**. La lecture YouTube ordinaire conserve le comportement upstream.

## Pourquoi DASH

Les previews HLS présentaient de façon reproductible une famille d'erreurs audio sur certains trailers : échec du sample reader, fallback ADTS puis `CVideoPlayerAudio::Process - stream stalled`. Les cas de référence comprenaient Mutiny, Insidious et Obsession.

La généralisation du chemin DASH à toutes les requêtes `therand_preview=true` a restauré l'audio sur ces cas sans régression constatée sur Minions ni sur les trailers déjà fonctionnels.

## Stack validé avec ce fork

- Kodi 21.3 / LibreELEC 12.2.1 ;
- skin `3.19.11+25widgets.89.5-develop` ;
- AutoTrailer `0.27.2` ;
- YouTube `7.4.4+therand.1.0.4` ;
- option YouTube « rafraîchir après avoir regardé » désactivée ;
- JackTook 1.18.0.4 avec adaptations locales cache widgets + `reuselanguageinvoker=false`.

## Fichiers Therand figés

Le bundle source canonique contient exactement :

```text
addon.xml
changelog.txt
resources/lib/youtube_plugin/kodion/constants/__init__.py
resources/lib/youtube_plugin/kodion/context/abstract_context.py
resources/lib/youtube_plugin/kodion/monitors/player_monitor.py
resources/lib/youtube_plugin/youtube/client/player_client.py
resources/lib/youtube_plugin/youtube/helper/yt_play.py
```

Archive source : `youtube-7.4.4-therand-1.0.4-source.tar.gz`.

SHA256 de l'archive source :

```text
1730f98375e25b38240a3f6ddc9589562fc69b7684cda32636a5b268330f7d5b
```

SHA256 du ZIP Kodi testé :

```text
912e4a3d164fb75634d029559227feab3ab69958ddc3319d72f98be0813cad2b
```

Le bundle Base64 découpé est stocké sous `snapshots/`. Voir `snapshots/README.md` pour la restauration.

## Invariant à préserver

Ne jamais forcer DASH globalement pour toutes les lectures de l'add-on. Le comportement spécial doit rester derrière `therand_preview=true`, afin que l'utilisation normale de YouTube reste indépendante des besoins de l'AutoTrailer.
