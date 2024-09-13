---
layout: page
title: Guess the Country
permalink: /guessthecountry/
---

Try to guess the country! You'll get distance feedback and a direction to help you.

<canvas id="worldMap" width="800" height="400"></canvas>

<style>
  #worldMap {
    border: 1px solid black;
  }
</style>

<script>
  const countries = {
    "United States": { lat: 37.0902, lon: -95.7129 },
    "Canada": { lat: 56.1304, lon: -106.3468 },
    "Brazil": { lat: -14.2350, lon: -51.9253 },
    "India": { lat: 20.5937, lon: 78.9629 },
    "China": { lat: 35.8617, lon: 104.1954 },
    "Russia": { lat: 61.5240, lon: 105.3188 },
    "Australia": { lat: -25.2744, lon: 133.7751 },
    // Add more countries as needed
  };

  let targetCountry = getRandomCountry();
  let previousGuess = null;

  document.addEventListener('keydown', guessCountry);

  function getRandomCountry() {
    const keys = Object.keys(countries);
    return keys[Math.floor(Math.random() * keys.length)];
  }

  function guessCountry(event) {
    const guess = prompt("Enter your guess for the country:");
    if (!guess || !countries[guess]) {
      alert("Country not found. Please try again.");
      return;
    }

    const targetCoords = countries[targetCountry];
    const guessCoords = countries[guess];

    const distance = haversineDistance(guessCoords, targetCoords);
    const direction = getDirection(guessCoords, targetCoords);

    alert(`Distance: ${Math.round(distance)} km\nDirection: ${direction}`);

    previousGuess = guess;
    drawMap(guessCoords, targetCoords, direction);
  }

  function haversineDistance(coords1, coords2) {
    const toRad = (x) => (x * Math.PI) / 180;
    const R = 6371; // Radius of the earth in km

    const dLat = toRad(coords2.lat - coords1.lat);
    const dLon = toRad(coords2.lon - coords1.lon);
    const a =
      Math.sin(dLat / 2) * Math.sin(dLat / 2) +
      Math.cos(toRad(coords1.lat)) *
        Math.cos(toRad(coords2.lat)) *
        Math.sin(dLon / 2) *
        Math.sin(dLon / 2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
    return R * c; // Distance in km
  }

  function getDirection(guessCoords, targetCoords) {
    const dLon = targetCoords.lon - guessCoords.lon;
    const y = Math.sin(dLon) * Math.cos(targetCoords.lat);
    const x =
      Math.cos(guessCoords.lat) * Math.sin(targetCoords.lat) -
      Math.sin(guessCoords.lat) * Math.cos(targetCoords.lat) * Math.cos(dLon);
    const bearing = (Math.atan2(y, x) * 180) / Math.PI;
    const compassBearing = (bearing + 360) % 360;

    if (compassBearing >= 337.5 || compassBearing < 22.5) return "North";
    if (compassBearing >= 22.5 && compassBearing < 67.5) return "North-East";
    if (compassBearing >= 67.5 && compassBearing < 112.5) return "East";
    if (compassBearing >= 112.5 && compassBearing < 157.5) return "South-East";
    if (compassBearing >= 157.5 && compassBearing < 202.5) return "South";
    if (compassBearing >= 202.5 && compassBearing < 247.5) return "South-West";
    if (compassBearing >= 247.5 && compassBearing < 292.5) return "West";
    if (compassBearing >= 292.5 && compassBearing < 337.5) return "North-West";
  }

  function drawMap(guessCoords, targetCoords, direction) {
    const canvas = document.getElementById('worldMap');
    const ctx = canvas.getContext('2d');
    const img = new Image();
    img.src = 'https://upload.wikimedia.org/wikipedia/commons/8/80/World_map_-_low_resolution.svg'; // Use any world map image

    img.onload = () => {
      ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

      // Drawing guess point
      ctx.fillStyle = 'blue';
      const guessPoint = latLonToCanvas(guessCoords, canvas);
      ctx.beginPath();
      ctx.arc(guessPoint.x, guessPoint.y, 5, 0, 2 * Math.PI);
      ctx.fill();

      // Drawing target point
      ctx.fillStyle = 'red';
      const targetPoint = latLonToCanvas(targetCoords, canvas);
      ctx.beginPath();
      ctx.arc(targetPoint.x, targetPoint.y, 5, 0, 2 * Math.PI);
      ctx.fill();

      // Drawing direction arrow
      drawArrow(ctx, guessPoint, targetPoint, direction);
    };
  }

  function latLonToCanvas(coords, canvas) {
    const x = ((coords.lon + 180) * canvas.width) / 360;
    const y = ((90 - coords.lat) * canvas.height) / 180;
    return { x, y };
  }

  function drawArrow(ctx, start, end, direction) {
    ctx.strokeStyle = 'black';
    ctx.lineWidth = 2;

    // Line
    ctx.beginPath();
    ctx.moveTo(start.x, start.y);
    ctx.lineTo(end.x, end.y);
    ctx.stroke();

    // Arrowhead
    const angle = Math.atan2(end.y - start.y, end.x - start.x);
    ctx.beginPath();
    ctx.moveTo(end.x, end.y);
    ctx.lineTo(
      end.x - 10 * Math.cos(angle - Math.PI / 6),
      end.y - 10 * Math.sin(angle - Math.PI / 6)
    );
    ctx.lineTo(
      end.x - 10 * Math.cos(angle + Math.PI / 6),
      end.y - 10 * Math.sin(angle + Math.PI / 6)
    );
    ctx.closePath();
    ctx.fill();
  }
</script>