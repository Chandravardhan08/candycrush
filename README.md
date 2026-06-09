<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Sugar Switch</title>
    <style>
:root {
  color-scheme: light;
  --ink: #241733;
  --muted: #746982;
  --panel: rgba(255, 255, 255, 0.84);
  --line: rgba(36, 23, 51, 0.12);
  --shadow: 0 22px 70px rgba(53, 26, 83, 0.2);
  --pink: #ff4f9a;
  --blue: #36a7ff;
  --green: #2fcf7a;
  --yellow: #ffd84d;
  --violet: #8e66ff;
  --red: #ff695f;
}

* {
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  margin: 0;
  overflow-x: hidden;
  color: var(--ink);
  font-family:
    Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI",
    sans-serif;
  background:
    radial-gradient(circle at 15% 10%, rgba(255, 221, 79, 0.42), transparent 26rem),
    radial-gradient(circle at 85% 20%, rgba(54, 167, 255, 0.28), transparent 24rem),
    linear-gradient(135deg, #fff4f8 0%, #edf8ff 47%, #fffbe9 100%);
}

button {
  border: 0;
  color: inherit;
  font: inherit;
  cursor: pointer;
}

.app {
  display: grid;
  grid-template-columns: minmax(320px, 680px) minmax(260px, 340px);
  gap: 22px;
  align-items: start;
  width: min(1120px, calc(100vw - 32px));
  margin: 0 auto;
  padding: 28px 0;
}

.play-area,
.side-panel {
  border: 1px solid var(--line);
  background: var(--panel);
  box-shadow: var(--shadow);
  backdrop-filter: blur(18px);
}

.play-area {
  min-width: 0;
  padding: 18px;
  border-radius: 8px;
}

.topbar {
  display: flex;
  gap: 16px;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 16px;
}

.kicker {
  margin: 0 0 4px;
  color: #c12f6d;
  font-size: 0.78rem;
  font-weight: 800;
  letter-spacing: 0;
  text-transform: uppercase;
}

h1,
h2,
p {
  margin-top: 0;
}

h1 {
  margin-bottom: 0;
  font-size: clamp(1.65rem, 2.2rem, 2.2rem);
  line-height: 1.02;
  letter-spacing: 0;
}

h2 {
  margin-bottom: 12px;
  font-size: 1rem;
}

.icon-text-button {
  display: inline-flex;
  gap: 8px;
  align-items: center;
  justify-content: center;
  min-height: 42px;
  padding: 0 14px;
  border-radius: 8px;
  color: #fff;
  font-weight: 800;
  background: #241733;
  box-shadow: 0 10px 24px rgba(36, 23, 51, 0.2);
}

.stats {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 10px;
  margin-bottom: 16px;
}

.stats article {
  min-width: 0;
  padding: 12px;
  border: 1px solid var(--line);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.62);
}

.stats span,
.meter-label span,
.booster small {
  display: block;
  color: var(--muted);
  font-size: 0.76rem;
  font-weight: 800;
}

.stats strong {
  display: block;
  margin-top: 3px;
  font-size: clamp(1rem, 1.35rem, 1.35rem);
}

.board-wrap {
  position: relative;
}

.board {
  --cell: min(9.3vw, 70px);
  display: grid;
  grid-template-columns: repeat(8, var(--cell));
  grid-template-rows: repeat(8, var(--cell));
  gap: 7px;
  justify-content: center;
  width: fit-content;
  max-width: 100%;
  margin: 0 auto;
  padding: 10px;
  border: 1px solid rgba(255, 255, 255, 0.8);
  border-radius: 8px;
  background:
    linear-gradient(45deg, rgba(255, 255, 255, 0.54) 25%, transparent 25% 75%, rgba(255, 255, 255, 0.54) 75%),
    linear-gradient(45deg, rgba(255, 255, 255, 0.54) 25%, transparent 25% 75%, rgba(255, 255, 255, 0.54) 75%),
    rgba(52, 30, 70, 0.14);
  background-position:
    0 0,
    16px 16px;
  background-size: 32px 32px;
}

