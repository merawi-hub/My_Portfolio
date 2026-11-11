<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)

onMounted(() => {
  setTimeout(() => {
    isVisible.value = true
  }, 100)
})
</script>

<template>
  <section id="home" class="hero" :class="{ visible: isVisible }">
    <div class="container">
      <div class="hero-name">
        <h2 class="name-greeting">Hi, I'm</h2>
        <h1 class="name-title">Merawi Ayele</h1>
      </div>

      <div class="hero-tagline">
        <h3 class="tagline-text">
          BUILDING INNOVATIVE SOLUTIONS WITH
        </h3>
        <div class="tagline-highlight">MODERN TECHNOLOGIES</div>
      </div>

      <div class="laptop-container">
        <img src="/images/profile.jpg" alt="Merawi Ayele - Full Stack Developer" class="laptop-screen" />
      </div>
    </div>
  </section>
</template>

<style scoped>
.hero {
  min-height: auto;
  height: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px 20px 30px;
  position: relative;
  opacity: 0;
  transform: translateY(20px);
  transition: all 0.8s ease;
  overflow: hidden;
}

.hero.visible {
  opacity: 1;
  transform: translateY(0);
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  text-align: center;
  width: 100%;
  max-width: 1200px;
  padding: 0 20px;
}

.hero-name {
  margin-bottom: 0;
}

.name-greeting {
  font-size: 16px;
  font-weight: 400;
  color: rgba(255, 255, 255, 0.6);
  margin: 0 0 4px 0;
  letter-spacing: 0.02em;
}

.name-title {
  font-size: 38px;
  font-weight: 700;
  color: rgb(90, 140, 140);
  margin: 0 0 4px 0;
  letter-spacing: 0.02em;
  text-shadow: 0 2px 20px rgba(90, 140, 140, 0.3);
}

.name-subtitle {
  font-size: 18px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.8);
  margin: 0;
  letter-spacing: 0.03em;
}

.hero-tagline {
  max-width: 800px;
  position: relative;
  z-index: 10;
}

.tagline-text {
  font-size: 16px;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.6);
  letter-spacing: 0.05em;
  line-height: 1.4;
  margin: 0 0 8px 0;
}

.tagline-highlight {
  display: inline-block;
  font-size: 18px;
  font-weight: 700;
  color: white;
  background: rgba(90, 140, 140, 0.3);
  padding: 6px 20px;
  border-radius: 8px;
  letter-spacing: 0.05em;
}

.laptop-container {
  position: relative;
  width: 100%;
  max-width: 500px;
  perspective: 1000px;
  z-index: 1;
}

.laptop-screen {
  width: 100%;
  height: auto;
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
  transition: transform 0.3s ease;
  display: block;
}

.laptop-screen:hover {
  transform: translateY(-8px);
}

@media (max-width: 768px) {
  .hero {
    padding: 25px 20px 40px;
  }

  .name-title {
    font-size: 34px;
  }

  .name-subtitle {
    font-size: 18px;
  }

  .tagline-text {
    font-size: 16px;
  }
  
  .tagline-highlight {
    font-size: 18px;
  }

  .laptop-container {
    max-width: 90%;
  }
}

@media (max-width: 480px) {
  .name-greeting {
    font-size: 15px;
  }

  .name-title {
    font-size: 26px;
  }

  .name-subtitle {
    font-size: 15px;
  }

  .tagline-text,
  .tagline-highlight {
    font-size: 15px;
  }
}
</style>
