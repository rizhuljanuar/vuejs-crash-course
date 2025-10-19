# 👀 Watchers - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja watchers untuk mengamati perubahan data dan menjalankan logika reaktif dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Watchers?](#-apa-itu-watchers)
- [🎮 Cara Menggunakan Watchers](#-cara-menggunakan-watchers)
- [⚡ Basic Watcher](#-basic-watcher)
- [🔄 Watcher dengan Getter Function](#-watcher-dengan-getter-function)
- [📊 Multiple Sources Watcher](#-multiple-sources-watcher)
- [🎯 Reactive Object Watcher](#-reactive-object-watcher)
- [🔧 Advanced Watcher Options](#-advanced-watcher-options)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Watchers?

**Watchers** adalah fitur dalam Vue.js yang memungkinkan kita untuk "mengawasi" perubahan pada data reaktif dan menjalankan fungsi ketika data tersebut berubah. Watchers sangat berguna untuk operasi yang asynchronous atau membutuhkan waktu eksekusi.

### Konsep Dasar Watchers

```javascript
// Syntax dasar watcher
watch(source, callback, options?)

// source: Data yang akan diawasi (ref, reactive object, atau getter function)
// callback: Fungsi yang dijalankan saat data berubah
// options: Konfigurasi tambahan (opsional)
```

### Mengapa Watchers Penting?

1. **⚡ Reaktifitas:** Otomatis menjalankan kode saat data berubah
2. **🌐 API Calls:** Mengambil data saat dependency berubah
3. **🔄 Side Effects:** Menjalankan operasi asynchronous
4. **🎯 Data Validation:** Validasi input secara real-time
5. **📊 Logging:** Mencatat perubahan data

### Watchers vs Computed Properties

| Fitur | Computed Properties | Watchers |
|-------|-------------------|----------|
| **Tujuan** | Menghitung turunan data | Menjalankan side effects |
| **Caching** | ✅ Otomatis di-cache | ❌ Tidak di-cache |
| **Return Value** | ✅ Mengembalikan nilai | ❌ Tidak mengembalikan nilai |
| **Side Effects** | ❌ Sebaiknya tidak | ✅ Dirancang untuk side effects |
| **Use Case** | Template data transformation | API calls, validation, logging |

## 🎮 Cara Menggunakan Watchers

### 1. Import Watch Function

```vue
<script setup>
import { ref, reactive, watch } from 'vue'
</script>
```

### 2. Basic Syntax

```vue
<script setup>
const count = ref(0)

// Basic watcher
watch(count, (newValue, oldValue) => {
  console.log(`Count changed from ${oldValue} to ${newValue}`)
})
</script>
```

### 3. Template Integration

```vue
<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="count++">Increment</button>
  </div>
</template>
```

## ⚡ Basic Watcher

### Simple Value Watching

```vue
<script setup>
import { ref, watch } from 'vue'

const message = ref('Hello, Vue!')
const inputText = ref('')
const characterCount = ref(0)

// Watch for changes in inputText
watch(inputText, (newValue, oldValue) => {
  console.log('Input changed from', oldValue, 'to', newValue)

  // Update message based on input
  message.value = `You typed: ${newValue}`

  // Update character count
  characterCount.value = newValue.length
})

// Watch message changes
watch(message, (newValue) => {
  console.log('Message updated:', newValue)
})
</script>

<template>
  <div class="basic-watcher-demo">
    <h2>{{ message }}</h2>

    <div class="input-section">
      <label for="textInput">Type something:</label>
      <input
        id="textInput"
        v-model="inputText"
        placeholder="Start typing..."
        type="text"
      />
    </div>

    <div class="stats">
      <p>Character count: {{ characterCount }}</p>
      <p>Last input: {{ inputText || 'Nothing yet' }}</p>
    </div>
  </div>
</template>

<style scoped>
.basic-watcher-demo {
  padding: 20px;
  max-width: 500px;
  margin: 0 auto;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
}

h2 {
  color: #3b82f6;
  margin-bottom: 20px;
  text-align: center;
}

.input-section {
  margin-bottom: 20px;
}

.input-section label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #374151;
}

.input-section input {
  width: 100%;
  padding: 10px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.input-section input:focus {
  outline: none;
  border-color: #3b82f6;
}

.stats {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.stats p {
  margin: 5px 0;
  color: #475569;
}
</style>
```

### Real-time Form Validation

```vue
<script setup>
import { ref, watch } from 'vue'

const email = ref('')
const password = ref('')
const emailError = ref('')
const passwordError = ref('')
const isValid = ref(false)

// Email validation watcher
watch(email, (newValue) => {
  if (!newValue) {
    emailError.value = 'Email is required'
  } else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(newValue)) {
    emailError.value = 'Please enter a valid email'
  } else {
    emailError.value = ''
  }
})

// Password validation watcher
watch(password, (newValue) => {
  if (!newValue) {
    passwordError.value = 'Password is required'
  } else if (newValue.length < 8) {
    passwordError.value = 'Password must be at least 8 characters'
  } else if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(newValue)) {
    passwordError.value = 'Password must contain uppercase, lowercase, and number'
  } else {
    passwordError.value = ''
  }
})

// Form validity watcher
watch([email, password, emailError, passwordError], () => {
  isValid.value = email.value &&
                 password.value &&
                 !emailError.value &&
                 !passwordError.value
})

const handleSubmit = () => {
  if (isValid.value) {
    console.log('Form submitted:', { email: email.value, password: password.value })
    alert('Form submitted successfully!')
  }
}
</script>

<template>
  <div class="form-validation-demo">
    <h2>Real-time Form Validation</h2>

    <form @submit.prevent="handleSubmit" class="validation-form">
      <div class="form-group">
        <label for="email">Email:</label>
        <input
          id="email"
          v-model="email"
          type="email"
          placeholder="Enter your email"
          :class="{ error: emailError }"
        />
        <span v-if="emailError" class="error-message">{{ emailError }}</span>
      </div>

      <div class="form-group">
        <label for="password">Password:</label>
        <input
          id="password"
          v-model="password"
          type="password"
          placeholder="Enter your password"
          :class="{ error: passwordError }"
        />
        <span v-if="passwordError" class="error-message">{{ passwordError }}</span>
      </div>

      <button
        type="submit"
        :disabled="!isValid"
        :class="{ disabled: !isValid }"
      >
        Submit
      </button>
    </form>

    <div class="validation-status">
      <p>Form is {{ isValid ? '✅ Valid' : '❌ Invalid' }}</p>
    </div>
  </div>
</template>

<style scoped>
.form-validation-demo {
  padding: 20px;
  max-width: 400px;
  margin: 0 auto;
}

.validation-form {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
  color: #374151;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.3s;
}

.form-group input.error {
  border-color: #ef4444;
}

.error-message {
  display: block;
  color: #ef4444;
  font-size: 12px;
  margin-top: 5px;
}

button {
  width: 100%;
  padding: 12px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover:not(.disabled) {
  background: #2563eb;
}

button.disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.validation-status {
  margin-top: 20px;
  text-align: center;
  padding: 10px;
  background: #f8fafc;
  border-radius: 4px;
}
</style>
```

## 🔄 Watcher dengan Getter Function

### Watching Specific Properties

```vue
<script setup>
import { reactive, watch } from 'vue'

const state = reactive({
  username: 'alex',
  email: 'alex@example.com',
  age: 25,
  preferences: {
    theme: 'light',
    notifications: true
  }
})

const usernameHistory = ref([])
const emailHistory = ref([])

// Watch specific property with getter function
watch(
  () => state.username,
  (newUsername, oldUsername) => {
    console.log('Username changed from', oldUsername, 'to', newUsername)

    usernameHistory.value.push({
      from: oldUsername,
      to: newUsername,
      timestamp: new Date().toLocaleTimeString()
    })
  }
)

// Watch email with getter function
watch(
  () => state.email,
  (newEmail, oldEmail) => {
    console.log('Email changed from', oldEmail, 'to', newEmail)

    emailHistory.value.push({
      from: oldEmail,
      to: newEmail,
      timestamp: new Date().toLocaleTimeString()
    })
  }
)

// Watch nested property with getter function
watch(
  () => state.preferences.theme,
  (newTheme, oldTheme) => {
    console.log('Theme changed from', oldTheme, 'to', newTheme)
    document.documentElement.className = newTheme
  }
)

const changeUsername = (newName) => {
  state.username = newName
}

const changeEmail = (newEmail) => {
  state.email = newEmail
}

const toggleTheme = () => {
  state.preferences.theme = state.preferences.theme === 'light' ? 'dark' : 'light'
}

const clearHistory = () => {
  usernameHistory.value = []
  emailHistory.value = []
}
</script>

<template>
  <div class="getter-watcher-demo">
    <h2>Watcher with Getter Function</h2>

    <div class="current-state">
      <h3>Current State:</h3>
      <p><strong>Username:</strong> {{ state.username }}</p>
      <p><strong>Email:</strong> {{ state.email }}</p>
      <p><strong>Theme:</strong> {{ state.preferences.theme }}</p>
    </div>

    <div class="controls">
      <div class="button-group">
        <h4>Change Username:</h4>
        <button @click="changeUsername('jordan')">Change to Jordan</button>
        <button @click="changeUsername('sarah')">Change to Sarah</button>
        <button @click="changeUsername('mike')">Change to Mike</button>
      </div>

      <div class="button-group">
        <h4>Change Email:</h4>
        <button @click="changeEmail('jordan@example.com')">
          Change to jordan@example.com
        </button>
        <button @click="changeEmail('sarah@example.com')">
          Change to sarah@example.com
        </button>
      </div>

      <div class="button-group">
        <h4>Toggle Theme:</h4>
        <button @click="toggleTheme">Toggle Theme</button>
      </div>

      <button @click="clearHistory" class="clear-btn">Clear History</button>
    </div>

    <div class="history">
      <div class="history-section">
        <h4>Username History:</h4>
        <div v-if="usernameHistory.length === 0" class="empty-history">
          No changes yet
        </div>
        <div v-else>
          <div
            v-for="(change, index) in usernameHistory"
            :key="index"
            class="history-item"
          >
            <span class="time">{{ change.timestamp }}</span>
            <span class="change">
              {{ change.from }} → {{ change.to }}
            </span>
          </div>
        </div>
      </div>

      <div class="history-section">
        <h4>Email History:</h4>
        <div v-if="emailHistory.length === 0" class="empty-history">
          No changes yet
        </div>
        <div v-else>
          <div
            v-for="(change, index) in emailHistory"
            :key="index"
            class="history-item"
          >
            <span class="time">{{ change.timestamp }}</span>
            <span class="change">
              {{ change.from }} → {{ change.to }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.getter-watcher-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
}

.current-state {
  background: #dbeafe;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.controls {
  margin-bottom: 20px;
}

.button-group {
  margin-bottom: 15px;
  padding: 10px;
  background: #f8fafc;
  border-radius: 8px;
}

.button-group h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #475569;
}

.button-group button {
  margin-right: 10px;
  margin-bottom: 5px;
  padding: 5px 10px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.clear-btn {
  background: #ef4444 !important;
  padding: 8px 16px;
  font-size: 14px;
}

.history {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.history-section {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
}

.history-section h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.empty-history {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
}

.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 0;
  border-bottom: 1px solid #f1f5f9;
}

.history-item:last-child {
  border-bottom: none;
}

.time {
  font-family: monospace;
  font-size: 11px;
  color: #64748b;
}

.change {
  font-size: 12px;
  color: #334155;
}
</style>
```

### Computed Property Watching

```vue
<script setup>
import { ref, computed, watch } from 'vue'

const firstName = ref('')
const lastName = ref('')
const age = ref(0)

const fullName = computed(() => {
  return `${firstName.value} ${lastName.value}`.trim()
})

const isAdult = computed(() => {
  return age.value >= 18
})

const canVote = computed(() => {
  return fullName.value && isAdult.value
})

// Watch computed property
watch(fullName, (newName, oldName) => {
  console.log(`Full name changed from "${oldName}" to "${newName}"`)
})

watch(isAdult, (newStatus, oldStatus) => {
  if (newStatus !== oldStatus) {
    console.log(`Age status changed from ${oldStatus ? 'adult' : 'minor'} to ${newStatus ? 'adult' : 'minor'}`)
  }
})

watch(canVote, (newStatus, oldStatus) => {
  if (newStatus !== oldStatus) {
    console.log(`Voting eligibility changed from ${oldStatus ? 'eligible' : 'not eligible'} to ${newStatus ? 'eligible' : 'not eligible'}`)
  }
})
</script>

<template>
  <div class="computed-watcher-demo">
    <h2>Watching Computed Properties</h2>

    <div class="form-section">
      <div class="input-group">
        <label for="firstName">First Name:</label>
        <input id="firstName" v-model="firstName" placeholder="Enter first name" />
      </div>

      <div class="input-group">
        <label for="lastName">Last Name:</label>
        <input id="lastName" v-model="lastName" placeholder="Enter last name" />
      </div>

      <div class="input-group">
        <label for="age">Age:</label>
        <input
          id="age"
          v-model.number="age"
          type="number"
          min="0"
          max="120"
          placeholder="Enter age"
        />
      </div>
    </div>

    <div class="results">
      <h3>Computed Results:</h3>
      <p><strong>Full Name:</strong> {{ fullName || 'Not provided' }}</p>
      <p><strong>Is Adult:</strong> {{ isAdult ? 'Yes (18+)' : 'No (under 18)' }}</p>
      <p><strong>Can Vote:</strong>
        <span :class="{ eligible: canVote, 'not-eligible': !canVote }">
          {{ canVote ? '✅ Eligible' : '❌ Not Eligible' }}
        </span>
      </p>
    </div>

    <div class="requirements">
      <h4>Voting Requirements:</h4>
      <ul>
        <li :class="{ met: firstName, 'not-met': !firstName }">
          First name provided: {{ firstName ? '✅' : '❌' }}
        </li>
        <li :class="{ met: lastName, 'not-met': !lastName }">
          Last name provided: {{ lastName ? '✅' : '❌' }}
        </li>
        <li :class="{ met: isAdult, 'not-met': !isAdult }">
          18 years or older: {{ isAdult ? '✅' : '❌' }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.computed-watcher-demo {
  padding: 20px;
  max-width: 500px;
  margin: 0 auto;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
}

.form-section {
  margin-bottom: 20px;
}

.input-group {
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
  color: #374151;
}

.input-group input {
  width: 100%;
  padding: 8px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
}

.input-group input:focus {
  outline: none;
  border-color: #3b82f6;
}

.results {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.results p {
  margin: 8px 0;
  color: #475569;
}

.eligible {
  color: #16a34a;
  font-weight: bold;
}

.not-eligible {
  color: #dc2626;
  font-weight: bold;
}

.requirements {
  background: #fef3c7;
  padding: 15px;
  border-radius: 8px;
}

.requirements h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #92400e;
}

.requirements ul {
  margin: 0;
  padding-left: 20px;
}

.requirements li {
  margin: 5px 0;
  padding: 5px;
  border-radius: 4px;
}

.met {
  color: #16a34a;
  background: #dcfce7;
}

.not-met {
  color: #dc2626;
  background: #fee2e2;
}
</style>
```

## 📊 Multiple Sources Watcher

### Watching Multiple Refs

```vue
<script setup>
import { ref, watch } from 'vue'

const username = ref('jordan')
const counter = ref(0)
const message = ref('')

// Watch multiple sources
watch([username, counter], (newValues, oldValues) => {
  console.log('New Values:', newValues)
  console.log('Old Values:', oldValues)

  const [newUsername, newCounter] = newValues
  const [oldUsername, oldCounter] = oldValues

  // Update message based on what changed
  if (newUsername !== oldUsername) {
    message.value = `Username changed from "${oldUsername}" to "${newUsername}"`
  } else if (newCounter !== oldCounter) {
    message.value = `Counter changed from ${oldCounter} to ${newCounter}`
  }
})

const changeUsername = (newName) => {
  username.value = newName
}

const incrementCounter = () => {
  counter.value++
}

const decrementCounter = () => {
  counter.value--
}

const resetAll = () => {
  username.value = 'jordan'
  counter.value = 0
  message.value = 'All values reset'
}
</script>

<template>
  <div class="multiple-watcher-demo">
    <h2>Multiple Sources Watcher</h2>

    <div class="state-display">
      <div class="state-item">
        <h3>Username</h3>
        <p class="value">{{ username }}</p>
        <div class="buttons">
          <button @click="changeUsername('HuXn')">Change to HuXn</button>
          <button @click="changeUsername('Sarah')">Change to Sarah</button>
        </div>
      </div>

      <div class="state-item">
        <h3>Counter</h3>
        <p class="value">{{ counter }}</p>
        <div class="buttons">
          <button @click="decrementCounter">-</button>
          <button @click="incrementCounter">+</button>
        </div>
      </div>
    </div>

    <div class="message-display">
      <h3>Last Change:</h3>
      <p :class="{ 'has-message': message }">
        {{ message || 'No changes yet' }}
      </p>
    </div>

    <div class="controls">
      <button @click="resetAll" class="reset-btn">Reset All</button>
    </div>

    <div class="info-box">
      <h4>ℹ️ How it works:</h4>
      <p>This watcher monitors both <code>username</code> and <code>counter</code> simultaneously.</p>
      <p>When either value changes, the watcher triggers and logs both old and new values as arrays.</p>
    </div>
  </div>
</template>

<style scoped>
.multiple-watcher-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.state-display {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.state-item {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
}

.state-item h3 {
  margin-top: 0;
  color: #475569;
}

.value {
  font-size: 24px;
  font-weight: bold;
  color: #3b82f6;
  margin: 10px 0;
}

.buttons {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.buttons button {
  padding: 8px 16px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.buttons button:hover {
  background: #2563eb;
}

.message-display {
  background: #dbeafe;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.message-display h3 {
  margin-top: 0;
  color: #1e40af;
}

.message-display p {
  margin: 10px 0;
  padding: 10px;
  background: white;
  border-radius: 4px;
  min-height: 20px;
}

.message-display .has-message {
  color: #1e40af;
  font-weight: 500;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.reset-btn {
  padding: 10px 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.reset-btn:hover {
  background: #dc2626;
}

.info-box {
  background: #f0f9ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #0ea5e9;
}

.info-box h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.info-box p {
  margin: 8px 0;
  color: #0c4a6e;
}

.info-box code {
  background: #e0f2fe;
  padding: 2px 6px;
  border-radius: 3px;
  font-family: monospace;
  font-size: 14px;
}
</style>
```

### Complex Multiple Sources

```vue
<script setup>
import { ref, reactive, watch } from 'vue'

const searchQuery = ref('')
const selectedCategory = ref('all')
const priceRange = ref({ min: 0, max: 1000 })
const sortBy = ref('name')

const products = ref([
  { id: 1, name: 'Laptop', price: 999, category: 'electronics' },
  { id: 2, name: 'Book', price: 29, category: 'books' },
  { id: 3, name: 'Headphones', price: 199, category: 'electronics' },
  { id: 4, name: 'Pen', price: 5, category: 'stationery' },
  { id: 5, name: 'Monitor', price: 399, category: 'electronics' },
  { id: 6, name: 'Notebook', price: 15, category: 'stationery' }
])

const filteredProducts = ref([])
const lastFilterChange = ref('')

// Watch multiple sources for filtering
watch(
  [searchQuery, selectedCategory, priceRange, sortBy],
  ([newQuery, newCategory, newPriceRange, newSortBy], [oldQuery, oldCategory, oldPriceRange, oldSortBy]) => {
    console.log('Filters changed:', {
      query: oldQuery + ' → ' + newQuery,
      category: oldCategory + ' → ' + newCategory,
      priceRange: `${oldPriceRange.min}-${oldPriceRange.max} → ${newPriceRange.min}-${newPriceRange.max}`,
      sortBy: oldSortBy + ' → ' + newSortBy
    })

    // Determine what changed
    if (newQuery !== oldQuery) {
      lastFilterChange.value = `Search query changed to "${newQuery}"`
    } else if (newCategory !== oldCategory) {
      lastFilterChange.value = `Category changed to "${newCategory}"`
    } else if (JSON.stringify(newPriceRange) !== JSON.stringify(oldPriceRange)) {
      lastFilterChange.value = `Price range changed to $${newPriceRange.min} - $${newPriceRange.max}`
    } else if (newSortBy !== oldSortBy) {
      lastFilterChange.value = `Sort order changed to "${newSortBy}"`
    }

    // Apply filters
    let filtered = [...products.value]

    // Filter by search query
    if (newQuery) {
      filtered = filtered.filter(product =>
        product.name.toLowerCase().includes(newQuery.toLowerCase())
      )
    }

    // Filter by category
    if (newCategory !== 'all') {
      filtered = filtered.filter(product =>
        product.category === newCategory
      )
    }

    // Filter by price range
    filtered = filtered.filter(product =>
      product.price >= newPriceRange.min && product.price <= newPriceRange.max
    )

    // Sort products
    filtered.sort((a, b) => {
      if (newSortBy === 'name') {
        return a.name.localeCompare(b.name)
      } else if (newSortBy === 'price') {
        return a.price - b.price
      } else if (newSortBy === 'category') {
        return a.category.localeCompare(b.category)
      }
      return 0
    })

    filteredProducts.value = filtered
  },
  { immediate: true } // Run immediately on component mount
)
</script>

<template>
  <div class="product-filter-demo">
    <h2>Product Filter with Multiple Watchers</h2>

    <div class="filters">
      <div class="filter-group">
        <label for="search">Search:</label>
        <input
          id="search"
          v-model="searchQuery"
          placeholder="Search products..."
          type="text"
        />
      </div>

      <div class="filter-group">
        <label for="category">Category:</label>
        <select id="category" v-model="selectedCategory">
          <option value="all">All Categories</option>
          <option value="electronics">Electronics</option>
          <option value="books">Books</option>
          <option value="stationery">Stationery</option>
        </select>
      </div>

      <div class="filter-group">
        <label>Price Range:</label>
        <div class="price-inputs">
          <input
            v-model.number="priceRange.min"
            type="number"
            placeholder="Min"
            min="0"
          />
          <span>-</span>
          <input
            v-model.number="priceRange.max"
            type="number"
            placeholder="Max"
            min="0"
          />
        </div>
      </div>

      <div class="filter-group">
        <label for="sortBy">Sort by:</label>
        <select id="sortBy" v-model="sortBy">
          <option value="name">Name</option>
          <option value="price">Price</option>
          <option value="category">Category</option>
        </select>
      </div>
    </div>

    <div class="filter-status">
      <p><strong>Last Change:</strong> {{ lastFilterChange || 'No filters applied yet' }}</p>
      <p><strong>Results:</strong> {{ filteredProducts.length }} of {{ products.length }} products</p>
    </div>

    <div class="products-grid">
      <div
        v-for="product in filteredProducts"
        :key="product.id"
        class="product-card"
      >
        <h4>{{ product.name }}</h4>
        <p class="price">${{ product.price }}</p>
        <span class="category">{{ product.category }}</span>
      </div>
    </div>

    <div v-if="filteredProducts.length === 0" class="no-results">
      <p>No products found matching your criteria.</p>
    </div>
  </div>
</template>

<style scoped>
.product-filter-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
}

.filters {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
}

.filter-group {
  display: flex;
  flex-direction: column;
}

.filter-group label {
  margin-bottom: 5px;
  font-weight: 500;
  color: #374151;
}

.filter-group input,
.filter-group select {
  padding: 8px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  font-size: 14px;
}

.price-inputs {
  display: flex;
  align-items: center;
  gap: 10px;
}

.price-inputs input {
  width: 80px;
}

.filter-status {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  padding: 10px;
  background: #dbeafe;
  border-radius: 4px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
}

.product-card {
  background: white;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  border: 1px solid #e2e8f0;
}

.product-card h4 {
  margin: 0 0 10px 0;
  color: #1f2937;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #059669;
  margin: 10px 0;
}

.category {
  display: inline-block;
  background: #e5e7eb;
  color: #374151;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
}

.no-results {
  text-align: center;
  padding: 40px;
  color: #6b7280;
  font-style: italic;
}
</style>
```

## 🎯 Reactive Object Watcher

### Watching Reactive Objects

```vue
<script setup>
import { reactive, watch } from 'vue'

const state = reactive({
  username: 'alex',
  email: 'alex@example.com',
  profile: {
    firstName: 'Alex',
    lastName: 'Johnson',
    age: 25
  },
  settings: {
    theme: 'light',
    notifications: true
  }
})

const changeLog = ref([])

// Watch entire reactive object
watch(state, (newState, oldState) => {
  console.log('State changed:', newState)
  console.log('Previous state:', oldState)

  // Note: With reactive objects, newState and oldState will be the same reference
  // This is because Vue tracks mutations on the same object
  changeLog.value.push({
    timestamp: new Date().toLocaleTimeString(),
    message: 'State object mutated',
    currentState: JSON.parse(JSON.stringify(newState))
  })
})

// Add change methods
const updateUsername = (newUsername) => {
  state.username = newUsername
}

const updateEmail = (newEmail) => {
  state.email = newEmail
}

const updateProfile = (updates) => {
  Object.assign(state.profile, updates)
}

const updateSettings = (updates) => {
  Object.assign(state.settings, updates)
}

const resetState = () => {
  state.username = 'alex'
  state.email = 'alex@example.com'
  state.profile = { firstName: 'Alex', lastName: 'Johnson', age: 25 }
  state.settings = { theme: 'light', notifications: true }
  changeLog.value = []
}
</script>

<template>
  <div class="reactive-watcher-demo">
    <h2>Watching Reactive Objects</h2>

    <div class="warning-box">
      <h4>⚠️ Important Note:</h4>
      <p>When watching reactive objects, <strong>newState and oldState will be the same reference</strong>.</p>
      <p>Vue tracks mutations on the same object rather than creating new ones.</p>
    </div>

    <div class="current-state">
      <h3>Current State:</h3>
      <div class="state-display">
        <div class="state-section">
          <h4>User Info:</h4>
          <p><strong>Username:</strong> {{ state.username }}</p>
          <p><strong>Email:</strong> {{ state.email }}</p>
        </div>

        <div class="state-section">
          <h4>Profile:</h4>
          <p><strong>Name:</strong> {{ state.profile.firstName }} {{ state.profile.lastName }}</p>
          <p><strong>Age:</strong> {{ state.profile.age }}</p>
        </div>

        <div class="state-section">
          <h4>Settings:</h4>
          <p><strong>Theme:</strong> {{ state.settings.theme }}</p>
          <p><strong>Notifications:</strong> {{ state.settings.notifications ? 'Enabled' : 'Disabled' }}</p>
        </div>
      </div>
    </div>

    <div class="controls">
      <h4>Update User Info:</h4>
      <div class="button-group">
        <button @click="updateUsername('jordan')">Username: Jordan</button>
        <button @click="updateUsername('sarah')">Username: Sarah</button>
        <button @click="updateEmail('jordan@example.com')">Update Email</button>
      </div>

      <h4>Update Profile:</h4>
      <div class="button-group">
        <button @click="updateProfile({ firstName: 'Jordan', lastName: 'Smith' })">
          Name: Jordan Smith
        </button>
        <button @click="updateProfile({ age: 26 })">
          Age: 26
        </button>
        <button @click="updateProfile({ firstName: 'Sarah', lastName: 'Johnson', age: 24 })">
          Profile: Sarah Johnson, 24
        </button>
      </div>

      <h4>Update Settings:</h4>
      <div class="button-group">
        <button @click="updateSettings({ theme: 'dark' })">
          Theme: Dark
        </button>
        <button @click="updateSettings({ notifications: false })">
          Disable Notifications
        </button>
      </div>

      <button @click="resetState" class="reset-btn">Reset All</button>
    </div>

    <div class="change-log">
      <h4>Change Log:</h4>
      <div v-if="changeLog.length === 0" class="empty-log">
        No changes yet. Try updating some values above.
      </div>
      <div v-else>
        <div
          v-for="(log, index) in changeLog"
          :key="index"
          class="log-entry"
        >
          <span class="timestamp">{{ log.timestamp }}</span>
          <span class="message">{{ log.message }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.reactive-watcher-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.warning-box {
  background: #fef3c7;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
  margin-bottom: 20px;
}

.warning-box h4 {
  margin-top: 0;
  color: #92400e;
}

.warning-box p {
  margin: 8px 0;
  color: #92400e;
}

.current-state {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.current-state h3 {
  margin-top: 0;
  color: #374151;
}

.state-display {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.state-section {
  background: white;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #e2e8f0;
}

.state-section h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #475569;
  border-bottom: 1px solid #e2e8f0;
  padding-bottom: 5px;
}

.state-section p {
  margin: 5px 0;
  font-size: 14px;
  color: #334155;
}

.controls {
  margin-bottom: 20px;
}

.controls h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.button-group {
  margin-bottom: 15px;
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.button-group button {
  padding: 6px 12px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: background-color 0.3s;
}

.button-group button:hover {
  background: #2563eb;
}

.reset-btn {
  background: #ef4444 !important;
  padding: 10px 20px;
  font-size: 14px;
}

.reset-btn:hover {
  background: #dc2626 !important;
}

.change-log {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
  max-height: 200px;
  overflow-y: auto;
}

.change-log h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.empty-log {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
}

.log-entry {
  display: flex;
  gap: 10px;
  padding: 5px 0;
  border-bottom: 1px solid #f1f5f9;
  font-size: 12px;
}

.log-entry:last-child {
  border-bottom: none;
}

.timestamp {
  font-family: monospace;
  color: #64748b;
  min-width: 80px;
}

.message {
  color: #334155;
}
</style>
```

### Deep Watching Nested Objects

```vue
<script setup>
import { reactive, watch } from 'vue'

const user = reactive({
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  profile: {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
    address: {
      street: '123 Main St',
      city: 'New York',
      country: 'USA',
      coordinates: {
        lat: 40.7128,
        lng: -74.0060
      }
    },
    preferences: {
      theme: 'light',
      language: 'en',
      notifications: {
        email: true,
        sms: false,
        push: true
      }
    }
  },
  activity: {
    lastLogin: new Date(),
    loginCount: 42,
    sessions: []
  }
})

const activityLog = ref([])

// Deep watch for nested changes
watch(
  () => JSON.parse(JSON.stringify(user)), // Deep clone to get old values
  (newUser, oldUser) => {
    const change = {
      timestamp: new Date().toLocaleTimeString(),
      type: 'deep_change',
      message: 'Nested object changed'
    }

    // Find what actually changed (simplified)
    if (newUser.name !== oldUser.name) {
      change.message = `Name changed: ${oldUser.name} → ${newUser.name}`
    } else if (newUser.email !== oldUser.email) {
      change.message = `Email changed: ${oldUser.email} → ${newUser.email}`
    } else if (newUser.profile.age !== oldUser.profile.age) {
      change.message = `Age changed: ${oldUser.profile.age} → ${newUser.profile.age}`
    } else if (newUser.profile.preferences.theme !== oldUser.profile.preferences.theme) {
      change.message = `Theme changed: ${oldUser.profile.preferences.theme} → ${newUser.profile.preferences.theme}`
    } else {
      change.message = 'Some nested property changed'
    }

    activityLog.value.unshift(change)

    // Keep only last 10 changes
    if (activityLog.value.length > 10) {
      activityLog.value = activityLog.value.slice(0, 10)
    }
  },
  { deep: true }
)

// Update methods
const updateName = (newName) => {
  user.name = newName
}

const updateAge = (newAge) => {
  user.profile.age = newAge
}

const updateAddress = (field, value) => {
  user.profile.address[field] = value
}

const updateCoordinates = (lat, lng) => {
  user.profile.address.coordinates.lat = lat
  user.profile.address.coordinates.lng = lng
}

const updateTheme = (theme) => {
  user.profile.preferences.theme = theme
}

const toggleNotification = (type) => {
  user.profile.preferences.notifications[type] = !user.profile.preferences.notifications[type]
}

const addSession = () => {
  user.activity.sessions.push({
    id: Date.now(),
    start: new Date(),
    duration: Math.floor(Math.random() * 60) + 1
  })
}
</script>

<template>
  <div class="deep-watcher-demo">
    <h2>Deep Watching Nested Objects</h2>

    <div class="info-box">
      <h4>🔍 Deep Watch:</h4>
      <p>This watcher uses <code>deep: true</code> to detect changes in nested objects.</p>
      <p>It monitors changes at any level within the reactive object.</p>
    </div>

    <div class="user-display">
      <h3>Current User Data:</h3>
      <div class="user-grid">
        <div class="user-section">
          <h4>Basic Info</h4>
          <p><strong>ID:</strong> {{ user.id }}</p>
          <p><strong>Name:</strong> {{ user.name }}</p>
          <p><strong>Email:</strong> {{ user.email }}</p>
        </div>

        <div class="user-section">
          <h4>Profile</h4>
          <p><strong>Age:</strong> {{ user.profile.age }}</p>
          <p><strong>Address:</strong> {{ user.profile.address.street }}, {{ user.profile.address.city }}</p>
          <p><strong>Coordinates:</strong> {{ user.profile.address.coordinates.lat }}, {{ user.profile.address.coordinates.lng }}</p>
        </div>

        <div class="user-section">
          <h4>Preferences</h4>
          <p><strong>Theme:</strong> {{ user.profile.preferences.theme }}</p>
          <p><strong>Email Notifications:</strong> {{ user.profile.preferences.notifications.email ? '✅' : '❌' }}</p>
          <p><strong>SMS Notifications:</strong> {{ user.profile.preferences.notifications.sms ? '✅' : '❌' }}</p>
          <p><strong>Push Notifications:</strong> {{ user.profile.preferences.notifications.push ? '✅' : '❌' }}</p>
        </div>

        <div class="user-section">
          <h4>Activity</h4>
          <p><strong>Login Count:</strong> {{ user.activity.loginCount }}</p>
          <p><strong>Sessions:</strong> {{ user.activity.sessions.length }}</p>
        </div>
      </div>
    </div>

    <div class="controls">
      <h4>Make Changes:</h4>

      <div class="control-group">
        <h5>Basic Info:</h5>
        <button @click="updateName('Jane Doe')">Change Name to Jane</button>
        <button @click="updateName('Bob Smith')">Change Name to Bob</button>
      </div>

      <div class="control-group">
        <h5>Profile:</h5>
        <button @click="updateAge(25)">Age: 25</button>
        <button @click="updateAge(35)">Age: 35</button>
        <button @click="updateAddress('city', 'Los Angeles')">City: Los Angeles</button>
        <button @click="updateCoordinates(34.0522, -118.2437)">Move to LA</button>
      </div>

      <div class="control-group">
        <h5>Preferences:</h5>
        <button @click="updateTheme('dark')">Theme: Dark</button>
        <button @click="updateTheme('light')">Theme: Light</button>
        <button @click="toggleNotification('email')">Toggle Email</button>
        <button @click="toggleNotification('sms')">Toggle SMS</button>
      </div>

      <div class="control-group">
        <h5>Activity:</h5>
        <button @click="addSession">Add Session</button>
      </div>
    </div>

    <div class="activity-log">
      <h4>Activity Log (Last 10 changes):</h4>
      <div v-if="activityLog.length === 0" class="empty-log">
        No changes detected yet. Make some changes above to see the watcher in action.
      </div>
      <div v-else>
        <div
          v-for="(log, index) in activityLog"
          :key="index"
          class="log-entry"
        >
          <span class="timestamp">{{ log.timestamp }}</span>
          <span class="message">{{ log.message }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.deep-watcher-demo {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.info-box {
  background: #f3e8ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
  margin-bottom: 20px;
}

.info-box h4 {
  margin-top: 0;
  color: #6b21a8;
}

.info-box p {
  margin: 8px 0;
  color: #6b21a8;
}

.info-box code {
  background: #e9d5ff;
  padding: 2px 6px;
  border-radius: 3px;
  font-family: monospace;
  font-size: 14px;
}

.user-display {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.user-display h3 {
  margin-top: 0;
  color: #374151;
}

.user-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.user-section {
  background: white;
  padding: 15px;
  border-radius: 6px;
  border: 1px solid #e2e8f0;
}

.user-section h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #475569;
  border-bottom: 1px solid #e2e8f0;
  padding-bottom: 5px;
}

.user-section p {
  margin: 5px 0;
  font-size: 13px;
  color: #334155;
}

.controls {
  margin-bottom: 20px;
}

.controls h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #374151;
}

.control-group {
  margin-bottom: 15px;
  padding: 10px;
  background: #f1f5f9;
  border-radius: 6px;
}

.control-group h5 {
  margin-top: 0;
  margin-bottom: 8px;
  color: #475569;
  font-size: 14px;
}

.control-group button {
  margin-right: 8px;
  margin-bottom: 5px;
  padding: 5px 10px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: background-color 0.3s;
}

.control-group button:hover {
  background: #7c3aed;
}

.activity-log {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
  max-height: 250px;
  overflow-y: auto;
}

.activity-log h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.empty-log {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
}

.log-entry {
  display: flex;
  gap: 10px;
  padding: 5px 0;
  border-bottom: 1px solid #f1f5f9;
  font-size: 12px;
}

.log-entry:last-child {
  border-bottom: none;
}

.timestamp {
  font-family: monospace;
  color: #64748b;
  min-width: 80px;
}

.message {
  color: #334155;
}
</style>
```

## 🔧 Advanced Watcher Options

### Immediate Watcher

```vue
<script setup>
import { ref, watch } from 'vue'

const count = ref(0)
const message = ref('Watcher ready!')

// Watcher with immediate option - runs immediately on creation
watch(
  count,
  (newValue, oldValue) => {
    console.log(`Count changed from ${oldValue} to ${newValue}`)
    message.value = `Count is now: ${newValue}`
  },
  { immediate: true } // This makes the watcher run immediately
)

const increment = () => {
  count.value++
}

const reset = () => {
  count.value = 0
}
</script>

<template>
  <div class="immediate-watcher-demo">
    <h2>Immediate Watcher</h2>

    <div class="display">
      <p><strong>Count:</strong> {{ count }}</p>
      <p><strong>Message:</strong> {{ message }}</p>
    </div>

    <div class="controls">
      <button @click="increment">Increment</button>
      <button @click="reset">Reset</button>
    </div>

    <div class="info">
      <p>ℹ️ This watcher runs immediately when the component is created, then runs on every change.</p>
    </div>
  </div>
</template>

<style scoped>
.immediate-watcher-demo {
  padding: 20px;
  border: 2px solid #10b981;
  border-radius: 8px;
  text-align: center;
}

.display {
  margin: 20px 0;
  padding: 15px;
  background: #d1fae5;
  border-radius: 8px;
}

.controls button {
  margin: 0 10px;
  padding: 8px 16px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.info {
  margin-top: 20px;
  padding: 10px;
  background: #f0fdf4;
  border-radius: 4px;
  font-style: italic;
  color: #166534;
}
</style>
```

### Watcher with Flush Options

```vue
<script setup>
import { ref, watch } from 'vue'

const message = ref('Initial message')
const log = ref([])

// Watcher with different flush options
const addLog = (source, value) => {
  log.value.push(`${source}: ${value}`)
}

// Pre-flush (default) - runs before component updates
watch(
  message,
  (newValue) => {
    addLog('Pre-flush', newValue)
  },
  { flush: 'pre' }
)

// Post-flush - runs after component updates
watch(
  message,
  (newValue) => {
    addLog('Post-flush', newValue)
  },
  { flush: 'post' }
)

// Sync - runs synchronously
watch(
  message,
  (newValue) => {
    addLog('Sync', newValue)
  },
  { flush: 'sync' }
)

const updateMessage = (newMessage) => {
  message.value = newMessage
}

const clearLog = () => {
  log.value = []
}
</script>

<template>
  <div class="flush-demo">
    <h2>Watcher Flush Options</h2>

    <div class="current-message">
      <p><strong>Current Message:</strong> {{ message }}</p>
    </div>

    <div class="controls">
      <button @click="updateMessage('Hello Vue!')">Set: Hello Vue!</button>
      <button @click="updateMessage('Updated message')">Set: Updated</button>
      <button @click="updateMessage('Final message')">Set: Final</button>
      <button @click="clearLog">Clear Log</button>
    </div>

    <div class="log-display">
      <h3>Execution Log:</h3>
      <div v-if="log.length === 0" class="empty">
        No logs yet. Update the message to see flush timing.
      </div>
      <div v-else>
        <div
          v-for="(entry, index) in log"
          :key="index"
          class="log-entry"
        >
          {{ entry }}
        </div>
      </div>
    </div>

    <div class="info">
      <h4>Flush Options:</h4>
      <ul>
        <li><strong>pre (default):</strong> Runs before component updates</li>
        <li><strong>post:</strong> Runs after component updates</li>
        <li><strong>sync:</strong> Runs synchronously when value changes</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.flush-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.current-message {
  text-align: center;
  font-size: 18px;
  margin: 20px 0;
  padding: 15px;
  background: #ecfeff;
  border-radius: 8px;
}

.controls {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 8px 12px;
  background: #06b6d4;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.log-display {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 20px;
  max-height: 200px;
  overflow-y: auto;
}

.log-display h3 {
  margin-top: 0;
  margin-bottom: 10px;
}

.empty {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
}

.log-entry {
  padding: 3px 0;
  font-family: monospace;
  font-size: 12px;
  border-bottom: 1px solid #f1f5f9;
}

.info {
  background: #f0f9ff;
  padding: 15px;
  border-radius: 8px;
}

.info h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info li {
  margin: 5px 0;
  color: #0c4a6e;
}
</style>
```

## 🚀 Best Practices

### 1. Use Watchers for Side Effects

```vue
<script setup>
import { ref, watch, computed } from 'vue'

const searchQuery = ref('')

// ✅ Good: Use computed for derived data
const filteredItems = computed(() => {
  return items.value.filter(item =>
    item.name.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

// ✅ Good: Use watcher for side effects (API calls)
watch(searchQuery, (newQuery) => {
  if (newQuery.length > 2) {
    searchAPI(newQuery)
  }
})

const searchAPI = async (query) => {
  // API call logic
}
</script>
```

### 2. Avoid Infinite Loops

```vue
<script setup>
import { ref, watch } from 'vue'

const count = ref(0)

// ❌ Bad: Causes infinite loop
watch(count, (newValue) => {
  count.value++ // This will trigger the watcher again!
})

// ✅ Good: Check conditions or use different approaches
watch(count, (newValue) => {
  if (newValue < 10) {
    // Only update under certain conditions
    console.log('Count is less than 10')
  }
})
</script>
```

### 3. Clean Up Watchers

```vue
<script setup>
import { ref, watch, onUnmounted } from 'vue'

const data = ref(null)

const stopWatcher = watch(data, (newValue) => {
  console.log('Data changed:', newValue)
})

// Clean up watcher when component unmounts
onUnmounted(() => {
  stopWatcher()
})
</script>
```

### 4. Use Appropriate Watcher Options

```vue
<script setup>
import { ref, watch } from 'vue'

const user = ref({ name: 'John', age: 30 })

// ✅ Use deep for objects
watch(user, (newUser) => {
  console.log('User object changed:', newUser)
}, { deep: true })

// ✅ Use immediate for initialization
watch(user, (newUser) => {
  console.log('User initialized or changed:', newUser)
}, { immediate: true })

// ✅ Use getter for specific properties
watch(
  () => user.value.name,
  (newName) => {
    console.log('Name changed:', newName)
  }
)
</script>
```

## 💡 Tips dan Trik

### 1. Debounced Watchers

```vue
<script setup>
import { ref, watch } from 'vue'

const searchQuery = ref('')
const results = ref([])
let debounceTimer = null

watch(searchQuery, (newQuery) => {
  clearTimeout(debounceTimer)

  debounceTimer = setTimeout(() => {
    if (newQuery.length > 2) {
      performSearch(newQuery)
    }
  }, 500)
})

const performSearch = async (query) => {
  // Search implementation
}
</script>
```

### 2. Watcher Cleanup

```vue
<script setup>
import { ref, watch } from 'vue'

const source = ref(null)

watch(source, (newValue, oldValue, onCleanup) => {
  const controller = new AbortController()

  fetch('/api/data', {
    signal: controller.signal
  }).then(response => {
    // Handle response
  })

  // Cleanup function
  onCleanup(() => {
    controller.abort()
  })
})
</script>
```

### 3. Conditional Watching

```vue
<script setup>
import { ref, watch } from 'vue'

const data = ref(null)
const isEnabled = ref(true)

let stopWatcher = null

watch(isEnabled, (enabled) => {
  if (enabled) {
    // Start watching
    stopWatcher = watch(data, (newValue) => {
      console.log('Data changed:', newValue)
    })
  } else {
    // Stop watching
    if (stopWatcher) {
      stopWatcher()
    }
  }
}, { immediate: true })
</script>
```

---

## 🎉 Kesimpulan

Watchers adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **⚡ Reactive Monitoring:** Mengamati perubahan data secara otomatis
2. **🌐 Async Operations:** Menjalankan API calls dan operasi asynchronous
3. **🔄 Side Effects:** Melakukan aksi saat data berubah
4. **📊 Data Validation:** Validasi input secara real-time
5. **🔧 Advanced Control:** Fine-grained control over reactivity

### Poin Penting:

- Gunakan `watch` untuk side effects, `computed` untuk derived data
- Gunakan getter functions untuk properties spesifik
- Gunakan `deep: true` untuk nested objects
- Gunakan `immediate: true` untuk inisialisasi
- Cleanup watchers untuk mencegah memory leaks

### Kapan Menggunakan Watchers:

- ✅ API calls saat data berubah
- ✅ Validasi form real-time
- ✅ Logging dan debugging
- ✅ Animasi dan transitions
- ✅ Complex data transformations

### Kapan Menghindari Watchers:

- ❌ Simple data transformations (gunakan computed)
- ❌ Template-only logic
- ❌ Performance-critical operations

Dengan memahami watchers, Anda sudah bisa membangun aplikasi Vue.js yang reaktif dan responsif! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan berbagai watcher patterns dalam aplikasi Anda untuk melihat kapan dan bagaimana mereka bekerja!