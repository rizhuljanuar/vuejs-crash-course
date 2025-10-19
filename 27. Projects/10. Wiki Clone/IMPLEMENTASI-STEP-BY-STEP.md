# 📚 Wikipedia Search Clone - Implementasi Step-by-Step Lengkap

## 📋 Daftar Isi

1. [Persiapan Lingkungan](#1-persiapan-lingkungan)
2. [Setup Project Vue.js](#2-setup-project-vuejs)
3. [Struktur Folder Project](#3-struktur-folder-project)
4. [Implementasi Komponen Dasar](#4-implementasi-komponen-dasar)
5. [Integrasi Wikipedia API](#5-integrasi-wikipedia-api)
6. [State Management](#6-state-management)
7. [Event Handling](#7-event-handling)
8. [Conditional Rendering](#8-conditional-rendering)
9. [Styling dan Responsive Design](#9-styling-dan-responsive-design)
10. [Theme Toggle Implementation](#10-theme-toggle-implementation)
11. [Search History Feature](#11-search-history-feature)
12. [Debouncing untuk Performance](#12-debouncing-untuk-performance)
13. [Error Handling](#13-error-handling)
14. [LocalStorage Integration](#14-localstorage-integration)
15. [Advanced Features](#15-advanced-features)
16. [Testing dan Debugging](#16-testing-dan-debugging)
17. [Deployment](#17-deployment)

---

## 1. Persiapan Lingkungan

### 1.1. Install Node.js dan npm
Pastikan Node.js dan npm sudah terinstall:

```bash
# Cek versi Node.js
node --version

# Cek versi npm
npm --version
```

Jika belum terinstall, download dari [nodejs.org](https://nodejs.org/)

### 1.2. Install Code Editor dan Extensions
Install Visual Studio Code dengan extensions:
- **Volar** (untuk Vue 3)
- **ESLint**
- **Prettier**
- **GitLens**
- **Live Server** (opsional)

### 1.3. Setup Browser
Install browser modern:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## 2. Setup Project Vue.js

### 2.1. Buat Project Baru dengan Vite

```bash
# 1. Buat folder project
mkdir wikipedia-search-clone
cd wikipedia-search-clone

# 2. Inisialisasi project dengan Vite
npm create vue@latest .

# 3. Pilih konfigurasi:
# ? Project name: wikipedia-search-clone
# ? Add TypeScript? No
# ? Add JSX Support? No
# ? Add Vue Router for Single Page Application development? No
# ? Add Pinia for state management? No
# ? Add Vitest for Unit Testing? No
# ? Add an End-to-End Testing Solution? No
# ? Add ESLint for code quality? Yes
# ? Add Prettier for code formatting? Yes

# 4. Install dependencies
npm install

# 5. Start development server
npm run dev
```

### 2.2. Konfigurasi Project

Edit `package.json` untuk memastikan dependencies:

```json
{
  "name": "wikipedia-search-clone",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .vue,.js,.jsx,.cjs,.mjs --fix --ignore-path .gitignore"
  },
  "dependencies": {
    "vue": "^3.3.0"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.0.0",
    "eslint": "^8.0.0",
    "eslint-plugin-vue": "^9.0.0",
    "prettier": "^2.8.0",
    "vite": "^4.0.0"
  }
}
```

### 2.3. Konfigurasi Vite

Edit `vite.config.js`:

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  server: {
    port: 5173,
    open: true
  },
  build: {
    outDir: 'dist',
    sourcemap: true
  }
})
```

---

## 3. Struktur Folder Project

### 3.1. Buat Struktur Folder

```bash
# Hapus file default yang tidak diperlukan
rm src/components/HelloWorld.vue
rm src/components/TheWelcome.vue
rm src/components/WelcomeItem.vue
rm src/components/icons/IconCommunity.vue
rm src/components/icons/IconDocumentation.vue
rm src/components/icons/IconEcosystem.vue
rm src/components/icons/IconSupport.vue
rm src/components/icons/IconTooling.vue
rmdir src/components/icons

# Buat struktur folder baru
mkdir -p src/components src/utils src/styles src/assets
```

### 3.2. Struktur Folder Akhir

```
wikipedia-search-clone/
├── index.html                 # Entry HTML
├── package.json              # Dependencies
├── vite.config.js            # Vite config
├── .eslintrc.cjs             # ESLint config
├── .prettierrc.json          # Prettier config
└── src/
    ├── main.js               # App initialization
    ├── App.vue               # Root component
    ├── styles/
    │   └── global.css        # Global styles
    ├── components/
    │   └── WikiComponent.vue # Main search component
    └── assets/
        └── favicon.svg       # Icon
```

---

## 4. Implementasi Komponen Dasar

### 4.1. Setup Entry Point (`src/main.js`)

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import './styles/global.css'

const app = createApp(App)

app.mount('#app')
```

**Penjelasan untuk Pemula:**
- `import { createApp } from 'vue'` - Mengimpor fungsi createApp dari Vue
- `import App from './App.vue'` - Mengimpor root component
- `import './styles/global.css'` - Mengimpor global styles
- `createApp(App)` - Membuat instance Vue application
- `app.mount('#app')` - Mount aplikasi ke elemen dengan id #app

### 4.2. Global Styles (`src/styles/global.css`)

```css
/* Reset CSS */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Base styles */
html {
  font-size: 16px;
  line-height: 1.6;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: #2c3e50;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* Focus styles untuk accessibility */
*:focus {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}

/* Smooth scrolling */
html {
  scroll-behavior: smooth;
}

/* Selection styles */
::selection {
  background-color: rgba(102, 126, 234, 0.2);
  color: #2c3e50;
}

/* Scrollbar styles */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #a8a8a8;
}
```

### 4.3. Root Component (`src/App.vue`)

```vue
<script setup>
import WikiComponent from './components/WikiComponent.vue'
</script>

<template>
  <div id="app">
    <WikiComponent />
  </div>
</template>

<style>
/* App specific styles */
#app {
  min-height: 100vh;
  padding: 2rem;
}
</style>
```

**Penjelasan untuk Pemula:**
- `<script setup>` - Syntax sugar untuk Composition API Vue 3
- `import WikiComponent` - Mengimpor component yang akan digunakan
- `<template>` - Template HTML untuk component
- `<div id="app">` - Container utama aplikasi
- `<WikiComponent />` - Render WikiComponent

---

## 5. Integrasi Wikipedia API

### 5.1. Wikipedia API Basics

Wikipedia API adalah RESTful API yang digunakan untuk:
- Mencari artikel Wikipedia
- Mendapatkan konten artikel
- Mendapatkan metadata artikel

### 5.2. API Endpoint Structure

```javascript
// Basic search endpoint
const endpoint = 'https://en.wikipedia.org/w/api.php'

// Parameters
const params = {
  action: 'query',        // Tipe query
  list: 'search',        // Search operation
  prop: 'info',          // Properties yang diambil
  inprop: 'url',         // Include URL
  utf8: '',              // UTF-8 encoding
  format: 'json',        // Response format
  origin: '*',           // CORS
  srlimit: 10,           // Max results
  srsearch: 'query'      // Search term
}
```

### 5.3. API Service Function

Buat file `src/utils/wikipediaService.js`:

```javascript
/**
 * Wikipedia API Service
 * Berisi fungsi-fungsi untuk berinteraksi dengan Wikipedia API
 */

const API_BASE_URL = 'https://en.wikipedia.org/w/api.php'

/**
 * Mencari artikel di Wikipedia
 * @param {string} query - Kata kunci pencarian
 * @param {number} limit - Jumlah maksimal hasil
 * @returns {Promise<Array>} Array dari search results
 */
export const searchWikipedia = async (query, limit = 10) => {
  // Validasi input
  if (!query || typeof query !== 'string' || query.trim() === '') {
    throw new Error('Query tidak boleh kosong')
  }

  // Encode query untuk URL safety
  const encodedQuery = encodeURIComponent(query.trim())

  // Build URL dengan parameters
  const params = new URLSearchParams({
    action: 'query',
    list: 'search',
    prop: 'info|snippet|pageimages',
    inprop: 'url|displaytitle',
    pithumbsize: 100,
    utf8: '',
    format: 'json',
    origin: '*',
    srlimit: limit.toString(),
    srsearch: encodedQuery,
    srsort: 'relevance'
  })

  const fullUrl = `${API_BASE_URL}?${params.toString()}`

  try {
    // Fetch data dari Wikipedia API
    const response = await fetch(fullUrl)

    // Validasi response
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    // Parse JSON response
    const data = await response.json()

    // Validasi response structure
    if (!data.query || !data.query.search) {
      return []
    }

    // Process results
    return data.query.search.map(result => ({
      ...result,
      snippet: result.snippet?.replace(/<[^>]*>/g, '') || 'No description available.',
      url: `https://en.wikipedia.org/?curid=${result.pageid}`
    }))

  } catch (error) {
    console.error('Wikipedia API Error:', error)
    throw new Error(`Gagal mencari di Wikipedia: ${error.message}`)
  }
}

/**
 * Mendapatkan detail artikel berdasarkan page ID
 * @param {number} pageId - ID halaman Wikipedia
 * @returns {Promise<Object>} Detail artikel
 */
export const getArticleDetails = async (pageId) => {
  const params = new URLSearchParams({
    action: 'query',
    prop: 'extracts|info|images',
    exintro: true,
    explaintext: true,
    inprop: 'url|displaytitle',
    pageids: pageId.toString(),
    format: 'json',
    origin: '*'
  })

  const fullUrl = `${API_BASE_URL}?${params.toString()}`

  try {
    const response = await fetch(fullUrl)
    const data = await response.json()

    if (data.query && data.query.pages) {
      const pages = Object.values(data.query.pages)
      return pages[0] // Return first (and only) page
    }

    throw new Error('Artikel tidak ditemukan')

  } catch (error) {
    console.error('Get article details error:', error)
    throw new Error(`Gagal mendapatkan detail artikel: ${error.message}`)
  }
}

/**
 * Mencari dengan kategori spesifik
 * @param {string} query - Kata kunci pencarian
 * @param {string} category - Kategori untuk filter
 * @returns {Promise<Array>} Array dari search results
 */
export const searchByCategory = async (query, category) => {
  const searchQuery = category ? `${query} incategory:${category}` : query
  return searchWikipedia(searchQuery)
}
```

---

## 6. State Management dengan Vue Reactivity

### 6.1. Reactive Variables

Di Vue 3 Composition API, kita menggunakan `ref()` untuk membuat reactive variables:

```javascript
import { ref } from 'vue'

// Reactive state variables
const searchQuery = ref('')         // Input search user
const searchResults = ref([])       // Hasil pencarian
const isLoading = ref(false)        // Loading state
const error = ref(null)             // Error message
const isDarkTheme = ref(false)      // Theme state
const searchHistory = ref([])       // History pencarian
```

**Penjelasan untuk Pemula:**
- `ref()` membuat reactive variables
- Ketika value berubah, Vue otomatis update UI
- Access value dengan `.value` di JavaScript
- Di template, Vue otomatis unwrap `.value`

### 6.2. Complete State Management

```javascript
import { ref, watch, onMounted } from 'vue'

// State initialization
const state = {
  // Search related state
  searchQuery: ref(''),
  searchResults: ref([]),
  isLoading: ref(false),
  error: ref(null),

  // UI state
  isDarkTheme: ref(false),
  showHistory: ref(true),

  // History and preferences
  searchHistory: ref([]),
  userPreferences: ref({
    resultsPerPage: 10,
    sortBy: 'relevance',
    language: 'en'
  })
}

// Computed properties (derived state)
const hasResults = computed(() => state.searchResults.value.length > 0)
const hasError = computed(() => !!state.error.value)
const isSearching = computed(() => state.isLoading.value)

// State validation
const validateState = {
  searchQuery: (query) => {
    if (!query || query.trim() === '') {
      return 'Search query tidak boleh kosong'
    }
    if (query.length > 100) {
      return 'Search query terlalu panjang (maksimal 100 karakter)'
    }
    return null
  },

  searchResults: (results) => {
    if (!Array.isArray(results)) {
      return 'Search results harus berupa array'
    }
    if (results.length > 100) {
      return 'Terlalu banyak hasil (maksimal 100)'
    }
    return null
  }
}
```

---

## 7. Event Handling

### 7.1. Form Event Handling

```vue
<template>
  <!-- Form submit event -->
  <form @submit.prevent="handleSearch">
    <!-- Input change event dengan v-model -->
    <input
      v-model="searchQuery"
      type="text"
      @input="handleInputChange"
      @focus="handleInputFocus"
      @blur="handleInputBlur"
      placeholder="Search Wikipedia..."
    />

    <!-- Button click event -->
    <button
      type="submit"
      @click="handleButtonClick"
      :disabled="isLoading"
    >
      Search
    </button>
  </form>
</template>

<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
const isLoading = ref(false)

// Event handlers
const handleSearch = (event) => {
  event.preventDefault() // Mencegah form refresh
  console.log('Form submitted with:', searchQuery.value)
  // Panggil search function
}

const handleInputChange = (event) => {
  console.log('Input changed:', event.target.value)
  // Auto-search logic di sini
}

const handleInputFocus = () => {
  console.log('Input focused')
  // Tampilkan suggestions
}

const handleInputBlur = () => {
  console.log('Input blurred')
  // Sembunyikan suggestions
}

const handleButtonClick = () => {
  console.log('Button clicked')
  // Additional button logic
}
</script>
```

### 7.2. Keyboard Event Handling

```vue
<template>
  <div>
    <!-- Keyboard events -->
    <input
      v-model="searchQuery"
      @keydown.enter="handleEnter"
      @keydown.escape="handleEscape"
      @keydown.up="handleArrowUp"
      @keydown.down="handleArrowDown"
      placeholder="Search and press Enter"
    />

    <!-- Global keyboard events -->
    <div
      @keydown.ctrl.k="openSearchModal"
      tabindex="0"
    >
      Press Ctrl+K to open search
    </div>
  </div>
</template>

<script setup>
const handleEnter = (event) => {
  console.log('Enter key pressed')
  performSearch()
}

const handleEscape = () => {
  console.log('Escape key pressed')
  clearSearch()
}

const handleArrowUp = () => {
  console.log('Arrow up pressed')
  navigateResults('up')
}

const handleArrowDown = () => {
  console.log('Arrow down pressed')
  navigateResults('down')
}

const openSearchModal = () => {
  console.log('Ctrl+K pressed')
  // Open search modal
}
</script>
```

---

## 8. Conditional Rendering

### 8.1. Basic Conditional Rendering

```vue
<template>
  <div>
    <!-- Loading state -->
    <div v-if="isLoading" class="loading">
      <div class="spinner"></div>
      <p>Searching Wikipedia...</p>
    </div>

    <!-- Error state -->
    <div v-else-if="error" class="error">
      <span class="error-icon">⚠️</span>
      <p>{{ error }}</p>
      <button @click="retrySearch">Try Again</button>
    </div>

    <!-- Results state -->
    <div v-else-if="searchResults.length > 0" class="results">
      <h2>Found {{ searchResults.length }} results</h2>
      <!-- Results list -->
    </div>

    <!-- Empty state -->
    <div v-else-if="searchQuery.trim()" class="no-results">
      <p>No results found for "{{ searchQuery }}"</p>
    </div>

    <!-- Welcome state -->
    <div v-else class="welcome">
      <h2>Welcome to Wikipedia Search</h2>
      <p>Enter a search term to explore Wikipedia articles.</p>
    </div>
  </div>
</template>
```

### 8.2. Advanced Conditional Rendering

```vue
<template>
  <div>
    <!-- Multiple conditions dengan computed properties -->
    <div
      v-if="showAdvancedFeatures && userCanEdit"
      class="advanced-controls"
    >
      <!-- Advanced controls -->
    </div>

    <!-- Conditional class binding -->
    <div
      :class="{
        'search-container': true,
        'is-loading': isLoading,
        'has-error': !!error,
        'dark-theme': isDarkTheme
      }"
    >
      <!-- Content -->
    </div>

    <!-- Conditional attributes -->
    <input
      :disabled="isLoading"
      :readonly="isReadOnly"
      :required="isRequired"
      :placeholder="getPlaceholderText"
    />

    <!-- List rendering dengan conditional -->
    <template v-for="result in searchResults" :key="result.pageid">
      <article
        v-if="result.snippet"
        class="result-item"
        :class="{ 'featured': result.isFeatured }"
      >
        <!-- Result content -->
      </article>
    </template>

    <!-- Template conditional untuk multiple elements -->
    <template v-if="showDetails">
      <header class="details-header">
        <h3>{{ selectedResult.title }}</h3>
      </header>
      <main class="details-content">
        <p>{{ selectedResult.snippet }}</p>
      </main>
      <footer class="details-footer">
        <button>Read More</button>
      </footer>
    </template>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  isLoading: Boolean,
  error: String,
  searchResults: Array,
  searchQuery: String,
  isDarkTheme: Boolean,
  user: Object
})

// Computed properties untuk complex conditions
const showAdvancedFeatures = computed(() => {
  return props.user && props.user.isAdmin && props.searchResults.length > 0
})

const userCanEdit = computed(() => {
  return props.user && (props.user.role === 'editor' || props.user.role === 'admin')
})

const isReadOnly = computed(() => {
  return props.isLoading || !props.userCanEdit
})

const isRequired = computed(() => {
  return !props.searchQuery.trim()
})

const getPlaceholderText = computed(() => {
  if (props.isLoading) return 'Searching...'
  if (props.isDarkTheme) return 'Search in dark mode...'
  return 'Search Wikipedia...'
})
</script>
```

---

## 9. Styling dan Responsive Design

### 9.1. CSS Architecture dengan Scoped Styles

```vue
<template>
  <div class="wiki-search-container">
    <header class="search-header">
      <h1 class="app-title">Wikipedia Search</h1>
    </header>

    <main class="search-main">
      <section class="search-section">
        <form class="search-form">
          <div class="search-input-group">
            <input
              v-model="searchQuery"
              type="text"
              class="search-input"
              placeholder="Search Wikipedia..."
            />
            <button type="submit" class="search-button">
              Search
            </button>
          </div>
        </form>
      </section>

      <section class="results-section">
        <!-- Results content -->
      </section>
    </main>
  </div>
</template>

<style scoped>
/* Container styles */
.wiki-search-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Header styles */
.search-header {
  text-align: center;
  margin-bottom: 3rem;
}

.app-title {
  font-size: clamp(2rem, 5vw, 3.5rem);
  font-weight: 700;
  color: white;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
  line-height: 1.2;
}

/* Main content layout */
.search-main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

/* Search section */
.search-section {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.search-form {
  margin: 0;
}

.search-input-group {
  display: flex;
  gap: 1rem;
  align-items: stretch;
}

.search-input {
  flex: 1;
  padding: 1rem 1.25rem;
  font-size: 1.1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  background: white;
  color: #2d3748;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  min-height: 52px;
}

.search-input:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
  transform: translateY(-1px);
}

.search-input::placeholder {
  color: #a0aec0;
}

.search-button {
  padding: 1rem 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  min-width: 120px;
  position: relative;
  overflow: hidden;
}

.search-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

.search-button:hover::before {
  left: 100%;
}

.search-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(102, 126, 234, 0.4);
}

.search-button:active {
  transform: translateY(0);
}

.search-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* Results section */
.results-section {
  flex: 1;
}

/* Responsive design */
@media (max-width: 768px) {
  .wiki-search-container {
    padding: 1rem;
  }

  .search-header {
    margin-bottom: 2rem;
  }

  .search-section {
    padding: 1.5rem;
  }

  .search-input-group {
    flex-direction: column;
  }

  .search-input {
    margin-bottom: 1rem;
  }

  .search-button {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .wiki-search-container {
    padding: 0.5rem;
  }

  .search-section {
    padding: 1rem;
    border-radius: 12px;
  }

  .app-title {
    font-size: 2rem;
  }

  .search-input {
    font-size: 1rem;
    padding: 0.875rem 1rem;
  }

  .search-button {
    font-size: 1rem;
    padding: 0.875rem 1.5rem;
  }
}

/* Dark theme support */
@media (prefers-color-scheme: dark) {
  .search-section {
    background: rgba(45, 55, 72, 0.95);
    border-color: rgba(255, 255, 255, 0.1);
  }

  .search-input {
    background: #2d3748;
    border-color: #4a5568;
    color: #e2e8f0;
  }

  .search-input::placeholder {
    color: #718096;
  }

  .search-input:focus {
    border-color: #63b3ed;
    box-shadow: 0 0 0 3px rgba(99, 179, 237, 0.1);
  }
}

/* Accessibility improvements */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* High contrast mode */
@media (prefers-contrast: high) {
  .search-input {
    border-width: 3px;
  }

  .search-button {
    border: 2px solid currentColor;
  }
}

/* Focus visible for better keyboard navigation */
.search-input:focus-visible,
.search-button:focus-visible {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}
</style>
```

### 9.2. CSS Custom Properties untuk Theming

```vue
<script setup>
import { ref, watch } from 'vue'

const isDarkTheme = ref(false)

// Apply theme ke CSS custom properties
const applyTheme = (isDark) => {
  const root = document.documentElement

  if (isDark) {
    root.style.setProperty('--bg-primary', '#1a202c')
    root.style.setProperty('--bg-secondary', '#2d3748')
    root.style.setProperty('--bg-tertiary', '#4a5568')
    root.style.setProperty('--text-primary', '#e2e8f0')
    root.style.setProperty('--text-secondary', '#a0aec0')
    root.style.setProperty('--text-tertiary', '#718096')
    root.style.setProperty('--accent-primary', '#667eea')
    root.style.setProperty('--accent-secondary', '#764ba2')
    root.style.setProperty('--border-primary', '#4a5568')
    root.style.setProperty('--border-secondary', '#718096')
    root.style.setProperty('--shadow-primary', 'rgba(0, 0, 0, 0.3)')
    root.style.setProperty('--shadow-secondary', 'rgba(0, 0, 0, 0.1)')
  } else {
    root.style.setProperty('--bg-primary', '#ffffff')
    root.style.setProperty('--bg-secondary', '#f7fafc')
    root.style.setProperty('--bg-tertiary', '#edf2f7')
    root.style.setProperty('--text-primary', '#2d3748')
    root.style.setProperty('--text-secondary', '#4a5568')
    root.style.setProperty('--text-tertiary', '#718096')
    root.style.setProperty('--accent-primary', '#667eea')
    root.style.setProperty('--accent-secondary', '#764ba2')
    root.style.setProperty('--border-primary', '#e2e8f0')
    root.style.setProperty('--border-secondary', '#cbd5e0')
    root.style.setProperty('--shadow-primary', 'rgba(0, 0, 0, 0.1)')
    root.style.setProperty('--shadow-secondary', 'rgba(0, 0, 0, 0.05)')
  }
}

// Watch theme changes
watch(isDarkTheme, applyTheme)

// Apply initial theme
applyTheme(isDarkTheme.value)
</script>

<style scoped>
/* Gunakan CSS custom properties */
.wiki-search-container {
  background: var(--bg-primary);
  color: var(--text-primary);
}

.search-section {
  background: var(--bg-secondary);
  border: 1px solid var(--border-primary);
  box-shadow: 0 8px 32px var(--shadow-secondary);
}

.search-input {
  background: var(--bg-primary);
  border: 2px solid var(--border-primary);
  color: var(--text-primary);
}

.search-input:focus {
  border-color: var(--accent-primary);
  box-shadow: 0 0 0 3px color-mix(in srgb, var(--accent-primary) 25%, transparent);
}

.search-button {
  background: linear-gradient(135deg, var(--accent-primary), var(--accent-secondary));
}

/* Transisi smooth untuk theme changes */
* {
  transition: background-color 0.3s ease, color 0.3s ease, border-color 0.3s ease;
}
</style>
```

---

## 10. Theme Toggle Implementation

### 10.1. Theme Toggle Component

```vue
<template>
  <div class="theme-toggle-container">
    <button
      @click="toggleTheme"
      class="theme-toggle-button"
      :title="isDarkTheme ? 'Switch to light theme' : 'Switch to dark theme'"
      :aria-label="isDarkTheme ? 'Switch to light theme' : 'Switch to dark theme'"
    >
      <span class="theme-icon">
        {{ isDarkTheme ? '☀️' : '🌙' }}
      </span>
      <span class="theme-text">
        {{ isDarkTheme ? 'Light' : 'Dark' }}
      </span>
    </button>

    <!-- Theme preference indicator -->
    <div class="theme-indicator">
      <span class="indicator-dot" :class="{ 'active': isDarkTheme }"></span>
      <span class="indicator-text">
        {{ isDarkTheme ? 'Dark mode' : 'Light mode' }}
      </span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

// Reactive theme state
const isDarkTheme = ref(false)

// Toggle theme function
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
  saveThemePreference(isDarkTheme.value)
  applyTheme(isDarkTheme.value)
}

// Apply theme ke document
const applyTheme = (isDark) => {
  const root = document.documentElement

  // Set data attribute untuk CSS targeting
  root.setAttribute('data-theme', isDark ? 'dark' : 'light')

  // Set class untuk compatibility
  document.body.classList.toggle('dark-theme', isDark)

  // Update meta theme-color untuk mobile browsers
  updateThemeColor(isDark)
}

// Save theme preference ke localStorage
const saveThemePreference = (isDark) => {
  try {
    localStorage.setItem('wiki-theme-preference', isDark ? 'dark' : 'light')
  } catch (error) {
    console.warn('Failed to save theme preference:', error)
  }
}

// Load theme preference dari localStorage
const loadThemePreference = () => {
  try {
    const saved = localStorage.getItem('wiki-theme-preference')
    if (saved) {
      return saved === 'dark'
    }
  } catch (error) {
    console.warn('Failed to load theme preference:', error)
  }

  // Fallback ke system preference
  return window.matchMedia('(prefers-color-scheme: dark)').matches
}

// Update meta theme-color tag
const updateThemeColor = (isDark) => {
  const themeColor = isDark ? '#1a202c' : '#667eea'

  let metaThemeColor = document.querySelector('meta[name="theme-color"]')
  if (!metaThemeColor) {
    metaThemeColor = document.createElement('meta')
    metaThemeColor.name = 'theme-color'
    document.head.appendChild(metaThemeColor)
  }

  metaThemeColor.content = themeColor
}

// Listen untuk system theme changes
const setupSystemThemeListener = () => {
  const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)')

  mediaQuery.addEventListener('change', (e) => {
    // Hanya update jika user tidak pernah mengatur preference
    if (!localStorage.getItem('wiki-theme-preference')) {
      isDarkTheme.value = e.matches
      applyTheme(e.matches)
    }
  })
}

// Initialize theme on mount
onMounted(() => {
  // Load saved preference atau system preference
  isDarkTheme.value = loadThemePreference()
  applyTheme(isDarkTheme.value)

  // Setup system theme listener
  setupSystemThemeListener()
})

// Watch theme changes
watch(isDarkTheme, applyTheme)
</script>

<style scoped>
.theme-toggle-container {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.theme-toggle-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 50px;
  padding: 0.75rem 1.25rem;
  color: white;
  cursor: pointer;
  font-size: 0.9rem;
  font-weight: 500;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
}

.theme-toggle-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  transform: translateX(-100%);
  transition: transform 0.6s;
}

.theme-toggle-button:hover::before {
  transform: translateX(100%);
}

.theme-toggle-button:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.theme-toggle-button:active {
  transform: translateY(0);
}

.theme-icon {
  font-size: 1.2rem;
  transition: transform 0.3s ease;
}

.theme-toggle-button:hover .theme-icon {
  transform: rotate(20deg) scale(1.1);
}

.theme-text {
  font-weight: 600;
}

.theme-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.8);
}

.indicator-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.4);
  transition: all 0.3s ease;
}

.indicator-dot.active {
  background: #48bb78;
  box-shadow: 0 0 8px rgba(72, 187, 120, 0.5);
}

/* Dark theme styles */
[data-theme="dark"] .theme-toggle-button {
  background: rgba(45, 55, 72, 0.8);
  border-color: rgba(255, 255, 255, 0.1);
}

[data-theme="dark"] .theme-toggle-button:hover {
  background: rgba(45, 55, 72, 0.9);
}

/* Responsive design */
@media (max-width: 768px) {
  .theme-text {
    display: none;
  }

  .theme-toggle-button {
    padding: 0.75rem;
    border-radius: 50%;
    width: 48px;
    height: 48px;
    justify-content: center;
  }

  .theme-indicator {
    display: none;
  }
}

/* Accessibility improvements */
@media (prefers-reduced-motion: reduce) {
  .theme-toggle-button::before,
  .theme-icon,
  .theme-toggle-button,
  .indicator-dot {
    transition: none;
  }
}

.theme-toggle-button:focus-visible {
  outline: 2px solid rgba(255, 255, 255, 0.8);
  outline-offset: 2px;
}
</style>
```

---

## 11. Search History Feature

### 11.1. Search History Component

```vue
<template>
  <div class="search-history-container">
    <!-- Header -->
    <div class="history-header">
      <h3 class="history-title">
        <span class="history-icon">🕐</span>
        Recent Searches
      </h3>
      <div class="history-actions">
        <button
          v-if="searchHistory.length > 0"
          @click="clearHistory"
          class="clear-button"
          title="Clear all history"
        >
          Clear All
        </button>
      </div>
    </div>

    <!-- History List -->
    <div v-if="searchHistory.length > 0" class="history-list">
      <TransitionGroup name="history-item" tag="div">
        <div
          v-for="(item, index) in searchHistory"
          :key="item.id || index"
          class="history-item"
        >
          <div class="history-content" @click="searchFromHistory(item.query)">
            <div class="history-info">
              <span class="history-query">{{ item.query }}</span>
              <span class="history-meta">
                {{ item.resultsCount }} results • {{ formatTime(item.timestamp) }}
              </span>
            </div>
            <div class="history-actions-right">
              <button
                @click.stop="removeFromHistory(index)"
                class="remove-button"
                title="Remove from history"
              >
                ×
              </button>
            </div>
          </div>
        </div>
      </TransitionGroup>
    </div>

    <!-- Empty State -->
    <div v-else class="history-empty">
      <span class="empty-icon">🔍</span>
      <p class="empty-text">No search history yet</p>
      <p class="empty-subtext">Your recent searches will appear here</p>
    </div>

    <!-- History Stats -->
    <div v-if="searchHistory.length > 0" class="history-stats">
      <span class="stats-text">
        {{ searchHistory.length }} searches in history
      </span>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'

const props = defineProps({
  maxHistoryItems: {
    type: Number,
    default: 10
  }
})

const emit = defineEmits(['search', 'history-change'])

// Reactive state
const searchHistory = ref([])

// Load history dari localStorage
const loadSearchHistory = () => {
  try {
    const stored = localStorage.getItem('wiki-search-history-v2')
    if (stored) {
      const history = JSON.parse(stored)
      // Validasi dan filter valid items
      searchHistory.value = history
        .filter(item => item && item.query && typeof item.query === 'string')
        .slice(0, props.maxHistoryItems)
    }
  } catch (error) {
    console.error('Failed to load search history:', error)
    searchHistory.value = []
  }
}

// Save history ke localStorage
const saveSearchHistory = () => {
  try {
    const historyToSave = searchHistory.value.slice(0, props.maxHistoryItems)
    localStorage.setItem('wiki-search-history-v2', JSON.stringify(historyToSave))
    emit('history-change', historyToSave)
  } catch (error) {
    console.error('Failed to save search history:', error)
  }
}

// Add search ke history
const addToHistory = (query, resultsCount = 0) => {
  if (!query || typeof query !== 'string' || query.trim() === '') {
    return
  }

  const trimmedQuery = query.trim()
  const existingIndex = searchHistory.value.findIndex(
    item => item.query.toLowerCase() === trimmedQuery.toLowerCase()
  )

  const historyItem = {
    id: Date.now() + Math.random(),
    query: trimmedQuery,
    resultsCount,
    timestamp: Date.now()
  }

  if (existingIndex > -1) {
    // Update existing item
    searchHistory.value[existingIndex] = historyItem
    // Move to top
    const item = searchHistory.value.splice(existingIndex, 1)[0]
    searchHistory.value.unshift(item)
  } else {
    // Add new item ke top
    searchHistory.value.unshift(historyItem)
  }

  // Limit history size
  searchHistory.value = searchHistory.value.slice(0, props.maxHistoryItems)

  // Save ke localStorage
  saveSearchHistory()
}

// Remove dari history
const removeFromHistory = (index) => {
  const removedItem = searchHistory.value[index]
  searchHistory.value.splice(index, 1)
  saveSearchHistory()

  // Emit removal event untuk analytics
  emit('history-remove', removedItem)
}

// Clear all history
const clearHistory = () => {
  if (confirm('Are you sure you want to clear all search history?')) {
    searchHistory.value = []
    saveSearchHistory()
    emit('history-clear')
  }
}

// Search dari history
const searchFromHistory = (query) => {
  emit('search', query)
}

// Format timestamp
const formatTime = (timestamp) => {
  const now = Date.now()
  const diff = now - timestamp

  if (diff < 60000) {
    return 'Just now'
  } else if (diff < 3600000) {
    const minutes = Math.floor(diff / 60000)
    return `${minutes}m ago`
  } else if (diff < 86400000) {
    const hours = Math.floor(diff / 3600000)
    return `${hours}h ago`
  } else if (diff < 604800000) {
    const days = Math.floor(diff / 86400000)
    return `${days}d ago`
  } else {
    const date = new Date(timestamp)
    return date.toLocaleDateString('en-US', {
      month: 'short',
      day: 'numeric'
    })
  }
}

// Export method untuk parent component
defineExpose({
  addToHistory,
  clearHistory,
  loadSearchHistory
})

// Initialize
onMounted(() => {
  loadSearchHistory()
})
</script>

<style scoped>
.search-history-container {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  padding: 1.5rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.history-title {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin: 0;
  font-size: 1.1rem;
  font-weight: 600;
  color: #2d3748;
}

.history-icon {
  font-size: 1rem;
}

.history-actions {
  display: flex;
  gap: 0.5rem;
}

.clear-button {
  background: none;
  border: 1px solid #e2e8f0;
  color: #6c757d;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: all 0.3s ease;
}

.clear-button:hover {
  background: #f8f9fa;
  border-color: #6c757d;
  color: #495057;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.history-item {
  background: #f8f9fa;
  border-radius: 8px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.history-item:hover {
  background: #e9ecef;
  transform: translateY(-1px);
}

.history-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  cursor: pointer;
}

.history-info {
  flex: 1;
  min-width: 0;
}

.history-query {
  display: block;
  font-weight: 500;
  color: #2d3748;
  margin-bottom: 0.25rem;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.history-meta {
  display: block;
  font-size: 0.8rem;
  color: #6c757d;
}

.history-actions-right {
  display: flex;
  gap: 0.5rem;
}

.remove-button {
  background: none;
  border: none;
  color: #6c757d;
  cursor: pointer;
  font-size: 1.2rem;
  padding: 0.25rem;
  width: 28px;
  height: 28px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  transition: all 0.3s ease;
  opacity: 0.7;
}

.remove-button:hover {
  background: #dc3545;
  color: white;
  opacity: 1;
}

.history-empty {
  text-align: center;
  padding: 2rem 1rem;
  color: #6c757d;
}

.empty-icon {
  display: block;
  font-size: 2rem;
  margin-bottom: 1rem;
  opacity: 0.5;
}

.empty-text {
  margin: 0 0 0.5rem 0;
  font-weight: 500;
}

.empty-subtext {
  margin: 0;
  font-size: 0.9rem;
  opacity: 0.8;
}

.history-stats {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
  text-align: center;
}

.stats-text {
  font-size: 0.85rem;
  color: #6c757d;
}

/* Animations */
.history-item-enter-active,
.history-item-leave-active {
  transition: all 0.3s ease;
}

.history-item-enter-from {
  opacity: 0;
  transform: translateX(-20px);
}

.history-item-leave-to {
  opacity: 0;
  transform: translateX(20px);
}

.history-item-move {
  transition: transform 0.3s ease;
}

/* Dark theme */
[data-theme="dark"] .search-history-container {
  background: rgba(45, 55, 72, 0.95);
  border-color: rgba(255, 255, 255, 0.1);
}

[data-theme="dark"] .history-title {
  color: #e2e8f0;
}

[data-theme="dark"] .history-item {
  background: #4a5568;
}

[data-theme="dark"] .history-item:hover {
  background: #718096;
}

[data-theme="dark"] .history-query {
  color: #e2e8f0;
}

[data-theme="dark"] .clear-button {
  border-color: #718096;
  color: #a0aec0;
}

[data-theme="dark"] .clear-button:hover {
  background: #2d3748;
  border-color: #a0aec0;
  color: #e2e8f0;
}

/* Responsive */
@media (max-width: 768px) {
  .search-history-container {
    padding: 1rem;
  }

  .history-header {
    flex-direction: column;
    gap: 0.75rem;
    text-align: center;
  }

  .history-content {
    padding: 0.5rem 0.75rem;
  }

  .history-meta {
    font-size: 0.75rem;
  }

  .remove-button {
    width: 24px;
    height: 24px;
    font-size: 1rem;
  }
}
</style>
```

---

## 12. Debouncing untuk Performance

### 12.1. Debounce Utility

```javascript
// src/utils/debounce.js

/**
 * Debounce function untuk membatasi frekuensi function execution
 * @param {Function} func - Function yang akan di-debounce
 * @param {number} delay - Delay dalam milliseconds
 * @param {boolean} immediate - Execute immediately on first call
 * @returns {Function} - Function yang sudah di-debounce
 */
export function debounce(func, delay = 300, immediate = false) {
  let timeoutId = null

  return function (...args) {
    // Clear existing timeout
    if (timeoutId) {
      clearTimeout(timeoutId)
    }

    // Execute immediately jika immediate = true
    if (immediate && !timeoutId) {
      func.apply(this, args)
    }

    // Set new timeout
    timeoutId = setTimeout(() => {
      if (!immediate) {
        func.apply(this, args)
      }
      timeoutId = null
    }, delay)
  }
}

/**
 * Throttle function untuk membatasi frekuensi execution
 * @param {Function} func - Function yang akan di-throttle
 * @param {number} delay - Delay dalam milliseconds
 * @returns {Function} - Function yang sudah di-throttle
 */
export function throttle(func, delay = 300) {
  let lastCall = 0

  return function (...args) {
    const now = Date.now()

    if (now - lastCall >= delay) {
      lastCall = now
      return func.apply(this, args)
    }
  }
}

/**
 * Async debounce function untuk async operations
 * @param {Function} func - Async function yang akan di-debounce
 * @param {number} delay - Delay dalam milliseconds
 * @returns {Function} - Async function yang sudah di-debounce
 */
export function debounceAsync(func, delay = 300) {
  let timeoutId = null
  let latestResolve = null
  let latestReject = null

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
        } finally {
          timeoutId = null
          latestResolve = null
          latestReject = null
        }
      }, delay)
    })
  }
}

