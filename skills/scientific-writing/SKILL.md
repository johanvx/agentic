---
name: scientific-writing
description: Draft, revise and proofread scientific and engineering documents in Michael Alley's style (precise, clear, connected sentences; informative summaries; organization readers expect) - emails, letters, memo and design reports, laboratory reports, research papers - in English, French and Chinese, with Typst (sci-propos-writing) or Word templates. Use also for grammar, punctuation and usage questions, AI-use statements, and reviews of a draft returned in its own file format.
compatibility: Typst 0.15 with the sci-propos-writing package for Typst documents; Word templates and python-docx (via uv) for .docx; poppler (pdftoppm) and LibreOffice to render and check pages. Emails need nothing.
---

# Scientific writing

This skill helps engineers and scientists write documents that readers can
use: emails, letters, memo and design reports, laboratory reports and
research papers. It follows Michael Alley's *The Craft of Scientific
Writing* (4th ed., Springer, 2018) and his website craftscicom.org. Every
writing decision there concerns **content**, **style** (how the content
reaches the reader) or **form** (grammar, punctuation, usage, format). We
assume the content belongs to the author; this skill works on style and
form, and on organization.

| Output | Where |
|---|---|
| Email, short message | In the conversation, plain text ready to paste (§6.1) |
| Letter, memo, report, paper | Typst with `sci-propos-writing` (§9.1); Word when a course or journal requires it (§9.2) |
| A review of someone's draft | In the draft's own format: Word comments, a marked Typst or LaTeX file, suggestions in Google Docs (§10) |

## 1. Find the tools first

The Typst package and the Word templates live in the **`sci-propos`**
repository; the package isn't on Typst Universe yet.

