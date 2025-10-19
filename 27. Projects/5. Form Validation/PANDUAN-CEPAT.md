# 🚀 Panduan Cepat: Form Validation Vue.js

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
npm create vue@latest form-validation-app

# Masuk ke folder project
cd form-validation-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed, watch } from "vue";

// Form data dengan reactive state
const formData = ref({
  name: "",
  email: "",
  password: "",
  confirmPassword: "",
  phone: "",
  age: "",
  website: "",
  terms: false,
  newsletter: false
});

// Submit state
const isSubmitting = ref(false);
const submitSuccess = ref(false);
const submitError = ref("");

// Validation states
const touchedFields = ref({
  name: false,
  email: false,
  password: false,
  confirmPassword: false,
  phone: false,
  age: false,
  website: false
});

// Validation rules
const validationRules = {
  name: {
    required: true,
    minLength: 2,
    maxLength: 50
  },
  email: {
    required: true,
    pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  },
  password: {
    required: true,
    minLength: 8,
    pattern: /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/
  },
  confirmPassword: {
    required: true,
    match: true
  },
  phone: {
    required: true,
    pattern: /^[\+]?[1-9][\d]{0,15}$/
  },
  age: {
    required: true,
    min: 18,
    max: 120
  },
  website: {
    required: false,
    pattern: /^https?:\/\/.+/
  },
  terms: {
    required: true
  }
};

// Computed validation results
const validationResults = computed(() => {
  const results = {};

  // Name validation
  results.name = {
    isValid: formData.value.name.trim().length >= validationRules.name.minLength &&
             formData.value.name.trim().length <= validationRules.name.maxLength,
    errors: []
  };
  if (!formData.value.name.trim()) {
    results.name.errors.push("Name is required");
  } else if (formData.value.name.trim().length < validationRules.name.minLength) {
    results.name.errors.push(`Name must be at least ${validationRules.name.minLength} characters`);
  } else if (formData.value.name.trim().length > validationRules.name.maxLength) {
    results.name.errors.push(`Name must be less than ${validationRules.name.maxLength} characters`);
  }

  // Email validation
  results.email = {
    isValid: validationRules.email.pattern.test(formData.value.email),
    errors: []
  };
  if (!formData.value.email) {
    results.email.errors.push("Email is required");
  } else if (!validationRules.email.pattern.test(formData.value.email)) {
    results.email.errors.push("Please enter a valid email address");
  }

  // Password validation
  results.password = {
    isValid: formData.value.password.length >= validationRules.password.minLength &&
             validationRules.password.pattern.test(formData.value.password),
    errors: [],
    strength: 0
  };
  if (!formData.value.password) {
    results.password.errors.push("Password is required");
  } else {
    if (formData.value.password.length < validationRules.password.minLength) {
      results.password.errors.push(`Password must be at least ${validationRules.password.minLength} characters`);
    }
    if (!validationRules.password.pattern.test(formData.value.password)) {
      results.password.errors.push("Password must contain uppercase, lowercase, number, and special character");
    }

    // Calculate password strength
    if (formData.value.password.length >= 8) results.password.strength++;
    if (/[a-z]/.test(formData.value.password)) results.password.strength++;
    if (/[A-Z]/.test(formData.value.password)) results.password.strength++;
    if (/\d/.test(formData.value.password)) results.password.strength++;
    if (/[@$!%*?&]/.test(formData.value.password)) results.password.strength++;
  }

  // Confirm password validation
  results.confirmPassword = {
    isValid: formData.value.confirmPassword === formData.value.password && formData.value.confirmPassword !== "",
    errors: []
  };
  if (!formData.value.confirmPassword) {
    results.confirmPassword.errors.push("Please confirm your password");
  } else if (formData.value.confirmPassword !== formData.value.password) {
    results.confirmPassword.errors.push("Passwords do not match");
  }

  // Phone validation
  results.phone = {
    isValid: validationRules.phone.pattern.test(formData.value.phone),
    errors: []
  };
  if (!formData.value.phone) {
    results.phone.errors.push("Phone number is required");
  } else if (!validationRules.phone.pattern.test(formData.value.phone)) {
    results.phone.errors.push("Please enter a valid phone number");
  }

  // Age validation
  results.age = {
    isValid: formData.value.age >= validationRules.age.min && formData.value.age <= validationRules.age.max,
    errors: []
  };
  if (!formData.value.age) {
    results.age.errors.push("Age is required");
  } else if (formData.value.age < validationRules.age.min) {
    results.age.errors.push(`You must be at least ${validationRules.age.min} years old`);
  } else if (formData.value.age > validationRules.age.max) {
    results.age.errors.push(`Please enter a valid age`);
  }

  // Website validation (optional)
  results.website = {
    isValid: !formData.value.website || validationRules.website.pattern.test(formData.value.website),
    errors: []
  };
  if (formData.value.website && !validationRules.website.pattern.test(formData.value.website)) {
    results.website.errors.push("Please enter a valid website URL (http:// or https://)");
  }

  // Terms validation
  results.terms = {
    isValid: formData.value.terms,
    errors: []
  };
  if (!formData.value.terms) {
    results.terms.errors.push("You must accept the terms and conditions");
  }

  return results;
});

