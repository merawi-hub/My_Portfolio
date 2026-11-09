<script setup lang="ts">
import { ref } from 'vue'

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
</script>

<template>
  <section id="projects" class="projects">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-title">MY PROJECTS</h2>
      </div>

      <!-- Stacked Cards -->
      <div class="stacked-cards">
        <div
          v-for="(project, index) in allProjects"
          :key="project.id"
          class="project-card"
          :style="{ '--index': index }"
        >
          <!-- Card Header with Title and Links -->
          <div class="card-header">
            <h3 class="project-title">{{ project.title }}</h3>
            <div class="project-links">
              <a 
                v-if="project.demo" 
                :href="project.demo" 
                target="_blank"
                rel="noopener noreferrer"
                class="link-btn demo"
              >
                Demo
              </a>
              <a 
                v-if="project.github" 
                :href="project.github" 
                target="_blank"
                rel="noopener noreferrer"
                class="link-btn code"
              >
                Code
              </a>
            </div>
          </div>

          <!-- Project Image -->
          <div class="project-image-wrapper">
            <img 
              :src="project.image" 
              :alt="project.title"
              class="project-image"
              @error="(e) => (e.target as HTMLImageElement).src = '/images/profile-placeholder.svg'"
            />
          </div>

          <!-- Project Info -->
          <div class="project-info">
            <p class="project-description">{{ project.description }}</p>
            <div class="project-tags">
              <span v-for="tag in project.tags" :key="tag" class="tag">
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
  padding: 60px 0 40px;
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

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: 60px;
}

.section-title {
  font-size: 48px;
  font-weight: 700;
  color: white;
  letter-spacing: 0.05em;
  margin: 0;
}

/* Stacked Cards Container */
.stacked-cards {
  position: relative;
  min-height: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Project Card */
.project-card {
  position: absolute;
  top: calc(var(--index) * 30px);
  left: 50%;
  transform: translateX(-50%) scale(calc(1 - var(--index) * 0.05));
  width: 90%;
  max-width: 900px;
  background: rgba(30, 35, 50, 0.9);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  overflow: hidden;
  transition: all 0.3s ease;
  z-index: calc(10 - var(--index));
}

.project-card:hover {
  transform: translateX(-50%) scale(calc(1 - var(--index) * 0.05)) translateY(-10px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
}

/* Card Header */
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 24px 32px;
  background: rgba(20, 25, 40, 0.8);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.project-title {
  font-size: 24px;
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

/* Project Image */
.project-image-wrapper {
  width: 100%;
  height: 300px;
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

/* Project Info */
.project-info {
  padding: 24px 32px;
}

.project-description {
  font-size: 16px;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
  margin: 0 0 20px 0;
}

.project-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.tag {
  padding: 6px 14px;
  background: rgba(90, 140, 140, 0.15);
  border: 1px solid rgba(90, 140, 140, 0.3);
  border-radius: 6px;
  font-size: 13px;
  color: rgba(255, 255, 255, 0.9);
  font-weight: 500;
}

/* Responsive */
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

  .project-card {
    width: 95%;
    top: calc(var(--index) * 20px);
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
