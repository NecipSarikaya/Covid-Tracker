<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <hr>
                <br>
                <select class="custom-select mr-sm-2" style="width:300px" v-model="selectedCountry" @change="getCountryData(selectedCountry)">
                    <option v-for="country in countryList">{{country}}</option>
                </select>
            </div>
        </div>
    </div>
</template>

<script>
    import {eventBus} from "../main"
    export default {
        props :["data"],
        data() {
            return {
                countryList : ["Global"],
                selectedCountry: null,
                country:{
                    confirmed : null,
                    recovered : null,
                    deaths : null,
                    date : null
                }
            }
        },
        methods:{
            getCountry(){
                fetch('https://covid19.mathdro.id/api/countries')
                .then(response => response.json())
                .then(data =>{
                    let countries= Object.values(data)
                    for(let key =0 ; key < countries[0].length ; key++){
                        this.countryList.push(countries[0][key].name)
                    }
                    console.log("Country")
                    console.log(this.countryList)
                }).catch(() =>{
                    alert("Country information is missing")
                })
            },
            getCountryData(selectedCountry){
                if(selectedCountry == "Global"){
                    eventBus.$emit("selectedCountry","Global");
                }else{
                    fetch('https://covid19.mathdro.id/api/countries/'+selectedCountry)
                    .then(response => response.json())
                    .then(data =>{
                        this.country.confirmed = data.confirmed.value;
                        this.country.recovered = data.recovered.value;
                        this.country.deaths = data.deaths.value;
                        this.country.date = data.lastUpdate;
                        this.country.date = this.country.date.toString();
                        this.country.date = this.country.date.substr(0,10)
                        console.log("Countryy")
                        console.log(this.country)
                        eventBus.$emit("CountryData",this.country)
                        eventBus.$emit("selectedCountry",this.selectedCountry)
                    }).catch(() =>{
                        alert("Cannot found the country")
                    })
                } 
            }
        },
        created(){
            this.getCountry();
        }
    }
</script>

<style>

</style>
