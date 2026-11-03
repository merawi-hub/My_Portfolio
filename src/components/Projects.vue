<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

interface Project {
  id: number
  title: string
  description: string
  image: string
  tags: string[]
  category: string
  github?: string
  demo?: string
  featured?: boolean
}

const allProjects = ref<Project[]>([
  {
    id: 1,
    title: 'E-Commerce Platform',
    description: 'A full-stack e-commerce application with user authentication, product management, shopping cart, and payment integration using modern web technologies.',
    image: '/images/project-ecommerce.jpg',
    tags: ['Vue.js', 'Laravel', 'MySQL', 'Tailwind CSS'],
    category: 'fullstack',
    github: 'https://github.com/yourusername/ecommerce',
    demo: 'https://demo.ecommerce.com',
    featured: true
  },
  {
    id: 2,
    title: 'Task Management App',
    description: 'A collaborative task management tool with real-time updates, team collaboration features, and progress tracking for enhanced productivity.',
    image: '/images/project-taskmanager.jpg',
    tags: ['React', 'Django', 'PostgreSQL', 'REST API'],
    category: 'fullstack',
    github: 'https://github.com/yourusername/taskmanager',
    demo: 'https://demo.taskmanager.com',
    featured: true
  },
  {
    id: 3,
    title: 'Portfolio Website',
    description: 'A modern, responsive portfolio website with smooth animations, interactive 3D elements, and optimized performance showcasing my work and skills.',
    image: '/images/project-portfolio.jpg',
    tags: ['Vue.js', 'TypeScript', 'Vite', 'CSS3'],
    category: 'frontend',
    github: 'https://github.com/yourusername/portfolio',
    demo: 'https://yourportfolio.com',
    featured: true
  },
  {
    id: 4,
    title: 'Blog Platform',
    description: 'A content management system for bloggers with rich text editor, media uploads, SEO optimization, and social sharing capabilities.',
    image: '/images/project-blog.jpg',
    tags: ['PHP', 'Laravel', 'MySQL', 'Bootstrap'],
    category: 'backend',
    github: 'https://github.com/yourusername/blog'
  },
  {
    id: 5,
    title: 'Weather Dashboard',
    description: 'A weather application that provides real-time weather data, forecasts, interactive maps, and data visualization using external APIs.',
    image: '/images/project-weather.jpg',
    tags: ['JavaScript', 'API Integration', 'Chart.js', 'CSS3'],
    category: 'frontend',
    github: 'https://github.com/yourusername/weather',
    demo: 'https://demo.weather.com'
  },
  {
    id: 6,
    title: 'REST API Service',
    description: 'A RESTful API service with JWT authentication, rate limiting, comprehensive Swagger documentation, and third-party integration support.',
    image: '/images/project-api.jpg',
    tags: ['Python', 'Django REST', 'JWT', 'Swagger'],
    category: 'backend',
    github: 'https://github.com/yourusername/api'
  }
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

  const section = document.querySelector('.projects')
  if (section) {
    observer.observe(section)
  }
})
</script>

<template>
  <section id="projects" class="projects" :class="{ visible: isVisible }">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-number">MY WORK</h2>
        <h3 class="section-title">
          Projects I've built with <span class="text-gradient">passion & purpose</span>
        </h3>
        <p class="section-description">
          Explore the applications I've developed, the challenges I've tackled, and the ideas I've brought to life.
        </p>
      </div>

      <!-- Projects Grid -->
      <transition-group name="project-list" tag="div" class="projects-grid">
        <div
          v-for="(project, index) in allProjects"
          :key="project.id"
          class="project-card"
          :class="{ featured: project.featured }"
          :style="{ '--delay': index * 0.1 + 's' }"
        >
          <div class="project-image-wrapper">
            <div v-if="project.featured" class="featured-badge">
              <svg width="16" height="16" viewBox="0 0 24 24" fill="currentColor">
                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
              </svg>
              Featured
            </div>
            <img 
              :src="project.image" 
              :alt="project.title"
              class="project-image"
              @error="(e) => (e.target as HTMLImageElement).src = '/images/profile-placeholder.svg'"
            />
            <div class="project-overlay">
              <div class="project-links">
                <a 
                  v-if="project.github" 
                  :href="project.github" 
                  target="_blank"
                  rel="noopener noreferrer"
                  class="project-link"
                  title="View Code"
                >
                  <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
                  </svg>
                </a>
                <a 
                  v-if="project.demo" 
                  :href="project.demo" 
                  target="_blank"
                  rel="noopener noreferrer"
                  class="project-link"
                  title="View Demo"
                >
                  <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/>
                    <polyline points="15 3 21 3 21 9"/>
                    <line x1="10" y1="14" x2="21" y2="3"/>
                  </svg>
                </a>
              </div>
            </div>
          </div>
          
          <div class="project-content">
            <h4 class="project-title">{{ project.title }}</h4>
            <p class="project-description">{{ project.description }}</p>
            
            <div class="project-tags">
              <span 
                v-for="tag in project.tags" 
                :key="tag"
                class="project-tag"
              >
                {{ tag }}
              </span>
            </div>
          </div>
        </div>
      </transition-group>
    </div>
  </section>
