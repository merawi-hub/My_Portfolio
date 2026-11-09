<script setup lang="ts">
import { ref } from 'vue'

const emit = defineEmits(['close-interests'])

const interests = ref([
  {
    id: 1,
    title: 'Writing Poem',
    image: '/images/poetry.jpg',
    description: 'Turning feelings into lines, capturing raw moments before they slip away.'
  },
  {
    id: 2,
    title: 'Reading Books',
    image: '/images/reading.jpg',
    description: 'Getting lost in stories and ideas that challenge how I see things.'
  },
  {
    id: 3,
    title: 'Touring',
    image: '/images/touring.jpg',
    description: 'Wandering through Ethiopia\'s hidden corners, finding beauty in unexpected places.'
  },
  {
    id: 4,
    title: 'ቡና',
    image: '/images/coffee.jpg',
    description: 'Those slow coffee moments where good conversations happen and time doesn\'t matter.'
  }
])

const handleBack = (e: Event) => {
  e.preventDefault()
  emit('close-interests')
  // Scroll to the interests button after a small delay to ensure About is rendered
  setTimeout(() => {
    const button = document.getElementById('interests-button')
    if (button) {
      button.scrollIntoView({ behavior: 'smooth', block: 'center' })
    }
  }, 100)
}
</script>

<template>
  <section id="interests" class="interests">
    <div class="container">
      <!-- Back Button -->
      <a href="#about" @click="handleBack" class="back-button">
        <span>Back</span>
      </a>

      <!-- Section Header -->
      <div id="interests-header" class="section-header">
        <h2 class="section-number">😎✌️ WHEN I'M NOT CODING</h2>
        <h3 class="section-title">
          Things that keep me <span class="text-gradient">alive & inspired</span>
        </h3>
        <p class="section-description">
          Life's better with coffee, books, wandering around, and putting feelings into words — <span class="libra-text">typical Libra things</span> 🤓♎
        </p>
      </div>

      <!-- Flip Cards Grid -->
      <div class="cards-grid">
        <div 
          v-for="interest in interests" 
          :key="interest.id"
          class="flip-card"
        >
          <div class="flip-card-inner">
            <!-- Front Face -->
            <div class="flip-card-front">
              <div class="card-icon-wrapper">
                <!-- Different icons for each card -->
                <div class="card-icon" v-if="interest.id === 1">📝</div>
                <div class="card-icon" v-else-if="interest.id === 2">📚</div>
                <div class="card-icon" v-else-if="interest.id === 3">🗺️</div>
                <div class="card-icon" v-else>☕</div>
              </div>
              <h4 class="card-title">{{ interest.title }}</h4>
              <div class="card-decoration"></div>
            </div>

            <!-- Back Face -->
            <div class="flip-card-back">
              <div class="card-image">
                <img 
                  :src="interest.image" 
                  :alt="interest.title"
                  class="interest-image"
                />
              </div>
              <div class="card-content">
                <h4 class="card-title-back">{{ interest.title }}</h4>
                <p class="card-description">{{ interest.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.interests {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  padding: 100px 20px 60px;
  background: #0a0e1a;
  overflow-y: auto;
  overflow-x: hidden;
  z-index: 9999;
  animation: fadeInSection 0.6s cubic-bezier(0.4, 0, 0.2, 1);
}

.interests::-webkit-scrollbar {
  width: 8px;
}

.interests::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.02);
}

.interests::-webkit-scrollbar-thumb {
  background: rgba(168, 85, 247, 0.4);
  border-radius: 4px;
}

.interests::-webkit-scrollbar-thumb:hover {
  background: rgba(168, 85, 247, 0.6);
}

@keyframes fadeInSection {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Back Button */
.back-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-sm) var(--spacing-xl);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.2) 0%, rgba(139, 92, 246, 0.1) 100%);
  border: 2px solid var(--color-accent-primary);
  border-radius: var(--radius-full);
  color: var(--color-text-primary);
  font-size: var(--text-base);
  font-weight: 600;
  text-decoration: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  margin-bottom: var(--spacing-lg);
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.3);
  animation: glow 2s ease-in-out infinite;
}

@keyframes glow {
  0%, 100% {
    box-shadow: 0 4px 15px rgba(168, 85, 247, 0.3);
  }
  50% {
    box-shadow: 0 6px 25px rgba(168, 85, 247, 0.6);
  }
}

