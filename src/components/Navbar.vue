<script setup lang="ts">
import { ref, inject, type Ref } from 'vue'

const isSidebarOpen = ref(false)

const closeInterests = inject<() => void>('closeInterests')
const showInterests = inject<Ref<boolean>>('showInterests')
const navigateToPage = inject<(page: string) => void>('navigateToPage')
const currentPage = inject<Ref<string>>('currentPage')

const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value
}

const closeSidebar = () => {
  isSidebarOpen.value = false
}

const handleNavClick = (e: Event, page: string) => {
  e.preventDefault()
  
  if (showInterests?.value && closeInterests) {
    closeInterests()
  }
  
  if (navigateToPage) {
    navigateToPage(page)
  }
  
  closeSidebar()
}
</script>

<template>
  <nav class="navbar">
    <!-- Hamburger Menu Button -->
    <button 
      class="hamburger-btn" 
      :class="{ active: isSidebarOpen }"
      @click="toggleSidebar"
      aria-label="Toggle menu"
    >
      <span></span>
      <span></span>
      <span></span>
    </button>

    <!-- Sidebar Navigation -->
    <div class="sidebar" :class="{ open: isSidebarOpen }">
      <!-- Navigation Label -->
      <div class="nav-label">Navigation</div>

      <!-- Navigation Links -->
      <div class="nav-links">
        <a href="#home" :class="{ active: currentPage === 'home' }" @click="(e) => handleNavClick(e, 'home')">Home</a>
        <a href="#about" :class="{ active: currentPage === 'about' }" @click="(e) => handleNavClick(e, 'about')">About</a>
        <a href="#projects" :class="{ active: currentPage === 'projects' }" @click="(e) => handleNavClick(e, 'projects')">Project</a>
        <a href="#skills" :class="{ active: currentPage === 'skills' }" @click="(e) => handleNavClick(e, 'skills')">Skills</a>
        <a href="#contact" :class="{ active: currentPage === 'contact' }" @click="(e) => handleNavClick(e, 'contact')">Contact</a>
      </div>
    </div>

    <!-- Overlay -->
    <div class="overlay" :class="{ show: isSidebarOpen }" @click="closeSidebar"></div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  pointer-events: none;
}

/* Hamburger Button */
.hamburger-btn {
  position: fixed;
  top: 20px;
  right: 20px;
  width: 60px;
  height: 60px;
  background: rgba(90, 140, 140, 0.9);
  border: none;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
  z-index: 1002;
  pointer-events: all;
  transition: all 0.3s ease;
}

.hamburger-btn:hover {
  background: rgba(90, 140, 140, 1);
  transform: scale(1.05);
}

.hamburger-btn span {
  width: 24px;
  height: 2px;
  background: white;
  transition: all 0.3s ease;
  border-radius: 2px;
}

.hamburger-btn.active span:nth-child(1) {
  transform: rotate(45deg) translate(6px, 6px);
}

.hamburger-btn.active span:nth-child(2) {
  opacity: 0;
}

.hamburger-btn.active span:nth-child(3) {
  transform: rotate(-45deg) translate(6px, -6px);
}

/* Sidebar */
.sidebar {
  position: fixed;
  top: 0;
  right: -400px;
  width: 400px;
  height: 100vh;
  background: #0a0e1a;
  border-left: 1px solid rgba(255, 255, 255, 0.1);
  padding: 40px 40px;
  transition: right 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1001;
  pointer-events: all;
  overflow-y: auto;
  overflow-x: hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.sidebar.open {
  right: 0;
}

/* Close Button */
.close-btn {
  display: none;
}

/* Navigation Label */
.nav-label {
  font-size: 12px;
  color: rgba(255, 255, 255, 0.5);
  text-transform: uppercase;
  letter-spacing: 0.1em;
  margin-bottom: 30px;
  padding-bottom: 12px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

/* Navigation Links */
.nav-links {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.nav-links a {
  font-size: 32px;
  font-weight: 300;
  color: white;
  text-decoration: none;
  transition: all 0.3s ease;
  padding: 6px 0;
  font-family: 'SF Pro Display', -apple-system, BlinkMacSystemFont, sans-serif;
  position: relative;
}

.nav-links a::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 0;
  width: 0;
  height: 3px;
  background: rgba(90, 140, 140, 1);
  transition: width 0.3s ease;
}

.nav-links a:hover,
.nav-links a.active {
  color: rgba(90, 140, 140, 1);
  padding-left: 16px;
}

.nav-links a.active::after {
  width: 60px;
}

/* Overlay */
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.7);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  z-index: 1000;
  pointer-events: none;
}

.overlay.show {
  opacity: 1;
  visibility: visible;
  pointer-events: all;
}

/* Responsive */
@media (max-width: 768px) {
  .hamburger-btn {
    top: 20px;
    right: 20px;
    width: 50px;
    height: 50px;
  }

  .sidebar {
    width: 100%;
    right: -100%;
    padding: 30px;
  }

  .nav-label {
    font-size: 11px;
    margin-bottom: 20px;
  }

  .nav-links {
    gap: 16px;
  }

  .nav-links a {
    font-size: 28px;
  }
}

@media (max-height: 600px) {
  .sidebar {
    padding: 30px 40px;
  }
  
  .nav-label {
    font-size: 11px;
    margin-bottom: 20px;
    padding-bottom: 10px;
  }
  
  .nav-links {
    gap: 12px;
  }
  
  .nav-links a {
    font-size: 24px;
    padding: 4px 0;
  }
}
</style>
