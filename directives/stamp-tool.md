# Directive: Top 10 Photo Stamper

## Goal

Provide students a zero-install browser tool to burn sequence numbers (1–10) onto their daily photo submissions, enabling unambiguous photo referencing during feedback sessions.

## The Tool

- **File**: `stamp.html` (single self-contained HTML file at project root)
- **Runtime dependency**: JSZip 3.10.1 (loaded from cdnjs CDN)
- **Requirements**: Modern browser (Chrome 99+, Safari 15.4+, Firefox 112+, Edge)

## How It Works

1. Student opens `stamp.html` in any browser
2. Drags 1–10 photos into the drop zone (or clicks to browse)
3. Photos appear in a numbered preview grid
4. Student reorders by dragging cards (or using arrow buttons)
5. Numbers update automatically on reorder
6. "Download All" stamps each photo and delivers a ZIP: `top-10-stamped-YYYY-MM-DD.zip`
7. ZIP contains `01.jpg` through `10.jpg` (matching the stamp numbers)

## Stamp Specifications

| Property | Value |
|----------|-------|
| Position | Top-left corner |
| Inset | `fontSize * 0.5` from edges |
| Font size | 8% of shorter image dimension |
| Font | Bold, system sans-serif (-apple-system, BlinkMacSystemFont, "Segoe UI") |
| Text color | White (#FFFFFF) |
| Background | Rounded rect, `rgba(0,0,0,0.65)` |
| Corner radius | `fontSize * 0.25` |
| Padding | `fontSize * 0.45` horizontal, `fontSize * 0.28` vertical |
| Output format | JPEG at 92% quality |
| File naming | `01.jpg` – `10.jpg` (zero-padded) |

## Distribution

- Upload `stamp.html` to Circle as a downloadable resource in the Start Here / Resources space
- Alternative: host on a static URL (GitHub Pages, S3, etc.) so students can bookmark it
- Students download or bookmark and reuse indefinitely — no updates needed on their end unless the tool changes

## Integration with Feedback Pipeline

This tool is a preprocessing step:

1. Student stamps photos with this tool → gets `01.jpg` through `10.jpg`
2. Student uploads stamped photos to Circle Top Tens space
3. `sync_photos.py` (in Circle PFM Feedback Submission System) syncs to Google Drive
4. Teachers reference photos by number during feedback calls ("Photo 7 needs exposure work")

The `01.jpg`–`10.jpg` naming convention matches `sync_photos.py` (line 319: `filename = f"{i:02d}{ext}"`).

## Edge Cases

- **HEIC files**: Not supported by Canvas API in most browsers. Students must export as JPEG from iPhone Photos first. iOS Safari can handle HEIC natively, but the safest path is JPEG.
- **Large DSLR files**: 10 images at ~30MB each works but processing is sequential to manage browser memory (~1.6GB canvas data). May take 10–20 seconds.
- **More than 10 photos**: Tool rejects with a clear error message. Students must remove extras before proceeding.
- **Mixed formats**: Accepts JPEG, PNG, and WebP. Output is always JPEG at 92%.
- **EXIF orientation**: Modern browsers auto-apply EXIF rotation when drawing to canvas. No manual parsing needed.
- **Older Chromebooks**: `roundRect` has an arc-path fallback for browsers that don't support the native method.

## Updating the Tool

Edit `stamp.html` directly. It is a standalone file with no build step. Test by opening in a browser and dragging in sample images. If distributing via a hosted URL, re-upload the updated file.
