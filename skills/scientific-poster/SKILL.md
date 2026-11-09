---
name: scientific-poster
description: Plan, write, build and review scientific and engineering posters in the assertion-evidence style (a sentence title stating the main finding, sections headed by their messages over large evidence, a QR code to the details) in Typst with the sci-propos-poster package or in PowerPoint with the sci-propos poster templates, for A0, A1, 48 x 36 in and 16:9 e-posters. Use for conference and student research posters, e-posters, poster critiques, and turning a talk or paper into a poster.
compatibility: Typst 0.15 with the sci-propos-poster package for Typst posters; PowerPoint or LibreOffice for the .potx poster templates; poppler (pdftoppm, pdfinfo) to render and check; uv with python-pptx for PowerPoint inspection; Node with pptxgenjs 4.0.1 only to rebuild templates.
---

# Scientific posters

A poster has to work in two situations: with you beside it, as the start of
a conversation, and alone, when it must tell its story to someone walking
past. Most visitors give it a few seconds; some stop for thirty; a few talk
for ten minutes. This skill builds posters for all three. It follows
Michael Alley's assertion-evidence approach and his poster guidance
(craftscicom.org), the MIT Communication Lab's #evenbetterposter, and Zen
Faulkes's *Better Posters*:

- the **title is the main finding**, stated as one sentence;
- each **section is headed by its message**, a sentence, over **large
  evidence**;
- a **QR code** leads to the paper, data and code, so the poster can show
  less;
- text stays short: paragraphs of four sentences at most.

| Format | What to use |
|---|---|
| Typst | The `sci-propos-poster` package (with `sci-propos-core`), §8 |
| PowerPoint | `poster-a0-*.potx`, `poster-48x36-*.potx`, `poster-16x9-*.potx`, §9 |

## 1. Find the tools first

The package and templates live in the **`sci-propos`** repository; the
package isn't on Typst Universe yet.