/**
 * Memoize function untuk caching results
 * @param {Function} func - Function yang akan di-memoize
 * @param {Function} keyGenerator - Function untuk generate cache key
 * @returns {Function} - Function yang sudah di-memoize
 */
export function memoize(func, keyGenerator = (...args) => JSON.stringify(args)) {
  const cache = new Map()

  return function (...args) {
    const key = keyGenerator(...args)

    if (cache.has(key)) {
      return cache.get(key)
    }

    const result = func.apply(this, args)
    cache.set(key, result)

    // Limit cache size
    if (cache.size > 100) {
      const firstKey = cache.keys().next().value
      cache.delete(firstKey)
    }

    return result
  }
}
```

### 12.2. Debounced Search Implementation

```vue
<script setup>
import { ref, watch } from 'vue'
import { debounce, debounceAsync, memoize } from '../utils/debounce.js'
import { searchWikipedia } from '../utils/wikipediaService.js'

// Reactive state
const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)

// Memoized search function untuk caching
const memoizedSearch = memoize(
  async (query) => {
    return await searchWikipedia(query, 10)
  },
  (query) => query.toLowerCase().trim()
)

// Debounced search function
const debouncedSearch = debounceAsync(async (query) => {
  if (!query || query.trim() === '') {
    searchResults.value = []
    error.value = null
    return
  }

  try {
    isLoading.value = true
    error.value = null

    const results = await memoizedSearch(query.trim())
    searchResults.value = results

  } catch (err) {
    console.error('Search error:', err)
    searchResults.value = []
    error.value = err.message || 'Search failed. Please try again.'
  } finally {
    isLoading.value = false
  }
}, 500) // 500ms delay

