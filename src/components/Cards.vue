<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 col-md-offset-3">
                <br>
                <ul class="list-group list-group-horizontal-md">
                    <li class="list-group-item list-group-item-primary">
                        <div class="card" style="width: 18rem;">
                            <div class="card-body">
                                <h5 class="card-subtitle mb-2 text-muted">Infected</h5>
                                <h6 class="card-title">{{data.confirmed}}</h6>
                                <p class="card-text">The active cases of COVID-19</p>
                                <p class="card-text">{{data.date}}</p>
                            </div>
                        </div>
                    </li>
                    <li class="list-group-item list-group-item-success">
                        <div class="card" style="width: 18rem;">
                            <div class="card-body">
                                <h5 class="card-subtitle mb-2 text-muted">Recovered</h5>
                                <h6 class="card-title">{{data.recovered}}</h6>
                                <p class="card-text">The recovered cases of COVID-19</p>
                                <p class="card-text">{{data.date}}</p>
                            </div>
                        </div>
                    </li>
                    <li class="list-group-item list-group-item-danger">
                       <div class="card" style="width: 18rem;">
                            <div class="card-body">
                                <h5 class="card-subtitle mb-2 text-muted">Deaths</h5>
                                <h6 class="card-title">{{data.deaths}}</h6>
                                <p class="card-text">The death cases of COVID-19</p>
                                <p class="card-text">{{data.date}}</p>
                            </div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props : ["data"],
        methods:{
           
        }
    }
</script>

<style scoped>
    li{
       padding: 20px;
       margin-right: 15px;
    } 
</style>
