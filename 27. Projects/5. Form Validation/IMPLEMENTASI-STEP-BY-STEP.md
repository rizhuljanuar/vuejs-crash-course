# 📚 Implementasi Step-by-Step Form Validation Vue.js

## 🎯 Panduan Detail Membuat Form Validation dari Nol

### 📋 Step 1: Setup Environment Development

#### 1.1 Install Node.js
```bash
# Download Node.js dari https://nodejs.org/
# Pilih versi LTS (Recommended for Most Users)

# Verifikasi instalasi
node --version
npm --version
```

#### 1.2 Buat Project Vue dengan Vite
```bash
# Buat project baru
npm create vue@latest form-validation-app

# Saat ditanya, pilih:
# ✅ TypeScript? No
# ✅ JSX Support? No
# ✅ Vue Router? No
# ✅ Pinia? No
# ✅ Vitest? No
# ✅ E2E Testing? No
# ✅ ESLint? Yes
# ✅ Prettier? Yes

# Masuk ke folder project
cd form-validation-app

# Install dependencies
npm install
```

#### 1.3 Jalankan Development Server
```bash
npm run dev
```
**Result:** Aplikasi Vue default berjalan di `http://localhost:5173`

---

### 📋 Step 2: Persiapan Struktur Project

#### 2.1 Lihat Struktur Folder
```
form-validation-app/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── App.vue
│   └── main.js
├── package.json
└── README.md
```

#### 2.2 Bersihkan Default Project
**Edit `src/App.vue`:**
```vue
<!-- Hapus semua isi dan ganti dengan template dasar -->
<template>
  <div id="app">
    <h1>📝 Form Validation</h1>
    <p>Membuat form validation dengan Vue.js</p>
  </div>
</template>

<script setup>
// Kosongkan script untuk sementara
</script>

<style>
/* Kosongkan style untuk sementara */
</style>
```

**Result:** Aplikasi sekarang menampilkan judul "Form Validation"

---

### 📋 Step 3: Implementasi Dasar Form Data

#### 3.1 Tambahkan Reactive Form Data
**Edit `src/App.vue` - Script section:**
```vue
<script setup>
import { ref } from "vue";

// 1. Reactive form data object
const formData = ref({
  name: "",
  email: "",
  password: "",
});

// 2. Form state
const isSubmitting = ref(false);
const submitSuccess = ref(false);

// 3. Debug info
const debugInfo = ref({
  formDataValue: {},
  formLength: 0,
  isDirty: false
});

// 4. Watch form data changes
const updateDebugInfo = () => {
  debugInfo.value = {
    formDataValue: { ...formData.value },
    formLength: Object.keys(formData.value).length,
    isDirty: Object.values(formData.value).some(value => value !== "")
  };
};

// Call debug update
updateDebugInfo();
</script>
```

#### 3.2 Tampilkan Form Data di UI
**Edit `src/App.vue` - Template section:**
```vue
<template>
  <div id="app">
    <h1>📝 Form Validation</h1>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Form Data Keys: {{ debugInfo.formLength }}</p>
      <p>Is Dirty: {{ debugInfo.isDirty ? 'Yes' : 'No' }}</p>

      <div class="data-preview">
        <h4>Current Form Data:</h4>
        <pre>{{ JSON.stringify(debugInfo.formDataValue, null, 2) }}</pre>
      </div>
    </div>

    <!-- Basic Form Structure -->
    <div class="form-container">
      <h3>Form Structure:</h3>
      <form @submit.prevent="console.log('Form submitted!')" class="basic-form">
        <div class="form-group">
          <label for="name">Name:</label>
          <input
            v-model="formData.name"
            type="text"
            id="name"
            placeholder="Enter your name"
          />
          <small>Current value: "{{ formData.name }}"</small>
        </div>

        <div class="form-group">
          <label for="email">Email:</label>
          <input
            v-model="formData.email"
            type="email"
            id="email"
            placeholder="Enter your email"
          />
          <small>Current value: "{{ formData.email }}"</small>
        </div>

        <div class="form-group">
          <label for="password">Password:</label>
          <input
            v-model="formData.password"
            type="password"
            id="password"
            placeholder="Enter your password"
          />
          <small>Current value: "{{ formData.password ? '***' : '' }}"</small>
        </div>

        <button type="submit" class="submit-btn">
          Submit Form
        </button>
      </form>
    </div>
  </div>
</template>
```

#### 3.3 Tambah Basic Styling
**Edit `src/App.vue` - Style section:**
```vue
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  padding: 20px;
}

#app {
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

/* Debug Section */
.debug-section {
  background: #e8f4f8;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.debug-section h3 {
  margin-bottom: 15px;
  color: #2c3e50;
}

.data-preview pre {
  background: #2c3e50;
  color: #ecf0f1;
  padding: 15px;
  border-radius: 5px;
  font-size: 0.8em;
  overflow-x: auto;
}

/* Form Container */
.form-container {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.form-container h3 {
  margin-bottom: 20px;
  color: #333;
}

/* Basic Form Styles */
.basic-form {
  max-width: 500px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
  color: #333;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 5px;
}

input:focus {
  outline: none;
  border-color: #3498db;
}

small {
  color: #666;
  font-size: 0.85em;
}

.submit-btn {
  background: #3498db;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.submit-btn:hover {
  background: #2980b9;
}
</style>
```

**Result:** Sekarang Anda bisa melihat form structure dan real-time data binding!

---

### 📋 Step 4: Implementasi Dasar Validation

#### 4.1 Tambah Validation Functions
**Edit `src/App.vue` - Tambah validation logic:**
```vue
<script setup>
import { ref, computed } from "vue";

// ... (formData dari step sebelumnya)

// 1. Validation functions
const isNameValid = computed(() => {
  return formData.value.name.trim().length > 0;
});

const isEmailValid = computed(() => {
  const email = formData.value.email.trim();
  return email !== "" && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
});

const isPasswordValid = computed(() => {
  return formData.value.password.length >= 8;
});

// 2. Overall form validity
const isFormValid = computed(() => {
  return isNameValid.value && isEmailValid.value && isPasswordValid.value;
});

// 3. Validation results object
const validationResults = computed(() => ({
  name: {
    isValid: isNameValid.value,
    message: isNameValid.value ? "" : "Name is required"
  },
  email: {
    isValid: isEmailValid.value,
    message: isEmailValid.value ? "" : "Please enter a valid email"
  },
  password: {
    isValid: isPasswordValid.value,
    message: isPasswordValid.value ? "" : "Password must be at least 8 characters"
  }
}));

// 4. Enhanced debug info
const debugInfo = computed(() => ({
  formData: { ...formData.value },
  validation: { ...validationResults.value },
  formValid: isFormValid.value,
  hasErrors: Object.values(validationResults.value).some(result => !result.isValid)
}));

// 5. Form submission
const submitForm = () => {
  if (isFormValid.value) {
    console.log("Form submitted successfully!", formData.value);
    submitSuccess.value = true;
    setTimeout(() => submitSuccess.value = false, 3000);
  } else {
    console.log("Form has validation errors");
  }
};
</script>
```

#### 4.2 Update Template dengan Validation
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📝 Form Validation</h1>

    <!-- Validation Status -->
    <div class="validation-status">
      <h3>Validation Status:</h3>
      <div class="status-grid">
        <div :class="['status-item', isNameValid ? 'valid' : 'invalid']">
          <span>Name: {{ isNameValid ? '✅ Valid' : '❌ Invalid' }}</span>
        </div>
        <div :class="['status-item', isEmailValid ? 'valid' : 'invalid']">
          <span>Email: {{ isEmailValid ? '✅ Valid' : '❌ Invalid' }}</span>
        </div>
        <div :class="['status-item', isPasswordValid ? 'valid' : 'invalid']">
          <span>Password: {{ isPasswordValid ? '✅ Valid' : '❌ Invalid' }}</span>
        </div>
        <div :class="['status-item', isFormValid ? 'valid' : 'invalid']">
          <strong>Overall: {{ isFormValid ? '✅ Ready' : '❌ Not Ready' }}</strong>
        </div>
      </div>
    </div>

    <!-- Enhanced Form -->
    <div class="form-container">
      <h3>Enhanced Form with Validation:</h3>
      <form @submit.prevent="submitForm" class="validated-form">
        <div class="form-group" :class="{ 'has-error': !isNameValid && formData.name !== '' }">
          <label for="name">Name:</label>
          <input
            v-model="formData.name"
            type="text"
            id="name"
            placeholder="Enter your name"
            :class="{ 'input-error': !isNameValid && formData.name !== '' }"
          />
          <div v-if="!isNameValid && formData.name !== ''" class="error-message">
            {{ validationResults.name.message }}
          </div>
        </div>

        <div class="form-group" :class="{ 'has-error': !isEmailValid && formData.email !== '' }">
          <label for="email">Email:</label>
          <input
            v-model="formData.email"
            type="email"
            id="email"
            placeholder="Enter your email"
            :class="{ 'input-error': !isEmailValid && formData.email !== '' }"
          />
          <div v-if="!isEmailValid && formData.email !== ''" class="error-message">
            {{ validationResults.email.message }}
          </div>
        </div>

        <div class="form-group" :class="{ 'has-error': !isPasswordValid && formData.password !== '' }">
          <label for="password">Password:</label>
          <input
            v-model="formData.password"
            type="password"
            id="password"
            placeholder="Enter your password"
            :class="{ 'input-error': !isPasswordValid && formData.password !== '' }"
          />
          <div v-if="!isPasswordValid && formData.password !== ''" class="error-message">
            {{ validationResults.password.message }}
          </div>
        </div>

        <div class="form-actions">
          <button type="submit" :disabled="!isFormValid" class="submit-btn">
            {{ isFormValid ? '✅ Submit Form' : '📝 Complete Required Fields' }}
          </button>
        </div>

        <!-- Success Message -->
        <div v-if="submitSuccess" class="success-message">
          🎉 Form submitted successfully!
        </div>
      </form>
    </div>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Form Valid: <strong>{{ debugInfo.formValid }}</strong></p>
      <p>Has Errors: <strong>{{ debugInfo.hasErrors }}</strong></p>
      <div class="data-preview">
        <h4>Validation Results:</h4>
        <pre>{{ JSON.stringify(debugInfo.validation, null, 2) }}</pre>
      </div>
    </div>
  </div>
