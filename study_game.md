# Study Game — User Manual

A Jeopardy-style flashcard board for group study over Zoom or in person.

---

## Quick start (60 seconds)

1. Open `study_game.html` in any modern browser (Chrome, Firefox, Edge, Safari). Just double-click the file.
2. Click **Download Sample Deck** to get an example file.
3. Click **Choose File** under "Load my deck" and pick the sample you just downloaded.
4. The other setup sections appear. Click **Start Game →**.
5. Click any cell on the board to flip a question.

That's it for a basic round. The rest of this manual covers the options.

---

## What this is for

The game is built for studying course material with friends or a class group, where one person hosts and others play along. It's especially designed to work well over Zoom screen-share. The host controls the board and scoring; the players see questions and call out answers.

It's not a solo study tool — there's no automatic answer-checking, no progress tracking across sessions, no spaced repetition between days. Everything happens within a single browser session.

---

## Setup screen

The setup screen has five sections. They appear progressively as you fill in earlier ones.

### 1. Get a deck

You need a "deck" — a JSON file containing the study material. There are three ways to get one:

**Load my deck** — pick a JSON file from your computer. Use this if someone else made you a deck or you've made one before.

**Try the sample** — downloads a small World Geography deck (4 categories, 13 cards). Useful for trying the game without preparing material.

**Make one with AI** — opens a window with a prompt and instructions for generating a deck from your study notes using any AI chatbot (ChatGPT, Claude, Gemini, etc.). Walk through the steps in the modal. You'll end up with a JSON file that you then load with the first option.

Once a deck is loaded you'll see the deck name, description, and category count. The other setup sections become available.

### 2. Select categories

Every category in the deck appears as a checkbox. Check the ones you want included in this game. Use **Select All** or **None** to toggle quickly.

You don't have to use everything in the deck — for a quick warmup pick three or four categories, for an exam-prep session pick everything.

### 3. Game settings

**Number of rounds** (1-20): Each round generates a fresh board. Cards don't repeat across rounds, so more rounds = more material covered. With 1 round, the game ends when the board is cleared. With 3 rounds, you'll play through three boards.

**Max columns per round** (1-8): Caps how many categories appear in each round. If you select 12 categories and set this to 5, each round randomly picks 5 of your 12. Set to 8 for the classic Jeopardy look. Set lower for shorter games.

**Max rows per round** (1-12): Caps how tall each column gets. Categories with more cards spill the extras to later rounds. Categories with fewer cards stay short — the board can be ragged (uneven heights). 

**Front of card shows**: Three difficulty modes:

- **Title only (hardest)** — the front shows just the topic name. Player has to recall everything.
- **Title + half the bullets (random)** — front shows the topic name plus a randomly selected half of the bullet points. The other half is hidden until flip.
- **Fill in the blanks** — front shows all the bullets but with random words blanked out as gold underscores. Player fills in the gaps. A slider appears letting you choose how much to blank (10% = a few hints missing, 80% = mostly blanks).

**Points per card**: Default 100. Change if you want different scoring. All cells are worth the same amount.

