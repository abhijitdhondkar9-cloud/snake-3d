# Tutorial: build 3D Snake step by step

This follows the video, chapter by chapter. Each step adds one piece to `main.js` (or `index.html`), and the game works a little more after each one. The finished files are in this repo.

---

## Step 0: The tools

- **VS Code**: the editor.
- **Live Server**: a VS Code extension that runs a local web server and reloads the page whenever you save.
- **Node.js**: not needed by this game, but useful for most web projects.

Make a folder called `snake-3d` and open it in VS Code (**File → Open Folder…**).

---

## Step 1: `index.html` loads Three.js

Create `index.html`. The important part is the **import map**: it tells the browser where to find the module called `"three"`, so `main.js` can write `import * as THREE from "three"`.

```html
<script type="importmap">{ "imports": {
  "three": "https://esm.sh/three@0.160.0"
} }</script>
<script type="module" src="main.js"></script>
```

The import map must come **before** the module script. The finished `index.html` also has styles for the score, the hint line and the menu.

---

## Step 2: Scene, camera and board

Every Three.js app needs three things:

1. a **scene**, which holds everything;
2. a **camera**, which looks at it (`PerspectiveCamera(50, aspect)` means a 50° field of view);
3. a **renderer**, which draws the camera's view into a `<canvas>`.

```js
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(50, innerWidth / innerHeight, 0.1, 100);
camera.position.set(0, 14, 12);
camera.lookAt(0, 0, 0);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(innerWidth, innerHeight);
document.body.append(renderer.domElement);
```

The board is a flat box: a **Mesh** made of a **geometry** (the shape) and a **material** (how it looks). `MeshStandardMaterial` reacts to light, so we add a `HemisphereLight` and a `DirectionalLight`, or it would render black. A `GridHelper` draws the cell lines.

---

## Step 3: Snake cubes and the ball

The snake is plain data: a list of cells, head first.

```js
let snake = [{ x: -1, z: 0 }, { x: -2, z: 0 }, { x: -3, z: 0 }];
```

Each cell gets a cube mesh. `syncMeshes()` keeps the number of cubes equal to the snake's length and moves each cube to its cell. The ball is a `SphereGeometry`, and `placeFood()` puts it on a random free cell.

> **Tip:** keep game logic (the list of cells) separate from drawing (the meshes). The logic stays simple, and the drawing just follows it.

---

## Step 4: Move, catch, grow

A timer moves the snake one cell per tick:

```js
const h = { x: snake[0].x + dir.x, z: snake[0].z + dir.z };
snake.unshift(h);                       // new head
if (h.x === food.x && h.z === food.z) {
  score++;                              // caught it: keep the tail, so the snake grows
  placeFood();
} else {
  snake.pop();                          // no catch: drop the tail
}
```

The arrow keys set `nextDir`, and a key that would turn the snake straight back into itself is ignored. `renderer.setAnimationLoop` redraws about 60 times a second, separately from the slower game tick.

---

## Step 5: Three camera views

Pressing **C** cycles through a list of views:

- **Angle:** a fixed position above and in front of the board.
- **Top:** straight down.
- **Follow:** behind the snake's head, moving with it.

Each frame, the camera moves a little toward its target with `camera.position.lerp(target, 0.08)`, so view changes glide instead of jumping.

---

## Step 6: Play options

The menu is built in JavaScript and handled with **one** click listener (event delegation):

- **Speed:** Easy, Normal or Hard sets the tick time (220, 150 or 90 ms).
- **Walls:** On means the edges end the game. Off makes the snake wrap round to the other side.
- **Play:** `start()` resets the snake, score and timer, and hides the menu.

The keyboard adds **Space** to pause (the tick returns early while `paused` is true) and **R** to restart.

---

## Step 7: Game over and restart

Before the new head is added, `hits(h)` checks two things:

1. **Wall:** the head is off the board, and walls are on.
2. **Tail:** the head lands on the snake's own body. The last cell doesn't count, because the tail moves away this same step.

If either is true, `gameOver()` stops the timer and shows the menu again with "Game over: score" and a **Play again** button.

---

## Where next?

See **Practice ideas** in the [README](README.md): high score, speed-up, obstacles, a golden ball, touch controls and sound.
