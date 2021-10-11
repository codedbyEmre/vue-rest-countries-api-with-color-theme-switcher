<template>
  <Header />
  <div class="container-md country-detail">
    <router-link to="/">
      <button class="my-5 bg-transparent border border-2 text-secondary fs-6 rounded py-2 fw-bold px-4">
        <i class="fas fa-long-arrow-alt-left fw-bold mx-1"></i>
        Back
      </button>
    </router-link>
    <div v-if="isLoading" class="container-md mt-5 text-center">
      <div class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
    <div v-else class="row">
      <div class="col-lg-6 py-2">
        <img :src="country.flags.png" :alt="country.name.common" class="img-fluid w-100 h-100">
      </div>
      <div class="col-lg-6 p-5">
        <h3 class="fw-bold">{{country.name.common}}</h3>
        <div class="row my-4">
          <div class="col-sm-6">
              <p>
                <span class="fw-bold">Native Name: </span> 
                <span v-for="(native,i) in country.name.nativeName" :key="i">
                  {{native.official}}  
                </span> 
              </p>
              <p><span class="fw-bold">Population: </span>{{country.population.toLocaleString()}}</p>
              <p><span class="fw-bold">Region: </span>{{country.region}}</p>
              <p><span class="fw-bold">Sub Region: </span>{{country.subregion}}</p>
              <p><span class="fw-bold">Capital: </span>{{formatCapital(country.capital)}}</p>
          </div>
          <div class="col-sm-6">
              <p>
                <span class="fw-bold">Top Level Domain: </span>
                <span v-for="(domain,i) in country.tld" :key="i">
                  {{domain}}
                </span>
              </p>
              <p>
                <span class="fw-bold">Currencies: </span>
                <span v-for="(currency,i) in country.currencies" :key="i">
                  {{formatCurrency(currency.name)}}
                </span>
              </p>
              <p>
                <span class="fw-bold">Languages: </span>
                <span v-for="(lang,i) in country.languages" :key="i">
                  {{formatLang(lang)}}
                </span>
              </p>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-12">
            <p>
              <span class="fw-bold">Border Countries: </span>
              <span class="badge bg-transparent text-secondary border border-2 p-2 mx-1 " v-for="(border,i) in country.borders" :key="i">
                {{border}}
              </span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from '../components/Header.vue'
export default {
  components: { Header },
  name: "CountryDetail",
  data(){
    return{
      country: null,
      isLoading: false
    }
  },
  methods:{
    async getCountries(){
      this.isLoading = true
      const res = await fetch(`https://restcountries.com/v3.1/all`)
      const data = await res.json()
      this.isLoading = false
      data.filter(item => {
        if(this.$route.params.name === item.name.common){
          this.country = item;
        }
      })
    },
    formatCapital(capital){
      if(capital != undefined){
          return capital + ''
      }else{
          return 'Unknown'
      }
    },
    formatCurrency(currency){
      return currency + ' '
    },
    formatLang(lang){
      return lang + ' '
    }
  },
  created(){
    this.getCountries()
  }
}
</script>

<style>
</style>