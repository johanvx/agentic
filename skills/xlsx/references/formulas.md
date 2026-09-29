# Formulas and trustworthy results

Read this before changing formula cells or delivering a workbook whose computed numbers matter. A valid `.xlsx` file can contain perfectly valid formulas with **no calculated result saved**.

## Keep expressions and values separate

`openpyxl` reads a formula such as `=B2*C2` with `data_only=False` (the default). Loading the *same input file separately* with `data_only=True` returns its last stored result, if any. It does not execute the formula; the stored result can be stale or absent. Never save a `data_only=True` workbook as the edited copy: that view does not retain its formula expressions.

Use formulas for results that need to update when inputs change; use literal values when the user requested a static snapshot. Preserve existing assumptions and refer to their labeled cells rather than baking unexplained numbers into formulas. Excel formulas use English function names and commas in the file format. Quote sheet names containing spaces, and check absolute versus relative references when copying across rows. Ask about the target Excel/LibreOffice version before choosing functions with limited cross-application support.

`openpyxl` **does not calculate formulas**. After writing one, reopen the output twice: once to confirm the expression and references, and again with `data_only=True` to see whether any cached result exists. New formula cells usually have no cache, so a `None` result is not proof of an empty calculation. Setting a workbook to recalculate on open is only a request to a future application, not evidence that the numbers have been checked.

## Validate against the task

1. Compare formulas, key input cells, and referenced sheets with the user's requested model. Spot-check at least one formula per pattern against an independently computed example. A formula can evaluate without an error and still point at the wrong rows.
2. Scan available cached values for spreadsheet error cells (for example, `cell.data_type == "e"`), and compare any errors with the original before attributing them to your edit. Check a few calculated outputs in a real spreadsheet engine when available.
3. If cached results or final numbers are required but no approved calculation engine is available, ask how to proceed rather than claiming success. Excel or LibreOffice can recalculate a *copy*, but a conversion/re-save may change formulas, external links, macros, charts, or unsupported features. Agree on that trade-off before using a different application and compare the recalculated file with the original.
4. Never execute an untrusted macro or enable external data updates just to make an error check pass. Preserve source files and sensitive model data; do not put private worksheets into online calculators.

For unusual features (dynamic arrays, linked workbooks, pivot caches, or protected models), treat generic library round trips as potentially lossy. The [openpyxl formula guidance](https://openpyxl.readthedocs.io/en/stable/simple_formulae.html) documents that formula evaluation is outside the library; its [loading options](https://openpyxl.readthedocs.io/en/stable/tutorial.html#loading-from-a-file) describe `data_only`, `keep_vba`, and external-link handling.
