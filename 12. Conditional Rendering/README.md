# 🎭 Tutorial Vue.js: Conditional Rendering untuk Pemula

## 📖 Pengenalan Conditional Rendering

**Conditional Rendering** adalah fitur Vue.js yang memungkinkan Anda menampilkan atau menyembunyikan elemen HTML berdasarkan kondisi tertentu. Ini sangat berguna untuk membuat aplikasi yang dinamis dan interaktif.

## 🤔 Apa itu Conditional Rendering?

**Conditional Rendering** adalah teknik untuk mengontrol apakah elemen harus dirender atau tidak berdasarkan nilai boolean atau ekspresi JavaScript.

**Sintaks Dasar:**
```vue
<!-- v-if - Menghapus elemen dari DOM -->
<p v-if="isVisible">This is visible</p>

<!-- v-show - Menyembunyikan elemen dengan CSS -->
<p v-show="isVisible">This is shown/hidden</p>

<!-- v-if dengan v-else-if dan v-else -->
<div v-if="userRole === 'admin'">Admin content</div>
<div v-else-if="userRole === 'user'">User content</div>
<div v-else>Guest content</div>
```

## 📁 Struktur Proyek

```
12. Conditional Rendering/
├── README.md                                 # Dokumentasi ini
└── src/
    ├── components/
    │   └── ComputedProperties.vue            # Komponen dengan contoh conditional rendering
    └── App.vue                                # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `ComputedProperties.vue`

```vue
<script setup>
import { ref } from 'vue'
let password = ref('mypersonalpassword')
</script>

<template>
  <h1 v-if="password.length > 8">Strong Password</h1>
  <h1 v-else-if="password.length < 8">Weak Password</h1>
  <h1 v-else>Please Enter Your Password</h1>
</template>
```

**Penjelasan:**
- `v-if` akan menampilkan h1 jika password length > 8
- `v-else-if` akan menampilkan h1 jika password length < 8
- `v-else` akan menampilkan h1 untuk kondisi lainnya
- Vue akan mengevaluasi kondisi secara berurutan

## 📖 Konsep Fundamental Conditional Rendering

### 1. **v-if vs v-show**

#### **v-if - True Conditional Rendering**
```vue
<template>
  <div v-if="isVisible">
    <!-- Elemen ini akan dihapus/ditambahkan dari DOM -->
    <p>This content is completely removed when isVisible is false</p>
  </div>
</template>
```
- ✅ **Element dihapus dari DOM** ketika false
- ✅ **Lebih efisien** untuk content yang jarang berubah
- ✅ **Initial render cost** lebih tinggi
- ❌ **Toggle cost** lebih tinggi (add/remove DOM)

#### **v-show - CSS-based Toggle**
```vue
<template>
  <div v-show="isVisible">
    <!-- Elemen ini akan tetap di DOM dengan display: none -->
    <p>This content stays in DOM with display: none when hidden</p>
  </div>
</template>
```
- ✅ **Element tetap di DOM** dengan `display: none`
- ✅ **Lebih efisien** untuk frequent toggling
- ✅ **Toggle cost** lebih rendah (hanya CSS change)
- ❌ **Initial render cost** lebih rendah
- ❌ **Memory usage** lebih tinggi

### 2. **v-if, v-else-if, v-else Chain**
```vue
<template>
  <div v-if="score >= 90">Grade: A</div>
  <div v-else-if="score >= 80">Grade: B</div>
  <div v-else-if="score >= 70">Grade: C</div>
  <div v-else>Grade: F</div>
</template>
```

### 3. **Conditional Rendering dengan Multiple Elements**
```vue
<template>
  <!-- Gunakan <template> untuk mengelompokkan multiple elements -->
  <template v-if="isLoggedIn">
    <h1>Welcome back!</h1>
    <p>You have 5 new messages</p>
    <button>View Dashboard</button>
  </template>
</template>
```

### 4. **v-if dengan v-for**
```vue
<template>
  <!-- ⚠️ Tidak disarankan - v-for memiliki prioritas lebih tinggi -->
  <div v-if="shouldShowItems" v-for="item in items" :key="item.id">
    {{ item.name }}
  </div>

  <!-- ✅ Disarankan - Gunakan template wrapper -->
  <template v-if="shouldShowItems">
    <div v-for="item in items" :key="item.id">
      {{ item.name }}
    </div>
  </template>
</template>
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai conditional rendering:

### Enhanced Demo: Conditional Rendering Showcase

```vue
<script setup>
import { ref, computed } from 'vue'

// 1. Basic v-if and v-else
const isVisible = ref(true)
const isLoggedIn = ref(false)
const username = ref('John Doe')

// 2. Multiple conditions with v-else-if
const score = ref(85)
const userRole = ref('user')
const weather = ref('sunny')

// 3. Complex conditions
const age = ref(25)
const hasLicense = ref(true)
const hasCar = ref(false)

const canDrive = computed(() => age.value >= 18 && hasLicense.value)

// 4. v-show vs v-if comparison
const toggleMethod = ref('v-if') // 'v-if' atau 'v-show'
const showContent = ref(true)

// 5. User authentication states
const authState = ref('loading') // 'loading', 'authenticated', 'unauthenticated', 'error'

// 6. Product availability
const products = ref([
  { id: 1, name: 'Laptop', inStock: true, onSale: false, price: 15000000 },
  { id: 2, name: 'Mouse', inStock: false, onSale: true, price: 250000 },
  { id: 3, name: 'Keyboard', inStock: true, onSale: true, price: 750000 }
])

// 7. Form validation states
const formData = ref({
  email: '',
  password: '',
  confirmPassword: ''
})

const emailError = ref('')
const passwordError = ref('')

const isFormValid = computed(() => {
  return formData.value.email.length > 0 &&
         formData.value.password.length >= 6 &&
         formData.value.password === formData.value.confirmPassword
})

// 8. Loading and error states
const isLoading = ref(false)
const hasError = ref(false)
const errorMessage = ref('')

// 9. Permission-based content
const userPermissions = ref(['read', 'write'])
const requiredPermission = ref('admin')

// 10. Dynamic component loading
const activeTab = ref('profile')
const tabs = ref(['profile', 'settings', 'notifications', 'security'])

// Event handlers
function toggleVisibility() {
  isVisible.value = !isVisible.value
}

function login() {
  isLoggedIn.value = true
  authState.value = 'authenticated'
}

function logout() {
  isLoggedIn.value = false
  authState.value = 'unauthenticated'
}

function changeScore(newScore) {
  score.value = newScore
}

function changeRole(role) {
  userRole.value = role
}

function changeWeather(newWeather) {
  weather.value = newWeather
}

function ageUser() {
  age.value++
}

function toggleLicense() {
  hasLicense.value = !hasLicense.value
}

function toggleCar() {
  hasCar.value = !hasCar.value
}

function setToggleMethod(method) {
  toggleMethod.value = method
}

function toggleContent() {
  showContent.value = !showContent.value
}

function simulateAuth(state) {
  authState.value = state
}

function validateEmail() {
  const email = formData.value.email
  if (email.length === 0) {
    emailError.value = 'Email is required'
  } else if (!email.includes('@')) {
    emailError.value = 'Please enter a valid email'
  } else {
    emailError.value = ''
  }
}

function validatePassword() {
  const password = formData.value.password
  if (password.length === 0) {
    passwordError.value = 'Password is required'
  } else if (password.length < 6) {
    passwordError.value = 'Password must be at least 6 characters'
  } else if (password !== formData.value.confirmPassword) {
    passwordError.value = 'Passwords do not match'
  } else {
    passwordError.value = ''
  }
}

function simulateLoading() {
  isLoading.value = true
  hasError.value = false
  errorMessage.value = ''

  setTimeout(() => {
    if (Math.random() > 0.3) {
      isLoading.value = false
    } else {
      isLoading.value = false
      hasError.value = true
      errorMessage.value = 'Failed to load data. Please try again.'
    }
  }, 2000)
}

function retryLoading() {
  hasError.value = false
  simulateLoading()
}

function addPermission(permission) {
  if (!userPermissions.value.includes(permission)) {
    userPermissions.value.push(permission)
  }
}

function removePermission(permission) {
  const index = userPermissions.value.indexOf(permission)
  if (index > -1) {
    userPermissions.value.splice(index, 1)
  }
}

function changeRequiredPermission(permission) {
  requiredPermission.value = permission
}

function changeTab(tab) {
  activeTab.value = tab
}

// Computed properties for complex conditions
const grade = computed(() => {
  const s = score.value
  if (s >= 90) return 'A'
  if (s >= 80) return 'B'
  if (s >= 70) return 'C'
  if (s >= 60) return 'D'
  return 'F'
})

const weatherMessage = computed(() => {
  const w = weather.value
  switch (w) {
    case 'sunny':
      return "It's a beautiful sunny day! ☀️"
    case 'rainy':
      return "Don't forget your umbrella! 🌧️"
    case 'cloudy':
      return "It's a bit cloudy today ☁️"
    case 'snowy':
      return "Time to build a snowman! ❄️"
    default:
      return "Check the weather forecast!"
  }
})

const weatherColor = computed(() => {
  const colors = {
    sunny: '#FFD700',
    rainy: '#4A90E2',
    cloudy: '#B0B0B0',
    snowy: '#E0F7FA'
  }
  return colors[weather.value] || '#666'
})

const permissionGranted = computed(() => {
  return userPermissions.value.includes(requiredPermission.value)
})

const passwordStrength = computed(() => {
  const password = formData.value.password
  if (password.length === 0) return { text: 'Enter password', color: '#999' }
  if (password.length < 6) return { text: 'Weak', color: '#e74c3c' }
  if (password.length < 10) return { text: 'Medium', color: '#f39c12' }
  return { text: 'Strong', color: '#27ae60' }
})
</script>

<template>
  <div class="conditional-rendering-demo">
    <h1>🎭 Vue.js Conditional Rendering Showcase</h1>

    <!-- 1. Basic v-if and v-else -->
    <section class="demo-section">
      <h2>1️⃣ Basic v-if and v-else</h2>
      <div class="demo-content">
        <div class="toggle-demo">
          <div v-if="isVisible" class="content-box visible">
            <h3>✅ Content is Visible</h3>
            <p>This content is rendered because isVisible is true.</p>
          </div>

          <div v-else class="content-box hidden">
            <h3>❌ Content is Hidden</h3>
            <p>This content is not rendered because isVisible is false.</p>
          </div>

          <button @click="toggleVisibility">
            {{ isVisible ? 'Hide Content' : 'Show Content' }}
          </button>
        </div>

        <div class="auth-demo">
          <div v-if="isLoggedIn" class="auth-box logged-in">
            <h3>Welcome back, {{ username }}! 👋</h3>
            <p>You have full access to the application.</p>
            <button @click="logout">Logout</button>
          </div>

          <div v-else class="auth-box logged-out">
            <h3>Please login to continue 🔐</h3>
            <p>You need to authenticate to access this content.</p>
            <button @click="login">Login</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 2. Multiple Conditions with v-else-if -->
    <section class="demo-section">
      <h2>2️⃣ Multiple Conditions with v-else-if</h2>
      <div class="demo-content">
        <div class="grade-demo">
          <h3>Grade Calculator</h3>
          <p>Current Score: <strong>{{ score }}</strong></p>

          <div class="grade-display">
            <div v-if="score >= 90" class="grade excellent">
              <h4>Grade: A - Excellent! 🌟</h4>
              <p>Outstanding performance!</p>
            </div>

            <div v-else-if="score >= 80" class="grade good">
              <h4>Grade: B - Good! 👍</h4>
              <p>Keep up the good work!</p>
            </div>

            <div v-else-if="score >= 70" class="grade average">
              <h4>Grade: C - Average 😐</h4>
              <p>Room for improvement!</p>
            </div>

            <div v-else-if="score >= 60" class="grade poor">
              <h4>Grade: D - Poor 😟</h4>
              <p>Need more practice!</p>
            </div>

            <div v-else class="grade failing">
              <h4>Grade: F - Failing 😢</h4>
              <p>Please seek additional help!</p>
            </div>
          </div>

          <div class="score-controls">
            <button @click="changeScore(95)">Set to 95</button>
            <button @click="changeScore(85)">Set to 85</button>
            <button @click="changeScore(75)">Set to 75</button>
            <button @click="changeScore(65)">Set to 65</button>
            <button @click="changeScore(55)">Set to 55</button>
          </div>
        </div>

        <div class="role-demo">
          <h3>User Role Management</h3>
          <p>Current Role: <strong>{{ userRole }}</strong></p>

          <div v-if="userRole === 'admin'" class="role-admin">
            <h4>👑 Admin Panel</h4>
            <p>You have full administrative access.</p>
            <ul>
              <li>Manage users</li>
              <li>System settings</li>
              <li>View analytics</li>
            </ul>
          </div>

          <div v-else-if="userRole === 'moderator'" class="role-moderator">
            <h4>🛡️ Moderator Panel</h4>
            <p>You can moderate content and users.</p>
            <ul>
              <li>Review content</li>
              <li>Manage reports</li>
              <li>Temporarily ban users</li>
            </ul>
          </div>

          <div v-else-if="userRole === 'user'" class="role-user">
            <h4>👤 User Dashboard</h4>
            <p>Welcome to your personal dashboard.</p>
            <ul>
              <li>View profile</li>
              <li>Update settings</li>
              <li>View activity</li>
            </ul>
          </div>

          <div v-else class="role-guest">
            <h4>👋 Guest Access</h4>
            <p>Limited access for unauthenticated users.</p>
            <ul>
              <li>View public content</li>
              <li>Read announcements</li>
              <li>Browse forums</li>
            </ul>
          </div>

          <div class="role-controls">
            <button @click="changeRole('admin')">Admin</button>
            <button @click="changeRole('moderator')">Moderator</button>
            <button @click="changeRole('user')">User</button>
            <button @click="changeRole('guest')">Guest</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. Complex Conditions -->
    <section class="demo-section">
      <h2>3️⃣ Complex Conditions</h2>
      <div class="demo-content">
        <div class="driving-demo">
          <h3>Driving Eligibility Check</h3>
          <p>Age: <strong>{{ age }}</strong></p>
          <p>Has License: <strong>{{ hasLicense ? 'Yes' : 'No' }}</strong></p>
          <p>Has Car: <strong>{{ hasCar ? 'Yes' : 'No' }}</strong></p>

          <div v-if="canDrive && hasCar" class="status can-drive">
            <h4>🚗 You can drive!</h4>
            <p>You have everything needed to drive.</p>
          </div>

          <div v-else-if="canDrive && !hasCar" class="status can-drive-no-car">
            <h4>🚗 You can drive (but need a car)</h4>
            <p>You have a license but no vehicle.</p>
          </div>

          <div v-else-if="!canDrive && age >= 18" class="status no-license">
            <h4>📋 Get your license first</h4>
            <p>You're old enough but need a license.</p>
          </div>

          <div v-else class="status too-young">
            <h4>⏰ Too young to drive</h4>
            <p>You must be at least 18 years old.</p>
          </div>

          <div class="driving-controls">
            <button @click="ageUser">Age +1</button>
            <button @click="toggleLicense">
              {{ hasLicense ? 'Remove License' : 'Add License' }}
            </button>
            <button @click="toggleCar">
              {{ hasCar ? 'Remove Car' : 'Add Car' }}
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- 4. v-show vs v-if Comparison -->
    <section class="demo-section">
      <h2>4️⃣ v-show vs v-if Comparison</h2>
      <div class="demo-content">
        <div class="toggle-method-demo">
          <h3>Toggle Method: {{ toggleMethod }}</h3>

          <div class="method-selector">
            <button
              @click="setToggleMethod('v-if')"
              :class="{ active: toggleMethod === 'v-if' }"
            >
              Use v-if
            </button>
            <button
              @click="setToggleMethod('v-show')"
              :class="{ active: toggleMethod === 'v-show' }"
            >
              Use v-show
            </button>
          </div>

          <div class="comparison-box">
            <h4>Content rendered with {{ toggleMethod }}:</h4>

            <div v-if="toggleMethod === 'v-if' && showContent" class="content-method">
              <p>📦 This content is controlled by <strong>v-if</strong></p>
              <p>It gets completely added/removed from the DOM.</p>
              <small>Check browser dev tools to see DOM changes!</small>
            </div>

            <div v-show="toggleMethod === 'v-show' && showContent" class="content-method">
              <p>👁️ This content is controlled by <strong>v-show</strong></p>
              <p>It stays in DOM with display: none when hidden.</p>
              <small>Check browser dev tools - element always exists!</small>
            </div>

            <div v-if="!showContent" class="hidden-message">
              <p>Content is currently hidden using {{ toggleMethod }}</p>
            </div>
          </div>

          <button @click="toggleContent">
            {{ showContent ? 'Hide Content' : 'Show Content' }}
          </button>

          <div class="performance-info">
            <h4>Performance Tips:</h4>
            <ul>
              <li><strong>v-if:</strong> Better for content that rarely changes</li>
              <li><strong>v-show:</strong> Better for frequent toggling</li>
              <li><strong>v-if:</strong> Lower memory usage when hidden</li>
              <li><strong>v-show:</strong> Faster toggle performance</li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. Authentication States -->
    <section class="demo-section">
      <h2>5️⃣ Authentication States</h2>
      <div class="demo-content">
        <div class="auth-states-demo">
          <h3>Authentication Flow</h3>

          <div v-if="authState === 'loading'" class="auth-state loading">
            <div class="spinner">⏳</div>
            <h4>Loading...</h4>
            <p>Please wait while we authenticate you.</p>
          </div>

          <div v-else-if="authState === 'authenticated'" class="auth-state authenticated">
            <div class="success-icon">✅</div>
            <h4>Authentication Successful!</h4>
            <p>Welcome back, {{ username }}!</p>
            <button @click="simulateAuth('unauthenticated')">Sign Out</button>
          </div>

          <div v-else-if="authState === 'unauthenticated'" class="auth-state unauthenticated">
            <div class="lock-icon">🔒</div>
            <h4>Please Sign In</h4>
            <p>You need to authenticate to access this content.</p>
            <div class="auth-form">
              <input type="text" placeholder="Username" />
              <input type="password" placeholder="Password" />
              <button @click="simulateAuth('loading')">Sign In</button>
            </div>
          </div>

          <div v-else-if="authState === 'error'" class="auth-state error">
            <div class="error-icon">❌</div>
            <h4>Authentication Failed</h4>
            <p>Invalid credentials. Please try again.</p>
            <button @click="simulateAuth('unauthenticated')">Try Again</button>
          </div>

          <div class="state-controls">
            <button @click="simulateAuth('loading')">Show Loading</button>
            <button @click="simulateAuth('authenticated')">Show Authenticated</button>
            <button @click="simulateAuth('unauthenticated')">Show Unauthenticated</button>
            <button @click="simulateAuth('error')">Show Error</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Product Availability -->
    <section class="demo-section">
      <h2>6️⃣ Product Availability</h2>
      <div class="demo-content">
        <div class="products-demo">
          <h3>Product Status</h3>

          <div
            v-for="product in products"
            :key="product.id"
            class="product-card"
          >
            <h4>{{ product.name }}</h4>
            <p class="price">{{ new Intl.NumberFormat('id-ID', {
              style: 'currency',
              currency: 'IDR'
            }).format(product.price) }}</p>

            <div v-if="product.inStock && product.onSale" class="status sale">
              <span class="badge sale-badge">🔥 ON SALE</span>
              <span class="badge in-stock-badge">✅ In Stock</span>
            </div>

            <div v-else-if="product.inStock && !product.onSale" class="status normal">
              <span class="badge in-stock-badge">✅ In Stock</span>
            </div>

            <div v-else-if="!product.inStock && product.onSale" class="status sale-out">
              <span class="badge sale-badge">🔥 ON SALE</span>
              <span class="badge out-stock-badge">❌ Out of Stock</span>
            </div>

            <div v-else class="status out-of-stock">
              <span class="badge out-stock-badge">❌ Out of Stock</span>
            </div>

            <button
              v-if="product.inStock"
              :disabled="!product.inStock"
              class="add-to-cart"
            >
              Add to Cart
            </button>

            <button
              v-else
              disabled
              class="notify-me"
            >
              Notify Me When Available
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- 7. Form Validation -->
    <section class="demo-section">
      <h2>7️⃣ Form Validation with Conditional Rendering</h2>
      <div class="demo-content">
        <div class="form-validation-demo">
          <h3>Real-time Form Validation</h3>

          <form @submit.prevent class="validation-form">
            <div class="form-group">
              <label>Email Address:</label>
              <input
                v-model="formData.email"
                type="email"
                placeholder="Enter your email"
                @input="validateEmail"
                :class="{ error: emailError }"
              />
              <div v-if="emailError" class="error-message">
                ⚠️ {{ emailError }}
              </div>
              <div v-else-if="formData.email && !emailError" class="success-message">
                ✅ Email is valid
              </div>
            </div>

            <div class="form-group">
              <label>Password:</label>
              <input
                v-model="formData.password"
                type="password"
                placeholder="Enter your password"
                @input="validatePassword"
                :class="{ error: passwordError }"
              />
              <div v-if="passwordError" class="error-message">
                ⚠️ {{ passwordError }}
              </div>
            </div>

            <div class="form-group">
              <label>Confirm Password:</label>
              <input
                v-model="formData.confirmPassword"
                type="password"
                placeholder="Confirm your password"
                @input="validatePassword"
              />
              <div v-if="formData.confirmPassword && formData.password !== formData.confirmPassword" class="error-message">
                ⚠️ Passwords do not match
              </div>
              <div v-else-if="formData.confirmPassword && formData.password === formData.confirmPassword && formData.password.length >= 6" class="success-message">
                ✅ Passwords match
              </div>
            </div>

            <div class="form-group">
              <label>Password Strength:</label>
              <div class="password-strength">
                <div
                  class="strength-bar"
                  :style="{ backgroundColor: passwordStrength.color }"
                ></div>
                <span :style="{ color: passwordStrength.color }">
                  {{ passwordStrength.text }}
                </span>
              </div>
            </div>

            <div class="form-submit">
              <button
                type="submit"
                :disabled="!isFormValid"
                :class="{ disabled: !isFormValid }"
              >
                {{ isFormValid ? 'Submit Form ✅' : 'Please complete the form ❌' }}
              </button>
            </div>

            <div v-if="isFormValid" class="form-ready">
              <h4>🎉 Form is ready to submit!</h4>
              <p>All required fields are filled correctly.</p>
            </div>
          </form>
        </div>
      </div>
    </section>

    <!-- 8. Loading and Error States -->
    <section class="demo-section">
      <h2>8️⃣ Loading and Error States</h2>
      <div class="demo-content">
        <div class="loading-states-demo">
          <h3>Data Loading States</h3>

          <div v-if="isLoading" class="loading-state">
            <div class="loading-spinner">
              <div class="spinner"></div>
            </div>
            <p>Loading data, please wait...</p>
          </div>

          <div v-else-if="hasError" class="error-state">
            <div class="error-icon">⚠️</div>
            <h4>Oops! Something went wrong</h4>
            <p>{{ errorMessage }}</p>
            <button @click="retryLoading">Try Again</button>
          </div>

          <div v-else class="success-state">
            <div class="success-icon">✅</div>
            <h4>Data loaded successfully!</h4>
            <p>All data has been loaded without any issues.</p>
          </div>

          <div class="loading-controls">
            <button @click="simulateLoading" :disabled="isLoading">
              {{ isLoading ? 'Loading...' : 'Simulate Data Loading' }}
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- 9. Permission-Based Content -->
    <section class="demo-section">
      <h2>9️⃣ Permission-Based Content</h2>
      <div class="demo-content">
        <div class="permissions-demo">
          <h3>Access Control</h3>

          <div class="current-permissions">
            <h4>Current Permissions:</h4>
            <div class="permission-list">
              <span
                v-for="permission in userPermissions"
                :key="permission"
                class="permission-tag"
              >
                {{ permission }}
                <button @click="removePermission(permission)" class="remove-permission">×</button>
              </span>
            </div>

            <div class="add-permission">
              <input
                v-model="newPermission"
                placeholder="Add permission"
                @keyup.enter="addPermission(newPermission)"
              />
              <button @click="addPermission(newPermission)">Add</button>
            </div>
          </div>

          <div class="required-permission">
            <h4>Required Permission: <strong>{{ requiredPermission }}</strong></h4>
            <div class="permission-selector">
              <button
                v-for="perm in ['admin', 'read', 'write', 'delete', 'moderate']"
                :key="perm"
                @click="changeRequiredPermission(perm)"
                :class="{ active: requiredPermission === perm }"
              >
                {{ perm }}
              </button>
            </div>
          </div>

          <div v-if="permissionGranted" class="access-granted">
            <div class="access-icon">🔓</div>
            <h4>Access Granted!</h4>
            <p>You have permission to view this content.</p>
            <div class="protected-content">
              <h5>Protected Content</h5>
              <p>This content is only visible to users with the "{{ requiredPermission }}" permission.</p>
              <ul>
                <li>Confidential information</li>
                <li>Admin controls</li>
                <li>Restricted features</li>
              </ul>
            </div>
          </div>

          <div v-else class="access-denied">
            <div class="access-icon">🔒</div>
            <h4>Access Denied!</h4>
            <p>You need the "{{ requiredPermission }}" permission to view this content.</p>
            <div class="missing-permission">
              <p>Missing permission: <strong>{{ requiredPermission }}</strong></p>
              <p>Please contact an administrator to gain access.</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 10. Dynamic Component Loading -->
    <section class="demo-section">
      <h2>🔟 Dynamic Component Loading</h2>
      <div class="demo-content">
        <div class="tabs-demo">
          <h3>Dynamic Tab Content</h3>

          <div class="tab-navigation">
            <button
              v-for="tab in tabs"
              :key="tab"
              @click="changeTab(tab)"
              :class="{ active: activeTab === tab }"
            >
              {{ tab.charAt(0).toUpperCase() + tab.slice(1) }}
            </button>
          </div>

          <div class="tab-content">
            <!-- Profile Tab -->
            <div v-if="activeTab === 'profile'" class="tab-panel">
              <h4>👤 Profile Settings</h4>
              <div class="form-group">
                <label>Display Name:</label>
                <input v-model="username" />
              </div>
              <div class="form-group">
                <label>Bio:</label>
                <textarea placeholder="Tell us about yourself..."></textarea>
              </div>
              <button>Save Profile</button>
            </div>

            <!-- Settings Tab -->
            <div v-else-if="activeTab === 'settings'" class="tab-panel">
              <h4>⚙️ Application Settings</h4>
              <div class="setting-group">
                <label>
                  <input type="checkbox" />
                  Enable email notifications
                </label>
              </div>
              <div class="setting-group">
                <label>
                  <input type="checkbox" />
                  Enable dark mode
                </label>
              </div>
              <div class="setting-group">
                <label>Language:</label>
                <select>
                  <option>English</option>
                  <option>Spanish</option>
                  <option>French</option>
                </select>
              </div>
              <button>Save Settings</button>
            </div>

            <!-- Notifications Tab -->
            <div v-else-if="activeTab === 'notifications'" class="tab-panel">
              <h4>🔔 Notification Preferences</h4>
              <div class="notification-item">
                <label>
                  <input type="checkbox" checked />
                  Email notifications
                </label>
                <p>Receive updates via email</p>
              </div>
              <div class="notification-item">
                <label>
                  <input type="checkbox" />
                  Push notifications
                </label>
                <p>Get instant alerts in your browser</p>
              </div>
              <div class="notification-item">
                <label>
                  <input type="checkbox" checked />
                  SMS notifications
                </label>
                <p>Receive text messages for important updates</p>
              </div>
              <button>Save Preferences</button>
            </div>

            <!-- Security Tab -->
            <div v-else-if="activeTab === 'security'" class="tab-panel">
              <h4>🔐 Security Settings</h4>
              <div class="security-item">
                <label>Current Password:</label>
                <input type="password" />
              </div>
              <div class="security-item">
                <label>New Password:</label>
                <input type="password" />
              </div>
              <div class="security-item">
                <label>Confirm New Password:</label>
                <input type="password" />
              </div>
              <div class="security-item">
                <label>
                  <input type="checkbox" />
                  Enable two-factor authentication
                </label>
              </div>
              <button>Update Security</button>
            </div>
          </div>

          <div class="tab-info">
            <p>Currently viewing: <strong>{{ activeTab }}</strong> tab</p>
            <p>Only one tab panel is rendered at a time for optimal performance.</p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// Additional reactive data for template
const newPermission = ref('')
</script>

<style scoped>
.conditional-rendering-demo {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #42b883;
  margin-bottom: 30px;
  font-size: 2.5em;
}

.demo-section {
  background: #ffffff;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border-left: 4px solid #42b883;
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.demo-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Toggle Demo */
.toggle-demo, .auth-demo {
  text-align: center;
}

.content-box {
  padding: 20px;
  margin: 15px 0;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.content-box.visible {
  background-color: #d4edda;
  border: 2px solid #c3e6cb;
  color: #155724;
}

.content-box.hidden {
  background-color: #f8d7da;
  border: 2px solid #f5c6cb;
  color: #721c24;
}

.auth-box {
  padding: 20px;
  margin: 15px 0;
  border-radius: 8px;
}

.auth-box.logged-in {
  background-color: #e3f2fd;
  border: 2px solid #bbdefb;
  color: #1565c0;
}

.auth-box.logged-out {
  background-color: #fff3cd;
  border: 2px solid #ffeaa7;
  color: #856404;
}

/* Grade Demo */
.grade-demo {
  text-align: center;
}

.grade-display {
  margin: 20px 0;
}

.grade {
  padding: 20px;
  border-radius: 8px;
  margin: 10px 0;
}

.grade.excellent {
  background-color: #d4edda;
  border: 2px solid #c3e6cb;
  color: #155724;
}

.grade.good {
  background-color: #cce5ff;
  border: 2px solid #99d6ff;
  color: #004085;
}

.grade.average {
  background-color: #fff3cd;
  border: 2px solid #ffeaa7;
  color: #856404;
}

.grade.poor {
  background-color: #f8d7da;
  border: 2px solid #f5c6cb;
  color: #721c24;
}

.grade.failing {
  background-color: #f5c6cb;
  border: 2px solid #ec8b8b;
  color: #491217;
}

.score-controls, .role-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

/* Role Demo */
.role-demo {
  text-align: center;
}

.role-admin, .role-moderator, .role-user, .role-guest {
  padding: 20px;
  border-radius: 8px;
  margin: 15px 0;
}

.role-admin {
  background-color: #f8d7da;
  border: 2px solid #ec8b8b;
  color: #721c24;
}

.role-moderator {
  background-color: #d1ecf1;
  border: 2px solid #bee5eb;
  color: #0c5460;
}

.role-user {
  background-color: #e2e3e5;
  border: 2px solid #ced4da;
  color: #383d41;
}

.role-guest {
  background-color: #f8f9fa;
  border: 2px solid #e9ecef;
  color: #495057;
}

.role-admin ul, .role-moderator ul, .role-user ul, .role-guest ul {
  text-align: left;
  margin: 10px 0;
  padding-left: 20px;
}

/* Driving Demo */
.driving-demo {
  text-align: center;
}

.status {
  padding: 20px;
  border-radius: 8px;
  margin: 15px 0;
}

.status.can-drive {
  background-color: #d4edda;
  border: 2px solid #c3e6cb;
  color: #155724;
}

.status.can-drive-no-car {
  background-color: #cce5ff;
  border: 2px solid #99d6ff;
  color: #004085;
}

.status.no-license {
  background-color: #fff3cd;
  border: 2px solid #ffeaa7;
  color: #856404;
}

.status.too-young {
  background-color: #f8d7da;
  border: 2px solid #f5c6cb;
  color: #721c24;
}

.driving-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

/* Toggle Method Demo */
.toggle-method-demo {
  text-align: center;
}

.method-selector {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin: 15px 0;
}

.method-selector button.active {
  background-color: #007bff;
  color: white;
}

.comparison-box {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin: 15px 0;
  text-align: left;
}

.content-method {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin: 10px 0;
  border-left: 4px solid #2196f3;
}

.hidden-message {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ffc107;
}

.performance-info {
  background-color: #e8f5e8;
  padding: 15px;
  border-radius: 8px;
  text-align: left;
  margin-top: 15px;
}

.performance-info h4 {
  margin: 0 0 10px 0;
  color: #2e7d32;
}

.performance-info ul {
  margin: 0;
  padding-left: 20px;
}

/* Auth States Demo */
.auth-states-demo {
  text-align: center;
}

.auth-state {
  padding: 30px;
  border-radius: 8px;
  margin: 15px 0;
}

.auth-state.loading {
  background-color: #e3f2fd;
  border: 2px solid #bbdefb;
  color: #1565c0;
}

.auth-state.authenticated {
  background-color: #d4edda;
  border: 2px solid #c3e6cb;
  color: #155724;
}

.auth-state.unauthenticated {
  background-color: #fff3cd;
  border: 2px solid #ffeaa7;
  color: #856404;
}

.auth-state.error {
  background-color: #f8d7da;
  border: 2px solid #f5c6cb;
  color: #721c24;
}

.spinner {
  font-size: 2em;
  margin-bottom: 10px;
}

.auth-form {
  margin: 15px 0;
}

.auth-form input {
  display: block;
  width: 200px;
  padding: 8px;
  margin: 5px auto;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.state-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

/* Products Demo */
.products-demo {
  text-align: center;
}

.products-demo h3 {
  margin-bottom: 20px;
}

.product-card {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin: 15px auto;
  max-width: 300px;
  border: 1px solid #dee2e6;
}

.product-card h4 {
  margin: 0 0 10px 0;
  color: #2c3e50;
}

.price {
  font-size: 1.2em;
  font-weight: bold;
  color: #42b883;
  margin: 10px 0;
}

.status {
  margin: 15px 0;
}

.badge {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 0.8em;
  font-weight: bold;
  margin: 2px;
}

.sale-badge {
  background-color: #dc3545;
  color: white;
}

.in-stock-badge {
  background-color: #28a745;
  color: white;
}

.out-stock-badge {
  background-color: #6c757d;
  color: white;
}

.add-to-cart {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-top: 10px;
}

.notify-me {
  background-color: #6c757d;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: not-allowed;
  margin-top: 10px;
}

/* Form Validation Demo */
.form-validation-demo {
  max-width: 500px;
  margin: 0 auto;
}

.validation-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.form-group {
  margin-bottom: 15px;
  text-align: left;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.form-group input.error {
  border-color: #dc3545;
}

.error-message {
  color: #dc3545;
  font-size: 12px;
  margin-top: 5px;
}

.success-message {
  color: #28a745;
  font-size: 12px;
  margin-top: 5px;
}

.password-strength {
  display: flex;
  align-items: center;
  gap: 10px;
}

.strength-bar {
  width: 100px;
  height: 10px;
  border-radius: 5px;
  transition: all 0.3s ease;
}

.form-submit {
  text-align: center;
  margin-top: 20px;
}

.form-submit button {
  padding: 12px 24px;
  font-size: 16px;
}

.form-submit button.disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

.form-ready {
  background-color: #d4edda;
  padding: 15px;
  border-radius: 8px;
  margin-top: 15px;
  text-align: center;
  color: #155724;
}

/* Loading States Demo */
.loading-states-demo {
  text-align: center;
}

.loading-state, .error-state, .success-state {
  padding: 30px;
  border-radius: 8px;
  margin: 15px 0;
}

.loading-state {
  background-color: #e3f2fd;
  color: #1565c0;
}

.error-state {
  background-color: #f8d7da;
  color: #721c24;
}

.success-state {
  background-color: #d4edda;
  color: #155724;
}

.loading-spinner {
  margin-bottom: 15px;
}

.loading-spinner .spinner {
  display: inline-block;
  width: 40px;
  height: 40px;
  border: 4px solid #bbdefb;
  border-top: 4px solid #1565c0;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-icon, .success-icon {
  font-size: 2em;
  margin-bottom: 10px;
}

/* Permissions Demo */
.permissions-demo {
  text-align: center;
}

.current-permissions {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.permission-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin: 15px 0;
}

.permission-tag {
  background-color: #007bff;
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  display: flex;
  align-items: center;
  gap: 5px;
}

.remove-permission {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 14px;
  padding: 0;
  margin-left: 5px;
}

.add-permission {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 10px;
}

.add-permission input {
  padding: 5px 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.required-permission {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.permission-selector {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 10px;
  flex-wrap: wrap;
}

.permission-selector button.active {
  background-color: #28a745;
  color: white;
}

.access-granted, .access-denied {
  padding: 20px;
  border-radius: 8px;
  margin: 15px 0;
}

.access-granted {
  background-color: #d4edda;
  border: 2px solid #c3e6cb;
  color: #155724;
}

.access-denied {
  background-color: #f8d7da;
  border: 2px solid #f5c6cb;
  color: #721c24;
}

.access-icon {
  font-size: 2em;
  margin-bottom: 10px;
}

.protected-content {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  margin-top: 15px;
  text-align: left;
}

.protected-content h5 {
  margin: 0 0 10px 0;
  color: #155724;
}

.missing-permission {
  margin-top: 15px;
  font-style: italic;
}

/* Tabs Demo */
.tabs-demo {
  text-align: center;
}

.tab-navigation {
  display: flex;
  gap: 5px;
  justify-content: center;
  margin-bottom: 20px;
  border-bottom: 2px solid #dee2e6;
  padding-bottom: 10px;
}

.tab-navigation button {
  background: none;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all 0.3s ease;
}

.tab-navigation button.active {
  color: #42b883;
  border-bottom-color: #42b883;
}

.tab-content {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  text-align: left;
  min-height: 200px;
}

.tab-panel h4 {
  margin: 0 0 15px 0;
  color: #2c3e50;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.form-group input,
.form-group textarea,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.setting-group,
.notification-item,
.security-item {
  margin-bottom: 15px;
}

.setting-group label,
.notification-item label,
.security-item label {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 5px;
}

.setting-group p,
.notification-item p {
  margin: 5px 0 0 20px;
  font-size: 12px;
  color: #6c757d;
}

.tab-info {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin-top: 15px;
  text-align: center;
}

/* Global button styles */
button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 14px;
}

button:hover {
  background-color: #369870;
  transform: translateY(-2px);
}

button:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
  transform: none;
}

/* Responsive Design */
@media (max-width: 768px) {
  .conditional-rendering-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .score-controls,
  .role-controls,
  .driving-controls,
  .state-controls,
  .method-selector,
  .permission-selector,
  .tab-navigation {
    flex-direction: column;
  }

  .tab-navigation {
    border-bottom: none;
    padding-bottom: 0;
  }

  .grade-display,
  .array-results {
    grid-template-columns: 1fr;
  }
}
</style>

## ❓ Pertanyaan: Apa Fungsi Conditional Rendering?

Conditional Rendering memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk mengelola tampilan dinamis:

### 1. **Kontrol Visibility Berdasarkan State**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 1: Mengontrol visibility berdasarkan state aplikasi
const isLoggedIn = ref(false)
const isLoading = ref(false)
const hasError = ref(false)

const login = () => {
  isLoggedIn.value = true
}

const logout = () => {
  isLoggedIn.value = false
}
</script>

<template>
  <!-- Content berubah berdasarkan authentication state -->
  <div v-if="isLoggedIn" class="user-dashboard">
    <h1>Welcome back! 👋</h1>
    <button @click="logout">Logout</button>
  </div>

  <div v-else class="login-form">
    <h1>Please login 🔐</h1>
    <button @click="login">Login</button>
  </div>

  <!-- Loading state -->
  <div v-if="isLoading" class="loading">
    <p>Loading data...</p>
  </div>

  <!-- Error state -->
  <div v-if="hasError" class="error">
    <p>Something went wrong!</p>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Dynamic UI** - tampilan berubah sesuai state aplikasi
- ✅ **User feedback** - memberikan feedback visual yang jelas
- ✅ **State management** - menghubungkan UI dengan business logic

### 2. **Optimasi Performa dengan Conditional Rendering**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 2: Optimasi performa dengan conditional rendering
const activeTab = ref('home')
const showHeavyContent = ref(false)

const heavyComponent = ref(null)
</script>

<template>
  <!-- v-if untuk component yang jarang diubah - lebih efisien untuk memory -->
  <div v-if="showHeavyContent" class="heavy-component">
    <!-- Complex charts, large data tables, dll -->
    <HeavyChart />
    <LargeDataTable />
  </div>

  <!-- v-show untuk content yang sering toggle - lebih cepat untuk switching -->
  <div v-show="isActive" class="frequently-toggled">
    <!-- Simple content yang sering show/hide -->
    <p>Quick toggle content</p>
  </div>

  <!-- Tab content - hanya render tab yang aktif -->
  <div v-if="activeTab === 'home'">
    <HomeContent />
  </div>
  <div v-else-if="activeTab === 'profile'">
    <ProfileContent />
  </div>
  <div v-else-if="activeTab === 'settings'">
    <SettingsContent />
  </div>
</template>
```

**Keuntungan:**
- ✅ **Memory efficient** - hanya render yang diperlukan
- ✅ **Better performance** - mengurangi DOM complexity
- ✅ **Lazy loading** - load component saat dibutuhkan

### 3. **Form Validation & User Guidance**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 3: Real-time form validation dan user guidance
const formData = ref({
  email: '',
  password: '',
  confirmPassword: ''
})

const emailError = computed(() => {
  const email = formData.value.email
  if (!email) return 'Email is required'
  if (!email.includes('@')) return 'Invalid email format'
  return ''
})

const showPasswordHelper = computed(() => {
  return formData.value.password.length > 0 && formData.value.password.length < 6
})

const isFormValid = computed(() => {
  return !emailError.value &&
         formData.value.password.length >= 6 &&
         formData.value.password === formData.value.confirmPassword
})
</script>

<template>
  <form @submit.prevent>
    <div class="form-group">
      <label>Email:</label>
      <input v-model="formData.email" type="email" />

      <!-- Conditional error message -->
      <div v-if="emailError" class="error">
        ⚠️ {{ emailError }}
      </div>

      <!-- Conditional success message -->
      <div v-else-if="formData.value.email && !emailError" class="success">
        ✅ Email is valid
      </div>
    </div>

    <div class="form-group">
      <label>Password:</label>
      <input v-model="formData.password" type="password" />

      <!-- Show helper text when password is being typed -->
      <div v-if="showPasswordHelper" class="helper">
        💡 Password should be at least 6 characters
      </div>
    </div>

    <!-- Disable button when form is invalid -->
    <button type="submit" :disabled="!isFormValid">
      {{ isFormValid ? 'Submit Form ✅' : 'Complete the form ❌' }}
    </button>
  </form>
