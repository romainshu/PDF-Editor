# PDF Workbench

A fully local, offline PDF editing and annotation workstation built as a single HTML file. No installation, no server, no uploads — everything runs inside the browser tab.

---

## Overview

PDF Workbench is a professional-grade document tool designed for environments where files must stay on-device. Drop a PDF, Word doc, Excel spreadsheet, or image file, annotate it, search through it, fill in form fields, combine it with other documents, and export a final PDF — all without a single byte leaving your machine.

```
┌────────────────────────────────────────────────────────────┐
│  Documents & Thumbnails  │  PDF Preview  │  Tools & Search │
│  (left panel, resizable) │  (center)     │  (right panel)  │
└────────────────────────────────────────────────────────────┘
```

---

## Security & Privacy

> **No data ever leaves your machine.**

- Runs entirely in the browser — no backend, no server, no API
- No network requests are made after the initial page load
- No telemetry, analytics, or tracking of any kind
- All file processing happens in-memory inside the browser tab
- Suitable for air-gapped or strict-policy enterprise environments

---

## Quick Start

1. Open `pdf_workbench.html` directly in Chrome
2. Drop any supported file onto the page (or click **Open**)
3. Annotate, search, fill, or combine — then hit **Export** (or `Ctrl+S`)

No installation, no build step, no dependencies to install.

---

## Supported File Types

| Type | Extensions | How it works |
|------|-----------|--------------|
| PDF | `.pdf` | Native rendering via pdf.js |
| Word document | `.docx` | Converted to PDF in-browser via Mammoth.js |
| Excel spreadsheet | `.xlsx` | Each sheet becomes a PDF page via SheetJS |
| Image | `.png`, `.jpg`, `.jpeg` | Wrapped into a single-page PDF via pdf-lib |

> **Note on Word/Excel conversion:** Content (headings, text, tables, formatting) is preserved faithfully and paginated sensibly. This is not a pixel-perfect copy of what Word or Excel would print — headers/footers, charts, exact fonts, and formula display are not reproduced.

---

## Features

### Annotation Tools

Click any tool in the Annotate panel and draw directly on the page. All annotations are stored as a transparent overlay — the underlying document is never touched until you export.

| Tool | Description |
|------|-------------|
| **Pen** | Freehand drawing |
| **Line** | Straight line between two points |
| **Arrow** | Line with an arrowhead |
| **Rectangle** | Outlined rectangle |
| **Circle** | Outlined ellipse |
| **Highlight** | Semi-transparent filled rectangle |
| **Whiteout** | Solid white rectangle (for hiding content) |
| **Text** | Editable text box, placed directly on the page |
| **Eraser** | Click a shape or text box to delete it |

**Style controls** (per annotation): color, opacity, stroke width, font size, bold, italic.

#### Moving and Resizing Shapes

- **Click** any existing shape (with any tool active) to select it — a dashed box appears with resize handles
- **Drag the body** to reposition
- **Drag a corner handle** to resize proportionally (opposite corner stays fixed)
- **Drag an edge midpoint handle** to resize in one dimension only
- For **lines and arrows**, each endpoint has its own handle — drag either end independently

#### Text Boxes

- **Click the page** with the Text tool to drop an editable box and type immediately
- **Click elsewhere** to finish editing; text is committed to the annotation layer
- **Single-click** a text box (with any tool) to select it without entering edit mode — then adjust color, size, bold, italic, or drag the corner handle to resize
- **Double-click** a text box to re-open it for editing
- **Drag** a text box to reposition it anywhere on the page

### Search

Four independent search modes can run simultaneously on the same document, each using a different highlight color:

| Mode | Color | Behavior |
|------|-------|----------|
| **Pattern** | Amber | Regex search — default finds 6-digit/letter codes like `1234b6T`. Click a highlight to copy. |
| **Literal text** | Pink | Type any word or phrase — no regex needed. Click a highlight to copy. |
| **Signature** | Blue | Finds the word "Signature" — highlight only, shows page count |
| **Fillable blanks** | Green | Finds underscore lines (`______`). Click a highlight to drop a text box right on top of it |

**Regex presets available:** code pattern, English name (heuristic), email address, US phone number, SSN-style.

**OCR mode:** Check "Also OCR scanned pages" to extend any search to pages with no real text layer (scanned documents, photographed forms). Results are cached per page — OCR only runs once per page even across multiple searches.

