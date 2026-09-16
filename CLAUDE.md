# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A public content archive for the Hitotoki Collective — no source code, no build, no test suite, no package manifest. Contributions are media files plus Markdown metadata. There is nothing to compile or lint; "correctness" here means the media is metadata-clean, the LFS routing is right, and the manifests match the directory layout.

Remote: `https://github.com/hitotoki-collective/archive.git`. Sibling repositories `../website` and `../website-exp` are the likely consumers of this content.

## Layout

- `performances/PERF-<NN>-<XY>-<ABC>/` — one directory per live art performance.
  - `NN` = zero-padded 01–99, monotonically increasing; `XY` = capitalized country code (`JP`); `ABC` = 3-char location code (`KYO`).
  - Each holds a `MANIFEST.md` whose `Code:` field must equal the directory name.
  - Images follow `IMG-<NNN>.jpeg`, where `NNN` is an ordinal image identifier within the performance's image set.
  - Videos follow `VID-<XXX>-<YY>.<ext>`:
    - `VID` — literal, capitalized.
    - `XXX` — ordinal video identifier within the performance's video set.
    - `YY` — ordinal segment identifier within that video, zero-padded. `00` is reserved for the whole-video master; `01` upwards are segments cut from it.
  - No media filename is ever bare: a master is `VID-<XXX>-00`, not `VID-<XXX>`.
  - An aspect-ratio variant of a segment appends a non-numeric suffix — currently
    `-9x16` for vertical social cuts, e.g. `VID-000-03-9x16.mp4` is the vertical
    rendering of `VID-000-03.mp4`. The unsuffixed file is always the canonical 16:9.
- `core/` — small brand assets, one subfolder per asset family. `core/seal/` holds the 一時 seal as an SVG plus dark and `-light` raster variants at 32/64/128/256/512; `favicon.ico` sits at the top of `core/`. Deliberately **not** LFS-tracked so a plain clone without `git lfs fetch` still yields usable assets. `.gitattributes` enforces this with `core/** -filter -diff -merge -text`, which covers subfolders; do not add large media to `core/`.

## Git LFS

`*.mp4 *.mov *.jpeg *.jpg *.png *.tif *.tiff *.heic *.wav *.flac` are LFS-tracked (see `.gitattributes`). `git-lfs` must be installed — the repo's hooks (`post-commit`, `post-checkout`, `pre-push`, `post-merge`) hard-fail without it.

Verify a new media file actually went to LFS rather than being committed as a blob:

```bash
git lfs ls-files
```

## Media ingestion: strip metadata before committing

This is a public archive of identifiable people. Phone cameras embed device model, lens, capture timestamps, face-detection regions, and sometimes GPS. Every new image must be stripped before it is committed:

```bash
jpegtran -copy icc -outfile stripped.jpeg source.jpeg
```

`-copy icc` keeps the colour profile and discards everything else; the transform is lossless. Verify by decoding both with `djpeg -ppm` and comparing hashes — the pixel data must be identical.

## MANIFEST.md schema

Manifests are hand-written Markdown with a fixed section order; follow an existing one exactly rather than inventing structure. Sections: `## Overview` (`Code`, `Date` as `YYYY-MM-DD`, `Time` as `HH:MM`, `Country`, `Location`), `## Space` (prose plus `google maps` / `website` / `wikipedia` links), `## Participants` (`### Artists` with `Type:` and, for musicians, `Instrument:`; then `### Producers`, `### Assistants`, `### Guests`), `## Links`.

`TODO` is an accepted placeholder for an unfinished roster section — leave existing ones in place unless given the real names.

## Editing conventions

- Participant names are real people. Never invent, guess, or "complete" a name, and do not add personal detail beyond what the existing fields carry.
- `.serena/` is gitignored tooling state; do not commit it.
