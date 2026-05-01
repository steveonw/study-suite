# Deck Builder — User Manual

A standalone HTML editor for creating and editing study decks for the Study Game. Build decks from scratch, edit AI-generated decks, or fix up existing JSON files — all without touching a text editor.

---

## Quick start (60 seconds)

1. Open `deck_builder.html` in any modern browser. Just double-click the file.
2. Click **+ New Deck**. The builder opens with a starter deck containing one example category and one example card.
3. Click on the example card — its title and bullets show on the right.
4. Edit the title. Click any bullet to edit it. Press Enter on a bullet to add a new one below.
5. Use **+ Add Category**, **+ Add Card**, and **+ Add Bullet** to build out your deck.
6. Click **Download Deck** when you're ready. Saves a `.json` file you can load into the Study Game.

That's the whole loop. The rest of this manual covers the details.

---

## What this is for

The Study Game uses a JSON deck file. There are three ways to make one: hand-write the JSON (technical), get an AI chatbot to generate it (fast but rough), or use this builder (visual and forgiving). You'll typically use this builder either to:

- **Build a deck from scratch** — for instance, when you have a textbook chapter and want to author flashcards while reading
- **Polish an AI-generated deck** — fix wonky categories, rewrite bullets, delete cards that don't work, move cards between categories
- **Edit an old deck** — pull up last semester's deck and update it for this year

The builder is intentionally separate from the game itself. You build decks here, you play with them in the game. They share the same JSON format.

---

## The interface

The builder has three columns when a deck is open:

**Left: Categories.** All the categories in the deck, listed top to bottom. The selected one is highlighted in blue. Each row shows the category name and the number of cards in it.

**Middle: Cards.** All the cards in the selected category. Click a card to open it in the editor on the right.

**Right: Card editor.** The currently-edited card. Title at the top, then bullet boxes below, then action buttons (move card to a different category, delete card).

On narrow screens (phones, narrow windows), the three columns stack into a single column.

### The top bar

- **Load Deck…** — open an existing `.json` deck file
- **Paste JSON…** — paste deck JSON directly (handy for AI chatbot output)
- **New Deck** — start fresh with a small starter deck. If you have unsaved work you'll be asked to confirm.
- **Download Deck** — save the current deck as a `.json` file

### The deck meta bar

Below the top bar:

- **Deck name** — shows on the game's setup screen and as part of the downloaded filename. Required.
- **Description** — one-line summary, optional. Shows on the game's setup screen.
- **Stats** — live count of categories, cards, and total bullets. Save status on the right.

A red highlight on the deck name means it's empty — you can keep editing, but the game won't accept the deck without one.

---

## Adding things — the `+` pattern

Three `+` buttons handle all creation, one per level:

- **+ Add Category** at the bottom of the left column. Adds an empty category and immediately puts it in rename mode so you can type the name.
- **+ Add Card** at the bottom of the middle column (only enabled when a category is selected). Adds an empty card in the current category and opens it for editing.
- **+ Add Bullet** at the bottom of the editor. Adds an empty bullet at the end of the current card; the cursor jumps into it so you can start typing right away.

You can also press **Enter** on any bullet to create a new bullet below it. Useful for typing out a list quickly.

---

## Editing categories

**Rename a category:**
- Double-click its name, OR click the ✎ pencil button on hover
- Type the new name; press Enter to save or Escape to cancel

**Reorder categories:**
- Use the ↑ / ↓ arrows on the right of each row (visible on hover)

**Delete a category:**
- Click the × button. You'll be asked to confirm — deleting a category also deletes all cards in it.

**Switching between categories:**
- Click any category name in the left column to switch to it. The middle column updates to show its cards.

---

## Editing cards

**Edit the title:**
- Just type in the title field. Changes save automatically.

**Edit a bullet:**
- Click the bullet text. The chip becomes editable.
- Type to change it.
- Press Enter to save and create a new bullet below.
- Press Backspace on an empty bullet to delete it.
- Click anywhere outside the bullet to save and stop editing.

**Reorder bullets:**
- Use the ↑ / ↓ arrows on each bullet.

**Delete a bullet:**
- Click the × button on the bullet, or backspace it empty.

**Reorder cards within a category:**
- Use the ↑ / ↓ arrows on each card row in the middle column.

**Move a card to a different category:**
- Click **Move to category…** at the bottom of the editor. A dropdown shows your other categories.
- Click the target category. The card jumps there and the dropdown closes.

