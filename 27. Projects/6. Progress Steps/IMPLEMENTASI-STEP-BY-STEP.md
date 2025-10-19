# 📋 Implementasi Step-by-Step: Progress Steps Vue.js

## 🎯 Panduan Lengkap Dari Nol Hingga Production

### 📝 Overview
Panduan ini akan memandu Anda langkah demi langkah dalam membuat aplikasi Progress Steps yang lengkap dengan Vue.js 3, mulai dari setup project hingga deployment.

---

## 🔥 Langkah 1: Persiapan Environment

### 1.1 Install Node.js & npm
```bash
# Cek versi Node.js (minimal 14)
node --version

# Cek versi npm
npm --version

# Jika belum ada, download dari https://nodejs.org/
```

### 1.2 Pilih Development Tool
**Opsi A: Vite (Recommended - Lebih Cepat)**
```bash
npm create vue@latest progress-steps-project
```

**Opsi B: Vue CLI (Traditional)**
```bash
npm install -g @vue/cli
vue create progress-steps-project
```

### 1.3 Setup Project
```bash
# Masuk ke folder project
cd progress-steps-project

# Install dependencies
npm install

# Start development server
npm run dev
```

---

## 🏗️ Langkah 2: Struktur Project Awal

### 2.1 Bersihkan File Default
**Hapus file yang tidak diperlukan:**
- `src/components/HelloWorld.vue`
- `src/assets/` (kosongkan)
- `src/components/TheWelcome.vue`
- `src/components/icons/`

### 2.2 Struktur Folder Baru
```
src/
├── App.vue                 # Main application component
├── main.js                 # Application entry point
├── components/
│   ├── ProgressSteps.vue   # Progress steps component
│   └── StepContent.vue     # Content untuk setiap step
├── assets/
│   └── styles/
│       └── main.css        # Global styles
└── utils/
    └── helpers.js          # Helper functions
```

---

## 🧩 Langkah 3: Membuat Komponen Dasar

### 3.1 Update main.js
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import './assets/styles/main.css'

createApp(App).mount('#app')
```

### 3.2 Membuat ProgressSteps.vue (Basic Version)
```vue
<template>
  <div class="progress-steps">
    <h1>Progress Steps</h1>

    <!-- Progress Bar Visual -->
    <div class="progress-bar">
      <div
        v-for="(step, index) in steps"
        :key="index"
        :class="getStepClass(index)"
        class="step"
        @click="goToStep(index)"
      >
        <span class="step-number">{{ index + 1 }}</span>
        <span class="step-title">{{ step }}</span>
      </div>
    </div>

    <!-- Navigation Controls -->
    <div class="navigation">
      <button
        @click="previousStep"
        :disabled="currentStep === 0"
        class="btn btn-prev"
      >
        Previous
      </button>

      <span class="step-info">
        Step {{ currentStep + 1 }} of {{ steps.length }}
      </span>

      <button
        @click="nextStep"
        :disabled="currentStep === steps.length - 1"
        class="btn btn-next"
      >
        Next
      </button>
    </div>

    <!-- Step Content -->
    <div class="step-content">
      <h2>{{ steps[currentStep] }}</h2>
      <p>Content for {{ steps[currentStep] }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Reactive data
const steps = ref(['Personal Info', 'Account Setup', 'Preferences'])
const currentStep = ref(0)

// Navigation functions
const nextStep = () => {
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++
  }
}

const previousStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--
  }
}

const goToStep = (index) => {
  currentStep.value = index
}

// Helper function for styling
const getStepClass = (index) => {
  return {
    'step-active': index === currentStep.value,
    'step-completed': index < currentStep.value,
    'step-upcoming': index > currentStep.value
  }
}
</script>

<style scoped>
.progress-steps {
  max-width: 600px;
  margin: 50px auto;
  padding: 30px;
  background: #f5f5f5;
  border-radius: 10px;
  text-align: center;
}

.progress-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 30px;
  position: relative;
}

.step {
  flex: 1;
  padding: 15px;
  cursor: pointer;
  border-radius: 5px;
  transition: all 0.3s ease;
}

.step:hover {
  background: #e0e0e0;
}

.step-active {
  background: #4CAF50;
  color: white;
}

.step-completed {
  background: #2196F3;
  color: white;
}

.step-upcoming {
  background: #ccc;
  color: #666;
}

.navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.btn {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.btn-prev {
  background: #f44336;
  color: white;
}

.btn-next {
  background: #4CAF50;
  color: white;
}

.step-content {
  padding: 20px;
  background: white;
  border-radius: 5px;
  margin-top: 20px;
}
</style>
```

### 3.3 Update App.vue
```vue
<template>
  <div id="app">
    <header>
      <h1>🚀 Vue.js Progress Steps</h1>
      <p>Interactive Multi-step Form Demo</p>
    </header>

    <main>
      <ProgressSteps />
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js 3</p>
    </footer>
  </div>
</template>

<script setup>
import ProgressSteps from './components/ProgressSteps.vue'
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

#app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header, footer {
  text-align: center;
  color: white;
  padding: 20px 0;
}

main {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
```

---

## 🎨 Langkah 4: Enhanced Styling & UI

### 4.1 Buat Global CSS
**File: `src/assets/styles/main.css`**
```css
/* CSS Variables untuk theme consistency */
:root {
  --primary-color: #4CAF50;
  --secondary-color: #2196F3;
  --danger-color: #f44336;
  --warning-color: #FF9800;
  --success-color: #8BC34A;
  --dark-color: #333;
  --light-color: #f5f5f5;
  --border-radius: 8px;
  --transition: all 0.3s ease;
}

/* Reset & Base Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  color: var(--dark-color);
}

/* Utility Classes */
.text-center { text-align: center; }
.mb-1 { margin-bottom: 0.5rem; }
.mb-2 { margin-bottom: 1rem; }
.mb-3 { margin-bottom: 1.5rem; }
.mt-1 { margin-top: 0.5rem; }
.mt-2 { margin-top: 1rem; }
.mt-3 { margin-top: 1.5rem; }

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: var(--border-radius);
  cursor: pointer;
  font-size: 1rem;
  font-weight: 600;
  transition: var(--transition);
  text-decoration: none;
  display: inline-block;
}

.btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
}

.btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.btn-primary { background: var(--primary-color); color: white; }
.btn-secondary { background: var(--secondary-color); color: white; }
.btn-danger { background: var(--danger-color); color: white; }
.btn-warning { background: var(--warning-color); color: white; }

/* Card Component */
.card {
  background: white;
  border-radius: var(--border-radius);
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 1.5rem;
  margin-bottom: 1rem;
}

/* Form Styles */
.form-group {
  margin-bottom: 1rem;
}

.form-group label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 600;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #ddd;
  border-radius: var(--border-radius);
  font-size: 1rem;
  transition: var(--transition);
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
}

/* Animation Classes */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

.slide-enter-active, .slide-leave-active {
  transition: transform 0.3s;
}

.slide-enter-from {
  transform: translateX(100%);
}

.slide-leave-to {
  transform: translateX(-100%);
}

/* Responsive Design */
@media (max-width: 768px) {
  .btn {
    width: 100%;
    margin-bottom: 0.5rem;
  }

  .card {
    padding: 1rem;
  }
}
```

---

## ⚡ Langkah 5: Advanced Features Implementation

### 5.1 Enhanced ProgressSteps.vue dengan Computed Properties
```vue
<script setup>
import { ref, computed, watch, onMounted } from 'vue'

// Enhanced reactive data
const steps = ref([
  {
    id: 1,
    title: 'Personal Information',
    description: 'Basic personal details',
    icon: '👤',
    isCompleted: false
  },
  {
    id: 2,
    title: 'Account Setup',
    description: 'Create your account',
    icon: '⚙️',
    isCompleted: false
  },
  {
    id: 3,
    title: 'Preferences',
    description: 'Customize your experience',
    icon: '🎯',
    isCompleted: false
  }
])

const currentStep = ref(0)
const stepData = ref({})
const isLoading = ref(false)

// Computed properties
const progressPercentage = computed(() => {
  return ((currentStep.value + 1) / steps.value.length) * 100
})

const currentStepData = computed(() => {
  return steps.value[currentStep.value]
})

const canGoNext = computed(() => {
  return currentStep.value < steps.value.length - 1
})

const canGoPrevious = computed(() => {
  return currentStep.value > 0
})

const isLastStep = computed(() => {
  return currentStep.value === steps.value.length - 1
})

const isFirstStep = computed(() => {
  return currentStep.value === 0
})

// Enhanced navigation functions
const nextStep = async () => {
  if (await validateCurrentStep()) {
    if (canGoNext.value) {
      // Mark current step as completed
      steps.value[currentStep.value].isCompleted = true

      // Save step data
      saveStepData()

      // Move to next step
      currentStep.value++

      // Track progress
      trackProgress('next', currentStep.value - 1)
    } else {
      // Complete the process
      completeProcess()
    }
  }
}

const previousStep = () => {
  if (canGoPrevious.value) {
    currentStep.value--
    trackProgress('previous', currentStep.value + 1)
  }
}

const goToStep = async (index) => {
  if (index !== currentStep.value && index >= 0 && index < steps.value.length) {
    // Validate if we can jump to this step
    if (await canJumpToStep(index)) {
      currentStep.value = index
      trackProgress('jump', index)
    }
  }
}

// Validation functions
const validateCurrentStep = async () => {
  isLoading.value = true

  try {
    // Simulate API validation
    await new Promise(resolve => setTimeout(resolve, 500))

    // Basic validation logic
    const data = stepData.value[currentStep.value] || {}

    switch (currentStep.value) {
      case 0: // Personal Info
        return validatePersonalInfo(data)
      case 1: // Account Setup
        return validateAccountSetup(data)
      case 2: // Preferences
        return validatePreferences(data)
      default:
        return true
    }
  } catch (error) {
    console.error('Validation error:', error)
    return false
  } finally {
    isLoading.value = false
  }
}

const validatePersonalInfo = (data) => {
  const errors = []

  if (!data.name || data.name.trim().length < 2) {
    errors.push('Name must be at least 2 characters')
  }

  if (!data.email || !isValidEmail(data.email)) {
    errors.push('Valid email is required')
  }

  if (!data.phone || data.phone.length < 10) {
    errors.push('Valid phone number is required')
  }

  if (errors.length > 0) {
    alert('Please fix the following errors:\n' + errors.join('\n'))
    return false
  }

  return true
}

const validateAccountSetup = (data) => {
  const errors = []

  if (!data.username || data.username.length < 3) {
    errors.push('Username must be at least 3 characters')
  }

  if (!data.password || data.password.length < 8) {
    errors.push('Password must be at least 8 characters')
  }

  if (data.password !== data.confirmPassword) {
    errors.push('Passwords do not match')
  }

  if (errors.length > 0) {
    alert('Please fix the following errors:\n' + errors.join('\n'))
    return false
  }

  return true
}

const validatePreferences = (data) => {
  // Preferences are usually optional
  return true
}

// Helper functions
const isValidEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return re.test(email)
}

const canJumpToStep = async (index) => {
  // Allow jumping to previous steps or next step if current is valid
  if (index < currentStep.value) {
    return true
  }

  if (index === currentStep.value + 1) {
    return await validateCurrentStep()
  }

  return false
}

// Data management
const saveStepData = () => {
  const data = stepData.value[currentStep.value] || {}

  // Save to localStorage for persistence
  localStorage.setItem(`step_${currentStep.value}_data`, JSON.stringify(data))
  localStorage.setItem('current_step', currentStep.value.toString())

  // Mark step as completed
  steps.value[currentStep.value].isCompleted = true
}

