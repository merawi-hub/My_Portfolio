<script setup lang="ts">
import { ref, onMounted } from 'vue'

const isVisible = ref(false)
const isSubmitting = ref(false)
const submitStatus = ref<'idle' | 'success' | 'error'>('idle')
const statusMessage = ref('')

interface FormData {
  name: string
  email: string
  subject: string
  message: string
}

const formData = ref<FormData>({
  name: '',
  email: '',
  subject: '',
  message: ''
})

const formErrors = ref<Partial<FormData>>({})

// Social links with real SVG icons
const socialLinks = [
  {
    name: 'GitHub',
    url: 'https://github.com/merawi-hub',
    icon: 'M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z',
    color: '#333333'
  },
  {
    name: 'LinkedIn',
    url: 'https://linkedin.com/in/merawi-kelemework',
    icon: 'M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z',
    color: '#0077B5'
  },
  {
    name: 'Instagram',
    url: 'https://instagram.com/merawi_kelemework', // Update with your Instagram username
    icon: 'M12 0C8.74 0 8.333.015 7.053.072 5.775.132 4.905.333 4.14.63c-.789.306-1.459.717-2.126 1.384S.935 3.35.63 4.14C.333 4.905.131 5.775.072 7.053.012 8.333 0 8.74 0 12s.015 3.667.072 4.947c.06 1.277.261 2.148.558 2.913.306.788.717 1.459 1.384 2.126.667.666 1.336 1.079 2.126 1.384.766.296 1.636.499 2.913.558C8.333 23.988 8.74 24 12 24s3.667-.015 4.947-.072c1.277-.06 2.148-.262 2.913-.558.788-.306 1.459-.718 2.126-1.384.666-.667 1.079-1.335 1.384-2.126.296-.765.499-1.636.558-2.913.06-1.28.072-1.687.072-4.947s-.015-3.667-.072-4.947c-.06-1.277-.262-2.149-.558-2.913-.306-.789-.718-1.459-1.384-2.126C21.319 1.347 20.651.935 19.86.63c-.765-.297-1.636-.499-2.913-.558C15.667.012 15.26 0 12 0zm0 2.16c3.203 0 3.585.016 4.85.071 1.17.055 1.805.249 2.227.415.562.217.96.477 1.382.896.419.42.679.819.896 1.381.164.422.36 1.057.413 2.227.057 1.266.07 1.646.07 4.85s-.015 3.585-.074 4.85c-.061 1.17-.256 1.805-.421 2.227-.224.562-.479.96-.899 1.382-.419.419-.824.679-1.38.896-.42.164-1.065.36-2.235.413-1.274.057-1.649.07-4.859.07-3.211 0-3.586-.015-4.859-.074-1.171-.061-1.816-.256-2.236-.421-.569-.224-.96-.479-1.379-.899-.421-.419-.69-.824-.9-1.38-.165-.42-.359-1.065-.42-2.235-.045-1.26-.061-1.649-.061-4.844 0-3.196.016-3.586.061-4.861.061-1.17.255-1.814.42-2.234.21-.57.479-.96.9-1.381.419-.419.81-.689 1.379-.898.42-.166 1.051-.361 2.221-.421 1.275-.045 1.65-.06 4.859-.06l.045.03zm0 3.678c-3.405 0-6.162 2.76-6.162 6.162 0 3.405 2.76 6.162 6.162 6.162 3.405 0 6.162-2.76 6.162-6.162 0-3.405-2.76-6.162-6.162-6.162zM12 16c-2.21 0-4-1.79-4-4s1.79-4 4-4 4 1.79 4 4-1.79 4-4 4zm7.846-10.405c0 .795-.646 1.44-1.44 1.44-.795 0-1.44-.646-1.44-1.44 0-.794.646-1.439 1.44-1.439.793-.001 1.44.645 1.44 1.439z',
    color: '#E4405F'
  },
  {
    name: 'WhatsApp',
    url: 'https://wa.me/1251969644384', // Format: country code + number (no + or spaces)
    icon: 'M17.472 14.382c-.297-.149-1.758-.867-2.03-.967-.273-.099-.471-.148-.67.15-.197.297-.767.966-.94 1.164-.173.199-.347.223-.644.075-.297-.15-1.255-.463-2.39-1.475-.883-.788-1.48-1.761-1.653-2.059-.173-.297-.018-.458.13-.606.134-.133.298-.347.446-.52.149-.174.198-.298.298-.497.099-.198.05-.371-.025-.52-.075-.149-.669-1.612-.916-2.207-.242-.579-.487-.5-.669-.51-.173-.008-.371-.01-.57-.01-.198 0-.52.074-.792.372-.272.297-1.04 1.016-1.04 2.479 0 1.462 1.065 2.875 1.213 3.074.149.198 2.096 3.2 5.077 4.487.709.306 1.262.489 1.694.625.712.227 1.36.195 1.871.118.571-.085 1.758-.719 2.006-1.413.248-.694.248-1.289.173-1.413-.074-.124-.272-.198-.57-.347m-5.421 7.403h-.004a9.87 9.87 0 01-5.031-1.378l-.361-.214-3.741.982.998-3.648-.235-.374a9.86 9.86 0 01-1.51-5.26c.001-5.45 4.436-9.884 9.888-9.884 2.64 0 5.122 1.03 6.988 2.898a9.825 9.825 0 012.893 6.994c-.003 5.45-4.437 9.884-9.885 9.884m8.413-18.297A11.815 11.815 0 0012.05 0C5.495 0 .16 5.335.157 11.892c0 2.096.547 4.142 1.588 5.945L.057 24l6.305-1.654a11.882 11.882 0 005.683 1.448h.005c6.554 0 11.89-5.335 11.893-11.893a11.821 11.821 0 00-3.48-8.413Z',
    color: '#25D366'
  },
  {
    name: 'Twitter',
    url: 'https://twitter.com/yourusername', // Update with your Twitter username
    icon: 'M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z',
    color: '#1DA1F2'
  },
  {
    name: 'Email',
    url: 'mailto:merawikelemework5@gmail.com',
    icon: 'M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z',
    color: '#EA4335'
  }
]

