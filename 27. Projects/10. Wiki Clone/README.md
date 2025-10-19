# 📚 Wikipedia Search Clone - Vue.js Project

Aplikasi pencarian Wikipedia yang dibangun dengan Vue.js 3 Composition API. Project ini mendemonstrasikan integrasi API, state management, dan theming dalam aplikasi Vue.js modern.

## 🎯 Project Overview

Aplikasi ini memungkinkan pengguna untuk:
- 🔍 Mencari artikel Wikipedia menggunakan API resmi
- 🌙 Mengganti tema antara Light dan Dark mode
- 📱 Responsive design untuk semua perangkat
- ⚡ Loading states dan error handling
- 🔗 Direct links ke artikel Wikipedia

## 🛠️ Technologies Used

- **Vue.js 3** dengan Composition API dan `<script setup>`
- **Wikipedia API** untuk pencarian artikel
- **CSS3** dengan custom properties untuk theming
- **Fetch API** untuk HTTP requests
- **Vite** sebagai development tool

## 📋 Prerequisites

Sebelum memulai, pastikan Anda sudah menginstall:
- Node.js (versi 16+)
- npm atau yarn
- Basic knowledge HTML, CSS, JavaScript
- Basic understanding Vue.js concepts

## 🚀 Quick Start

### 1. Setup Project

```bash
# Buat folder project baru
mkdir wiki-clone
cd wiki-clone

# Inisialisasi dengan Vite
npm create vue@latest . -- --template vue

# Install dependencies
npm install
```

### 2. Start Development Server

```bash
npm run dev
```

Buka browser dan navigasi ke `http://localhost:5173`

## 📁 Project Structure

```
wiki-clone/
├── index.html              # Entry point HTML
├── package.json           # Project dependencies
├── vite.config.js         # Vite configuration
└── src/
    ├── main.js            # App initialization
    ├── App.vue            # Root component
    └── components/
        └── WikiComponent.vue  # Main search component
```

## 🧩 Vue.js Concepts Covered

### 1. Composition API dengan `<script setup>`

```vue
<script setup>
import { ref } from 'vue'

// Reactive state variables
const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)
const isDarkTheme = ref(false)
</script>
```

**Penjelasan untuk Pemula:**
- `<script setup>` adalah syntax sugar yang lebih ringkas untuk Composition API
- `ref()` membuat reactive variables yang bisa berubah dan otomatis update UI
- Setiap variable yang dibuat dengan `ref()` memiliki property `.value`

### 2. Reactive State Management

```javascript
// State untuk input pencarian
const searchQuery = ref('')

// State untuk hasil pencarian
const searchResults = ref([])

// State untuk loading indicator
const isLoading = ref(false)

// State untuk error handling
const error = ref(null)

// State untuk theme management
const isDarkTheme = ref(false)
```

**Penjelasan untuk Pemula:**
- Reactive state otomatis update tampilan ketika nilai berubah
- `isLoading` digunakan untuk menampilkan loading indicator
- `error` digunakan untuk menampilkan pesan error
- `isDarkTheme` mengontrol tema aplikasi

### 3. Event Handling

```vue
<template>
  <!-- Form submit event -->
  <form @submit.prevent="submitSearch">

    <!-- Input dengan v-model binding -->
    <input v-model="searchQuery" type="text" />

    <!-- Button dengan click event -->
    <button type="submit">Search</button>
  </form>
</template>
```

**Penjelasan untuk Pemula:**
- `@submit.prevent` mencegah form dari default behavior (refresh page)
- `v-model` membuat two-way data binding antara input dan variable
- Event handler di Vue diawali dengan `@` atau `v-on:`

### 4. Conditional Rendering

```vue
<template>
  <!-- Loading state -->
  <div v-if="isLoading" class="spinner">Loading ...</div>

  <!-- Error state -->
  <p v-if="error">{{ error }}</p>

  <!-- Results state -->
  <div v-if="searchResults.length">
    <div v-for="result in searchResults" :key="result.pageid">
      <!-- Result item -->
    </div>
  </div>
</template>
```

**Penjelasan untuk Pemula:**
- `v-if` untuk conditional rendering (menyembunyikan/menampilkan elemen)
- `v-for` untuk rendering list/array
- `:key` penting untuk Vue tracking di list rendering

### 5. Dynamic CSS Classes

```vue
<template>
  <div :class="{ 'dark-theme': isDarkTheme }">
    <!-- Content -->
  </div>
</template>
```

**Penjelasan untuk Pemula:**
- `:class` adalah shorthand untuk `v-bind:class`
- Object syntax `{ 'class-name': condition }` menambah class jika condition true
- Dynamic classes berguna untuk conditional styling

## 🔧 Implementation Details

### Wikipedia API Integration

```javascript
const searchWikipedia = async (query) => {
  const encodedQuery = encodeURIComponent(query)
  const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}`

  try {
    isLoading.value = true
    const response = await fetch(endpoint)
    const data = await response.json()

    if (data.query && data.query.search) {
      searchResults.value = data.query.search
      error.value = null
    } else {
      searchResults.value = []
      error.value = 'No results found.'
    }
  } catch (err) {
    console.error('Error fetching data:', err)
    searchResults.value = []
    error.value = 'An error occurred while fetching data.'
  } finally {
    isLoading.value = false
  }
}
```

**Penjelasan API Parameters:**
- `action=query`: Melakukan query ke Wikipedia API
- `list=search`: Mencari artikel
- `prop=info`: Mendapatkan informasi artikel
- `inprop=url`: Mendapatkan URL artikel
- `format=json`: Response format JSON
- `origin=*`: Mengizinkan CORS requests
- `srlimit=10`: Maksimal 10 hasil
- `srsearch=${query}: Kata kunci pencarian