</template>
```

#### 4.3 Tambah Validation Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Validation Status */
.validation-status {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.validation-status h3 {
  margin-bottom: 15px;
  color: #333;
}

.status-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.status-item {
  padding: 10px;
  border-radius: 5px;
  font-weight: 600;
}

.status-item.valid {
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.status-item.invalid {
  background: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

/* Enhanced Form Styles */
.validated-form {
  max-width: 500px;
}

.form-group.has-error input {
  border-color: #dc3545;
  box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.2);
}

.input-error {
  animation: shake 0.3s ease-in-out;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}

.error-message {
  color: #dc3545;
  font-size: 0.85em;
  margin-top: 5px;
  font-weight: 500;
}

.form-actions {
  margin-top: 30px;
}

.submit-btn {
  width: 100%;
  padding: 15px;
  font-size: 18px;
  font-weight: 600;
  background: #28a745;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-btn:hover:not(:disabled) {
  background: #218838;
  transform: translateY(-2px);
}

.submit-btn:disabled {
  background: #6c757d;
  cursor: not-allowed;
  transform: none;
}

.success-message {
  margin-top: 20px;
  padding: 15px;
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
  border-radius: 6px;
  text-align: center;
  font-weight: 600;
}
</style>
```

**Result:** Form sekarang memiliki real-time validation dengan visual feedback!

---

### 📋 Step 5: Implementasi Advanced Validation Rules

#### 5.1 Tambah Validation Configuration
**Edit `src/App.vue` - Tambah validation rules:**
```vue
<script setup>
import { ref, computed } from "vue";

// ... (formData dari step sebelumnya)

// 1. Validation rules configuration
const validationRules = {
  name: {
    required: true,
    minLength: 2,
    maxLength: 50,
    pattern: /^[a-zA-Z\s]+$/
  },
  email: {
    required: true,
    pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  password: {
    required: true,
    minLength: 8,
    maxLength: 128,
    pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/
  }
};

// 2. Enhanced validation functions
const validateName = computed(() => {
  const name = formData.value.name.trim();
  const rules = validationRules.name;

  if (rules.required && !name) {
    return { isValid: false, message: "Name is required" };
  }

  if (name.length < rules.minLength) {
    return { isValid: false, message: `Name must be at least ${rules.minLength} characters` };
  }

  if (name.length > rules.maxLength) {
    return { isValid: false, message: `Name must be less than ${rules.maxLength} characters` };
  }

  if (rules.pattern && !rules.pattern.test(name)) {
    return { isValid: false, message: "Name can only contain letters and spaces" };
  }

  return { isValid: true, message: "" };
});

const validateEmail = computed(() => {
  const email = formData.value.email.trim();
  const rules = validationRules.email;

  if (rules.required && !email) {
    return { isValid: false, message: "Email is required" };
  }

  if (rules.pattern && !rules.pattern.test(email)) {
    return { isValid: false, message: "Please enter a valid email address" };
  }

  return { isValid: true, message: "" };
});

const validatePassword = computed(() => {
  const password = formData.value.password;
  const rules = validationRules.password;

  if (rules.required && !password) {
    return { isValid: false, message: "Password is required" };
  }

  if (password.length < rules.minLength) {
    return { isValid: false, message: `Password must be at least ${rules.minLength} characters` };
  }

  if (password.length > rules.maxLength) {
    return { isValid: false, message: `Password must be less than ${rules.maxLength} characters` };
  }

  if (rules.pattern && !rules.pattern.test(password)) {
    return { isValid: false, message: "Password must contain uppercase, lowercase, and numbers" };
  }

  return { isValid: true, message: "" };
});

// 3. Password strength calculation
const passwordStrength = computed(() => {
  const password = formData.value.password;
  if (!password) return 0;

  let strength = 0;

  // Length
  if (password.length >= 8) strength++;
  if (password.length >= 12) strength++;

  // Character types
  if (/[a-z]/.test(password)) strength++;
  if (/[A-Z]/.test(password)) strength++;
  if (/\d/.test(password)) strength++;
  if (/[^a-zA-Z0-9]/.test(password)) strength++;

  return Math.min(strength, 5);
});

const passwordStrengthText = computed(() => {
  const strength = passwordStrength.value;
  if (strength <= 2) return "Weak";
  if (strength <= 3) return "Medium";
  return "Strong";
});

// 4. Overall form validity
const isFormValid = computed(() => {
  return validateName.value.isValid &&
         validateEmail.value.isValid &&
         validatePassword.value.isValid;
});

// 5. Validation results object
const validationResults = computed(() => ({
  name: validateName.value,
  email: validateEmail.value,
  password: validatePassword.value
}));
</script>
```

#### 5.2 Update Template dengan Advanced Validation
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📝 Advanced Form Validation</h1>

    <!-- Password Strength Indicator -->
    <div class="password-strength-section">
      <h3>🔒 Password Strength:</h3>
      <div class="strength-meter">
        <div class="strength-bar">
          <div
            class="strength-fill"
            :class="[
              passwordStrength <= 2 ? 'weak' :
              passwordStrength <= 3 ? 'medium' : 'strong'
            ]"
            :style="{ width: (passwordStrength * 20) + '%' }"
          ></div>
        </div>
        <span class="strength-text">
          {{ passwordStrengthText }} ({{ passwordStrength }}/5)
        </span>
      </div>
    </div>

    <!-- Enhanced Form -->
    <div class="form-container">
      <h3>Advanced Validation Rules:</h3>
      <form @submit.prevent="submitForm" class="advanced-form">
        <div class="form-group" :class="{ 'has-error': !validationResults.name.isValid && formData.name !== '' }">
          <label for="name">
            Name:
            <span class="char-count">({{ formData.name.length }}/50)</span>
          </label>
          <input
            v-model.trim="formData.name"
            type="text"
            id="name"
            placeholder="Enter your full name"
            maxlength="50"
            :class="{ 'input-error': !validationResults.name.isValid && formData.name !== '' }"
          />
          <div v-if="formData.name !== ''" class="validation-feedback">
            <span v-if="validationResults.name.isValid" class="valid-feedback">✅ Valid name</span>
            <span v-else class="error-message">{{ validationResults.name.message }}</span>
          </div>
        </div>

        <div class="form-group" :class="{ 'has-error': !validationResults.email.isValid && formData.email !== '' }">
          <label for="email">Email:</label>
          <input
            v-model.trim="formData.email"
            type="email"
            id="email"
            placeholder="your.email@example.com"
            :class="{ 'input-error': !validationResults.email.isValid && formData.email !== '' }"
          />
          <div v-if="formData.email !== ''" class="validation-feedback">
            <span v-if="validationResults.email.isValid" class="valid-feedback">✅ Valid email</span>
            <span v-else class="error-message">{{ validationResults.email.message }}</span>
          </div>
        </div>

        <div class="form-group" :class="{ 'has-error': !validationResults.password.isValid && formData.password !== '' }">
          <label for="password">
            Password:
            <span class="char-count">({{ formData.password.length }}/128)</span>
          </label>
          <input
            v-model="formData.password"
            type="password"
            id="password"
            placeholder="Enter your password"
            maxlength="128"
            :class="{ 'input-error': !validationResults.password.isValid && formData.password !== '' }"
          />

          <!-- Password Requirements -->
          <div class="password-requirements">
            <h4>Password Requirements:</h4>
            <ul>
              <li :class="{ 'requirement-met': formData.password.length >= 8 }">
                {{ formData.password.length >= 8 ? '✅' : '❌' }} At least 8 characters
              </li>
              <li :class="{ 'requirement-met': /[A-Z]/.test(formData.password) }">
                {{ /[A-Z]/.test(formData.password) ? '✅' : '❌' }} One uppercase letter
              </li>
              <li :class="{ 'requirement-met': /[a-z]/.test(formData.password) }">
                {{ /[a-z]/.test(formData.password) ? '✅' : '❌' }} One lowercase letter
              </li>
              <li :class="{ 'requirement-met': /\d/.test(formData.password) }">
                {{ /\d/.test(formData.password) ? '✅' : '❌' }} One number
              </li>
            </ul>
          </div>

          <div v-if="formData.password !== ''" class="validation-feedback">
            <span v-if="validationResults.password.isValid" class="valid-feedback">✅ Strong password</span>
            <span v-else class="error-message">{{ validationResults.password.message }}</span>
          </div>
        </div>

        <div class="form-actions">
          <button type="button" @click="resetForm" class="reset-btn">
            🔄 Reset Form
          </button>
          <button type="submit" :disabled="!isFormValid" class="submit-btn">
            {{ isFormValid ? '🚀 Submit Form' : '📝 Complete Required Fields' }}
          </button>
        </div>

        <!-- Success Message -->
        <div v-if="submitSuccess" class="success-message">
          🎉 Form submitted successfully! Data: {{ JSON.stringify(formData) }}
        </div>
      </form>
    </div>

    <!-- Validation Summary -->
    <div class="validation-summary">
      <h3>📊 Validation Summary:</h3>
      <div class="summary-grid">
        <div class="summary-item">
          <span class="field-name">Name</span>
          <span :class="['field-status', validationResults.name.isValid ? 'valid' : 'invalid']">
            {{ validationResults.name.isValid ? '✅' : '❌' }}
          </span>
          <span class="field-message">{{ validationResults.name.message || 'Valid' }}</span>
        </div>
        <div class="summary-item">
          <span class="field-name">Email</span>
          <span :class="['field-status', validationResults.email.isValid ? 'valid' : 'invalid']">
            {{ validationResults.email.isValid ? '✅' : '❌' }}
          </span>
          <span class="field-message">{{ validationResults.email.message || 'Valid' }}</span>
        </div>
        <div class="summary-item">
          <span class="field-name">Password</span>
          <span :class="['field-status', validationResults.password.isValid ? 'valid' : 'invalid']">
            {{ validationResults.password.isValid ? '✅' : '❌' }}
          </span>
          <span class="field-message">{{ validationResults.password.message || 'Valid' }}</span>
        </div>
      </div>
      <div class="overall-status">
        <span>Overall Form Status:</span>
        <span :class="['overall-status-text', isFormValid ? 'valid' : 'invalid']">
          {{ isFormValid ? '✅ Ready to Submit' : '❌ Validation Required' }}
        </span>
      </div>
    </div>
  </div>
