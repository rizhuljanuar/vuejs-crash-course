# 💉 Provide & Inject - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja provide/inject untuk dependency injection dan sharing data antar komponen dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Provide & Inject?](#-apa-itu-provide--inject)
- [🎮 Cara Menggunakan Provide & Inject](#-cara-menggunakan-provide--inject)
- [📡 Provide Data](#-provide-data)
- [💉 Inject Data](#-inject-data)
- [🔄 Problem Solving](#-problem-solving)
- [🎯 Advanced Patterns](#-advanced-patterns)
- [🔧 Reactive Providers](#-reactive-providers)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Provide & Inject?

**Provide & Inject** adalah fitur dependency injection dalam Vue.js yang memungkinkan ancestor component untuk "provide" data ke semua descendant components, terlepas dari seberapa dalam component hierarchy-nya.

### Konsep Dasar Provide & Inject

```
Ancestor Component (provide)
    ↓ (data tersedia untuk semua descendants)
Child Component
    ↓ (bisa inject data)
Grandchild Component
    ↓ (masih bisa inject data)
Deep Nested Component
```

### Mengapa Provide & Inject Penting?

1. **🔄 Prop Drilling Prevention:** Menghindari props passing melalui banyak层级
2. **📦 Global State Management:** Berbagi data global tanpa library eksternal
3. **🎯 Service Injection:** Menginjeksi services atau utilities
4. **🛡️ Decoupling:** Mengurangi ketergantungan antar komponen

### Masalah yang Diselesaikan

**❌ Tanpa Provide & Inject (Prop Drilling):**

```vue
<!-- App.vue -->
<template>
  <Layout :theme="theme" :user="user">
    <Header :theme="theme" :user="user">
      <Navbar :theme="theme" :user="user">
        <UserMenu :theme="theme" :user="user" />
      </Navbar>
    </Header>
  </Layout>
</template>
```

**✅ Dengan Provide & Inject:**

```vue
<!-- App.vue -->
<script setup>
import { provide } from 'vue'

provide('theme', 'dark')
provide('user', { name: 'John', role: 'admin' })
</script>

<template>
  <Layout>
    <Header>
      <Navbar>
        <UserMenu /> <!-- Bisa inject theme dan user langsung -->
      </Navbar>
    </Header>
  </Layout>
</template>
```

## 🎮 Cara Menggunakan Provide & Inject

### 1. Basic Provide & Inject

**Provider (Ancestor Component):**

```vue
<script setup>
import { provide } from 'vue'

// Provide data dasar
provide('message', 'Hello from App!')
provide('count', 0)
provide('config', { theme: 'dark', lang: 'id' })
</script>

<template>
  <div>
    <ChildComponent />
  </div>
</template>
```

**Injector (Descendant Component):**

```vue
<script setup>
import { inject } from 'vue'

// Inject data dari ancestor
const message = inject('message')
const count = inject('count')
const config = inject('config')
</script>

<template>
  <div>
    <p>{{ message }}</p>
    <p>Count: {{ count }}</p>
    <p>Theme: {{ config.theme }}</p>
  </div>
</template>
```

### 2. Provide dengan Default Values

```vue
<script setup>
import { inject } from 'vue'

// Inject dengan default value
const theme = inject('theme', 'light')
const user = inject('user', { name: 'Guest', role: 'visitor' })
const settings = inject('settings', {
  notifications: true,
  autoSave: false
})
</script>

<template>
  <div>
    <p>Theme: {{ theme }}</p>
    <p>User: {{ user.name }} ({{ user.role }})</p>
    <p>Notifications: {{ settings.notifications ? 'Enabled' : 'Disabled' }}</p>
  </div>
</template>
```

### 3. Multiple Providers

```vue
<!-- App.vue -->
<script setup>
import { provide } from 'vue'

// Provider untuk auth
provide('auth', {
  user: { id: 1, name: 'John Doe' },
  token: 'abc123',
  login: () => console.log('Login'),
  logout: () => console.log('Logout')
})

// Provider untuk theme
provide('theme', {
  current: 'dark',
  colors: {
    primary: '#3b82f6',
    secondary: '#10b981'
  },
  setTheme: (theme) => console.log('Set theme:', theme)
})

// Provider untuk config
provide('appConfig', {
  apiURL: 'https://api.example.com',
  version: '1.0.0',
  debug: true
})
</script>
```

## 📡 Provide Data

### 1. Provide dengan Static Data

```vue
<script setup>
import { provide } from 'vue'

// Static values
provide('appName', 'My Awesome App')
provide('version', '1.0.0')
provide('author', 'Vue Developer')

// Static arrays
provide('friends', ['Alex', 'Jordan', 'HuXn', 'John'])

// Static objects
provide('gameInfo', {
  id: 1,
  title: 'Epic Adventure Game',
  genre: ['Action', 'Adventure', 'RPG'],
  platform: ['PC', 'PlayStation', 'Xbox'],
  releaseDate: '2022-03-15'
})
</script>
```

### 2. Provide dengan Reactive Data

```vue
<script setup>
import { ref, reactive, provide } from 'vue'

// Reactive primitive
const count = ref(0)
const message = ref('Hello World')

// Reactive object
const user = reactive({
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  role: 'admin'
})

// Reactive array
const notifications = ref([])

// Provide reactive data
provide('count', count)
provide('message', message)
provide('user', user)
provide('notifications', notifications)

// Methods untuk update data
provide('updateUser', (newData) => {
  Object.assign(user, newData)
})

provide('addNotification', (notification) => {
  notifications.value.push(notification)
})
</script>
```

### 3. Provide dengan Functions

```vue
<script setup>
import { provide } from 'vue'

// Service functions
provide('apiService', {
  getUsers: async () => {
    const response = await fetch('/api/users')
    return response.json()
  },
  createUser: async (userData) => {
    const response = await fetch('/api/users', {
      method: 'POST',
      body: JSON.stringify(userData)
    })
    return response.json()
  }
})

// Utility functions
provide('utils', {
  formatDate: (date) => new Date(date).toLocaleDateString(),
  capitalize: (str) => str.charAt(0).toUpperCase() + str.slice(1),
  debounce: (func, delay) => {
    let timeoutId
    return (...args) => {
      clearTimeout(timeoutId)
      timeoutId = setTimeout(() => func.apply(this, args), delay)
    }
  }
})

// Event handlers
provide('events', {
  showNotification: (message, type = 'info') => {
    console.log(`${type}: ${message}`)
  },
  showError: (error) => {
    console.error('Error:', error)
  }
})
</script>
```

## 💉 Inject Data

### 1. Basic Injection

```vue
<script setup>
import { inject } from 'vue'

// Inject data tunggal
const appName = inject('appName')
const version = inject('version')

// Inject array
const friends = inject('friends')

// Inject object
const gameInfo = inject('gameInfo')
</script>

<template>
  <div>
    <h2>{{ appName }} v{{ version }}</h2>

    <h3>Friends List:</h3>
    <ul>
      <li v-for="friend in friends" :key="friend">{{ friend }}</li>
    </ul>

    <h3>Game Info:</h3>
    <p>Title: {{ gameInfo.title }}</p>
    <p>Genre: {{ gameInfo.genre.join(', ') }}</p>
    <p>Platforms: {{ gameInfo.platform.join(', ') }}</p>
  </div>
</template>
```

### 2. Inject dengan Default Values

```vue
<script setup>
import { inject } from 'vue'

// Inject dengan default value
const theme = inject('theme', 'light')
const user = inject('user', { name: 'Guest', role: 'visitor' })

// Inject object dengan default
const settings = inject('settings', {
  notifications: true,
  autoSave: false,
  theme: 'light'
})
</script>

<template>
  <div :class="`theme-${theme}`">
    <h2>Welcome {{ user.name }}!</h2>
    <p>Role: {{ user.role }}</p>

    <h3>Settings:</h3>
    <ul>
      <li>Notifications: {{ settings.notifications ? 'On' : 'Off' }}</li>
      <li>Auto-save: {{ settings.autoSave ? 'On' : 'Off' }}</li>
      <li>Theme: {{ settings.theme }}</li>
    </ul>
  </div>
</template>
```

### 3. Object Destructuring dengan Inject

```vue
<script setup>
import { inject } from 'vue'

// Inject dan destructure langsung
const { title, genre, platform, releaseDate } = inject('gameInfo')

// Inject array dan destructure dalam loop
const moreGames = inject('moreGames', [])
</script>

<template>
  <div>
    <h2>{{ title }}</h2>
    <p>Genre: {{ Array.isArray(genre) ? genre.join(', ') : genre }}</p>
    <p>Platforms: {{ Array.isArray(platform) ? platform.join(', ') : platform }}</p>
    <p>Release Date: {{ releaseDate }}</p>

    <h3>More Games:</h3>
    <div v-for="(game, index) in moreGames" :key="index" class="game-card">
      <h4>{{ game.title }}</h4>
      <p>Genre: {{ game.genre }}</p>
      <p>Platform: {{ game.platform }}</p>
      <p>Release: {{ game.releaseDate }}</p>
    </div>
  </div>
</template>

<style scoped>
.game-card {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 10px;
  background: #f8fafc;
}
</style>
```

## 🔄 Problem Solving

### Masalah 1: Prop Drilling Chain

**❌ Cara Lama (Prop Drilling):**

```vue
<!-- App.vue -->
<script setup>
import SchoolComponent from './SchoolComponent.vue'

const studentName = 'Alex'
const studentAge = 20
const studentLocation = ['Earth', 'IDK']
</script>

<template>
  <SchoolComponent
    :studentName="studentName"
    :studentAge="studentAge"
    :studentLocation="studentLocation"
  />
</template>

<!-- SchoolComponent.vue -->
<script setup>
import StudentComponent from './StudentComponent.vue'
defineProps(['studentName', 'studentAge', 'studentLocation'])
</script>

<template>
  <StudentComponent
    :studentName="studentName"
    :studentAge="studentAge"
    :studentLocation="studentLocation"
  />
</template>

<!-- StudentComponent.vue -->
<script setup>
defineProps(['studentName', 'studentAge', 'studentLocation'])
</script>

<template>
  <h1>Student Name: {{ studentName }}</h1>
  <h1>Student Age: {{ studentAge }}</h1>
  <h1>Student Location: {{ studentLocation }}</h1>
</template>
```

**✅ Solusi dengan Provide & Inject:**

```vue
<!-- App.vue -->
<script setup>
import { provide } from 'vue'
import SchoolComponent from './SchoolComponent.vue'

provide('studentName', 'Alex')
provide('studentAge', 20)
provide('studentLocation', ['Earth', 'IDK'])
</script>

<template>
  <SchoolComponent />
</template>

<!-- SchoolComponent.vue -->
<script setup>
import StudentComponent from './StudentComponent.vue'
// Tidak perlu props lagi!
</script>

<template>
  <StudentComponent />
</template>

<!-- StudentComponent.vue -->
<script setup>
import { inject } from 'vue'

const studentName = inject('studentName')
const studentAge = inject('studentAge')
const studentLocation = inject('studentLocation')
</script>

<template>
  <h1>Student Name: {{ studentName }}</h1>
  <h1>Student Age: {{ studentAge }}</h1>
  <h1>Student Location: {{ studentLocation }}</h1>
</template>
```

### Masalah 2: Global State Management

```vue
<!-- App.vue -->
<script setup>
import { provide, ref, reactive } from 'vue'
import RouterView from './RouterView.vue'

// Global auth state
const auth = reactive({
  user: null,
  token: null,
  isAuthenticated: false
})

// Global theme state
const theme = ref('light')

// Global loading state
const loading = ref(false)

// Auth methods
const login = async (credentials) => {
  loading.value = true
  try {
    // Simulasi API call
    await new Promise(resolve => setTimeout(resolve, 1000))
    auth.user = { id: 1, name: 'John Doe', email: 'john@example.com' }
    auth.token = 'fake-jwt-token'
    auth.isAuthenticated = true
  } finally {
    loading.value = false
  }
}

const logout = () => {
  auth.user = null
  auth.token = null
  auth.isAuthenticated = false
}

// Theme methods
const setTheme = (newTheme) => {
  theme.value = newTheme
  document.documentElement.className = newTheme
}

// Provide semua yang dibutuhkan
provide('auth', auth)
provide('theme', theme)
provide('loading', loading)
provide('login', login)
provide('logout', logout)
provide('setTheme', setTheme)
</script>

<template>
  <div :class="`theme-${theme}`">
    <RouterView />
  </div>
</template>
```

## 🎯 Advanced Patterns

### 1. Provider Composition

```vue
<!-- App.vue -->
<script setup>
import { provide, ref, computed } from 'vue'

// User provider
const useUserProvider = () => {
  const user = ref(null)
  const isAdmin = computed(() => user.value?.role === 'admin')

  const setUser = (newUser) => {
    user.value = newUser
  }

  provide('user', user)
  provide('isAdmin', isAdmin)
  provide('setUser', setUser)
}

// Theme provider
const useThemeProvider = () => {
  const theme = ref('light')
  const isDark = computed(() => theme.value === 'dark')

  const setTheme = (newTheme) => {
    theme.value = newTheme
    document.documentElement.setAttribute('data-theme', newTheme)
  }

  const toggleTheme = () => {
    setTheme(isDark.value ? 'light' : 'dark')
  }

  provide('theme', theme)
  provide('isDark', isDark)
  provide('setTheme', setTheme)
  provide('toggleTheme', toggleTheme)
}

// Notification provider
const useNotificationProvider = () => {
  const notifications = ref([])

  const addNotification = (message, type = 'info') => {
    const id = Date.now()
    notifications.value.push({ id, message, type })

    // Auto remove after 3 seconds
    setTimeout(() => {
      removeNotification(id)
    }, 3000)
  }

  const removeNotification = (id) => {
    const index = notifications.value.findIndex(n => n.id === id)
    if (index > -1) {
      notifications.value.splice(index, 1)
    }
  }

  provide('notifications', notifications)
  provide('addNotification', addNotification)
  provide('removeNotification', removeNotification)
}

// Gunakan semua providers
useUserProvider()
useThemeProvider()
useNotificationProvider()
</script>
```

### 2. Symbol Keys untuk Type Safety

```vue
<!-- keys.js -->
import { Symbol } from 'vue'

export const UserKey = Symbol('user')
export const ThemeKey = Symbol('theme')
export const ApiKey = Symbol('api')

<!-- App.vue -->
<script setup>
import { provide, reactive } from 'vue'
import { UserKey, ThemeKey, ApiKey } from './keys'

provide(UserKey, reactive({
  name: 'John Doe',
  email: 'john@example.com',
  role: 'admin'
}))

provide(ThemeKey, {
  current: 'dark',
  colors: {
    primary: '#3b82f6',
    secondary: '#10b981'
  }
})

provide(ApiKey, {
  baseURL: 'https://api.example.com',
  version: 'v1'
})
</script>

<!-- ChildComponent.vue -->
<script setup>
import { inject } from 'vue'
import { UserKey, ThemeKey, ApiKey } from './keys'

const user = inject(UserKey)
const theme = inject(ThemeKey)
const api = inject(ApiKey)
</script>
```

### 3. Plugin Pattern dengan Provide & Inject

```vue
<!-- plugins/auth.js -->
import { ref, reactive, provide } from 'vue'

export const createAuthPlugin = () => {
  const auth = reactive({
    user: null,
    token: null,
    isAuthenticated: false
  })

  const login = async (credentials) => {
    // Login logic
  }

  const logout = () => {
    auth.user = null
    auth.token = null
    auth.isAuthenticated = false
  }

  provide('auth', auth)
  provide('login', login)
  provide('logout', logout)

  return {
    auth,
    login,
    logout
  }
}

<!-- App.vue -->
<script setup>
import { createAuthPlugin } from './plugins/auth'

// Initialize auth plugin
createAuthPlugin()
</script>
```

## 🔧 Reactive Providers

### 1. Reactive Primitive Values

```vue
<!-- App.vue -->
<script setup>
import { provide, ref } from 'vue'

const counter = ref(0)
const message = ref('Hello Vue')

provide('counter', counter)
provide('message', message)

// Methods untuk update
provide('increment', () => counter.value++)
provide('decrement', () => counter.value--)
provide('updateMessage', (newMessage) => {
  message.value = newMessage
})
</script>

<!-- CounterComponent.vue -->
<script setup>
import { inject } from 'vue'

const counter = inject('counter')
const increment = inject('increment')
const decrement = inject('decrement')
</script>

<template>
  <div>
    <h2>Counter: {{ counter }}</h2>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>
```

### 2. Reactive Objects

```vue
<!-- App.vue -->
<script setup>
import { provide, reactive, computed } from 'vue'

const user = reactive({
  id: 1,
  name: 'John Doe',
  email: 'john@example.com',
  preferences: {
    theme: 'light',
    notifications: true,
    language: 'en'
  }
})

const fullName = computed(() => user.name)

provide('user', user)
provide('fullName', fullName)

// Update methods
provide('updateUser', (updates) => {
  Object.assign(user, updates)
})

provide('updatePreferences', (prefs) => {
  Object.assign(user.preferences, prefs)
})

provide('toggleTheme', () => {
  user.preferences.theme = user.preferences.theme === 'light' ? 'dark' : 'light'
})
</script>

<!-- UserProfile.vue -->
<script setup>
import { inject } from 'vue'

const user = inject('user')
const fullName = inject('fullName')
const toggleTheme = inject('toggleTheme')
</script>

<template>
  <div class="user-profile">
    <h2>{{ fullName }}</h2>
    <p>Email: {{ user.email }}</p>
    <p>Theme: {{ user.preferences.theme }}</p>
    <p>Notifications: {{ user.preferences.notifications ? 'Enabled' : 'Disabled' }}</p>

    <button @click="toggleTheme">Toggle Theme</button>
  </div>
</template>
```

### 3. Computed Providers

```vue
<!-- App.vue -->
<script setup>
import { provide, ref, computed } from 'vue'

const products = ref([
  { id: 1, name: 'Laptop', price: 999, category: 'electronics' },
  { id: 2, name: 'Book', price: 29, category: 'books' },
  { id: 3, name: 'Headphones', price: 199, category: 'electronics' },
  { id: 4, name: 'Pen', price: 5, category: 'stationery' }
])

const searchQuery = ref('')
const selectedCategory = ref('all')

// Computed properties
const filteredProducts = computed(() => {
  return products.value.filter(product => {
    const matchesSearch = product.name.toLowerCase()
      .includes(searchQuery.value.toLowerCase())
    const matchesCategory = selectedCategory.value === 'all' ||
      product.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
})

const categories = computed(() => {
  const cats = [...new Set(products.value.map(p => p.category))]
  return ['all', ...cats]
})

const totalProducts = computed(() => filteredProducts.value.length)

provide('products', products)
provide('filteredProducts', filteredProducts)
provide('categories', categories)
provide('totalProducts', totalProducts)
provide('searchQuery', searchQuery)
provide('selectedCategory', selectedCategory)

// Methods
provide('setSearchQuery', (query) => {
  searchQuery.value = query
})

provide('setCategory', (category) => {
  selectedCategory.value = category
})
</script>
```

## 🚀 Best Practices

### 1. Gunakan Symbol Keys untuk Type Safety

```vue
<!-- ✅ Gunakan Symbol keys -->
import { Symbol } from 'vue'

export const ThemeKey = Symbol('theme')
export const UserKey = Symbol('user')

provide(ThemeKey, themeData)
const theme = inject(ThemeKey)

<!-- ❌ Hindari string keys (typo prone) -->
provide('theme', themeData)
const theme = inject('theme') // Bisa typo: 'themee'
```

### 2. Provide Default Values

```vue
<script setup>
import { inject } from 'vue'

// ✅ Selalu berikan default value
const theme = inject('theme', 'light')
const user = inject('user', { name: 'Guest' })
const settings = inject('settings', {
  notifications: true,
  autoSave: false
})

// ❌ Jangan inject tanpa default
const theme = inject('theme') // Bisa undefined
</script>
```

### 3. Group Related Data

```vue
<script setup>
// ✅ Group related data
provide('auth', {
  user: user.value,
  token: token.value,
  isAuthenticated: computed(() => !!user.value),
  login: login,
  logout: logout
})

// ❌ Jangan provide data terpisah jika related
provide('user', user.value)
provide('token', token.value)
provide('isAuthenticated', computed(() => !!user.value))
provide('login', login)
provide('logout', logout)
</script>
```

### 4. Gunakan Composables untuk Complex Providers

```vue
<!-- composables/useAuth.js -->
import { ref, reactive, computed, provide } from 'vue'

export const useAuthProvider = () => {
  const auth = reactive({
    user: null,
    token: null,
    isAuthenticated: false
  })

  const login = async (credentials) => {
    // Login implementation
  }

  const logout = () => {
    auth.user = null
    auth.token = null
    auth.isAuthenticated = false
  }

  provide('auth', auth)
  provide('login', login)
  provide('logout', logout)

  return {
    auth,
    login,
    logout
  }
}

<!-- App.vue -->
<script setup>
import { useAuthProvider } from './composables/useAuth'

useAuthProvider()
</script>
```

## 💡 Tips dan Trik

### 1. Injection Validation

```vue
<script setup>
import { inject } from 'vue'

// Validasi inject result
const injectWithValidation = (key, defaultValue) => {
  const value = inject(key, defaultValue)

  if (process.env.NODE_ENV === 'development') {
    if (value === defaultValue && defaultValue === undefined) {
      console.warn(`Injection "${key}" not found and no default value provided`)
    }
  }

  return value
}

const theme = injectWithValidation('theme', 'light')
const user = injectWithValidation('user')
</script>
```

### 2. Conditional Injection

```vue
<script setup>
import { inject, computed } from 'vue'

const user = inject('user')
const settings = inject('settings')

// Conditional data berdasarkan user role
const adminSettings = computed(() => {
  if (user?.role === 'admin') {
    return {
      ...settings,
      adminPanel: true,
      debugMode: true
    }
  }
  return settings
})

// Conditional methods
const canEdit = computed(() => {
  return user?.role === 'admin' || user?.role === 'editor'
})
</script>
```

### 3. Provider Factory Pattern

```vue
<!-- factories/providerFactory.js -->
export const createProvider = (key, initialValue) => {
  const data = ref(initialValue)

  const update = (newValue) => {
    data.value = newValue
  }

  const reset = () => {
    data.value = initialValue
  }

  provide(key, data)
  provide(`${key}Update`, update)
  provide(`${key}Reset`, reset)

  return { data, update, reset }
}

<!-- App.vue -->
<script setup>
import { createProvider } from './factories/providerFactory'

createProvider('theme', 'light')
createProvider('language', 'en')
createProvider('fontSize', 16)
</script>
```

### 4. Debug Provide & Inject

```vue
<script setup>
import { provide, inject } from 'vue'

// Debug provider
const debugProvide = (key, value) => {
  if (process.env.NODE_ENV === 'development') {
    console.log(`[PROVIDE] ${key}:`, value)
  }
  provide(key, value)
}

// Debug inject
const debugInject = (key, defaultValue) => {
  const value = inject(key, defaultValue)

  if (process.env.NODE_ENV === 'development') {
    console.log(`[INJECT] ${key}:`, value)
  }

  return value
}

// Usage
debugProvide('user', { name: 'John' })
const user = debugInject('user', { name: 'Guest' })
</script>
```

---

## 🎉 Kesimpulan

Provide & Inject adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🔄 Dependency Injection** - Menghindari prop drilling
2. **📦 Global State** - Berbagi data tanpa library eksternal
3. **🎯 Service Injection** - Menginjeksi services dan utilities
4. **🛡️ Decoupling** - Mengurangi ketergantungan antar komponen
5. **🔧 Flexible Architecture** - Membangun sistem yang modular

### Poin Penting:

- Provide di ancestor, inject di descendant
- Gunakan default values untuk safety
- Symbol keys lebih aman dari typo
- Reactive data memungkinkan real-time updates
- Composables membuat providers lebih terorganisir

### Kapan Menggunakan Provide & Inject:

- ✅ Global configuration dan settings
- ✅ Theme dan user preferences
- ✅ Authentication dan authorization
- ✅ Services dan utilities
- ✅ Complex component hierarchies

### Kapan Tidak Menggunakan:

- ❌ Simple parent-child communication (gunakan props/emit)
- ❌ Data yang berubah sering di deep nesting (pertimbangkan state management)
- ❌ Communication antar sibling components (gunakan event bus atau state management)

Dengan memahami provide & inject, Anda sudah bisa membangun aplikasi Vue.js yang lebih terstruktur dan maintainable! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan provide & inject dalam berbagai skenario untuk memperkuat pemahaman Anda!

## ❓ Pertanyaan: Apa Fungsi Provide & Inject?

Provide & Inject memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk dependency injection:

### 1. **Dependency Injection & Decoupling**
```vue
<!-- App.vue - Root component yang menyediakan services -->
<script setup>
import { provide, reactive } from 'vue'

// Fungsi 1: Menyediakan dependencies dan mengurangi coupling
// Database service
const databaseService = reactive({
  connection: null,
  isConnected: false,

  async connect() {
    // Simulasi database connection
    await new Promise(resolve => setTimeout(resolve, 1000))
    this.connection = 'database_connection_established'
    this.isConnected = true
    console.log('Database connected')
  },

  async disconnect() {
    await new Promise(resolve => setTimeout(resolve, 500))
    this.connection = null
    this.isConnected = false
    console.log('Database disconnected')
  },

  async query(sql) {
    if (!this.isConnected) {
      throw new Error('Database not connected')
    }
    console.log(`Executing query: ${sql}`)
    return { results: [], affected: 0 }
  }
})

// API service
const apiService = reactive({
  baseURL: 'https://api.example.com',
  token: null,

  setToken(newToken) {
    this.token = newToken
  },

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`
    const headers = {
      'Content-Type': 'application/json',
      ...(this.token && { 'Authorization': `Bearer ${this.token}` }),
      ...options.headers
    }

    console.log(`API Request: ${method} ${url}`)
    // Simulasi API call
    await new Promise(resolve => setTimeout(resolve, 300))
    return { data: [], status: 200 }
  }
})

// Logger service
const loggerService = reactive({
  logs: [],

  log(message, level = 'info') {
    const entry = {
      timestamp: new Date().toISOString(),
      level,
      message
    }
    this.logs.push(entry)
    console.log(`[${level.toUpperCase()}] ${message}`)
  },

  error(message) {
    this.log(message, 'error')
  },

  warn(message) {
    this.log(message, 'warn')
  },

  info(message) {
    this.log(message, 'info')
  },

  getLogs() {
    return [...this.logs]
  },

  clearLogs() {
    this.logs = []
  }
})

// Provide semua services
provide('databaseService', databaseService)
provide('apiService', apiService)
provide('loggerService', loggerService)
</script>

<template>
  <div class="app">
    <header>
      <h1>Application with Dependency Injection</h1>
    </header>
    <main>
      <!-- Semua child components bisa inject services tanpa tight coupling -->
      <Dashboard />
      <UserManagement />
      <Reports />
    </main>
  </div>
</template>

<!-- Dashboard.vue - Child component yang menggunakan services -->
<script setup>
import { inject, onMounted } from 'vue'

// Inject services yang dibutuhkan
const databaseService = inject('databaseService')
const apiService = inject('apiService')
const loggerService = inject('loggerService')

// Component logic terpisah dari service implementation
onMounted(async () => {
  try {
    loggerService.info('Dashboard component mounted')

    // Connect to database
    await databaseService.connect()

    // Set API token
    apiService.setToken('dashboard_token_123')

    // Load dashboard data
    const data = await apiService.request('/dashboard')
    loggerService.info('Dashboard data loaded successfully')

  } catch (error) {
    loggerService.error(`Failed to initialize dashboard: ${error.message}`)
  }
})

const refreshData = async () => {
  try {
    loggerService.info('Refreshing dashboard data...')
    const data = await apiService.request('/dashboard/refresh')
    loggerService.info('Dashboard data refreshed')
  } catch (error) {
    loggerService.error(`Failed to refresh data: ${error.message}`)
  }
}
</script>

<template>
  <div class="dashboard">
    <h2>Dashboard</h2>
    <p>Database Status: {{ databaseService.isConnected ? 'Connected' : 'Disconnected' }}</p>
    <p>API Base URL: {{ apiService.baseURL }}</p>
    <p>Log Count: {{ loggerService.logs.length }}</p>

    <button @click="refreshData">Refresh Data</button>
    <button @click="loggerService.clearLogs()">Clear Logs</button>

    <div class="logs">
      <h3>Recent Logs:</h3>
      <div v-for="log in loggerService.getLogs().slice(-5)" :key="log.timestamp" class="log-entry">
        <span class="timestamp">{{ new Date(log.timestamp).toLocaleTimeString() }}</span>
        <span :class="['level', log.level]">[{{ log.level.toUpperCase() }}]</span>
        <span class="message">{{ log.message }}</span>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Loose coupling** - components tidak terikat ke implementation spesifik
- ✅ **Testability** - mudah mock services untuk testing
- ✅ **Reusability** - services dapat digunakan di banyak components
- ✅ **Maintainability** - perubahan implementation tidak affect components

### 2. **Global State Management**
```vue
<!-- App.vue - Global state provider -->
<script setup>
import { provide, reactive, computed } from 'vue'

// Fungsi 2: Centralized global state management
const globalState = reactive({
  // User state
  user: {
    id: null,
    name: null,
    email: null,
    role: null,
    preferences: {
      theme: 'light',
      language: 'en',
      notifications: true
    }
  },

  // App state
  app: {
    isLoading: false,
    sidebarOpen: true,
    modals: {
      settings: false,
      profile: false
    },
    notifications: []
  },

  // Cache state
  cache: {
    users: new Map(),
    posts: new Map(),
    lastUpdated: null
  }
})

// Computed properties untuk derived state
const isAuthenticated = computed(() => globalState.user.id !== null)
const isAdmin = computed(() => globalState.user.role === 'admin')
const currentTheme = computed(() => globalState.user.preferences.theme)
const unreadNotifications = computed(() =>
  globalState.app.notifications.filter(n => !n.read).length
)

// Actions untuk state management
const userActions = {
  async login(credentials) {
    globalState.app.isLoading = true

    try {
      // Simulasi API call
      await new Promise(resolve => setTimeout(resolve, 1000))

      globalState.user = {
        id: 1,
        name: 'John Doe',
        email: 'john@example.com',
        role: 'user',
        preferences: {
          theme: 'light',
          language: 'en',
          notifications: true
        }
      }

      globalState.app.notifications.push({
        id: Date.now(),
        type: 'success',
        message: `Welcome back, ${globalState.user.name}!`,
        read: false,
        timestamp: new Date()
      })

    } finally {
      globalState.app.isLoading = false
    }
  },

  logout() {
    globalState.user = {
      id: null,
      name: null,
      email: null,
      role: null,
      preferences: {
        theme: 'light',
        language: 'en',
        notifications: true
      }
    }

    globalState.app.notifications.push({
      id: Date.now(),
      type: 'info',
      message: 'You have been logged out',
      read: false,
      timestamp: new Date()
    })
  },

  updatePreferences(newPreferences) {
    globalState.user.preferences = { ...globalState.user.preferences, ...newPreferences }
  }
}

const appActions = {
  setLoading(isLoading) {
    globalState.app.isLoading = isLoading
  },

  toggleSidebar() {
    globalState.app.sidebarOpen = !globalState.app.sidebarOpen
  },

  openModal(modalName) {
    globalState.app.modals[modalName] = true
  },

  closeModal(modalName) {
    globalState.app.modals[modalName] = false
  },

  addNotification(notification) {
    globalState.app.notifications.push({
      id: Date.now(),
      ...notification,
      read: false,
      timestamp: new Date()
    })
  },

  markNotificationRead(notificationId) {
    const notification = globalState.app.notifications.find(n => n.id === notificationId)
    if (notification) {
      notification.read = true
    }
  }
}

const cacheActions = {
  setCache(key, data) {
    globalState.cache[key] = data
    globalState.cache.lastUpdated = new Date()
  },

  getCache(key) {
    return globalState.cache[key]
  },

  clearCache() {
    globalState.cache.users.clear()
    globalState.cache.posts.clear()
    globalState.cache.lastUpdated = null
  }
}

// Provide state dan actions
provide('globalState', globalState)
provide('isAuthenticated', isAuthenticated)
provide('isAdmin', isAdmin)
provide('currentTheme', currentTheme)
provide('unreadNotifications', unreadNotifications)

provide('userActions', userActions)
provide('appActions', appActions)
provide('cacheActions', cacheActions)
</script>

<template>
  <div class="app" :class="`theme-${currentTheme}`">
    <!-- Global loading indicator -->
    <div v-if="globalState.app.isLoading" class="global-loading">
      Loading...
    </div>

    <!-- Sidebar dapat akses global state -->
    <Sidebar v-if="globalState.app.sidebarOpen" />

    <!-- Header dapat akses user state -->
    <Header />

    <!-- Main content -->
    <main>
      <RouterView />
    </main>

    <!-- Global notifications -->
    <NotificationCenter />
  </div>
</template>

<!-- UserMenu.vue - Component yang mengakses global state -->
<script setup>
import { inject, computed } from 'vue'

// Inject global state dan actions
const globalState = inject('globalState')
const userActions = inject('userActions')
const appActions = inject('appActions')
const isAuthenticated = inject('isAuthenticated')
const unreadNotifications = inject('unreadNotifications')

const showUserMenu = ref(false)

const handleLogout = () => {
  userActions.logout()
  showUserMenu.value = false
}

const openSettings = () => {
  appActions.openModal('settings')
  showUserMenu.value = false
}

const openProfile = () => {
  appActions.openModal('profile')
  showUserMenu.value = false
}
</script>

<template>
  <div class="user-menu">
    <!-- User info dari global state -->
    <div v-if="isAuthenticated" class="user-info" @click="showUserMenu = !showUserMenu">
      <span class="user-name">{{ globalState.user.name }}</span>
      <span class="user-role">{{ globalState.user.role }}</span>

      <!-- Notification badge dari global state -->
      <span v-if="unreadNotifications > 0" class="notification-badge">
        {{ unreadNotifications }}
      </span>
    </div>

    <!-- Dropdown menu -->
    <div v-if="showUserMenu" class="dropdown">
      <button @click="openProfile">👤 Profile</button>
      <button @click="openSettings">⚙️ Settings</button>
      <button @click="handleLogout">🚪 Logout</button>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Centralized state** - semua state management di satu tempat
- ✅ **Reactive updates** - perubahan state otomatis di semua components
- ✅ **Action-based** - state changes melalui actions yang terstruktur
- ✅ **Debuggable** - mudah trace state changes

### 3. **Configuration & Theme Management**
```vue
<!-- App.vue - Configuration provider -->
<script setup>
import { provide, reactive, watch } from 'vue'

// Fungsi 3: Centralized configuration dan theme management
const config = reactive({
  app: {
    name: 'Vue.js Application',
    version: '2.1.0',
    apiURL: 'https://api.example.com',
    environment: import.meta.env.MODE,
    debug: import.meta.env.DEV
  },

  ui: {
    theme: 'light',
    primaryColor: '#3b82f6',
    secondaryColor: '#10b981',
    fontSize: 16,
    fontFamily: 'Inter, sans-serif',
    borderRadius: 8,
    shadows: true,
    animations: true
  },

  features: {
    darkMode: true,
    notifications: true,
    analytics: true,
    betaFeatures: false,
    experimentalFeatures: false
  },

  localization: {
    language: 'en',
    dateFormat: 'MM/DD/YYYY',
    timeFormat: '12h',
    currency: 'USD',
    timezone: 'UTC'
  }
})

// Theme configurations
const themes = {
  light: {
    name: 'Light',
    colors: {
      background: '#ffffff',
      surface: '#f8fafc',
      primary: '#3b82f6',
      secondary: '#10b981',
      text: '#1e293b',
      textSecondary: '#64748b',
      border: '#e2e8f0',
      error: '#ef4444',
      warning: '#f59e0b',
      success: '#10b981'
    }
  },

  dark: {
    name: 'Dark',
    colors: {
      background: '#0f172a',
      surface: '#1e293b',
      primary: '#60a5fa',
      secondary: '#34d399',
      text: '#f1f5f9',
      textSecondary: '#94a3b8',
      border: '#334155',
      error: '#f87171',
      warning: '#fbbf24',
      success: '#34d399'
    }
  },

  auto: {
    name: 'Auto',
    colors: {} // Will be determined by system preference
  }
}

// Current theme computed
const currentTheme = computed(() => {
  if (config.ui.theme === 'auto') {
    // Check system preference
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
    return prefersDark ? themes.dark : themes.light
  }
  return themes[config.ui.theme] || themes.light
})

// Apply theme to CSS variables
const applyTheme = (theme) => {
  const root = document.documentElement

  Object.entries(theme.colors).forEach(([key, value]) => {
    root.style.setProperty(`--color-${key}`, value)
  })

  root.style.setProperty('--font-size', `${config.ui.fontSize}px`)
  root.style.setProperty('--font-family', config.ui.fontFamily)
  root.style.setProperty('--border-radius', `${config.ui.borderRadius}px`)
  root.style.setProperty('--primary-color', config.ui.primaryColor)
  root.style.setProperty('--secondary-color', config.ui.secondaryColor)

  // Enable/disable animations
  root.style.setProperty('--animations-enabled', config.ui.animations ? '1' : '0')
}

// Watch theme changes
watch(() => config.ui.theme, (newTheme) => {
  applyTheme(currentTheme.value)
  localStorage.setItem('theme', newTheme)
}, { immediate: true })

// Watch UI changes
watch(() => config.ui, (newUI) => {
  applyTheme(currentTheme.value)
  localStorage.setItem('ui-config', JSON.stringify(newUI))
}, { deep: true })

// Config management functions
const configActions = {
  setTheme(themeName) {
    if (themes[themeName]) {
      config.ui.theme = themeName
    }
  },

  updateUI(updates) {
    Object.assign(config.ui, updates)
  },

  updateFeatures(updates) {
    Object.assign(config.features, updates)
  },

  setLanguage(language) {
    config.localization.language = language
    localStorage.setItem('language', language)
  },

  resetToDefaults() {
    config.ui.theme = 'light'
    config.ui.fontSize = 16
    config.ui.primaryColor = '#3b82f6'
    config.features.darkMode = true
    config.localization.language = 'en'
  },

  exportConfig() {
    return JSON.stringify(config, null, 2)
  },

  importConfig(configString) {
    try {
      const imported = JSON.parse(configString)
      Object.assign(config, imported)
    } catch (error) {
      console.error('Failed to import config:', error)
    }
  }
}

// Provide configuration
provide('config', config)
provide('themes', themes)
provide('currentTheme', currentTheme)
provide('configActions', configActions)
</script>

<!-- ThemeSelector.vue - Component untuk theme management -->
<script setup>
import { inject } from 'vue'

// Inject configuration
const config = inject('config')
const currentTheme = inject('currentTheme')
const configActions = inject('configActions')

const availableThemes = ['light', 'dark', 'auto']
const availableLanguages = ['en', 'es', 'fr', 'de', 'id']
const fontSizes = [12, 14, 16, 18, 20]
</script>

<template>
  <div class="theme-selector">
    <h3>Theme Settings</h3>

    <!-- Theme selection -->
    <div class="setting-group">
      <label>Theme:</label>
      <div class="theme-options">
        <button
          v-for="theme in availableThemes"
          :key="theme"
          @click="configActions.setTheme(theme)"
          :class="{ active: config.ui.theme === theme }"
        >
          {{ theme.charAt(0).toUpperCase() + theme.slice(1) }}
        </button>
      </div>
    </div>

    <!-- Primary color -->
    <div class="setting-group">
      <label>Primary Color:</label>
      <input
        v-model="config.ui.primaryColor"
        type="color"
        @change="configActions.updateUI({ primaryColor: $event.target.value })"
      />
    </div>

    <!-- Font size -->
    <div class="setting-group">
      <label>Font Size:</label>
      <select
        :value="config.ui.fontSize"
        @change="configActions.updateUI({ fontSize: parseInt($event.target.value) })"
      >
        <option v-for="size in fontSizes" :key="size" :value="size">
          {{ size }}px
        </option>
      </select>
    </div>

    <!-- Language -->
    <div class="setting-group">
      <label>Language:</label>
      <select
        :value="config.localization.language"
        @change="configActions.setLanguage($event.target.value)"
      >
        <option v-for="lang in availableLanguages" :key="lang" :value="lang">
          {{ lang.toUpperCase() }}
        </option>
      </select>
    </div>

    <!-- Feature toggles -->
    <div class="setting-group">
      <label>Features:</label>
      <div class="feature-toggles">
        <label>
          <input
            v-model="config.features.darkMode"
            type="checkbox"
            @change="configActions.updateFeatures({ darkMode: $event.target.checked })"
          />
          Dark Mode
        </label>
        <label>
          <input
            v-model="config.features.notifications"
            type="checkbox"
            @change="configActions.updateFeatures({ notifications: $event.target.checked })"
          />
          Notifications
        </label>
        <label>
          <input
            v-model="config.ui.animations"
            type="checkbox"
            @change="configActions.updateUI({ animations: $event.target.checked })"
          />
          Animations
        </label>
      </div>
    </div>

    <!-- Actions -->
    <div class="setting-actions">
      <button @click="configActions.resetToDefaults">Reset to Defaults</button>
      <button @click="console.log(configActions.exportConfig())">Export Config</button>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Centralized config** - semua pengaturan di satu tempat
- ✅ **Dynamic theming** - theme dapat diubah runtime
- ✅ **Persistent settings** - konfigurasi tersimpan di localStorage
- ✅ **Real-time updates** - perubahan langsung terlihat di UI

### 4. **Plugin Architecture & Service Injection**
```vue>
<!-- plugins/AuthPlugin.js - Authentication service plugin -->
<script setup>
import { provide, reactive, computed } from 'vue'

// Fungsi 4: Plugin architecture dengan service injection
const createAuthPlugin = (config = {}) => {
  const auth = reactive({
    user: null,
    token: null,
    refreshToken: null,
    isAuthenticated: false,
    permissions: [],
    isLoading: false,
    error: null,
    lastLogin: null
  })

  // Auth configuration
  const authConfig = reactive({
    tokenKey: 'auth_token',
    refreshTokenKey: 'refresh_token',
    autoRefresh: true,
    refreshThreshold: 5 * 60 * 1000, // 5 minutes
    apiURL: 'https://api.example.com/auth',
    ...config
  })

  // Computed properties
  const userRoles = computed(() => auth.user?.roles || [])
  const isAdmin = computed(() => userRoles.value.includes('admin'))
  const isModerator = computed(() => userRoles.value.includes('moderator'))
  const hasPermission = (permission) => auth.permissions.includes(permission)

  // Authentication methods
  const login = async (credentials) => {
    auth.isLoading = true
    auth.error = null

    try {
      // Simulasi API call
      const response = await mockAuthAPI('login', credentials)

      auth.user = response.user
      auth.token = response.token
      auth.refreshToken = response.refreshToken
      auth.isAuthenticated = true
      auth.permissions = response.permissions || []
      auth.lastLogin = new Date()

      // Store tokens
      localStorage.setItem(authConfig.tokenKey, auth.token)
      localStorage.setItem(authConfig.refreshTokenKey, auth.refreshToken)

      // Setup auto refresh
      if (authConfig.autoRefresh) {
        setupTokenRefresh()
      }

      return { success: true, user: auth.user }
    } catch (error) {
      auth.error = error.message
      return { success: false, error: error.message }
    } finally {
      auth.isLoading = false
    }
  }

  const logout = async () => {
    try {
      if (auth.token) {
        await mockAuthAPI('logout', { token: auth.token })
      }
    } catch (error) {
      console.error('Logout error:', error)
    } finally {
      // Clear auth state
      auth.user = null
      auth.token = null
      auth.refreshToken = null
      auth.isAuthenticated = false
      auth.permissions = []
      auth.lastLogin = null
      auth.error = null

      // Clear storage
      localStorage.removeItem(authConfig.tokenKey)
      localStorage.removeItem(authConfig.refreshTokenKey)
    }
  }

  const refreshToken = async () => {
    if (!auth.refreshToken) {
      await logout()
      return false
    }

    try {
      const response = await mockAuthAPI('refresh', {
        refreshToken: auth.refreshToken
      })

      auth.token = response.token
      auth.refreshToken = response.refreshToken

      localStorage.setItem(authConfig.tokenKey, auth.token)
      localStorage.setItem(authConfig.refreshTokenKey, auth.refreshToken)

      return true
    } catch (error) {
      console.error('Token refresh failed:', error)
      await logout()
      return false
    }
  }

  const updateProfile = async (updates) => {
    auth.isLoading = true

    try {
      const response = await mockAuthAPI('updateProfile', {
        token: auth.token,
        updates
      })

      auth.user = { ...auth.user, ...response.user }
      return { success: true, user: auth.user }
    } catch (error) {
      auth.error = error.message
      return { success: false, error: error.message }
    } finally {
      auth.isLoading = false
    }
  }

  // Token refresh setup
  let refreshTimer = null

  const setupTokenRefresh = () => {
    if (refreshTimer) {
      clearInterval(refreshTimer)
    }

    refreshTimer = setInterval(async () => {
      const tokenAge = Date.now() - new Date(auth.lastLogin).getTime()
      if (tokenAge > authConfig.refreshThreshold) {
        await refreshToken()
      }
    }, 60000) // Check every minute
  }

  // Initialize auth from stored tokens
  const initializeAuth = async () => {
    const storedToken = localStorage.getItem(authConfig.tokenKey)
    const storedRefreshToken = localStorage.getItem(authConfig.refreshTokenKey)

    if (storedToken && storedRefreshToken) {
      auth.token = storedToken
      auth.refreshToken = storedRefreshToken

      try {
        // Validate token with API
        const response = await mockAuthAPI('validate', { token: storedToken })

        auth.user = response.user
        auth.isAuthenticated = true
        auth.permissions = response.permissions || []
        auth.lastLogin = new Date(response.lastLogin)

        if (authConfig.autoRefresh) {
          setupTokenRefresh()
        }
      } catch (error) {
        // Token invalid, clear everything
        await logout()
      }
    }
  }

  // Mock API for demonstration
  const mockAuthAPI = async (endpoint, data) => {
    await new Promise(resolve => setTimeout(resolve, 500))

    switch (endpoint) {
      case 'login':
        if (data.email === 'admin@example.com' && data.password === 'password') {
          return {
            user: {
              id: 1,
              name: 'Admin User',
              email: 'admin@example.com',
              roles: ['admin', 'user']
            },
            token: 'mock_jwt_token_' + Date.now(),
            refreshToken: 'mock_refresh_token_' + Date.now(),
            permissions: ['read', 'write', 'delete', 'admin']
          }
        }
        throw new Error('Invalid credentials')

      case 'validate':
        return {
          user: {
            id: 1,
            name: 'Admin User',
            email: 'admin@example.com',
            roles: ['admin', 'user']
          },
          permissions: ['read', 'write', 'delete', 'admin'],
          lastLogin: new Date().toISOString()
        }

      case 'refresh':
        return {
          token: 'refreshed_jwt_token_' + Date.now(),
          refreshToken: 'refreshed_refresh_token_' + Date.now()
        }

      case 'updateProfile':
        return { user: { ...auth.user, ...data.updates } }

      default:
        return {}
    }
  }

  // Provide auth services
  provide('auth', auth)
  provide('authConfig', authConfig)
  provide('userRoles', userRoles)
  provide('isAdmin', isAdmin)
  provide('isModerator', isModerator)
  provide('hasPermission', hasPermission)
  provide('login', login)
  provide('logout', logout)
  provide('refreshToken', refreshToken)
  provide('updateProfile', updateProfile)
  provide('initializeAuth', initializeAuth)

  return {
    auth,
    login,
    logout,
    initializeAuth
  }
}

export { createAuthPlugin }
</script>

<!-- App.vue - Menggunakan auth plugin -->
<script setup>
import { onMounted } from 'vue'
import { createAuthPlugin } from './plugins/AuthPlugin'

// Initialize auth plugin
const { initializeAuth } = createAuthPlugin({
  autoRefresh: true,
  apiURL: 'https://myapp.com/api/auth'
})

// Initialize auth on app mount
onMounted(() => {
  initializeAuth()
})
</script>

<!-- AdminPanel.vue - Component dengan permission checking -->
<script setup>
import { inject } from 'vue'

// Inject auth services
const auth = inject('auth')
const isAdmin = inject('isAdmin')
const hasPermission = inject('hasPermission')
const updateProfile = inject('updateProfile')

const adminActions = {
  deleteUser: async (userId) => {
    if (!hasPermission('delete')) {
      alert('You do not have permission to delete users')
      return
    }

    console.log(`Deleting user ${userId}`)
  },

  banUser: async (userId) => {
    if (!hasPermission('ban')) {
      alert('You do not have permission to ban users')
      return
    }

    console.log(`Banning user ${userId}`)
  },

  updateSettings: async (settings) => {
    if (!hasPermission('admin')) {
      alert('Admin access required')
      return
    }

    console.log('Updating admin settings:', settings)
  }
}

const handleProfileUpdate = async () => {
  const result = await updateProfile({
    name: 'Updated Name',
    email: 'updated@example.com'
  })

  if (result.success) {
    alert('Profile updated successfully!')
  } else {
    alert('Update failed: ' + result.error)
  }
}
</script>

<template>
  <div class="admin-panel">
    <h2>Admin Panel</h2>

    <!-- User info -->
    <div class="user-info">
      <h3>Current User: {{ auth.user?.name }}</h3>
      <p>Roles: {{ auth.user?.roles?.join(', ') }}</p>
      <p>Permissions: {{ auth.permissions.join(', ') }}</p>
      <p>Admin Access: {{ isAdmin ? 'Yes' : 'No' }}</p>
    </div>

    <!-- Admin actions based on permissions -->
    <div class="admin-actions">
      <button v-if="hasPermission('read')" @click="console.log('Viewing users')">
        👥 View Users
      </button>

      <button v-if="hasPermission('write')" @click="console.log('Creating user')">
        ➕ Create User
      </button>

      <button v-if="hasPermission('delete')" @click="adminActions.deleteUser(123)">
        🗑️ Delete User
      </button>

      <button v-if="hasPermission('admin')" @click="adminActions.updateSettings({ debug: true })">
        ⚙️ Update Settings
      </button>

      <button @click="handleProfileUpdate">
        ✏️ Update Profile
      </button>
    </div>

    <!-- Loading state -->
    <div v-if="auth.isLoading" class="loading">
      Processing...
    </div>

    <!-- Error display -->
    <div v-if="auth.error" class="error">
      Error: {{ auth.error }}
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Modular architecture** - plugin dapat di-load secara kondisional
- ✅ **Service injection** - services tersedia untuk semua components
- ✅ **Configuration flexibility** - plugin dapat dikonfigurasi
- ✅ **Auto-initialization** - services siap digunakan saat app start

### 5. **Cross-Component Communication**
```vue
<!-- App.vue - Event system provider -->
<script setup>
import { provide, reactive } from 'vue'

// Fungsi 5: Event system untuk cross-component communication
const eventBus = reactive({
  listeners: new Map(),
  history: [],
  maxHistory: 100
})

// Event bus methods
const eventBusActions = {
  // Subscribe to event
  on(event, callback) {
    if (!eventBus.listeners.has(event)) {
      eventBus.listeners.set(event, [])
    }
    eventBus.listeners.get(event).push(callback)

    // Return unsubscribe function
    return () => {
      const callbacks = eventBus.listeners.get(event)
      if (callbacks) {
        const index = callbacks.indexOf(callback)
        if (index > -1) {
          callbacks.splice(index, 1)
        }
      }
    }
  },

  // Emit event
  emit(event, data) {
    const eventEntry = {
      id: Date.now() + Math.random(),
      event,
      data,
      timestamp: new Date(),
      source: 'eventBus'
    }

    // Add to history
    eventBus.history.push(eventEntry)
    if (eventBus.history.length > eventBus.maxHistory) {
      eventBus.history.shift()
    }

    // Notify all listeners
    const callbacks = eventBus.listeners.get(event)
    if (callbacks) {
      callbacks.forEach(callback => {
        try {
          callback(data)
        } catch (error) {
          console.error(`Error in event listener for ${event}:`, error)
        }
      })
    }
  },

  // Subscribe once
  once(event, callback) {
    const onceCallback = (data) => {
      callback(data)
      // Auto unsubscribe
      const callbacks = eventBus.listeners.get(event)
      if (callbacks) {
        const index = callbacks.indexOf(onceCallback)
        if (index > -1) {
          callbacks.splice(index, 1)
        }
      }
    }

    return this.on(event, onceCallback)
  },

  // Clear all listeners for event
  off(event) {
    eventBus.listeners.delete(event)
  },

  // Clear all listeners
  clear() {
    eventBus.listeners.clear()
    eventBus.history = []
  },

  // Get event history
  getHistory(event) {
    if (event) {
      return eventBus.history.filter(entry => entry.event === event)
    }
    return [...eventBus.history]
  }
})

// Toast notification system
const toastSystem = reactive({
  toasts: [],
  maxToasts: 5,
  defaultDuration: 3000
})

const toastActions = {
  show(message, options = {}) {
    const toast = {
      id: Date.now() + Math.random(),
      message,
      type: options.type || 'info',
      duration: options.duration || toastSystem.defaultDuration,
      persistent: options.persistent || false,
      timestamp: new Date()
    }

    toastSystem.toasts.push(toast)

    // Auto remove if not persistent
    if (!toast.persistent) {
      setTimeout(() => {
        this.remove(toast.id)
      }, toast.duration)
    }

    // Emit event
    eventBusActions.emit('toast:shown', toast)

    return toast.id
  },

  success(message, options = {}) {
    return this.show(message, { ...options, type: 'success' })
  },

  error(message, options = {}) {
    return this.show(message, { ...options, type: 'error', duration: 5000 })
  },

  warning(message, options = {}) {
    return this.show(message, { ...options, type: 'warning' })
  },

  info(message, options = {}) {
    return this.show(message, { ...options, type: 'info' })
  },

  remove(toastId) {
    const index = toastSystem.toasts.findIndex(t => t.id === toastId)
    if (index > -1) {
      const toast = toastSystem.toasts[index]
      toastSystem.toasts.splice(index, 1)
      eventBusActions.emit('toast:removed', toast)
    }
  },

  clear() {
    const toasts = [...toastSystem.toasts]
    toastSystem.toasts = []
    eventBusActions.emit('toast:cleared', toasts)
  }
}

// Modal system
const modalSystem = reactive({
  activeModals: new Map(),
  stack: []
})

const modalActions = {
  open(name, component, props = {}) {
    const modal = {
      id: Date.now() + Math.random(),
      name,
      component,
      props,
      timestamp: new Date()
    }

    modalSystem.activeModals.set(name, modal)
    modalSystem.stack.push(modal)

    eventBusActions.emit('modal:opened', modal)

    return modal.id
  },

  close(name) {
    const modal = modalSystem.activeModals.get(name)
    if (modal) {
      modalSystem.activeModals.delete(name)

      // Remove from stack
      const index = modalSystem.stack.findIndex(m => m.name === name)
      if (index > -1) {
        modalSystem.stack.splice(index, 1)
      }

      eventBusActions.emit('modal:closed', modal)
    }
  },

  closeAll() {
    const modals = Array.from(modalSystem.activeModals.values())
    modalSystem.activeModals.clear()
    modalSystem.stack = []

    modals.forEach(modal => {
      eventBusActions.emit('modal:closed', modal)
    })
  },

  isOpen(name) {
    return modalSystem.activeModals.has(name)
  },

  getActive() {
    return Array.from(modalSystem.activeModals.values())
  }
}

// Provide all systems
provide('eventBus', eventBusActions)
provide('toastSystem', toastSystem)
provide('toastActions', toastActions)
provide('modalSystem', modalSystem)
provide('modalActions', modalActions)
</script>

<!-- NotificationCenter.vue - Component yang menggunakan event system -->
<script setup>
import { inject, onMounted, onUnmounted } from 'vue'

const eventBus = inject('eventBus')
const toastActions = inject('toastActions')

let unsubscribe = null

onMounted(() => {
  // Subscribe to global events
  unsubscribe = eventBus.on('user:action', (data) => {
    console.log('User action detected:', data)

    if (data.action === 'login') {
      toastActions.success(`Welcome back, ${data.user.name}!`)
    } else if (data.action === 'logout') {
      toastActions.info('You have been logged out')
    }
  })

  // Subscribe to toast events
  eventBus.on('toast:shown', (toast) => {
    console.log('Toast shown:', toast)
  })

  // Subscribe to modal events
  eventBus.on('modal:opened', (modal) => {
    console.log('Modal opened:', modal.name)
  })
})

onUnmounted(() => {
  // Clean up listeners
  if (unsubscribe) {
    unsubscribe()
  }
})

const triggerTestEvents = () => {
  // Trigger various events
  eventBus.emit('test:event', { message: 'Test event triggered' })
  eventBus.emit('user:action', { action: 'test', user: { name: 'Test User' } })

  toastActions.info('Test event triggered!')
}
</script>

<template>
  <div class="notification-center">
    <h3>Event Monitor</h3>
    <button @click="triggerTestEvents">Trigger Test Events</button>

    <div class="event-history">
      <h4>Recent Events:</h4>
      <div v-for="event in eventBus.getHistory().slice(-10)" :key="event.id" class="event-item">
        <span class="time">{{ new Date(event.timestamp).toLocaleTimeString() }}</span>
        <span class="event">{{ event.event }}</span>
        <span class="data">{{ JSON.stringify(event.data) }}</span>
      </div>
    </div>
  </div>
</template>

<!-- UserButton.vue - Component yang emits events -->
<script setup>
import { inject } from 'vue'

const eventBus = inject('eventBus')
const toastActions = inject('toastActions')
const modalActions = inject('modalActions')

const handleLogin = () => {
  const user = { name: 'John Doe', email: 'john@example.com' }

  // Emit event untuk global notification
  eventBus.emit('user:action', {
    action: 'login',
    user,
    timestamp: new Date()
  })

  // Show toast
  toastActions.success('Login successful!')

  // Open welcome modal
  modalActions.open('welcome', 'WelcomeModal', { user })
}

const handleLogout = () => {
  eventBus.emit('user:action', {
    action: 'logout',
    timestamp: new Date()
  })

  toastActions.info('Logged out successfully')
}
</script>

<template>
  <div class="user-actions">
    <button @click="handleLogin">🔐 Login</button>
    <button @click="handleLogout">🚪 Logout</button>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Decoupled communication** - components tidak perlu direct import
- ✅ **Event-driven architecture** - reactive system untuk notifications
- ✅ **Global systems** - toast, modal, notification systems
- ✅ **Debuggable** - event history dan logging

## 🎯 Best Practices untuk Provide & Inject

### 1. **Gunakan Symbol Keys untuk Type Safety**
```vue
<script setup>
import { Symbol } from 'vue'

// ✅ GOOD: Symbol keys
export const UserKey = Symbol('user')
export const ThemeKey = Symbol('theme')

provide(UserKey, userData)
const user = inject(UserKey)

// ❌ AVOID: String keys (typo prone)
provide('user', userData)
const user = inject('user') // Bisa typo: 'useer'
</script>
```

### 2. **Provide Default Values**
```vue
<script setup>
// ✅ GOOD: Selalu berikan default values
const theme = inject('theme', 'light')
const user = inject('user', { name: 'Guest' })

// ❌ AVOID: Inject tanpa default
const theme = inject('theme') // Bisa undefined
</script>
```

### 3. **Group Related Data**
```vue
<script setup>
// ✅ GOOD: Group related data
provide('auth', {
  user,
  token,
  isAuthenticated,
  login,
  logout
})

// ❌ AVOID: Provide data terpisah
provide('user', user)
provide('token', token)
provide('isAuthenticated', isAuthenticated)
</script>
```

## 🎉 Kesimpulan

Provide & Inject adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Dependency injection** - menghindari prop drilling
- ✅ **Global state management** - centralized state management
- ✅ **Configuration management** - theme dan settings management
- ✅ **Plugin architecture** - modular service injection
- ✅ **Cross-component communication** - event-driven communication

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Provide & Inject
- Vue.js Dependency Injection Guide
- Vue.js Composition API Documentation

---

**🔥 Tips Pro:**
- Gunakan **Symbol keys** untuk type safety
- Selalu sediakan **default values** untuk safety
- Group related data dalam **single provider**
- Manfaatkan **reactive data** untuk real-time updates
- Implement **plugin patterns** untuk modular architecture

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation