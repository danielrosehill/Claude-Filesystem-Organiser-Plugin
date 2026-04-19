Sort loose files in a directory into subdirectories by file type.

Ask the user for a target directory if not provided as $ARGUMENTS.

1. **Load guardrails** from `guardrails/`.

2. **Catalogue files** in the target directory (non-recursive by default):
   - Group by file extension
   - Show count per type

3. **Propose organisation** using sensible category folders:
   - `documents/` — .pdf, .doc, .docx, .odt, .txt, .md, .rtf
   - `images/` — .jpg, .jpeg, .png, .gif, .svg, .webp, .bmp
   - `videos/` — .mp4, .mkv, .avi, .mov, .webm
   - `audio/` — .mp3, .wav, .flac, .ogg, .m4a
   - `archives/` — .zip, .tar, .gz, .7z, .rar
   - `spreadsheets/` — .csv, .xlsx, .xls, .ods
   - `code/` — .py, .js, .ts, .sh, .go, .rs, .java, etc.
   - `other/` — anything that doesn't fit the above

4. **Wait for user approval**. The user may want to customise the categories.

5. **Execute and log** to `logs/`.