</template>
```

**Keuntungan:**
- ✅ **Real-time feedback** - user mendapat feedback langsung
- ✅ **Progressive disclosure** - bantuan muncul saat dibutuhkan
- ✅ **Error prevention** - prevent submit form yang invalid

### 4. **Responsive Design & Device Adaptation**
```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Fungsi 4: Responsive design dan device adaptation
const windowWidth = ref(window.innerWidth)
const isMobile = ref(false)
const isTablet = ref(false)
const isDesktop = ref(false)

const updateWindowSize = () => {
  windowWidth.value = window.innerWidth
  isMobile.value = windowWidth.value < 768
  isTablet.value = windowWidth.value >= 768 && windowWidth.value < 1024
  isDesktop.value = windowWidth.value >= 1024
}

onMounted(() => {
  updateWindowSize()
  window.addEventListener('resize', updateWindowSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateWindowSize)
})
</script>

<template>
  <!-- Mobile layout -->
  <div v-if="isMobile" class="mobile-layout">
    <header class="mobile-header">
      <h1>Mobile App</h1>
      <button>☰ Menu</button>
    </header>
    <nav class="mobile-nav">
      <button>🏠</button>
      <button>🔍</button>
      <button>👤</button>
    </nav>
    <main class="mobile-content">
      <!-- Mobile-optimized content -->
    </main>
  </div>

  <!-- Tablet layout -->
  <div v-else-if="isTablet" class="tablet-layout">
    <aside class="tablet-sidebar">
      <nav>
        <a href="#">Home</a>
        <a href="#">Profile</a>
        <a href="#">Settings</a>
      </nav>
    </aside>
    <main class="tablet-content">
      <!-- Tablet-optimized content -->
    </main>
  </div>

  <!-- Desktop layout -->
  <div v-else class="desktop-layout">
    <header class="desktop-header">
      <nav>
        <a href="#">Home</a>
        <a href="#">About</a>
        <a href="#">Services</a>
        <a href="#">Contact</a>
      </nav>
    </header>
    <main class="desktop-content">
      <!-- Desktop-optimized content with full features -->
    </main>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Device optimization** - tampilan optimal untuk setiap device
