<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

interface ExperienceItem {
  id: number
  type: 'work' | 'education' | 'certification'
  title: string
  organization: string
  location: string
  period: string
  description: string
  technologies?: string[]
  achievements?: string[]
  icon: string
}

const experiences = ref<ExperienceItem[]>([
  {
    id: 1,
    type: 'work',
    title: 'Full Stack Developer',
    organization: 'Tech Company',
    location: 'Remote',
    period: '2023 - Present',
    description: 'Building modern web applications with Vue.js, Laravel, and MySQL. Leading frontend development and API design.',
    technologies: ['Vue.js', 'Laravel', 'MySQL', 'TypeScript', 'Tailwind CSS'],
    achievements: [
      'Developed 5+ production-ready web applications',
      'Improved application performance by 40%',
      'Mentored 3 junior developers'
    ],
    icon: '💼'
  },
  {
    id: 2,
    type: 'work',
    title: 'Frontend Developer',
    organization: 'Digital Agency',
    location: 'Hybrid',
    period: '2022 - 2023',
    description: 'Created responsive and interactive user interfaces using React and Vue.js. Collaborated with designers and backend teams.',
    technologies: ['React', 'Vue.js', 'JavaScript', 'CSS3', 'Git'],
    achievements: [
      'Built 10+ client websites',
      'Reduced page load time by 50%',
      'Implemented design system'
    ],
    icon: '💼'
  },
  {
    id: 3,
    type: 'education',
    title: 'Bachelor of Computer Science',
    organization: 'University Name',
    location: 'City, Country',
    period: '2018 - 2022',
    description: 'Studied software engineering, data structures, algorithms, and web technologies. Graduated with honors.',
    achievements: [
      'GPA: 3.8/4.0',
      'Dean\'s List all semesters',
      'Final project: E-commerce Platform'
    ],
    icon: '🎓'
  },
  {
    id: 4,
    type: 'certification',
    title: 'AWS Certified Developer',
    organization: 'Amazon Web Services',
    location: 'Online',
    period: '2023',
    description: 'Certified in AWS cloud services, serverless architecture, and deployment strategies.',
    icon: '📜'
  },
  {
    id: 5,
    type: 'certification',
    title: 'Vue.js Certification',
    organization: 'Vue School',
    location: 'Online',
    period: '2022',
    description: 'Advanced certification in Vue.js 3 composition API, state management, and best practices.',
    icon: '📜'
  }
])

const activeFilter = ref<'all' | 'work' | 'education' | 'certification'>('all')

const filteredExperiences = ref(experiences.value)

const filterExperiences = (type: 'all' | 'work' | 'education' | 'certification') => {
  activeFilter.value = type
  if (type === 'all') {
    filteredExperiences.value = experiences.value
  } else {
    filteredExperiences.value = experiences.value.filter(exp => exp.type === type)
  }
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

  const section = document.querySelector('.experience')
  if (section) {
    observer.observe(section)
  }
})
</script>

<template>
  <section id="journey" class="experience" :class="{ visible: isVisible }">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-number">MY JOURNEY</h2>
        
        <h3 class="section-title">
          <span class="text-gradient">Experience & Education</span>
        </h3>
        <p class="section-description">
          A timeline of my professional journey, education, and continuous learning in web development.
        </p>
      </div>

      <!-- Filter Buttons -->
      <div class="filter-buttons">
        <button 
          class="filter-btn" 
          :class="{ active: activeFilter === 'all' }"
          @click="filterExperiences('all')"
        >
          <span class="filter-icon">⭐</span>
          <span>All</span>
        </button>
        <button 
          class="filter-btn"
          :class="{ active: activeFilter === 'work' }"
          @click="filterExperiences('work')"
        >
          <span class="filter-icon">💼</span>
          <span>Work</span>
        </button>
        <button 
          class="filter-btn"
          :class="{ active: activeFilter === 'education' }"
          @click="filterExperiences('education')"
        >
          <span class="filter-icon">🎓</span>
          <span>Education</span>
        </button>
        <button 
          class="filter-btn"
          :class="{ active: activeFilter === 'certification' }"
          @click="filterExperiences('certification')"
        >
          <span class="filter-icon">📜</span>
          <span>Certifications</span>
        </button>
      </div>

      <!-- Timeline -->
      <div class="timeline">
        <div 
          v-for="(item, index) in filteredExperiences" 
          :key="item.id"
          class="timeline-item"
          :class="[item.type, { 'left': index % 2 === 0, 'right': index % 2 !== 0 }]"
          :style="{ '--item-index': index }"
        >
          <!-- Timeline Dot -->
          <div class="timeline-dot">
            <span class="dot-icon">{{ item.icon }}</span>
          </div>

          <!-- Timeline Card -->
          <div class="timeline-card">
            <div class="card-header">
              <div class="card-title-group">
                <h4 class="card-title">{{ item.title }}</h4>
                <p class="card-organization">{{ item.organization }}</p>
              </div>
              <div class="card-meta">
                <span class="card-period">{{ item.period }}</span>
                <span class="card-location">📍 {{ item.location }}</span>
              </div>
            </div>

            <div class="card-content">
              <p class="card-description">{{ item.description }}</p>

              <!-- Technologies -->
              <div v-if="item.technologies" class="card-technologies">
                <span 
                  v-for="tech in item.technologies" 
                  :key="tech"
                  class="tech-tag"
                >
                  {{ tech }}
                </span>
              </div>

              <!-- Achievements -->
              <div v-if="item.achievements" class="card-achievements">
                <h5 class="achievements-title">Key Achievements:</h5>
                <ul class="achievements-list">
                  <li v-for="achievement in item.achievements" :key="achievement">
                    {{ achievement }}
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Download Resume Button -->
      <div class="resume-section">
        <a href="#" class="resume-btn" download>
          <span class="btn-icon">📄</span>
          <span>Download Resume</span>
        </a>
      </div>
    </div>
  </section>
