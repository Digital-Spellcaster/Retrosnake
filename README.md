# 🐍 Retro Snake — Pixel Art Edition (Final Edition)

A high-performance, single-file, zero-dependency HTML5 arcade engine. **Free to play, free to study, and free to build on under the MIT License.**

*Coded through a harmonious partnership of human design and AI direction—forged with care, iterated step-by-step, and optimized to run flawlessly across desktop, high-Hz monitors, and mobile screens.*

---

## ✨ The Story of Retrosnake

This codebase has lived three distinct lives:
1. **The Learning Project:** Born as a small, self-contained experiment to explore vanilla browser technologies.
2. **The Gift:** Rebuilt and personalized as a special birthday gift for my mother (who generously provided the AI subscription that made this level of deep-trench coding possible). 
3. **The Final Edition:** Polished, generalized, and structurally optimized so that the public-facing version stands as a highly performant, education-ready arcade template for other developers.

It is designed to close a creative loop: *created, given, and then given again.* If it helps you fall in love with building software alongside artificial intelligence, it has accomplished its purpose.

---

# 🕹️ PART I: THE PLAYERS & HACKERS MANUAL
*A user-friendly guide to the features, mechanics, and quick-win visual customizations.*

### 🎮 Gameplay Features

*   **15 Hand-Tuned Visual Themes:** Every palette is custom-engineered to ensure high visual contrast between the snake and the board. Includes a *Keep Current Theme* setting to lock in your favorite style.
*   **Gameplay Unlock Progression:** Collect normal food to build your lifetime score and unlock legendary phone body skins, snake patterns, and aesthetic themes.
*   **Advanced Power-Ups:** 
    *   *Ghost Mode:* Slip right through your own tail and obstacles.
    *   *Star Power:* Grant full invincibility and transform obstacles into point multipliers.
    *   *Food Magnet:* Automatically pull nearby items towards your head.
    *   *Length Cutter:* Shrink your tail by up to 5 segments to buy yourself breathing room.
*   **Neon Multiplier & Combos:** Grab consecutive items quickly to build high-score combos with custom on-screen neon progression timers.
*   **Double-Multiplier Obstacles Mode:** Engage Classic (+50% score) or Extreme (+100% score) obstacle layers for high-risk, high-reward tactical play.
*   **Walls: OFF (Wrap-Around):** Toggle a wrap-around coordinate setting to play a gentler, mathematically wrapped layout.

---

### 🕹️ How to Play
*   **Steer:** Swipe anywhere inside the screen border on mobile, or tap the responsive on-screen D-pad. On desktop, steer using **WASD** or **Arrow Keys**.
*   **Grow & Score:** Eat normal food to grow. Collect coins to trigger a double power-up (Ghost + Magnet) and stack consecutive combos for exponential score multipliers.
*   **Gear Icon (⚙️):** Tap the physical button at the top-left to pull up the settings tray (basic options sit on top; performance controls reside under *Advanced Settings*).
*   **Play/Pause (▶ / ❚❚):** Real-time pause state handling that suspends game updates while preserving rendering and animation threads.

---

### 🔧 Hacking Guide (Quick-Win Customizations)
Because the entire engine is written in a single file and clearly segmented, you can open `index.html` in any text editor and easily customize the game. Search for `SECTION:` to jump directly to any functional subsystem.

| What you want to do | Where to look | How to do it |
| :--- | :--- | :--- |
| **Add a visual theme** | `SECTION: CONFIG & CONSTANTS` | Push a new theme object onto the `gameThemes` array. |
| **Create a custom snake skin** | `SECTION: CONFIG & CONSTANTS` | Add a new configuration onto the `snakeSkins` array. |
| **Resize the grid board** | `SECTION: CONFIG & CONSTANTS` | Modify the `gridSize`, `tileCountX`, or `tileCountY` constants. |
| **Change point values** | `SECTION: CONFIG & CONSTANTS` | Tweak the speeds and values inside the `difficultySettings` structure. |
| **Modify sound pitches** | `SECTION: AUDIO ENGINE` | Edit the frequency maps inside the `play*Sound()` helper functions. |

---

# ⚙️ PART II: THE SYSTEMS ARCHITECT SPECIFICATION
*An uncompromising technical breakdown of the performance optimizations, mathematical safeguards, and thread-friendly rendering architecture running under the hood.*

