<template>
  <div id="app">
    <ImageGallery />
  </div>
</template>

<script>
import ImageGallery from './components/ImageGallery.vue';

export default {
  components: {
    ImageGallery,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #fdfdfd;
  margin-top: 60px;
}
</style>