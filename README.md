# Inkwell — Fill & Sign

A zero-backend document fill-and-sign tool that runs entirely in the browser. Upload a PDF, image, or Word file, type anywhere on it, let Inkwell auto-detect blank fields, and sign it by hand — then download a flattened PDF or PNG.

**Live demo:** enable GitHub Pages on this repo (see [Deployment](#deployment)) and it's live at `https://<your-username>.github.io/inkwell/`

![status](https://img.shields.io/badge/build-static-2438c4) ![license](https://img.shields.io/badge/license-MIT-181b22)

## Features

- **Multi-format upload** — PDF (multi-page, via pdf.js), PNG/JPG, and DOCX (via Mammoth; Word layout is approximated)
- **Type anywhere** — click to place editable text boxes; drag to move, resize text, switch blue/black ink
- **Auto-detect blanks** — a pixel-scan pass finds underlined fill-in spaces (`Name: ______`) and places dashed suggestion fields on them
- **E-signature** — draw on a smoothed signature pad (mouse/touch/stylus) or type your name in script styles; signatures are trimmed, placed with a click, draggable and resizable
- **Export** — flattens everything into a signed PDF (jsPDF) or per-page PNGs
- **Private by design** — no server, no upload, no storage; documents never leave the device

## Quick start

No build step. It is one HTML file.

```bash
git clone https://github.com/<your-username>/inkwell.git
cd inkwell
# open directly, or serve locally:
python3 -m http.server 8080
# → http://localhost:8080
```

Click **Try a sample form** on the empty screen to test auto-detect and signing without uploading anything.

## Keyboard shortcuts

| Key | Action |
|-----|--------|
| `V` | Select / move |
| `T` | Text tool |
| `S` | Sign tool (re-click to create a new signature) |
| `D` | Detect blank fields |
| `Esc` | Deselect / cancel placement |
| `Delete` | Remove selected field |

## How auto-detect works

1. Each rendered page is scanned row-by-row for dark horizontal pixel runs (length between 5% and 92% of page width, small gaps tolerated).
2. Overlapping runs on adjacent rows are merged into single line segments.
3. A segment qualifies as a fill-in blank only if the region above it is ~98% empty — this filters out divider rules, table borders, and text strokes.
4. A dashed suggestion field is placed above each qualifying underline, skipping spots that already contain a field.

## Architecture

Single-file vanilla JS (`index.html`), no framework, no bundler.

| Concern | Approach |
|---------|----------|
| PDF rendering | [pdf.js](https://mozilla.github.io/pdf.js/) 3.11 (cdnjs) |
| DOCX → pages | [Mammoth](https://github.com/mwilliamson/mammoth.js) → styled stage → [html2canvas](https://html2canvas.hertzen.com/) → sliced into A4-ratio pages |
| PDF export | [jsPDF](https://github.com/parallax/jsPDF) 2.5, `px_scaling` hotfix |
| Zoom | CSS `transform: scale()` on a fixed-size page wrapper — field coordinates stay in page-pixel space |
| Fields | Absolutely positioned overlays; `contenteditable` for text, `<img>` for signatures; pointer events for drag/resize |

### Known limitations

- Exported PDFs are **rasterized** (image-based), not re-editable AcroForm PDFs. For true form-field output, a pdf-lib pipeline would be the next step.
- DOCX rendering is best-effort; complex Word layouts (columns, floating images, headers/footers) may shift.
- Auto-detect targets underline-style blanks; it does not yet detect checkboxes or boxed fields.

## Deployment

Pushing to `main` deploys automatically via the included GitHub Actions workflow (`.github/workflows/deploy.yml`).

One-time setup: **Settings → Pages → Source → GitHub Actions**.

## 繁體中文簡介

Inkwell 是一個純前端的文件填寫與簽名工具：上傳 PDF／圖片／Word 檔，直接在文件上輸入文字，自動偵測待填空格（底線欄位），並以手寫或打字方式插入電子簽名，最後匯出合併後的 PDF 或 PNG。所有處理皆在瀏覽器本機完成，文件不會上傳到任何伺服器。

## License

[MIT](LICENSE)
