<script setup>
import { capitalizeFirstLetter } from '../utils'

const props = defineProps({
  weatherInfo: {
    type: [Object, null],
    required: true
  }
})

const today = new Date().toLocaleString('en-EN', { weekday: 'short', year: 'numeric', month: 'long', day: 'numeric' })
</script>

<template>
  <div class="summary">
    <div :style="`background-image: url('./weather-main/${weatherInfo?.weather[0].description}.png');`" class="pic-main">
    </div>
    <div class="weather">
      <div class="temp">
        {{ Math.round(weatherInfo?.main?.temp) }} °C / {{ Math.round(weatherInfo?.main?.temp*1.8+32) }} °F
      </div>
      <div class="weather-desc text-block">
        {{ capitalizeFirstLetter(weatherInfo?.weather[0].description) }}
      </div>
    </div>
    <div class="city text-block">
      {{ weatherInfo?.name }},
      {{ weatherInfo?.sys?.country }}
    </div>
    <div class="date text-block">
      {{ today }}
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/main.scss';

.pic-main {
  width: 60px;
  height: 60px;
  margin: 20px 0 12px;
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: contain;
}

.city {
  font-size: 24px;
}

.weather {
  margin: 0 0 2em;
  padding: 1em 2em;
  border-bottom: 1px solid gold
}

.temp {
  padding-bottom: 1em;
  font-size: 2em;

}

.text-block {
  position: relative;
  padding-left: 3em;
  padding-bottom: 0.5em;
  font-size: 1em;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 25px;
    height: 25px;
    margin-right: 6px;
    background-repeat: no-repeat;
    background-position: 50% 50%;
    background-size: contain;
  }
}

.weather-desc {
  &::before {
    background-image: url('/src/assets/img/weather.svg');
  }
}

.city {
  &::before {
    background-image: url('/src/assets/img/location.svg');
  }
}

.date {
  &::before {
    left: 2px;
    width: 20px;
    height: 20px;
    background-image: url('/src/assets/img/calendar.svg');
  }
}
</style>