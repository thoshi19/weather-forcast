# weather-forcastCreating a web application to check the weather forecast involves several steps, including setting up the HTML structure, using CSS for styling, JavaScript for interactivity, and integrating a weather API to fetch real-time data. Here's a step-by-step guide:

### 1. HTML Structure

html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather Forecast</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Weather Forecast</h1>
        <div class="search-container">
            <input type="text" id="location-input" placeholder="Enter your location">
            <button id="search-button">Get Weather</button>
            <button id="detect-button">Detect Location</button>
        </div>
        <div id="weather-container"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>


### 2. CSS Styling (styles.css)

css
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

.container {
    width: 80%;
    margin: 50px auto;
    text-align: center;
}

.search-container {
    margin-bottom: 20px;
}

input[type="text"] {
    padding: 10px;
    width: 200px;
}

button {
    padding: 10px;
    margin-left: 10px;
    cursor: pointer;
}

#weather-container {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
}

.weather-card {
    background-color: #fff;
    padding: 20px;
    margin: 10px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 200px;
    text-align: center;
}


### 3. JavaScript for Interactivity and API Integration (script.js)

You will need to sign up for an API key from a weather service like OpenWeatherMap. For this example, we'll use OpenWeatherMap's API.

javascript
const apiKey = 'YOUR_API_KEY';
const apiUrl = 'https://api.openweathermap.org/data/2.5/forecast';

document.getElementById('search-button').addEventListener('click', () => {
    const location = document.getElementById('location-input').value;
    fetchWeather(location);
});

document.getElementById('detect-button').addEventListener('click', () => {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
            const { latitude, longitude } = position.coords;
            fetchWeatherByCoordinates(latitude, longitude);
        });
    } else {
        alert('Geolocation is not supported by this browser.');
    }
});

function fetchWeather(location) {
    fetch(`${apiUrl}?q=${location}&units=metric&cnt=5&appid=${apiKey}`)
        .then(response => response.json())
        .then(data => displayWeather(data))
        .catch(error => console.error('Error fetching weather data:', error));
}

function fetchWeatherByCoordinates(lat, lon) {
    fetch(`${apiUrl}?lat=${lat}&lon=${lon}&units=metric&cnt=5&appid=${apiKey}`)
        .then(response => response.json())
        .then(data => displayWeather(data))
        .catch(error => console.error('Error fetching weather data:', error));
}

function displayWeather(data) {
    const weatherContainer = document.getElementById('weather-container');
    weatherContainer.innerHTML = '';

    if (data.cod === '200') {
        data.list.forEach(forecast => {
            const weatherCard = document.createElement('div');
            weatherCard.className = 'weather-card';
            
            const date = new Date(forecast.dt * 1000).toLocaleDateString();
            const temp = forecast.main.temp.toFixed(1);
            const description = forecast.weather[0].description;
            const icon = forecast.weather[0].icon;

            weatherCard.innerHTML = `
                <h3>${date}</h3>
                <img src="http://openweathermap.org/img/wn/${icon}.png" alt="${description}">
                <p>${temp}°C</p>
                <p>${description}</p>
            `;

            weatherContainer.appendChild(weatherCard);
        });
    } else {
        weatherContainer.innerHTML = '<p>Location not found. Please try again.</p>';
    }
}


### Explanation

1. *HTML Structure:* 
   - A simple structure with a header, input field, buttons for searching and detecting location, and a container to display weather data.

2. *CSS Styling:* 
   - Basic styling to ensure the layout is clean and visually appealing.

3. *JavaScript:* 
   - *API Key and URL:* Replace 'YOUR_API_KEY' with your actual OpenWeatherMap API key.
   - *Event Listeners:* For the search button to fetch weather based on user input and for the detect button to fetch weather based on geolocation.
   - *Fetch Functions:* fetchWeather for fetching data based on location name and fetchWeatherByCoordinates for fetching data based on coordinates.
   - *Display Function:* displayWeather to update the DOM with weather data.



