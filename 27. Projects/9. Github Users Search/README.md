# GitHub Users Search - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project GitHub Users Search adalah aplikasi web yang memungkinkan pengguna untuk mencari dan menampilkan profil pengguna GitHub secara real-time menggunakan Vue.js 3 dengan Composition API. Aplikasi ini mendemonstrasikan cara kerja API integration, data fetching, asynchronous programming, dan state management. Project ini sangat cocok untuk pemula yang ingin mempelajari cara mengintegrasikan aplikasi Vue.js dengan REST API eksternal.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- HTTP requests dengan Fetch API dan Axios
- Asynchronous JavaScript dengan async/await
- API integration dengan GitHub API
- Error handling untuk API calls
- Conditional rendering dengan `v-if`
- Event handling dengan `@input` dan `@click`
- Form handling dan user input validation
- Loading states dan user feedback
- Component-based architecture
- API rate limiting dan best practices
- State management untuk asynchronous data
- Data transformation dan formatting

## 📋 Fitur Aplikasi

1. **Real-time Search**: Mencari pengguna GitHub secara real-time
2. **Profile Display**: Menampilkan profil pengguna lengkap dengan avatar
3. **User Details**: Menampilkan informasi detail (lokasi, followers, repositories)
4. **Error Handling**: Menangani error saat pengguna tidak ditemukan
5. **Loading States**: Visual feedback saat mengambil data
6. **Input Validation**: Validasi input username
7. **Responsive Design**: Layout yang bekerja di semua device sizes
8. **Clean UI**: Interface yang modern dan user-friendly
9. **Search History**: Menyimpan history pencarian (enhanced)
10. **Multiple Results**: Menampilkan hasil pencarian multiple (enhanced)

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)
- Koneksi internet untuk mengakses GitHub API

## 📂 Struktur Project

```
9. Github Users Search/
├── App.vue                              # Komponen utama aplikasi
├── components/
│   └── GithubUsersSearch.vue            # Komponen search profil GitHub
├── assets/
│   └── styles/
│       └── main.css                    # Global styles
├── utils/
│   └── githubApi.js                    # API helper functions
└── README.md                           # Dokumentasi project
```

## 🚀 Langkah-Langkah Pembuatan Project

### Langkah 1: Setup Environment