// Overall form validity
const isFormValid = computed(() => {
  return Object.values(validationResults.value).every(result => result.isValid);
});

// Form statistics
const formStats = computed(() => {
  const total = Object.keys(validationResults.value).length;
  const valid = Object.values(validationResults.value).filter(result => result.isValid).length;
  const invalid = total - valid;
  const completion = Math.round((valid / total) * 100);

  return { total, valid, invalid, completion };
});

// Touch field when user interacts
const touchField = (fieldName) => {
  touchedFields.value[fieldName] = true;
};

// Reset form
const resetForm = () => {
  formData.value = {
    name: "",
    email: "",
    password: "",
    confirmPassword: "",
    phone: "",
    age: "",
    website: "",
    terms: false,
    newsletter: false
  };
  touchedFields.value = Object.keys(touchedFields.value).reduce((acc, key) => {
    acc[key] = false;
    return acc;
  }, {});
  submitSuccess.value = false;
  submitError.value = "";
};

// Submit form
const submitForm = async () => {
  if (!isFormValid.value) {
    submitError.value = "Please fill in all required fields correctly";
    return;
  }

  isSubmitting.value = true;
  submitError.value = "";

  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 2000));

    console.log("Form submitted successfully!", formData.value);
    submitSuccess.value = true;

    // Reset form after successful submission
    setTimeout(() => {
      resetForm();
      submitSuccess.value = false;
    }, 3000);

  } catch (error) {
    console.error("Submission error:", error);
    submitError.value = "An error occurred. Please try again.";
  } finally {
    isSubmitting.value = false;
  }
};

// Auto-save to localStorage
watch(formData, (newData) => {
  localStorage.setItem('formData', JSON.stringify(newData));
}, { deep: true });

// Load from localStorage on mount
const loadSavedData = () => {
  const saved = localStorage.getItem('formData');
  if (saved) {
    try {
      const parsed = JSON.parse(saved);
      // Only load non-sensitive fields
      formData.value = {
        ...formData.value,
        name: parsed.name || "",
        email: parsed.email || "",
        phone: parsed.phone || "",
        age: parsed.age || "",
        website: parsed.website || "",
        newsletter: parsed.newsletter || false
      };
    } catch (error) {
      console.error("Error loading saved data:", error);
    }
  }
};

loadSavedData();
</script>

