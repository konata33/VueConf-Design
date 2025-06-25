<script setup lang="ts">
import Journey from './components/Journey.vue'
import Member from './components/Member.vue'
import TextAnimation from './components/TextAnimation.vue'
import VueIcon from './components/VueIcon.vue'
</script>

<template>
  <div class="app-container">
    <!-- 第一个板块：主页 -->
    <section class="section hero-section">
      <!-- 背景图片 -->
      <div class="background-image">
        <!-- 这里可以放置背景图片 -->
      </div>
      
      <!-- 有质感的黑色背景覆盖层 -->
      <div class="background-overlay"></div>
      
      <!-- 主要内容区域 -->
      <div class="main-content">
        <!-- 左侧文字SVG动画区域 -->
        <div class="text-svg-container">
          <TextAnimation />
        </div>
        
        <!-- 右侧Vue图标区域 -->
        <div class="vue-icon-container">
          <VueIcon />
        </div>
      </div>
      
      <!-- 滚动提示 -->
      <div class="scroll-indicator">
        <div class="scroll-arrow"></div>
        <span class="scroll-text">Scroll</span>
      </div>
    </section>
    
    <!-- 第二个板块：Us -->
    <section class="section us-section">
      <!-- 背景图片（继承） -->
      <div class="background-image">
        <!-- 这里可以放置背景图片 -->
      </div>
      
      <!-- 有质感的黑色背景覆盖层 -->
      <div class="background-overlay"></div>
      
      <Member />
    </section>
    
    <!-- 第三个板块：Journey -->
    <section class="section journey-section">
      <!-- 背景图片（继承） -->
      <div class="background-image">
        <!-- 这里可以放置背景图片 -->
      </div>
      
      <!-- 有质感的黑色背景覆盖层 -->
      <div class="background-overlay"></div>
      
      <!-- Journey 组件 -->
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
  /* 隐藏滚动条但保持滚动功能 */
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE and Edge */
}

.app-container::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera */
}

/* 每个section样式 */
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



/* 滚动提示样式 */
.scroll-indicator {
  position: absolute;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  z-index: 10;
  opacity: 0;
  animation: scrollIndicatorFadeIn 1s ease-out 9s forwards;
}

.scroll-arrow {
  width: 20px;
  height: 20px;
  border-right: 2px solid rgba(44, 226, 126, 0.8);
  border-bottom: 2px solid rgba(44, 226, 126, 0.8);
  transform: rotate(45deg);
  animation: scrollBounce 2s infinite;
}

.scroll-text {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 0.9rem;
  color: rgba(44, 226, 126, 0.8);
  letter-spacing: 0.1em;
  text-transform: uppercase;
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



/* 响应式调整 */
@media (max-width: 768px) {
  .main-content {
    gap: 100px;
    flex-direction: column;
  }
}

/* Journey section样式 */
.journey-section {
  align-items: stretch;
  justify-content: stretch;
  padding: 0;
}

@media (max-width: 480px) {
  .main-content {
    gap: 80px;
  }
  
  .scroll-indicator {
    bottom: 20px;
  }
  
  .scroll-arrow {
    width: 16px;
    height: 16px;
  }
  
  .scroll-text {
    font-size: 0.8rem;
  }
}
</style>
