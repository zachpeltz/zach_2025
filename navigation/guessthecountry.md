---
layout: page
title: Guess the Country
permalink: /guessthecountry/
---

1. Enter the name of a country.
2. The game will tell you the distance and direction (in meters) to the correct country.
3. Keep guessing until you get the correct country.
4. Countries that you guessed will be colored in on the map.

<div id="map" style="height: 500px; width: 100%;"></div>

<p>Guess the country: <input type="text" id="guess" placeholder="Enter country name" /> <button onclick="checkGuess()">Submit</button></p>
<p id="feedback"></p>
<p id="distance"></p>
<p id="direction"></p>
<div id="arrow" style="font-size: 30px;">⬆️</div>

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
    "Andorra": { lat: 42.506285, lon: 1.521801 },
    "Angola": { lat: -11.202692, lon: 17.873887 },
    "Argentina": { lat: -38.416097, lon: -63.616672 },
    "Australia": { lat: -25.274398, lon: 133.775136 },
    "Austria": { lat: 47.516231, lon: 14.550072 },
    "Bahamas": { lat: 25.03428, lon: -77.39628 },
    "Bahrain": { lat: 26.0667, lon: 50.5577 },
    "Bangladesh": { lat: 23.6850, lon: 90.3563 },
    "Belgium": { lat: 50.8503, lon: 4.3517 },
    "Bolivia": { lat: -16.2902, lon: -63.5887 },
    "Brazil": { lat: -14.2350, lon: -51.9253 },
    "Canada": { lat: 56.1304, lon: -106.3468 },
    "Chile": { lat: -35.6751, lon: -71.5430 },
    "China": { lat: 35.8617, lon: 104.1954 },
    "Colombia": { lat: 4.5709, lon: -74.2973 },
    "Denmark": { lat: 56.2639, lon: 9.5018 },
    "Egypt": { lat: 26.8206, lon: 30.8025 },
    "Finland": { lat: 61.9241, lon: 25.7482 },
    "France": { lat: 46.6034, lon: 1.8883 },
    "Germany": { lat: 51.1657, lon: 10.4515 },
    "Greece": { lat: 39.0742, lon: 21.8243 },
    "India": { lat: 20.5937, lon: 78.9629 },
    "Iran": { lat: 32.4279, lon: 53.6880 },
    "Italy": { lat: 41.8719, lon: 12.5674 },
    "Japan": { lat: 36.2048, lon: 138.2529 },
    "Mexico": { lat: 23.6345, lon: -102.5528 },
    "Nepal": { lat: 28.3949, lon: 84.1240 },
    "Netherlands": { lat: 52.1326, lon: 5.2913 },
    "New Zealand": { lat: -40.9006, lon: 174.8860 },
    "Norway": { lat: 60.4720, lon: 8.4689 },
    "Pakistan": { lat: 30.3753, lon: 69.3451 },
    "Peru": { lat: -9.1900, lon: -75.0152 },
    "Russia": { lat: 61.5240, lon: 105.3188 },
    "Saudi Arabia": { lat: 23.8859, lon: 45.0792 },
    "South Africa": { lat: -30.5595, lon: 22.9375 },
    "Spain": { lat: 40.4637, lon: -3.7492 },
    "Sweden": { lat: 60.1282, lon: 18.6435 },
    "Switzerland": { lat: 46.8182, lon: 8.2275 },
    "Turkey": { lat: 38.9637, lon: 35.2433 },
    "United Kingdom": { lat: 55.3781, lon: -3.4360 },
    "United States": { lat: 37.0902, lon: -95.7129 },
  };

  let correctCountry = getRandomCountry();
  let lastGuess = null;
  let guessedCountries = [];

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
    const distanceElem = document.getElementById('distance');
    const directionElem = document.getElementById('direction');

    if (!countries[userGuess]) {
      feedback.innerText = "Country not found, try again!";
      return;
    }

    const guessedCountry = countries[userGuess];
    const correctCoords = countries[correctCountry];

    const dist = getDistance(guessedCountry.lat, guessedCountry.lon, correctCoords.lat, correctCoords.lon);
    feedback.innerText = `Your guess: ${userGuess}`;
    distanceElem.innerText = `Distance to correct country: ${Math.round(dist)} meters`;

    const direction = getDirection(guessedCountry, correctCoords);
    drawArrow(direction);

    // Color in the guessed country on the map
    if (!guessedCountries.includes(userGuess)) {
      L.marker([guessedCountry.lat, guessedCountry.lon]).addTo(map)
        .bindPopup(userGuess).openPopup();
      guessedCountries.push(userGuess);
    }

    // Check if the guess is correct
    if (userGuess === correctCountry) {
      feedback.innerText = `Congratulations! You guessed the correct country: ${correctCountry}`;
    }
  }

  function getDistance(lat1, lon1, lat2, lon2) {
    const R = 6371000;  // Radius of the Earth in meters
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
    arrow.style.transform = `rotate(${angle}deg)`;
  }
</script>