</template>
```

#### 5.3 Tambah Advanced Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Password Strength Section */
.password-strength-section {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.password-strength-section h3 {
  margin-bottom: 15px;
  color: #333;
}

.strength-meter {
  max-width: 300px;
}

.strength-bar {
  width: 100%;
  height: 8px;
  background: #e9ecef;
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.strength-fill {
  height: 100%;
  transition: all 0.3s ease;
}

.strength-fill.weak {
  background: #dc3545;
}

.strength-fill.medium {
  background: #ffc107;
}

.strength-fill.strong {
  background: #28a745;
}

.strength-text {
  font-size: 0.9em;
  font-weight: 600;
}

.char-count {
  font-size: 0.8em;
  color: #6c757d;
  margin-left: 10px;
}

/* Password Requirements */
.password-requirements {
  margin-top: 15px;
  padding: 15px;
  background: #f8f9fa;
  border-radius: 6px;
  border: 1px solid #e9ecef;
}

.password-requirements h4 {
  margin-bottom: 10px;
  font-size: 0.9em;
  color: #495057;
}

.password-requirements ul {
  list-style: none;
  padding: 0;
}

.password-requirements li {
  font-size: 0.85em;
  margin-bottom: 5px;
  color: #6c757d;
}

.requirement-met {
  color: #28a745 !important;
  font-weight: 600;
}

/* Validation Feedback */
.validation-feedback {
  margin-top: 8px;
  min-height: 20px;
}

.valid-feedback {
  color: #28a745;
  font-size: 0.85em;
  font-weight: 600;
}

/* Form Actions */
.form-actions {
  display: flex;
  gap: 15px;
  margin-top: 30px;
}

.reset-btn {
  flex: 1;
  padding: 15px;
  font-size: 16px;
  font-weight: 600;
  background: #6c757d;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.reset-btn:hover {
  background: #5a6268;
  transform: translateY(-2px);
}

/* Validation Summary */
.validation-summary {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.validation-summary h3 {
  margin-bottom: 15px;
  color: #333;
}

.summary-grid {
  display: grid;
  gap: 10px;
  margin-bottom: 20px;
}

.summary-item {
  display: grid;
  grid-template-columns: 100px 40px 1fr;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e9ecef;
}

.field-name {
  font-weight: 600;
  color: #495057;
}

.field-status {
  font-weight: bold;
  text-align: center;
}

.field-status.valid {
  color: #28a745;
}

.field-status.invalid {
  color: #dc3545;
}

.field-message {
  font-size: 0.85em;
  color: #6c757d;
}

.overall-status {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e9ecef;
  font-weight: 600;
}

.overall-status-text {
  font-weight: bold;
}

.overall-status-text.valid {
  color: #28a745;
}

.overall-status-text.invalid {
  color: #dc3545;
}

/* Enhanced Success Message */
.success-message {
  margin-top: 20px;
  padding: 20px;
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
  border-radius: 6px;
  text-align: center;
  font-weight: 600;
  word-break: break-all;
}
</style>
```

#### 5.4 Tambah Reset Function
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
// ... (kode sebelumnya)

// Reset form function
const resetForm = () => {
  formData.value = {
    name: "",
    email: "",
    password: ""
  };
  submitSuccess.value = false;
};

// Enhanced submit function
const submitForm = () => {
  if (isFormValid.value) {
    console.log("Form submitted successfully!", formData.value);
    submitSuccess.value = true;

    // Reset setelah 3 detik
    setTimeout(() => {
      resetForm();
      submitSuccess.value = false;
    }, 3000);
  } else {
    console.log("Form has validation errors");
  }
};
</script>
```

**Result:** Form sekarang memiliki advanced validation dengan password strength indicator!

---

### 📋 Step 6: Implementasi Form Field Interactions

#### 6.1 Tambah Field Interaction Tracking
**Edit `src/App.vue` - Tambah interaction tracking:**
```vue
<script setup>
import { ref, computed } from "vue";

// ... (formData dan validation dari step sebelumnya)

// 1. Field interaction tracking
const touchedFields = ref({
  name: false,
  email: false,
  password: false
});

const fieldErrors = ref({
  name: false,
  email: false,
  password: false
});

// 2. Touch field function
const touchField = (fieldName) => {
  touchedFields.value[fieldName] = true;
};

// 3. Enhanced validation with field tracking
const showNameError = computed(() => {
  return touchedFields.value.name && !validationResults.value.name.isValid;
});

const showEmailError = computed(() => {
  return touchedFields.value.email && !validationResults.value.email.isValid;
});

const showPasswordError = computed(() => {
  return touchedFields.value.password && !validationResults.value.password.isValid;
});

// 4. Field focus tracking
const focusedField = ref(null);

const focusField = (fieldName) => {
  focusedField.value = fieldName;
};

const blurField = (fieldName) => {
  focusedField.value = null;
  touchField(fieldName);
};

// 5. Form statistics
const formStats = computed(() => {
  const total = Object.keys(touchedFields.value).length;
  const touched = Object.values(touchedFields.value).filter(Boolean).length;
  const valid = Object.values(validationResults.value).filter(result => result.isValid).length;

  return {
    total,
    touched,
    valid,
    completion: Math.round((valid / total) * 100)
  };
});

