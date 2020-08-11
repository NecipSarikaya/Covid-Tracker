<template>
    <div class="container">
      <div class="row">
        <div class="col-md-6 col-md-offset-3">
          <app-card :data="global"></app-card>
          <app-chart :data="global"></app-chart>
          <br>
          <br>
          <div v-if="show" style="margin-left:250px">
            <h1 style="font-size:30px ">{{selectedCon}}</h1>
            <br>
            <h3 style="color:blue">Confirmed : {{ conData.confirmed }}</h3>
            <h3 style="color:green">Recovered : {{ conData.recovered }}</h3>
            <h3 style="color:red">Deaths : {{ conData.deaths }}</h3>
            <h3>Date : {{ conData.date }}</h3>
          </div>
          <br>
          <hr>
          <div class="small">
            <line-chart :chart-data="datacollection" style="width:500px"></line-chart>
          </div>
        </div>
      </div>
    </div>
</template>

  <script>
  import Cards from "./components/Cards"
  import Chart from "./components/Chart"
  import LineChart from './LineChart.js'
  import {eventBus} from "./main"
  
  export default {
    components:{
      appCard : Cards,
      appChart : Chart,
      LineChart
    },
    data() {
      return {
        global:{
          confirmed: null,
          recovered : null,
          deaths: null,
          date : null
        },
        dailyData:{
          dailyConfirmed : [],
          dailyRecovered : [],
          dailyDeaths : [],
          dailyDate : [],
        },
        datacollection: {},
        selectedCon : "Global",
        show : false,
        conData: {}
      }
    },
    methods:{
      getDaily(){
            fetch('https://covid19.mathdro.id/api/daily')
            .then(response => response.json())
            .then(data =>{
                console.log("data")
                console.log(data)
                for(let key =0 ; key < data.length ; key++){
                    this.dailyData.dailyConfirmed.push(data[key].confirmed.total)
                    this.dailyData.dailyRecovered.push(data[key].recovered.total)
                    this.dailyData.dailyDeaths.push(data[key].deaths.total)
                    this.dailyData.dailyDate.push(data[key].reportDate)
                }
                console.log("Chart")
                console.log(this.dailyData)
            }).catch(() =>{
                alert("Can not get the data")
            })
      },
      getData(){
        fetch('https://covid19.mathdro.id/api')
        .then(response => response.json())
        .then(data =>{
          console.log("App.vue")
          console.log(data)
          this.global.confirmed = data.confirmed.value;
          this.global.recovered = data.recovered.value;
          this.global.deaths = data.deaths.value;
          this.global.date = data.lastUpdate;
          this.global.date = this.global.date.toString();
          this.global.date = this.global.date.substr(0,10)
        }).catch(() =>{
          alert("404 Sorry we cant get the data")
        })
      },
      fillData () {
          this.datacollection = {
          labels: this.dailyData.dailyDate,
          datasets: [
            {
              label: 'Confirmed',
              backgroundColor: 'blue',
              data: this.dailyData.dailyConfirmed
            },
            {
              label: 'Deaths',
              backgroundColor: 'red',
              data: this.dailyData.dailyDeaths
            }
          ]
        }
      },
    },
    mounted() {
      this.fillData()
    },
    created(){
      
      eventBus.$on("selectedCountry",(selectedCountry)=>{
        this.selectedCon = selectedCountry
        if(this.selectedCon=="Global"){
          this.show = false
          this.getDaily();
        }
      })
      eventBus.$on("CountryData",(val)=>{
        this.conData = val
        this.show=true
      })
      this.getDaily();
      this.getData();
    }
  }
</script>

<style>

</style>
