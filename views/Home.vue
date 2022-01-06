<template>
    <main v-if="!loading">
        <DataTitle :text="title" :dataDate="dataDate" />

        <DataBoxes :stats="stats" />
           
        <CountrySelect :countries="countries" @get-country="getCountryData" />

        <button @click="clearCountry" v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
            Clear Country
        </button>
    </main>

    <main class="flex flex-col align-center justify-center text-center" v-else>
        <div class="text-gray-500 text-3xl mt-10 mb-6">
            Fetching Data 
        </div>
        <img :src="loadingImage" class="w-24 m-auto" alt="" />
    </main>
</template>

<script>
import DataTitle from '../src/components/DataTitle.vue'

import DataBoxes from '../src/components/DataBoxes.vue'

import CountrySelect from '../src/components/CountrySelect.vue';

export default {
    name: 'Home',
    components: {
        DataTitle,
        DataBoxes,
        CountrySelect
    },
    data(){
      return{
          loading: true,
          title: 'Global',
          dataDate:'',
          stats:{},
          countries: [],
          loadingImage:require('../src/assets/loading.gif')
      }
    },
    methods:{
      async fetchcoviddata(){
          const res = await fetch('https://api.covid19api.com/summary')

          const data = await res.json()
          return data
      },
     
      getCountryData(country){
          this.stats = country
          this.title = country.Country

          console.log(this.title);
      },
      async clearCountry()
      {
         this.loading = true
         const data = await this.fetchcoviddata()
         this.title = 'Global'
         this.stats = data.Global
         this.loading = false
      }

    },
    async created() {
        const data = await this.fetchcoviddata()
        
        this.dataDate = data.Date
        this.stats = data.Global
        this.countries = data.Countries
        this.loading = false
    }
}
</script>