# woi

**Drag a file, tap a key, drop it on a slice.**

woi is a tiny macOS utility that converts, compresses and moves files with a radial
wheel that appears right under your cursor while you drag. No windows, no Dock icon,
no uploads: everything happens on your Mac.

- **Fully local and private.** woi never connects to the internet. No accounts, no
  telemetry, no analytics. Its log keeps counts and formats only, never file names
  or paths.
- **Near-zero footprint.** When you're not dragging, woi sleeps: no CPU use and about
  50 MB of memory.
- **Never overwrites.** Results are saved next to the original as a new file.

Requires a Mac with Apple Silicon running macOS 13 Ventura or later.

---

## Install

1. Download `woi-1.0.0.zip` from the [latest release](https://github.com/sirrayi/woi-releases/releases/latest)
   and double-click it to unzip.
2. Drag **woi.app** into your **Applications** folder.
3. Open woi. Because woi isn't distributed through the App Store, macOS asks you to
   confirm the first launch:
   - Open woi once. macOS says it can't verify the developer; click **Done**.
   - Go to **System Settings → Privacy & Security**, scroll down, and click
     **Open Anyway** next to the woi message. Confirm with your password or Touch ID.

   Or, in Terminal:
   ```sh
   xattr -dr com.apple.quarantine /Applications/woi.app
   ```
4. The woi wheel icon appears in your menu bar. That's it, woi is running.

**Start woi automatically:** System Settings → General → Login Items → click **+** under
"Open at Login" and choose woi.

**Quit woi:** click the wheel icon in the menu bar → **Quit woi**.

woi needs **no special permissions** to see your drags. macOS only asks once, the first
time you convert a PowerPoint deck (see [Optional extras](#optional-extras)).

---

## How to use

### The basics

1. **Start dragging** a file (or several) from Finder, your desktop, or almost any app.
2. While still holding the mouse button, **tap a modifier key**:

   | Key | Wheel | What it's for |
   |---|---|---|
   | **Shift** ⇧ | **Convert** | Change the file into another format |
   | **Command** ⌘ | **Tools** | Compress, resize, rotate, cut out, merge, zip… |
   | **Control** ⌃ | **Actions** | Copy path, copy or move somewhere, Terminal, AirDrop |

3. The wheel appears around your cursor. Move onto a slice; the center of the wheel
   tells you what that slice will do.
4. **Let go of the mouse on the slice.** A small pill shows progress, then the result.
   **Click the pill** to reveal the new file in Finder.

### Opening and closing the wheel

- **Tap** the key (press and release quickly) and the wheel **stays open** for the rest of
  the drag. Use both hands, take your time.
- **Hold** the key, and the wheel stays open only **while you hold it**. Release the key to
  close it. Press it again during the same drag to bring it back.
- Already holding the key when you start dragging? The wheel opens straight away.
- **Changed your mind?** Drop on the **center** of the wheel, or drop anywhere outside
  it. Nothing happens to your file.
- You can switch wheels mid-drag: tap Shift, then ⌘, to swap Convert for Tools.

### Where results go

- New files are saved **next to the original**, e.g. `photo.heic` → `photo.jpg`,
  `photo compressed.jpg`, `photo cutout.png`.
- woi **never overwrites**. If the name is taken, it adds a number: `photo 2.jpg`.
- If the folder is read-only, results go to your **Downloads** folder instead.
- Tools that produce several files (Split, PDF → images, multi-sheet spreadsheets)
  make a folder such as `report pages/`.

### Several files at once

Drag a whole selection. The wheel shows only what applies to **all** of them, and every
file is processed in parallel. Some tools exist only for groups:

- several images → **Make PDF** (one PDF, one page per image)
- several PDFs → **Merge**
- anything, folders included → **Zip** (one archive)

---

## Convert wheel (Shift)

| You drag | You can make |
|---|---|
| **Photos**: JPEG, PNG, HEIC, WebP, TIFF, AVIF, BMP, ICO | each other, PDF |
| **Camera RAW, Photoshop PSD, JPEG XL** | JPEG, PNG, HEIC, TIFF, WebP, AVIF, PDF |
| **SVG** | PNG, JPEG, PDF, WebP, ICO, HEIC, TIFF, AVIF |
| **GIF** | MP4, WebM, PNG, JPEG, WebP, HEIC, TIFF, PDF |
| **PDF** | Word (DOCX), JPEG / PNG (one per page), TXT, RTF |
| **Word, RTF, ODT, DOC** | PDF, DOCX, TXT, HTML, RTF, ODT, DOC |
| **Markdown, TXT, HTML** | PDF, DOCX, TXT, RTF, HTML, Markdown |
| **EPUB** | PDF, DOCX, TXT, RTF |
| **PowerPoint (PPTX, PPT)** | PDF *(needs PowerPoint)* |
| **Excel, XLS, ODS** | XLSX, PDF, CSV, TSV, HTML |
| **CSV, TSV** | XLSX, PDF, TSV/CSV |
| **Subtitles**: SRT, VTT | each other, TXT |
| **Archives**: ZIP, TAR, TAR.GZ, TAR.XZ, TAR.BZ2, 7Z, RAR | ZIP, TAR.GZ, TAR |
| **Video**: MP4, MOV | each other, GIF, MP3, M4A, WAV, WebM*, MKV* |
| **Video**: MKV, WebM, AVI, WMV, MPEG, FLV, 3GP, M4V | MP4, MOV, GIF, MP3, M4A, WebM, MKV, WAV *(needs ffmpeg)* |
| **Audio**: MP3, M4A, WAV, FLAC, AIFF, AAC, OGG, Opus, WMA | M4A, WAV, FLAC, AIFF, MP3*, OGG*, Opus* |

\* needs ffmpeg, see [Optional extras](#optional-extras). The wheel only ever shows what
works on your Mac right now.

Notes:
- Photo rotation from your camera is applied, so nothing comes out sideways.
- Transparent images turned into JPEG or BMP get a white background.
- ICO output is snapped to a standard icon size.

## Tools wheel (⌘)

| Tool | Works on | What it does |
|---|---|---|
| **Cutout** | images | Removes the background, saves a transparent PNG (macOS 14+) |
| **Compress** | every image format, PDF, video | Makes the file smaller (see below) |
| **Half Size** | images | Resizes to 50% |
| **Rotate** | images, PDF | Rotates 90° to the right |
| **Flip** | images | Mirrors horizontally |
| **Clean** | images | Removes location and all other metadata |
| **Split** | PDF | One PDF per page |
| **Mute** | video | Removes the audio track *(needs ffmpeg)* |
| **Extract** | archives | Unpacks next to the archive |
| **Zip** | anything, folders too | Makes a ZIP |
| **Make PDF** | several images | Combines them into one PDF |
| **Merge** | several PDFs | Joins them into one PDF |

**How Compress works per format:**

| Format | Method |
|---|---|
| JPEG, HEIC, WebP, AVIF, TIFF | Re-encoded at a lower quality |
| PNG | Reduced to at most 256 colors with smooth dithering; stays a PNG with transparency |
| GIF | Reduced colors per frame; animation kept |
| BMP | Turned into a lossless PNG |
| ICO | Icon images stored as PNG inside the ICO |
| TIFF (lossless use) | LZW compression |
| SVG | Saved as compressed SVGZ |
| RAW, PSD, JPEG XL | Saved as a HEIC copy |
| PDF | Images inside are re-compressed |
| Video | Re-encoded to H.264 MP4 (MP4, MOV, M4V built in; other video needs ffmpeg) |

If the result wouldn't be at least 5% smaller, woi says **"already as small as it gets"**
and leaves no extra file.

## Actions wheel (⌃)

| Action | What it does |
|---|---|
| **Copy Path** | Copies the full path(s) to the clipboard |
| **Copy** | Asks for a folder, copies the file(s) there |
| **Move** | Asks for a folder, moves the file(s) there |
| **Terminal** | Opens Terminal in the file's folder |
| **AirDrop** | Opens the AirDrop sheet with the file(s) |

---

## Optional extras

woi works out of the box. Two free or common add-ons unlock more:

- **ffmpeg** adds MP3/OGG/Opus audio, WebM/MKV video, video → GIF, many more video input
  formats, and the Mute tool. Install it with [Homebrew](https://brew.sh):
  ```sh
  brew install ffmpeg
  ```
  woi finds it automatically, no restart needed. Without it, audio still converts to
  M4A/WAV/FLAC/AIFF and MP4 ↔ MOV still works, using macOS's own tools.
- **Microsoft PowerPoint** lets woi turn PPTX/PPT into PDF. The first time, macOS asks
  whether woi may control PowerPoint; click **OK**. woi closes PowerPoint again afterwards
  if it opened it.

Not supported yet: Pages, Keynote and Numbers files (the wheel says "No conversions
available").

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "woi can't be opened" on first launch | See step 3 of [Install](#install). |
| No wheel appears | Check the wheel icon is in the menu bar. Tap the key *while* the mouse button is still down and the file is moving. |
| The wheel is empty / "No conversions available" | That format isn't supported, or needs ffmpeg/PowerPoint. |
| "already as small as it gets" | The file was already well compressed. Nothing was changed. |
| PowerPoint conversion fails | Open PowerPoint once and close any welcome screen, then try again. Allow woi under System Settings → Privacy & Security → Automation. |
| Want to see what happened | The log is at `~/Library/Logs/woi/woi.log` (no file names are ever recorded). |

**Uninstall:** quit woi, then drag `woi.app` to the Trash. Optionally delete
`~/Library/Logs/woi`. woi stores nothing else.

---

This repository hosts woi release builds only; the source code is private.

© woi. All rights reserved.
