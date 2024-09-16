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

- Left-click to reveal a tile.
- Right-click to flag a suspected bomb.
- Win by flagging all 8 suspected bombs and clearing the rest of the safe tiles.

<table id="minesweeper"></table>
<p id="status"></p>
<button onclick="resetGame()">Reset Game</button>

<script>
const rows = 8;
const cols = 8;
const numBombs = 8;
let board, bombLocations, revealedTiles, flaggedTiles, gameEnded;

function createBoard() {
  board = Array(rows).fill().map(() => Array(cols).fill({ bomb: false, revealed: false, flagged: false }));
  revealedTiles = 0;
  flaggedTiles = 0;
  gameEnded = false;
  document.getElementById("status").textContent = "Game in progress...";
  renderBoard();
}

function placeBombs(excludeX, excludeY) {
  bombLocations = new Set();
  while (bombLocations.size < numBombs) {
    let x = Math.floor(Math.random() * rows);
    let y = Math.floor(Math.random() * cols);
    if ((x === excludeX && y === excludeY) || bombLocations.has(`${x},${y}`)) continue;
    bombLocations.add(`${x},${y}`);
    board[x][y] = { bomb: true, revealed: false, flagged: false };
  }
  revealSafeArea(excludeX, excludeY);
}

function revealSafeArea(x, y) {
  const directions = [[0,1], [1,0], [0,-1], [-1,0], [1,1], [1,-1], [-1,1], [-1,-1]];
  let safeTiles = [[x, y]];

  while (safeTiles.length < 15) {
    let randomX = Math.floor(Math.random() * rows);
    let randomY = Math.floor(Math.random() * cols);
    if (!board[randomX][randomY].bomb && !safeTiles.some(([sx, sy]) => sx === randomX && sy === randomY)) {
      safeTiles.push([randomX, randomY]);
    }
  }

  safeTiles.forEach(([sx, sy]) => revealTile(sx, sy));
}

function renderBoard() {
  let tableHTML = '';
  for (let x = 0; x < rows; x++) {
    tableHTML += '<tr>';
    for (let y = 0; y < cols; y++) {
      let cell = board[x][y];
      if (cell.revealed) {
        let bombsAround = countBombsAround(x, y);
        tableHTML += `<td onclick="handleLeftClick(${x}, ${y})" oncontextmenu="handleRightClick(event, ${x}, ${y})" style="background-color: lightgray;">${cell.bomb ? '💣' : bombsAround || ''}</td>`;
      } else if (cell.flagged) {
        tableHTML += `<td onclick="handleLeftClick(${x}, ${y})" oncontextmenu="handleRightClick(event, ${x}, ${y})" style="background-color: lightblue;">🚩</td>`;
      } else {
        tableHTML += `<td onclick="handleLeftClick(${x}, ${y})" oncontextmenu="handleRightClick(event, ${x}, ${y})" style="background-color: darkgray;"></td>`;
      }
    }
    tableHTML += '</tr>';
  }
  document.getElementById('minesweeper').innerHTML = tableHTML;
}

function handleLeftClick(x, y) {
  if (gameEnded || board[x][y].revealed || board[x][y].flagged) return;

  if (revealedTiles === 0) {
    placeBombs(x, y);
  }

  revealTile(x, y);
  checkWin();
}

function handleRightClick(event, x, y) {
  event.preventDefault();
  if (gameEnded || board[x][y].revealed) return;

  if (board[x][y].flagged) {
    board[x][y].flagged = false;
    flaggedTiles--;
  } else {
    board[x][y].flagged = true;
    flaggedTiles++;
  }

  renderBoard();
  checkWin();
}

function revealTile(x, y) {
  if (board[x][y].revealed || board[x][y].flagged) return;
  board[x][y].revealed = true;
  revealedTiles++;

  if (board[x][y].bomb) {
    document.getElementById('status').textContent = "Game over! You hit a bomb!";
    gameEnded = true;
  } else if (countBombsAround(x, y) === 0) {
    revealAdjacentTiles(x, y);
  }

  renderBoard();
}

function revealAdjacentTiles(x, y) {
  const directions = [[0,1], [1,0], [0,-1], [-1,0], [1,1], [1,-1], [-1,1], [-1,-1]];
  directions.forEach(([dx, dy]) => {
    let newX = x + dx, newY = y + dy;
    if (newX >= 0 && newX < rows && newY >= 0 && newY < cols) {
      revealTile(newX, newY);
    }
  });
}

function countBombsAround(x, y) {
  const directions = [[0,1], [1,0], [0,-1], [-1,0], [1,1], [1,-1], [-1,1], [-1,-1]];
  return directions.reduce((count, [dx, dy]) => {
    let newX = x + dx, newY = y + dy;
    return count + (newX >= 0 && newX < rows && newY >= 0 && newY < cols && board[newX][newY].bomb ? 1 : 0);
  }, 0);
}

function checkWin() {
  if (flaggedTiles === numBombs && revealedTiles === rows * cols - numBombs) {
    document.getElementById('status').textContent = "Congratulations! You've flagged all bombs and cleared the board!";
    gameEnded = true;
  }
}

function resetGame() {
  createBoard();
}

resetGame();
</script>