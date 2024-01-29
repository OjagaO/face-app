<template>
  <div id="app">
    <FaceTracking />
  </div>
</template>

<script>
import FaceTracking from "./components/FaceTracking.vue";

export default {
  components: {
    FaceTracking,
  },
};
</script>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0 !important;
  font-weight: normal;
  padding: 0 !important;
}
</style>