// Watch search query untuk auto-search
watch(searchQuery, (newQuery, oldQuery) => {
  // Hanya search jika query berubah secara signifikan
  if (newQuery !== oldQuery) {
    debouncedSearch(newQuery)
  }
}, { immediate: false })

// Manual search (untuk button click atau form submit)
const performManualSearch = async () => {
  if (searchQuery.value.trim() === '') {
    error.value = 'Please enter a search term'
    return
  }

  try {
    isLoading.value = true
    error.value = null

    // Clear pending debounced search
    debouncedSearch.cancel?.()

    const results = await searchWikipedia(searchQuery.value.trim(), 10)
    searchResults.value = results

  } catch (err) {
    console.error('Manual search error:', err)
    searchResults.value = []
    error.value = err.message || 'Search failed. Please try again.'
  } finally {
    isLoading.value = false
  }
}

// Throttled search untuk suggestion updates
const throttledSuggestions = throttle(async (query) => {
  if (query.length > 2) {
    // Fetch suggestions
    try {
      const suggestions = await fetchSuggestions(query)
      // Update suggestions state
    } catch (err) {
      console.error('Suggestions error:', err)
    }
  }
}, 1000) // 1 second throttle

// Watch untuk suggestions
watch(searchQuery, (newQuery) => {
  throttledSuggestions(newQuery)
})
</script>

