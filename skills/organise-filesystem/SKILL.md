---
name: organise-filesystem
description: Use when the user wants to clean up, sort, or restructure a directory tree on the local filesystem, a mounted Google Drive (rclone), or an Android device (ADB). Triggers on phrases like "organise this folder", "sort these files", "clean up the filesystem", "tidy this directory", "sort my downloads", "organize by date/year/month/resolution/aspect ratio", "split this mixed-media folder by type", "remove accidental short clips".
---

# Filesystem Organiser

You reorganise file trees against an explicit operational profile. Always pick (1) a **target** (where the files live) and (2) one or more **modes** (how to organise) before touching anything.

## Step 1 — Identify the target

Pick the right target adapter and read its guide:

| Target | When | Guide |
|--------|------|-------|
| Local filesystem | Default. Path is on `/home`, `/mnt`, external drive, etc. | `targets/local.md` |
| Mounted Google Drive | Path is under an rclone mount (e.g. `~/gdrive/`). Operations cost API calls; batch them. | `targets/gdrive.md` |
| Android device | User says "phone", "android", `adb`, or path looks like `/sdcard/...`. | `targets/android.md` |

If the target is ambiguous, ask. Never run mode-specific moves until the target is confirmed.

## Step 2 — Pick the mode(s)

Modes are composable but ordered. For mixed-media folders, run `by-media-type` first to split the tree, then run media-specific modes inside each branch.

| Mode | What it does | Guide |
|------|--------------|-------|
| `cleanup` | Snake_case, typo fix, orphan placement, dedupe, empty-folder pruning. Safe baseline. | `modes/cleanup.md` |
| `by-date` | Hierarchy by creation/EXIF date — `YYYY/MM/`, `YY/MM/DD/`, or flat `YYMMDD-` prefixes. | `modes/by-date.md` |
| `by-category` | Subject/category/subcategory folders (vision + filename heuristics). | `modes/by-category.md` |
| `by-media-type` | Split mixed dump into `images/`, `videos/`, `audio/`, `docs/`, `archives/`, `installers/`. | `modes/by-media-type.md` |
| `video` | Resolution buckets (`4k/`, `1080p/`, `720p/`, `sd/`), aspect-ratio split, prune unintentional sub-3s clips. | `modes/video.md` |
| `image` | Resolution + aspect-ratio buckets, screenshot vs photo split, burst dedupe. | `modes/image.md` |
| `audio` | Format/sample-rate buckets, prune sub-2s accidental recordings, voice-memo vs music split. | `modes/audio.md` |

**Default if user is vague:** run `cleanup` only, then propose further modes based on what you saw.

## Step 3 — Confirm the plan

Before any destructive action:

1. State the target adapter and selected modes.
2. Summarise scope (file count, total size, top-level breakdown).
3. List proposed deletions explicitly. Anything ambiguous goes to a sibling `_review/` folder, not the trash.
4. Get user sign-off unless they have explicitly told you to proceed autonomously.

## Universal principles

These apply across every mode and target. Don't restate them in mode files.

### Naming
- Default `snake_case`, lowercase, no special chars except `_` and `-`.
- Names <50 chars. Don't repeat the parent folder.
- Lowercase extensions. Preserve originals.
- Disambiguate with descriptive suffixes (`contract_draft_final.pdf`), not `_1`/`_2`.
- Use states (`draft`, `review`, `final`) over `FINAL_v3_REAL_FINAL`.

### Dates
- General display: `DD-MM-YYYY` (Daniel's preference) or compact `YYMMDD`.
- Sort prefixes: `YYMMDD-`.
- Hierarchical: `YYYY/MM/` or `YY/MM/DD/`.
- For media, prefer EXIF/metadata date over filesystem mtime — fall back only if missing.

### Recursion depth
- 1–3 items: no subfolder.
- 4–9 items: subfolder only if a natural grouping exists.
- 10+: subfolders almost always help.
- Rarely exceed 5 levels deep.

### Safety
- Never delete files from active processes (open handles, lockfiles, `~$*`).
- Always-safe deletions: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.bak`, empty folders created during this run.
- Anything else borderline: move to `_review/` at the operation root, don't delete.
- Cross-platform: avoid Windows reserved names (`CON`, `PRN`, `AUX`, `NUL`, `COM1-9`, `LPT1-9`), trailing spaces/periods, leading hyphens. Keep paths <260 chars.

### Anti-over-editing
If the tree is already well-organised (consistent naming, logical hierarchy, no orphans, no typos):
- 1–3 minor issues: report, don't fix.
- 4–10 issues: targeted fixes only, don't restructure.
- 10+ or structural chaos: full reorganisation.

Don't force `snake_case` over an already-consistent convention (e.g. `Train-Case` repos).

### Reporting
End every run with a summary: counts moved/renamed/deleted, space reclaimed, `_review/` items needing user input, and a one-line verdict ("done" / "needs review" / "skipped — already clean").

## Tooling notes

- Hashing for dedupe: `sha256sum` or `rclone dedupe` (gdrive).
- Media probing: `ffprobe` for video/audio (duration, resolution, codec), `identify` (ImageMagick) or `exiftool` for images.
- EXIF dates: `exiftool -DateTimeOriginal -s3`.
- Always dry-run first when the user asks for a large operation (`mv -n`, `rclone --dry-run`, `adb` `ls` before `mv`).
