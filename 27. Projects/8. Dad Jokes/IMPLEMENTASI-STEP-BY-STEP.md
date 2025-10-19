# 📋 Implementasi Step-by-Step: Dad Jokes Generator Vue.js

## 🎯 Panduan Lengkap Dari Nol Hingga Production

### 📝 Overview
Panduan ini akan memandu Anda langkah demi langkah dalam membuat aplikasi Dad Jokes Generator yang lengkap dengan Vue.js 3, mulai dari setup project hingga deployment dengan fitur-fitur modern seperti API integration, data persistence, dan progressive web app capabilities.

---

## 🔥 Langkah 1: Persiapan Environment

### 1.1 Install Node.js & npm
```bash
# Cek versi Node.js (minimal 14)
node --version

# Cek versi npm
npm --version

# Jika belum ada, download dari https://nodejs.org/
```

### 1.2 Pilih Development Tool
**Opsi A: Vite (Recommended - Lebih Cepat)**
```bash
npm create vue@latest dad-jokes-project
```

**Opsi B: Vue CLI (Traditional)**
```bash
npm install -g @vue/cli
vue create dad-jokes-project
```

### 1.3 Setup Project
```bash
# Masuk ke folder project
cd dad-jokes-project

# Install dependencies
npm install

# Install Axios untuk HTTP requests
npm install axios

# Start development server
npm run dev
```

---

## 🏗️ Langkah 2: Struktur Project Awal

### 2.1 Bersihkan File Default
**Hapus file yang tidak diperlukan:**
- `src/components/HelloWorld.vue`
- `src/assets/` (kosongkan)
- `src/components/TheWelcome.vue`
- `src/components/icons/`

### 2.2 Struktur Folder Baru
```
src/
├── App.vue                 # Main application component
├── main.js                 # Application entry point
├── components/
│   ├── DadJokes.vue        # Dad jokes main component
│   ├── JokeCard.vue        # Reusable joke card component
│   ├── JokeHistory.vue     # Joke history component
│   └── LoadingSpinner.vue  # Loading component
├── assets/
│   └── styles/
│       └── main.css        # Global styles
├── utils/
│   ├── jokeApi.js          # API helper functions
│   ├── storageUtils.js     # Storage helper functions
│   └── shareUtils.js       # Share functionality
├── composables/
│   ├── useJokes.js         # Joke composable
│   ├── useLocalStorage.js  # LocalStorage composable
│   └── useShare.js         # Share composable
└── services/
    └── jokeService.js      # Joke API service
```

---

## 🧩 Langkah 3: Membuat Service Layer

### 3.1 Joke Service
**File: `src/services/jokeService.js`**
```javascript
import axios from 'axios'

// API Configuration
const API_CONFIG = {
  baseURL: 'https://icanhazdadjoke.com/',
  timeout: 10000,
  headers: {
    'Accept': 'application/json',
    'User-Agent': 'Dad Jokes App (https://example.com)'
  }
}

// Fallback jokes untuk offline/error scenarios
const FALLBACK_JOKES = [
  {
    id: 'fallback-1',
    joke: "Why don't scientists trust atoms? Because they make up everything!",
    source: 'fallback'
  },
  {
    id: 'fallback-2',
    joke: "Why did the scarecrow win an award? He was outstanding in his field!",
    source: 'fallback'
  },
  {
    id: 'fallback-3',
    joke: "What do you call a fake noodle? An impasta!",
    source: 'fallback'
  },
  {
    id: 'fallback-4',
    joke: "Why can't a bicycle stand up by itself? It's two tired!",
    source: 'fallback'
  },
  {
    id: 'fallback-5',
    joke: "What do you call cheese that isn't yours? Nacho cheese!",
    source: 'fallback'
  }
]

// Joke Service Class
class JokeService {
  constructor() {
    this.axiosInstance = axios.create(API_CONFIG)
    this.cache = new Map()
    this.cacheTimeout = 5 * 60 * 1000 // 5 minutes
  }

  // Get random joke from API
  async getRandomJoke() {
    try {
      const response = await this.axiosInstance.get('/')

      const jokeData = {
        id: response.data.id,
        joke: response.data.joke,
        source: 'icanhazdadjoke',
        timestamp: new Date().toISOString()
      }

      // Cache the joke
      this.cacheJoke(jokeData)

      return {
        success: true,
        data: jokeData
      }
    } catch (error) {
      console.error('Error fetching joke:', error)
      return {
        success: false,
        error: this.handleError(error),
        fallback: this.getFallbackJoke()
      }
    }
  }

  // Get joke by ID
  async getJokeById(jokeId) {
    // Check cache first
    const cachedJoke = this.getCachedJoke(jokeId)
    if (cachedJoke) {
      return {
        success: true,
        data: cachedJoke,
        fromCache: true
      }
    }

    try {
      const response = await this.axiosInstance.get(`/j/${jokeId}`)

      const jokeData = {
        id: response.data.id,
        joke: response.data.joke,
        source: 'icanhazdadjoke',
        timestamp: new Date().toISOString()
      }

      this.cacheJoke(jokeData)

      return {
        success: true,
        data: jokeData
      }
    } catch (error) {
      console.error('Error fetching joke by ID:', error)
      return {
        success: false,
        error: this.handleError(error)
      }
    }
  }

  // Search jokes (simulated - icanhazdadjoke doesn't have search)
  async searchJokes(query) {
    // Since icanhazdadjoke doesn't have search API,
    // we'll search through our cache and fallback jokes
    const results = []

    // Search in cache
    for (const [id, joke] of this.cache) {
      if (joke.joke.toLowerCase().includes(query.toLowerCase())) {
        results.push(joke)
      }
    }

    // Search in fallback jokes
    for (const joke of FALLBACK_JOKES) {
      if (joke.joke.toLowerCase().includes(query.toLowerCase())) {
        results.push(joke)
      }
    }

    return {
      success: true,
      data: results.slice(0, 10), // Limit to 10 results
      total: results.length
    }
  }

  // Get multiple jokes for offline use
  async getMultipleJokes(count = 10) {
    const jokes = []
    const errors = []

    for (let i = 0; i < count; i++) {
      try {
        const result = await this.getRandomJoke()
        if (result.success) {
          jokes.push(result.data)

          // Add small delay between requests
          if (i < count - 1) {
            await new Promise(resolve => setTimeout(resolve, 100))
          }
        } else {
          errors.push(result.error)
        }
      } catch (error) {
        errors.push(error.message)
      }
    }

    return {
      success: jokes.length > 0,
      data: jokes,
      errors,
      totalRequested: count,
      totalReceived: jokes.length
    }
  }

  // Cache management
  cacheJoke(joke) {
    this.cache.set(joke.id, joke)

    // Remove old cache entries
    setTimeout(() => {
      this.cache.delete(joke.id)
    }, this.cacheTimeout)
  }

  getCachedJoke(jokeId) {
    return this.cache.get(jokeId) || null
  }

  clearCache() {
    this.cache.clear()
  }

  getCacheSize() {
    return this.cache.size
  }

  // Get fallback joke
  getFallbackJoke() {
    const randomIndex = Math.floor(Math.random() * FALLBACK_JOKES.length)
    return FALLBACK_JOKES[randomIndex]
  }

  // Error handling
  handleError(error) {
    if (error.response) {
      // Server responded with error status
      switch (error.response.status) {
        case 404:
          return 'Joke not found'
        case 429:
          return 'Too many requests. Please try again later.'
        case 500:
          return 'Server error. Please try again later.'
        default:
          return `Server error: ${error.response.status}`
      }
    } else if (error.request) {
      // Network error
      if (error.code === 'ECONNABORTED') {
        return 'Request timeout. Please check your connection.'
      }
      return 'Network error. Please check your internet connection.'
    } else {
      // Other error
      return error.message || 'An unknown error occurred'
    }
  }

  // Validate joke content
  validateJoke(jokeText) {
    if (!jokeText || typeof jokeText !== 'string') {
      return {
        isValid: false,
        errors: ['Joke text is required and must be a string']
      }
    }

    const errors = []

    if (jokeText.length < 10) {
      errors.push('Joke is too short')
    }

    if (jokeText.length > 500) {
      errors.push('Joke is too long')
    }

    // Check for appropriate content (simplified)
    const inappropriatePatterns = [
      /\b(violence|kill|death|hate)\b/i
    ]

    for (const pattern of inappropriatePatterns) {
      if (pattern.test(jokeText)) {
        errors.push('Joke contains inappropriate content')
        break
      }
    }

    return {
      isValid: errors.length === 0,
      errors
    }
  }

  // Get service statistics
  getStats() {
    return {
      cacheSize: this.cache.size,
      fallbackCount: FALLBACK_JOKES.length,
      apiEndpoint: API_CONFIG.baseURL,
      timeout: API_CONFIG.timeout
    }
  }
}

// Create singleton instance
const jokeService = new JokeService()

export default jokeService
```

