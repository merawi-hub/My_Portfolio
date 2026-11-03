<script setup lang="ts">
import { ref, onMounted } from 'vue'

const emit = defineEmits(['open-interests'])

const isVisible = ref(false)

const handleInterestsClick = (e: Event) => {
  e.preventDefault()
  emit('open-interests')
  // Scroll to the top of the page to show the interests header
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
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-number">ABOUT ME</h2>
        <h3 class="section-title">
          Building practical solutions<br />
          <span class="text-gradient">through technology.</span>
        </h3>
      </div>

      <!-- Main Content -->
      <div class="about-content">
        <!-- Paragraph 1 - Full Width Above Photo -->
        <p class="full-width-paragraph">
          I'm Merawi Ayele, a software engineer and full-stack web developer interested in building 
          applications that solve real problems and make everyday tasks easier.
        </p>
        
        <!-- Paragraph 2 First Line - Full Width -->
        <p class="full-width-paragraph">
          I enjoy working across frontend and backend development, building end-to-end web applications 
          with an emphasis on functionality, reliability, and user experience.
        </p>
        
        <!-- Two Column Layout: Text Left + Photo Right -->
        <div class="about-main">
          <!-- Left Column: Remaining Text -->
          <div class="text-column">
            <p class="about-paragraph">
              Each project challenges me to make thoughtful technical decisions, refine my approach, and deliver 
              practical solutions to real-world problems.
            </p>
            
            <p class="about-paragraph">
              Beyond development, I value communication, teamwork, curiosity, consistency, and continuous learning.
            </p>
            
            <p class="about-paragraph">
              My goal is to create reliable, user-friendly applications that provide meaningful value to people 
              and businesses.
            </p>
            
            <!-- FREE TIME LINK - Below paragraphs, aligned with photo bottom -->
            <div id="interests-button" class="free-time-link">
              <a href="#" @click="handleInterestsClick" class="interests-link">
                <span class="interests-icon">😉</span>
                <span class="interests-text">Things I enjoy in my free time</span>
                <svg width="20" height="20" viewBox="0 0 20 20" fill="none" stroke="currentColor" stroke-width="2">
                  <path d="M7.5 15L12.5 10L7.5 5" />
                </svg>
              </a>
            </div>
          </div>
          
          <!-- Right Column: Photo -->
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
  min-height: 100vh;
  padding: var(--spacing-xl) 0 0 0;
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

/* Section Header */
.section-header {
  margin-bottom: var(--spacing-xl);
}

.section-number {
  font-size: var(--text-xs);
  color: var(--color-accent-primary);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 600;
  margin-bottom: var(--spacing-sm);
}

.section-title {
  font-size: var(--text-4xl);
  line-height: 1.2;
  margin: 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

/* About Content Wrapper */
.about-content {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
}

.full-width-paragraph {
  font-size: var(--text-base);
  line-height: 1.7;
  color: var(--color-text-secondary);
  margin: 0;
  width: 100%;
}

/* About Main Content */
.about-main {
  display: grid;
  grid-template-columns: 1.5fr 1fr;
  gap: var(--spacing-2xl);
  margin-bottom: var(--spacing-xl);
  align-items: start;
}

.text-column {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
  height: 100%;
}

.about-paragraph {
  font-size: var(--text-base);
  line-height: 1.7;
  color: var(--color-text-secondary);
  margin: 0;
}

/* Photo Column */
.photo-column {
  position: sticky;
  top: 100px;
}

/* Simple Photo */
.photo-wrapper {
  width: 100%;
  height: 300px;
  border-radius: var(--radius-lg);
  overflow: hidden;
  border: 1px solid var(--color-border);
  background: var(--color-bg-tertiary);
}

.about-photo {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center 20%;
  transition: transform 0.3s ease;
}

.about-photo {
  transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.photo-wrapper:hover .about-photo {
  transform: scale(1.05);
}

/* Free Time Link */
.free-time-link {
  display: flex;
  justify-content: flex-start;
  margin-top: auto;
  padding-top: var(--spacing-md);
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

/* Responsive */
@media (max-width: 768px) {
  .about-main {
    grid-template-columns: 1fr;
    gap: var(--spacing-xl);
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
