<script setup>

</script>
<template>
  <div class="frontpage">
    <div class="welcome-message">
      <h1>Velkommen til Studenterhuset Odense!</h1>
      <p>Klik eller swipe op for at se kommende events</p>
      <div class="cta">
      <button class="ctaknap" @click="scrollToMainSection">Se Events</button>
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