const loadStepData = (stepIndex) => {
  const saved = localStorage.getItem(`step_${stepIndex}_data`)
  if (saved) {
    stepData.value[stepIndex] = JSON.parse(saved)
  }
}

const loadSavedProgress = () => {
  const savedStep = localStorage.getItem('current_step')
  if (savedStep) {
    const stepIndex = parseInt(savedStep)
    if (stepIndex >= 0 && stepIndex < steps.value.length) {
      currentStep.value = stepIndex

      // Load all step data
      steps.value.forEach((_, index) => {
        loadStepData(index)
      })
    }
  }
}

// Progress tracking
const trackProgress = (action, stepIndex) => {
  const event = {
    action,
    stepIndex,
    stepTitle: steps.value[stepIndex].title,
    timestamp: new Date().toISOString(),
    userAgent: navigator.userAgent
  }

  // Save to analytics (in real app, send to server)
  console.log('Progress tracked:', event)

  // Save to local history
  const history = JSON.parse(localStorage.getItem('progress_history') || '[]')
  history.unshift(event)

  // Keep only last 50 events
  if (history.length > 50) {
    history.splice(50)
  }

  localStorage.setItem('progress_history', JSON.stringify(history))
}

// Process completion
const completeProcess = async () => {
  isLoading.value = true

  try {
    // Simulate API call to save all data
    await new Promise(resolve => setTimeout(resolve, 1000))

    // Mark all steps as completed
    steps.value.forEach(step => {
      step.isCompleted = true
    })

    // Save completion data
    const completionData = {
      allStepData: stepData.value,
      completedAt: new Date().toISOString(),
      totalSteps: steps.value.length
    }

    localStorage.setItem('process_completion', JSON.stringify(completionData))

    // Show success message
    alert('🎉 Process completed successfully!')

    // Optionally reset or redirect
    // resetProgress()

  } catch (error) {
    console.error('Completion error:', error)
    alert('Error completing process. Please try again.')
  } finally {
    isLoading.value = false
  }
}

// Reset functionality
const resetProgress = () => {
  if (confirm('Are you sure you want to reset all progress?')) {
    currentStep.value = 0
    stepData.value = {}

    steps.value.forEach(step => {
      step.isCompleted = false
    })

    // Clear localStorage
    Object.keys(localStorage).forEach(key => {
      if (key.startsWith('step_') || key === 'current_step' || key === 'progress_history') {
        localStorage.removeItem(key)
      }
    })
  }
}

// Step management functions
const addStep = () => {
  const newStep = {
    id: steps.value.length + 1,
    title: `Step ${steps.value.length + 1}`,
    description: 'New step description',
    icon: '📝',
    isCompleted: false
  }

  steps.value.push(newStep)
}

const removeStep = (index) => {
  if (steps.value.length > 1 && index < steps.value.length) {
    steps.value.splice(index, 1)

    // Adjust current step if needed
    if (currentStep.value >= steps.value.length) {
      currentStep.value = steps.value.length - 1
    }
  }
}

// Watch for step changes
watch(currentStep, (newStep, oldStep) => {
  if (oldStep !== undefined) {
    // Load data for the new step
    loadStepData(newStep)
  }
})

