<script setup>
import PlantCard from './components/PlantCard.vue';
import PlantsJson from './assets/plants.json'

</script>

<script>
export default {
  data() {
    return {
      plantIndex: 0,
      plants: []
    }
  },
  methods: {
    prevPlant() {
      if (this.plantIndex == 0) {
        this.plantIndex = this.plants.length - 1;
        return;
      }

      this.plantIndex--;
    },
    nextPlant() {
      if (this.plantIndex + 1 == this.plants.length) {
        this.plantIndex = 0;
        return;
      }

      this.plantIndex++;
    },
  },
  mounted() {
    this.plants = PlantsJson.plants
      .map(value => ({ value, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ value }) => value);
  }
}
</script>

<template>
  <header>
    <h1 id="title">Visum</h1>
  </header>
  <main>
    <PlantCard :plant="plants[plantIndex]" />
    <div class="buttons-container">
      <button @click="prevPlant">Prev plant</button>
      <button @click="nextPlant">Next plant</button>
    </div>
  </main>
  <footer>

  </footer>
</template>

<style>
@import url('https://necolas.github.io/normalize.css/8.0.1/normalize.css');
@import './assets/base.css';

html {
  background-color: var(--background-color);
}

#app {
  min-width: 100vw;
  min-height: 100vh;

  font-family: var(--font-family);

  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

#title {
  text-transform: uppercase;
  letter-spacing: .5rem;
}

.buttons-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
</style>
