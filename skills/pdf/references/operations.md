# Reading, rendering, and editing PDFs

Use this reference for ordinary PDF operations. Verify dependencies on the machine; these examples do not install system tools or change the user's project environment.

## Extract and inspect

`pdfinfo input.pdf` reports page count and document metadata when Poppler is installed. For a text sample, use `pdftotext -f 1 -l 3 -layout input.pdf tmp/pdf-sample.txt`, then read the result with Pi's `read` tool. Remove `-layout` if its spacing obscures reading order. For large files, extract only relevant pages rather than dumping the whole document into context.

When `pdftotext` is unavailable, write a short task-specific script to `tmp/pdf-text.py`:

```python
from pathlib import Path
import sys
from pypdf import PdfReader

source, destination = sys.argv[1:3]
document = PdfReader(source)
if document.is_encrypted:
    raise SystemExit("Encrypted PDF: arrange authorized access first")

sections = []
for number, page in enumerate(document.pages, start=1):
    sections.append(f"[Page {number}]\n{page.extract_text() or ''}")
Path(destination).write_text("\n\n".join(sections), encoding="utf-8")
```

Run `uv run --no-project --with pypdf tmp/pdf-text.py input.pdf tmp/pdf-text.txt` (or use the project's established Python tooling). Read the output file. Empty text does not imply an empty page: check a rendered page for scans, text drawn as outlines, or unusual layout.

For tables in a digitally generated PDF, write a task-specific `pdfplumber` script and run it with `uv run --no-project --with pdfplumber ...`. Its `page.extract_tables()` returns candidate rows and cells, not a verified spreadsheet. Record page numbers and compare headers, merged cells, blank entries, and totals against a rendered page before exporting CSV. Scanned tables need OCR and extra manual checking.

## See the page

If Poppler is present, render only the pages you need: `pdftoppm -f 2 -l 2 -r 160 -png input.pdf tmp/pdf-page`. Check the produced filename, then open its PNG with Pi's `read` tool. For higher-resolution inspection raise `-r`; large pages consume more time and image context. If Poppler is absent, a task-specific `pypdfium2` script can render a selected page to PNG; run it with `uv run --no-project --with pypdfium2 --with pillow ...`. When previewing a form, call `PdfDocument.init_forms()` **before** retrieving the page; otherwise its field values may not appear in the rendered image. Do not claim visual verification until an image has actually been examined.

## Reorganize or produce pages

Use `pypdf` for routine page changes. `PdfWriter.append(source)` copies a document's pages; `PdfWriter.append(source, pages=(start - 1, end))` selects the user's inclusive 1-based page range `start`–`end` (the Python end index is exclusive). Append several inputs in the requested order, then `writer.write(output)`. For repeated work, create a small script in `tmp/` that accepts input and output paths as arguments, run it with `uv run --no-project --with pypdf ...`, and check the output page count. If `qpdf` is already installed, it is another option for page-level operations; consult its help for the installed version rather than assuming a particular release.

Page-copying is not the same as preserving every feature: inspect bookmarks, attachments, annotations, and interactive forms when they matter. When merging forms that reuse field names, address those collisions before merging; the [pypdf merging guide](https://pypdf.readthedocs.io/en/stable/user/merging-pdfs.html) describes namespacing with `add_form_topname()`. Rotations and overlays require a visual check. Cropping or drawing a black rectangle does **not** securely redact underlying content.

To extract *embedded raster images*, check for `pdfimages` and use `pdfimages -list input.pdf` to inspect candidates, then `pdfimages -all input.pdf tmp/pdf-image` for the originals. This differs from rendering pages: vector drawings and text are not standalone embedded images. Inspect the extracted files and their page associations before naming them as figures.

For an overlay or watermark, `pypdf` can merge a PDF page over or under existing page content. Check orientation, page dimensions, stacking order, and the rendered result; an overlay is not a redaction. If the document is signed, get agreement before altering it.

For password protection, use the [pypdf encryption guide](https://pypdf.readthedocs.io/en/stable/user/encryption-decryption.html) and explicitly select a supported AES algorithm; pypdf's default encryption algorithm is not suitable for new secure PDFs. AES operations may require the `cryptography` dependency. Work only with documents the user is authorized to access. Arrange a safe way to provide any password, rather than placing it in a command, script, or chat log; confirm the output opens as intended. Do not promise that PDF passwords replace proper access control.

For a new PDF, prefer the source project's normal export or build workflow when one exists (for example, a document generator already configured by the project). For a standalone report, `reportlab` can lay out text and tables; run task-specific code with `uv run --no-project --with reportlab ...`. Choose page size and fonts deliberately, especially for non-Latin text. Reopen and render the output before delivery: valid PDF syntax does not prove text fits on the page.

## Public API documentation

- [pypdf documentation](https://pypdf.readthedocs.io/en/stable/) (page operations, extraction, and forms)
- [pdfplumber documentation](https://github.com/jsvine/pdfplumber#readme) (text positions and table detection)
- [ReportLab user guide](https://www.reportlab.com/docs/reportlab-userguide.pdf) (PDF generation)
- [pypdfium2 documentation](https://pypdfium2.readthedocs.io/) (rendering)