</template>

<style scoped>
.experience {
  min-height: 100vh;
  padding: var(--spacing-xl) 0;
  position: relative;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1), 
              transform 1s cubic-bezier(0.4, 0, 0.2, 1);
}

.experience.visible {
  opacity: 1;
  transform: translateY(0);
}

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: var(--spacing-xl);
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

.section-title {
  font-size: var(--text-2xl);
  line-height: 1.2;
  margin: 0 0 var(--spacing-sm) 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

.text-gradient {
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
}

/* Filter Buttons */
.filter-buttons {
  display: flex;
  justify-content: center;
  gap: var(--spacing-sm);
  margin-bottom: var(--spacing-xl);
  flex-wrap: wrap;
}

.filter-btn {
  display: flex;
  align-items: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-sm) var(--spacing-lg);
  background: var(--color-bg-card);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-full);
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.filter-btn:hover {
  border-color: var(--color-accent-primary);
  color: var(--color-accent-primary);
  transform: translateY(-2px);
}

.filter-btn.active {
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-color: transparent;
  color: white;
}

.filter-icon {
  font-size: var(--text-base);
}

/* Timeline */
.timeline {
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
  padding: var(--spacing-lg) 0;
}

/* Timeline Center Line */
.timeline::before {
  content: '';
  position: absolute;
  left: 50%;
  top: 0;
  bottom: 0;
  width: 3px;
  background: linear-gradient(180deg, 
    transparent 0%,
    var(--color-accent-primary) 10%,
    var(--color-accent-secondary) 90%,
    transparent 100%
  );
  transform: translateX(-50%);
  box-shadow: 0 0 10px rgba(168, 85, 247, 0.5);
}

/* Timeline Item */
.timeline-item {
  position: relative;
  width: 50%;
  padding: var(--spacing-lg);
  opacity: 0;
  animation: slideIn 0.6s ease forwards;
  animation-delay: calc(var(--item-index) * 0.15s);
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.timeline-item.left {
  left: 0;
  padding-right: var(--spacing-xl);
}

.timeline-item.right {
  left: 50%;
  padding-left: var(--spacing-xl);
}

/* Timeline Dot */
.timeline-dot {
  position: absolute;
  width: 60px;
  height: 60px;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border: 4px solid var(--color-bg-primary);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  top: var(--spacing-lg);
  z-index: 2;
  box-shadow: 0 0 20px rgba(168, 85, 247, 0.5);
}

.timeline-item.left .timeline-dot {
  right: calc(-30px - var(--spacing-md));
}

.timeline-item.right .timeline-dot {
  left: calc(-30px - var(--spacing-md));
}

.dot-icon {
  font-size: 28px;
}

/* Timeline Card */
.timeline-card {
  background: var(--color-bg-card);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-lg);
  position: relative;
  transition: all 0.3s ease;
}

.timeline-card:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(168, 85, 247, 0.2);
}

/* Timeline Arrow */
.timeline-card::before {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
  top: 32px;
}

.timeline-item.left .timeline-card::before {
  right: -16px;
  border-width: 12px 0 12px 16px;
  border-color: transparent transparent transparent var(--color-border);
}

