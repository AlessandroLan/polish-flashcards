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

## Card scheduling and game rules
- A card is a word in one direction (`pl|<word>` or `en|<word>`); Mix mode uses both directions.
- Rounds: the user picks a round size (10/20/30/50/All) on the start screen. Cards are drawn from a shuffle-bag cycle over all cards, so every card is drawn equally often; the cycle reshuffles forever.
- In a round: swipe right = got it (card leaves the round), swipe up = not yet (card returns 3–8 cards later), swipe left = undo the last action. Round ends when every card is "got". No card is ever removed permanently.
- Scoring: first-try accuracy (cards got without any "not yet"), stars at 40/70/90%, combo of consecutive first-try cards, personal best per round size, day streak, session counters (session resets after 30 min idle).
- Trouble words: each "not yet" adds 1 to a word's count, each first-try "got it" removes 1; the top 5 appear on the start screen.
- All state lives in `localStorage` key `fiszki.v2`; new vocabulary words are slotted into the unseen part of the cycle.
