<script>
export default {
    props: ["visibleSlide", "index"]
}
</script>

<template>
    <div v-show="visibleSlide == index" class="carousel-slide">
        <slot></slot>
    </div>
</template>

<style>
.carousel-slide {
    width: 100%;
}
</style>