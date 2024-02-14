<template>
   <div class="container">
    <Header title="PUBLIC HOLIDAY FINDER" />
    <HolidayStats title="STATS"/>
    <p>Currently selected country: <span>{{ activeCountry.name }}</span></p>
    <div class="card" v-for="holiday in holidayRender" :key="holiday.name">
      <div class="card-header">
        <h3>{{ holiday.name }}</h3>
      </div>
      <div class="card-body">
        <p>Date: {{ formatDate(holiday.datetime) }}</p>
      </div>
    </div>
    <CountrySelector title="COUNTRIES" @selected-country="setCountry" :countries="countries" />
  </div>
</template>

<script setup>

import { onMounted, reactive, ref } from 'vue'

import Header from './components/Header.vue'
import HolidayStats from './components/HolidayStats.vue'
import CountrySelector from './components/CountrySelector.vue'


const countries = ref([])

onMounted(async () => {
  try {
    const response = await fetch('https://restcountries.com/v3.1/all');
    const data = await response.json();
    countries.value = data.map(country => ({
      code: country.cca2,
      name: country.name.common
    }));
  } catch (error) {
    console.error("Failed to fetch countries:", error);
  }
})


const activeCountry = reactive( {
  code: '',
  name: ''
} )


function extractHolidayDetails(apiResponse) {
  const holidays = apiResponse.response.holidays;
  const holidayDetails = holidays.map(holiday => ({
    name: holiday.name,
    datetime: holiday.date?.datetime
  }));
  return holidayDetails;
}

const holidayRender = ref([])

const setCountry = async (country) => {
  activeCountry.code = country.code;
  activeCountry.name = country.name;

  try {
    const response = await fetch(`https://calendarific.com/api/v2/holidays?&api_key=wAt78Sh1xawra3r10Krj1XcdJB272aFQ&country=${activeCountry.code}&year=2024`)
    if (response.ok) {
      const data = await response.json()
      holidayRender.value = extractHolidayDetails(data);
      console.log(holidayRender.value)
    } else {
      throw new Error('Failed to fetch holidays')
    }
  
  } catch (error) {
    console.error("API call error:", error.message)
  }
}

function formatDate(datetime) {
  if (!datetime) return '';
  // Assuming datetime is an object like { year: 2024, month: 1, day: 1 }
  const { year, month, day } = datetime;
  // Format the date as YYYY-MM-DD. Adjust the formatting as needed.
  return `${year}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
}


/*
watch(activeCountry.code,
      async () => {
  try {
    const response = await fetch(`https://calendarific.com/api/v2/holidays?&api_key=wAt78Sh1xawra3r10Krj1XcdJB272aFQ&country=${activeCountry.code}&year=2024`)
    if (response.ok) {
      const data = await response.json()
      console.log(data)
    } else {
      throw new Error('Failed to fetch holidays')
    }
  
  } catch (error) {
    console.error("API call error:", error.message)
  }

  
})
*/
</script>

<style> 

p {
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }

    .container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.card {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  margin-bottom: 20px;
  overflow: hidden;
}

.card-header h3 {
  margin: 0;
  padding: 20px;
  background: #007bff;
  color: white;
}

.card-body {
  padding: 20px;
  line-height: 1.5;
  color: #333;
}

p {
  margin: 0 0 10px;
}

span {
  font-weight: bold;
}    

</style>