// Contact information with SVG icons
const contactInfo = [
  {
    icon: 'M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z',
    label: 'Email',
    value: 'merawikelemework5@gmail.com',
    link: 'mailto:merawikelemework5@gmail.com'
  },
  {
    icon: 'M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z',
    label: 'Phone',
    value: '+1251 969 644 384',
    link: 'tel:+1251969644384'
  },
  {
    icon: 'M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z',
    label: 'Location',
    value: 'Addis Ababa, Ethiopia',
    link: null
  }
]

// Validate email format
const isValidEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email)
}

// Validate name (only letters, spaces, hyphens, and apostrophes)
const isValidName = (name: string): boolean => {
  const nameRegex = /^[a-zA-Z\s'-]+$/
  return nameRegex.test(name)
}

// Validate subject (letters, numbers, spaces, and basic punctuation)
const isValidSubject = (subject: string): boolean => {
  const subjectRegex = /^[a-zA-Z0-9\s,.!?'-]+$/
  return subjectRegex.test(subject)
}

// Validate message (letters, numbers, spaces, and common punctuation)
const isValidMessage = (message: string): boolean => {
  const messageRegex = /^[a-zA-Z0-9\s,.!?'"()\-_:;@#$%&+=\n]+$/
  return messageRegex.test(message)
}

// Validate form
const validateForm = (): boolean => {
  const errors: Partial<FormData> = {}
  
  // Name validation
  if (!formData.value.name.trim()) {
    errors.name = 'Name is required'
  } else if (formData.value.name.trim().length < 2) {
    errors.name = 'Name must be at least 2 characters'
  } else if (formData.value.name.trim().length > 50) {
    errors.name = 'Name must be less than 50 characters'
  } else if (!isValidName(formData.value.name.trim())) {
    errors.name = 'Name can only contain letters, spaces, hyphens, and apostrophes'
  }
  
  // Email validation
  if (!formData.value.email.trim()) {
    errors.email = 'Email is required'
  } else if (!isValidEmail(formData.value.email.trim())) {
    errors.email = 'Please enter a valid email address (e.g., name@example.com)'
  } else if (formData.value.email.trim().length > 100) {
    errors.email = 'Email must be less than 100 characters'
  }
  
  // Subject validation
  if (!formData.value.subject.trim()) {
    errors.subject = 'Subject is required'
  } else if (formData.value.subject.trim().length < 5) {
    errors.subject = 'Subject must be at least 5 characters'
  } else if (formData.value.subject.trim().length > 100) {
    errors.subject = 'Subject must be less than 100 characters'
  } else if (!isValidSubject(formData.value.subject.trim())) {
    errors.subject = 'Subject contains invalid characters'
  }
  
  // Message validation
  if (!formData.value.message.trim()) {
    errors.message = 'Message is required'
  } else if (formData.value.message.trim().length < 10) {
    errors.message = 'Message must be at least 10 characters'
  } else if (formData.value.message.trim().length > 1000) {
    errors.message = 'Message must be less than 1000 characters'
  } else if (!isValidMessage(formData.value.message.trim())) {
    errors.message = 'Message contains invalid characters'
  }
  
  formErrors.value = errors
  return Object.keys(errors).length === 0
}

// Handle form submission using Formspree
// SETUP INSTRUCTIONS:
// 1. Go to https://formspree.io/
// 2. Sign up for free account
// 3. Create a new form and get your form endpoint
// 4. Replace 'YOUR_FORM_ID' below with your actual form ID
const FORMSPREE_ENDPOINT = 'https://formspree.io/f/YOUR_FORM_ID'

const handleSubmit = async (e: Event) => {
  e.preventDefault()
  
  // Reset status
  submitStatus.value = 'idle'
  statusMessage.value = ''
  
  // Validate form
  if (!validateForm()) {
    return
  }
  
  isSubmitting.value = true
  
  try {
    // Send to Formspree
    const response = await fetch(FORMSPREE_ENDPOINT, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify(formData.value)
    })
    
    const data = await response.json()
    
    if (response.ok) {
      submitStatus.value = 'success'
      statusMessage.value = 'Thank you for your message! I\'ll get back to you soon.'
      
      // Reset form
      formData.value = {
        name: '',
        email: '',
        subject: '',
        message: ''
      }
      formErrors.value = {}
      
      // Clear success message after 5 seconds
      setTimeout(() => {
        submitStatus.value = 'idle'
        statusMessage.value = ''
      }, 5000)
    } else {
      throw new Error(data.error || 'Failed to send message')
    }
  } catch (error) {
    submitStatus.value = 'error'
    statusMessage.value = 'Failed to send message. Please try again or contact me directly via email.'
    console.error('Contact form error:', error)
  } finally {
    isSubmitting.value = false
  }
}

// Clear error when user starts typing and validate input
const handleNameInput = () => {
  clearError('name')
  // Real-time validation feedback
  if (formData.value.name && !isValidName(formData.value.name)) {
    formErrors.value.name = 'Only letters, spaces, hyphens, and apostrophes allowed'
  }
}

const handleEmailInput = () => {
  clearError('email')
  // Real-time validation feedback for format
  if (formData.value.email && formData.value.email.length > 5 && !isValidEmail(formData.value.email)) {
    formErrors.value.email = 'Please enter a valid email format'
  }
}

const handleSubjectInput = () => {
  clearError('subject')
  if (formData.value.subject && !isValidSubject(formData.value.subject)) {
    formErrors.value.subject = 'Subject contains invalid characters'
  }
}

const handleMessageInput = () => {
  clearError('message')
  if (formData.value.message && !isValidMessage(formData.value.message)) {
    formErrors.value.message = 'Message contains invalid characters'
  }
}

// Clear error when user starts typing
const clearError = (field: keyof FormData) => {
  if (formErrors.value[field]) {
    delete formErrors.value[field]
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

  const section = document.querySelector('.contact')
  if (section) {
    observer.observe(section)
  }
})
</script>

<template>
  <section id="contact" class="contact" :class="{ visible: isVisible }">
    <div class="container">
      <!-- Section Header -->
      <div class="section-header">
        <h2 class="section-number">GET IN TOUCH</h2>
        
        <h3 class="section-title">
          <span class="text-gradient">Let's Work Together</span>
        </h3>
        <p class="section-description">
          Have a project in mind or want to collaborate? I'd love to hear from you. 
          Send me a message and I'll respond as soon as possible.
        </p>
      </div>

      <div class="contact-content">
        <!-- Contact Information -->
        <div class="contact-info-section">
          <div class="info-card">
            <h4 class="info-title">Let's build something special</h4>
            <p class="info-description">
              Feel free to reach out to me. I'm always open to discussing new projects and opportunities.
            </p>
            
            <div class="info-list">
              <a 
                v-for="info in contactInfo" 
                :key="info.label"
                :href="info.link || '#'"
                :class="['info-item', { 'no-link': !info.link }]"
                @click.prevent="info.link && window.open(info.link, '_self')"
              >
                <svg class="info-icon" viewBox="0 0 24 24" fill="currentColor">
                  <path :d="info.icon" />
                </svg>
                <div class="info-details">
                  <span class="info-label">{{ info.label }}</span>
                  <span class="info-value">{{ info.value }}</span>
                </div>
              </a>
            </div>

            <!-- Social Links -->
            <div class="social-links">
              <h5 class="social-title">Connect with me</h5>
              <div class="social-icons">
                <a 
                  v-for="social in socialLinks" 
                  :key="social.name"
                  :href="social.url"
                  :title="social.name"
                  class="social-link"
                  target="_blank"
                  rel="noopener noreferrer"
                >
                  <svg class="social-icon" viewBox="0 0 24 24" fill="currentColor">
                    <path :d="social.icon" />
                  </svg>
                  <span class="social-name">{{ social.name }}</span>
                </a>
              </div>
            </div>
          </div>
        </div>

        <!-- Contact Form -->
        <div class="contact-form-section">
          <form @submit="handleSubmit" class="contact-form" novalidate>
            <!-- Name Field -->
            <div class="form-group">
              <label for="name" class="form-label">
                Name <span class="required"></span>
              </label>
              <input
                id="name"
                v-model="formData.name"
                type="text"
                class="form-input"
                :class="{ 'error': formErrors.name }"
                placeholder="Your full name"
                @input="handleNameInput"
                maxlength="50"
                required
              />
              <span v-if="formErrors.name" class="error-message">{{ formErrors.name }}</span>
            </div>

            <!-- Email Field -->
            <div class="form-group">
              <label for="email" class="form-label">
                Email <span class="required"></span>
              </label>
              <input
                id="email"
                v-model="formData.email"
                type="email"
                class="form-input"
                :class="{ 'error': formErrors.email }"
                placeholder="your.email@example.com"
                @input="handleEmailInput"
                maxlength="100"
                required
              />
              <span v-if="formErrors.email" class="error-message">{{ formErrors.email }}</span>
            </div>

            <!-- Subject Field -->
            <div class="form-group">
              <label for="subject" class="form-label">
                Subject <span class="required"></span>
              </label>
              <input
                id="subject"
                v-model="formData.subject"
                type="text"
                class="form-input"
                :class="{ 'error': formErrors.subject }"
                placeholder="How can I help you?"
                @input="handleSubjectInput"
                maxlength="100"
                required
              />
              <span v-if="formErrors.subject" class="error-message">{{ formErrors.subject }}</span>
            </div>

            <!-- Message Field -->
            <div class="form-group">
              <label for="message" class="form-label">
                Message <span class="required"></span>
              </label>
              <textarea
                id="message"
                v-model="formData.message"
                class="form-input form-textarea"
                :class="{ 'error': formErrors.message }"
                placeholder="Tell me about your project or inquiry..."
                rows="6"
                @input="handleMessageInput"
                maxlength="1000"
                required
              ></textarea>
              <span v-if="formErrors.message" class="error-message">{{ formErrors.message }}</span>
            </div>

            <!-- Status Message -->
            <div v-if="statusMessage" class="status-message" :class="submitStatus">
              {{ statusMessage }}
            </div>

            <!-- Submit Button -->
            <button 
              type="submit" 
              class="submit-btn"
              :disabled="isSubmitting"
            >
              <span v-if="!isSubmitting">
                <span class="btn-icon">📨</span>
                <span>Send Message</span>
              </span>
              <span v-else class="loading">
                <span class="spinner"></span>
                <span>Sending...</span>
              </span>
            </button>
          </form>
        </div>
      </div>

      <!-- Footer -->
      <div class="contact-footer">
        <p class="footer-text">
          © {{ new Date().getFullYear() }} Merawi Ayele. All rights reserved.
        </p>
        
      </div>
    </div>
  </section>
</template>

<style scoped>
.contact {
  min-height: 100vh;
  padding: var(--spacing-xl) 0;
  position: relative;
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1), 
              transform 1s cubic-bezier(0.4, 0, 0.2, 1);
}

.contact.visible {
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
  text-align: center;
}

/* Contact Content */
.contact-content {
  display: grid;
  grid-template-columns: 1fr 1.5fr;
  gap: var(--spacing-2xl);
  max-width: 1400px;
  margin: 0 auto;
}

/* Contact Information */
.contact-info-section {
  display: flex;
  flex-direction: column;
}

.info-card {
  background: var(--color-bg-card);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
  height: fit-content;
  position: sticky;
  top: 100px;
}

.info-title {
  font-size: var(--text-xl);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 var(--spacing-sm) 0;
}

.info-description {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  line-height: 1.6;
  margin: 0 0 var(--spacing-lg) 0;
}

.info-list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-md);
  margin-bottom: var(--spacing-xl);
}

