# Press

Material for editorial use, and the terms it is offered under. Everything
referenced here lives in this repository; nothing is duplicated into a press
folder, so what you fetch is the same file the archive holds.

**Contact:** media@hitotoki-collective.org

---

## Terms of use

> **TODO — not yet agreed. Until this section is completed, nothing in this
> repository is cleared for editorial use, and enquiries should go to the
> contact above.**
>
> Points that need deciding:
>
> - The repository licence is CC BY-NC-SA 4.0. That **excludes commercial use**,
>   which covers most newspapers and broadcasters. Editorial use therefore needs
>   a separate grant, stated here, rather than reliance on the licence.
> - Which assets are cleared: stills, montages, segments, the master films, the
>   seal.
> - Whether the grant is open, or by request per outlet.
> - Whether cuts, crops, overlays or re-edits are permitted.
> - Whether the participants have consented to press use of their likeness and
>   their words, separately from the archive being public.
> - Embargo dates, if any.
> - Who to approach for the master films at broadcast quality.

---

## About the Hitotoki Collective

> Draft copy, written for this file and **not yet approved**. Replace with the
> collective's own wording before issuing.

**50 words.** Hitotoki — 一時, "one moment" — is a collective staging live
collaborations between a painter and musicians inside Japanese temples. Each
performance is improvised once and never repeated: ink meets sound in a single
sitting, and the finished work and the recording are all that remain.

**100 words.** Hitotoki — 一時, "one moment" — is a collective staging live
collaborations between a painter and musicians inside Japanese temples. Taro
Nordberg paints in sumi-e and calligraphy while Jason Reolon, Akira Ishiguro and
Kazuya Sato improvise on piano, guitar and shinobue. Nothing is rehearsed and
nothing is repeated: the painting responds to the music as it is played, and the
music to the brush. The collective has performed at Daikaku-ji and at Jingo-ji
in Kyoto, in spaces rarely opened to filming, and at Jingo-ji the temple's head
priest joined the work in progress with his own calligraphy.

---

## Performances

| | PERF-01 | PERF-02 |
| --- | --- | --- |
| Date | 2025-05-16, 12:00 | 2025-10-01, 15:00 |
| Venue | The royal tea house, Daikaku-ji, Kyoto | Jingo-ji, Kyoto |
| Painter | Taro Nordberg | Taro Nordberg |
| Musicians | Jason Reolon (piano), Akira Ishiguro (guitar), Kazuya Sato (flutes) | as PERF-01 |
| Also | — | Kosho Taniuchi, head priest, calligraphy |
| Film | Sam King | Sam King |
| Executive producers | Mark Greenslade | Mark Greenslade, Sherif Shaw |
| Screener | https://f.io/xoLWqB2Y | https://f.io/adMDsngz |

Full participant and crew lists are in each performance's `MANIFEST.md`.

---

## Available assets

| Asset | Where | Notes |
| --- | --- | --- |
| Montages, 30s | `performances/<PERF>/video/VID-00-MON-*` | Best broadcast material: no on-screen text, music-led, in 16:9 and full-bleed 9:16 |
| Segments, 30s | `performances/<PERF>/video/VID-00-SEG-*` | Continuous excerpts, but carry burned-in subtitles |
| Stills | `performances/<PERF>/images/` | See the caveat below on credit |
| Master films | `## Links` in each `MANIFEST.md` | Screener links; the repository copies are 4K and large |
| Seal / logo | `core/seal/` | SVG plus light and dark rasters, 32–512px. Not in Git LFS, so a plain clone retrieves them |
| Quotes | `performances/<PERF>/quotes/` | One file per speaker, timecoded. Machine-transcribed — see below |

Clone with `git lfs install` first, or the video and images arrive as text
pointers rather than content.

---

## Credit lines

Fill in once the terms are agreed. The distinction that matters: a montage is
the archive's edit of someone else's cinematography, so both are named.

- **A still:** `Photograph: Mark Greenslade / Hitotoki Collective`
- **A film excerpt (segment):** `Film: Sam King / Hitotoki Collective`
- **A montage:** `Footage: Sam King. Edit: Hitotoki Collective archive.`
- **A quotation:** name the speaker and the performance, e.g.
  `Kazuya Sato, Hitotoki Collective, Daikaku-ji, May 2025`

---

## Caveats worth knowing before you publish

**The stills carry no embedded credit.** Every image in this archive is stripped
of metadata before it is committed, because it is a public archive of
identifiable people and phone cameras embed device details and sometimes GPS.
That strip also removes the IPTC credit and copyright fields a picture desk
expects. Ingestion now writes the credit back after stripping, but the three
images already in the archive predate that step and still carry nothing. Every
still currently here was taken by Mark Greenslade; a later performance may bring
another photographer, so check the `## Provenance` table of the performance you
are drawing from rather than assuming.

**The montages are the archive's edit, not the film's.** Shot selection, running
order, the audio bed and a reframing crop are editorial decisions made when
building this archive. The cinematography is Sam King's. Both need crediting,
and the montages should not be described as the director's cut.

**Quotes and subtitles are machine-derived and unproofed.** The films carry no
subtitle track; the captions were recovered from the picture by OCR, and speaker
attribution was inferred from evidence recorded cue by cue. Known errors remain.
Verify any quotation against the film before printing it.

**PERF-02's Japanese-to-English translation in `VID-00-SUB-ja-en.srt` is the
archive's**, not the film's. The film's own translation is by Akane Saiki and is
what appears on screen.

**The crew lists are unverified.** They were read off the films' closing credits
by OCR and have not yet been checked against the production's own records.
