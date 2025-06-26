<script setup lang="ts">
import Journey from './components/Journey.vue'
import Member from './components/Member.vue'
import TextAnimation from './components/TextAnimation.vue'
import VueIcon from './components/VueIcon.vue'
</script>
<template>
  <div class="app-container">
    <section class="section hero-section">
      <div class="background-image">
      </div>
      <div class="background-overlay"></div>
      <div class="main-content">
        <div class="text-svg-container">
          <TextAnimation />
        </div>
        <div class="vue-icon-container">
          <VueIcon />
        </div>
      </div>
    </section>
    <section class="section us-section">
      <div class="background-image">
      </div>
      <div class="background-overlay"></div>
      <Member />
    </section>
    <section class="section journey-section">
      <div class="background-image">
      </div>
      <div class="background-overlay"></div>
      <Journey />
    </section>
  </div>
</template>
<style scoped>
@font-face {
  font-family: 'Inter Display';
  src: url('/Inter-Display-Bold.woff2') format('woff2');
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}
.app-container {
  width: 100vw;
  height: 100vh;
  overflow-y: scroll;
  scroll-snap-type: y mandatory;
  scroll-behavior: smooth;
  scrollbar-width: none;
  -ms-overflow-style: none;
}
.app-container::-webkit-scrollbar {
  display: none;
}
.section {
  width: 100vw;
  height: 100vh;
  scroll-snap-align: start;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}
.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('/index.png');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  z-index: 1;
}
.background-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #000000 0%, #1a1a1a 50%, #000000 100%);
  opacity: 0.6;
  z-index: 2;
}
.background-overlay::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: 
    radial-gradient(circle at 20% 20%, rgba(255, 255, 255, 0.05) 0%, transparent 50%),
    radial-gradient(circle at 80% 80%, rgba(255, 255, 255, 0.03) 0%, transparent 50%),
    radial-gradient(circle at 40% 60%, rgba(255, 255, 255, 0.02) 0%, transparent 50%);
  z-index: 3;
}
.main-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 150px;
  width: auto;
}
.text-svg-container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.vue-icon-container {
  display: flex;
  align-items: center;
  justify-content: center;
}
@keyframes scrollIndicatorFadeIn {
  from {
    opacity: 0;
    transform: translateX(-50%) translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }
}
@keyframes scrollBounce {
  0%, 20%, 50%, 80%, 100% {
    transform: rotate(45deg) translateY(0);
  }
  40% {
    transform: rotate(45deg) translateY(-5px);
  }
  60% {
    transform: rotate(45deg) translateY(-3px);
  }
}
@media (max-width: 768px) {
  .main-content {
    gap: 100px;
    flex-direction: column;
  }
}
.journey-section {
  align-items: stretch;
  justify-content: stretch;
  padding: 0;
}
@media (max-width: 480px) {
  .main-content {
    gap: 80px;
  }
}
</style>
