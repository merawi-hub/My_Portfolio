<script setup lang="ts">
import { ref, onMounted, onUnmounted, inject, type Ref } from 'vue'

const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)
const isVisible = ref(false)
const activeSection = ref('home')

// Inject the closeInterests function and showInterests ref from App.vue
const closeInterests = inject<() => void>('closeInterests')
const showInterests = inject<Ref<boolean>>('showInterests')

const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
  
  // Detect active section
  const sections = ['home', 'about', 'skills', 'projects', 'journey', 'contact']
  const navbarHeight = 100
  
  // Check if we're in the interests section
  if (showInterests?.value) {
    activeSection.value = 'about' // Keep About highlighted when in Interests
    return
  }
  
  for (const section of sections) {
    const element = document.getElementById(section)
    if (element) {
      const rect = element.getBoundingClientRect()
      // Check if section is in viewport (accounting for navbar)
      if (rect.top <= navbarHeight && rect.bottom >= navbarHeight) {
        activeSection.value = section
        break
      }
    }
  }
}

const handleNavClick = (e: Event, section: string) => {
  e.preventDefault()
  
  // If interests section is open, close it first
  if (showInterests?.value && closeInterests) {
    closeInterests()
  }
  
  // After closing any overlay, scroll to the target section
  setTimeout(() => {
    const element = document.getElementById(section)
    if (element) {
      const navbarHeight = 80
      const elementPosition = element.getBoundingClientRect().top + window.pageYOffset
      const offsetPosition = elementPosition - navbarHeight
      
      window.scrollTo({
        top: offsetPosition,
        behavior: 'smooth'
      })
    }
  }, 150)
}

const toggleMobileMenu = () => {
  isMobileMenuOpen.value = !isMobileMenuOpen.value
}

const closeMobileMenu = () => {
  isMobileMenuOpen.value = false
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  
  // Show navbar after 10 seconds (when photo flips back)
  setTimeout(() => {
    isVisible.value = true
  }, 10000)
  
  // Initial check for active section
  handleScroll()
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <nav class="navbar" :class="{ scrolled: isScrolled, visible: isVisible }">
    <div class="container">
      <div class="navbar-content">
        <!-- Logo/Name -->
        <a href="#home" class="navbar-logo">
          <span class="logo-text">Merawi</span>
          <span class="logo-accent">Ayele</span>
        </a>

        <!-- Desktop Navigation -->
        <div class="navbar-menu">
          <a href="#home" class="nav-link" :class="{ active: activeSection === 'home' }" @click="(e) => handleNavClick(e, 'home')">Home</a>
          <a href="#about" class="nav-link" :class="{ active: activeSection === 'about' }" @click="(e) => handleNavClick(e, 'about')">About</a>
          <a href="#skills" class="nav-link" :class="{ active: activeSection === 'skills' }" @click="(e) => handleNavClick(e, 'skills')">Skills</a>
          <a href="#projects" class="nav-link" :class="{ active: activeSection === 'projects' }" @click="(e) => handleNavClick(e, 'projects')">Projects</a>
          <a href="#journey" class="nav-link" :class="{ active: activeSection === 'journey' }" @click="(e) => handleNavClick(e, 'journey')">Journey</a>
          <a href="#contact" class="nav-link" :class="{ active: activeSection === 'contact' }" @click="(e) => handleNavClick(e, 'contact')">Contact</a>
        </div>

        <!-- Mobile Menu Toggle -->
        <button 
          class="mobile-menu-toggle" 
          :class="{ active: isMobileMenuOpen }"
          @click="toggleMobileMenu"
          aria-label="Toggle menu"
        >
          <span class="hamburger-line"></span>
          <span class="hamburger-line"></span>
          <span class="hamburger-line"></span>
        </button>
      </div>

      <!-- Mobile Menu -->
      <div class="mobile-menu" :class="{ open: isMobileMenuOpen }">
        <div class="mobile-menu-links">
          <a href="#home" class="mobile-nav-link" :class="{ active: activeSection === 'home' }" @click="(e) => { handleNavClick(e, 'home'); closeMobileMenu(); }">Home</a>
          <a href="#about" class="mobile-nav-link" :class="{ active: activeSection === 'about' }" @click="(e) => { handleNavClick(e, 'about'); closeMobileMenu(); }">About</a>
          <a href="#skills" class="mobile-nav-link" :class="{ active: activeSection === 'skills' }" @click="(e) => { handleNavClick(e, 'skills'); closeMobileMenu(); }">Skills</a>
          <a href="#projects" class="mobile-nav-link" :class="{ active: activeSection === 'projects' }" @click="(e) => { handleNavClick(e, 'projects'); closeMobileMenu(); }">Projects</a>
          <a href="#journey" class="mobile-nav-link" :class="{ active: activeSection === 'journey' }" @click="(e) => { handleNavClick(e, 'journey'); closeMobileMenu(); }">Journey</a>
          <a href="#contact" class="mobile-nav-link" :class="{ active: activeSection === 'contact' }" @click="(e) => { handleNavClick(e, 'contact'); closeMobileMenu(); }">Contact</a>
        </div>
        <a href="#contact" class="mobile-cta btn btn-primary" @click="(e) => { handleNavClick(e, 'contact'); closeMobileMenu(); }">
          Contact
        </a>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: var(--z-fixed);
  background: transparent;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  padding: var(--spacing-md) 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-20px);
}

