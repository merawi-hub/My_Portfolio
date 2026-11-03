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

// Social links - Update with your real profiles
const socialLinks = [
  {
    name: 'GitHub',
    icon: '🐙',
    url: 'https://github.com/yourusername',
    color: '#333'
  },
  {
    name: 'LinkedIn',
    icon: '💼',
    url: 'https://linkedin.com/in/yourusername',
    color: '#0077B5'
  },
  {
    name: 'Twitter',
    icon: '🐦',
    url: 'https://twitter.com/yourusername',
    color: '#1DA1F2'
  },
  {
    name: 'Email',
    icon: '✉️',
    url: 'mailto:your.email@example.com',
    color: '#EA4335'
  }
]

// Contact information
const contactInfo = [
  {
    icon: '📧',
    label: 'Email',
    value: 'your.email@example.com',
    link: 'mailto:your.email@example.com'
  },
  {
    icon: '📱',
    label: 'Phone',
    value: '+1 (555) 123-4567',
    link: 'tel:+15551234567'
  },
  {
    icon: '📍',
    label: 'Location',
    value: 'City, Country',
    link: null
  }
]

// Validate email format
const isValidEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email)
}

// Validate form
const validateForm = (): boolean => {
  const errors: Partial<FormData> = {}
  
  if (!formData.value.name.trim()) {
    errors.name = 'Name is required'
  } else if (formData.value.name.trim().length < 2) {
    errors.name = 'Name must be at least 2 characters'
  }
  
  if (!formData.value.email.trim()) {
    errors.email = 'Email is required'
  } else if (!isValidEmail(formData.value.email)) {
    errors.email = 'Please enter a valid email address'
  }
  
  if (!formData.value.subject.trim()) {
    errors.subject = 'Subject is required'
  } else if (formData.value.subject.trim().length < 5) {
    errors.subject = 'Subject must be at least 5 characters'
  }
  
  if (!formData.value.message.trim()) {
    errors.message = 'Message is required'
  } else if (formData.value.message.trim().length < 10) {
    errors.message = 'Message must be at least 10 characters'
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
            <h4 class="info-title">Contact Information</h4>
            <p class="info-description">
              Feel free to reach out through any of these channels. I'm always open to discussing new projects and opportunities.
            </p>
            
            <div class="info-list">
              <a 
                v-for="info in contactInfo" 
                :key="info.label"
                :href="info.link || '#'"
                :class="['info-item', { 'no-link': !info.link }]"
                @click.prevent="info.link && window.open(info.link, '_self')"
              >
                <span class="info-icon">{{ info.icon }}</span>
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
                  <span class="social-icon">{{ social.icon }}</span>
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
                Name <span class="required">*</span>
              </label>
              <input
                id="name"
                v-model="formData.name"
                type="text"
                class="form-input"
                :class="{ 'error': formErrors.name }"
                placeholder="Your full name"
                @input="clearError('name')"
                required
              />
              <span v-if="formErrors.name" class="error-message">{{ formErrors.name }}</span>
            </div>

            <!-- Email Field -->
            <div class="form-group">
              <label for="email" class="form-label">
                Email <span class="required">*</span>
              </label>
              <input
                id="email"
                v-model="formData.email"
                type="email"
                class="form-input"
                :class="{ 'error': formErrors.email }"
                placeholder="your.email@example.com"
                @input="clearError('email')"
                required
              />
              <span v-if="formErrors.email" class="error-message">{{ formErrors.email }}</span>
            </div>

            <!-- Subject Field -->
            <div class="form-group">
              <label for="subject" class="form-label">
                Subject <span class="required">*</span>
              </label>
              <input
                id="subject"
                v-model="formData.subject"
                type="text"
                class="form-input"
                :class="{ 'error': formErrors.subject }"
                placeholder="How can I help you?"
                @input="clearError('subject')"
                required
              />
              <span v-if="formErrors.subject" class="error-message">{{ formErrors.subject }}</span>
            </div>

            <!-- Message Field -->
            <div class="form-group">
              <label for="message" class="form-label">
                Message <span class="required">*</span>
              </label>
              <textarea
                id="message"
                v-model="formData.message"
                class="form-input form-textarea"
                :class="{ 'error': formErrors.message }"
                placeholder="Tell me about your project or inquiry..."
                rows="6"
                @input="clearError('message')"
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
        <p class="footer-subtext">
          Built with Vue.js, TypeScript & Laravel
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
  font-size: 24px;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-radius: var(--radius-md);
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
  font-size: var(--text-sm);
  color: var(--color-text-primary);
  font-weight: 500;
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
  grid-template-columns: repeat(2, 1fr);
  gap: var(--spacing-sm);
}

.social-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-xs);
  padding: var(--spacing-md);
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  text-decoration: none;
  transition: all 0.2s ease;
}

.social-link:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(168, 85, 247, 0.2);
}

.social-icon {
  font-size: 32px;
}

.social-name {
  font-size: var(--text-xs);
  color: var(--color-text-secondary);
  font-weight: 600;
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
