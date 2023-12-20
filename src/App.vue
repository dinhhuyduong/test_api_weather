<script setup>
import { ref, onMounted, computed } from 'vue';
import { API_KEY, BASE_URL } from './constants/index'
import { capitalizeFirstLetter } from './utils'
import WeatherSummary from './components/WeatherSummary.vue';
import Highlights from './components/Highlights.vue';
import Coords from './components/Coords.vue';
import Humidity from './components/Humidity.vue';

const city = ref('Hanoi')
const weatherInfo = ref(null)
const isError = computed(() => weatherInfo.value?.cod !== 200)

const videoSource = ref('');

const getWeather = async () => {
  try {
    const response = await fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`);
    const data = await response.json();
    weatherInfo.value = data;


    setVideoBackground(weatherInfo.value.weather[0].main);
  } catch (error) {
    isError.value = true;
  }
};

const setVideoBackground = (weatherCondition) => {

  weatherCondition = weatherCondition.toLowerCase();

  if (weatherCondition === 'clear') {
    videoSource.value = './videos/weather/clear_sky.mp4';
  } else if (weatherCondition === 'clouds') {
    videoSource.value = './videos/weather/cloudy.mp4';
  } else if (weatherCondition === 'rain') {
    videoSource.value = './videos/weather/rainy.mp4';
  } else if (weatherCondition === 'snow') {
    videoSource.value = './videos/weather/snow.mp4';
  } else if (weatherCondition === 'mist' || 'smoke' || 'haze' || 'dust' || 'fog' || 'sand' || 'ash' || 'squall' || 'tornado' ) {
    videoSource.value = './videos/weather/mist.mp4';
  } else if (weatherCondition === 'thunderstorm') {
    videoSource.value = './videos/weather/thunderstorm.mp4';
  } else if (weatherCondition === 'drizzle') {
    videoSource.value = './videos/weather/drizzle.mp4';
  } else {
    videoSource.value = './videos/weather/sky.mp4';
  }
};

onMounted(getWeather)
</script>

<template>
  <div class="page">

    <video :src="videoSource" autoplay loop muted playsinline class="video-background"></video>

    <main class="main">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section', 'section-left', { 'section-error': isError }]">
              <div class="info">
                <div class="city-inner">
                  <input v-model="city" type="text" class="search" @keyup.enter="getWeather">
                </div>
                <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo" />
                <div v-else class="error">
                  <div class="error-title">
                    Oops ! Something went wrong ! <br>
                    Please try again !
                  </div>
                  <div v-if="weatherInfo?.message" class="error-message">
                    {{ capitalizeFirstLetter(weatherInfo?.message) }}
                  </div>
                </div>

              </div>
            </section>
            <section v-if="!isError" class="section section-right">
              <Highlights :weatherInfo="weatherInfo" />
            </section>
          </div>
          <div v-if="!isError" class="sections">
            <Coords :coord="weatherInfo.coord" />
            <Humidity :humidity="weatherInfo.main.humidity" />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style lang="scss" scoped>
@import './assets/styles/main.scss';

.page {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 1em 0;
}

.video-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
}

.laptop {
  width: 100%;
  padding: 1em;
  // background-color: #000;
  border-radius: 25px;
}

.sections {
  display: flex;
  width: 100%;

  @media (max-width: 767px) {
    flex-direction: column;
  }
}

.section-left {
  width: 34%;
  padding-right: 1em;

  @media (max-width: 767px) {
    width: 100%;
    padding-right: 0;
  }

  &.section-error {
    min-width: 30em;
    width: auto;
    padding-right: 0;
  }
}

.section-right {
  width: 90%;
  padding-left: 0.5em;

  @media (max-width: 767px) {
    width: 100%;
    margin-top: 16px;
    padding-left: 0;
  }

}

.city-inner {
  position: relative;
  display: inline-block;
  width: 100%;

  &::after {
    content: '';
    position: absolute;
    top: 0;
    right: 1em;
    width: 2em;
    height: 2em;
    background: url('./assets/img/search.svg') no-repeat 50% 50%;
    background-size: contain;
    transform: translateY(25%);
    cursor: pointer;
  }
}

.info {
  height: 100%;
  padding: 1em;
  background: url('./assets/img/gradient-1.jpg') no-repeat 50% 50%;
  background-size: cover;
  border-radius: 25px;
}

.search {
  width: 100%;
  padding: 1em;
  font-family: 'Inter', Arial, sans-serif;
  color: $white;
  background-color: black;
  border-radius: 2em;
  border: none;
  outline: none;
  cursor: pointer;
}

.section-bottom {
  width: 50%;
  margin-top: 1em;
  z-index: 2;

  @media (max-width: 767px) {
    width: 100%;
  }
}

.right {
  margin-right: 1em;
}

.error {
  padding: 4em;

  &-title {
    font-size: 1.2em;
    text-align: center;
    font-weight: 700;
  }

  &-message {
    padding-top: 0.5em;
    font-size: 2.5em;
    text-align: center;
  }
}</style>
