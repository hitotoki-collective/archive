# Archive

Public archive of public content (raw and generated) pertinent to the Hitotoki Collective project.

## Contents

- performances
  - multimedia content of live art performances
  - each performance has a dedicated content subdirectory
  - performance subdirectory naming convention: `PERF-<NN>-<XY>-<ABC>`
    - NN = monotonically increasing integer in range 01-99, zero-padded
    - XY = capitalized country code, e.g. JP = Japan
    - ABC = 3 character location code, e.g. KYO = Kyoto
  - each performance subdirectory holds a `MANIFEST.md` whose `Code` field is
    `PERF-<NN>-<XY>-<ABC>`, matching the subdirectory name
  - image naming convention: `IMG-<NNN>`
    - NNN = ordinal image identifier within the performance's set of images,
      zero-padded
  - video naming convention: `VID-<XXX>-<YY>`
    - XXX = ordinal video identifier within the performance's set of videos,
      zero-padded
    - YY = ordinal segment identifier within that video, zero-padded; `00` is
      reserved for the whole-video master and `01` upwards are segments cut
      from it
  - aspect-ratio variants append a non-numeric suffix, currently `-9x16` for
    vertical social cuts; the unsuffixed file is always the canonical 16:9,
    e.g. `VID-000-03-9x16.mp4` is the vertical rendering of `VID-000-03.mp4`

## Media ingestion

This is a public archive, so images must be stripped of camera metadata before
they are committed. Phone cameras embed the device model, lens, capture
timestamps, and face-detection regions, and can embed GPS coordinates — none of
which belongs in a public repository of identifiable people.

Strip every new image before committing it:

```bash
jpegtran -copy icc -outfile stripped.jpeg source.jpeg
```

`-copy icc` keeps the colour profile and discards everything else. The transform
is lossless: the decoded pixels are unchanged. Verify with
`djpeg -ppm` on the source and the result and compare the hashes.
