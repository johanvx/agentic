# Create and edit ordinary DOCX files

Use this reference for new Word documents and straightforward edits. Read [review and verification](review.md) first if the source has tracked changes, comments, protected sections, or a template content type.

## Choose a source of truth

- **Prose already in Markdown:** Pandoc can make a Word document without a custom script: `pandoc draft.md -o report.docx`. If an approved `reference.docx` exists, add `--reference-doc=reference.docx` for its styles and page properties. Pandoc uses the reference file's styles, *not* its body content. Check the result in the intended editor; conversion may change styling or table structure. In particular, do not use a DOCX → Markdown → DOCX round trip as a formatting-preserving edit of an existing document.
- **New structured document or small edits to an existing one:** Use `python-docx` to create paragraphs, semantic headings, tables, sections, and inline images. Keep styles and content separate; don't simulate headings or lists by spaces, Unicode bullets, or manual font changes. If editing an existing document, start with `Document(input_path)` and save to a different filename.

For example, write a task-specific script at `tmp/docx-task.py` using the `write` tool:

```python
import sys

from docx import Document

output = sys.argv[1]
report = Document()
report.add_heading("Project status", level=0)
report.add_paragraph("Summary of the current milestone.")
report.add_heading("Next steps", level=1)
steps = report.add_table(rows=1, cols=2)
steps.style = "Table Grid"
steps.rows[0].cells[0].text = "Owner"
steps.rows[0].cells[1].text = "Action"
steps.add_row()
steps.rows[1].cells[0].text = "Avery"
steps.rows[1].cells[1].text = "Review the rollout plan"
report.save(output)
```

Run `uv run --no-project --with python-docx tmp/docx-task.py report.docx` only where no existing project toolchain applies. Replace example content with the user's real text; do not invent facts in a deliverable. For a long report, build from styles and sections, use table headers, and inspect page breaks and cell widths in a rendered copy. `python-docx` saves an editable DOCX, but does not perform Word's layout engine or calculate the final page count.

## Preserve existing formatting

`document.paragraphs` does not contain the paragraphs *inside tables*; inspect `document.tables` too. Word often divides a visible phrase into multiple differently formatted runs. A naive `paragraph.text = paragraph.text.replace(...)` can discard character styling and links, while `run.text.replace(...)` can miss a phrase split between runs. Locate the text in the actual structure, make a targeted edit, and compare the affected paragraph and layout afterward. If the change crosses runs or complex elements, discuss the preservation trade-off before replacing them wholesale.

Adding an image with `add_picture()` creates an inline picture; a floating or anchored image in a source document can behave differently. Save to a new DOCX and compare the relevant pages and section properties. Do not perform raw string replacement inside the DOCX ZIP: the same text can occur in several XML parts and relationships need to stay consistent.

Public documentation: [python-docx user guide](https://python-docx.readthedocs.io/en/latest/user/quickstart.html), [editing existing documents](https://python-docx.readthedocs.io/en/latest/user/documents.html), and [Pandoc's `--reference-doc` option](https://pandoc.org/MANUAL.html#option--reference-doc).