- ✅ **Better UX** - user experience yang sesuai device
- ✅ **Performance** - load content yang relevan saja

### 5. **Access Control & Permission-Based Content**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 5: Access control dan permission-based content
const userRole = ref('user')
const userPermissions = ref(['read'])
const isAuthenticated = ref(true)

const canEdit = computed(() => {
  return userPermissions.value.includes('edit') || userRole.value === 'admin'
})

const canDelete = computed(() => {
  return userPermissions.value.includes('delete') || userRole.value === 'admin'
})

const canViewAdminPanel = computed(() => {
  return userRole.value === 'admin' || userPermissions.value.includes('admin_access')
})
</script>

<template>
  <!-- Public content - semua user bisa lihat -->
  <div class="public-content">
    <h1>Welcome to Our App!</h1>
    <p>This content is visible to everyone.</p>
  </div>

  <!-- Authenticated user content -->
  <div v-if="isAuthenticated" class="user-content">
    <h2>User Dashboard</h2>
    <p>Welcome back! Here's your personal content.</p>
  </div>

  <!-- Editor permissions -->
  <div v-if="canEdit" class="editor-tools">
    <button>✏️ Edit Content</button>
    <button>📝 Create New</button>
  </div>

  <!-- Admin permissions -->
  <div v-if="canViewAdminPanel" class="admin-panel">
    <h2>Admin Panel</h2>
    <button>👥 Manage Users</button>
    <button>⚙️ System Settings</button>
    <button>📊 View Analytics</button>
  </div>

  <!-- Super admin only -->
  <div v-if="userRole === 'admin'" class="super-admin">
    <button>🔒 System Security</button>
    <button>💾 Database Management</button>
  </div>

  <!-- Guest users - limited access -->
  <div v-if="!isAuthenticated" class="guest-cta">
    <p>Please login to access more features!</p>
    <button>Login</button>
    <button>Sign Up</button>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Security** - hanya user authorized yang bisa lihat content
