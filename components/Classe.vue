<template>
  <div class="">
    <div class="card">
      <div class="">
        <h2>{{ title }}</h2>
        <p>Nombre de personnes dans la salle : {{ personnes }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Classe',
  props: {
    title: String,
    personnes: Number  }
}
</script>
