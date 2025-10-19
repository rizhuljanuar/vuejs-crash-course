# 🚀 Panduan Cepat: GitHub Users Search Vue.js

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
npm create vue@latest github-search-app

# Masuk ke folder project
cd github-search-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed, watch, onMounted } from "vue";

// Reactive state
const username = ref("");
const userProfile = ref(null);
const isLoading = ref(false);
const error = ref(null);
const searchHistory = ref([]);
const showHistory = ref(false);

// API Configuration
const API_BASE_URL = "https://api.github.com/users/";
const DEBOUNCE_DELAY = 500;

// Debounce function
let debounceTimer = null;
const debouncedSearch = (callback) => {
  clearTimeout(debounceTimer);
  debounceTimer = setTimeout(callback, DEBOUNCE_DELAY);
};

// Main search function
const getUserProfile = async () => {
  if (!username.value || username.value.trim().length < 1) {
    error.value = "Please enter a valid username";
    userProfile.value = null;
    return;
  }

  isLoading.value = true;
  error.value = null;

  try {
    const response = await fetch(`${API_BASE_URL}${username.value}`);

    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(errorData.message || "User not found");
    }

    const data = await response.json();
    userProfile.value = data;
    addToHistory(username.value);

  } catch (err) {
    console.error("Error fetching user profile:", err);
    error.value = err.message || "Failed to fetch user profile";
    userProfile.value = null;
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

  // Check if already exists
  const existingIndex = searchHistory.value.findIndex(
    item => item.username.toLowerCase() === searchedUsername.toLowerCase()
  );

  if (existingIndex >= 0) {
    searchHistory.value.splice(existingIndex, 1);
  }

  searchHistory.value.unshift(historyItem);

  // Limit to 20 items
  if (searchHistory.value.length > 20) {
    searchHistory.value = searchHistory.value.slice(0, 20);
  }

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
  if (confirm('Clear search history?')) {
    searchHistory.value = [];
    localStorage.removeItem('githubSearchHistory');
  }
};

const searchFromHistory = (historyItem) => {
  username.value = historyItem.username;
  userProfile.value = historyItem.userProfile ? { ...historyItem.userProfile } : null;
  showHistory.value = false;
  getUserProfile();
};

const toggleHistory = () => {
  showHistory.value = !showHistory.value;
};

const formatTime = (timestamp) => {
  const date = new Date(timestamp);
  const now = new Date();
  const diffMs = now - date;
  const diffMins = Math.floor(diffMs / 60000);

  if (diffMins < 1) return 'Just now';
  if (diffMins < 60) return `${diffMins}m ago`;
  if (diffMins < 1440) return `${Math.floor(diffMins / 60)}h ago`;
  return `${Math.floor(diffMins / 1440)}d ago`;
};

const handleKeyPress = (event) => {
  if (event.key === 'Enter') {
    getUserProfile();
  } else if (event.key === 'Escape') {
    showHistory.value = false;
  }
};

// Auto-search with debounce
watch(username, (newUsername) => {
  if (newUsername && newUsername.trim().length > 0) {
    debouncedSearch(() => {
      getUserProfile();
    });
  }
});

// Computed properties
const hasProfile = computed(() => !!userProfile.value);
const hasError = computed(() => !!error.value);
const hasHistory = computed(() => searchHistory.value.length > 0);

// Initialize
onMounted(() => {
  loadFromLocalStorage();
});
</script>

