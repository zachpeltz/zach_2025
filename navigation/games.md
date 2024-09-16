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

Paintball 2 Player Game:
Player 1 and Player 2, take turns shooting! First to get hit 3 times loses.

<div id="gameArea"></div>
<p id="gameStatus"></p>
<button onclick="resetGame()">Start New Game</button>

<script>
let player1Lives = 3;
let player2Lives = 3;
let currentPlayer = 1;
let gameActive = true;

function createBoard() {
  let gameHTML = '<table>';
  for (let i = 0; i < 5; i++) {
    gameHTML += '<tr>';
    for (let j = 0; j < 5; j++) {
      gameHTML += `<td onclick="shoot(${i}, ${j})" style="width: 50px; height: 50px; text-align: center; border: 1px solid black; cursor: pointer;"> </td>`;
    }
    gameHTML += '</tr>';
  }
  gameHTML += '</table>';
  document.getElementById('gameArea').innerHTML = gameHTML;
  document.getElementById('gameStatus').textContent = "Player 1's turn! (3 lives each)";
}

function shoot(x, y) {
  if (!gameActive) return;

  const hit = Math.random() < 0.5; // 50% chance of hitting the opponent
  if (hit) {
    if (currentPlayer === 1) {
      player2Lives--;
      alert("Player 1 hits Player 2!");
    } else {
      player1Lives--;
      alert("Player 2 hits Player 1!");
    }
  } else {
    alert(`Player ${currentPlayer} missed!`);
  }

  checkGameOver();
  switchPlayer();
}

function switchPlayer() {
  if (gameActive) {
    currentPlayer = currentPlayer === 1 ? 2 : 1;
    document.getElementById('gameStatus').textContent = `Player ${currentPlayer}'s turn! Player 1: ${player1Lives} lives, Player 2: ${player2Lives} lives`;
  }
}

function checkGameOver() {
  if (player1Lives <= 0) {
    document.getElementById('gameStatus').textContent = "Player 2 wins!";
    gameActive = false;
  } else if (player2Lives <= 0) {
    document.getElementById('gameStatus').textContent = "Player 1 wins!";
    gameActive = false;
  }
}

function resetGame() {
  player1Lives = 3;
  player2Lives = 3;
  currentPlayer = 1;
  gameActive = true;
  createBoard();
}

createBoard();
</script>