<template>
  <div v-show="visibleSlide === index" class="carousel-slide">
    <slot></slot>
  </div>
</template>

<script>
export default {
  props: ["visibleSlide", "index"],
};
</script>

<style>
.carousel-slide {
  height: 100%;
  width: 100%;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
}
</style>