### 1. The Fixed-Timestep Game Loop
Most amateur web games tie their game update ticks directly to the browser's render rate (`requestAnimationFrame`). This causes a severe gameplay bug: the snake moves at light-speed on 144Hz desktop monitors and crawls sluggishly on 60Hz mobile screens.

Retrosnake solves this by utilizing an accumulator-based delta loop. Physics updates are locked to a strict millisecond interval (`currentSnakeSpeed`), while frames are drawn dynamically according to the screen's actual refresh rate.

```javascript
// Fixed-Timestep Physics Loop inside requestAnimationFrame
gameUpdateAccumulator += deltaTime;
while (gameUpdateAccumulator >= currentSnakeSpeed) {
    updateGameLogic();
    gameUpdateAccumulator -= currentSnakeSpeed;
    if (isGameOver) break;
}
```

---

### 2. Offscreen Canvas Caching (`blockImageCache`)
Drawing complex visual effects like 3D beveled blocks, multi-stop linear gradients, and neon shadows inside the main canvas animation cycle is incredibly heavy. On low-end mobile devices, redrawing these elements 60 to 90 times per second directly causes frame drops and high battery drain.

Retrosnake implements a dynamic offscreen cache. The first time a styled block of a specific color is rendered, the engine compiles its shadows, bevels, and borders onto an in-memory `tempCanvas` once. Every subsequent frame bypasses active drawing math completely and performs a lightning-fast, GPU-accelerated blit of the pre-cached canvas block.

```javascript
// Render Cache Check & pre-draw allocation
if (fillColor && blockImageCache[fillColor]) {
    targetCtx.drawImage(blockImageCache[fillColor], blockX, blockY);
    return;
}
```

---

### 3. Smashed Retina/High-DPI Scaling Bug
Modern mobile devices utilize high Device Pixel Ratios (DPR > 1) to keep text and shapes sharp. If you scale a Canvas backing store to match the physical pixel density but blit an offscreen canvas without destination constraints, the browser renders physical pixels 1:1, resulting in a giant, zoomed-in, off-center snake on Retina displays.

This engine completely squashes this bug by decoupling the backing store scaling from the final composite pass. The logical game bounds are explicitly enforced on the draw call, instructing the browser's GPU to scale the high-DPI backing canvas down cleanly to the screen boundaries.

```javascript
// Scale constraint blitting
ctx.drawImage(snakeCanvas, 0, 0, tileCountX * gridSize, tileCountY * gridSize);
```

---

### 4. Non-Blocking Mobile Touch & Passive Listeners
To achieve an immediate, console-grade feedback loop on touch screens without chugging the main thread, the engine incorporates two distinct mobile input optimizations:
1. **Passive Listeners:** The touch hooks register with `{ passive: true }`. This explicitly signals to the mobile browser's layout engine that the swipe interactions will not block scrolling, completely eliminating the 100-300ms touch-responsiveness delay.
2. **Single-Trigger Swiping:** Touchmove listener locks direction adjustments with a strict boolean check (`__swipeDone`). The engine fires exactly *one* direction update the instant displacement crosses a 24px threshold, ignoring the rest of the finger-dragging stream until the touch is lifted. This shields the main thread from redundant event processing.

```javascript
swipeTarget.addEventListener('touchmove', (e) => {
    if (!swipeEnabled || __swipeDone) return;
    const t = e.changedTouches[0];
    const ddx = t.clientX - __swipeStartX;
    const ddy = t.clientY - __swipeStartY;
    
    if (Math.abs(ddx) < SWIPE_THRESHOLD && Math.abs(ddy) < SWIPE_THRESHOLD) return;
    
    if (Math.abs(ddx) > Math.abs(ddy)) {
        queueDirectionChange(ddx > 0 ? 1 : -1, 0);
    } else {
        queueDirectionChange(0, ddy > 0 ? 1 : -1);
    }
    __swipeDone = true; // Locks swipe process until next touchstart
}, { passive: true });
```

---

### 5. Buffered D-Pad Command Queue
To support hyper-fast, tactical arcade plays without input dropping or accidental self-destruction (such as immediate 180-degree self-collisions), Retrosnake implements a custom command buffer queue with a length limit of 2. It tracks inputs independently of the physics ticks, ensuring swift, sequential inputs are queued and executed exactly one valid step at a time.