1. **Install Node.js**
   - Download Node.js dari [nodejs.org](https://nodejs.org/)
   - Pilih versi LTS (Long Term Support)
   - Ikuti proses instalasi sesuai sistem operasi Anda

2. **Verifikasi Instalasi**
   ```bash
   node --version
   npm --version
   ```

3. **Install Vue CLI** (opsional, jika ingin menggunakan template)
   ```bash
   npm install -g @vue/cli
   ```

### Langkah 2: Membuat Project Baru

**Cara 1: Menggunakan Vite (Direkomendasikan)**
```bash
npm create vue@latest github-users-search-app
cd github-users-search-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create github-users-search-app
cd github-users-search-app
```

### Langkah 3: Memahami GitHub API

**GitHub API Basics:**
- **Endpoint**: `https://api.github.com/users/{username}`
- **Rate Limiting**: 60 requests per hour untuk unauthenticated requests
- **Response Format**: JSON dengan user profile data
- **HTTP Methods**: GET untuk mengambil data

**Response Fields:**
- `login`: Username GitHub
- `avatar_url`: URL avatar pengguna
- `name`: Nama lengkap pengguna
- `location`: Lokasi pengguna
- `followers`: Jumlah followers
- `following`: Jumlah following
- `public_repos`: Jumlah repository publik
- `bio`: Bio pengguna
- `company`: Perusahaan
- `blog`: Blog/website
- `created_at`: Tanggal pembuatan akun
- `updated_at`: Terakhir update profil

### Langkah 4: Membuat Komponen GithubUsersSearch

**File: `components/GithubUsersSearch.vue`**

#### Bagian Script Setup - Reactive Data & API Integration
```vue
<script setup>
import { ref, computed, watch, onMounted } from "vue";

// Reactive state
const username = ref("");
const userProfile = ref(null);
const isLoading = ref(false);
const error = ref(null);
const searchHistory = ref([]);
const searchResults = ref([]);
const showHistory = ref(false);

// Computed properties
const hasProfile = computed(() => !!userProfile.value);
const hasError = computed(() => !!error.value);
const hasHistory = computed(() => searchHistory.value.length > 0);

// API Configuration
const API_BASE_URL = "https://api.github.com/users/";
const DEBOUNCE_DELAY = 500; // milliseconds

// Debounce function untuk menghindari terlalu banyak API calls
let debounceTimer = null;
const debouncedSearch = (callback) => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(callback, DEBOUNCE_DELAY);
};

// Main search function
const getUserProfile = async (searchUsername = null) => {
  const targetUsername = searchUsername || username.value;

  if (!targetUsername || targetUsername.trim().length < 1) {
    error.value = "Please enter a valid username";
    userProfile.value = null;
    return;
  }

  isLoading.value = true;
  error.value = null;

  try {
    const response = await fetch(`${API_BASE_URL}${targetUsername}`);

    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.message || `User not found (HTTP ${response.status})`);
    }

    const data = await response.json();

    // Process user data
    const processedData = {
      ...data,
      // Format date
      created_at_formatted: formatDate(data.created_at),
      updated_at_formatted: formatDate(data.updated_at),
      // Format large numbers
      followers_formatted: formatNumber(data.followers),
      following_formatted: formatNumber(data.following),
      public_repos_formatted: formatNumber(data.public_repos)
    };

    userProfile.value = processedData;
    addToHistory(targetUsername);

  } catch (err) {
    console.error("Error fetching user profile:", err);
    error.value = err.message || "Failed to fetch user profile";
    userProfile.value = null;
  } finally {
    isLoading.value = false;
  }
};

// Search multiple users (enhanced feature)
const searchMultipleUsers = async (query) => {
  isLoading.value = true;
  error.value = null;
  searchResults.value = [];

  try {
    // GitHub Search API untuk multiple users
    const searchResponse = await fetch(
      `https://api.github.com/search/users?q=${encodeURIComponent(query)}&per_page=10`
    );

    if (!searchResponse.ok) {
      throw new Error("Search failed");
    }

    const searchData = await searchResponse.json();

    // Get detailed profile for each user
    const detailedProfiles = await Promise.all(
      searchData.items.map(async (user) => {
        try {
          const profileResponse = await fetch(user.url);
          const profileData = await profileResponse.json();

          return {
            ...profileData,
            created_at_formatted: formatDate(profileData.created_at),
            followers_formatted: formatNumber(profileData.followers),
            public_repos_formatted: formatNumber(profileData.public_repos)
          };
        } catch (error) {
          console.error(`Error fetching profile for ${user.login}:`, error);
          return null;
        }
      })
    );

    searchResults.value = detailedProfiles.filter(profile => profile !== null);

  } catch (err) {
    console.error("Error searching users:", err);
    error.value = err.message || "Search failed";
  } finally {
    isLoading.value = false;
  }
};

// Helper functions
const formatDate = (dateString) => {
  const date = new Date(dateString);
  return date.toLocaleDateString('en-US', {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  });
};

const formatNumber = (num) => {
  if (num >= 1000000) {
    return (num / 1000000).toFixed(1) + 'M';
  } else if (num >= 1000) {
    return (num / 1000).toFixed(1) + 'K';
  }
  return num.toString();
};

const addToHistory = (searchedUsername) => {
  const historyItem = {
    username: searchedUsername,
    timestamp: new Date().toISOString(),
    userProfile: userProfile.value ? { ...userProfile.value } : null
  };

  // Check if username already exists in history
  const existingIndex = searchHistory.value.findIndex(
    item => item.username.toLowerCase() === searchedUsername.toLowerCase()
  );

  if (existingIndex >= 0) {
    // Move existing item to top
    searchHistory.value.splice(existingIndex, 1);
  }

  searchHistory.value.unshift(historyItem);

  // Limit history to 20 items
  if (searchHistory.value.length > 20) {
    searchHistory.value = searchHistory.value.slice(0, 20);
  }

  // Save to localStorage
  saveToLocalStorage();
};

const saveToLocalStorage = () => {
  try {
    localStorage.setItem('githubSearchHistory', JSON.stringify(searchHistory.value));
  } catch (error) {
    console.error('Error saving to localStorage:', error);
  }
};

