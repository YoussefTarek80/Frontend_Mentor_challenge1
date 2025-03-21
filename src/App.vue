<script setup>
import {ref,provide} from'vue'
import patternLines from './assets/images/pattern-lines.svg';
import circle from './assets/images/pattern-circle.svg';
import squiggly from './assets/images/pattern-squiggly-line-bottom-desktop.svg';
import squiggly_2 from './assets/images/pattern-squiggly-line-top.svg';
import FormTicket from './components/form.vue';
import ticket from './components/ticket.vue';
const added=ref(false)
const data = ref([]); 
provide('data', data);
const generateTicket = (formData) => {
    data.value = formData;
    added.value = true; 
};
</script>

<template>
    <div class="relative h-auto ">
      <img :src="squiggly_2" alt="" class="absolute top-0 right-0 z-0 sm:w-auto w-40 ">
      <img :src="patternLines" alt="" class="absolute max-h-screen w-full top-0 ">
      <img :src="squiggly" alt="" class="absolute bottom-0  sm:w-6/12 w-full -z-10 ">
      <img :src="circle" alt="" class="absolute top-60 right-36 z-0">
      <div class="">
        <div v-if="!added">
          <FormTicket v-if="!added" @generateTicket="generateTicket" />
        </div>
        <div v-else>
          <ticket  />
        </div>
      </div>

    </div>

</template>

<style scoped>

</style>
