<script setup>
import PlantCard from './components/PlantCard.vue';
import PlantsJson from './assets/plants.json'
import Carousel from './components/Carousel.vue';
import CarouselSlide from './components/CarouselSlide.vue';
</script>

<script>
export default {
  data() {
    return {
      plantIndex: 0,
      visibleSlide: 0,
      plants: []
    }
  },
  methods: {
    prevPlant() {
      if (this.plantIndex <= 0) {
        this.plantIndex = this.plants.length - 1;
        return;
      }

      this.plantIndex--;
    },
    nextPlant() {
      if (this.plantIndex + 1 >= this.plants.length) {
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
  <div class="app">
    <header>
      <h1 id="title">Visum</h1>
    </header>
    <main>
      <div class="card">
        <Carousel v-if="plants.length != 0">
          <CarouselSlide v-for="(imatge, index) in plants[plantIndex].imatges" :key="imatge" :index="index"
            :visibleSlide="visibleSlide">
            <img :src="imatge" alt="" srcset="">
          </CarouselSlide>
        </Carousel>
        <div class="buttons-container">
          <button @click="prevPlant">Prev plant</button>
          <button @click="nextPlant">Next plant</button>
        </div>
      </div>
    </main>
    <footer>
    </footer>
  </div>
</template>

<style>
@import url('https://necolas.github.io/normalize.css/8.0.1/normalize.css');
@import './assets/base.css';

html {
  background-color: var(--background-color);
}

.app {
  min-height: 100vh;

  font-family: var(--font-family);

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}

#title {
  text-transform: uppercase;
  letter-spacing: .5rem;
  font-family: Garamond, serif;
}

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.buttons-container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  min-height: 10%;
  width: 100%;
}

.buttons-container button {
  flex: 1 1 auto;
  background-color: var(--primary-color);
  color: white;
  border: none;
  padding: 20px;
  cursor: pointer;
  outline: inherit;
}

header,
footer {
  min-height: 10vh;
  max-height: 10vh;
}

main {
  min-height: 80vh;
  max-height: 80vh;
}

header,
main,
footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 90%;
}

img {
  display: block;
  width: inherit;
  max-height: calc(80vh * 0.9);
  border-top-left-radius: 1rem;
  border-top-right-radius: 1rem;
}
</style>
