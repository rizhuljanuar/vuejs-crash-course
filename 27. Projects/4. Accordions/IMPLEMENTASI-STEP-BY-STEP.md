# 📚 Implementasi Step-by-Step Accordions Vue.js

## 🎯 Panduan Detail Membuat Accordions dari Nol

### 📋 Step 1: Setup Environment Development

#### 1.1 Install Node.js
```bash
# Download Node.js dari https://nodejs.org/
# Pilih versi LTS (Recommended for Most Users)

# Verifikasi instalasi
node --version
npm --version
```

#### 1.2 Buat Project Vue dengan Vite
```bash
# Buat project baru
npm create vue@latest accordion-app

# Saat ditanya, pilih:
# ✅ TypeScript? No
# ✅ JSX Support? No
# ✅ Vue Router? No
# ✅ Pinia? No
# ✅ Vitest? No
# ✅ E2E Testing? No
# ✅ ESLint? Yes
# ✅ Prettier? Yes

# Masuk ke folder project
cd accordion-app

# Install dependencies
npm install
```

#### 1.3 Jalankan Development Server
```bash
npm run dev
```
**Result:** Aplikasi Vue default berjalan di `http://localhost:5173`

---

### 📋 Step 2: Persiapan Struktur Project

#### 2.1 Lihat Struktur Folder
```
accordion-app/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── App.vue
│   └── main.js
├── package.json
└── README.md
```

#### 2.2 Bersihkan Default Project
**Edit `src/App.vue`:**
```vue
<!-- Hapus semua isi dan ganti dengan template dasar -->
<template>
  <div id="app">
    <h1>📋 Accordions</h1>
    <p>Membuat accordion interaktif dengan Vue.js</p>
  </div>
</template>

<script setup>
// Kosongkan script untuk sementara
</script>

<style>
/* Kosongkan style untuk sementara */
</style>
```

**Result:** Aplikasi sekarang menampilkan judul "Accordions"

---

### 📋 Step 3: Implementasi Dasar Data Accordions

#### 3.1 Tambahkan Reactive Data
**Edit `src/App.vue` - Script section:**
```vue
<script setup>
import { ref } from "vue";

// 1. Data untuk accordion items
const accordionItems = ref([
  {
    id: 1,
    title: "What is HTML?",
    content: "The HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser.",
    open: false  // State untuk menunjukkan apakah item terbuka
  },
  {
    id: 2,
    title: "What is CSS?",
    content: "Cascading Style Sheets is a style sheet language used for specifying the presentation and styling of a document written in a markup language.",
    open: false
  },
  {
    id: 3,
    title: "What is JavaScript?",
    content: "JavaScript, often abbreviated as JS, is a programming language and core technology of the World Wide Web, alongside HTML and CSS.",
    open: false
  }
]);

// 2. Debug info
const debugInfo = ref({
  totalItems: accordionItems.value.length,
  openItems: 0
});
</script>
```

#### 3.2 Tampilkan Data di UI
**Edit `src/App.vue` - Template section:**
```vue
<template>
  <div id="app">
    <h1>📋 Accordions</h1>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Total Items: {{ accordionItems.length }}</p>
      <p>Open Items: {{ accordionItems.filter(item => item.open).length }}</p>
      <div class="data-preview">
        <h4>Data Preview:</h4>
        <pre>{{ JSON.stringify(accordionItems, null, 2) }}</pre>
      </div>
    </div>

    <!-- Basic Data Display -->
    <div class="basic-display">
      <h3>Basic Data Display:</h3>
      <div v-for="(item, index) in accordionItems" :key="item.id" class="item-preview">
        <p><strong>{{ index + 1 }}.</strong> {{ item.title }}</p>
        <p><em>Content:</em> {{ item.content.substring(0, 50) }}...</p>
        <p><em>Open:</em> {{ item.open ? 'Yes' : 'No' }}</p>
      </div>
    </div>
  </div>
</template>
```

#### 3.3 Tambah Basic Styling
**Edit `src/App.vue` - Style section:**
```vue
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  padding: 20px;
}

#app {
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

.debug-section {
  background: #e8f4f8;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
}

.debug-section h3 {
  margin-bottom: 15px;
  color: #2c3e50;
}

.data-preview pre {
  background: #2c3e50;
  color: #ecf0f1;
  padding: 15px;
  border-radius: 5px;
  font-size: 0.8em;
  overflow-x: auto;
}

.basic-display {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.item-preview {
  background: white;
  padding: 15px;
  margin-bottom: 10px;
  border-radius: 5px;
  border-left: 4px solid #3498db;
}
</style>
```

**Result:** Sekarang Anda bisa melihat accordion data dan debug info!

---

### 📋 Step 4: Implementasi Toggle Functionality

#### 4.1 Tambah Toggle Function
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
import { ref } from "vue";

// ... (accordion data dari step sebelumnya)

// Toggle function untuk membuka/menutup accordion
const toggleAccordion = (index) => {
  console.log(`Toggling accordion at index: ${index}`);
  console.log("Current state:", accordionItems.value[index].open);

  // Toggle open state
  accordionItems.value[index].open = !accordionItems.value[index].open;

  console.log("New state:", accordionItems.value[index].open);
};