.info-item {
  display: flex;
  align-items: center;
  gap: var(--spacing-md);
  padding: var(--spacing-md);
  background: rgba(168, 85, 247, 0.05);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  text-decoration: none;
  transition: all 0.2s ease;
}

.info-item .info-icon {
  width: 24px;
  height: 24px;
  padding: 8px;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-radius: var(--radius-md);
  color: white;
  flex-shrink: 0;
}

.info-item:not(.no-link) {
  cursor: pointer;
}

.info-item:not(.no-link):hover {
  border-color: var(--color-accent-primary);
  transform: translateX(4px);
}

.info-item.no-link {
  cursor: default;
}

.info-icon {
  width: 24px;
  height: 24px;
  color: white;
  flex-shrink: 0;
}

.info-details {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.info-label {
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.info-value {
  font-size: var(--text-base);
  color: var(--color-text-primary);
  font-weight: 600;
  letter-spacing: 0.02em;
}

/* Social Links */
.social-links {
  padding-top: var(--spacing-lg);
  border-top: 1px solid var(--color-border);
}

.social-title {
  font-size: var(--text-sm);
  font-weight: 700;
  color: var(--color-text-primary);
  margin: 0 0 var(--spacing-md) 0;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.social-icons {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--spacing-sm);
}

.social-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-md) var(--spacing-sm);
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  text-decoration: none;
  transition: all 0.2s ease;
  min-height: 80px;
}

.social-link:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.2);
}

.social-icon {
  width: 24px;
  height: 24px;
  color: var(--color-text-primary);
  transition: color 0.2s ease;
}

.social-link:hover .social-icon {
  color: var(--color-accent-primary);
}

.social-name {
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  font-weight: 600;
  text-align: center;
}

/* Contact Form */
.contact-form-section {
  background: var(--color-bg-card);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: var(--spacing-xl);
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-lg);
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-xs);
}

.form-label {
  font-size: var(--text-sm);
  font-weight: 600;
  color: var(--color-text-primary);
}

.required {
  color: var(--color-accent-primary);
}

.form-input {
  padding: var(--spacing-md);
  background: var(--color-bg-secondary);
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  color: var(--color-text-primary);
  font-size: var(--text-sm);
  font-family: inherit;
  transition: all 0.2s ease;
  outline: none;
}

.form-input:focus {
  border-color: var(--color-accent-primary);
  box-shadow: 0 0 0 3px rgba(168, 85, 247, 0.1);
}

.form-input.error {
  border-color: #ef4444;
}

.form-input::placeholder {
  color: var(--color-text-muted);
}

.form-textarea {
  resize: vertical;
  min-height: 150px;
  max-height: 300px;
  line-height: 1.6;
}

.error-message {
  font-size: var(--text-xs);
  color: #ef4444;
  font-weight: 500;
}

