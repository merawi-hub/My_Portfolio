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
    title: 'Online Grocery Delivery App',
    description: 'A comprehensive online grocery shopping platform with real-time order tracking, payment integration, inventory management, and delivery scheduling system.',
    image: '/images/project-grocery.jpg',
    tags: ['Laravel', 'Vue.js', 'MySQL', 'REST API'],
    category: 'fullstack',
    github: 'https://github.com/merawi-hub/online_grocery_delivery_app.git',
    featured: true
  },
  {
    id: 2,
    title: 'Clinic Appointment Scheduling System',
    description: 'A healthcare management system for scheduling patient appointments, managing doctor availability, patient records, and automated appointment reminders.',
    image: '/images/project-clinic.jpg',
    tags: ['Django', 'Python', 'PostgreSQL', 'Bootstrap'],
    category: 'fullstack',
    github: 'https://github.com/merawi-hub/Clinic_appointment_scheduling_system.git',
    featured: true
  },
  {
    id: 3,
    title: 'Task Scheduling Management System',
    description: 'A collaborative task management application with team workflows, priority-based scheduling, deadline tracking, and progress monitoring for enhanced productivity.',
    image: '/images/project-taskmanager.jpg',
    tags: ['Laravel', 'Vue.js', 'MySQL', 'Tailwind CSS'],
    category: 'fullstack',
    github: 'https://github.com/merawi-hub/Task-Scheduler-System.git',
    featured: true
  },
  {
    id: 4,
    title: 'Portfolio Website',
    description: 'Personal portfolio built with Vue 3 and TypeScript, featuring interactive UI components, form validation, and performance-optimized animations to showcase projects and technical expertise.',
    image: '/images/project-portfolio.jpg',
    tags: ['Vue.js', 'TypeScript', 'Vite', 'CSS3'],
    category: 'frontend',
    github: 'https://github.com/merawi-hub/My_Portfolio.git',
    demo: 'https://merawiayele.com',
    featured: false
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
      <div class="projects-grid">
        <div
          v-for="project in allProjects"
          :key="project.id"
          class="project-card"
        >
          <!-- Project Image -->
          <div class="project-image-container">
            <img 
              :src="project.image" 
              :alt="project.title"
              class="project-image"
              @error="(e) => (e.target as HTMLImageElement).src = '/images/profile-placeholder.svg'"
            />
            <div v-if="project.featured" class="featured-badge">FEATURED</div>
          </div>

          <!-- Project Info -->
          <div class="project-info">
            <h4 class="project-title">{{ project.title }}</h4>
            <p class="project-description">{{ project.description }}</p>

            <!-- Tags -->
            <div class="project-tags">
              <span v-for="tag in project.tags" :key="tag" class="tag">
                {{ tag }}
              </span>
            </div>

            <!-- Links -->
            <div class="project-links">
              <a 
                v-if="project.github" 
                :href="project.github" 
                target="_blank"
                rel="noopener noreferrer"
                class="project-link github"
                title="View on GitHub"
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
                class="project-link demo"
                title="View Live Demo"
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
      </div>
    </div>
  </section>
</template>

<style scoped>
.projects {
  min-height: 100vh;
  padding: 100px 0;
  display: flex;
  align-items: center;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.6s ease;
}

.projects.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: 60px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.section-number {
  font-size: 12px;
  color: var(--color-accent-primary);
  text-transform: uppercase;
  letter-spacing: 0.15em;
  font-weight: 600;
  margin-bottom: 16px;
}

.section-title {
  font-size: 48px;
  line-height: 1.2;
  margin: 0 0 24px 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

.section-title .text-gradient {
  display: inline-block;
}

.section-description {
  font-size: 18px;
  color: var(--color-text-secondary);
  line-height: 1.7;
  margin: 0;
  text-align: center;
}

/* Projects Grid */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 32px;
}

/* Project Card */
.project-card {
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
  border-radius: 16px;
  overflow: hidden;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  -webkit-font-smoothing: subpixel-antialiased;
}

.project-card:hover {
  border-color: var(--color-accent-primary);
  box-shadow: 0 8px 32px rgba(168, 85, 247, 0.2);
}

/* Project Image */
.project-image-container {
  position: relative;
  width: 100%;
  height: 220px;
  overflow: hidden;
  background: var(--color-bg-tertiary);
  transform: translate3d(0, 0, 0);
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transform: translate3d(0, 0, 0);
}

.featured-badge {
  position: absolute;
  top: 16px;
  right: 16px;
  background: linear-gradient(135deg, var(--color-accent-primary), var(--color-accent-secondary));
  color: white;
  padding: 6px 12px;
  border-radius: 6px;
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.05em;
}

/* Project Info */
.project-info {
  padding: 24px;
  transform: translate3d(0, 0, 0);
}

.project-title {
  font-size: 22px;
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 12px 0;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  -webkit-font-smoothing: subpixel-antialiased;
}

.project-description {
  font-size: 14px;
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0 0 16px 0;
}

/* Tags */
.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 20px;
}

.tag {
  padding: 6px 12px;
  background: rgba(168, 85, 247, 0.1);
  border: 1px solid rgba(168, 85, 247, 0.3);
  border-radius: 6px;
  font-size: 12px;
  color: var(--color-text-secondary);
  font-weight: 500;
}

/* Project Links */
.project-links {
  display: flex;
  gap: 12px;
}

.project-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 48px;
  height: 48px;
  background: rgba(168, 85, 247, 0.1);
  border: 1px solid var(--color-accent-primary);
  border-radius: 50%;
  color: var(--color-accent-primary);
  text-decoration: none;
  transition: all 0.3s ease;
}

.project-link:hover {
  background: var(--color-accent-primary);
  color: white;
  transform: translateY(-2px);
}

.project-link.demo {
  background: var(--color-accent-primary);
  color: white;
}

.project-link.demo:hover {
  background: var(--color-accent-secondary);
}

/* Responsive */
@media (max-width: 768px) {
  .projects {
    padding: 80px 0;
  }

  .section-title {
    font-size: 32px;
  }

  .section-description {
    font-size: 16px;
  }

  .projects-grid {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  .project-image-container {
    height: 200px;
  }
}
</style>
