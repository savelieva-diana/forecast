<script setup>
import { ref, computed } from 'vue';
import { API_KEY, BASE_URL } from './constants';
import WeatherSummary from './components/WeatherSummary.vue';
import { capitalizeFirstLetter } from './utils';
import HighLights from './components/HighLights.vue';
import Coords from './components/TheCoords.vue';
import Humidity from './components/TheHumidity.vue';
import Loading from '@/components/TheLoading.vue';

const city = ref('Spain');
const weatherInfo = ref(null);
const isLoading = ref(true);
const isError = computed(() => weatherInfo.value?.cod !== 200);

function getWeather() {
  fetch(`${BASE_URL}?q=${city.value}&units=metric&appid=${API_KEY}`)
    .then((response) => response.json())
    .then((data) => {
      weatherInfo.value = data;
      isLoading.value = false;
    });
}

getWeather();
</script>

<template>
  <div class="page">
    <main class="main" v-if="!isLoading">
      <div class="container">
        <div class="laptop">
          <div class="sections">
            <section :class="['section', 'section - left', { 'section-error': isError }]">
              <div class="info">
                <div class="city-inner">
                  <input v-model="city" type="text" class="search" @keyup.enter="getWeather" />
                  <i class="city-inner__icon" @click="getWeather" />
                </div>
                <WeatherSummary v-if="!isError" :weatherInfo="weatherInfo" />
                <div class="error">
                  <p class="error-title">Oooops! Something went wrong</p>
                  <p v-if="weatherInfo?.message" class="error-message">
                    {{ capitalizeFirstLetter(weatherInfo?.message) }}
                  </p>
                </div>
              </div>
            </section>

            <section v-if="!isError" class="section__right">
              <HighLights :weatherInfo="weatherInfo" />
            </section>
          </div>

          <div v-if="!isError" class="sections">
            <Coords :coord="weatherInfo.coord" />
            <Humidity :humidity="weatherInfo.main.humidity" />
          </div>
        </div>
      </div>
    </main>
    <Loading :loading="isLoading" />
  </div>
</template>

<style lang="scss" scoped>
@import './assets/styles/main';

.page {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px 0;
  background-color: #59585d;
}

.laptop {
  width: 100%;
  padding: 20px;
  background-color: #0e100f;
  border-radius: 25px;
}

.sections {
  display: flex;
  width: 100%;

  @media (max-width: 767px) {
    flex-direction: column;
  }
}

.section {
  &__left {
    width: 30%;
    padding-right: 10px;

    @media (max-width: 767px) {
      width: 100%;
      padding-right: 0;
    }
  }

  &__right {
    width: 70%;
    padding-left: 10px;

    @media (max-width: 767px) {
      width: 100%;
      margin-top: 16px;
      padding-left: 0;
    }
  }

  &__bottom {
    width: 50%;
    margin-top: 16px;

    @media (max-width: 767px) {
      width: 100%;
    }
  }
}

.city-inner {
  position: relative;
  display: inline-block;
  width: 100%;

  &__icon {
    position: absolute;
    top: 0;
    right: 10px;
    width: 25px;
    height: 25px;
    background: url('./assets/img/search.svg') no-repeat 50% 50%;
    background-size: contain;
    transform: translateY(50%);
    cursor: pointer;
  }
}

.info {
  height: 100%;
  padding: 16px;
  background: url('./assets/img/gradient-1.jpg') no-repeat 50% 50%;
  background-size: cover;
  border-radius: 25px;
}

.search {
  width: 100%;
  padding: 16px;
  font-family: 'Inter', Arial, sans-serif;
  color: $white;
  background-color: rgba(0, 0, 0, 0.75);
  border-radius: 16px;
  border: none;
  outline: none;
  cursor: pointer;
}
</style>
