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



```javascript
let canvas, ctx;
let dinosaur, obstacles, lanes;
let score, lives;
let gameInterval;
let keys = {};

const dinoImage = new Image();
dinoImage.src = 'https://thumbs.dreamstime.com/z/cute-dinosaur-cartoon-illustration-33230511.jpg';

const barrelImage = new Image();
barrelImage.src = 'https://caterrent.com/store/wp-content/uploads/2018/12/BARR01-1.jpg';

function setup() {
    canvas = document.createElement('canvas');
    canvas.width = 800;
    canvas.height = 600;
    document.body.appendChild(canvas);
    ctx = canvas.getContext('2d');

    dinosaur = { x: 50, y: canvas.height / 2, width: 50, height: 50 };
    obstacles = [];
    lanes = [canvas.height / 3, canvas.height / 2, canvas.height * 2 / 3];
    score = 0;
    lives = 3;

    document.addEventListener('keydown', (e) => keys[e.key] = true);
    document.addEventListener('keyup', (e) => keys[e.key] = false);

    gameInterval = setInterval(update, 20);

  
    const resetButton = document.createElement('button');
    resetButton.textContent = 'Reset Game';
    resetButton.addEventListener('click', resetGame);
    document.body.appendChild(resetButton);
}

function update() {
    moveDinosaur();
    moveObstacles();
    detectCollisions();
    draw();
}

function moveDinosaur() {
    if (keys['w'] || keys['ArrowUp']) {
        dinosaur.y = Math.max(lanes[0], dinosaur.y - 10);
    } else if (keys['s'] || keys['ArrowDown']) {
        dinosaur.y = Math.min(lanes[2], dinosaur.y + 10);
    }
}

function moveObstacles() {
    if (Math.random() < 0.02) {
        const lane = lanes[Math.floor(Math.random() * lanes.length)];
        obstacles.push({ x: canvas.width, y: lane, width: 50, height: 50 });
    }
    obstacles.forEach(obstacle => {
        obstacle.x -= 5;
    });
    obstacles = obstacles.filter(obstacle => obstacle.x + obstacle.width > 0);
}

function detectCollisions() {
    obstacles.forEach(obstacle => {
        if (dinosaur.x < obstacle.x + obstacle.width &&
            dinosaur.x + dinosaur.width > obstacle.x &&
            dinosaur.y < obstacle.y + obstacle.height &&
            dinosaur.y + dinosaur.height > obstacle.y) {
            lives--;
            if (lives <= 0) {
                endGame();
            } else {
                obstacles = obstacles.filter(o => o !== obstacle);
            }
        }
    });
}

function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

   
    ctx.drawImage(dinoImage, dinosaur.x, dinosaur.y, dinosaur.width, dinosaur.height);

   
    obstacles.forEach(obstacle => {
        ctx.drawImage(barrelImage, obstacle.x, obstacle.y, obstacle.width, obstacle.height);
    });

   
    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText(`Score: ${Math.floor(score)}`, 10, 20);
    ctx.fillText(`Lives: ${lives}`, 10, 40);

    score += 0.05; 
}

function endGame() {
    clearInterval(gameInterval);
    alert('Game Over! Your score was ' + Math.floor(score));
}

function resetGame() {
    clearInterval(gameInterval);
    document.body.removeChild(canvas);
    document.body.removeChild(document.querySelector('button'));
    setup();
}

setup();