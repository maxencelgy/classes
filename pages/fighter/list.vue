<template>
  <div class="p-4 bg-gray-900 min-h-screen">

    <div>
      <p class="text-white" v-for="fighter in fighters" :key="fighter.id">
        {{ fighter.name }}

      </p>

    </div>


    <h1 class="text-5xl font-extrabold dark:text-white mb-8 text-center">List des fighters</h1>
    <!--    <ul>-->
    <!--      <li v-for="fighter in fighters" :key="fighter.id">-->
    <!--        <router-link :to="{ name: 'fighter-detail.vue', params: { id: fighter.id } }">-->
    <!--          {{ fighter.name }}-->
    <!--        </router-link>-->
    <!--      </li>-->

    <!--    </ul>-->

    <transition name="fade">
      <NuxtLink to="/fighter/1/detail"
                class="mb-8 flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row  hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700">
        <img class="w-screen rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
             src="https://images.rtl.fr/~c/2000v2000/rtl/www/1413100-conor-mcgregor-le-26-aout-2017-a-las-vegas-nevada.jpg"
             alt="">
        <div class="flex flex-col justify-between p-4 leading-normal">
          <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Conor Mc Gregor</h5>
          <p class="mb-3 font-normal text-gray-700 dark:text-gray-400">
            Mc Gregor est un fighter irlandais qui a fait ses débuts en 2008 dans le MMA (Mixed Martial Arts) et qui a
            remporté le titre de champion des poids légers de l'UFC en 2016. Il est le premier combattant de l'histoire
            de l'UFC à avoir remporté deux titres de champion dans deux catégories de poids différentes. Il est
            également le premier combattant de l'histoire de l'UFC à avoir remporté deux titres de champion dans deux
            catégories de poids différentes. Il est également le premier combattant de l'histoire de l'UFC à avoir
            remporté deux titres de champion dans deux catégories de poids différentes.


          </p>
        </div>
      </NuxtLink>
      <NuxtLink to="/fighter/1/detail"
                class="mb-8 flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row  hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700">
        <img class="w-screen rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
             src="https://images.bfmtv.com/Xt8upncb0wBUuC8vUiwPXkL9UVc=/0x14:2048x1166/images/Charles-OLIVEIRA-1408476.jpg"
             alt="">
        <div class="flex flex-col justify-between p-4 leading-normal">
          <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Charles Oliveira</h5>
          <p class="mb-3 font-normal text-gray-700 dark:text-gray-400">
            Charles Oliveira est un combattant brésilien de MMA. Il est actuellement classé numéro 2 au classement
            des poids légers de l'UFC. Il est le premier combattant de l'histoire de l'UFC à avoir remporté deux titres
            de champion dans deux catégories de poids différentes. Il est également le premier combattant de l'histoire
            de l'UFC à avoir remporté deux titres de champion dans deux catégories de poids différentes.
          </p>
        </div>
      </NuxtLink>
      <NuxtLink to="/fighter/1/detail"
                class="mb-8 flex flex-col items-center bg-white border border-gray-200 rounded-lg shadow md:flex-row  hover:bg-gray-100 dark:border-gray-700 dark:bg-gray-800 dark:hover:bg-gray-700">
        <img class="w-screen rounded-t-lg h-96 md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
             src="https://www.lequipe.fr/_medias/img-photo-jpg/khamzat-chimaev-le-9-avril-dernier-apres-son-combat-victorieux-face-a-gilbert-burns-afp/1500000001685387/322:59,1577:1314-828-828-75/c1ade.jpg"
             alt="">
        <div class="flex flex-col justify-between p-4 leading-normal">
          <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">Khamzat Chimaev</h5>
          <p class="mb-3 font-normal text-gray-700 dark:text-gray-400">
            Khamzat Chimaev est un fighter russe qui a fait ses débuts en 2008 dans le MMA (Mixed Martial Arts) et qui a
            remporté le titre de champion des poids légers de l'UFC en 2016. Il est le premier combattant de l'histoire
            de l'UFC à avoir remporté deux titres de champion dans deux catégories de poids différentes. Il est
            également le premier combattant de l'histoire de l'UFC à avoir remporté deux titres de champion dans deux
            catégories de poids différentes.
          </p>
        </div>
      </NuxtLink>


    </transition>

  </div>

</template>


<script lang="ts">

import Vue from 'vue'

export default Vue.extend({

  async asyncData({$axios}) {
    const fighters = await $axios.$get('https://my-json-server.typicode.com/theodac/needforfight/fighter')
    console.log(fighters)
    return {fighters};
  },
  transition: {
    name: 'fade',
    mode: 'out-in'
  },
})


</script>
<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