### Theme Toggle Functionality

```javascript
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
}
```

```css
.dark-theme {
  background-color: #282c34;
  color: #fff;
}

.dark-theme #search-input {
  background-color: #454545;
  color: #fff;
  border-color: #fff;
}
```

**Penjelasan untuk Pemula:**
- Theme toggle mengubah state `isDarkTheme`
- CSS class `.dark-theme` diaplikasikan secara dinamis
- CSS styling berubah berdasarkan class yang aktif

## 📝 Step-by-Step Implementation

### Langkah 1: Setup Project Vue.js

```bash
# 1. Buat project baru
npm create vue@latest wiki-clone

# 2. Pilih opsi:
# ✅ Vue 3
# ✅ JavaScript
# ❌ TypeScript (opsional)
# ❌ JSX (opsional)
# ❌ Vue Router (tidak diperlukan)
# ❌ Pinia (tidak diperlukan)
# ❌ Vitest (opsional)
# ❌ End-to-End Testing (opsional)
# ❅ ESLint (recommended)
# ❅ Prettier (recommended)

# 3. Masuk ke folder project
cd wiki-clone

# 4. Install dependencies
npm install

# 5. Start development server
npm run dev
```

### Langkah 2: Struktur Folder dan File

```bash
# Struktur folder yang akan dibuat
src/
├── App.vue              # Root component
├── main.js              # Entry point
└── components/
    └── WikiComponent.vue    # Main search component
```

### Langkah 3: Setup Main App Component

**src/App.vue:**
```vue
<script setup>
import WikiComponent from './components/WikiComponent.vue'
</script>

<template>
  <WikiComponent />
</template>

<style>
/* Global styles */
body {
  margin: 0;
  padding: 0;
  font-family: 'Arial', sans-serif;
}

#app {
  min-height: 100vh;
}
</style>
```

### Langkah 4: Implementasi WikiComponent

**src/components/WikiComponent.vue:**

#### 4.1. Script Setup Section

```vue
<script setup>
import { ref } from 'vue'

// Reactive state variables
const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)
const isDarkTheme = ref(false)

// Search function
const searchWikipedia = async (query) => {
  // Implementation details...
}

// Theme toggle function
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
}

// Form submit handler
const submitSearch = () => {
  if (searchQuery.value.trim() !== '') {
    searchWikipedia(searchQuery.value)
  } else {
    searchResults.value = []
    error.value = 'Please enter a valid search term.'
  }
}
</script>
```

#### 4.2. Template Section

```vue
<template>
  <div :class="{ 'dark-theme': isDarkTheme }">
    <div class="container">
      <!-- Header with theme toggle -->
      <div class="header-container">
        <h1>Search Wikipedia</h1>
        <span id="theme-toggler" @click="toggleTheme">
          {{ isDarkTheme ? 'Light' : 'Dark' }}
        </span>
      </div>

      <!-- Search form -->
      <form @submit.prevent="submitSearch">
        <input
          v-model="searchQuery"
          type="text"
          id="search-input"
          placeholder="Enter search term"
        />
        <button type="submit">Search</button>
      </form>

      <!-- Search results -->
      <div id="search-results">
        <div v-if="isLoading" class="spinner">Loading ...</div>
        <p v-if="error">{{ error }}</p>
        <div v-if="searchResults.length">
          <div
            v-for="result in searchResults"
            :key="result.pageid"
            class="result-item"
          >
            <h3 class="result-title">
              <a
                :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                target="_blank"
                rel="noopener"
              >
                {{ result.title }}
              </a>
            </h3>
            <a
              :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
              class="result-link"
              target="_blank"
              rel="noopener"
            >
              {{ `https://en.wikipedia.org/?curid=${result.pageid}` }}
            </a>
            <p class="result-snippet" v-html="result.snippet"></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

#### 4.3. Style Section

