
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Online Compass</title>
<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #f0f0f0;
    flex-direction: column;
    text-align: center;
  }

  .compass-container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .compass {
    width: 200px;
    height: 200px;
    border: 5px solid #000;
    border-radius: 50%;
    position: relative;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .needle {
    width: 2px;
    height: 50%;
    background-color: red;
    position: absolute;
    top: 50%;
    left: 50%;
    transform-origin: 50% 100%;
    transform: translateX(-50%) translateY(-100%);
    transition: transform 0.3s ease-out;
  }

  .info {
    font-size: 18px;
    margin-bottom: 20px;
  }

  button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }

  footer {
    font-size: 16px;
    color: #555;
    position: absolute;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
  }
</style>
</head>
<body>

<h1>Online Compass</h1>

<div class="compass-container">
  <div class="compass">
    <div class="needle" id="needle"></div>
  </div>

  <div class="info">
    <p id="heading">Heading: 0°</p>
    <p id="location">Location: Not available</p>
    <button id="lockBtn">Lock Compass</button>
  </div>
</div>

<footer>Made with ❤ by Armeen</footer>

<script>
  const needle = document.getElementById('needle');
  const headingDisplay = document.getElementById('heading');
  const locationDisplay = document.getElementById('location');
  const lockBtn = document.getElementById('lockBtn');

  let isLocked = false;
  let currentHeading = 0;

  function updateCompass(heading) {
    if (isLocked) return;

    const rotation = 360 - heading;
    needle.style.transform = `translateX(-50%) translateY(-100%) rotate(${rotation}deg)`;
    headingDisplay.textContent = `Heading: ${Math.round(heading)}°`;
  }

  function updateLocation(position) {
    const { latitude, longitude } = position.coords;
    locationDisplay.textContent = `Location: Lat ${latitude.toFixed(4)}, Lon ${longitude.toFixed(4)}`;
  }

  function handleError(error) {
    locationDisplay.textContent = 'Location: Permission denied or unavailable';
  }

  function toggleLock() {
    isLocked = !isLocked;
    lockBtn.textContent = isLocked ? 'Unlock Compass' : 'Lock Compass';
  }

  if (window.DeviceOrientationEvent) {
    window.addEventListener('deviceorientation', (event) => {
      if (event.alpha !== null) {
        currentHeading = event.alpha;
        updateCompass(currentHeading);
      }
    });
  } else {
    headingDisplay.textContent = 'Device orientation not supported';
  }

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(updateLocation, handleError);
  } else {
    locationDisplay.textContent = 'Geolocation not supported';
  }

  lockBtn.addEventListener('click', toggleLock);
</script>

</body>
</html>
# Compass