1. **Typst.** Compile a two-line file:
   `#import "@preview/sci-propos-poster:0.1.0": *` then
   `#show: poster`. If the package is missing, look for a `sci-propos`
   checkout in the user's folders. Its README says how to copy `core/` and
   `poster/` into Typst's local package folder:

   | System | Package folder |
   |---|---|
   | macOS | `~/Library/Application Support/typst/packages/preview/<name>/0.1.0/` |
   | Linux | `~/.local/share/typst/packages/preview/<name>/0.1.0/` |
   | Windows | `%APPDATA%\typst\packages\preview\<name>\0.1.0\` |

   Ask before copying into system folders.
2. **PowerPoint.** The checkout's `powerpoint/posters/` folder.
3. **Nothing found?** Ask for the repository, or build from the rules in
   this file and say so.

Work files (renders, crops, test prints) go in the project's `tmp/` folder.
For Python steps use `uv` (`uv run --with python-pptx python …`); if `uv`
is missing, stop and ask.

## 2. Settle the brief

Ask only for what the request and files don't already say.

- **Occasion:** conference, student symposium, department open day,
  e-poster session. Get the **board size and orientation** from the call
  for posters; printers and boards differ (A0 portrait in much of Europe
  and Asia, 48 × 36 in landscape in much of North America, a 16:9 screen
  for e-posters).
- **Audience:** mixed, nearly always: specialists, people from nearby
  fields, students. Write the title and the first section for the widest
  of them.
- **Main finding:** the one sentence a visitor should leave with.
- **Source material:** the paper, the talk, the data. Which figures exist?
- **Language and style:** `en`, `fr`, `zh-Hans`, `zh-Hant`; sans (default,
  as AE recommends) or serif.
- **Placeholders:** names, affiliations, logos, the QR target, funding.
  Never invent them; mark them `[like this]`.

## 3. Plan from messages, then sketch

1. **Scope and depth.** Choose what to leave out. With four experiments,
   show the one or two that carry the finding. Go deep enough for
   specialists on those, and keep the rest for the QR code and the
   conversation. Too much content is the most common failure.
2. **Write the title** as the main finding, one sentence (§5.1).
3. **Write the section messages**, three to six sentences, in the order of
   the research: why it matters, what was done, what was found, what it
   means. Read them in order: they must tell the story alone.
4. **Pick the evidence** for each message: usually one figure, sometimes
   two, each able to prove its sentence (§6).
5. **Sketch two or three layouts** on paper, in the orientation of the
   board. Keep the one where the evidence is largest and the reading order
   is plain (down each column, left to right). Don't let a template decide
   the content.
6. **Layer the reading:** the title in five seconds; the headings,
   highlights and figures in thirty; the text and captions for those who
   stay.

## 4. Structure

From top to bottom:

1. **Title** (§5.1), then authors with numbered affiliations.
2. **Orienting block:** a picture that shows what the work is about (the
   system, the site, the question) beside two or three **highlights**,
   short sentences with numbers. On portrait boards it runs across the
   page under the title; on landscape boards, where height is scarce, it
   tops the first column.
3. **Sections**, each headed by its message. Typical sequence:
   - the problem and why it matters (the first section must orient a
     visitor in about **20 seconds**);
   - what was done (method, set-up, design), with a diagram, not a list;
   - the results, the largest figures;
   - what they mean, or what comes next, ending with an invitation to talk.
4. **Bottom band**, in small type: the QR code with what it leads to
   ("Paper, data and code"), contact, a few references, acknowledgments
   in one or two lines, logos.

Leave out:
- an **abstract block**: the title, highlights and headings do its job.
  Include one only if the conference requires it.
- **logos at the top**: they compete with the title. Keep them small, in
  the bottom band.
- **long methods and references**: they go behind the QR code.

## 5. Writing

### 5.1 The title

- One sentence stating the main finding, in plain words: "A $120 optical
  sensor records storm-driven river turbidity within 5 % of a laboratory
  instrument", not "Development of a Low-Cost Turbidity Sensor".
- Sentence case, no final period, **never all capitals** (capitals hide
  the word shapes that readers rely on).
- At most two lines on the poster; break between phrases; avoid a one-word
  last line. Keep the small words (of, in, the) that show relations.
- A phrase title is acceptable when the work has no single result, but
  then the highlights must carry the findings.

### 5.2 Section messages

- Full sentences with a verb, stating what the section shows, with the
  key number: "Over six months, daily means stayed within 5 % of the
  reference", not "Field results".
- One or two lines; sentence case; no final period.
- When a section holds two figures, head each with its own message
  (`message[…][…]` in Typst).

### 5.3 Text

- **Paragraphs of four sentences at most**; most sections need two or
  three sentences. Short sentences, under 20 words on average.
- Lists only for items that are a set (three to four, one line each);
  otherwise a diagram or a table.
- Say what to notice in each figure, in the text or the caption.
- Aim for roughly 300–500 words of running text on an A0 poster, fewer on
  an e-poster. Count them.
- Cite sources by number in the text; full references are few and small,
  or behind the QR code.

## 6. Evidence

- **Large.** Figures take most of the area. A poster that is mostly text
  is a paper on a wall.
- **Readable at print scale.** Labels, tick numbers and legends at least
  the caption size (24 pt on A0). Slide figures usually need enlarging by
  1.5–2. Never shrink a dense figure to fit; cut it to what proves the
  sentence.
- **One claim per figure:** remove series and panels that don't support
  the heading above.
- **Choose the form by what links the items:** a photo for an object, call-
  outs for its parts (at most four), a graph for a trend, a bar chart for
  a comparison, a diagram for a process. Small tables (four rows and
  columns) are fine; larger ones become graphs.
- **Contrast and color:** dark on light; one accent color; never red
  against green alone; text on photographs only over a calm area.
- **Credit** every borrowed image in its caption.

## 7. Visual style

- White background, one primary color (default `#00561F`) for section
  messages, highlights and links; near-black text; grays of at least 4.5:1
  contrast on white. No gradients, shadows, or boxes around every section
  (white space separates sections better than frames). One tinted block
  for the orienting picture is enough.
- **Sans by default** (Noto Sans in Typst, Calibri in PowerPoint); serif
  on request.
- **Type sizes on A0** (scaled by the square root of the page area for
  other sizes, so every size reads alike):

  | Element | Size |
  |---|---|
  | Title, bold | 84 pt (readable from about 5 m) |
  | Authors / affiliation | 38 / 30 pt |
  | Highlights | 40 pt |
  | Section message, bold, primary | 44 pt |
  | Body | 30 pt (readable from about 1.5 m) |
  | Captions, figure labels | 24 pt |
  | Bottom band | 21 pt |

- **Grid:** two columns on portrait A0 or A1, three on 48 × 36 in, four on
  a 16:9 e-poster. Columns fill in order; each section stays in one column;
  columns start at the same top.
- **One page.** Content that doesn't fit must be cut, not shrunk below the
  sizes above.

## 8. Typst: `sci-propos-poster`

```typst
#import "@preview/sci-propos-poster:0.1.0": *

#show: poster.with(
  size: "a0",            // "a0-landscape", "a1", "48x36", "36x48", "16:9"
  title: [A \$120 optical sensor records storm-driven river turbidity within 5 % of a laboratory instrument],
  authors: [#ph[First Author]#inst(1), #ph[Second Author]#inst(2)],
  affiliation: [#inst(1)#ph[Department, University] · #inst(2)#ph[Organization]],
  graphic: image("site.jpg", width: 100%),
  points: ([Weekly samples miss the storms], [The reading is linear to 1000 NTU], [Six months within 5 %]),
  qr: "https://example.org/paper",
  qr-caption: [Paper, data and code],
  contact: [#ph[Name] · #ph[email\@example.org]],
  references: [[1] Author, title, journal (year).],
  acknowledgments: [#ph[Funding agency, grant number.]],
  logos: image("logos.svg", height: 40mm),
)

= Weekly samples miss the storms that carry most of the sediment
Between storms, the river runs near 20 NTU; weekly samples land on quiet days [1].
#figure(image("storms.svg", width: 100%), caption: [One month of turbidity.])
```

- `typst init @preview/sci-propos-poster:0.1.0 my-poster` starts from a
  template whose comments say what each part needs. The repository's
  `examples/river-sensor/poster.typ` is a complete poster built from a
  talk's figures, in three sizes from one source.
- **Options:** `size`, `columns`, `title`, `authors`, `affiliation`,
  `graphic`, `points`, `orient` (`"band"` or `"column"`; `auto` follows the
  orientation), `qr`, `qr-caption`, `contact`, `references`,
  `acknowledgments`, `logos`, `keep-sections` (each section in one
  column; default on), `lang`, `style`, `primary`, `scale`, font overrides.
- **Helpers:** `highlights(..)`, `message(msg, body)`, `annotate` (call-outs
  with poster-weight lines), `two-columns`, `code-panel`, `qr-code`,
  `credit`, `ph`, `inst`. `#colbreak()` forces a new column.
- **Enlarging slide figures:**
  `scale(170%, reflow: true, figure-content)`, with some space above when
  the figure's axis title sits above its box.
- **One page, always:** too much content spills onto a second page that
  carries a red flag. Check `pdfinfo poster.pdf` shows one page, for every
  size you will print.
- Fonts: pass `--input fonts="$(typst fonts | paste -sd, -)"` to avoid
  warnings; Noto fonts must be installed.

## 9. PowerPoint: the poster templates

`powerpoint/posters/` holds `poster-a0-*.potx` (A0 portrait, two
columns), `poster-48x36-*.potx` (landscape, three columns) and
`poster-16x9-*.potx` (e-poster, four columns), each in sans and serif.

- The slide **is** the poster at full size: type sizes are the printed
  ones. Print at 100 %.
- Title, authors and affiliation are placeholders; sections, highlights,
  gray picture boxes and the bottom band are ordinary shapes to edit, move
  or delete. Replace each gray box with a picture (Insert › Pictures).
- Replace the QR code (it points to example.org) and the logo box. Make QR
  codes elsewhere (Typst's tiaoma package, for instance); PowerPoint can't.
- The speaker notes repeat the size ladder and the checks.
- Building in code: open the `.potx` with python-pptx (switch its content
  type to a presentation), or adapt `source/build_poster.js`.

## 10. At the session

- Prepare a **30-second version** (the title, then the one figure that
  proves it) and a **two-minute version** (the story through the section
  messages). Ask visitors what they work on, and choose the version.
- Stand beside the poster, not in front of it; point at the evidence.
- For an e-poster, check the screen's resolution and viewing distance; text
  that is fine on paper may be too small on a monitor. Fewer words.
- Bring the QR target online before the session; a few printed A4 copies
  help visitors who don't scan.

## 11. Workflow and checks

1. Brief (§2), messages and sketches (§3), agreed with the author.
2. Build on the package or template.
3. **Render and look**, at two scales:
   - the whole poster at about 10 dpi (`pdftoppm -r 10 -png poster.pdf
     tmp/far`): this is the view from across the room. The title and the
     section messages must read; the layout must show its order.
   - crops at about 40 dpi: labels, captions and the bottom band.
   For PowerPoint, convert a copy to PDF first (`soffice --headless
   --convert-to pdf --outdir tmp poster.pptx`). If you can't view images,
   ask the user to check them.
4. **Arm's-length test:** print the PDF on A4 or Letter. If the body text
   reads at arm's length, it reads at about 1.5 m on the poster.
5. Check before delivering:
   - one page, in every size that will be printed;
   - the title states the finding in at most two lines; the section
     messages read in order tell the story;
   - a visitor knows the topic and why it matters within 20 seconds;
   - each figure proves its message; labels at caption size or larger;
     every borrowed image credited;
   - paragraphs at most four sentences; no abstract block unless required;
   - type sizes at least the ladder of §7; contrast at least 4.5:1;
   - the QR code works and says what it leads to; logos small, at the
     bottom;
   - facts and numbers match the source; placeholders marked.

## Related skills

When they are installed: `scientific-presentation` for the talk on the
same work (posters often start from its figures), and `scientific-writing`
for the abstract submitted with the poster and the paper behind the QR code.

## Sources

The rules are paraphrased; no text or templates of the sources are
included.

- craftscicom.org: Posters, Poster Examples, Poster Templates (Michael
  Alley).
- M. Alley, *The Craft of Scientific Presentations*, 2nd ed., Springer,
  2013 (audience and scope).
- MIT Biological Engineering Communication Lab, "Toward an
  #evenbetterposter" (2023), and its Poster Resources.
- Z. Faulkes, *Better Posters* (Pelagic, 2021) and the Better Posters blog.
