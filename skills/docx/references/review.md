# Review-marked documents, templates, and verification

Use this reference when reading revisions or comments, changing an existing document with features outside ordinary paragraphs/tables, or checking a generated DOCX.

## Read with the right view

Pandoc's DOCX reader **accepts tracked edits and omits comments by default**. For review tasks, request the all-changes view explicitly:

```sh
pandoc -f docx -t markdown -s --track-changes=all draft.docx -o tmp/draft-review.md
```

Read the Markdown with Pi's `read` tool and distinguish original text, inserted/deleted text, and comments. `-s` includes metadata such as a Word Title-style heading that otherwise may disappear from Markdown output. For an accepted-text view, choose `--track-changes=accept` deliberately; don't conflate it with the original or with a Word-rendered review view. Use `--extract-media=tmp/docx-media` when embedded images are relevant. Neither Markdown view guarantees precise page layout or support for every Word feature. If Pandoc is unavailable, `python-docx` can inspect ordinary text and tables, but do not claim it recovered all comments or revisions.

`.dotx` templates carry a different content type from `.docx`; `.doc` is a legacy binary format. Check the actual file before choosing a converter, keep the template untouched, and ask whether the result should be a new `.docx` or a template. Do not relabel an extension to force a library to open it. Pandoc's `--reference-doc` uses a `.docx` style reference, not a general-purpose `.dotx` template merge.

## Avoid destructive review edits

Do not accept/reject changes, manufacture revision markup, or add/remove comments by rewriting XML as a routine text edit. These features involve document-wide relationships and metadata; a DOCX that opens can still misrepresent the review history. Preserve the original, agree on the intended reviewer/acceptance policy, and use the target editor or a separately tested tool when such changes are explicitly requested. If only a clean *reading copy* is needed, label a Pandoc conversion as a conversion, not as the edited original.

<!-- TODO: Add a separately tested, redistributable workflow for tracked-change and comment edits if users need one. -->

## Verify structure, content, and appearance

1. `unzip -t output.docx` checks ZIP integrity; reopening with `python-docx` or Pandoc checks that a consumer can parse it. These checks alone cannot establish visual fidelity or that review markup survived. Avoid unpacking untrusted archives into arbitrary directories or blindly executing any embedded content.
2. Compare important text, tables, links, images, section breaks, and page numbering to the request and, for edits, to the source document. Treat round trips through another format as potentially lossy.
3. If LibreOffice is installed, `soffice --headless --convert-to pdf --outdir tmp output.docx` can generate a preview. If Poppler is also installed, `pdftoppm -f 1 -l 2 -r 120 -png tmp/output.pdf tmp/docx-page` renders sample pages to PNG; open the resulting images with Pi's `read` tool. Use the PDF skill or another local renderer if available. Office engines, fonts, and printer settings can change pagination; a LibreOffice preview is not proof of identical Microsoft Word layout.
4. If a rendering path is unavailable, say so and request a visual check in the intended editor for layout-critical documents. Never claim a document is visually verified just because its ZIP archive passed a test.

Public documentation: [Pandoc's change-tracking and media options](https://pandoc.org/MANUAL.html#reader-options) and [LibreOffice command-line parameters](https://help.libreoffice.org/latest/en-US/text/shared/guide/start_parameters.html).
