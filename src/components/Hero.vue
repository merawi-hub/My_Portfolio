<script setup lang="ts">
import { ref, onMounted, inject, watch, type Ref } from 'vue'

// Inject showInterests state to know when coming back from interests
const showInterests = inject<Ref<boolean>>('showInterests')

const isVisible = ref(false)
const showCartoon = ref(true)
const showOriginal = ref(false)
const hasAnimatedBefore = ref(false)

const scrollToProjects = () => {
  const projectsSection = document.getElementById('projects')
  if (projectsSection) {
    const navbarHeight = 80
    const elementPosition = projectsSection.getBoundingClientRect().top + window.pageYOffset
    const offsetPosition = elementPosition - navbarHeight
    
    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
}

const downloadCV = () => {
  // Option 1: Direct download if you have a CV file in public/cv/ folder
  const link = document.createElement('a')
  link.href = '/cv/Merawi_Ayele_CV.pdf' // File should be in client/public/cv/
  link.download = 'Merawi_Ayele_CV.pdf'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
  
  // Option 2: If you don't have CV yet - scroll to contact section
  // Uncomment these lines and comment out the above if you don't have a CV:
  // const contactSection = document.getElementById('contact')
  // if (contactSection) {
  //   const navbarHeight = 80
  //   const elementPosition = contactSection.getBoundingClientRect().top + window.pageYOffset
  //   const offsetPosition = elementPosition - navbarHeight
  //   window.scrollTo({ top: offsetPosition, behavior: 'smooth' })
  // }
}

const toggleLanguage = () => {
  showOriginal.value = !showOriginal.value
}

const onVideoEnded = () => {
  // Flip to real photo when video ends
  showCartoon.value = false
  hasAnimatedBefore.value = true
  // Store in sessionStorage so it persists
  sessionStorage.setItem('heroAnimated', 'true')
}

onMounted(() => {
  // Check if animation has already played
  const alreadyAnimated = sessionStorage.getItem('heroAnimated') === 'true'
  
  if (alreadyAnimated) {
    // Skip animation - show real photo immediately
    hasAnimatedBefore.value = true
    showCartoon.value = false
    isVisible.value = true
  } else {
    // Play animation on first visit
    setTimeout(() => {
      isVisible.value = true
    }, 100)
    
    // Backup: flip to real photo after 10 seconds even if video doesn't end
    setTimeout(() => {
      showCartoon.value = false
      hasAnimatedBefore.value = true
      sessionStorage.setItem('heroAnimated', 'true')
    }, 10000)
  }
})

// Watch for returning from interests
watch(showInterests || ref(false), (newVal) => {
  if (!newVal) {
    // Coming back from interests - show stable photo
    showCartoon.value = false
    hasAnimatedBefore.value = true
  }
})
</script>

<template>
  <section id="home" class="hero" :class="{ visible: isVisible }">
    <div class="container">
      <div class="hero-content">
        <!-- LEFT SIDE: Text Content -->
        <div class="hero-text">
          <div class="greeting" :class="{ 'glow-text glow-1': !hasAnimatedBefore }">
            <span class="wave">👋</span>
            <span>Hello, I'm</span>
          </div>
          <h1 class="name" :class="{ 'glow-text glow-2': !hasAnimatedBefore }">
            <span class="text-gradient">Merawi Ayele</span>
          </h1>
          <div class="title-wrapper" :class="{ 'glow-text glow-3': !hasAnimatedBefore }">
            <h2 class="title">Software Engineer</h2>
            <span class="separator">|</span>
            <h2 class="title">Full Stack Web Developer</h2>
          </div>
          <p class="description" :class="{ 'animated-description': !hasAnimatedBefore }">
            I'm a software engineer passionate about building useful web applications that solve real-world problems. 
            I enjoy turning ideas into practical solutions, learning new technologies, and creating digital experiences 
            that are simple, reliable, and easy to use.
          </p>
          
          <div class="hero-actions" :class="{ 'animated-actions': !hasAnimatedBefore }">
            <button class="btn btn-primary" @click="scrollToProjects">
              <span>View My Work</span>
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M7.5 15L12.5 10L7.5 5" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
            </button>
            <button class="btn btn-secondary" @click="downloadCV">
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M17.5 12.5V15.8333C17.5 16.2754 17.3244 16.6993 17.0118 17.0118C16.6993 17.3244 16.2754 17.5 15.8333 17.5H4.16667C3.72464 17.5 3.30072 17.3244 2.98816 17.0118C2.67559 16.6993 2.5 16.2754 2.5 15.8333V12.5" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M5.83337 8.33333L10 12.5L14.1667 8.33333" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                <path d="M10 12.5V2.5" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
              </svg>
              <span>Download CV</span>
            </button>
          </div>
        </div>

        <!-- RIGHT SIDE: Photo -->
        <div class="hero-photo-wrapper">
          <div class="photo-border">
            <div class="photo-flip-container" :class="{ flipped: !showCartoon }">
              <div class="photo-flipper">
                <!-- Front: Cartoon with Wave -->
                <div class="photo-front">
                  <div class="photo-container">
                    <video 
                      autoplay
                      muted
                      playsinline
                      class="profile-photo cartoon-video"
                      @ended="onVideoEnded"
                    >
                      <source src="/images/cartoon-wave.mp4" type="video/mp4">
                      <!-- Fallback to image if video doesn't load -->
                      <img 
                        src="/images/cartoon.jpg" 
                        alt="Merawi Ayele Cartoon"
                        class="profile-photo"
                      />
                    </video>
                  </div>
                </div>
                <!-- Back: Real Photo -->
                <div class="photo-back">
                  <div class="photo-container">
                    <img 
                      src="/images/profile.jpg" 
                      alt="Merawi Ayele"
                      class="profile-photo"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div class="photo-glow"></div>
          
          <!-- Local Language Badge -->
          <div class="language-badge">
            <transition name="fade" mode="out-in">
              <div v-if="!showOriginal" key="amharic" class="language-text">
                <span class="local-text">ሶፍትዌር ኢንጂነር  ነኝ🙌</span>
              </div>
              <div v-else key="english" class="language-text">
                <span class="translated-text">I'm a Software Engineer 😎✌️</span>
              </div>
            </transition>
            <button @click="toggleLanguage" class="translate-btn">
              translate
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="hero-bg-decoration decoration-1"></div>
    <div class="hero-bg-decoration decoration-2"></div>
  </section>
</template>

<style scoped>
.hero {
  min-height: 100vh;
  position: relative;
  padding-top: 70px;
  padding-bottom: 0;
  overflow: hidden;
  display: flex;
  align-items: flex-start;
}

.hero-content {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: flex-start;
  gap: var(--spacing-md);
  width: 100%;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1),
              transform 1s cubic-bezier(0.4, 0, 0.2, 1);
  padding-top: var(--spacing-md);
}

