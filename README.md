🐍 Retro Snake — Pixel Art Edition
A single-file, fully offline, vanilla-JavaScript arcade Snake game. Free to play, free to study, and free to build on — no permission or credit required.

Coded with AI assistance and human guidance working in harmony — one person's vision and care, one model's hands on the keyboard, iterating together until it shined.

✨ The story
This game has lived three lives.

It was first written as a small personal project — a way to learn. Then, it was rebuilt and personalized as a gift for my mom (the same mom who gifted me the AI subscription that started this whole journey). Finally, it became this: the personal touches lifted out, the architecture polished, and the whole thing opened up so anyone can take it, learn from it, and make it their own.

That's the loop I wanted to close — created, given, and then given again. If it helps you fall in love with making things, especially making things alongside AI, then it has done exactly what it was meant to do.

🎮 Features
15 hand-tuned visual themes — every palette checked so the snake is always easy to see, plus a Keep Current Theme lock so it never changes on you mid-game.
Snake skins & phone-body color skins — cosmetic variety, unlocked natively through gameplay achievements.
Advanced Power-ups — Ghost Mode (pass through walls/self), Star Power (invincibility), Food Magnet (pulls items), and Length Cutter (shrinks tail size).
Coins, combos & a multiplier — quick consecutive pickups build a cascading combo with on-screen neon timers for massive high scores.
Unlock progression — earn extra themes and cosmetic skins as your lifetime food count grows, persisted directly in local storage.
Dynamic Obstacles mode — features Classic and Extreme (tri-state) modes with score multipliers (+50% / +100%) and special interaction behaviors.
No-Walls (wrap-around) mode — a gentler option that wraps coordinates using modulo arithmetic to prevent negative index crashes.
Dual optimized inputs — swipe anywhere on the board or tap the on-screen D-pad.
Zero-Jank Pause overlay — real pause and resume state tracking that pauses the game physics step without halting rendering or timing hooks.
Synthesized Web Audio — every effect is generated in real-time in code via the Web Audio API. No external audio files to ship, nothing to license.
Power & performance scaling — built-in graphics engine presets and hardware FPS limit controls to let players balance visual fidelity against device power usage.
Truly self-contained — one HTML file, zero dependencies, zero build steps, and zero network calls. Works offline forever.
▶ Play it
Pick whichever is easiest:

Locally: Double-click the HTML file (index.html) — it opens instantly in any modern web browser.
On the web: Drop the single file onto any static hosting platform — GitHub Pages, itch.io, Netlify, anything.
As a mobile app: Wrap the file with Capacitor or Cordova to compile a native Android/iOS package. No server, no installation, and no internet required.
🕹 How to play
Steer: Swipe across the screen or tap the physical-style arrow buttons.
Grow: Eat normal food to grow. Collect coins and chain quick pickups to stack multiplier combos.
Survive: Avoid your own tail and any spawned obstacles. Hitting a wall ends the run — unless you enable No-Walls, which safely wraps you to the opposite side.
⚙ Gear icon: Tap to open the Settings menu (basic options up top, highly technical performance settings under Advanced).
▶ / ❚❚ button: Start, pause, and resume your run.
🔧 Built to be hacked on
Everything is encapsulated in a single file and meticulously sectioned. Open it in any text editor and search for SECTION: (JavaScript logic) or STYLES: (CSS formatting) to jump to any element of the engine instantly. The header comment at the top is a complete codebase map.

Quick wins:

Want to…	Do this
Add a color theme	Push a new object onto the gameThemes array.
Add a snake skin	Push a new object onto the snakeSkins array.
Resize the board	Modify the gridSize, tileCountX, or tileCountY constants.
Retune the sounds	Edit the synthesizer frequency maps inside the Audio Engine section.
Tweak difficulty	Modify the speed and points structures in the difficultySettings object.
🛠 Under the hood (The Machine)
While the game looks like a standard retro title, the single-file engine is packed with professional-grade web optimizations designed to protect device battery life and eliminate layout lag:

Fixed-Timestep Game Loop: Most browser games run physics steps directly tied to the screen's refresh rate, making the snake move too fast on a 144Hz monitor and crawl on a 60Hz screen. This loop utilizes a high-precision requestAnimationFrame delta accumulator to step physics logically at a fixed millisecond rate, keeping movement speed 100% frame-rate independent.
Offscreen Canvas Caching (blockImageCache): Drawing radial gradients, complex 3D bevels, and nested shadows on every single frame causes severe CPU overhead. The rendering engine creates an in-memory tempCanvas the first time a graphic is requested, pre-renders the heavy visual effects onto it once, and caches the result. All subsequent rendering steps perform lightning-fast GPU-accelerated blits of these pre-cached image blocks.
High-DPI "Retina" Scaling Bug Smash: High-resolution mobile screens scale up canvas backings to keep lines crisp, but this often introduces a layout lurch or off-center zooming during blitting. This codebase explicitly handles devicePixelRatio on the drawing context while constraining the final blit to logical coordinates: ctx.drawImage(snakeCanvas, 0, 0, tileCountX * gridSize, tileCountY * gridSize); This ensures flawless, razor-sharp visual rendering across all high-density desktop and mobile displays.
Passive Mobile Touch Handling: To eliminate input lag, touch listeners are explicitly registered with { passive: true }. This prevents the browser from blocking touch-move events to check if a page scroll is occurring. Furthermore, the engine locks displacement tracking with a single-trigger __swipeDone flag per swipe event, keeping the frame rate butter-smooth during vigorous swiping.
Buffered D-Pad Command Queue: Rapid, high-speed maneuvers on a mobile screen often get lost between frames, or cause a self-collision if two inputs are registered within the same physics step. A custom array queue (MAX_INPUT_QUEUE_LENGTH = 2) stores inputs and processes only one valid, non-reversing direction change per tick, giving the controls a console-quality snap.
Web Audio API Synth Engine: Zero heavy MP3s, WAVs, or audio networks. Every arpeggio, collision tone, and coin collection effect is synthesized from scratch using low-level browser oscillators. By automating frequency glides (exponentialRampToValueAtTime), wave-type envelopes (square & triangle), and custom delay timers, the game achieves a robust 8-bit soundscape with absolutely zero network bytes shipped.
Adaptive Battery Controls: Features three user-selectable FPS limits (30, 60, 90 FPS) and five visual density presets (PERFORMANCE, QUALITY, ULTRA, ULTIMATE, and ULTIMATE+). Users can scale settings all the way down to a flat-shaded 30 FPS for extreme battery saving, or crank them up to a composited 90 FPS neon trail overlay.
Safe Modulo Wrapping: Wrapping coordinates on wrap-less games often crashes when moving off the left or top edges because standard modulo arithmetic yields negative values. The coordinate math is hardened to account for grid dimensions before wrapping: __hx = (__hx + tileCountX) % tileCountX; __hy = (__hy + tileCountY) % tileCountY; This guarantees mathematically bulletproof boundary wrapping on every coordinate pass.
AI-Legible Architecture: The code is explicitly written with structured sections, clear constants, and plain-English comments to act as a flawless blueprint for AI pair-programmers (Copilot, Gemini, Claude). By making the code readable for models, human developers can safely instruct an AI assistant to make modifications without risking regressions.
📜 License
Released under the MIT License — use it for anything (personal or commercial), modify it, ship it, sell it. Credit is appreciated but never required. See the LICENSE file for the full text.

🤝 Credits & acknowledgments
Created by the Digital Spellcaster — human design, direction, and guidance.

The core architecture, relentless debugging, and thirteen months of deep-trench pair-programming were forged hand-in-hand with Gemini. That massive collaboration built the engine block. Claude (Anthropic) stepped in at the final hour to help put a sparkle on the paint for a special gift version. Every decision was shaped by a human; every line was written with AI. Neither half built this alone, and that collaboration is the point of the project.

And to the person who started it all by handing me the tools: thank you, Mom. ❤

Made to be enjoyed, and to be taken apart. If you build something from this, I hope you have as much fun as we did. — the Digital Spellcaster & Gemini (with a tip of the hat to Claude)

Gemini Notebook can be inaccurate; please double check its responses.
