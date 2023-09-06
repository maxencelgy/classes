<template>
  <div>
    <header>
      <nav>
        <p>Trouve ta place</p>
      </nav>
    </header>
    <section id="presentation">
      <div class="container_presentation">
        <div class="img"><img src="@/src/img/logo.png" alt="logo"></div>
        <p class="contenue_presentation">
          Vous cherchez des places dans une salle ? <br> Notre site Web vous aide à trouver des places disponibles
          dans les salles
        </p>
      </div>
    </section>
    <form action="" class="card_search">
      <input type="search" placeholder="Rechercher" id="search" v-model="searchQuery">
      <i class="fa-solid fa-magnifying-glass"></i>
    </form>
    <div class="">
      <div id="section_card flex items-center justify-between" class="pb-8">
        <NuxtLink
          v-for="classe in filteredClasses"
          :to="'/classe/' + classe._id + '/detail'">
        <div class="card_container ">

          <Classe
            :key="classe._id"
            :title="classe.title"
            :personnes="classe.personnes"
            style="width: 46%;"
          />

        </div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      searchQuery: '',
      classes: [], // Stockez les classes complètes ici
    };
  },
  async asyncData({$axios}) {
    const classes = await $axios.$get('https://api-node-breathe.hop.sh/api/classe/all');
    console.log(classes);
    return {classes};
  },
  computed: {
    filteredClasses() {
      // Filtrer la liste des classes en fonction de la recherche
      const query = this.searchQuery.toLowerCase().trim();
      return this.classes.filter(classe => {
        // Vous pouvez personnaliser la logique de filtrage ici
        // Par exemple, vous pouvez vérifier si le titre ou d'autres propriétés de la classe correspondent à la recherche
        return classe.title.toLowerCase().includes(query);
      });
    },
  },
  // ...
})
</script>

<style>
/* reset */
* {
  margin: 0;
  padding: 0;
  border: 0;
}

body {
  background-color: #EFF0F2;
}

ul {
  list-style: none;
}

header {
  background-color: white;
  padding: 1rem;
}

img {
  width: 100%;
}

header nav ul {
  display: flex;
  justify-content: space-between;
  width: 100%;
  max-width: 960px;
  margin: 0 auto;
  padding: 0 10px;
  align-items: center;
}

header nav p {
  font-size: 2rem;
  font-weight: bold;
  text-align: -webkit-center;
}

header nav ul li:nth-child(1) {
  font-weight: bold;
  font-size: 1.5rem;
}

header nav ul li:nth-child(4) {
  background-color: #8A3275;
  border-radius: 1rem;
  padding: 0.6rem;
  color: white;
  width: 10%;
  text-align: center;
}

#presentation {
  max-width: 680px;
  margin: 0 auto;
  padding: 0 10px;
  text-align: -webkit-center;
}

#presentation .container_presentation .img {
  width: 20%;
}

.container_presentation {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 2rem;
  gap: 2rem;
  position: relative;
}

.demarrer {
  background-color: #4351E8;
  color: white;
  padding: 0.5rem;
  border-radius: 1rem;
  cursor: pointer;
  font-size: 1.2rem;
}

.card_container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 10px;
}

.card_search {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 10px;
}

.card {
  margin-top: 1rem;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.5),
    rgba(0, 0, 0, 0.5)
  ), url('@/src/img/salle.png');
  background-position: center center;
  background-repeat: no-repeat;
  height: 600px;
  width: 100%;
  display: flex;
  flex-direction: column;
  place-items: center;
  place-content: center;
  text-align: center;
  gap: 1rem;
}

.card_container h2, .card_container p {
  color: white;
}

form {
  text-align: -webkit-center;
}

#search {
  padding: 0.5rem;
  text-align: center;
  width: 100%;
  margin-top: 1rem;
}

form i {
  position: absolute;
  left: 55%;
  top: 55%;
  color: black;
}


</style>
