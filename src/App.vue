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
      APP_STATUS: {
        plants: [],
        visumConfig: {
          notDefined: true,
          known: true,
          unknown: true,
        },
        plantIndex: 0,
      },
      displayInfo: false,
      visibleSlide: 0,
      navBarActive: 1,
    };
  },
  computed: {
    slidesLen() {
      return this.filteredPlants[this.APP_STATUS.plantIndex].imatges.length;
    },
    knownPlants() {
      return this.APP_STATUS.plants
        .filter((plant) => plant.known)
        .sort((a, b) => a.nom.localeCompare(b.nom));
    },
    unknownPlants() {
      return this.APP_STATUS.plants
        .filter((plant) => plant.known == false)
        .sort((a, b) => a.nom.localeCompare(b.nom));
    },
    isThisPlantKnown() {
      return this.filteredPlants[this.APP_STATUS.plantIndex].known == true;
    },
    isKnownNull() {
      return this.filteredPlants[this.APP_STATUS.plantIndex].known == null;
    },
    filteredPlants() {
      return this.APP_STATUS.plants.filter((plant) => {
        let known = this.APP_STATUS.visumConfig.known
          ? plant.known === true
          : false;
        let unknown = this.APP_STATUS.visumConfig.unknown
          ? plant.known === false
          : false;
        let notDefined = this.APP_STATUS.visumConfig.notDefined
          ? plant.known === null
          : false;
        return known || unknown || notDefined;
      });
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
      if (this.APP_STATUS.plantIndex <= 0) {
        this.APP_STATUS.plantIndex = this.filteredPlants.length - 1;
        return;
      }

      this.APP_STATUS.plantIndex--;
      this.updateLocalStorage();
    },
    nextPlant() {
      this.visibleSlide = 0;
      if (this.APP_STATUS.plantIndex + 1 >= this.filteredPlants.length) {
        this.APP_STATUS.plantIndex = 0;
        return;
      }

      this.APP_STATUS.plantIndex++;
      this.updateLocalStorage();
    },
    markAsKnown() {
      this.filteredPlants[this.APP_STATUS.plantIndex].known = true;
      this.displayInfo = false;
      this.updateLocalStorage();
    },
    markAsUnknown() {
      this.filteredPlants[this.APP_STATUS.plantIndex].known = false;
      this.displayInfo = false;
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      console.log("lsupdated");
      localStorage.setItem("APP_STATUS", JSON.stringify(this.APP_STATUS));
    },
    deleteLocalStorage() {
      localStorage.clear();
      location.reload();
    },
  },
  mounted() {
    if (localStorage.getItem("APP_STATUS") !== null) {
      let local = JSON.parse(localStorage.getItem("APP_STATUS"));

      this.APP_STATUS.plants = local.plants;
      this.APP_STATUS.visumConfig = local.visumConfig;
      this.APP_STATUS.plantIndex = local.plantIndex;
    } else {
      this.APP_STATUS.plants = PlantsJson.map((value) => ({
        value,
        sort: Math.random(),
      }))
        .sort((a, b) => a.sort - b.sort)
        .map(({ value }) => ({ ...value, known: null }));

      this.updateLocalStorage();
    }
  },
  watch: {
    "APP_STATUS.visumConfig.known"() {
      this.APP_STATUS.plantIndex = 0;
      this.updateLocalStorage();
    },
    "APP_STATUS.visumConfig.unknown"() {
      this.APP_STATUS.plantIndex = 0;
      this.updateLocalStorage();
    },
    "APP_STATUS.visumConfig.notDefined"() {
      this.APP_STATUS.plantIndex = 0;
      this.updateLocalStorage();
    },
  },
};
</script>

