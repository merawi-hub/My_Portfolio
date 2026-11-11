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
    name: 'Twitter',
    url: 'https://twitter.com/yourusername',
    icon: 'M23.953 4.57a10 10 0 01-2.825.775 4.958 4.958 0 002.163-2.723c-.951.555-2.005.959-3.127 1.184a4.92 4.92 0 00-8.384 4.482C7.69 8.095 4.067 6.13 1.64 3.162a4.822 4.822 0 00-.666 2.475c0 1.71.87 3.213 2.188 4.096a4.904 4.904 0 01-2.228-.616v.06a4.923 4.923 0 003.946 4.827 4.996 4.996 0 01-2.212.085 4.936 4.936 0 004.604 3.417 9.867 9.867 0 01-6.102 2.105c-.39 0-.779-.023-1.17-.067a13.995 13.995 0 007.557 2.209c9.053 0 13.998-7.496 13.998-13.985 0-.21 0-.42-.015-.63A9.935 9.935 0 0024 4.59z',
    color: '#1DA1F2'
  }
]

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

const isValidEmail = (email: string): boolean => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email)
}

const isValidName = (name: string): boolean => {
  const nameRegex = /^[a-zA-Z\s'-]+$/
  return nameRegex.test(name)
}

const isValidSubject = (subject: string): boolean => {
  const subjectRegex = /^[a-zA-Z0-9\s,.!?'-]+$/
  return subjectRegex.test(subject)
}

const isValidMessage = (message: string): boolean => {
  const messageRegex = /^[a-zA-Z0-9\s,.!?'"()\-_:;@#$%&+=\n]+$/
  return messageRegex.test(message)
}

const validateForm = (): boolean => {
  const errors: Partial<FormData> = {}
  
  if (!formData.value.name.trim()) {
    errors.name = 'Name is required'
  } else if (formData.value.name.trim().length < 2) {
    errors.name = 'Name must be at least 2 characters'
  } else if (formData.value.name.trim().length > 50) {
    errors.name = 'Name must be less than 50 characters'
  } else if (!isValidName(formData.value.name.trim())) {
    errors.name = 'Name can only contain letters, spaces, hyphens, and apostrophes'
  }
  
  if (!formData.value.email.trim()) {
    errors.email = 'Email is required'
  } else if (!isValidEmail(formData.value.email.trim())) {
    errors.email = 'Please enter a valid email address (e.g., name@example.com)'
  } else if (formData.value.email.trim().length > 100) {
    errors.email = 'Email must be less than 100 characters'
  }
  
  if (!formData.value.subject.trim()) {
    errors.subject = 'Subject is required'
  } else if (formData.value.subject.trim().length < 5) {
    errors.subject = 'Subject must be at least 5 characters'
  } else if (formData.value.subject.trim().length > 100) {
    errors.subject = 'Subject must be less than 100 characters'
  } else if (!isValidSubject(formData.value.subject.trim())) {
    errors.subject = 'Subject contains invalid characters'
  }
  
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

const FORMSPREE_ENDPOINT = 'https://formspree.io/f/YOUR_FORM_ID'

const handleSubmit = async (e: Event) => {
  e.preventDefault()
  
  submitStatus.value = 'idle'
  statusMessage.value = ''
  
  if (!validateForm()) {
    return
  }
  
  isSubmitting.value = true
  
  try {
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
      
      formData.value = {
        name: '',
        email: '',
        subject: '',
        message: ''
      }
      formErrors.value = {}
      
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

const handleNameInput = () => {
  clearError('name')
  if (formData.value.name && !isValidName(formData.value.name)) {
    formErrors.value.name = 'Only letters, spaces, hyphens, and apostrophes allowed'
  }
}

const handleEmailInput = () => {
  clearError('email')
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
      <div class="section-header">
        <h3 class="section-title">
          <span class="text-gradient">Let's Work Together</span>
        </h3>
        <p class="section-description">
          Have a project in mind or want to collaborate? Feel free to reach out.
        </p>
      </div>

      <div class="contact-content">
        <div class="contact-info-simple">
          <div class="contact-methods">
            <a 
              href="mailto:merawikelemework5@gmail.com"
              class="contact-item"
            >
              <svg class="contact-icon" viewBox="0 0 24 24" fill="currentColor">
                <path d="M20 4H4c-1.1 0-1.99.9-1.99 2L2 18c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z" />
              </svg>
              <div class="contact-details">
                <span class="contact-label">Email</span>
                <span class="contact-value">merawikelemework5@gmail.com</span>
              </div>
            </a>

            <a 
              href="tel:+1251969644384"
              class="contact-item"
            >
              <svg class="contact-icon" viewBox="0 0 24 24" fill="currentColor">
                <path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z" />
              </svg>
              <div class="contact-details">
                <span class="contact-label">Phone</span>
                <span class="contact-value">+1251 969 644 384</span>
              </div>
            </a>
          </div>

          <div class="social-section">
            <div class="social-links">
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
    </div>
  </section>
</template>

<style scoped>
.contact {
  min-height: auto;
  height: auto;
  padding: 25px 30px;
  position: relative;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.8s ease, transform 0.8s ease;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}

.contact.visible {
  opacity: 1;
  transform: translateY(0);
}

.container {
  width: 100%;
  max-width: 850px;
}

.section-header {
  text-align: center;
  margin-bottom: 30px;
}

.section-title {
  font-size: 28px;
  line-height: 1.2;
  margin: 0 0 10px 0;
  font-weight: 700;
}

.text-gradient {
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
}

.section-description {
  font-size: 14px;
  color: var(--color-text-secondary);
  line-height: 1.5;
  margin: 0;
}

.contact-content {
  display: flex;
  justify-content: center;
}

.contact-info-simple {
  width: 100%;
}

.contact-methods {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 18px;
  margin-bottom: 30px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 18px;
  background: var(--color-bg-card);
  border: 1px solid var(--color-border);
  border-radius: 12px;
  text-decoration: none;
  transition: all 0.3s ease;
  cursor: pointer;
}

.contact-item:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(168, 85, 247, 0.2);
}

.contact-icon {
  width: 18px;
  height: 18px;
  padding: 10px;
  background: linear-gradient(135deg, var(--color-accent-primary) 0%, var(--color-accent-secondary) 100%);
  border-radius: 8px;
  color: white;
  flex-shrink: 0;
}

.contact-details {
  display: flex;
  flex-direction: column;
  gap: 3px;
  min-width: 0;
}

.contact-label {
  font-size: 11px;
  color: var(--color-text-muted);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.contact-value {
  font-size: 14px;
  color: var(--color-text-primary);
  font-weight: 600;
  word-break: break-word;
}

.social-section {
  text-align: center;
  padding-top: 25px;
  border-top: 1px solid var(--color-border);
}

.social-links {
  display: flex;
  justify-content: center;
  gap: 20px;
  flex-wrap: wrap;
}

.social-link {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  padding: 14px;
  background: var(--color-bg-secondary);
  border: 1px solid var(--color-border);
  border-radius: 12px;
  text-decoration: none;
  transition: all 0.3s ease;
  min-width: 90px;
}

.social-link:hover {
  border-color: var(--color-accent-primary);
  transform: translateY(-4px);
  box-shadow: 0 8px 20px rgba(168, 85, 247, 0.2);
}

.social-icon {
  width: 26px;
  height: 26px;
  color: var(--color-text-primary);
  transition: color 0.3s ease;
}

.social-link:hover .social-icon {
  color: var(--color-accent-primary);
}

.social-name {
  font-size: 12px;
  color: var(--color-text-secondary);
  font-weight: 600;
}

@media (max-width: 768px) {
  .contact {
    padding: 20px 25px;
  }

  .section-header {
    margin-bottom: 25px;
  }

  .section-title {
    font-size: 24px;
  }

  .contact-methods {
    grid-template-columns: 1fr;
    gap: 14px;
    margin-bottom: 25px;
  }

  .social-links {
    gap: 16px;
  }
}

@media (max-width: 480px) {
  .contact {
    padding: 18px 20px;
  }

  .section-title {
    font-size: 22px;
  }

  .contact-item {
    padding: 14px;
  }

  .social-link {
    min-width: 80px;
    padding: 12px;
  }
}
</style>