1. **Typst.** Compile a two-line file:
   `#import "@preview/sci-propos-writing:0.1.0": *` then `#show: report`.
   If the package is missing, look for a `sci-propos` checkout in the user's
   folders; its README says how to copy `core/` and `writing/` into Typst's
   local package folder (macOS `~/Library/Application Support/typst/packages/preview/<name>/0.1.0/`,
   Linux `~/.local/share/typst/packages/preview/<name>/0.1.0/`, Windows
   `%APPDATA%\typst\packages\preview\<name>\0.1.0\`). Ask before copying
   into system folders.
2. **Word.** `word/sci-propos-report.dotx` (A4) and
   `sci-propos-report-us-letter.dotx` in the checkout.
3. **Nothing found?** Ask for the repository, or build from the rules in
   this file and say so.

Work files go in the project's `tmp/` folder. For Python steps use `uv`
(`uv run --with python-docx python …`); if `uv` is missing, stop and ask.

## 2. Before writing: audience, purpose, occasion

Settle these first; ask only for what the request and files don't say.

- **Audience:** who reads, what they already know about the work, and how
  they will read (on a phone, skimming, forwarding, filing).
- **Purpose:** inform, request, persuade, instruct, record. How much
  persuasion does the reader expect?
- **Occasion:** length, formality, format, deadline, politics. A course or
  journal format wins over this skill's defaults.
- **Language:** English, French, Simplified or Traditional Chinese. The
  conventions of §6–§8 change with it.
- **AI policy:** ask whether the course, employer or journal has one, and
  follow it (§3).
- **Placeholders:** never invent names, numbers, sources or results. Mark
  what the author must supply as `[placeholder]`.

## 3. AI use and integrity

Alley's position, which this skill follows: AI helps most in revising and
in summarizing trusted documents, and least in drafting important technical
text, where it tends to lose precision.

- **The author does the thinking.** For important documents (a paper, a
  thesis chapter, a proposal), encourage the author to draft and use this
  skill to revise. Drafting is where writers find out what they think.
- **Good uses:** revise a human draft toward a stated goal; summarize a
  trusted document; tighten to a page limit; polish a second-language
  draft; proofread. After each, check nothing essential was cut.
- **Precision check, every time.** Compare the output with the source,
  above all the **first sentence (scope)** and the **last sentence (next
  steps)**, where AI summaries most often go wrong.
- **Never invent a citation, a number or a result.** Verify every
  reference against the source before it goes into a document.
- **Accountability and disclosure** (Nature Portfolio's policy): the
  authors stay responsible for everything; AI is never an author; language
  polishing is fine, drafting summaries needs oversight and disclosure,
  fabricated data or citations and undisclosed AI reasoning are not
  allowed. Offer a short statement when disclosure is expected, for
  example: "We used [tool] to check grammar and to shorten Section 3. The
  authors wrote and verified all content."

## 4. How this skill writes (and checks its own text)

Alley's group measured AI summaries against human ones: AI sentences were
longer (22 against 20 words), their words longer (6.1 against 4.9
letters), and 85 % of them opened with the subject. So everything this
skill writes, and every revision it proposes, meets these targets:

1. **Mean sentence length under 20 words**, with real variety: some of 10
   words, some of 25.
2. **Short, plain words.** Prefer verbs to abstract nouns: "identify", not
   "identification"; "portable", not "portability".
3. **Vary the openers.** At most about half the sentences start with the
   subject. Use the openers of §5.5, choosing the one that states the real
   link: cause, contrast, purpose, time.
4. **Few participial phrases**, at the start or the end of a sentence.
   Avoid trailing ", enabling …", ", ensuring …", ", highlighting …".
5. **"We"** where the writers did the work.
6. **Precise scope:** say what the document did, not what the project will
   do.

Measure, don't guess, when the text matters. A quick check (paste the text
into `tmp/text.txt`):

```sh
uv run python - <<'EOF'
import re, statistics as st
t = open("tmp/text.txt", encoding="utf-8").read()
s = [x for x in re.split(r"(?<=[.!?])\s+", t) if len(x.split()) > 2]
w = re.findall(r"[A-Za-z]+", t)
print("sentences", len(s), "mean words", round(st.mean(len(x.split()) for x in s), 1))
print("letters per word", round(st.mean(len(x) for x in w), 2))
subj = sum(bool(re.match(r"(The|This|These|A|An|It|We|Our|[A-Z][a-z]+ (is|are|was|were))\b", x)) for x in s)
print("share opening with a subject (rough)", round(subj / len(s), 2))
EOF
```

## 5. Style

### 5.1 Precision and clarity

- Choose the exact word; give the number and its unit; keep terms
  consistent (one thing, one name).
- Avoid needless complexity: unexplained abbreviations, long noun stacks,
  words such as "utilize" for "use".

### 5.2 Ambiguity

- Never let **"this"** stand alone: write "this result", "this method".
- Place **"only"** next to what it limits.
- Use **"it"** sparingly, and only when the antecedent is clear.
- Put a **comma after an introductory phrase or clause**.
- Complete every comparison: "higher than with chlorine".

### 5.3 Energy

- Put the main verb early; prefer active voice and first person where
  they help ("We measured…").
- Concrete nouns over abstract ones.
- Cut needless words: "in order to" → "to"; "due to the fact that" →
  "because". Challenge every *-ability*, *-ize*, *-ization*.
- Don't call anything "obvious" or "clearly".

### 5.4 Begin with the familiar

Each first sentence orients the reader and also takes a step forward.
Define unavoidable unfamiliar terms. Map long sections. Anchor
illustrations in something the reader knows.

### 5.5 Connecting ideas

Begin each sentence so it links to the one before. Sentence openers that
make connections (Alley's handout):

| Kind | Openers |
|---|---|
| Basic | subject; prepositional phrase ("In the second test, …"); adverb ("However, …"); dependent clause ("Because the flow separated, …"); infinitive phrase ("To reduce the loss, …") |
| Sophisticated | gerund phrase ("Measuring the loss directly …"); participial phrase, sparingly; correlative pair ("Not only …, but also …"); clause as subject ("What we found was …") |
| Occasional | double clause ("The higher the speed, the larger the loss"); a question; an appositive; an adjective phrase |

Transitions by function: **continuation** (also, in addition, therefore, as
a result, consequently); **pause** (in other words, for example, in
particular, likewise); **reversal** (however, in contrast, conversely, that
said, granted, alternatively).

### 5.6 Organization

- **Title:** specifies the scope: the field first, then what separates the
  work from others. Keep the small words that show relations. Name special
  document types ("progress report"). A sentence title suits some fields
  when the work has one takeaway.
- **Introduction:** what the work is (scope and limitations), why it
  matters (show it: for curiosity-driven work, raise the question), the
  background needed (in research, a literature review that credits others
  and shows the gap), and a map of the document.
- **Middle:** logical and persuasive; stages in groups of two to four;
  parallel headings; never a lone subsection; paragraphs mostly 7–11
  lines, none over 15; each paragraph opens with its point.
- **Conclusion:** summarises the middle and looks ahead; no new evidence.
  It reaches a plateau, not a peak.

### 5.7 Emphasis and summaries

- Emphasize by wording, repetition (an important result up to three
  times: summary, middle, conclusion) and placement (first and last
  positions).
- An **informative summary** gives the results with numbers: what was
  done, how, what was found, what it means. A **descriptive summary** only
  lists contents; avoid it in reports and papers. A summary stands on its
  own; write it last.
- Move long secondary material to **appendices**, each with a beginning,
  middle and end, each mentioned in the text.

### 5.8 Illustrations and equations

- Refer to every figure and table in the text before it appears, and say
  what to notice.
- Captions: an identifying phrase, then one or two sentences on the key
  details. Figure captions below, table titles above.
- Equations belong to the sentence: punctuate around them and define
  every symbol after them ("where *h* is …").

## 6. Document types

### 6.1 Emails

First ask whether email is the right medium. Then:

- **Subject line:** the topic and its perspective, from the reader's side
  ("Request: two days of wind-tunnel time in March for the cylinder
  tests"), not a one-word label.
- **Beginning:** the purpose in the first paragraph, ideally the first
  sentence ("Please consider me for …", "Could you …").
- **Middle:** one paragraph per supporting point; facts the reader needs.
- **Ending:** the next step: what you want the reader to do, or what you
  will do ("With your approval, I will book the tunnel on Monday.").
- **Length and form:** fits one screen; sentences averaging under 20
  words; paragraphs under 7 lines; single spacing, a blank line between
  paragraphs, no indents. Copy (cc) anyone named or directly affected.
- **Tone:** balance *I* and *you*; respect titles (Dr., Professor); never
  "Hey"; watch exclamation marks.

Conventions by language:

| | Salutation | Closing | Notes |
|---|---|---|---|
| English (US) | "Dear Dr. Lee:" (business), "Dear Dr. Lee," (less formal) | "Sincerely," / "Best regards," | Colon after a formal salutation |
| French | « Madame, » / « Monsieur, » / « Madame, Monsieur, »; « Bonjour Madame Martin, » for routine emails | « Cordialement, » (email); formal letters end with a formule de politesse | Space before : ; ? ! |
| Simplified Chinese | 尊敬的王教授：/ 王老师：+ 您好！ | 此致 / 敬礼！, or 祝好！; then name and date | Full-width punctuation；正文首行可不缩进 in email |
| Traditional Chinese | 王教授您好：/ 王教授鈞鑑：(formal) | 敬祝 / 研安 (to a professor), 順頌 / 時祺; 學生 王小明 敬上 | Full-width punctuation |

Deliver an email as plain text in the reply: Subject line, then the body.

### 6.2 Letters

Formal letters (cover letters, recommendations, formal requests). Get to
the point in the first sentence; one page if possible; sentences under 20
words on average; paragraphs under 7 lines; the last paragraph says what
you want the reader to do. Build them with `letter` (§9.1), which sets each
language's layout:

- **English**, modified block style: your address and the date on the
  right; the inside address on the left; "Dear …:"; indented paragraphs;
  "Sincerely," and your name aligned with the date; "Enclosure:", "cc:".
- **French:** expéditeur en haut à gauche, destinataire à droite, « Ville,
  le 7 novembre 2026 », « Objet : », « P. J. : », civilité, formule de
  politesse reprenant la civilité (« Je vous prie d'agréer, Madame,
  l'expression de mes salutations distinguées. »), signature à droite.
- **Chinese:** 称呼顶格；问候语「您好！」另起一行；正文首行缩进两字；结尾
  「此致」缩进两字、「敬礼！」另起一行顶格（繁體常用「敬祝」「研安」）；署名
  和日期在右下；附件在最后。

### 6.3 Memo reports and design reports

A memo heading (Date, To, From, Subject), then sections. The subject states
the document type and scope. Alley's model design report shows the moves:

1. **Introduction:** the problem and why it matters (with a sourced
   number); the client and the assignment; the customer defined precisely;
   a map of the report.
2. **Customer needs:** where they came from (interviews, site visits, the
   project description); primary and secondary needs; one paragraph per
   primary need, opening with a sentence that names it.
3. **Metrics and specifications:** a table linking each need to a metric,
   a target value, the source of the target, and the test with its pass
   criterion.
4. **Conclusion:** the needs restated, then the next phase.
5. **Appendices** (schedule, management plan) and numbered references.

Design reports at later stages follow the design process: customer needs,
concept generation, concept selection (show the reasoning step by step,
with a selection matrix), prototypes and tests, final design.

### 6.4 Laboratory reports

Follow the instructor's order if it differs. Usually:

1. **Title and summary (abstract):** an informative synopsis, about 200
   words, with numbers: set-up, method, findings, their meaning.
2. **Introduction:** the experiment, its objectives (the conclusions check
   them), why it matters, the background (often the theory).
3. **Procedures:** a first-person narrative of what was done, enough to
   repeat it. For standard procedures, only what could change the result;
   for new ones, more detail and the reason for each step.
4. **Results and discussion:** the heart of the report. Analyse; give
   implications; discuss errors (sources, size, effect on the conclusions);
   graphs for numbers. Combine results and discussion when one result is
   needed to understand the next.
5. **Conclusions:** each objective, and whether it was met; if not, why.
6. **Appendices:** full data and calibrations, each introduced.

### 6.5 Research papers

Whitesides' process, which Alley recommends:

- **Write from outlines** of data: figures with captions, tables,
  equations, section headings, little text. Iterate the outline with the
  co-authors before drafting prose.
- **Order by importance**, not chronology.
- **Introduction:** the first sentence states the objective and why it
  matters; then the background (who did what), the gap, the approach, and
  the expected conclusion.
- **Results and discussion**, usually combined, organized by topic, with
  **specific, information-rich subheadings, often full sentences** ("The
  rate of self-exchange decreases with the polarity of the solvent"), the
  headline of the assertion-evidence approach applied to a paper.
- **Conclusions** interpret at a higher level; they don't just summarize.
- **Write the abstract last.** Past tense for what was done, present tense
  for what remains true.
- Mind the journal: its format, word limits, reference style and AI policy
  override these defaults.

### 6.6 Instructions and proposals

Instructions rely on numbered steps, one action per step, with the result
the user should see. A proposal is an argument for how to solve a problem:
the problem and its importance, the approach and why it will work, the
plan, the team, the cost.

## 7. Form: grammar, punctuation, usage

Use these names when flagging an error, so the author can look the rule up.

### 7.1 Grammar

- A **sentence** has a subject and a finite verb and makes a complete
  thought. A **fragment** lacks one of them.
- A **run-on** joins two independent clauses wrongly: a **comma splice**
  ("…, it is used …") or no punctuation at all. Four fixes: a period; a
  semicolon; a comma and a coordinating conjunction (and, but, or, nor, for,
  so, yet); or a subordinate clause ("Because …, …"), the most
  sophisticated. Adverbs such as *however* and *therefore* can't join
  clauses with a comma.
- **Misplaced or dangling modifiers**, often participial phrases at the
  start: "Using a wind tunnel, the number was measured" (the number didn't
  use it).
- **Parallelism:** items in a series, and in correlative pairs (both …
  and, not only … but also, either … or), share one grammatical form.
- Severity: errors that unsettle every reader (run-ons, fragments,
  dangling modifiers) come first; "rules" that only some readers enforce
  (split infinitives, a final preposition) last.

### 7.2 Punctuation

- **Period:** the strongest stop; challenge strings of short, stilted
  sentences.
- **Comma:** after introductory phrases and clauses; around parenthetical
  elements (both commas); before contrasting elements; the serial comma in
  series of three or more (US technical style; British and French usage
  often omit it: follow the document's style).
- **Colon:** the text before it must stand alone as a sentence; it
  introduces a list, a definition, an equation.
- **Semicolon:** joins closely linked independent clauses; separates items
  of a list that contain commas.
- **Dash:** stronger than a comma, sets off a phrase, often at the end;
  overuse breaks the flow.
- **Hyphen:** compound modifiers before a noun ("low-cost sensor", but
  "the sensor is low cost").
- **Quotation marks:** US style puts periods and commas inside. French uses
  « » with spaces inside; Chinese uses “ ” and 「 」 (Traditional).
- **French spacing:** a space before : ; ? ! and inside « » (narrow
  no-break spaces where the tool allows). **Chinese:** full-width
  punctuation (，。：；？！), no spaces around it.

### 7.3 Usage

- **Confused pairs:** affect/effect, amount/number, compose/comprise
  (the whole comprises the parts), continual/continuous, farther/further,
  fewer/less (fewer for countable things), its/it's, lead/led,
  principal/principle, that/which (restrictive *that*; *which* after a
  comma).
- **Misused words:** *hopefully* doesn't mean "it is hoped"; *plethora*
  means too many; *simplistic* means too simple.
- **Nonsense expressions:** center around → center on; irregardless →
  regardless; is comprised of → comprises or is composed of; peaked my
  interest → piqued.
- **Tense:** past for what was done ("we measured"), present for what
  remains true ("the correlation overpredicts").
- **Numbers:** numerals with units ("5 mm"), for values of 10 and more, and
  in series of values; words for small counts in running text ("three
  runs"); never start a sentence with a numeral. *Data* is plural.
- **Agreement and possessives:** the verb agrees with the subject, not the
  nearest noun.

### 7.4 Citing sources

- Cite others' ideas, measurements, computations, wording and images, and
  any controversial assertion. Common knowledge (widely known constants,
  dates) needs none; a specific measurement does.
- **Paraphrase** more than you quote. Quote only when the wording is
  unusual and nothing is more precise, when the speaker's authority
  matters, or when the exact words matter (law). Long quotations (three
  lines or more) are indented blocks without quotation marks.
- Alley's default style is **author–year in brackets**: "[Perry and
  Siebers, 1986]", or "Perry and Siebers [1986]" when the names are part of
  the sentence; three or more authors as "first author and others";
  references alphabetical at the end. Use the course's or journal's style
  instead when it has one (IEEE numbered is common in engineering; GB/T
  7714 in Chinese).

## 8. Format

- **Type:** serif body text for long documents (Noto Serif, or Cambria in
  Word), bold sans-serif titles, headings and labels (Noto Sans, Calibri);
  sans body text is fine for emails and on request. 11 pt Noto Serif (12 pt
  Times) single column; no all capitals; italics for terms and foreign
  words.
- **Headings:** in English, initial capitals (capitalize the first and last
  words and all others except articles, conjunctions and short
  prepositions); in French and Chinese, sentence style. Short reports may
  use the title as the only major heading; formal reports name sections.
- **Paragraphs:** English and French in block style with space between;
  Chinese with a two-character first-line indent.
- **Illustrations:** placed after the paragraph that first mentions them;
  figure captions below, table titles above; "Figure 1", "Table A-1" in
  appendices.
- **Abbreviations:** plurals and decades without apostrophes (CDs, 1970s).

## 9. Building documents

### 9.1 Typst: `sci-propos-writing`

```typst
#import "@preview/sci-propos-writing:0.1.0": *
#show: report.with(
  kind: [Laboratory report],
  title: [Heat Transfer from a Heated Cylinder in Cross-Flow],
  authors: ([#ph[Author one]], [#ph[Author two]]),
  affiliation: [#ph[Course] · #ph[University]],
  summary: [What was done, how, what was found (with numbers), and what it means.],
  lang: "en", style: "serif", paper: "a4",
)
= Introduction
...
#show: appendix
= Raw Data
...
#bibliography("refs.bib", style: "chicago-author-date")
```

- **`report`**: `kind`, `title`, `subtitle`, `authors`, `affiliation`,
  `date`, `summary`, `summary-title`, `keywords`, `toc`, `title-page`,
  `numbered`.
- **`memo`**: `to`, `from`, `cc`, `date`, `subject`, `kind`, `note`
  (footer), `numbered` (off).
- **`letter`**: `sender`, `recipient`, `date`, `place` (French), `subject`,
  `salutation`, `closing` (`auto` per language; two lines in Chinese),
  `signature`, `signature-image`, `enclosures`, `cc`.
- **`paper`**: `title`, `authors` (with `inst(1)`), `affiliations`,
  `correspondence`, `date`, `abstract`, `keywords`.
- **Common options:** `lang` (`en`, `fr`, `zh-Hans`, `zh-Hant`), `style`
  (`serif` default, `sans`), `paper` (`a4`, `us-letter`), `size` (11 pt),
  `justify` (English ragged, French and Chinese justified), `primary` (the
  accent color of placeholders and links; green by default), font
  overrides.
- **`#show: appendix`** starts appendices: each on a new page as "Appendix
  A: Title", figures A-1. Figures and tables are referenced with labels
  (`@fig:nu` gives "Figure 1"); tables get the three-line style.
- `typst init @preview/sci-propos-writing:0.1.0 my-report` starts from a
  report template whose comments list what each section needs. The
  package's `gallery/` has a letter, memo report, lab report and paper in
  each language.
- Fonts: pass `--input fonts="$(typst fonts | paste -sd, -)"` to avoid
  warnings; Noto fonts must be installed.

### 9.2 Word

Start from `sci-propos-report.dotx`, or build with python-docx (through
`uv`) on a copy of it, using its styles: Normal, Title, Subtitle, Document
Kind, Byline, Summary Heading, Heading 1–3, Caption with Caption Label,
Three-Line Table, Bibliography, Code, Placeholder. Type heading numbers;
keep captions below figures and above tables. If a docx skill is
available, it can help with tracked changes and complex layouts.

### 9.3 Other formats

LaTeX, Markdown or Google Docs: apply the same rules with the document's
own tools. Edit a file the user gave in its own format; don't convert it.

## 10. Proofreading and reviewing

1. **Ask what kind of review** if unclear: a light proofread, a style
   edit, or a full review. Respect the author's voice; never rewrite
   silently.
2. **Read in passes:** content and organization (title, summary,
   introduction, headings, conclusion, illustrations), then style (§5),
   then form in the order grammar → punctuation → usage (§7), then format
   (§8). Report the most serious problems first.
3. **Name the rule in every comment**, in brackets, then one sentence of
   explanation and a suggested fix:
   "Run-on (comma splice) [grammar]: two independent clauses joined by a
   comma. Suggest: '…applications: engineers use it to design heat
   exchangers.'"
4. **Return the review in the file's own format:**
   - **.docx:** Word comments anchored to the words concerned
     (python-docx 1.2: split the problem into its own run, then
     `document.add_comment(run, text=…, author="Reviewer", initials="R")`);
     tracked changes only when asked. Keep the text otherwise unchanged.
   - **Typst, LaTeX, Markdown:** a copy with comments at the lines
     concerned (`// REVIEW: …`, `% REVIEW: …`, `<!-- REVIEW: … -->`), plus a
     short list of findings by line in the reply.
   - **Google Docs:** suggestions or comments through the connected app,
     if there is one.
   - **Pasted text:** a list of findings in the reply, then a corrected
     version only if asked.
5. **Check your own suggestions** against §4: they must be shorter and
   plainer, not longer.
6. **Summarize** the review in a few lines: the main patterns (for
   example, "subject openers in 80 % of sentences; three comma splices")
   and what to fix first.

The repository's `examples/proofreading/` shows a laboratory-report draft
returned with twelve such comments.

## 11. Workflow and checks

1. Settle audience, purpose and occasion (§2), and the format.
2. Outline before prose for anything longer than a page; for papers, an
   outline of the data (§6.5).
3. Draft, or revise the author's draft; write summaries and abstracts last.
4. Build; render every page to PNG (`pdftoppm -r 60 -png doc.pdf tmp/p`;
   for Word, convert a copy with `soffice --headless --convert-to pdf`) and
   look at each one. If you can't view images, ask the user to check.
5. Check, before delivering:
   - the title specifies the scope; the summary is informative, with
     numbers, and stands alone;
   - the first sentence of each section orients and takes a step; the
     conclusion adds no new evidence;
   - every figure and table is mentioned before it appears, with captions
     in the right place;
   - every number has a unit and a source; every citation is verified; no
     invented content; placeholders are marked;
   - sentences average under 20 words; openers vary; no standalone "this";
     no comma splices or dangling modifiers;
   - the format and language conventions of §6–§8, or the course's or
     journal's own;
   - an AI-use statement where one is expected.

## Related skills

When they are installed: `scientific-presentation` for talks and
`scientific-poster` for posters on the same work; both follow the same
assertion-evidence rules and share the sci-propos style.

## Sources

The rules are paraphrased; no text or templates of the sources are
included.

- M. Alley, *The Craft of Scientific Writing*, 4th ed., Springer, 2018
  (Lessons 1–10, Appendices A–D).
- craftscicom.org: Writing; Emails and Letters; Professional Email Format;
  Professional Letter Format; Reports; Sample Report Format; Design Reports;
  Laboratory Reports; Research Papers; Grammar Refresher, Grammar,
  Punctuation, Usage, Grammar Glossary; AI Writing; Strong Examples of AI
  Writing; Writing Styles of AI; Writing Style of Claude; Writing Style of
  ChatGPT; Faculty Quotations: AI Writing.
- M. Alley, sentence-variety handout; model design report (Penn State
  ME 340); Penn State engineering writing workbooks (sample abstract,
  laboratory reports, documentation).
- G. M. Whitesides, "Whitesides' Group: Writing a Paper", *Advanced
  Materials* 16(15), 2004.
- Nature Portfolio, editorial policy on artificial intelligence.
