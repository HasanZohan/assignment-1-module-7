<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const images = ref([]);
const currentIndex = ref(0);
const currentImage = ref('');

const fetchRandomImages = async () => {
  try {
    const response = await fetch('https://picsum.photos/v2/list?page=1&limit=3');
    const data = await response.json();
    images.value = data.map(image => `https://picsum.photos/id/${image.id}/400/300`);
    currentImage.value = images.value[currentIndex.value]; // Set initial image
  } catch (error) {
    console.error('Error fetching images:', error);
  }
};

const nextSlide = () => {
  currentIndex.value = (currentIndex.value + 1) % images.value.length;
  currentImage.value = images.value[currentIndex.value];
};

const prevSlide = () => {
  currentIndex.value = (currentIndex.value - 1 + images.value.length) % images.value.length;
  currentImage.value = images.value[currentIndex.value];
};

const intervalId = setInterval(nextSlide, 3000);

onMounted(() => {
  fetchRandomImages();
});

onBeforeUnmount(() => {
  clearInterval(intervalId);
});
</script>

<template>
  <div class="carousel">
    <div class="carousel-content">
      <img :src="currentImage" alt="Carousel Slide" />
    </div>
    <div class="carousel-buttons">
      <button @click="prevSlide" class="carousel-button prev">
        <span class="arrow">←</span> Previous
      </button>
      <button @click="nextSlide" class="carousel-button next">
        Next <span class="arrow">→</span>
      </button>
    </div>
  </div>
</template>

<style>
.carousel {
  position: relative;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}

.carousel-content {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
  overflow: hidden;
}

.carousel img {
  max-width: 100%;
  height: auto;
}

.carousel-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.carousel-button {
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  padding: 8px 16px;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
}

.carousel-button.prev {
  margin-right: 10px;
}

.carousel-button.next {
  margin-left: 10px;
}

.arrow {
  margin-right: 5px;
  font-size: 18px;
}
</style>
