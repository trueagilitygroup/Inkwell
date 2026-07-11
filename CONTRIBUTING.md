# Contributing to Inkwell

Thanks for helping improve Inkwell.

## Ground rules

- The app stays a **single HTML file** (`index.html`) with **no build step**. Libraries load from cdnjs only.
- No servers, no analytics, no storage of user documents. Privacy is the core promise.
- Keep the UI vocabulary consistent (a button that says "Download PDF" produces a toast that says "PDF downloaded").

## Development

```bash
python3 -m http.server 8080   # or any static server
```

Test with the built-in **Try a sample form** button, plus at least one real multi-page PDF and one image scan.

## Pull requests

1. One feature or fix per PR.
2. Describe how you tested (browser, input formats).
3. For changes to field detection, include a before/after screenshot on the sample form.

## Good first issues

- Checkbox detection (small dark squares with empty interiors)
- Date field quick-insert (today's date, locale-aware)
- AcroForm export via pdf-lib as an optional second export path
