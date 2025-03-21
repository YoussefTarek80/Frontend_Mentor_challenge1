<template>
    <section class="text-white flex items-center justify-center flex-col gap-5 p-9" v-for="da in data" :key="da.email" >
        <div class="header" >
            <div class="logo"><img src="../assets/images/logo-full.svg" alt=""></div>
        </div>
        <div class="description flex items-center flex-col gap-2 justify-centers text-center">
            <div class="sm:w-7/12 sm:text-5xl text-2xl font-bold"><h1>Congrats, <span class="name">{{da.fullName}}!</span> Your ticket is ready</h1></div>
            <div class="sm:w-5/12 text-2xl text-gray-400"><span>We've emailed your ticket to <span class="name">{{da.email}}</span> and we send updates in the run up to the event.</span></div>
        </div>
        <div class="ticket relative h-96 sm:m-1 my-10">
            <img src="../assets/images/pattern-ticket.svg" alt="">
            <div class="absolute inset-0 m-5">
                <div class="logo"><img src="../assets/images/logo-full.svg" alt="" class=" sm:w-96 w-52"></div>
                <div class="date flex items-center gap-5  sm:mx-24 sm:text-lg text-sm"><span>Jan 31, 2025</span>/<span>Austin, TX</span></div>
                <div class="flex items-center gap-2 inset-1 sm:bottom-0 bottom-40 absolute">
                    <img :src="da.image" class="sm:w-20 w-10 rounded-xl" alt="">
                    <div class="flex flex-col">
                        <span class="text-2xl">{{da.fullName}}</span>
                        <span class="text-neutral-400">@{{da.github}}</span>
                    </div>
                </div>
                <div class="sm:right-0 -right-5  absolute rotate-90 sm:text-2xl text-neutral-400 sm:top-24 top-10 ">#012597</div>
            </div>
    
        </div>
    </section>
    
</template>
<script setup>
import { inject } from 'vue';
const data = inject('data');
</script>

<style scoped>
.name{
    background: linear-gradient(to right, hsl(7, 88%, 67%), white);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
</style>