### Document Operations

**Split** a document by page ranges (e.g. `1-3,5,7-10`) — each range downloads as its own PDF.

**Rotate** pages: 90°, 180°, or 270°, applied to the current page, selected pages, or the whole document.

**Merge** multiple documents into one: drag to reorder in the sidebar, then click Merge. Works across PDFs, images, Word docs, and Excel files in any combination.

All three operations (Export, Split, Merge) bake your annotations into the downloaded file.

### OCR

Extracts text from the currently rendered page (or a region you selected with the screenshot tool) using Tesseract.js, running entirely locally. Output appears in the OCR panel with copy and download-as-TXT options.

### Screenshot Tool

Click the camera icon in the viewport toolbar to arm the screenshot tool, then drag to select any region on the page. A floating toolbar appears with options to:

- Copy as image to clipboard
- Copy OCR text to clipboard
- Download as PNG
- Apply annotation (highlight, arrow, text, mosaic/pixelate, whiteout)

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+O` | Open file(s) |
| `Ctrl+S` | Export current document |
| `Ctrl+Z` | Undo |
| `Ctrl+Y` | Redo |
| `Delete` | Delete selected shape/text box, or clear current page |
| `Ctrl+C` | Copy screenshot region as image (when region selected) |
| `Ctrl+Shift+C` | Copy screenshot region as OCR text |
| `Escape` | Dismiss screenshot tool / close modal |

---

## Interface Layout

Three resizable panels — drag the dividers between them to adjust.

**Left panel**
- Document list (drag to reorder for merge)
- Page thumbnails (click to jump, check to select for split/rotate)
- Search match results

**Center panel**
- PDF preview with lazy rendering (only visible pages are rasterized)
- Zoom in/out, fit-width, fit-page
- Page jump input
- Annotation canvas overlay (draw directly here)

**Right panel — four tabs**
- **Annotate:** tool picker, color/opacity/stroke/font controls
- **Pages:** rotate, split, merge controls
- **OCR:** run OCR on the current page or selected region
- **Search:** pattern, literal, signature, and blank-line search

---

## Export

Click **Export** in the toolbar (or `Ctrl+S`) to download an annotated PDF. All drawings, text boxes, highlights, and whiteout regions are rasterized at 2× resolution and stamped as a transparent overlay onto each page, producing a standard PDF viewable in any reader.

Split and Merge downloads also include all annotations.

---

## Dependencies

All libraries are loaded from CDN at startup and cached by the browser. After the first load, the app functions offline.

| Library | Version | Purpose |
|---------|---------|---------|
| [pdf.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | PDF rendering and text extraction |
| [pdf-lib](https://pdf-lib.js.org/) | 1.17.1 | PDF creation, embedding, and structural edits (rotate, merge, split, export) |
| [html2canvas](https://html2canvas.hertzen.com/) | 1.4.1 | Page capture for screenshot tool and office-doc conversion |
| [Tesseract.js](https://tesseract.projectnaptha.com/) | 4.1.1 | Local OCR engine |
| [Mammoth.js](https://github.com/mwilliamson/mammoth.js) | 1.11.0 | Word (.docx) to HTML conversion |
| [SheetJS](https://sheetjs.com/) | 0.18.5 | Excel (.xlsx) parsing and HTML table export |

---

## Browser Compatibility

Designed and tested on **Chrome**. Open `pdf_workbench.html` directly (no web server required). Other Chromium-based browsers should work; Firefox and Safari may have minor differences in `contentEditable` focus behavior or `ClipboardItem` support.

---

## Limitations

- Word/Excel conversion does not reproduce headers/footers, charts, live formulas, exact font metrics, or complex styles
- English name regex search is a heuristic — it will also match other capitalized proper nouns (city names, organization names, etc.)
- Freehand pen strokes can be moved but not resized — erase and redraw for a different size
- Old binary `.doc` and `.xls` formats are not supported (only `.docx` and `.xlsx`)
- OCR accuracy depends on image quality and scan resolution; best results on 200+ DPI scans
- Clipboard image copy (`ClipboardItem`) requires a secure context (HTTPS or localhost) in some browsers

---

## File Structure

```
pdf_workbench.html    ← The entire application (HTML + CSS + JS, ~3,500 lines)
README.md             ← This file
```

---

## License

This project is provided as-is for personal and internal enterprise use.