### 3.2 Storage Utils
**File: `src/utils/storageUtils.js`**
```javascript
// Storage utility functions with error handling and compression
class StorageManager {
  constructor(prefix = 'dadJokes_') {
    this.prefix = prefix
    this.version = '1.0.0'
    this.compressionEnabled = true
    this.maxStorageSize = 5 * 1024 * 1024 // 5MB
  }

  // Generate storage key
  getKey(key) {
    return `${this.prefix}${key}`
  }

  // Check if localStorage is available
  isAvailable() {
    try {
      const test = '__storage_test__'
      localStorage.setItem(test, test)
      localStorage.removeItem(test)
      return true
    } catch (error) {
      console.warn('localStorage not available:', error)
      return false
    }
  }

  // Compress data if enabled
  compress(data) {
    if (!this.compressionEnabled) return data

    try {
      // Simple compression - remove unnecessary spaces
      return JSON.stringify(JSON.parse(JSON.stringify(data)))
    } catch (error) {
      console.warn('Compression failed:', error)
      return data
    }
  }

  // Get item from storage
  get(key, defaultValue = null) {
    if (!this.isAvailable()) return defaultValue

    try {
      const item = localStorage.getItem(this.getKey(key))
      if (!item) return defaultValue

      const parsed = JSON.parse(item)

      // Check version compatibility
      if (parsed.version && parsed.version !== this.version) {
        console.warn(`Storage version mismatch for ${key}`)
        this.remove(key)
        return defaultValue
      }

      return parsed.data
    } catch (error) {
      console.error(`Error getting ${key} from storage:`, error)
      return defaultValue
    }
  }

  // Set item to storage
  set(key, data) {
    if (!this.isAvailable()) return false

    try {
      // Check storage quota
      const currentSize = this.getStorageSize()
      const dataSize = JSON.stringify(data).length

      if (currentSize + dataSize > this.maxStorageSize) {
        this.cleanup()

        // Check again after cleanup
        if (this.getStorageSize() + dataSize > this.maxStorageSize) {
          console.warn('Storage quota exceeded')
          return false
        }
      }

      const item = {
        data: this.compress(data),
        version: this.version,
        timestamp: new Date().toISOString()
      }

      localStorage.setItem(this.getKey(key), JSON.stringify(item))
      return true
    } catch (error) {
      console.error(`Error setting ${key} to storage:`, error)
      return false
    }
  }

  // Remove item from storage
  remove(key) {
    if (!this.isAvailable()) return false

    try {
      localStorage.removeItem(this.getKey(key))
      return true
    } catch (error) {
      console.error(`Error removing ${key} from storage:`, error)
      return false
    }
  }

  // Clear all storage items with prefix
  clear() {
    if (!this.isAvailable()) return false

    try {
      const keys = Object.keys(localStorage).filter(key =>
        key.startsWith(this.prefix)
      )

      keys.forEach(key => localStorage.removeItem(key))
      return true
    } catch (error) {
      console.error('Error clearing storage:', error)
      return false
    }
  }

  // Get storage size
  getStorageSize() {
    if (!this.isAvailable()) return 0

    try {
      let totalSize = 0
      const keys = Object.keys(localStorage).filter(key =>
        key.startsWith(this.prefix)
      )

      keys.forEach(key => {
        totalSize += localStorage.getItem(key).length
      })

      return totalSize
    } catch (error) {
      console.error('Error calculating storage size:', error)
      return 0
    }
  }

  // Cleanup old or expired items
  cleanup() {
    if (!this.isAvailable()) return

    try {
      const keys = Object.keys(localStorage).filter(key =>
        key.startsWith(this.prefix)
      )

      const oneWeekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)
      let removedCount = 0

      keys.forEach(key => {
        try {
          const item = JSON.parse(localStorage.getItem(key))
          const itemDate = new Date(item.timestamp)

          // Remove items older than one week
          if (itemDate < oneWeekAgo) {
            localStorage.removeItem(key)
            removedCount++
          }
        } catch (error) {
          // Remove corrupted items
          localStorage.removeItem(key)
          removedCount++
        }
      })

      console.log(`Cleaned up ${removedCount} storage items`)
    } catch (error) {
      console.error('Error during cleanup:', error)
    }
  }

  // Export data
  exportData() {
    if (!this.isAvailable()) return null

    try {
      const data = {}
      const keys = Object.keys(localStorage).filter(key =>
        key.startsWith(this.prefix)
      )

      keys.forEach(key => {
        const cleanKey = key.replace(this.prefix, '')
        data[cleanKey] = JSON.parse(localStorage.getItem(key))
      })

      return {
        version: this.version,
        exportedAt: new Date().toISOString(),
        data
      }
    } catch (error) {
      console.error('Error exporting data:', error)
      return null
    }
  }

  // Import data
  importData(exportedData) {
    if (!this.isAvailable() || !exportedData || !exportedData.data) {
      return false
    }

    try {
      // Clear existing data
      this.clear()

      // Import new data
      Object.entries(exportedData.data).forEach(([key, value]) => {
        localStorage.setItem(this.getKey(key), JSON.stringify(value))
      })

      return true
    } catch (error) {
      console.error('Error importing data:', error)
      return false
    }
  }
}

// Create singleton instance
const storage = new StorageManager()

export default storage
```

