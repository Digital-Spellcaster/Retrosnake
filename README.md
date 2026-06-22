[README.md](https://github.com/user-attachments/files/29210093/README.md)
# 🐍 Retro Snake — Pixel Art Edition

A single-file, fully offline, vanilla-JavaScript arcade Snake game.
**Free to play, free to study, and free to build on — no permission or credit required.**

> Coded with **AI assistance and human guidance working in harmony** — one person's vision and care, one model's hands on the keyboard, iterating together until it shined.

---

## ✨ The story

This game has lived three lives.

It was first written a year ago as a small personal project — a way to learn. Then, this year, it was rebuilt and personalized as a **gift for my mom** (the same mom who gifted me the AI subscription that started this whole journey). Finally, it became this: the personal touches lifted out, the architecture polished, and the whole thing opened up so **anyone** can take it, learn from it, and make it their own.

That's the loop I wanted to close — *created, given, and then given again.* If it helps you fall in love with making things, especially making things alongside AI, then it has done exactly what it was meant to do.

---

## 🎮 Features

- **15 hand-tuned visual themes** — every palette checked so the snake is always easy to see, plus a *Keep Current Theme* lock so it never changes on you mid-game.
- **Snake skins & phone-body color skins** — cosmetic variety, some earned through play.
- **Power-ups** — ghost mode, star power, food magnet, and more.
- **Coins, combos & a multiplier** — quick consecutive pickups build a combo for bigger scores.
- **Unlock progression** — earn extra themes and skins as your lifetime food count grows.
- **Obstacles mode** (classic & extreme) and multiple **difficulty levels**.
- **No-Walls (wrap-around) mode** — a gentler option that lets the snake pass through edges.
- **Two control schemes** — swipe anywhere on the board *or* tap the on-screen D-pad.
- **Real pause/resume** with a clear on-screen overlay.
- **Lifetime stats** and **per-difficulty high scores**.
- **Accessibility** — a Large Fonts toggle for easier reading.
- **Synthesized sound** — every effect is generated in code via the Web Audio API. No audio files to ship, nothing to license.
- **Truly self-contained** — one HTML file, no dependencies, no build step, no network calls. Works offline forever.
- **Mobile-ready & crisp** — high-DPI-correct rendering and a Capacitor-friendly layout.

---

## ▶️ Play it

Pick whichever is easiest:

- **Locally:** double-click the HTML file (rename it to `index.html` if you like) — it opens in any modern browser.
- **On the web:** drop the single file onto any static host — GitHub Pages, itch.io, Netlify, anything.
- **As a mobile app:** wrap the file with [Capacitor](https://capacitorjs.com/) or Cordova to build an Android/iOS package.

No server, no install, no internet required.

---

## 🕹️ How to play

- **Steer:** swipe across the screen, or tap the arrow buttons.
- **Grow:** eat the food. Collect coins and chain pickups to build combos.
- **Survive:** avoid your own tail and any obstacles. Hitting a wall ends the run — unless you enable **No-Walls**, which wraps you to the opposite side.
- **⚙️ Gear icon:** open Settings (basic options up top, everything else under *Advanced*).
- **▶ / ❚❚ button:** start, pause, and resume.

---

## 🔧 Built to be hacked on

Everything is in one file and clearly sectioned. Open it and search for `SECTION:` (JavaScript) or `STYLES:` (CSS) to jump anywhere instantly. The header comment at the top is a full map.

Quick wins:

| Want to… | Do this |
| --- | --- |
| Add a color theme | Push an object onto the `gameThemes` array |
| Add a snake skin | Push an object onto the `snakeSkins` array |
| Resize the board | Change `gridSize`, `tileCountX`, `tileCountY` |
| Retune the sounds | Edit the `play*Sound()` helpers in the Audio Engine section |
| Tweak difficulty | Edit the `difficultySettings` object |

Two spots carry plain-English warnings in the code because they're easy to break: the **high-DPI snake blit** (keep the logical size argument) and the **fixed-timestep game loop** (keeps speed frame-rate-independent). Read those notes before touching them.

---

## 🛠️ Under the hood

- **Vanilla HTML / CSS / JavaScript** — no frameworks, no bundler, no tooling.
- **Canvas rendering** with correct device-pixel-ratio handling so it stays sharp on any screen.
- **Web Audio API** for all sound (procedurally synthesized — public-domain by nature).
- **localStorage** for settings, high scores, and lifetime stats.

---

## 📜 License

Released under the **MIT License** — use it for anything (personal *or* commercial), modify it, ship it, sell it. Credit is appreciated but **never required**. See the `LICENSE` file for the full text.

---

### 🤝 Credits & acknowledgments

Created by **the Digital Spellcaster** — human design, direction, and guidance.

The core architecture, relentless debugging, and thirteen months of deep-trench pair-programming were forged hand-in-hand with **Gemini**. That massive collaboration built the engine block. **Claude (Anthropic)** stepped in at the final hour to help put a sparkle on the paint for a special gift version. Every decision was shaped by a human; every line was written with AI. Neither half built this alone, and that collaboration *is* the point of the project.

And to the person who started it all by handing me the tools: thank you, Mom. ❤️

> *Made to be enjoyed, and to be taken apart. If you build something from this, I hope you have as much fun as we did.*

— **the Digital Spellcaster** & **Gemini** (with a tip of the hat to Claude)
— **the Digital Spellcaster** & **Claude**
