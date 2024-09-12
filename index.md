---
layout: base
title: Student Home 
description: Home Page
hide: true
---
{% include nav/home.html %}

This is my website: some links below
<img src="https://media.tenor.com/xKJ0blGgIlQAAAAM/dance-happy.gif" alt="mario gif">

<ul>
  <li><a href="https://zachpeltz.github.io/zach_2025/">Home</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/blogs/">Blogs</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/about/">About</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/devops/hacks">Hacks</a></li>
</ul>

const canvas = document.createElement('canvas');
document.body.appendChild(canvas);
const ctx = canvas.getContext('2d');
canvas.width = 400;
canvas.height = 400;

const gridSize = 20;
let snake = [{ x: 160, y: 160 }];
let food = { x: 80, y: 80 };
let direction = { x: 0, y: 0 };

const randomPosition = () => ({
    x: Math.floor(Math.random() * canvas.width / gridSize) * gridSize,
    y: Math.floor(Math.random() * canvas.height / gridSize) * gridSize,
});

function gameLoop() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Move Snake
    const newHead = {
        x: snake[0].x + direction.x * gridSize,
        y: snake[0].y + direction.y * gridSize,
    };
    snake.unshift(newHead);
    if (newHead.x === food.x && newHead.y === food.y) {
        food = randomPosition();
    } else {
        snake.pop();
    }

    // Draw Snake
    ctx.fillStyle = 'green';
    snake.forEach(part => ctx.fillRect(part.x, part.y, gridSize, gridSize));

    // Draw Food
    ctx.fillStyle = 'red';
    ctx.fillRect(food.x, food.y, gridSize, gridSize);

    // Check collision
    if (
        newHead.x < 0 || newHead.x >= canvas.width ||
        newHead.y < 0 || newHead.y >= canvas.height ||
        snake.slice(1).some(part => part.x === newHead.x && part.y === newHead.y)
    ) {
        alert("Game Over");
        snake = [{ x: 160, y: 160 }];
        direction = { x: 0, y: 0 };
    }

    requestAnimationFrame(gameLoop);
}

document.addEventListener('keydown', (e) => {
    switch (e.key) {
        case 'ArrowUp': direction = { x: 0, y: -1 }; break;
        case 'ArrowDown': direction = { x: 0, y: 1 }; break;
        case 'ArrowLeft': direction = { x: -1, y: 0 }; break;
        case 'ArrowRight': direction = { x: 1, y: 0 }; break;
    }
});

gameLoop();
