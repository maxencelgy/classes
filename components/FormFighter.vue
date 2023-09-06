<template>
  <form @submit.prevent="createFighter">
    <div>
      <label for="name">Name:</label>
      <input type="text" id="name" v-model="fighterName" />
    </div>
    <div>
      <label for="weight">Weight:</label>
      <input type="text" id="weight" v-model="fighterWeight" />
    </div>
    <div>
      <label for="height">Height:</label>
      <input type="text" id="height" v-model="fighterHeight" />
    </div>
    <button type="submit">Create fighter</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      fighterName: "",
      fighterWeight: "",
      fighterHeight: ""
    };
  },
  methods: {
    async createFighter() {
      const postData = {
        name: this.fighterName,
        weight: this.fighterWeight,
        height: this.fighterHeight
      };
      const response = await this.$axios.$post(
        "https://my-json-server.typicode.com/theodac/needforfight/fighter",
        postData
      );
      console.log(response);
    }
  }
};
</script>