// Lifecycle hooks
onMounted(() => {
  loadSavedProgress()
})
</script>
```

### 5.2 Enhanced Template dengan Dynamic Content
```vue
<template>
  <div class="progress-steps-enhanced">
    <!-- Progress Header -->
    <div class="progress-header">
      <h1>🚀 Multi-Step Registration</h1>
      <p>Complete your profile in just a few steps</p>

      <!-- Overall Progress Bar -->
      <div class="overall-progress">
        <div class="progress-track">
          <div
            class="progress-fill"
            :style="{ width: progressPercentage + '%' }"
          ></div>
        </div>
        <div class="progress-text">
          {{ progressPercentage.toFixed(0) }}% Complete
        </div>
      </div>
    </div>

    <!-- Interactive Steps -->
    <div class="steps-container">
      <div
        v-for="(step, index) in steps"
        :key="step.id"
        :class="getStepClasses(index)"
        class="step-item"
        @click="goToStep(index)"
      >
        <!-- Step Connector -->
        <div
          v-if="index < steps.length - 1"
          class="step-connector"
          :class="{ 'connector-active': index < currentStep }"
        ></div>

        <!-- Step Content -->
        <div class="step-content">
          <div class="step-icon">
            <span v-if="step.isCompleted" class="completed-check">✓</span>
            <span v-else>{{ step.icon }}</span>
          </div>

          <div class="step-info">
            <div class="step-number">Step {{ index + 1 }}</div>
            <div class="step-title">{{ step.title }}</div>
            <div class="step-description">{{ step.description }}</div>
          </div>

          <div class="step-status">
            <span v-if="index === currentStep" class="status-active">Active</span>
            <span v-else-if="step.isCompleted" class="status-completed">Completed</span>
            <span v-else class="status-upcoming">Upcoming</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Current Step Content -->
    <div class="current-step-content">
      <div class="step-header">
        <h2>{{ currentStepData.icon }} {{ currentStepData.title }}</h2>
        <p>{{ currentStepData.description }}</p>
      </div>

      <!-- Dynamic Step Forms -->
      <div class="step-form" v-if="!isLoading">
        <!-- Personal Information Step -->
        <div v-if="currentStep === 0" class="form-content">
          <div class="form-row">
            <div class="form-group">
              <label for="name">Full Name *</label>
              <input
                id="name"
                v-model="stepData[currentStep].name"
                type="text"
                placeholder="Enter your full name"
                required
              >
            </div>

            <div class="form-group">
              <label for="email">Email Address *</label>
              <input
                id="email"
                v-model="stepData[currentStep].email"
                type="email"
                placeholder="your.email@example.com"
                required
              >
            </div>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="phone">Phone Number *</label>
              <input
                id="phone"
                v-model="stepData[currentStep].phone"
                type="tel"
                placeholder="+1234567890"
                required
              >
            </div>

            <div class="form-group">
              <label for="birthdate">Date of Birth</label>
              <input
                id="birthdate"
                v-model="stepData[currentStep].birthdate"
                type="date"
              >
            </div>
          </div>

          <div class="form-group">
            <label for="address">Address</label>
            <textarea
              id="address"
              v-model="stepData[currentStep].address"
              placeholder="Enter your address"
              rows="3"
            ></textarea>
          </div>
        </div>

        <!-- Account Setup Step -->
        <div v-else-if="currentStep === 1" class="form-content">
          <div class="form-row">
            <div class="form-group">
              <label for="username">Username *</label>
              <input
                id="username"
                v-model="stepData[currentStep].username"
                type="text"
                placeholder="Choose a username"
                required
              >
            </div>

            <div class="form-group">
              <label for="email">Account Email *</label>
              <input
                id="account-email"
                v-model="stepData[currentStep].accountEmail"
                type="email"
                placeholder="account@example.com"
                required
              >
            </div>
          </div>

          <div class="form-row">
            <div class="form-group">
              <label for="password">Password *</label>
              <input
                id="password"
                v-model="stepData[currentStep].password"
                type="password"
                placeholder="Create a strong password"
                required
              >
            </div>

            <div class="form-group">
              <label for="confirmPassword">Confirm Password *</label>
              <input
                id="confirmPassword"
                v-model="stepData[currentStep].confirmPassword"
                type="password"
                placeholder="Confirm your password"
                required
              >
            </div>
          </div>

          <div class="form-group">
            <label>
              <input
                v-model="stepData[currentStep].newsletter"
                type="checkbox"
              >
              Subscribe to newsletter and updates
            </label>
          </div>
        </div>

        <!-- Preferences Step -->
        <div v-else-if="currentStep === 2" class="form-content">
          <div class="form-group">
            <label for="language">Preferred Language</label>
            <select id="language" v-model="stepData[currentStep].language">
              <option value="">Select Language</option>
              <option value="en">English</option>
              <option value="es">Spanish</option>
              <option value="fr">French</option>
              <option value="de">German</option>
              <option value="zh">Chinese</option>
            </select>
          </div>

          <div class="form-group">
            <label for="timezone">Timezone</label>
            <select id="timezone" v-model="stepData[currentStep].timezone">
              <option value="">Select Timezone</option>
              <option value="UTC">UTC</option>
              <option value="EST">Eastern Time</option>
              <option value="CST">Central Time</option>
              <option value="MST">Mountain Time</option>
              <option value="PST">Pacific Time</option>
            </select>
          </div>

          <div class="form-group">
            <label>Notification Preferences</label>
            <div class="checkbox-group">
              <label>
                <input
                  v-model="stepData[currentStep].emailNotifications"
                  type="checkbox"
                >
                Email notifications
              </label>
              <label>
                <input
                  v-model="stepData[currentStep].smsNotifications"
                  type="checkbox"
                >
                SMS notifications
              </label>
              <label>
                <input
                  v-model="stepData[currentStep].pushNotifications"
                  type="checkbox"
                >
                Push notifications
              </label>
            </div>
          </div>

          <div class="form-group">
            <label>Privacy Settings</label>
            <div class="radio-group">
              <label>
                <input
                  v-model="stepData[currentStep].privacy"
                  type="radio"
                  value="public"
                >
                Public profile
              </label>
              <label>
                <input
                  v-model="stepData[currentStep].privacy"
                  type="radio"
                  value="friends"
                >
                Friends only
              </label>
              <label>
                <input
                  v-model="stepData[currentStep].privacy"
                  type="radio"
                  value="private"
                >
                Private profile
              </label>
            </div>
          </div>
        </div>
      </div>

      <!-- Loading State -->
      <div v-else class="loading-state">
        <div class="spinner"></div>
        <p>Processing...</p>
      </div>

      <!-- Navigation Buttons -->
      <div class="navigation-controls">
        <button
          @click="previousStep"
          :disabled="isFirstStep || isLoading"
          class="btn btn-secondary"
        >
          ← Previous
        </button>

        <div class="step-indicators">
          <span
            v-for="(step, index) in steps"
            :key="index"
            :class="getIndicatorClass(index)"
            class="indicator"
          ></span>
        </div>

        <button
          @click="nextStep"
          :disabled="isLoading"
          class="btn btn-primary"
        >
          <span v-if="isLoading">Processing...</span>
          <span v-else-if="isLastStep">✓ Complete</span>
          <span v-else>Next →</span>
        </button>
      </div>
    </div>

    <!-- Quick Actions -->
    <div class="quick-actions">
      <button @click="resetProgress" class="btn btn-danger">
        🔄 Reset Progress
      </button>
      <button @click="addStep" class="btn btn-secondary">
        ➕ Add Step
      </button>
      <button
        @click="removeStep(currentStep)"
        class="btn btn-danger"
        :disabled="steps.length <= 1"
      >
        ➖ Remove Current Step
      </button>
    </div>

    <!-- Progress Summary -->
    <div class="progress-summary">
      <h3>📊 Progress Summary</h3>
      <div class="summary-grid">
        <div class="summary-item">
          <span class="summary-label">Current Step:</span>
          <span class="summary-value">{{ currentStep + 1 }} / {{ steps.length }}</span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Completed Steps:</span>
          <span class="summary-value">{{ steps.filter(s => s.isCompleted).length }}</span>
        </div>
        <div class="summary-item">
          <span class="summary-label">Progress:</span>
          <span class="summary-value">{{ progressPercentage.toFixed(1) }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>
```

### 5.3 Enhanced CSS Styling
```vue
<style scoped>
.progress-steps-enhanced {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

/* Progress Header */
.progress-header {
  text-align: center;
  margin-bottom: 40px;
  padding-bottom: 30px;
  border-bottom: 2px solid #f0f0f0;
}

.progress-header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  color: var(--primary-color);
}

.progress-header p {
  font-size: 1.2em;
  color: #666;
  margin-bottom: 30px;
}

.overall-progress {
  max-width: 400px;
  margin: 0 auto;
}

.progress-track {
  width: 100%;
  height: 8px;
  background: #f0f0f0;
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--primary-color), var(--success-color));
  transition: width 0.5s ease;
  border-radius: 4px;
}