.navbar.visible {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.navbar.scrolled {
  background: rgba(10, 10, 15, 0.95);
  backdrop-filter: blur(10px);
  padding: var(--spacing-sm) 0;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.navbar-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

/* Logo */
.navbar-logo {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  font-size: var(--text-xl);
  font-weight: 700;
  text-decoration: none;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.navbar-logo:hover {
  transform: translateY(-2px);
}

.logo-text {
  color: var(--color-text-primary);
}

.logo-accent {
  color: var(--color-accent-primary);
}

/* Desktop Menu */
.navbar-menu {
  display: flex;
  align-items: center;
  gap: var(--spacing-xl);
  margin-left: auto;
}

.nav-link {
  color: var(--color-text-secondary);
  font-size: var(--text-base);
  font-weight: 500;
  text-decoration: none;
  position: relative;
  transition: color 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: var(--color-accent-primary);
  transition: width 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.nav-link:hover {
  color: var(--color-text-primary);
}

.nav-link:hover::after,
.nav-link.active::after {
  width: 100%;
}

.nav-link.active {
  color: var(--color-text-primary);
}

/* Responsive */
@media (max-width: 768px) {
  .navbar-menu {
    display: none;
  }

  .mobile-menu-toggle {
    display: flex;
  }
}

/* Mobile Menu Toggle */
.mobile-menu-toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: var(--spacing-xs);
  z-index: calc(var(--z-fixed) + 2);
}

.hamburger-line {
  width: 25px;
  height: 2px;
  background: var(--color-text-primary);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  border-radius: 2px;
}

.mobile-menu-toggle.active .hamburger-line:nth-child(1) {
  transform: rotate(45deg) translate(7px, 7px);
}

.mobile-menu-toggle.active .hamburger-line:nth-child(2) {
  opacity: 0;
}

.mobile-menu-toggle.active .hamburger-line:nth-child(3) {
  transform: rotate(-45deg) translate(7px, -7px);
}

/* Mobile Menu */
.mobile-menu {
  position: fixed;
  top: 0;
  right: -100%;
  width: 80%;
  max-width: 300px;
  height: 100vh;
  background: var(--color-bg-secondary);
  border-left: 1px solid var(--color-border);
  padding: var(--spacing-3xl) var(--spacing-lg);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xl);
  transition: right 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: calc(var(--z-fixed) + 1);
  overflow-y: auto;
}

.mobile-menu.open {
  right: 0;
}

.mobile-menu-links {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
}

.mobile-nav-link {
  color: var(--color-text-secondary);
  font-size: var(--text-lg);
  font-weight: 500;
  text-decoration: none;
  padding: var(--spacing-sm) 0;
  border-bottom: 1px solid var(--color-border);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.mobile-nav-link:hover,
.mobile-nav-link.active {
  color: var(--color-accent-primary);
  padding-left: var(--spacing-sm);
}

.mobile-cta {
  width: 100%;
  justify-content: center;
  margin-top: auto;
}

/* Responsive */
@media (max-width: 768px) {
  .navbar-menu,
  .navbar-cta {
    display: none;
  }

  .mobile-menu-toggle {
    display: flex;
  }
}

/* Smooth scroll behavior */
html {
  scroll-behavior: smooth;
  scroll-padding-top: 80px;
}
</style>