.hero.visible .hero-content {
  opacity: 1;
  transform: translateY(0);
}

.hero-text {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
  flex: 1;
  max-width: 600px;
}

.greeting {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  font-size: var(--text-lg);
  color: var(--color-text-secondary);
}

.wave {
  display: inline-block;
  animation: wave 2.5s ease-in-out infinite;
  transform-origin: 70% 70%;
}

@keyframes wave {
  0%, 100% { transform: rotate(0deg); }
  10%, 30% { transform: rotate(14deg); }
  20% { transform: rotate(-8deg); }
  40%, 100% { transform: rotate(0deg); }
}

.name {
  font-size: var(--text-6xl);
  line-height: 1.1;
  margin: 0;
}

.title-wrapper {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: var(--spacing-sm);
}

.title {
  font-size: var(--text-2xl);
  font-weight: 600;
  color: var(--color-text-primary);
  margin: 0;
}

.separator {
  color: var(--color-accent-primary);
  font-size: var(--text-xl);
}

.description {
  font-size: var(--text-lg);
  line-height: 1.7;
}

.description.animated-description {
  opacity: 0;
  animation: fadeInUp 0.6s ease-out 8.5s both;
}

.hero-actions {
  display: flex;
  gap: var(--spacing-md);
  margin-top: var(--spacing-sm);
  flex-wrap: wrap;
}

.hero-actions.animated-actions {
  opacity: 0;
  animation: fadeInUp 0.6s ease-out 9s both;
}

.hero-photo-wrapper {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0;
  flex-shrink: 0;
}

/* Language Badge */
.language-badge {
  margin-top: 0;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: var(--spacing-sm);
  width: 100%;
  max-width: 300px;
}

.language-text {
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--spacing-xs) var(--spacing-sm);
  display: flex;
  align-items: center;
  justify-content: center;
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
}

.local-text {
  font-size: var(--text-base);
  font-weight: 600;
  color: var(--color-accent-primary);
  letter-spacing: 0.03em;
  white-space: nowrap;
}

.translated-text {
  font-size: var(--text-base);
  font-weight: 500;
  color: var(--color-text-primary);
  white-space: nowrap;
}

.translate-btn {
  background: transparent;
  color: var(--color-text-muted);
  border: 1px solid var(--color-border);
  border-radius: 4px;
  padding: 4px 8px;
  font-size: 10px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  font-weight: 500;
  white-space: nowrap;
  flex-shrink: 0;
}

