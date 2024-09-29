<template>
  <div class="gallery">
    <h1 class="gallery-heading"><b><i>GALLERY !</i></b></h1>

    <!-- Folder Navigation -->
    <div class="folder-navigation">
      <div
        v-for="(folder, index) in folders"
        :key="index"
        class="folder-item"
      >
        <button @click="selectFolder(folder)" :class="{ active: selectedFolder === folder }">
          {{ folder.name }}
        </button>
        <button class="delete-folder" @click.stop="deleteFolder(index)">🗑</button>
      </div>
      <button @click="createNewFolder">+ New Folder</button>
    </div>

    <!-- Display Folder Content or Image Input -->
    <div v-if="selectedFolder" class="folder-content">
      <h2>{{ selectedFolder.name }}</h2>
      <div class="input-container">
        <input v-model="newImageName" type="text" placeholder="Enter image name" />
        <input type="file" @change="handleImageUpload" />
        <button @click="addImage">Add Image</button>
      </div>
    </div>

    <!-- Gallery Grid -->
    <div class="gallery-grid" v-if="selectedFolder && selectedFolder.images.length > 0">
      <ImageItem
        v-for="(image, index) in selectedFolder.images"
        :key="index"
        :image="image"
        :index="index"
        @select="handleSelect"
        @delete="deleteImage"
      />
    </div>

    <!-- Lightbox for Image Preview -->
    <div v-if="selectedImage" class="lightbox" @click="closeLightbox">
      <img :src="selectedImage.src" :alt="selectedImage.alt" />
    </div>
  </div>
</template>

<script>
import ImageItem from './ImageItem.vue';

export default {
  components: { ImageItem },
  data() {
    return {
      folders: [{ name: 'Default', images: [] }],
      selectedFolder: null,
      selectedImage: null,
      newImageName: '',
      newImageFile: null,
    };
  },
  methods: {
    selectFolder(folder) {
      this.selectedFolder = folder;
    },
    createNewFolder() {
      const folderName = prompt('Enter the new folder name:');
      if (folderName) {
        this.folders.push({ name: folderName, images: [] });
        this.selectedFolder = this.folders[this.folders.length - 1];
      }
    },
    deleteFolder(index) {
      this.folders.splice(index, 1);
      this.selectedFolder = this.folders[0] || null;
    },
    handleImageUpload(event) {
      this.newImageFile = event.target.files[0];
    },
    addImage() {
      if (this.newImageName.trim() !== '' && this.newImageFile) {
        const reader = new FileReader();
        reader.onload = (e) => {
          this.selectedFolder.images.push({
            src: e.target.result,
            alt: this.newImageName.trim(),
          });
          this.newImageName = '';
          this.newImageFile = null;
        };
        reader.readAsDataURL(this.newImageFile);
      }
    },
    handleSelect(image) {
      this.selectedImage = image;
    },
    closeLightbox() {
      this.selectedImage = null;
    },
    deleteImage(index) {
      this.selectedFolder.images.splice(index, 1);
    },
  },
  created() {
    this.selectedFolder = this.folders[0]; // Default to the first folder
  },
};
</script>

<style scoped>
/* Base styles for the gallery */
.gallery {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
  background-color: rgb(123, 216, 228);
  border:2px;
  border-color: black;
  border-radius: 5;
  color: #e0e0e0;
  font-family: 'Roboto', sans-serif;
}

/* Heading style */
.gallery-heading {
  text-align: center;
  margin-bottom: 30px;
  font-size: 3  em;
  font-weight: bold;
  color: #ffffff;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

/* Folder navigation styles */
.folder-navigation {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.folder-item {
  display: flex;
  align-items: center;
}

.folder-item button {
  margin: 0 5px;
  padding: 12px 25px;
  font-size: 16px;
  background-color: #3a3a3a;
  color: #ffffff;
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.folder-item button:hover {
  background-color: #565656;
}

.folder-item button.active {
  background-color: #007acc;
  color: #ffffff;
}

.delete-folder {
  background-color: transparent;
  color: #ff4c4c;
  border: none;
  font-size: 20px;
  cursor: pointer;
  transition: color 0.3s ease;
}

.delete-folder:hover {
  color: #ff2c2c;
}

.folder-content {
  text-align: center;
  margin-bottom: 30px;
}

.input-container {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 20px;
  
}

.input-container input[type="text"],
.input-container input[type="file"] {
  padding: 12px;
  font-size: 16px;
  background-color: #5cc0b6;
  color: #e0e0e0;
  border: 1px solid #444444;
  border-radius: 50px;
  outline: none;
  transition: border-color 0.3s ease;
}

.input-container input[type="text"]:focus,
.input-container input[type="file"]:focus {
  border-color: #007acc;
}

.input-container button {
  padding: 12px 30px;
  font-size: 16px;
  border: none;
  border-radius: 50px;
  background-color: #007acc;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;
}

.input-container button:hover {
  background-color: #005b99;
  transform: translateY(-2px);
}

/* Gallery grid styles */
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

/* Lightbox styles */
.lightbox {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  display: flex;
  justify-content: center;
  align-items: center;
  animation: fadeIn 0.3s ease;
}

.lightbox img {
  max-width: 90%;
  max-height: 80%;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
  transition: transform 0.3s ease;
}

.lightbox img:hover {
  transform: scale(1.02);
}

/* Keyframes for lightbox fade-in effect */
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>


