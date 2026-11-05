<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const isVisible = ref(false)
const activeCategory = ref('')
const showPopup = ref(false)

interface TechStack {
  name: string
  icon: string
  category: string
}

// All tech stacks with categories
const allTechStacks = ref<TechStack[]>([
  // Frontend - ordered for special layout
  { name: 'HTML5', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original.svg', category: 'frontend' },
  { name: 'CSS3', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original.svg', category: 'frontend' },
  { name: 'TypeScript', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-original.svg', category: 'frontend' },
  { name: 'Vue.js', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/vuejs/vuejs-original.svg', category: 'frontend' },
  { name: 'React', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg', category: 'frontend' },
  { name: 'JavaScript', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg', category: 'frontend' },
  { name: 'Tailwind', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/tailwindcss/tailwindcss-original.svg', category: 'frontend' },
  
  // Backend
  { name: 'PHP', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/php/php-original.svg', category: 'backend' },
  { name: 'Laravel', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/laravel/laravel-original.svg', category: 'backend' },
  { name: 'Python', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg', category: 'backend' },
  { name: 'Django', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/django/django-plain.svg', category: 'backend' },
  { name: 'MySQL', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/mysql/mysql-original.svg', category: 'backend' },
  
  // Tools
  { name: 'Git', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/git/git-original.svg', category: 'tools' },
  { name: 'GitHub', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/github/github-original.svg', category: 'tools' },
  { name: 'VS Code', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/vscode/vscode-original.svg', category: 'tools' },
  { name: 'Figma', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/figma/figma-original.svg', category: 'tools' }
])

// Filtered tech stacks based on active category for popup
const displayedTechStacks = computed(() => {
  if (activeCategory.value === 'all') {
    return allTechStacks.value
  }
  return allTechStacks.value.filter(tech => tech.category === activeCategory.value)
})

const setCategory = (category: string) => {
  activeCategory.value = category
  showPopup.value = true
}

const closePopup = () => {
  showPopup.value = false
  activeCategory.value = ''
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

  const section = document.querySelector('.skills')
  if (section) {
    observer.observe(section)
  }
})
</script>

<template>
  <section id="skills" class="skills" :class="{ visible: isVisible }">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-number">TECH STACK</h2>
        
        <h3 class="section-title">
          <span class="text-gradient">Technologies & Tools</span>
        </h3>
        <p class="section-description">
          Building modern web applications with a diverse set of frontend, backend, and development tools.
        </p>
        
        <p class="section-footer-text">
          Always learning and improving to stay current with modern web development.
        </p>
      </div>

      <!-- Category Filter Buttons -->
      <div class="category-filter">
        <button 
          class="category-btn" 
          :class="{ active: activeCategory === 'all' }"
          @click="setCategory('all')"
        >
          <span class="category-icon">⭐</span>
          <span>All Skills</span>
        </button>
        <button 
          class="category-btn"
          :class="{ active: activeCategory === 'frontend' }"
          @click="setCategory('frontend')"
        >
          <span class="category-icon">🎨</span>
          <span>Frontend</span>
        </button>
        <button 
          class="category-btn"
          :class="{ active: activeCategory === 'backend' }"
          @click="setCategory('backend')"
        >
          <span class="category-icon">⚙️</span>
          <span>Backend</span>
        </button>
        <button 
          class="category-btn"
          :class="{ active: activeCategory === 'tools' }"
          @click="setCategory('tools')"
        >
          <span class="category-icon">🛠️</span>
          <span>Tools</span>
        </button>
      </div>

      <!-- Popup Overlay for Tech Stacks -->
      <transition name="popup">
        <div v-if="showPopup" class="popup-overlay" @click="closePopup">
          <div class="popup-content" @click.stop>
            <!-- Category Buttons at Top -->
            <div class="popup-category-filter">
              <button 
                class="popup-category-btn" 
                :class="{ active: activeCategory === 'all' }"
                @click="setCategory('all')"
              >
                <span class="category-icon">⭐</span>
                <span>All Skills</span>
              </button>
              <button 
                class="popup-category-btn"
                :class="{ active: activeCategory === 'frontend' }"
                @click="setCategory('frontend')"
              >
                <span class="category-icon">🎨</span>
                <span>Frontend</span>
              </button>
              <button 
                class="popup-category-btn"
                :class="{ active: activeCategory === 'backend' }"
                @click="setCategory('backend')"
              >
                <span class="category-icon">⚙️</span>
                <span>Backend</span>
              </button>
              <button 
                class="popup-category-btn"
                :class="{ active: activeCategory === 'tools' }"
                @click="setCategory('tools')"
              >
                <span class="category-icon">🛠️</span>
                <span>Tools</span>
              </button>
            </div>
            
            <h3 class="popup-title">
              {{ activeCategory === 'all' ? 'All Tech Stacks' : 
                 activeCategory === 'frontend' ? 'Frontend Technologies' :
                 activeCategory === 'backend' ? 'Backend Technologies' : 'Development Tools' }}
            </h3>
            
            <p class="popup-description">
              {{ activeCategory === 'all' ? 'A comprehensive view of all the technologies and tools I use to build modern web applications.' :
                 activeCategory === 'frontend' ? 'Building responsive and interactive user interfaces with modern JavaScript frameworks and styling tools.' :
                 activeCategory === 'backend' ? 'Creating robust server-side applications, APIs, and database solutions for scalable web systems.' :
                 'Essential tools and technologies I use daily for version control, API development, and design.' }}
            </p>
            
            <div class="popup-grid" :data-count="displayedTechStacks.length">
              <div
                v-for="(tech, index) in displayedTechStacks"
                :key="tech.name"
                class="tech-card"
                :style="{ '--index': index }"
              >
                <img :src="tech.icon" :alt="tech.name" class="tech-logo" />
                <p class="tech-label">{{ tech.name }}</p>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </section>
</template>

<style scoped>
.skills {
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

.skills.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: var(--spacing-lg);
  max-width: 900px;
  margin-left: auto;
  margin-right: auto;
}

.section-number {
  font-size: var(--text-xs);
  color: var(--color-accent-primary);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 600;
  margin-bottom: var(--spacing-sm);
}

/* Dynamic Category Display */
.category-display {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-xs);
  margin-bottom: var(--spacing-sm);
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.category-logo-container {
  width: 60px;
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.1) 0%, rgba(139, 92, 246, 0.05) 100%);
  border-radius: var(--radius-lg);
  padding: var(--spacing-sm);
  border: 2px solid var(--color-border);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.category-logo {
  width: 100%;
  height: 100%;
  object-fit: contain;
  filter: drop-shadow(0 2px 8px rgba(168, 85, 247, 0.3));
}

.category-logo[src*="github"],
.category-logo[src*="django"] {
  filter: invert(1) drop-shadow(0 2px 8px rgba(255, 255, 255, 0.3));
}

.category-icon-large {
  font-size: 48px;
  font-weight: 700;
  color: var(--color-accent-primary);
  padding: var(--spacing-sm) var(--spacing-lg);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.1) 0%, rgba(139, 92, 246, 0.05) 100%);
  border-radius: var(--radius-lg);
  border: 2px solid var(--color-border);
}

.section-title {
  font-size: var(--text-2xl);
  line-height: 1.2;
  margin: 0;
  color: var(--color-text-primary);
  font-weight: 700;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.section-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0;
  max-width: 700px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

.section-footer-text {
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  font-style: italic;
  margin-top: var(--spacing-xs);
  text-align: center;
}

/* Category Filter */
.category-filter {
  display: flex;
  justify-content: center;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-lg);
  flex-wrap: wrap;
}

.category-btn {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-md) var(--spacing-xl);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.1) 0%, rgba(139, 92, 246, 0.05) 100%);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-full);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: none;
}

.category-btn:hover {
  border-color: var(--color-accent-primary);
  color: var(--color-accent-primary);
  transform: translateY(-2px);
}

.category-btn.active {
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-color: var(--color-accent-primary);
  color: white;
}

.category-icon {
  font-size: var(--text-xl);
  font-weight: 700;
}

/* Responsive */
@media (max-width: 768px) {
  .skills {
    padding: var(--spacing-md) 0;
  }

  .section-title {
    font-size: var(--text-xl);
  }

  .section-description {
    font-size: var(--text-xs);
  }

  .category-logo-container {
    width: 50px;
    height: 50px;
  }

  .category-icon-large {
    font-size: 36px;
    padding: var(--spacing-xs) var(--spacing-md);
  }

  .skills-grid {
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: var(--spacing-sm);
    max-height: 45vh;
  }

  .category-filter {
    gap: var(--spacing-xs);
  }

  .category-btn {
    padding: var(--spacing-sm) var(--spacing-md);
    font-size: var(--text-sm);
  }

  .category-icon {
    font-size: var(--text-lg);
  }
}

@media (max-width: 480px) {
  .category-filter {
    flex-direction: column;
  }
  
  .category-btn {
    width: 100%;
    justify-content: center;
  }
}

/* Popup Overlay */
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(10, 10, 30, 0.98);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9999;
  padding: var(--spacing-lg);
  overflow-y: auto;
}

.popup-content {
  background: transparent;
  border: none;
  border-radius: 0;
  padding: var(--spacing-xl) var(--spacing-lg);
  max-width: 1400px;
  width: 100%;
  display: flex;
  flex-direction: column;
  position: relative;
  margin: auto;
}

/* Popup Category Filter */
.popup-category-filter {
  display: flex;
  justify-content: center;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-md);
  flex-wrap: wrap;
}

.popup-category-btn {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-sm) var(--spacing-lg);
  background: var(--color-bg-tertiary);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-full);
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.popup-category-btn:hover {
  border-color: var(--color-accent-primary);
  color: var(--color-accent-primary);
}

.popup-category-btn.active {
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-color: transparent;
  color: white;
}

.popup-category-btn .category-icon {
  font-size: var(--text-base);
}

.popup-title {
  font-size: var(--text-xl);
  font-weight: 700;
  color: var(--color-accent-primary);
  text-align: center;
  margin: 0 0 var(--spacing-xs) 0;
}

.popup-description {
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  text-align: center;
  line-height: 1.5;
  margin: 0 0 var(--spacing-lg) 0;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.popup-grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: var(--spacing-lg);
  max-width: 1200px;
  margin: 0 auto;
  justify-items: center;
  align-items: center;
}

/* All Skills Layout - 16 items: 5-6-5 rows */
.popup-grid[data-count="16"] .tech-card:nth-child(1) { grid-column: 1 / 3; }
.popup-grid[data-count="16"] .tech-card:nth-child(2) { grid-column: 3 / 5; }
.popup-grid[data-count="16"] .tech-card:nth-child(3) { grid-column: 6 / 8; } /* JavaScript - centered */
.popup-grid[data-count="16"] .tech-card:nth-child(4) { grid-column: 9 / 11; }
.popup-grid[data-count="16"] .tech-card:nth-child(5) { grid-column: 11 / 13; }

.popup-grid[data-count="16"] .tech-card:nth-child(6) { grid-column: 1 / 3; grid-row: 2; }
.popup-grid[data-count="16"] .tech-card:nth-child(7) { grid-column: 3 / 5; grid-row: 2; }
.popup-grid[data-count="16"] .tech-card:nth-child(8) { grid-column: 5 / 7; grid-row: 2; }
.popup-grid[data-count="16"] .tech-card:nth-child(9) { grid-column: 7 / 9; grid-row: 2; }
.popup-grid[data-count="16"] .tech-card:nth-child(10) { grid-column: 9 / 11; grid-row: 2; }
.popup-grid[data-count="16"] .tech-card:nth-child(11) { grid-column: 11 / 13; grid-row: 2; }

.popup-grid[data-count="16"] .tech-card:nth-child(12) { grid-column: 1 / 3; grid-row: 3; }
.popup-grid[data-count="16"] .tech-card:nth-child(13) { grid-column: 3 / 5; grid-row: 3; }
.popup-grid[data-count="16"] .tech-card:nth-child(14) { grid-column: 6 / 8; grid-row: 3; } /* GitHub - centered */
.popup-grid[data-count="16"] .tech-card:nth-child(15) { grid-column: 9 / 11; grid-row: 3; }
.popup-grid[data-count="16"] .tech-card:nth-child(16) { grid-column: 11 / 13; grid-row: 3; }

/* Frontend Layout - 7 items: custom positioning */
.popup-grid[data-count="7"] .tech-card:nth-child(1) { grid-column: 1 / 3; } /* HTML5 */
.popup-grid[data-count="7"] .tech-card:nth-child(2) { grid-column: 3 / 5; } /* CSS3 */
.popup-grid[data-count="7"] .tech-card:nth-child(3) { grid-column: 9 / 11; } /* TypeScript */
.popup-grid[data-count="7"] .tech-card:nth-child(4) { grid-column: 11 / 13; } /* Vue.js */
.popup-grid[data-count="7"] .tech-card:nth-child(5) { grid-column: 2 / 4; grid-row: 2; } /* React - under HTML5/CSS3 */
.popup-grid[data-count="7"] .tech-card:nth-child(6) { grid-column: 6 / 8; grid-row: 2; } /* JavaScript - centered */
.popup-grid[data-count="7"] .tech-card:nth-child(7) { grid-column: 10 / 12; grid-row: 2; } /* Tailwind - under TS/Vue */

/* Backend Layout - 5 items: centered 2-row layout */
.popup-grid[data-count="5"] {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: var(--spacing-lg);
  max-width: 1000px;
}

.popup-grid[data-count="5"] .tech-card {
  flex: 0 0 auto;
}

/* Tools Layout - 4 items: centered 2x2 layout */
.popup-grid[data-count="4"] {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: var(--spacing-lg);
  max-width: 700px;
}

.popup-grid[data-count="4"] .tech-card {
  flex: 0 0 auto;
}

.tech-card {
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--spacing-md);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-xs);
  min-height: 100px;
  width: 100%;
  max-width: 140px;
  transition: border-color 0.2s ease;
}

.tech-card:hover {
  border-color: var(--color-accent-primary);
}

.tech-logo {
  width: 40px;
  height: 40px;
  object-fit: contain;
}

.tech-logo[src*="github"],
.tech-logo[src*="django"] {
  filter: invert(1);
}

.tech-label {
  font-size: var(--text-xs);
  font-weight: 600;
  color: var(--color-text-primary);
  text-align: center;
  margin: 0;
  line-height: 1.2;
}

/* Popup Transition */
.popup-enter-active, .popup-leave-active {
  transition: opacity 0.3s ease;
}

.popup-enter-from, .popup-leave-to {
  opacity: 0;
}

.popup-enter-active .popup-content,
.popup-leave-active .popup-content {
  transition: transform 0.3s ease;
}

.popup-enter-from .popup-content,
.popup-leave-to .popup-content {
  transform: scale(0.95);
}

/* Responsive */
@media (max-width: 768px) {
  .popup-content {
    padding: var(--spacing-md);
  }

  .popup-category-filter {
    gap: var(--spacing-xs);
  }

  .popup-category-btn {
    padding: var(--spacing-xs) var(--spacing-md);
    font-size: var(--text-xs);
  }

  .popup-category-btn .category-icon {
    font-size: var(--text-sm);
  }

  .popup-title {
    font-size: var(--text-lg);
  }

  .popup-description {
    font-size: 11px;
    margin-bottom: var(--spacing-sm);
  }

  .popup-grid {
    grid-template-columns: repeat(3, 1fr);
    gap: var(--spacing-sm);
  }

  .tech-card {
    padding: var(--spacing-sm);
    min-height: 80px;
  }

  .tech-logo {
    width: 32px;
    height: 32px;
  }

  .tech-label {
    font-size: 10px;
  }
}

@media (max-width: 480px) {
  .popup-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>