```vue
<style scoped>
/* Container and layout */
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

/* Header styles */
.header-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

h1 {
  font-size: 3rem;
  margin-bottom: 2rem;
  color: #2c3e50;
}

/* Theme toggler */
#theme-toggler {
  border: none;
  background: transparent;
  cursor: pointer;
  background: #e2e2e2;
  padding: 10px 20px;
  border-radius: 100px;
  transition: background-color 0.3s;
}

#theme-toggler:hover {
  background: #d0d0d0;
}

/* Form styles */
form {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
}

#search-input {
  flex: 1;
  font-size: 1.2rem;
  padding: 0.75rem 1rem;
  border: 2px solid #e1e5e9;
  border-radius: 8px;
  transition: border-color 0.3s;
}

#search-input:focus {
  outline: none;
  border-color: #0074d9;
}

button[type='submit'] {
  font-size: 1.2rem;
  padding: 0.75rem 1.5rem;
  background-color: #0074d9;
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button[type='submit']:hover {
  background-color: #0063ad;
}

/* Results styles */
.result-item {
  background: white;
  border: 1px solid #e1e5e9;
  border-radius: 8px;
  padding: 1.5rem;
  margin-bottom: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.result-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.result-title {
  font-size: 1.5rem;
  margin-top: 0;
  margin-bottom: 0.5rem;
}

.result-title a {
  color: #2c3e50;
  text-decoration: none;
}

.result-title a:hover {
  color: #0074d9;
  text-decoration: underline;
}

.result-link {
  display: block;
  font-size: 0.9rem;
  color: #6c757d;
  margin-bottom: 1rem;
  text-decoration: none;
}

.result-link:hover {
  color: #0074d9;
  text-decoration: underline;
}

.result-snippet {
  margin-top: 0;
  line-height: 1.6;
  color: #4a5568;
}

/* Loading spinner */
.spinner {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 1.5rem;
  height: 5rem;
  color: #6c757d;
}

/* Error message */
.error-message {
  background: #fee;
  color: #c53030;
  padding: 1rem;
  border-radius: 8px;
  border: 1px solid #fed7d7;
  text-align: center;
}

/* Dark theme styles */
.dark-theme {
  background-color: #1a202c;
  color: #e2e8f0;
  min-height: 100vh;
  transition: background-color 0.3s, color 0.3s;
}

.dark-theme h1 {
  color: #e2e8f0;
}

.dark-theme #search-input {
  background-color: #2d3748;
  color: #e2e8f0;
  border-color: #4a5568;
}

.dark-theme #search-input:focus {
  border-color: #63b3ed;
}

.dark-theme .result-item {
  background: #2d3748;
  border-color: #4a5568;
}

.dark-theme .result-title a {
  color: #e2e8f0;
}

.dark-theme .result-title a:hover {
  color: #63b3ed;
}

.dark-theme .result-link {
  color: #a0aec0;
}

.dark-theme .result-link:hover {
  color: #63b3ed;
}

.dark-theme .result-snippet {
  color: #cbd5e0;
}

.dark-theme #theme-toggler {
  background: #4a5568;
  color: #e2e8f0;
}

.dark-theme #theme-toggler:hover {
  background: #718096;
}

/* Responsive design */
@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  h1 {
    font-size: 2rem;
  }

  .header-container {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  form {
    flex-direction: column;
  }

  #search-input {
    margin-right: 0;
  }

  .result-item {
    padding: 1rem;
  }
}
</style>
```

### Langkah 5: Complete Wikipedia Search Function

```javascript
const searchWikipedia = async (query) => {
  // Encode query untuk URL safety
  const encodedQuery = encodeURIComponent(query)

  // Wikipedia API endpoint
  const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}`

  try {
    // Set loading state
    isLoading.value = true
    error.value = null

    // Fetch data dari API
    const response = await fetch(endpoint)

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const data = await response.json()

    // Process response
    if (data.query && data.query.search) {
      searchResults.value = data.query.search
      error.value = null
    } else {
      searchResults.value = []
      error.value = 'No results found.'
    }

  } catch (err) {
    console.error('Error fetching data:', err)
    searchResults.value = []
    error.value = 'An error occurred while fetching data. Please try again.'
  } finally {
    // Reset loading state
    isLoading.value = false
  }
}
```

## 🎨 Enhanced Features Implementation

### 1. Debounced Search

Untuk meningkatkan user experience dan mengurangi API calls:

```javascript
import { ref, debounce } from 'vue'

// Debounced search function
const debouncedSearch = debounce(async (query) => {
  if (query.trim() !== '') {
    await searchWikipedia(query)
  } else {
    searchResults.value = []
    error.value = null
  }
}, 500)

// Watch search query changes
watch(searchQuery, (newQuery) => {
  if (newQuery.trim() !== '') {
    debouncedSearch(newQuery)
  }
})
```

### 2. Search History

```javascript
const searchHistory = ref([])

// Load history dari localStorage
const loadSearchHistory = () => {
  const history = localStorage.getItem('wiki-search-history')
  if (history) {
    searchHistory.value = JSON.parse(history)
  }
}

// Save search ke history
const saveToHistory = (query) => {
  if (!searchHistory.value.includes(query)) {
    searchHistory.value.unshift(query)
    searchHistory.value = searchHistory.value.slice(0, 10) // Max 10 items
    localStorage.setItem('wiki-search-history', JSON.stringify(searchHistory.value))
  }
}

// Initialize history
loadSearchHistory()
```

### 3. Advanced Filtering

```javascript
const searchFilters = ref({
  limit: 10,
  sort: 'relevance'
})

const searchWikipedia = async (query) => {
  const params = new URLSearchParams({
    action: 'query',
    list: 'search',
    prop: 'info',
    inprop: 'url|displaytitle',
    utf8: '',
    format: 'json',
    origin: '*',
    srlimit: searchFilters.value.limit,
    srsearch: query,
    srsort: searchFilters.value.sort
  })

  const endpoint = `https://en.wikipedia.org/w/api.php?${params.toString()}`
  // ... rest of implementation
}
```

## 📱 Responsive Design

### Mobile-First Approach

```css
/* Base styles untuk mobile */
.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 1rem;
}

/* Tablet styles */
@media (min-width: 768px) {
  .container {
    max-width: 768px;
    padding: 2rem;
  }
}

/* Desktop styles */
@media (min-width: 1024px) {
  .container {
    max-width: 1024px;
  }
}
```

## 🧪 Testing

### Manual Testing Checklist

- [ ] Search functionality works correctly
- [ ] Loading states appear properly
- [ ] Error handling works for network errors
- [ ] Theme toggle switches between light/dark
- [ ] Results display correctly
- [ ] Links open in new tabs
- [ ] Responsive design works on mobile
- [ ] Form validation works
- [ ] Empty state displays correctly

### Browser Testing

Test on:
- ✅ Chrome (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Edge (latest)

## 🚀 Deployment

### 1. Build for Production

```bash
npm run build
```

### 2. Deploy to Netlify

1. Push code ke GitHub repository
2. Connect repository ke Netlify
3. Set build command: `npm run build`
4. Set publish directory: `dist`

### 3. Deploy to Vercel

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel --prod
```

