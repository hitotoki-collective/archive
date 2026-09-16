# CLAUDE.md

Public content archive for the Hitotoki Collective: media plus Markdown metadata, no code, build or tests. "Correct" means media is metadata-clean, LFS routing is right, manifests match the tree. Remote `github.com/hitotoki-collective/archive`; sibling repos `../website` and `../website-exp` consume it.

## Layout

`performances/PERF-<NN>-<XY>-<ABC>/`, one per performance: `NN` zero-padded 01–99, `XY` capitalised country code, `ABC` 3-char location code. `MANIFEST.md` is the only file at the top; the rest sit in `images/`, `video/`, `subtitles/`, `quotes/`, `interviews/`, each present only when that content exists. Filenames keep their full prefix inside those folders, since these files go to collaborators and social channels and must stay self-describing.

| Name | Is |
| --- | --- |
| `IMG-<NN>.jpeg` | a still |
| `VID-<XX>.mp4` | a master video |
| `VID-<XX>-SEG-<YY>-16x9.mp4` / `-9x16.mp4` | a segment, landscape / vertical |
| `VID-<XX>-MON-<YY>-16x9.mp4` / `-9x16.mp4` | a montage, landscape / vertical |
| `VID-<XX>-SUB-<ll>.srt` | captions OCR'd from the burned-in subtitles (`ll` = ISO 639-1) |
| `VID-<XX>-SUB-<ll>-<mm>.srt` | those cues translated into `mm`, same timings |
| `VID-<XX>-SUB-<ll>-combined.srt` | the whole film in `ll`, native and translated cues interleaved |
| any of those `.srt` as `.vtt` | the same cues with `<v Name>` speaker tags |
| `VID-<XX>-QTE-<Name>.md` | one speaker's lines as timecoded passages |
| `TSC-<NN>.docx` / `TSC-<NN>-<Name>.md` | a supplied interview transcription / one participant split out of it |

`NN`, `XX` and `YY` are all two-digit zero-padded ordinals. **A name with no kind code is source material the archive received; every kind code (`SEG`, `MON`, `SUB`, `QTE`, and a `TSC` split) marks something the archive produced.** The repo is CC BY-NC-SA 4.0, so that line matters: each manifest's `## Provenance` records who authored what, and a derived file is not the same as derived authorship — subtitle files are the archive's OCR of someone else's words, while montages are the archive's edit of someone else's footage. `SEG` and `MON` number independently. Only the master is bare: every video derivative carries a kind and an aspect ratio, while subtitles carry neither, being text rather than picture.

- **Segment** = a contiguous slice, so reproducible from one start timecode — record it in `MANIFEST.md`. **Montage** = an edited assembly of shots from across the master; no single offset, not mechanically regenerable, hence its own kind.
- `.vtt` attribution is evidence-based, not authoritative: cues carry `NOTE` lines recording the evidence, or that the speaker is unresolved. `QTE` files are generated from the `.vtt` — fix attribution there and regenerate; never hand-edit a `QTE`.
- A participant may have both a `TSC` and a `QTE` file. They overlap but differ: captions are edited for screen.

`core/` — brand assets, one subfolder per family (`core/seal/` holds the 一時 seal as SVG plus dark and `-light` rasters at 32–512; `favicon.ico` sits at the top). Deliberately **not** LFS-tracked so a plain clone yields usable assets; `.gitattributes` enforces this with `core/** -filter -diff -merge -text`, which covers subfolders. Keep large media out of it.

## Derivatives: regenerate rather than accumulate

Each manifest's `## Derivatives` lists stored segments with start timecodes plus reviewed excerpts deliberately not stored, those identified by timecode alone since a number denotes an archived file. Stored segments are 16:9 only; cut a vertical when a clip needs one.

```bash
ffmpeg -ss <start> -i VID-00.mp4 -t 30 -vf "scale=1920:1080:flags=lanczos" \
  -c:v libx264 -profile:v high -preset medium -crf 18 -maxrate 12M -bufsize 24M \
  -pix_fmt yuv420p -g 48 -c:a aac -b:a 192k -ar 48000 -ac 2 \
  -movflags +faststart VID-00-SEG-<YY>-16x9.mp4
```

For a segment's `-9x16` swap `-vf` for a blurred-fill composite, never a centre crop: a hard crop cuts the burned-in subtitles mid-word and loses the composition.

```
-filter_complex "[0:v]scale=192:342,gblur=sigma=8,scale=1080:1920,setsar=1[bg];[0:v]scale=1080:-2:flags=lanczos[fg];[bg][fg]overlay=(W-w)/2:(H-h)/2"
```

Montages cannot be regenerated this way; their manifest shot lists are provenance, not a recipe. Their picture drops the subtitle band by cropping the 4K master: `crop=3264:1836:288:0` then scale to 1080p for 16:9, or `crop=1033:1836:<x>:0` then scale to 1080x1920 for full-bleed vertical, `x` centred at 1403 unless the shot needs otherwise — a third field on a manifest shot list is that offset.

The films carry no subtitle stream: captions are burned into the picture, recovered by OCR (Apple Vision via a small `swiftc` tool over the caption band at 2 fps). Timings are good to ~0.5s; the text is unproofed.

## Git LFS

`*.mp4 *.mov *.jpeg *.jpg *.png *.tif *.tiff *.heic *.wav *.flac` are tracked (`.gitattributes`). `git-lfs` must be installed — the hooks (`post-commit`, `post-checkout`, `pre-push`, `post-merge`) hard-fail without it. Confirm a new file went to LFS rather than in as a blob with `git lfs ls-files`.

## Ingestion: strip image metadata first

Public archive of identifiable people; phone cameras embed device, lens, timestamps, face-detection regions and sometimes GPS. Strip every new image before committing:

```bash
jpegtran -copy icc -outfile stripped.jpeg source.jpeg
```

Keeps the colour profile and nothing else, losslessly. Verify by comparing `djpeg -ppm` hashes of source and result.

## MANIFEST.md schema

Fixed section order — copy an existing manifest rather than inventing structure: `## Overview` (`Code` equal to the directory name, `Date` as YYYY-MM-DD, `Time` as HH:MM, `Country`, `Location`) · `## Space` (prose plus `google maps` / `website` / `wikipedia`) · `## Participants` (`### Artists` with `Type:` and `Instrument:`, a painter's instrument being their brush; then `### Crew`, `### Executive Producers`, `### Assistants`, `### Guests`) · `## Links` · `## Provenance` · `## Derivatives`. `TODO` is an accepted placeholder — leave one in place unless given the real names.

## Rules

- Participants are real people. Never invent, guess or complete a name, and add no personal detail beyond what the existing fields carry.
- Record the photographer of new stills in that performance's `## Provenance`. It is Mark Greenslade for every still so far, but that is a per-performance fact, not a global one — never carry it across to a performance whose photographer you have not been told.
- Captions, transcripts and speaker attribution are machine output and unproofed. Don't present them as verbatim, and don't silently "correct" a person's words.
- `.serena/` and `.claude/settings.local.json` are gitignored; don't commit them.
