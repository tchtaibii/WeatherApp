<template>
  <div v-if="weatherData" class="Widget">
    <h2 class="city">{{ weatherData.location.name }}</h2>
    <h1 class="degree">{{ weatherData.current.temp_c }}°C</h1>
    <h6 class="status">{{ weatherData.current.condition.text }}</h6>
    <div class="hoursStatus">
      <div v-for="(e, index) in datesObj" :key="index">
        <HourStatus :hour="e.time" :tempC="e.tempC" :imgP="e.img" />
      </div>
    </div>
    <div class="details">
      <h1>Details</h1>
      <div class="setup">
        <div class="timeinD"><img :src="up" alt="" srcset="">{{ sunSet }}</div>
        <img :src="circle" alt="" srcset="">
        <div class="timeinD"><img :src="set" alt="" srcset="">{{ sunUp }}</div>
      </div>

    </div>
    <div class="more">
      <div v-for="(value, key) in more" :key="key" class="moreD">
        <div class="val">{{ value }}</div>
        <div class="key">{{ key }}</div>
      </div>
    </div>

  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import HourStatus from './HourStatus.vue';
import axios from 'axios';
@Options({
  components: {
    HourStatus
  },
  props: {

  }
})

export default class WidgetContent extends Vue {
  latitude: number | null = null;
  longitude: number | null = null;
  weatherData: any = null;
  error: string | null = null;
  datesObj: object[] | null = null;
  up: any = require('@/assets/up.svg');
  set: any = require('@/assets/set.svg');
  circle: any = require('@/assets/circle.svg');
  more: any = { RealFeel: '18°C', Humidity: '63%', w_Force: '3', Pressure: '1012hPa' };
  sunSet: any  = '00:00';
  sunUp: any  = '00:00';
  dateNow: any = 0;

  mounted() {
    this.getLocation();
  }

  getLocation() {
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
    const apiUrl = `http://api.weatherapi.com/v1/forecast.json`;

    axios
      .get(apiUrl, {
        params: {
          key: apiKey,
          q: `${this.latitude},${this.longitude}`,
          days: 1,
          aqi: 'yes',
          alerts: 'no'
        },
      })
      .then((response) => {
        this.weatherData = response.data;
        this.more.Humidity = this.weatherData.current.humidity + '%';
        this.more.RealFeel = this.weatherData.current.feelslike_c + '°C';
        this.more.Pressure = this.weatherData.current.pressure_mb + 'hPa';
        this.more.w_Force = this.weatherData.current.wind_mph;
        this.sunSet = this.weatherData.forecast.forecastday[0].astro.sunset;
        this.sunUp = this.weatherData.forecast.forecastday[0].astro.sunrise;
        this.dateNow = parseInt(this.weatherData.location.localtime.slice(11, 13));
        this.datesObj = Array.from(this.weatherData.forecast.forecastday[0].hour).map((e: any, index) => {
          return {
            time: e.time.slice(11, 13),
            tempC: e.temp_c.toString(),
            img: e.condition.icon
          };
        });
        var neA = Array.from(this.datesObj.filter((e: any) => e.time >= this.dateNow).slice(0, 5));
        if (neA.length < 5) {
          var j = 0;
          for (let i = 5; i >= neA.length; i--) {
            neA.push(this.datesObj[j++]);
          }
          console.log(neA)
        }
        this.datesObj = neA;
      })
      .catch((error) => {
        this.error = 'Error: Unable to fetch weather data.';
      });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import '../assets/style.scss';

.Widget {
  width: toRem(648);
  height: toRem(760);
  background: linear-gradient(143.98deg, rgba(255, 255, 255, 0.11) 0%, rgba(255, 255, 255, 0.22) 100%);
  backdrop-filter: blur(toRem(25));
  border-radius: toRem(30);
  margin: auto;
  display: flex;
  flex-direction: column;
  padding: toRem(40);
  align-items: center;
  justify-content: space-between;

  .city {
    font-family: 'Sarabun';
    font-style: normal;
    font-weight: 400;
    font-size: toRem(40);
    color: #FFFFFF;
  }

  .degree {
    font-family: 'Sarabun';
    font-style: normal;
    font-weight: 400;
    font-size: toRem(100);
    color: #FFFFFF;
  }

  .status {
    font-family: 'Sarabun';
    font-style: normal;
    font-weight: 300;
    font-size: toRem(30);
    text-align: center;
    color: #FFFFFF;
  }

  .hoursStatus {
    display: flex;
    gap: toRem(50);
    margin-top: toRem(60);
    align-items: center;
    padding-bottom: toRem(26);
    border-bottom: 1px solid #7A7878;
  }

  .details {
    width: toRem(500);

    h1 {
      font-family: 'Sarabun';
      font-style: normal;
      font-weight: 500;
      font-size: toRem(25);
      color: #FFFFFF;
    }

    .setup {
      display: flex;
      width: 100%;
      justify-content: center;
      align-items: flex-end;
      gap: toRem(44);

      .timeinD {
        font-family: 'Sarabun';
        font-style: normal;
        font-weight: 400;
        font-size: toRem(15);
        display: flex;
        align-items: center;
        flex-direction: column;
        justify-content: center;
        text-align: center;
        color: #FFFFFF;
      }
    }
  }

  .more {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: toRem(30);

    .moreD {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: toRem(5);

      .val {
        font-family: 'Sarabun';
        font-style: normal;
        font-weight: 500;
        font-size: toRem(25);
        color: #FFFFFF;
      }

      .key {
        font-family: 'Sarabun';
        font-style: normal;
        font-weight: 500;
        font-size: toRem(15);
        color: #B7B7B7;
      }
    }
  }
}
</style>
