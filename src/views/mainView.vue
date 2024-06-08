<template>
    <div class="app">
        <h1>Edited</h1>
        <h5>Weather App</h5>
        <div class="mainn" v-bind:class="{ warm: state_weather }">
            <div class="search-box">
                <div class="inputs">
                    <input class="search-bar" type="text" placeholder="Search..." v-model="city" @keyup.enter="getAPI()">
                    <button @click="getAPI()">search</button>
                </div>
                <div class="data" v-if="weather">
                    <div class="header">
                        <h1>{{ weather.name }}</h1>
                        <h3>{{ new Date().toLocaleString() }}</h3>
                    </div>
                    <div class="temp">
                        <h2>{{ Math.round(weather.main.temp) }}&deg;</h2>
                    </div>
                    <div class="state">
                        <h3>{{ weather.weather[0].main }}</h3>
                    </div>
                </div>
                <div class="not-found" v-if="not_found">
                    <p>{{ not_found }}</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.app {
    height: 35em;
    width: 18em;
    border: 2px blueviolet solid;
    background-color: blueviolet;
    border-radius: 5px;
}

h5 {
    margin-top: 15px;
    margin-bottom: 15px;
    color: white;
}

input {
    font-size: 16px;
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    outline: none;
    border-radius: 5px;
}

button {
    background-color: transparent;
    color: white;
    border-radius: 5px;
}

.data {
    margin-top: 5em;
}

header {
    margin-top: 25px;
}

h1,
h3,
h2 {
    color: blue;
}

.not-found {
    color: red;
}
</style>

<script>
import axios from 'axios';

export default {
    name: 'main-view',
    data() {
        return {
            city: '',
            weather: null,
            current_day: '',
            state_weather: false,
            not_found: '',
        };
    },
    methods: {
        async getAPI() {
            if (this.city === '') {
                this.city = 'Egypt';
            }

            try {
                const getWeather = await axios.get(`https://api.openweathermap.org/data/2.5/weather?units=metric&q=${this.city}&appid=261514ec6ead072a338a344dce0fb58f`);
                this.weather = getWeather.data;
                this.city = '';
                if (this.weather.main.temp > 16) {
                    this.state_weather = true;
                    this.not_found = '';
                } else {
                    this.state_weather = false;    
                    this.not_found = '';
                }
            } catch (error) {
                if (error.response && error.response.status === 404) {
                    this.weather = null;
                    this.not_found = 'This City Not Found';
                } else {
                    console.error('Error:', error);
                    this.weather=null;
                    this.not_found=''
                }
            }
        },
    },
};
</script>
