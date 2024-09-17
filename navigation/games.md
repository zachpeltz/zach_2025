---
layout: page
title: Games
search_exclude: true
permalink: /games/
---

Tic tac toe game:
Player 1 (X) vs Player 2 (O)
First to get 3 in a row (any direction) wins!

<table id="ticTacToeBoard"></table>
<p id="gameStatus">Player 1's turn (X)</p>
<button onclick="resetGame()">Reset Game</button>

<script>
let board, currentPlayer, gameActive, movesMade;

function createBoard() {
  board = Array(3).fill().map(() => Array(3).fill(''));
  currentPlayer = 'X';
  gameActive = true;
  movesMade = 0;
  document.getElementById("gameStatus").textContent = "Player 1's turn (X)";
  renderBoard();
}

function renderBoard() {
  let tableHTML = '';
  for (let i = 0; i < 3; i++) {
    tableHTML += '<tr>';
    for (let j = 0; j < 3; j++) {
      tableHTML += `<td onclick="handleClick(${i}, ${j})" style="width: 50px; height: 50px; text-align: center; font-size: 24px;">${board[i][j]}</td>`;
    }
    tableHTML += '</tr>';
  }
  document.getElementById("ticTacToeBoard").innerHTML = tableHTML;
}

function handleClick(row, col) {
  if (board[row][col] === '' && gameActive) {
    board[row][col] = currentPlayer;
    movesMade++;
    renderBoard();
    checkWinner();
    switchPlayer();
  }
}

function switchPlayer() {
  if (gameActive) {
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    document.getElementById("gameStatus").textContent = `Player ${currentPlayer === 'X' ? 1 : 2}'s turn (${currentPlayer})`;
  }
}

function checkWinner() {
  const winConditions = [
    [[0, 0], [0, 1], [0, 2]], // Row 1
    [[1, 0], [1, 1], [1, 2]], // Row 2
    [[2, 0], [2, 1], [2, 2]], // Row 3
    [[0, 0], [1, 0], [2, 0]], // Col 1
    [[0, 1], [1, 1], [2, 1]], // Col 2
    [[0, 2], [1, 2], [2, 2]], // Col 3
    [[0, 0], [1, 1], [2, 2]], // Diagonal 1
    [[0, 2], [1, 1], [2, 0]]  // Diagonal 2
  ];

  for (let condition of winConditions) {
    const [a, b, c] = condition;
    if (board[a[0]][a[1]] !== '' && board[a[0]][a[1]] === board[b[0]][b[1]] && board[a[0]][a[1]] === board[c[0]][c[1]]) {
      document.getElementById("gameStatus").textContent = `Player ${currentPlayer === 'X' ? 1 : 2} wins!`;
      gameActive = false;
      return;
    }
  }

  if (movesMade === 9) {
    document.getElementById("gameStatus").textContent = "It's a draw!";
    gameActive = false;
  }
}

function resetGame() {
  createBoard();
}

createBoard();
</script>

Subway Surfers Game - avoid the obstacles!

<table id="gameBoard"></table>
<p id="gameStatus">Press Start to Begin!</p>
<p id="scoreBoard">Best Score: 0 | Best Time: 0s</p>
<p id="currentScore">Score: 0 | Time: 0s</p>
<button onclick="startGame()">Start Game</button>
<button onclick="resetGame()">Reset Game</button>

<script>
let playerLane, score, time, gameActive, bestScore = 0, bestTime = 0, intervalId, lanes;
let obstacleLane;
const totalLanes = 3; // Number of lanes
let obstacleSpeed = 1000; // Speed of obstacle

// Initialize game
function createBoard() {
  let boardHTML = '';
  for (let i = 0; i < totalLanes; i++) {
    boardHTML += '<tr>';
    boardHTML += `<td id="lane${i}" style="width: 100px; height: 100px; text-align: center; font-size: 24px; border: 1px solid black;"></td>`;
    boardHTML += '</tr>';
  }
  document.getElementById("gameBoard").innerHTML = boardHTML;
}

function startGame() {
  playerLane = 1; // Start in the middle lane
  score = 0;
  time = 0;
  gameActive = true;
  obstacleSpeed = 1000;
  
  createBoard();
  document.getElementById(`lane${playerLane}`).textContent = 'P'; // Place player on the board
  document.getElementById("gameStatus").textContent = "Game Started!";
  
  intervalId = setInterval(gameLoop, obstacleSpeed); // Start obstacle generation
  startTimer();
}

function gameLoop() {
  generateObstacle();
  moveObstacle();
}

function generateObstacle() {
  obstacleLane = Math.floor(Math.random() * totalLanes);
  document.getElementById(`lane${obstacleLane}`).textContent = 'X'; // Display obstacle
}

function moveObstacle() {
  setTimeout(() => {
    if (obstacleLane === playerLane) {
      gameOver();
    } else {
      document.getElementById(`lane${obstacleLane}`).textContent = ''; // Clear obstacle
    }
  }, obstacleSpeed); 
}

// Handle player movement with arrow keys
document.onkeydown = function(e) {
  if (!gameActive) return;

  if (e.key === 'ArrowUp' && playerLane > 0) {
    movePlayerTo(playerLane - 1);
  } else if (e.key === 'ArrowDown' && playerLane < totalLanes - 1) {
    movePlayerTo(playerLane + 1);
  }
};

// Move player
function movePlayerTo(newLane) {
  document.getElementById(`lane${playerLane}`).textContent = ''; // Clear old position
  playerLane = newLane;
  document.getElementById(`lane${playerLane}`).textContent = 'P'; // Show player in new lane
}

// Timer
function startTimer() {
  setInterval(() => {
    if (gameActive) {
      time++;
      document.getElementById("currentScore").textContent = `Score: ${score} | Time: ${time}s`;
    }
  }, 1000);
}

// Game over
function gameOver() {
  clearInterval(intervalId);
  gameActive = false;
  document.getElementById("gameStatus").textContent = "Game Over!";

  if (score > bestScore) {
    bestScore = score;
  }
  if (time > bestTime) {
    bestTime = time;
  }
  document.getElementById("scoreBoard").textContent = `Best Score: ${bestScore} | Best Time: ${bestTime}s`;
}

// Reset game but keep best scores
function resetGame() {
  clearInterval(intervalId);
  gameActive = false;
  document.getElementById("currentScore").textContent = "Score: 0 | Time: 0s";
  document.getElementById("gameStatus").textContent = "Press Start to Begin!";
  createBoard();
}

createBoard();
</script>