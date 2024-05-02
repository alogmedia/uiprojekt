<script setup>
import { onMounted, ref } from 'vue';

const mainSection = ref(null);

const scrollToMainSection = () => {
  if (mainSection.value) {
    window.scrollTo({
      top: mainSection.value.offsetTop,
      behavior: 'smooth'
    });
  }
};

const isMobileClick = () => {
  if (window.matchMedia('(pointer: coarse)').matches) {
    console.log('Mobile user clicked the button');
  }
}

onMounted(() => {
  mainSection.value = document.getElementById('main-section');
});
</script>

<template>
  <div class="frontpage">
    <div class="welcome-message">
      <h1>Velkommen til Studenterhuset Odense!</h1>
      <p>Klik eller swipe op for at se kommende events</p>
      <div class="cta">
        <button class="ctaknap" @click="scrollToMainSection" @touchend="isMobileClick">Se Events</button>
        <a href="/qr">
          <button class="ctaknap" @touchend="isMobileClick">QR</button>
        </a>
      </div>
    </div>
  </div>
</template>
