<template>
  <form @submit.prevent="createFight">
    <div>
      <label for="name">Fighter 1:</label>
      <input type="text" id="id_fighter1" v-model="fightOne" />
      <select  >
        <option v-for="fighter in fighters" :key="fighter.id" value="">{{ fighter.name }}</option>
      </select>


    </div>
    <div>
      <label for="weight">Fighter 2:</label>
      <input type="text" id="id_fighter2" v-model="fightTwo" />
    </div>
    <div>
      <label for="height">Cote :</label>
      <input type="text" id="bet" v-model="fightBet" />
    </div>
    <button type="submit">Create fighter</button>
  </form>



</template>

<script>
export default {

  data() {
    return {
      fightOne: "",
      fightTwo: "",
      fightBet: ""
    };
  },
  methods: {
    async createFight() {
      const postData = {
        name: this.fightOne,
        weight: this.fightTwo,
        height: this.fightBet
      };
      const response = await this.$axios.$post(
        "https://my-json-server.typicode.com/theodac/needforfight/fight",
        postData
      );
      console.log(response);
    }
  }
};
</script>
