<template>
  <div class="text-animation-container">
    <!-- 主标题 -->
    <div class="main-title" ref="mainTitle">Vue Conf 2025</div>
    
    <svg 
      width="600" 
      height="220" 
      viewBox="0 0 600 220" 
      xmlns="http://www.w3.org/2000/svg"
      class="quote-svg"
    >
      <!-- 背景装饰 -->
      <defs>
        <linearGradient id="textGradient" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" style="stop-color:#ffffff;stop-opacity:0.9" />
          <stop offset="100%" style="stop-color:#e0e0e0;stop-opacity:0.8" />
        </linearGradient>
        
        <linearGradient id="textGradientGreen" x1="0%" y1="0%" x2="100%" y2="0%">
          <stop offset="0%" style="stop-color:#2ce27e;stop-opacity:0.9" />
          <stop offset="100%" style="stop-color:#1ea228;stop-opacity:0.8" />
        </linearGradient>
        
        <filter id="glow">
          <feGaussianBlur stdDeviation="2" result="coloredBlur"/>
          <feMerge> 
            <feMergeNode in="coloredBlur"/>
            <feMergeNode in="SourceGraphic"/>
          </feMerge>
        </filter>
      </defs>

      <!-- 主要文字内容 -->
      <g class="quote-text" ref="quoteText">
                <!-- 第一行 -->
        <text x="15" y="40" class="line line-1" fill="url(#textGradient)" font-family="'Inter Display', 'Georgia', serif" font-size="22" font-weight="bold">
          <tspan class="word word-1">There</tspan>
          <tspan class="word word-2" dx="8">must</tspan>
          <tspan class="word word-3" dx="8">be</tspan>
          <tspan class="word word-4" dx="8">a</tspan>
          <tspan class="word word-5" dx="8">beginning</tspan>
          <tspan class="word word-6" dx="8">of</tspan>
          <tspan class="word word-7" dx="8">any</tspan>
          <tspan class="word word-8" dx="8">great</tspan>
          <tspan class="word word-9" dx="8">matter,</tspan>
        </text>
        
        <!-- 第二行 -->
        <text x="15" y="70" class="line line-2" fill="url(#textGradient)" font-family="'Inter Display', 'Georgia', serif" font-size="22" font-weight="bold">
          <tspan class="word word-10">but</tspan>
          <tspan class="word word-11" dx="8">the</tspan>
          <tspan class="word word-12" dx="8">continuing</tspan>
          <tspan class="word word-13" dx="8">unto</tspan>
          <tspan class="word word-14" dx="8">the</tspan>
          <tspan class="word word-15" dx="8">end</tspan>
          <tspan class="word word-16" dx="8">until</tspan>
          <tspan class="word word-17" dx="8">it</tspan>
          <tspan class="word word-18" dx="8">be</tspan>
        </text>
        
        <!-- 第三行 -->
        <text x="15" y="100" class="line line-3" fill="url(#textGradient)" font-family="'Inter Display', 'Georgia', serif" font-size="22" font-weight="bold">
          <tspan class="word word-19">thoroughly</tspan>
          <tspan class="word word-20" dx="8">finished</tspan>
          <tspan class="word word-21" dx="8">yields</tspan>
          <tspan class="word word-22" dx="8">the</tspan>
          <tspan class="word word-23" dx="8">true</tspan>
          <tspan class="word word-24" dx="8">glory.</tspan>
                </text>
        
        <!-- 署名 -->
        <text x="300" y="160" class="signature" fill="url(#textGradient)" font-family="'Inter Display', 'Georgia', serif" font-size="18" font-weight="bold" opacity="0.8">
          <tspan class="word word-25">Sir</tspan>
          <tspan class="word word-26" dx="8">Francis</tspan>
          <tspan class="word word-27" dx="8">Drake,</tspan>
          <tspan class="word word-28" dx="8">1587</tspan>
        </text>
      </g>
      
      <!-- 装饰性引号 -->
      <text x="5" y="30" fill="rgba(255,255,255,0.3)" font-family="'Inter Display', 'Georgia', serif" font-size="28" font-weight="bold" class="quote-mark-start" ref="quoteMarkStart">"</text>
      <text x="570" y="110" fill="rgba(255,255,255,0.3)" font-family="'Inter Display', 'Georgia', serif" font-size="28" font-weight="bold" class="quote-mark-end" ref="quoteMarkEnd">"</text>
    </svg>
  </div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'

const quoteText = ref<SVGGElement>()
const quoteMarkStart = ref<SVGTextElement>()
const quoteMarkEnd = ref<SVGTextElement>()
const mainTitle = ref<HTMLElement>()

onMounted(() => {
  // 启动动画
  startAnimation()
})