.cell {
  position: relative;
  display: grid;
  place-items: center;
  width: var(--cell);
  height: var(--cell);
  border-radius: 8px;
  background:
    radial-gradient(circle at 35% 28%, rgba(255, 255, 255, 0.85), transparent 28%),
    linear-gradient(145deg, rgba(255, 255, 255, 0.8), rgba(255, 160, 212, 0.26));
  box-shadow:
    inset 0 0 0 2px rgba(255, 255, 255, 0.42),
    inset 0 -8px 14px rgba(143, 48, 119, 0.12),
    0 5px 10px rgba(63, 30, 89, 0.12);
  transition:
    transform 160ms ease,
    background 160ms ease;
}

.cell:focus-visible {
  outline: 3px solid #241733;
  outline-offset: 2px;
}

.cell.selected {
  transform: scale(0.94);
  background: rgba(36, 23, 51, 0.14);
}

.candy {
  position: relative;
  width: 78%;
  height: 78%;
  overflow: hidden;
  box-shadow:
    inset -10px -12px 18px rgba(60, 16, 45, 0.22),
    inset 8px 9px 15px rgba(255, 255, 255, 0.55),
    0 9px 13px rgba(36, 23, 51, 0.22);
  transform: rotate(-12deg);
}

.candy::before {
  content: "";
  position: absolute;
  inset: 13% 16% auto auto;
  z-index: 2;
  width: 34%;
  height: 20%;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.82);
  filter: blur(0.4px);
}

.candy::after {
  content: "";
  position: absolute;
  inset: 0;
  z-index: 1;
  background:
    linear-gradient(120deg, transparent 0 32%, rgba(255, 255, 255, 0.4) 33% 42%, transparent 43% 100%),
    radial-gradient(circle at 22% 80%, rgba(255, 255, 255, 0.24), transparent 26%);
  mix-blend-mode: screen;
}