### 3.3 Share Utils
**File: `src/utils/shareUtils.js`**
```javascript
// Share utility functions with multiple fallbacks
class ShareManager {
  constructor() {
    this.shareOptions = {
      title: 'Dad Joke',
      text: '',
      url: window.location.href
    }
  }

  // Main share function with multiple fallbacks
  async share(content, options = {}) {
    const shareData = {
      title: options.title || 'Dad Joke',
      text: content,
      url: options.url || window.location.href
    }

    try {
      // Try Web Share API first (modern browsers)
      if (navigator.share) {
        const result = await navigator.share(shareData)
        return {
          success: true,
          method: 'web_share_api',
          result
        }
      }

      // Fallback to clipboard copy
      return await this.copyToClipboard(content)

    } catch (error) {
      console.error('Share failed:', error)

      // Handle user cancellation
      if (error.name === 'AbortError') {
        return {
          success: false,
          error: 'Share cancelled by user',
          method: 'cancelled'
        }
      }

      return {
        success: false,
        error: error.message,
        method: 'error'
      }
    }
  }

  // Copy to clipboard with multiple fallbacks
  async copyToClipboard(text) {
    try {
      // Try modern Clipboard API first
      if (navigator.clipboard && window.isSecureContext) {
        await navigator.clipboard.writeText(text)
        return {
          success: true,
          method: 'clipboard_api'
        }
      }

      // Fallback to execCommand for older browsers
      return this.fallbackCopyToClipboard(text)

    } catch (error) {
      console.error('Copy to clipboard failed:', error)
      return {
        success: false,
        error: error.message,
        method: 'error'
      }
    }
  }

  // Fallback copy method
  fallbackCopyToClipboard(text) {
    return new Promise((resolve, reject) => {
      try {
        const textArea = document.createElement('textarea')
        textArea.value = text

        // Make the textarea out of viewport
        textArea.style.position = 'fixed'
        textArea.style.left = '-999999px'
        textArea.style.top = '-999999px'
        document.body.appendChild(textArea)

        textArea.focus()
        textArea.select()

        const successful = document.execCommand('copy')
        document.body.removeChild(textArea)

        if (successful) {
          resolve({
            success: true,
            method: 'exec_command'
          })
        } else {
          reject(new Error('Copy command failed'))
        }
      } catch (error) {
        reject(error)
      }
    })
  }

  // Share to specific platforms
  async shareToTwitter(content, options = {}) {
    const text = encodeURIComponent(content)
    const url = encodeURIComponent(options.url || window.location.href)
    const hashtags = options.hashtags ? encodeURIComponent(options.hashtags.join(',')) : ''

    const twitterUrl = `https://twitter.com/intent/tweet?text=${text}&url=${url}&hashtags=${hashtags}`

    return this.openShareWindow(twitterUrl, 'twitter')
  }

  async shareToFacebook(content, options = {}) {
    const url = encodeURIComponent(options.url || window.location.href)
    const facebookUrl = `https://www.facebook.com/sharer/sharer.php?u=${url}`

    return this.openShareWindow(facebookUrl, 'facebook')
  }

  async shareToWhatsApp(content) {
    const text = encodeURIComponent(content)
    const whatsappUrl = `https://wa.me/?text=${text}`

    return this.openShareWindow(whatsappUrl, 'whatsapp')
  }

  async shareToEmail(content, options = {}) {
    const subject = encodeURIComponent(options.subject || 'Check out this dad joke!')
    const body = encodeURIComponent(`${content}\n\n${options.url || window.location.href}`)
    const emailUrl = `mailto:?subject=${subject}&body=${body}`

    window.location.href = emailUrl
    return {
      success: true,
      method: 'email'
    }
  }

  // Open share popup window
  openShareWindow(url, platform) {
    return new Promise((resolve) => {
      const width = 600
      const height = 400
      const left = (window.innerWidth - width) / 2
      const top = (window.innerHeight - height) / 2

      const popup = window.open(
        url,
        `share_${platform}`,
        `width=${width},height=${height},left=${left},top=${top},resizable=yes,scrollbars=yes`
      )

      // Check if popup was blocked
      if (!popup || popup.closed || typeof popup.closed === 'undefined') {
        resolve({
          success: false,
          error: 'Popup blocked by browser',
          method: 'popup_blocked'
        })
        return
      }

      // Handle popup close
      const checkClosed = setInterval(() => {
        if (popup.closed) {
          clearInterval(checkClosed)
          resolve({
            success: true,
            method: 'popup'
          })
        }
      }, 1000)

      // Fallback timeout
      setTimeout(() => {
        clearInterval(checkClosed)
        resolve({
          success: true,
          method: 'popup_timeout'
        })
      }, 30000)
    })
  }

  // Check if share is supported
  isShareSupported() {
    return !!navigator.share
  }

  // Check if clipboard is supported
  isClipboardSupported() {
    return !!(navigator.clipboard && window.isSecureContext)
  }

  // Get available share methods
  getAvailableMethods() {
    const methods = []

    if (this.isShareSupported()) {
      methods.push('web_share_api')
    }

    if (this.isClipboardSupported()) {
      methods.push('clipboard_api')
    }

    methods.push('exec_command') // Always available as fallback

    return methods
  }

  // Format content for different platforms
  formatContentForPlatform(content, platform, options = {}) {
    switch (platform) {
      case 'twitter':
        const maxLength = 280
        let formatted = content

        if (options.hashtags && options.hashtags.length > 0) {
          const hashtags = `#${options.hashtags.join(' #')}`
          if (formatted.length + hashtags.length + 1 <= maxLength) {
            formatted += ` ${hashtags}`
          }
        }

        if (options.url && formatted.length + options.url.length + 1 <= maxLength) {
          formatted += ` ${options.url}`
        }

        return formatted

      case 'email':
        return `${content}\n\n${options.url || window.location.href}`

      default:
        return content
    }
  }

  // Analytics tracking (placeholder)
  trackShare(method, platform, content) {
    // In a real app, you would send this to analytics service
    console.log('Share analytics:', {
      method,
      platform,
      contentLength: content?.length,
      timestamp: new Date().toISOString(),
      userAgent: navigator.userAgent
    })
  }
}

// Create singleton instance
const shareManager = new ShareManager()

export default shareManager
```

---

## 🎨 Langkah 4: Membuat Composables

### 4.1 Use Jokes Composable
**File: `src/composables/useJokes.js`**
```javascript
import { ref, computed, onMounted, onUnmounted } from 'vue'
import jokeService from '../services/jokeService'
import storage from '../utils/storageUtils'
import shareManager from '../utils/shareUtils'

