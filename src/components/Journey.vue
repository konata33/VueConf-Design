<template>
  <div class="journey-container">
    <div class="journey-title-container">
      <h2 class="journey-title">Journey</h2>
      <div class="journey-underline"></div>
    </div>
    <div class="timeline-wrapper">
      <div class="timeline-container" ref="timelineContainer">
        <div class="timeline-line" ref="timelineLine"></div>
        <div 
          v-for="(event) in timelineEvents" 
          :key="event.year"
          class="timeline-item"
          :class="{ 'timeline-item-current': event.isCurrent }"
          ref="timelineItems"
        >
          <div class="timeline-card" @click="handleCardClick(event)">
            <div class="timeline-image">
              <img :src="`/journey/${event.year}.png`" :alt="`VueConf ${event.year}`" />
              <div class="timeline-image-overlay"></div>
            </div>
            <div class="timeline-year-badge">{{ event.year }}</div>
            <div class="timeline-content">
              <h3 class="timeline-title">{{ event.title }}</h3>
              <div class="timeline-brief">
                {{ event.description }}
              </div>
              <div class="timeline-tags">
                <div 
                  v-for="(highlight, index) in event.highlights.slice(0, 2)" 
                  :key="highlight"
                  class="timeline-tag"
                  :style="{ animationDelay: `${index * 0.1}s` }"
                >
                  <span class="tag-icon">✨</span>
                  {{ highlight }}
                </div>
              </div>
            </div>
            <div class="timeline-hover-info">
              <p class="timeline-description">{{ event.description }}</p>
              <div class="timeline-highlights">
                <div 
                  v-for="highlight in event.highlights.slice(0, 3)" 
                  :key="highlight"
                  class="timeline-highlight"
                >
                  {{ highlight }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
import { gsap } from 'gsap'
import { onMounted, ref } from 'vue'
const timelineItems = ref<HTMLElement[]>([])
const timelineContainer = ref<HTMLElement>()
const handleCardClick = (event: any) => {
  const isMobile = window.innerWidth <= 768
  if (isMobile) {
    const message = `${event.title}\n\n${event.description}\n\n亮点：\n${event.highlights.join('\n• ')}`
    alert(message)
  } else {
    console.log('Card clicked:', event.title)
  }
}
const timelineEvents = ref([
  {
    year: '2017',
    title: 'VueConf CN 北京',
    location: '北京航空航天大学',
    date: '05.20',
    description: '全球首届Vue.js开发者大会，Vue.js作者尤雨溪出席并发表主题演讲。',
    highlights: ['Vue 2017 现状与展望', 'Vue SSR实践', '工程化优雅设计'],
    isCurrent: false
  },
  {
    year: '2018',
    title: 'VueConf CN 杭州',
    location: '杭州金逸影视中心',
    date: '11.24',
    description: '第二届Vue.js开发者大会，Vue.js官方举办，探讨声明式交互能力。',
    highlights: ['Vue CLI维护经验', '声明式交互能力', 'CRDT技术展望'],
    isCurrent: false
  },
  {
    year: '2019',
    title: 'VueConf CN 上海',
    location: '上海',
    date: '',
    description: 'Vue.js生态系统进一步发展，更多企业级应用案例分享。',
    highlights: ['企业级应用', '生态系统发展', 'Vue 3.0 展望'],
    isCurrent: false
  },
  {
    year: '2021',
    title: 'VueConf CN 杭州',
    location: '杭州',
    date: '',
    description: 'Vue 3.0正式版发布后的首次大会，Composition API成为焦点。',
    highlights: ['Vue 3.0 新特性', 'Composition API', '性能优化'],
    isCurrent: false
  },
  {
    year: '2022',
    title: 'VueConf CN 线上',
    location: '线上会议',
    date: '',
    description: '疫情期间首次线上举办，全球开发者齐聚云端交流Vue最新技术。',
    highlights: ['线上会议新体验', '全球开发者参与', 'Vue生态丰富'],
    isCurrent: false
  },
  {
    year: '2024',
    title: 'VueConf CN 深圳',
    location: '深圳',
    date: '',
    description: 'Vue生态系统日趋完善，工具链和周边库更加成熟。',
    highlights: ['工具链完善', 'Vue Vapor模式', '生态系统成熟'],
    isCurrent: false
  },
  {
    year: '2025',
    title: 'VueConf CN 新篇章',
    location: '即将揭晓',
    date: '',
    description: 'Vue.js迎来新的发展机遇，与开发者共同开创前端新时代。',
    highlights: ['新技术突破', '社区蓬勃发展', '未来无限可能'],
    isCurrent: true
  }
])
onMounted(() => {
  const isMobile = window.innerWidth <= 768
  if (!isMobile) {
    if (timelineItems.value.length > 0) {
      let totalWidth = 0
      timelineItems.value.forEach((item) => {
        gsap.set(item, {
          left: totalWidth,
          transform: 'translateY(-50%)'
        })
        totalWidth += 420
      })
      const container = timelineContainer.value
      if (container) {
        const scrollWidth = totalWidth + 150
        gsap.set(container, {
          width: scrollWidth + 'px'
        })
      }
    }
  }
  setTimeout(() => {
    if (!isMobile) {
      gsap.fromTo('.timeline-line', 
        { width: 0 },
        { width: '100%', duration: 1.5, ease: "power2.out" }
      )
    }
    if (timelineItems.value.length > 0) {
      timelineItems.value.forEach((item, index) => {
        const initialY = isMobile ? 30 : 50
        const initialScale = isMobile ? 0.95 : 0.8
        gsap.fromTo(item,
          { opacity: 0, y: initialY, scale: initialScale },
          { 
            opacity: 1, 
            y: 0, 
            scale: 1,
            duration: isMobile ? 0.6 : 0.8, 
            ease: isMobile ? "power2.out" : "back.out(1.4)",
            delay: 0.2 + index * (isMobile ? 0.05 : 0.1)
          }
        )
      })
    }
  }, 800)
})
</script>
<style scoped>
@font-face {
  font-family: 'Inter Display';
  src: url('/Inter-Display-Bold.woff2') format('woff2');
  font-weight: bold;
  font-style: normal;
  font-display: swap;
}
@font-face {
  font-family: 'Inter Display Bold';
  src: url('/Inter-Display-Bold.woff2') format('woff2');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
.journey-container {
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
  padding: 0;
  z-index: 10;
}
.journey-title-container {
  position: absolute;
  top: 60px;
  left: 60px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 15px;
  z-index: 20;
}
.journey-title {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 4rem;
  font-weight: bold;
  color: #2ce27e;
  margin: 0;
  text-align: left;
  letter-spacing: 0.05em;
  text-shadow: 
    0 0 20px rgba(44, 226, 126, 0.3),
    0 0 40px rgba(44, 226, 126, 0.15),
    0 2px 8px rgba(0, 0, 0, 0.5);
  opacity: 0;
  transform: translateY(20px);
  animation: titleFadeIn 1s ease-out 0.5s forwards;
}
.journey-underline {
  width: 120px;
  height: 4px;
  background: linear-gradient(90deg, 
    rgba(44, 226, 126, 1) 0%, 
    rgba(44, 226, 126, 0.8) 30%, 
    rgba(44, 226, 126, 0.4) 70%, 
    rgba(44, 226, 126, 0) 100%
  );
  border-radius: 2px;
  opacity: 0;
  transform: scaleX(0);
  transform-origin: left;
  animation: underlineExpand 1.2s ease-out 1s forwards;
  box-shadow: 
    0 0 10px rgba(44, 226, 126, 0.5),
    0 0 20px rgba(44, 226, 126, 0.3);
}
.timeline-wrapper {
  position: absolute;
  bottom: 150px;
  left: 0;
  right: 0;
  height: 580px;
  overflow-x: auto;
  overflow-y: hidden;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding: 0 60px;
  box-sizing: border-box;
}
.timeline-wrapper::-webkit-scrollbar {
  display: none;
}
.timeline-container {
  position: relative;
  width: max-content;
  height: 100%;
  display: block;
}
.timeline-line {
  position: absolute;
  left: 0;
  top: 50%;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, 
    rgba(44, 226, 126, 0.6) 0%,
    rgba(44, 226, 126, 0.8) 50%,
    rgba(44, 226, 126, 0.6) 100%
  );
  transform: translateY(-50%);
  border-radius: 1px;
  box-shadow: 
    0 0 8px rgba(44, 226, 126, 0.4),
    0 0 16px rgba(44, 226, 126, 0.2);
  z-index: 1;
}
.timeline-item {
  position: absolute;
  top: 50%;
  left: 0;
  display: inline-block;
  margin-right: 60px;
  opacity: 0;
  z-index: 10;
  transform: translateY(-50%);
}
.timeline-item:first-child {
  margin-left: 0;
}
.timeline-item:last-child {
  margin-right: 150px;
}
.timeline-card {
  position: relative;
  width: 360px;
  height: 566px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  box-shadow: 
    0 8px 32px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(255, 255, 255, 0.05);
}
.timeline-card:hover {
  background: rgba(255, 255, 255, 0.08);
  border-color: rgba(44, 226, 126, 0.4);
  transform: translateY(-12px) scale(1.03);
  box-shadow: 
    0 20px 60px rgba(0, 0, 0, 0.4),
    0 0 0 1px rgba(44, 226, 126, 0.3);
}
.timeline-card:hover .timeline-year-badge {
  transform: scale(1.05);
  box-shadow: 
    0 6px 24px rgba(44, 226, 126, 0.7),
    0 3px 12px rgba(0, 0, 0, 0.5),
    0 0 0 3px rgba(255, 255, 255, 0.2);
}
.timeline-item-current .timeline-card:hover .timeline-year-badge {
  transform: scale(1.05);
  box-shadow: 
    0 6px 30px rgba(255, 215, 0, 0.9),
    0 3px 15px rgba(0, 0, 0, 0.6),
    0 0 0 4px rgba(255, 255, 255, 0.3);
}
.timeline-item-current .timeline-card {
  background: rgba(255, 215, 0, 0.1);
  border-color: rgba(255, 215, 0, 0.4);
  box-shadow: 
    0 8px 32px rgba(255, 215, 0, 0.15),
    0 0 0 1px rgba(255, 215, 0, 0.1);
}
.timeline-item-current .timeline-card:hover {
  border-color: rgba(255, 215, 0, 0.6);
  box-shadow: 
    0 20px 60px rgba(255, 215, 0, 0.2),
    0 0 0 1px rgba(255, 215, 0, 0.4);
}
.timeline-image {
  position: relative;
  width: 100%;
  height: 340px;
  overflow: hidden;
  border-radius: 16px 16px 0 0;
  background: transparent;
  display: flex;
  align-items: center;
  justify-content: center;
}
.timeline-image img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  object-position: center;
  transition: transform 0.5s ease;
  min-height: 100%;
  min-width: 100%;
}
.timeline-card:hover .timeline-image img {
  transform: scale(1.08);
}
.timeline-image-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.4) 0%,
    rgba(0, 0, 0, 0.2) 30%,
    rgba(0, 0, 0, 0.3) 60%,
    rgba(0, 0, 0, 0.7) 100%
  );
  z-index: 2;
}
.timeline-year-badge {
  position: absolute;
  top: 15px;
  right: 15px;
  background: rgba(44, 226, 126, 1);
  color: rgba(15, 15, 35, 1);
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1.2rem;
  font-weight: bold;
  padding: 10px 18px;
  border-radius: 30px;
  z-index: 15;
  box-shadow: 
    0 4px 20px rgba(44, 226, 126, 0.6),
    0 2px 8px rgba(0, 0, 0, 0.4),
    0 0 0 2px rgba(255, 255, 255, 0.1);
  transition: all 0.3s ease;
  letter-spacing: 0.05em;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
}
.timeline-item-current .timeline-year-badge {
  background: linear-gradient(135deg, #ffd700, #ffed4e);
  color: rgba(0, 0, 0, 1);
  box-shadow: 
    0 4px 24px rgba(255, 215, 0, 0.8),
    0 2px 12px rgba(0, 0, 0, 0.5),
    0 0 0 3px rgba(255, 255, 255, 0.2);
  font-size: 1.3rem;
  padding: 12px 20px;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.3);
}
.timeline-content {
  position: relative;
  padding: 20px;
  height: calc(100% - 340px);
  display: flex;
  flex-direction: column;
  z-index: 3;
}
.timeline-hover-info {
  position: absolute;
  top: 338px;
  left: 2px;
  right: 2px;
  bottom: 2px;
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-radius: 0 0 14px 14px;
  padding: 30px 25px 25px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transform: translateY(30px) scale(0.95);
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 60;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-top: none;
}
.timeline-card:hover .timeline-hover-info {
  opacity: 1;
  transform: translateY(0) scale(1);
}
.timeline-title {
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  font-size: 1.3rem;
  font-weight: normal;
  color: #ffffff;
  margin: 0 0 16px 0;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
  line-height: 1.2;
  letter-spacing: 0.02em;
}
.timeline-brief {
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.7);
  line-height: 1.4;
  margin-bottom: 12px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
