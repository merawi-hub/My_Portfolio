<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

interface TechStack {
  name: string
  icon: string
}

const frontendStacks = ref<TechStack[]>([
  { name: 'HTML5', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original.svg' },
  { name: 'CSS3', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original.svg' },
  { name: 'TypeScript', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-original.svg' },
  { name: 'Vue.js', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/vuejs/vuejs-original.svg' },
  { name: 'React', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg' },
  { name: 'JavaScript', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-original.svg' },
  { name: 'Tailwind', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/tailwindcss/tailwindcss-original.svg' }
])

const backendStacks = ref<TechStack[]>([
  { name: 'PHP', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/php/php-original.svg' },
  { name: 'Laravel', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/laravel/laravel-original.svg' },
  { name: 'Python', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/python/python-original.svg' },
  { name: 'Django', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/django/django-plain.svg' },
  { name: 'MySQL', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/mysql/mysql-original.svg' }
])

const toolsStacks = ref<TechStack[]>([
  { name: 'Git', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/git/git-original.svg' },
  { name: 'GitHub', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/github/github-original.svg' },
  { name: 'VS Code', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/vscode/vscode-original.svg' },
  { name: 'Figma', icon: 'https://raw.githubusercontent.com/devicons/devicon/master/icons/figma/figma-original.svg' }
])

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
      <div class="main-header">
        <h2 class="main-title">Technologies & Tools</h2>
        <p class="main-description">
          Building modern web applications with diverse frontend, backend, and development tools.
        </p>
      </div>

      <div class="stacks-wrapper">
        <div class="stack-section">
          <div class="section-header">
            <span class="section-icon">🎨</span>
            <h3 class="section-title">Frontend</h3>
          </div>
          <p class="section-description">
            Building responsive and interactive user interfaces with modern frameworks
          </p>
          <div class="tech-grid">
            <div
              v-for="tech in frontendStacks"
              :key="tech.name"
              class="tech-item"
            >
              <img :src="tech.icon" :alt="tech.name" class="tech-icon" />
              <span class="tech-name">{{ tech.name }}</span>
            </div>
          </div>
        </div>

        <div class="stack-section">
          <div class="section-header">
            <span class="section-icon">⚙️</span>
            <h3 class="section-title">Backend</h3>
          </div>
          <p class="section-description">
            Creating robust server-side applications and database solutions
          </p>
          <div class="tech-grid">
            <div
              v-for="tech in backendStacks"
              :key="tech.name"
              class="tech-item"
            >
              <img :src="tech.icon" :alt="tech.name" class="tech-icon" />
              <span class="tech-name">{{ tech.name }}</span>
            </div>
          </div>
        </div>

        <div class="stack-section">
          <div class="section-header">
            <span class="section-icon">🛠️</span>
            <h3 class="section-title">Tools</h3>
          </div>
          <p class="section-description">
            Essential tools for version control, development, and design
          </p>
          <div class="tech-grid">
            <div
              v-for="tech in toolsStacks"
              :key="tech.name"
              class="tech-item"
            >
              <img :src="tech.icon" :alt="tech.name" class="tech-icon" />
              <span class="tech-name">{{ tech.name }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.skills {
  min-height: auto;
  padding: 20px 30px;
  position: relative;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.skills.visible {
  opacity: 1;
  transform: translateY(0);
}

.container {
  max-width: 100%;
  margin: 0 auto;
}

.main-header {
  text-align: center;
  margin-bottom: 20px;
}

.main-title {
  font-size: 24px;
  font-weight: 700;
  margin: 0 0 8px 0;
  background: linear-gradient(135deg, rgba(168, 85, 247, 1) 0%, rgba(139, 92, 246, 1) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.main-description {
  font-size: 13px;
  color: rgba(255, 255, 255, 0.6);
  margin: 0;
  line-height: 1.4;
}

.stacks-wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-top: 20px;
}

.stack-section {
  background: rgba(25, 30, 45, 0.5);
  border: 1px solid rgba(90, 140, 140, 0.2);
  border-radius: 12px;
  padding: 15px;
  transition: border-color 0.3s ease;
}

.stack-section:hover {
  border-color: rgba(90, 140, 140, 0.4);
}

.section-header {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 6px;
}

.section-icon {
  font-size: 20px;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.9);
  margin: 0;
}

.section-description {
  font-size: 11px;
  color: rgba(255, 255, 255, 0.5);
  margin: 0 0 12px 0;
  line-height: 1.3;
}

.tech-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(60px, 1fr));
  gap: 10px;
}

.tech-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  padding: 8px;
  background: rgba(15, 20, 35, 0.6);
  border: 1px solid rgba(60, 70, 90, 0.5);
  border-radius: 8px;
  transition: all 0.2s ease;
  cursor: pointer;
}

.tech-item:hover {
  border-color: rgba(90, 140, 140, 0.5);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.tech-icon {
  width: 28px;
  height: 28px;
  object-fit: contain;
}

.tech-icon[src*="github"],
.tech-icon[src*="django"] {
  filter: invert(1);
}

.tech-name {
  font-size: 9px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.8);
  text-align: center;
  line-height: 1.2;
}

@media (max-width: 900px) {
  .stacks-wrapper {
    grid-template-columns: 1fr;
    gap: 15px;
  }
  
  .tech-grid {
    grid-template-columns: repeat(auto-fit, minmax(70px, 1fr));
  }
}

@media (max-width: 768px) {
  .skills {
    padding: 15px 20px;
  }

  .main-title {
    font-size: 20px;
  }

  .main-description {
    font-size: 12px;
  }

  .stack-section {
    padding: 12px;
  }

  .section-title {
    font-size: 14px;
  }

  .section-description {
    font-size: 10px;
  }

  .tech-icon {
    width: 24px;
    height: 24px;
  }

  .tech-name {
    font-size: 8px;
  }
}
</style>
