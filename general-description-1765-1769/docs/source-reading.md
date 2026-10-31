# Working with the edition

How the published edition was read, located and quoted. The rules on witnesses are in [`coding-manual.md`](coding-manual.md), section 1.

## 1. Read the whole entry, not the page

- Read the entry with its continuation, the route and day headings that precede it, signatures, marginal notes and the editor's notes. A page break is not the end of an entry.
- Check numbers, signs, superscript text and the place a note refers to on the **page image**. A text layer does not guarantee any of these.
- Distinguish the editor's correction of the edition from a correction in the historical document. A printer's or editor's grammatical gloss is not a new historical layer.

## 2. Locators

Every evidence record carries:

| Locator | Meaning |
|---|---|
| Printed page | The page number printed in the edition |
| PDF page | The page of the supplied PDF file |
| Side of the spread | Left or right half, where a PDF page holds a two-page spread |
| Folio | The manuscript folio as given by the edition, `v` for verso |
| Segment | Number of the passage within an entry, where the text is split |

Rules:

- Printed and PDF page numbers are mapped for each place, not by one offset for the whole volume. Illustration pages and changing spreads break any fixed offset. *(B31)*
- A folio marker `[N]` standing directly before the number of the next entry, or after `//` before a route or day heading, belongs to the **next** entry. The last folio of an entry is the last marker inside its text. *(B31a)*
- Rotated spreads keep their original orientation when page regions are cut from them; forcing the rotation to zero gave wrong crops.
- A file name or a frame number is not a folio.

## 3. Evidence texts

- **The full text of each entry is stored** in `EVIDENCE` with its locator, together with fragments where needed (heading, marginal note, crossed-out text, editor's note). Long texts are segmented, never truncated (an Excel cell holds at most 32,767 characters).
- **Evidence texts are not normalised.** They are taken from the PDF text layer with the edition's original letters (ѣ, ъ, ѳ, ѯ, ї). Numbers, names, ages and health formulas are checked against the 200 dpi image. *(B17)*
- **Font substitutions in the text layer.** The Poltava PDF uses a Church Slavonic display font whose glyphs map to wrong characters in the text layer. They were decoded consistently (є → е, ꙋ → у, ꙗ → я, ѡ → о) and every substitution was checked against the image; residual garbled glyphs were corrected by image and logged. *(B17, B42, B44)*
- **Uncertain readings** from the image are marked with square brackets and get the transcription status `partly_unclear`; nothing is silently completed.
- **Working `*_raw` fields** may hold a readable extract or a summary. They are not a diplomatic transcription. The exact text and its locator stay in `EVIDENCE`. *(F04)*
- **Check the text layer before trusting full-text search.** Compare a few characteristic passages with the image. A search that finds nothing in the text layer does not show that a number or name is absent; check the pages.

## 4. Reading conventions

These are the conventions of the Poltava run. Another edition may need its own list.

- **ѣ** is read in context as і, є, ї or я; it is not a fixed letter.
- A Cyrillic **ю** that looks like the glyph of **у** is read as у (*кюпца* → *купца*).
- **Superscript words** in the same hand are part of the line they belong to, with a note (*FOR 122*: an age written partly above the line). *(B11)*
- **Day headings** give dates only after the sequence of headings has been checked. *(B37)*
- **The editor's notes** ("other hand", "darker ink", "evidently a slip", "no number and no signature") are evidence for separating layers and for review, never text of the document.

## 5. Splitting the text into entries

Entry boundaries come from the numbers in the text, checked on the page. Automatic splitting fails on joined numbers: in Poltava a heading `186.Дворъ` with no space merged two entries in an early text cache. A missing key in a cache is not a missing entry in the source.
