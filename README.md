# Study Suite

A no-install, no-account, no-server toolkit for turning your messy lecture notes into a Jeopardy-style group study session.

Three single-file HTML tools that work together:

1. **Photo to Text** — point your phone at slides, notes, or whiteboards; get text back.
2. **Deck Builder** — visually edit and polish flashcard decks. No JSON wrangling.
3. **Study Game** — host a Jeopardy-style study session with up to 5 players over Zoom or in person.

The whole thing runs in your browser. Nothing gets uploaded anywhere except (optionally, if you use the OCR features) the AI provider you configured with your own API key.

---

## Why this exists

The standard study-tool problem: you have material as photos of slides, scrawled lecture notes, a textbook chapter, or a mess of files. You want to study it with your friends in a way that's not deathly boring. Existing options:

- **Quizlet/Anki** — great, but require accounts, subscriptions, and you have to type out everything yourself.
- **AI chatbots** — can generate flashcards, but the output isn't playable in any way; you'd still need a tool to actually use them.
- **PowerPoint Jeopardy templates** — fine for one session, painful to maintain or reuse.

This suite addresses that gap with a deliberately simple architecture. Each tool does one thing. They share a common deck format. There's no backend, no auth, no telemetry. You can email someone the three HTML files and they're set up.

---

## Quick start

1. Download the three HTML files (or clone this repo).
2. Double-click `study_game.html` to open it in your browser.
3. Click **Try the sample → Download Sample Deck**.
4. Click **Load my deck** and pick the file you just downloaded.
5. Click **Start Game →**.

That's the game working with the bundled sample deck. Once you have a feel for it, look at the other tools to start building your own decks.

---

## The three tools

### `study_game.html` — The Game

A Jeopardy-style flashcard board. You load a deck (a JSON file), select which categories to play, set up players, and click cells to flip cards. The host awards points; the score panel updates live.

Designed for hosting over Zoom screen-share — there's a separate-window Host Cheat Sheet that the host can keep on a second monitor without sharing it. Has three difficulty modes (title-only / half-bullets-revealed / fill-in-the-blanks), missed-card recycling that brings tough cards back in later rounds, undo for misclicks, keyboard shortcuts for fast hosting, and auto-save that survives an accidental browser refresh.

→ **Full manual:** [docs/study_game.md](docs/study_game.md)

### `deck_builder.html` — The Editor

A visual editor for the JSON deck format. Three columns: categories, cards, card editor. Click `+` to add anything. Bullets are little editable chips — type, press Enter for a new bullet below, paste multi-line content and it offers to split into separate bullets.

Especially useful for cleaning up AI-generated decks (which are usually 80% right and 20% wrong). It's deliberately forgiving on load — malformed input gets coerced into editable form rather than rejected, so you can fix issues visually instead of fighting JSON syntax errors. Has single-level undo for destructive actions and auto-save like the game.

→ **Full manual:** [docs/deck_builder.md](docs/deck_builder.md)

### `photo_to_text.html` — The OCR Companion

Drop in photos of slides, handwritten notes, or whiteboards; get text back via your choice of AI vision provider (OpenAI, Anthropic, Google Gemini, or any OpenAI-compatible endpoint). Edit the extracted text inline. After processing, click **Convert to Game Deck** to turn the bundled text into a deck JSON file via the same AI.

Bring-your-own-API-key — no account on this end, no subscription. Costs are typically a few cents per image. Your API key is never saved to disk.

→ **Full manual:** [docs/photo_to_text.md](docs/photo_to_text.md)

---

## The pipeline

The intended end-to-end workflow:

```
phone photos of slides/notes
        ↓
   photo_to_text.html      (extract text + generate rough deck)
        ↓
   deck_builder.html       (polish categories, fix bullets, refine titles)
        ↓
   study_game.html         (run the study session)
```

You don't have to use all three. If you've already got a deck, skip the first two. If you'd rather hand-write the JSON in a text editor, skip the builder. If you just want to extract text from photos with no game involved, the OCR tool works standalone too.

---

## File layout

```
study-suite/
├── README.md                     ← you are here
├── LICENSE
├── study_game.html               ← the game (open this to play)
├── deck_builder.html             ← visual deck editor
├── photo_to_text.html            ← OCR + AI deck generation
├── docs/
│   ├── study_game.md             ← full game manual
│   ├── deck_builder.md           ← builder manual
│   └── photo_to_text.md          ← OCR tool manual
└── samples/
    └── improv_theatre_deck.json  ← example deck (improv theatre course notes)
```

---

## How to use

### Just want to play with the sample deck

Open `study_game.html`. Click **Load my deck**. Pick `samples/improv_theatre_deck.json`. Click **Start Game →**.