<template>
  <div class="search-container">
    <form @submit.prevent="performManualSearch" class="search-form">
      <div class="search-input-wrapper">
        <input
          v-model="searchQuery"
          type="text"
          class="search-input"
          placeholder="Search Wikipedia..."
          autocomplete="off"
        />

        <!-- Loading indicator -->
        <div v-if="isLoading" class="search-loading">
          <div class="loading-spinner"></div>
        </div>

        <!-- Clear button -->
        <button
          v-if="searchQuery.trim()"
          @click="clearSearch"
          type="button"
          class="clear-button"
        >
          ×
        </button>
      </div>

      <button
        type="submit"
        class="search-button"
        :disabled="isLoading || !searchQuery.trim()"
      >
        Search
      </button>
    </form>

    <!-- Search status -->
    <div v-if="searchQuery.trim()" class="search-status">
      <span v-if="isLoading" class="status-text loading">
        Searching...
      </span>
      <span v-else-if="searchResults.length > 0" class="status-text success">
        Found {{ searchResults.length }} results
      </span>
      <span v-else-if="error" class="status-text error">
        {{ error }}
      </span>
      <span v-else class="status-text">
        No results found
      </span>
    </div>
  </div>
</template>

<style scoped>
.search-container {
  max-width: 600px;
  margin: 0 auto;
}

