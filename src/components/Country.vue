<template>
    <!-- Изначально я никак не мог разобраться как же реализовать запрос до того -->
    <!-- как это сделает пользователь, иначе всё ломалось и ничего не отображалось -->
    <!-- самое смешное, что я уже пробовал в created ф-ии передать аргумент -->
    <!-- и тогда по какой-то причине не сработало это, поэтому костыли -->
    <!-- я не стал удалять, а закоментил, пусть останется как напоминание -->
    <!-- <button
        class="start-btn"
        v-if="click === true"
        @click="
            getCountryDataLoad();
            clicked();
        "
    >
        START
    </button> -->
    <div class="grid">
        <input
            class="type-in-country"
            type="text"
            @keyup.enter.prevent="getCountryData(countryName)"
            v-model="countryName"
            placeholder="Country..."
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
        this.getCountryData("Russia");
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
        // getCountryDataLoad(countryName) {
        //     axios
        //         .get(`https://restcountries.com/v3.1/name/russia`)
        //         .then((response) => {
        //             console.log(Object.values(response.data)[0]);
        //             this.countryData = Object.values(response.data)[0];
        //             this.imgUrl = Object.values(response.data)[0].flags.svg;
        //         })
        //         .catch((error) => console.log(error));
        // },
    },
};
</script>

<style scoped>
.start-btn {
    width: 200px;
    height: 70px;
    border-style: none;
    border-radius: 100px;
    background-color: #4f3981;
    color: #f9d66f;
    font-size: 50px;
    cursor: pointer;
}

.type-in-country {
    margin: 20px 20px;
    width: 60%;
    height: 30px;
    border: none;
    border-radius: 20px;
    padding-left: 15px;

    color: #555;
}

.grid {
    display: grid;
    grid-template-columns: 1;
    grid-template-rows: 2;

    justify-items: center;
    align-items: center;
}
</style>