.candy-0 {
  border-radius: 50%;
  background:
    repeating-linear-gradient(45deg, rgba(255, 255, 255, 0.32) 0 8px, transparent 8px 17px),
    radial-gradient(circle at 28% 25%, #ffb9dc, #ff4f9a 45%, #c91864 100%);
}

.candy-1 {
  border-radius: 45% 55% 48% 52%;
  background:
    radial-gradient(circle at 30% 24%, #b8ecff, #36a7ff 48%, #126bc8 100%);
}

.candy-2 {
  border-radius: 50% 50% 22% 22%;
  background:
    repeating-linear-gradient(90deg, transparent 0 9px, rgba(255, 255, 255, 0.34) 9px 15px),
    radial-gradient(circle at 35% 22%, #baffd4, #2fcf7a 48%, #0b8d4c 100%);
}

.candy-3 {
  border-radius: 24%;
  background:
    radial-gradient(circle at 25% 22%, #fff7b0, #ffd84d 45%, #e88b08 100%);
  transform: rotate(32deg);
}

.candy-4 {
  border-radius: 62% 38% 62% 38%;
  background:
    repeating-linear-gradient(-35deg, rgba(255, 255, 255, 0.34) 0 7px, transparent 7px 16px),
    radial-gradient(circle at 28% 22%, #d9c8ff, #8e66ff 46%, #5630c8 100%);
}

.candy-5 {
  border-radius: 50% 18% 50% 18%;
  background:
    radial-gradient(circle at 28% 25%, #ffc0bb, #ff695f 45%, #c92631 100%);
  transform: rotate(18deg);
}

.cell.clearing .candy {
  animation: pop 260ms ease forwards;
}

@keyframes pop {
  to {
    opacity: 0;
    transform: scale(1.35) rotate(18deg);
  }
}

.toast {
  position: absolute;
  left: 50%;
  top: 50%;
  z-index: 3;
  min-width: min(280px, 78vw);
  padding: 16px 18px;
  border-radius: 8px;
  color: #fff;
  font-weight: 900;
  text-align: center;
  pointer-events: none;
  background: rgba(36, 23, 51, 0.92);
  box-shadow: 0 18px 42px rgba(36, 23, 51, 0.28);
  opacity: 0;
  transform: translate(-50%, -45%) scale(0.96);
  transition:
    opacity 180ms ease,
    transform 180ms ease;
}

.toast.show {
  opacity: 1;
  transform: translate(-50%, -50%) scale(1);
}

.side-panel {
  display: grid;
  gap: 14px;
  padding: 16px;
  border-radius: 8px;
}

.panel-block {
  padding: 14px;
  border: 1px solid var(--line);
  border-radius: 8px;
  background: rgba(255, 255, 255, 0.62);
}

.panel-block.compact p {
  margin-bottom: 0;
  color: var(--muted);
  line-height: 1.45;
}

.meter-label {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 9px;
}

.meter {
  overflow: hidden;
  height: 14px;
  border-radius: 999px;
  background: rgba(36, 23, 51, 0.12);
}

.meter div {
  width: 0%;
  height: 100%;
  border-radius: inherit;
  background: linear-gradient(90deg, #2fcf7a, #ffd84d, #ff4f9a);
  transition: width 260ms ease;
}

.boosters {
  display: grid;
  gap: 10px;
}

.booster {
  display: grid;
  grid-template-columns: 38px 1fr;
  grid-template-areas:
    "icon title"
    "icon count";
  gap: 1px 10px;
  align-items: center;
  min-height: 62px;
  padding: 10px;
  border: 1px solid var(--line);
  border-radius: 8px;
  text-align: left;
  background: #fff;
}

.booster span {
  grid-area: icon;
  display: grid;
  place-items: center;
  width: 38px;
  height: 38px;
  border-radius: 8px;
  color: #fff;
  font-size: 1.2rem;
  font-weight: 900;
  background: #241733;
}

.booster strong {
  grid-area: title;
  font-size: 0.95rem;
}

.booster small {
  grid-area: count;
}

.booster:disabled,
.icon-text-button:disabled {
  cursor: not-allowed;
  opacity: 0.48;
}

@media (max-width: 860px) {
  .app {
    grid-template-columns: 1fr;
    width: min(680px, calc(100vw - 24px));
    padding: 12px 0;
  }

  .side-panel {
    grid-template-columns: 1fr 1fr;
  }
}

@media (max-width: 560px) {
  .play-area {
    padding: 12px;
  }

  .topbar {
    align-items: flex-start;
  }

  .stats {
    grid-template-columns: repeat(2, 1fr);
  }

  .board {
    --cell: min(10.3vw, 52px);
    gap: 5px;
    padding: 7px;
  }

  .side-panel {
    grid-template-columns: 1fr;
  }
}

    </style>
  </head>
  <body>
    <main class="app">
      <section class="play-area" aria-label="Sugar Switch match three game">
        <header class="topbar">
          <div>
            <p class="kicker">Sugar Switch Saga</p>
            <h1>Clear the jelly.</h1>
          </div>
          <button id="newGame" class="icon-text-button" type="button" aria-label="Start a new game">
            <span aria-hidden="true">â†»</span>
            New
          </button>
        </header>

        <section class="stats" aria-label="Game stats">
          <article>
            <span>Score</span>
            <strong id="score">0</strong>
          </article>
          <article>
            <span>Target</span>
            <strong id="target">2500</strong>
          </article>
          <article>
            <span>Moves</span>
            <strong id="moves">24</strong>
          </article>
          <article>
            <span>Level</span>
            <strong id="level">1</strong>
          </article>
        </section>

        <div class="board-wrap">
          <div id="board" class="board" role="grid" aria-label="Candy board"></div>
          <div id="toast" class="toast" role="status" aria-live="polite"></div>
        </div>
      </section>

      <aside class="side-panel" aria-label="Tools and progress">
        <section class="panel-block">
          <div class="meter-label">
            <span>Target progress</span>
            <strong id="progressText">0%</strong>
          </div>
          <div class="meter" aria-hidden="true">
            <div id="progressFill"></div>
          </div>
        </section>

        <section class="panel-block">
          <h2>Boosters</h2>
          <div class="boosters">
            <button id="shuffle" type="button" class="booster">
              <span aria-hidden="true">S</span>
              <strong>Shuffle</strong>
              <small id="shuffleCount">2 left</small>
            </button>
            <button id="hammer" type="button" class="booster">
              <span aria-hidden="true">*</span>
              <strong>Pop</strong>
              <small id="hammerCount">3 left</small>
            </button>
          </div>
        </section>

        <section class="panel-block compact">
          <h2>Goal</h2>
          <p id="goalText">Score 2500 points before the moves run out.</p>
        </section>
      </aside>
    </main>

    <script>
const size = 8;
const types = 6;
const baseMoves = 24;
const baseTarget = 2500;
const boardEl = document.querySelector("#board");
const scoreEl = document.querySelector("#score");
const targetEl = document.querySelector("#target");
const movesEl = document.querySelector("#moves");
const levelEl = document.querySelector("#level");
const progressFill = document.querySelector("#progressFill");
const progressText = document.querySelector("#progressText");
const goalText = document.querySelector("#goalText");
const toast = document.querySelector("#toast");
const newGameButton = document.querySelector("#newGame");
const shuffleButton = document.querySelector("#shuffle");
const hammerButton = document.querySelector("#hammer");
const shuffleCountEl = document.querySelector("#shuffleCount");
const hammerCountEl = document.querySelector("#hammerCount");

let board = [];
let selected = null;
let score = 0;
let moves = baseMoves;
let level = 1;
let target = baseTarget;
let busy = false;
let gameOver = false;
let shuffleCount = 2;
let hammerCount = 3;
let hammerMode = false;
let toastTimer = null;

function randomCandy() {
  return Math.floor(Math.random() * types);
}

function index(row, col) {
  return row * size + col;
}

function coords(i) {
  return {
    row: Math.floor(i / size),
    col: i % size,
  };
}

function areAdjacent(a, b) {
  const ca = coords(a);
  const cb = coords(b);
  return Math.abs(ca.row - cb.row) + Math.abs(ca.col - cb.col) === 1;
}

function createBoard() {
  board = Array.from({ length: size }, () => Array(size).fill(0));

  for (let row = 0; row < size; row += 1) {
    for (let col = 0; col < size; col += 1) {
      let candy = randomCandy();
      while (
        (col >= 2 && board[row][col - 1] === candy && board[row][col - 2] === candy) ||
        (row >= 2 && board[row - 1][col] === candy && board[row - 2][col] === candy)
      ) {
        candy = randomCandy();
      }
      board[row][col] = candy;
    }
  }

  if (!hasPossibleMove()) {
    createBoard();
  }
}

function renderBoard(clearing = new Set()) {
  boardEl.innerHTML = "";
  board.forEach((row, rowIndex) => {
    row.forEach((candy, colIndex) => {
      const cellIndex = index(rowIndex, colIndex);
      const cell = document.createElement("button");
      cell.className = "cell";
      cell.type = "button";
      cell.setAttribute("role", "gridcell");
      cell.setAttribute("aria-label", `Candy at row ${rowIndex + 1}, column ${colIndex + 1}`);
      cell.dataset.index = String(cellIndex);

      if (selected === cellIndex) {
        cell.classList.add("selected");
      }
      if (clearing.has(cellIndex)) {
        cell.classList.add("clearing");
      }

      if (candy !== null) {
        const candyEl = document.createElement("span");
        candyEl.className = `candy candy-${candy}`;
        candyEl.setAttribute("aria-hidden", "true");
        cell.appendChild(candyEl);
      }

      cell.addEventListener("click", () => handleCellClick(cellIndex));
      boardEl.appendChild(cell);
    });
  });
}

function updateHud() {
  const progress = Math.min(100, Math.round((score / target) * 100));
  scoreEl.textContent = String(score);
  targetEl.textContent = String(target);
  movesEl.textContent = String(moves);
  levelEl.textContent = String(level);
  progressFill.style.width = `${progress}%`;
  progressText.textContent = `${progress}%`;
  goalText.textContent = `Score ${target} points before the moves run out.`;
  shuffleCountEl.textContent = `${shuffleCount} left`;
  hammerCountEl.textContent = `${hammerCount} left`;
  shuffleButton.disabled = busy || gameOver || shuffleCount <= 0;
  hammerButton.disabled = busy || gameOver || hammerCount <= 0;
}

function showToast(message, duration = 1200) {
  clearTimeout(toastTimer);
  toast.textContent = message;
  toast.classList.add("show");
  toastTimer = setTimeout(() => toast.classList.remove("show"), duration);
}

function swap(a, b) {
  const ca = coords(a);
  const cb = coords(b);
  [board[ca.row][ca.col], board[cb.row][cb.col]] = [board[cb.row][cb.col], board[ca.row][ca.col]];
}

function findMatches() {
  const matches = new Set();

  for (let row = 0; row < size; row += 1) {
    let runStart = 0;
    for (let col = 1; col <= size; col += 1) {
      const same = col < size && board[row][col] === board[row][runStart] && board[row][col] !== null;
      if (!same) {
        if (col - runStart >= 3) {
          for (let c = runStart; c < col; c += 1) {
            matches.add(index(row, c));
          }
        }
        runStart = col;
      }
    }
  }

  for (let col = 0; col < size; col += 1) {
    let runStart = 0;
    for (let row = 1; row <= size; row += 1) {
      const same = row < size && board[row][col] === board[runStart][col] && board[row][col] !== null;
      if (!same) {
        if (row - runStart >= 3) {
          for (let r = runStart; r < row; r += 1) {
            matches.add(index(r, col));
          }
        }
        runStart = row;
      }
    }
  }

  return matches;
}

function clearMatches(matches) {
  matches.forEach((cellIndex) => {
    const { row, col } = coords(cellIndex);
    board[row][col] = null;
  });
}

function dropCandies() {
  for (let col = 0; col < size; col += 1) {
    const column = [];
    for (let row = size - 1; row >= 0; row -= 1) {
      if (board[row][col] !== null) {
        column.push(board[row][col]);
      }
    }
    while (column.length < size) {
      column.push(randomCandy());
    }
    for (let row = size - 1; row >= 0; row -= 1) {
      board[row][col] = column[size - 1 - row];
    }
  }
}

function sleep(ms) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}

async function resolveBoard() {
  let chain = 0;
  let matches = findMatches();

  while (matches.size > 0) {
    chain += 1;
    score += matches.size * 80 * chain;
    renderBoard(matches);
    updateHud();
    await sleep(280);
    clearMatches(matches);
    dropCandies();
    renderBoard();
    await sleep(170);
    matches = findMatches();
  }

  if (!hasPossibleMove() && !gameOver) {
    shuffleBoard(false);
    showToast("Board shuffled");
  }

  checkGameState();
}

async function trySwap(first, second) {
  if (!areAdjacent(first, second) || busy || gameOver) {
    selected = second;
    renderBoard();
    return;
  }

  busy = true;
  swap(first, second);
  selected = null;
  renderBoard();
  await sleep(120);

  if (findMatches().size === 0) {
    swap(first, second);
    renderBoard();
    showToast("Try another swap");
    busy = false;
    updateHud();
    return;
  }

  moves -= 1;
  await resolveBoard();
  busy = false;
  updateHud();
}

function handleCellClick(cellIndex) {
  if (busy || gameOver) {
    return;
  }

  if (hammerMode) {
    useHammer(cellIndex);
    return;
  }

  if (selected === null) {
    selected = cellIndex;
    renderBoard();
    return;
  }

  if (selected === cellIndex) {
    selected = null;
    renderBoard();
    return;
  }

  trySwap(selected, cellIndex);
}

function hasPossibleMove() {
  for (let row = 0; row < size; row += 1) {
    for (let col = 0; col < size; col += 1) {
      const current = index(row, col);
      const neighbors = [];
      if (col + 1 < size) neighbors.push(index(row, col + 1));
      if (row + 1 < size) neighbors.push(index(row + 1, col));

      for (const neighbor of neighbors) {
        swap(current, neighbor);
        const works = findMatches().size > 0;
        swap(current, neighbor);
        if (works) {
          return true;
        }
      }
    }
  }
  return false;
}

function shuffleBoard(costsBooster = true) {
  if (busy || gameOver || (costsBooster && shuffleCount <= 0)) {
    return;
  }

  if (costsBooster) {
    shuffleCount -= 1;
  }

  const candies = board.flat().sort(() => Math.random() - 0.5);
  board = Array.from({ length: size }, (_, row) =>
    candies.slice(row * size, row * size + size)
  );

  let guard = 0;
  while ((findMatches().size > 0 || !hasPossibleMove()) && guard < 60) {
    candies.sort(() => Math.random() - 0.5);
    board = Array.from({ length: size }, (_, row) =>
      candies.slice(row * size, row * size + size)
    );
    guard += 1;
  }

  renderBoard();
  updateHud();
}

async function useHammer(cellIndex) {
  if (hammerCount <= 0 || busy || gameOver) {
    return;
  }

  busy = true;
  hammerMode = false;
  hammerCount -= 1;
  const { row, col } = coords(cellIndex);
  const targets = new Set([cellIndex]);

  if (row > 0) targets.add(index(row - 1, col));
  if (row < size - 1) targets.add(index(row + 1, col));
  if (col > 0) targets.add(index(row, col - 1));
  if (col < size - 1) targets.add(index(row, col + 1));

  score += targets.size * 90;
  renderBoard(targets);
  updateHud();
  await sleep(260);
  clearMatches(targets);
  dropCandies();
  renderBoard();
  await sleep(150);
  await resolveBoard();
  busy = false;
  updateHud();
}

function checkGameState() {
  if (score >= target) {
    gameOver = true;
    showToast(`Level ${level} complete`, 1600);
    setTimeout(nextLevel, 1700);
    return;
  }

  if (moves <= 0) {
    gameOver = true;
    showToast("Out of moves. Start a new round?", 2200);
  }
}

function nextLevel() {
  level += 1;
  target += 1300 + level * 300;
  moves = Math.max(16, baseMoves - level + 2);
  shuffleCount = 2;
  hammerCount = 3;
  selected = null;
  gameOver = false;
  hammerMode = false;
  createBoard();
  renderBoard();
  updateHud();
  showToast(`Level ${level}`);
}

function startGame() {
  score = 0;
  moves = baseMoves;
  level = 1;
  target = baseTarget;
  selected = null;
  busy = false;
  gameOver = false;
  shuffleCount = 2;
  hammerCount = 3;
  hammerMode = false;
  createBoard();
  renderBoard();
  updateHud();
}

newGameButton.addEventListener("click", startGame);
shuffleButton.addEventListener("click", () => {
  shuffleBoard(true);
  showToast("Shuffled");
});
hammerButton.addEventListener("click", () => {
  if (hammerCount <= 0 || busy || gameOver) {
    return;
  }
  hammerMode = !hammerMode;
  selected = null;
  renderBoard();
  showToast(hammerMode ? "Pick a candy to pop" : "Pop canceled");
});

startGame();

    </script>
  </body>
</html>