// 6. Field blur validation
const validateFieldOnBlur = (fieldName) => {
  const validationResult = validationResults.value[fieldName];
  fieldErrors.value[fieldName] = !validationResult.isValid;
};
</script>
```

#### 6.2 Update Template dengan Interactions
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📝 Interactive Form Validation</h1>

    <!-- Form Statistics -->
    <div class="form-stats">
      <h3>📊 Form Statistics:</h3>
      <div class="stats-grid">
        <div class="stat-item">
          <span class="stat-label">Fields Touched:</span>
          <span class="stat-value">{{ formStats.touched }}/{{ formStats.total }}</span>
        </div>
        <div class="stat-item">
          <span class="stat-label">Fields Valid:</span>
          <span class="stat-value">{{ formStats.valid }}/{{ formStats.total }}</span>
        </div>
        <div class="stat-item">
          <span class="stat-label">Completion:</span>
          <span class="stat-value">{{ formStats.completion }}%</span>
        </div>
        <div class="stat-item">
          <span class="stat-label">Status:</span>
          <span :class="['stat-value', isFormValid ? 'valid' : 'invalid']">
            {{ isFormValid ? '✅ Ready' : '❌ Incomplete' }}
          </span>
        </div>
      </div>

      <!-- Progress Bar -->
      <div class="progress-bar">
        <div
          class="progress-fill"
          :style="{ width: formStats.completion + '%' }"
        ></div>
      </div>
    </div>

    <!-- Enhanced Interactive Form -->
    <div class="form-container">
      <h3>Interactive Form with Field Tracking:</h3>
      <form @submit.prevent="submitForm" class="interactive-form">
        <!-- Name Field -->
        <div
          class="form-group"
          :class="{
            'has-error': showNameError,
            'is-focused': focusedField === 'name',
            'is-touched': touchedFields.name
          }"
        >
          <label for="name">
            Name:
            <span class="required">*</span>
            <span v-if="touchedFields.name" class="touch-indicator">✋</span>
          </label>
          <input
            v-model.trim="formData.name"
            @focus="focusField('name')"
            @blur="blurField('name')"
            type="text"
            id="name"
            placeholder="Enter your full name"
            maxlength="50"
            :class="{
              'input-error': showNameError,
              'input-focused': focusedField === 'name'
            }"
          />

          <div class="field-feedback">
            <!-- Character count -->
            <div class="char-count">
              {{ formData.name.length }}/50 characters
            </div>

            <!-- Error message -->
            <div v-if="showNameError" class="error-message">
              {{ validationResults.name.message }}
            </div>

            <!-- Success message -->
            <div v-else-if="touchedFields.name && validationResults.name.isValid" class="success-message">
              ✅ Valid name
            </div>

            <!-- Hint message -->
            <div v-else-if="focusedField === 'name'" class="hint-message">
              💡 Enter your full name (letters and spaces only)
            </div>
          </div>
        </div>

        <!-- Email Field -->
        <div
          class="form-group"
          :class="{
            'has-error': showEmailError,
            'is-focused': focusedField === 'email',
            'is-touched': touchedFields.email
          }"
        >
          <label for="email">
            Email:
            <span class="required">*</span>
            <span v-if="touchedFields.email" class="touch-indicator">✋</span>
          </label>
          <input
            v-model.trim="formData.email"
            @focus="focusField('email')"
            @blur="blurField('email')"
            type="email"
            id="email"
            placeholder="your.email@example.com"
            :class="{
              'input-error': showEmailError,
              'input-focused': focusedField === 'email'
            }"
          />

          <div class="field-feedback">
            <!-- Error message -->
            <div v-if="showEmailError" class="error-message">
              {{ validationResults.email.message }}
            </div>

            <!-- Success message -->
            <div v-else-if="touchedFields.email && validationResults.email.isValid" class="success-message">
              ✅ Valid email
            </div>

            <!-- Hint message -->
            <div v-else-if="focusedField === 'email'" class="hint-message">
              💡 Enter a valid email address (example@domain.com)
            </div>
          </div>
        </div>

        <!-- Password Field -->
        <div
          class="form-group"
          :class="{
            'has-error': showPasswordError,
            'is-focused': focusedField === 'password',
            'is-touched': touchedFields.password
          }"
        >
          <label for="password">
            Password:
            <span class="required">*</span>
            <span v-if="touchedFields.password" class="touch-indicator">✋</span>
          </label>
          <input
            v-model="formData.password"
            @focus="focusField('password')"
            @blur="blurField('password')"
            type="password"
            id="password"
            placeholder="Enter your password"
            maxlength="128"
            :class="{
              'input-error': showPasswordError,
              'input-focused': focusedField === 'password'
            }"
          />

          <!-- Password Strength -->
          <div v-if="formData.password" class="password-strength">
            <div class="strength-bar">
              <div
                class="strength-fill"
                :class="[
                  passwordStrength <= 2 ? 'weak' :
                  passwordStrength <= 3 ? 'medium' : 'strong'
                ]"
                :style="{ width: (passwordStrength * 20) + '%' }"
              ></div>
            </div>
            <span class="strength-text">
              Strength: {{ passwordStrengthText }}
            </span>
          </div>

          <div class="field-feedback">
            <!-- Error message -->
            <div v-if="showPasswordError" class="error-message">
              {{ validationResults.password.message }}
            </div>

            <!-- Success message -->
            <div v-else-if="touchedFields.password && validationResults.password.isValid" class="success-message">
              ✅ Strong password
            </div>

            <!-- Hint message -->
            <div v-else-if="focusedField === 'password'" class="hint-message">
              💡 Use uppercase, lowercase, and numbers for strong password
            </div>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="form-actions">
          <button type="button" @click="resetForm" class="reset-btn">
            🔄 Reset Form
          </button>
          <button type="submit" :disabled="!isFormValid" class="submit-btn">
            <span v-if="isFormValid">🚀 Submit Form</span>
            <span v-else>📝 Complete Required Fields</span>
          </button>
        </div>

        <!-- Success Message -->
        <div v-if="submitSuccess" class="success-message">
          🎉 Form submitted successfully!
        </div>
      </form>
    </div>

    <!-- Field Interaction Log -->
    <div class="interaction-log">
      <h3>📝 Field Interaction Log:</h3>
      <div class="log-grid">
        <div class="log-item">
          <span class="field-name">Name:</span>
          <span :class="['log-status', touchedFields.name ? 'touched' : 'untouched']">
            {{ touchedFields.name ? '✋ Touched' : '👆 Not Touched' }}
          </span>
        </div>
        <div class="log-item">
          <span class="field-name">Email:</span>
          <span :class="['log-status', touchedFields.email ? 'touched' : 'untouched']">
            {{ touchedFields.email ? '✋ Touched' : '👆 Not Touched' }}
          </span>
        </div>
        <div class="log-item">
          <span class="field-name">Password:</span>
          <span :class="['log-status', touchedFields.password ? 'touched' : 'untouched']">
            {{ touchedFields.password ? '✋ Touched' : '👆 Not Touched' }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>
```

#### 6.3 Tambah Interactive Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Form Statistics */
.form-stats {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.form-stats h3 {
  margin-bottom: 15px;
  color: #333;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e9ecef;
}

.stat-label {
  font-weight: 600;
  color: #495057;
}

.stat-value {
  font-weight: bold;
}

.stat-value.valid {
  color: #28a745;
}

.stat-value.invalid {
  color: #dc3545;
}

.progress-bar {
  width: 100%;
  height: 8px;
  background: #e9ecef;
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #dc3545, #ffc107, #28a745);
  transition: width 0.3s ease;
}

/* Interactive Form Styles */
.interactive-form .form-group {
  position: relative;
  transition: all 0.3s ease;
}

.form-group.is-focused {
  transform: translateY(-2px);
}

.form-group.is-touched {
  background: rgba(248, 249, 250, 0.5);
  border-radius: 8px;
  padding: 15px;
}

.touch-indicator {
  font-size: 0.8em;
  color: #17a2b8;
  margin-left: 5px;
}

.input-focused {
  border-color: #007bff !important;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1) !important;
}

.field-feedback {
  margin-top: 8px;
  min-height: 60px;
}

.char-count {
  font-size: 0.8em;
  color: #6c757d;
  margin-bottom: 5px;
}

.hint-message {
  color: #17a2b8;
  font-size: 0.85em;
  font-style: italic;
}

.success-message {
  color: #28a745;
  font-size: 0.85em;
  font-weight: 600;
}

.error-message {
  color: #dc3545;
  font-size: 0.85em;
  font-weight: 600;
}

.password-strength {
  margin: 10px 0;
}

.password-strength .strength-bar {
  width: 100%;
  height: 4px;
  background: #e9ecef;
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 5px;
}

.password-strength .strength-text {
  font-size: 0.8em;
  font-weight: 600;
}

/* Interaction Log */
.interaction-log {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.interaction-log h3 {
  margin-bottom: 15px;
  color: #333;
}

.log-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.log-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: white;
  border-radius: 6px;
  border: 1px solid #e9ecef;
}

.log-status {
  font-weight: 600;
}

.log-status.touched {
  color: #28a745;
}

.log-status.untouched {
  color: #6c757d;
}

/* Enhanced Animations */
.form-group {
  transition: all 0.3s ease;
}

.form-group:hover {
  transform: translateY(-1px);
}

input {
  transition: all 0.2s ease;
}

input:focus {
  transform: scale(1.02);
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .stats-grid {
    grid-template-columns: 1fr;
  }

  .log-grid {
    grid-template-columns: 1fr;
  }

  .form-actions {
    flex-direction: column;
  }
}
</style>
```

**Result:** Form sekarang memiliki field interaction tracking dengan visual feedback!

---

### 📋 Step 7: Implementasi Real-time Validation

#### 7.1 Tambah Real-time Validation dengan Debounce
**Edit `src/App.vue` - Tambah real-time validation:**
```vue
<script setup>
import { ref, computed, watch } from "vue";

// ... (formData dan validation dari step sebelumnya)

// 1. Debounce function untuk real-time validation
const debounce = (func, wait) => {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
};

// 2. Real-time validation states
const realTimeValidation = ref({
  name: { isValid: false, message: "", isChecking: false },
  email: { isValid: false, message: "", isChecking: false },
  password: { isValid: false, message: "", isChecking: false }
});

// 3. Real-time validation functions
const validateNameRealTime = debounce((value) => {
  realTimeValidation.value.name.isChecking = true;

  // Simulate async validation
  setTimeout(() => {
    if (!value.trim()) {
      realTimeValidation.value.name = {
        isValid: false,
        message: "Name cannot be empty",
        isChecking: false
      };
    } else if (value.length < 2) {
      realTimeValidation.value.name = {
        isValid: false,
        message: "Name must be at least 2 characters",
        isChecking: false
      };
    } else if (!/^[a-zA-Z\s]+$/.test(value)) {
      realTimeValidation.value.name = {
        isValid: false,
        message: "Name can only contain letters",
        isChecking: false
      };
    } else {
      realTimeValidation.value.name = {
        isValid: true,
        message: "Name looks good!",
        isChecking: false
      };
    }
  }, 300);
}, 500);

const validateEmailRealTime = debounce((value) => {
  realTimeValidation.value.email.isChecking = true;

  setTimeout(() => {
    const email = value.trim();
    if (!email) {
      realTimeValidation.value.email = {
        isValid: false,
        message: "Email cannot be empty",
        isChecking: false
      };
    } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      realTimeValidation.value.email = {
        isValid: false,
        message: "Please enter a valid email",
        isChecking: false
      };
    } else {
      realTimeValidation.value.email = {
        isValid: true,
        message: "Valid email address",
        isChecking: false
      };
    }
  }, 500);
}, 500);