// Function untuk toggle semua items (debug)
const toggleAll = () => {
  accordionItems.value.forEach((item, index) => {
    item.open = !item.open;
    console.log(`Item ${index + 1} is now ${item.open ? 'open' : 'closed'}`);
  });
};
</script>
```

#### 4.2 Tambah Tombol Toggle
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📋 Accordions</h1>

    <!-- Control Buttons -->
    <div class="controls">
      <button @click="toggleAll" class="control-btn">
        🔄 Toggle All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = false)" class="control-btn">
        📕 Close All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = true)" class="control-btn">
        📖 Open All
      </button>
    </div>

    <!-- Interactive Accordion Items -->
    <div class="accordion-list">
      <h3>Interactive Accordions:</h3>
      <div
        v-for="(item, index) in accordionItems"
        :key="item.id"
        class="accordion-item"
        :class="{ open: item.open }"
      >
        <div class="accordion-header" @click="toggleAccordion(index)">
          <span class="item-number">{{ index + 1 }}.</span>
          <span class="item-title">{{ item.title }}</span>
          <span class="toggle-icon">{{ item.open ? '📖' : '📄' }}</span>
        </div>

        <div class="accordion-content" v-show="item.open">
          <p>{{ item.content }}</p>
          <div class="content-info">
            <small>ID: {{ item.id }} | State: {{ item.open ? 'Open' : 'Closed' }}</small>
          </div>
        </div>
      </div>
    </div>

    <!-- Debug Section (tetap dipertahankan) -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Click on headers to toggle accordions!</p>
      <p>Open Items: {{ accordionItems.filter(item => item.open).length }} / {{ accordionItems.length }}</p>
    </div>
  </div>
</template>
```

#### 4.3 Tambah Styling untuk Accordions
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.controls {
  text-align: center;
  margin-bottom: 30px;
}

.control-btn {
  background: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 0 5px;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.3s;
}

.control-btn:hover {
  background: #2980b9;
}

.accordion-list {
  margin-bottom: 30px;
}

.accordion-list h3 {
  margin-bottom: 20px;
  color: #333;
}

.accordion-item {
  background: white;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.accordion-item.open {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  border-color: #3498db;
}

.accordion-header {
  padding: 15px 20px;
  background: #f8f9fa;
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: background 0.3s;
}

.accordion-header:hover {
  background: #e9ecef;
}

.item-number {
  font-weight: bold;
  margin-right: 10px;
  color: #3498db;
}

.item-title {
  flex: 1;
  font-weight: 600;
  color: #333;
}

.toggle-icon {
  font-size: 1.2em;
  margin-left: 10px;
  transition: transform 0.3s ease;
}

.accordion-item.open .toggle-icon {
  transform: scale(1.1);
}

.accordion-content {
  padding: 20px;
  border-top: 1px solid #ddd;
  background: #ffffff;
}

.content-info {
  margin-top: 10px;
  padding-top: 10px;
  border-top: 1px solid #e9ecef;
  color: #666;
}
</style>
```

**Result:** Sekarang accordions bisa dibuka dan ditutup dengan klik!

---

### 📋 Step 5: Implementasi Single Item Open Pattern

#### 5.1 Update Toggle Function
**Edit `src/App.vue` - Update toggle function:**
```vue
<script setup>
import { ref } from "vue";

// ... (accordion data dari step sebelumnya)

// Toggle function dengan single item open pattern
const toggleAccordion = (index) => {
  console.log(`Toggling accordion at index: ${index}`);

  // Get current item
  const currentItem = accordionItems.value[index];
  const willOpen = !currentItem.open;

  console.log(`Item ${index + 1} will ${willOpen ? 'open' : 'close'}`);

  // Update all items
  accordionItems.value = accordionItems.value.map((item, i) => ({
    ...item,  // Copy existing properties
    open: i === index ? willOpen : false  // Only toggle selected item
  }));

  console.log("All items updated:");
  accordionItems.value.forEach((item, i) => {
    console.log(`Item ${i + 1}: ${item.open ? 'open' : 'closed'}`);
  });
};

// Toggle all function (untuk testing)
const toggleAll = () => {
  const allOpen = accordionItems.value.every(item => item.open);
  accordionItems.value.forEach(item => {
    item.open = !allOpen;
  });
};
</script>
```

#### 5.2 Tambah Mode Selection
**Edit `src/App.vue` - Tambah mode selection:**
```vue
<script setup>
// ... (import dan data dari step sebelumnya)

// Mode selection: single atau multi
const mode = ref("single"); // "single" atau "multi"

// Toggle function dengan mode support
const toggleAccordion = (index) => {
  console.log(`Toggling accordion at index: ${index} in ${mode.value} mode`);

  if (mode.value === "single") {
    // Single mode: tutup yang lain
    accordionItems.value = accordionItems.value.map((item, i) => ({
      ...item,
      open: i === index ? !item.open : false
    }));
  } else {
    // Multi mode: toggle individual item
    accordionItems.value[index].open = !accordionItems.value[index].open;
  }
};

// Change mode function
const changeMode = (newMode) => {
  mode.value = newMode;
  console.log(`Changed to ${newMode} mode`);

  // Reset all items when changing mode
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};
</script>
```

#### 5.3 Update Template dengan Mode Controls
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📋 Accordions</h1>

    <!-- Mode Selection -->
    <div class="mode-selection">
      <h3>Mode Selection:</h3>
      <button
        @click="changeMode('single')"
        :class="['mode-btn', { active: mode === 'single' }]"
      >
        📄 Single Item Open
      </button>
      <button
        @click="changeMode('multi')"
        :class="['mode-btn', { active: mode === 'multi' }]"
      >
        📂 Multiple Items Open
      </button>
    </div>

    <!-- Control Buttons -->
    <div class="controls">
      <button @click="toggleAll" class="control-btn">
        🔄 Toggle All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = false)" class="control-btn">
        📕 Close All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = true)" class="control-btn" v-if="mode === 'multi'">
        📖 Open All
      </button>
    </div>

    <!-- Interactive Accordion Items -->
    <div class="accordion-list">
      <h3>Accordion List ({{ mode === 'single' ? 'Single Mode' : 'Multi Mode' }}):</h3>
      <div
        v-for="(item, index) in accordionItems"
        :key="item.id"
        class="accordion-item"
        :class="{ open: item.open }"
      >
        <div class="accordion-header" @click="toggleAccordion(index)">
          <span class="item-number">{{ index + 1 }}.</span>
          <span class="item-title">{{ item.title }}</span>
          <span class="toggle-icon">{{ item.open ? '📖' : '📄' }}</span>
        </div>

        <div class="accordion-content" v-show="item.open">
          <p>{{ item.content }}</p>
          <div class="content-info">
            <small>ID: {{ item.id }} | Mode: {{ mode }} | State: {{ item.open ? 'Open' : 'Closed' }}</small>
          </div>
        </div>
      </div>
    </div>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Current Mode: <strong>{{ mode }}</strong></p>
      <p>Open Items: {{ accordionItems.filter(item => item.open).length }} / {{ accordionItems.length }}</p>
      <p>Try switching between single and multi modes!</p>
    </div>
  </div>
</template>
```

#### 5.4 Tambah Styling untuk Mode Selection
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.mode-selection {
  text-align: center;
  margin-bottom: 30px;
}

.mode-selection h3 {
  margin-bottom: 15px;
  color: #333;
}

.mode-btn {
  background: #ecf0f1;
  color: #333;
  border: 2px solid #bdc3c7;
  padding: 10px 20px;
  margin: 0 5px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s;
}

.mode-btn.active {
  background: #3498db;
  color: white;
  border-color: #2980b9;
}

.mode-btn:hover {
  background: #d5dbdb;
}

.mode-btn.active:hover {
  background: #2980b9;
}

.accordion-list h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.mode-indicator {
  font-size: 0.8em;
  color: #666;
  background: #ecf0f1;
  padding: 5px 10px;
  border-radius: 15px;
}
</style>
```

**Result:** Aplikasi sekarang memiliki single dan multi-select modes!

---

### 📋 Step 6: Implementasi Advanced Styling

#### 6.1 Tambah Modern Design Elements
**Edit `src/App.vue` - Update CSS:**
```vue
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
  color: white;
  padding: 20px;
}

#app {
  max-width: 800px;
  margin: 0 auto;
}

h1 {
  text-align: center;
  font-size: 3em;
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

/* Modern Controls */
.mode-selection {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.mode-selection h3 {
  margin-bottom: 15px;
  text-align: center;
}

.mode-btn {
  background: rgba(255,255,255,0.2);
  color: white;
  border: 2px solid rgba(255,255,255,0.3);
  padding: 12px 20px;
  margin: 0 5px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.mode-btn.active {
  background: rgba(76, 175, 80, 0.8);
  border-color: #4CAF50;
  transform: scale(1.05);
}

.mode-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.controls {
  text-align: center;
  margin-bottom: 30px;
}

.control-btn {
  background: rgba(255,255,255,0.2);
  color: white;
  border: none;
  padding: 12px 20px;
  margin: 0 5px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  backdrop-filter: blur(5px);
}

.control-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}

/* Enhanced Accordion Styles */
.accordion-list {
  margin-bottom: 30px;
}

.accordion-list h3 {
  text-align: center;
  margin-bottom: 25px;
  font-size: 1.5em;
}

.accordion-item {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 12px;
  margin-bottom: 15px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.accordion-item.open {
  box-shadow: 0 8px 25px rgba(0,0,0,0.2);
  transform: translateY(-2px);
  border-color: rgba(76, 175, 80, 0.5);
}

.accordion-header {
  padding: 20px 25px;
  background: rgba(255,255,255,0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: all 0.3s ease;
  user-select: none;
}

.accordion-header:hover {
  background: rgba(255,255,255,0.15);
}

.item-number {
  font-weight: bold;
  margin-right: 15px;
  color: #4CAF50;
  font-size: 1.2em;
}

.item-title {
  flex: 1;
  font-weight: 600;
  font-size: 1.1em;
}

.toggle-icon {
  font-size: 1.4em;
  margin-left: 15px;
  transition: all 0.3s ease;
}

.accordion-item.open .toggle-icon {
  transform: scale(1.2) rotate(5deg);
}

.accordion-content {
  padding: 25px;
  border-top: 1px solid rgba(255,255,255,0.1);
  background: rgba(255,255,255,0.05);
}

.accordion-content p {
  line-height: 1.6;
  margin-bottom: 15px;
  font-size: 1.05em;
}

.content-info {
  padding-top: 15px;
  border-top: 1px solid rgba(255,255,255,0.1);
  color: rgba(255,255,255,0.7);
  font-size: 0.9em;
}

/* Debug Section with Modern Style */
.debug-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.debug-section h3 {
  margin-bottom: 15px;
  text-align: center;
}

.debug-section p {
  margin-bottom: 10px;
  opacity: 0.9;
}

/* Responsive Design */
@media (max-width: 768px) {
  body {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .mode-btn, .control-btn {
    padding: 10px 15px;
    margin: 5px 2px;
    font-size: 0.9em;
  }

  .accordion-header {
    padding: 15px 20px;
  }

  .item-title {
    font-size: 1em;
  }

  .accordion-content {
    padding: 20px;
  }
}
</style>
```

**Result:** Aplikasi sekarang memiliki modern glassmorphism design!

---

### 📋 Step 7: Implementasi Search Functionality

#### 7.1 Tambah Search State dan Function
**Edit `src/App.vue` - Tambah search functionality:**
```vue
<script setup>
import { ref, computed } from "vue";

// ... (accordion data dan mode dari step sebelumnya)

// Search functionality
const searchTerm = ref("");

// Computed property untuk filtered items
const filteredItems = computed(() => {
  if (!searchTerm.value) {
    return accordionItems.value;
  }

  const term = searchTerm.value.toLowerCase();
  return accordionItems.value.filter(item =>
    item.title.toLowerCase().includes(term) ||
    item.content.toLowerCase().includes(term)
  );
});

// Toggle function dengan search support
const toggleAccordion = (index) => {
  // Use filteredItems index untuk mendapatkan original index
  const item = filteredItems.value[index];
  const originalIndex = accordionItems.value.findIndex(originalItem => originalItem.id === item.id);

  console.log(`Toggling accordion "${item.title}" at original index: ${originalIndex}`);

  if (mode.value === "single") {
    // Single mode: tutup yang lain
    accordionItems.value = accordionItems.value.map((accItem, i) => ({
      ...accItem,
      open: i === originalIndex ? !accItem.open : false
    }));
  } else {
    // Multi mode: toggle individual item
    accordionItems.value[originalIndex].open = !accordionItems.value[originalIndex].open;
  }
};

// Clear search
const clearSearch = () => {
  searchTerm.value = "";
  // Close all accordions when clearing search
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};
</script>
```

#### 7.2 Tambah Search UI
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📋 Accordions</h1>

    <!-- Search Section -->
    <div class="search-section">
      <div class="search-container">
        <input
          v-model="searchTerm"
          placeholder="🔍 Search accordions..."
          class="search-input"
        />
        <button
          v-if="searchTerm"
          @click="clearSearch"
          class="clear-search-btn"
        >
          ✖️
        </button>
      </div>
      <div v-if="searchTerm" class="search-info">
        Found {{ filteredItems.length }} result(s) for "{{ searchTerm }}"
      </div>
    </div>

    <!-- Mode Selection -->
    <div class="mode-selection">
      <h3>Mode Selection:</h3>
      <button
        @click="changeMode('single')"
        :class="['mode-btn', { active: mode === 'single' }]"
      >
        📄 Single Item Open
      </button>
      <button
        @click="changeMode('multi')"
        :class="['mode-btn', { active: mode === 'multi' }]"
      >
        📂 Multiple Items Open
      </button>
    </div>

    <!-- Control Buttons -->
    <div class="controls">
      <button @click="toggleAll" class="control-btn">
        🔄 Toggle All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = false)" class="control-btn">
        📕 Close All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = true)" class="control-btn" v-if="mode === 'multi'">
        📖 Open All
      </button>
    </div>

    <!-- Interactive Accordion Items -->
    <div class="accordion-list">
      <h3>
        Accordion List ({{ mode === 'single' ? 'Single Mode' : 'Multi Mode' }})
        <span v-if="searchTerm" class="search-indicator">
          | Filtered: {{ filteredItems.length }}/{{ accordionItems.length }}
        </span>
      </h3>

      <!-- No Results Message -->
      <div v-if="filteredItems.length === 0" class="no-results">
        <div class="no-results-icon">🔍</div>
        <h3>No Results Found</h3>
        <p>No accordions match your search for "{{ searchTerm }}"</p>
        <button @click="clearSearch" class="clear-search-btn-large">
          Clear Search
        </button>
      </div>

      <!-- Filtered Accordion Items -->
      <div
        v-for="(item, index) in filteredItems"
        :key="item.id"
        class="accordion-item"
        :class="{ open: item.open, highlighted: searchTerm }"
      >
        <div class="accordion-header" @click="toggleAccordion(index)">
          <span class="item-number">{{ accordionItems.findIndex(original => original.id === item.id) + 1 }}.</span>
          <span class="item-title">{{ item.title }}</span>
          <span class="toggle-icon">{{ item.open ? '📖' : '📄' }}</span>
        </div>

        <div class="accordion-content" v-show="item.open">
          <p>{{ item.content }}</p>
          <div class="content-info">
            <small>ID: {{ item.id }} | Mode: {{ mode }} | State: {{ item.open ? 'Open' : 'Closed' }}</small>
            <small v-if="searchTerm">| Matching search term</small>
          </div>
        </div>
      </div>
    </div>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Current Mode: <strong>{{ mode }}</strong></p>
      <p>Search Term: <strong>"{{ searchTerm || 'None' }}"</strong></p>
      <p>Total Items: <strong>{{ accordionItems.length }}</strong></p>
      <p>Filtered Items: <strong>{{ filteredItems.length }}</strong></p>
      <p>Open Items: <strong>{{ accordionItems.filter(item => item.open).length }}</strong></p>
    </div>
  </div>
</template>
```

#### 7.3 Tambah Search Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Search Section */
.search-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.search-container {
  position: relative;
  max-width: 500px;
  margin: 0 auto;
}

.search-input {
  width: 100%;
  padding: 15px 20px;
  font-size: 1.1em;
  border: none;
  border-radius: 10px;
  background: rgba(255,255,255,0.2);
  color: white;
  backdrop-filter: blur(5px);
  transition: all 0.3s ease;
}

.search-input::placeholder {
  color: rgba(255,255,255,0.7);
}

.search-input:focus {
  outline: none;
  background: rgba(255,255,255,0.3);
  box-shadow: 0 0 20px rgba(255,255,255,0.3);
}

.clear-search-btn {
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(244, 67, 54, 0.8);
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  color: white;
  cursor: pointer;
  font-size: 12px;
  transition: all 0.3s ease;
}

.clear-search-btn:hover {
  background: #d32f2f;
  transform: translateY(-50%) scale(1.1);
}

.search-info {
  text-align: center;
  margin-top: 15px;
  font-size: 0.9em;
  opacity: 0.8;
}

.search-indicator {
  font-size: 0.8em;
  color: rgba(255,255,255,0.7);
  font-weight: normal;
}

/* Highlighted Search Results */
.accordion-item.highlighted {
  border-color: rgba(255, 193, 7, 0.8);
  box-shadow: 0 0 15px rgba(255, 193, 7, 0.3);
}

/* No Results */
.no-results {
  text-align: center;
  padding: 60px 20px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.no-results-icon {
  font-size: 4em;
  margin-bottom: 20px;
  opacity: 0.7;
}

.no-results h3 {
  margin-bottom: 15px;
  font-size: 1.5em;
}

.no-results p {
  margin-bottom: 25px;
  opacity: 0.8;
}

.clear-search-btn-large {
  background: rgba(255,255,255,0.2);
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.clear-search-btn-large:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

/* Update existing styles for better integration */
.accordion-list h3 {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
}
</style>
```

**Result:** Aplikasi sekarang memiliki search functionality dengan highlighting!

---

### 📋 Step 8: Implementasi Categories & Filtering

#### 8.1 Tambah Category Data
**Edit `src/App.vue` - Update data structure:**
```vue
<script setup>
import { ref, computed } from "vue";

// Enhanced accordion data with categories
const accordionItems = ref([
  {
    id: 1,
    title: "What is HTML?",
    content: "The HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser.",
    category: "web-technologies",
    open: false
  },
  {
    id: 2,
    title: "What is CSS?",
    content: "Cascading Style Sheets is a style sheet language used for specifying the presentation and styling of a document written in a markup language.",
    category: "web-technologies",
    open: false
  },
  {
    id: 3,
    title: "What is JavaScript?",
    content: "JavaScript, often abbreviated as JS, is a programming language and core technology of the World Wide Web, alongside HTML and CSS.",
    category: "web-technologies",
    open: false
  },
  {
    id: 4,
    title: "What is Vue.js?",
    content: "Vue.js is a progressive JavaScript framework for building user interfaces. It's designed to be approachable, performant, and versatile.",
    category: "frameworks",
    open: false
  },
  {
    id: 5,
    title: "What is React?",
    content: "React is a JavaScript library for building user interfaces. It is maintained by Facebook and a community of individual developers and companies.",
    category: "frameworks",
    open: false
  },
  {
    id: 6,
    title: "What is Responsive Design?",
    content: "Responsive design is an approach to web design that makes web pages render well on a variety of devices and window sizes.",
    category: "design",
    open: false
  }
]);

// ... (search, mode, dan functions dari step sebelumnya)

// Category filter
const selectedCategory = ref("all");

// Computed property untuk categories
const categories = computed(() => {
  const uniqueCategories = [...new Set(accordionItems.value.map(item => item.category))];
  return ["all", ...uniqueCategories];
});

// Enhanced computed property untuk filtered items
const filteredItems = computed(() => {
  let items = accordionItems.value;

  // Filter by category
  if (selectedCategory.value !== "all") {
    items = items.filter(item => item.category === selectedCategory.value);
  }

  // Filter by search term
  if (searchTerm.value) {
    const term = searchTerm.value.toLowerCase();
    items = items.filter(item =>
      item.title.toLowerCase().includes(term) ||
      item.content.toLowerCase().includes(term)
    );
  }

  return items;
});

// Category display names
const categoryDisplayNames = {
  "all": "🌍 All Categories",
  "web-technologies": "🌐 Web Technologies",
  "frameworks": "⚡ Frameworks",
  "design": "🎨 Design"
};
</script>
```

#### 8.2 Tambah Category UI
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>📋 Accordions</h1>

    <!-- Search Section -->
    <div class="search-section">
      <div class="search-container">
        <input
          v-model="searchTerm"
          placeholder="🔍 Search accordions..."
          class="search-input"
        />
        <button
          v-if="searchTerm"
          @click="clearSearch"
          class="clear-search-btn"
        >
          ✖️
        </button>
      </div>
      <div v-if="searchTerm" class="search-info">
        Found {{ filteredItems.length }} result(s) for "{{ searchTerm }}"
      </div>
    </div>

    <!-- Category Filter -->
    <div class="category-section">
      <h3>📁 Filter by Category:</h3>
      <div class="category-buttons">
        <button
          v-for="category in categories"
          :key="category"
          @click="selectedCategory = category"
          :class="['category-btn', { active: selectedCategory === category }]"
        >
          {{ categoryDisplayNames[category] }}
        </button>
      </div>
    </div>

    <!-- Mode Selection -->
    <div class="mode-selection">
      <h3>Mode Selection:</h3>
      <button
        @click="changeMode('single')"
        :class="['mode-btn', { active: mode === 'single' }]"
      >
        📄 Single Item Open
      </button>
      <button
        @click="changeMode('multi')"
        :class="['mode-btn', { active: mode === 'multi' }]"
      >
        📂 Multiple Items Open
      </button>
    </div>

    <!-- Control Buttons -->
    <div class="controls">
      <button @click="toggleAll" class="control-btn">
        🔄 Toggle All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = false)" class="control-btn">
        📕 Close All
      </button>
      <button @click="() => accordionItems.forEach(item => item.open = true)" class="control-btn" v-if="mode === 'multi'">
        📖 Open All
      </button>
    </div>

    <!-- Interactive Accordion Items -->
    <div class="accordion-list">
      <h3>
        Accordion List ({{ mode === 'single' ? 'Single Mode' : 'Multi Mode' }})
        <span v-if="selectedCategory !== 'all'" class="category-indicator">
          | Category: {{ categoryDisplayNames[selectedCategory] }}
        </span>
        <span v-if="searchTerm" class="search-indicator">
          | Filtered: {{ filteredItems.length }}/{{ accordionItems.length }}
        </span>
      </h3>

      <!-- No Results Message -->
      <div v-if="filteredItems.length === 0" class="no-results">
        <div class="no-results-icon">🔍</div>
        <h3>No Results Found</h3>
        <p v-if="searchTerm && selectedCategory !== 'all'">
          No accordions match your search for "{{ searchTerm }}" in category "{{ selectedCategory }}"
        </p>
        <p v-else-if="searchTerm">
          No accordions match your search for "{{ searchTerm }}"
        </p>
        <p v-else>
          No accordions found in category "{{ selectedCategory }}"
        </p>
        <button @click="resetFilters" class="clear-search-btn-large">
          Reset Filters
        </button>
      </div>

      <!-- Filtered Accordion Items -->
      <div
        v-for="(item, index) in filteredItems"
        :key="item.id"
        class="accordion-item"
        :class="{ open: item.open, highlighted: searchTerm }"
      >
        <div class="accordion-header" @click="toggleAccordion(index)">
          <span class="item-number">{{ accordionItems.findIndex(original => original.id === item.id) + 1 }}.</span>
          <span class="item-title">{{ item.title }}</span>
          <span class="category-badge">{{ item.category }}</span>
          <span class="toggle-icon">{{ item.open ? '📖' : '📄' }}</span>
        </div>

        <div class="accordion-content" v-show="item.open">
          <p>{{ item.content }}</p>
          <div class="content-info">
            <small>ID: {{ item.id }} | Category: {{ item.category }} | Mode: {{ mode }}</small>
            <small v-if="searchTerm">| Matching search term</small>
          </div>
        </div>
      </div>
    </div>

    <!-- Debug Section -->
    <div class="debug-section">
      <h3>Debug Info:</h3>
      <p>Current Mode: <strong>{{ mode }}</strong></p>
      <p>Selected Category: <strong>{{ selectedCategory }}</strong></p>
      <p>Search Term: <strong>"{{ searchTerm || 'None' }}"</strong></p>
      <p>Total Items: <strong>{{ accordionItems.length }}</strong></p>
      <p>Filtered Items: <strong>{{ filteredItems.length }}</strong></p>
      <p>Open Items: <strong>{{ accordionItems.filter(item => item.open).length }}</strong></p>
    </div>
  </div>
</template>
```

#### 8.3 Tambah Function untuk Reset Filters
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
// ... (kode sebelumnya)

// Reset all filters
const resetFilters = () => {
  searchTerm.value = "";
  selectedCategory.value = "all";
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};

// Update clear search function
const clearSearch = () => {
  searchTerm.value = "";
  // Close all accordions when clearing search
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};
</script>
```

#### 8.4 Tambah Category Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

/* Category Section */
.category-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.category-section h3 {
  text-align: center;
  margin-bottom: 15px;
}

.category-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.category-btn {
  background: rgba(255,255,255,0.2);
  color: white;
  border: none;
  padding: 10px 15px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
  font-size: 0.9em;
}

.category-btn.active {
  background: rgba(156, 39, 176, 0.8);
  transform: scale(1.05);
}

.category-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.category-indicator {
  font-size: 0.8em;
  color: rgba(255,255,255,0.7);
  font-weight: normal;
}

/* Category Badge */
.category-badge {
  background: rgba(156, 39, 176, 0.7);
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.8em;
  font-weight: 500;
  margin-right: 10px;
}

/* Update accordion header for category badge */
.accordion-header {
  padding: 20px 25px;
  background: rgba(255,255,255,0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: all 0.3s ease;
  user-select: none;
  gap: 15px;
}

.item-title {
  flex: 1;
  font-weight: 600;
  font-size: 1.1em;
}

/* Enhanced no results */
.no-results p {
  margin-bottom: 15px;
  opacity: 0.8;
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

.clear-search-btn-large {
  background: rgba(156, 39, 176, 0.8);
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.clear-search-btn-large:hover {
  background: rgba(156, 39, 176, 0.9);
  transform: translateY(-2px);
}
</style>
```

**Result:** Aplikasi sekarang memiliki category filtering dengan visual indicators!

---

### 📋 Step 9: Refactor ke Component Architecture

#### 9.1 Buat Component AccordionItem
**Buat file `src/components/AccordionItem.vue`:**
```vue
<template>
  <div
    class="accordion-item"
    :class="{
      open: modelValue.open,
      highlighted: searchTerm
    }"
  >
    <div class="accordion-header" @click="$emit('toggle')">
      <span class="item-number">{{ index + 1 }}.</span>
      <span class="item-title">{{ modelValue.title }}</span>
      <span class="category-badge">{{ modelValue.category }}</span>
      <span class="toggle-icon">{{ modelValue.open ? '📖' : '📄' }}</span>
    </div>

    <div class="accordion-content" v-show="modelValue.open">
      <p>{{ modelValue.content }}</p>
      <div class="content-info">
        <small>ID: {{ modelValue.id }} | Category: {{ modelValue.category }}</small>
        <small v-if="searchTerm">| Matching search term</small>
      </div>
    </div>
  </div>
</template>

<script setup>
// Props
defineProps({
  modelValue: {
    type: Object,
    required: true
  },
  index: {
    type: Number,
    required: true
  },
  searchTerm: {
    type: String,
    default: ""
  }
});

// Events
defineEmits(['toggle']);
</script>

<style scoped>
.accordion-item {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.2);
  border-radius: 12px;
  margin-bottom: 15px;
  overflow: hidden;
  transition: all 0.3s ease;
}

.accordion-item.open {
  box-shadow: 0 8px 25px rgba(0,0,0,0.2);
  transform: translateY(-2px);
  border-color: rgba(76, 175, 80, 0.5);
}

.accordion-item.highlighted {
  border-color: rgba(255, 193, 7, 0.8);
  box-shadow: 0 0 15px rgba(255, 193, 7, 0.3);
}

.accordion-header {
  padding: 20px 25px;
  background: rgba(255,255,255,0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  transition: all 0.3s ease;
  user-select: none;
  gap: 15px;
}

.accordion-header:hover {
  background: rgba(255,255,255,0.15);
}

.item-number {
  font-weight: bold;
  color: #4CAF50;
  font-size: 1.2em;
}

.item-title {
  flex: 1;
  font-weight: 600;
  font-size: 1.1em;
}

.category-badge {
  background: rgba(156, 39, 176, 0.7);
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.8em;
  font-weight: 500;
}

.toggle-icon {
  font-size: 1.4em;
  transition: all 0.3s ease;
}

.accordion-item.open .toggle-icon {
  transform: scale(1.2) rotate(5deg);
}

.accordion-content {
  padding: 25px;
  border-top: 1px solid rgba(255,255,255,0.1);
  background: rgba(255,255,255,0.05);
}

.accordion-content p {
  line-height: 1.6;
  margin-bottom: 15px;
  font-size: 1.05em;
}

.content-info {
  padding-top: 15px;
  border-top: 1px solid rgba(255,255,255,0.1);
  color: rgba(255,255,255,0.7);
  font-size: 0.9em;
}
</style>
```

#### 9.2 Buat Component SearchBar
**Buat file `src/components/SearchBar.vue`:**
```vue
<template>
  <div class="search-section">
    <div class="search-container">
      <input
        :value="modelValue"
        @input="$emit('update:modelValue', $event.target.value)"
        placeholder="🔍 Search accordions..."
        class="search-input"
      />
      <button
        v-if="modelValue"
        @click="$emit('clear')"
        class="clear-search-btn"
      >
        ✖️
      </button>
    </div>
    <div v-if="modelValue && filteredCount !== undefined" class="search-info">
      Found {{ filteredCount }} result(s) for "{{ modelValue }}"
    </div>
  </div>
</template>

<script setup>
// Props
defineProps({
  modelValue: {
    type: String,
    default: ""
  },
  filteredCount: {
    type: Number,
    default: undefined
  }
});

// Events
defineEmits(['update:modelValue', 'clear']);
</script>

<style scoped>
.search-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.search-container {
  position: relative;
  max-width: 500px;
  margin: 0 auto;
}

.search-input {
  width: 100%;
  padding: 15px 20px;
  font-size: 1.1em;
  border: none;
  border-radius: 10px;
  background: rgba(255,255,255,0.2);
  color: white;
  backdrop-filter: blur(5px);
  transition: all 0.3s ease;
}

.search-input::placeholder {
  color: rgba(255,255,255,0.7);
}

.search-input:focus {
  outline: none;
  background: rgba(255,255,255,0.3);
  box-shadow: 0 0 20px rgba(255,255,255,0.3);
}

.clear-search-btn {
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(244, 67, 54, 0.8);
  border: none;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  color: white;
  cursor: pointer;
  font-size: 12px;
  transition: all 0.3s ease;
}

.clear-search-btn:hover {
  background: #d32f2f;
  transform: translateY(-50%) scale(1.1);
}

.search-info {
  text-align: center;
  margin-top: 15px;
  font-size: 0.9em;
  opacity: 0.8;
}
</style>
```

#### 9.3 Update App.vue (Main Component)
**Edit `src/App.vue` menjadi:**
```vue
<script setup>
import { ref, computed } from "vue";
import AccordionItem from './components/AccordionItem.vue';
import SearchBar from './components/SearchBar.vue';

// Data
const accordionItems = ref([
  // ... (sama seperti step sebelumnya)
]);

// State
const mode = ref("single");
const searchTerm = ref("");
const selectedCategory = ref("all");

// Computed
const categories = computed(() => {
  const uniqueCategories = [...new Set(accordionItems.value.map(item => item.category))];
  return ["all", ...uniqueCategories];
});

const categoryDisplayNames = {
  "all": "🌍 All Categories",
  "web-technologies": "🌐 Web Technologies",
  "frameworks": "⚡ Frameworks",
  "design": "🎨 Design"
};

const filteredItems = computed(() => {
  let items = accordionItems.value;

  if (selectedCategory.value !== "all") {
    items = items.filter(item => item.category === selectedCategory.value);
  }

  if (searchTerm.value) {
    const term = searchTerm.value.toLowerCase();
    items = items.filter(item =>
      item.title.toLowerCase().includes(term) ||
      item.content.toLowerCase().includes(term)
    );
  }

  return items;
});

// Methods
const toggleAccordion = (index) => {
  const item = filteredItems.value[index];
  const originalIndex = accordionItems.value.findIndex(originalItem => originalItem.id === item.id);

  if (mode.value === "single") {
    accordionItems.value = accordionItems.value.map((accItem, i) => ({
      ...accItem,
      open: i === originalIndex ? !accItem.open : false
    }));
  } else {
    accordionItems.value[originalIndex].open = !accordionItems.value[originalIndex].open;
  }
};

const changeMode = (newMode) => {
  mode.value = newMode;
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};

const toggleAll = () => {
  const allOpen = filteredItems.value.every(item => item.open);
  filteredItems.value.forEach(item => {
    const originalItem = accordionItems.value.find(original => original.id === item.id);
    if (originalItem) {
      originalItem.open = !allOpen;
    }
  });
};

const resetFilters = () => {
  searchTerm.value = "";
  selectedCategory.value = "all";
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};

const clearSearch = () => {
  searchTerm.value = "";
  accordionItems.value.forEach(item => {
    item.open = false;
  });
};
</script>

<template>
  <div id="app">
    <header>
      <h1>📋 Vue.js Accordions</h1>
      <p>Interactive FAQ with Search, Filters & Components</p>
    </header>

    <main>
      <SearchBar
        v-model="searchTerm"
        :filtered-count="filteredItems.length"
        @clear="clearSearch"
      />

      <!-- Category Filter -->
      <div class="category-section">
        <h3>📁 Filter by Category:</h3>
        <div class="category-buttons">
          <button
            v-for="category in categories"
            :key="category"
            @click="selectedCategory = category"
            :class="['category-btn', { active: selectedCategory === category }]"
          >
            {{ categoryDisplayNames[category] }}
          </button>
        </div>
      </div>

      <!-- Mode Selection -->
      <div class="mode-selection">
        <h3>Mode Selection:</h3>
        <button
          @click="changeMode('single')"
          :class="['mode-btn', { active: mode === 'single' }]"
        >
          📄 Single Item Open
        </button>
        <button
          @click="changeMode('multi')"
          :class="['mode-btn', { active: mode === 'multi' }]"
        >
          📂 Multiple Items Open
        </button>
      </div>

      <!-- Control Buttons -->
      <div class="controls">
        <button @click="toggleAll" class="control-btn">
          🔄 Toggle Filtered
        </button>
        <button @click="() => accordionItems.forEach(item => item.open = false)" class="control-btn">
          📕 Close All
        </button>
        <button @click="() => accordionItems.forEach(item => item.open = true)" class="control-btn" v-if="mode === 'multi'">
          📖 Open All
        </button>
        <button @click="resetFilters" class="control-btn">
          🔄 Reset Filters
        </button>
      </div>

      <!-- Accordion List -->
      <div class="accordion-list">
        <h3>
          Accordion List ({{ mode === 'single' ? 'Single Mode' : 'Multi Mode' }})
          <span v-if="selectedCategory !== 'all'" class="category-indicator">
            | Category: {{ categoryDisplayNames[selectedCategory] }}
          </span>
          <span v-if="searchTerm" class="search-indicator">
            | Filtered: {{ filteredItems.length }}/{{ accordionItems.length }}
          </span>
        </h3>

        <!-- No Results -->
        <div v-if="filteredItems.length === 0" class="no-results">
          <div class="no-results-icon">🔍</div>
          <h3>No Results Found</h3>
          <button @click="resetFilters" class="clear-search-btn-large">
            Reset Filters
          </button>
        </div>

        <!-- Accordion Items -->
        <AccordionItem
          v-for="(item, index) in filteredItems"
          :key="item.id"
          :model-value="item"
          :index="accordionItems.findIndex(original => original.id === item.id)"
          :search-term="searchTerm"
          @toggle="toggleAccordion(index)"
        />
      </div>

      <!-- Debug Section -->
      <div class="debug-section">
        <h3>🐛 Component Architecture Debug:</h3>
        <p>✅ Search Component: Implemented</p>
        <p>✅ AccordionItem Component: Implemented</p>
        <p>✅ Props & Events: Working</p>
        <p>✅ Reactive Data: Connected</p>
        <p>✅ Component Communication: Active</p>
      </div>
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js 3 Composition API</p>
    </footer>
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
  color: white;
}

#app {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 20px;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header {
  text-align: center;
  margin-bottom: 40px;
}

header h1 {
  font-size: 3em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  font-size: 1.2em;
  opacity: 0.9;
}

main {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 30px;
}

/* Component Styles (diimport dari components) */

/* Layout Styles */
.category-section, .mode-selection, .controls, .accordion-list, .debug-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.category-section h3, .mode-selection h3, .accordion-list h3, .debug-section h3 {
  text-align: center;
  margin-bottom: 20px;
}

.category-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.category-btn, .mode-btn, .control-btn {
  background: rgba(255,255,255,0.2);
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

.category-btn.active, .mode-btn.active {
  background: rgba(76, 175, 80, 0.8);
  transform: scale(1.05);
}

.category-btn:hover, .mode-btn:hover, .control-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.controls {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.no-results {
  text-align: center;
  padding: 60px 20px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.no-results-icon {
  font-size: 4em;
  margin-bottom: 20px;
  opacity: 0.7;
}

.clear-search-btn-large {
  background: rgba(76, 175, 80, 0.8);
  color: white;
  border: none;
  padding: 12px 25px;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
}

footer {
  text-align: center;
  margin-top: 40px;
  opacity: 0.7;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

/* Responsive Design */
@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .category-buttons, .controls {
    flex-direction: column;
  }

  .category-btn, .mode-btn, .control-btn {
    width: 100%;
  }
}
</style>
```

**Result:** Aplikasi sekarang menggunakan component-based architecture yang modular!

---

### 📋 Step 10: Final Testing & Deployment

#### 10.1 Test Semua Fitur
**Coba semua fungsi:**
- ✅ Accordion toggle (single dan multi mode)
- ✅ Search functionality dengan highlighting
- ✅ Category filtering
- ✅ Mode switching (single/multi)
- ✅ Control buttons (toggle, close all, open all, reset)
- ✅ Component communication
- ✅ Responsive design
- ✅ Error handling (no results)

#### 10.2 Debug Issues
**Common issues & fixes:**
```vue
<!-- Issue: Component tidak menerima props -->
<!-- Fix: Pastikan props definition benar -->
defineProps({
  modelValue: { type: Object, required: true }
});

<!-- Issue: Events tidak diterima -->
<!-- Fix: Pastikan emit definition benar -->
defineEmits(['toggle']);

<!-- Issue: Computed tidak update -->
<!-- Fix: Pastikan dependency tracking benar -->
const filteredItems = computed(() => {
  // Logic yang benar
});
```

#### 10.3 Build untuk Production
```bash
# Build aplikasi
npm run build

# Preview build
npm run preview

# Hasilnya ada di folder /dist
```

#### 10.4 Deployment
```bash
# 1. Push ke GitHub
git add .
git commit -m "Complete Vue Accordions with Component Architecture"
git push origin main

# 2. Deploy ke Netlify/Vercel
# - Connect GitHub repository
# - Auto deploy dari main branch
# - Settings: Build command = npm run build
# - Settings: Publish directory = dist
```

---

## 🎉 Hasil Akhir

✅ **Aplikasi Accordions Lengkap dengan:**
- Interactive accordion panels
- Single dan multi-select modes
- Search functionality dengan highlighting
- Category filtering system
- Component-based architecture
- Modern glassmorphism design
- Responsive layout
- Error handling untuk no results
- Production-ready code

## 📚 Yang Telah Dipelajari

1. **Vue.js Fundamentals**
   - Composition API (`<script setup>`)
   - Reactive data (`ref()`)
   - Computed properties
   - Props & Events system
   - Component architecture

2. **Advanced Concepts**
   - Array manipulation methods
   - String searching dan filtering
   - State management patterns
   - Component communication
   - Dynamic styling

3. **UI/UX Best Practices**
   - User feedback systems
   - Responsive design
   - Accessibility considerations
   - Visual hierarchy
   - Modern CSS techniques

## 🚀 Next Steps

1. **Animation Libraries** - GSAP atau Vue Transition
2. **API Integration** - Dynamic data loading
3. **User Authentication** - Private accordions
4. **Advanced Search** - Full-text search engine
5. **Mobile App** - PWA conversion
6. **Database Integration** - CRUD operations
7. **Advanced Features** - Drag & drop, nested accordions

---

**🎯 Mission Accomplished!**
Anda berhasil membuat aplikasi Accordions yang fully functional dengan Vue.js 3 dan component-based architecture!

**💡 Pro Tip:** Lanjutkan dengan menambah advanced features atau mulai project Vue.js lainnya untuk memperdalam pemahaman!

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [Component Guide](https://vuejs.org/guide/components/registration.html) | [MDN Web Docs](https://developer.mozilla.org/)