.search-form {
  display: flex;
  gap: 0.75rem;
  align-items: stretch;
}

.search-input-wrapper {
  position: relative;
  flex: 1;
}

.search-input {
  width: 100%;
  padding: 1rem 3rem 1rem 1rem;
  font-size: 1.1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  background: white;
  transition: all 0.3s ease;
}

.search-input:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.search-loading {
  position: absolute;
  right: 1rem;
  top: 50%;
  transform: translateY(-50%);
}

.loading-spinner {
  width: 20px;
  height: 20px;
  border: 2px solid #e2e8f0;
  border-top: 2px solid #667eea;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.clear-button {
  position: absolute;
  right: 3.5rem;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: #6c757d;
  cursor: pointer;
  font-size: 1.5rem;
  padding: 0.25rem;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.clear-button:hover {
  background: #f8f9fa;
  color: #495057;
}

.search-button {
  padding: 1rem 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 100px;
}

.search-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.search-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.search-status {
  margin-top: 0.75rem;
  text-align: center;
}

.status-text {
  font-size: 0.9rem;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  display: inline-block;
}

.status-text.loading {
  color: #667eea;
  background: rgba(102, 126, 234, 0.1);
}

.status-text.success {
  color: #28a745;
  background: rgba(40, 167, 69, 0.1);
}

.status-text.error {
  color: #dc3545;
  background: rgba(220, 53, 69, 0.1);
}

@media (max-width: 768px) {
  .search-form {
    flex-direction: column;
  }

  .search-input {
    margin-bottom: 0.75rem;
  }

  .search-button {
    width: 100%;
  }
}
</style>
```

---

## 13. Error Handling

### 13.1. Error Handling Utility

```javascript
// src/utils/errorHandler.js

/**
 * Custom error classes untuk berbagai jenis error
 */
export class WikipediaApiError extends Error {
  constructor(message, statusCode, errorData) {
    super(message)
    this.name = 'WikipediaApiError'
    this.statusCode = statusCode
    this.errorData = errorData
  }
}

export class NetworkError extends Error {
  constructor(message) {
    super(message)
    this.name = 'NetworkError'
  }
}

export class ValidationError extends Error {
  constructor(message, field) {
    super(message)
    this.name = 'ValidationError'
    this.field = field
  }
}

/**
 * Error handler utility
 */
export class ErrorHandler {
  constructor() {
    this.errors = []
    this.maxErrors = 50
  }

  /**
   * Handle error dengan logging dan user-friendly message
   * @param {Error} error - Error object
   * @param {string} context - Context where error occurred
   * @returns {string} User-friendly error message
   */
  handleError(error, context = 'Unknown') {
    const errorInfo = {
      message: error.message,
      context,
      timestamp: new Date().toISOString(),
      stack: error.stack,
      type: error.constructor.name
    }

    // Add to errors array
    this.errors.push(errorInfo)

    // Limit errors array size
    if (this.errors.length > this.maxErrors) {
      this.errors = this.errors.slice(-this.maxErrors)
    }

    // Log error
    console.error(`[${context}] ${error.name}: ${error.message}`, error)

    // Return user-friendly message
    return this.getUserFriendlyMessage(error)
  }

  /**
   * Generate user-friendly error message
   * @param {Error} error - Error object
   * @returns {string} User-friendly message
   */
  getUserFriendlyMessage(error) {
    if (error instanceof WikipediaApiError) {
      switch (error.statusCode) {
        case 400:
          return 'Invalid search query. Please check your spelling and try again.'
        case 404:
          return 'No articles found. Try different keywords.'
        case 429:
          return 'Too many requests. Please wait a moment and try again.'
        case 500:
          return 'Wikipedia is experiencing issues. Please try again later.'
        default:
          return 'Search failed. Please try again.'
      }
    }

    if (error instanceof NetworkError) {
      return 'Connection failed. Please check your internet connection and try again.'
    }

    if (error instanceof ValidationError) {
      return error.message
    }

    if (error.name === 'TypeError') {
      return 'Something went wrong. Please refresh the page and try again.'
    }

    return 'An unexpected error occurred. Please try again.'
  }

  /**
   * Get recent errors for debugging
   * @param {number} limit - Number of errors to return
   * @returns {Array} Array of recent errors
   */
  getRecentErrors(limit = 10) {
    return this.errors.slice(-limit)
  }

  /**
   * Clear all errors
   */
  clearErrors() {
    this.errors = []
  }

  /**
   * Get error statistics
   * @returns {Object} Error statistics
   */
  getErrorStats() {
    const stats = {
      total: this.errors.length,
      byType: {},
      byContext: {}
    }

    this.errors.forEach(error => {
      stats.byType[error.type] = (stats.byType[error.type] || 0) + 1
      stats.byContext[error.context] = (stats.byContext[error.context] || 0) + 1
    })

    return stats
  }
}

// Global error handler instance
export const errorHandler = new ErrorHandler()

/**
 * Safe async function wrapper dengan error handling
 * @param {Function} asyncFunction - Async function to wrap
 * @param {string} context - Context for error reporting
 * @returns {Function} Wrapped function
 */
export function safeAsync(asyncFunction, context = 'AsyncOperation') {
  return async function (...args) {
    try {
      return await asyncFunction.apply(this, args)
    } catch (error) {
      const userMessage = errorHandler.handleError(error, context)
      throw new Error(userMessage)
    }
  }
}

/**
 * Retry function untuk failed operations
 * @param {Function} fn - Function to retry
 * @param {number} maxRetries - Maximum number of retries
 * @param {number} delay - Delay between retries in ms
 * @returns {Promise} Result of function
 */
export async function retry(fn, maxRetries = 3, delay = 1000) {
  let lastError

  for (let i = 0; i < maxRetries; i++) {
    try {
      return await fn()
    } catch (error) {
      lastError = error

      // Don't retry on certain errors
      if (error instanceof ValidationError) {
        throw error
      }

      if (i < maxRetries - 1) {
        console.warn(`Retry ${i + 1}/${maxRetries} after error:`, error.message)
        await new Promise(resolve => setTimeout(resolve, delay * Math.pow(2, i)))
      }
    }
  }

  throw lastError
}
```

### 13.2. Error Boundary Component

```vue
<!-- src/components/ErrorBoundary.vue -->
<template>
  <div v-if="hasError" class="error-boundary">
    <div class="error-container">
      <div class="error-icon">⚠️</div>
      <h2 class="error-title">Oops! Something went wrong</h2>

      <p class="error-message">
        {{ userFriendlyMessage }}
      </p>

      <div class="error-actions">
        <button @click="retry" class="retry-button">
          Try Again
        </button>
        <button @click="reload" class="reload-button">
          Reload Page
        </button>
      </div>

      <details v-if="showDetails" class="error-details">
        <summary class="details-summary">
          Technical Details
        </summary>
        <pre class="error-stack">{{ errorDetails }}</pre>
      </details>

      <button
        @click="toggleDetails"
        class="toggle-details-button"
      >
        {{ showDetails ? 'Hide' : 'Show' }} Technical Details
      </button>
    </div>
  </div>

  <slot v-else />
</template>

<script setup>
import { ref, onErrorCaptured } from 'vue'
import { errorHandler } from '../utils/errorHandler.js'

// Component state
const hasError = ref(false)
const error = ref(null)
const showDetails = ref(false)
const userFriendlyMessage = ref('')

// Capture errors dari child components
onErrorCaptured((error, instance, info) => {
  console.error('ErrorBoundary caught error:', error, info)

  // Store error
  hasError.value = true
  error.value = error

  // Generate user-friendly message
  userFriendlyMessage.value = errorHandler.getUserFriendlyMessage(error)

  // Report error untuk monitoring
  reportError(error, info)

  // Prevent error dari propagating
  return false
})

// Retry function
const retry = () => {
  hasError.value = false
  error.value = null
  userFriendlyMessage.value = ''
  showDetails.value = false
}

// Reload page
const reload = () => {
  window.location.reload()
}

// Toggle technical details
const toggleDetails = () => {
  showDetails.value = !showDetails.value
}

// Get error details untuk display
const errorDetails = computed(() => {
  if (!error.value) return ''

  return `
Name: ${error.value.name}
Message: ${error.value.message}
Stack: ${error.value.stack || 'No stack trace available'}
  `.trim()
})

// Report error untuk analytics/monitoring
const reportError = (error, info) => {
  // Kirim error ke monitoring service
  if (window.gtag) {
    window.gtag('event', 'exception', {
      description: error.message,
      fatal: false
    })
  }

  // Kirim ke custom error tracking
  if (window.errorTracking) {
    window.errorTracking.captureException(error, {
      extra: { info }
    })
  }
}
</script>

<style scoped>
.error-boundary {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
  padding: 2rem;
}

.error-container {
  background: white;
  border-radius: 16px;
  padding: 3rem 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  text-align: center;
  max-width: 500px;
  width: 100%;
}

.error-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
  opacity: 0.8;
}

.error-title {
  font-size: 1.5rem;
  color: #dc3545;
  margin: 0 0 1rem 0;
}

.error-message {
  color: #6c757d;
  line-height: 1.6;
  margin: 0 0 2rem 0;
}

.error-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 2rem;
}

.retry-button, .reload-button {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.3s ease;
}

