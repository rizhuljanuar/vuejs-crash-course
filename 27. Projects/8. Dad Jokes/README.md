# Dad Jokes Generator - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Dad Jokes Generator adalah aplikasi web yang mengambil dan menampilkan jokes lucu dari API eksternal menggunakan Vue.js 3 dengan Composition API. Aplikasi ini mendemonstrasikan bagaimana melakukan HTTP requests, mengelola state asynchronous, dan menampilkan data yang dinamis. Project ini sangat cocok untuk pemula yang ingin mempelajari cara kerja API integrasi dan state management dalam Vue.js.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- HTTP requests dengan Axios library
- Asynchronous JavaScript dengan async/await
- Error handling untuk API calls
- Conditional rendering dengan `v-if`
- Event handling dengan `@click`
- Loading states dan user feedback
- Component-based architecture
- API integration best practices
- State management untuk asynchronous data

## 📋 Fitur Aplikasi

1. **Joke Fetching**: Mengambil jokes dari icanhazdadjoke.com API
2. **One-Click Loading**: Tombol untuk mengambil joke baru
3. **Dynamic Display**: Menampilkan joke yang diambil secara real-time
4. **Error Handling**: Menangani error saat API gagal
5. **Clean UI**: Interface yang sederhana dan user-friendly
6. **Responsive Design**: Layout yang bekerja di semua device sizes
7. **Loading Feedback**: Visual indicator saat mengambil data
8. **Joke History**: Menyimpan jokes yang telah dilihat (enhanced)
9. **Share Functionality**: Bagikan jokes ke social media (enhanced)
10. **Favorites**: Simpan jokes favorit (enhanced)

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)
- Koneksi internet untuk mengakses API

## 📂 Struktur Project

```
8. Dad Jokes/
├── App.vue                              # Komponen utama aplikasi
├── components/
│   └── DadJokes.vue                     # Komponen dad jokes generator
├── assets/
│   └── styles/
│       └── main.css                    # Global styles
├── utils/
│   └── jokeApi.js                      # API helper functions
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
npm create vue@latest dad-jokes-app
cd dad-jokes-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create dad-jokes-app
cd dad-jokes-app
```

### Langkah 3: Install Dependencies

```bash
# Install Axios untuk HTTP requests
npm install axios

# Atau gunakan fetch API bawaan browser (tanpa install library)
```

### Langkah 4: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style scoped>`**: Bagian CSS untuk styling

### Langkah 5: Membuat Komponen DadJokes

**File: `components/DadJokes.vue`**