## 🔄 Enhanced Implementation (Advanced Version)

### Complete Enhanced WikiComponent.vue

```vue
<script setup>
import { ref, watch, onMounted } from 'vue'

// Reactive state
const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)
const isDarkTheme = ref(false)
const searchHistory = ref([])
const searchFilters = ref({
  limit: 10,
  sort: 'relevance'
})

// Debounce utility
const debounce = (func, delay) => {
  let timeoutId
  return (...args) => {
    clearTimeout(timeoutId)
    timeoutId = setTimeout(() => func.apply(this, args), delay)
  }
}

// Search function with advanced features
const searchWikipedia = async (query, filters = {}) => {
  if (!query || query.trim() === '') {
    searchResults.value = []
    error.value = 'Please enter a search term.'
    return
  }

  const encodedQuery = encodeURIComponent(query.trim())
  const limit = filters.limit || searchFilters.value.limit
  const sort = filters.sort || searchFilters.value.sort

  const params = new URLSearchParams({
    action: 'query',
    list: 'search',
    prop: 'info|snippet|pageimages',
    inprop: 'url|displaytitle',
    pithumbsize: 100,
    utf8: '',
    format: 'json',
    origin: '*',
    srlimit: limit,
    srsearch: encodedQuery,
    srsort: sort
  })

  const endpoint = `https://en.wikipedia.org/w/api.php?${params.toString()}`

  try {
    isLoading.value = true
    error.value = null

    const response = await fetch(endpoint)

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const data = await response.json()

    if (data.query && data.query.search) {
      searchResults.value = data.query.search.map(result => ({
        ...result,
        snippet: result.snippet?.replace(/<[^>]*>/g, '') || ''
      }))

      // Add to search history
      addToHistory(query.trim())

    } else {
      searchResults.value = []
      error.value = `No results found for "${query}"`
    }

  } catch (err) {
    console.error('Search error:', err)
    searchResults.value = []
    error.value = `Search failed: ${err.message}. Please try again.`
  } finally {
    isLoading.value = false
  }
}

// Debounced search
const debouncedSearch = debounce(searchWikipedia, 500)

// Theme management
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
  localStorage.setItem('wiki-theme', isDarkTheme.value ? 'dark' : 'light')
}

// Search history management
const loadSearchHistory = () => {
  try {
    const history = localStorage.getItem('wiki-search-history')
    if (history) {
      searchHistory.value = JSON.parse(history)
    }
  } catch (err) {
    console.error('Failed to load search history:', err)
  }
}

const addToHistory = (query) => {
  if (!query || query.trim() === '') return

  const trimmedQuery = query.trim()
  const existingIndex = searchHistory.value.indexOf(trimmedQuery)

  if (existingIndex > -1) {
    searchHistory.value.splice(existingIndex, 1)
  }

  searchHistory.value.unshift(trimmedQuery)
  searchHistory.value = searchHistory.value.slice(0, 10)

  try {
    localStorage.setItem('wiki-search-history', JSON.stringify(searchHistory.value))
  } catch (err) {
    console.error('Failed to save search history:', err)
  }
}

const removeFromHistory = (index) => {
  searchHistory.value.splice(index, 1)
  localStorage.setItem('wiki-search-history', JSON.stringify(searchHistory.value))
}

const clearHistory = () => {
  searchHistory.value = []
  localStorage.removeItem('wiki-search-history')
}

const searchFromHistory = (query) => {
  searchQuery.value = query
  searchWikipedia(query)
}

// Form submission
const submitSearch = (event) => {
  event.preventDefault()
  if (searchQuery.value.trim() !== '') {
    searchWikipedia(searchQuery.value.trim())
  } else {
    error.value = 'Please enter a valid search term.'
  }
}

// Load theme and history on mount
onMounted(() => {
  // Load theme preference
  const savedTheme = localStorage.getItem('wiki-theme')
  if (savedTheme === 'dark') {
    isDarkTheme.value = true
  }

  // Load search history
  loadSearchHistory()

  // Watch search query for auto-search
  watch(searchQuery, (newQuery) => {
    if (newQuery.trim() !== '') {
      debouncedSearch(newQuery.trim())
    } else {
      searchResults.value = []
      error.value = null
    }
  })
})
</script>

