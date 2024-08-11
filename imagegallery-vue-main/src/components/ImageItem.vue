<template>
  <div class="image-item">
    <div class="image-container">
      <img :src="image.src" :alt="image.alt" @click="selectImage" />
    </div>
    <div class="image-info">
      <p>{{ image.alt }}</p>
      <button class="delete-button" @click.stop="deleteImage">Delete</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    image: {
      type: Object,
      required: true,
    },
    index: {
      type: Number,
      required: true,
    },
  },
  methods: {
    selectImage() {
      this.$emit('select', this.image);
    },
    deleteImage() {
      this.$emit('delete', this.index);
    },
  },
};
</script>

<style scoped>
.image-item {
  background-color: #fff;
  border-radius: 12px;
  overflow: hidden;
  transition: box-shadow 0.3s ease;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
  display: flex;
  flex-direction: column;
}

.image-item:hover {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.image-container {
  position: relative;
  padding-top: 100%; /* Aspect ratio 1:1 */
  overflow: hidden;
}

.image-container img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.image-container img:hover {
  transform: scale(1.1);
}

.image-info {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.image-info p {
  margin: 0;
  font-size: 14px;
  color: #555;
}

.delete-button {
  background-color: #ff6b6b;
  border: none;
  border-radius: 5px;
  padding: 5px 10px;
  font-size: 12px;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-button:hover {
  background-color: #ff4d4d;
}
</style>
