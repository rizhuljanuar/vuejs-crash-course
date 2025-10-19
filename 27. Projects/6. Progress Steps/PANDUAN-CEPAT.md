# 🚀 Panduan Cepat: Progress Steps Vue.js

## 🎯 Target: 5 Menit Memahami & Membuat Aplikasi

### 📋 Step 1: Persiapan (1 Menit)

**Install Node.js (belum ada?)**
```bash
# Cek apakah Node.js sudah terinstall
node --version

# Belum ada? Download di nodejs.org
```

### 📋 Step 2: Setup Project (2 Menit)

```bash
# Buat project baru dengan Vite
npm create vue@latest progress-steps-app

# Masuk ke folder project
cd progress-steps-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed } from 'vue';

// Enhanced step configuration
const steps = ref([
  { id: 1, title: '🎯 Personal Info', description: 'Basic personal information', icon: '👤' },
  { id: 2, title: '📧 Account Setup', description: 'Account configuration', icon: '⚙️' },
  { id: 3, title: '🔒 Security', description: 'Security settings', icon: '🔐' },
  { id: 4, title: '💳 Payment', description: 'Payment information', icon: '💳' },
  { id: 5, title: '✅ Confirmation', description: 'Review and submit', icon: '✅' }
]);

const currentStep = ref(0);
const stepData = ref({});
const isCompleted = ref(false);

// Progress calculations
const progressPercentage = computed(() => ((currentStep.value + 1) / steps.value.length) * 100);
const completedSteps = computed(() => Math.max(0, currentStep.value));
const totalSteps = computed(() => steps.value.length);

// Navigation functions
const nextStep = () => {
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++;
  } else {
    isCompleted.value = true;
  }
};

const prevStep = () => {
  if (currentStep.value > 0) {
    currentStep.value--;
  }
  isCompleted.value = false;
};

const goToStep = (index) => {
  if (index >= 0 && index < steps.value.length) {
    currentStep.value = index;
    isCompleted.value = false;
  }
};

// Step management
const addStep = () => {
  const newId = Math.max(...steps.value.map(s => s.id)) + 1;
  steps.value.push({
    id: newId,
    title: `📋 Step ${newId}`,
    description: `New step ${newId}`,
    icon: '📝'
  });
};

const removeStep = (index) => {
  if (steps.value.length > 1) {
    steps.value.splice(index, 1);
    if (currentStep.value >= steps.value.length) {
      currentStep.value = steps.value.length - 1;
    }
  }
};

// Step content management
const saveStepData = (data) => {
  stepData.value[currentStep.value] = { ...stepData.value[currentStep.value], ...data };
};

const getStepData = () => {
  return stepData.value[currentStep.value] || {};
};

// Step validation
const stepValidation = ref({
  0: { isValid: true, errors: [] },
  1: { isValid: true, errors: [] },
  2: { isValid: true, errors: [] },
  3: { isValid: true, errors: [] },
  4: { isValid: true, errors: [] }
});

const validateStep = (stepIndex) => {
  // Simulate validation logic
  const validation = stepValidation.value[stepIndex];
  const errors = [];

  // Simulate validation delay
  setTimeout(() => {
    validation.isValid = errors.length === 0;
    validation.errors = errors;
  }, 300);

  return errors.length === 0;
};

// Progress tracking
const progressHistory = ref([]);

const trackProgress = (action, stepIndex) => {
  progressHistory.value.unshift({
    action,
    step: steps.value[stepIndex].title,
    timestamp: new Date().toLocaleTimeString(),
    stepIndex
  });

  // Keep only last 10 entries
  if (progressHistory.value.length > 10) {
    progressHistory.value = progressHistory.value.slice(0, 10);
  }
};

// Enhanced navigation with tracking
const nextStepWithTracking = () => {
  if (validateStep(currentStep.value)) {
    trackProgress('next', currentStep.value);
    nextStep();
  } else {
    console.log('Validation failed for step', currentStep.value);
  }
};

const prevStepWithTracking = () => {
  trackProgress('previous', currentStep.value);
  prevStep();
};

// Reset functions
const resetProgress = () => {
  currentStep.value = 0;
  stepData.value = {};
  isCompleted.value = false;
  progressHistory.value = [];
};

// Keyboard navigation
const handleKeyDown = (event) => {
  switch (event.key) {
    case 'ArrowRight':
      event.preventDefault();
      if (currentStep.value < steps.value.length - 1) {
        nextStepWithTracking();
      }
      break;
    case 'ArrowLeft':
      event.preventDefault();
      if (currentStep.value > 0) {
        prevStepWithTracking();
      }
      break;
    case 'Enter':
      event.preventDefault();
      if (currentStep.value === steps.value.length - 1 && stepValidation.value[currentStep.value].isValid) {
        nextStepWithTracking();
      }
      break;
  }
};
</script>

<template>
  <div class="app">
    <header>
      <h1>📊 Advanced Progress Steps</h1>
      <p>Multi-step wizard with validation and tracking</p>
    </header>

    <main>
      <!-- Progress Overview -->
      <div class="progress-overview">
        <div class="overview-header">
          <h2>📈 Progress Overview</h2>
          <div class="overview-stats">
            <div class="stat-item">
              <span class="stat-label">Progress:</span>
              <span class="stat-value">{{ progressPercentage.toFixed(0) }}%</span>
            </div>
            <div class="stat-item">
              <span class="stat-label">Steps:</span>
              <span class="stat-value">{{ completedSteps }}/{{ totalSteps }}</span>
            </div>
            <div class="stat-item">
              <span class="stat-label">Status:</span>
              <span class="stat-value" :class="isCompleted ? 'completed' : 'in-progress'">
                {{ isCompleted ? '✅ Completed' : '📝 In Progress' }}
              </span>
            </div>
          </div>
        </div>

        <!-- Progress Bar -->
        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: progressPercentage + '%' }"></div>
        </div>
      </div>

      <!-- Interactive Progress Steps -->
      <div class="progress-steps-container">
        <div class="steps-visual">
          <div
            v-for="(step, index) in steps"
            :key="step.id"
            :class="getStepClasses(index)"
            class="step-item"
            @click="goToStep(index)"
          >
            <div class="step-connector" v-if="index < steps.length - 1"></div>
            <div class="step-content">
              <div class="step-number">
                <span v-if="index < currentStep" class="step-completed">✓</span>
                <span v-else>{{ index + 1 }}</span>
              </div>
              <div class="step-icon">{{ step.icon }}</div>
              <div class="step-title">{{ step.title }}</div>
              <div class="step-description">{{ step.description }}</div>
            </div>
            <div v-if="stepValidation[index].errors.length > 0" class="step-errors">
              <span v-for="error in stepValidation[index].errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>
        </div>
      </div>

      <!-- Step Content Area -->
      <div class="step-content-area">
        <div class="step-header">
          <h2>{{ steps[currentStep].title }}</h2>
          <p>{{ steps[currentStep].description }}</p>
        </div>

        <div class="step-form">
          <!-- Dynamic Step Content -->
          <div v-if="currentStep === 0">
            <h3>👤 Personal Information</h3>
            <div class="form-group">
              <label>Full Name</label>
              <input v-model="stepData[currentStep].name" type="text" placeholder="Enter your full name" />
            </div>
            <div class="form-group">
              <label>Email</label>
              <input v-model="stepData[currentStep].email" type="email" placeholder="your.email@example.com" />
            </div>
            <div class="form-group">
              <label>Phone</label>
              <input v-model="stepData[currentStep].phone" type="tel" placeholder="+1234567890" />
            </div>
          </div>

          <div v-else-if="currentStep === 1">
            <h3>⚙️ Account Setup</h3>
            <div class="form-group">
              <label>Username</label>
              <input v-model="stepData[currentStep].username" type="text" placeholder="Choose a username" />
            </div>
            <div class="form-group">
              <label>Password</label>
              <input v-model="stepData[currentStep].password" type="password" placeholder="Create a password" />
            </div>
            <div class="form-group">
              <label>Confirm Password</label>
              <input v-model="stepData[currentStep].confirmPassword" type="password" placeholder="Confirm your password" />
            </div>
          </div>

          <div v-else-if="currentStep === 2">
            <h3>🔒 Security Settings</h3>
            <div class="form-group">
              <label>Security Question</label>
              <select v-model="stepData[currentStep].securityQuestion">
                <option value="">Select a security question</option>
                <option value="pet">What was your first pet's name?</option>
                <option value="school">What elementary school did you attend?</option>
                <option value="city">In what city were you born?</option>
              </select>
            </div>
            <div class="form-group">
              <label>Answer</label>
              <input v-model="stepData[currentStep].securityAnswer" type="text" placeholder="Enter your answer" />
            </div>
          </div>

          <div v-else-if="currentStep === 3">
            <h3>💳 Payment Information</h3>
            <div class="form-group">
              <label>Card Number</label>
              <input v-model="stepData[currentStep].cardNumber" type="text" placeholder="1234-5678-9012-3456" maxlength="19" />
            </div>
            <div class="form-group">
              <label>Expiry Date</label>
              <input v-model="stepData[currentStep].expiryDate" type="text" placeholder="MM/YY" maxlength="5" />
            </div>
            <div class="form-group">
              <label>CVV</label>
              <input v-model="stepData[currentStep].cvv" type="text" placeholder="123" maxlength="3" />
            </div>
          </div>

          <div v-else-if="currentStep === 4">
            <h3>✅ Confirmation</h3>
            <div class="confirmation-content">
              <h4>Review Your Information</h4>
              <div class="review-section">
                <h5>Personal Information</h5>
                <div class="review-item">
                  <span>Name:</span>
                  <span>{{ stepData[0]?.name || 'Not provided' }}</span>
                </div>
                <div class="review-item">
                  <span>Email:</span>
                  <span>{{ stepData[0]?.email || 'Not provided' }}</span>
                </div>
                <div class="review-item">
                  <span>Phone:</span>
                  <span>{{ stepData[0]?.phone || 'Not provided' }}</span>
                </div>
              </div>

              <div class="review-section">
                <h5>Account Settings</h5>
                <div class="review-item">
                  <span>Username:</span>
                  <span>{{ stepData[1]?.username || 'Not provided' }}</span>
                </div>
                <div class="review-item">
                  <span>Password:</span>
                  <span>{{ stepData[1]?.password ? '••••••••••' : 'Not provided' }}</span>
                </div>
              </div>

              <div class="review-section">
                <h5>Security Settings</h5>
                <div class="review-item">
                  <span>Security Question:</span>
                  <span>{{ stepData[2]?.securityQuestion || 'Not provided' }}</span>
                </div>
              </div>

              <div class="review-section">
                <h5>Payment Information</h5>
                <div class="review-item">
                  <span>Card:</span>
                  <span>{{ stepData[3]?.cardNumber ? `****-****-****-${stepData[3].cardNumber.slice(-4)}` : 'Not provided' }}</span>
                </div>
                <div class="review-item">
                  <span>Expiry:</span>
                  <span>{{ stepData[3]?.expiryDate || 'Not provided' }}</span>
                </div>
              </div>

              <div class="terms-checkbox">
                <label>
                  <input v-model="stepData[currentStep].terms" type="checkbox" />
                  <span>I agree to the terms and conditions</span>
                </label>
              </div>
            </div>
          </div>

          <!-- Step Actions -->
          <div class="step-actions">
            <button @click="prevStepWithTracking" :disabled="currentStep === 0" class="btn btn-prev">
              ← Previous
            </button>
            <button
              @click="nextStepWithTracking"
              :disabled="currentStep === steps.length - 1"
              class="btn btn-next"
            >
              {{ currentStep === steps.length - 1 ? '✅ Complete' : 'Next →' }}
            </button>
          </div>
        </div>
      </div>

      <!-- Progress History -->
      <div class="progress-history">
        <h3>📜 Progress History</h3>
        <div class="history-list">
          <div
            v-for="(item, index) in progressHistory"
            :key="index"
            class="history-item"
          >
            <span class="history-time">{{ item.timestamp }}</span>
            <span class="history-action">{{ item.action }}: {{ item.step }}</span>
          </div>
          <div v-if="progressHistory.length === 0" class="no-history">
            No progress history yet
          </div>
        </div>
      </div>

      <!-- Quick Actions -->
      <div class="quick-actions">
        <button @click="resetProgress" class="btn btn-reset">
          🔄 Reset Progress
        </button>
        <button @click="addStep" class="btn btn-add">
          ➕ Add Step
        </button>
        <button @click="removeStep(currentStep)" class="btn btn-remove" :disabled="steps.length <= 1">
          ➖ Remove Current Step
        </button>
      </div>

      <!-- Keyboard Shortcuts -->
      <div class="keyboard-shortcuts">
        <h4>⌨️ Keyboard Shortcuts</h4>
        <div class="shortcuts-list">
          <div class="shortcut-item">
            <kbd>→</kbd>
            <span>Next step</span>
          </div>
          <div class="shortcut-item">
            <kbd>←</kbd>
            <span>Previous step</span>
          </div>
          <div class="shortcut-item">
            <kbd>Enter</kbd>
            <span>Complete wizard</span>
          </div>
        </div>
      </div>
    </main>

    <footer>
      <div class="footer-content">
        <p>💡 Use arrow keys to navigate between steps</p>
        <p>Made with ❤️ using Vue.js 3 Composition API</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: white;
}

header {
  text-align: center;
  padding: 40px 20px 30px;
}

header h1 {
  font-size: 3em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  font-size: 1.1em;
  opacity: 0.9;
}

main {
  max-width: 900px;
  margin: 0 auto;
  padding: 0 20px 40px;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

/* Progress Overview */
.progress-overview {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.overview-header {
  text-align: center;
  margin-bottom: 20px;
}

.overview-header h2 {
  margin-bottom: 15px;
}

.overview-stats {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  gap: 15px;
}

.stat-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  background: rgba(255,255,255,0.1);
  padding: 15px 20px;
  border-radius: 10px;
  min-width: 120px;
}

.stat-label {
  font-size: 0.9em;
  opacity: 0.8;
  margin-bottom: 5px;
}

.stat-value {
  font-size: 1.2em;
  font-weight: bold;
}

.stat-value.completed {
  color: #4CAF50;
}

.stat-value.in-progress {
  color: #FFC107;
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: rgba(255,255,255,0.2);
  border-radius: 5px;
  overflow: hidden;
  margin-top: 15px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #4CAF50, #8BC34A);
  transition: width 0.5s ease;
  border-radius: 5px;
}

/* Progress Steps */
.progress-steps-container {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.steps-visual {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.step-item {
  position: relative;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  padding: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 2px solid transparent;
}

.step-item:hover {
  transform: translateY(-2px);
  background: rgba(255,255,255,0.2);
}

.step-connector {
  position: absolute;
  left: -50%;
  top: 50%;
  width: 100%;
  height: 2px;
  background: rgba(255,255,255,0.3);
  z-index: 1;
}

.step-content {
  display: flex;
  align-items: center;
  gap: 15px;
  position: relative;
  z-index: 2;
}

.step-number {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255,255,255,0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 1em;
  transition: all 0.3s ease;
}

.step-icon {
  font-size: 1.5em;
}

.step-title {
  font-weight: 600;
  font-size: 1.1em;
}

.step-description {
  font-size: 0.9em;
  opacity: 0.8;
}

.step-errors {
  margin-top: 10px;
  color: #ffcccc;
  font-size: 0.85em;
}

.error {
  display: block;
  margin-bottom: 5px;
}

/* Step States */
.step-item.step-active {
  border-color: #4CAF50;
  background: rgba(76, 175, 80, 0.2);
  transform: scale(1.02);
}

.step-item.step-active .step-number {
  background: #4CAF50;
  color: white;
}

.step-item.step-completed {
  border-color: #2196F3;
  background: rgba(33, 150, 243, 0.1);
}

.step-item.step-completed .step-number {
  background: #2196F3;
  color: white;
}

.step-completed {
  color: #4CAF50;
}

/* Step Content Area */
.step-content-area {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.step-header {
  text-align: center;
  margin-bottom: 30px;
}

.step-header h2 {
  margin-bottom: 10px;
  font-size: 2em;
}

.step-header p {
  opacity: 0.8;
}

.step-form {
  max-width: 500px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 600;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  color: white;
  font-size: 16px;
}

.form-group input::placeholder,
.form-group select::placeholder {
  color: rgba(255,255,255,0.6);
}

.form-group input:focus,
.form-group select:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.3);
}

.step-actions {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-top: 30px;
}

.btn {
  padding: 15px 30px;
  font-size: 1.1em;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
}

.btn-prev {
  background: #f44336;
}

.btn-next {
  background: #4CAF50;
}

/* Confirmation Content */
.confirmation-content {
  max-width: 600px;
  margin: 0 auto;
}

.review-section {
  margin-bottom: 25px;
  padding: 20px;
  background: rgba(255,255,255,0.1);
  border-radius: 10px;
}

.review-section h5 {
  margin-bottom: 15px;
  color: #4CAF50;
}

.review-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  padding: 10px 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.review-item:last-child {
  border-bottom: none;
}

.terms-checkbox {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 20px;
  padding: 15px;
  background: rgba(255,255,255,0.1);
  border-radius: 8px;
}

/* Progress History */
.progress-history {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 20px;
  border: 1px solid rgba(255,255,255,0.2);
}

.progress-history h3 {
  margin-bottom: 15px;
}

.history-list {
  max-height: 200px;
  overflow-y: auto;
}

.history-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 10px;
  background: rgba(255,255,255,0.05);
  border-radius: 5px;
  margin-bottom: 5px;
}

.history-time {
  font-size: 0.8em;
  color: rgba(255,255,255,0.7);
}

.history-action {
  font-weight: 600;
}

.no-history {
  text-align: center;
  color: rgba(255,255,255,0.6);
  font-style: italic;
}

/* Quick Actions */
.quick-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}

.btn-reset {
  background: #FF9800;
}

.btn-add {
  background: #2196F3;
}

.btn-remove {
  background: #f44336;
}

.btn-remove:disabled {
  background: #666;
}

/* Keyboard Shortcuts */
.keyboard-shortcuts {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 20px;
  border: 1px solid rgba(255,255,255,0.2);
}

.keyboard-shortcuts h4 {
  margin-bottom: 15px;
  text-align: center;
}

.shortcuts-list {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
  justify-content: center;
}

.shortcut-item {
  display: flex;
  align-items: center;
  gap: 10px;
  background: rgba(255,255,255,0.1);
  padding: 8px 15px;
  border-radius: 8px;
}

kbd {
  background: rgba(255,255,255,0.2);
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 4px;
  padding: 4px 8px;
  font-family: monospace;
  font-size: 0.9em;
}

/* Footer */
footer {
  text-align: center;
  padding: 30px 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

.footer-content p {
  margin-bottom: 5px;
  opacity: 0.8;
}

/* Responsive Design */
@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .overview-stats {
    flex-direction: column;
    gap: 10px;
  }

  .step-content {
    flex-direction: column;
    gap: 10px;
  }

  .step-actions {
    flex-direction: column;
  }

  .quick-actions {
    flex-direction: column;
  }

  .btn {
    width: 100%;
  }
}
</style>

<script>
// Global keyboard event listener
document.addEventListener('keydown', (event) => {
  // The component will handle keyboard events
});
</script>
```

