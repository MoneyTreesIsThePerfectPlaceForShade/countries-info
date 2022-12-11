<template>
    <button
        v-if="click === true"
        @click="
            getCountryDataLoad();
            clicked();
        "
    >
        START
    </button>
    <div v-else>
        <input
            type="text"
            @keyup.enter.prevent="getCountryData(countryName)"
            v-model="countryName"
        />

        <article class="country">
            <img class="country__img" :src="imgUrl" />
            <div class="country__data">
                <h3 class="country__name">{{ countryData.name.common }}</h3>
                <h4 class="country__region">{{ countryData.region }}</h4>
                <p class="country__row">
                    <span>👫</span
                    >{{
                        (+countryData.population / 1000000).toFixed(1) +
                        " millions"
                    }}
                </p>
                <p class="country__row">
                    <span>🗣️</span>{{ Object.values(countryData.languages)[0] }}
                </p>
                <p class="country__row">
                    <span>💰</span
                    >{{ Object.values(countryData.currencies)[0].name }}
                </p>
            </div>
        </article>
    </div>
</template>

<script>
import axios from "axios";

export default {
    name: "Country",
    // чтоб подцеплял инфу в момент создания приложения
    created() {
        this.getCountryData();
    },
    data() {
        return {
            countryData: {},
            imgUrl: "",
            countryName: "",
            click: true,
        };
    },
    methods: {
        clicked() {
            this.click = false;
        },
        getCountryData(countryName) {
            // axios
            //     .get(`https://restcountries.com/v3.1/name/russia`)
            //     .then((response) => {
            //         console.log(Object.values(response.data)[0]);
            //         this.countryData = Object.values(response.data)[0];
            //         this.imgUrl = Object.values(response.data)[0].flags.svg;
            //     })
            //     .catch((error) => console.log(error));
            axios
                .get(`https://restcountries.com/v3.1/name/${countryName}`)
                .then((response) => {
                    console.log(Object.values(response.data)[0]);
                    this.countryData = Object.values(response.data)[0];
                    this.imgUrl = Object.values(response.data)[0].flags.svg;
                })
                .catch((error) => console.log(error));
        },
        getCountryDataLoad(countryName) {
            axios
                .get(`https://restcountries.com/v3.1/name/russia`)
                .then((response) => {
                    console.log(Object.values(response.data)[0]);
                    this.countryData = Object.values(response.data)[0];
                    this.imgUrl = Object.values(response.data)[0].flags.svg;
                })
                .catch((error) => console.log(error));
        },
    },
};
</script>

<style scoped></style>
