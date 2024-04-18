<script setup>
import HomePage from './HomePage.vue';
</script>
<template>
  <div class="frontpage">
    <div class="welcome-message">
      <h1>Velkommen til Studenterhuset Odense!</h1>
      <p>Klik eller swipe op for at se kommende events eller book et af vores mange lokaler</p>
      <div class="cta">
      <div class="arrow arrow-first"></div>
      <div class="arrow arrow-second"></div>
    </div>
    </div>
    
  </div>
</template>

<script>
export default {
  methods: {
    scrollToMainSection() {
      const mainSection = document.getElementById('main-section');
      if (mainSection) {
        window.scrollTo({
          top: mainSection.offsetTop,
          behavior: 'smooth'
        });
      }
    }
  }
}
</script>