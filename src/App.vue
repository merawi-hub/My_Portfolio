<script setup lang="ts">
import { ref, provide } from 'vue'
import Hero from './components/Hero.vue'
import Navbar from './components/Navbar.vue'
import About from './components/About.vue'
import Interests from './components/Interests.vue'
import Skills from './components/Skills.vue'
import Projects from './components/Projects.vue'
import Experience from './components/Experience.vue'
import Contact from './components/Contact.vue'

const showInterests = ref(false)

const openInterests = () => {
  showInterests.value = true
}

const closeInterests = () => {
  showInterests.value = false
}

// Provide the close function so Navbar can access it
provide('closeInterests', closeInterests)
provide('showInterests', showInterests)
</script>

<template>
  <div class="app">
    <Navbar />
    <transition name="page-transition" mode="out-in">
      <Hero v-if="!showInterests" key="hero" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <About v-if="!showInterests" key="about" @open-interests="openInterests" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <Skills v-if="!showInterests" key="skills" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <Projects v-if="!showInterests" key="projects" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <Experience v-if="!showInterests" key="experience" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <Contact v-if="!showInterests" key="contact" />
    </transition>
    <transition name="page-transition" mode="out-in">
      <Interests v-if="showInterests" key="interests" @close-interests="closeInterests" />
    </transition>
  </div>
</template>

<style scoped>
.app {
  position: relative;
  z-index: 1;
}

/* Page Transition */
.page-transition-enter-active,
.page-transition-leave-active {
  transition: opacity 0.5s cubic-bezier(0.4, 0, 0.2, 1),
              transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.page-transition-enter-from {
  opacity: 0;
  transform: translateY(20px);
}

.page-transition-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
</style>
