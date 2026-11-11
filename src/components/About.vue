<script setup lang="ts">
import { ref, onMounted } from 'vue'

const emit = defineEmits(['open-interests'])

const isVisible = ref(false)

const handleInterestsClick = (e: Event) => {
  e.preventDefault()
  emit('open-interests')
  setTimeout(() => {
    window.scrollTo({ top: 0, behavior: 'smooth' })
  }, 100)
}

onMounted(() => {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          isVisible.value = true
        }
      })
    },
    { threshold: 0.1 }
  )

  const section = document.querySelector('.about')
  if (section) {
    observer.observe(section)
  }
})
</script>

<template>
  <section id="about" class="about" :class="{ visible: isVisible }">
    <div class="container">
      <div class="section-header">
        <h2 class="section-number">ABOUT ME</h2>
        <h3 class="section-title">
          Building practical solutions<br />
          <span class="text-gradient">through technology.</span>
        </h3>
      </div>

      <div class="about-content">
        <div class="about-main">
          <div class="text-column">
            <p class="about-paragraph">
              I'm Merawi Ayele, a software engineer and full-stack web developer interested in building 
              applications that solve real problems and make everyday tasks easier.
            </p>
            
            <p class="about-paragraph">
              I enjoy working across frontend and backend development, building end-to-end web applications 
              with an emphasis on functionality, reliability, and user experience.
            </p>
            
            <p class="about-paragraph">
              I'm currently working with Laravel and Vue.js
            </p>
            
            <div class="free-time-link">
              <a href="#" @click="handleInterestsClick" class="interests-link">
                <span class="interests-icon">😉</span>
                <span class="interests-text">Things I enjoy in my free time</span>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M7.5 15L12.5 10L7.5 5" />
                </svg>
              </a>
              
              <a href="/resume.pdf" download="Merawi_Ayele_Resume.pdf" class="resume-btn">
                <svg class="resume-icon" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4M7 10l5 5 5-5M12 15V3"/>
                </svg>
                <span class="resume-label">Download Resume</span>
              </a>
            </div>
          </div>
          
          <div class="photo-column">
            <div class="photo-wrapper">
              <img 
                src="/images/about.jpg" 
                alt="Merawi Ayele"
                class="about-photo"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.about {
  min-height: auto;
  padding: 20px 0;
  position: relative;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1), 
              transform 1s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  align-items: center;
}

.about.visible {
  opacity: 1;
  transform: translateY(0);
}

.section-header {
  margin-bottom: 20px;
}

.section-number {
  font-size: 11px;
  color: var(--color-accent-primary);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 600;
  margin-bottom: 6px;
}

.section-title {
  font-size: 28px;
  line-height: 1.2;
  margin: 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

.about-content {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.about-paragraph {
  font-size: 15px;
  line-height: 1.6;
  color: var(--color-text-secondary);
  margin: 0;
}

.about-main {
  display: grid;
  grid-template-columns: 1.8fr 1fr;
  gap: 30px;
  margin-bottom: 0;
  align-items: start;
}

.text-column {
  display: flex;
  flex-direction: column;
  gap: 14px;
  height: 100%;
}

.free-time-link {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  gap: 16px;
  margin-top: auto;
  padding-top: 8px;
}

.resume-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border);
  border-radius: 50px;
  text-decoration: none;
  transition: all 0.3s ease;
  white-space: nowrap;
  flex-shrink: 0;
}

.resume-btn:hover {
  border-color: var(--color-accent-primary);
  background: var(--color-bg-tertiary);
  transform: translateY(-2px);
  box-shadow: 0 6px 18px rgba(168, 85, 247, 0.25);
}

.resume-icon {
  color: var(--color-text-primary);
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  transition: all 0.3s ease;
}

.resume-btn:hover .resume-icon {
  color: var(--color-accent-primary);
  transform: translateY(2px);
}

.resume-label {
  color: var(--color-text-primary);
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.01em;
  transition: color 0.3s ease;
  flex-shrink: 0;
}

.resume-btn:hover .resume-label {
  color: var(--color-accent-primary);
}

.photo-column {
  position: relative;
  margin-top: -10px;
}

.photo-wrapper {
  width: 100%;
  height: auto;
  aspect-ratio: 1 / 1;
  border-radius: var(--radius-lg);
  overflow: hidden;
  border: 1px solid var(--color-border);
  background: var(--color-bg-tertiary);
}

.about-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.photo-wrapper:hover .about-photo {
  transform: scale(1.05);
}

.interests-link {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
  padding: var(--spacing-md) var(--spacing-xl);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.2) 0%, rgba(139, 92, 246, 0.1) 100%);
  border: 2px solid var(--color-accent-primary);
  border-radius: var(--radius-full);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 600;
  text-decoration: none;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.3);
  cursor: pointer;
  flex-shrink: 0;
}

.interests-link:hover {
  border-color: var(--color-accent-primary);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.3) 0%, rgba(139, 92, 246, 0.2) 100%);
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(168, 85, 247, 0.5);
}

.interests-link:active {
  transform: translateY(-1px);
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
}

.interests-icon {
  font-size: var(--text-2xl);
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.interests-link:hover {
  border-color: var(--color-accent-primary);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.3) 0%, rgba(139, 92, 246, 0.2) 100%);
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(168, 85, 247, 0.5);
}

.interests-link:active {
  transform: translateY(-1px);
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
}

.interests-icon {
  font-size: var(--text-2xl);
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
}

.interests-text {
  transition: color var(--transition-base);
}

.interests-link:hover .interests-text {
  color: var(--color-accent-primary);
}

.interests-link svg {
  transition: transform var(--transition-base);
}

.interests-link:hover svg {
  transform: translateX(5px);
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .about-main {
    grid-template-columns: 1fr;
    gap: var(--spacing-xl);
  }

  .free-time-link {
    flex-direction: column;
    align-items: stretch;
  }
  
  .photo-column {
    order: -1;
    position: static;
  }
  
  .photo-wrapper {
    height: 300px;
  }
}

@media (max-width: 768px) {
  .about {
    min-height: auto;
  }

  .section-title {
    font-size: var(--text-2xl);
  }

  .photo-wrapper {
    height: 250px;
  }

  .interests-link {
    padding: var(--spacing-sm) var(--spacing-lg);
    font-size: var(--text-sm);
  }
}
</style>
