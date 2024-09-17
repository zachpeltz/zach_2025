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

Dodge Game - don't hit the moving obstacles and score points as you go!


```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body { margin: 0; overflow: hidden; }
    canvas { display: block; background-color: #87CEEB; }
    #score { position: absolute; top: 10px; left: 10px; color: #fff; font-size: 24px; }
    #lives { position: absolute; top: 50px; left: 10px; color: #fff; font-size: 24px; }
  </style>
</head>
<body>
  <canvas id="gameCanvas"></canvas>
  <div id="score">Score: 0</div>
  <div id="lives">Lives: 3</div>
  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const lanes = 3;
    const laneWidth = canvas.width / lanes;
    let dino = { x: laneWidth, y: canvas.height / 2, width: 50, height: 50 };
    const dinoImage = new Image();
    dinoImage.src = 'https://thumbs.dreamstime.com/z/cute-dinosaur-cartoon-illustration-33230511.jpg';

    const barrels = [];
    let score = 0;
    let lives = 3;
    let gameSpeed = 3;
    let lastObstacleTime = 0;

    function drawDino() {
      ctx.drawImage(dinoImage, dino.x, dino.y, dino.width, dino.height);
    }

    function drawBarrels() {
      barrels.forEach(barrel => {
        ctx.drawImage(barrel.image, barrel.x, barrel.y, barrel.width, barrel.height);
      });
    }

    function draw() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawDino();
      drawBarrels();
      ctx.fillStyle = 'white';
      ctx.font = '24px Arial';
      ctx.fillText(`Score: ${score}`, 10, 30);
      ctx.fillText(`Lives: ${lives}`, 10, 60);
    }

    function update() {
      score++;
      barrels.forEach(barrel => {
        barrel.x -= gameSpeed;
      });
      if (barrels.length > 0 && barrels[0].x < -barrels[0].width) {
        barrels.shift();
      }

      if (Date.now() - lastObstacleTime > 2000) {
        createBarrel();
        lastObstacleTime = Date.now();
      }
      
      if (lives <= 0) {
        alert('Game Over');
        document.location.reload();
      }
    }

    function createBarrel() {
      const barrelImage = new Image();
      barrelImage.src = 'https://caterrent.com/store/wp-content/uploads/2018/12/BARR01-1.jpg';
      barrels.push({
        x: canvas.width,
        y: Math.random() * (canvas.height - 50),
        width: 50,
        height: 50,
        image: barrelImage
      });
    }

    function checkCollision() {
      barrels.forEach(barrel => {
        if (dino.x < barrel.x + barrel.width &&
            dino.x + dino.width > barrel.x &&
            dino.y < barrel.y + barrel.height &&
            dino.y + dino.height > barrel.y) {
          lives--;
          barrels.splice(barrels.indexOf(barrel), 1);
          if (lives > 0) {
            dino.x = laneWidth;
            dino.y = canvas.height / 2;
          }
        }
      });
    }

    function handleInput(e) {
      switch (e.key) {
        case 'ArrowUp':
        case 'w':
          dino.y = Math.max(dino.y - 60, 0);
          break;
        case 'ArrowDown':
        case 's':
          dino.y = Math.min(dino.y + 60, canvas.height - dino.height);
          break;
      }
    }

    document.addEventListener('keydown', handleInput);

    function gameLoop() {
      update();
      checkCollision();
      draw();
      requestAnimationFrame(gameLoop);
    }

    gameLoop();
  </script>
</body>
</html>