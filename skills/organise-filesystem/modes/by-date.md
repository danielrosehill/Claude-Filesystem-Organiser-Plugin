# Mode: by-date

Reorganise into a date hierarchy based on creation/EXIF time.

## Choose granularity

Ask the user (or infer from volume):

| Layout | When to use |
|--------|-------------|
| `YYYY/` | <50 files/year, long-tail archive |
| `YYYY/MM/` | Most photo libraries, document archives |
| `YY/MM/DD/` | High-volume daily output (logs, dashcam, voice memos) |
| `YYMMDD-<original>` flat | User wants a single sortable folder, not nesting |

Daniel's preference is `YY/MM/DD/` for high-volume; `YYYY/MM/` otherwise.

## Date source priority

1. **EXIF/metadata** — `exiftool -DateTimeOriginal -s3 <file>` (images), `ffprobe -show_entries format_tags=creation_time` (video/audio).
2. **Filename pattern** — `IMG_20230815_...`, `Screenshot 2024-...`, `WhatsApp Image YYYY-MM-DD ...`. Parse before falling back to mtime.
3. **Filesystem mtime** — last resort. Note in report that fallback was used.
4. **ctime/birth time** — only on filesystems that record it (ext4 with 256-inode, btrfs); never on FAT/exFAT.

If a file has no recoverable date, drop it in `_review/no-date/`.

## Bucketing rules

- One file → one folder. No symlinks across years.
- Empty months/days are not pre-created.
- If two files have the same target path and different content, keep both with `-1`/`-2` suffix.

## Cross-mode composition

If composed with `video`/`image`/`audio`, date hierarchy goes **inside** the resolution buckets, not outside (e.g. `4k/2024/03/`), unless the user says otherwise.