.progress-text {
  font-weight: 600;
  color: var(--primary-color);
}

/* Steps Container */
.steps-container {
  margin-bottom: 40px;
}

.step-item {
  position: relative;
  padding: 20px;
  margin-bottom: 20px;
  border: 2px solid #e0e0e0;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s ease;
  background: #fafafa;
}

.step-item:hover {
  border-color: var(--primary-color);
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.1);
}

.step-connector {
  position: absolute;
  left: 30px;
  top: 100%;
  width: 2px;
  height: 20px;
  background: #e0e0e0;
  z-index: 1;
}

.step-connector.connector-active {
  background: var(--primary-color);
}

.step-content {
  display: flex;
  align-items: center;
  gap: 20px;
  position: relative;
  z-index: 2;
}

.step-icon {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: #e0e0e0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.5em;
  transition: all 0.3s ease;
}

.step-info {
  flex: 1;
}

.step-number {
  font-size: 0.9em;
  color: #666;
  margin-bottom: 5px;
}

.step-title {
  font-weight: 600;
  font-size: 1.1em;
  margin-bottom: 5px;
}

.step-description {
  color: #666;
  font-size: 0.9em;
}

.step-status {
  text-align: right;
}

.status-active {
  color: var(--primary-color);
  font-weight: 600;
}

.status-completed {
  color: var(--success-color);
  font-weight: 600;
}

.status-upcoming {
  color: #999;
  font-size: 0.9em;
}

/* Step States */
.step-item.step-active {
  border-color: var(--primary-color);
  background: rgba(76, 175, 80, 0.05);
}

.step-item.step-active .step-icon {
  background: var(--primary-color);
  color: white;
}

.step-item.step-completed {
  border-color: var(--success-color);
  background: rgba(139, 195, 74, 0.05);
}

.step-item.step-completed .step-icon {
  background: var(--success-color);
  color: white;
}

.completed-check {
  font-weight: bold;
  color: white;
}

/* Current Step Content */
.current-step-content {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 30px;
  margin-bottom: 30px;
}

.step-header {
  text-align: center;
  margin-bottom: 30px;
}

.step-header h2 {
  font-size: 2em;
  margin-bottom: 10px;
  color: var(--dark-color);
}

.step-header p {
  color: #666;
  font-size: 1.1em;
}

/* Form Styles */
.form-content {
  max-width: 600px;
  margin: 0 auto;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
  color: var(--dark-color);
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.1);
}

.checkbox-group,
.radio-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.checkbox-group label,
.radio-group label {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: normal;
}

/* Loading State */
.loading-state {
  text-align: center;
  padding: 40px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid var(--primary-color);
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Navigation Controls */
.navigation-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 30px;
  padding-top: 20px;
  border-top: 1px solid #e0e0e0;
}

.step-indicators {
  display: flex;
  gap: 8px;
}

.indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #e0e0e0;
  transition: all 0.3s ease;
}

.indicator.active {
  background: var(--primary-color);
  width: 24px;
  border-radius: 4px;
}

.indicator.completed {
  background: var(--success-color);
}

/* Quick Actions */
.quick-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

/* Progress Summary */
.progress-summary {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 20px;
  border: 1px solid #e0e0e0;
}

.progress-summary h3 {
  margin-bottom: 15px;
  text-align: center;
  color: var(--dark-color);
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
}

.summary-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 15px;
  background: white;
  border-radius: 8px;
  border: 1px solid #e0e0e0;
}

.summary-label {
  font-size: 0.9em;
  color: #666;
  margin-bottom: 5px;
}

.summary-value {
  font-size: 1.2em;
  font-weight: 600;
  color: var(--primary-color);
}

/* Responsive Design */
@media (max-width: 768px) {
  .progress-steps-enhanced {
    padding: 15px;
    margin: 10px;
  }

  .progress-header h1 {
    font-size: 2em;
  }

  .form-row {
    grid-template-columns: 1fr;
  }

  .navigation-controls {
    flex-direction: column;
    gap: 15px;
  }

  .quick-actions {
    flex-direction: column;
  }

  .btn {
    width: 100%;
  }

  .step-content {
    flex-direction: column;
    text-align: center;
  }

  .summary-grid {
    grid-template-columns: 1fr;
  }
}

/* Animation Classes */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

.slide-enter-active, .slide-leave-active {
  transition: transform 0.3s ease;
}

.slide-enter-from {
  transform: translateX(30px);
  opacity: 0;
}

.slide-leave-to {
  transform: translateX(-30px);
  opacity: 0;
}
</style>
```

---

## 🔧 Langkah 6: Helper Functions & Utilities

### 6.1 Buat File Helper Functions
**File: `src/utils/helpers.js`**
```javascript
// Validation helpers
export const validators = {
  email: (email) => {
    const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    return re.test(email)
  },

  phone: (phone) => {
    const re = /^\+?[\d\s-()]{10,}$/
    return re.test(phone)
  },

  password: (password) => {
    return password.length >= 8
  },

  username: (username) => {
    return username.length >= 3 && /^[a-zA-Z0-9_]+$/.test(username)
  },

  required: (value) => {
    return value !== null && value !== undefined && value.toString().trim() !== ''
  }
}