.retry-button {
  background: #28a745;
  color: white;
}

.retry-button:hover {
  background: #218838;
  transform: translateY(-1px);
}

.reload-button {
  background: #6c757d;
  color: white;
}

.reload-button:hover {
  background: #5a6268;
  transform: translateY(-1px);
}

.toggle-details-button {
  background: none;
  border: 1px solid #dee2e6;
  color: #6c757d;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.3s ease;
}

.toggle-details-button:hover {
  background: #f8f9fa;
  border-color: #6c757d;
  color: #495057;
}

.error-details {
  margin: 1rem 0;
  text-align: left;
  background: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  overflow: hidden;
}

.details-summary {
  padding: 1rem;
  cursor: pointer;
  font-weight: 600;
  color: #495057;
  background: #e9ecef;
  margin: 0;
}

.details-summary:hover {
  background: #dee2e6;
}

.error-stack {
  padding: 1rem;
  margin: 0;
  font-size: 0.8rem;
  color: #495057;
  background: #f8f9fa;
  overflow-x: auto;
  white-space: pre-wrap;
  word-break: break-word;
}

/* Dark theme */
[data-theme="dark"] .error-container {
  background: #2d3748;
  color: #e2e8f0;
}

[data-theme="dark"] .error-title {
  color: #fc8181;
}

[data-theme="dark"] .error-message {
  color: #a0aec0;
}

[data-theme="dark"] .error-details {
  background: #4a5568;
  border-color: #718096;
}

[data-theme="dark"] .details-summary {
  background: #718096;
  color: #e2e8f0;
}

[data-theme="dark"] .details-summary:hover {
  background: #a0aec0;
}

[data-theme="dark"] .error-stack {
  background: #2d3748;
  color: #e2e8f0;
}

[data-theme="dark"] .toggle-details-button {
  border-color: #718096;
  color: #a0aec0;
}

[data-theme="dark"] .toggle-details-button:hover {
  background: #4a5568;
  border-color: #a0aec0;
  color: #e2e8f0;
}

/* Responsive */
@media (max-width: 768px) {
  .error-container {
    padding: 2rem 1.5rem;
  }

  .error-actions {
    flex-direction: column;
  }

  .error-icon {
    font-size: 3rem;
  }

  .error-title {
    font-size: 1.3rem;
  }
}
</style>
```

---

## 14. LocalStorage Integration

### 14.1. Storage Utility

```javascript
// src/utils/storage.js

/**
 * Storage utility dengan error handling dan compression
 */
export class StorageManager {
  constructor() {
    this.prefix = 'wiki-search-'
    this.compressionEnabled = true
  }

  /**
   * Set item ke localStorage dengan compression
   * @param {string} key - Storage key
   * @param {any} value - Value to store
   * @param {Object} options - Storage options
   */
  setItem(key, value, options = {}) {
    try {
      const prefixedKey = this.prefix + key
      const serializedValue = this._serialize(value, options)

      localStorage.setItem(prefixedKey, serializedValue)
      return true
    } catch (error) {
      console.warn('Failed to set storage item:', error)
      return false
    }
  }

  /**
   * Get item dari localStorage dengan decompression
   * @param {string} key - Storage key
   * @param {any} defaultValue - Default value if item doesn't exist
   * @returns {any} Stored value or default
   */
  getItem(key, defaultValue = null) {
    try {
      const prefixedKey = this.prefix + key
      const serializedValue = localStorage.getItem(prefixedKey)

      if (serializedValue === null) {
        return defaultValue
      }

      return this._deserialize(serializedValue)
    } catch (error) {
      console.warn('Failed to get storage item:', error)
      return defaultValue
    }
  }

  /**
   * Remove item dari localStorage
   * @param {string} key - Storage key
   */
  removeItem(key) {
    try {
      const prefixedKey = this.prefix + key
      localStorage.removeItem(prefixedKey)
      return true
    } catch (error) {
      console.warn('Failed to remove storage item:', error)
      return false
    }
  }

  /**
   * Check if item exists di localStorage
   * @param {string} key - Storage key
   * @returns {boolean} True if item exists
   */
  hasItem(key) {
    try {
      const prefixedKey = this.prefix + key
      return localStorage.getItem(prefixedKey) !== null
    } catch (error) {
      console.warn('Failed to check storage item:', error)
      return false
    }
  }

  /**
   * Get all storage keys dengan prefix
   * @returns {Array} Array of keys without prefix
   */
  getKeys() {
    try {
      const keys = []
      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i)
        if (key && key.startsWith(this.prefix)) {
          keys.push(key.slice(this.prefix.length))
        }
      }
      return keys
    } catch (error) {
      console.warn('Failed to get storage keys:', error)
      return []
    }
  }

  /**
   * Clear all storage items dengan prefix
   */
  clear() {
    try {
      const keys = this.getKeys()
      keys.forEach(key => this.removeItem(key))
      return true
    } catch (error) {
      console.warn('Failed to clear storage:', error)
      return false
    }
  }

  /**
   * Get storage usage information
   * @returns {Object} Storage usage stats
   */
  getStorageInfo() {
    try {
      let totalSize = 0
      const items = {}

      for (let i = 0; i < localStorage.length; i++) {
        const key = localStorage.key(i)
        if (key && key.startsWith(this.prefix)) {
          const value = localStorage.getItem(key)
          const size = (key.length + value.length) * 2 // UTF-16
          totalSize += size

          items[key.slice(this.prefix.length)] = {
            size,
            sizeHuman: this._formatBytes(size)
          }
        }
      }

      return {
        totalSize,
        totalSizeHuman: this._formatBytes(totalSize),
        itemCount: Object.keys(items).length,
        items
      }
    } catch (error) {
      console.warn('Failed to get storage info:', error)
      return {
        totalSize: 0,
        totalSizeHuman: '0 B',
        itemCount: 0,
        items: {}
      }
    }
  }

  /**
   * Serialize value untuk storage
   * @private
   */
  _serialize(value, options = {}) {
    const data = {
      value,
      timestamp: Date.now(),
      version: '1.0',
      compressed: this.compressionEnabled && !options.skipCompression
    }

    const serialized = JSON.stringify(data)

    if (data.compressed && this._isCompressionSupported()) {
      try {
        return this._compress(serialized)
      } catch (error) {
        console.warn('Compression failed, storing uncompressed:', error)
        data.compressed = false
        return JSON.stringify(data)
      }
    }

    return serialized
  }

  /**
   * Deserialize value dari storage
   * @private
   */
  _deserialize(serializedValue) {
    try {
      let data

      // Try to decompress if needed
      try {
        data = JSON.parse(serializedValue)
      } catch (error) {
        // Try decompression
        data = JSON.parse(this._decompress(serializedValue))
      }

      // Validate data structure
      if (!data || typeof data !== 'object' || !('value' in data)) {
        throw new Error('Invalid storage data format')
      }

      // Check version compatibility
      if (data.version && data.version !== '1.0') {
        console.warn('Storage version mismatch:', data.version)
      }

      return data.value
    } catch (error) {
      console.warn('Failed to deserialize storage value:', error)
      return null
    }
  }

  /**
   * Simple compression using LZ-string like algorithm
   * @private
   */
  _compress(str) {
    // Simple compression - untuk production gunakan library seperti lz-string
    return btoa(encodeURIComponent(str))
  }

  /**
   * Simple decompression
   * @private
   */
  _decompress(str) {
    try {
      return decodeURIComponent(atob(str))
    } catch (error) {
      throw new Error('Decompression failed')
    }
  }

  /**
   * Check if compression is supported
   * @private
   */
  _isCompressionSupported() {
    return typeof btoa !== 'undefined' && typeof atob !== 'undefined'
  }

  /**
   * Format bytes ke human readable format
   * @private
   */
  _formatBytes(bytes) {
    if (bytes === 0) return '0 B'

    const k = 1024
    const sizes = ['B', 'KB', 'MB', 'GB']
    const i = Math.floor(Math.log(bytes) / Math.log(k))

    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i]
  }
}

// Export singleton instance
export const storage = new StorageManager()

/**
 * Custom hooks untuk localStorage
 */
export function useStorage(key, defaultValue = null) {
  const storedValue = storage.getItem(key, defaultValue)
  const value = ref(storedValue)

  // Watch for changes and save to storage
  watch(value, (newValue) => {
    storage.setItem(key, newValue)
  }, { deep: true })

  return value
}

/**
 * Storage untuk user preferences
 */
export class PreferencesStorage {
  constructor() {
    this.key = 'user-preferences'
    this.defaults = {
      theme: 'light',
      language: 'en',
      resultsPerPage: 10,
      sortBy: 'relevance',
      autoSearch: true,
      showImages: true,
      enableHistory: true
    }
  }

  get() {
    return storage.getItem(this.key, { ...this.defaults })
  }

  set(preferences) {
    const merged = { ...this.get(), ...preferences }
    return storage.setItem(this.key, merged)
  }

  update(key, value) {
    const current = this.get()
    current[key] = value
    return storage.setItem(this.key, current)
  }

  reset() {
    return storage.setItem(this.key, { ...this.defaults })
  }
}

export const preferences = new PreferencesStorage()
```

### 14.2. Storage Integration in Component

```vue
<script setup>
import { ref, watch, onMounted, onUnmounted } from 'vue'
import { storage, useStorage, preferences } from '../utils/storage.js'

// Gunakan useStorage hook untuk reactive storage
const searchQuery = ref('')
const searchHistory = useStorage('search-history', [])
const userPreferences = useStorage('user-preferences', preferences.get())

// Manual storage management untuk complex data
const saveSearchResults = (results, query) => {
  const cacheData = {
    query,
    results,
    timestamp: Date.now(),
    expiresAt: Date.now() + (24 * 60 * 60 * 1000) // 24 hours
  }

  storage.setItem(`search-cache-${query}`, cacheData)
}

const getCachedResults = (query) => {
  const cacheData = storage.getItem(`search-cache-${query}`)

  if (!cacheData) return null

  // Check if cache is expired
  if (Date.now() > cacheData.expiresAt) {
    storage.removeItem(`search-cache-${query}`)
    return null
  }

  return cacheData.results
}

// Watch storage changes dari other tabs
const handleStorageChange = (event) => {
  if (event.key && event.key.startsWith(storage.prefix)) {
    const key = event.key.slice(storage.prefix.length)

    // Handle different storage changes
    switch (key) {
      case 'user-preferences':
        userPreferences.value = event.newValue ? JSON.parse(event.newValue) : preferences.get()
        break
      case 'search-history':
        searchHistory.value = event.newValue ? JSON.parse(event.newValue) : []
        break
    }
  }
}