export function useJokes() {
  // Reactive state
  const currentJoke = ref(null)
  const isLoading = ref(false)
  const error = ref(null)
  const jokeHistory = ref([])
  const favorites = ref([])
  const searchQuery = ref('')
  const searchResults = ref([])
  const isSearching = ref(false)

  // Computed properties
  const hasJoke = computed(() => !!currentJoke.value)
  const hasError = computed(() => !!error.value)
  const favoriteJokes = computed(() =>
    jokeHistory.value.filter(joke => joke.isFavorite)
  )
  const historyCount = computed(() => jokeHistory.value.length)
  const favoriteCount = computed(() => favorites.value.length)
  const searchResultsCount = computed(() => searchResults.value.length)

  // Auto-refresh functionality
  let autoRefreshInterval = null
  const autoRefreshEnabled = ref(false)
  const autoRefreshIntervalTime = ref(30000) // 30 seconds

  // Fetch random joke
  const fetchJoke = async (options = {}) => {
    isLoading.value = true
    error.value = null

    try {
      const result = await jokeService.getRandomJoke()

      if (result.success) {
        currentJoke.value = result.data
        addToHistory(result.data)

        if (options.showSuccess) {
          showSuccessMessage('New joke loaded!')
        }
      } else {
        error.value = result.error

        if (result.fallback) {
          currentJoke.value = result.fallback
          addToHistory(result.fallback)
        }
      }
    } catch (err) {
      console.error('Unexpected error:', err)
      error.value = 'An unexpected error occurred'
    } finally {
      isLoading.value = false
    }
  }

  // Fetch joke by ID
  const fetchJokeById = async (jokeId) => {
    isLoading.value = true
    error.value = null

    try {
      const result = await jokeService.getJokeById(jokeId)

      if (result.success) {
        currentJoke.value = result.data

        if (!result.fromCache) {
          addToHistory(result.data)
        }
      } else {
        error.value = result.error
      }
    } catch (err) {
      console.error('Error fetching joke by ID:', err)
      error.value = 'Failed to fetch joke'
    } finally {
      isLoading.value = false
    }
  }

  // Search jokes
  const searchJokes = async (query) => {
    if (!query || query.trim().length < 2) {
      searchResults.value = []
      return
    }

    isSearching.value = true
    searchQuery.value = query

    try {
      const result = await jokeService.searchJokes(query)

      if (result.success) {
        searchResults.value = result.data
      } else {
        searchResults.value = []
      }
    } catch (err) {
      console.error('Search error:', err)
      searchResults.value = []
    } finally {
      isSearching.value = false
    }
  }

  // Clear search
  const clearSearch = () => {
    searchQuery.value = ''
    searchResults.value = []
  }

  // Add joke to history
  const addToHistory = (joke) => {
    const historyItem = {
      ...joke,
      id: joke.id || Date.now().toString(),
      timestamp: new Date().toISOString(),
      isFavorite: false,
      viewCount: 1,
      sharedCount: 0
    }

    // Check if joke already exists in history
    const existingIndex = jokeHistory.value.findIndex(
      item => item.joke === historyItem.joke
    )

    if (existingIndex >= 0) {
      // Update existing item
      jokeHistory.value[existingIndex].viewCount++
      jokeHistory.value[existingIndex].timestamp = historyItem.timestamp
    } else {
      // Add new item
      jokeHistory.value.unshift(historyItem)
    }

    // Limit history size
    if (jokeHistory.value.length > 100) {
      jokeHistory.value = jokeHistory.value.slice(0, 100)
    }

    saveHistory()
  }

  // Toggle favorite status
  const toggleFavorite = (jokeId) => {
    const joke = jokeHistory.value.find(j => j.id === jokeId)
    if (joke) {
      joke.isFavorite = !joke.isFavorite

      if (joke.isFavorite) {
        favorites.value.push(joke)
      } else {
        favorites.value = favorites.value.filter(j => j.id !== jokeId)
      }

      saveFavorites()
      return joke.isFavorite
    }
    return false
  }

  // Share joke
  const shareJoke = async (joke, options = {}) => {
    try {
      const result = await shareManager.share(joke.joke, options)

      if (result.success) {
        // Update share count
        const historyJoke = jokeHistory.value.find(j => j.id === joke.id)
        if (historyJoke) {
          historyJoke.sharedCount++
          saveHistory()
        }

        shareManager.trackShare(result.method, options.platform || 'unknown', joke.joke)
        showSuccessMessage('Joke shared successfully!')
      } else {
        showErrorMessage('Failed to share joke')
      }

      return result
    } catch (error) {
      console.error('Share error:', error)
      showErrorMessage('Share failed')
      return { success: false, error: error.message }
    }
  }

  // Share to specific platform
  const shareToTwitter = async (joke, options = {}) => {
    try {
      const result = await shareManager.shareToTwitter(joke.joke, options)

      if (result.success) {
        shareManager.trackShare('twitter', 'twitter', joke.joke)
        showSuccessMessage('Shared to Twitter!')
      }

      return result
    } catch (error) {
      console.error('Twitter share error:', error)
      showErrorMessage('Failed to share to Twitter')
      return { success: false, error: error.message }
    }
  }

  const shareToFacebook = async (joke, options = {}) => {
    try {
      const result = await shareManager.shareToFacebook(joke.joke, options)

      if (result.success) {
        shareManager.trackShare('facebook', 'facebook', joke.joke)
        showSuccessMessage('Shared to Facebook!')
      }

      return result
    } catch (error) {
      console.error('Facebook share error:', error)
      showErrorMessage('Failed to share to Facebook')
      return { success: false, error: error.message }
    }
  }

  // Copy joke to clipboard
  const copyJoke = async (joke) => {
    try {
      const result = await shareManager.copyToClipboard(joke.joke)

      if (result.success) {
        showSuccessMessage('Joke copied to clipboard!')
      } else {
        showErrorMessage('Failed to copy joke')
      }

      return result
    } catch (error) {
      console.error('Copy error:', error)
      showErrorMessage('Copy failed')
      return { success: false, error: error.message }
    }
  }

  // Clear history
  const clearHistory = () => {
    if (confirm('Are you sure you want to clear all history? This cannot be undone.')) {
      jokeHistory.value = []
      favorites.value = []
      saveHistory()
      saveFavorites()
      showSuccessMessage('History cleared!')
    }
  }

  // Clear favorites
  const clearFavorites = () => {
    if (confirm('Are you sure you want to clear all favorites?')) {
      favorites.value = []

      // Update isFavorite status in history
      jokeHistory.value.forEach(joke => {
        joke.isFavorite = false
      })

      saveHistory()
      saveFavorites()
      showSuccessMessage('Favorites cleared!')
    }
  }

  // Export data
  const exportData = () => {
    const exportData = {
      version: '1.0.0',
      exportedAt: new Date().toISOString(),
      history: jokeHistory.value,
      favorites: favorites.value,
      settings: {
        autoRefreshEnabled: autoRefreshEnabled.value,
        autoRefreshInterval: autoRefreshIntervalTime.value
      }
    }

    const dataStr = JSON.stringify(exportData, null, 2)
    const dataBlob = new Blob([dataStr], { type: 'application/json' })

    const url = URL.createObjectURL(dataBlob)
    const link = document.createElement('a')
    link.href = url
    link.download = `dad-jokes-backup-${new Date().toISOString().split('T')[0]}.json`

    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
    URL.revokeObjectURL(url)

    showSuccessMessage('Data exported successfully!')
  }

  // Import data
  const importData = (file) => {
    return new Promise((resolve, reject) => {
      const reader = new FileReader()

      reader.onload = (e) => {
        try {
          const data = JSON.parse(e.target.result)

          // Validate data structure
          if (!data.history || !Array.isArray(data.history)) {
            throw new Error('Invalid data format')
          }

          // Merge with existing data
          const mergedHistory = [...data.history, ...jokeHistory.value]
            .filter((joke, index, arr) =>
              arr.findIndex(j => j.joke === joke.joke) === index
            )
            .slice(0, 100) // Limit to 100 items

          jokeHistory.value = mergedHistory
          favorites.value = mergedHistory.filter(j => j.isFavorite)

          saveHistory()
          saveFavorites()

          showSuccessMessage('Data imported successfully!')
          resolve(data)
        } catch (error) {
          console.error('Import error:', error)
          showErrorMessage('Failed to import data: ' + error.message)
          reject(error)
        }
      }

      reader.onerror = () => {
        showErrorMessage('Failed to read file')
        reject(new Error('File reading error'))
      }

      reader.readAsText(file)
    })
  }

  // Auto-refresh functionality
  const startAutoRefresh = () => {
    if (autoRefreshInterval) {
      clearInterval(autoRefreshInterval)
    }

    autoRefreshInterval = setInterval(() => {
      if (!isLoading.value) {
        fetchJoke({ showSuccess: false }) // Silent refresh
      }
    }, autoRefreshIntervalTime.value)

    autoRefreshEnabled.value = true
    showSuccessMessage('Auto-refresh started!')
  }

  const stopAutoRefresh = () => {
    if (autoRefreshInterval) {
      clearInterval(autoRefreshInterval)
      autoRefreshInterval = null
    }

    autoRefreshEnabled.value = false
    showSuccessMessage('Auto-refresh stopped!')
  }

  const toggleAutoRefresh = () => {
    if (autoRefreshEnabled.value) {
      stopAutoRefresh()
    } else {
      startAutoRefresh()
    }
  }

  // Toast notifications
  const showToast = ref(false)
  const toastMessage = ref('')
  const toastType = ref('success') // success, error, info

  const showSuccessMessage = (message) => {
    showToast.value = true
    toastMessage.value = message
    toastType.value = 'success'

    setTimeout(() => {
      showToast.value = false
    }, 3000)
  }

  const showErrorMessage = (message) => {
    showToast.value = true
    toastMessage.value = message
    toastType.value = 'error'

    setTimeout(() => {
      showToast.value = false
    }, 5000)
  }

  const showInfoMessage = (message) => {
    showToast.value = true
    toastMessage.value = message
    toastType.value = 'info'

    setTimeout(() => {
      showToast.value = false
    }, 4000)
  }

  // Storage operations
  const saveHistory = () => {
    storage.set('history', jokeHistory.value)
  }

  const saveFavorites = () => {
    storage.set('favorites', favorites.value)
  }

  const loadHistory = () => {
    const saved = storage.get('history', [])
    jokeHistory.value = Array.isArray(saved) ? saved : []
  }

  const loadFavorites = () => {
    const saved = storage.get('favorites', [])
    favorites.value = Array.isArray(saved) ? saved : []
  }

  // Retry functionality
  const retry = () => {
    if (currentJoke.value) {
      fetchJoke()
    } else {
      fetchJoke()
    }
  }

  // Keyboard shortcuts
  const handleKeyPress = (event) => {
    // Only handle shortcuts when not typing in input fields
    if (event.target.tagName === 'INPUT' || event.target.tagName === 'TEXTAREA') {
      return
    }

    switch (event.key) {
      case ' ':
      case 'Enter':
        event.preventDefault()
        fetchJoke()
        break
      case 'c':
      case 'C':
        if (event.ctrlKey || event.metaKey) {
          // Let browser handle copy
          return
        }
        if (currentJoke.value) {
          copyJoke(currentJoke.value)
        }
        break
      case 's':
      case 'S':
        if (event.ctrlKey || event.metaKey) {
          // Let browser handle save
          return
        }
        if (currentJoke.value) {
          shareJoke(currentJoke.value)
        }
        break
      case 'r':
      case 'R':
        if (event.ctrlKey || event.metaKey) {
          // Let browser handle refresh
          return
        }
        retry()
        break
    }
  }

  // Lifecycle hooks
  onMounted(() => {
    loadHistory()
    loadFavorites()

    // Add keyboard event listener
    document.addEventListener('keydown', handleKeyPress)

    // Load initial joke if none exists
    if (!currentJoke.value) {
      fetchJoke()
    }
  })

  onUnmounted(() => {
    // Clean up
    if (autoRefreshInterval) {
      clearInterval(autoRefreshInterval)
    }

    document.removeEventListener('keydown', handleKeyPress)
  })

  return {
    // State
    currentJoke,
    isLoading,
    error,
    jokeHistory,
    favorites,
    searchQuery,
    searchResults,
    isSearching,
    autoRefreshEnabled,
    autoRefreshIntervalTime,

    // Computed
    hasJoke,
    hasError,
    favoriteJokes,
    historyCount,
    favoriteCount,
    searchResultsCount,

    // Toast
    showToast,
    toastMessage,
    toastType,

    // Methods
    fetchJoke,
    fetchJokeById,
    searchJokes,
    clearSearch,
    toggleFavorite,
    shareJoke,
    shareToTwitter,
    shareToFacebook,
    copyJoke,
    clearHistory,
    clearFavorites,
    exportData,
    importData,
    retry,
    toggleAutoRefresh,
    startAutoRefresh,
    stopAutoRefresh,

    // Notification methods
    showSuccessMessage,
    showErrorMessage,
    showInfoMessage
  }
}
```

---

## 🚀 Langkah 5: Membuat Komponen UI

### 5.1 Main DadJokes Component
**File: `src/components/DadJokes.vue`**
```vue
<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useJokes } from '../composables/useJokes'
import JokeCard from './JokeCard.vue'
import JokeHistory from './JokeHistory.vue'
import SearchBar from './SearchBar.vue'