#### Bagian Script Setup - Reactive Data & API Integration
```vue
<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// Reactive state untuk menyimpan joke
const joke = ref(null);
const isLoading = ref(false);
const error = ref(null);
const jokeHistory = ref([]);
const favorites = ref([]);
const showHistory = ref(false);

// API endpoint
const API_URL = "https://icanhazdadjoke.com/";

// Fungsi untuk mengambil joke dari API
const fetchJoke = async () => {
  isLoading.value = true;
  error.value = null;

  try {
    const response = await axios.get(API_URL, {
      headers: {
        Accept: "application/json",
        "User-Agent": "Dad Jokes App (https://example.com)"
      },
      timeout: 10000 // 10 seconds timeout
    });

    const newJoke = response.data.joke;
    joke.value = newJoke;

    // Tambahkan ke history
    addToHistory(newJoke);

  } catch (err) {
    console.error("Error fetching dad joke:", err);
    error.value = "Gagal mengambil joke. Coba lagi nanti!";

    // Joke fallback jika API gagal
    joke.value = getFallbackJoke();
  } finally {
    isLoading.value = false;
  }
};

// Fungsi untuk mengambil joke dengan fetch API (alternatif)
const fetchJokeWithFetch = async () => {
  isLoading.value = true;
  error.value = null;

  try {
    const response = await fetch(API_URL, {
      method: 'GET',
      headers: {
        'Accept': 'application/json',
        'User-Agent': 'Dad Jokes App'
      }
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    joke.value = data.joke;
    addToHistory(data.joke);

  } catch (err) {
    console.error("Error fetching dad joke:", err);
    error.value = "Gagal mengambil joke. Coba lagi nanti!";
    joke.value = getFallbackJoke();
  } finally {
    isLoading.value = false;
  }
};

// Fungsi untuk mendapatkan joke fallback
const getFallbackJoke = () => {
  const fallbackJokes = [
    "Why don't scientists trust atoms? Because they make up everything!",
    "Why did the scarecrow win an award? He was outstanding in his field!",
    "Why don't eggs tell jokes? They'd crack each other up!",
    "What do you call a fake noodle? An impasta!",
    "Why did the math book look so sad? Because it had too many problems!"
  ];

  return fallbackJokes[Math.floor(Math.random() * fallbackJokes.length)];
};

// Tambahkan joke ke history
const addToHistory = (jokeText) => {
  const historyItem = {
    id: Date.now(),
    text: jokeText,
    timestamp: new Date().toLocaleString(),
    isFavorite: false
  };

  jokeHistory.value.unshift(historyItem);

  // Batasi history hanya 50 items
  if (jokeHistory.value.length > 50) {
    jokeHistory.value = jokeHistory.value.slice(0, 50);
  }

  // Simpan ke localStorage
  saveToLocalStorage();
};

// Fungsi favorit
const toggleFavorite = (jokeId) => {
  const joke = jokeHistory.value.find(j => j.id === jokeId);
  if (joke) {
    joke.isFavorite = !joke.isFavorite;

    if (joke.isFavorite) {
      favorites.value.push(joke);
    } else {
      favorites.value = favorites.value.filter(j => j.id !== jokeId);
    }

    saveToLocalStorage();
  }
};

const removeFromFavorites = (jokeId) => {
  favorites.value = favorites.value.filter(j => j.id !== jokeId);

  // Update status di history
  const historyJoke = jokeHistory.value.find(j => j.id === jokeId);
  if (historyJoke) {
    historyJoke.isFavorite = false;
  }

  saveToLocalStorage();
};

// Share functionality
const shareJoke = async (jokeText) => {
  if (navigator.share) {
    try {
      await navigator.share({
        title: 'Dad Joke',
        text: jokeText,
        url: window.location.href
      });
    } catch (err) {
      console.log('Error sharing:', err);
      copyToClipboard(jokeText);
    }
  } else {
    copyToClipboard(jokeText);
  }
};

const copyToClipboard = async (text) => {
  try {
    await navigator.clipboard.writeText(text);
    alert('Joke berhasil disalin ke clipboard!');
  } catch (err) {
    // Fallback untuk older browsers
    const textArea = document.createElement('textarea');
    textArea.value = text;
    document.body.appendChild(textArea);
    textArea.select();
    document.execCommand('copy');
    document.body.removeChild(textArea);
    alert('Joke berhasil disalin ke clipboard!');
  }
};

// LocalStorage functions
const saveToLocalStorage = () => {
  try {
    localStorage.setItem('dadJokesHistory', JSON.stringify(jokeHistory.value));
    localStorage.setItem('dadJokesFavorites', JSON.stringify(favorites.value));
  } catch (error) {
    console.error('Error saving to localStorage:', error);
  }
};

const loadFromLocalStorage = () => {
  try {
    const savedHistory = localStorage.getItem('dadJokesHistory');
    if (savedHistory) {
      jokeHistory.value = JSON.parse(savedHistory);
    }

    const savedFavorites = localStorage.getItem('dadJokesFavorites');
    if (savedFavorites) {
      favorites.value = JSON.parse(savedFavorites);
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
};

// Clear history
const clearHistory = () => {
  if (confirm('Apakah Anda yakin ingin menghapus semua history?')) {
    jokeHistory.value = [];
    favorites.value = [];
    saveToLocalStorage();
  }
};

// Retry function
const retryFetch = () => {
  fetchJoke();
};

// Lifecycle hooks
onMounted(() => {
  loadFromLocalStorage();
  fetchJoke(); // Ambil joke pertama saat component dimuat
});
</script>
```

#### Penjelasan Kode Script:

1. **`import { ref, onMounted } from "vue"`**
   - Mengimpor `ref` untuk membuat reactive variables
   - Mengimpor `onMounted` untuk lifecycle hook
   - `ref` membuat data yang otomatis update UI saat berubah

2. **`import axios from "axios"`**
   - Mengimpor Axios library untuk HTTP requests
   - Axios menyederhanakan API calls dengan Promise-based approach

3. **Reactive Variables dengan `ref()`**
   - `joke`: Menyimpan joke yang sedang ditampilkan
   - `isLoading`: Status loading untuk user feedback
   - `error`: Menyimpan pesan error jika API gagal
   - `jokeHistory`: Daftar jokes yang telah dilihat
   - `favorites`: Daftar jokes favorit

4. **`fetchJoke()` Function**
   - Menggunakan async/await untuk asynchronous operation
   - Error handling dengan try-catch-finally
   - Mengatur loading states sebelum dan sesudah request
   - Menambahkan joke ke history setelah berhasil

5. **Helper Functions**
   - `addToHistory()`: Menyimpan joke ke history
   - `toggleFavorite()`: Mengelola favorites
   - `shareJoke()`: Share functionality dengan Web Share API
   - `copyToClipboard()`: Copy to clipboard dengan fallback

6. **LocalStorage Integration**
   - `saveToLocalStorage()`: Menyimpan data ke browser storage
   - `loadFromLocalStorage()`: Memuat data dari browser storage
   - Persistensi data antar session

### Langkah 6: Membuat Template HTML

```vue
<template>
  <div class="dad-jokes-container">
    <!-- Header -->
    <header class="jokes-header">
      <h1 class="jokes-title">🎭 Dad Jokes Generator</h1>
      <p class="jokes-subtitle">Klik tombol untuk mendapatkan joke lucu!</p>
    </header>

    <!-- Loading State -->
    <div v-if="isLoading" class="loading-container">
      <div class="loading-spinner"></div>
      <p>Sedang mengambil joke...</p>
    </div>

    <!-- Error State -->
    <div v-if="error && !isLoading" class="error-container">
      <div class="error-icon">⚠️</div>
      <p class="error-message">{{ error }}</p>
      <button @click="retryFetch" class="retry-button">Coba Lagi</button>
    </div>

    <!-- Joke Display -->
    <main class="joke-main" v-if="!isLoading">
      <section class="joke-display">
        <div v-if="joke" class="joke-card">
          <div class="joke-content">
            <p class="joke-text">{{ joke }}</p>
          </div>

          <div class="joke-actions">
            <button @click="shareJoke(joke)" class="action-button share-button" title="Bagikan">
              📤
            </button>
            <button @click="copyToClipboard(joke)" class="action-button copy-button" title="Salin">
              📋
            </button>
            <button @click="toggleFavorite(Date.now())" class="action-button favorite-button" title="Favorit">
              ❤️
            </button>
          </div>
        </div>

        <div v-else class="no-joke-placeholder">
          <p>Klik tombol di bawah untuk mendapatkan joke!</p>
        </div>
      </section>

      <!-- Generate Button -->
      <section class="generate-section">
        <button
          @click="fetchJoke"
          :disabled="isLoading"
          class="generate-button"
        >
          <span v-if="isLoading">🔄 Loading...</span>
          <span v-else>🎲 Get New Joke</span>
        </button>
      </section>
    </main>

    <!-- Navigation Tabs -->
    <nav class="navigation-tabs">
      <button
        @click="showHistory = false"
        :class="{ 'active': !showHistory }"
        class="tab-button"
      >
        🏠 Utama
      </button>
      <button
        @click="showHistory = true"
        :class="{ 'active': showHistory }"
        class="tab-button"
      >
        📜 History ({{ jokeHistory.length }})
      </button>
      <button
        @click="showHistory = 'favorites'"
        :class="{ 'active': showHistory === 'favorites' }"
        class="tab-button"
      >
        ❤️ Favorit ({{ favorites.length }})
      </button>
    </nav>

    <!-- History/Favorites Section -->
    <section v-if="showHistory" class="history-section">
      <div class="history-header">
        <h2 v-if="showHistory === 'favorites'">❤️ Joke Favorit</h2>
        <h2 v-else>📜 History Jokes</h2>
        <button
          v-if="jokeHistory.length > 0"
          @click="clearHistory"
          class="clear-history-button"
        >
          🗑️ Hapus Semua
        </button>
      </div>

      <div v-if="getCurrentList().length === 0" class="empty-state">
        <p v-if="showHistory === 'favorites'">Belum ada joke favorit.</p>
        <p v-else>Belum ada history.</p>
      </div>

      <div v-else class="history-list">
        <div
          v-for="item in getCurrentList()"
          :key="item.id"
          class="history-item"
          :class="{ 'favorite': item.isFavorite }"
        >
          <div class="history-content">
            <p class="history-joke">{{ item.text }}</p>
            <div class="history-meta">
              <span class="history-time">{{ item.timestamp }}</span>
              <span v-if="item.isFavorite" class="favorite-badge">❤️</span>
            </div>
          </div>

          <div class="history-actions">
            <button
              @click="shareJoke(item.text)"
              class="history-action-button"
              title="Bagikan"
            >
              📤
            </button>
            <button
              @click="copyToClipboard(item.text)"
              class="history-action-button"
              title="Salin"
            >
              📋
            </button>
            <button
              v-if="showHistory !== 'favorites'"
              @click="toggleFavorite(item.id)"
              class="history-action-button"
              :class="{ 'active': item.isFavorite }"
              title="Favorit"
            >
              {{ item.isFavorite ? '💔' : '❤️' }}
            </button>
            <button
              v-else
              @click="removeFromFavorites(item.id)"
              class="history-action-button remove"
              title="Hapus dari favorit"
            >
              💔
            </button>
          </div>
        </div>
      </div>
    </section>

    <!-- Footer -->
    <footer class="jokes-footer">
      <p>
        Dibuat dengan ❤️ menggunakan Vue.js 3 |
        Powered by
        <a href="https://icanhazdadjoke.com/" target="_blank" rel="noopener">
          icanhazdadjoke.com
        </a>
      </p>
    </footer>
  </div>
</template>
```