<template>
  <div class="github-search">
    <!-- Header -->
    <header class="header">
      <h1>🔍 GitHub Users Search</h1>
      <p>Find and explore GitHub profiles instantly</p>
    </header>

    <!-- Search Section -->
    <main class="main">
      <div class="search-section">
        <!-- Search Input -->
        <div class="search-container">
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
              class="search-btn"
            >
              <span v-if="isLoading" class="spinner">🔄</span>
              <span v-else>🔍</span>
            </button>
          </div>

          <!-- History Controls -->
          <div class="controls">
            <button
              v-if="hasHistory"
              @click="toggleHistory"
              class="history-btn"
              :class="{ active: showHistory }"
            >
              📜 History ({{ searchHistory.length }})
            </button>

            <button
              v-if="hasHistory"
              @click="clearHistory"
              class="clear-btn"
            >
              🗑️ Clear
            </button>
          </div>
        </div>

        <!-- Search History -->
        <div v-if="showHistory && hasHistory" class="history">
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
              <span class="history-name">{{ item.username }}</span>
              <span class="history-time">{{ formatTime(item.timestamp) }}</span>
            </li>
          </ul>
        </div>

        <!-- Loading State -->
        <div v-if="isLoading" class="loading">
          <div class="spinner-large"></div>
          <p>Fetching GitHub profile...</p>
        </div>

        <!-- Error State -->
        <div v-else-if="hasError" class="error">
          <div class="error-icon">😞</div>
          <h3>Oops! Something went wrong</h3>
          <p>{{ error }}</p>
          <button @click="getUserProfile" class="retry-btn">
            🔄 Try Again
          </button>
        </div>

        <!-- User Profile -->
        <div v-else-if="hasProfile" class="profile">
          <div class="profile-header">
            <img
              :src="userProfile.avatar_url"
              :alt="userProfile.login"
              class="avatar"
            />

            <div class="user-info">
              <h2>
                {{ userProfile.name || userProfile.login }}
                <a
                  :href="userProfile.html_url"
                  target="_blank"
                  class="github-link"
                >
                  <svg class="github-icon" viewBox="0 0 24 24" width="20" height="20">
                    <path fill="currentColor" d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957.266 1.783.693 2.74 1.236 1.548.865 3.346.862 5.223-.006 2.094-.262 3.814-.789 4.768-.525 1.424-1.274 2.613-2.279 1.842-1.004 2.347-1.831 2.391-2.83.024-.567.049-1.16.049-1.755 0-1.991-.181-3.648-.967-4.696-1.865-.417-.353-.795-.722-1.146-1.092-.322-.332-.548-.717-.683-1.125-.082-.25-.085-.521-.015-.78.069-.258.198-.522.388-.788.261-.393.427-.783.427-1.274 0-.828-.463-1.562-1.263-1.562-1.262 0-.874.65-1.753 1.651-2.023.647-.198 1.011-.511 1.651-.511 1.044 0 1.891.864 1.891 1.93 0 1.07-.848 1.93-1.891 1.93-.64 0-1.004.313-1.651.511-1.001.27-1.651 1.149-1.651 2.023 0 1.115.668 1.736 1.587 1.842.146.125.336.2.584.215.317.091.638.055.957-.093.411-.334.635-.661.635-1.089 0-.628-.37-1.268-1.092-1.268-1.063 0-1.924.873-1.924 1.95 0 .711.529 1.671.865 1.916.231.245.354.546.354.874 0 .418-.299.836-.592 1.286-.335.447-.665.862-.973 1.235-.307.371-.594.722-.861 1.033-.267.311-.517.588-.75.839-.233.251-.447.476-.643.676-.196.2-.379.375-.549.527-.17.152-.324.273-.463.36-.139.087-.267.152-.383.194-.115.042-.221.061-.317.061-.193 0-.341-.088-.447-.264-.105-.176-.164-.398-.164-.673 0-.581.433-1.181 1.283-1.78.848-.597 1.281-1.179 1.281-1.78 0-.582.433-1.181 1.283-1.78.848-.597 1.281-1.179 1.281-1.78 0-.582.433-1.181 1.283-1.78.848-.597 1.281-1.179 1.281-1.78z"/>
                  </svg>
                </a>
              </h2>

              <p v-if="userProfile.bio" class="bio">
                {{ userProfile.bio }}
              </p>

              <div class="meta">
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
                    class="blog-link"
                  >
                    {{ userProfile.blog }}
                  </a>
                </span>
              </div>
            </div>
          </div>

          <!-- Stats -->
          <div class="stats">
            <div class="stat">
              <div class="stat-number">{{ formatNumber(userProfile.followers) }}</div>
              <div class="stat-label">Followers</div>
            </div>
            <div class="stat">
              <div class="stat-number">{{ formatNumber(userProfile.following) }}</div>
              <div class="stat-label">Following</div>
            </div>
            <div class="stat">
              <div class="stat-number">{{ formatNumber(userProfile.public_repos) }}</div>
              <div class="stat-label">Repos</div>
            </div>
          </div>

          <!-- Additional Info -->
          <div class="info">
            <div class="info-row">
              <span>📅 Member Since:</span>
              <span>{{ formatDate(userProfile.created_at) }}</span>
            </div>
            <div class="info-row">
              <span>🔄 Last Updated:</span>
              <span>{{ formatDate(userProfile.updated_at) }}</span>
            </div>
          </div>
        </div>

        <!-- Empty State -->
        <div v-else class="empty">
          <div class="empty-icon">👤</div>
          <h3>Enter a GitHub username</h3>
          <p>Search for any GitHub user to see their profile</p>
        </div>
      </div>
    </main>

    <!-- Footer -->
    <footer class="footer">
      <p>
        Powered by
        <a href="https://docs.github.com/en/rest/users/users" target="_blank">
          GitHub API
        </a>
      </p>
      <p>Made with ❤️ using Vue.js 3</p>
    </footer>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.github-search {
  max-width: 700px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
  display: flex;
  flex-direction: column;
}

