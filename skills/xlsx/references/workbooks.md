# Inspect and edit workbooks

Use this reference for cell-oriented reading, ordinary edits, and new workbook output. If the file includes formulas, also read [formulas and verification](formulas.md) before modifying it.

## Inspect a bounded region

`openpyxl.load_workbook(path, read_only=True)` avoids loading a large workbook into memory at once; close it when finished. In a task-specific script under `tmp/`, list sheet names, then print a few rows with each cell's coordinate and value. For example:

```python
import sys
from openpyxl import load_workbook

book = load_workbook(sys.argv[1], read_only=True, data_only=False)
for sheet in book:
    print(f"[{sheet.title}]")
    for row in sheet.iter_rows(min_row=1, max_row=6, max_col=8):
        print("  ".join(f"{cell.coordinate}: {cell.value!r}" for cell in row if cell.value is not None))
book.close()
```

Run `uv run --no-project --with openpyxl tmp/xlsx-inspect.py input.xlsx > tmp/xlsx-preview.txt`, then inspect that file with Pi's `read` tool. This is a *sample*, not an assertion that the data ends at row 6 or column H. Inspect additional ranges named by the user; a sheet's used-range dimensions may be inflated by formatting or wrong in read-only mode. Read-only mode does not expose every workbook feature (notably charts and images), so open normally for a structural audit when memory permits. Check hidden sheets, merged cells, tables, named ranges, charts, and input conventions when they matter. Write only to the upper-left anchor of a merged range.

## Create a small editable workbook

Use `openpyxl` for workbooks that need cell formulas, layout, styles, or multiple sheets. Write an ad-hoc script using the `write` tool; for example:

```python
import sys
from openpyxl import Workbook

book = Workbook()
sheet = book.active
sheet.title = "Orders"
sheet.append(["Item", "Quantity", "Unit price", "Amount"])
sheet.append(["Notebook", 3, 12.50, "=B2*C2"])
sheet["C2"].number_format = "#,##0.00"
sheet["D2"].number_format = "#,##0.00"
sheet.freeze_panes = "A2"
book.save(sys.argv[1])
```

Run `uv run --no-project --with openpyxl tmp/xlsx-task.py orders.xlsx` outside a project with established Python tooling. The sample row is illustrative: use only the user's real figures in a deliverable. Explain units and any assumptions in the workbook, and follow the recipient's house style rather than adding arbitrary colors or fonts. For a large data-only export, consider streaming modes or a CSV/TSV file if the user agrees; they do not preserve an Excel model.

## Respect file types and data boundaries

To edit an existing `.xlsx`, load it normally (`data_only=False`), target explicit sheet names/cells, save to a *new* filename, and compare representative formulas and structure to the original. `read_only=True` and `data_only=True` are for inspection; do not save that view to produce an edited model. An openpyxl round trip can discard workbook elements it does not support, including some shapes. If the file contains pivots, external connections, embedded objects, or features outside the library's support, avoid claiming that a successful save preserved them; agree on a compatible editor or conversion method first.

For `.xlsm`, `keep_vba=True` can preserve VBA parts while editing, but cannot execute or edit macros; verify the output in an appropriate application before delivery. Match extensions and template flags for `.xltx` / `.xltm`; a rename alone is not conversion. Legacy `.xls` requires a separate, authorized conversion path.

For CSV/TSV, use the standard-library `csv` module for small jobs (explicit encoding, delimiter, and newline handling). CSV has no sheet tabs, styles, or formulas. Keep identifiers with leading zeroes as text. Untrusted strings beginning with `=`, `+`, `-`, or `@` can be interpreted as formulas by spreadsheet programs on import; decide how the recipient will open exported data before escaping those values, since escaping also changes the contents.

Public documentation: [openpyxl tutorial](https://openpyxl.readthedocs.io/en/stable/tutorial.html), [optimized modes](https://openpyxl.readthedocs.io/en/stable/optimized.html), and [Python's CSV module](https://docs.python.org/3/library/csv.html).
