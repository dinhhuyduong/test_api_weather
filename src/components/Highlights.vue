<script setup>
import { computed } from 'vue';
import { getPressureMm, getTime } from '../utils'

const props = defineProps({
    weatherInfo: {
        type: [Object, null],
        required: true
    },
});

const timezone = computed(() => props.weatherInfo?.timezone)

const sunriseTime = computed(() => {
    return getTime(props.weatherInfo?.sys?.sunrise + timezone.value)
})

const sunsetTime = computed(() => {
    return getTime(props.weatherInfo?.sys?.sunset + timezone.value)
})
</script>

<template>
    <div class="section highlights">
        <div class="title">
            Today's Weather
        </div>
        <div class="highlights-wrapper">
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Wind Speed
                    </div>
                    <div class="card-pic card-pic--wind"></div>
                    <div class="card-info">
                        <div class="card-justify">
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ weatherInfo?.wind?.speed }}
                                </div>
                                <div class="info-main-text">
                                    m/s
                                </div>
                            </div>
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ weatherInfo?.wind?.deg }}
                                </div>
                                <div class="info-main-text">
                                    deg
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Wind gusts
                    </div>
                    <div class="card-small-info">
                        <div v-if="weatherInfo?.wind?.gust" class="card-small-data">
                            <div class="info-main-num">
                                {{ Math.round(weatherInfo?.wind?.gust) }}
                            </div>
                            <div class="info-main-text">
                                m/s
                            </div>
                        </div>
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--wind"></div>
                            <div class="card-small-text text-egorova">
                                Learn
                                <a href="https://www.windy.com/articles/weather-phenomena-what-s-the-difference-between-sustained-winds-and-wind-gusts-10390?satellite,7.787,115.115,5"
                                    target="_blank">more</a>
                                about gusts
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Atmospheric Pressure
                    </div>
                    <div class="card-pic card-pic--pressure"></div>
                    <div class="card-info">
                        <div class="card-centered">
                            <div class="info-main">
                                <div class="info-main-num">
                                    {{ getPressureMm(weatherInfo?.main?.pressure) }}
                                </div>
                                <div class="info-main-text">
                                    mm
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Feels like
                    </div>
                    <div class="card-small-info">
                        <div class="card-small-data">
                            <div class="info-main-num">
                                {{ Math.round(weatherInfo?.main?.feels_like) }}
                            </div>
                            <div class="info-main-text">
                                °C 
                            </div> 
                        </div>
                        <!-- <div> or </div>
                        <div class="card-small-data">
                            <div class="info-main-num">
                                {{ Math.round(weatherInfo?.main?.feels_like*1.8+32) }}
                            </div>
                            <div class="info-main-text">
                                °F
                            </div>
                        </div> -->
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--margin card-small-pic--pressure">
                            </div>
                            <div class="card-small-text">
                                How hot or cold it really feels
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="highlight">
                <div class="card">
                    <div class="card-title">
                        Sunrise and sunset time
                    </div>
                    <div class="card-pic card-pic--sun"></div>
                    <div class="card-info">
                        <div class="states">
                            <div class="state">
                                <div class="state-pic"></div>
                                <div class="state-title">
                                    Sunrise
                                </div>
                                <div class="state-time">
                                    {{ sunriseTime }}
                                </div>
                            </div>
                            <div class="state">
                                <div class="state-pic state-pic--flipped"></div>
                                <div class="state-title">
                                    Sunset
                                </div>
                                <div class="state-time">
                                    {{ sunsetTime }}
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="card-small">
                    <div class="card-small-title">
                        Cloudiness
                    </div>
                    <div class="card-small-info">
                        <div class="card-small-data">
                            <div class="info-main-num">
                                {{ weatherInfo?.clouds?.all }}
                            </div>
                            <div class="info-main-text">
                                %
                            </div>
                        </div>
                        <div class="card-small-hint">
                            <div class="card-small-pic card-small-pic--sun"></div>
                            <div class="card-small-text">
                                The sky fraction obscured by clouds
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/main.scss';