### 📋 Step 4: Jalankan Aplikasi (1 Menit)

```bash
# Start development server
npm run dev
```

**Buka browser:** `http://localhost:5173`

🎉 **Selesai! Aplikasi Advanced Progress Steps Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis
3. **`v-model`** - Two-way data binding
4. **`@click`** - Event handling
5. **`v-for`** - List rendering
6. **`:class`** - Dynamic class binding

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed } from "vue";
const steps = ref([...]);      // Array of steps
const currentStep = ref(0);      // Current step tracker
const stepData = ref({});        // Data per step

// 2. Computed Properties
const progressPercentage = computed(() => { /* calculate progress */ });

// 3. Functions
const nextStep = () => { /* next step logic */ };
const prevStep = () => { /* previous step logic */ };
const goToStep = () => { /* direct navigation */ };
</script>

<template>
<!-- 4. Tampilan HTML -->
</template>

<style scoped>
/* 5. Styling CSS */
</style>
```

### 🔄 Logika Navigation

```javascript
const nextStep = () => {
  // Check if we can move forward
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++;
  }
};

// Auto-complete when reaching last step
if (currentStep.value === steps.value.length - 1) {
  isCompleted.value = true;
}
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Progress Persistence
```vue
<script setup>
// Save progress to localStorage
watch(currentStep, (newStep) => {
  localStorage.setItem('currentStep', newStep.toString());
});

// Load saved progress
const loadProgress = () => {
  const saved = localStorage.getItem('currentStep');
  if (saved) currentStep.value = parseInt(saved);
};

onMounted(loadProgress);
</script>
```

