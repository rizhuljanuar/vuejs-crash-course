# Implementasi Step-by-Step Github Users Search

## Panduan Lengkap Pembuatan Project dari Awal hingga Akhir

### Daftar Isi
1. [Persiapan Lingkungan](#1-persiapan-lingkungan)
2. [Setup Project Vue.js](#2-setup-project-vuejs)
3. [Struktur Folder Project](#3-struktur-folder-project)
4. [Implementasi Komponen Dasar](#4-implementasi-komponen-dasar)
5. [Integrasi GitHub API](#5-integrasi-github-api)
6. [Menambahkan Fitur Search History](#6-menambahkan-fitur-search-history)
7. [Implementasi Debouncing](#7-implementasi-debouncing)
8. [Optimasi Performance](#8-optimasi-performance)
9. [Styling dan Responsive Design](#9-styling-dan-responsive-design)
10. [Error Handling](#10-error-handling)
11. [Testing](#11-testing)
12. [Deployment](#12-deployment)

---

## 1. Persiapan Lingkungan

### 1.1. Install Node.js dan npm
Pastikan Node.js dan npm sudah terinstall di komputer Anda:

```bash
# Cek versi Node.js
node --version

# Cek versi npm
npm --version
```

Jika belum terinstall, download dari [nodejs.org](https://nodejs.org/)

### 1.2. Install Vue CLI atau Gunakan Vite
Kita akan menggunakan Vite untuk setup project yang lebih cepat:

```bash
# Install Vite globally (opsional)
npm install -g vite

# Atau gunakan npx untuk menjalankan tanpa install global
npx create-vite@latest
```

### 1.3. Setup Code Editor
Install Visual Studio Code dengan extensions berikut:
- Vetur (untuk Vue 2) atau Volar (untuk Vue 3)
- ESLint
- Prettier
- GitLens

---

## 2. Setup Project Vue.js

### 2.1. Buat Project Baru
```bash
# Buat folder project
mkdir github-users-search
cd github-users-search

# Inisialisasi project dengan Vite
npm create vue@latest . -- --template vue

# Install dependencies
npm install
```

### 2.2. Struktur Package.json
Pastikan `package.json` Anda memiliki dependencies berikut:

```json
{
  "name": "github-users-search",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.3.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.0.0",
    "vite": "^4.0.0"
  }
}
```

### 2.3. Konfigurasi Vite
Buat file `vite.config.js`:

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  server: {
    port: 3000,
    open: true
  }
})
```

---

## 3. Struktur Folder Project

### 3.1. Buat Struktur Folder
```
github-users-search/
├── public/
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── GithubUsersSearch.vue
│   │   ├── UserProfileCard.vue
│   │   └── SearchHistory.vue
│   ├── services/
│   │   └── githubService.js
│   ├── utils/
│   │   └── debounce.js
│   ├── composables/
│   │   └── useGithubSearch.js
│   ├── styles/
│   │   └── main.css
│   ├── App.vue
│   └── main.js
├── index.html
├── package.json
└── vite.config.js
```

### 3.2. Buat Folder dan File Kosong
```bash
mkdir -p src/components src/services src/utils src/composables src/styles

# Buat file-file kosong
touch src/components/GithubUsersSearch.vue
touch src/components/UserProfileCard.vue
touch src/components/SearchHistory.vue
touch src/services/githubService.js
touch src/utils/debounce.js
touch src/composables/useGithubSearch.js
touch src/styles/main.css
```

---

## 4. Implementasi Komponen Dasar

### 4.1. Setup Main Entry Point
Edit `src/main.js`:

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import './styles/main.css'

createApp(App).mount('#app')
```

### 4.2. Edit App.vue
Edit `src/App.vue`:

```vue
<template>
  <div id="app">
    <header>
      <h1>GitHub Users Search</h1>
      <p>Cari profil pengguna GitHub</p>
    </header>

    <main>
      <GithubUsersSearch />
    </main>

    <footer>
      <p>&copy; 2024 GitHub Users Search</p>
    </footer>
  </div>
</template>

<script setup>
import GithubUsersSearch from './components/GithubUsersSearch.vue'
</script>

<style scoped>
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  text-align: center;
}

main {
  flex: 1;
  padding: 2rem;
}

footer {
  background: #f8f9fa;
  padding: 1rem;
  text-align: center;
  color: #6c757d;
}
</style>
```

### 4.3. Implementasi Komponen Search Dasar
Edit `src/components/GithubUsersSearch.vue` (versi dasar):

```vue
<template>
  <div class="github-search">
    <div class="search-container">
      <input
        v-model="username"
        type="text"
        placeholder="Masukkan username GitHub"
        @keyup.enter="searchUser"
        class="search-input"
      />
      <button
        @click="searchUser"
        :disabled="loading || !username.trim()"
        class="search-button"
      >
        {{ loading ? 'Mencari...' : 'Cari' }}
      </button>
    </div>

    <div v-if="error" class="error-message">
      {{ error }}
    </div>

    <div v-if="loading" class="loading">
      Memuat data...
    </div>

    <UserProfileCard
      v-if="userProfile"
      :user="userProfile"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UserProfileCard from './UserProfileCard.vue'

// Reactive state
const username = ref('')
const userProfile = ref(null)
const error = ref(null)
const loading = ref(false)

// GitHub API service
const searchUser = async () => {
  if (!username.value.trim()) return

  loading.value = true
  error.value = null

  try {
    const response = await fetch(
      `https://api.github.com/users/${username.value}`
    )

    if (!response.ok) {
      throw new Error(`User "${username.value}" tidak ditemukan`)
    }

    const data = await response.json()
    userProfile.value = data

  } catch (err) {
    error.value = err.message
    userProfile.value = null
  } finally {
    loading.value = false
  }
}
</script>

<style scoped>
.github-search {
  max-width: 600px;
  margin: 0 auto;
}

.search-container {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 2rem;
}

.search-input {
  flex: 1;
  padding: 0.75rem;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  font-size: 1rem;
  transition: border-color 0.3s;
}

.search-input:focus {
  outline: none;
  border-color: #667eea;
}

.search-button {
  padding: 0.75rem 1.5rem;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s;
}

.search-button:hover:not(:disabled) {
  background: #5a67d8;
}

.search-button:disabled {
  background: #a0aec0;
  cursor: not-allowed;
}

.error-message {
  background: #fed7d7;
  color: #c53030;
  padding: 1rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

.loading {
  text-align: center;
  padding: 2rem;
  color: #4a5568;
}
</style>
```

---

## 5. Implementasi User Profile Card

### 5.1. Buat UserProfileCard Component
Edit `src/components/UserProfileCard.vue`:

```vue
<template>
  <div class="user-profile-card">
    <div class="profile-header">
      <img
        :src="user.avatar_url"
        :alt="user.name || user.login"
        class="avatar"
      />
      <div class="user-info">
        <h2>{{ user.name || user.login }}</h2>
        <p class="username">@{{ user.login }}</p>
        <p v-if="user.bio" class="bio">{{ user.bio }}</p>
      </div>
    </div>

    <div class="profile-stats">
      <div class="stat">
        <strong>{{ user.followers }}</strong>
        <span>Followers</span>
      </div>
      <div class="stat">
        <strong>{{ user.following }}</strong>
        <span>Following</span>
      </div>
      <div class="stat">
        <strong>{{ user.public_repos }}</strong>
        <span>Repositories</span>
      </div>
    </div>

    <div class="profile-links">
      <a
        v-if="user.blog"
        :href="user.blog"
        target="_blank"
        class="link"
      >
        🌐 Website
      </a>
      <a
        :href="user.html_url"
        target="_blank"
        class="link github-link"
      >
        🐙 GitHub Profile
      </a>
    </div>

    <div class="profile-details">
      <div v-if="user.location" class="detail">
        📍 {{ user.location }}
      </div>
      <div v-if="user.company" class="detail">
        🏢 {{ user.company }}
      </div>
      <div v-if="user.twitter_username" class="detail">
        🐦 @{{ user.twitter_username }}
      </div>
      <div class="detail">
        📅 Bergabung: {{ formatDate(user.created_at) }}
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  user: {
    type: Object,
    required: true
  }
})

const formatDate = (dateString) => {
  const options = { year: 'numeric', month: 'long', day: 'numeric' }
  return new Date(dateString).toLocaleDateString('id-ID', options)
}
</script>

<style scoped>
.user-profile-card {
  background: white;
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border: 1px solid #e1e5e9;
}

.profile-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.avatar {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #e1e5e9;
}

.user-info h2 {
  margin: 0 0 0.5rem 0;
  color: #2d3748;
  font-size: 1.5rem;
}

.username {
  color: #718096;
  font-size: 1rem;
  margin: 0 0 0.5rem 0;
}

.bio {
  color: #4a5568;
  margin: 0;
  line-height: 1.5;
}

.profile-stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  margin-bottom: 2rem;
  padding: 1rem 0;
  border-top: 1px solid #e1e5e9;
  border-bottom: 1px solid #e1e5e9;
}

.stat {
  text-align: center;
}

.stat strong {
  display: block;
  font-size: 1.5rem;
  color: #2d3748;
}

.stat span {
  color: #718096;
  font-size: 0.875rem;
}

.profile-links {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}

.link {
  padding: 0.5rem 1rem;
  background: #f7fafc;
  border: 1px solid #e1e5e9;
  border-radius: 6px;
  text-decoration: none;
  color: #4a5568;
  transition: all 0.3s;
}

.link:hover {
  background: #edf2f7;
  color: #2d3748;
}

.github-link {
  background: #2d3748;
  color: white;
  border-color: #2d3748;
}

.github-link:hover {
  background: #1a202c;
}

.profile-details {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.detail {
  color: #4a5568;
  font-size: 0.9rem;
}

@media (max-width: 768px) {
  .profile-header {
    flex-direction: column;
    text-align: center;
  }

  .profile-stats {
    grid-template-columns: 1fr;
  }

  .profile-links {
    flex-direction: column;
  }
}
</style>
```

---

## 6. Integrasi GitHub API Service

### 6.1. Buat Service Layer
Edit `src/services/githubService.js`:

```javascript
const API_BASE_URL = 'https://api.github.com'

class GithubService {
  constructor() {
    this.cache = new Map()
    this.cacheTimeout = 5 * 60 * 1000 // 5 menit
  }

  // Helper method untuk cache
  getCachedData(key) {
    const cached = this.cache.get(key)
    if (cached && Date.now() - cached.timestamp < this.cacheTimeout) {
      return cached.data
    }
    this.cache.delete(key)
    return null
  }

  setCacheData(key, data) {
    this.cache.set(key, {
      data,
      timestamp: Date.now()
    })
  }

  // Method untuk mencari user
  async getUser(username) {
    if (!username || typeof username !== 'string') {
      throw new Error('Username harus berupa string yang valid')
    }

    const cacheKey = `user:${username}`
    const cachedData = this.getCachedData(cacheKey)

    if (cachedData) {
      return cachedData
    }

    try {
      const response = await fetch(`${API_BASE_URL}/users/${username}`)

      if (!response.ok) {
        if (response.status === 404) {
          throw new Error(`User "${username}" tidak ditemukan`)
        } else if (response.status === 403) {
          throw new Error('Rate limit terlampaui. Silakan coba lagi dalam beberapa saat')
        } else {
          throw new Error(`Error ${response.status}: ${response.statusText}`)
        }
      }

      const userData = await response.json()
      this.setCacheData(cacheKey, userData)
      return userData

    } catch (error) {
      if (error.name === 'TypeError') {
        throw new Error('Koneksi gagal. Periksa koneksi internet Anda')
      }
      throw error
    }
  }

  // Method untuk mendapatkan repositories user
  async getUserRepos(username, page = 1, perPage = 10) {
    try {
      const response = await fetch(
        `${API_BASE_URL}/users/${username}/repos?page=${page}&per_page=${perPage}&sort=updated`
      )

      if (!response.ok) {
        throw new Error(`Gagal mengambil repositories: ${response.statusText}`)
      }

      return await response.json()

    } catch (error) {
      throw new Error(`Gagal mengambil repositories: ${error.message}`)
    }
  }

  // Method untuk mendapatkan followers
  async getUserFollowers(username, page = 1, perPage = 10) {
    try {
      const response = await fetch(
        `${API_BASE_URL}/users/${username}/followers?page=${page}&per_page=${perPage}`
      )

      if (!response.ok) {
        throw new Error(`Gagal mengambil followers: ${response.statusText}`)
      }

      return await response.json()

    } catch (error) {
      throw new Error(`Gagal mengambil followers: ${error.message}`)
    }
  }

  // Method untuk mendapatkan following
  async getUserFollowing(username, page = 1, perPage = 10) {
    try {
      const response = await fetch(
        `${API_BASE_URL}/users/${username}/following?page=${page}&per_page=${perPage}`
      )

      if (!response.ok) {
        throw new Error(`Gagal mengambil following: ${response.statusText}`)
      }

      return await response.json()

    } catch (error) {
      throw new Error(`Gagal mengambil following: ${error.message}`)
    }
  }

  // Method untuk search users
  async searchUsers(query, page = 1, perPage = 10) {
    if (!query) {
      throw new Error('Query pencarian tidak boleh kosong')
    }

    try {
      const response = await fetch(
        `${API_BASE_URL}/search/users?q=${encodeURIComponent(query)}&page=${page}&per_page=${perPage}`
      )

      if (!response.ok) {
        throw new Error(`Gagal mencari users: ${response.statusText}`)
      }

      const data = await response.json()
      return data

    } catch (error) {
      throw new Error(`Gagal mencari users: ${error.message}`)
    }
  }
}

export default new GithubService()
```

---

## 7. Menambahkan Fitur Search History

### 7.1. Buat Search History Component
Edit `src/components/SearchHistory.vue`:

```vue
<template>
  <div class="search-history">
    <div class="history-header">
      <h3>Riwayat Pencarian</h3>
      <button
        v-if="searchHistory.length > 0"
        @click="clearHistory"
        class="clear-button"
      >
        Hapus Semua
      </button>
    </div>

    <div v-if="searchHistory.length === 0" class="empty-history">
      Belum ada riwayat pencarian
    </div>

    <div v-else class="history-list">
      <div
        v-for="(item, index) in searchHistory"
        :key="index"
        class="history-item"
      >
        <div class="history-info">
          <img
            :src="item.avatar_url"
            :alt="item.login"
            class="history-avatar"
          />
          <div class="history-details">
            <span class="history-username">{{ item.login }}</span>
            <span class="history-time">{{ formatTime(item.timestamp) }}</span>
          </div>
        </div>
        <div class="history-actions">
          <button
            @click="searchAgain(item.login)"
            class="search-again-button"
          >
            Cari Lagi
          </button>
          <button
            @click="removeFromHistory(index)"
            class="remove-button"
          >
            ×
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const props = defineProps({
  currentSearch: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['search'])

const searchHistory = ref([])

// Load dari localStorage saat component dimount
onMounted(() => {
  loadHistory()
})

// Watch currentSearch untuk tambah ke history
watch(() => props.currentSearch, (newValue) => {
  if (newValue && newValue.trim()) {
    addToHistory(newValue)
  }
})

const loadHistory = () => {
  try {
    const history = localStorage.getItem('github-search-history')
    if (history) {
      searchHistory.value = JSON.parse(history)
    }
  } catch (error) {
    console.error('Gagal memuat riwayat pencarian:', error)
  }
}

const saveHistory = () => {
  try {
    localStorage.setItem('github-search-history', JSON.stringify(searchHistory.value))
  } catch (error) {
    console.error('Gagal menyimpan riwayat pencarian:', error)
  }
}

const addToHistory = async (username) => {
  try {
    // Cek apakah user sudah ada di history
    const existingIndex = searchHistory.value.findIndex(item => item.login === username)

    // Get user data untuk avatar
    const response = await fetch(`https://api.github.com/users/${username}`)
    if (response.ok) {
      const userData = await response.json()

      if (existingIndex !== -1) {
        // Update existing item
        searchHistory.value[existingIndex] = {
          ...userData,
          timestamp: Date.now()
        }
        // Move to top
        const item = searchHistory.value.splice(existingIndex, 1)[0]
        searchHistory.value.unshift(item)
      } else {
        // Add new item
        searchHistory.value.unshift({
          ...userData,
          timestamp: Date.now()
        })
      }

      // Limit history to 10 items
      searchHistory.value = searchHistory.value.slice(0, 10)
      saveHistory()
    }
  } catch (error) {
    console.error('Gagal menambah ke history:', error)
  }
}

const removeFromHistory = (index) => {
  searchHistory.value.splice(index, 1)
  saveHistory()
}

const clearHistory = () => {
  searchHistory.value = []
  saveHistory()
}

const searchAgain = (username) => {
  emit('search', username)
}

const formatTime = (timestamp) => {
  const date = new Date(timestamp)
  const now = new Date()
  const diff = now - date

  if (diff < 60000) { // Less than 1 minute
    return 'Baru saja'
  } else if (diff < 3600000) { // Less than 1 hour
    const minutes = Math.floor(diff / 60000)
    return `${minutes} menit lalu`
  } else if (diff < 86400000) { // Less than 1 day
    const hours = Math.floor(diff / 3600000)
    return `${hours} jam lalu`
  } else {
    const days = Math.floor(diff / 86400000)
    return `${days} hari lalu`
  }
}
</script>

<style scoped>
.search-history {
  background: white;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin-top: 2rem;
}

.history-header {
  display: flex;
  justify-content: between;
  align-items: center;
  margin-bottom: 1rem;
}

.history-header h3 {
  margin: 0;
  color: #2d3748;
  font-size: 1.25rem;
}

.clear-button {
  background: #fc8181;
  color: white;
  border: none;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: background-color 0.3s;
}

.clear-button:hover {
  background: #f56565;
}

.empty-history {
  text-align: center;
  color: #718096;
  padding: 2rem 0;
  font-style: italic;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.history-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem;
  background: #f7fafc;
  border-radius: 8px;
  transition: background-color 0.3s;
}

.history-item:hover {
  background: #edf2f7;
}

.history-info {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.history-avatar {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  object-fit: cover;
}

.history-details {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.history-username {
  font-weight: 600;
  color: #2d3748;
}

.history-time {
  font-size: 0.75rem;
  color: #718096;
}

.history-actions {
  display: flex;
  gap: 0.5rem;
}

.search-again-button {
  background: #4299e1;
  color: white;
  border: none;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: background-color 0.3s;
}

.search-again-button:hover {
  background: #3182ce;
}

.remove-button {
  background: #cbd5e0;
  color: #4a5568;
  border: none;
  padding: 0.25rem 0.5rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
  font-weight: bold;
  transition: background-color 0.3s;
}

.remove-button:hover {
  background: #a0aec0;
}

@media (max-width: 768px) {
  .history-item {
    flex-direction: column;
    align-items: stretch;
    gap: 0.75rem;
  }

  .history-actions {
    justify-content: center;
  }
}
</style>
```

---

## 8. Implementasi Debouncing

### 8.1. Buat Debounce Utility
Edit `src/utils/debounce.js`:

```javascript
/**
 * Utility function untuk debounce
 * @param {Function} func - Function yang akan di-debounce
 * @param {number} delay - Delay dalam milliseconds
 * @returns {Function} - Function yang sudah di-debounce
 */
export function debounce(func, delay) {
  let timeoutId

  return function (...args) {
    // Clear existing timeout
    if (timeoutId) {
      clearTimeout(timeoutId)
    }

    // Set new timeout
    timeoutId = setTimeout(() => {
      func.apply(this, args)
    }, delay)
  }
}

/**
 * Utility function untuk throttle
 * @param {Function} func - Function yang akan di-throttle
 * @param {number} delay - Delay dalam milliseconds
 * @returns {Function} - Function yang sudah di-throttle
 */
export function throttle(func, delay) {
  let lastCall = 0

  return function (...args) {
    const now = Date.now()

    if (now - lastCall >= delay) {
      lastCall = now
      func.apply(this, args)
    }
  }
}

/**
 * Async version of debounce
 * @param {Function} func - Async function yang akan di-debounce
 * @param {number} delay - Delay dalam milliseconds
 * @returns {Function} - Async function yang sudah di-debounce
 */
export function debounceAsync(func, delay) {
  let timeoutId
  let latestResolve
  let latestReject

  return function (...args) {
    return new Promise((resolve, reject) => {
      // Clear existing timeout
      if (timeoutId) {
        clearTimeout(timeoutId)
        // Reject previous promise
        if (latestReject) {
          latestReject(new Error('Debounced'))
        }
      }

      // Store latest resolve/reject
      latestResolve = resolve
      latestReject = reject

      // Set new timeout
      timeoutId = setTimeout(async () => {
        try {
          const result = await func.apply(this, args)
          latestResolve(result)
        } catch (error) {
          latestReject(error)
        }
      }, delay)
    })
  }
}
```

---

## 9. Membuat Composable untuk GitHub Search

### 9.1. Buat Composable
Edit `src/composables/useGithubSearch.js`:

```javascript
import { ref, computed } from 'vue'
import githubService from '../services/githubService'
import { debounceAsync } from '../utils/debounce'

export function useGithubSearch() {
  // State
  const searchQuery = ref('')
  const userProfile = ref(null)
  const userRepos = ref([])
  const userFollowers = ref([])
  const userFollowing = ref([])
  const loading = ref(false)
  const error = ref(null)
  const searchHistory = ref([])

  // Computed properties
  const hasResults = computed(() => !!userProfile.value)
  const hasError = computed(() => !!error.value)
  const isLoading = computed(() => loading.value)

  // Debounced search function
  const debouncedSearch = debounceAsync(async (username) => {
    if (!username || typeof username !== 'string') {
      throw new Error('Username harus berupa string yang valid')
    }

    loading.value = true
    error.value = null

    try {
      // Search user profile
      const [profile, repos, followers, following] = await Promise.allSettled([
        githubService.getUser(username),
        githubService.getUserRepos(username),
        githubService.getUserFollowers(username),
        githubService.getUserFollowing(username)
      ])

      // Handle results
      if (profile.status === 'fulfilled') {
        userProfile.value = profile.value
        addToSearchHistory(profile.value)
      } else {
        throw profile.reason
      }

      userRepos.value = repos.status === 'fulfilled' ? repos.value : []
      userFollowers.value = followers.status === 'fulfilled' ? followers.value : []
      userFollowing.value = following.status === 'fulfilled' ? following.value : []

    } catch (err) {
      error.value = err.message
      clearResults()
    } finally {
      loading.value = false
    }
  }, 500)

  // Search user
  const searchUser = async (username) => {
    if (!username || !username.trim()) {
      error.value = 'Username tidak boleh kosong'
      return
    }

    searchQuery.value = username.trim()

    try {
      await debouncedSearch(searchQuery.value)
    } catch (err) {
      // Handle debounced promise rejection
      if (err.message !== 'Debounced') {
        error.value = err.message
      }
    }
  }

  // Search multiple users
  const searchMultipleUsers = async (usernames) => {
    if (!Array.isArray(usernames) || usernames.length === 0) {
      error.value = 'Daftar username tidak valid'
      return
    }

    loading.value = true
    error.value = null

    try {
      const results = await Promise.allSettled(
        usernames.map(username => githubService.getUser(username))
      )

      const successful = results
        .filter(result => result.status === 'fulfilled')
        .map(result => result.value)

      const failed = results
        .filter(result => result.status === 'rejected')
        .map(result => result.reason.message)

      if (successful.length === 0) {
        throw new Error('Semua pencarian gagal')
      }

      userProfile.value = successful[0] // Show first successful result
      addToSearchHistory(userProfile.value)

      if (failed.length > 0) {
        error.value = `Beberapa pencarian gagal: ${failed.join(', ')}`
      }

    } catch (err) {
      error.value = err.message
      clearResults()
    } finally {
      loading.value = false
    }
  }

  // Clear results
  const clearResults = () => {
    userProfile.value = null
    userRepos.value = []
    userFollowers.value = []
    userFollowing.value = []
  }

  // Clear error
  const clearError = () => {
    error.value = null
  }

  // Reset search
  const resetSearch = () => {
    searchQuery.value = ''
    clearResults()
    clearError()
  }

  // Search history management
  const loadSearchHistory = () => {
    try {
      const history = localStorage.getItem('github-search-history')
      if (history) {
        searchHistory.value = JSON.parse(history)
      }
    } catch (err) {
      console.error('Gagal memuat riwayat pencarian:', err)
    }
  }

  const saveSearchHistory = () => {
    try {
      localStorage.setItem('github-search-history', JSON.stringify(searchHistory.value))
    } catch (err) {
      console.error('Gagal menyimpan riwayat pencarian:', err)
    }
  }

  const addToSearchHistory = (user) => {
    if (!user || !user.login) return

    // Remove existing entry
    const existingIndex = searchHistory.value.findIndex(item => item.login === user.login)
    if (existingIndex !== -1) {
      searchHistory.value.splice(existingIndex, 1)
    }

    // Add to beginning
    searchHistory.value.unshift({
      ...user,
      timestamp: Date.now()
    })

    // Limit to 10 items
    searchHistory.value = searchHistory.value.slice(0, 10)
    saveSearchHistory()
  }

  const removeFromSearchHistory = (index) => {
    searchHistory.value.splice(index, 1)
    saveSearchHistory()
  }

  const clearSearchHistory = () => {
    searchHistory.value = []
    saveSearchHistory()
  }

  // Initialize
  loadSearchHistory()

  return {
    // State
    searchQuery,
    userProfile,
    userRepos,
    userFollowers,
    userFollowing,
    loading,
    error,
    searchHistory,

    // Computed
    hasResults,
    hasError,
    isLoading,

    // Methods
    searchUser,
    searchMultipleUsers,
    clearResults,
    clearError,
    resetSearch,
    removeFromSearchHistory,
    clearSearchHistory
  }
}
```

---

## 10. Optimasi Performance

### 10.1. Implementasi Lazy Loading
Tambahkan lazy loading untuk data yang tidak perlu dimuat langsung:

```javascript
// Di dalam useGithubSearch.js
const loadRepos = async (username, page = 1) => {
  try {
    const repos = await githubService.getUserRepos(username, page)
    if (page === 1) {
      userRepos.value = repos
    } else {
      userRepos.value.push(...repos)
    }
  } catch (err) {
    console.error('Gagal memuat repositories:', err)
  }
}

const loadFollowers = async (username, page = 1) => {
  try {
    const followers = await githubService.getUserFollowers(username, page)
    if (page === 1) {
      userFollowers.value = followers
    } else {
      userFollowers.value.push(...followers)
    }
  } catch (err) {
    console.error('Gagal memuat followers:', err)
  }
}

const loadFollowing = async (username, page = 1) => {
  try {
    const following = await githubService.getUserFollowing(username, page)
    if (page === 1) {
      userFollowing.value = following
    } else {
      userFollowing.value.push(...following)
    }
  } catch (err) {
    console.error('Gagal memuat following:', err)
  }
}
```

### 10.2. Implementasi Infinite Scroll
Buat composable untuk infinite scroll:

```javascript
// src/composables/useInfiniteScroll.js
import { ref, onMounted, onUnmounted } from 'vue'

export function useInfiniteScroll(callback, options = {}) {
  const { threshold = 100, container = window } = options
  const isLoading = ref(false)
  const hasMore = ref(true)

  const handleScroll = () => {
    if (isLoading.value || !hasMore.value) return

    const scrollTop = container === window
      ? window.pageYOffset
      : container.scrollTop
    const scrollHeight = container === window
      ? document.documentElement.scrollHeight
      : container.scrollHeight
    const clientHeight = container === window
      ? window.innerHeight
      : container.clientHeight

    if (scrollTop + clientHeight >= scrollHeight - threshold) {
      loadMore()
    }
  }

  const loadMore = async () => {
    if (isLoading.value || !hasMore.value) return

    isLoading.value = true
    try {
      const result = await callback()
      hasMore.value = result.hasMore !== false
    } catch (error) {
      console.error('Error loading more:', error)
    } finally {
      isLoading.value = false
    }
  }

  onMounted(() => {
    container.addEventListener('scroll', handleScroll, { passive: true })
  })

  onUnmounted(() => {
    container.removeEventListener('scroll', handleScroll)
  })

  return {
    isLoading,
    hasMore,
    loadMore
  }
}
```

---

## 11. Error Handling Komprehensif

### 11.1. Global Error Handler
Buat error handler global:

```javascript
// src/utils/errorHandler.js
class ErrorHandler {
  constructor() {
    this.errors = []
  }

  handleError(error, context = '') {
    const errorInfo = {
      message: error.message,
      context,
      timestamp: new Date().toISOString(),
      stack: error.stack
    }

    this.errors.push(errorInfo)

    // Log error
    console.error(`[${context}] ${error.message}`, error)

    // Show user-friendly message
    return this.getUserFriendlyMessage(error)
  }

  getUserFriendlyMessage(error) {
    if (error.message.includes('NetworkError')) {
      return 'Koneksi internet gagal. Periksa koneksi Anda dan coba lagi.'
    } else if (error.message.includes('404')) {
      return 'User tidak ditemukan. Periksa username dan coba lagi.'
    } else if (error.message.includes('403')) {
      return 'Rate limit terlampaui. Silakan tunggu beberapa saat sebelum mencoba lagi.'
    } else if (error.message.includes('timeout')) {
      return 'Request timeout. Silakan coba lagi.'
    } else {
      return 'Terjadi kesalahan. Silakan coba lagi.'
    }
  }

  clearErrors() {
    this.errors = []
  }

  getRecentErrors() {
    return this.errors.slice(-10)
  }
}

export default new ErrorHandler()
```

### 11.2. Error Boundary Component
Buat error boundary component:

```vue
<!-- src/components/ErrorBoundary.vue -->
<template>
  <div v-if="hasError" class="error-boundary">
    <div class="error-content">
      <h2>🚫 Terjadi Kesalahan</h2>
      <p>{{ errorMessage }}</p>
      <div class="error-actions">
        <button @click="retry" class="retry-button">
          Coba Lagi
        </button>
        <button @click="reset" class="reset-button">
          Reset
        </button>
      </div>
      <details v-if="showDetails" class="error-details">
        <summary>Detail Error</summary>
        <pre>{{ errorDetails }}</pre>
      </details>
    </div>
  </div>
  <slot v-else />
</template>

<script setup>
import { ref, onErrorCaptured } from 'vue'
import errorHandler from '../utils/errorHandler'

const hasError = ref(false)
const errorMessage = ref('')
const errorDetails = ref('')
const showDetails = ref(false)

onErrorCaptured((error, instance, info) => {
  console.error('Error captured:', error, info)

  errorMessage.value = errorHandler.handleError(error, 'ErrorBoundary')
  errorDetails.value = error.stack || error.message
  hasError.value = true

  return false // Prevent error from propagating
})

const retry = () => {
  hasError.value = false
  errorMessage.value = ''
  errorDetails.value = ''
}

const reset = () => {
  retry()
  window.location.reload()
}
</script>

<style scoped>
.error-boundary {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 400px;
  padding: 2rem;
}

.error-content {
  text-align: center;
  max-width: 500px;
}

.error-content h2 {
  color: #e53e3e;
  margin-bottom: 1rem;
}

.error-content p {
  color: #4a5568;
  margin-bottom: 2rem;
  line-height: 1.6;
}

.error-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 2rem;
}

.retry-button, .reset-button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.3s;
}

.retry-button {
  background: #4299e1;
  color: white;
}

.retry-button:hover {
  background: #3182ce;
}

.reset-button {
  background: #e2e8f0;
  color: #4a5568;
}

.reset-button:hover {
  background: #cbd5e0;
}

.error-details {
  text-align: left;
  background: #f7fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem;
  margin-top: 1rem;
}

.error-details summary {
  cursor: pointer;
  font-weight: 600;
  color: #2d3748;
  margin-bottom: 1rem;
}

.error-details pre {
  font-size: 0.875rem;
  color: #4a5568;
  overflow-x: auto;
  white-space: pre-wrap;
}
</style>
```

---

## 12. Testing

### 12.1. Unit Testing dengan Vitest
Install dan setup Vitest:

```bash
npm install -D vitest @vue/test-utils jsdom
```

Konfigurasi `vitest.config.js`:

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

Buat test untuk service:

```javascript
// tests/services/githubService.test.js
import { describe, it, expect, vi, beforeEach } from 'vitest'
import githubService from '../../src/services/githubService'

describe('GithubService', () => {
  beforeEach(() => {
    // Clear cache before each test
    githubService.cache.clear()
  })

  describe('getUser', () => {
    it('should fetch user data successfully', async () => {
      const mockUser = {
        id: 1,
        login: 'testuser',
        name: 'Test User',
        avatar_url: 'https://example.com/avatar.jpg'
      }

      global.fetch = vi.fn().mockResolvedValueOnce({
        ok: true,
        json: () => Promise.resolve(mockUser)
      })

      const result = await githubService.getUser('testuser')

      expect(result).toEqual(mockUser)
      expect(fetch).toHaveBeenCalledWith('https://api.github.com/users/testuser')
    })

    it('should handle 404 error', async () => {
      global.fetch = vi.fn().mockResolvedValueOnce({
        ok: false,
        status: 404,
        json: () => Promise.resolve({ message: 'Not Found' })
      })

      await expect(githubService.getUser('nonexistentuser')).rejects.toThrow(
        'User "nonexistentuser" tidak ditemukan'
      )
    })

    it('should cache results', async () => {
      const mockUser = {
        id: 1,
        login: 'testuser',
        name: 'Test User'
      }

      global.fetch = vi.fn().mockResolvedValueOnce({
        ok: true,
        json: () => Promise.resolve(mockUser)
      })

      // First call
      const result1 = await githubService.getUser('testuser')
      expect(fetch).toHaveBeenCalledTimes(1)

      // Second call should use cache
      const result2 = await githubService.getUser('testuser')
      expect(fetch).toHaveBeenCalledTimes(1) // Still called only once
      expect(result1).toEqual(result2)
    })
  })
})
```

### 12.2. Component Testing
Test untuk component:

```javascript
// tests/components/SearchHistory.test.js
import { describe, it, expect, beforeEach } from 'vitest'
import { mount } from '@vue/test-utils'
import SearchHistory from '../../src/components/SearchHistory.vue'

describe('SearchHistory', () => {
  let wrapper

  beforeEach(() => {
    wrapper = mount(SearchHistory, {
      props: {
        currentSearch: ''
      }
    })
  })

  it('renders empty state when no history', () => {
    expect(wrapper.find('.empty-history').exists()).toBe(true)
    expect(wrapper.text()).toContain('Belum ada riwayat pencarian')
  })

  it('emits search event when search again button clicked', async () => {
    const historyData = [
      {
        login: 'testuser',
        avatar_url: 'https://example.com/avatar.jpg',
        timestamp: Date.now()
      }
    ]

    await wrapper.setData({ searchHistory: historyData })

    await wrapper.find('.search-again-button').trigger('click')

    expect(wrapper.emitted('search')).toBeTruthy()
    expect(wrapper.emitted('search')[0]).toEqual(['testuser'])
  })
})
```

---

## 13. Deployment

### 13.1. Build untuk Production
```bash
# Build project
npm run build

# Preview build
npm run preview
```

### 13.2. Deploy ke Netlify
1. Push code ke GitHub
2. Connect repository ke Netlify
3. Set build command: `npm run build`
4. Set publish directory: `dist`

### 13.3. Deploy ke Vercel
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

### 13.4. Environment Variables
Buat `.env.production`:

```
VITE_GITHUB_TOKEN=your_github_token_here
VITE_API_BASE_URL=https://api.github.com
```

---

## 14. Best Practices dan Tips

### 14.1. Performance Tips
- Gunakan debouncing untuk search input
- Implementasi cache untuk API responses
- Lazy loading untuk data yang tidak perlu
- Gunakan pagination untuk data besar
- Optimalkan image loading

### 14.2. Security Tips
- Validasi input user
- Handle API keys dengan aman
- Implementasi rate limiting
- Sanitize data dari API

### 14.3. User Experience Tips
- Loading states yang jelas
- Error messages yang user-friendly
- Responsive design
- Keyboard navigation
- Accessibility features

### 14.4. Code Organization Tips
- Gunakan composables untuk reusable logic
- Pisahkan business logic dari components
- Konsistent naming conventions
- Comments untuk complex logic
- Component composition over inheritance

---

## 15. Troubleshooting

### 15.1. Common Issues

**Issue: API Rate Limit**
```
Error: Rate limit terlampaui
Solution: Tunggu beberapa saat atau gunakan GitHub token
```

**Issue: CORS Error**
```
Error: CORS policy error
Solution: Gunakan proxy server atau GitHub token
```

**Issue: Network Error**
```
Error: Network connection failed
Solution: Periksa koneksi internet
```

### 15.2. Debug Tips
- Gunakan Vue DevTools
- Console logging untuk debugging
- Network tab untuk API requests
- Error boundary untuk error handling

---

## 16. Kesimpulan

Setelah mengikuti panduan ini, Anda telah berhasil membuat aplikasi GitHub Users Search yang lengkap dengan:

- ✅ Basic search functionality
- ✅ User profile display
- ✅ Search history
- ✅ Debouncing
- ✅ Error handling
- ✅ Caching
- ✅ Responsive design
- ✅ Loading states
- ✅ Unit testing
- ✅ Deployment ready

### Next Steps
- Tambahkan fitur advanced filtering
- Implementasi real-time updates
- Tambahkan social features
- Implementasi analytics
- Tambahkan A/B testing

### Resources
- [Vue.js Documentation](https://vuejs.org/)
- [GitHub API Documentation](https://docs.github.com/en/rest)
- [Vite Documentation](https://vitejs.dev/)
- [Vitest Documentation](https://vitest.dev/)

---

**Selamat! 🎉** Anda telah menyelesaikan implementasi lengkap aplikasi GitHub Users Search dengan Vue.js 3!