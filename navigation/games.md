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

```javascript
let player1 = { left: 1, right: 1 };
let player2 = { left: 1, right: 1 };
let currentPlayer = 1;

function updateStatus() {
  console.log(`Player 1: Left - ${player1.left}, Right - ${player1.right}`);
  console.log(`Player 2: Left - ${player2.left}, Right - ${player2.right}`);
  if ((player1.left >= 5 && player1.right >= 5) || (player2.left >= 5 && player2.right >= 5)) {
    console.log("Game Over! Last player with a hand wins.");
  }
}

function tap(player, hand) {
  if (currentPlayer === 1) {
    if (hand === 'left') player2.left += player1.left;
    else player2.right += player1.right;
    player2.left = player2.left >= 5 ? 0 : player2.left;
    player2.right = player2.right >= 5 ? 0 : player2.right;
    currentPlayer = 2;
  } else {
    if (hand === 'left') player1.left += player2.left;
    else player1.right += player2.right;
    player1.left = player1.left >= 5 ? 0 : player1.left;
    player1.right = player1.right >= 5 ? 0 : player1.right;
    currentPlayer = 1;
  }
  updateStatus();
}

function combine(player, fromHand, toHand) {
  if (currentPlayer === 1) {
    let moveAmount = fromHand === 'left' ? player1.left : player1.right;
    if (fromHand === 'left') { player1.left -= moveAmount; player1.right += moveAmount; }
    else { player1.right -= moveAmount; player1.left += moveAmount; }
    currentPlayer = 2;
  } else {
    let moveAmount = fromHand === 'left' ? player2.left : player2.right;
    if (fromHand === 'left') { player2.left -= moveAmount; player2.right += moveAmount; }
    else { player2.right -= moveAmount; player2.left += moveAmount; }
    currentPlayer = 1;
  }
  updateStatus();
}