const validatePasswordRealTime = debounce((value) => {
  realTimeValidation.value.password.isChecking = true;

  setTimeout(() => {
    if (!value) {
      realTimeValidation.value.password = {
        isValid: false,
        message: "Password cannot be empty",
        isChecking: false
      };
    } else if (value.length < 8) {
      realTimeValidation.value.password = {
        isValid: false,
        message: "Password must be at least 8 characters",
        isChecking: false
      };
    } else if (!/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(value)) {
      realTimeValidation.value.password = {
        isValid: false,
        message: "Include uppercase, lowercase, and numbers",
        isChecking: false
      };
    } else {
      realTimeValidation.value.password = {
        isValid: true,
        message: "Strong password!",
        isChecking: false
      };
    }
  }, 500);
}, 500);

// 4. Watch for real-time validation
watch(() => formData.value.name, (newValue) => {
  if (newValue) {
    validateNameRealTime(newValue);
  }
});

watch(() => formData.value.email, (newValue) => {
  if (newValue) {
    validateEmailRealTime(newValue);
  }
});

watch(() => formData.value.password, (newValue) => {
  if (newValue) {
    validatePasswordRealTime(newValue);
  }
});

// 5. Validation history untuk tracking
const validationHistory = ref([]);

const addValidationHistory = (field, result) => {
  validationHistory.value.unshift({
    field,
    result,
    timestamp: new Date().toLocaleTimeString()
  });

  // Keep only last 10 entries
  if (validationHistory.value.length > 10) {
    validationHistory.value = validationHistory.value.slice(0, 10);
  }
};

// 6. Enhanced field tracking
const fieldActivity = ref({
  name: { lastChanged: null, changeCount: 0 },
  email: { lastChanged: null, changeCount: 0 },
  password: { lastChanged: null, changeCount: 0 }
});

const trackFieldActivity = (fieldName, value) => {
  fieldActivity.value[fieldName] = {
    lastChanged: new Date().toLocaleTimeString(),
    changeCount: fieldActivity.value[fieldName].changeCount + 1
  };
};
</script>
```

#### 7.2 Update Template dengan Real-time Validation
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>⚡ Real-time Form Validation</h1>

    <!-- Real-time Validation Status -->
    <div class="realtime-status">
      <h3>🔍 Real-time Validation Status:</h3>
      <div class="validation-timeline">
        <div class="timeline-item">
          <span class="field-name">Name:</span>
          <div class="validation-status">
            <span v-if="realTimeValidation.name.isChecking" class="checking">
              ⏳ Checking...
            </span>
            <span v-else-if="realTimeValidation.name.message" :class="[
              'validation-result',
              realTimeValidation.name.isValid ? 'valid' : 'invalid'
            ]">
              {{ realTimeValidation.name.isValid ? '✅' : '❌' }}
              {{ realTimeValidation.name.message }}
            </span>
            <span v-else class="validation-result pending">
              ⏳ Waiting for input
            </span>
          </div>
        </div>

        <div class="timeline-item">
          <span class="field-name">Email:</span>
          <div class="validation-status">
            <span v-if="realTimeValidation.email.isChecking" class="checking">
              ⏳ Checking...
            </span>
            <span v-else-if="realTimeValidation.email.message" :class="[
              'validation-result',
              realTimeValidation.email.isValid ? 'valid' : 'invalid'
            ]">
              {{ realTimeValidation.email.isValid ? '✅' : '❌' }}
              {{ realTimeValidation.email.message }}
            </span>
            <span v-else class="validation-result pending">
              ⏳ Waiting for input
            </span>
          </div>
        </div>

        <div class="timeline-item">
          <span class="field-name">Password:</span>
          <div class="validation-status">
            <span v-if="realTimeValidation.password.isChecking" class="checking">
              ⏳ Checking...
            </span>
            <span v-else-if="realTimeValidation.password.message" :class="[
              'validation-result',
              realTimeValidation.password.isValid ? 'valid' : 'invalid'
            ]">
              {{ realTimeValidation.password.isValid ? '✅' : '❌' }}
              {{ realTimeValidation.password.message }}
            </span>
            <span v-else class="validation-result pending">
              ⏳ Waiting for input
            </span>
          </div>
        </div>
      </div>
    </div>

    <!-- Real-time Form -->
    <div class="form-container">
      <h3>🚀 Real-time Validation Form:</h3>
      <form @submit.prevent="submitForm" class="realtime-form">
        <!-- Name Field -->
        <div class="form-group">
          <label for="name">
            Name:
            <span class="required">*</span>
            <span v-if="fieldActivity.name.lastChanged" class="activity-indicator">
              📝 {{ fieldActivity.name.lastChanged }}
            </span>
          </label>
          <input
            v-model.trim="formData.name"
            @input="trackFieldActivity('name', $event.target.value)"
            type="text"
            id="name"
            placeholder="Start typing your name..."
            :class="{
              'input-valid': realTimeValidation.name.isValid,
              'input-invalid': !realTimeValidation.name.isValid && formData.name !== '',
              'input-checking': realTimeValidation.name.isChecking
            }"
          />

          <div class="realtime-feedback">
            <div v-if="realTimeValidation.name.isChecking" class="checking-indicator">
              <div class="spinner"></div>
              Validating name...
            </div>
            <div v-else-if="realTimeValidation.name.message" :class="[
              'feedback-message',
              realTimeValidation.name.isValid ? 'success' : 'error'
            ]">
              {{ realTimeValidation.name.message }}
            </div>
          </div>
        </div>

        <!-- Email Field -->
        <div class="form-group">
          <label for="email">
            Email:
            <span class="required">*</span>
            <span v-if="fieldActivity.email.lastChanged" class="activity-indicator">
              📝 {{ fieldActivity.email.lastChanged }}
            </span>
          </label>
          <input
            v-model.trim="formData.email"
            @input="trackFieldActivity('email', $event.target.value)"
            type="email"
            id="email"
            placeholder="Enter your email address..."
            :class="{
              'input-valid': realTimeValidation.email.isValid,
              'input-invalid': !realTimeValidation.email.isValid && formData.email !== '',
              'input-checking': realTimeValidation.email.isChecking
            }"
          />

          <div class="realtime-feedback">
            <div v-if="realTimeValidation.email.isChecking" class="checking-indicator">
              <div class="spinner"></div>
              Validating email...
            </div>
            <div v-else-if="realTimeValidation.email.message" :class="[
              'feedback-message',
              realTimeValidation.email.isValid ? 'success' : 'error'
            ]">
              {{ realTimeValidation.email.message }}
            </div>
          </div>
        </div>

        <!-- Password Field -->
        <div class="form-group">
          <label for="password">
            Password:
            <span class="required">*</span>
            <span v-if="fieldActivity.password.lastChanged" class="activity-indicator">
              📝 {{ fieldActivity.password.lastChanged }}
            </span>
          </label>
          <input
            v-model="formData.password"
            @input="trackFieldActivity('password', $event.target.value)"
            type="password"
            id="password"
            placeholder="Create a strong password..."
            :class="{
              'input-valid': realTimeValidation.password.isValid,
              'input-invalid': !realTimeValidation.password.isValid && formData.password !== '',
              'input-checking': realTimeValidation.password.isChecking
            }"
          />

          <div class="realtime-feedback">
            <div v-if="realTimeValidation.password.isChecking" class="checking-indicator">
              <div class="spinner"></div>
              Validating password...
            </div>
            <div v-else-if="realTimeValidation.password.message" :class="[
              'feedback-message',
              realTimeValidation.password.isValid ? 'success' : 'error'
            ]">
              {{ realTimeValidation.password.message }}
            </div>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="form-actions">
          <button type="button" @click="resetForm" class="reset-btn">
            🔄 Reset Form
          </button>
          <button type="submit" :disabled="!isFormValid" class="submit-btn">
            <span v-if="isFormValid">🚀 Submit Form</span>
            <span v-else>⏳ Complete Validation</span>
          </button>
        </div>

        <!-- Success Message -->
        <div v-if="submitSuccess" class="success-message">
          🎉 Form submitted successfully with real-time validation!
        </div>
      </form>
    </div>

    <!-- Field Activity Log -->
    <div class="activity-log">
      <h3>📊 Field Activity Log:</h3>
      <div class="activity-grid">
        <div class="activity-item">
          <span class="field-name">Name:</span>
          <span class="activity-count">Changed {{ fieldActivity.name.changeCount }} times</span>
          <span class="last-change">{{ fieldActivity.name.lastChanged || 'Never' }}</span>
        </div>
        <div class="activity-item">
          <span class="field-name">Email:</span>
          <span class="activity-count">Changed {{ fieldActivity.email.changeCount }} times</span>
          <span class="last-change">{{ fieldActivity.email.lastChanged || 'Never' }}</span>
        </div>
        <div class="activity-item">
          <span class="field-name">Password:</span>
          <span class="activity-count">Changed {{ fieldActivity.password.changeCount }} times</span>
          <span class="last-change">{{ fieldActivity.password.lastChanged || 'Never' }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
```