</template>

<style scoped>
.projects {
  min-height: 100vh;
  padding: var(--spacing-xl) 0 var(--spacing-xl) 0;
  position: relative;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1), 
              transform 1s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  align-items: center;
}

.projects.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: var(--spacing-lg);
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
  opacity: 1;
  transform: none;
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
  margin: 0 0 var(--spacing-md) 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

.section-title .text-gradient {
  display: inline-block;
}

.section-description {
  font-size: var(--text-base);
  color: var(--color-text-secondary);
  line-height: 1.7;
  margin: 0;
}

/* Projects Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(360px, 1fr));
  gap: var(--spacing-xl);
  animation: fadeInUp 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
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

.project-card {
  background: var(--color-bg-card);
  backdrop-filter: blur(10px);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  overflow: hidden;
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  flex-direction: column;
  position: relative;
  opacity: 0;
  animation: projectCardEntrance 0.6s cubic-bezier(0.4, 0, 0.2, 1) forwards;
  animation-delay: var(--delay);
}

@keyframes projectCardEntrance {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

.project-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: linear-gradient(90deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  z-index: 1;
}

.project-card:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-12px) rotateX(2deg);
  box-shadow: 0 20px 40px rgba(168, 85, 247, 0.35),
              0 0 0 1px rgba(168, 85, 247, 0.1);
}

.project-card:hover::before {
  transform: scaleX(1);
}

/* Featured Badge */
.featured-badge {
  position: absolute;
  top: var(--spacing-md);
  right: var(--spacing-md);
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  color: white;
  padding: var(--spacing-xs) var(--spacing-sm);
  border-radius: var(--radius-sm);
  font-size: var(--text-xs);
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  z-index: 2;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.6);
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
  0%, 100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.85;
    transform: scale(1.05);
  }
}

.featured-badge svg {
  animation: rotateStar 3s linear infinite;
}

@keyframes rotateStar {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.project-card.featured {
  border-color: rgba(168, 85, 247, 0.3);
  background: linear-gradient(135deg, 
    rgba(168, 85, 247, 0.05) 0%, 
    var(--color-bg-card) 50%,
    rgba(139, 92, 246, 0.05) 100%
  );
}

/* Project Image */
.project-image-wrapper {
  position: relative;
  width: 100%;
  height: 240px;
  overflow: hidden;
  background: var(--color-bg-tertiary);
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.project-card:hover .project-image {
  transform: scale(1.15) rotate(1deg);
}

.project-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(4px);
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: opacity 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.project-card:hover .project-overlay {
  opacity: 1;
}

.project-links {
  display: flex;
  gap: var(--spacing-md);
  transform: translateY(20px);
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1) 0.1s;
}

.project-card:hover .project-links {
  transform: translateY(0);
}

.project-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 56px;
  height: 56px;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-radius: var(--radius-full);
  color: white;
  text-decoration: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 16px rgba(168, 85, 247, 0.5);
  position: relative;
  overflow: hidden;
}

.project-link::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.3s, height 0.3s;
}

.project-link:hover::before {
  width: 100px;
  height: 100px;
}

.project-link:hover {
  transform: translateY(-4px) scale(1.1);
  box-shadow: 0 8px 24px rgba(168, 85, 247, 0.7);
}

.project-link svg {
  position: relative;
  z-index: 1;
}

/* Project Content */
.project-content {
  padding: var(--spacing-lg);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
  flex: 1;
  background: var(--color-bg-card);
}

.project-title {
  font-size: var(--text-xl);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0;
  transition: color 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.project-card:hover .project-title {
  color: var(--color-accent-primary);
}

.project-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: 1.7;
  margin: 0;
  flex: 1;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: var(--spacing-xs);
  margin-top: var(--spacing-sm);
}

.project-tag {
  padding: var(--spacing-xs) var(--spacing-sm);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.15) 0%, rgba(139, 92, 246, 0.08) 100%);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  font-weight: 500;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.project-tag::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  transition: left 0.5s;
}

.project-card:hover .project-tag::before {
  left: 100%;
}

.project-card:hover .project-tag {
  border-color: var(--color-accent-primary);
  color: var(--color-accent-primary);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.25) 0%, rgba(139, 92, 246, 0.15) 100%);
  transform: translateY(-2px);
}

/* Transition Animations */
.project-list-enter-active,
.project-list-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.project-list-enter-from {
  opacity: 0;
  transform: scale(0.9) translateY(20px);
}

.project-list-leave-to {
  opacity: 0;
  transform: scale(0.9) translateY(-20px);
}

.project-list-move {
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

/* Responsive */
@media (max-width: 1024px) {
  .projects-grid {
    grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  }
}

@media (max-width: 768px) {
  .projects {
    padding: var(--spacing-md) 0;
  }

  .section-title {
    font-size: var(--text-2xl);
  }

  .section-description {
    font-size: var(--text-sm);
  }

  .projects-grid {
    grid-template-columns: 1fr;
    gap: var(--spacing-lg);
  }

  .project-image-wrapper {
    height: 200px;
  }

  .project-link {
    width: 48px;
    height: 48px;
  }

  .project-card:hover {
    transform: translateY(-8px);
  }
}
</style>
