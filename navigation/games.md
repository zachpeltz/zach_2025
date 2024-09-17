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


<div id="game">
  <p>Guess a number between 1 and 1000!</p>
  <input type="number" id="guess" placeholder="Enter your guess here">
  <button onclick="checkGuess()">Submit Guess</button>
  <p id="result"></p>
</div>

<script>
  const randomNumber = Math.floor(Math.random() * 1000) + 1;
  let attempts = 0;

  function checkGuess() {
    const userGuess = parseInt(document.getElementById('guess').value);
    const result = document.getElementById('result');
    attempts++;
    
    if (userGuess === randomNumber) {
      result.textContent = `Congratulations! You guessed the number ${randomNumber} correctly in ${attempts} attempts.`;
    } else if (userGuess > randomNumber) {
      result.textContent = "Too high! Try again.";
    } else {
      result.textContent = "Too low! Try again.";
    }
  }
</script>


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

Choose Rock, Paper, or Scissors and see if you can beat the computer!

Dodge Game - don't hit the moving obstacles! 

<script>
  let canvas = document.createElement("canvas");
  let ctx = canvas.getContext("2d");
  document.body.appendChild(canvas);
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;

  let dino = {
    x: canvas.width / 4,
    y: canvas.height / 2,
    width: 50,
    height: 50,
    image: new Image(),
    lives: 3,
    score: 0
  };
  dino.image.src = 'dino.png'; 

  let obstacles = [];
  let lanes = [canvas.height / 3, canvas.height / 2, canvas.height * 2 / 3];
  let currentLane = 1;
  let speed = 2;

  function drawDino() {
    ctx.drawImage(dino.image, dino.x, lanes[currentLane], dino.width, dino.height);
  }

  function drawObstacles() {
    for (let i = 0; i < obstacles.length; i++) {
      ctx.fillStyle = 'red';
      ctx.fillRect(obstacles[i].x, obstacles[i].y, 50, 50);
    }
  }

  function moveObstacles() {
    for (let i = 0; i < obstacles.length; i++) {
      obstacles[i].x -= speed;
      if (obstacles[i].x + 50 < 0) {
        obstacles.splice(i, 1);
        i--;
      }
    }
  }

  function detectCollision() {
    for (let i = 0; i < obstacles.length; i++) {
      if (
        dino.x < obstacles[i].x + 50 &&
        dino.x + dino.width > obstacles[i].x &&
        lanes[currentLane] < obstacles[i].y + 50 &&
        lanes[currentLane] + dino.height > obstacles[i].y
      ) {
        if (dino.lives > 1) {
          dino.lives--;
          obstacles.splice(i, 1);
          return;
        } else {
          alert('Game Over!');
          document.location.reload();
        }
      }
    }
  }

  function updateScore() {
    dino.score++;
    ctx.fillStyle = 'black';
    ctx.font = '30px Arial';
    ctx.fillText('Score: ' + dino.score, 10, 30);
  }

  function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawDino();
    drawObstacles();
    moveObstacles();
    detectCollision();
    updateScore();
    if (Math.random() < 0.02) {
      obstacles.push({
        x: canvas.width,
        y: lanes[Math.floor(Math.random() * 3)]
      });
    }
    requestAnimationFrame(gameLoop);
  }

  function handleKeyPress(e) {
    if (e.key === 'ArrowUp' || e.key === 'w') {
      currentLane = Math.max(0, currentLane - 1);
    } else if (e.key === 'ArrowDown' || e.key === 's') {
      currentLane = Math.min(2, currentLane + 1);
    }
  }

  document.addEventListener('keydown', handleKeyPress);
  gameLoop();
</script>