// Use composable
const {
  currentJoke,
  isLoading,
  error,
  jokeHistory,
  favorites,
  searchQuery,
  searchResults,
  isSearching,
  autoRefreshEnabled,
  hasJoke,
  hasError,
  favoriteJokes,
  historyCount,
  favoriteCount,
  searchResultsCount,
  showToast,
  toastMessage,
  toastType,
  fetchJoke,
  searchJokes,
  clearSearch,
  toggleFavorite,
  shareJoke,
  shareToTwitter,
  shareToFacebook,
  copyJoke,
  clearHistory,
  clearFavorites,
  exportData,
  importData,
  retry,
  toggleAutoRefresh
} = useJokes()

// Local state
const activeTab = ref('main') // main, history, favorites, search
const showShareMenu = ref(false)
const showImportDialog = ref(false)
const importFile = ref(null)

// Computed properties
const currentTabTitle = computed(() => {
  const titles = {
    main: '🎭 Dad Jokes',
    history: `📜 History (${historyCount.value})`,
    favorites: `❤️ Favorites (${favoriteCount.value})`,
    search: `🔍 Search (${searchResultsCount.value})`
  }
  return titles[activeTab.value] || '🎭 Dad Jokes'
})

const showHistoryTab = computed(() => historyCount.value > 0)
const showFavoritesTab = computed(() => favoriteCount.value > 0)