.highlights {
    padding: 2em 1em;
    background: url('/src/assets/img/gradient-4.jpg') no-repeat 0% 0%;
    background-size: cover;
    border-radius: 25px;

    &-wrapper {
        display: flex;
        justify-content: space-between;

        @media (max-width: 575px) {
            flex-direction: column;
        }
    }
}

.title {
    padding-bottom: 0.4em;
    font-size: 1.5em;
    text-align: center;
}

.highlight {
    width: 33%;

    @media (max-width: 575px) {
        width: 100%;
        margin-bottom: 16px;
    }
}

.card {
    min-height: 15em;
    padding: 1em;
    background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%;
    background-size: cover;
    border-radius: 2em;

    @media (max-width: 1199px) {
        padding: 12px;
    }

    &-centered {
        display: flex;
        justify-content: center;
        margin-top: 2.5em;
    }

    &-justify {
        display: flex;
        justify-content: space-between;
        margin-top: 2.5em;
    }


    &-title {
        padding-bottom: 0.3em;
        font-size: 1em;
        text-align: center;

        @media (max-width: 1199px) {
            font-size: 12px;
        }
    }

    &-pic {
        width: 100%;
        height: 6em;
        margin-bottom: 1em;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: contain;

        &--wind {
        background-image: url('/src/assets/img/wind.png');
    }

    &--pressure {
        background-image: url('/src/assets/img/pressure.png');
    }

    &--sun {
        background-image: url('/src/assets/img/sun-moving.png');
    }
    }
 
}

.states {
    display: flex;
    justify-content: space-between;

    &--margin {
        margin-top: 40px;
    }
}

.state {
    width: 40%;

    &:last-child {
        text-align: right;
    }

    &-pic {
        width: 20px;
        height: 20px;
        margin-bottom: 6px;
        background: url('/src/assets/img/sun.svg') no-repeat 50% 50%;
        background-size: cover;

        &--flipped {
            margin-left: auto;
            -webkit-transform: scaleX(-1);
            transform: scaleX(-1);
        }
    }

    &-title {
        font-size: 1em;
        color: $gold;
    }

    &-time {
        font-size: 1em;
        font-weight: 700;

        @media (max-width: 1199px) {
            font-size: 11px;
        }
    }
}

.info-main {
    display: flex;
    align-items: flex-end;

    &:last-child {
        text-align: right;
    }

    &-num {
        font-size: 2em;

        @media (max-width: 1199px) {
            font-size: 18px;
        }
    }

    &-text {
        padding-left: 0.2em;
        padding-bottom: 0.4em;
        font-size: 1em;
        color: rgba($white, 0.75);

        @media (max-width: 1199px) {
            padding-bottom: 1.5px;
            font-size: 12px;
        }
    }
}

.card-small {
    margin-top: 1em;
    padding: 1.5em 1em;
    background: url('/src/assets/img/gradient-2.jpg') no-repeat 50% 50%;
    background-size: cover;
    border-radius: 1.5em;

    &-title {
        font-size: 1em;
    }

    &-info {
        display: flex;
        justify-content: space-between;
        align-items: center;

        @media (max-width: 1199px) {
            flex-direction: column;
            align-items: flex-start;
        }
    }

    &-pic {
        width: 1em;
        height: 1em;
        background-repeat: no-repeat;
        background-position: 50% 50%;
        background-size: contain;

        @media (max-width: 1199px) {
            display: none;
        }

        &--margin {
            width: 1em;
            height: 1em;
            margin-bottom: 0.2em;
        }

        &--wind {
            background-image: url('/src/assets/img/gusts.svg');
        }

        &--pressure {
            background-image: url('/src/assets/img/humidity.svg');
        }

        &--sun {
            background-image: url('/src/assets/img/cloud.svg');
        }
    }

    &-data {
        display: flex;
        align-items: flex-end;
        width: 60%;

        @media (max-width: 1199px) {
            width: 100%;
            padding-top: 8px;
        }
    }

    &-hint {
        width: 75%;

        @media (max-width: 1199px) {
            width: 100%;
        }
    }

    &-text {
        font-size: 0.7em;
        line-height: 1.3;
        color: rgba($white, 0.6);

        @media (max-width: 1199px) {
            min-height: 22px;
            font-size: 9px;
        }
    }
}
</style>