**Recycle missed cards** (default ON): When a card is marked No Points, it joins a queue and comes back as a "♻ Missed Cards" column in a later round (when there's room). Lets the group revisit material they got wrong. Turn off if you want a one-shot game with no second chances.

### 4. Players

Up to 5 player slots. Check the box next to a slot to enable it. Names are optional — leave blank and the slot just shows as "P1," "P2," etc.

The names-optional design is deliberate: when sharing screen on Zoom, sometimes you don't want everyone's name showing on screen, or you want to keep things anonymous. Just toggle on the slots you need.

### 5. Start

**Start Game** — locks in your settings, generates the rounds, opens the game board.

**Open Host Cheat Sheet** — opens a separate browser window with every card's title and full details, organized by round and category. This is for the host's eyes only — see "The cheat sheet" section below.

---

## Playing the game

### The board

Looks like a Jeopardy board. Categories across the top, point values (e.g. 100) in the cells. Click any cell to flip a card.

If recycling is on and missed cards are queued, a red-tinted **♻ Missed Cards** column appears starting in round 2 (or any later round with room).

### The card view

Clicking a cell takes over the whole window with the card. Big title, optionally some bullets or blanks (depending on which front-mode you picked).

The category tag at the top of the card view shows where this card came from. For cards pulled from the recycled-missed column, it's prefixed with **♻ MISSED** so players can see they're on a second-chance question — but the original category name is still shown so they know what subject is being tested.

Two buttons:
- **Flip to Answer →** reveals the full back of the card with all bullets shown, plus the player buttons.
- **← Back to Board** returns without scoring (use if a cell was clicked by accident).

### Awarding points

After flipping, the host sees buttons for each active player plus a "No Points" button.

- Click **P1** / **P2** / etc. to award the card's points to that player.
- Click **No Points** if nobody got it. With recycling on, this also flags the card to come back later.

The card is then marked used (the cell darkens on the board) and you return to the board.

### Undoing a mistake

Misclicked the wrong player? The **↶ Undo** button in the top bar reverses the last award — it returns the points, un-marks the cell as used, and (if applicable) removes the card from the missed-cards queue. The button is disabled when there's nothing to undo, and hovering over it shows which action will be reversed.

The undo stack holds your last 20 awards, so even if you realize the mistake several cards later, you can still walk back. Keyboard shortcut: **Ctrl+Z** (or **Cmd+Z** on Mac).

For larger fixes — like manually crediting a partial answer — click any score in the right-hand panel to edit it directly. Type the new value, press Enter to save or Escape to cancel.

### Round transitions

Once you've played some cells in a round, the **Next Round →** button appears in the top bar. It turns gold (primary) when the current round is fully cleared, but you can advance early at any time — leftover cells in the current round are just abandoned.

### Ending the game

**End Game** anytime brings up the standings screen. Top scorer is highlighted in gold. Options:

- **New Game** — same deck, same settings, fresh shuffle and zero scores.
- **Back to Setup** — go back to change settings or pick a different deck.
- **Keep Playing** — close the standings, return to the board.

If recycling was on and there are unanswered cards still queued at game end, the standings note this — those are good candidates for next session.

### Auto-save and resume

Your game is automatically saved to your browser's local storage after every meaningful action (start, award, undo, score edit, round advance). If something goes wrong — accidental refresh, browser crash, you closed the tab to look something up — your game survives.

When you reopen the tool with a saved game in progress, you'll see a **Resume Game?** prompt with the deck name, current round, and combined score. Click **Resume** to pick up where you left off, or **Discard & Start Fresh** to clear it.

The save is automatically discarded when you click **Back to Setup** from the end-of-game screen — that's the signal that the previous game is done. Otherwise it's preserved indefinitely until a new game replaces it.

A few notes:

- **The save lives on the device and browser you used.** Different computer, different browser, or private/incognito window means no save. The auto-save also won't survive clearing your browser's site data.
- **Only the most recent game is saved.** Starting a new game replaces the previous save.
- **The save includes the full deck.** You don't need the original JSON file to resume — even if you've moved the file or deleted it.
- **In private/incognito mode**, the save still works for the current session but disappears when the window closes.

### Keyboard shortcuts

For hosts running a fast-paced session, the keyboard is much faster than the mouse:

**On the card front (before flip):**
- **Space** or **Enter** — flip to the answer
- **Esc** — back to board (if you opened the wrong card)

**On the card back (after flip):**
- **1** to **5** — award points to that player number (only works for active players)
- **0** — No Points
- **Esc** — back to board

**On the game board:**
- **Ctrl+Z** (or **Cmd+Z** on Mac) — undo the last award
- **N** — next round (when available)
- **C** — open the cheat sheet

Shortcuts are paused while you're typing in a text field (player names, score editing, etc.) and while modal dialogs are showing — so they don't interfere with normal interaction.

---

## The cheat sheet

The cheat sheet is the host's answer key. It opens in a **separate browser window** so when you screen-share the game, the cheat sheet stays private.

Available in two places:

- **Setup screen**: After loading a deck, click **Open Host Cheat Sheet**. This generates the rounds (so the cheat sheet matches what'll be played) and opens them.
- **Game screen**: The **Cheat Sheet** button in the top bar opens the same view.

The cheat sheet shows every round, every category, every card with its title and full details. For Missed Cards columns, each card is prefixed with its source category in red so you know what's being asked.

There's a **Print** button at the top of the cheat sheet window — prints with sensible page breaks, one round per page where possible.

### Zoom screen-sharing tip

When sharing your screen on Zoom, share **just the game window** (not your whole desktop or screen). Your cheat sheet stays in a separate window that Zoom isn't sharing, so players can't see it. This is the whole reason it opens in a new window.

---

## Tips for hosts

**Generate the cheat sheet before starting the game** so you can print it or have it on a second monitor. If you change any setting after opening the cheat sheet, the rounds regenerate and the next time you open the cheat sheet it'll show the new content.

**Pick category and round counts that fit your time.** Rough guide:
- Quick warmup: 3-4 categories, 1 round, ~10 minutes
- Standard study session: 5-6 categories, 2 rounds, ~30 minutes
- Exam-prep marathon: 8 categories, 3-4 rounds, an hour or more

**Use Title-Only mode for hard recall and Fill-in-the-Blanks for fluency.** Halfway through a semester you might use Half-Bullets; the night before an exam, switch to Title-Only.

**Don't be afraid to fix scores by clicking them.** It's there for a reason.

**The "No Points" button doesn't have to mean "complete failure."** If your group's struggling with a card, marking it missed brings it back later — by which point someone might've remembered. Lighter use of the recycle feature.

**Learn the keyboard shortcuts.** Even just `Space` to flip and `1-5` to award points is much faster than clicking, especially over a 30-card session. If you misclick, `Ctrl+Z` undoes immediately.

---

## Tips for making good decks

If you're hand-writing decks (not using AI), the schema is:

```json
{
  "schema_version": 1,
  "deck_name": "Your deck name here",
  "description": "A short one-sentence description",
  "categories": [
    {
      "name": "Category name",
      "cards": [
        {
          "title": "The prompt that goes on the front",
          "details": ["First bullet", "Second bullet", "..."]
        }
      ]
    }
  ]
}
```

Practical guidelines:

- **4-12 categories per deck** is comfortable. Less than 4 and the board feels thin. More than 12 and selection becomes tedious.
- **2-10 cards per category**. Tiny categories (1 card) just take up a board column slot. Huge categories (15+) end up with most cards spilling to later rounds.
- **Keep titles short** — they're the front of the card and need to read well at large size.
- **Keep bullets short** — one fact per bullet. Avoid full paragraphs. The card view shows them as bulleted items, not prose.
- **Use the source's wording where possible** — for studying, paraphrasing too aggressively can confuse the player who has the original notes.

If you're using the AI generation flow (the "Make one with AI" option), the prompt already encodes most of this guidance. Edit the resulting JSON by hand if categories are off — it's just a text file.

---

## Troubleshooting

**"Invalid deck file" when loading.** The JSON didn't parse or didn't match the schema. Check that the file starts with `{` and ends with `}`, that all field names are double-quoted, and that there are no trailing commas. If you used AI to generate it and didn't strip the ```` ```json ```` code fences, that'll cause this.

**Board cells aren't clickable.** Either the round is fully played (every cell darkened) or the cell is marked empty because the category had fewer cards than the row count. Empty cells appear as faint dashed boxes.

**Cheat sheet window doesn't open.** Pop-up blocker. Allow pop-ups from the page (or wherever you're hosting the file from) and try again.

**My game disappeared after closing the browser.** It shouldn't — auto-save preserves the in-progress game. Possible causes: you were using a private/incognito window (saves don't persist), your browser is configured to clear site data on close, or you clicked "Discard & Start Fresh" on the resume prompt. If the resume prompt didn't appear at all, your localStorage may be disabled or full.

**The resume prompt isn't appearing.** Either there's no saved game (you finished the last one cleanly with "Back to Setup"), or localStorage is unavailable. Strict privacy settings, ad-blockers, or some corporate environments can block localStorage entirely.

**Fonts look wrong.** The game uses Google Fonts (Bebas Neue, Crimson Text). If you're offline or behind a strict firewall, those won't load and you'll see fallback fonts. Game still works fine.

**Something seems broken.** Open the browser console (F12 or Cmd+Opt+I) and look for red error messages. Most issues are caused by a malformed deck file.

---

## What this game does NOT do

To set expectations clearly:

- It does not check whether players' answers are correct. The host decides.
- It does not have built-in voice/video — use Zoom, Google Meet, etc. for that.
- It does not track which cards you've answered well over time, only which you've missed in the current game.
- It does not have multiplayer over the internet — there's one host, one shared screen.
- It does not have a built-in deck editor. Edit JSON files in any text editor.
- It does not sync your saved game across devices. The save lives only on the browser where you played.

For most of these, that's because the game is meant to be a tool the host runs during a study session, not a study app you live in.