<template>
  <header>
    <div class="nav-bar">
      <button @click="navBarActive = 0">Llista</button>
      <button class="title" @click="navBarActive = 1">Visum</button>
      <button @click="navBarActive = 2">Config</button>
      <span
        id="nav-select-color"
        :class="navBarActive == 0 ? '' : navBarActive == 1 ? 'second' : 'third'"
      ></span>
    </div>
  </header>

  <main v-if="APP_STATUS.plants.length != 0">
    <div class="lista" v-show="navBarActive == 0">
      <div class="plantes-conegudes">
        <h2 class="title">
          <span>Plantes conegudes</span
          ><span
            >{{ knownPlants.length }} / {{ APP_STATUS.plants.length }}</span
          >
        </h2>
        <ul>
          <li v-for="plant in knownPlants" :key="plant.nom">
            {{ plant.nom }}
          </li>
        </ul>
      </div>
      <div class="plantes-desconegudes">
        <h2 class="title">
          <span>Plantes desconegudes</span
          ><span
            >{{ knownPlants.length }} / {{ APP_STATUS.plants.length }}</span
          >
        </h2>
        <ul>
          <li v-for="plant in unknownPlants" :key="plant.nom">
            {{ plant.nom }}
          </li>
        </ul>
      </div>
    </div>

    <div class="visum" v-show="navBarActive == 1">
      <div v-if="filteredPlants.length > 0" class="card">
        <Carousel @prevImage="prevImage" @nextImage="nextImage">
          <CarouselSlide
            v-for="(imatge, index) in filteredPlants[APP_STATUS.plantIndex]
              .imatges"
            :key="imatge"
            :index="index"
            :visibleSlide="visibleSlide"
            :style="{ 'background-image': 'url(' + imatge + ')' }"
          >
          </CarouselSlide>
        </Carousel>
        <transition name="info-card">
          <div class="info-card" v-show="displayInfo">
            <div class="info-card-info">
              <div class="info-group">
                <h2 class="info-title">Nom</h2>
                <p>
                  <em>{{ filteredPlants[APP_STATUS.plantIndex].nom }}</em>
                </p>
              </div>
              <div class="info-group">
                <h2 class="info-title">Classificació</h2>
                <p>
                  <em>{{
                    filteredPlants[APP_STATUS.plantIndex].classificacio
                  }}</em>
                </p>
              </div>
              <div class="info-group">
                <h2 class="info-title">Característiques</h2>
                <p
                  v-for="carac in filteredPlants[APP_STATUS.plantIndex]
                    .caracteristiques"
                  :key="carac"
                >
                  {{ carac }}
                </p>
              </div>
            </div>
            <div class="info-card-buttons">
              <button
                @click="markAsKnown"
                v-if="!isThisPlantKnown || isKnownNull"
              >
                Marcar com coneguda
              </button>
              <button
                @click="markAsUnknown"
                v-if="isThisPlantKnown || isKnownNull"
              >
                Marcar com desconeguda
              </button>
            </div>
          </div>
        </transition>
      </div>
      <div v-else>
        <p>No hi han plantes per mostrar amb la configuració actual.</p>
      </div>
      <div class="buttons-container" v-if="filteredPlants.length > 0">
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
    </div>

    <div class="config" v-show="navBarActive == 2">
      <h2 class="title">Configuració</h2>
      <div class="configuracio">
        <p class="desc">Marca les opcions que vulguis veure al Visum!</p>
        <div class="checkboxes">
          <label>
            <input
              type="checkbox"
              v-model="APP_STATUS.visumConfig.notDefined"
            />
            Mostra plantes sense definir
          </label>
          <label>
            <input type="checkbox" v-model="APP_STATUS.visumConfig.known" />
            Mostra plantes conegudes
          </label>
          <label>
            <input type="checkbox" v-model="APP_STATUS.visumConfig.unknown" />
            Mostra plantes desconegudes
          </label>
          <button @click="deleteLocalStorage" class="deleteLocalStorage">
            Reset
          </button>
        </div>
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
  font-size: 18px;
  overflow: hidden;
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
  max-height: 60px;
}

.nav-bar {
  min-height: 100%;
  display: flex;
  width: 100%;
  position: relative;
  background-color: var(--accent-color);
}

.nav-bar button {
  background-color: transparent;
  color: var(--font-color);
  border: none;
  cursor: pointer;
  outline: inherit;
  width: 100%;
  min-height: 100%;
  z-index: 10;
}

.nav-bar .title {
  min-width: 50%;
  font-size: 1.5rem;
  text-transform: uppercase;
  font-weight: bold;
  letter-spacing: 5px;
}

#nav-select-color {
  background-color: var(--secondary-color);
  height: 100%;
  width: 25%;
  position: absolute;
  left: 0;
  transition: left 0.2s, width 0.2s;
}

#nav-select-color.second {
  left: 25%;
  width: 50%;
}

#nav-select-color.third {
  left: 75%;
}

#title {
  height: 100%;
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
  color: var(--accent-color);
}

#app {
  background-color: var(--primary-color);
  font-family: var(--font-family);
  color: var(--font-color);
}

main {
  height: 100%;
  min-height: 90%;
  max-height: calc(100% - 60px);
}

.visum,
.lista,
.config {
  display: contents;
  width: 100%;
  height: 100%;
}

.lista {
  display: block;
}

.lista > * {
  width: 100%;
  height: 50%;
  max-height: 50%;
  overflow-y: auto;
}

.plantes-conegudes > .title,
.plantes-desconegudes > .title {
  position: sticky;
  top: 0;
  background-color: var(--secondary-color);
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}

.lista .title {
  font-size: 1.2rem;
  padding: 0.5rem 0.8rem;
}

.lista ul li:not(:last-child) {
  margin-bottom: 0.3rem;
}

.config {
  display: block;
  position: relative;
}

.config .title {
  background-color: var(--secondary-color);
  font-size: 1.2rem;
  padding: 0.5rem 0.8rem;
}

.configuracio {
  padding: 1rem;
}

.configuracio .desc {
  font-style: italic;
  margin-top: 0.5rem;
}

.checkboxes {
  margin-top: 1rem;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.deleteLocalStorage {
  position: absolute;
  bottom: 3rem;
  left: 50%;
  transform: translateX(-50%);
  background-color: #c23b22;
  color: white;
  border: none;
  cursor: pointer;
  outline: inherit;
  padding: 1rem 2rem;
}

.info-card {
  background-color: var(--secondary-color);
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  text-align: center;
}

.info-card-info {
  height: 90%;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1.5rem;
}

.info-card-buttons {
  width: 100%;
  height: 10%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  text-decoration: underline;
}

.info-card-buttons > :only-child {
  grid-column: 1/-1;
}

.info-card-buttons button {
  background-color: transparent;
  color: var(--font-color);
  border: none;
  cursor: pointer;
  outline: inherit;
  transition: font 0.2s, color 0.2s;
  width: 100%;
  height: 100%;
}

.info-title {
  text-decoration: underline;
  font-size: 1.1rem;
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
  border-top: 0.5px solid var(--primary-color);
}

.buttons-container .row {
  display: flex;
  width: 100%;
  height: 50%;
}

.buttons-container button {
  background-color: var(--accent-color);
  color: var(--font-color);
  border: none;
  cursor: pointer;
  outline: inherit;
  transition: font 0.2s, color 0.2s;
  width: 100%;
  height: 45px;
}

.buttons-container button:hover,
.buttons-container button:active {
  font-size: 1.1rem;
  color: var(--primary-color);
}

.show-info button {
  background-color: var(--secondary-color);
}
</style>