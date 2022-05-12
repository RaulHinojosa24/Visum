<script>
import CarouselSlide from "./CarouselSlide.vue";
export default {
    data() {
        return {

        }
    },
    components: { CarouselSlide },
    methods: {
        prev() {
            this.$emit("prevImage");
        },
        next() {
            this.$emit("nextImage");
        }
    }
}
</script>

<template>
    <div class="carousel">
        <slot></slot>

        <i @click="prev" class="fa-solid fa-chevron-left arrow" id="left-arrow"></i>
        <i @click="next" class="fa-solid fa-chevron-right arrow" id="right-arrow"></i>
    </div>
</template>

<style scoped>
.carousel {
    position: relative;
    height: 100%;
    width: 100%;
}

.arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    color: white;
    font-size: 3rem;
    text-shadow: 0px 3px 3px black;
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