<template>
  <div class="app">
    <header>
      <h1>🔍 Advanced Form Validation</h1>
      <p>Master form validation with Vue.js 3 Composition API</p>
    </header>

    <main>
      <!-- Form Progress -->
      <div class="progress-section">
        <h3>📊 Form Completion: {{ formStats.completion }}%</h3>
        <div class="progress-bar">
          <div class="progress-fill" :style="{ width: formStats.completion + '%' }"></div>
        </div>
        <div class="progress-stats">
          <span class="valid-count">✅ {{ formStats.valid }} Valid</span>
          <span class="invalid-count">❌ {{ formStats.invalid }} Invalid</span>
        </div>
      </div>

      <!-- Main Form -->
      <form @submit.prevent="submitForm" class="advanced-form">
        <div class="form-grid">
          <!-- Name Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.name.isValid && touchedFields.name }">
            <label for="name">
              👤 Full Name <span class="required">*</span>
            </label>
            <input
              v-model.trim="formData.name"
              @input="touchField('name')"
              type="text"
              id="name"
              placeholder="Enter your full name"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.name && validationResults.name.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.name.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Email Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.email.isValid && touchedFields.email }">
            <label for="email">
              📧 Email Address <span class="required">*</span>
            </label>
            <input
              v-model.trim="formData.email"
              @input="touchField('email')"
              type="email"
              id="email"
              placeholder="your.email@example.com"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.email && validationResults.email.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.email.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Password Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.password.isValid && touchedFields.password }">
            <label for="password">
              🔒 Password <span class="required">*</span>
            </label>
            <input
              v-model="formData.password"
              @input="touchField('password')"
              type="password"
              id="password"
              placeholder="Enter your password"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.password" class="password-strength">
              <div class="strength-bar">
                <div
                  class="strength-fill"
                  :class="[
                    validationResults.password.strength <= 2 ? 'weak' :
                    validationResults.password.strength <= 3 ? 'medium' : 'strong'
                  ]"
                  :style="{ width: (validationResults.password.strength * 20) + '%' }"
                ></div>
              </div>
              <span class="strength-text">
                Strength: {{ validationResults.password.strength <= 2 ? 'Weak' : validationResults.password.strength <= 3 ? 'Medium' : 'Strong' }}
              </span>
            </div>
            <div v-if="touchedFields.password && validationResults.password.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.password.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Confirm Password Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.confirmPassword.isValid && touchedFields.confirmPassword }">
            <label for="confirmPassword">
              🔐 Confirm Password <span class="required">*</span>
            </label>
            <input
              v-model="formData.confirmPassword"
              @input="touchField('confirmPassword')"
              type="password"
              id="confirmPassword"
              placeholder="Confirm your password"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.confirmPassword && validationResults.confirmPassword.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.confirmPassword.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Phone Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.phone.isValid && touchedFields.phone }">
            <label for="phone">
              📱 Phone Number <span class="required">*</span>
            </label>
            <input
              v-model.trim="formData.phone"
              @input="touchField('phone')"
              type="tel"
              id="phone"
              placeholder="+1234567890"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.phone && validationResults.phone.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.phone.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Age Field -->
          <div class="form-group" :class="{ 'has-error': !validationResults.age.isValid && touchedFields.age }">
            <label for="age">
              🎂 Age <span class="required">*</span>
            </label>
            <input
              v-model.number="formData.age"
              @input="touchField('age')"
              type="number"
              id="age"
              placeholder="25"
              min="18"
              max="120"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.age && validationResults.age.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.age.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <!-- Website Field (Optional) -->
          <div class="form-group" :class="{ 'has-error': !validationResults.website.isValid && touchedFields.website }">
            <label for="website">
              🌐 Website (Optional)
            </label>
            <input
              v-model.trim="formData.website"
              @input="touchField('website')"
              type="url"
              id="website"
              placeholder="https://yourwebsite.com"
              :disabled="isSubmitting"
            />
            <div v-if="touchedFields.website && validationResults.website.errors.length > 0" class="error-messages">
              <span v-for="error in validationResults.website.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>
        </div>

        <!-- Checkbox Fields -->
        <div class="checkbox-section">
          <div class="form-group" :class="{ 'has-error': !validationResults.terms.isValid }">
            <label class="checkbox-label">
              <input
                v-model="formData.terms"
                type="checkbox"
                id="terms"
                :disabled="isSubmitting"
              />
              <span>
                I accept the <a href="#" class="link">Terms and Conditions</a>
                <span class="required">*</span>
              </span>
            </label>
            <div v-if="!validationResults.terms.isValid" class="error-messages">
              <span v-for="error in validationResults.terms.errors" :key="error" class="error">
                {{ error }}
              </span>
            </div>
          </div>

          <div class="form-group">
            <label class="checkbox-label">
              <input
                v-model="formData.newsletter"
                type="checkbox"
                id="newsletter"
                :disabled="isSubmitting"
              />
              <span>📧 Subscribe to our newsletter</span>
            </label>
          </div>
        </div>

        <!-- Form Actions -->
        <div class="form-actions">
          <button type="button" @click="resetForm" class="reset-btn" :disabled="isSubmitting">
            🔄 Reset Form
          </button>
          <button type="submit" class="submit-btn" :disabled="!isFormValid || isSubmitting">
            <span v-if="isSubmitting" class="loading-spinner"></span>
            {{ isSubmitting ? 'Submitting...' : '🚀 Submit Form' }}
          </button>
        </div>

        <!-- Success/Error Messages -->
        <div v-if="submitSuccess" class="success-message">
          ✅ Form submitted successfully! Redirecting...
        </div>

        <div v-if="submitError" class="error-message">
          ❌ {{ submitError }}
        </div>
      </form>

      <!-- Validation Summary -->
      <div class="validation-summary">
        <h3>📋 Validation Summary</h3>
        <div class="summary-grid">
          <div v-for="(result, field) in validationResults" :key="field" class="summary-item">
            <span class="field-name">{{ field.charAt(0).toUpperCase() + field.slice(1) }}</span>
            <span :class="['validation-status', result.isValid ? 'valid' : 'invalid']">
              {{ result.isValid ? '✅ Valid' : '❌ Invalid' }}
            </span>
          </div>
        </div>
        <div class="overall-status">
          <span>Overall Status:</span>
          <span :class="['status-text', isFormValid ? 'valid' : 'invalid']">
            {{ isFormValid ? '✅ Ready to Submit' : '❌ Complete Required Fields' }}
          </span>
        </div>
      </div>
    </main>

    <footer>
      <div class="footer-content">
        <p>💡 Tips: Fill in all required fields to enable submission</p>
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
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  font-size: 1.1em;
  opacity: 0.9;
}

