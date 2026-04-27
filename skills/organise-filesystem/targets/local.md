# Target: local filesystem

Default target. Path is on a normal mounted POSIX filesystem.

## Capabilities

- Full `mv`, `rm`, `mkdir`, `rsync`. Hashing via `sha256sum` is fast and free.
- Safe to recurse without batching concerns.
- Trash: prefer `gio trash <file>` (KDE/GNOME-aware) over `rm` for anything reversible.

## Caveats

- Check filesystem type before operations. `stat -f -c %T <path>`:
  - `ext4`/`btrfs`/`xfs`: full POSIX, birth-time available on btrfs/ext4 (256-inode).
  - `ntfs`/`exfat`/`fat`/`vfat`: case-insensitive on some mounts, no symlinks, limited mtime resolution. Don't rely on case-sensitive renames; do them in two steps (`a → tmp → A`).
  - `fuse.rclone`: see `targets/gdrive.md` instead.
- External drives: confirm the user wants the operation to apply there before recursing — ask if path is `/media/*` or `/mnt/*` and the user wasn't explicit.
- Watch for symlink loops on deep scans — use `find -L` only when needed.

## Performance

- For >100k files, prefer `fd` over `find` and `rclone hashsum sha256` (yes, works locally) for parallel hashing.
- Batch `mv` operations with `mv -t <dest>` rather than one-per-file when possible.
