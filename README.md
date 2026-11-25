# 3D Snake: Catch the Ball

A 3D Snake game built with **Three.js**, written step by step in **VS Code**. Steer the snake, catch the orange ball, grow longer, and don't hit the wall or your own tail.

This repo is the source code for the CorePulse video **"Build a 3D Snake Game with Three.js in VS Code"**.

▶️ **Watch the video:** `https://youtube.com/@CorePulseOffical` *(replace with the video link once it's live)*

![The 3D Snake board with the options menu](preview.png)

---

## Features

- 🧊 **Real 3D:** a Three.js scene with a board, a snake made of cubes, a glowing ball, and lights
- 🎮 **Play options menu:** pick the speed (Easy, Normal, Hard) and turn walls on or off
- 🎥 **Three camera views:** Angle, Top and Follow, switched with the **C** key
- ⏸️ **Pause and restart:** **Space** pauses, **R** starts a new game
- 💥 **Game over:** hitting a wall (when walls are on) or your own tail ends the game and shows your score
- 🌀 **Walls off:** the snake wraps from one edge to the other
- ⚙️ **No build step:** two files, with Three.js loaded through an import map

---

## Controls

| Key | Action |
| --- | --- |
| ← ↑ → ↓ or W A S D | Steer the snake |
| Space | Pause / resume |
| R | Restart |
| C | Switch camera: Angle → Top → Follow |

---

## Quick start

### 1. Install the tools

1. **Node.js** from [nodejs.org](https://nodejs.org) (LTS version). Live Server doesn't need it, but you'll want it for most web projects.
2. **VS Code** from [code.visualstudio.com](https://code.visualstudio.com).
3. In VS Code, open **Extensions** (`Ctrl+Shift+X`), search for **Live Server** (by Ritwick Dey) and click **Install**.

### 2. Get the code

Download the repo (**Code → Download ZIP**) and unzip it, or clone it:

```bash
git clone https://github.com/YOUR-USERNAME/snake-3d.git
```

### 3. Run it

1. In VS Code: **File → Open Folder…** and choose `snake-3d`.
2. Right-click `index.html` and choose **Open with Live Server**, or click **Go Live** in the bottom bar.
3. The game opens at `http://127.0.0.1:5500`. Pick your options and press **Play**.

> **Why Live Server?** Browsers block JavaScript modules opened straight from a file (`file://`), so double-clicking `index.html` shows a blank page. Any local web server works: Live Server is just the easiest.

---

## Project structure

```text
snake-3d/
├── index.html    # page, styles, score display, and the import map that loads Three.js
├── main.js       # the whole game: scene, snake, ball, controls, options, cameras, game over
├── preview.png   # screenshot used in this README
├── TUTORIAL.md   # the video's steps, explained one by one
└── LICENSE
```

---

## How it works

The game runs on a **grid**: the board has 12 × 12 cells, from −6 to 5 on each axis. The snake is a list of cells, head first.

- **Every tick** (`setInterval`, faster on Hard), a new head is added one cell ahead.
  - If the head lands on the ball, the score goes up and the tail stays, so the snake grows by one.
  - Otherwise the tail is removed, so the snake moves without growing.
- **Every frame** (`renderer.setAnimationLoop`), the cubes are moved to their cells, the ball bobs, the camera glides toward the chosen view, and the scene is drawn.

The step-by-step walkthrough, matching the video's chapters, is in **[TUTORIAL.md](TUTORIAL.md)**.

---

## Make it yours

| What | Where in `main.js` |
| --- | --- |
| Colors of board, snake, ball | `color: 0x3b5bdb`, `0x1b1f2a`, `0xf08c00` |
| Speeds (milliseconds per step) | `const speeds = { Easy: 220, Normal: 150, Hard: 90 }` |
| Board size | `MIN`, `MAX` and the `BoxGeometry(12, 0.5, 12)` / `GridHelper(12, 12)` sizes |
| Camera positions | the `views` list |

---

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Blank page | Open it with Live Server (or any local server), not by double-clicking the file. |
| "Failed to resolve module specifier 'three'" | The import map in `index.html` must come **before** the `main.js` script tag. |
| Nothing loads with no internet | Three.js comes from `esm.sh`. To work offline, run `npm install three` and point the import map at `./node_modules/three/build/three.module.js`. |
| Arrow keys scroll the page | Click the game once so it has focus. |

---

## Practice ideas

1. Add a **high score** saved with `localStorage`.
2. Make the snake **speed up** a little after every catch.
3. Add **obstacles**: a few blocks on the board that end the game.
4. Add a **golden ball** worth 3 points that disappears after 5 seconds.
5. Add **touch controls** (swipe) for phones.
6. Add **sound effects** with the Web Audio API.

Built something? Share it in the video's comments.

---

## License

MIT. Use this code freely in your own projects.

**CorePulse:** trading tech skills and updates. Subscribe on YouTube: **@CorePulseOffical**