.translate-btn:hover {
  color: var(--color-accent-primary);
  border-color: var(--color-accent-primary);
  background: rgba(168, 85, 247, 0.05);
  transform: translateY(-1px);
}

/* Fade Transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.photo-border {
  position: relative;
  padding: 8px;
  background: var(--color-bg-tertiary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
}

@keyframes borderPulse {
  0%, 100% {
    box-shadow: 0 0 30px rgba(0, 0, 0, 0.3);
  }
  50% {
    box-shadow: 0 0 60px rgba(0, 0, 0, 0.5);
  }
}

/* Flip Container */
.photo-flip-container {
  perspective: 1000px;
  width: 300px;
  height: 300px;
}

.photo-flipper {
  position: relative;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
  transition: transform 1s ease-in-out;
  transform: rotateY(0deg);
}

.photo-flip-container.flipped .photo-flipper {
  transform: rotateY(180deg);
}

.photo-front,
.photo-back {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.photo-back {
  transform: rotateY(180deg);
}

.photo-container {
  width: 300px;
  height: 300px;
  border-radius: var(--radius-xl);
  overflow: hidden;
  background: var(--color-bg-tertiary);
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.profile-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.cartoon-video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.photo-container:hover .profile-photo {
  transform: scale(1.05);
}

/* Cartoon Wave Animation - Hand waving motion */
.cartoon-wave {
  animation: cartoonHandWave 10s ease-in-out 1;
  transform-origin: center center;
}

@keyframes cartoonHandWave {
  0%, 100% {
    transform: scale(1);
  }
  5%, 15%, 25%, 35%, 45%, 55%, 65% {
    transform: scale(1.03);
  }
  10%, 20%, 30%, 40%, 50%, 60%, 70% {
    transform: scale(1);
  }
  75% {
    transform: scale(1);
  }
}

/* Glowing Text Animation */
.glow-text {
  opacity: 0;
  visibility: hidden;
  animation-fill-mode: forwards;
}

.glow-1 {
  animation: glowIn 2s ease-out 0.5s forwards;
}

.glow-2 {
  animation: glowIn 2s ease-out 3s forwards;
}

.glow-3 {
  animation: glowIn 2s ease-out 6s forwards;
}

@keyframes glowIn {
  0% {
    opacity: 0;
    visibility: visible;
    text-shadow: 0 0 0px transparent;
    transform: translateY(20px) scale(0.9);
    filter: brightness(0);
  }
  30% {
    opacity: 0.7;
    text-shadow: 
      0 0 20px var(--color-accent-primary),
      0 0 40px var(--color-accent-primary),
      0 0 60px var(--color-accent-glow),
      0 0 80px var(--color-accent-glow);
    filter: brightness(1.8);
  }
  60% {
    opacity: 1;
    text-shadow: 
      0 0 30px var(--color-accent-primary),
      0 0 50px var(--color-accent-primary),
      0 0 70px var(--color-accent-glow);
    filter: brightness(1.5);
  }
  100% {
    opacity: 1;
    visibility: visible;
    text-shadow: 0 0 0px transparent;
    transform: translateY(0) scale(1);
    filter: brightness(1);
  }
}

.photo-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 350px;
  height: 350px;
  background: radial-gradient(circle, var(--color-accent-glow) 0%, transparent 70%);
  pointer-events: none;
  z-index: -1;
  animation: glowPulse 4s ease-in-out infinite;
}

@keyframes glowPulse {
  0%, 100% {
    opacity: 0.5;
  }
  50% {
    opacity: 0.8;
  }
}

.hero-bg-decoration {
  position: absolute;
  border-radius: 50%;
  opacity: 0.05;
  pointer-events: none;
  z-index: 0;
}

.decoration-1 {
  width: 500px;
  height: 500px;
  background: var(--color-accent-primary);
  top: -10%;
  right: -10%;
  filter: blur(100px);
}

.decoration-2 {
  width: 400px;
  height: 400px;
  background: var(--color-accent-secondary);
  bottom: -10%;
  left: -10%;
  filter: blur(100px);
}

@media (max-width: 768px) {
  .hero-content {
    flex-direction: column;
    align-items: center;
  }

  .hero-photo-wrapper {
    order: -1;
  }

  .photo-container {
    width: 250px;
    height: 250px;
  }
  
  .name {
    font-size: var(--text-4xl);
  }

  .title {
    font-size: var(--text-xl);
  }

  .title-wrapper {
    flex-direction: column;
    gap: var(--spacing-xs);
    align-items: flex-start;
  }

  .separator {
    display: none;
  }

  .hero-actions {
    flex-direction: column;
    width: 100%;
  }
}
</style>