/* Status Message */
.status-message {
  padding: var(--spacing-md);
  border-radius: var(--radius-md);
  font-size: var(--text-sm);
  font-weight: 600;
  text-align: center;
}

.status-message.success {
  background: rgba(34, 197, 94, 0.1);
  border: 1px solid #22c55e;
  color: #22c55e;
}

.status-message.error {
  background: rgba(239, 68, 68, 0.1);
  border: 1px solid #ef4444;
  color: #ef4444;
}

/* Submit Button */
.submit-btn {
  padding: var(--spacing-md) var(--spacing-xl);
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border: none;
  border-radius: var(--radius-full);
  color: white;
  font-size: var(--text-base);
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-sm);
}

.submit-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(168, 85, 247, 0.4);
}

.submit-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.btn-icon {
  font-size: var(--text-xl);
}

.loading {
  display: flex;
  align-items: center;
  gap: var(--spacing-sm);
}

.spinner {
  width: 16px;
  height: 16px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 0.6s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Contact Footer */
.contact-footer {
  margin-top: var(--spacing-2xl);
  padding-top: var(--spacing-xl);
  border-top: 1px solid var(--color-border);
  text-align: center;
}

.footer-text {
  font-size: var(--text-sm);
  color: var(--color-text-secondary);
  margin: 0 0 var(--spacing-xs) 0;
}

.footer-subtext {
  font-size: var(--text-xs);
  color: var(--color-text-muted);
  margin: 0;
}

/* Responsive */
@media (max-width: 1024px) {
  .contact-content {
    grid-template-columns: 1fr;
    gap: var(--spacing-xl);
  }

  .info-card {
    position: static;
  }
}

@media (max-width: 768px) {
  .section-title {
    font-size: var(--text-xl);
  }

  .info-card,
  .contact-form-section {
    padding: var(--spacing-lg);
  }

  .social-icons {
    grid-template-columns: repeat(2, 1fr);
  }

  .contact-footer {
    margin-top: var(--spacing-xl);
  }
}

@media (max-width: 480px) {
  .contact {
    padding: var(--spacing-lg) 0;
  }

  .info-card,
  .contact-form-section {
    padding: var(--spacing-md);
  }

  .social-icons {
    grid-template-columns: 1fr;
  }

  .form-input {
    padding: var(--spacing-sm);
  }
}
</style>
