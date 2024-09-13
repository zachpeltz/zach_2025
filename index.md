---
layout: base
title: Student Home 
description: Home Page
hide: true
---

<ul>
  <li><a href="https://zachpeltz.github.io/zach_2025/blogs/">Blogs</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/about/">About</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/devops/hacks">Hacks</a></li>
  <li><a href="https://zachpeltz.github.io/zach_2025/snake/">Snake</a></li>
</ul>
<style>
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}
li {
  float: left;
}
li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
li a:hover {
  background-color: #111;
}
</style>

<img src="https://media.tenor.com/xKJ0blGgIlQAAAAM/dance-happy.gif" alt="mario gif">
This is my website: some links above

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