<template>
  <div class="us-container">
    <div class="us-title-container">
      <h2 class="us-title">Member</h2>
      <div class="us-underline"></div>
    </div>
    <div class="team-members" ref="teamMembersRef">
      <div 
        v-for="member in teamMembers" 
        :key="member.id"
        class="member-card"
        :class="`member-${member.id}`"
        :style="{ backgroundImage: `url(${member.avatar})` }"
        @click="handleMemberClick(member)"
      >
        <div class="member-info">
          <div class="member-name">{{ member.name }}</div>
        </div>
        <div v-if="selectedMember?.id === member.id && isExpanded" class="member-detail">
          <div class="detail-content">
            <div class="detail-header">
              <div class="swipe-indicator"></div>
              <h2>{{ member.name }}</h2>
              <p class="detail-role">{{ member.role }}</p>
              <button @click.stop="closeDetail" class="close-btn">✕</button>
            </div>
            <div class="detail-body">
              <div class="detail-section">
                <h3>About</h3>
                <p>{{ member.bio }}</p>
              </div>
              <div class="detail-section">
                <h3>Skills</h3>
                <div class="skills-list">
                  <span v-for="skill in member.skills" :key="skill" class="skill-tag">
                    {{ skill }}
                  </span>
                </div>
              </div>
              <div class="detail-section">
                <h3>Achievements</h3>
                <ul class="achievements-list">
                  <li v-for="achievement in member.achievements" :key="achievement">
                    {{ achievement }}
                  </li>
                </ul>
              </div>
              <div class="detail-section">
                <h3>GitHub</h3>
                <a :href="`https://github.com/${member.github}`" target="_blank" class="github-link">
                  @{{ member.github }}
                </a>
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
import { nextTick, onMounted, ref } from 'vue'
const teamMembers = ref([
  {
    id: 1,
    name: 'Evan You',
    role: 'Vue.js Creator',
    avatar: '/avatars/Evan.png',
    bio: 'Creator of Vue.js and Vite. Previously worked at Google and Meteor.',
    skills: ['JavaScript', 'Vue.js', 'Vite', 'Open Source'],
    achievements: ['Created Vue.js framework', 'Built Vite build tool', 'Google Chrome team member'],
    github: 'yyx990803'
  },
  {
    id: 2,
    name: 'Anthony Fu',
    role: 'Core Team Member', 
    avatar: '/avatars/antfu.png',
    bio: 'Vue core team member, creator of VueUse, Vitest, and many other tools.',
    skills: ['TypeScript', 'Vue.js', 'Nuxt', 'Vite'],
    achievements: ['VueUse creator', 'Vitest creator', 'Nuxt team member'],
    github: 'antfu'
  },
  {
    id: 3,
    name: 'posva',
    role: 'Vue Router Maintainer',
    avatar: '/avatars/posva.png',
    bio: 'Maintainer of Vue Router and Pinia. Vue.js core team member.',
    skills: ['Vue.js', 'Vue Router', 'Pinia', 'TypeScript'],
    achievements: ['Vue Router maintainer', 'Pinia creator', 'Vue core team'],
    github: 'posva'
  },
  {
    id: 4,
    name: 'Patak',
    role: 'Vite Core Team',
    avatar: '/avatars/patak.png',
    bio: 'Vite core team member and maintainer of ecosystem tools.',
    skills: ['Vite', 'Rollup', 'Build Tools', 'JavaScript'],
    achievements: ['Vite core team member', 'Ecosystem maintainer'],
    github: 'patak-dev'
  },
  {
    id: 5,
    name: 'sxzz',
    role: 'Vue Core Team & Vue Vapor',
    avatar: '/avatars/sxzz.png',
    bio: 'Vue.js core team member, Vue Vapor co-author, Vue Macros creator. Active maintainer across Vue, Vite, VueUse, Rolldown and many ecosystem projects.',
    skills: ['Vue.js', 'Vue Vapor', 'TypeScript', 'Rolldown'],
    achievements: ['Vue.js core team member', 'Vue Vapor co-author', 'Vue Macros creator'],
    github: 'sxzz'
  },
  {
    id: 6,
    name: 'Johnson Chu', 
    role: 'Vue Language Tools Creator',
    avatar: '/avatars/JohnsonChu.png',
    bio: 'Creator of Vue Language Tools and Volar.js, revolutionizing Vue development experience. TypeScript expert focused on Vue tooling ecosystem.',
    skills: ['Vue.js', 'TypeScript', 'Language Server', 'Volar'],
    achievements: ['Vue Language Tools creator', 'Volar.js creator', 'TSSlint author'],
    github: 'johnsoncodehk'
  },
  {
    id: 7,
    name: '山吹色御守',
    role: 'Vue Language Tools',
    avatar: '/avatars/山吹色御守.png',
    bio: 'Front-end enthusiast specializing in Vue development tools. Core contributor to Vue Language Tools and Vue ecosystem projects.',
    skills: ['Vue.js', 'Nuxt.js', 'TypeScript', 'Developer Tools'],
    achievements: ['Vue.js org member', 'Vue Language Tools contributor', 'Vue-Vine team member'],
    github: 'KazariEX'
  },
  {
    id: 8,
    name: 'Shen Qingchuang',
    role: 'Vue-Vine Author & Docs Maintainer',
    avatar: '/avatars/shenqingchuang.png',
    bio: 'Author of Vue-Vine, passionate front-end developer. Maintains Vite & Rollup Chinese documentation, active in Vue ecosystem.',
    skills: ['Vue.js', 'Vite', 'Vue-Vine', 'Documentation'],
    achievements: ['Vue-Vine creator', 'Vite docs maintainer', 'Vue translations contributor'],
    github: 'ShenQingchuan'
  },
  {
    id: 9,
    name: 'Doctor Wu',
    role: 'Vue & VueUse Core Team',
    avatar: '/avatars/DoctorWu.png',
    bio: 'Vue.js & VueUse core team member, open source enthusiast. Former Tencent developer with expertise in Vue Vapor mode.',
    skills: ['Vue.js', 'VueUse', 'Vue Vapor', 'Open Source'],
    achievements: ['Vue.js core team member', 'VueUse core team member', 'Vue Vapor contributor'],
    github: 'Doctor-wu'
  },
  {
    id: 10,
    name: 'zhiyuanzmj',
    role: 'Vue Tooling Specialist',
    avatar: '/avatars/zhiyuanzmj.png',
    bio: 'Vue ecosystem contributor specializing in developer tooling, macros, and language services. Core contributor to Vue.js and related projects.',
    skills: ['TypeScript', 'Vue.js', 'Developer Tools', 'Macros'],
    achievements: ['Vue.js contributor', 'Vue Macros maintainer', 'TS Macro creator'],
    github: 'zhiyuanzmj'
  }
])
const teamMembersRef = ref<HTMLElement>()
const selectedMember = ref<any>(null)
const isExpanded = ref(false)
let startY = 0
let startTime = 0
const touchListenersMap = new WeakMap()
const handleMemberClick = (member: any) => {
  if (isExpanded.value && selectedMember.value?.id === member.id) {
    closeDetail()
    return
  }
  const isMobile = window.innerWidth <= 768
  if (isMobile) {
    expandMemberDetailMobile(member)
  } else {
    expandMemberDetail(member)
  }
}
const expandMemberDetailMobile = (member: any) => {
  selectedMember.value = member
  isExpanded.value = true
  nextTick(() => {
    const detailPanel = document.querySelector('.member-detail')
    if (detailPanel) {
      gsap.fromTo(detailPanel, 
        { opacity: 0, scale: 0.9 },
        { opacity: 1, scale: 1, duration: 0.3, ease: "power2.out" }
      )
      addMobileTouchListeners(detailPanel as HTMLElement)
    }
  })
}
const addMobileTouchListeners = (element: HTMLElement) => {
  const handleTouchStart = (e: TouchEvent) => {
    startY = e.touches[0].clientY
    startTime = Date.now()
  }
  const handleTouchMove = (e: TouchEvent) => {
    const currentY = e.touches[0].clientY
    const deltaY = currentY - startY
    if (deltaY > 0) {
      const progress = Math.min(deltaY / 200, 1)
      const opacity = 1 - progress * 0.5
      const scale = 1 - progress * 0.1
      gsap.set(element, {
        opacity: opacity,
        scale: scale,
        y: deltaY * 0.5
      })
    }
  }
  const handleTouchEnd = (e: TouchEvent) => {
    const endY = e.changedTouches[0].clientY
    const deltaY = endY - startY
    const deltaTime = Date.now() - startTime
    const velocity = deltaY / deltaTime
    if (deltaY > 100 || (deltaY > 50 && velocity > 0.5)) {
      closeDetail()
    } else {
      gsap.to(element, {
        opacity: 1,
        scale: 1,
        y: 0,
        duration: 0.3,
        ease: "power2.out"
      })
    }
  }
  element.addEventListener('touchstart', handleTouchStart, { passive: true })
  element.addEventListener('touchmove', handleTouchMove, { passive: false })
  element.addEventListener('touchend', handleTouchEnd, { passive: true })
  touchListenersMap.set(element, {
    touchstart: handleTouchStart,
    touchmove: handleTouchMove,
    touchend: handleTouchEnd
  })
}
const removeMobileTouchListeners = (element: HTMLElement) => {
  const listeners = touchListenersMap.get(element)
  if (listeners) {
    element.removeEventListener('touchstart', listeners.touchstart)
    element.removeEventListener('touchmove', listeners.touchmove)
    element.removeEventListener('touchend', listeners.touchend)
    touchListenersMap.delete(element)
  }
}
const expandMemberDetail = (member: any) => {
  if (isExpanded.value && selectedMember.value && selectedMember.value.id !== member.id) {
    const currentCard = document.querySelector(`.member-${selectedMember.value?.id}`)
    const tempSpacer = document.querySelector('.temp-spacer')
    gsap.set('.member-detail', { opacity: 0 })
    if (currentCard) {
      gsap.set(currentCard, { width: 500, height: 600, x: 0, y: 0, '--bg-opacity': 1, transition: 'all 0.4s ease' })
    }
    gsap.set('.member-card', { opacity: 1, scale: 1, transition: 'all 0.4s ease' })
    gsap.set('.team-members', { overflowX: 'auto' })
    if (tempSpacer) {
      tempSpacer.remove()
    }
    selectedMember.value = null
    isExpanded.value = false
    nextTick(() => {
      expandMemberDetail(member)
    })
    return
  }
  selectedMember.value = member
  isExpanded.value = true
  const allCards = document.querySelectorAll('.member-card')
  const selectedCard = document.querySelector(`.member-${member.id}`)
  const otherCards = Array.from(allCards).filter(card => !card.classList.contains(`member-${member.id}`))
  const teamMembersContainer = document.querySelector('.team-members')
  if (!selectedCard || !teamMembersContainer) {
    console.error('Selected card or container not found')
    return
  }
  const memberIndex = teamMembers.value.findIndex(m => m.id === member.id)
  const totalMembers = teamMembers.value.length
  const isLastTwo = memberIndex >= totalMembers - 2
  const isLastOne = memberIndex === totalMembers - 1
  gsap.set(selectedCard, { x: 0, y: 0, scale: 1, rotation: 0 })
  const cardRect = selectedCard.getBoundingClientRect()
  const containerRect = teamMembersContainer.getBoundingClientRect()
  const cardLeftInContainer = cardRect.left - containerRect.left + teamMembersContainer.scrollLeft
  const cardWidth = cardRect.width
  const containerWidth = containerRect.width
  const existingSpacer = document.querySelector('.temp-spacer')
  if (existingSpacer) {
    existingSpacer.remove()
  }
  let targetScrollLeft
  if (isLastTwo) {
    const currentMaxScroll = teamMembersContainer.scrollWidth - containerWidth
    const requiredScrollSpace = isLastOne ? 800 : 400
    const shouldAddSpace = isLastOne || teamMembersContainer.scrollLeft >= currentMaxScroll - 100
    if (shouldAddSpace) {
      const spacer = document.createElement('div')
      spacer.className = 'temp-spacer'
      spacer.style.width = requiredScrollSpace + 'px'
      spacer.style.height = '1px'
      spacer.style.flexShrink = '0'
      teamMembersContainer.appendChild(spacer)
    }
  }
  const extraOffset = containerWidth * 0.15
  targetScrollLeft = cardLeftInContainer + (cardWidth / 2) - (containerWidth / 2) + extraOffset
  const tl = gsap.timeline()
  tl.set(selectedCard, { transition: 'none' })
  tl.set('.scroll-hint', { display: 'none' })
  const finalScrollLeft = Math.max(0, Math.min(targetScrollLeft, teamMembersContainer.scrollWidth - containerWidth))
  tl.to(teamMembersContainer, {
    scrollLeft: finalScrollLeft,
    duration: 0.5,
    ease: "power2.out"
  }, 0)
  const centerX = window.innerWidth / 2
  const centerY = window.innerHeight / 2
  const cardCenterX = cardRect.left + cardRect.width / 2
  const cardCenterY = cardRect.top + cardRect.height / 2
  let finalOffsetX = centerX - cardCenterX
  let finalOffsetY = centerY - cardCenterY
  if (memberIndex === 0) {
    finalOffsetX = 400
  }
  tl.set('.team-members', { overflowX: 'hidden' }, 0.3)
  tl.to(otherCards, {
    opacity: 0,
    scale: 0.8,
    duration: 0.5,
    ease: "power2.out"
  }, 0.2)
  tl.to(selectedCard, {
    width: window.innerWidth * 0.75,
    height: window.innerHeight * 0.8,
    x: finalOffsetX,
    y: finalOffsetY,
    duration: 0.8,
    ease: "power2.out"
  }, 0.3)
  tl.to(selectedCard, { 
    '--bg-opacity': 0.3,
    duration: 0.4,
    ease: "power2.out"
  }, 0.7)
  tl.call(() => {
    nextTick(() => {
      const detailPanel = document.querySelector('.member-detail')
      if (detailPanel) {
        gsap.fromTo(detailPanel, 
          { opacity: 0, x: 50 },
          { opacity: 1, x: 0, duration: 0.5, ease: "power2.out", 
            onComplete: () => {
              gsap.set(selectedCard, { transition: 'all 0.4s ease' })
            }
          }
        )
      } else {
        console.warn('Detail panel not found')
      }
    })
  }, [], 1.0)
}
const closeDetail = () => {
  const isMobile = window.innerWidth <= 768
  if (isMobile) {
    closeDetailMobile()
  } else {
    closeDetailDesktop()
  }
}
const closeDetailMobile = () => {
  const detailPanel = document.querySelector('.member-detail') as HTMLElement
  if (detailPanel) {
    removeMobileTouchListeners(detailPanel)
    gsap.to(detailPanel, {
      opacity: 0,
      scale: 0.9,
      y: 50,
      duration: 0.25,
      ease: "power2.in",
      onComplete: () => {
        selectedMember.value = null
        isExpanded.value = false
      }
    })
  } else {
    selectedMember.value = null
    isExpanded.value = false
  }
}
const closeDetailDesktop = () => {
  const selectedCardElement = document.querySelector(`.member-${selectedMember.value?.id}`)
  const otherCards = document.querySelectorAll('.member-card:not(.member-' + selectedMember.value?.id + ')')
  const tl = gsap.timeline({
    onComplete: () => {
      selectedMember.value = null
      isExpanded.value = false
      if (selectedCardElement) {
        gsap.set(selectedCardElement, { transition: 'all 0.4s ease' })
      }
      const tempSpacer = document.querySelector('.temp-spacer')
      if (tempSpacer) {
        tempSpacer.remove()
      }
    }
  })
  if (selectedCardElement) {
    tl.set(selectedCardElement, { transition: 'none' })
  }
  tl.to('.member-detail', { opacity: 0, x: 50, duration: 0.3, ease: "power2.in" }, 0)
  if (selectedCardElement) {
    tl.to(selectedCardElement, { 
      '--bg-opacity': 1,
      duration: 0.3,
      ease: "power2.out"
    }, 0)
  }
  tl.to(selectedCardElement, {
    width: 500,
    height: 600,
    x: 0,
    y: 0,
    duration: 0.6,
    ease: "power2.out"
  }, 0.1)
  tl.to(otherCards, {
    opacity: 1,
    scale: 1,
    duration: 0.5,
    ease: "power2.out"
  }, 0.2)
  tl.set('.team-members', { overflowX: 'auto' }, 0.7)
  tl.set('.scroll-hint', { display: 'block', opacity: 1 }, 0.7)
}
onMounted(() => {
  setTimeout(() => {
    if (teamMembersRef.value) {
      const cards = teamMembersRef.value.querySelectorAll('.member-card')
      cards.forEach((card, index) => {
        setTimeout(() => {
          card.classList.add('animate-in')
        }, index * 200)
      })
    }
  }, 1000)
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
.us-container {
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
.us-title-container {
  position: absolute;
  top: 60px;
  left: 60px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 15px;
  z-index: 20;
}
.us-title {
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
  animation: usFadeIn 1s ease-out 1s forwards;
}
.us-underline {
  width: 80px;
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
  animation: underlineExpand 1.2s ease-out 1.5s forwards;
  box-shadow: 
    0 0 10px rgba(44, 226, 126, 0.5),
    0 0 20px rgba(44, 226, 126, 0.3);
}
.team-members {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 60px;
  width: 100%;
  height: 100vh;
  margin-top:150px;
  padding: 0 100px 0 120px;
  overflow-x: auto;
  overflow-y: hidden;
  scroll-behavior: smooth;
  box-sizing: border-box;
  scrollbar-width: none;
  -ms-overflow-style: none;
  -webkit-overflow-scrolling: touch;
}
.team-members::-webkit-scrollbar {
  display: none;
}
.member-card {
  flex-shrink: 0;
  width: 500px;
  height: 600px;
  --bg-opacity: 1;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: 24px;
  padding: 0;
  display: flex;
  flex-direction: row;
  align-items: stretch;
  justify-content: flex-end;
  transition: all 0.4s ease;
  position: relative;
  overflow: hidden;
  opacity: 0;
  transform: translateX(-80px);
  cursor: pointer;
  box-shadow: 
    0 12px 48px rgba(0, 0, 0, 0.4),
    inset 0 0 0 1px rgba(255, 255, 255, 0.1);
}
.member-card::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, calc(1 - var(--bg-opacity)));
  pointer-events: none;
  z-index: 1;
  transition: background 0.4s ease;
}
.member-card:not(.expanded) {
  flex-direction: column;
  justify-content: flex-end;
}
.member-card.expanded {
  flex-direction: row;
  justify-content: space-between;
}
.member-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0) 0%,
    rgba(0, 0, 0, 0) 50%,
    rgba(0, 0, 0, 0.3) 75%,
    rgba(0, 0, 0, 0.6) 100%
  );
  z-index: 1;
  pointer-events: none;
}
.member-card:hover {
  transform: translateY(-12px) scale(1.02);
  box-shadow: 
    0 24px 64px rgba(0, 0, 0, 0.5),
    0 0 0 2px rgba(44, 226, 126, 0.4),
    inset 0 0 0 1px rgba(255, 255, 255, 0.2);
  border-color: rgba(44, 226, 126, 0.3);
}
@media (max-width: 768px) {
  .member-card:active {
    transform: scale(0.98);
    transition: transform 0.1s ease;
  }
  .member-card:hover {
    transform: none;
    box-shadow: 
      0 12px 48px rgba(0, 0, 0, 0.4),
      inset 0 0 0 1px rgba(255, 255, 255, 0.1);
    border-color: rgba(255, 255, 255, 0.1);
  }
  .member-card:hover .member-info {
    backdrop-filter: blur(15px);
    -webkit-backdrop-filter: blur(15px);
    background: rgba(0, 0, 0, 0.5);
  }
}
.member-card:hover .member-info {
  backdrop-filter: blur(25px);
  -webkit-backdrop-filter: blur(25px);
  background: rgba(0, 0, 0, 0.6);
}
.member-info {
  padding: 20px 0;
  background: rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(15px);
  -webkit-backdrop-filter: blur(15px);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: all 0.4s ease;
  position: relative;
  z-index: 10;
}
.member-name {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1.6rem;
  font-weight: bold;
  color: #ffffff;
  text-align: center;
  margin: 0;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
  letter-spacing: 0.02em;
  line-height: 1.2;
}
.member-role {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1rem;
  color: rgba(44, 226, 126, 0.95);
  text-align: center;
  margin: 0;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.8);
  letter-spacing: 0.01em;
  line-height: 1.3;
}
.member-detail {
  position: absolute;
  top: 0;
  right: 0;
  width: 60%;
  height: 100%;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-left: 1px solid rgba(255, 255, 255, 0.1);
  padding: 0;
  overflow: hidden;
  opacity: 0;
  z-index: 20;
}
.detail-content {
  height: 100%;
  overflow-y: auto;
  padding: 40px;
  scrollbar-width: none;
  -ms-overflow-style: none;
  -webkit-overflow-scrolling: touch;
}
.detail-content::-webkit-scrollbar {
  display: none;
}
.detail-header {
  position: relative;
  margin-bottom: 40px;
  padding-bottom: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}
