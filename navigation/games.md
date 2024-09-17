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

Poker 2 player game - best hand wins!
<table id="player1Cards"></table>
<table id="player2Cards"></table>
<table id="communityCards"></table>
<p id="gameStatus">Press "Deal Cards" to start the game!</p>
<button onclick="dealCards()">Deal Cards</button>
<button onclick="resetGame()">Reset Game</button>

<script>
let deck, player1Hand, player2Hand, communityCards;

// Initialize deck and shuffle it
function createDeck() {
  const suits = ['Hearts', 'Diamonds', 'Clubs', 'Spades'];
  const ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
  deck = [];
  
  for (let suit of suits) {
    for (let rank of ranks) {
      deck.push(`${rank} of ${suit}`);
    }
  }
  
  // Shuffle the deck
  deck.sort(() => Math.random() - 0.5);
}

function dealCards() {
  createDeck();
  player1Hand = [deck.pop(), deck.pop()];
  player2Hand = [deck.pop(), deck.pop()];
  communityCards = [deck.pop(), deck.pop(), deck.pop(), deck.pop(), deck.pop()];

  renderCards();
  document.getElementById('gameStatus').textContent = "Cards dealt! Check the hands.";
}

function renderCards() {
  // Render player 1's hand
  let player1HTML = '<tr><td>Player 1 Cards</td>';
  for (let card of player1Hand) {
    player1HTML += `<td>${card}</td>`;
  }
  player1HTML += '</tr>';
  document.getElementById("player1Cards").innerHTML = player1HTML;

  // Render player 2's hand
  let player2HTML = '<tr><td>Player 2 Cards</td>';
  for (let card of player2Hand) {
    player2HTML += `<td>${card}</td>`;
  }
  player2HTML += '</tr>';
  document.getElementById("player2Cards").innerHTML = player2HTML;

  // Render community cards
  let communityHTML = '<tr><td>Community Cards</td>';
  for (let card of communityCards) {
    communityHTML += `<td>${card}</td>`;
  }
  communityHTML += '</tr>';
  document.getElementById("communityCards").innerHTML = communityHTML;
}

function resetGame() {
  document.getElementById("gameStatus").textContent = "Press 'Deal Cards' to start the game!";
  document.getElementById("player1Cards").innerHTML = "";
  document.getElementById("player2Cards").innerHTML = "";
  document.getElementById("communityCards").innerHTML = "";
}

dealCards();
</script>