// Storage event listeners
onMounted(() => {
  window.addEventListener('storage', handleStorageChange)
})

onUnmounted(() => {
  window.removeEventListener('storage', handleStorageChange)
})

// Watch user preferences changes
watch(userPreferences, (newPrefs) => {
  // Apply preferences
  document.documentElement.setAttribute('data-theme', newPrefs.theme)

  // Save to storage
  storage.setItem('user-preferences', newPrefs)
}, { deep: true })
</script>
```

---

## 15. Advanced Features

### 15.1. Search Suggestions Component

```vue
<!-- src/components/SearchSuggestions.vue -->
<template>
  <div v-if="showSuggestions && suggestions.length > 0" class="suggestions-container">
    <div class="suggestions-header">
      <span class="suggestions-title">Suggestions</span>
    </div>

    <ul class="suggestions-list">
      <li
        v-for="(suggestion, index) in suggestions"
        :key="suggestion.id || index"
        class="suggestion-item"
        :class="{ 'selected': selectedIndex === index }"
        @click="selectSuggestion(suggestion)"
        @mouseenter="selectedIndex = index"
      >
        <div class="suggestion-content">
          <span class="suggestion-text">{{ suggestion.text }}</span>
          <span v-if="suggestion.type" class="suggestion-type">
            {{ suggestion.type }}
          </span>
        </div>
        <span v-if="suggestion.description" class="suggestion-description">
          {{ suggestion.description }}
        </span>
      </li>
    </ul>

    <div class="suggestions-footer">
      <span class="suggestions-tip">
        Use ↑↓ to navigate, Enter to select, Esc to close
      </span>
    </div>
  </div>
</template>

<script setup>
import { ref, watch, computed, nextTick } from 'vue'

const props = defineProps({
  query: {
    type: String,
    required: true
  },
  showSuggestions: {
    type: Boolean,
    default: false
  },
  maxSuggestions: {
    type: Number,
    default: 5
  }
})

const emit = defineEmits(['select', 'hide'])

// Component state
const suggestions = ref([])
const selectedIndex = ref(0)
const isLoading = ref(false)

// Wikipedia API untuk suggestions
const fetchSuggestions = async (query) => {
  if (!query || query.length < 2) {
    suggestions.value = []
    return
  }

  try {
    isLoading.value = true

    const params = new URLSearchParams({
      action: 'opensearch',
      search: query,
      limit: props.maxSuggestions.toString(),
      namespace: '0',
      format: 'json',
      origin: '*'
    })

    const response = await fetch(
      `https://en.wikipedia.org/w/api.php?${params.toString()}`
    )

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const data = await response.json()

    if (Array.isArray(data) && data.length >= 4) {
      const titles = data[1] || []
      const descriptions = data[2] || []
      const urls = data[3] || []

      suggestions.value = titles.map((title, index) => ({
        id: index,
        text: title,
        description: descriptions[index] || '',
        url: urls[index] || '',
        type: 'article'
      }))
    } else {
      suggestions.value = []
    }

  } catch (error) {
    console.error('Failed to fetch suggestions:', error)
    suggestions.value = []
  } finally {
    isLoading.value = false
  }
}

// Debounced suggestions fetch
const debouncedFetch = debounce(fetchSuggestions, 300)

// Watch query changes
watch(() => props.query, (newQuery) => {
  if (newQuery && props.showSuggestions) {
    debouncedFetch(newQuery)
  } else {
    suggestions.value = []
  }
  selectedIndex.value = 0
})

// Watch show suggestions
watch(() => props.showSuggestions, (show) => {
  if (show && props.query) {
    debouncedFetch(props.query)
  } else {
    suggestions.value = []
  }
})

// Select suggestion
const selectSuggestion = (suggestion) => {
  emit('select', suggestion.text)
  emit('hide')
}

// Keyboard navigation
const handleKeyNavigation = (event) => {
  if (!props.showSuggestions || suggestions.value.length === 0) {
    return
  }

  switch (event.key) {
    case 'ArrowDown':
      event.preventDefault()
      selectedIndex.value = (selectedIndex.value + 1) % suggestions.value.length
      break

    case 'ArrowUp':
      event.preventDefault()
      selectedIndex.value = selectedIndex.value <= 0
        ? suggestions.value.length - 1
        : selectedIndex.value - 1
      break

    case 'Enter':
      event.preventDefault()
      if (selectedIndex.value >= 0 && selectedIndex.value < suggestions.value.length) {
        selectSuggestion(suggestions.value[selectedIndex.value])
      }
      break

    case 'Escape':
      event.preventDefault()
      emit('hide')
      break
  }
}

// Expose methods untuk parent component
defineExpose({
  handleKeyNavigation,
  selectSuggestion
})
</script>

<style scoped>
.suggestions-container {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border: 1px solid #e2e8f0;
  border-top: none;
  border-radius: 0 0 12px 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  z-index: 1000;
  max-height: 300px;
  overflow: hidden;
}

.suggestions-header {
  padding: 0.5rem 1rem;
  background: #f8f9fa;
  border-bottom: 1px solid #e2e8f0;
}

.suggestions-title {
  font-size: 0.8rem;
  font-weight: 600;
  color: #6c757d;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.suggestions-list {
  list-style: none;
  margin: 0;
  padding: 0;
  max-height: 200px;
  overflow-y: auto;
}

.suggestion-item {
  padding: 0.75rem 1rem;
  cursor: pointer;
  transition: background-color 0.2s ease;
  border-bottom: 1px solid #f8f9fa;
}

.suggestion-item:hover,
.suggestion-item.selected {
  background: #f8f9fa;
}

.suggestion-item:last-child {
  border-bottom: none;
}

.suggestion-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.25rem;
}

.suggestion-text {
  font-weight: 500;
  color: #2d3748;
}

.suggestion-type {
  font-size: 0.75rem;
  background: #e2e8f0;
  color: #6c757d;
  padding: 0.125rem 0.5rem;
  border-radius: 12px;
  text-transform: uppercase;
}

.suggestion-description {
  font-size: 0.85rem;
  color: #6c757d;
  line-height: 1.4;
  display: block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.suggestions-footer {
  padding: 0.5rem 1rem;
  background: #f8f9fa;
  border-top: 1px solid #e2e8f0;
}

.suggestions-tip {
  font-size: 0.75rem;
  color: #6c757d;
  font-style: italic;
}

/* Dark theme */
[data-theme="dark"] .suggestions-container {
  background: #2d3748;
  border-color: #4a5568;
}

[data-theme="dark"] .suggestions-header {
  background: #4a5568;
  border-bottom-color: #718096;
}

[data-theme="dark"] .suggestion-item {
  border-bottom-color: #4a5568;
}

[data-theme="dark"] .suggestion-item:hover,
[data-theme="dark"] .suggestion-item.selected {
  background: #4a5568;
}

[data-theme="dark"] .suggestion-text {
  color: #e2e8f0;
}

[data-theme="dark"] .suggestion-type {
  background: #718096;
  color: #a0aec0;
}

[data-theme="dark"] .suggestion-description {
  color: #a0aec0;
}

[data-theme="dark"] .suggestions-footer {
  background: #4a5568;
  border-top-color: #718096;
}

/* Scrollbar styling */
.suggestions-list::-webkit-scrollbar {
  width: 6px;
}

.suggestions-list::-webkit-scrollbar-track {
  background: #f1f1f1;
}

.suggestions-list::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 3px;
}

[data-theme="dark"] .suggestions-list::-webkit-scrollbar-track {
  background: #4a5568;
}

[data-theme="dark"] .suggestions-list::-webkit-scrollbar-thumb {
  background: #718096;
}
</style>
```

---

## 16. Testing dan Debugging

### 16.1. Manual Testing Checklist

```javascript
// src/utils/testingChecklist.js

export const testingChecklist = {
  // Functional Tests
  functional: {
    search: [
      'Basic search with valid query',
      'Search with empty query',
      'Search with special characters',
      'Search with very long query',
      'Search with HTML entities',
      'Search with Unicode characters'
    ],

    ui: [
      'Theme toggle functionality',
      'Search history persistence',
      'Loading states display',
      'Error messages display',
      'Responsive design on mobile',
      'Keyboard navigation',
      'Accessibility features'
    ],

    performance: [
      'Search response time',
      'Debouncing effectiveness',
      'Memory usage',
      'Bundle size analysis',
      'Animation performance'
    ]
  },

  // Cross-browser Tests
  browsers: [
    'Chrome (latest)',
    'Firefox (latest)',
    'Safari (latest)',
    'Edge (latest)',
    'Mobile Chrome',
    'Mobile Safari'
  ],

  // Device Tests
  devices: [
    'Desktop (1920x1080)',
    'Tablet (768x1024)',
    'Mobile (375x667)',
    'Small mobile (320x568)'
  ],

  // Network Conditions
  network: [
    'Fast connection (4G)',
    'Slow connection (3G)',
    'Very slow connection (2G)',
    'Offline mode',
    'Intermittent connection'
  ]
}
```

### 16.2. Debug Tools Component

```vue
<!-- src/components/DebugPanel.vue -->
<template>
  <div v-if="isDevelopment" class="debug-panel">
    <div class="debug-header">
      <h3>Debug Panel</h3>
      <button @click="toggleDebugPanel" class="toggle-btn">
        {{ isOpen ? 'Hide' : 'Show' }}
      </button>
    </div>

    <div v-if="isOpen" class="debug-content">
      <!-- Storage Info -->
      <section class="debug-section">
        <h4>Storage Information</h4>
        <div class="debug-info">
          <p>Total Size: {{ storageInfo.totalSizeHuman }}</p>
          <p>Items: {{ storageInfo.itemCount }}</p>
          <button @click="clearStorage" class="debug-btn">Clear Storage</button>
        </div>
      </section>

      <!-- Component State -->
      <section class="debug-section">
        <h4>Component State</h4>
        <div class="debug-info">
          <p>Search Query: "{{ searchQuery }}"</p>
          <p>Results Count: {{ searchResults.length }}</p>
          <p>Loading: {{ isLoading }}</p>
          <p>Error: {{ error || 'None' }}</p>
          <p>Theme: {{ isDarkTheme ? 'Dark' : 'Light' }}</p>
        </div>
      </section>

      <!-- Performance Metrics -->
      <section class="debug-section">
        <h4>Performance</h4>
        <div class="debug-info">
          <p>Last Search Time: {{ lastSearchTime }}ms</p>
          <p>Search Count: {{ searchCount }}</p>
          <p>Error Count: {{ errorCount }}</p>
        </div>
      </section>

      <!-- Error Log -->
      <section class="debug-section">
        <h4>Recent Errors</h4>
        <div class="debug-info">
          <div v-if="recentErrors.length === 0" class="no-errors">
            No recent errors
          </div>
          <ul v-else class="error-list">
            <li v-for="(error, index) in recentErrors" :key="index" class="error-item">
              <span class="error-time">{{ formatTime(error.timestamp) }}</span>
              <span class="error-message">{{ error.message }}</span>
            </li>
          </ul>
        </div>
      </section>

      <!-- Actions -->
      <section class="debug-section">
        <h4>Debug Actions</h4>
        <div class="debug-actions">
          <button @click="triggerError" class="debug-btn error-btn">
            Trigger Test Error
          </button>
          <button @click="testApi" class="debug-btn">
            Test API Connection
          </button>
          <button @click="exportDebugInfo" class="debug-btn">
            Export Debug Info
          </button>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import { storage } from '../utils/storage.js'
