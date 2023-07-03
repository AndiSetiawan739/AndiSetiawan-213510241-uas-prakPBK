<template>
  <div class="random-photo">
    <h1>{{ title }}</h1>
    <div class="photo-container">
      <div class="photo-wrapper">
        <img :src="photoUrl" alt="Random Photo" @mouseover="zoomIn" @mouseleave="zoomOut" />
      </div>
      <div class="button-container">
        <button @click="getRandomPhoto">Get New Photo</button>
        <a :href="photoUrl" download class="download-button" @click="downloadPhoto">Download</a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      photoUrl: '',
      isZoomed: false,
    };
  },
  mounted() {
    this.getRandomPhoto();
  },
  methods: {
    async getRandomPhoto() {
      try {
        const apiKey = '38037020-2c48722c03be8437a05b588e6';
        const apiUrl = `https://pixabay.com/api/?key=${apiKey}&q=nature&image_type=photo&orientation=horizontal`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        const randomIndex = Math.floor(Math.random() * data.hits.length);
        const randomPhoto = data.hits[randomIndex];

        this.title = randomPhoto.tags;
        this.photoUrl = randomPhoto.webformatURL;
      } catch (error) {
        console.error('Error fetching random photo:', error);
      }
    },
    zoomIn() {
      this.isZoomed = true;
    },
    zoomOut() {
      this.isZoomed = false;
    },
    downloadPhoto() {
      const link = document.createElement('a');
      link.href = this.photoUrl;
      link.download = 'random_photo.jpg';
      link.click();
    },
  },
};
</script>


<style scoped>
.random-photo {
  text-align: center;
  padding: 20px;
}

h1 {
  font-size: 1.5rem;
  margin-bottom: 20px;
  color: white;
}

.photo-container {
  position: relative;
  display: inline-block;
}

.photo-wrapper {
  border: -5px solid #ccc;
  border-radius: 0px;
  overflow: hidden;
}

img {
  max-width: 100%;
  height: auto;
  transition: transform 0.3s ease;
  transform-origin: center;
}

img:hover {
  transform: scale(1.1);
}

.button-container {
  margin-top: 20px;
  text-align: center;
}

button {
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 10px;
}

.download-button {
  padding: 10px 20px;
  font-size: 1rem;
  background-color: #2196f3;
  color: white;
  border: none;
  border-radius: 5px;
  text-decoration: none;
  cursor: pointer;
}

.download-button:hover {
  background-color: #1976d2;
}
</style>
