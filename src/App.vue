<script setup lang="ts">
import { ref, provide, onMounted, onUnmounted } from 'vue'
import Hero from './components/Hero.vue'
import Navbar from './components/Navbar.vue'
import About from './components/About.vue'
import Interests from './components/Interests.vue'
import Skills from './components/Skills.vue'
import Projects from './components/Projects.vue'
import Contact from './components/Contact.vue'

const showInterests = ref(false)
const activeCard = ref(0)
const transitionProgress = ref(0)
const isTransitioning = ref(false)

const pages = [
  { name: 'Home', component: 'Hero', title: 'Home' },
  { name: 'About', component: 'About', title: 'About Me' },
  { name: 'Skills', component: 'Skills', title: 'My Skills' },
  { name: 'Projects', component: 'Projects', title: 'My Projects' },
  { name: 'Contact', component: 'Contact', title: 'Contact Me' }
]

const openInterests = () => {
  showInterests.value = true
}

const closeInterests = () => {
  showInterests.value = false
}

const navigateToPage = (pageName: string) => {
  const index = pages.findIndex(p => p.name.toLowerCase() === pageName.toLowerCase())
  if (index !== -1) {
    activeCard.value = index
    transitionProgress.value = 0
  }
  showInterests.value = false
}

const getCardStyle = (index: number) => {
  const current = activeCard.value
  const progress = transitionProgress.value
  
  if (index < current) {
    return {
      display: 'none'
    }
  }
  else if (index === current && progress > 0) {
    const shrink = progress
    return {
      top: '50%',
      left: '50%',
      transform: `translate(-50%, -50%) scale(${1 - shrink * 0.9})`,
      width: `${90 - shrink * 85}%`,
      height: `${100 - shrink * 95}vh`,
      borderRadius: `${shrink * 50}%`,
      opacity: 1 - shrink * 0.7,
      zIndex: 99
    }
  }
  else if (index === current) {
    return {
      top: '0',
      left: '50%',
      transform: 'translateX(-50%)',
      width: '100%',
      height: '100vh',
      borderRadius: '0',
      opacity: 1,
      zIndex: 100
    }
  }
  else if (index === current + 1 && progress > 0) {
    const grow = progress
    return {
      top: '50%',
      left: '50%',
      transform: `translate(-50%, -50%) scale(${grow * 0.9 + 0.1})`,
      width: `${5 + grow * 85}%`,
      height: `${5 + grow * 95}vh`,
      borderRadius: `${50 - grow * 30}%`,
      opacity: grow * 0.3 + 0.7,
      zIndex: 101
    }
  }
  else {
    return {
      display: 'none'
    }
  }
}

let scrollAccumulator = 0
const SCROLL_THRESHOLD = 500
let isLocked = ref(false)

const handleWheel = (e: WheelEvent) => {
  if (showInterests.value || isLocked.value) return
  
  if (activeCard.value === pages.length - 1) {
    const activePageCard = document.querySelector('.page-card.active .page-card-content')
    if (activePageCard && !isTransitioning.value) {
      const hasScroll = activePageCard.scrollHeight > activePageCard.clientHeight
      const isAtTop = activePageCard.scrollTop <= 5
      const isAtBottom = activePageCard.scrollTop + activePageCard.clientHeight >= activePageCard.scrollHeight - 5
      
      if (e.deltaY > 0 && hasScroll && !isAtBottom) {
        return
      }
      if (e.deltaY < 0 && hasScroll && !isAtTop) {
        return
      }
    }
  }
  
  e.preventDefault()
  
  scrollAccumulator += e.deltaY
  
  if (scrollAccumulator > 0 && activeCard.value < pages.length - 1) {
    isTransitioning.value = true
    const progress = Math.min(scrollAccumulator / SCROLL_THRESHOLD, 1)
    transitionProgress.value = progress
    
    if (progress >= 1) {
      activeCard.value++
      scrollAccumulator = 0
      transitionProgress.value = 0
      isTransitioning.value = false
      
      isLocked.value = true
      setTimeout(() => {
        isLocked.value = false
      }, 1000)
    }
  }
  else if (scrollAccumulator < 0 && activeCard.value > 0) {
    isTransitioning.value = true
    const progress = Math.min(Math.abs(scrollAccumulator) / SCROLL_THRESHOLD, 1)
    transitionProgress.value = progress
    
    if (progress >= 1) {
      activeCard.value--
      scrollAccumulator = 0
      transitionProgress.value = 0
      isTransitioning.value = false
      
      isLocked.value = true
      setTimeout(() => {
        isLocked.value = false
      }, 1000)
    }
  }
  else {
    scrollAccumulator = 0
    transitionProgress.value = 0
    isTransitioning.value = false
  }
}

