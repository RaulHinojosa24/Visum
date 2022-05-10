<script>
import CarouselSlide from "./CarouselSlide.vue";
export default {
    data() {
        return {

        }
    },
    props: {
        plant: Object
    },
    components: { CarouselSlide },
    methods: {
        prevImage() {
            if (this.imgIndex <= 0) {
                this.imgIndex = this.plant.imatges.length - 1;
                return;
            }

            this.imgIndex--;
        },
        nextImage() {
            if (this.imgIndex + 1 >= this.plant.imatges.length) {
                this.imgIndex = 0;
                return;
            }

            this.imgIndex++;
        }
    }
}
</script>

<template>
    <div class="carousel">
        <slot></slot>
        <i @click="prevImage" class="fa-solid fa-chevron-left arrow" id="left-arrow"></i>
        <i @click="nextImage" class="fa-solid fa-chevron-right arrow" id="right-arrow"></i>
    </div>
</template>

<style scoped>
.carousel {
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
}

.arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    color: white;
    font-size: 3rem;
    text-shadow: 0px 0px 20px #000000;
}

.arrow:hover {
    cursor: pointer;
}

#left-arrow {
    left: 1rem;
}

#right-arrow {
    right: 1rem;
}
</style>