#### Penjelasan Template:

1. **Header Section**
   - Menampilkan judul dan subtitle aplikasi
   - Branding dengan emoji untuk visual appeal

2. **Conditional Rendering**
   - `v-if="isLoading"`: Menampilkan loading spinner
   - `v-if="error"`: Menampilkan pesan error
   - `v-if="joke"`: Menampilkan joke jika ada

3. **Loading States**
   - Visual feedback saat mengambil data dari API
   - Loading spinner dengan animasi
   - Disabled state untuk tombol saat loading

4. **Error Handling**
   - Menampilkan pesan error yang user-friendly
   - Retry button untuk mencoba lagi
   - Fallback jokes jika API gagal

5. **Joke Display Card**
   - Card design untuk menampilkan joke
   - Action buttons untuk share, copy, favorite
   - Responsive layout

6. **Navigation Tabs**
   - Tab navigation untuk main view, history, favorites
   - Active state indicators
   - Badge counters untuk jumlah items

7. **History/Favorites List**
   - List view untuk menampilkan jokes
   - Metadata (timestamp, favorite status)
   - Action buttons untuk setiap item

### Langkah 7: Advanced CSS Styling

```vue
<style scoped>
/* Main Container */
.dad-jokes-container {
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
.jokes-header {
  text-align: center;
  margin-bottom: 30px;
  padding: 30px 0;
}

.jokes-title {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
  animation: bounce 2s infinite;
}

.jokes-subtitle {
  font-size: 1.1em;
  opacity: 0.9;
}

/* Loading State */
.loading-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background: rgba(255,255,255,0.1);
  border-radius: 15px;
  margin-bottom: 20px;
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 4px solid rgba(255,255,255,0.3);
  border-top: 4px solid white;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Error State */
.error-container {
  text-align: center;
  padding: 30px;
  background: rgba(231, 76, 60, 0.2);
  border: 2px solid #e74c3c;
  border-radius: 15px;
  margin-bottom: 20px;
  backdrop-filter: blur(10px);
}

.error-icon {
  font-size: 3em;
  margin-bottom: 15px;
}

.error-message {
  font-size: 1.1em;
  margin-bottom: 20px;
  color: #ffcccc;
}

.retry-button {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 12px 24px;
  font-size: 1em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.retry-button:hover {
  background: #c0392b;
  transform: translateY(-2px);
}

/* Main Content */
.joke-main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Joke Display */
.joke-display {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
  min-height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.joke-card {
  width: 100%;
  text-align: center;
}

.joke-content {
  margin-bottom: 25px;
}

.joke-text {
  font-size: 1.3em;
  line-height: 1.6;
  margin: 0;
  padding: 20px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  border-left: 4px solid #f39c12;
}

.joke-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}

.action-button {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 50%;
  width: 50px;
  height: 50px;
  font-size: 1.2em;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.action-button:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0,0,0,0.3);
}

.favorite-button.active {
  background: #e74c3c;
}

.no-joke-placeholder {
  text-align: center;
  opacity: 0.8;
  font-style: italic;
}

/* Generate Button */
.generate-section {
  text-align: center;
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
  background: linear-gradient(135deg, #45a049, #4CAF50);
}

.generate-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

/* Navigation Tabs */
.navigation-tabs {
  display: flex;
  justify-content: center;
  gap: 5px;
  margin: 30px 0;
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

/* History Section */
.history-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  border: 1px solid rgba(255,255,255,0.2);
  max-height: 500px;
  overflow-y: auto;
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 1px solid rgba(255,255,255,0.2);
}

.history-header h2 {
  margin: 0;
  font-size: 1.5em;
}

.clear-history-button {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 8px 15px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
}

.clear-history-button:hover {
  background: #c0392b;
}

.empty-state {
  text-align: center;
  padding: 40px;
  opacity: 0.7;
  font-style: italic;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.history-item {
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 20px;
  transition: all 0.3s ease;
  border-left: 4px solid transparent;
}

.history-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateX(5px);
}

.history-item.favorite {
  border-left-color: #e74c3c;
}

.history-content {
  margin-bottom: 15px;
}

.history-joke {
  margin: 0 0 10px 0;
  font-size: 1.1em;
  line-height: 1.5;
}

.history-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.9em;
  opacity: 0.8;
}

.favorite-badge {
  color: #e74c3c;
  font-weight: bold;
}

.history-actions {
  display: flex;
  gap: 10px;
  justify-content: flex-end;
}

.history-action-button {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 6px;
  padding: 8px 12px;
  font-size: 0.9em;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 40px;
}

.history-action-button:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.history-action-button.active {
  background: #e74c3c;
}

.history-action-button.remove {
  background: #95a5a6;
}

/* Footer */
.jokes-footer {
  text-align: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
  opacity: 0.8;
}

.jokes-footer a {
  color: #f39c12;
  text-decoration: none;
  transition: color 0.3s ease;
}

.jokes-footer a:hover {
  color: #e67e22;
  text-decoration: underline;
}

/* Animations */
@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .dad-jokes-container {
    padding: 15px;
    margin: 10px;
  }

  .jokes-title {
    font-size: 2em;
  }

  .joke-text {
    font-size: 1.1em;
    padding: 15px;
  }

  .navigation-tabs {
    flex-direction: column;
    gap: 0;
  }

  .tab-button {
    border-radius: 0;
    width: 100%;
  }

  .history-header {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }

  .history-actions {
    justify-content: center;
    flex-wrap: wrap;
  }

  .joke-actions {
    gap: 10px;
  }

  .action-button {
    width: 45px;
    height: 45px;
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
button:focus,
a:focus {
  outline: 3px solid #f39c12;
  outline-offset: 2px;
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  .dad-jokes-container {
    background: #000;
    color: #fff;
  }

  .joke-display,
  .history-section {
    border: 2px solid #fff;
  }
}
</style>
```