- ✅ **Role-based UI** - tampilan sesuai role user
- ✅ **Progressive access** - reveal features sesuai permissions

## 🎯 Best Practices untuk Conditional Rendering

### 1. **Pilih v-if vs v-show dengan Bijak**
```vue
<script setup>
const isVisible = ref(true)
const isToggleFrequent = ref(false)
</script>

<template>
  <!-- ✅ Gunakan v-if untuk content yang jarang berubah -->
  <div v-if="isVisible">
    <HeavyComponent />
  </div>

  <!-- ✅ Gunakan v-show untuk content yang sering toggle -->
  <div v-show="isToggleFrequent">
    <SimpleMessage />
  </div>

  <!-- ❌ Hindari v-if untuk frequent toggling -->
  <div v-if="isToggleFrequent">
    <!-- Ini akan cause performance issues -->
  </div>
</template>
```

### 2. **Gunakan Template Wrapper untuk Multiple Elements**
```vue
<template>
  <!-- ✅ GOOD: Gunakan <template> untuk multiple elements -->
  <template v-if="isLoggedIn">
    <header>Welcome back!</header>
    <main>Your dashboard content</main>
    <footer>© 2024 App</footer>
  </template>

  <!-- ❌ AVOID: v-if pada parent element -->
  <div v-if="isLoggedIn">
    <header>Welcome back!</header>
    <main>Your dashboard content</main>
    <footer>© 2024 App</footer>
  </div>
</template>
```

