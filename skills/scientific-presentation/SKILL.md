---
name: scientific-presentation
description: Plan, write, build and review scientific and engineering talks with the assertion-evidence approach (a sentence lead stating each slide's point, visual evidence below it) in Typst with the sci-propos-presentation package, in PowerPoint with the sci-propos templates, or in LaTeX Beamer. Use for conference and seminar talks, thesis defences, course lectures, design reviews, internship reports and outreach talks, including speaker scripts, notes handouts, slide critiques and ports between formats.
compatibility: Typst 0.15 with the sci-propos-presentation package (Touying 0.8) for Typst decks; PowerPoint or LibreOffice for the .potx templates; poppler (pdftoppm, pdfinfo) to render and check pages; uv with python-pptx for PowerPoint inspection; Node with pptxgenjs 4.0.1 only to rebuild templates.
---

# Scientific presentations

This skill turns research and teaching material into talks that an audience
can follow and remember. It rests on Michael Alley's **assertion-evidence**
(AE) approach: build the talk on messages, not topics; state each message
as a sentence; support it with visual evidence; then practice until you can
say it in your own words. Controlled studies by Alley's group found that
students who saw AE slides understood more than those who saw bullet slides
(59 % against 42 % on one test of understanding; Garner et al. 2013).

We adapt AE in one respect. Committees and students find slides by name
("the Methods slide", "slide 14"), so every content slide keeps a **short
title** for navigation. The AE sentence headline becomes the slide's **lead
line**, set just under the title. Everything else follows AE unless this
file says otherwise.

| Format | What to use |
|---|---|
| Typst + Touying | The `sci-propos-presentation` package (with `sci-propos-core`), §8 |
| PowerPoint | `presentation-sans.potx` or `presentation-serif.potx`, §9 |
| LaTeX Beamer | Guidance only, §10 |

## 1. Find the tools first

The packages and templates live in the **`sci-propos`** repository. They
aren't on Typst Universe yet, so check before building:

1. **Typst package.** Compile a two-line test file:
   `#import "@preview/sci-propos-presentation:0.1.0": *` then
   `#show: presentation-theme`. If it fails with "package not found", look
   for a `sci-propos` checkout in the user's folders. Its README says how to
   copy `core/` and `presentation/` into Typst's local package folder:

   | System | Package folder |
   |---|---|
   | macOS | `~/Library/Application Support/typst/packages/preview/<name>/0.1.0/` |
   | Linux | `~/.local/share/typst/packages/preview/<name>/0.1.0/` |
   | Windows | `%APPDATA%\typst\packages\preview\<name>\0.1.0\` |

   Ask before copying anything into the user's system folders.
2. **PowerPoint templates** are in the checkout's `powerpoint/` folder.
3. **Nothing found?** Ask the user for the repository. If they prefer, build
   from the rules in this file and say so.

Old decks on `touying-diapos` port by changing two lines: the import becomes
`@preview/sci-propos-presentation:0.1.0` and `diapos-theme` becomes
`presentation-theme`. Lead lines then need rewriting to the rules in §5.

Work files (renders, contact sheets, extracted text) go in the project's
`tmp/` folder, never beside the user's sources. For Python steps, use `uv`
(`uv run --with python-pptx python …`). If `uv` is missing, stop and ask
which Python to use.

## 2. Settle the brief

Before designing anything, know the following. Ask only for what the
request and the files don't already tell you.

- **Audience:** field, level, what they already know, why they came, and
  any bias against the claim.
- **Purpose and occasion:** inform, persuade or teach; conference session,
  defence, lecture, design review, outreach.
- **Length:** minutes, hard or soft limit, and time for questions.
- **Central message:** the one sentence the audience should remember.
- **Format, language and style:** Typst, PowerPoint or Beamer; `en`, `fr`,
  `zh-Hans` or `zh-Hant`; sans (default) or serif.
- **Placeholders:** presenter, affiliation, date, logo, photos and
  screenshots the presenter will supply. Never invent private details, and
  keep reusable files (packages, templates, skills) free of them.

## 3. Plan from messages

AE plans the story before any slide exists. Work in this order:

1. **Headline storyboard.** Write every slide's lead as a sentence, in
   order, in a plain list. Read the list alone: it must tell the talk's
   story. If two sentences say the same thing, merge them. If a gap
   appears, add a sentence. Agree on this list with the presenter.
2. **Evidence.** For each sentence, sketch the evidence that proves it
   (§5.3). A sentence with no possible evidence is background for the
   script, not a slide.
3. **Sections.** Group the sentences into two to four parts. Name each part
   in one or two words.
4. **Time.** About one minute per content slide in a research talk; less in
   a lecture with many quick steps. Script at about 140 spoken words per
   minute and keep a running total with two or three checkpoints.

Talk recipes. Adjust them to the occasion, and say which one you used.

| Talk | Opening | Body | Ending |
|---|---|---|---|
| Conference talk (10–20 min) | Title with orienting image; no outline, or the `"map"` style | Gap with its societal and technical significance, method, two or three results | Conclusion slide, last; backup slides |
| Invited seminar (45–60 min) | Title; outline; background the audience knows | Three or four parts, each opened by a section page | Conclusion slide; contact band, or a contact slide when the talk also promotes the group |
| Thesis defence | Title; outline without descriptions; a map of the field with the covered slice highlighted, and a sentence saying what is and isn't presented | One part per chapter or paper; methods backed by others' work | Acknowledgments, specific (who did what), then the conclusion slide, last; backup slides for the rest of the thesis |
| Course lecture | Title; outline with descriptions; what students will be able to do | Short sections; side cards allowed; procedures as step slides | Summary slide, then one slide for the next session, homework, readings or links |
| Group meeting | Title; the question of the week | Results with their leads; open problems stated as sentences | The decision or help needed |
| Team design review | Title; the requirement or customer need | Concepts, criteria table, selection step by step; one section per speaker with clean hand-offs; a demonstration where possible | Recommendation as the conclusion |
| Internship or industry report | Title; the problem in the company's terms | What was done, measured savings or results | Conclusion slide with the recommendation |
| Outreach | Title with a striking image | Stories and full-screen images, very few words | One memorable takeaway |

For a mathematical talk, give the first main point to **defining the
central concept** clearly, with a picture of it.

## 4. Structure

- **Title slide.** A specific title (field, then what sets the work apart),
  a subtitle that says what the talk shows, presenter, affiliation and
  date. The plain centered page (logo, then the text) is the default. Add
  an **orienting image** (the setting, the scope, or why the work matters)
  when a good one exists: AE recommends it, and it suits conference talks
  and seminars more often than defences and lectures. Stay on the title
  slide while you introduce the topic. The talk's first sentence is learnt
  by heart.
- **Outline.** Call the page "Outline" ("Plan" in French, 目录 / 目錄 in
  Chinese). The traditional list is the default, because committees expect
  it. Three styles:
  - `"parts"`: sections, each with an optional one-line **description**
    under its name (teaching and seminars: yes; defences and conference
    talks: usually none);
  - `"thesis"`: sections and subsections with dotted leaders and slide
    numbers;
  - `"map"`: AE's mapping slide, one picture per section. Repeat each
    picture on that section's section page. Good for short talks.
  Leave "Introduction", "Conclusion" and "Questions" out of a map.
- **Section pages** open every section, the first one included. They repeat
  the outline with the current section in the primary color (its
  description black) and the others gray. A section image may sit to the
  right.
- **One idea per slide.** A slide that needs two ideas becomes two slides.
- **Step-by-step procedures** get one slide per step: title
  `Platform · n/N · Step name`, one instruction line, one full-window
  screenshot. Keep a separate sequence per platform.
- **Black slide.** When no visual helps (a story, a question to the room),
  show a black slide for at least 30 seconds rather than leaving an old
  slide up.
- **Ending.**
  - A **research talk ends on its conclusion slide**. Its lead starts "In
    summary," and states the main takeaway. At most three supporting
    points follow, parallel in form, each ideally beside a miniature of a
    figure shown earlier. A slim band may carry a photo, contact details, a
    QR code and a small "Questions?". This slide stays up during questions.
    Speakers often go back to earlier slides when asked; the conclusion is
    where they return.
  - Don't end on a slide that only says "Thank you" or "Questions?". Thank
    the audience aloud. The talk's last sentence is learnt by heart.
  - **Promotional talks** (a seminar elsewhere, recruitment, outreach) may
    add a contact slide after the conclusion: photo of the group, details,
    QR code, and the takeaway as its lead. Go back to the conclusion for
    questions, or use the contact band instead.
  - **Teaching decks** may add one slide after the summary: next session,
    homework, readings, links (`thanks-slide` or `resources-slide`).
  - **Acknowledgments** in defences go before the conclusion, names grouped
    by role.
  - **Backup slides** follow an appendix marker. They don't raise the
    total and show their number alone.
- **Numbering.** The title page counts as slide 1 but shows no number, so
  the outline shows "2 / N". Pause steps don't add numbers. The footer
  locator reads "2 Results · 5 / 7".

## 5. Writing a slide

### 5.1 Title and lead

- **Title:** a short label, at most about four words ("Heat transfer"). It
  names the topic, so people can find the slide.
- **Lead:** the slide's point as **one full sentence with a finite verb**.
  It states the finding, not the topic: "At 20 % turbulence, heat transfer
  rises by 35 %", not "Heat transfer results".
  - At most **two lines** (about 2 × 45 Latin or 2 × 27 CJK characters on
    the 16:9 page). It is never shrunk; if it runs to three lines, shorten
    the sentence. The Typst theme flags a long lead in red.
  - Sentence case, no final period. Break the line between phrases; keep
    noun, verb and prepositional phrases whole; avoid a one-word second
    line.
  - Give the key number when there is one.
  - French: sentence case, no final period, French spacing before `:` `;`
    `?` `!`. Chinese: a full sentence without the final 。.
- **Slides without a claim** (title, outline, section, step, code demo,
  black) have no lead, or a lead that says what to notice.
- **Teaching variant.** To let students infer a claim, a lead may ask a
  question and the script reveals the answer. Use it sparingly.

### 5.2 Words on the slide

- Aim for **at most 20 projected words per minute** of talk. Typical
  slides show 35–40. Count them in review; it is a target, not a hard rule.
- Any other text block holds at most two lines. Most people read a one- or
  two-line block; more than half skip longer ones.
- **Credit line:** small, dark gray, bottom left: `[Author, year]`, package
  names and versions, "illustrative data". The full reference goes in the
  notes.
- No italics, underlining or capitals for emphasis on slides. Use the
  primary color or bold. In Chinese, use color, never italics.

### 5.3 Evidence

Choose the evidence from what links the items:

| The items are linked by… | Show |
|---|---|
| a fact about an object or place | a photo or drawing |
| parts of one object | call-outs on its image (`annotate`), at most four |
| time or sequence | a timeline or numbered steps |
| size or proportion | a bar chart (a pie only for parts of a whole) |
| a trend | a graph; walk the audience through the axes |
| flow, cause, feedback | a diagram (`pipeline`, arrows) |
| comparison on shared criteria | a short table, at most 4 × 4 |
| a model | the equation, with its terms labeled |
| software behavior | code beside the output it produced |
| nothing beyond being a set (requirements, options) | bullets: at most four, two lines each, one level |

Bullets are allowed, but they are the last choice: they hide hierarchy and
connections and invite reading aloud. Credit every borrowed image.

### 5.4 Side cards

Cards are for teaching only: a definition or a reminder beside the
evidence, never on a results slide. At most one deck of one to three
cards, each at most 25 words. Longer material needs its own slide.

### 5.5 Facts and data

- Verify people, dates, versions, licences and data before they go on a
  slide. Label illustrative data as such.
- Code shown next to its output must be the code that produced it.
- Reuse data across slides where it tells a story: one CSV can feed both a
  table and a graph.

## 6. Notes, rehearsal and handout

- **Rehearsal script** for every slide, in the speaker notes: what to say,
  written for the ear (short sentences), at about 140 words per minute,
  with the slide's time and a running total.
- Under it, **three to five key phrases**. AE's advice is to practice
  until you can form sentences on the spot, so the presenter rehearses from
  the script and speaks from the phrases. Mark the talk's **first and last
  sentences** "learn by heart".
- **Secondary details** belong in the notes, not on the slide: background,
  sub-assertions, caveats, and **full references**.
- **Notes handout.** AE slides plus their notes make a better handout than
  bullet slides. In Typst, compile with `handout: "notes"` (or
  `--input handout=notes`): each page shows the slide above its notes. In
  PowerPoint, print or export Notes Pages to PDF. Send the PDF, not the
  deck: people don't find the notes.
- **Delivery advice** worth passing on: give an important talk once to a
  live audience and ask for feedback; arrive early and test the equipment;
  pause in silence instead of filling; for questions, make sure you
  understood, then answer honestly and briefly.

## 7. Visual style

### Palette and type

- One primary color (default `#00561F`) for titles, highlighted section
  names, bullets, emphasis, links and the footer's section name.
  Near-black text (`#141414`). Gray `#6E6E6E` for labels, descriptions and
  inactive sections; `#505050` for credit lines; `#3C3C3C` for slide
  numbers. A hairline (`luma(205)`) under titles. White background, no
  gradients, shadows or decoration.
- **Every text color keeps at least 4.5:1 contrast on white**, so people
  with weaker sight can read it from the back of the room. Don't use
  lighter grays for text.
- **Sans by default** for slides, as AE recommends (Noto Sans in Typst,
  Calibri in PowerPoint). Serif on request (Noto Serif, Cambria). Code in
  Noto Sans Mono or Consolas. Chinese in Noto Sans/Serif SC or TC (DengXian
  or SimSun in PowerPoint).
- Placeholders are green text in square brackets: `[Presenter name]`.

### Sizes

On the standard 16:9 page. Never enlarge the page to fit content; split the
slide.

| Element | Typst (474 pt high) | PowerPoint (540 pt high) |
|---|---|---|
| Slide title, bold, primary | 28 pt | 32 pt |
| Lead line, medium, near-black | 22 pt | 25 pt |
| Body, call-outs | 20 pt (18–24) | 23 pt |
| Code | 15 pt (dense listings down to 12) | 17 pt |
| Side-card text | 17 pt | 19.4 pt |
| Outline: names / descriptions | 26 / 18 pt | 29.6 / 20.5 pt |
| Credit line | 13 pt | 14.8 pt |
| Footer: section (primary, medium) and number | 14 pt | 16 pt |
| Title page: title / subtitle | 34 / 22 pt | 38.8 / 25 pt |

### Geometry (Typst page 841.89 × 473.56 pt; PowerPoint × 960 / 841.89)

- Side margins 40 pt. Logo small at the top right; `show-logo: false` on a
  slide whose evidence needs the space, or `logo-on: "title-closing"`.
- Title level with the logo, ending at y ≈ 49; hairline at y = 58; the
  lead follows at once. With `section-label: "top"` the band keeps room for
  the label: hairline at y = 80.
- Evidence fills the space between the lead and the credit line, centered
  vertically. Space goes between elements, not into wide margins.
- Footer: the locator "2 Results · 5 / 7" at the bottom right, the section
  in the primary color and the number in dark gray; the credit line at the
  bottom left.

### Layout

- **At most two columns.** The usual exceptions are a three-box pipeline
  and a row of map tiles or cards.
- **Every slide fits one page.** The PDF page count must equal the planned
  slides plus pause steps; an extra page means overflow.
- Columns start at the same top. Captions sit right under their picture.
  Plot legends never cover data.
- **Code panels:** light gray (`#F6F6F6`), syntax highlighting on, lines
  never wrap. A label above a panel (`Typst`, `Output`) is small, bold and
  gray.
- **Tables:** three-line style (rules above and below the header and at the
  end; on slides the top and bottom rules 2 pt, the one under the header
  0.6 pt, so the difference shows on screen too), no vertical lines, bold
  header.
- **Screenshots:** full window, as large as the slide allows, no border by
  default. Missing images show a placeholder with their expected path.
- **Cards:** a frame on one side only (solid for note, definition, intro;
  dashed for recap; dotted for reminder), filled with the primary color at
  95 % lightness.

## 8. Typst: `sci-propos-presentation`

A minimal deck:

```typst
#import "@preview/sci-propos-presentation:0.1.0": *
#show: presentation-theme.with(
  config-info(title: [A specific title], subtitle: [What the talk shows],
    author: [#ph[Presenter name]], institution: [#ph[Department] · #ph[University]],
    date: datetime.today()),
  lang: "en",               // "fr", "zh-Hans", "zh-Hant"
  style: "sans",            // or "serif"
  contact: (lines: [#ph[Group] · #ph[email\@example.org]], qr: "https://example.org"),
  files: p => path(p),      // lets the theme open your logo and screenshots
)
#title-slide(image: image("setting.jpg"))
#speaker-note[First sentence, learnt by heart. …]
#outline-slide()

= Results
what the measurements show          // the section's description
#section-image(image("results.png", width: 100%, height: 100%, fit: "cover"))

== Heat transfer
#lead[At 20 % turbulence, heat transfer rises by 35 %, which the correlations miss]
#image("stanton.svg", height: 220pt)
#credit[[Author, year]]
#speaker-note[Script … Key phrases: 20 % · 35 % · correlations miss it.]

#conclusion-slide(
  lead: [In summary, high turbulence raises heat transfer well beyond the correlations],
  points: ([First point], [Second point], [Third point]),
  figures: (mini-1, mini-2, mini-3),     // optional miniatures
)
#show: appendix
== Backup: uncertainty
#lead[The rise exceeds the uncertainty at every level]
```

`typst init @preview/sci-propos-presentation:0.1.0 my-talk` starts from the
package's template. The repository's `examples/river-sensor/` is a complete
10-minute talk, with its notes handout and PowerPoint port. The package
README is the full reference; the essentials follow.

**Theme options.** Misspelled options stop the build.
- Language and fonts: `lang`, `style`, `font`, `cjk-font`, `code-font`,
  `math-font`, `math-align`.
- Look: `primary`, `base-size`, `lead-size`, `lead-weight`, `title-color`,
  `title-rule`, `content-align`.
- Structure: `slide-level` (2, or 3 for subsections), `outline-style`
  (`"parts"`, `"thesis"`, `"map"`), `descriptions`, `numbering`,
  `section-label` (`"footer"`, `"top"`, `none`), `count-title`.
- Logo and files: `logo`, `logo-on` (`"all"`, `"title-closing"`), `files`,
  `shots`, `frame-images`.
- Ending and checks: `contact: (photo:, lines:, qr:)`, `checks` (the lead
  flag).
- Handout: `handout: true` (no pause steps) or `"notes"` (slide above its
  notes).

**Slides:** `title-slide()` (centered) or `title-slide(image:)` (text left,
picture right), `outline-slide()`, automatic section
pages from `= Section`, `== Title` or `slide(title:, section-label:,
text-size:, show-logo:, …)[…]`, `conclusion-slide(lead:, points:, figures:,
image:, contact:, questions:)`, `contact-slide(lead:, photo:, lines:, qr:)`,
`black-slide()`, `focus-slide[…]`, `empty-slide[…]`,
`thanks-slide(title:, qr:)[links]`, `resources-slide(qr:)[links]`,
`bibliography-slide(bib:)`. Backup slides follow `#show: appendix`.

**Helpers:** `lead`, `credit`, `annotate(picture, (x:, y:, dx:, dy:,
label:), …)` for call-outs, `two-columns`, `aside`, `card`, `term`,
`card-deck`, `code-panel`, `labelled`, `demo`, `numbered`,
`algorithm-figure`, `shot`, `step`, `pipeline`, `remaining`, `qr-code`,
`ph`, `pkg`, `citet`, `citep`, `footcite`, `small`, `tiny`, `large`.

**Building.** `typst compile deck.typ` always works; missing files show as
placeholders. Tell the theme what exists with options or inputs:
`logo: image("logo.svg")` or `--input logo=logo.svg`; `shots: ("win-1",)`
or `--input shots=win-1,win-2`; installed fonts with
`--input fonts="$(typst fonts | paste -sd, -)"` (PowerShell:
`"fonts=$((typst fonts) -join ',')"`); fonts carried with the deck with
`--font-path fonts`.

**Pitfalls learnt:**
- A top-level `#set` or `#show` inside a slide applies to **every later
  slide**. Scope it: `#[ #set text(17pt) … ]`, `slide(text-size:)` or
  `slide(setting: body => { … })`. Deck-wide rules go right after the
  theme's `#show`.
- `lead` and `credit` move to their places only at the top level of a slide
  body, not inside grids.
- Text right after `= Section` is the section's description;
  `#section-image(…)` there sets its picture.
- `layout` gives the whole slide body; use `remaining(size => …)` to fill
  what is left.
- Dense code: shrink one panel's `size:` (down to 12 pt), widen its column,
  or split the slide. `code-panel(size:)` works; a surrounding
  `#set text(…)` doesn't.
- Noto CJK names differ: "Noto Sans SC" on macOS and Windows, "Noto Sans
  CJK SC" on Linux. On Linux set `cjk-font:` or pass `--input fonts`.

## 9. PowerPoint: the sci-propos templates

`powerpoint/presentation-sans.potx` and `presentation-serif.potx` have one
master and 20 layouts: Title, Title with Image, Outline, Outline Map,
Section, Section with Image, Title and Content, Two Content, Picture with
Call-outs, Code and Output, Content with Aside, Step, Title Only,
Conclusion, Contact, Closing, Thank You, Focus, Black and Blank. Their style
guide is a short AE talk with notes on every slide.

**Using them.** Start a deck from the template, then:
- open with the Title layout (centered) or Title with Image;
- replace the `[Logo]` box once in View › Slide Master;
- type the short title and the lead into their placeholders; the lead
  doesn't shrink, so a third line means a shorter sentence;
- set the footer locator with Insert › Header & Footer › Footer ("2
  Results ·"), per part;
- copy outlines, call-outs, cards and the three-line table from the style
  guide; edit native charts by double-clicking them;
- leave empty placeholders alone: they don't show in the slide show;
- make QR codes elsewhere and insert them as pictures.

Fonts come with Office (Calibri or Cambria, Consolas, DengXian or SimSun).
For Traditional Chinese, change the theme fonts or rebuild with
`SP_CJK=Hant`. If a company template is required, keep its look and apply
the writing rules (§5) inside it.

**Building decks in code.** Two ways:
- python-pptx: open the `.potx`, switch its content type back to a
  presentation, delete the style-guide slides, add slides by layout name;
- pptxgenjs with `source/style.js` (layouts and helpers such as `addTitle`,
  `addCallout`, `addCard`, `addCode`, `outlineRuns`), then
  `python3 finish.py raw.pptx deck.pptx --theme theme.json --total`.

`powerpoint/README.md` gives the rebuild commands and `finish.py` options.
pptxgenjs quirks to remember: put `fontSize` on runs, not on placeholder
options; put paragraph options and `align` on every run of a multi-run
paragraph; text `margin` is in points, `rectRadius` in inches.

python-pptx doesn't copy a layout's footer and slide-number placeholders
into new slides: copy them from the layout yourself, or the deck has no
numbers and no locator.

**Porting a Typst deck.** Keep text, lists, outlines and simple tables
native. Insert rendered outputs (formulae, plots) as pictures cropped from
the Typst PDF at 300 dpi. Copy the notes. List the deviations: Office fonts,
pictures instead of native objects, a static total.

## 10. LaTeX Beamer (guidance)

- `\documentclass[aspectratio=169]{beamer}`, the default theme, no
  navigation symbols, the primary color via `\setbeamercolor`, Noto through
  `fontspec` (XeLaTeX or LuaLaTeX).
- Short frame title, then the lead as the first line of the frame body at
  22/20 of the body size, in medium weight.
- Section pages: `\AtBeginSection{\begin{frame}\tableofcontents[currentsection]\end{frame}}`.
- Title frame `[plain]`; footer `\insertframenumber/\inserttotalframenumber`;
  backup slides after `\appendix` with `appendixnumberbeamer`.
- `minted` for code (`[fragile]` frames), `booktabs` for tables,
  `tcolorbox` with `borderline west` for cards, `\note{…}` for notes, and
  `\setbeameroption{show notes on second screen=bottom}` for a notes
  handout.

## 11. Workflow

1. **Brief** (§2), **storyboard** of lead sentences (§3), agreed with the
   presenter.
2. **Spec** when someone else builds the deck: one `DESIGN.md` with a
   version log, the global rules, and per slide its ID, title, lead,
   evidence, credit, script and time.
3. **Build** on the package or template.
4. **Render and look.** Render every page to PNG
   (`pdftoppm -r 40 -png deck.pdf tmp/p`) and view every image; contact
   sheets help for long decks. Never judge slides from source. For
   PowerPoint, convert a copy to PDF first
   (`soffice --headless --convert-to pdf --outdir tmp deck.pptx`). If you
   can't view images, ask the user to check the pages and say so.
5. **Review** against §12, with findings by slide ID (R1, R2, …), including
   what to keep. Repeat until accepted.
6. **Rehearsal pack:** the notes handout, plus the scripts with running
   times.

## 12. Acceptance checks

Build and fit:
1. The deck builds without errors or unexplained font warnings; the page
   count equals the planned slides plus pause steps.
2. Nothing overflows or is cut off; no slide has an empty bottom half.

Story (from Alley's critique sheet):
3. The lead sentences, read alone, tell the story.
4. The beginning sets the scope, shows the importance, gives the needed
   background, establishes credibility, and maps the talk.
5. Each content slide makes one point, stated in its lead; the evidence
   supports it and the script explains it; nothing extraneous.
6. Terms are defined at first use; key limitations are stated; sources are
   acknowledged on the slide (`[Author, year]`) and in full in the notes.
7. The ending summarises the assertions and gives closure; a research talk
   ends on its conclusion slide.

Slides:
8. Every lead is a full sentence, at most two lines, never shrunk; titles
   are short labels.
9. Bullets appear only where no stronger form fits (§5.3): at most four
   items, two lines each.
10. Projected words stay near 20 per minute of talk; other text blocks are
    at most two lines.
11. At most two columns, top-aligned; captions attached; legends clear of
    data; tables three-line; code highlighted and unwrapped; the shown code
    produced the shown output.
12. Sizes meet §7; no slide inherits another slide's `#set`.
13. Section pages open every section with the right highlighting; the
    title is counted with its number hidden; backup slides don't raise the
    total.

Script and facts:
14. Every slide has a script, a time and key phrases; the first and last
    sentences are marked; the total fits the time.
15. Facts, versions and data are checked; illustrative data is labeled;
    private details stay placeholders.

## Related skills

When they are installed: `scientific-poster` turns the same work into a
poster (its figures usually need enlarging), and `scientific-writing`
handles the abstract, the paper and the emails around the talk.

## Sources

The rules are paraphrased; no text, slides or templates of the sources are
included.
- M. Alley, *The Craft of Scientific Presentations*, 2nd ed., Springer,
  2013 (chapter 4 and the critique sheet).
- assertion-evidence.com: principles, model talks, PhD defence tutorial,
  handouts.
- craftscicom.org: AE tutorial, research talk tutorial, AE templates page.
- J. Garner, M. Alley, K. Wolfe and S. Sawarynski, *International Journal
  of Engineering Education* 29(6), 2013 (comprehension study).
- MIT CEE Communication Lab (CommKit), "Using miniature figures to enhance
  your conclusion slides".