### Langkah 8: Update App.vue

**File: `App.vue`**
```vue
<script setup>
import DadJokes from './components/DadJokes.vue'
import { onMounted } from 'vue'

// Set page title dan meta
onMounted(() => {
  document.title = '🎭 Dad Jokes Generator - Vue.js'

  // Set meta description untuk SEO
  const metaDescription = document.querySelector('meta[name="description"]')
  if (metaDescription) {
    metaDescription.content = 'Dad Jokes Generator - Aplikasi Vue.js untuk menampilkan jokes lucu'
  } else {
    const meta = document.createElement('meta')
    meta.name = 'description'
    meta.content = 'Dad Jokes Generator - Aplikasi Vue.js untuk menampilkan jokes lucu'
    document.head.appendChild(meta)
  }
})
</script>

<template>
  <div id="app">
    <DadJokes />
  </div>
</template>

<style>
/* Global Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: #333;
  line-height: 1.6;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Print Styles */
@media print {
  body {
    background: white;
    color: black;
  }

  .jokes-footer,
  .joke-actions,
  .history-actions,
  .navigation-tabs,
  .generate-section {
    display: none;
  }
}
</style>
```

### Langkah 9: Membuat Helper Functions

**File: `utils/jokeApi.js`**
```javascript
// API configuration
const API_CONFIG = {
  baseURL: 'https://icanhazdadjoke.com/',
  timeout: 10000,
  headers: {
    'Accept': 'application/json',
    'User-Agent': 'Dad Jokes App (https://example.com)'
  }
};

// Fallback jokes jika API gagal
const FALLBACK_JOKES = [
  "Why don't scientists trust atoms? Because they make up everything!",
  "Why did the scarecrow win an award? He was outstanding in his field!",
  "Why don't eggs tell jokes? They'd crack each other up!",
  "What do you call a fake noodle? An impasta!",
  "Why did the math book look so sad? Because it had too many problems!",
  "Why can't a bicycle stand up by itself? It's two tired!",
  "What do you call cheese that isn't yours? Nacho cheese!",
  "Why did the cookie go to the doctor? Because it felt crumbly!",
  "What do you call a bear with no teeth? A gummy bear!",
  "Why don't skeletons fight each other? They don't have the guts!"
];

// Fetch joke dengan Axios
export const fetchJokeWithAxios = async () => {
  try {
    const response = await axios.get(API_CONFIG.baseURL, {
      headers: API_CONFIG.headers,
      timeout: API_CONFIG.timeout
    });

    return {
      success: true,
      data: response.data.joke,
      source: 'api'
    };
  } catch (error) {
    console.error('Error fetching joke with Axios:', error);
    return {
      success: false,
      error: error.message,
      fallback: getFallbackJoke()
    };
  }
};

// Fetch joke dengan Fetch API
export const fetchJokeWithFetch = async () => {
  try {
    const response = await fetch(API_CONFIG.baseURL, {
      method: 'GET',
      headers: API_CONFIG.headers,
      signal: AbortSignal.timeout(API_CONFIG.timeout)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();

    return {
      success: true,
      data: data.joke,
      source: 'api'
    };
  } catch (error) {
    console.error('Error fetching joke with Fetch:', error);
    return {
      success: false,
      error: error.message,
      fallback: getFallbackJoke()
    };
  }
};

// Mendapatkan joke fallback
export const getFallbackJoke = () => {
  const randomIndex = Math.floor(Math.random() * FALLBACK_JOKES.length);
  return FALLBACK_JOKES[randomIndex];
};

// Validasi joke content
export const validateJoke = (jokeText) => {
  if (!jokeText || typeof jokeText !== 'string') {
    return {
      isValid: false,
      errors: ['Joke text is required and must be a string']
    };
  }

  if (jokeText.length < 10) {
    return {
      isValid: false,
      errors: ['Joke is too short']
    };
  }

  if (jokeText.length > 500) {
    return {
      isValid: false,
      errors: ['Joke is too long']
    };
  }

  // Check for inappropriate content (simplified)
  const inappropriateWords = ['badword1', 'badword2']; // Add actual inappropriate words
  const hasInappropriate = inappropriateWords.some(word =>
    jokeText.toLowerCase().includes(word.toLowerCase())
  );

  if (hasInappropriate) {
    return {
      isValid: false,
      errors: ['Joke contains inappropriate content']
    };
  }

  return {
    isValid: true,
    errors: []
  };
};

// Format joke untuk display
export const formatJoke = (jokeText) => {
  if (!jokeText) return '';

  // Capitalize first letter
  const formatted = jokeText.charAt(0).toUpperCase() + jokeText.slice(1);

  // Ensure it ends with proper punctuation
  if (!formatted.match(/[.!?]$/)) {
    return formatted + '.';
  }

  return formatted;
};

// Share functionality
export const shareJoke = async (jokeText, title = 'Dad Joke') => {
  const shareData = {
    title,
    text: jokeText,
    url: window.location.href
  };

  try {
    if (navigator.share) {
      await navigator.share(shareData);
      return { success: true, method: 'web_share_api' };
    } else {
      // Fallback to copying to clipboard
      await navigator.clipboard.writeText(jokeText);
      return { success: true, method: 'clipboard' };
    }
  } catch (error) {
    console.error('Error sharing joke:', error);
    return { success: false, error: error.message };
  }
};

// Analytics tracking (placeholder)
export const trackJokeAction = (action, jokeText) => {
  // In a real app, you would send this to analytics service
  console.log('Analytics:', {
    action,
    jokeLength: jokeText?.length,
    timestamp: new Date().toISOString()
  });
};

export default {
  fetchJokeWithAxios,
  fetchJokeWithFetch,
  getFallbackJoke,
  validateJoke,
  formatJoke,
  shareJoke,
  trackJokeAction
};
```

