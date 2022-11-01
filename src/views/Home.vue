<template>
  <main v-if="!loading" class="container mb-10">

    <DataTitle :text="title" :dataDate="dataDate" />

    <DataBoxes :stats="stats" />

    <CountrySelect @get-country="getCountryData" :countries="countries" />


    <button v-if="stats.Country" class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
      @click="clearCountryData">
      Clear Country
    </button>
  </main>
  <!-- while loading  -->
  <main v-else="" class=" flex flex-clon align-center justify-center text-center">
    <div class=" text-gray-500 text-3xl mt-10 mb-6 ">
      Fetching Data
      <img :src="require('../assets/hourglass.gif')" class=" w-24 m-auto" alt="" />
    </div>
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle.vue';
import DataBoxes from '@/components/DataBoxes.vue';
import CountrySelect from '@/components/CountrySelect.vue';
import { ref } from 'vue';

export default {
  'name': 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect
  },
  setup() {
    const loading = ref(true)
    const title = ref('Global')
    const dataDate = ref('')
    const stats = ref({})
    const countries = ref([])

    const fetchCovidData = async () => {
      let res = await fetch('https://api.covid19api.com/summary');
      return await res.json();
    };


    const getCountryData = (country) => {
      stats.value = country
      title.value = country.Countrys
    }
    const clearCountryData = async () => {
      loading.value = true
      const data = await fetchCovidData()
      title.value = 'Global'
      stats.value = data.Global
      loading.value = false
    }
    const baseSetup = async () => {
      const data = await fetchCovidData()
      dataDate.value = data.Date
      stats.value = data.Global
      countries.value = data.Countries
      loading.value = false
    };

    baseSetup();

    return {
      loading,
      title,
      dataDate,
      stats,
      countries,
      getCountryData,
      clearCountryData
    };
  }
}
</script>