.timeline-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 12px;
}
.timeline-tag {
  display: flex;
  align-items: center;
  gap: 4px;
  background: linear-gradient(135deg, rgba(44, 226, 126, 0.15), rgba(44, 226, 126, 0.08));
  border: 1px solid rgba(44, 226, 126, 0.3);
  border-radius: 12px;
  padding: 3px 8px;
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  font-size: 0.7rem;
  color: rgba(44, 226, 126, 0.9);
  font-weight: normal;
  white-space: nowrap;
  animation: tagFadeIn 0.6s ease-out forwards;
  opacity: 0;
  transform: translateY(10px);
}
.tag-icon {
  font-size: 0.7rem;
}
.timeline-description {
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.9);
  line-height: 1.7;
  margin: 0 0 25px 0;
  text-align: center;
  font-weight: normal;
  max-width: 300px;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
}
.timeline-highlights {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  max-width: 300px;
}
.timeline-highlight {
  background: linear-gradient(135deg, rgba(44, 226, 126, 0.3), rgba(44, 226, 126, 0.15));
  border: 1px solid rgba(44, 226, 126, 0.6);
  border-radius: 20px;
  padding: 10px 18px;
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  font-size: 0.95rem;
  color: rgba(44, 226, 126, 1);
  font-weight: normal;
  white-space: nowrap;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.6);
  box-shadow: 
    0 4px 12px rgba(44, 226, 126, 0.2),
    0 0 0 1px rgba(255, 255, 255, 0.05);
  transition: all 0.3s ease;
}
.timeline-highlight:hover {
  background: linear-gradient(135deg, rgba(44, 226, 126, 0.4), rgba(44, 226, 126, 0.2));
  border-color: rgba(44, 226, 126, 0.8);
  transform: translateY(-2px);
  box-shadow: 
    0 6px 16px rgba(44, 226, 126, 0.3),
    0 0 0 2px rgba(255, 255, 255, 0.1);
}
.timeline-item-current .timeline-hover-info .timeline-highlight {
  background: linear-gradient(135deg, rgba(255, 215, 0, 0.35), rgba(255, 215, 0, 0.2));
  border-color: rgba(255, 215, 0, 0.7);
  color: rgba(255, 215, 0, 1);
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
  box-shadow: 
    0 4px 12px rgba(255, 215, 0, 0.25),
    0 0 0 1px rgba(255, 255, 255, 0.1);
}
.timeline-item-current .timeline-hover-info .timeline-highlight:hover {
  background: linear-gradient(135deg, rgba(255, 215, 0, 0.45), rgba(255, 215, 0, 0.25));
  border-color: rgba(255, 215, 0, 0.9);
  transform: translateY(-2px);
  box-shadow: 
    0 6px 16px rgba(255, 215, 0, 0.35),
    0 0 0 2px rgba(255, 255, 255, 0.15);
}
.timeline-item-current .timeline-tag {
  background: linear-gradient(135deg, rgba(255, 215, 0, 0.2), rgba(255, 215, 0, 0.1));
  border-color: rgba(255, 215, 0, 0.4);
  color: rgba(255, 215, 0, 1);
  font-family: 'Inter Display Bold', 'Inter Display', 'Georgia', serif;
}
@keyframes titleFadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes underlineExpand {
  from {
    opacity: 0;
    transform: scaleX(0);
  }
  to {
    opacity: 1;
    transform: scaleX(1);
  }
}
@keyframes scrollHintFloat {
  0%, 100% {
    transform: translateX(-50%) translateY(0);
  }
  50% {
    transform: translateX(-50%) translateY(-5px);
  }
}
@keyframes tagFadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@media (max-width: 768px) {
  .journey-container {
    height: 100vh;
    overflow: hidden;
  }
  .journey-title-container {
    top: 30px;
    left: 30px;
  }
  .journey-title {
    font-size: 2.8rem;
  }
  .journey-underline {
    width: 80px;
  }
  .timeline-wrapper {
    position: absolute;
    top: 120px;
    left: 0;
    right: 0;
    bottom: 0;
    height: calc(100vh - 120px);
    overflow-x: hidden;
    overflow-y: auto;
    padding: 0 30px 30px 30px;
    -webkit-overflow-scrolling: touch;
  }
  .timeline-container {
    display: flex;
    flex-direction: column;
    gap: 25px;
    width: 100%;
    height: auto;
    min-height: 100%;
  }
  .timeline-line {
    display: none; 
  }
  .timeline-item {
    position: relative;
    top: auto;
    left: auto;
    transform: none;
    margin: 0;
    width: 100%;
    opacity: 1;
  }
  .timeline-card {
    width: 100%;
    height: auto;
    min-height: 400px;
    transform: none !important;
  }
  .timeline-card:hover {
    transform: none !important;
    background: rgba(255, 255, 255, 0.05);
    border-color: rgba(255, 255, 255, 0.15);
    box-shadow: 
      0 8px 32px rgba(0, 0, 0, 0.3),
      0 0 0 1px rgba(255, 255, 255, 0.05);
  }
  .timeline-card:active {
    transform: scale(0.98) !important;
    transition: transform 0.1s ease;
  }
  .timeline-card {
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
  }
  .timeline-image {
    height: 250px;
  }
  .timeline-content {
    padding: 20px;
    height: auto;
    min-height: 130px;
  }
  .timeline-title {
    font-size: 1.2rem;
    margin-bottom: 12px;
  }
  .timeline-brief {
    font-size: 0.85rem;
    margin-bottom: 12px;
    -webkit-line-clamp: 3;
  }
  .timeline-tags {
    gap: 8px;
    margin-bottom: 0;
  }
  .timeline-tag {
    font-size: 0.7rem;
    padding: 4px 8px;
  }
  .timeline-hover-info {
    display: none;
  }
}
@media (max-width: 480px) {
  .journey-container {
    height: 100vh;
    overflow: hidden;
  }
  .journey-title-container {
    top: 20px;
    left: 20px;
  }
  .journey-title {
    font-size: 2.2rem;
  }
  .journey-underline {
    width: 70px;
    height: 3px;
  }
  .timeline-wrapper {
    position: absolute;
    top: 100px;
    left: 0;
    right: 0;
    bottom: 0;
    height: calc(100vh - 100px);
    overflow-x: hidden;
    overflow-y: auto;
    padding: 0 20px 20px 20px;
    -webkit-overflow-scrolling: touch;
  }
  .timeline-container {
    display: flex;
    flex-direction: column;
    gap: 20px;
    width: 100%;
  }
  .timeline-card {
    width: 100%;
    height: auto;
    min-height: 350px;
  }
  .timeline-image {
    height: 200px;
  }
  .timeline-content {
    padding: 16px;
    height: auto;
    min-height: 120px;
  }
  .timeline-title {
    font-size: 1.1rem;
    margin-bottom: 10px;
  }
  .timeline-brief {
    font-size: 0.8rem;
    margin-bottom: 10px;
    -webkit-line-clamp: 2;
  }
  .timeline-tags {
    gap: 6px;
  }
  .timeline-tag {
    font-size: 0.65rem;
    padding: 3px 6px;
  }
  .timeline-year-badge {
    font-size: 1rem;
    padding: 8px 14px;
    top: 12px;
    right: 12px;
  }
  .timeline-item-current .timeline-year-badge {
    font-size: 1.1rem;
    padding: 10px 16px;
  }
  .timeline-hover-info {
    display: none;
  }
}
@media (max-width: 360px) {
  .journey-title-container {
    top: 15px;
    left: 15px;
  }
  .journey-title {
    font-size: 1.8rem;
  }
  .journey-underline {
    width: 60px;
    height: 2px;
  }
  .timeline-wrapper {
    top: 80px;
    height: calc(100vh - 80px);
    padding: 0 15px 15px 15px;
  }
  .timeline-container {
    gap: 15px;
  }
  .timeline-card {
    min-height: 320px;
    border-radius: 12px;
  }
  .timeline-image {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  .timeline-content {
    padding: 14px;
    min-height: 110px;
  }
  .timeline-title {
    font-size: 1rem;
    margin-bottom: 8px;
  }
  .timeline-brief {
    font-size: 0.75rem;
    margin-bottom: 8px;
  }
  .timeline-tags {
    gap: 4px;
  }
  .timeline-tag {
    font-size: 0.6rem;
    padding: 2px 5px;
    border-radius: 8px;
  }
  .timeline-year-badge {
    font-size: 0.9rem;
    padding: 6px 12px;
    top: 10px;
    right: 10px;
  }
  .timeline-item-current .timeline-year-badge {
    font-size: 1rem;
    padding: 8px 14px;
  }
}
</style> 
