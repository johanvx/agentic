# Scanned PDFs and OCR

Use this reference when text extraction produces little useful content or the user needs a searchable scan. Inspect at least one rendered page first: selectable text may be present on only some pages, and some PDFs contain a poor existing OCR layer.

## Choose the output

- **Searchable PDF:** If `ocrmypdf` is installed, produce a separate output, for example `ocrmypdf --output-type pdf --skip-text input.pdf output-searchable.pdf`. `--skip-text` keeps pages with existing text from being OCRed again; check mixed documents carefully. OCRmyPDF defaults to PDF/A output without `--output-type pdf`, which may change the user's intended format. Its defaults may optimize or rewrite images; discuss preservation requirements before processing a sensitive original. Add `--rotate-pages` or `--deskew` only when visually needed. Check the installed version's `--help` before selecting newer options.
- **Text transcription only:** Render pages to images, then run local Tesseract with the appropriate installed language data (`tesseract --list-langs`). Store text with page markers and inspect difficult sections against the images. This does *not* add a search layer to the original PDF. If `pdftoppm` is unavailable, a small `pypdfium2` rendering script is another local route; see [operations](operations.md). Do not assume Tesseract alone accepts PDF pages as input.

If a needed renderer or OCR engine is unavailable, explain what is missing and ask before installing system utilities or using any external service. Python rendering, when appropriate, uses `uv` as described in the main skill; missing `uv` requires the user's decision before a fallback.

OCR can misread numbers, names, marks, and multi-column layouts. Validate a few pages, including low-quality ones, and label uncertain passages. Do not use OCR output as proof that the layout or legal wording has been preserved. A digitally signed PDF cannot be OCRed and retain its signature; stop and consult the user rather than bypassing that protection.

[OCRmyPDF's public cookbook](https://ocrmypdf.readthedocs.io/en/latest/cookbook.html) documents output formats, language selection, existing-text behavior, and signature limitations.