#### 7.3 Tambah Real-time Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Real-time Validation Status */
.realtime-status {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.realtime-status h3 {
  margin-bottom: 15px;
  color: #333;
}

.validation-timeline {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.timeline-item {
  display: flex;
  align-items: flex-start;
  gap: 15px;
  padding: 15px;
  background: white;
  border-radius: 8px;
  border: 1px solid #e9ecef;
}

.field-name {
  font-weight: 600;
  color: #495057;
  min-width: 80px;
}

.validation-status {
  flex: 1;
}

.checking {
  color: #17a2b8;
  font-style: italic;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

.validation-result {
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 600;
}

.validation-result.valid {
  color: #28a745;
}

.validation-result.invalid {
  color: #dc3545;
}

.validation-result.pending {
  color: #6c757d;
  font-style: italic;
}

/* Real-time Form Styles */
.realtime-form .form-group {
  margin-bottom: 25px;
}

.activity-indicator {
  font-size: 0.8em;
  color: #6c757d;
  margin-left: 10px;
}

.input-valid {
  border-color: #28a745 !important;
  box-shadow: 0 0 0 2px rgba(40, 167, 69, 0.2) !important;
}

.input-invalid {
  border-color: #dc3545 !important;
  box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.2) !important;
}

.input-checking {
  border-color: #17a2b8 !important;
  box-shadow: 0 0 0 2px rgba(23, 162, 184, 0.2) !important;
}

.realtime-feedback {
  margin-top: 8px;
  min-height: 30px;
}

.checking-indicator {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #17a2b8;
  font-style: italic;
}

.spinner {
  width: 16px;
  height: 16px;
  border: 2px solid #17a2b8;
  border-radius: 50%;
  border-top-color: transparent;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.feedback-message {
  font-weight: 600;
  font-size: 0.9em;
}

.feedback-message.success {
  color: #28a745;
}

.feedback-message.error {
  color: #dc3545;
}

/* Activity Log */
.activity-log {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.activity-log h3 {
  margin-bottom: 15px;
  color: #333;
}

.activity-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.activity-item {
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 15px;
  background: white;
  border-radius: 8px;
  border: 1px solid #e9ecef;
}

.activity-count {
  font-size: 0.9em;
  color: #6c757d;
}

.last-change {
  font-size: 0.8em;
  color: #17a2b8;
  font-weight: 600;
}

/* Enhanced Animations */
.form-group {
  transition: all 0.3s ease;
}

.form-group:hover {
  transform: translateY(-2px);
}

input {
  transition: all 0.2s ease;
}

.input-valid, .input-invalid, .input-checking {
  animation: validationPulse 0.5s ease;
}

@keyframes validationPulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.02); }
  100% { transform: scale(1); }
}

/* Responsive Design */
@media (max-width: 768px) {
  .timeline-item {
    flex-direction: column;
    gap: 10px;
  }

  .activity-grid {
    grid-template-columns: 1fr;
  }
}
</style>
```

#### 7.4 Update Submit Function
**Edit `src/App.vue` - Update submit function:**
```vue
<script setup>
// ... (kode sebelumnya)

// Enhanced submit function
const submitForm = () => {
  if (isFormValid.value) {
    // Add validation history
    Object.entries(realTimeValidation.value).forEach(([field, result]) => {
      addValidationHistory(field, result);
    });

    console.log("Form submitted successfully!", formData.value);
    console.log("Validation history:", validationHistory.value);

    submitSuccess.value = true;

    // Reset setelah 3 detik
    setTimeout(() => {
      resetForm();
      submitSuccess.value = false;
    }, 3000);
  } else {
    console.log("Form has validation errors");
  }
};

// Reset function untuk real-time
const resetForm = () => {
  formData.value = {
    name: "",
    email: "",
    password: ""
  };

  // Reset real-time validation
  realTimeValidation.value = {
    name: { isValid: false, message: "", isChecking: false },
    email: { isValid: false, message: "", isChecking: false },
    password: { isValid: false, message: "", isChecking: false }
  };

  // Reset field activity
  fieldActivity.value = {
    name: { lastChanged: null, changeCount: 0 },
    email: { lastChanged: null, changeCount: 0 },
    password: { lastChanged: null, changeCount: 0 }
  };

  submitSuccess.value = false;
};
</script>
```

**Result:** Form sekarang memiliki real-time validation dengan debouncing!

---

### 📋 Step 8: Refactor ke Component Architecture

#### 8.1 Buat Component FormField
**Buat file `src/components/FormField.vue`:**
```vue
<template>
  <div class="form-field-component">
    <label :for="fieldId">
      {{ label }}
      <span v-if="required" class="required">*</span>
      <span v-if="showActivity && fieldActivity.lastChanged" class="activity-indicator">
        📝 {{ fieldActivity.lastChanged }}
      </span>
    </label>

    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      @focus="$emit('focus')"
      @blur="$emit('blur')"
      :type="type"
      :id="fieldId"
      :placeholder="placeholder"
      :maxlength="maxLength"
      :class="inputClasses"
      :disabled="disabled"
    />

    <div class="field-feedback">
      <!-- Character count -->
      <div v-if="showCharCount" class="char-count">
        {{ valueLength }}/{{ maxLength || '∞' }} characters
      </div>

      <!-- Checking indicator -->
      <div v-if="isValidating" class="checking-indicator">
        <div class="spinner"></div>
        Validating...
      </div>

      <!-- Real-time feedback -->
      <div v-else-if="showRealtimeFeedback && validationMessage" :class="[
        'feedback-message',
        isValid ? 'success' : 'error'
      ]">
        {{ validationMessage }}
      </div>

      <!-- Success message -->
      <div v-else-if="touched && isValid && showSuccessMessage" class="success-message">
        ✅ Valid {{ fieldName }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

// Props
const props = defineProps({
  modelValue: { type: [String, Number], required: true },
  label: { type: String, required: true },
  type: { type: String, default: 'text' },
  fieldId: { type: String, required: true },
  placeholder: { type: String, default: '' },
  required: { type: Boolean, default: false },
  maxLength: { type: Number, default: null },
  disabled: { type: Boolean, default: false },
  isValid: { type: Boolean, default: false },
  isValidating: { type: Boolean, default: false },
  validationMessage: { type: String, default: '' },
  touched: { type: Boolean, default: false },
  showRealtimeFeedback: { type: Boolean, default: true },
  showSuccessMessage: { type: Boolean, default: true },
  showCharCount: { type: Boolean, default: false },
  showActivity: { type: Boolean, default: false },
  fieldActivity: { type: Object, default: () => ({}) }
});

// Computed
const valueLength = computed(() => String(props.modelValue).length);

const fieldName = computed(() => props.label.toLowerCase());

const inputClasses = computed(() => ({
  'input-valid': props.isValid && props.modelValue !== '',
  'input-invalid': !props.isValid && props.modelValue !== '',
  'input-checking': props.isValidating,
  'input-disabled': props.disabled
}));

// Events
defineEmits(['update:modelValue', 'focus', 'blur']);
</script>

<style scoped>
.form-field-component {
  margin-bottom: 25px;
}

label {
  display: flex;
  align-items: center;
  font-weight: 600;
  margin-bottom: 8px;
  color: #495057;
}

.required {
  color: #dc3545;
  margin-left: 3px;
}

.activity-indicator {
  font-size: 0.8em;
  color: #6c757d;
  margin-left: 10px;
}

input {
  width: 100%;
  padding: 12px 15px;
  font-size: 16px;
  border: 2px solid #e9ecef;
  border-radius: 8px;
  transition: all 0.3s ease;
  box-sizing: border-box;
}

input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 0 2px rgba(0, 123, 255, 0.1);
}