<template>
  <div :class="{ 'dark-theme': isDarkTheme }" class="app-container">
    <div class="container">
      <!-- Header with theme toggle -->
      <header class="header">
        <div class="header-content">
          <h1 class="title">
            <span class="wiki-icon">📚</span>
            Wikipedia Search
          </h1>
          <button
            @click="toggleTheme"
            class="theme-toggle"
            :title="isDarkTheme ? 'Switch to light theme' : 'Switch to dark theme'"
          >
            {{ isDarkTheme ? '☀️' : '🌙' }}
          </button>
        </div>
      </header>

      <!-- Search form with enhanced features -->
      <section class="search-section">
        <form @submit="submitSearch" class="search-form">
          <div class="search-input-container">
            <input
              v-model="searchQuery"
              type="text"
              id="search-input"
              class="search-input"
              placeholder="Search Wikipedia..."
              autocomplete="off"
            />
            <button
              type="submit"
              class="search-button"
              :disabled="isLoading || !searchQuery.trim()"
            >
              {{ isLoading ? '🔍' : 'Search' }}
            </button>
          </div>

          <!-- Search filters -->
          <div class="search-filters">
            <label class="filter-label">
              Results per page:
              <select v-model="searchFilters.limit" class="filter-select">
                <option value="5">5</option>
                <option value="10">10</option>
                <option value="20">20</option>
                <option value="50">50</option>
              </select>
            </label>

            <label class="filter-label">
              Sort by:
              <select v-model="searchFilters.sort" class="filter-select">
                <option value="relevance">Relevance</option>
                <option value="last_edit_desc">Last Edited</option>
                <option value="create_timestamp_desc">Newest First</option>
                <option value="just_match">Just Match</option>
              </select>
            </label>
          </div>
        </form>

        <!-- Search history -->
        <section v-if="searchHistory.length > 0" class="search-history">
          <div class="history-header">
            <h3>Recent Searches</h3>
            <button @click="clearHistory" class="clear-history-btn">Clear All</button>
          </div>
          <div class="history-items">
            <span
              v-for="(term, index) in searchHistory"
              :key="index"
              class="history-item"
              @click="searchFromHistory(term)"
            >
              {{ term }}
              <button
                @click.stop="removeFromHistory(index)"
                class="remove-history-item"
              >
                ×
              </button>
            </span>
          </div>
        </section>
      </section>

      <!-- Search results -->
      <main class="results-section">
        <div v-if="isLoading" class="loading-container">
          <div class="spinner"></div>
          <p>Searching Wikipedia...</p>
        </div>

        <div v-else-if="error" class="error-message">
          <span class="error-icon">⚠️</span>
          {{ error }}
        </div>

        <div v-else-if="searchResults.length > 0" class="results-container">
          <div class="results-header">
            <h2>Search Results</h2>
            <span class="results-count">{{ searchResults.length }} results found</span>
          </div>

          <div class="results-list">
            <article
              v-for="result in searchResults"
              :key="result.pageid"
              class="result-item"
            >
              <div class="result-content">
                <h3 class="result-title">
                  <a
                    :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="result-link"
                  >
                    {{ result.title }}
                  </a>
                </h3>

                <p class="result-snippet">
                  {{ result.snippet || 'No description available.' }}
                </p>

                <div class="result-meta">
                  <span class="result-size">
                    {{ result.size ? `${Math.round(result.size / 1024)}KB` : 'Unknown size' }}
                  </span>
                  <span class="result-wordcount">
                    {{ result.wordcount ? `${result.wordcount} words` : '' }}
                  </span>
                  <a
                    :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                    target="_blank"
                    rel="noopener noreferrer"
                    class="read-more-link"
                  >
                    Read more →
                  </a>
                </div>
              </div>
            </article>
          </div>
        </div>

        <div v-else-if="searchQuery.trim() && !isLoading && !error" class="no-results">
          <p>No results found for "{{ searchQuery }}"</p>
          <p>Try different keywords or check your spelling.</p>
        </div>

        <div v-else-if="!searchQuery.trim()" class="welcome-message">
          <h2>Welcome to Wikipedia Search</h2>
          <p>Enter a search term above to explore Wikipedia articles.</p>
          <div class="search-suggestions">
            <p>Popular searches:</p>
            <div class="suggestion-chips">
              <button
                v-for="suggestion in ['Vue.js', 'JavaScript', 'Web Development', 'Artificial Intelligence']"
                :key="suggestion"
                @click="searchQuery = suggestion"
                class="suggestion-chip"
              >
                {{ suggestion }}
              </button>
            </div>
          </div>
        </div>
      </main>

      <!-- Footer -->
      <footer class="footer">
        <p>
          Powered by
          <a href="https://en.wikipedia.org" target="_blank" rel="noopener">
            Wikipedia API
          </a>
        </p>
      </footer>
    </div>
  </div>
</template>

<style scoped>
/* App container */
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  transition: background 0.3s ease;
}

