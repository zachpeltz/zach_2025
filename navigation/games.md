---
layout: page
title: Games
search_exclude: true
permalink: /games/
---

Choose Rock, Paper, or Scissors and see if you can beat the computer!

<button onclick="playGame('Rock')">Rock</button>
<button onclick="playGame('Paper')">Paper</button>
<button onclick="playGame('Scissors')">Scissors</button>

<p id="result"></p>

<script>
  function playGame(playerChoice) {
    const choices = ['Rock', 'Paper', 'Scissors'];
    let computerChoice = choices[Math.floor(Math.random() * 3)];
    let result = '';

    if (playerChoice === computerChoice) {
      result = 'It\'s a tie!';
    } else if (
      (playerChoice === 'Rock' && computerChoice === 'Scissors') ||
      (playerChoice === 'Paper' && computerChoice === 'Rock') ||
      (playerChoice === 'Scissors' && computerChoice === 'Paper')
    ) {
      result = 'You win! ' + playerChoice + ' beats ' + computerChoice;
    } else {
      result = 'You lose! ' + computerChoice + ' beats ' + playerChoice;
    }

    document.getElementById('result').textContent = result;
  }
</script> 


Minesweeper Game:
Click a cell to reveal it. Avoid the mines!

<div id="grid"></div>
<p id="status"></p>
<button onclick="resetGame()">Reset Game</button>

<script>
const gridSize = 8;
const mineCount = 10;
let grid = [];
let revealedCells = 0;

function createGrid() {
  let minePositions = new Set();
  while (minePositions.size < mineCount) {
    minePositions.add(Math.floor(Math.random() * gridSize * gridSize));
  }

  grid = [];
  revealedCells = 0;
  document.getElementById("grid").innerHTML = '';
  document.getElementById("status").textContent = '';

  for (let i = 0; i < gridSize; i++) {
    let row = [];
    let rowDiv = document.createElement('div');
    for (let j = 0; j < gridSize; j++) {
      let cell = {
        isMine: minePositions.has(i * gridSize + j),
        revealed: false,
        adjacentMines: 0
      };
      row.push(cell);

      let button = document.createElement('button');
      button.style.width = '40px';
      button.style.height = '40px';
      button.onclick = () => revealCell(i, j);
      button.id = `cell-${i}-${j}`;
      rowDiv.appendChild(button);
    }
    grid.push(row);
    document.getElementById("grid").appendChild(rowDiv);
  }

  calculateAdjacentMines();
}

function calculateAdjacentMines() {
  for (let i = 0; i < gridSize; i++) {
    for (let j = 0; j < gridSize; j++) {
      if (!grid[i][j].isMine) {
        let mines = 0;
        for (let x = -1; x <= 1; x++) {
          for (let y = -1; y <= 1; y++) {
            if (i + x >= 0 && i + x < gridSize && j + y >= 0 && j + y < gridSize) {
              if (grid[i + x][j + y].isMine) mines++;
            }
          }
        }
        grid[i][j].adjacentMines = mines;
      }
    }
  }
}

function revealCell(x, y) {
  if (grid[x][y].revealed) return;
  grid[x][y].revealed = true;

  let button = document.getElementById(`cell-${x}-${y}`);
  if (grid[x][y].isMine) {
    button.textContent = '💣';
    button.style.backgroundColor = 'red';
    document.getElementById("status").textContent = 'Game Over!';
    revealAllMines();
  } else {
    button.textContent = grid[x][y].adjacentMines || '';
    button.disabled = true;
    button.style.backgroundColor = '#ddd';
    revealedCells++;

    if (grid[x][y].adjacentMines === 0) {
      revealAdjacentCells(x, y);
    }

    if (revealedCells === gridSize * gridSize - mineCount) {
      document.getElementById("status").textContent = 'You Win!';
    }
  }
}

function revealAdjacentCells(x, y) {
  for (let i = -1; i <= 1; i++) {
    for (let j = -1; j <= 1; j++) {
      if (x + i >= 0 && x + i < gridSize && y + j >= 0 && y + j < gridSize) {
        if (!grid[x + i][y + j].revealed && !grid[x + i][y + j].isMine) {
          revealCell(x + i, y + j);
        }
      }
    }
  }
}

function revealAllMines() {
  for (let i = 0; i < gridSize; i++) {
    for (let j = 0; j < gridSize; j++) {
      if (grid[i][j].isMine && !grid[i][j].revealed) {
        let button = document.getElementById(`cell-${i}-${j}`);
        button.textContent = '💣';
        button.style.backgroundColor = 'red';
      }
    }
  }
}

function resetGame() {
  createGrid();
}

createGrid();
</script>