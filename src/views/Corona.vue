<template>   
  <div class="pt-16 px-48 bg-indigo-200 h-full">
    <div class="text-3xl font-extrabold text-grayx-800">APLIKASI DATA COVID-19
      <div class="text-base">
        <select v-model="selectedCountry" style="width: 740px;" class="form-select mt-10 block border ml-28 p-2 rounded" v-if="countries" @change="getDataByCountry">
          <option value="">Global</option>
          <option v-for="country in countries" :value="country.Slug" :key="country.Slug">{{ country.Country }}</option>
        </select>
      </div>
      <div class="flex flex-row mt-20 justify-between"> 
        <div class="shadow-md text-red-600 bg-green-300 border-2 border-green-400 pt-6 text-center" style="width: 280px; height: 170px">
          <h3 class="text-4xl text-black font-extrabold pb-8">Khasus</h3>
          <div class="text-3xl font-extrabold mb-4">{{ confirmed | numberFormat }}
          </div>
        </div>
        <div class="shadow-md text-red-600 bg-green-300 border-2 border-green-400 pt-6 text-center" style="width: 280px; height: 170px">
          <h3 class="text-4xl text-black font-extrabold pb-8">Meninggal</h3>
          <div class="text-3xl font-extrabold mb-4">{{ deaths | numberFormat}}
          </div>
        </div>
        <div class="shadow-md text-red-600 bg-green-300 border-2 border-green-400 pt-6 text-center" style="width: 280px; height: 170px">
          <h3 class="text-4xl text-black font-extrabold pb-8">Sembuh</h3>
          <div class="text-3xl font-extrabold mb-4">{{ recovered | numberFormat}}
          </div>
        </div>
      </div>
      <div class="pt-48"></div> 
    </div>
  </div>   
</template>

<script>
  export default {
    data() {
      return {
        countries: {},
        selectedCountry: '',
        data: {},
        countryData: {},
        confirmed: 0,
        deaths: 0,
        recovered: 0,   
      }
    },
    filters: {
      numberFormat(number) {
        return number.toLocaleString();
      }
    },
    methods: {
      getCountries() {
        axios
          .get('https://api.covid19api.com/countries')
          .then(response => {
            this.countries = response.data; 
          })
      },
      getSummaryData() {
        axios
          .get('https://api.covid19api.com/summary')
          .then(response => {
            const data = response.data;
            this.data = data;
            this.confirmed = data.Global.TotalConfirmed;
            this.deaths = data.Global.TotalDeaths;
            this.recovered = data.Global.TotalRecovered;
          })
      },
      getDataByCountry() {
        let data = this.data.Countries.filter(country => {
          return country.Slug == this.selectedCountry;
        })
        console.log(data);
        data = data[0];
        this.confirmed = data.TotalConfirmed;
        this.deaths = data.TotalDeaths;
        this.recovered = data.TotalRecovered;
      }
    },
      mounted() {
        this.getCountries();
        this.getSummaryData()
      }
    }
</script>

<style>
    
</style> 