/* Header */
.header {
  text-align: center;
  margin-bottom: 30px;
  padding: 30px 0;
}

.header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.header p {
  font-size: 1.1em;
  opacity: 0.9;
}

/* Main Content */
.main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Search Section */
.search-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  border: 1px solid rgba(255,255,255,0.2);
}

.search-container {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.input-group {
  display: flex;
  gap: 10px;
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

.search-btn {
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 10px;
  padding: 15px 25px;
  font-size: 1.1em;
  cursor: pointer;
  transition: all 0s ease;
  min-width: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search-btn:hover:not(:disabled) {
  background: #45a049;
  transform: translateY(-2px);
}

.search-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

.history-btn {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.history-btn:hover {
  background: rgba(255,255,255,0.3);
}

.history-btn.active {
  background: #4CAF50;
}

.clear-btn {
  background: transparent;
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 20px;
  padding: 8px 16px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.clear-btn:hover {
  background: rgba(231, 76, 60, 0.3);
  border-color: #e74c3c;
}

/* History */
.history {
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 20px;
}

.history h3 {
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

.history-name {
  flex: 1;
  font-weight: 600;
}

.history-time {
  font-size: 0.8em;
  opacity: 0.7;
}

/* States */
.loading {
  text-align: center;
  padding: 40px;
}

.spinner-large {
  width: 50px;
  height: 50px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

.error {
  text-align: center;
  padding: 40px;
  background: rgba(231, 76, 60, 0.2);
  border: 2px solid #e74c3c;
  border-radius: 15px;
}

.error-icon {
  font-size: 3em;
  margin-bottom: 20px;
}

.retry-btn {
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

.retry-btn:hover {
  background: #c0392b;
  transform: translateY(-2px);
}

/* Profile */
.profile {
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

.avatar {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid rgba(255,255,255,0.2);
  transition: transform 0.3s ease;
}

.avatar:hover {
  transform: scale(1.05);
}

.user-info {
  flex: 1;
  min-width: 200px;
}

.user-info h2 {
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

.bio {
  margin: 0 0 15px 0;
  line-height: 1.6;
  opacity: 0.9;
}

.meta {
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

/* Stats */
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.stat {
  text-align: center;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 20px;
  transition: all 0.3s ease;
}

.stat:hover {
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

/* Info */
.info {
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

/* Empty State */
.empty {
  text-align: center;
  padding: 60px 20px;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
}

.empty-icon {
  font-size: 4em;
  margin-bottom: 20px;
}

/* Footer */
.footer {
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
  text-align: center;
  opacity: 0.8;
}

.footer p {
  margin: 5px 0;
}

.footer a {
  color: #4CAF50;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer a:hover {
  color: #45a049;
  text-decoration: underline;
}

/* Responsive */
@media (max-width: 768px) {
  .github-search {
    padding: 15px;
    margin: 10px;
  }

  .header h1 {
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

  .search-btn {
    width: 100%;
  }

  .stats {
    grid-template-columns: 1fr;
  }

  .meta {
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

button:focus,
input:focus {
  outline: 3px solid #4CAF50;
  outline-offset: 2px;
}
</style>
```

### 📋 Step 4: Jalankan Aplikasi (1 Menit)

```bash
# Start development server
npm run dev
```

**Buka browser:** `http://localhost:5173`

🎉 **Selesai! Aplikasi GitHub Users Search Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis
3. **`watch()`** - Reaksi terhadap perubahan data
4. **`v-model`** - Two-way data binding
5. **`@click`** - Event handling
6. **`v-if`** - Conditional rendering
7. **`onMounted`** - Lifecycle hook

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed, watch } from "vue";
const username = ref("");          // Input username
const userProfile = ref(null);       // GitHub user data
const isLoading = ref(false);        // Loading state

// 2. API Function
const getUserProfile = async () => { /* API logic */ };

// 3. Helper Functions
const formatDate = (date) => { /* Format date */ };
const formatNumber = (num) => { /* Format numbers */ };

// 4. LocalStorage Functions
const saveToLocalStorage = () => { /* Save data */ };
</script>

<template>
<!-- 5. Tampilan HTML -->
</template>

<style scoped>
/* 6. Styling CSS */
</style>
```

### 🔄 Logika API Integration

```javascript
const getUserProfile = async () => {
  isLoading.value = true;              // 1. Set loading

  try {
    const response = await fetch(`${API_BASE_URL}${username.value}`); // 2. API call

    if (!response.ok) {
      throw new Error("User not found");  // 3. Handle error
    }

    const data = await response.json();    // 4. Parse response
    userProfile.value = data;             // 5. Update data
    addToHistory(username.value);       // 6. Add to history

  } catch (error) {
    error.value = error.message;         // 7. Handle error
  } finally {
    isLoading.value = false;             // 8. Reset loading
  }
};
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Multiple User Search
```vue
<script setup>
const searchQuery = ref('');
const searchResults = ref([]);

const searchUsers = async () => {
  try {
    const response = await fetch(
      `https://api.github.com/search/users?q=${encodeURIComponent(searchQuery.value)}&per_page=10`
    );
    const data = await response.json();
    searchResults.value = data.items;
  } catch (error) {
    console.error('Search error:', error);
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

### 3. Advanced Filters
```vue
<script setup>
const filters = ref({
  type: 'users',
  sort: 'best-match',
  order: 'desc',
  language: 'javascript'
});

const advancedSearch = async () => {
  const params = new URLSearchParams({
    q: `${filters.value.language} in:${filters.value.type}`,
    sort: filters.value.sort,
    order: filters.value.order
  });

  const response = await fetch(
    `https://api.github.com/search/${filters.value.type}?${params}`
  );
  // Process results...
};
</script>
```

### 4. User Analytics
```vue
<script setup>
const userAnalytics = computed(() => {
  if (!userProfile.value) return null;

  const accountAge = calculateAge(userProfile.value.created_at);
  const contributionLevel = getContributionLevel(userProfile.value.public_repos);

  return { accountAge, contributionLevel };
});

const calculateAge = (createdAt) => {
  const created = new Date(createdAt);
  const now = new Date();
  const diffTime = Math.abs(now - created);
  return Math.floor(diffTime / (1000 * 60 * 60 * 24));
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

  // Fetch fresh data...
};
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
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  color: #333;
}
```

### 2. Animations
```vue
<script setup>
const isLoading = ref(false);

const withAnimation = async (callback) => {
  isLoading.value = true;
  await new Promise(resolve => setTimeout(resolve, 300));
  await callback();
  isLoading.value = false;
};
</script>

<style>
.profile-enter-active {
  animation: slideIn 0.5s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>
```

### 3. Sound Effects
```javascript
const playSound = (type) => {
  const audio = new Audio(`/sounds/${type}.mp3`);
  audio.play().catch(e => console.log('Sound play failed:', e));
};

const getUserProfile = async () => {
  playSound('search');
  // API logic...
};
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| API rate limit exceeded | Implement caching, add delay between requests |
| User not found | Validate username, suggest alternatives |
| Network error | Add retry mechanism, offline indicator |
| Avatar not loading | Add error handling, fallback avatar |
| Responsive issues | Test with dev tools, use mobile-first design |

## 🚀 Next Steps

1. **Repository Search** - Tambah pencarian repository
2. **Advanced Filters** - Filter berdasarkan lokasi, bahasa
3. **User Analytics** - Analisis profil GitHub
4. **Caching** - Cache API responses untuk performance
5. **Offline Support** - Offline capability dengan service worker
6. **PWA** - Convert ke Progressive Web App

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi GitHub Users Search yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [GitHub API](https://docs.github.com/en/rest/) | [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) | [LocalStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)