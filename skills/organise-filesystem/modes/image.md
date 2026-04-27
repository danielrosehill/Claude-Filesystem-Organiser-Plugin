# Mode: image

Organise a folder of image files. Default sequence: classify (screenshot vs photo) → resolution buckets → aspect-ratio split → date hierarchy.

## Probe

Prefer `exiftool` for full metadata, fall back to `identify`:
```
exiftool -j -DateTimeOriginal -ImageWidth -ImageHeight -Make -Model -Orientation <file>
```

Record: width, height, EXIF date, camera make/model (absence often = screenshot/edit).

## Screenshot vs photo split

Heuristics for screenshot detection:
- Filename matches `Screenshot*`, `Screen Shot*`, `Bildschirmfoto*`, etc.
- No EXIF camera Make/Model AND PNG format AND dimensions match a common screen resolution.
- Inside a folder named `screenshots`, `Screen*`, etc.

Top-level layout:
```
photos/
screenshots/
edits/        derived files (filename suffixes -edit, -edited, -copy, -cropped)
```

## Resolution buckets (inside `photos/`)

```
huge/      max(width, height) >= 6000   (full-frame, medium-format)
large/     >= 4000                      (typical modern phone/DSLR)
medium/    >= 2000
small/     >= 800
thumb/     < 800                        (probably extracted thumbnails or icons)
```

## Aspect ratio (inside resolution bucket, optional)

Same scheme as video: `landscape/`, `portrait/`, `square/`, `other/`. Tolerance ±2%.

## Burst dedupe

Within a date bucket, detect bursts: ≥3 photos taken within 2 seconds of each other. Move all but the largest-file (proxy for sharpest) to `_review/bursts/<date>/`. Don't auto-delete.

## Composition with by-date

Inside resolution (and aspect) buckets, apply `by-date` using EXIF date (mandatory for photos — never use mtime for photos unless EXIF is genuinely absent).

## Naming

`YYYYMMDD-HHMMSS-<camera_model_short>.<ext>` if EXIF available, else `YYYYMMDD-<original_stem>.<ext>`.

## Output

Report: photos vs screenshots vs edits counts, bucket distribution, burst groups flagged for review, files with missing EXIF placed in `_review/no-date/`.