.dark-theme {
  background: linear-gradient(135deg, #1a202c 0%, #2d3748 100%);
}

/* Main container */
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 2rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Header styles */
.header {
  margin-bottom: 2rem;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 1.5rem;
  border-radius: 16px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.title {
  margin: 0;
  color: white;
  font-size: 2.5rem;
  font-weight: 700;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.wiki-icon {
  font-size: 2rem;
}

.theme-toggle {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: white;
  padding: 0.75rem;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.5rem;
  transition: all 0.3s ease;
  width: 50px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.theme-toggle:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

/* Search section */
.search-section {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 16px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  margin-bottom: 2rem;
}

.dark-theme .search-section {
  background: rgba(45, 55, 72, 0.95);
  color: #e2e8f0;
}

.search-form {
  margin-bottom: 1.5rem;
}

.search-input-container {
  display: flex;
  gap: 1rem;
  margin-bottom: 1rem;
}

.search-input {
  flex: 1;
  padding: 1rem 1.25rem;
  font-size: 1.1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  transition: all 0.3s ease;
  background: white;
}

.search-input:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.dark-theme .search-input {
  background: #2d3748;
  border-color: #4a5568;
  color: #e2e8f0;
}

.dark-theme .search-input:focus {
  border-color: #63b3ed;
  box-shadow: 0 0 0 3px rgba(99, 179, 237, 0.1);
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
  min-width: 120px;
}

.search-button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.search-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

/* Search filters */
.search-filters {
  display: flex;
  gap: 2rem;
  flex-wrap: wrap;
}

.filter-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-weight: 500;
}

.filter-select {
  padding: 0.5rem 1rem;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  background: white;
  cursor: pointer;
}

.dark-theme .filter-select {
  background: #2d3748;
  border-color: #4a5568;
  color: #e2e8f0;
}

/* Search history */
.search-history {
  margin-top: 1.5rem;
  padding-top: 1.5rem;
  border-top: 1px solid #e2e8f0;
}

.dark-theme .search-history {
  border-top-color: #4a5568;
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.history-header h3 {
  margin: 0;
  font-size: 1.1rem;
  color: #4a5568;
}

.dark-theme .history-header h3 {
  color: #a0aec0;
}

.clear-history-btn {
  background: none;
  border: 1px solid #e2e8f0;
  color: #6c757d;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.875rem;
  transition: all 0.3s ease;
}

.clear-history-btn:hover {
  background: #f8f9fa;
  border-color: #6c757d;
}

.history-items {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.history-item {
  background: #f8f9fa;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.9rem;
}

.history-item:hover {
  background: #e9ecef;
  transform: translateY(-1px);
}

.dark-theme .history-item {
  background: #4a5568;
  color: #e2e8f0;
}

.dark-theme .history-item:hover {
  background: #718096;
}

.remove-history-item {
  background: none;
  border: none;
  color: #6c757d;
  cursor: pointer;
  font-size: 1.2rem;
  padding: 0;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  transition: all 0.3s ease;
}

.remove-history-item:hover {
  background: #dc3545;
  color: white;
}

/* Results section */
.results-section {
  flex: 1;
}

.loading-container {
  text-align: center;
  padding: 3rem;
  color: white;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid rgba(255, 255, 255, 0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-message {
  background: #fee;
  color: #c53030;
  padding: 1.5rem;
  border-radius: 12px;
  text-align: center;
  border: 1px solid #fed7d7;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  font-weight: 500;
}

.dark-theme .error-message {
  background: rgba(229, 62, 62, 0.1);
  border-color: rgba(229, 62, 62, 0.3);
  color: #fc8181;
}

.results-container {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.dark-theme .results-container {
  background: rgba(45, 55, 72, 0.95);
  color: #e2e8f0;
}

.results-header {
  padding: 1.5rem;
  background: rgba(102, 126, 234, 0.1);
  border-bottom: 1px solid rgba(102, 126, 234, 0.2);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.results-header h2 {
  margin: 0;
  color: #2c3e50;
  font-size: 1.5rem;
}

.dark-theme .results-header h2 {
  color: #e2e8f0;
}

.results-count {
  color: #6c757d;
  font-size: 0.9rem;
}

.dark-theme .results-count {
  color: #a0aec0;
}

.results-list {
  max-height: 600px;
  overflow-y: auto;
}

.result-item {
  padding: 1.5rem;
  border-bottom: 1px solid #e2e8f0;
  transition: all 0.3s ease;
}

.result-item:last-child {
  border-bottom: none;
}

.result-item:hover {
  background: rgba(102, 126, 234, 0.05);
}

.dark-theme .result-item {
  border-bottom-color: #4a5568;
}

.dark-theme .result-item:hover {
  background: rgba(99, 179, 237, 0.1);
}

.result-title {
  margin: 0 0 0.5rem 0;
  font-size: 1.3rem;
}

.result-link {
  color: #667eea;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
}

.result-link:hover {
  color: #5a67d8;
  text-decoration: underline;
}

.result-snippet {
  margin: 0.5rem 0 1rem 0;
  line-height: 1.6;
  color: #4a5568;
}

.dark-theme .result-snippet {
  color: #cbd5e0;
}

.result-meta {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 0.875rem;
  color: #6c757d;
}

.dark-theme .result-meta {
  color: #a0aec0;
}

.read-more-link {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.read-more-link:hover {
  color: #5a67d8;
  text-decoration: underline;
}

.no-results {
  text-align: center;
  padding: 3rem;
  color: white;
}

.no-results p {
  margin: 0.5rem 0;
}

.welcome-message {
  text-align: center;
  padding: 3rem;
  color: white;
}

.welcome-message h2 {
  margin: 0 0 1rem 0;
  font-size: 2rem;
}

.search-suggestions {
  margin-top: 2rem;
}

.search-suggestions p {
  margin: 0 0 1rem 0;
  opacity: 0.9;
}

.suggestion-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

.suggestion-chip {
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.9rem;
}

.suggestion-chip:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-1px);
}

/* Footer */
.footer {
  text-align: center;
  padding: 2rem 0 0 0;
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.9rem;
}

.footer a {
  color: white;
  text-decoration: none;
  font-weight: 500;
}

.footer a:hover {
  text-decoration: underline;
}

/* Responsive design */
@media (max-width: 768px) {
  .container {
    padding: 1rem;
  }

  .header-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .title {
    font-size: 2rem;
  }

  .search-input-container {
    flex-direction: column;
  }

  .search-filters {
    flex-direction: column;
    gap: 1rem;
  }

  .results-header {
    flex-direction: column;
    gap: 0.5rem;
    text-align: center;
  }

  .result-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.5rem;
  }

  .suggestion-chips {
    flex-direction: column;
    align-items: center;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.5rem;
  }

  .search-input {
    font-size: 1rem;
  }

  .search-button {
    font-size: 1rem;
    padding: 0.75rem 1.5rem;
  }

  .result-title {
    font-size: 1.1rem;
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

/* Focus styles for better accessibility */
button:focus-visible,
input:focus-visible,
select:focus-visible {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}

/* High contrast mode support */
@media (prefers-contrast: high) {
  .search-input {
    border-width: 2px;
  }

  .search-button {
    border: 2px solid currentColor;
  }
}
</style>
```

## 📋 Learning Checklist

### Vue.js Concepts Mastered:
- [x] Vue.js 3 Composition API dengan `<script setup>`
- [x] Reactive state management dengan `ref()`
- [x] Event handling (`@submit`, `@click`)
- [x] Conditional rendering (`v-if`, `v-else`)
- [x] List rendering (`v-for`)
- [x] Two-way data binding (`v-model`)
- [x] Dynamic classes (`:class`)
- [x] Component lifecycle (`onMounted`)
- [x] Watchers (`watch`)
- [x] Props dan emits

### Web Development Skills:
- [x] API integration dengan Fetch API
- [x] Asynchronous programming dengan async/await
- [x] Error handling dan validation
- [x] LocalStorage untuk data persistence
- [x] Responsive design dengan CSS Grid dan Flexbox
- [x] CSS custom properties untuk theming
- [x] Accessibility best practices
- [x] Performance optimization dengan debouncing

### Advanced Features:
- [x] Search history management
- [x] Advanced filtering dan sorting
- [x] Loading states dan skeleton screens
- [x] Component composition
- [x] State management patterns
- [x] Form validation
- [x] Cross-browser compatibility

## 🔧 Troubleshooting & Common Issues

### 1. Wikipedia API Issues

#### 1.1 CORS Error
**Problem**: Error "Access-Control-Allow-Origin" di browser console
**Cause**: Wikipedia API memerlukan `origin=*` parameter untuk CORS
**Solution**:
```javascript
// Pastikan API URL mengandung origin=* parameter
const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&format=json&origin=*&srsearch=${query}`
```

#### 1.2 API Rate Limiting
**Problem**: Error "429 Too Many Requests" atau responses lambat
**Cause**: Terlalu banyak requests dalam waktu singkat
**Solution**:
- Implementasi debouncing (minimal 500ms delay)
- Batasi auto-search frequency
- Gunakan caching untuk frequent queries
```javascript
// Debounce search untuk mengurangi API calls
const debouncedSearch = debounce(searchWikipedia, 500)
```

#### 1.3 Network Connection Error
**Problem**: "Failed to fetch" atau "NetworkError"
**Cause**: Masalah koneksi internet atau Wikipedia API down
**Solution**:
- Periksa koneksi internet
- Cek Wikipedia API status: https://en.wikipedia.org/wiki/Special:ApiStatus
- Implementasi retry logic
```javascript
const retrySearch = async (query, maxRetries = 3) => {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await searchWikipedia(query)
    } catch (error) {
      if (i === maxRetries - 1) throw error
      await new Promise(resolve => setTimeout(resolve, 1000 * (i + 1)))
    }
  }
}
```

### 2. State Management Issues

#### 2.1 Reactive Data Not Updating
**Problem**: UI tidak update ketika data berubah
**Cause**: Lupa menggunakan `.value` untuk ref variables
**Solution**:
```javascript
// ❌ Salah
const results = ref([])
results = [] // Ini mengganti ref, bukan value

// ✅ Benar
const results = ref([])
results.value = [] // Ini mengubah value
```

#### 2.2 Search Query Not Working
**Problem**: Search tidak terpicu saat mengetik
**Cause**: Watch function tidak setup dengan benar
**Solution**:
```javascript
// Pastikan watch ter-setup dengan benar
watch(searchQuery, (newQuery) => {
  if (newQuery.trim() !== '') {
    debouncedSearch(newQuery)
  }
})
```

### 3. CSS and Styling Issues

#### 3.1 Dark Theme Not Applied
**Problem**: CSS dark theme tidak berfungsi
**Cause**: CSS selector atau class tidak benar
**Solution**:
```css
/* Pastikan selector benar */
.dark-theme .search-section {
  background: #2d3748;
  color: #e2e8f0;
}

/* Atau gunakan data attribute */
[data-theme="dark"] .search-section {
  background: #2d3748;
  color: #e2e8f0;
}
```

#### 3.2 Responsive Design Issues
**Problem**: Layout rusak di mobile
**Cause**: Media queries tidak benar atau viewport meta tag missing
**Solution**:
```html
<!-- Pastikan viewport meta tag ada di index.html -->
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

```css
/* Gunakan mobile-first approach */
.container {
  width: 100%;
  padding: 1rem;
}

@media (min-width: 768px) {
  .container {
    max-width: 768px;
    margin: 0 auto;
    padding: 2rem;
  }
}
```

### 4. LocalStorage Issues

#### 4.1 Storage Quota Exceeded
**Problem**: Error "QuotaExceededError"
**Cause**: LocalStorage penuh (biasanya 5-10MB limit)
**Solution**:
- Implementasi storage cleanup
- Compress data sebelum menyimpan
- Gunakan IndexedDB untuk data besar
```javascript
// Cleanup old data
const cleanupStorage = () => {
  const history = storage.getItem('search-history') || []
  const recentHistory = history.slice(0, 20) // Keep only 20 recent items
  storage.setItem('search-history', recentHistory)
}
```

#### 4.2 Privacy Mode Issues
**Problem**: LocalStorage tidak bekerja di incognito/private mode
**Cause**: Beberapa browser disable localStorage di private mode
**Solution**:
- Deteksi localStorage availability
- Fallback ke memory storage
```javascript
const isLocalStorageAvailable = () => {
  try {
    const test = '__test__'
    localStorage.setItem(test, test)
    localStorage.removeItem(test)
    return true
  } catch {
    return false
  }
}
```

### 5. Performance Issues

#### 5.1 Slow Search Response
**Problem**: Search terasa lambat
**Cause**: API latency atau tidak ada caching
**Solution**:
- Implementasi debouncing
- Cache frequent searches
- Optimize API calls
```javascript
// Cache implementation
const cache = new Map()

const cachedSearch = async (query) => {
  if (cache.has(query)) {
    return cache.get(query)
  }

  const results = await searchWikipedia(query)
  cache.set(query, results)
  return results
}
```

#### 5.2 Memory Leaks
**Problem**: Memory usage terus meningkat
**Cause**: Event listeners tidak cleaned up atau large data retained
**Solution**:
- Cleanup event listeners di onUnmounted
- Limit data yang disimpan di memory
```javascript
import { onUnmounted } from 'vue'

// Cleanup event listeners
onUnmounted(() => {
  window.removeEventListener('storage', handleStorageChange)
})
```

### 6. Browser Compatibility Issues

#### 6.1 IE11 Compatibility
**Problem**: Aplikasi tidak bekerja di IE11
**Cause**: Vue 3 tidak support IE11
**Solution**:
- Gunakan Vue 2 untuk IE11 support
- Atau gunakan polyfills dan transpiler

#### 6.2 Safari Issues
**Problem**: CSS atau JavaScript tidak bekerja di Safari
**Cause**: Safari memiliki implementasi yang berbeda
**Solution**:
```css
/* Safari specific fixes */
@supports not (backdrop-filter: blur(10px)) {
  .search-section {
    background: rgba(255, 255, 255, 0.95);
  }
}
```

### 7. Development Issues

#### 7.1 Vite Development Server Not Starting
**Problem**: `npm run dev` error
**Cause**: Port conflict atau dependencies issue
**Solution**:
```bash
# Kill process di port 5173
npx kill-port 5173

# Atau gunakan port berbeda
npm run dev -- --port 3000

# Clear cache dan reinstall
rm -rf node_modules package-lock.json
npm install
```

#### 7.2 Hot Module Replacement (HMR) Not Working
**Problem**: Changes tidak auto-refresh
**Cause**: Configuration issue atau browser cache
**Solution**:
- Clear browser cache
- Restart development server
- Check vite.config.js configuration

### 8. Debugging Tips

#### 8.1 Using Browser DevTools
- **Network Tab**: Monitor API requests dan responses
- **Console Tab**: Lihat error messages dan logs
- **Elements Tab**: Inspect DOM dan CSS
- **Vue DevTools**: Inspect Vue component state

#### 8.2 Common Debug Commands
```javascript
// Log search results
console.log('Search Results:', searchResults.value)

// Log API response
console.log('API Response:', data)

// Log errors
console.error('Search Error:', error)

// Monitor state changes
watch(searchQuery, (newVal, oldVal) => {
  console.log('Query changed:', oldVal, '->', newVal)
})
```

#### 8.3 Performance Debugging
```javascript
// Measure search performance
const startTime = performance.now()
await searchWikipedia(query)
const endTime = performance.now()
console.log(`Search took ${endTime - startTime} milliseconds`)
```

### 9. Production Issues

#### 9.1 Build Fails
**Problem**: `npm run build` error
**Cause**: TypeScript errors, missing dependencies, atau configuration issues
**Solution**:
```bash
# Check for TypeScript errors
npx tsc --noEmit

# Check for unused dependencies
npm audit

# Clean build
rm -rf dist
npm run build
```

#### 9.2 Deployment Issues
**Problem**: App tidak berfungsi setelah deployment
**Cause**: Base URL issues, missing files, atau server configuration
**Solution**:
- Check console errors di production
- Verify base URL configuration
- Ensure all static files are deployed

### 10. Security Considerations

#### 10.1 XSS Prevention
**Problem**: User input dapat inject malicious scripts
**Solution**:
- Sanitize user input
- Gunakan text content instead of HTML
- Vue.js otomatis escapes content di template

#### 10.2 API Key Security
**Problem**: API keys exposed di client-side code
**Solution**:
- Jangan store sensitive API keys di frontend
- Gunakan environment variables untuk development
- Consider backend proxy untuk API calls

### Quick Debug Checklist

If something's not working, check:

- [ ] Browser console untuk error messages
- [ ] Network tab untuk failed requests
- [ ] Vue DevTools untuk component state
- [ ] LocalStorage availability dan contents
- [ ] CSS untuk styling issues
- [ ] Responsive design di different screen sizes
- [ ] API response format dan structure
- [ ] Event handlers dan function bindings

## 🎯 Next Steps

### Enhancement Ideas:
1. **Advanced Search**: Add filters untuk language, date range, categories
2. **Caching**: Implement client-side caching untuk improved performance
3. **Pagination**: Add pagination untuk large result sets
4. **Analytics**: Track search patterns dan popular topics
5. **Offline Support**: Service worker untuk offline functionality
6. **Voice Search**: Implement Web Speech API
7. **Image Search**: Search Wikipedia images
8. **Export Results**: Export search results ke PDF/JSON
9. **Collaborative Features**: Share search results dengan others
10. **AI Integration**: Implement AI-powered search suggestions

## 🎉 Conclusion

Project Wikipedia Search Clone ini memberikan pemahaman mendalam tentang:
- Vue.js 3 Composition API dan modern JavaScript patterns
- API integration dan asynchronous programming
- State management dan data persistence
- Responsive design dan modern CSS techniques
- User experience optimization
- Performance best practices

Dengan mengikuti panduan ini, Anda telah membangun aplikasi web modern yang functional dan user-friendly menggunakan Vue.js 3!

---

**Happy Coding! 🚀**