**Delete a card:**
- Click **Delete card** at the bottom of the editor. You'll be asked to confirm.

---

## Pasting content

This is where a builder beats hand-writing JSON. Three useful patterns:

**Paste a whole deck.** Click **Paste JSON…** in the top bar, paste the JSON, click Load. Markdown code fences (```` ```json ````) are stripped automatically.

**Paste a multi-line block into a bullet.** If you copy several lines from a textbook or notes and paste them into a single bullet, the builder asks: "Split this into N separate bullets?" Click yes and each line becomes its own bullet. This is the fast way to convert prose notes into a study deck.

**Paste a single line into a bullet.** Just behaves like normal text. No prompt.

The split detection strips common bullet markers (`-`, `•`, `*`, `◆`) from the front of each line, so notes that already look like bullets in your source paste cleanly.

### Loading imperfect decks

The builder is forgiving when loading. If an AI-generated deck is missing the `schema_version` field, has a category with no name, or has a card with weird detail values, the builder loads it anyway and patches the issues into editable form so you can fix them visually. You'll see a toast like "Loaded 'My Deck' (5 issues auto-patched — review and edit)" telling you how many things were normalized. Common patches include:

- Adding the missing `schema_version: 1`
- Coercing non-string descriptions or details into strings
- Wrapping a single-string `details` field into an array
- Replacing null categories or cards with empty editable ones

Truly broken input — the file isn't valid JSON at all, or the JSON parses to something that isn't an object (a string, an array, a number) — gets rejected with an error toast. If the JSON is an object but is missing key fields (no `deck_name`, no `categories`), the builder fills in safe defaults (an empty deck with no categories) so you can start adding content visually.

This means you can take a rough AI output and start polishing immediately, instead of fighting JSON syntax errors.

---

## Undo

Built-in undo lets you reverse one destructive action — delete category, delete card, delete bullet, move card between categories, paste-split, or any reorder. Click the **↶ Undo** button in the top bar, or press **Ctrl+Z** (Cmd+Z on Mac) when you're not typing in a text field.

A few things worth knowing:

- **Single-level undo only.** Each new destructive action replaces the previous undo target. So undo can rescue you from one misclick at a time, but not a chain of them.
- **Text edits aren't on the undo stack.** Typing in titles, bullets, or the deck name doesn't create undo points — the browser's native text-input undo (also Ctrl+Z) handles those when the field is focused.
- **The undo button shows a tooltip** describing what will be undone (e.g. "Undo: delete card 'Mitochondria'") so you know what you're getting before you click.
- **Loading a deck clears undo.** You can't undo into a different deck.

If you're about to do something irreversible (like deleting a big category), the builder always asks for confirmation — that's the first line of defense. Undo is the safety net if you confirm by accident.

---

## Auto-save and resume

The builder saves your work to your browser's local storage as you go. If you close the tab, refresh, or your browser crashes, your work is preserved.

Saves are debounced — when you're typing fast, the actual write to storage happens about half a second after you stop typing, so the editor stays smooth even on large decks. Right before you click **Download Deck** or close the tab, any pending save flushes immediately so you never lose the most recent keystrokes.

When you reopen the builder with unsaved work, you'll see a **Resume editing?** prompt with the deck name, category count, card count, and save time. Click **Resume** to pick up exactly where you left off, or **Discard** to clear the save.

A few important notes:

- **The save lives on the device and browser you used.** If you switch computers, you start fresh.
- **The save is overwritten with each change** — there's no version history. Use Download Deck periodically to make `.json` snapshots if you want backups.
- **Private/incognito windows** save fine within the session but lose data when the window closes.
- **Auto-save can fail if storage is full or disabled.** A red "⚠ Auto-save failed" indicator appears in the deck stats bar. If you see it, download your deck immediately so you don't lose work.

The save status indicator in the deck stats bar shows:

- **✓ Auto-saved** — work is preserved
- **⚠ Auto-save failed** — something is wrong, download immediately

---

## Downloading the deck

Click **Download Deck** in the top bar. The builder runs validation first:

- **If the deck is fully valid**, it downloads immediately as `<deck_name>.json`. The filename is generated from the deck name (lowercased, spaces become underscores).
- **If there are validation issues**, you'll see a confirmation dialog listing them. You can choose to download anyway (sometimes useful — for instance, if you're handing the file to someone else who'll fix it) or cancel and fix the issues first.

Common validation issues the dialog will surface:

- Empty deck name
- A category with no name or no cards
- A card with no title
- A bullet that's somehow not text (rare, mostly happens with imported AI output)

Once downloaded, you load the file into the Study Game's "Load my deck" section (or paste it via "Paste JSON" if you prefer).

---

## Tips for building good decks

**Aim for 4-12 categories.** Fewer than 4 makes the game board feel thin; more than 12 makes category selection in the game tedious.

**Aim for 2-10 cards per category.** Tiny categories (1 card) take up a board slot but feel anticlimactic. Huge categories (15+) end up with most cards spilling into later rounds — fine if you have multiple rounds, wasteful otherwise.

**Keep titles short.** They're shown big on the front of the card. "Stanislavski's Method of Physical Actions" reads well; long full sentences don't.

**Keep bullets short and punchy.** Each bullet is one fact, one phrase, or one short sentence. The card view shows them as a bulleted list, not prose. If you find a bullet running long, see if it should be split into two.

**Group related material into categories.** This is the part the AI gets wrong most often. A category should be 5-8 cards that hang together — one topic, one theme, one figure's work, etc. If a category has cards that don't really connect, it's a sign to split it.

**Use the original source's wording where possible.** For studying, paraphrasing too aggressively can confuse the player who has the original notes in front of them.

**Polish AI-generated decks before using them.** AI does the bulk-conversion work well but often makes weird category groupings or includes throwaway cards. Five minutes in this builder turns a rough AI deck into a polished one.

---

## The full pipeline

These three tools work together as a pipeline for turning material into playable study sessions:

1. **`photo_to_text.html`** — take photos of slides, notes, or whiteboards, run them through AI vision to get text, optionally generate a rough deck JSON
2. **`deck_builder.html`** — clean up the deck (this tool). Edit titles, fix categories, polish bullets, add anything missing
3. **`study_game.html`** — load the cleaned deck, run a study session with friends

You don't have to use all three — you can hand-write JSON, you can skip the OCR step, you can even skip the builder if your AI-generated deck is good enough. But for a serious study group, the full pipeline turns "I have a phone full of lecture-slide photos and an exam in 3 days" into a playable study session in about an hour of work.

---

## Troubleshooting

**"Could not parse JSON" when pasting.** The JSON is malformed (syntax error). Common causes: missing closing brace, trailing commas, unescaped quotes inside strings. The dialog shows the parser's specific error message — usually points you to the issue. Try fixing it in a text editor and pasting again, or ask the AI that generated it to fix the syntax.

**"Deck must be a JSON object" or similar.** The JSON parsed but isn't even shaped like a deck (e.g. you pasted a JSON array, or a primitive value). The builder needs at least an object with a `categories` field to work with. Re-generate the JSON or fix it manually.

**"Loaded with N issues auto-patched" toast.** Some fields in the loaded deck didn't match the schema, but the builder fixed them so you can edit them visually. This is normal for AI-generated decks. Look for empty category names, untitled cards, or empty cards arrays in the UI and fill them in. Once everything looks good, hit Download Deck.

**The starter deck has placeholder content I don't want.** Just edit/delete the example category and example card. They're there to show you the pattern; nothing requires keeping them.

**My changes aren't saving.** Check the save status indicator in the stats bar. If it shows "⚠ Auto-save failed," your browser's localStorage is full or disabled. Use Download Deck immediately to save a copy of your work.

**The browser warns me when I close the tab.** That's intentional — to make sure you don't accidentally lose unsaved work. Auto-save still preserves your work, so you can dismiss the warning safely if you've been editing recently.

**Resume prompt didn't appear after refresh.** Either there's no saved deck (you cleaned up after a previous session) or localStorage is unavailable. Strict privacy settings, ad-blockers, or some corporate environments can block localStorage entirely.

**The downloaded JSON has a weird filename.** The filename is generated from the deck name — non-letters become underscores, then lowercased. So "Bio 101: Midterm Review!" becomes `bio_101_midterm_review.json`. You can rename the file after download.

---

## What this builder does NOT do

- **No multi-level undo or redo.** You can reverse one destructive action at a time, but not a chain, and there's no redo after undoing.
- **It does not check whether your bullet content is factually correct.** That's on you.
- **It does not generate decks from raw notes** — that's the OCR tool's job, or an AI chatbot's.
- **It does not let you preview cards as they'd look in the game.** Open the deck in the game to see how cards present.
- **It does not sync your work across devices.** The save lives only on the browser where you built the deck.

For all of these, the workflow is: download the deck regularly, version your `.json` files yourself if you want history, and keep the deck source-of-truth in files rather than browser storage.