.back-button:hover {
  border-color: var(--color-accent-primary);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.3) 0%, rgba(139, 92, 246, 0.2) 100%);
  transform: translateY(-3px);
  box-shadow: 0 8px 30px rgba(168, 85, 247, 0.5);
}

.back-button:active {
  transform: translateY(-1px);
  box-shadow: 0 4px 15px rgba(168, 85, 247, 0.4);
}

/* Section Header */
.section-header {
  text-align: center;
  margin-bottom: var(--spacing-lg);
  max-width: 600px;
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
  font-size: var(--text-4xl);
  line-height: 1.2;
  margin: 0 0 var(--spacing-md) 0;
  color: var(--color-text-primary);
  font-weight: 700;
}

.section-description {
  font-size: var(--text-lg);
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0;
  text-align: center;
}

.libra-text {
  color: var(--color-accent-primary);
  font-weight: 600;
  animation: shine 2s ease-in-out infinite;
  background: linear-gradient(90deg, 
    var(--color-accent-primary) 0%, 
    #c084fc 50%, 
    var(--color-accent-primary) 100%
  );
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

@keyframes shine {
  0%, 100% {
    background-position: 0% center;
  }
  50% {
    background-position: 100% center;
  }
}

/* Cards Grid */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: var(--spacing-lg);
  max-width: 1400px;
  margin: 0 auto;
}

/* Flip Card */
.flip-card {
  height: 400px;
  perspective: 1000px;
  position: relative;
}

.flip-card-inner {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  transform-style: preserve-3d;
}

/* Flip on HOVER instead of click */
.flip-card:hover .flip-card-inner {
  transform: rotateY(180deg);
}

.flip-card-front,
.flip-card-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: var(--radius-lg);
  border: 1px solid var(--color-border);
  overflow: hidden;
}

/* Front Face */
.flip-card-front {
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.1) 0%, rgba(139, 92, 246, 0.05) 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-md);
  padding: var(--spacing-xl);
  transition: all var(--transition-base);
}

.flip-card:hover .flip-card-front {
  border-color: var(--color-accent-primary);
}

/* Front Face */
.flip-card-front {
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.15) 0%, rgba(139, 92, 246, 0.08) 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-lg);
  padding: var(--spacing-xl);
  transition: all var(--transition-base);
  position: relative;
  overflow: hidden;
}

.flip-card:hover .flip-card-front {
  border-color: var(--color-accent-primary);
  background: linear-gradient(135deg, rgba(168, 85, 247, 0.25) 0%, rgba(139, 92, 246, 0.15) 100%);
}

.card-icon-wrapper {
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(168, 85, 247, 0.2);
  border-radius: 50%;
  transition: all var(--transition-base);
}

.flip-card:hover .card-icon-wrapper {
  transform: scale(1.1) rotate(5deg);
  background: rgba(168, 85, 247, 0.3);
}

.card-icon {
  font-size: 56px;
  animation: float 3s ease-in-out infinite;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
}

@keyframes float {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.card-title {
  font-size: var(--text-2xl);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0;
  text-align: center;
  position: relative;
  z-index: 1;
}

.card-decoration {
  position: absolute;
  bottom: -50px;
  right: -50px;
  width: 150px;
  height: 150px;
  background: radial-gradient(circle, rgba(168, 85, 247, 0.2) 0%, transparent 70%);
  border-radius: 50%;
  transition: all var(--transition-base);
}

.flip-card:hover .card-decoration {
  transform: scale(1.5);
  opacity: 0.5;
}

.card-hint {
  font-size: var(--text-sm);
  color: var(--color-text-muted);
  margin: 0;
}

/* Back Face */
.flip-card-back {
  background: var(--color-bg-card);
  transform: rotateY(180deg);
  display: flex;
  flex-direction: column;
}

.card-image {
  height: 200px;
  overflow: hidden;
  background: var(--color-bg-tertiary);
}

.interest-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}

.card-content {
  padding: var(--spacing-lg);
  display: flex;
  flex-direction: column;
  gap: var(--spacing-sm);
  flex: 1;
}

.card-title-back {
  font-size: var(--text-xl);
  font-weight: 700;
  color: var(--color-accent-primary);
  margin: 0;
}

.card-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0;
}

/* Responsive */
@media (max-width: 1200px) {
  .cards-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .section-title {
    font-size: var(--text-3xl);
  }

  .cards-grid {
    grid-template-columns: 1fr;
    gap: var(--spacing-lg);
  }

  .flip-card {
    height: 320px;
  }

  .card-image {
    height: 160px;
  }
}
</style>