### 2. Step Validation
```vue
<script setup>
const stepValidation = ref({
  0: { isValid: false, errors: [] },
  // ... other steps
});

const validateCurrentStep = () => {
  // Validation logic for current step
  const validation = stepValidation.value[currentStep.value];
  // Perform validation and update errors
};
</script>
```

### 3. Step Completion Callbacks
```vue
<script setup>
const stepCallbacks = {
  0: () => console.log('Personal info step started'),
  1: () => console.log('Account setup step started'),
  // ... other callbacks
};

watch(currentStep, (newStep) => {
  if (stepCallbacks[newStep]) {
    stepCallbacks[newStep]();
  }
});
</script>
```

### 4. Animated Transitions
```vue
<script setup>
import { ref } from 'vue';

const isTransitioning = ref(false);

const animateStepChange = async (from, to) => {
  isTransitioning.value = true;
  // Animation logic
  setTimeout(() => {
    isTransitioning.value = false;
  }, 300);
};
</script>
```

### 5. Multiple Progress Tracks
```vue
<script setup>
const tracks = ref({
  main: { steps: ['Setup', 'Deploy'], current: 0 },
  secondary: { steps: ['Test', 'Verify'], current: 0 }
});

const activeTrack = ref('main');
</script>
```

## 🎨 Customization Ideas

