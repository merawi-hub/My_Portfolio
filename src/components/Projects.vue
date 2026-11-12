<script setup lang="ts">
import { ref, computed } from 'vue'

interface Project {
  id: number
  title: string
  description: string
  image: string
  tags: string[]
  github?: string
  demo?: string
}

const allProjects = ref<Project[]>([
  {
    id: 1,
    title: 'Online Grocery Delivery App',
    description: 'A comprehensive online grocery shopping platform with real-time order tracking, payment integration, inventory management, and delivery scheduling system.',
    image: '/images/project-grocery.jpg',
    tags: ['Laravel', 'Vue.js', 'MySQL', 'REST API'],
    github: 'https://github.com/merawi-hub/online_grocery_delivery_app.git',
  },
  {
    id: 2,
    title: 'Clinic Appointment Scheduling System',
    description: 'A healthcare management system for scheduling patient appointments, managing doctor availability, patient records, and automated appointment reminders.',
    image: '/images/project-clinic.jpg',
    tags: ['Django', 'Python', 'PostgreSQL', 'Bootstrap'],
    github: 'https://github.com/merawi-hub/Clinic_appointment_scheduling_system.git',
  },
  {
    id: 3,
    title: 'Task Scheduling Management System',
    description: 'A collaborative task management application with team workflows, priority-based scheduling, deadline tracking, and progress monitoring for enhanced productivity.',
    image: '/images/project-taskmanager.jpg',
    tags: ['Laravel', 'Vue.js', 'MySQL', 'Tailwind CSS'],
    github: 'https://github.com/merawi-hub/Task-Scheduler-System.git',
  },
  {
    id: 4,
    title: 'Portfolio Website',
    description: 'Personal portfolio built with Vue 3 and TypeScript, featuring interactive UI components, form validation, and performance-optimized animations to showcase projects and technical expertise.',
    image: '/images/project-portfolio.jpg',
    tags: ['Vue.js', 'TypeScript', 'Vite', 'CSS3'],
    github: 'https://github.com/merawi-hub/My_Portfolio.git',
    demo: 'https://merawiayele.com',
  }
])

const activeProjectIndex = ref(0)

const activeProject = computed<Project>(() => {
  const project = allProjects.value[activeProjectIndex.value]
  return project ?? allProjects.value[0]!
})

const updateActiveProject = (index: number) => {
  activeProjectIndex.value = index
}
</script>

<template>
  <section id="projects" class="projects">
    <div class="container">
      <div class="section-header">
        <h2 class="section-title">MY PROJECTS</h2>
      </div>

      <div class="project-tabs">
        <button
          v-for="(project, index) in allProjects"
          :key="project.id"
          @click="updateActiveProject(index)"
          class="tab-btn"
          :class="{ active: activeProjectIndex === index }"
        >
          Project {{ index + 1 }}
        </button>
      </div>

      <div class="project-showcase">
        <div class="project-card">
          <div class="card-header">
            <h3 class="project-title">{{ activeProject.title }}</h3>
            <div class="project-links">
              <a 
                v-if="activeProject.demo" 
                :href="activeProject.demo" 
                target="_blank"
                rel="noopener noreferrer"
                class="link-btn demo"
              >
                Demo
              </a>
              <a 
                v-if="activeProject.github" 
                :href="activeProject.github" 
                target="_blank"
                rel="noopener noreferrer"
                class="link-btn code"
              >
                Code
              </a>
            </div>
          </div>

          <div class="project-image-wrapper">
            <img 
              :src="activeProject.image" 
              :alt="activeProject.title"
              class="project-image"
              @error="(e) => (e.target as HTMLImageElement).src = '/images/profile-placeholder.svg'"
            />
          </div>

          <div class="project-info">
            <p class="project-description">{{ activeProject.description }}</p>
            <div class="project-tags">
              <span v-for="tag in activeProject.tags" :key="tag" class="tag">
                {{ tag }}
              </span>
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
  height: 100vh;
  padding: 90px 0 20px;
  display: flex;
  align-items: center;
  overflow-y: auto;
  background: linear-gradient(180deg, #0a0e1a 0%, #1a1f2e 100%);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 40px;
  width: 100%;
}

.section-header {
  text-align: center;
  margin-bottom: 12px;
}

.section-title {
  font-size: 32px;
  font-weight: 700;
  color: white;
  letter-spacing: 0.05em;
  margin: 0;
}

.project-tabs {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-bottom: 16px;
  flex-wrap: wrap;
}

.tab-btn {
  padding: 6px 18px;
  background: rgba(30, 35, 50, 0.6);
  border: 2px solid rgba(255, 255, 255, 0.1);
  border-radius: 6px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  background: rgba(30, 35, 50, 0.9);
  border-color: rgba(90, 140, 140, 0.5);
  color: rgb(90, 140, 140);
}

.tab-btn.active {
  background: rgba(90, 140, 140, 0.2);
  border-color: rgb(90, 140, 140);
  color: rgb(90, 140, 140);
}

.project-showcase {
  display: flex;
  justify-content: center;
}

.project-card {
  width: 100%;
  max-width: 900px;
  background: rgba(30, 35, 50, 0.9);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.project-card:hover {
  transform: translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px 24px;
  background: rgba(20, 25, 40, 0.8);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.project-title {
  font-size: 18px;
  font-weight: 600;
  color: white;
  margin: 0;
}

.project-links {
  display: flex;
  gap: 12px;
}

.link-btn {
  padding: 8px 20px;
  border-radius: 6px;
  font-size: 14px;
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s ease;
  border: 1px solid;
}

.link-btn.demo {
  background: rgba(90, 140, 140, 0.2);
  border-color: rgba(90, 140, 140, 0.5);
  color: rgb(90, 140, 140);
}

.link-btn.demo:hover {
  background: rgb(90, 140, 140);
  color: white;
}

.link-btn.code {
  background: transparent;
  border-color: rgba(255, 255, 255, 0.3);
  color: white;
}

.link-btn.code:hover {
  background: rgba(255, 255, 255, 0.1);
  border-color: rgba(255, 255, 255, 0.5);
}

.project-image-wrapper {
  width: 100%;
  height: 220px;
  overflow: hidden;
  background: rgba(20, 25, 40, 0.5);
}

.project-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.project-card:hover .project-image {
  transform: scale(1.05);
}

.project-info {
  padding: 14px 24px;
}

.project-description {
  font-size: 13px;
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.8);
  margin: 0 0 10px 0;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
}

.tag {
  padding: 3px 10px;
  background: rgba(90, 140, 140, 0.15);
  border: 1px solid rgba(90, 140, 140, 0.3);
  border-radius: 4px;
  font-size: 11px;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 500;
}

@media (max-width: 768px) {
  .projects {
    padding: 40px 0 20px;
  }

  .container {
    padding: 0 20px;
  }

  .section-title {
    font-size: 32px;
  }

  .project-tabs {
    gap: 10px;
  }

  .tab-btn {
    padding: 10px 20px;
    font-size: 14px;
  }

  .card-header {
    flex-direction: column;
    gap: 16px;
    align-items: flex-start;
    padding: 20px;
  }

  .project-title {
    font-size: 20px;
  }

  .project-image-wrapper {
    height: 200px;
  }

  .project-info {
    padding: 20px;
  }

  .project-description {
    font-size: 14px;
  }
}
</style>
