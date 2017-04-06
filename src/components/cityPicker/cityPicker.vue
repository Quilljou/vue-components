<template lang="html">
    <div>
        <select class="" name="" v-model="selectedProvince">
            <option v-for="item in provinces" :value="item" v-text="item"></option>
        </select>
        <select class="" name="" v-model="selectedCity">
            <option v-for="item in cities" :value="item" v-text="item"></option>
        </select>
        <select class="" name="" v-model="selectedCounty">
            <option v-for="item in counties" :value="item" v-text="item"></option>
        </select>
    </div>
</template>

<script>
import cityData from './address3.json';
export default {
    props: [

    ],
    data () {
        return {
            selectedProvince: null,
            selectedCity: null,
            selectedCounty: null,
            provinces: [],
            cities: [],
            counties: []
        }
    },
    watch: {
        selectedProvince (val,oldVal) {
            var excludes = ['上海市','北京市','重庆市','天津市'],
                provice = this.selectedProvince;

                if(excludes.indexOf(provice) > -1) {
                    this.cities = this.concatCounty(cityData[provice]);
                }else {
                    this.cities = Object.keys(cityData[provice]);
                }
            this.selectedCity = this.cities[0];
        },
        selectedCity  (val,oldVal) {
            var city = this.selectedCity,
                provice = this.selectedProvince;
                this.counties = cityData[provice][city];
                if(this.counties) {
                    // this.counties = this.concatCounty();
                    this.selectedCounty = this.counties[0];
                }
        }
    },
    methods: {
        concatCounty (obj) {
            var result = [],
                children = Object.keys(obj);
            children.forEach(function(item){
                obj[item].forEach(function(i){
                    result.push(i);
                })
            })
            return result;
        },
        level () {
        }
    },
    created () {
        this.provinces = Object.keys(cityData);
        this.selectedProvince = this.provinces[0];
    },
    mounted () {
        this.selectedProvince = this.provinces[0];
    }
}
</script>

<style lang="css">
</style>
