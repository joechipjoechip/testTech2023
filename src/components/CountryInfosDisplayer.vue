<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  countryInfos: {
    type: Object,
    required: true
  }
})

</script>

<template>
    
  <section class="country-infos">

    <div class="info-wrapper">

    <h2 class="name">{{  props.countryInfos.name  }}</h2>

    <span class="flag">{{ props.countryInfos.emoji }}</span>

    <div v-if="props.countryInfos.capital">
        <p class="generic-label">Capitale :</p>
        <p>{{ props.countryInfos.capital }}</p>
    </div>

    <div v-if="props.countryInfos.currencies.filter(currency => currency !== '').length">
        <!-- .filter() due to bad api data (example : AQ (antartica) have an empty entry, 
        @TODO : to contact them to ask for cleaning or parse on our side -->
        <p class="generic-label"> Monnaie{{ props.countryInfos.currencies.filter(currency => currency !== '').length > 1 ? 's' : '' }} :</p>
        <p class="currency" v-for="currency in props.countryInfos.currencies" :key="currency">{{ currency }}</p>
    </div>

    <div v-if="props.countryInfos.languages.length">
        <p class="generic-label"> Langue{{ props.countryInfos.languages.length > 1 ? 's' : '' }} officielle{{ props.countryInfos.languages.length > 1 ? 's' : '' }} :</p>
        <p class="language" v-for="language in props.countryInfos.languages" :key="language">{{ language.name }}</p>
    </div>

    <div v-if="props.countryInfos.phones.length">
        <p class="generic-label"> Préfixe{{ props.countryInfos.phones.length > 1 ? 's' : '' }} téléphonique{{ props.countryInfos.phones.length > 1 ? 's' : '' }} :</p>
        <p class="phone" v-for="phone in props.countryInfos.phones" :key="phone">{{ phone }}</p>
    </div>

    </div>

</section>

</template>

<style lang="scss" scoped>
.country-infos {
  .name {
    font-size: 3rem;
  }

  .flag {
    display: block;
    font-size: 10rem;
  }

  .generic-label {
    color: #7c7c7c80;
    margin: 3rem auto 0.5rem auto;
  }

  .currency,
  .language,
  .phone {
    
    display: inline-block;

    &::after {
      content: ",";
      margin-right: 0.5rem;
    }

    &:last-of-type {
      &::after {
        content: "";
        display: none;
      }
    }

  }

}

</style>