.detail-header h2 {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 2.5rem;
  font-weight: bold;
  color: #ffffff;
  margin: 0 0 10px 0;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.8);
}
.detail-role {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1.2rem;
  color: rgba(44, 226, 126, 0.9);
  margin: 0;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.8);
}
.close-btn {
  position: absolute;
  top: 0;
  right: 0;
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  color: #ffffff;
  font-size: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}
.close-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(44, 226, 126, 0.5);
}
.detail-body {
  display: flex;
  flex-direction: column;
  gap: 30px;
}
.detail-section h3 {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1.3rem;
  font-weight: bold;
  color: #ffffff;
  margin: 0 0 15px 0;
  text-shadow: 0 1px 4px rgba(0, 0, 0, 0.8);
}
.detail-section p {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.6;
  margin: 0;
}
.skills-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
.skill-tag {
  background: rgba(44, 226, 126, 0.2);
  border: 1px solid rgba(44, 226, 126, 0.4);
  border-radius: 20px;
  padding: 6px 14px;
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 0.9rem;
  color: rgba(44, 226, 126, 0.9);
  font-weight: 500;
}
.achievements-list {
  list-style: none;
  padding: 0;
  margin: 0;
}
.achievements-list li {
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1rem;
  color: rgba(255, 255, 255, 0.8);
  line-height: 1.6;
  margin-bottom: 8px;
  padding-left: 20px;
  position: relative;
}
.achievements-list li::before {
  content: '→';
  position: absolute;
  left: 0;
  color: rgba(44, 226, 126, 0.7);
  font-weight: bold;
}
.github-link {
  color: rgba(44, 226, 126, 0.9);
  text-decoration: none;
  font-family: 'Inter Display', 'Georgia', serif;
  font-size: 1rem;
  font-weight: 500;
  transition: color 0.3s ease;
}
.github-link:hover {
  color: rgba(44, 226, 126, 1);
}
.swipe-indicator {
  display: none;
}
.member-card.animate-in {
  animation: slideInFromLeft 0.8s ease-out forwards;
}
@keyframes scrollHintFadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes scrollHintPulse {
  0%, 100% {
    transform: scale(1);
    box-shadow: 0 0 0 0 rgba(44, 226, 126, 0.3);
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 0 0 10px rgba(44, 226, 126, 0);
  }
}
@keyframes usFadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes slideInFromLeft {
  from {
    opacity: 0;
    transform: translateX(-80px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
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
@media (max-width: 768px) {
  .us-container {
    height: 100vh;
    overflow: hidden;
  }
  .us-title-container {
    top: 30px;
    left: 30px;
  }
  .us-title {
    font-size: 2.8rem;
  }
  .us-underline {
    width: 60px;
    height: 3px;
  }
  .team-members {
    flex-direction: row;
    height: 100vh;
    padding: 0 30px;
    gap: 30px;
    justify-content: flex-start;
    align-items: center;
    overflow-x: auto;
    overflow-y: hidden;
  }
  .member-card {
    width: 280px;
    height: 380px;
    flex-shrink: 0;
    margin-bottom: 0;
  }
  .member-info {
    padding: 20px 16px;
  }
  .member-name {
    font-size: 1.3rem;
  }
  .member-role {
    font-size: 0.85rem;
  }
  .member-detail {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    right: auto;
    border-radius: 0;
    border-left: none;
    z-index: 100;
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    background: rgba(0, 0, 0, 0.95);
  }
  .detail-content {
    padding: 25px 20px;
    padding-top: 60px; 
  }
  .detail-header {
    margin-bottom: 30px;
    padding-bottom: 15px;
    position: relative;
  }
  .swipe-indicator {
    display: block;
    width: 40px;
    height: 4px;
    background: rgba(255, 255, 255, 0.3);
    border-radius: 2px;
    margin: 0 auto 20px auto;
    position: absolute;
    top: -30px;
    left: 50%;
    transform: translateX(-50%);
  }
  .detail-header h2 {
    font-size: 2rem;
    padding-right: 50px; 
  }
  .detail-role {
    font-size: 1rem;
  }
  .close-btn {
    position: fixed !important;
    top: 20px;
    right: 20px;
    width: 44px;
    height: 44px;
    font-size: 20px;
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 2px solid rgba(255, 255, 255, 0.2);
    z-index: 101;
  }
  .detail-section h3 {
    font-size: 1.1rem;
    margin-bottom: 10px;
  }
  .detail-section p {
    font-size: 0.9rem;
  }
  .skills-list {
    gap: 8px;
  }
  .skill-tag {
    font-size: 0.8rem;
    padding: 5px 12px;
  }
  .scroll-hint {
    bottom: 30px;
    right: 30px;
  }
  .scroll-hint-text {
    font-size: 0.9rem;
    padding: 10px 16px;
  }
}
@media (max-width: 480px) {
  .us-container {
    height: 100vh;
    overflow: hidden;
  }
  .us-title-container {
    top: 15px;
    left: 20px;
  }
  .us-title {
    font-size: 1.8rem;
    letter-spacing: 0.03em;
  }
  .us-underline {
    width: 40px;
    height: 2px;
  }
  .team-members {
    display: flex;
    flex-direction: column;
    height: calc(100vh - 80px);
    margin-top: 80px;
    padding: 0 20px 20px 20px;
    gap: 15px;
    overflow-y: auto;
    overflow-x: hidden;
    align-content: start;
  }
  .member-card {
    width: 100%;
    height: 120px;
    flex-shrink: 0;
  }
  .member-info {
    padding: 12px 15px;
  }
  .member-name {
    font-size: 1.1rem;
    line-height: 1.2;
  }
  .member-role {
    font-size: 0.8rem;
  }
  .member-detail {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 0;
    z-index: 100;
  }
  .detail-content {
    padding: 15px;
    padding-top: 70px; 
  }
  .detail-header {
    margin-bottom: 20px;
    padding-bottom: 10px;
  }
  .swipe-indicator {
    display: block;
    width: 35px;
    height: 3px;
    background: rgba(255, 255, 255, 0.4);
    border-radius: 2px;
    margin: 0 auto 15px auto;
    position: absolute;
    top: -25px;
    left: 50%;
    transform: translateX(-50%);
  }
  .detail-header h2 {
    font-size: 1.6rem;
    margin-bottom: 5px;
    padding-right: 50px; 
  }
  .detail-role {
    font-size: 0.9rem;
  }
  .close-btn {
    position: fixed !important;
    top: 15px;
    right: 15px;
    width: 40px;
    height: 40px;
    font-size: 18px;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 2px solid rgba(255, 255, 255, 0.3);
    z-index: 101;
  }
  .detail-section {
    margin-bottom: 20px;
  }
  .detail-section h3 {
    font-size: 1rem;
    margin-bottom: 8px;
  }
  .detail-section p, .achievements-list li {
    font-size: 0.85rem;
    line-height: 1.5;
  }
  .achievements-list li {
    margin-bottom: 6px;
    padding-left: 15px;
  }
  .skills-list {
    gap: 6px;
  }
  .skill-tag {
    font-size: 0.75rem;
    padding: 4px 8px;
  }
  .github-link {
    font-size: 0.85rem;
  }
  .scroll-hint {
    display: none;
  }
}
@media (max-width: 360px) {
  .us-container {
    height: 100vh;
    overflow: hidden;
  }
  .us-title-container {
    top: 10px;
    left: 15px;
  }
  .us-title {
    font-size: 1.6rem;
  }
  .us-underline {
    width: 35px;
    height: 2px;
  }
  .team-members {
    display: flex;
    flex-direction: column;
    height: calc(100vh - 70px);
    margin-top: 70px;
    padding: 0 15px 15px 15px;
    gap: 12px;
    overflow-y: auto;
    overflow-x: hidden;
    align-content: start;
  }
  .member-card {
    width: 100%;
    height: 100px;
    flex-shrink: 0;
  }
  .member-info {
    padding: 10px 12px;
  }
  .member-name {
    font-size: 1rem;
    line-height: 1.1;
  }
  .member-role {
    font-size: 0.75rem;
  }
  .member-detail {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 0;
    z-index: 100;
  }
  .detail-content {
    padding: 12px;
    padding-top: 60px; 
  }
  .detail-header h2 {
    font-size: 1.4rem;
    padding-right: 45px; 
  }
  .swipe-indicator {
    display: block;
    width: 30px;
    height: 3px;
    background: rgba(255, 255, 255, 0.4);
    border-radius: 2px;
    margin: 0 auto 12px auto;
    position: absolute;
    top: -20px;
    left: 50%;
    transform: translateX(-50%);
  }
  .detail-role {
    font-size: 0.8rem;
  }
  .close-btn {
    position: fixed !important;
    top: 10px;
    right: 10px;
    width: 36px;
    height: 36px;
    font-size: 16px;
    background: rgba(255, 255, 255, 0.15);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);
    border: 2px solid rgba(255, 255, 255, 0.3);
    z-index: 101;
  }
  .detail-section h3 {
    font-size: 0.9rem;
    margin-bottom: 6px;
  }
  .detail-section p, .achievements-list li {
    font-size: 0.8rem;
  }
  .skill-tag {
    font-size: 0.7rem;
    padding: 3px 6px;
  }
}
</style> 
