---
layout: page
title: Guess the Country
permalink: /guessthecountry/
---


Welcome to the Guess the Country Game! Click on the map to guess a country. The distance from your guess to the correct country and a directional arrow will be shown below. Keep guessing until you get it right!

![World Map](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/World_map_blank_with_countries.svg/1920px-World_map_blank_with_countries.svg.png)

## Instructions

1. Click on the map to guess a country.
2. The distance and direction to the correct country will be displayed below.

<script>
// List of countries with their lat/lon coordinates
const countries = [
    {name: 'United States', lat: 37.0902, lon: -95.7129},
    {name: 'Canada', lat: 56.1304, lon: -106.3468},
    // Add all 195 countries here
];

let correctCountry = countries[Math.floor(Math.random() * countries.length)];

function getDistance(lat1, lon1, lat2, lon2) {
    const R = 6371000; // Radius of the Earth in meters
    const φ1 = lat1 * Math.PI / 180;
    const φ2 = lat2 * Math.PI / 180;
    const Δφ = (lat2 - lat1) * Math.PI / 180;
    const Δλ = (lon2 - lon1) * Math.PI / 180;

    const a = Math.sin(Δφ / 2) * Math.sin(Δφ / 2) +
              Math.cos(φ1) * Math.cos(φ2) *
              Math.sin(Δλ / 2) * Math.sin(Δλ / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));

    return R * c; // in meters
}

function getDirection(lat1, lon1, lat2, lon2) {
    const dx = lon2 - lon1;
    const dy = lat2 - lat1;
    const angle = Math.atan2(dy, dx) * 180 / Math.PI;
    const compass = ['N', 'NE', 'E', 'SE', 'S', 'SW', 'W', 'NW'];
    return compass[Math.round(((angle % 360) + 360) % 360 / 45) % 8];
}

function guessCountry(lat, lon) {
    const distance = getDistance(lat, lon, correctCountry.lat, correctCountry.lon);
    const direction = getDirection(lat, lon, correctCountry.lat, correctCountry.lon);
    document.getElementById('result').innerHTML = `Distance: ${Math.round(distance)} meters <br> Direction: ${direction}`;
}

document.querySelector('img').addEventListener('click', function(event) {
    const rect = event.target.getBoundingClientRect();
    const x = event.clientX - rect.left;
    const y = event.clientY - rect.top;
    const lat = /* Convert y coordinate to latitude */;
    const lon = /* Convert x coordinate to longitude */;
    guessCountry(lat, lon);
});
</script>

<div id="result"></div>