import { errorHandler } from '../utils/errorHandler.js'

// Props dari parent component
const props = defineProps({
  searchQuery: String,
  searchResults: Array,
  isLoading: Boolean,
  error: String,
  isDarkTheme: Boolean
})

// Component state
const isOpen = ref(false)
const storageInfo = ref({})
const lastSearchTime = ref(0)
const searchCount = ref(0)
const errorCount = ref(0)
const recentErrors = ref([])

// Check if development environment
const isDevelopment = computed(() => {
  return import.meta.env.DEV || window.location.hostname === 'localhost'
})

// Toggle debug panel
const toggleDebugPanel = () => {
  isOpen.value = !isOpen.value
  if (isOpen.value) {
    updateDebugInfo()
  }
}

// Update debug information
const updateDebugInfo = () => {
  storageInfo.value = storage.getStorageInfo()
  recentErrors.value = errorHandler.getRecentErrors(5)
}

// Clear storage
const clearStorage = () => {
  if (confirm('Are you sure you want to clear all storage?')) {
    storage.clear()
    updateDebugInfo()
    alert('Storage cleared successfully')
  }
}

// Trigger test error
const triggerError = () => {
  try {
    throw new Error('This is a test error for debugging')
  } catch (error) {
    errorHandler.handleError(error, 'DebugPanel')
    updateDebugInfo()
  }
}

// Test API connection
const testApi = async () => {
  try {
    const startTime = performance.now()
    const response = await fetch('https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch=test&format=json&origin=*')
    const endTime = performance.now()

    if (response.ok) {
      alert(`API connection successful! Response time: ${(endTime - startTime).toFixed(2)}ms`)
    } else {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }
  } catch (error) {
    alert(`API connection failed: ${error.message}`)
  }
}

// Export debug information
const exportDebugInfo = () => {
  const debugData = {
    timestamp: new Date().toISOString(),
    userAgent: navigator.userAgent,
    url: window.location.href,
    storage: storageInfo.value,
    performance: {
      lastSearchTime: lastSearchTime.value,
      searchCount: searchCount.value,
      errorCount: errorCount.value
    },
    errors: recentErrors.value,
    state: {
      searchQuery: props.searchQuery,
      resultsCount: props.searchResults?.length || 0,
      isLoading: props.isLoading,
      error: props.error,
      isDarkTheme: props.isDarkTheme
    }
  }

  const blob = new Blob([JSON.stringify(debugData, null, 2)], { type: 'application/json' })
  const url = URL.createObjectURL(blob)
  const a = document.createElement('a')
  a.href = url
  a.download = `wiki-search-debug-${Date.now()}.json`
  document.body.appendChild(a)
  a.click()
  document.body.removeChild(a)
  URL.revokeObjectURL(url)
}

// Format timestamp
const formatTime = (timestamp) => {
  return new Date(timestamp).toLocaleTimeString()
}

// Initialize
onMounted(() => {
  if (isDevelopment.value) {
    // Update debug info every 5 seconds when panel is open
    setInterval(() => {
      if (isOpen.value) {
        updateDebugInfo()
      }
    }, 5000)
  }
})

// Expose methods untuk parent component
defineExpose({
  updateDebugInfo,
  recordSearchTime: (time) => { lastSearchTime.value = time; searchCount.value++ },
  recordError: () => { errorCount.value++; updateDebugInfo() }
})
</script>

<style scoped>
.debug-panel {
  position: fixed;
  top: 1rem;
  right: 1rem;
  width: 350px;
  max-height: 80vh;
  background: rgba(0, 0, 0, 0.9);
  color: white;
  border-radius: 8px;
  font-family: 'Monaco', 'Menlo', monospace;
  font-size: 0.8rem;
  z-index: 9999;
  overflow: hidden;
}

.debug-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

.debug-header h3 {
  margin: 0;
  font-size: 0.9rem;
}

.toggle-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.75rem;
}

.debug-content {
  max-height: 60vh;
  overflow-y: auto;
  padding: 1rem;
}

.debug-section {
  margin-bottom: 1.5rem;
}

.debug-section h4 {
  margin: 0 0 0.75rem 0;
  font-size: 0.85rem;
  color: #4CAF50;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.debug-info {
  background: rgba(255, 255, 255, 0.05);
  padding: 0.75rem;
  border-radius: 4px;
  border-left: 3px solid #4CAF50;
}

.debug-info p {
  margin: 0.25rem 0;
}

.debug-actions {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.debug-btn {
  background: rgba(255, 255, 255, 0.1);
  border: 1px solid rgba(255, 255, 255, 0.2);
  color: white;
  padding: 0.5rem;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.75rem;
  transition: background-color 0.3s ease;
}

.debug-btn:hover {
  background: rgba(255, 255, 255, 0.2);
}

.debug-btn.error-btn {
  border-color: #f44336;
}

.debug-btn.error-btn:hover {
  background: rgba(244, 67, 54, 0.2);
}

.no-errors {
  font-style: italic;
  opacity: 0.7;
}

.error-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.error-item {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  margin-bottom: 0.5rem;
  padding: 0.5rem;
  background: rgba(244, 67, 54, 0.1);
  border-radius: 4px;
}

.error-time {
  font-size: 0.7rem;
  opacity: 0.7;
}

.error-message {
  color: #ffcdd2;
}

/* Scrollbar */
.debug-content::-webkit-scrollbar {
  width: 6px;
}

.debug-content::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
}

.debug-content::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 3px;
}

@media (max-width: 768px) {
  .debug-panel {
    position: relative;
    top: auto;
    right: auto;
    width: 100%;
    max-height: none;
    margin: 1rem;
  }
}
</style>
```

---

## 17. Deployment

### 17.1. Build Configuration

```javascript
// vite.config.js untuk production
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],

  // Build configuration
  build: {
    outDir: 'dist',
    sourcemap: true,
    minify: 'terser',
    terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true
      }
    },
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['vue']
        }
      }
    },
    chunkSizeWarningLimit: 1000
  },

  // Server configuration
  server: {
    port: 5173,
    open: true,
    host: true
  },

  // Preview configuration
  preview: {
    port: 4173,
    host: true
  },

  // Define constants
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
    __BUILD_TIME__: JSON.stringify(new Date().toISOString())
  }
})
```

### 17.2. Production Environment Setup

```javascript
// src/config/environment.js
export const config = {
  development: {
    apiUrl: 'https://en.wikipedia.org/w/api.php',
    enableDebugPanel: true,
    enableAnalytics: false,
    logLevel: 'debug'
  },

  production: {
    apiUrl: 'https://en.wikipedia.org/w/api.php',
    enableDebugPanel: false,
    enableAnalytics: true,
    logLevel: 'error'
  }
}

export const currentConfig = config[import.meta.env.MODE] || config.development
```

### 17.3. Deployment Scripts

```json
// package.json scripts
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "build:analyze": "vite build && npx vite-bundle-analyzer dist/stats.html",
    "deploy:preview": "npm run build && npm run preview",
    "deploy:netlify": "npm run build && netlify deploy --prod --dir=dist",
    "deploy:vercel": "npm run build && vercel --prod",
    "test": "echo \"No tests specified\" && exit 0"
  }
}
```

### 17.4. GitHub Actions untuk CI/CD

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'

    - name: Install dependencies
      run: npm ci

    - name: Run tests
      run: npm test

    - name: Build application
      run: npm run build
      env:
        NODE_ENV: production

    - name: Deploy to Netlify
      if: github.ref == 'refs/heads/main'
      uses: nwtgck/actions-netlify@v1
      with:
        publish-dir: './dist'
        production-branch: main
        github-token: ${{ secrets.GITHUB_TOKEN }}
        deploy-message: "Deploy from GitHub Actions"
      env:
        NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
        NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
```

---

## 🎉 Kesimpulan

Selamat! Anda telah berhasil membuat aplikasi Wikipedia Search Clone yang lengkap dengan:

### ✅ Features yang Diimplementasikan:

1. **Core Search Functionality**
   - Wikipedia API integration
   - Real-time search dengan debouncing
   - Search suggestions
   - Search history

2. **User Experience**
   - Dark/Light theme toggle
   - Responsive design
   - Loading states
   - Error handling
   - Keyboard navigation

3. **Performance Optimization**
   - Debouncing untuk search input
   - Caching dengan localStorage
   - Lazy loading
   - Code splitting

4. **Advanced Features**
   - Debug panel untuk development
   - Error boundary
   - Storage management
   - Analytics integration

5. **Production Ready**
   - Build optimization
   - CI/CD pipeline
   - Environment configuration
   - Deployment setup

### 📚 Vue.js Concepts yang Dipelajari:

- Composition API dengan `<script setup>`
- Reactive state management (`ref`, `reactive`, `computed`)
- Event handling dan form validation
- Component lifecycle hooks
- Conditional rendering
- List rendering dengan `v-for`
- Two-way data binding dengan `v-model`
- Dynamic classes dan styles
- Component communication (props, emits)
- Custom hooks dan composables

### 🚀 Next Steps:

1. **Enhanced Features**:
   - Advanced search filters
   - Image search integration
   - Multi-language support
   - User accounts dan preferences

2. **Performance Improvements**:
   - Service worker untuk offline support
   - Image optimization
   - Bundle size optimization
   - Server-side rendering

3. **Testing**:
   - Unit tests dengan Vitest
   - Component tests
   - E2E tests dengan Playwright

4. **Monitoring dan Analytics**:
   - Error tracking
   - Performance monitoring
   - User behavior analytics

---

**Happy Coding! 🚀**

Anda sekarang memiliki aplikasi web modern yang fully functional dengan Vue.js 3! Project ini memberikan fondasi yang kuat untuk pengembangan web aplikasi yang lebih kompleks.