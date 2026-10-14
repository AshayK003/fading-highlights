<div align="center">

# Fading Highlights

**A memory decay & active recall document tool — highlights fade as you forget, forcing you to earn them back.**

[![License: AGPL v3](https://img.shields.io/badge/license-AGPLv3-22C55E)](LICENSE)
[![Live Demo](https://img.shields.io/badge/demo-live-22C55E)](https://ashayk003.github.io/fading-highlights/)
[![Status](https://img.shields.io/badge/status-prototype-F59E0B)](https://github.com/AshayK003/fading-highlights)
[![Dependencies](https://img.shields.io/badge/runtime%20deps-zero-38BDF8)]()

</div>

---

Most highlighting tools treat a highlight as permanent ink. This one treats it as **memory** — and memory fades. Load a PDF or paste text, mark what matters, and the tool models when you're about to forget each passage. Faded highlights pull you back into recall mode, where you commit to remembering *before* the reveal.

Built as a single self-contained HTML file. No build step, no server, no accounts.

## Quick Start

**[Try the live demo](https://ashayk003.github.io/fading-highlights/)** — no install needed.

Or run it locally:

```bash
git clone https://github.com/AshayK003/fading-highlights.git
cd fading-highlights
```

Open `fading-highlights.html` in any modern browser. That's it.

> An internet connection is required only on first use — the PDF parser (pdf.js) and webfonts load from CDNs. All study state is stored locally in your browser.

## Features

- **PDF + paste ingestion** — Load a digital-text PDF or paste raw text (blank-line paragraph splitting). Scanned PDFs are detected and rejected with a clear message instead of silently producing garbage.
- **Select-to-highlight with suggestion chips** — Drag to mark a passage, or accept auto-suggested key terms. Suggestions dedupe against existing highlights.
- **Retrievability model** — Every highlight carries a stability `S` and a current retrievability `R`, computed from time since last review: `R = (1 + 0.95·decay·t/S)^-0.6`. Passages fade visually as `R` drops toward the lapse threshold.
- **Commit-before-reveal recall** — The masked passage never reveals until you commit to "I remember this." Overclaiming is tracked against actual performance as a **calibration gap** — the distance between what you think you know and what you do.
- **Latency-graded stability** — Your grade is inferred from how fast you commit, not from buttons: fast commit → Good (S ×1.6), slow commit → Hard (S ×1.15), miss or timeout → lapse (S halves, review in ~1 day).
- **Simulation clock** — Run at realtime or accelerate 1 day = 10 seconds to watch decay and lapse cycles play out.
- **Memory map** — A compact panel of every highlight's live state: fresh, decaying, due, or lapsed.
- **Per-highlight control** — From the memory map, act on any single highlight: **pin** it to stop fading, mark it **already known** to retire it from the rotation, **ask me again** to force it due now, or **delete** it with a 5-second undo. Overrides persist and freeze the highlight's decay clock until lifted.
- **Persistence** — State (documents, highlights, calibration, active doc) survives refresh via `localStorage`. Corrupt or stale state degrades gracefully to a clean boot.
- **Multi-document** — Independent documents with independent calibration and decay.
- **XSS-safe rendering** — User-supplied text and highlight targets are escaped before any DOM insertion.

## How It Works

The model is a simplified, single-pass take on spaced-repetition schedulers (FSRS-family), not a port of any specific implementation:

```
stability S      — how long until this passage is likely forgotten
retrievability R — probability you'd recall it right now, decays with time
lapse            — R drops below threshold → passage routes to re-read, S halves
```

The value is the loop: **fade → mask → commit → reveal → adjust**. Each cycle tightens your calibration — the feedback signal the tool exists to produce.

## Tech

- Single `fading-highlights.html` — vanilla JS, ~3k lines, no framework
- [pdf.js 3.11](https://mozilla.github.io/pdf.js/) via CDN for PDF text extraction
- `localStorage` for persistence
- No backend, no telemetry, no accounts

## Project Status

Prototype. The retention model is intentionally simple and the verification tests are static. Treat scheduling decisions as illustrative, not calibrated.

## License

[GNU AGPL v3](LICENSE) — free to use, modify, and share; network use of modified versions must release source.

## Author

Built by [Ashay Kushwaha](https://github.com/AshayK003)

<p align="center">
  Built by <a href="https://github.com/AshayK003">Ashay Kushwaha</a> ·
  <a href="LICENSE">GNU AGPL v3</a> ·
  <a href="https://chai4.me/ashaykushwaha003"><img src="https://img.shields.io/badge/support%20developer-chai4.me-FFD700?style=flat&logo=buymeacoffee&logoColor=black" alt="Support Developer"></a>
</p>
