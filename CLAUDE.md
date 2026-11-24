# Fiszki – Polish flashcards

Static web app (no build step) hosted on GitHub Pages. `index.html` loads `vocab.json` and shows flashcards in both directions (PL→EN, EN→PL, or mixed).

## Updating vocabulary from a new PDF
The user keeps a handwritten vocabulary in GoodNotes and periodically exports `Vocabulary.pdf` (two columns: Polish | English). Each new export contains all previous words plus new ones.

1. The PDF is handwriting with a poor OCR layer (diacritics lost) — render the pages to images and read them visually.
2. Append only entries whose Polish word (case-insensitive) is not already in `vocab.json`. Never duplicate. Keep the existing order; new entries go at the end.
3. Entry format: `{ "pl": "...", "en": "...", "pron": "...", "note": "optional usage note" }`. Restore correct Polish diacritics/spelling; mention any corrections to the user.
   - `pron` is required for every entry: an English-reader respelling, syllables joined by `-`, stressed syllable in CAPS (Polish stress is on the penultimate syllable), one-syllable words in lowercase. Conventions: sz/ś→sh, cz/ć→ch, rz/ż/ź→zh, w→v, ł→w, j→y, c→ts, ch→h, dz→dz, dzi/dź→j, y→ih, i→ee, u/ó→oo, e→eh (final), ę→en/eh, ą→on/om, final ń→ny, final voiced consonants devoice (mąż→monsh). Pronunciation hints the user writes in brackets in the PDF go into `pron`, not `note`.
4. If a Polish word reappears with a new meaning, merge the meanings into the existing entry instead of adding a duplicate.
5. Commit and push to `main`; GitHub Pages redeploys automatically.

## Card scheduling
Shuffle-bag: every card appears exactly once per round (equal probability), rounds repeat indefinitely. Progress lives in `localStorage`; new words are slotted into the unseen part of the current round.
