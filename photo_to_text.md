# Photo to Text — User Manual

A standalone HTML tool that takes images (photos of slides, notes, whiteboards, textbook pages, etc.), runs them through an AI vision model, and gives you back the text. It can also convert that text into a study deck for the Study Game.

---

## Quick start (5 minutes)

1. Open `photo_to_text.html` in any modern browser.
2. Pick your AI provider in the dropdown (OpenAI, Anthropic, Google Gemini, or any OpenAI-compatible service).
3. Get an API key from your chosen provider (links below) and paste it into the API key field.
4. Drag a few images into the drop zone (or click to browse).
5. Click **Process batch**. Each image will be sent to the AI and the extracted text will appear below.
6. Edit any errors in the text boxes, then click **Copy all text**, **Download .md**, or **Convert to Game Deck**.

---

## What this is for

Three main use cases:

1. **Capturing notes from slides or photos.** You took pictures of a lecture's slides, a whiteboard, or pages from a textbook. Run them through here and get text you can edit, search, or paste elsewhere.
2. **OCR on handwriting.** Most modern AI vision models handle handwriting reasonably well — better than traditional OCR for messy notes.
3. **Building a study deck.** After processing your notes, click **Convert to Game Deck** to send the extracted text back through the AI to produce a JSON deck file for the Study Game. This turns "photos of slides" into "playable study session" in two clicks.

---

## You'll need an API key

This tool calls AI APIs directly from your browser. You need your own API key from one of the supported providers. There is no free or built-in option — APIs cost money (usually a few cents per image, more on that below).

### Where to get keys

- **OpenAI**: https://platform.openai.com/api-keys (requires a paid account with credit; new accounts sometimes get a small free trial)
- **Anthropic**: https://console.anthropic.com/ (also requires credit on file)
- **Google Gemini**: https://aistudio.google.com/app/apikey (Google AI Studio has a free tier with rate limits)
- **OpenAI-compatible**: any service that exposes a standard `/chat/completions` endpoint, like a local Ollama server, OpenRouter, Together, etc. You'll provide the base URL.

### How keys are stored

The tool saves your provider, model, base URL, and prompt to your browser's localStorage so they persist between sessions. **The API key is NOT saved** — you have to paste it each time you open the tool. This is a deliberate security choice. If your computer is shared or you don't want the hassle, you can leave it that way; if it's your personal machine and you trust it, just keep the tab open or paste the key from a password manager.

### Cost estimates

Rough rates as of writing — check current pricing before going wild:

- A single image extraction: usually **half a cent to two cents** depending on the model
- A 50-image batch: roughly **25 cents to a dollar**
- The deck-generation conversion (after the batch): another few cents — same range as a single image, since it's text-only at that point

If you're running this for class study notes, expect total cost in the **single-digit dollars** for an entire semester's worth of material. Cheap enough that running it at home is fine; not cheap enough to leave running unattended.

---

## Provider settings

### Provider dropdown

Pick one of: OpenAI, Anthropic, Google Gemini, OpenAI-compatible. Each has different strengths:

- **OpenAI** (default model `gpt-4.1-mini`): generally good at text-heavy images, decent at handwriting, fast.
- **Anthropic** (default `claude-3-7-sonnet-latest`): strong at structured content, formulas, complex layouts. Slightly more expensive but often more accurate.
- **Google Gemini** (default `gemini-2.5-flash`): cheapest, has a free tier, decent quality. Good first choice if you're cost-sensitive.
- **OpenAI-compatible**: for local LLMs (Ollama, LM Studio) or aggregators (OpenRouter, Together AI, etc.). You'll need to know the base URL.

### Model

The model name. Each provider auto-fills its default when you switch. You can change it to any vision-capable model the provider offers — if you're not sure, leave the default.

### Compatible base URL

Only appears when "OpenAI-compatible" is selected. This is the API endpoint URL minus the `/chat/completions` part. Examples:
- Local Ollama: `http://localhost:11434/v1`
- OpenRouter: `https://openrouter.ai/api/v1`
- Together AI: `https://api.together.xyz/v1`

