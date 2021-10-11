<template>
  <div class="container-md mt-5">
    <div class="row d-flex justify-content-between">
        <div class="col-lg-4 col-md-5">
            <div class="input-group shadow-sm">
                <span class="input-group-text border-end-0">
                    <i class="fas fa-search"></i>
                </span>
                <input type="text" class="form-control py-2 border-start-0" placeholder="Search for a country..." v-model="search" @keyup="searchByCountry">
            </div>
        </div>

        <div class="col-xl-2 col-md-4 mt-md-0 mt-3">
            <select class="form-select py-2 text-capitalize shadow-sm" v-model="filter" @click="filterByRegion">
                <option value="" disabled>Filter by Region</option>
                <option :value="region" v-for="(region,index) in regions" :key="index">
                    {{region}}
                </option>
            </select>
        </div>
    </div>
  </div>
  
  <div v-if="isLoading" class="container-md mt-5 text-center">
    <div class="spinner-border mt-5" role="status">
        <span class="visually-hidden">Loading...</span>
    </div>
  </div>
  <div v-else class="container-md mt-5 d-flex flex-wrap justify-content-sm-between justify-content-center">
      <div class="card shadow-sm mb-5 pb-3 mx-md-0 mx-3" v-for="(country,index) in countries" :key="index">
        <router-link :to="'/country/' + country.name.common" class="country-link">
            <img :src="country.flags.png" class="card-img-top" :alt="country.name.common">
            <div class="card-body">
                <h5 class="card-title mb-3 fw-bold">{{country.name.common}}</h5>
                <p class="card-text my-1"> <span class="fw-bold">Population: </span>{{country.population.toLocaleString()}}</p>
                <p class="card-text my-1"><span class="fw-bold">Region: </span>{{country.region}}</p>
                <p class="card-text"><span class="fw-bold">Capital: </span>{{formatCapital(country.capital)}}</p>
            </div>
        </router-link>
      </div>
  </div>
</template>

<script>
export default {
    name: "Countries",
    data(){
        return{
            isLoading: false,
            countries: [],
            regions: ['asia', 'africa', 'americas', 'europe', 'antarctic', 'oceania'],
            filter: "",
            search: ""
        }
    },
    methods:{
        async getCountries(){
            this.isLoading = true
            const res = await fetch(`https://restcountries.com/v3.1/all`)
            const data = await res.json()
            this.isLoading = false
            this.countries = data
        },
        formatCapital(capital){
            if(capital != undefined){
                return capital + ''
            }else{
                return 'Unknown'
            }
        },
        async filterByRegion(){
            this.search = ''
            const res = await fetch(`https://restcountries.com/v3.1/region/${this.filter}`)
            const data = await res.json()
            this.countries = data
        },
        async searchByCountry(){
            this.filter = ''
            const res = await fetch(`https://restcountries.com/v3.1/name/${this.search}`)
            const data = await res.json()
            if(this.search !== ''){
                this.countries = data
            }else{
                this.getCountries()
            }
        }
    },
    mounted(){
        this.getCountries()
    }
}
</script>

<style>

</style>