onMounted(() => {
  window.addEventListener('wheel', handleWheel, { passive: false })
})

onUnmounted(() => {
  window.removeEventListener('wheel', handleWheel)
})

provide('closeInterests', closeInterests)
provide('showInterests', showInterests)
provide('navigateToPage', navigateToPage)
provide('activeCard', activeCard)
</script>

<template>
  <div class="app">
    <Navbar />
    
    <div class="stacked-cards-container" v-if="!showInterests">
      <div
        v-for="(page, index) in pages"
        :key="page.name"
        class="page-card"
        :class="{ 
          active: activeCard === index && transitionProgress === 0,
          transitioning: (index === activeCard && transitionProgress > 0) || (index === activeCard + 1 && transitionProgress > 0)
        }"
        :style="getCardStyle(index)"
      >
        <div class="page-card-header">
          <h2 class="page-title">{{ page.name }}</h2>
        </div>
        
        <div class="page-card-content">
          <Hero v-if="page.component === 'Hero'" />
          <About v-else-if="page.component === 'About'" @open-interests="openInterests" />
          <Skills v-else-if="page.component === 'Skills'" />
          <Projects v-else-if="page.component === 'Projects'" />
          <Contact v-else-if="page.component === 'Contact'" />
        </div>
      </div>
    </div>
    
    <transition name="page-transition" mode="out-in">
      <Interests v-if="showInterests" key="interests" @close-interests="closeInterests" />
    </transition>
  </div>
</template>

<style scoped>
.app {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
  background: #0a0e1a;
}

.stacked-cards-container {
  position: relative;
  width: 100%;
  min-height: 100vh;
  perspective: 1000px;
  overflow-y: auto;
  overflow-x: hidden;
}

.stacked-cards-container::-webkit-scrollbar {
  width: 8px;
}

.stacked-cards-container::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.02);
}

.stacked-cards-container::-webkit-scrollbar-thumb {
  background: rgba(90, 140, 140, 0.3);
  border-radius: 4px;
}

.stacked-cards-container::-webkit-scrollbar-thumb:hover {
  background: rgba(90, 140, 140, 0.5);
}

.page-card {
  position: fixed;
  background: linear-gradient(135deg, rgba(25, 30, 45, 0.98), rgba(15, 20, 35, 0.98));
  overflow: hidden;
  backdrop-filter: blur(15px);
  box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.3);
  border: none;
  transition: background 0.3s ease, border 0.3s ease;
}

.page-card.transitioning {
  background: linear-gradient(135deg, rgba(25, 30, 45, 0.98), rgba(15, 20, 35, 0.98));
  border: 1px solid rgba(90, 140, 140, 0.2);
  box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.3);
}

.page-card.active {
  background: #0a0e1a;
  box-shadow: none;
  border: none;
}

.page-card-header {
  position: sticky;
  top: 0;
  width: 100%;
  height: 60px;
  padding: 0 50px;
  background: linear-gradient(135deg, rgba(35, 40, 60, 0.98), rgba(25, 30, 50, 0.98));
  border-bottom: 1px solid rgba(50, 60, 80, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
  backdrop-filter: blur(10px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
  cursor: pointer;
}

.page-card.active .page-card-header {
  background: linear-gradient(135deg, rgba(50, 55, 75, 0.98), rgba(40, 45, 65, 0.98));
  border-bottom-color: rgba(50, 60, 80, 0.4);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.page-title {
  font-size: 22px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.95);
  letter-spacing: 0.03em;
  margin: 0;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
}

.page-card-content {
  width: 100%;
  padding: 0;
  padding-bottom: 40px;
  background: transparent;
  overflow-y: hidden;
  max-height: calc(100vh - 60px);
}

.page-card:has(.contact) .page-card-content {
  overflow-y: auto;
}

.page-card-content::-webkit-scrollbar {
  width: 8px;
}

.page-card-content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.02);
}

.page-card-content::-webkit-scrollbar-thumb {
  background: rgba(90, 140, 140, 0.3);
  border-radius: 4px;
}

.page-card-content::-webkit-scrollbar-thumb:hover {
  background: rgba(90, 140, 140, 0.5);
}

.page-transition-enter-active,
.page-transition-leave-active {
  transition: all 0.5s ease;
}

.page-transition-enter-from,
.page-transition-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

@media (max-width: 768px) {
  .page-card {
    width: 95%;
    border-radius: 16px 16px 0 0;
  }
  
  .page-card.active {
    top: calc(var(--passed-count, 0) * 50px);
  }
  
  .page-card-header {
    height: 50px;
    padding: 0 25px;
  }
  
  .page-title {
    font-size: 18px;
  }
}
</style>