const startAnimation = () => {
  const words = document.querySelectorAll('.word')
  
  words.forEach((word, index) => {
    // 每个单词延迟显示
    setTimeout(() => {
      word.classList.add('animate-in')
    }, index * 120) // 每个单词间隔120ms
  })
  
  // 引号动画
  setTimeout(() => {
    document.querySelector('.quote-mark-start')?.classList.add('animate-in')
  }, 50)
  
  setTimeout(() => {
    document.querySelector('.quote-mark-end')?.classList.add('animate-in')
  }, words.length * 120 + 300)
  
  // 主标题在4秒时与Vue图标同步出现
  setTimeout(() => {
    if (mainTitle.value) {
      mainTitle.value.classList.add('title-animate-in')
    }
  }, 4000)
  
  // 在Vue图标出现后（4.5秒）将文字变为绿色，提前2秒
  setTimeout(() => {
    changeTextToGreen()
  }, 4500)
}

const changeTextToGreen = () => {
  if (!quoteText.value) return

  const textElements = quoteText.value.querySelectorAll('text')
  
  // 添加过渡类，让每个元素依次变绿
  textElements.forEach((textElement, index) => {
    setTimeout(() => {
      textElement.style.fill = 'url(#textGradientGreen)'
      textElement.classList.add('text-green')
    }, index * 300) // 每个元素间隔300ms
  })
  
  // 引号颜色变化
  setTimeout(() => {
    if (quoteMarkStart.value) {
      quoteMarkStart.value.style.fill = 'rgba(44, 226, 126, 0.6)'
      quoteMarkStart.value.classList.add('text-green')
    }
    
    if (quoteMarkEnd.value) {
      quoteMarkEnd.value.style.fill = 'rgba(44, 226, 126, 0.6)'
      quoteMarkEnd.value.classList.add('text-green')
    }
  }, 600) // 在文字变化之后
  
  // 主标题颜色变化
  setTimeout(() => {
    if (mainTitle.value) {
      mainTitle.value.classList.add('title-green')
    }
  }, 300) // 和第一个文字元素同时变化
}
</script>

<style scoped>
@font-face {
  font-family: 'Inter Display';
  src: url('/Inter-Display-Bold.woff2') format('woff2');
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}

.text-animation-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 40px;
}

.main-title {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 7rem;
  font-weight: bold;
  color: #ffffff;
  text-align: center;
  opacity: 0;
  transform: translateY(-20px) scale(0.8);
  transition: all 1.2s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  text-shadow: 
    0 0 20px rgba(255, 255, 255, 0.3),
    0 0 40px rgba(255, 255, 255, 0.15),
    0 2px 8px rgba(0, 0, 0, 0.5);
  letter-spacing: 0.05em;
  line-height: 1.1;
}

.main-title.title-animate-in {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.main-title.title-green {
  color: #2ce27e;
  text-shadow: 
    0 0 25px rgba(44, 226, 126, 0.4),
    0 0 50px rgba(44, 226, 126, 0.2),
    0 2px 8px rgba(0, 0, 0, 0.5);
  transition: all 0.8s ease-in-out;
}

.quote-svg {
  filter: drop-shadow(0 0 10px rgba(255, 255, 255, 0.1));
}

.word {
  opacity: 0;
  transform: translateY(5px);
  transition: all 0.6s ease-out;
}

.word.animate-in {
  opacity: 1;
  transform: translateY(0);
}

text {
  transition: fill 0.8s ease-in-out;
}

.text-green {
  filter: drop-shadow(0 0 8px rgba(44, 226, 126, 0.4));
  animation: greenGlow 0.6s ease-out;
}

@keyframes greenGlow {
  0% {
    filter: drop-shadow(0 0 4px rgba(44, 226, 126, 0.2));
  }
  100% {
    filter: drop-shadow(0 0 8px rgba(44, 226, 126, 0.4));
  }
}

.quote-mark-start,
.quote-mark-end {
  opacity: 0;
  transform: scale(0.5);
  transition: all 0.8s ease-out;
}

.quote-mark-start.animate-in,
.quote-mark-end.animate-in {
  opacity: 1;
  transform: scale(1);
}

.signature {
  font-weight: bold;
}

/* 响应式调整 */
@media (max-width: 768px) {
  .main-title {
    font-size: 3rem;
  }
  
  .text-animation-container {
    gap: 30px;
  }
  
        .quote-svg {
    width: 480px;
    height: 180px;
  }
}

@media (max-width: 480px) {
  .main-title {
    font-size: 2.2rem;
    letter-spacing: 0.03em;
  }
  
  .text-animation-container {
    gap: 20px;
  }
  
        .quote-svg {
    width: 380px;
    height: 160px;
  }
}
</style> 
