---
layout: page
title: Cookie Clicker
permalink: /cookieclicker/
comments: true
---

<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    text-align: center;
    background-color: #F7F7F7;
    color: #333;
    margin: 0;
  }
  h1 {
    font-size: 2.5em;
    margin-top: 20px;
  }
  .cookie-container {
    margin: 20px auto;
    display: flex;
    justify-content: center;
  }
  .cookie {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    background-color: #DEB887;
    cursor: pointer;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: transform 0.1s;
  }
  .cookie:active {
    transform: scale(0.9);
  }
  .stats {
    margin: 20px;
    font-size: 1.2em;
  }
  .shop {
    margin: 30px auto;
    padding: 20px;
    max-width: 300px;
    background-color: #fff;
    border: 2px solid #ddd;
    border-radius: 8px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  }
  .shop button {
    display: block;
    width: 100%;
    padding: 12px;
    margin: 10px 0;
    border: none;
    border-radius: 5px;
    background-color: #4CAF50;
    color: white;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.1s;
  }
  .shop button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
  .shop button:hover:enabled {
    background-color: #45A049;
    transform: scale(1.05);
  }
  .shop button:enabled {
    transform: scale(1);
  }
</style>
<h1>Cookie Clicker Game</h1>
<div class="cookie-container">
  <div class="cookie" id="cookie" onclick="clickCookie()"></div>
</div>
<div class="stats">
  <p>:Cookies: <span id="cookieCount">0</span></p>
  <p>:Cookies per second: <span id="cookiesPerSecond">0</span></p>
</div>
<div class="shop">
  <h2>Shop Upgrades</h2>
  <button id="upgrade1" onclick="buyUpgrade(1)">+1 Cookie per Second (Cost: 100 cookies)</button>
  <button id="upgrade2" onclick="buyUpgrade(2)">+10 Cookies per Second (Cost: 500 cookies)</button>
  <button id="upgrade3" onclick="buyUpgrade(3)">+30 Cookies per Second (Cost: 1000 cookies)</button>
</div>
<script>
  let cookies = 0;
  let cookiesPerSecond = 0;
  function clickCookie() {
    cookies++;
    updateDisplay();
    updateButtons();
  }
  function buyUpgrade(upgrade) {
    if (upgrade === 1 && cookies >= 50) {
      cookies -= 50;
      cookiesPerSecond += 1;
    } else if (upgrade === 2 && cookies >= 200) {
      cookies -= 200;
      cookiesPerSecond += 5;
    } else if (upgrade === 3 && cookies >= 500) {
      cookies -= 500;
      cookiesPerSecond += 10;
    }
    updateDisplay();
    updateButtons();
  }
  function updateDisplay() {
    document.getElementById('cookieCount').innerText = cookies;
    document.getElementById('cookiesPerSecond').innerText = cookiesPerSecond;
  }
  function updateButtons() {
    document.getElementById('upgrade1').disabled = cookies < 50;
    document.getElementById('upgrade2').disabled = cookies < 200;
    document.getElementById('upgrade3').disabled = cookies < 500;
  }
  function autoGenerateCookies() {
    cookies += cookiesPerSecond;
    updateDisplay();
    updateButtons();
  }
  setInterval(autoGenerateCookies, 1000);
</script>