.timeline-item.right .timeline-card::before {
  left: -16px;
  border-width: 12px 16px 12px 0;
  border-color: transparent var(--color-border) transparent transparent;
}

/* Card Content */
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-md);
  padding-bottom: var(--spacing-md);
  border-bottom: 1px solid var(--color-border);
}

.card-title-group {
  flex: 1;
}

.card-title {
  font-size: var(--text-lg);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 var(--spacing-xs) 0;
}

.card-organization {
  font-size: var(--text-sm);
  font-weight: 600;
  color: var(--color-accent-primary);
  margin: 0;
}

.card-meta {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: var(--spacing-xs);
  font-size: var(--text-xs);
}

.card-period {
  color: var(--color-accent-secondary);
  font-weight: 600;
  padding: 4px 12px;
  background: rgba(168, 85, 247, 0.1);
  border-radius: var(--radius-full);
}

.card-location {
  color: var(--color-text-muted);
}

.card-content {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
}

.card-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0;
}

/* Technologies */
.card-technologies {
  display: flex;
  flex-wrap: wrap;
  gap: var(--spacing-xs);
}

.tech-tag {
  padding: 4px 12px;
  background: rgba(168, 85, 247, 0.1);
  border: 1px solid var(--color-accent-primary);
  border-radius: var(--radius-full);
  font-size: var(--text-xs);
  font-weight: 600;
  color: var(--color-accent-primary);
}

/* Achievements */
.card-achievements {
  background: rgba(168, 85, 247, 0.05);
  border-left: 3px solid var(--color-accent-primary);
  padding: var(--spacing-sm) var(--spacing-md);
  border-radius: var(--radius-sm);
}

.achievements-title {
  font-size: var(--text-sm);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 var(--spacing-xs) 0;
}

.achievements-list {
  margin: 0;
  padding-left: var(--spacing-md);
  list-style: none;
}

.achievements-list li {
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  line-height: 1.8;
  position: relative;
}

.achievements-list li::before {
  content: '✓';
  position: absolute;
  left: -20px;
  color: var(--color-accent-primary);
  font-weight: bold;
}

/* Resume Section */
.resume-section {
  display: flex;
  justify-content: center;
  margin-top: var(--spacing-xl);
  padding-top: var(--spacing-xl);
  border-top: 1px solid var(--color-border);
}

.resume-btn {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-sm);
  padding: var(--spacing-md) var(--spacing-xl);
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border: none;
  border-radius: var(--radius-full);
  color: white;
  font-size: var(--text-base);
  font-weight: 700;
  text-decoration: none;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.3);
}

.resume-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(168, 85, 247, 0.4);
}

.btn-icon {
  font-size: var(--text-xl);
}

/* Responsive */
@media (max-width: 768px) {
  .timeline::before {
    left: 30px;
  }

  .timeline-item {
    width: 100%;
    padding-left: 80px;
    padding-right: var(--spacing-md);
  }

  .timeline-item.left,
  .timeline-item.right {
    left: 0;
    padding-left: 80px;
    padding-right: var(--spacing-md);
  }

  .timeline-dot {
    left: 0;
    width: 50px;
    height: 50px;
  }

  .timeline-item.left .timeline-dot,
  .timeline-item.right .timeline-dot {
    left: 0;
    right: auto;
  }

  .dot-icon {
    font-size: 24px;
  }

  .timeline-card::before {
    left: -16px;
    border-width: 12px 16px 12px 0;
    border-color: transparent var(--color-border) transparent transparent;
  }

  .timeline-item.left .timeline-card::before {
    left: -16px;
    right: auto;
    border-width: 12px 16px 12px 0;
    border-color: transparent var(--color-border) transparent transparent;
  }

  .card-header {
    flex-direction: column;
    gap: var(--spacing-sm);
  }

  .card-meta {
    align-items: flex-start;
    width: 100%;
  }

  .filter-buttons {
    gap: var(--spacing-xs);
  }

  .filter-btn {
    padding: var(--spacing-xs) var(--spacing-md);
    font-size: var(--text-xs);
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: var(--text-xl);
  }

  .timeline-item {
    padding-left: 70px;
  }

  .timeline-item.left,
  .timeline-item.right {
    padding-left: 70px;
  }

  .timeline-dot {
    width: 40px;
    height: 40px;
  }

  .dot-icon {
    font-size: 20px;
  }

  .card-title {
    font-size: var(--text-base);
  }

  .tech-tag {
    font-size: 10px;
    padding: 3px 8px;
  }
}
</style>