### Langkah 10: Menjalankan Aplikasi

1. **Install Dependencies**
   ```bash
   npm install
   npm install axios
   ```

2. **Jalankan Development Server**
   ```bash
   npm run dev
   ```

3. **Buka Browser**
   - Buka alamat yang ditampilkan di terminal (biasanya `http://localhost:5173`)
   - Aplikasi Dad Jokes Generator siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API dengan `<script setup>`
- Syntax yang lebih ringkas dan modern
- Reaktivitas yang lebih mudah dipahami
- Better TypeScript support
- Auto-imports untuk reactivity functions

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- State management untuk joke, loading, error states
- Reactive data untuk history dan favorites

### 3. Asynchronous JavaScript
- `async/await` syntax untuk API calls
- Promise handling dengan try-catch-finally
- Error handling yang robust
- Loading state management

### 4. HTTP Requests dengan Axios
- GET request ke external API
- Headers configuration
- Timeout handling
- Error response handling

### 5. Conditional Rendering
- `v-if` untuk menampilkan/loading/error states
- `v-else` untuk alternative content
- Dynamic template rendering

### 6. Event Handling
- `@click` untuk button interactions
- Method calls dengan arguments
- Event handlers untuk share/copy functionality

### 7. Lifecycle Hooks
- `onMounted` untuk initialization
- Auto-fetch joke saat component dimuat
- Load data dari localStorage

