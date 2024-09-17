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
document.addEventListener('DOMContentLoaded', function() {
    const canvas = document.createElement('canvas');
    const ctx = canvas.getContext('2d');
    canvas.width = 800;
    canvas.height = 400;
    document.body.appendChild(canvas);

    let player = { x: 50, y: 300, width: 50, height: 50, dy: 0, grounded: true, ducking: false };
    let obstacles = [];
    let score = 0;
    let gameOver = false;
    let speed = 5;

    function drawPlayer() {
        ctx.fillStyle = 'blue';
        ctx.fillRect(player.x, player.y, player.width, player.height);
    }

    function drawObstacles() {
        ctx.fillStyle = 'red';
        obstacles.forEach(obstacle => {
            ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
        });
    }

    function addObstacle() {
        let height = Math.random() > 0.5 ? 50 : 100;
        obstacles.push({ x: canvas.width, y: canvas.height - height, width: 50, height: height });
    }

    function updateObstacles() {
        obstacles.forEach(obstacle => obstacle.x -= speed);
        if (obstacles.length > 0 && obstacles[0].x + obstacles[0].width < 0) {
            obstacles.shift();
        }
    }

    function checkCollision() {
        obstacles.forEach(obstacle => {
            if (player.x < obstacle.x + obstacle.width && player.x + player.width > obstacle.x &&
                player.y < obstacle.y + obstacle.height && player.y + player.height > obstacle.y) {
                gameOver = true;
            }
        });
    }

    function updatePlayer() {
        if (!player.grounded) {
            player.dy += 0.8;
            player.y += player.dy;
            if (player.y + player.height >= canvas.height) {
                player.y = canvas.height - player.height;
                player.dy = 0;
                player.grounded = true;
            }
        }

        if (player.ducking) {
            player.height = 25;
        } else {
            player.height = 50;
        }
    }

    function gameLoop() {
        if (!gameOver) {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawPlayer();
            drawObstacles();
            updateObstacles();
            updatePlayer();
            checkCollision();
            score++;
            speed += 0.001;

            if (Math.random() < 0.01) {
                addObstacle();
            }

            ctx.font = '20px Arial';
            ctx.fillText('Score: ' + Math.floor(score / 10), 10, 20);

            requestAnimationFrame(gameLoop);
        } else {
            ctx.font = '50px Arial';
            ctx.fillText('Game Over', canvas.width / 2 - 150, canvas.height / 2);
        }
    }

    document.addEventListener('keydown', function(event) {
        if (event.key === 'ArrowUp' || event.key === 'w') {
            if (player.grounded) {
                player.dy = -15;
                player.grounded = false;
            }
        }
        if (event.key === 'ArrowDown' || event.key === 's') {
            player.ducking = true;
        }
    });

    document.addEventListener('keyup', function(event) {
        if (event.key === 'ArrowDown' || event.key === 's') {
            player.ducking = false;
        }
    });

    gameLoop();
});
</script>