### 3. **Hindari v-if dan v-for pada Element yang Sama**
```vue
<template>
  <!-- ❌ AVOID: v-if dan v-for pada element yang sama -->
  <div v-if="shouldShowItems" v-for="item in items" :key="item.id">
    {{ item.name }}
  </div>

  <!-- ✅ GOOD: Pisahkan dengan template wrapper -->
  <template v-if="shouldShowItems">
    <div v-for="item in items" :key="item.id">
      {{ item.name }}
    </div>
  </template>

  <!-- ✅ ATAU gunakan computed properties -->
  <div v-for="item in visibleItems" :key="item.id">
    {{ item.name }}
  </div>
</template>
```

## 🎉 Kesimpulan

Conditional Rendering adalah fitur **sangat powerful** dalam Vue.js yang memberikan:

- ✅ **Dynamic UI** - tampilan yang berubah sesuai kondisi
- ✅ **Performance optimization** - kontrol yang tepat untuk rendering
- ✅ **User experience** - feedback dan guidance yang real-time
- ✅ **Responsive design** - adaptasi untuk berbagai devices
- ✅ **Security control** - access control berbasis permissions

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Conditional Rendering
- Vue.js Template Syntax Guide
- Vue.js Performance Best Practices

---

**🔥 Tips Pro:**
- Gunakan `v-if` untuk content yang **jarang berubah** dan expensive to render
- Gunakan `v-show` untuk content yang **sering toggle** dan lightweight
- Manfaatkan `computed properties` untuk complex conditions
- Gunakan `<template>` wrapper untuk multiple elements dengan condition yang sama
- Pertimbangkan a11y (accessibility) saat menyembunyikan content

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Style Guide