.input-valid {
  border-color: #28a745;
  box-shadow: 0 0 0 2px rgba(40, 167, 69, 0.1);
}

.input-invalid {
  border-color: #dc3545;
  box-shadow: 0 0 0 2px rgba(220, 53, 69, 0.1);
}

.input-checking {
  border-color: #17a2b8;
  box-shadow: 0 0 0 2px rgba(23, 162, 184, 0.1);
}

.input-disabled {
  background-color: #e9ecef;
  opacity: 0.6;
  cursor: not-allowed;
}

.field-feedback {
  margin-top: 8px;
  min-height: 20px;
}

.char-count {
  font-size: 0.8em;
  color: #6c757d;
}

.checking-indicator {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #17a2b8;
  font-style: italic;
  font-size: 0.9em;
}

.spinner {
  width: 16px;
  height: 16px;
  border: 2px solid #17a2b8;
  border-radius: 50%;
  border-top-color: transparent;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.feedback-message {
  font-weight: 600;
  font-size: 0.9em;
}

.feedback-message.success {
  color: #28a745;
}

.feedback-message.error {
  color: #dc3545;
}

.success-message {
  color: #28a745;
  font-size: 0.9em;
  font-weight: 600;
}
</style>
```

#### 8.2 Buat Component ValidationSummary
**Buat file `src/components/ValidationSummary.vue`:**
```vue
<template>
  <div class="validation-summary">
    <h3>📊 Validation Summary</h3>

    <!-- Progress Section -->
    <div class="progress-section">
      <div class="progress-header">
        <span>Overall Progress: {{ stats.completion }}%</span>
        <span :class="['status-badge', overallValid ? 'valid' : 'invalid']">
          {{ overallValid ? '✅ Ready' : '❌ Incomplete' }}
        </span>
      </div>
      <div class="progress-bar">
        <div
          class="progress-fill"
          :style="{ width: stats.completion + '%' }"
          :class="[
            stats.completion === 100 ? 'complete' : 'incomplete'
          ]"
        ></div>
      </div>
      <div class="progress-stats">
        <span class="stat">{{ stats.valid }} Valid</span>
        <span class="stat">{{ stats.invalid }} Invalid</span>
        <span class="stat">{{ stats.total }} Total</span>
      </div>
    </div>

    <!-- Field Status Grid -->
    <div class="field-status-grid">
      <div
        v-for="(result, field) in validationResults"
        :key="field"
        class="status-item"
        :class="{ 'valid': result.isValid, 'invalid': !result.isValid }"
      >
        <div class="status-header">
          <span class="field-name">{{ formatFieldName(field) }}</span>
          <span class="status-icon">{{ result.isValid ? '✅' : '❌' }}</span>
        </div>
        <div class="status-message">
          {{ result.message || (result.isValid ? 'Valid' : 'Invalid') }}
        </div>
      </div>
    </div>

    <!-- Real-time Status -->
    <div v-if="showRealtimeStatus" class="realtime-status">
      <h4>⚡ Real-time Status</h4>
      <div class="realtime-grid">
        <div
          v-for="(field, index) in realTimeFields"
          :key="index"
          class="realtime-item"
        >
          <span class="field-name">{{ field.name }}</span>
          <div class="realtime-validation">
            <span v-if="field.isValidating" class="checking">
              ⏳ Checking...
            </span>
            <span v-else-if="field.message" :class="[
              'validation-result',
              field.isValid ? 'valid' : 'invalid'
            ]">
              {{ field.message }}
            </span>
            <span v-else class="pending">⏳ Waiting</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';

// Props
const props = defineProps({
  validationResults: { type: Object, required: true },
  showRealtimeStatus: { type: Boolean, default: false },
  realTimeFields: { type: Array, default: () => [] }
});

// Computed
const stats = computed(() => {
  const results = Object.values(props.validationResults);
  return {
    total: results.length,
    valid: results.filter(r => r.isValid).length,
    invalid: results.filter(r => !r.isValid).length,
    completion: Math.round((results.filter(r => r.isValid).length / results.length) * 100)
  };
});

const overallValid = computed(() => {
  return Object.values(props.validationResults).every(result => result.isValid);
});

// Methods
const formatFieldName = (fieldName) => {
  return fieldName.charAt(0).toUpperCase() + fieldName.slice(1);
};
</script>

<style scoped>
.validation-summary {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
  color: white;
}

.validation-summary h3 {
  text-align: center;
  margin-bottom: 20px;
}

/* Progress Section */
.progress-section {
  margin-bottom: 25px;
}

.progress-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.status-badge {
  padding: 4px 12px;
  border-radius: 15px;
  font-size: 0.8em;
  font-weight: 600;
}

.status-badge.valid {
  background: rgba(40, 167, 69, 0.8);
}

.status-badge.invalid {
  background: rgba(220, 53, 69, 0.8);
}

