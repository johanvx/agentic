# Inspect and verify a PowerPoint deck

Use this reference to read a PPTX or validate an edited/generated one. Text extraction, file integrity, and visual review answer different questions; report which were actually performed.

## Map the slide content

For a one-off script, write `tmp/pptx-outline.py` with Pi's `write` tool:

```python
import sys

from pptx import Presentation
from pptx.enum.shapes import MSO_SHAPE_TYPE


def nested_shapes(shapes):
    for shape in shapes:
        if shape.shape_type == MSO_SHAPE_TYPE.GROUP:
            yield from nested_shapes(shape.shapes)
        else:
            yield shape


deck = Presentation(sys.argv[1])
for slide_number, slide in enumerate(deck.slides, start=1):
    print(f"\n[Slide {slide_number}]")
    for shape in nested_shapes(slide.shapes):
        if shape.has_text_frame and shape.text.strip():
            print(shape.text)
        if shape.has_table:
            for row in shape.table.rows:
                print(" | ".join(cell.text for cell in row.cells))
        if shape.has_chart:
            print("[Chart: inspect labels and data separately]")
    if slide.has_notes_slide:
        notes = slide.notes_slide.notes_text_frame
        if notes and notes.text.strip():
            print("[Notes]", notes.text)
```

Run `uv run --no-project --with python-pptx tmp/pptx-outline.py input.pptx > tmp/pptx-outline.txt` outside existing Python projects. Read the text file with Pi's `read` tool. Checking `has_notes_slide` before accessing `notes_slide` matters: merely accessing that property can create notes in an otherwise note-free presentation. If the deck contains sensitive content, keep the outline project-local and remove it when finished.

This outline omits visuals, inherited master elements, many chart labels, and some embedded objects. A screenshot of each slide is needed to describe layout or chart appearance; the PowerPoint application may be needed for complex animations or comments. To extract an actual picture from a shape, inspect supported picture shapes with `python-pptx` (`shape.image.blob` and `shape.image.ext`) rather than unpacking arbitrary external archives into the project. Keep slide numbers when reporting findings.

## Verify the result

- Reopen the output with `Presentation(output_path)` and compare slide count, notes, visible text, table entries, and editable charts with the request. `unzip -t output.pptx` checks container integrity, not whether PowerPoint renders it correctly.
- Where LibreOffice and Poppler are available, make a fresh directory (`mkdir -p tmp/pptx-preview`), create a PDF with `soffice --headless --convert-to pdf --outdir tmp/pptx-preview output.pptx`, then render it with `pdftoppm -png -r 120 tmp/pptx-preview/output.pdf tmp/pptx-preview/slide`. Use a separate preview directory per deck, inspect the actual produced paths, and open **every** slide image using Pi's `read` tool. If `pdftoppm` is missing, a local PDF renderer (such as `pypdfium2` through `uv`) can produce PNGs instead. Do not silently install an office suite or upload the presentation to a web converter.
- Check cropping, off-slide text, shapes covering labels, placeholders left behind, legibility at projection size, chart scale and units, and consistent spacing. Objects deliberately extending beyond slide edges (bleed) can be intentional, so bounds checks are only a warning. A LibreOffice preview may substitute fonts and does not prove identical rendering in Microsoft PowerPoint.
- If no local rendering path exists, say visual QA was unavailable and ask the user to check the output in their presentation app. Do not claim the deck is polished based only on a successful save, archive test, or outline extraction.

Public API documentation: [python-pptx shape objects](https://python-pptx.readthedocs.io/en/latest/api/shapes.html) and [speaker notes](https://python-pptx.readthedocs.io/en/latest/user/notes.html).
