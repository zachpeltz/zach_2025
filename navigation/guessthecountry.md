---
layout: page
title: Guess the Country
permalink: /guessthecountry/
---

## How to Play:
1. Enter a country name.
2. You will see how far your guess is from the correct country.
3. An arrow will point in the direction of the correct country.

<div id="map" style="height: 500px; width: 100%;"></div>

<p>Guess the country: <input type="text" id="guess" placeholder="Enter country name" /> <button onclick="checkGuess()">Submit</button></p>
<p id="feedback"></p>
<p id="distance"></p>

<style>
  #map {
    height: 500px;
    width: 100%;
    margin-top: 20px;
  }
  #arrow {
    margin-top: 10px;
  }
</style>

<script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />

<script>
  const countries = {
    "Afghanistan": { lat: 33.93911, lon: 67.709953 },
    "Albania": { lat: 41.153332, lon: 20.168331 },
    "Algeria": { lat: 28.033886, lon: 1.659626 },
    // Add more countries here...
  };

  let correctCountry = getRandomCountry();
  let lastGuess = null;

  // Initialize the map
  const map = L.map('map').setView([20, 0], 2);  // Centered globally
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);

  function getRandomCountry() {
    const keys = Object.keys(countries);
    return keys[Math.floor(Math.random() * keys.length)];
  }

  function checkGuess() {
    const userGuess = document.getElementById('guess').value.trim();
    const feedback = document.getElementById('feedback');
    const distance = document.getElementById('distance');

    if (!countries[userGuess]) {
      feedback.innerText = "Country not found, try again!";
      return;
    }

    const guessedCountry = countries[userGuess];
    const correctCoords = countries[correctCountry];

    const dist = getDistance(guessedCountry.lat, guessedCountry.lon, correctCoords.lat, correctCoords.lon);
    feedback.innerText = `Your guess: ${userGuess}, Correct Country: ${correctCountry}`;
    distance.innerText = `Distance to correct country: ${Math.round(dist)} km`;

    const direction = getDirection(guessedCountry, correctCoords);
    drawArrow(direction);

    lastGuess = userGuess;
  }

  function getDistance(lat1, lon1, lat2, lon2) {
    const R = 6371;  // Radius of the Earth in km
    const dLat = deg2rad(lat2 - lat1);
    const dLon = deg2rad(lon2 - lon1);
    const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) +
              Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) *
              Math.sin(dLon / 2) * Math.sin(dLon / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
    return R * c;
  }

  function deg2rad(deg) {
    return deg * (Math.PI / 180);
  }

  function getDirection(guessedCountry, correctCountry) {
    const latDiff = correctCountry.lat - guessedCountry.lat;
    const lonDiff = correctCountry.lon - guessedCountry.lon;
    const angle = Math.atan2(lonDiff, latDiff) * (180 / Math.PI);
    return angle;
  }

  function drawArrow(angle) {
    const arrow = document.getElementById('arrow');
    if (!arrow) {
      const newArrow = document.createElement('div');
      newArrow.id = 'arrow';
      newArrow.style.transform = `rotate(${angle}deg)`;
      newArrow.style.width = '50px';
      newArrow.style.height = '50px';
      newArrow.innerHTML = '➡️';
      document.body.appendChild(newArrow);
    } else {
      arrow.style.transform = `rotate(${angle}deg)`;
    }
  }
</script>