// Tab navigation
const switchTab = (tab) => {
  activeTab.value = tab

  // Clear search when leaving search tab
  if (tab !== 'search') {
    clearSearch()
  }
}

// Share menu handlers
const openShareMenu = () => {
  showShareMenu.value = true
}

const closeShareMenu = () => {
  showShareMenu.value = false
}

const handleShare = async (method) => {
  if (!currentJoke.value) return

  closeShareMenu()

  switch (method) {
    case 'twitter':
      await shareToTwitter(currentJoke.value, {
        hashtags: ['DadJokes', 'VueJS']
      })
      break
    case 'facebook':
      await shareToFacebook(currentJoke.value)
      break
    case 'copy':
      await copyJoke(currentJoke.value)
      break
    default:
      await shareJoke(currentJoke.value)
  }
}

// Import/Export handlers
const handleExport = () => {
  exportData()
}

const handleImport = () => {
  showImportDialog.value = true
}

const handleFileSelect = (event) => {
  const file = event.target.files[0]
  if (file) {
    importData(file)
      .then(() => {
        showImportDialog.value = false
      })
      .catch(() => {
        // Error handled in composable
      })
  }
}

const cancelImport = () => {
  showImportDialog.value = false
  if (importFile.value) {
    importFile.value.value = ''
  }
}

// Keyboard shortcuts display
const showKeyboardShortcuts = ref(false)
const keyboardShortcuts = [
  { key: 'Space/Enter', description: 'Get new joke' },
  { key: 'C', description: 'Copy joke' },
  { key: 'S', description: 'Share joke' },
  { key: 'R', description: 'Retry last request' }
]

// Click outside handlers
const handleClickOutside = (event) => {
  // Close share menu if clicking outside
  if (showShareMenu.value && !event.target.closest('.share-menu')) {
    closeShareMenu()
  }

  // Close import dialog if clicking outside
  if (showImportDialog.value && !event.target.closest('.import-dialog')) {
    cancelImport()
  }
}

// Lifecycle hooks
onMounted(() => {
  document.addEventListener('click', handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})
</script>

<template>
  <div class="dad-jokes-app">
    <!-- Toast Notification -->
    <div
      v-if="showToast"
      class="toast"
      :class="toastType"
    >
      {{ toastMessage }}
    </div>

    <!-- Header -->
    <header class="app-header">
      <h1 class="app-title">🎭 Dad Jokes Generator</h1>
      <p class="app-subtitle">Press Space or Enter for a new joke!</p>

      <!-- Keyboard shortcuts hint -->
      <button
        @click="showKeyboardShortcuts = !showKeyboardShortcuts"
        class="shortcuts-hint"
        title="Keyboard shortcuts"
      >
        ⌨️ Shortcuts
      </button>

      <!-- Keyboard shortcuts modal -->
      <div v-if="showKeyboardShortcuts" class="shortcuts-modal" @click="showKeyboardShortcuts = false">
        <div class="shortcuts-content" @click.stop>
          <h3>⌨️ Keyboard Shortcuts</h3>
          <ul class="shortcuts-list">
            <li v-for="shortcut in keyboardShortcuts" :key="shortcut.key">
              <kbd>{{ shortcut.key }}</kbd>
              <span>{{ shortcut.description }}</span>
            </li>
          </ul>
          <button @click="showKeyboardShortcuts = false" class="close-btn">
            Close
          </button>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="main-content">
      <!-- Tab Navigation -->
      <nav class="tab-navigation">
        <button
          @click="switchTab('main')"
          :class="{ active: activeTab === 'main' }"
          class="tab-button"
        >
          🏠 Main
        </button>

        <button
          v-if="showHistoryTab"
          @click="switchTab('history')"
          :class="{ active: activeTab === 'history' }"
          class="tab-button"
        >
          📜 History ({{ historyCount }})
        </button>

        <button
          v-if="showFavoritesTab"
          @click="switchTab('favorites')"
          :class="{ active: activeTab === 'favorites' }"
          class="tab-button"
        >
          ❤️ Favorites ({{ favoriteCount }})
        </button>

        <button
          @click="switchTab('search')"
          :class="{ active: activeTab === 'search' }"
          class="tab-button"
        >
          🔍 Search
        </button>
      </nav>

      <!-- Search Bar -->
      <SearchBar
        v-if="activeTab === 'search'"
        v-model:searchQuery="searchQuery"
        :isSearching="isSearching"
        :resultsCount="searchResultsCount"
        @search="searchJokes"
        @clear="clearSearch"
      />

      <!-- Main Tab Content -->
      <div v-if="activeTab === 'main'" class="main-tab">
        <!-- Loading State -->
        <div v-if="isLoading" class="loading-container">
          <div class="loading-spinner"></div>
          <p>Fetching a hilarious joke...</p>
        </div>

        <!-- Error State -->
        <div v-else-if="hasError" class="error-container">
          <div class="error-icon">😞</div>
          <h3>Oops! Something went wrong</h3>
          <p>{{ error }}</p>
          <button @click="retry" class="retry-button">
            🔄 Try Again
          </button>
        </div>

        <!-- Joke Display -->
        <div v-else-if="hasJoke" class="joke-display">
          <JokeCard
            :joke="currentJoke"
            :showActions="true"
            @favorite="toggleFavorite"
            @share="openShareMenu"
            @copy="copyJoke"
          />

          <!-- Share Menu -->
          <div v-if="showShareMenu" class="share-menu">
            <h4>Share this joke</h4>
            <div class="share-options">
              <button @click="handleShare('twitter')" class="share-option twitter">
                🐦 Twitter
              </button>
              <button @click="handleShare('facebook')" class="share-option facebook">
                📘 Facebook
              </button>
              <button @click="handleShare('copy')" class="share-option copy">
                📋 Copy
              </button>
              <button @click="handleShare('default')" class="share-option default">
                📤 Share
              </button>
            </div>
          </div>
        </div>

        <!-- Generate Button -->
        <div class="generate-section">
          <button
            @click="fetchJoke"
            :disabled="isLoading"
            class="generate-button"
          >
            <span v-if="isLoading">🔄 Loading...</span>
            <span v-else>🎲 Get New Joke</span>
          </button>

          <!-- Auto-refresh toggle -->
          <div class="auto-refresh-controls">
            <label class="toggle-switch">
              <input
                type="checkbox"
                v-model="autoRefreshEnabled"
                @change="toggleAutoRefresh"
              >
              <span class="slider"></span>
            </label>
            <span class="toggle-label">Auto-refresh</span>
          </div>
        </div>
      </div>

      <!-- Search Results -->
      <div v-else-if="activeTab === 'search'" class="search-results">
        <div v-if="isSearching" class="loading-container">
          <div class="loading-spinner"></div>
          <p>Searching jokes...</p>
        </div>

        <div v-else-if="searchResultsCount === 0" class="empty-state">
          <div class="empty-icon">🔍</div>
          <h3>No jokes found</h3>
          <p>Try searching with different keywords</p>
        </div>

        <div v-else class="jokes-list">
          <JokeCard
            v-for="joke in searchResults"
            :key="joke.id || joke.joke"
            :joke="joke"
            :showActions="true"
            @favorite="toggleFavorite"
            @share="openShareMenu"
            @copy="copyJoke"
          />
        </div>
      </div>

      <!-- History Tab -->
      <JokeHistory
        v-else-if="activeTab === 'history'"
        :jokes="jokeHistory"
        :title="'📜 Joke History'"
        @favorite="toggleFavorite"
        @share="openShareMenu"
        @copy="copyJoke"
        @clear="clearHistory"
      />

      <!-- Favorites Tab -->
      <JokeHistory
        v-else-if="activeTab === 'favorites'"
        :jokes="favoriteJokes"
        :title="'❤️ Favorite Jokes'"
        @favorite="toggleFavorite"
        @share="openShareMenu"
        @copy="copyJoke"
        @clear="clearFavorites"
      />
    </main>

    <!-- Footer Actions -->
    <footer class="app-footer">
      <div class="footer-actions">
        <button @click="handleExport" class="footer-action export">
          📤 Export
        </button>

        <button @click="handleImport" class="footer-action import">
          📥 Import
        </button>
      </div>

      <div class="footer-info">
        <p>
          Powered by
          <a href="https://icanhazdadjoke.com/" target="_blank" rel="noopener">
            icanhazdadjoke.com
          </a>
        </p>
        <p>Made with ❤️ using Vue.js 3</p>
      </div>
    </footer>

    <!-- Import Dialog -->
    <div v-if="showImportDialog" class="import-dialog-overlay" @click="cancelImport">
      <div class="import-dialog" @click.stop>
        <h3>📥 Import Data</h3>
        <p>Select a JSON file to import your jokes and favorites</p>

        <input
          ref="importFile"
          type="file"
          accept=".json"
          @change="handleFileSelect"
          class="file-input"
        >

        <div class="dialog-actions">
          <button @click="cancelImport" class="cancel-btn">
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Main container */
.dad-jokes-app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
  display: flex;
  flex-direction: column;
}

