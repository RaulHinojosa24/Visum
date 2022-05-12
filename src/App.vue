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
  computed: {
    slidesLen() {
      return this.plants[this.plantIndex].imatges.length;
    }
  },
  methods: {
    prevImage() {
      if (this.visibleSlide <= 0) {
        this.visibleSlide = this.slidesLen - 1;
      } else {
        this.visibleSlide--;
      }
    },
    nextImage() {
      if (this.visibleSlide >= this.slidesLen - 1) {
        this.visibleSlide = 0;
      } else {
        this.visibleSlide++;
      }
    },
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
    <header>
      <h1 id="title">visum</h1>
    </header>
    <main>
      <div class="card">
        <Carousel v-if="plants.length != 0" @prevImage="prevImage" @nextImage="nextImage">
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
</template>

<style>
@import url('https://necolas.github.io/normalize.css/8.0.1/normalize.css');
@import './assets/base.css';

html, body, #app {
  width: 100%;
  height: 100%;
  margin: 0;
}

#app {
  background-color: var(--background-color);

  font-family: var(--font-family);
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
  width: 100%;
  height: 100%;
}

footer {
  background-color: black;
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
  min-height: 10%;
  max-height: 10%;
  height: 10%;
}

main {
  min-height: 80%;
  max-height: 80%;
  height: 80%;
}

header,
main,
footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

img {
  display: block;
  max-height: 100%;
  max-width: 100%;
}
</style>
