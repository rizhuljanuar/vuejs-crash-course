# 📚 Wikipedia Search Clone - Panduan Cepat Implementasi

## 🚀 Setup Super Cepat (5 Menit)

### 1. Inisialisasi Project
```bash
# Buat project baru dengan Vite
npm create vue@latest wiki-clone-quick

# Pilih opsi:
# ✅ Vue 3
# ✅ JavaScript
# ❌ TypeScript
# ❌ Semua yang lain (tekan enter)

cd wiki-clone-quick
npm install
npm run dev
```

### 2. Ganti Isi File

#### `src/App.vue` (Copy & Paste)
```vue
<script setup>
import WikiComponent from './components/WikiComponent.vue'
</script>

<template>
  <WikiComponent />
</template>

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
  min-height: 100vh;
  padding: 2rem;
}
</style>
```

#### `src/components/WikiComponent.vue` (Copy & Paste)
```vue
<script setup>
import { ref, watch, onMounted } from 'vue'

// State variables
const searchQuery = ref('')
const searchResults = ref([])
const isLoading = ref(false)
const error = ref(null)
const isDarkTheme = ref(false)
const searchHistory = ref([])

// Wikipedia API Search
const searchWikipedia = async (query) => {
  if (!query || query.trim() === '') {
    searchResults.value = []
    error.value = null
    return
  }

  const encodedQuery = encodeURIComponent(query.trim())
  const endpoint = `https://en.wikipedia.org/w/api.php?action=query&list=search&prop=info&inprop=url&utf8=&format=json&origin=*&srlimit=10&srsearch=${encodedQuery}`

  try {
    isLoading.value = true
    error.value = null

    const response = await fetch(endpoint)
    const data = await response.json()

    if (data.query && data.query.search) {
      searchResults.value = data.query.search
      addToHistory(query.trim())
    } else {
      searchResults.value = []
      error.value = `No results found for "${query}"`
    }

  } catch (err) {
    console.error('Search error:', err)
    searchResults.value = []
    error.value = 'Failed to search. Please try again.'
  } finally {
    isLoading.value = false
  }
}

// Debounce function
const debounce = (func, delay) => {
  let timeoutId
  return (...args) => {
    clearTimeout(timeoutId)
    timeoutId = setTimeout(() => func.apply(this, args), delay)
  }
}

// Debounced search
const debouncedSearch = debounce(searchWikipedia, 500)

// Theme toggle
const toggleTheme = () => {
  isDarkTheme.value = !isDarkTheme.value
  localStorage.setItem('wiki-theme', isDarkTheme.value ? 'dark' : 'light')
}