.progress-bar {
  width: 100%;
  height: 8px;
  background: rgba(255,255,255,0.2);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.progress-fill {
  height: 100%;
  transition: width 0.3s ease;
}

.progress-fill.complete {
  background: linear-gradient(90deg, #28a745, #20c997);
}

.progress-fill.incomplete {
  background: linear-gradient(90deg, #dc3545, #ffc107);
}

.progress-stats {
  display: flex;
  justify-content: space-around;
  font-size: 0.9em;
}

.stat {
  padding: 4px 8px;
  background: rgba(255,255,255,0.1);
  border-radius: 10px;
}

/* Field Status Grid */
.field-status-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 25px;
}

.status-item {
  padding: 15px;
  background: rgba(255,255,255,0.1);
  border-radius: 10px;
  border: 1px solid rgba(255,255,255,0.1);
  transition: all 0.3s ease;
}

.status-item:hover {
  transform: translateY(-2px);
}

.status-item.valid {
  border-color: rgba(40, 167, 69, 0.5);
}

.status-item.invalid {
  border-color: rgba(220, 53, 69, 0.5);
}

.status-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.field-name {
  font-weight: 600;
}

.status-icon {
  font-weight: bold;
}

.status-message {
  font-size: 0.9em;
  opacity: 0.9;
}

/* Real-time Status */
.realtime-status h4 {
  margin-bottom: 15px;
  text-align: center;
}

.realtime-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.realtime-item {
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 10px;
  background: rgba(255,255,255,0.05);
  border-radius: 8px;
}

.realtime-item .field-name {
  font-size: 0.9em;
  font-weight: 600;
}

.validation-result {
  font-size: 0.8em;
  font-weight: 600;
}

.validation-result.valid {
  color: #28a745;
}

.validation-result.invalid {
  color: #dc3545;
}

.checking {
  color: #17a2b8;
  font-style: italic;
  animation: pulse 1.5s infinite;
}

.pending {
  color: #6c757d;
  font-style: italic;
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

/* Responsive Design */
@media (max-width: 768px) {
  .progress-header {
    flex-direction: column;
    gap: 10px;
  }

  .field-status-grid,
  .realtime-grid {
    grid-template-columns: 1fr;
  }
}
</style>
```

#### 8.3 Update App.vue (Main Component)
**Edit `src/App.vue` menjadi:**
```vue
<script setup>
import { ref, computed } from "vue";
import FormField from './components/FormField.vue';
import ValidationSummary from './components/ValidationSummary.vue';

// Form data
const formData = ref({
  name: "",
  email: "",
  password: ""
});

// Validation state
const isSubmitting = ref(false);
const submitSuccess = ref(false);

// Real-time validation state
const realTimeValidation = ref({
  name: { isValid: false, message: "", isChecking: false },
  email: { isValid: false, message: "", isChecking: false },
  password: { isValid: false, message: "", isChecking: false }
});

// Field activity tracking
const fieldActivity = ref({
  name: { lastChanged: null, changeCount: 0 },
  email: { lastChanged: null, changeCount: 0 },
  password: { lastChanged: null, changeCount: 0 }
});

// Validation results
const validationResults = computed(() => ({
  name: realTimeValidation.value.name,
  email: realTimeValidation.value.email,
  password: realTimeValidation.value.password
}));

// Overall form validity
const isFormValid = computed(() => {
  return Object.values(validationResults.value).every(result => result.isValid);
});

// Real-time fields for summary
const realTimeFields = computed(() => [
  { name: 'Name', ...realTimeValidation.value.name },
  { name: 'Email', ...realTimeValidation.value.email },
  { name: 'Password', ...realTimeValidation.value.password }
]);

// Form submission
const submitForm = () => {
  if (isFormValid.value) {
    console.log("Form submitted successfully!", formData.value);
    submitSuccess.value = true;
    setTimeout(() => {
      resetForm();
      submitSuccess.value = false;
    }, 3000);
  }
};

// Reset form
const resetForm = () => {
  formData.value = { name: "", email: "", password: "" };
  realTimeValidation.value = {
    name: { isValid: false, message: "", isChecking: false },
    email: { isValid: false, message: "", isChecking: false },
    password: { isValid: false, message: "", isChecking: false }
  };
  fieldActivity.value = {
    name: { lastChanged: null, changeCount: 0 },
    email: { lastChanged: null, changeCount: 0 },
    password: { lastChanged: null, changeCount: 0 }
  };
};

// Track field activity
const trackFieldActivity = (fieldName) => {
  fieldActivity.value[fieldName] = {
    lastChanged: new Date().toLocaleTimeString(),
    changeCount: fieldActivity.value[fieldName].changeCount + 1
  };
};

// Validate field (simplified for demo)
const validateField = (fieldName, value) => {
  realTimeValidation.value[fieldName].isChecking = true;

  setTimeout(() => {
    switch (fieldName) {
      case 'name':
        realTimeValidation.value.name = {
          isValid: value.trim().length >= 2,
          message: value.trim().length >= 2 ? 'Valid name' : 'Name must be at least 2 characters',
          isChecking: false
        };
        break;
      case 'email':
        realTimeValidation.value.email = {
          isValid: /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
          message: /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) ? 'Valid email' : 'Please enter a valid email',
          isChecking: false
        };
        break;
      case 'password':
        realTimeValidation.value.password = {
          isValid: value.length >= 8,
          message: value.length >= 8 ? 'Strong password' : 'Password must be at least 8 characters',
          isChecking: false
        };
        break;
    }
  }, 500);
};
</script>

<template>
  <div class="app">
    <header>
      <h1>🔍 Component-Based Form Validation</h1>
      <p>Reusable form components with real-time validation</p>
    </header>

    <main>
      <!-- Validation Summary -->
      <ValidationSummary
        :validation-results="validationResults"
        :show-realtime-status="true"
        :real-time-fields="realTimeFields"
      />

      <!-- Main Form -->
      <div class="form-container">
        <h3>📝 Component-Based Form</h3>
        <form @submit.prevent="submitForm" class="component-form">
          <FormField
            v-model="formData.name"
            label="Full Name"
            type="text"
            field-id="name"
            placeholder="Enter your full name"
            :required="true"
            :max-length="50"
            :is-valid="validationResults.name.isValid"
            :is-validating="realTimeValidation.name.isChecking"
            :validation-message="validationResults.name.message"
            :show-char-count="true"
            :show-activity="true"
            :field-activity="fieldActivity.name"
            @input="trackFieldActivity('name')"
            @input="validateField('name', $event.target.value)"
          />

          <FormField
            v-model="formData.email"
            label="Email Address"
            type="email"
            field-id="email"
            placeholder="your.email@example.com"
            :required="true"
            :is-valid="validationResults.email.isValid"
            :is-validating="realTimeValidation.email.isChecking"
            :validation-message="validationResults.email.message"
            :show-activity="true"
            :field-activity="fieldActivity.email"
            @input="trackFieldActivity('email')"
            @input="validateField('email', $event.target.value)"
          />

          <FormField
            v-model="formData.password"
            label="Password"
            type="password"
            field-id="password"
            placeholder="Enter your password"
            :required="true"
            :max-length="128"
            :is-valid="validationResults.password.isValid"
            :is-validating="realTimeValidation.password.isChecking"
            :validation-message="validationResults.password.message"
            :show-activity="true"
            :field-activity="fieldActivity.password"
            @input="trackFieldActivity('password')"
            @input="validateField('password', $event.target.value)"
          />

          <div class="form-actions">
            <button type="button" @click="resetForm" class="reset-btn">
              🔄 Reset Form
            </button>
            <button type="submit" :disabled="!isFormValid" class="submit-btn">
              {{ isFormValid ? '🚀 Submit Form' : '⏳ Complete Validation' }}
            </button>
          </div>

          <!-- Success Message -->
          <div v-if="submitSuccess" class="success-message">
            🎉 Form submitted successfully with components!
          </div>
        </form>
      </div>
    </main>

    <footer>
      <p>Component-based architecture for better maintainability</p>
      <p>Made with ❤️ using Vue.js 3</p>
    </footer>
  </div>
</template>

<style>
/* Global Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
}

.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header {
  text-align: center;
  margin-bottom: 40px;
}

header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  font-size: 1.1em;
  opacity: 0.9;
}

main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

.form-container {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.form-container h3 {
  text-align: center;
  margin-bottom: 25px;
}

.component-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-actions {
  display: flex;
  gap: 15px;
  margin-top: 20px;
}

.reset-btn, .submit-btn {
  flex: 1;
  padding: 15px 25px;
  font-size: 1.1em;
  font-weight: 600;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.reset-btn {
  background: rgba(255,255,255,0.2);
  color: white;
}

.reset-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.submit-btn {
  background: #4CAF50;
  color: white;
}

.submit-btn:hover:not(:disabled) {
  background: #45a049;
  transform: translateY(-2px);
}

.submit-btn:disabled {
  background: rgba(255,255,255,0.2);
  color: rgba(255,255,255,0.5);
  cursor: not-allowed;
}

.success-message {
  margin-top: 20px;
  padding: 15px;
  background: rgba(76, 175, 80, 0.2);
  border: 1px solid #4CAF50;
  border-radius: 8px;
  text-align: center;
  font-weight: 600;
}

footer {
  text-align: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
  opacity: 0.8;
}

footer p {
  margin-bottom: 5px;
}

/* Responsive Design */
@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .form-actions {
    flex-direction: column;
  }
}
</style>
```

**Result:** Aplikasi sekarang menggunakan component-based architecture yang modular!

---

### 📋 Step 9: Final Testing & Deployment

#### 9.1 Test Semua Fitur
**Coba semua fungsi:**
- ✅ Real-time validation dengan debouncing
- ✅ Component-based architecture
- ✅ Field interaction tracking
- ✅ Visual feedback dan animations
- ✅ Form submission handling
- ✅ Validation summary
- ✅ Reset functionality
- ✅ Responsive design

#### 9.2 Debug Issues
**Common issues & fixes:**
```vue
<!-- Issue: Component tidak menerima props -->
<!-- Fix: Pastikan props definition benar -->
defineProps({
  modelValue: { type: String, required: true }
});

<!-- Issue: Events tidak diterima -->
<!-- Fix: Pastikan emit definition benar -->
defineEmits(['update:modelValue', 'focus']);

<!-- Issue: Real-time validation tidak bekerja -->
<!-- Fix: Pastikan debouncing logic benar -->
const validateField = debounce((value) => {
  // validation logic
}, 500);
```

#### 9.3 Build untuk Production
```bash
# Build aplikasi
npm run build

# Preview build
npm run preview

# Hasilnya ada di folder /dist
```

#### 9.4 Deployment
```bash
# 1. Push ke GitHub
git add .
git commit -m "Complete Component-Based Form Validation"
git push origin main

# 2. Deploy ke Netlify/Vercel
# - Connect GitHub repository
# - Auto deploy dari main branch
# - Settings: Build command = npm run build
# - Settings: Publish directory = dist
```

---

## 🎉 Hasil Akhir

✅ **Aplikasi Form Validation Lengkap dengan:**
- Real-time validation dengan debouncing
- Component-based architecture
- Reusable form field components
- Field interaction tracking
- Visual feedback systems
- Validation summary dengan progress
- Modern UI dengan animations
- Responsive design
- Production-ready code

## 📚 Yang Telah Dipelajari

1. **Vue.js Fundamentals**
   - Composition API (`<script setup>`)
   - Reactive data management
   - Computed properties
   - Watchers untuk reactivity
   - Props dan Events system

2. **Advanced Concepts**
   - Debouncing techniques
   - Component architecture
   - Real-time validation
   - State management patterns
   - Performance optimization

3. **UI/UX Best Practices**
   - User feedback systems
   - Loading states
   - Error handling
   - Accessibility considerations
   - Responsive design

## 🚀 Next Steps

1. **Advanced Validation** - Custom rules, async validation
2. **Multi-step Forms** - Complex form workflows
3. **API Integration** - Server-side validation
4. **Form Libraries** - Integrate with VeeValidate
5. **Testing** - Unit dan E2E testing
6. **Performance** - Optimization techniques
7. **Mobile Optimization** - Touch-friendly interfaces

---

**🎯 Mission Accomplished!**
Anda berhasil membuat aplikasi Form Validation yang fully functional dengan Vue.js 3 dan component-based architecture!

**💡 Pro Tip:** Lanjutkan dengan menambah advanced features atau mulai project Vue.js lainnya!

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [Component Guide](https://vuejs.org/guide/components/registration.html) | [Form Validation Best Practices](https://www.smashingmagazine.com/2018/08/complete-guide-form-validation/)