### 8. LocalStorage Integration
- Data persistence antar sessions
- JSON serialization/deserialization
- Error handling untuk storage operations

### 9. Component Architecture
- Single-file components structure
- Props dan events communication
- Reusable component design

### 10. Browser API Integration
- Clipboard API untuk copy functionality
- Web Share API untuk mobile sharing
- Fallback methods untuk older browsers

## 🔧 Enhancement Ideas

### 1. Multiple API Sources
```javascript
const API_SOURCES = [
  'https://icanhazdadjoke.com/',
  'https://official-joke-api.appspot.com/jokes/random',
  'https://v2.jokeapi.dev/joke/Any?type=single'
];

const fetchFromMultipleSources = async () => {
  for (const source of API_SOURCES) {
    try {
      const joke = await fetchFromSource(source);
      if (joke) return joke;
    } catch (error) {
      console.log(`Failed to fetch from ${source}:`, error);
      continue;
    }
  }
  return getFallbackJoke();
};
```

### 2. Joke Categories
```javascript
const fetchJokeByCategory = async (category) => {
  const categoryMap = {
    'dad': 'https://icanhazdadjoke.com/',
    'programming': 'https://v2.jokeapi.dev/joke/Programming?type=single',
    'misc': 'https://v2.jokeapi.dev/joke/Miscellaneous?type=single'
  };

  const url = categoryMap[category];
  if (url) {
    return await fetchJoke(url);
  }
};
```

