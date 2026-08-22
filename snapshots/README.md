# Snapshot source Therand 1.0.4

Le bundle source validé est stocké en plusieurs fragments Base64 nommés :

```text
youtube-7.4.4-therand-1.0.4-source.tar.gz.b64.part00
...
```

## Reconstruction

```sh
cat youtube-7.4.4-therand-1.0.4-source.tar.gz.b64.part* \
  | base64 -d > youtube-7.4.4-therand-1.0.4-source.tar.gz

sha256sum youtube-7.4.4-therand-1.0.4-source.tar.gz

tar -xzf youtube-7.4.4-therand-1.0.4-source.tar.gz
```

SHA256 attendu du bundle source :

```text
1730f98375e25b38240a3f6ddc9589562fc69b7684cda32636a5b268330f7d5b
```

SHA256 du ZIP Kodi réellement testé :

```text
912e4a3d164fb75634d029559227feab3ab69958ddc3319d72f98be0813cad2b
```

La branche stable a été créée depuis l'upstream 7.4.4 afin de conserver un historique lisible. Le bundle ci-dessus est l'autorité pour les fichiers Therand exacts de `7.4.4+therand.1.0.4`.