// Form validation helper
export const validateForm = (data, rules) => {
  const errors = []

  for (const [field, rule] of Object.entries(rules)) {
    const value = data[field]

    if (rule.required && !validators.required(value)) {
      errors.push(`${field} is required`)
      continue
    }

    if (rule.type && value) {
      switch (rule.type) {
        case 'email':
          if (!validators.email(value)) {
            errors.push(`${field} must be a valid email`)
          }
          break
        case 'phone':
          if (!validators.phone(value)) {
            errors.push(`${field} must be a valid phone number`)
          }
          break
        case 'password':
          if (!validators.password(value)) {
            errors.push(`${field} must be at least 8 characters`)
          }
          break
        case 'username':
          if (!validators.username(value)) {
            errors.push(`${field} must be at least 3 characters and contain only letters, numbers, and underscores`)
          }
          break
      }
    }

    if (rule.minLength && value && value.length < rule.minLength) {
      errors.push(`${field} must be at least ${rule.minLength} characters`)
    }

    if (rule.maxLength && value && value.length > rule.maxLength) {
      errors.push(`${field} must not exceed ${rule.maxLength} characters`)
    }
  }

  return errors
}

// Storage helpers
export const storage = {
  get: (key, defaultValue = null) => {
    try {
      const item = localStorage.getItem(key)
      return item ? JSON.parse(item) : defaultValue
    } catch {
      return defaultValue
    }
  },

  set: (key, value) => {
    try {
      localStorage.setItem(key, JSON.stringify(value))
      return true
    } catch {
      return false
    }
  },

  remove: (key) => {
    try {
      localStorage.removeItem(key)
      return true
    } catch {
      return false
    }
  },

  clear: (prefix = '') => {
    try {
      Object.keys(localStorage).forEach(key => {
        if (key.startsWith(prefix)) {
          localStorage.removeItem(key)
        }
      })
      return true
    } catch {
      return false
    }
  }
}

// Date helpers
export const dateHelpers = {
  format: (date, format = 'YYYY-MM-DD') => {
    const d = new Date(date)
    const year = d.getFullYear()
    const month = String(d.getMonth() + 1).padStart(2, '0')
    const day = String(d.getDate()).padStart(2, '0')

    return format
      .replace('YYYY', year)
      .replace('MM', month)
      .replace('DD', day)
  },

  age: (birthdate) => {
    const today = new Date()
    const birth = new Date(birthdate)
    let age = today.getFullYear() - birth.getFullYear()
    const monthDiff = today.getMonth() - birth.getMonth()

    if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birth.getDate())) {
      age--
    }

    return age
  },

  isValid: (date) => {
    const d = new Date(date)
    return d instanceof Date && !isNaN(d)
  }
}

// String helpers
export const stringHelpers = {
  capitalize: (str) => {
    return str.charAt(0).toUpperCase() + str.slice(1).toLowerCase()
  },

  slugify: (str) => {
    return str
      .toLowerCase()
      .trim()
      .replace(/[^\w\s-]/g, '')
      .replace(/[\s_-]+/g, '-')
      .replace(/^-+|-+$/g, '')
  },

  truncate: (str, length, suffix = '...') => {
    if (str.length <= length) return str
    return str.substring(0, length - suffix.length) + suffix
  },

  generateRandom: (length = 8) => {
    const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'
    let result = ''
    for (let i = 0; i < length; i++) {
      result += chars.charAt(Math.floor(Math.random() * chars.length))
    }
    return result
  }
}

// Array helpers
export const arrayHelpers = {
  move: (array, fromIndex, toIndex) => {
    const newArray = [...array]
    const [movedItem] = newArray.splice(fromIndex, 1)
    newArray.splice(toIndex, 0, movedItem)
    return newArray
  },

  remove: (array, index) => {
    return array.filter((_, i) => i !== index)
  },

  swap: (array, index1, index2) => {
    const newArray = [...array]
    ;[newArray[index1], newArray[index2]] = [newArray[index2], newArray[index1]]
    return newArray
  },

  unique: (array, key = null) => {
    if (key) {
      const seen = new Set()
      return array.filter(item => {
        const value = item[key]
        if (seen.has(value)) {
          return false
        }
        seen.add(value)
        return true
      })
    }
    return [...new Set(array)]
  }
}

// Object helpers
export const objectHelpers = {
  deepClone: (obj) => {
    if (obj === null || typeof obj !== 'object') return obj
    if (obj instanceof Date) return new Date(obj.getTime())
    if (obj instanceof Array) return obj.map(item => objectHelpers.deepClone(item))
    if (typeof obj === 'object') {
      const clonedObj = {}
      for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
          clonedObj[key] = objectHelpers.deepClone(obj[key])
        }
      }
      return clonedObj
    }
  },

  merge: (target, source) => {
    const result = { ...target }
    for (const key in source) {
      if (source.hasOwnProperty(key)) {
        if (typeof source[key] === 'object' && source[key] !== null && !Array.isArray(source[key])) {
          result[key] = objectHelpers.merge(result[key] || {}, source[key])
        } else {
          result[key] = source[key]
        }
      }
    }
    return result
  },

  pick: (obj, keys) => {
    const result = {}
    keys.forEach(key => {
      if (obj.hasOwnProperty(key)) {
        result[key] = obj[key]
      }
    })
    return result
  },

  omit: (obj, keys) => {
    const result = { ...obj }
    keys.forEach(key => {
      delete result[key]
    })
    return result
  }
}

// URL helpers
export const urlHelpers = {
  getParams: () => {
    const params = new URLSearchParams(window.location.search)
    const result = {}
    for (const [key, value] of params) {
      result[key] = value
    }
    return result
  },

  setParams: (params) => {
    const url = new URL(window.location)
    Object.entries(params).forEach(([key, value]) => {
      if (value !== null && value !== undefined) {
        url.searchParams.set(key, value)
      } else {
        url.searchParams.delete(key)
      }
    })
    window.history.replaceState({}, '', url)
  },

  buildQuery: (params) => {
    return new URLSearchParams(params).toString()
  }
}

