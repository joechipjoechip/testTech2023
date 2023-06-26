<script setup>
import { reactive } from "vue"
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

import CountryInfosDisplayerVue from "./components/CountryInfosDisplayer.vue"

// QUERY LOGIC
const COUNTRY_QUERY = gql`
  query {
    countries{
      code
      name(lang: "fr")
      emoji
      capital
      currencies
      languages{
        name
      }
      phones
    }
  }
  `

const { result, loading, error } = useQuery(COUNTRY_QUERY);

// EVENT LOGIC
const currentCountryInfos = reactive({})

function handleCountrySelection( event ){

  const selectedCode = event.target.value

  if( selectedCode ){
    Object.assign(currentCountryInfos, result.value.countries.find(country => country.code === selectedCode))
  } else {
    const resetInfos = {}
    Object.keys(currentCountryInfos).forEach(key => resetInfos[key] = null)
    Object.assign(currentCountryInfos, resetInfos)
  }

}

</script>

<template>

  <p v-if="error">Oupsy...</p>

  <p v-if="loading">Chargement en cours...</p>

  <div v-else>

    <select name="countrySelector" @change="handleCountrySelection">

      <option value="">Selectionnez un pays</option>

      <option :value="country.code" v-for="country in result.countries" :key="country.code">
        {{ country.name }}
      </option>

    </select>

  </div>

  <CountryInfosDisplayerVue v-if="currentCountryInfos.code"
    :countryInfos="currentCountryInfos"
  />

</template>


<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

</style>
