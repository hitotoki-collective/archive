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
  - inside a performance subdirectory, `MANIFEST.md` sits at the top and every
    other file lives in a subfolder by kind:
    - `images/` = the stills
    - `video/` = the master and every segment and montage cut from it
    - `subtitles/` = the caption tracks
    - `quotes/` = one file per speaker, extracted from the attributed captions
    - `interviews/` = interview recordings transcribed to text
  - a subfolder is present only when the performance has that kind of content,
    so a performance with no interview recordings has no `interviews/`
  - image naming convention: `IMG-<NN>`
    - NN = ordinal image identifier within the performance's set of images,
      zero-padded to two digits
  - video naming convention
    - `VID-<XX>.mp4` = a master video
    - `VID-<XX>-SEG-<YY>-16x9.mp4` = a segment file in landscape
    - `VID-<XX>-SEG-<YY>-9x16.mp4` = a segment file in vertical
    - `VID-<XX>-MON-<YY>-16x9.mp4` = a montage file in landscape
    - `VID-<XX>-MON-<YY>-9x16.mp4` = a montage file in vertical
    - `VID-<XX>-SUB-<ll>.srt` = burned-in subtitles recovered by OCR, one
      file per language, where ll is the ISO 639-1 code
    - `VID-<XX>-SUB-<ll>-<mm>.srt` = the ll cues translated into mm
    - `VID-<XX>-SUB-<ll>-combined.srt` = every cue in the film rendered
      in ll, interleaving native and translated cues
    - a `.vtt` alongside any of the above `.srt` forms is the same cues as
      WebVTT, with speaker attribution as voice tags where it could be
      established, e.g. `VID-00-SUB-en-combined.vtt`
    - `VID-<XX>-QTE-<Name>.md` = every line one speaker has in that video,
      extracted from the attributed WebVTT
    - XX = ordinal video identifier within the performance's set of videos,
      zero-padded to two digits
    - YY = ordinal derivative identifier within its own kind, zero-padded to
      two digits;
      segments and montages are numbered independently, so `SEG-01` and
      `MON-01` can both exist for the same master
  - interview naming convention
    - `TSC-<NN>.docx` = an interview recording transcribed to text, as supplied
    - `TSC-<NN>-<Name>.md` = one participant's interview, split out of it
    - NN = ordinal transcript identifier within the performance, zero-padded
  - a participant may therefore have two text records: their interview in
    `interviews/`, and their lines from the film in `quotes/`. They overlap but
    are not identical, since the captions are edited for screen
  - source and derived content are distinguished by the name: a file with no
    kind code is source material the archive received, and every kind code
    (`SEG`, `MON`, `SUB`, `QTE`, and a `TSC` split) marks something the archive
    produced. Each `MANIFEST.md` has a `## Provenance` section recording who
    authored what, which the CC BY-NC-SA licence obliges anyone reusing this
    material to honour
  - a segment is a contiguous slice of its master, so it can be re-cut from a
    single start timecode; a montage is an edited assembly of shots drawn from
    across the master and cannot be regenerated mechanically

## Cloning

Large media is stored in Git LFS, so install `git-lfs` before cloning or the
media files arrive as small text pointers rather than content:

```bash
git lfs install
git clone https://github.com/hitotoki-collective/archive.git
```

The brand assets in `core/` are deliberately exempt, so they are usable from a
plain clone without fetching LFS objects.

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