const loadFromLocalStorage = () => {
  try {
    const saved = localStorage.getItem('githubSearchHistory');
    if (saved) {
      searchHistory.value = JSON.parse(saved);
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
};

const clearHistory = () => {
  if (confirm('Are you sure you want to clear search history?')) {
    searchHistory.value = [];
    localStorage.removeItem('githubSearchHistory');
  }
};

const searchFromHistory = (historyItem) => {
  username.value = historyItem.username;
  userProfile.value = historyItem.userProfile ? { ...historyItem.userProfile } : null;
  showHistory.value = false;

  // Refresh the data to get latest information
  if (historyItem.userProfile) {
    getUserProfile(historyItem.username);
  }
};

const toggleHistory = () => {
  showHistory.value = !showHistory.value;
};

// Auto-search when username changes (with debounce)
watch(username, (newUsername) => {
  if (newUsername && newUsername.trim().length > 0) {
    debouncedSearch(() => {
      getUserProfile();
    });
  }
});

// Keyboard shortcuts
const handleKeyPress = (event) => {
  if (event.key === 'Enter') {
    getUserProfile();
  } else if (event.key === 'Escape') {
    showHistory.value = false;
  }
};

// Initialize
onMounted(() => {
  loadFromLocalStorage();
});
</script>
```

#### Penjelasan Kode Script:

1. **`import { ref, computed, watch, onMounted } from "vue"`**
   - Mengimpor Vue.js reactivity functions
   - `ref` untuk membuat reactive variables
   - `computed` untuk derived state
   - `watch` untuk reaksi terhadap perubahan data
   - `onMounted` untuk lifecycle hook

2. **Reactive Variables dengan `ref()`**
   - `username`: Input username dari user
   - `userProfile`: Data profil pengguna GitHub
   - `isLoading`: Status loading untuk user feedback
   - `error`: Pesan error jika API gagal
   - `searchHistory`: History pencarian pengguna
   - `searchResults`: Hasil pencarian multiple users

3. **`getUserProfile()` Function**
   - Mengambil data profil dari GitHub API
   - Error handling dengan try-catch-finally
   - Data processing dan formatting
   - Menambahkan ke history

4. **Debouncing**
   - Mencegah terlalu banyak API calls
   - Delay 500ms setelah user berhenti mengetik
   - Meningkatkan performance dan user experience

5. **Helper Functions**
   - `formatDate()`: Format tanggal ke readable format
   - `formatNumber()`: Format angka besar (1K, 1M)
   - `addToHistory()`: Tambahkan ke search history
   - `saveToLocalStorage()`: Simpan data ke browser storage

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="github-search-container">
    <!-- Header -->
    <header class="search-header">
      <h1 class="app-title">🔍 GitHub Users Search</h1>
      <p class="app-subtitle">Find and explore GitHub profiles instantly</p>
    </header>

    <!-- Search Section -->
    <section class="search-section">
      <div class="search-input-container">
        <div class="input-group">
          <input
            v-model="username"
            type="text"
            placeholder="Enter GitHub username..."
            class="search-input"
            @keypress="handleKeyPress"
            @input="showHistory = false"
          />
          <button
            @click="getUserProfile"
            :disabled="isLoading || !username.trim()"
            class="search-button"
          >
            <span v-if="isLoading" class="loading-spinner">🔄</span>
            <span v-else>🔍</span>
          </button>
        </div>

        <!-- History toggle -->
        <div class="search-controls">
          <button
            v-if="hasHistory"
            @click="toggleHistory"
            class="history-toggle"
            :class="{ active: showHistory }"
          >
            📜 History ({{ searchHistory.length }})
          </button>

          <button
            v-if="hasHistory"
            @click="clearHistory"
            class="clear-history"
            title="Clear search history"
          >
            🗑️ Clear
          </button>
        </div>
      </div>

      <!-- Search History -->
      <div v-if="showHistory && hasHistory" class="search-history">
        <h3>Recent Searches</h3>
        <ul class="history-list">
          <li
            v-for="item in searchHistory.slice(0, 10)"
            :key="item.username"
            @click="searchFromHistory(item)"
            class="history-item"
          >
            <img
              v-if="item.userProfile?.avatar_url"
              :src="item.userProfile.avatar_url"
              :alt="item.username"
              class="history-avatar"
            />
            <span class="history-username">{{ item.username }}</span>
            <span class="history-time">{{ formatTime(item.timestamp) }}</span>
          </li>
        </ul>
      </div>

      <!-- Loading State -->
      <div v-if="isLoading" class="loading-container">
        <div class="loading-spinner-large"></div>
        <p>Fetching GitHub profile...</p>
      </div>

      <!-- Error State -->
      <div v-else-if="hasError" class="error-container">
        <div class="error-icon">😞</div>
        <h3>Oops! Something went wrong</h3>
        <p>{{ error }}</p>
        <button @click="getUserProfile" class="retry-button">
          🔄 Try Again
        </button>
      </div>

      <!-- User Profile Display -->
      <div v-else-if="hasProfile" class="user-profile">
        <div class="profile-header">
          <img
            :src="userProfile.avatar_url"
            :alt="userProfile.login"
            class="user-avatar"
            @error="handleImageError"
          />

          <div class="user-basic-info">
            <h2 class="user-name">
              {{ userProfile.name || userProfile.login }}
              <a
                :href="userProfile.html_url"
                target="_blank"
                rel="noopener noreferrer"
                class="github-link"
                title="View on GitHub"
              >
                <svg class="github-icon" viewBox="0 0 24 24" width="20" height="20">
                  <path fill="currentColor" d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957.266 1.783.693 2.74 1.236 1.548.865 3.346.862 5.223-.006 2.094-.262 3.814-.789 4.768-.525 1.424-1.274 2.613-2.279 1.842-1.004 2.347-1.831 2.391-2.83.024-.567.049-1.16.049-1.755 0-1.991-.181-3.648-.967-4.696-1.865-.417-.353-.795-.722-1.146-1.092-.322-.332-.548-.717-.683-1.125-.082-.25-.085-.521-.015-.78.069-.258.198-.522.388-.788.261-.393.427-.783.427-1.274 0-.828-.463-1.562-1.263-1.562-1.262 0-.874.65-1.753 1.651-2.023.647-.198 1.011-.511 1.651-.511 1.044 0 1.891.864 1.891 1.93 0 1.07-.848 1.93-1.891 1.93-.64 0-1.004.313-1.651.511-1.001.27-1.651 1.149-1.651 2.023 0 1.115.668 1.736 1.587 1.842.146.125.336.2.584.215.317.091.638.055.957-.093.411-.334.635-.661.635-1.089 0-.628-.37-1.268-1.092-1.268-1.063 0-1.924.873-1.924 1.95 0 .711.529 1.671.865 1.916.231.245.354.546.354.874 0 .418-.299.836-.592 1.286-.335.447-.665.862-.973 1.235-.307.371-.594.722-.861 1.033-.267.311-.517.588-.75.839-.233.251-.447.476-.643.676-.196.2-.379.375-.549.527-.17.152-.324.273-.463.36-.139.087-.267.152-.383.194-.115.042-.221.061-.317.061-.193 0-.341-.088-.447-.264-.105-.176-.164-.398-.164-.673 0-.581.433-1.181 1.283-1.78.848-.597 1.281-1.179 1.281-1.78 0-.582-.433-1.181-1.283-1.78-.848-.597-1.281-1.179-1.281-1.78 0-.582.433-1.181 1.283-1.78.848-.597 1.281-1.179 1.281-1.78z"/>
                </svg>
              </a>
            </h2>

            <p class="user-bio" v-if="userProfile.bio">
              {{ userProfile.bio }}
            </p>

            <div class="user-meta">
              <span v-if="userProfile.company" class="meta-item">
                🏢 {{ userProfile.company }}
              </span>
              <span v-if="userProfile.location" class="meta-item">
                📍 {{ userProfile.location }}
              </span>
              <span v-if="userProfile.blog" class="meta-item">
                🔗
                <a
                  :href="userProfile.blog.startsWith('http') ? userProfile.blog : `https://${userProfile.blog}`"
                  target="_blank"
                  rel="noopener noreferrer"
                  class="blog-link"
                >
                  {{ userProfile.blog }}
                </a>
              </span>
            </div>
          </div>
        </div>

        <!-- Stats Grid -->
        <div class="stats-grid">
          <div class="stat-item">
            <div class="stat-number">{{ userProfile.followers_formatted }}</div>
            <div class="stat-label">Followers</div>
          </div>
          <div class="stat-item">
            <div class="stat-number">{{ userProfile.following_formatted }}</div>
            <div class="stat-label">Following</div>
          </div>
          <div class="stat-item">
            <div class="stat-number">{{ userProfile.public_repos_formatted }}</div>
            <div class="stat-label">Public Repos</div>
          </div>
        </div>

        <!-- Additional Info -->
        <div class="additional-info">
          <div class="info-row">
            <span class="info-label">📅 Member Since:</span>
            <span class="info-value">{{ userProfile.created_at_formatted }}</span>
          </div>
          <div class="info-row">
            <span class="info-label">🔄 Last Updated:</span>
            <span class="info-value">{{ userProfile.updated_at_formatted }}</span>
          </div>
          <div class="info-row" v-if="userProfile.twitter_username">
            <span class="info-label">🐦 Twitter:</span>
            <span class="info-value">@{{ userProfile.twitter_username }}</span>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div v-else class="empty-state">
        <div class="empty-icon">👤</div>
        <h3>Enter a GitHub username</h3>
        <p>Search for any GitHub user to see their profile information</p>
      </div>
    </section>

    <!-- Footer -->
    <footer class="search-footer">
      <p>
        Powered by
        <a href="https://docs.github.com/en/rest/users/users#get-a-user" target="_blank" rel="noopener">
          GitHub API
        </a>
      </p>
      <p>Made with ❤️ using Vue.js 3</p>
    </footer>
  </div>
</template>
```

#### Penjelasan Template:

1. **Header Section**
   - Menampilkan judul dan deskripsi aplikasi
   - Branding dengan emoji untuk visual appeal

2. **Search Section**
   - Input field dengan debouncing
   - Search button dengan loading state
   - History toggle dan clear buttons
   - Real-time search results

3. **Search History**
   - Dropdown history dengan avatar thumbnails
   - Click to search dari history
   - Timestamp untuk setiap pencarian

4. **Loading State**
   - Loading spinner dengan visual feedback
   - Prevent duplicate requests

5. **Error State**
   - User-friendly error messages
   - Retry button untuk mencoba lagi

6. **User Profile Display**
   - Avatar dengan error handling
   - User basic info dengan GitHub link
   - Stats grid untuk followers, following, repos
   - Additional info dengan formatted dates

7. **Empty State**
   - Helpful placeholder untuk user guidance

### Langkah 6: Advanced CSS Styling

```vue
<style scoped>
/* Main Container */
.github-search-container {
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

/* Header Styles */
.search-header {
  text-align: center;
  margin-bottom: 30px;
  padding: 30px 0;
}

.app-title {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.app-subtitle {
  font-size: 1.1em;
  opacity: 0.9;
}

/* Search Section */
.search-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.search-input-container {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  border: 1px solid rgba(255,255,255,0.2);
}

.input-group {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.search-input {
  flex: 1;
  padding: 15px 20px;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 10px;
  background: rgba(255,255,255,0.1);
  color: white;
  font-size: 1.1em;
  transition: all 0.3s ease;
}

.search-input::placeholder {
  color: rgba(255,255,255,0.7);
}

.search-input:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.2);
}

.search-button {
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 15px 25px;
  font-size: 1.1em;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search-button:hover:not(:disabled) {
  background: #45a049;
  transform: translateY(-2px);
}

.search-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

.loading-spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.search-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

.history-toggle {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.history-toggle:hover {
  background: rgba(255,255,255,0.3);
}

.history-toggle.active {
  background: #4CAF50;
}

.clear-history {
  background: transparent;
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.clear-history:hover {
  background: rgba(231, 76, 60, 0.3);
  border-color: #e74c3c;
}

/* Search History */
.search-history {
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
}

.search-history h3 {
  margin: 0 0 15px 0;
  font-size: 1.1em;
}

.history-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.history-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.history-item:hover {
  background: rgba(255,255,255,0.1);
}

.history-avatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  object-fit: cover;
}

.history-username {
  flex: 1;
  font-weight: 600;
}

.history-time {
  font-size: 0.8em;
  opacity: 0.7;
}

/* Loading State */
.loading-container {
  text-align: center;
  padding: 40px;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  backdrop-filter: blur(10px);
}

.loading-spinner-large {
  width: 50px;
  height: 50px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

/* Error State */
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

/* User Profile */
.user-profile {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.profile-header {
  display: flex;
  align-items: flex-start;
  gap: 25px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.user-avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid rgba(255,255,255,0.2);
  transition: transform 0.3s ease;
}

.user-avatar:hover {
  transform: scale(1.05);
}

.user-basic-info {
  flex: 1;
  min-width: 200px;
}

.user-name {
  font-size: 2em;
  margin: 0 0 10px 0;
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.github-link {
  color: white;
  text-decoration: none;
  transition: all 0.3s ease;
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

.github-link:hover {
  transform: translateY(-2px);
}

.github-icon {
  transition: transform 0.3s ease;
}

.github-link:hover .github-icon {
  transform: rotate(360deg);
}

.user-bio {
  margin: 0 0 15px 0;
  line-height: 1.6;
  opacity: 0.9;
}

.user-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 15px;
}

.meta-item {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: 0.9em;
  opacity: 0.8;
}

.blog-link {
  color: #4CAF50;
  text-decoration: none;
  transition: color 0.3s ease;
}

.blog-link:hover {
  color: #45a049;
  text-decoration: underline;
}

/* Stats Grid */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.stat-item {
  text-align: center;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 20px;
  transition: all 0.3s ease;
}

.stat-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateY(-2px);
}

.stat-number {
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 5px;
  color: #4CAF50;
}

.stat-label {
  font-size: 0.9em;
  opacity: 0.8;
}

/* Additional Info */
.additional-info {
  border-top: 1px solid rgba(255,255,255,0.2);
  padding-top: 20px;
}

.info-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.info-row:last-child {
  border-bottom: none;
}

.info-label {
  font-weight: 600;
  opacity: 0.8;
}

.info-value {
  text-align: right;
}

/* Empty State */
.empty-state {
  text-align: center;
  padding: 60px 20px;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  backdrop-filter: blur(10px);
}

.empty-icon {
  font-size: 4em;
  margin-bottom: 20px;
}

/* Footer */
.search-footer {
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
  text-align: center;
  opacity: 0.8;
}

.search-footer p {
  margin: 5px 0;
}

.search-footer a {
  color: #4CAF50;
  text-decoration: none;
  transition: color 0.3s ease;
}

.search-footer a:hover {
  color: #45a049;
  text-decoration: underline;
}

/* Responsive Design */
@media (max-width: 768px) {
  .github-search-container {
    padding: 15px;
    margin: 10px;
  }

  .app-title {
    font-size: 2em;
  }

  .profile-header {
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 20px;
  }

  .input-group {
    flex-direction: column;
  }

  .search-button {
    width: 100%;
  }

  .stats-grid {
    grid-template-columns: 1fr;
  }

  .user-meta {
    justify-content: center;
  }

  .info-row {
    flex-direction: column;
    gap: 5px;
    text-align: center;
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

/* Focus Styles */
input:focus,
button:focus {
  outline: 3px solid #4CAF50;
  outline-offset: 2px;
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  .github-search-container {
    background: #000;
    color: #fff;
  }

  .search-input-container,
  .user-profile {
    border: 2px solid #fff;
  }
}
</style>
```

---

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API dengan `<script setup>`
- Syntax yang lebih ringkas dan modern
- Reaktivitas yang lebih mudah dipahami
- Better TypeScript support

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- State management untuk form inputs dan API results
- Reactive data untuk history dan search results

### 3. Computed Properties
- Menghitung derived state secara otomatis
- Conditional rendering logic
- Formatted data untuk display

### 4. Watchers (`watch`)
- Reaksi terhadap perubahan data
- Auto-search dengan debouncing
- Side effects management

### 5. Asynchronous JavaScript
- `async/await` syntax untuk API calls
- Promise handling dengan try-catch-finally
- Error handling yang robust

### 6. HTTP Requests
- Fetch API untuk HTTP requests
- GitHub API integration
- Response handling dan parsing

### 7. Event Handling
- `@input` untuk real-time search
- `@click` untuk button interactions
- `@keypress` untuk keyboard shortcuts

### 8. Conditional Rendering
- `v-if` untuk loading/error/empty states
- `v-else` untuk alternative content
- Dynamic template rendering

### 9. Component Lifecycle
- `onMounted` untuk initialization
- Data loading dari localStorage
- Setup dan cleanup operations

### 10. Browser API Integration
- LocalStorage untuk data persistence
- Fetch API untuk HTTP requests
- Error handling dengan browser APIs

## 🔧 Enhancement Ideas

### 1. Multiple User Search
```vue
<script setup>
const searchQuery = ref('');
const searchResults = ref([]);
const isSearching = ref(false);

const searchUsers = async () => {
  isSearching.value = true;
  try {
    const response = await fetch(
      `https://api.github.com/search/users?q=${encodeURIComponent(searchQuery.value)}&per_page=10`
    );
    const data = await response.json();
    searchResults.value = data.items;
  } catch (error) {
    console.error('Search error:', error);
  } finally {
    isSearching.value = false;
  }
};
</script>
```

### 2. Repository Information
```vue
<script setup>
const userRepos = ref([]);

const getUserRepos = async (username) => {
  try {
    const response = await fetch(
      `https://api.github.com/users/${username}/repos?per_page=5&sort=updated`
    );
    const data = await response.json();
    userRepos.value = data;
  } catch (error) {
    console.error('Error fetching repos:', error);
  }
};
</script>
```

### 3. User Analytics
```vue
<script setup>
const userStats = computed(() => {
  if (!userProfile.value) return null;

  return {
    accountAge: calculateAccountAge(userProfile.value.created_at),
    activityScore: calculateActivityScore(userProfile.value),
    contributionLevel: getContributionLevel(userProfile.value.public_repos)
  };
});

const calculateAccountAge = (createdAt) => {
  const created = new Date(createdAt);
  const now = new Date();
  const diffTime = Math.abs(now - created);
  const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
  return diffDays;
};
</script>
```

### 4. Advanced Search Filters
```vue
<script setup>
const searchFilters = ref({
  type: 'users', // users, repositories
  sort: 'best-match',
  order: 'desc',
  language: ''
});

const advancedSearch = async () => {
  const params = new URLSearchParams({
    q: buildSearchQuery(),
    sort: searchFilters.value.sort,
    order: searchFilters.value.order
  });

  const response = await fetch(
    `https://api.github.com/search/${searchFilters.value.type}?${params}`
  );
  // Process results...
};
</script>
```

### 5. Caching Strategy
```vue
<script setup>
const cache = ref(new Map());
const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes

const getCachedUser = async (username) => {
  const cached = cache.value.get(username);
  if (cached && Date.now() - cached.timestamp < CACHE_DURATION) {
    return cached.data;
  }

  // Fetch from API
  const userData = await getUserProfile(username);
  cache.value.set(username, {
    data: userData,
    timestamp: Date.now()
  });

  return userData;
};
</script>
```

## 🐞 Common Issues & Troubleshooting

### Issue 1: API Rate Limiting
**Symptoms**: Error "API rate limit exceeded"
**Solutions**:
- Implement caching untuk reduce API calls
- Add delay antar requests
- Show user-friendly rate limit message
- Implement retry logic with exponential backoff

### Issue 2: User Not Found
**Symptoms**: Error "User not found" atau 404 status
**Solutions**:
- Validate username format sebelum API call
- Handle 404 responses gracefully
- Suggest similar usernames
- Provide helpful error messages

### Issue 3: Network Errors
**Symptoms**: "Failed to fetch" atau network timeout
**Solutions**:
- Add retry mechanism
- Show offline indicator
- Implement fallback functionality
- Check internet connection

### Issue 4: Image Loading Errors
**Symptoms**: Avatar tidak muncul
**Solutions**:
- Add error handling untuk image load
- Implement fallback avatar
- Use lazy loading untuk performance
- Add loading skeleton

### Issue 5: Responsive Design Issues
**Symptoms**: Layout broken pada mobile devices
**Solutions**:
- Test pada berbagai screen sizes
- Use CSS Grid dan Flexbox
- Implement mobile-first design
- Add touch-friendly interactions

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [GitHub API Documentation](https://docs.github.com/en/rest/)
- [Fetch API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Async/Await Guide](https://javascript.info/async-await)
- [LocalStorage API](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [CSS Grid and Flexbox](https://css-tricks.com/snippets/css/complete-guide-grid/)
- [Responsive Web Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Implementasi komponen GithubUsersSearch
- [ ] Tambahkan reactive data untuk form inputs
- [ ] Implementasi GitHub API integration
- [ ] Tambahkan error handling
- [ ] Implementasi loading states
- [ ] Tambahkan search history functionality
- [ ] Implementasi localStorage persistence
- [ ] Tambahkan responsive design
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🔧 Troubleshooting & Common Issues

### Common Problems and Solutions

#### 1. API Rate Limit Error
**Problem**: Error message "API rate limit exceeded"
**Cause**: GitHub API has rate limits (60 requests/hour for unauthenticated requests)
**Solution**:
- Gunakan GitHub Personal Access Token
- Tambahkan token ke request headers:
```javascript
const response = await fetch(`https://api.github.com/users/${username}`, {
  headers: {
    'Authorization': `token YOUR_GITHUB_TOKEN`
  }
});
```

#### 2. CORS Error
**Problem**: CORS policy error di browser console
**Cause**: Browser security policy untuk cross-origin requests
**Solution**:
- Untuk development, gunakan CORS browser extension
- Untuk production, gunakan proxy server
- Atau gunakan GitHub token untuk authenticated requests

#### 3. Network Error
**Problem**: "Failed to fetch" atau "Network error"
**Cause**: Masalah koneksi internet atau GitHub API sedang down
**Solution**:
- Periksa koneksi internet
- Cek GitHub API status di [status.github.com](https://status.github.com/)
- Tambahkan retry logic

#### 4. User Not Found Error
**Problem**: User tidak ditemukan padahal ada
**Cause**: Username case-sensitive atau typo
**Solution**:
- Pastikan username tepat (case-sensitive)
- Gunakan trim() untuk menghapus whitespace
- Validasi input sebelum API call

#### 5. Data Loading Issues
**Problem**: Data tidak muncul atau loading terlalu lama
**Cause**: Network lambat atau API response besar
**Solution**:
- Tambahkan loading indicator
- Implementasi timeout
- Gunakan caching

### Debug Tips

#### 1. Gunakan Browser DevTools
- **Network Tab**: Monitor API requests
- **Console Tab**: Lihat error messages
- **Vue DevTools**: Inspect component state

#### 2. Console Logging
```javascript
console.log('Searching for:', username);
console.log('API Response:', data);
console.log('Error:', error);
```

#### 3. Error Boundary
Implementasi error boundary untuk catch Vue errors:
```javascript
app.config.errorHandler = (err, instance, info) => {
  console.error('Global error:', err);
  // Send error to logging service
};
```

### Performance Optimization

#### 1. Reduce API Calls
- Gunakan debouncing untuk search input
- Implementasi caching
- Load data on-demand

#### 2. Optimize Images
- Gunakan lazy loading untuk avatar
- Compress images
- Gunakan WebP format jika support

#### 3. Code Splitting
```javascript
// Dynamic import untuk components
const UserProfileCard = defineAsyncComponent(() =>
  import('./UserProfileCard.vue')
);
```

### Browser Compatibility

#### Supported Browsers
- Chrome 60+
- Firefox 60+
- Safari 12+
- Edge 79+

#### Polyfills untuk Older Browsers
```javascript
// Untuk fetch API
import 'whatwg-fetch';

// Untuk async/await
import 'regenerator-runtime/runtime';
```

### Security Considerations

#### 1. Input Validation
```javascript
const validateUsername = (username) => {
  // Hanya allow alphanumeric characters dan hyphens
  return /^[a-zA-Z0-9-]+$/.test(username);
};
```

#### 2. XSS Prevention
- Gunakan text content, bukan HTML
- Escape user input
- Gunakan Vue.js built-in protections

#### 3. API Key Security
- Jangan expose API keys di client-side code
- Gunakan environment variables
- Implementasi rate limiting

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi GitHub Users Search yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:

- **HTTP API integration** dengan GitHub API
- **Asynchronous programming** dengan async/await
- **State management** untuk form dan API results
- **Error handling** yang robust
- **Data persistence** dengan localStorage
- **User experience design** untuk search applications
- **Component-based architecture**
- **Modern CSS** dengan animations dan transitions
- **Responsive design** untuk semua devices

---

**Happy Coding! 🚀**