### Have notes/slides; want a study session ASAP

1. Open `photo_to_text.html`. Add your API key (see the manual for where to get one).
2. Drop in your photos. Click **Process batch**.
3. Edit any errors in the extracted text.
4. Click **Convert to Game Deck**. A `.json` file downloads.
5. Open `study_game.html`. Click **Load my deck**. Pick the file. Play.

### Want to build a deck from scratch

1. Open `deck_builder.html`. Click **+ New Deck**.
2. Edit the example category and cards. Use **+ Add Category** and **+ Add Card** to build out.
3. Each bullet is its own editable chip — Enter creates the next one below.
4. Click **Download Deck** when done. Load it in `study_game.html`.

### Have an AI-generated deck JSON; want to polish it

1. Open `deck_builder.html`. Click **Paste JSON…** (or **Load my deck**).
2. The builder will load it, auto-fix common AI mistakes, and tell you how many issues it patched.
3. Fix the remaining issues visually — empty category names, weird groupings, oversized bullets, etc.
4. Click **Download Deck**. Load it in the game.

---

## Deck format

All three tools speak the same JSON schema:

```json
{
  "schema_version": 1,
  "deck_name": "Biology 101 Midterm",
  "description": "Chapters 1 through 5",
  "categories": [
    {
      "name": "Cell Structure",
      "cards": [
        {
          "title": "Mitochondria",
          "details": [
            "Powerhouse of the cell",
            "Generates ATP",
            "Has its own DNA"
          ]
        }
      ]
    }
  ]
}
```

- **`title`** is what the player sees on the front of the card (the prompt).
- **`details`** is what's revealed on flip (the answer, as bullet points).
- **Categories** group related cards. The game lets you pick which categories to include in a session.

You can hand-write decks in a text editor if you want — the format is meant to be human-friendly. The Deck Builder is just a more comfortable way to do the same thing.

---

## Privacy and data

This project deliberately has no backend, no analytics, no telemetry. The HTML files don't phone home.

- **The Study Game and Deck Builder** are entirely client-side. No data leaves your browser. They use `localStorage` to auto-save your in-progress game/deck so a browser refresh doesn't wipe it.
- **The Photo to Text tool** sends your images and text *directly* from your browser to whichever AI provider you configure. Nothing is routed through any server I control. Your API key is held only in memory while the page is open — it's never written to disk or storage. Each provider's privacy policy applies to what they do with your inputs.

If you're working with sensitive material (medical records, confidential documents), use a local OpenAI-compatible endpoint (Ollama, LM Studio) instead of a hosted provider. The OCR tool supports that.

---

## What this is NOT

To set realistic expectations:

- **Not a replacement for Quizlet or Anki for solo flashcard study.** This is a *group* study tool. There's no spaced repetition, no learning history, no automatic answer-checking. The host decides who got the right answer.
- **Not multiplayer over the internet.** One host, one screen, players join via a video call.
- **Not a content provider.** You bring the material. The tools help you turn it into a deck and play with it.
- **Not finished software.** It's a personal project that became useful. Bugs may exist; pull requests welcome but no guarantees of upkeep.

---

## Browser requirements

Any reasonably modern browser: Chrome, Firefox, Safari, Edge from the last few years. Uses standard APIs (`fetch`, `localStorage`, `FileReader`, `Blob`). No external dependencies are loaded except Google Fonts in the Study Game (and even those degrade to system fonts gracefully if blocked).

Mobile browsers work but the tools are designed primarily for desktop/laptop use, especially the game (since the host is sharing screen).

---

## Built with

Plain HTML, CSS, and vanilla JavaScript. No frameworks, no build step, no dependencies. Each tool is a single file you can open directly. Total project size is around 200 KB.

This was deliberate — it means the tools work on locked-down school laptops, can be emailed as attachments, run from a USB drive, or hosted as static files anywhere. There's nothing to install or configure.

---

## License

MIT — see [LICENSE](LICENSE). Use it, modify it, share it, embed it in your own projects.

---

## Contributing

This is a personal project, but if you find a bug or have an idea, feel free to open an issue or PR. The codebase is intentionally simple — each tool is one file, no build pipeline, anyone with a browser and a text editor can hack on it.

A few things worth knowing if you want to extend it:

- The deck schema is versioned (`schema_version: 1`). If you add fields, bump the version and update the validator in all three tools (they each have an identical `validateDeck` function with a "keep in sync" comment).
- The Study Game's `DECK_GENERATION_PROMPT` is duplicated in `photo_to_text.html`. Same caveat — keep them in sync.
- Each tool's manual lives in `docs/`. Update the manual when you change behavior; the manuals are how new users figure out what the tools do.
