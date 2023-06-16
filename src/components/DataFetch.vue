<template>
    <div>
        <h2>Current Weather</h2>

        <div v-if="error">{{ error }}</div>

        <div v-if="weatherData">
            <p>Location: {{ weatherData.location.name }}</p>
            <p>Temperature: {{ weatherData.current.temp_c }}°C</p>
            <p>Humidity: {{ weatherData.current.humidity }}%</p>
            <!-- Add more weather data properties as needed -->
        </div>
    </div>
</template>
  
<script lang="ts">
import { Vue } from 'vue-class-component';
import axios from 'axios';

export default class MyComponent extends Vue {
    latitude: number | null = null;
    longitude: number | null = null;
    weatherData: any = null;
    error: string | null = null;

    mounted() {
        this.getLocation();
    }

    getLocation() {
        console.log(navigator.geolocation)
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    this.latitude = position.coords.latitude;
                    this.longitude = position.coords.longitude;
                    this.getWeatherData();
                },
                (error) => {
                    this.error = 'Error: Unable to retrieve location.';
                }
            );
        } else {
            this.error = 'Error: Geolocation is not supported by your browser.';
        }
    }

    getWeatherData() {
        const apiKey = '0c173054ba0b44ce9a8185519231506'; // Replace with your actual weather API key
        const apiUrl = `https://api.weatherapi.com/v1/current.json`;

        axios
            .get(apiUrl, {
                params: {
                    key: apiKey,
                    q: `${this.latitude},${this.longitude}`,
                },
            })
            .then((response) => {
                this.weatherData = response.data;
                console.log(this.weatherData);
            })
            .catch((error) => {
                this.error = 'Error: Unable to fetch weather data.';
            });
    }
}
</script>
<style>
p{
    color: aqua;
}
</style>