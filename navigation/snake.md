---
layout: page
title: Snake
permalink: /snake/
---

Press any key to start!

<canvas id="snakeGame" width="400" height="400"></canvas>

<style>
  #snakeGame {
    border: 1px solid black;
    background-color: #f0f0f0;
  }
  body {
    font-family: sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100vh;
  }
</style>

<script>
  const canvas = document.getElementById('snakeGame');
  const ctx = canvas.getContext('2d');
  const box = 20;
  let snake = [{ x: 8 * box, y: 8 * box }];
  let direction = 'RIGHT';
  let food = {
    x: Math.floor(Math.random() * 19) * box,
    y: Math.floor(Math.random() * 19) * box,
  };
  let score = 0;

  document.addEventListener('keydown', changeDirection);

  function changeDirection(event) {
    if (event.key === 'ArrowUp' && direction !== 'DOWN') direction = 'UP';
    else if (event.key === 'ArrowDown' && direction !== 'UP') direction = 'DOWN';
    else if (event.key === 'ArrowLeft' && direction !== 'RIGHT') direction = 'LEFT';
    else if (event.key === 'ArrowRight' && direction !== 'LEFT') direction = 'RIGHT';
  }

  function drawSnake() {
    for (let i = 0; i < snake.length; i++) {
      ctx.fillStyle = i === 0 ? 'green' : 'lightgreen';
      ctx.fillRect(snake[i].x, snake[i].y, box, box);
      ctx.strokeStyle = 'darkgreen';
      ctx.strokeRect(snake[i].x, snake[i].y, box, box);
    }
  }

  function drawFood() {
    ctx.fillStyle = 'red';
    ctx.fillRect(food.x, food.y, box, box);
  }

  function updateSnake() {
    let head = { ...snake[0] };

    if (direction === 'UP') head.y -= box;
    if (direction === 'DOWN') head.y += box;
    if (direction === 'LEFT') head.x -= box;
    if (direction === 'RIGHT') head.x += box;

    // Game Over conditions
    if (head.x < 0 || head.x >= 400 || head.y < 0 || head.y >= 400 || collision(head, snake)) {
      clearInterval(game);
      alert('Game Over! Your score: ' + score);
      window.location.reload();  // Restart the game
    }

    if (head.x === food.x && head.y === food.y) {
      score++;
      food = {
        x: Math.floor(Math.random() * 19) * box,
        y: Math.floor(Math.random() * 19) * box,
      };
    } else {
      snake.pop();
    }

    snake.unshift(head);
  }

  function collision(head, array) {
    for (let i = 1; i < array.length; i++) {
      if (head.x === array[i].x && head.y === array[i].y) return true;
    }
    return false;
  }

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawSnake();
    drawFood();
    updateSnake();
    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText('Score: ' + score, 10, 20);
  }

  let game = setInterval(draw, 100);
</script>