main {
  max-width: 800px;
  margin: 0 auto;
  padding: 0 20px 40px;
}

/* Progress Section */
.progress-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.progress-section h3 {
  margin-bottom: 15px;
  text-align: center;
}

.progress-bar {
  width: 100%;
  height: 10px;
  background: rgba(255,255,255,0.2);
  border-radius: 5px;
  overflow: hidden;
  margin-bottom: 15px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #4CAF50, #8BC34A);
  transition: width 0.3s ease;
}

.progress-stats {
  display: flex;
  justify-content: space-around;
  font-size: 0.9em;
}

.valid-count { color: #4CAF50; }
.invalid-count { color: #f44336; }

/* Form Styles */
.advanced-form {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.form-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group.has-error input {
  border-color: #f44336;
  box-shadow: 0 0 0 2px rgba(244, 67, 54, 0.2);
}

label {
  margin-bottom: 8px;
  font-weight: 600;
  font-size: 1em;
}

.required {
  color: #f44336;
  margin-left: 3px;
}

input {
  padding: 12px 15px;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  color: white;
  font-size: 16px;
  transition: all 0.3s ease;
}

input::placeholder {
  color: rgba(255,255,255,0.6);
}

input:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 0 2px rgba(76, 175, 80, 0.2);
}

input:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

/* Password Strength */
.password-strength {
  margin-top: 8px;
}

.strength-bar {
  width: 100%;
  height: 4px;
  background: rgba(255,255,255,0.2);
  border-radius: 2px;
  overflow: hidden;
  margin-bottom: 5px;
}

.strength-fill {
  height: 100%;
  transition: all 0.3s ease;
}

.strength-fill.weak { background: #f44336; }
.strength-fill.medium { background: #ff9800; }
.strength-fill.strong { background: #4CAF50; }

.strength-text {
  font-size: 0.8em;
  opacity: 0.8;
}

/* Error Messages */
.error-messages {
  margin-top: 8px;
}

.error {
  display: block;
  color: #ffcccc;
  font-size: 0.85em;
  margin-bottom: 2px;
}

/* Checkbox Section */
.checkbox-section {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 30px;
}

.checkbox-label {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-weight: normal;
}

.checkbox-label input[type="checkbox"] {
  width: auto;
  margin-right: 10px;
}

.link {
  color: #4CAF50;
  text-decoration: underline;
}

/* Form Actions */
.form-actions {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
}

.reset-btn, .submit-btn {
  flex: 1;
  padding: 15px 25px;
  border: none;
  border-radius: 8px;
  font-size: 1.1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.reset-btn {
  background: rgba(255,255,255,0.2);
  color: white;
}

.reset-btn:hover:not(:disabled) {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.submit-btn {
  background: #4CAF50;
  color: white;
  position: relative;
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

.loading-spinner {
  display: inline-block;
  width: 16px;
  height: 16px;
  border: 2px solid #ffffff;
  border-radius: 50%;
  border-top-color: transparent;
  animation: spin 1s linear infinite;
  margin-right: 8px;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

/* Success/Error Messages */
.success-message, .error-message {
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  font-weight: 600;
  margin-bottom: 20px;
}

.success-message {
  background: rgba(76, 175, 80, 0.2);
  border: 1px solid #4CAF50;
  color: #a5d6a7;
}

.error-message {
  background: rgba(244, 67, 54, 0.2);
  border: 1px solid #f44336;
  color: #ffcdd2;
}

/* Validation Summary */
.validation-summary {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.validation-summary h3 {
  text-align: center;
  margin-bottom: 20px;
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
  margin-bottom: 20px;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  padding: 10px;
  background: rgba(255,255,255,0.1);
  border-radius: 5px;
}

.field-name {
  font-weight: 500;
}

.validation-status {
  font-weight: bold;
}

.validation-status.valid { color: #4CAF50; }
.validation-status.invalid { color: #f44336; }

.overall-status {
  text-align: center;
  padding: 15px;
  background: rgba(255,255,255,0.1);
  border-radius: 8px;
  font-weight: 600;
}

.status-text.valid { color: #4CAF50; }
.status-text.invalid { color: #f44336; }

/* Footer */
footer {
  text-align: center;
  padding: 30px 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

.footer-content p {
  margin-bottom: 10px;
  opacity: 0.8;
}

/* Responsive Design */
@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .form-grid {
    grid-template-columns: 1fr;
  }

  .form-actions {
    flex-direction: column;
  }

  .progress-stats {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }

  .summary-grid {
    grid-template-columns: 1fr;
  }
}
</style>
```

### 📋 Step 4: Jalankan Aplikasi (1 Menit)

```bash
# Start development server
npm run dev
```

**Buka browser:** `http://localhost:5173`

🎉 **Selesai! Aplikasi Advanced Form Validation Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis
3. **`watch()`** - Monitor perubahan data
4. **`v-model`** - Two-way data binding
5. **`@input`** - Event handling
6. **`:class`** - Dynamic class binding
7. **`v-if`** - Conditional rendering

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed, watch } from "vue";
const formData = ref({...});      // Form data
const touchedFields = ref({...}); // Field interaction tracking

// 2. Validation Rules
const validationResults = computed(() => { /* validation logic */ });

// 3. Form Statistics
const formStats = computed(() => { /* completion stats */ });

// 4. Functions
const submitForm = () => { /* submit logic */ };
const resetForm = () => { /* reset logic */ };
</script>

<template>
<!-- 5. Tampilan HTML -->
</template>

<style scoped>
/* 6. Styling CSS */
</style>
```

### 🔍 Logika Validasi

```javascript
// Computed validation results
const validationResults = computed(() => {
  const results = {};

  // Dynamic validation untuk setiap field
  results.name = {
    isValid: formData.value.name.trim().length >= 2,
    errors: []
  };

  if (!formData.value.name.trim()) {
    results.name.errors.push("Name is required");
  }

  return results;
});

// Overall form validity
const isFormValid = computed(() => {
  return Object.values(validationResults.value).every(result => result.isValid);
});
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Custom Validation Rules
```vue
<script setup>
const customRules = {
  username: {
    minLength: 3,
    maxLength: 20,
    pattern: /^[a-zA-Z0-9_]+$/,
    unique: async (value) => {
      // Check username uniqueness via API
      return !(await checkUsernameExists(value));
    }
  }
};
</script>
```

### 2. Field Dependencies
```vue
<script setup>
const showPasswordField = computed(() => {
  return formData.value.accountType === 'premium';
});

const minPasswordLength = computed(() => {
  return formData.value.accountType === 'premium' ? 12 : 8;
});
</script>
```

### 3. Real-time Formatting
```vue
<script setup>
const formatPhone = (value) => {
  // Format phone number as user types
  return value.replace(/(\d{3})(\d{3})(\d{4})/, '($1) $2-$3');
};

watch(() => formData.value.phone, (newValue) => {
  formData.value.phone = formatPhone(newValue);
});
</script>
```

### 4. Multi-step Form
```vue
<script setup>
const currentStep = ref(1);
const steps = ['Personal', 'Contact', 'Preferences'];

const isStepValid = computed(() => {
  // Validation logic untuk current step
  switch (currentStep.value) {
    case 1: return validationResults.value.name.isValid && validationResults.value.age.isValid;
    case 2: return validationResults.value.email.isValid && validationResults.value.phone.isValid;
    case 3: return validationResults.value.password.isValid;
    default: return false;
  }
});
</script>
```

### 5. Form Field Components
```vue
<!-- components/FormField.vue -->
<template>
  <div class="form-field">
    <label :for="fieldId">{{ label }}</label>
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      :type="type"
      :id="fieldId"
    />
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script setup>
defineProps({
  modelValue: String,
  label: String,
  type: String,
  fieldId: String,
  error: String
});

defineEmits(['update:modelValue']);
</script>
```

## 🎨 Customization Ideas

### 1. Theme Variations
```css
/* Dark Theme */
.dark-theme {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

/* Light Theme */
.light-theme {
  background: linear-gradient(135deg, #f5f5f5 0%, #e0e0e0 100%);
  color: #333;
}
```

### 2. Animation Libraries
```vue
<script setup>
import { useTransition } from '@vueuse/core';

const animatedProgress = useTransition(formStats.completion, {
  duration: 300
});
</script>
```

### 3. Advanced Feedback
```vue
<script setup>
const showTooltip = ref(false);
const tooltipMessage = ref('');

const showFieldTooltip = (message) => {
  tooltipMessage.value = message;
  showTooltip.value = true;
  setTimeout(() => showTooltip.value = false, 3000);
};
</script>
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Validasi tidak bekerja | Cek computed properties dan reactive data |
| Error messages tidak muncul | Pastikan touched fields tracking benar |
| Submit button tidak aktif | Verifikasi isFormValid computed property |
| Password strength tidak update | Cek validation logic dan reactivity |
| Form tidak auto-save | Pastikan watch function benar |

## 🚀 Next Steps

1. **Advanced Validation** - Custom rules dan async validation
2. **Field Components** - Reusable form field components
3. **Multi-step Forms** - Complex form workflows
4. **API Integration** - Server-side validation
5. **Accessibility** - Screen reader support
6. **Testing** - Unit dan E2E testing
7. **Performance** - Optimization techniques

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Form Validation yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [Form Validation Guide](https://vuejs.org/guide/essentials/forms.html) | [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)