### 3. Search Functionality
```javascript
const searchJokes = async (query) => {
  try {
    const response = await axios.get(
      `https://v2.jokeapi.dev/joke/Any?contains=${encodeURIComponent(query)}`
    );
    return response.data.jokes || [];
  } catch (error) {
    console.error('Error searching jokes:', error);
    return [];
  }
};
```

### 4. Joke Rating System
```javascript
const rateJoke = (jokeId, rating) => {
  const ratings = loadRatings();
  ratings[jokeId] = rating;
  saveRatings(ratings);

  // Calculate average rating
  const allRatings = Object.values(ratings);
  const average = allRatings.reduce((a, b) => a + b, 0) / allRatings.length;

  return average;
};
```

### 5. Offline Support
```javascript
// Cache jokes for offline use
const cacheJokes = async () => {
  const cache = await caches.open('dad-jokes-cache');
  const jokes = [];

  // Pre-fetch 10 jokes
  for (let i = 0; i < 10; i++) {
    const joke = await fetchJoke();
    jokes.push(joke);
  }

  cache.put('/offline-jokes', new Response(JSON.stringify(jokes)));
};

const getOfflineJokes = async () => {
  const cache = await caches.open('dad-jokes-cache');
  const response = await cache.match('/offline-jokes');

  if (response) {
    return await response.json();
  }

  return [];
};
```

## 🐞 Common Issues & Troubleshooting

### Issue 1: API Request Fails
**Symptoms**: Error message "Gagal mengambil joke"
**Solutions**:
- Check internet connection
- Verify API endpoint is accessible
- Check CORS settings
- Try again with retry button

### Issue 2: Loading State Stuck
**Symptoms**: Loading spinner never disappears
**Solutions**:
- Check network connection
- Verify API timeout settings
- Check for JavaScript errors in console
- Implement timeout handling

### Issue 3: LocalStorage Not Working
**Symptoms**: History/favorites not saved
**Solutions**:
- Check browser storage permissions
- Clear browser cache
- Check for quota exceeded errors
- Implement graceful degradation

### Issue 4: Share Functionality Not Working
**Symptoms**: Share button doesn't work
**Solutions**:
- Check HTTPS requirement for Web Share API
- Implement clipboard fallback
- Test on different devices/browsers
- Add user feedback

### Issue 5: Responsive Design Issues
**Symptoms**: Layout broken on mobile
**Solutions**:
- Check viewport meta tag
- Test with browser dev tools
- Verify CSS media queries
- Test on actual devices

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Axios Documentation](https://axios-http.com/)
- [Fetch API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Async/Aawait Guide](https://javascript.info/async-await)
- [LocalStorage API](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [Web Share API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Share_API)
- [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [icanhazdadjoke.com API](https://icanhazdadjoke.com/api)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Install Axios library
- [ ] Implementasi komponen DadJokes
- [ ] Tambahkan reactive data untuk joke
- [ ] Implementasi API integration
- [ ] Tambahkan error handling
- [ ] Implementasi loading states
- [ ] Tambahkan history functionality
- [ ] Implementasi favorites system
- [ ] Tambahkan share functionality
- [ ] Implementasi localStorage persistence
- [ ] Tambahkan responsive design
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Dad Jokes Generator yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:

- **HTTP API integration** dengan Axios dan Fetch API
- **Asynchronous programming** dengan async/await
- **Error handling** yang robust
- **State management** untuk asynchronous data
- **Data persistence** dengan localStorage
- **User experience design** untuk loading states
- **Component-based architecture**
- **Browser API integration** (Clipboard, Web Share)
- **Progressive enhancement** dengan fallbacks
- **Mobile-friendly interactions**

---

**Happy Coding! 🚀**