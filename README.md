# Memoriser

A tiny web app to help you memorise text by progressively **cutting** words (keep first 3/2/1 letters), optionally **substituting** parts with underscores, then **scrambling** and **reordering** sentences while you track progress.

Designed to run as a static site (GitHub Pages) and also inside **Telegram WebApp** with optional CloudStorage persistence.

---

## Features

* **Labels**: save multiple sets (e.g., *Genesis 1*, *Genesis 2*).
* **Generate**: split the textarea into sentences and attach to the selected label.
* **Cut** modes: 3 / 2 / 1 (keeps first N letters of each word).
* **Substitute**: mask words with `_` at 0/30/50/90% probability (per word, deterministic per sentence/label).
* **Scramble & Drag**: shuffle and drag sentences to reorder (long‑press to drag on mobile).
* **Practice cards**: ✅ increments *ok* (auto‑hides), ❌ increments *bad*.
* **Struggling filter**: show only items with ❌ count ≥ *N*.
* **Tap a card**: highlights the full sentence inside the textarea for quick reference.
* **Manage**: New / Rename / Delete label; **Copy Progress (CSV)**.
* **Persistence**: write‑through to LocalStorage and (when available) Telegram CloudStorage.

---

## Quick start (GitHub Pages)

1. Create a repo (e.g., `crammie`).
2. Add **`index.html`** (the built app) and **`favicon.svg`** to the repo root.
3. Push to `main`.
4. In *Settings → Pages*, set **Build and deployment** to `Deploy from a branch`, `main` / `/ (root)`.
5. Open `https://<your-username>.github.io/<repo>/`.

### Custom domain

* Add your domain in *Settings → Pages* under **Custom domain**.
* Create a CNAME record pointing to `<your-username>.github.io`.
* Wait for DNS to propagate. Ensure the Page is set to **Enforce HTTPS**.

---

## Using the app

### 1) Add or select a label

* **Manage → New** shows the *New label* input above the textarea.
* Paste your text, then click **Generate**. The text is saved under that label and shown as practice cards.
* Selecting a label in the dropdown loads its sentences back into the textarea (one per line).

### 2) Generate / Replace

* **Generate** with an existing label **replaces** the label’s sentences with the current textarea lines.
* The app splits text into sentences by **terminal punctuation** (`. ! ? …`) or **blank lines**. Single line‑wraps are collapsed.

### 3) Practice

* Choose **Cut**: 3/2/1. With **Substitute** 30/50/90%, some words are replaced by `_` (per‑word masking; deterministic by label/sentence).
* **Scramble** to shuffle; long‑press on a card (mobile) to drag and reorder.
* **✅** marks *correct* → increments ok and auto‑hides; **❌** marks *incorrect* → increments bad.
* **Show only struggling** displays cards whose ❌ ≥ *N* (configurable).
* **Check order** briefly outlines cards **green** if in natural order for the visible set, or **red** otherwise.

### 4) Manage labels

* **New**: create a new label (Generate saves it).
* **Rename**: rename selected label.
* **Delete**: removes the label and its sentences (undo supported within the session).
* **Copy Progress (CSV)**: copies a CSV to the clipboard with sentence, label, cut/sub%, ok/bad, and learned.

---

## Storage & Data

* **LocalStorage key**: `memoriser:v1` (JSON). This persists in any modern browser and in Telegram’s in‑app browser.
* **Telegram CloudStorage**: used when Telegram WebApp version ≥ **6.9**. Data is chunked and saved as `memoriser:v1:index` + chunk keys.
* **Migration**: the app auto‑loads from legacy keys (`mv7`, `memoriserv1`, `memoriser v1`) and rewrites to `memoriser:v1`.
* The header shows a small status line indicating where the last save occurred.

> **Note:** Telegram CloudStorage is user‑ & bot‑scoped. On very old Telegram clients CloudStorage may be unavailable; LocalStorage still works.

---

## Keyboard shortcuts

* **Enter** = ✅ mark correct
* **Backspace** = ❌ mark incorrect
* **↑ / ↓** = move focus across cards
* **S** = Scramble
* **R** = Reset order

---

## Troubleshooting

* **Sentences don’t split** → Ensure text uses `. ! ? …` or insert blank lines. Soft line‑wraps are collapsed automatically.
* **Nothing after refresh** → Check DevTools → Application → Local Storage → look for `memoriser:v1`. If absent, add a label + Generate once.
* **Inside Telegram** → CloudStorage requires client version ≥ **6.9**. If unavailable, the header will show *Local storage* only.
* **Drag on mobile** feels fiddly → long‑press ~200ms; device may need a short haptic nudge before drag starts.

---

## Development notes

* Single‑file app (no build step). Open `index.html` directly or host statically.
* Main areas: *Input* (labels + textarea), *Configuration* (cut / substitute / scramble / filters), *Practice* (cards, progress).
* Core functions: `splitSentences`, `applyCutAndSub`, `render`, storage helpers, label management, mobile drag.

---

## License

MIT — do whatever you like, attribution appreciated.