// Search history functions
const loadSearchHistory = () => {
  try {
    const history = localStorage.getItem('wiki-search-history')
    if (history) {
      searchHistory.value = JSON.parse(history)
    }
  } catch (err) {
    console.error('Failed to load history:', err)
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
  searchHistory.value = searchHistory.value.slice(0, 8)

  try {
    localStorage.setItem('wiki-search-history', JSON.stringify(searchHistory.value))
  } catch (err) {
    console.error('Failed to save history:', err)
  }
}

const clearHistory = () => {
  searchHistory.value = []
  localStorage.removeItem('wiki-search-history')
}

const searchFromHistory = (query) => {
  searchQuery.value = query
  searchWikipedia(query)
}

// Form submit
const submitSearch = (e) => {
  e.preventDefault()
  if (searchQuery.value.trim() !== '') {
    searchWikipedia(searchQuery.value.trim())
  }
}

// Initialize
onMounted(() => {
  // Load theme
  const savedTheme = localStorage.getItem('wiki-theme')
  if (savedTheme === 'dark') {
    isDarkTheme.value = true
  }

  // Load history
  loadSearchHistory()

  // Setup auto-search dengan debouncing
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
  <div :class="{ 'dark-theme': isDarkTheme }" class="app">
    <div class="container">
      <!-- Header -->
      <header class="header">
        <div class="header-content">
          <h1 class="title">
            <span class="icon">📚</span>
            Wikipedia Search
          </h1>
          <button @click="toggleTheme" class="theme-btn">
            {{ isDarkTheme ? '☀️' : '🌙' }}
          </button>
        </div>
      </header>

      <!-- Search Section -->
      <section class="search-section">
        <form @submit="submitSearch" class="search-form">
          <div class="search-box">
            <input
              v-model="searchQuery"
              type="text"
              class="search-input"
              placeholder="Search Wikipedia..."
              autocomplete="off"
            />
            <button
              type="submit"
              class="search-btn"
              :disabled="isLoading || !searchQuery.trim()"
            >
              {{ isLoading ? '🔍' : 'Search' }}
            </button>
          </div>
        </form>

        <!-- Search History -->
        <div v-if="searchHistory.length > 0" class="search-history">
          <div class="history-header">
            <span>Recent Searches:</span>
            <button @click="clearHistory" class="clear-btn">Clear</button>
          </div>
          <div class="history-items">
            <span
              v-for="(term, index) in searchHistory.slice(0, 5)"
              :key="index"
              class="history-item"
              @click="searchFromHistory(term)"
            >
              {{ term }}
            </span>
          </div>
        </div>
      </section>

      <!-- Results Section -->
      <main class="results">
        <!-- Loading State -->
        <div v-if="isLoading" class="loading">
          <div class="spinner"></div>
          <p>Searching Wikipedia...</p>
        </div>

        <!-- Error State -->
        <div v-else-if="error" class="error">
          <span class="error-icon">⚠️</span>
          {{ error }}
        </div>

        <!-- Results -->
        <div v-else-if="searchResults.length > 0" class="results-container">
          <h2 class="results-title">Found {{ searchResults.length }} Results</h2>

          <div class="results-list">
            <article
              v-for="result in searchResults"
              :key="result.pageid"
              class="result-item"
            >
              <h3 class="result-title">
                <a
                  :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                  target="_blank"
                  rel="noopener"
                  class="result-link"
                >
                  {{ result.title }}
                </a>
              </h3>

              <p class="result-snippet">
                {{ result.snippet?.replace(/<[^>]*>/g, '') || 'No description available.' }}
              </p>

              <div class="result-meta">
                <span class="result-size">
                  {{ result.size ? `${Math.round(result.size / 1024)}KB` : 'Unknown size' }}
                </span>
                <a
                  :href="`https://en.wikipedia.org/?curid=${result.pageid}`"
                  target="_blank"
                  rel="noopener"
                  class="read-more"
                >
                  Read more →
                </a>
              </div>
            </article>
          </div>
        </div>

        <!-- Welcome State -->
        <div v-else-if="!searchQuery.trim()" class="welcome">
          <h2>Welcome to Wikipedia Search</h2>
          <p>Enter a search term to explore Wikipedia articles.</p>

          <div class="suggestions">
            <p>Try searching for:</p>
            <div class="suggestion-chips">
              <button
                v-for="suggestion in ['Vue.js', 'JavaScript', 'Artificial Intelligence', 'Climate Change']"
                :key="suggestion"
                @click="searchQuery = suggestion"
                class="chip"
              >
                {{ suggestion }}
              </button>
            </div>
          </div>
        </div>

        <!-- No Results State -->
        <div v-else class="no-results">
          <p>No results found for "{{ searchQuery }}"</p>
          <p>Try different keywords or check your spelling.</p>
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
/* App Container */
.app {
  min-height: 100vh;
  transition: all 0.3s ease;
}

.dark-theme {
  background: #1a202c;
}

/* Container */
.container {
  max-width: 800px;
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Header */
.header {
  margin-bottom: 2rem;
}

.header-content {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 1.5rem;
  border-radius: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.title {
  color: white;
  font-size: 2rem;
  font-weight: 700;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.icon {
  font-size: 1.8rem;
}

.theme-btn {
  background: rgba(255, 255, 255, 0.2);
  border: none;
  color: white;
  padding: 0.75rem;
  border-radius: 50%;
  cursor: pointer;
  font-size: 1.3rem;
  transition: all 0.3s ease;
  width: 45px;
  height: 45px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.theme-btn:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.1);
}

/* Search Section */
.search-section {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  padding: 2rem;
  border-radius: 16px;
  margin-bottom: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.dark-theme .search-section {
  background: rgba(45, 55, 72, 0.95);
  color: #e2e8f0;
}

.search-form {
  margin-bottom: 1.5rem;
}

.search-box {
  display: flex;
  gap: 1rem;
}

.search-input {
  flex: 1;
  padding: 1rem;
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

.search-btn {
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

.search-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
}

.search-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* Search History */
.search-history {
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
}

.dark-theme .search-history {
  border-top-color: #4a5568;
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
  font-size: 0.9rem;
  color: #6c757d;
}

.dark-theme .history-header {
  color: #a0aec0;
}

.clear-btn {
  background: none;
  border: 1px solid #dc3545;
  color: #dc3545;
  padding: 0.25rem 0.75rem;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.8rem;
  transition: all 0.3s ease;
}

.clear-btn:hover {
  background: #dc3545;
  color: white;
}

.history-items {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.history-item {
  background: #f8f9fa;
  padding: 0.4rem 0.8rem;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.85rem;
  color: #495057;
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

/* Results Section */
.results {
  flex: 1;
}

.loading {
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

.error {
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

.dark-theme .error {
  background: rgba(229, 62, 62, 0.1);
  border-color: rgba(229, 62, 62, 0.3);
  color: #fc8181;
}

.error-icon {
  font-size: 1.2rem;
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

.results-title {
  padding: 1.5rem;
  margin: 0;
  background: rgba(102, 126, 234, 0.1);
  border-bottom: 1px solid rgba(102, 126, 234, 0.2);
  color: #2c3e50;
  font-size: 1.3rem;
}

.dark-theme .results-title {
  color: #e2e8f0;
}

.results-list {
  max-height: 500px;
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
  font-size: 1.2rem;
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
  justify-content: space-between;
  align-items: center;
  font-size: 0.85rem;
  color: #6c757d;
}

.dark-theme .result-meta {
  color: #a0aec0;
}

.read-more {
  color: #667eea;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
}

.read-more:hover {
  color: #5a67d8;
  text-decoration: underline;
}

/* Welcome State */
.welcome {
  text-align: center;
  padding: 3rem;
  color: white;
}

.welcome h2 {
  margin: 0 0 1rem 0;
  font-size: 2rem;
}

.suggestions {
  margin-top: 2rem;
}

.suggestions p {
  margin: 0 0 1rem 0;
  opacity: 0.9;
}

.suggestion-chips {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  justify-content: center;
}

.chip {
  background: rgba(255, 255, 255, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.3);
  color: white;
  padding: 0.5rem 1rem;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.9rem;
}

.chip:hover {
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-1px);
}

/* No Results */
.no-results {
  text-align: center;
  padding: 3rem;
  color: white;
}

.no-results p {
  margin: 0.5rem 0;
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

/* Responsive */
@media (max-width: 768px) {
  #app {
    padding: 1rem;
  }

  .container {
    padding: 0;
  }

  .header-content {
    flex-direction: column;
    gap: 1rem;
    text-align: center;
  }

  .title {
    font-size: 1.5rem;
  }

  .search-box {
    flex-direction: column;
  }

  .search-section {
    padding: 1.5rem;
  }

  .results-title {
    font-size: 1.1rem;
  }

  .result-item {
    padding: 1rem;
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

  .chip {
    width: 200px;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.3rem;
  }

  .search-input {
    font-size: 1rem;
  }

  .search-btn {
    font-size: 1rem;
    padding: 0.75rem 1.5rem;
  }

  .result-title {
    font-size: 1.1rem;
  }

  .welcome h2 {
    font-size: 1.5rem;
  }
}

/* Accessibility */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

button:focus-visible,
input:focus-visible {
  outline: 2px solid #667eea;
  outline-offset: 2px;
}
</style>
```

### 3. Jalankan Aplikasi
```bash
npm run dev
```

Buka `http://localhost:5173` dan aplikasi Anda sudah siap digunakan! 🎉

## ✨ Fitur yang Saya Tambahkan:

1. **🎨 Beautiful UI** dengan glassmorphism design
2. **🌙 Dark/Light Theme** yang persisten
3. **🔍 Real-time Search** dengan debouncing
4. **📝 Search History** dengan localStorage
5. **⚡ Loading States** dengan spinner
6. **🎯 Error Handling** yang user-friendly
7. **📱 Responsive Design** untuk mobile
8. **♿ Accessibility Features**
9. **🚀 Performance Optimized**
10. **💡 Smart Suggestions**

## 🎯 Cara Penggunaan:

1. **Ketik keyword** di search box untuk auto-search
2. **Klik Search** atau tekan Enter untuk manual search
3. **Klik theme button** untuk ganti tema
4. **Klik history items** untuk search cepat
5. **Klik result links** untuk buka artikel Wikipedia

## 🔧 Customization Cepat:

### Ganti Warna Theme:
Edit CSS variables di bagian `.app`:
```css
.app {
  background: linear-gradient(135deg, #YOUR_COLOR_1 0%, #YOUR_COLOR_2 100%);
}
```

### Ganti Search Suggestions:
Edit array `suggestions` di template:
```javascript
['Vue.js', 'JavaScript', 'Artificial Intelligence', 'Climate Change']
```

### Ganti Max Results:
Edit parameter `srlimit` di API URL:
```javascript
`&srlimit=20&srsearch=${encodedQuery}` // Ganti 20 sesuai kebutuhan
```

## 🚀 Deploy ke Netlify:

1. **Push ke GitHub:**
```bash
git init
git add .
git commit -m "Wiki Clone App"
git branch -M main
git remote add origin https://github.com/username/wiki-clone.git
git push -u origin main
```

2. **Deploy ke Netlify:**
- Login ke netlify.com
- Click "New site from Git"
- Pilih GitHub repository
- Build command: `npm run build`
- Publish directory: `dist`
- Click "Deploy site"

## 🎉 Selamat! Aplikasi Wikipedia Search Anda siap digunakan!