### 1. Theme Variations
```css
/* Dark Theme */
.dark-theme {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

/* Neon Theme */
.neon-theme {
  background: #0a0a0a;
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.5);
}
```

### 2. Animation Libraries
```vue
<script setup>
import { useTransition } from '@vueuse/core';

const slideTransition = useTransition(currentStep, {
  duration: 300,
  easing: 'ease-in-out'
});
</script>
```

### 3. Progress Sound Effects
```vue
<script setup>
const playSound = (type) => {
  const audio = new Audio(`/sounds/${type}.mp3`);
  audio.play().catch(e => console.log('Sound play failed:', e));
};

const nextStep = () => {
  playSound('success');
  // ... existing logic
};
</script>
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Navigation tidak berfungsi | Cek `ref()` dan event handlers |
| Progress tidak update | Pastikan computed properties terhubung |
| Styling berantakan | Cek CSS selectors dan specificity |
| Keyboard shortcuts tidak bekerja | Pastikan event listener terpasang |

## 🚀 Next Steps

1. **Multi-step Forms** - Integrate dengan form data
2. **API Integration** - Save progress ke server
3. **Validation Logic** - Add comprehensive validation
4. **Progress Persistence** - Auto-save and resume
5. **Multiple Tracks** - Parallel progress tracking
6. **Mobile Optimization** - Touch-friendly interface

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Progress Steps yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) | [VueUse](https://vueuse.org/)