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

Anagram game:
- Words must be at least 3 letters long.
- Use all 7 letters for a **pangram** and earn extra points!

<p id="letters"></p>

Click on letters to spell a word:  
<p id="currentWord"></p>
<button onclick="submitWord()">Submit Word</button>
<button onclick="resetGame()">Reset Game</button>
<p id="timer"></p>
<p id="score"></p>
<p id="feedback"></p>
<p id="leaderboard"></p>

<script>
const wordList = {
  "example": ["ex", "map", "lamp", "example", "amp", "plea"], // Replace with an actual wordlist
  "letters": ["letter", "settle", "set", "let", "test", "rest"]
  // Add more sets of 7 letters and words
};
let currentLetters, correctWords, score, timer, interval, usedWords = [];

function startGame() {
  let keys = Object.keys(wordList);
  let randomKey = keys[Math.floor(Math.random() * keys.length)];
  currentLetters = randomKey.split('');
  correctWords = wordList[randomKey];
  score = 0;
  usedWords = [];

  document.getElementById("letters").textContent = currentLetters.join(' ');
  document.getElementById("currentWord").textContent = '';
  document.getElementById("feedback").textContent = '';
  document.getElementById("score").textContent = `Score: ${score}`;
  document.getElementById("leaderboard").textContent = '';

  startTimer(60);
}

function startTimer(seconds) {
  clearInterval(interval);
  timer = seconds;
  interval = setInterval(function() {
    timer--;
    document.getElementById("timer").textContent = `Time left: ${timer}s`;
    if (timer <= 0) {
      clearInterval(interval);
      endGame();
    }
  }, 1000);
}

function clickLetter(letter) {
  document.getElementById("currentWord").textContent += letter;
}

function submitWord() {
  let word = document.getElementById("currentWord").textContent;
  if (word.length >= 3 && correctWords.includes(word) && !usedWords.includes(word)) {
    usedWords.push(word);
    let points = word.length;
    if (word.length === 7) {
      points += 10; // Bonus for pangram
      document.getElementById("feedback").textContent = "Pangram! Extra points!";
    } else {
      document.getElementById("feedback").textContent = `Great! You scored ${points} points.`;
    }
    score += points;
    document.getElementById("score").textContent = `Score: ${score}`;
  } else {
    document.getElementById("feedback").textContent = "Invalid word or already used.";
  }
  document.getElementById("currentWord").textContent = '';
}

function resetGame() {
  endGame();
  startGame();
}

function endGame() {
  document.getElementById("leaderboard").textContent = `Final Score: ${score}. Refresh to play again.`;
  clearInterval(interval);
}

// Initialize game on page load
startGame();
</script>