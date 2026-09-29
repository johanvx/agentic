# Authoring and changing slides

Use this reference for new editable decks and ordinary updates to existing PPTX files. Work from the user's content and visual requirements; do not fill gaps with invented metrics, citations, or images.

## Start with structure

A new `Presentation()` uses a default theme and size. Set `slide_width` and `slide_height` *before* adding slides if the requested aspect ratio differs. When starting from a deck, keep its master, dimensions, and theme unless the user requests a change. Inspect the actual `slide_layouts` and their names; indexes in a branded template need not match the defaults. Add slides with an appropriate layout and fill the existing title/content placeholders before drawing everything as independent text boxes.

Write a one-off script under `tmp/` using Pi's `write` tool. For example:

```python
import sys

from pptx import Presentation
from pptx.util import Inches

output = sys.argv[1]
deck = Presentation()
deck.slide_width = Inches(13.333)
deck.slide_height = Inches(7.5)

title = deck.slides.add_slide(deck.slide_layouts[0])
title.shapes.title.text = "Facilities update"
title.placeholders[1].text = "Operations · September 2026"

status = deck.slides.add_slide(deck.slide_layouts[5])
status.shapes.title.text = "Readiness"
body = status.shapes.add_textbox(Inches(1), Inches(2), Inches(10), Inches(2))
body.text_frame.text = "Two sites are ready for inspection."
status.notes_slide.notes_text_frame.text = "Confirm site names before presenting."

deck.save(output)
```

Run `uv run --no-project --with python-pptx tmp/pptx-task.py facilities-update.pptx` if no established Python project workflow applies. The example's layout indexes refer only to `python-pptx`'s default presentation; for an existing theme, inspect its layouts first. Replace sample text with supplied facts and remove unneeded placeholder content.

## Keep it editable and readable

Use real paragraphs for lists; size and align text boxes to the chosen canvas. Prefer native PowerPoint charts for supported chart types (`CategoryChartData` and `slide.shapes.add_chart()`), with units, labelled axes, and the user's verified data. Tables or diagrams may work better than a crowded chart; rasterized charts are a last resort when editability matters. Include meaningful headings and source context on the slide or in notes. Fonts are not automatically embedded by naming them: check availability and leave room for substitution in the user's editor.

Pictures inserted with `slide.shapes.add_picture()` must come from an authorized asset; document image credits when required. Avoid external asset services unless the user approves. A PDF or image pasted as a full-slide background is not an editable deck. Keep contrast and typography readable at presentation size and vary layouts to fit the argument rather than applying one decorative template to every slide.

## Edit a copy, not the ZIP

Load a regular `.pptx` with `Presentation(input_path)`, target the intended slide and shape, and save to a new path. Replacing `shape.text` or `shape.text_frame.text` clears its existing paragraphs and run-level formatting. For a small text substitution, inspect the paragraphs and runs and edit the correct run if possible; phrases split across runs need a considered approach, not blanket string replacement. Check groups, tables, charts, speaker notes, and master-derived elements before claiming a slide is fully updated.

`python-pptx` has a public API to append new slides but not a general, fidelity-preserving API to duplicate, reorder, or combine arbitrary slides across presentations. Don't copy slide XML or delete relationship parts by hand for a routine request; those operations can break links to layouts, notes, charts, and media. If exact duplication or complex template surgery is required, discuss using PowerPoint or another independently tested tool. A `.potx` template and legacy `.ppt` need a verified conversion or compatible application, not just a renamed extension.

Public documentation: [python-pptx presentations](https://python-pptx.readthedocs.io/en/latest/user/presentations.html), [slides and layouts](https://python-pptx.readthedocs.io/en/latest/user/slides.html), [charts](https://python-pptx.readthedocs.io/en/latest/user/charts.html), and [speaker notes](https://python-pptx.readthedocs.io/en/latest/user/notes.html).
