<script setup>
import PlantCard from "./components/PlantCard.vue";
import PlantsJson from "./assets/plants.json";
import Carousel from "./components/Carousel.vue";
import CarouselSlide from "./components/CarouselSlide.vue";
</script>

<script>
export default {
  data() {
    return {
      plantIndex: 0,
      visibleSlide: 0,
      plants: [],
    };
  },
  computed: {
    slidesLen() {
      return this.plants[this.plantIndex].imatges.length;
    },
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
      this.visibleSlide = 0;
      if (this.plantIndex <= 0) {
        this.plantIndex = this.plants.length - 1;
        return;
      }

      this.plantIndex--;
    },
    nextPlant() {
      this.visibleSlide = 0;
      if (this.plantIndex + 1 >= this.plants.length) {
        this.plantIndex = 0;
        return;
      }

      this.plantIndex++;
    },
  },
  mounted() {
    this.plants = PlantsJson.plants
      .map((value) => ({ value, sort: Math.random() }))
      .sort((a, b) => a.sort - b.sort)
      .map(({ value }) => value);
  },
};
</script>

<template>
  <header>
    <h1 id="title">
      <span>v</span><span>i</span><span>s</span><span>u</span><span>m</span>
    </h1>
  </header>
  <main>
    <div class="card">
      <Carousel
        v-if="plants.length != 0"
        @prevImage="prevImage"
        @nextImage="nextImage"
      >
        <CarouselSlide
          v-for="(imatge, index) in plants[plantIndex].imatges"
          :key="imatge"
          :index="index"
          :visibleSlide="visibleSlide"
          :style="{ 'background-image': 'url(' + imatge + ')' }"
        >
        </CarouselSlide>
      </Carousel>
    </div>
    <div class="buttons-container">
      <div class="row show-info">
        <button>Mostra Informació</button>
      </div>
      <div class="row move-plant">
        <button @click="prevPlant">Anterior</button>
        <button @click="nextPlant">Següent</button>
      </div>
    </div>
  </main>
  <footer></footer>
</template>

<style>
@import url("https://necolas.github.io/normalize.css/8.0.1/normalize.css");
@import "./assets/base.css";

html,
body,
#app {
  width: 100%;
  height: 100%;
  margin: 0;
}

header,
main,
footer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

header {
  height: 10%;
}

main {
  height: 90%;
}

#app {
  background-color: var(--primary-color);
  font-family: var(--font-family);
  color: var(--secondary-color);
}

#title {
  text-transform: uppercase;
  letter-spacing: 0.5rem;
  font-family: Garamond, serif;
  display: flex;
  flex-direction: row;
  align-items: center;
}

#title span {
  transition: font 0.2s, color 0.2s;
}

#title span:hover {
  font-size: 110%;
  color: var(--accent-color);
}

.card {
  height: 85%;
  width: 100%;
}

.buttons-container {
  width: 100%;
  height: 15%;
}

.buttons-container .row {
  display: flex;
  width: 100%;
  height: 50%;
}

.show-info {
  grid: 1fr;
}

button {
  background-color: var(--accent-color);
  color: var(--secondary-color);
  border: none;
  cursor: pointer;
  outline: inherit;
  transition: font 0.2s, color 0.2s;
  width: 100%;
  height: 100%;
}

button:hover,
button:active {
  font-size: 1.1rem;
  color: var(--primary-color);
}
</style>