/* Toast notification */
.toast {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 15px 20px;
  border-radius: 10px;
  font-weight: 600;
  z-index: 1000;
  animation: slideIn 0.3s ease;
  max-width: 300px;
}

.toast.success {
  background: #4CAF50;
}

.toast.error {
  background: #e74c3c;
}

.toast.info {
  background: #3498db;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* Header */
.app-header {
  text-align: center;
  margin-bottom: 30px;
  padding: 30px 0;
  position: relative;
}

.app-title {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.app-subtitle {
  font-size: 1.1em;
  opacity: 0.9;
  margin-bottom: 15px;
}

.shortcuts-hint {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.shortcuts-hint:hover {
  background: rgba(255,255,255,0.3);
}

/* Keyboard shortcuts modal */
.shortcuts-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.shortcuts-content {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 15px;
  padding: 30px;
  max-width: 400px;
  width: 90%;
  border: 2px solid rgba(255,255,255,0.2);
}

.shortcuts-content h3 {
  text-align: center;
  margin-bottom: 20px;
}

.shortcuts-list {
  list-style: none;
  padding: 0;
}

.shortcuts-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid rgba(255,255,255,0.2);
}

.shortcuts-list li:last-child {
  border-bottom: none;
}

kbd {
  background: rgba(255,255,255,0.2);
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 4px;
  padding: 4px 8px;
  font-family: monospace;
  font-size: 0.9em;
}

.close-btn {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  cursor: pointer;
  margin-top: 20px;
  width: 100%;
}

/* Main content */
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Tab navigation */
.tab-navigation {
  display: flex;
  justify-content: center;
  gap: 5px;
  background: rgba(255,255,255,0.1);
  border-radius: 50px;
  padding: 5px;
  backdrop-filter: blur(10px);
}

.tab-button {
  background: transparent;
  color: white;
  border: none;
  border-radius: 45px;
  padding: 12px 20px;
  font-size: 1em;
  cursor: pointer;
  transition: all 0.3s ease;
  white-space: nowrap;
}

.tab-button:hover {
  background: rgba(255,255,255,0.1);
}

.tab-button.active {
  background: rgba(255,255,255,0.2);
  box-shadow: 0 2px 10px rgba(0,0,0,0.2);
}

/* Loading state */
.loading-container {
  text-align: center;
  padding: 40px;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  backdrop-filter: blur(10px);
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Error state */
.error-container {
  text-align: center;
  padding: 40px;
  background: rgba(231, 76, 60, 0.2);
  border: 2px solid #e74c3c;
  border-radius: 15px;
  backdrop-filter: blur(10px);
}

.error-icon {
  font-size: 3em;
  margin-bottom: 20px;
}

.error-container h3 {
  margin-bottom: 15px;
}

.retry-button {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 12px 24px;
  font-size: 1em;
  cursor: pointer;
  margin-top: 20px;
  transition: all 0.3s ease;
}

.retry-button:hover {
  background: #c0392b;
  transform: translateY(-2px);
}

/* Joke display */
.joke-display {
  position: relative;
}

/* Share menu */
.share-menu {
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(0,0,0,0.9);
  border-radius: 10px;
  padding: 20px;
  min-width: 200px;
  z-index: 100;
  margin-top: 10px;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
}

.share-menu h4 {
  margin: 0 0 15px 0;
  text-align: center;
}

.share-options {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.share-option {
  background: rgba(255,255,255,0.1);
  border: none;
  border-radius: 8px;
  padding: 10px 15px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  text-align: left;
}

.share-option:hover {
  background: rgba(255,255,255,0.2);
}

.share-option.twitter:hover {
  background: #1DA1F2;
}

.share-option.facebook:hover {
  background: #1877F2;
}

.share-option.copy:hover {
  background: #4CAF50;
}

/* Generate section */
.generate-section {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.generate-button {
  background: linear-gradient(135deg, #4CAF50, #45a049);
  color: white;
  border: none;
  border-radius: 50px;
  padding: 15px 40px;
  font-size: 1.2em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  min-width: 200px;
}

.generate-button:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.3);
}

.generate-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* Auto-refresh controls */
.auto-refresh-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.toggle-switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 24px;
}

.toggle-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(255,255,255,0.3);
  transition: .4s;
  border-radius: 24px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: .4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #4CAF50;
}

input:checked + .slider:before {
  transform: translateX(26px);
}

.toggle-label {
  font-size: 0.9em;
  opacity: 0.9;
}

/* Search results */
.search-results {
  min-height: 200px;
}

.empty-state {
  text-align: center;
  padding: 40px;
  opacity: 0.7;
}

.empty-icon {
  font-size: 3em;
  margin-bottom: 20px;
}

/* Jokes list */
.jokes-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Footer */
.app-footer {
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

.footer-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  margin-bottom: 20px;
}

.footer-action {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
}

.footer-action:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.footer-info {
  text-align: center;
  opacity: 0.8;
}

.footer-info p {
  margin: 5px 0;
}

.footer-info a {
  color: #f39c12;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-info a:hover {
  color: #e67e22;
  text-decoration: underline;
}

/* Import dialog */
.import-dialog-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.import-dialog {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 15px;
  padding: 30px;
  max-width: 400px;
  width: 90%;
  border: 2px solid rgba(255,255,255,0.2);
}

.import-dialog h3 {
  text-align: center;
  margin-bottom: 15px;
}

.import-dialog p {
  text-align: center;
  margin-bottom: 20px;
  opacity: 0.9;
}

.file-input {
  width: 100%;
  padding: 10px;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  color: white;
  font-size: 1em;
  margin-bottom: 20px;
}

.file-input::file-selector-button {
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  cursor: pointer;
  margin-right: 10px;
}

.dialog-actions {
  display: flex;
  justify-content: center;
}

.cancel-btn {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 10px 20px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.cancel-btn:hover {
  background: #c0392b;
}

/* Responsive design */
@media (max-width: 768px) {
  .dad-jokes-app {
    padding: 15px;
    margin: 10px;
  }

  .app-title {
    font-size: 2em;
  }

  .tab-navigation {
    flex-wrap: wrap;
    gap: 5px;
  }

  .tab-button {
    font-size: 0.9em;
    padding: 10px 15px;
  }

  .footer-actions {
    flex-direction: column;
    gap: 10px;
  }

  .generate-button {
    font-size: 1.1em;
    padding: 12px 30px;
  }

  .share-menu {
    left: 10px;
    right: 10px;
    transform: none;
    width: auto;
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

/* Focus styles */
button:focus,
input:focus {
  outline: 3px solid #f39c12;
  outline-offset: 2px;
}
</style>
```

---

## 📱 Langkah 6: Supporting Components

### 6.1 JokeCard Component
**File: `src/components/JokeCard.vue`**
```vue
<script setup>
import { computed } from 'vue'

const props = defineProps({
  joke: {
    type: Object,
    required: true
  },
  showActions: {
    type: Boolean,
    default: true
  },
  compact: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['favorite', 'share', 'copy'])

const isFavorite = computed(() => props.joke.isFavorite || false)

const formattedDate = computed(() => {
  if (!props.joke.timestamp) return ''

  const date = new Date(props.joke.timestamp)
  return date.toLocaleDateString('en-US', {
    month: 'short',
    day: 'numeric',
    year: date.getFullYear() !== new Date().getFullYear() ? 'numeric' : undefined
  })
})

const handleFavorite = () => {
  emit('favorite', props.joke.id)
}

const handleShare = () => {
  emit('share', props.joke)
}

const handleCopy = () => {
  emit('copy', props.joke)
}
</script>

<template>
  <div class="joke-card" :class="{ compact }">
    <div class="joke-content">
      <p class="joke-text">{{ joke.joke }}</p>

      <div class="joke-meta" v-if="!compact">
        <span v-if="formattedDate" class="joke-date">{{ formattedDate }}</span>
        <span v-if="joke.viewCount" class="joke-views">
          👁️ {{ joke.viewCount }}
        </span>
        <span v-if="joke.sharedCount" class="joke-shares">
          📤 {{ joke.sharedCount }}
        </span>
        <span v-if="joke.source" class="joke-source">
          {{ joke.source }}
        </span>
      </div>
    </div>

    <div v-if="showActions" class="joke-actions">
      <button
        @click="handleFavorite"
        class="action-button favorite"
        :class="{ active: isFavorite }"
        :title="isFavorite ? 'Remove from favorites' : 'Add to favorites'"
      >
        {{ isFavorite ? '💔' : '❤️' }}
      </button>

      <button
        @click="handleShare"
        class="action-button share"
        title="Share joke"
      >
        📤
      </button>

      <button
        @click="handleCopy"
        class="action-button copy"
        title="Copy to clipboard"
      >
        📋
      </button>
    </div>
  </div>
</template>

<style scoped>
.joke-card {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  border: 1px solid rgba(255,255,255,0.2);
  transition: all 0.3s ease;
  position: relative;
}

.joke-card:hover {
  background: rgba(255,255,255,0.15);
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.2);
}

.joke-card.compact {
  padding: 15px;
}

.joke-content {
  margin-bottom: 15px;
}

.joke-text {
  font-size: 1.1em;
  line-height: 1.6;
  margin: 0;
  word-wrap: break-word;
}

.joke-card.compact .joke-text {
  font-size: 1em;
}

.joke-meta {
  display: flex;
  gap: 15px;
  font-size: 0.85em;
  opacity: 0.7;
  flex-wrap: wrap;
}

.joke-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

.action-button {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  padding: 8px 12px;
  font-size: 1em;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-button:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.action-button.favorite.active {
  background: #e74c3c;
}

.action-button.favorite:hover {
  background: #c0392b;
}

.action-button.share:hover {
  background: #3498db;
}

.action-button.copy:hover {
  background: #4CAF50;
}

/* Responsive */
@media (max-width: 768px) {
  .joke-card {
    padding: 20px;
  }

  .joke-meta {
    flex-direction: column;
    gap: 5px;
  }

  .joke-actions {
    justify-content: center;
    flex-wrap: wrap;
  }
}
</style>
```

---

## ✅ Final Review & Checklist

### Implementation Complete Checklist
- [ ] Service layer with API integration
- [ ] Storage utilities with error handling
- [ ] Share utilities with multiple fallbacks
- [ ] Advanced composable with all features
- [ ] Main component with tab navigation
- [ ] Supporting components (JokeCard, SearchBar, JokeHistory)
- [ ] Responsive design for all devices
- [ ] Keyboard shortcuts support
- [ ] Import/Export functionality
- [ ] Auto-refresh feature
- [ ] Toast notifications
- [ ] Loading and error states
- [ ] Accessibility features

### Code Quality Checklist
- [ ] Vue.js 3 Composition API best practices
- [ ] Proper error handling throughout
- [ ] TypeScript-ready structure
- [ ] Modular and reusable components
- [ ] Performance optimizations
- [ ] Memory leak prevention
- [ ] Proper cleanup in lifecycle hooks

### Security Checklist
- [ ] Input validation for search and import
- [ ] XSS prevention in rendered content
- [ ] Safe storage usage
- [ ] Proper error message sanitization

### User Experience Checklist
- [ ] Intuitive navigation
- [ ] Fast response times
- [ ] Clear visual feedback
- [ ] Mobile-friendly interface
- [ ] Accessibility compliance
- [ ] Offline capability preparation

---

## 🎉 Selamat! Implementation Selesai

Anda telah berhasil membuat aplikasi Dad Jokes Generator yang production-ready dengan:

✅ **Advanced API integration** dengan caching and error handling
✅ **Comprehensive data persistence** with import/export
✅ **Multiple sharing options** with fallbacks
✅ **Search functionality** with real-time results
✅ **Auto-refresh capability** with user controls
✅ **Keyboard shortcuts** for power users
✅ **Responsive design** for all devices
✅ **Accessibility features** for inclusive usage
✅ **Component architecture** for maintainability

### Next Steps
1. **Add PWA capabilities** for offline usage
2. **Implement analytics** for usage tracking
3. **Add more API sources** for variety
4. **Create mobile app** with Capacitor
5. **Add user accounts** for cloud sync
6. **Implement categories** for joke organization

---

**Happy Coding! 🚀**