// Debounce helper
export const debounce = (func, wait) => {
  let timeout
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout)
      func(...args)
    }
    clearTimeout(timeout)
    timeout = setTimeout(later, wait)
  }
}

// Throttle helper
export const throttle = (func, limit) => {
  let inThrottle
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args)
      inThrottle = true
      setTimeout(() => inThrottle = false, limit)
    }
  }
}

// Color helpers
export const colorHelpers = {
  hexToRgb: (hex) => {
    const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex)
    return result ? {
      r: parseInt(result[1], 16),
      g: parseInt(result[2], 16),
      b: parseInt(result[3], 16)
    } : null
  },

  rgbToHex: (r, g, b) => {
    return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1)
  },

  getRandomColor: () => {
    const letters = '0123456789ABCDEF'
    let color = '#'
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)]
    }
    return color
  }
}

// Export all helpers
export default {
  validators,
  validateForm,
  storage,
  dateHelpers,
  stringHelpers,
  arrayHelpers,
  objectHelpers,
  urlHelpers,
  debounce,
  throttle,
  colorHelpers
}
```

---

## 📱 Langkah 7: Mobile Responsive Design

### 7.1 Responsive CSS Media Queries
```css
/* Mobile-first approach */

/* Small devices (phones, 576px and up) */
@media (max-width: 575.98px) {
  .progress-steps-enhanced {
    margin: 10px;
    padding: 15px;
  }

  .progress-header h1 {
    font-size: 1.8em;
  }

  .step-content {
    flex-direction: column;
    text-align: center;
    gap: 15px;
  }

  .step-icon {
    margin: 0 auto;
  }

  .form-row {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .navigation-controls {
    flex-direction: column;
    gap: 15px;
  }

  .btn {
    width: 100%;
    padding: 15px;
    font-size: 1.1em;
  }

  .quick-actions {
    flex-direction: column;
    gap: 10px;
  }

  .summary-grid {
    grid-template-columns: 1fr;
    gap: 10px;
  }
}

/* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) and (max-width: 991.98px) {
  .progress-steps-enhanced {
    padding: 20px;
  }

  .form-row {
    gap: 15px;
  }

  .navigation-controls {
    gap: 20px;
  }

  .quick-actions {
    gap: 12px;
  }
}

/* Large devices (desktops, 992px and up) */
@media (min-width: 992px) {
  .progress-steps-enhanced {
    padding: 30px;
  }

  .step-content {
    gap: 25px;
  }

  .form-row {
    gap: 25px;
  }
}

/* Extra large devices (large desktops, 1200px and up) */
@media (min-width: 1200px) {
  .progress-steps-enhanced {
    max-width: 1000px;
  }
}

/* Touch device optimizations */
@media (hover: none) and (pointer: coarse) {
  .step-item:hover {
    transform: none;
  }

  .btn:hover:not(:disabled) {
    transform: none;
  }

  .step-item {
    padding: 25px;
  }

  .btn {
    padding: 18px;
    min-height: 48px;
  }
}

/* High contrast mode */
@media (prefers-contrast: high) {
  .step-item {
    border-width: 3px;
  }

  .btn {
    border: 2px solid currentColor;
  }

  .form-group input:focus,
  .form-group select:focus,
  .form-group textarea:focus {
    border-width: 3px;
  }
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 7.2 Touch-friendly JavaScript Enhancements
```javascript
// Add to ProgressSteps.vue script setup
import { ref, onMounted, onUnmounted } from 'vue'

// Touch handling
const touchStartX = ref(0)
const touchEndX = ref(0)
const touchThreshold = 50

const handleTouchStart = (e) => {
  touchStartX.value = e.changedTouches[0].screenX
}

const handleTouchEnd = (e) => {
  touchEndX.value = e.changedTouches[0].screenX
  handleSwipeGesture()
}

const handleSwipeGesture = () => {
  const swipeDistance = touchEndX.value - touchStartX.value

  if (Math.abs(swipeDistance) > touchThreshold) {
    if (swipeDistance > 0) {
      // Swipe right - previous step
      if (canGoPrevious.value) {
        previousStep()
      }
    } else {
      // Swipe left - next step
      if (canGoNext.value) {
        nextStep()
      }
    }
  }
}

// Keyboard navigation enhancements
const handleKeyDown = (e) => {
  switch (e.key) {
    case 'ArrowRight':
      if (canGoNext.value) {
        nextStep()
      }
      break
    case 'ArrowLeft':
      if (canGoPrevious.value) {
        previousStep()
      }
      break
    case 'Enter':
      if (isLastStep.value) {
        completeProcess()
      } else if (canGoNext.value) {
        nextStep()
      }
      break
    case 'Escape':
      if (confirm('Are you sure you want to reset your progress?')) {
        resetProgress()
      }
      break
  }
}

// Lifecycle hooks
onMounted(() => {
  document.addEventListener('keydown', handleKeyDown)

  // Add touch event listeners
  const container = document.querySelector('.progress-steps-enhanced')
  if (container) {
    container.addEventListener('touchstart', handleTouchStart, { passive: true })
    container.addEventListener('touchend', handleTouchEnd, { passive: true })
  }
})

onUnmounted(() => {
  document.removeEventListener('keydown', handleKeyDown)

  // Remove touch event listeners
  const container = document.querySelector('.progress-steps-enhanced')
  if (container) {
    container.removeEventListener('touchstart', handleTouchStart)
    container.removeEventListener('touchend', handleTouchEnd)
  }
})
```

---

## 🧪 Langkah 8: Testing & Quality Assurance

### 8.1 Manual Testing Checklist
```markdown
## Testing Checklist

### Basic Functionality
- [ ] Step navigation works (next/previous)
- [ ] Direct step clicking works
- [ ] Progress bar updates correctly
- [ ] Form validation works
- [ ] Data persistence works

### Edge Cases
- [ ] Can't go beyond first/last step
- [ ] Validation prevents invalid navigation
- [ ] Data persists on page refresh
- [ ] Reset functionality works
- [ ] Dynamic step addition/removal

### Responsive Design
- [ ] Mobile view works correctly
- [ ] Tablet view works correctly
- [ ] Desktop view works correctly
- [ ] Touch gestures work
- [ ] Keyboard navigation works

### Browser Compatibility
- [ ] Chrome/Chromium
- [ ] Firefox
- [ ] Safari
- [ ] Edge
- [ ] Mobile browsers

### Performance
- [ ] Fast initial load
- [ ] Smooth transitions
- [ ] No memory leaks
- [ ] Efficient re-renders
```

### 8.2 Unit Tests Setup (Optional)
```bash
# Install testing dependencies
npm install --save-dev vitest @vue/test-utils jsdom

# Create vitest.config.js
npm install --save-dev @vitejs/plugin-vue-jsx
```

**File: `vitest.config.js`**
```javascript
import { defineConfig } from 'vitest/config'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  test: {
    environment: 'jsdom',
    globals: true
  }
})
```

**File: `tests/ProgressSteps.test.js`**
```javascript
import { describe, it, expect, beforeEach } from 'vitest'
import { mount } from '@vue/test-utils'
import ProgressSteps from '../src/components/ProgressSteps.vue'

describe('ProgressSteps', () => {
  let wrapper

  beforeEach(() => {
    wrapper = mount(ProgressSteps)
  })

  it('renders correctly', () => {
    expect(wrapper.find('.progress-steps-enhanced').exists()).toBe(true)
    expect(wrapper.find('h1').text()).toContain('Multi-Step Registration')
  })

  it('initializes with first step active', () => {
    expect(wrapper.vm.currentStep).toBe(0)
  })

  it('navigates to next step', async () => {
    await wrapper.vm.nextStep()
    expect(wrapper.vm.currentStep).toBe(1)
  })

  it('prevents navigation beyond last step', async () => {
    wrapper.vm.currentStep = 2
    await wrapper.vm.nextStep()
    expect(wrapper.vm.currentStep).toBe(2)
  })

  it('validates required fields', async () => {
    wrapper.vm.currentStep = 0
    wrapper.vm.stepData[0] = {}

    const isValid = await wrapper.vm.validateCurrentStep()
    expect(isValid).toBe(false)
  })
})
```

---

## 🚀 Langkah 9: Deployment & Production

### 9.1 Build for Production
```bash
# Build the project
npm run build

# Preview production build
npm run preview
```

### 9.2 Environment Configuration
**File: `.env.production`**
```
VITE_API_BASE_URL=https://your-api.com
VITE_APP_TITLE=Progress Steps App
VITE_ANALYTICS_ID=your-analytics-id
```

### 9.3 Deployment Options

#### Option A: Netlify
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Deploy to Netlify
netlify deploy --prod --dir=dist
```

**File: `netlify.toml`**
```toml
[build]
  publish = "dist"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "16"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

#### Option B: Vercel
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy to Vercel
vercel --prod
```

#### Option C: GitHub Pages
```bash
# Deploy to GitHub Pages
npm run build

# Copy dist folder to gh-pages branch
git add dist
git commit -m "Add production build"
git subtree push --prefix dist origin gh-pages
```

### 9.4 Performance Optimization
**File: `vite.config.js`**
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['vue'],
          'utils': ['./src/utils/helpers.js']
        }
      }
    },
    minify: 'terser',
    sourcemap: false
  },
  server: {
    host: true,
    port: 3000
  }
})
```

---

## ✅ Final Review & Checklist

### Implementation Complete Checklist
- [ ] Basic progress steps functionality
- [ ] Form validation for each step
- [ ] Data persistence with localStorage
- [ ] Responsive design for all devices
- [ ] Touch and keyboard navigation
- [ ] Progress tracking and analytics
- [ ] Error handling and validation
- [ ] Accessibility features
- [ ] Performance optimization
- [ ] Production build configuration
- [ ] Deployment setup
- [ ] Documentation complete

### Code Quality Checklist
- [ ] Code follows Vue.js best practices
- [ ] Components are properly structured
- [ ] CSS is organized and maintainable
- [ ] JavaScript functions are reusable
- [ ] Error handling is comprehensive
- [ ] Code is properly commented
- [ ] File naming conventions followed
- [ ] Git repository is properly initialized

### User Experience Checklist
- [ ] Intuitive navigation
- [ ] Clear visual feedback
- [ ] Responsive design
- [ ] Accessibility features
- [ ] Error messages are helpful
- [ ] Loading states are handled
- [ ] Progress is clearly indicated
- [ ] Mobile-friendly interface

---

## 🎉 Selamat! Implementation Selesai

Anda telah berhasil membuat aplikasi Progress Steps yang lengkap dengan:

✅ **Vue.js 3 Composition API**
✅ **Advanced form validation**
✅ **Data persistence**
✅ **Responsive design**
✅ **Touch and keyboard navigation**
✅ **Progress tracking**
✅ **Error handling**
✅ **Accessibility features**
✅ **Performance optimization**
✅ **Production deployment**

### Next Steps
1. **Add more complex validation rules**
2. **Integrate with backend API**
3. **Add more sophisticated animations**
4. **Implement A/B testing**
5. **Add internationalization (i18n)**
6. **Create comprehensive test suite**

### Resources for Further Learning
- [Vue.js Documentation](https://vuejs.org/)
- [Vite Documentation](https://vitejs.dev/)
- [Web.dev for best practices](https://web.dev/)
- [MDN Web Docs](https://developer.mozilla.org/)

---

**Happy Coding! 🚀**