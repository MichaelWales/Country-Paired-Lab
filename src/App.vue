<template>
  <div id="app">
    <h2>Global population: {{totalPopulation}} perps </h2>

    <label for="country_select">Select a Country:</label>
    <select id="country_select" v-model="selectedCountry">
      <option disabled value="">Select a country</option>
      <option v-for="country in countries" :value="country">{{country.name}}</option>
    </select>

    <button v-on:click="addCountry">Add Country</button>

    <div v-if="selectedCountry !== null"> {{selectedCountry["name"]}}
      <br>
      <img class="small-flag" :src="selectedCountry['flag']">
    </div>
    <h3 v-else="selectedCountry === null"> {{selectedCountry}}</h3>

    <!-- Favourite Countries goes here -->
    <div>
      Favourite Countries:
      <br>
      <p v-for="country in favouriteCountries">{{country}}</p>
      <!-- <p v-if="selectedCountry !== null" v-for="country in selectedCountry" >{{country['borders']}}</p>
      <p v-else="selectedCountry === null">{{selectedCountry}}</p> -->
    </div>


</div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      countries: [],
      selectedCountry: null,
      favouriteCountries: []
    }
  },
    computed: {
      totalPopulation: function(){
      return this.countries.reduce((total, country) => {
      return total + country.population
    }, 0);
  }
    },
    mounted(){
      this.fetchCountries();
    },
    methods: {
      fetchCountries: function(){
        fetch("https://restcountries.eu/rest/v2/all")
        .then(response => response.json())
        .then(countryData => this.countries = countryData);
      },
      addCountry: function(){
        this.favouriteCountries.push(this.selectedCountry["name"])
        this.selectedCountry = null
      }
    }
}
</script>

<style>
.small-flag {
  height: 150px
}



</style>
