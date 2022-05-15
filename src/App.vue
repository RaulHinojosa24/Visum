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
      displayInfo: false,
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
  <main v-if="plants.length != 0">
    <div class="card">
      <Carousel @prevImage="prevImage" @nextImage="nextImage">
        <CarouselSlide
          v-for="(imatge, index) in plants[plantIndex].imatges"
          :key="imatge"
          :index="index"
          :visibleSlide="visibleSlide"
          :style="{ 'background-image': 'url(' + imatge + ')' }"
        >
        </CarouselSlide>
      </Carousel>

      <transition name="info-card">
        <div class="info-card" v-show="displayInfo">
          <div class="info-group">
            <h2 class="info-title">Nom</h2>
            <p>
              <em>{{ plants[plantIndex].nom }}</em>
            </p>
          </div>
          <div class="info-group">
            <h2 class="info-title">Classificació</h2>
            <p>
              <em>{{ plants[plantIndex].classificacio }}</em>
            </p>
          </div>
          <div class="info-group">
            <h2 class="info-title">Característiques</h2>
            <p
              v-for="carac in plants[plantIndex].caracteristiques"
              :key="carac"
            >
              {{ carac }}
            </p>
          </div>
        </div>
      </transition>
    </div>
    <div class="buttons-container">
      <div class="row show-info">
        <button @click="displayInfo = !displayInfo">
          {{ displayInfo ? "Amaga Informació" : "Mostra Informació" }}
        </button>
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
  color: var(--font-color);
}

#title {
  text-transform: uppercase;
  letter-spacing: 0.5rem;
  font-weight: bold;
  display: flex;
  flex-direction: row;
  align-items: center;
  user-select: none;
}

#title span {
  transition: font 0.2s, color 0.2s;
}

#title span:hover {
  font-size: 110%;
  color: var(--secondary-color);
}

.info-card {
  background-color: var(--secondary-color);
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
  text-align: center;
}

.info-title {
  text-decoration: underline;
  font-size: 1.4rem;
}

.info-card-enter-active,
.info-card-leave-active {
  transition: top 0.5s ease;
}

.info-card-enter-from,
.info-card-leave-to {
  top: 100%;
}

.card {
  height: 100%;
  width: 100%;
  position: relative;
}

.buttons-container {
  width: 100%;
  z-index: 10;
}

.buttons-container .row {
  display: flex;
  width: 100%;
  height: 50%;
}

.show-info button {
  background-color: var(--accent-color);
}

button {
  background-color: var(--secondary-color);
  color: var(--font-color);
  border: none;
  cursor: pointer;
  outline: inherit;
  transition: font 0.2s, color 0.2s;
  width: 100%;
  height: 45px;
}

button:hover,
button:active {
  font-size: 1.1rem;
  color: var(--primary-color);
}
</style>