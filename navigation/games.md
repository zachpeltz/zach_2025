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

Poker game 
```javascript
// Setup the deck, shuffle, and deal cards
let suits = ["Hearts", "Diamonds", "Clubs", "Spades"];
let ranks = ["2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"];
let deck = [];

// Initialize deck
for (let suit of suits) {
  for (let rank of ranks) {
    deck.push(`${rank} of ${suit}`);
  }
}

// Shuffle the deck
function shuffleDeck(deck) {
  for (let i = deck.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [deck[i], deck[j]] = [deck[j], deck[i]];
  }
}

shuffleDeck(deck);

// Deal cards to players and the community
let player1Hand = [deck.pop(), deck.pop()];
let player2Hand = [deck.pop(), deck.pop()];
let communityCards = [deck.pop(), deck.pop(), deck.pop(), deck.pop(), deck.pop()];

// Display hands and community cards
console.log("Player 1 Hand:", player1Hand);
console.log("Player 2 Hand:", player2Hand);
console.log("Community Cards:", communityCards);

// Basic function to rank hands (simplified)
function rankHand(playerHand, communityCards) {
  let allCards = playerHand.concat(communityCards);
  // In a real game, this would involve a complex ranking algorithm
  // For simplicity, we just count high cards here
  let highCard = Math.max(...allCards.map(card => ranks.indexOf(card.split(" ")[0])));
  return highCard;
}

// Compare hands to determine winner
function determineWinner() {
  let player1Score = rankHand(player1Hand, communityCards);
  let player2Score = rankHand(player2Hand, communityCards);

  if (player1Score > player2Score) {
    console.log("Player 1 wins with a higher hand!");
  } else if (player2Score > player1Score) {
    console.log("Player 2 wins with a higher hand!");
  } else {
    console.log("It's a tie!");
  }
}

// Showdown
determineWinner();