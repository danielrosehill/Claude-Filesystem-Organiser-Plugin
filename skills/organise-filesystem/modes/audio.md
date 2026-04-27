# Mode: audio

Organise a folder of audio files. Default sequence: classify (voice memo vs music vs other) → prune junk → format/quality buckets → date hierarchy.

## Probe

```
ffprobe -v error -select_streams a:0 \
  -show_entries stream=codec_name,sample_rate,channels,bit_rate \
  -show_entries format=duration,tags \
  -of json <file>
```

Record: codec, sample rate, channels, duration, bitrate, ID3 tags (artist/album/title).

## Top-level classification

Heuristics for voice memo / recording (not music):
- Mono, 16/22/44.1 kHz typical, no ID3 artist/album.
- Filename matches `Recording*`, `voice*`, `memo*`, `Audio*`, `Note*`, dated patterns from recorder apps.
- Duration spread is wide (seconds to hours), no album structure.

Layout:
```
recordings/        voice memos, dictation, interviews
music/             tagged ID3 with artist/album → respect existing artist/album/track conventions
podcasts/          long-form, ID3 genre = Podcast, or filename pattern
samples/           short loops/SFX (<10s, in folder named samples/loops/sfx)
other/
```

For `music/`, do NOT impose snake_case — preserve `Artist/Album/01 Track Name.flac`.

## Junk-clip pruning (recordings/)

| Signal | Action |
|--------|--------|
| Duration < 2.0s | Move to `_review/short-clips/` (likely accidental tap) |
| Duration < 0.5s AND no ID3 tags | Auto-delete |
| Total RMS power below silence threshold (use `ffmpeg -af silencedetect`) | `_review/silent/` |
| File unreadable by ffprobe | `_review/corrupt/` |

## Format buckets (inside `recordings/`)

```
hi-res/     sample_rate >= 96000  OR  bit_depth >= 24
cd/         sample_rate 44100/48000, lossless (flac/wav)
lossy/      mp3, opus, m4a, ogg
voice/      mono, <= 22050 Hz (telephony / voice-memo grade)
```

## Date hierarchy

Inside `recordings/`, apply `by-date` (default `YYYY/MM/`). Use `creation_time` from format tags, fallback to mtime.

## Naming (recordings only)

`YYMMDD-HHMMSS-<dur>s.<ext>` — duration in the name is genuinely useful for voice memos. Don't rename inside `music/`.

## Output

Report: classification split, deleted (auto), short clips flagged, format bucket distribution.
