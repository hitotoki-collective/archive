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