### API key

Your secret. Paste it in. Required for every provider (Gemini's free tier still requires one, just no payment).

### OCR / extraction prompt

The prompt sent to the AI for each image. The default is reasonable for general use — it tells the model to transcribe everything visible, preserve structure, mark unclear handwriting with `[unclear]`, and return `[No readable text found]` if there's nothing to read.

You can edit this if you have specific needs. For example, if your photos are math notes, you could add "Use LaTeX notation for equations." If your photos are recipes, "Format the output as ingredients first, then steps."

The prompt is saved to localStorage so you don't have to re-enter it.

---

## Uploading and processing

### Drop zone

Drag image files in, or click to open a file picker. Supports PNG, JPG, WEBP, GIF, BMP, and HEIC (if your browser can decode HEIC, which Safari/iOS do natively but most desktop browsers don't).

You can drop multiple files at once. They'll be added to the queue in the order you drop them.

### Stats bar

Four counters above the upload area: Images, Done, Queued, Failed. Updated live as the batch runs.

### Process batch

Sends all queued images through the AI, one at a time, in order. Each image is shown with its preview on the left and the resulting text in an editable text area on the right.

Processing happens **sequentially**, not in parallel. This is intentional — most providers have rate limits, and batching one at a time avoids tripping them. A 10-image batch takes 30 seconds to a couple of minutes depending on the model.

### Per-image controls

Each image card has:
- **Run** — re-runs just that image. Useful if the result was bad and you want to try again, or if you changed the prompt.
- **Remove** — removes the image from the queue and the page.
- **Copy page** — copies just that image's extracted text to your clipboard.
- **Download .txt** — saves just that image's text as a `.txt` file.

You can edit the text directly in the box. Edits stick (they're not overwritten unless you click Run again).

### Status badges

Each image shows a status:
- **Queued** — waiting to be processed
- **Running** — currently being sent to the AI
- **Done** — completed successfully
- **Failed** — error occurred (shown in red below the image; details shown above the text box)

---

## Toolbar buttons

Above the image list:

**Process batch** — runs all queued images.

**Copy all text** — copies every image's extracted text to your clipboard, formatted as Markdown with `# Page N: filename` headers separating them.

**Download .md** — downloads the same combined text as `batch-photo-to-text.md`.

**Convert to Game Deck** — sends the combined text through the same AI to produce a study deck JSON file. See "Converting to a study deck" below.

**Print / Save PDF** — opens the browser's print dialog. The page is styled for clean printing — one image per page, with the text alongside the photo.

**Clear all** — removes all images from the page and resets the queue. Doesn't change settings.

---

## Converting to a study deck

This is the bridge to the Study Game. After you've processed your batch and the text looks good, click **Convert to Game Deck**. The tool:

1. Bundles all extracted text from completed pages into one document
2. Sends it back to the AI (same provider/key/model) with a prompt asking for a study deck in the right JSON format
3. Strips any markdown code fences the model might add despite instructions
4. Validates the response is JSON and roughly looks like a deck
5. Downloads it as `<deck_name>.json`

Drop that file into the Study Game's "Load my deck" section and you're playing.

### What the deck will look like

The AI groups your material into 4-12 categories of related topics, with 2-10 cards per category. Each card has a short title (the prompt) and a list of bullet-point details (the answer). The AI tries to use your source's wording rather than paraphrasing.

The output quality depends heavily on input quality. **Bullet-pointed notes work great**; long unstructured prose works less well. If a deck looks bad, the fix is usually to clean up the source text in the page text boxes before clicking Convert again.

### If conversion fails

Sometimes the AI returns something that isn't valid JSON. When that happens, the tool saves the raw response as `deck-raw-response.txt` so you don't lose the work. You can:

- Open it in a text editor, fix the issue (usually a missing brace or a trailing comma), and rename to `.json`
- Or paste the raw response into a new chat with the AI and ask it to fix the syntax

---

## Tips for better results

**For OCR (extracting text):**

- Take photos with good lighting and minimal glare.
- Get the page reasonably square in the frame — don't worry about perfect alignment, but very tilted shots reduce accuracy.
- For handwriting, write neatly and use dark ink on light paper.
- If a specific image has a lot of errors, click its **Run** button to retry — sometimes the model has a better attempt the second time.

**For deck conversion:**

- Process images in the order the material appears in your course. The AI is more likely to group related content into categories if it sees them sequentially.
- Edit out junk text in the per-page text boxes before converting (slide numbers, copyright footers, "Continued on next page", etc.). The AI works better with clean source material.
- If your notes cover multiple distinct topics that should be separate decks, run separate batches. Don't mix Bio 101 and English Lit into one batch and expect a coherent deck.

**For cost control:**

- Start with a small batch (3-5 images) to verify your settings work, then scale up.
- Use the cheaper "mini" or "flash" models for OCR — they're usually plenty good for typed text. Reserve the bigger models for bad handwriting or complex layouts.
- The deck conversion uses one extra API call regardless of batch size, so the marginal cost of "Convert to Deck" is small.

---

## Troubleshooting

**"OpenAI request failed" (or similar) with a 401 error.** API key is invalid or expired. Check it. For OpenAI/Anthropic, also check your account has a positive balance.

**"OpenAI request failed" with a 429 error.** Rate limited. Wait a minute and try again, or switch to a different model on the same provider.

**CORS error in the browser console.** Some self-hosted compatible endpoints don't allow direct browser access. The warning text on the page mentions this. Solutions: (a) use a different provider, (b) configure CORS on your endpoint to allow the page origin, or (c) run a small proxy.

**Anthropic CORS error.** Anthropic does support browser access if you use the `anthropic-dangerous-direct-browser-access` header, which this tool does. If you still get CORS issues, your network might be blocking it — try a different network.

**Convert to Deck produces a deck with weird categories or missing content.** The AI's grouping is best-effort and sometimes wrong. Try one of:
- Clean up the source text first (remove headers/footers, fix obvious OCR errors)
- Edit the deck JSON file directly afterward — it's plain text
- Re-run conversion (each call is randomized, you might get a better grouping)

**The image previews are sideways or upside-down.** Some cameras embed orientation metadata that browsers display correctly but the AI ignores. If extracted text seems jumbled, try rotating the original image before uploading.

**Browser refresh wipes my work.** Yes. Image files, processed text, and queue state all live in memory. Settings (provider, model, base URL, prompt) are saved to localStorage; the API key and your batch are not. Don't refresh mid-batch.

---

## Privacy notes

- Your API key, images, and extracted text are all sent to whichever provider you choose. Their privacy policies apply.
- The page itself doesn't have a server — there's no analytics, no telemetry, no tracking. Images and keys go directly from your browser to the AI provider.
- Anthropic and OpenAI both have policies about not training on API content by default, but check current terms.
- For sensitive material (medical records, confidential documents, etc.), be aware that you are uploading those documents to a third-party AI service. If that's a concern, use a local OpenAI-compatible endpoint (Ollama running on your machine) instead.

---

## What this tool does NOT do

- It doesn't store your batch between sessions. Refresh and you start over.
- It doesn't process PDFs directly. Convert PDF pages to images first (most PDF viewers can do this, or use a tool like ImageMagick).
- It doesn't proxy your API calls — your browser talks directly to the provider, which is why CORS sometimes matters.
- It doesn't have built-in image editing. Crop, rotate, or adjust contrast in any photo app before uploading if needed.
- It doesn't run AI models locally — even with the "Compatible" provider, you're still pointing at an HTTP endpoint somewhere (which could be local, like Ollama, but is still a separate process).

For most of these, simpler is better — keeping it a single-file HTML tool with no install means you can use it anywhere a browser runs.
