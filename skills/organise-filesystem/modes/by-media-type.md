# Mode: by-media-type

Split a mixed dump (typical Downloads folder, phone backup, "stuff" directory) into top-level type buckets.

## Buckets

```
images/       jpg jpeg png webp heic heif gif tiff bmp avif
videos/       mp4 mov mkv avi webm m4v 3gp
audio/        mp3 wav flac ogg opus m4a aac aif
docs/         pdf doc docx odt rtf txt md tex pages
sheets/       xls xlsx ods csv tsv numbers
slides/       ppt pptx odp key
archives/     zip tar gz bz2 xz 7z rar tgz
installers/   deb rpm appimage msi exe pkg dmg snap flatpak
code/         py js ts go rs c cpp java sh ipynb
ebooks/       epub mobi azw azw3
fonts/        ttf otf woff woff2
data/         json xml yaml yml sql sqlite db
3d-print/     stl 3mf obj gcode
misc/         everything else
```

## Behaviour

- This is a **dispatch** mode, not a final layout. After splitting, run `video`/`image`/`audio` inside their respective branches.
- Don't move files that are clearly part of a project structure (e.g. a folder containing both `.tsx` and `.json` together — that's a repo, leave it).
- A folder with mixed contents that all clearly belong together (e.g. a downloaded course with PDFs+videos+slides under one name) stays together — move the *folder* into the most representative bucket, don't shred it.
- Hidden files (`.foo`) and dotfolders (`.config/`) are never moved.

## Output

Show pre/post tree at top-level, file counts per bucket, and any items left in `misc/` that the user might want to manually classify.
