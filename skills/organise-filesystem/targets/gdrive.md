# Target: Google Drive (rclone)

Path is under an rclone mount (typically `~/gdrive/`) or accessed via `rclone` directly.

## Mode preference

**Prefer `rclone` commands over operating on the mount.** Mount-based `mv`/`rm` works but is slow and can hit rate limits. Use:
- `rclone lsf` — listing
- `rclone moveto` / `rclone move` — moves
- `rclone delete` — deletions
- `rclone dedupe --dedupe-mode newest` — Google Drive duplicate file collapse (uses Drive's native duplicate concept)
- `rclone --dry-run` — always first, on any large operation
- `rclone hashsum sha1` — Google Drive provides SHA1 natively, no download needed

Translate user-facing paths (`~/gdrive/Photos/2024`) to remote paths (`gdrive:Photos/2024`). Confirm the remote name with `rclone listremotes` if unsure.

## Caveats

- **API quotas**: bulk renames burn against per-user-per-100s quota. Batch into chunks of ~500 ops with brief pauses.
- **Shortcuts vs files**: Drive shortcuts look like files on the mount. `rclone lsjson --drive-shared-with-me` to inspect. Don't move shortcuts as if they were files — they break.
- **Trash**: `rclone delete` sends to Drive trash by default (recoverable for 30 days). Use `--drive-use-trash=false` only if user explicitly says permanent.
- **No POSIX permissions / no symlinks / no special files**.
- **Case sensitivity**: Drive is case-insensitive for matches but case-preserving on display. Avoid case-only renames.
- **Date metadata**: rclone preserves modtime when `--drive-server-side-modtime-set` works; EXIF inside files is unaffected.

## Mount sanity check

Before any operation on `~/gdrive/...`, run:
```
mountpoint -q ~/gdrive && echo mounted || echo NOT MOUNTED
```

If not mounted, the path looks like an empty directory and operations will silently write to the local disk. Stop and ask.

## Performance

- Hashing: `rclone hashsum sha1 gdrive:Folder` — server-side, no download.
- Listing huge trees: `rclone lsf -R --files-only` is faster than walking the mount.
