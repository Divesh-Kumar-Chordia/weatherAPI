<template>
  <div id="app">
    <div :style="pageStyle" class="background-image">
      <h1 class="app-title">Weather App</h1>
      <!-- Hide the input container when weatherData is available -->
      <div v-if="!weatherData" class="input-container">
        <label for="city">Enter City: </label>
        <input type="text" v-model="city" id="city" class="city-input">
        <button @click="getWeather" :style="buttonStyle">Get Weather</button>
      </div>
      <div v-if="weatherData" class="weather-info">
        <h2 class="weather-city">Weather in {{ city }}</h2>
        <p class="weather-temperature">Temperature: {{ weatherData.main.temp }}°C</p>
        <p class="weather-description">Weather: {{ weatherData.weather[0].description }}</p>
        <p class="weather-humidity">Humidity: {{ weatherData.main.humidity }}%</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      city: '',
      apiKey: '1f9abcaa1fb8dc2d0e4b0e6637ddba44', // Replace with your API key
      weatherData: null,
      above20ImageUrl: '../src/assets/warm-bg.jpg', // Replace with the URL for above 20°C image
      below20ImageUrl: '../src/assets/cold-bg.jpg', // Replace with the URL for below 20°C image
    };
  },
  computed: {
    buttonStyle() {
      return {
        backgroundColor: this.weatherData && this.weatherData.main.temp > 20 ? 'blue' : 'orange',
        color: 'white',
        border: 'none',
        padding: '10px 20px',
        cursor: 'pointer',
        fontSize: '16px',
      };
    },
    pageStyle() {
      return {
        backgroundImage: this.weatherData && this.weatherData.main.temp > 20
          ? `url('${this.above20ImageUrl}')`
          : `url('${this.below20ImageUrl}')`,
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        backgroundAttachment: 'fixed',
        backgroundPosition: 'center center',
        backgroundColor: this.weatherData && this.weatherData.main.temp > 20 ? '#ff5722' : '#2196F3', // Background color based on temperature
        boxShadow: '0 0 10px rgba(0, 0, 0, 0.5)', // Adding shadow
        padding: '20px', // Adding padding
        minHeight: '100vh',
        fontFamily: 'Arial, sans-serif',
        color: '#333',
        transition: 'background-color 0.5s ease', // Adding background color transition animation
      };
    },
  },
  methods: {
    getWeather() {
      if (this.city === '') {
        alert('Please enter a city name.');
        return;
      }

      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${this.apiKey}`;

      fetch(apiUrl)
        .then((response) => response.json())
        .then((data) => {
          this.weatherData = data;
        })
        .catch((error) => {
          console('Error fetching weather data:', error); // Fixed console.log typo
        });
    },
  },
};
</script>

<style scoped>
/* Rest of the styles remain the same */

/* Add a box shadow to the "Get Weather" button on hover */

*{
  color:white;
  padding:5px;
}
button:hover {
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.7);
}
.background-image {
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center center;
  margin: 0;
  padding: 0;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  color: #333;
}

.app-title {
  font-size: 24px;
  text-align: center;
}

.input-container {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.city-input {
  padding: 5px;
  font-size: 16px;
}

.weather-info {
  margin-top: 20px;
  text-align: center;
  font-size: 18px;
}

.weather-city {
  font-size: 24px;
  font-weight: bold;
}

.weather-temperature {
  margin-top: 10px;
}

.weather-description {
  margin-top: 10px;
}

.weather-humidity {
  margin-top: 10px;
}

/* Style for the "Get Weather" button */
button {
  margin-top: 10px;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}
  button:hover {
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.7);
  }
</style>
