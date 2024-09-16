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

Dinosaur Game: Jump with W/Up arrow and crouch/duck with s or down arrow.
<script>
let canvas, ctx;
let dino, obstacles = [], score = 0, lives = 3;
let isJumping = false, isDucking = false;
let jumpHeight = 0, jumpSpeed = 12, jumpDuration = 20;
let gameSpeed = 2, obstacleSpeed = 4;
let obstacleTimer = 0, obstacleInterval = 100;
let gameOver = false;

document.addEventListener('DOMContentLoaded', () => {
    canvas = document.createElement('canvas');
    ctx = canvas.getContext('2d');
    canvas.width = 800;
    canvas.height = 300;
    document.body.appendChild(canvas);
    
    dino = { x: 50, y: 250, width: 50, height: 50, color: 'green' };
    
    document.addEventListener('keydown', handleKeyDown);
    document.addEventListener('keyup', handleKeyUp);
    
    requestAnimationFrame(gameLoop);
});

function handleKeyDown(e) {
    if (e.key === 'w' || e.key === 'ArrowUp') isJumping = true;
    if (e.key === 's' || e.key === 'ArrowDown') isDucking = true;
}

function handleKeyUp(e) {
    if (e.key === 'w' || e.key === 'ArrowUp') isJumping = false;
    if (e.key === 's' || e.key === 'ArrowDown') isDucking = false;
}

function gameLoop() {
    if (gameOver) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = 'red';
        ctx.font = '30px Arial';
        ctx.fillText('Game Over! Score: ' + score, 250, 150);
        return;
    }

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    updateGame();
    drawGame();
    
    requestAnimationFrame(gameLoop);
}

function updateGame() {
    // Jump logic
    if (isJumping) {
        if (jumpHeight < jumpDuration) {
            dino.y -= jumpSpeed;
            jumpHeight++;
        } else if (jumpHeight < 2 * jumpDuration) {
            dino.y += jumpSpeed;
            jumpHeight++;
        } else {
            isJumping = false;
            jumpHeight = 0;
        }
    }

    // Duck logic
    if (isDucking) {
        dino.height = 30;
        dino.y = 250;
    } else {
        dino.height = 50;
        dino.y = 250;
    }

    // Move obstacles and check for collisions
    obstacles.forEach(obstacle => {
        obstacle.x -= obstacleSpeed;
        if (obstacle.x < dino.x + dino.width &&
            obstacle.x + obstacle.width > dino.x &&
            (dino.y + dino.height > obstacle.y)) {
            if (!isDucking) {
                loseLife();
            }
            obstacle.x = -obstacle.width;  // Move obstacle out of view
        }
    });
    
    obstacles = obstacles.filter(obstacle => obstacle.x > -obstacle.width);

    // Create new obstacles
    obstacleTimer++;
    if (obstacleTimer > obstacleInterval) {
        obstacles.push({ x: canvas.width, y: 250, width: 20, height: 20, color: 'red' });
        obstacleTimer = 0;
        obstacleInterval = Math.max(50, obstacleInterval - 1);  // Increase speed
    }

    // Increase score
    score = Math.floor((Date.now() - startTime) / 1000) * 100;
}

function drawGame() {
    ctx.fillStyle = dino.color;
    ctx.fillRect(dino.x, dino.y, dino.width, dino.height);

    obstacles.forEach(obstacle => {
        ctx.fillStyle = obstacle.color;
        ctx.fillRect(obstacle.x, obstacle.y - obstacle.height, obstacle.width, obstacle.height);
    });

    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText('Score: ' + score, 10, 20);
    ctx.fillText('Lives: ' + lives, 10, 40);
}

function loseLife() {
    lives--;
    if (lives <= 0) {
        gameOver = true;
    }
}

let startTime = Date.now();
</script>