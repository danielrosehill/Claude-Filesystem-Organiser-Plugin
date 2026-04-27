# Target: Android device (ADB)

Path lives on an Android phone or tablet, typically under `/sdcard/...` (alias for `/storage/emulated/0/`). Operate via `adb`.

## Connection sanity check

```
adb devices
```

Expect exactly one device with `device` status. If `unauthorized`, prompt the user to accept on the phone. If multiple devices, require `adb -s <serial>` for every command.

## Capabilities

- `adb shell ls`, `mv`, `rm`, `mkdir` — standard.
- `adb pull` / `adb push` — for round-tripping batches when shell ops are too slow.
- No symlinks under `/sdcard/` (FUSE-mediated emulated FS).
- Case-insensitive on emulated storage.

## Strategy

For large reorganisations, prefer **pull → reorganise locally → push back → verify → delete original**:

```
adb pull /sdcard/DCIM /tmp/dcim_work
# operate on /tmp/dcim_work locally
adb push /tmp/dcim_work /sdcard/DCIM_new
# user verifies on phone, then:
adb shell rm -rf /sdcard/DCIM
adb shell mv /sdcard/DCIM_new /sdcard/DCIM
```

Only do in-place `adb shell mv` for small operations (<200 files) where the user is watching.

## Caveats

- **Media scanner cache** — the Gallery / Files app may show stale entries after moves. Trigger a rescan:
  ```
  adb shell am broadcast -a android.intent.action.MEDIA_SCANNER_SCAN_FILE -d file:///sdcard
  ```
- **Scoped storage** — paths under `/sdcard/Android/data/<pkg>/` belong to specific apps and may be read-only or invisible without root. Stay out unless the user explicitly asks.
- **Reserved paths** — never touch `/data/`, `/system/`, `/vendor/`, etc.
- **EXIF**: photos under `/sdcard/DCIM/Camera/` carry full EXIF. Use `exiftool` after `adb pull`.
- **WhatsApp / messenger media**: `WhatsApp Image YYYY-MM-DD at HH.MM.SS.jpg` filenames are reliable date sources when EXIF was stripped by the app.

## Safety

- Always `adb pull` a backup of the target tree to `/tmp/<name>-backup-<date>` before any destructive in-place operation, unless the user has waived this.
- No `adb shell rm -rf` on anything except a folder created by this run, without explicit user confirmation including the literal path.
