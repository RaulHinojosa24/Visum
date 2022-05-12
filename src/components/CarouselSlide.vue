<template>
    <div v-show="visibleSlide === index" class="carousel-slide">
        <slot></slot>
    </div>
</template>

<script>
export default {
    props: ["visibleSlide", "index"]
}
</script>

<style>

</style>