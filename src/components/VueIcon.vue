<template>
  <div class="vue-icon-container">
    <div class="icon-wrapper" ref="iconWrapper">
      <img 
        src="/vue_logo.png" 
        alt="Vue.js Logo" 
        class="vue-logo"
        @load="startAnimation"
      />
      <div class="magic-glow-background" ref="magicGlowBackground">
        <div class="magic-glow-layer layer-1"></div>
        <div class="magic-glow-layer layer-2"></div>
        <div class="magic-glow-layer layer-3"></div>
        <div class="magic-glow-layer layer-4"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'

const iconWrapper = ref<HTMLElement>()
const magicGlowBackground = ref<HTMLElement>()

const startAnimation = () => {
  // Vue图标进入动画
  const iconTimeline = gsap.timeline({ delay: 4 }) // 4秒后开始，配合加快的文字动画
  
  if (iconWrapper.value) {
    // 先设置初始状态
    gsap.set(iconWrapper.value, {
      opacity: 0,
      scale: 0.3,
      rotation: -10
    })
    
    iconTimeline.to(iconWrapper.value, {
      opacity: 1,
      scale: 1,
      rotation: 0,
      duration: 1.5,
      ease: "back.out(1.7)"
    })
  }
  
  // 光晕效果动画
  if (magicGlowBackground.value) {
    const layers = magicGlowBackground.value.querySelectorAll('.magic-glow-layer')
    
    // 设置每层的初始旋转角度
    const rotations = [0, 45, -30, 20]
    layers.forEach((layer, index) => {
      gsap.set(layer, {
        opacity: 0,
        scale: 0.1,
        rotation: rotations[index] || 0
      })
    })
    
    // 创建主光晕时间线
    const mainGlowTimeline = gsap.timeline({ delay: 5.8 }) // 调整为5.8秒，配合新的Vue图标时机
    
    // 光晕展开动画
    layers.forEach((layer, index) => {
      mainGlowTimeline.to(layer, {
        opacity: 1,
        scale: 1,
        duration: 1.8,
        ease: "power3.out"
      }, index * 0.15)
    })
    
    // 在展开完成后开始呼吸动画
    mainGlowTimeline.to(layers, {
      scale: 1.06,
      duration: 3,
      yoyo: true,
      repeat: -1,
      ease: "sine.inOut",
      stagger: {
        each: 0.3,
        from: "center"
      }
    }, "+=0.5") // 展开完成后0.5秒开始呼吸
  }
}

onMounted(() => {
  // 备用动画触发
  setTimeout(() => {
    startAnimation()
  }, 4500) // 调整为4.5秒，配合加快的动画节奏
})
</script>

<style scoped>
.vue-icon-container {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.icon-wrapper {
  position: relative;
  opacity: 0;
  transform: scale(0.3) rotate(-10deg);
  display: flex;
  align-items: center;
  justify-content: center;
}

.vue-logo {
  width: 400px;
  height: 400px;
  object-fit: contain;
  filter: drop-shadow(0 0 30px rgba(64, 184, 131, 0.5));
  transform: scale(1.05);
  transition: all 0.3s ease;
  z-index: 2;
  position: relative;
}



.magic-glow-background {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 800px;
  height: 800px;
  z-index: 0;
  pointer-events: none;
}

.magic-glow-layer {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  opacity: 0;
}

.layer-1 {
  width: 600px;
  height: 450px;
  background: radial-gradient(
    ellipse,
    rgba(34, 197, 94, 0.25) 0%,
    rgba(44, 226, 126, 0.18) 25%,
    rgba(16, 185, 129, 0.12) 50%,
    rgba(5, 150, 105, 0.08) 75%,
    transparent 100%
  );
  transform: translate(-50%, -50%) scale(0.1);
  opacity: 0;
}

.layer-2 {
  width: 450px;
  height: 600px;
  background: radial-gradient(
    ellipse,
    rgba(21, 128, 61, 0.22) 0%,
    rgba(34, 197, 94, 0.15) 30%,
    rgba(44, 226, 126, 0.1) 60%,
    transparent 100%
  );
  transform: translate(-50%, -50%) scale(0.1) rotate(45deg);
  opacity: 0;
}

.layer-3 {
  width: 520px;
  height: 380px;
  background: radial-gradient(
    ellipse,
    rgba(16, 185, 129, 0.18) 0%,
    rgba(20, 184, 166, 0.12) 40%,
    rgba(13, 148, 136, 0.08) 70%,
    transparent 100%
  );
  transform: translate(-50%, -50%) scale(0.1) rotate(-30deg);
  opacity: 0;
}

.layer-4 {
  width: 680px;
  height: 340px;
  background: radial-gradient(
    ellipse,
    rgba(5, 150, 105, 0.15) 0%,
    rgba(16, 185, 129, 0.1) 35%,
    rgba(34, 197, 94, 0.06) 70%,
    transparent 100%
  );
  transform: translate(-50%, -50%) scale(0.1) rotate(20deg);
  opacity: 0;
}



/* 响应式调整 */
@media (max-width: 768px) {
  .vue-logo {
    width: 300px;
    height: 300px;
  }
  
  .glow-effect {
    width: 360px;
    height: 360px;
  }
  
  .magic-glow-background {
    width: 600px;
    height: 600px;
  }
  
  .layer-1 { width: 450px; height: 340px; }
  .layer-2 { width: 340px; height: 450px; }
  .layer-3 { width: 390px; height: 285px; }
  .layer-4 { width: 510px; height: 255px; }
}

@media (max-width: 480px) {
  .vue-logo {
    width: 250px;
    height: 250px;
  }
  
  .glow-effect {
    width: 300px;
    height: 300px;
  }
  
  .magic-glow-background {
    width: 480px;
    height: 480px;
  }
  
  .layer-1 { width: 360px; height: 270px; }
  .layer-2 { width: 270px; height: 360px; }
  .layer-3 { width: 315px; height: 230px; }
  .layer-4 { width: 410px; height: 205px; }
}
</style> 
