# Mode: video

Organise a folder of video files. Default sequence: prune junk → resolution buckets → aspect-ratio split (optional) → date hierarchy inside.

## Probe

For each file, capture:
```
ffprobe -v error -select_streams v:0 \
  -show_entries stream=width,height,duration,codec_name,r_frame_rate \
  -show_entries format=duration,bit_rate \
  -of json <file>
```

Record: width, height, duration (s), codec, framerate, bitrate.

## Junk-clip pruning

Default thresholds (override only if user asks):

| Signal | Action |
|--------|--------|
| Duration < 3.0s | Move to `_review/short-clips/` (likely accidental record-button taps) |
| Duration < 0.5s | Auto-delete (unrecoverable garbage) |
| File size < 100 KB AND duration < 5s | `_review/short-clips/` |
| Codec/container unreadable by ffprobe | `_review/corrupt/` |

Never delete a clip > 3s without user confirmation, even if it looks like junk.

## Resolution buckets

```
8k/        height >= 4320
4k/        height >= 2160 (UHD or DCI 4K)
1440p/     height >= 1440
1080p/     height >= 1080
720p/      height >= 720
sd/        height < 720
```

Use `height` as the primary axis (handles vertical phone footage correctly via aspect mode below).

## Aspect-ratio split (optional)

Trigger only if the user asks or the folder has clearly mixed orientations. Inside each resolution bucket:

```
landscape/    16:9, 16:10, 21:9, 4:3
portrait/     9:16, 9:21, 3:4 (phone vertical)
square/       1:1 (~±5%)
other/        anything else
```

Compute aspect = width / height. Bucket by ratio with ±2% tolerance.

## Composition with by-date

Inside each resolution (and aspect, if used) bucket, apply `by-date` to get e.g. `4k/landscape/2024/03/`.

## Naming

Rename each kept file to `YYMMDD-HHMMSS-<original-stem>.<ext>` using the metadata creation_time, falling back to mtime. Keep original extension lowercase.

## Output

Report: total scanned, deleted (auto), moved to `_review/short-clips/`, moved to `_review/corrupt/`, bucket distribution.
