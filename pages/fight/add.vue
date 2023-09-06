<template>
  <div class="p-4">
    Créer un fight

    <FormFight />

  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({

  async asyncData({ $axios }) {
    const fighters = await $axios.$get('https://my-json-server.typicode.com/theodac/needforfight/fighter')
    return {fighters};
  },

})
</script>