```javascript
// Buffered Input Queue handling
const lastMove = inputQueue.length > 0 ? inputQueue[inputQueue.length - 1] : { dx: dx, dy: dy };
const isReverseHorizontal = lastMove.dx !== 0 && newDx === -lastMove.dx;
const isReverseVertical = lastMove.dy !== 0 && newDy === -lastMove.dy;

if (!isReverseHorizontal && !isReverseVertical) {
    inputQueue.push({ dx: newDx, dy: newDy });
}
```

---

### 6. Procedural Web Audio Synthesizer
Rather than loading, caching, and licensing heavy MP3 or WAV files, Retrosnake features a fully integrated Web Audio API sound generator. Sounds are synthesized programmatically on the fly from raw audio nodes, generating clean, nostalgic 8-bit soundscapes with **absolutely zero network payload.**

To prevent harsh speaker clicking and deliver organic sound envelopes, the generator utilizes exponential curves to shape pitch glides, volume attacks, and decays.

```javascript
// Low-level Web Audio Oscillator synthesis
function __sfxTone(freq, freqEnd, type, dur, vol, delay) {
    const ctx = getAudioCtx();
    if (!ctx) return;
    const t0 = ctx.currentTime + (delay || 0);
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    
    osc.type = type || 'square';
    osc.frequency.setValueAtTime(freq, t0);
    if (freqEnd && freqEnd !== freq) {
        osc.frequency.exponentialRampToValueAtTime(Math.max(1, freqEnd), t0 + dur);
    }
    
    gain.gain.setValueAtTime(0.0001, t0);
    gain.gain.exponentialRampToValueAtTime(vol, t0 + 0.006); // Linear-styled sharp attack
    gain.gain.exponentialRampToValueAtTime(0.0001, t0 + dur);  // Exponential smooth decay
    
    osc.connect(gain);
    gain.connect(ctx.destination);
    osc.start(t0);
    osc.stop(t0 + dur + 0.03);
}
```

---

### 7. Safe Modulo Coordinate Wrapping
Wrapping coordinates in wrap-less (wall-off) modes can easily cause index crashes when the snake glides off the left or top boundaries, as standard JavaScript modulo operations return negative values on negative dividends (e.g., `-1 % 20` resolves to `-1` instead of wrapping back to `19`).

Retrosnake hardens its wrapping math to be entirely boundary-safe. Adding grid bounds prior to calculating modulo ensures wrapped coordinates remain strictly positive integers under all movement circumstances.

```javascript
// Boundary-Safe Modulo coordinate wrapper
__hx = (__hx + tileCountX) % tileCountX;
__hy = (__hy + tileCountY) % tileCountY;
```

---

# 🤖 PART III: THE AI COLLABORATION BLUEPRINT
*How to design code to act as a flawless blueprint for artificial intelligence.*

Retrosnake was built under a modern development philosophy: **writing code that is as legible to AI LLMs as it is to human compilers.** 

To prevent AI assistants from breaking critical subsystems during collaborative refactoring sessions, this file features a strict semantic system:
1. **Plain-English Inline Warnings:** Critical segments (like high-DPI scaling constraints and the delta loop logic) are marked with detailed warning comments. This signals to AI models to maintain those exact parameters during code revisions.
2. **Modular Architecture Maps:** The header comment of `index.html` details a complete map of the monolithic file. This provides instant structural hierarchy to a language model's attention window, preventing it from generating duplicate helper functions or redundant namespaces.
3. **Strict Constants Separation:** Environmental parameters (like tick speeds, physics constants, and theme arrays) are isolated from execution states, allowing AI code models to easily adjust or add features without introducing logical regressions.

---

## 📜 License
Released under the **MIT License**—completely open source. You can study it, hack on it, build on top of it, or sell it commercially. See the `LICENSE` file for full terms.

##### 🤝 Credits & Acknowledgments
Created by **the Digital Spellcaster**—human design, direction, and structural vision.

The engine core, optimization sweeps, and thirteen months of deep-trench coding were developed hand-in-hand with **Gemini**. **Claude (Anthropic)** contributed to the final aesthetic visual polish. Every single decision was directed by a human; every block of code was forged through collaborative AI programming. This codebase stands as a testament to what a human-AI partnership can achieve.

*And a very special thank you to my Mom—the one who started this loop by giving me the tools. This one is for you. ❤*
