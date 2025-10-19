# 🚀 Panduan Cepat: Accordions Vue.js

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
npm create vue@latest accordion-app

# Masuk ke folder project
cd accordion-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed } from "vue";

// Enhanced accordion data with categories and metadata
const accordionData = ref([
  {
    id: 1,
    title: "🌐 What is HTML?",
    content: "HTML (HyperText Markup Language) is the foundation of web development. It provides the structure and semantic meaning to web content. Modern HTML5 includes rich media elements, form controls, and semantic tags that improve accessibility and SEO.",
    category: "web-development",
    tags: ["html", "markup", "semantics"],
    open: false,
    lastModified: "2024-01-15"
  },
  {
    id: 2,
    title: "🎨 What is CSS?",
    content: "CSS (Cascading Style Sheets) is the styling language of the web. It controls the visual presentation, layout, and animations of web pages. With modern CSS3 features like Grid, Flexbox, and custom properties, developers can create stunning, responsive designs without complex JavaScript.",
    category: "web-development",
    tags: ["css", "styling", "layout", "animations"],
    open: false,
    lastModified: "2024-01-16"
  },
  {
    id: 3,
    title: "⚡ What is JavaScript?",
    content: "JavaScript is a versatile programming language that powers interactivity on the web. From simple DOM manipulation to complex single-page applications, JavaScript has evolved into a powerful ecosystem with frameworks like Vue.js, React, and Angular enabling sophisticated client-side applications.",
    category: "web-development",
    tags: ["javascript", "programming", "frameworks"],
    open: false,
    lastModified: "2024-01-17"
  },
  {
    id: 4,
    title: "🔧 What is Vue.js?",
    content: "Vue.js is a progressive JavaScript framework for building user interfaces. It's designed to be approachable, performant, and versatile. With its intuitive API, powerful reactivity system, and excellent documentation, Vue.js has become a popular choice for both beginners and experienced developers.",
    category: "frameworks",
    tags: ["vue", "framework", "javascript"],
    open: false,
    lastModified: "2024-01-18"
  },
  {
    id: 5,
    title: "📱 What is Responsive Design?",
    content: "Responsive design is an approach to web design that ensures websites work well on all devices and screen sizes. Using flexible grids, flexible images, and CSS media queries, developers can create experiences that adapt seamlessly from mobile phones to desktop computers.",
    category: "design",
    tags: ["responsive", "mobile", "css", "ux"],
    open: false,
    lastModified: "2024-01-19"
  }
]);

// Search functionality
const searchTerm = ref("");

// Category filter
const selectedCategory = ref("all");
const categories = computed(() => {
  const cats = ["all", ...new Set(accordionData.value.map(item => item.category))];
  return cats;
});

// Multi-select mode
const multiSelectMode = ref(false);

// Computed filtered items
const filteredItems = computed(() => {
  let items = accordionData.value;

  // Filter by category
  if (selectedCategory.value !== "all") {
    items = items.filter(item => item.category === selectedCategory.value);
  }

  // Filter by search term
  if (searchTerm.value) {
    const term = searchTerm.value.toLowerCase();
    items = items.filter(item =>
      item.title.toLowerCase().includes(term) ||
      item.content.toLowerCase().includes(term) ||
      item.tags.some(tag => tag.toLowerCase().includes(term))
    );
  }

  return items;
});

// Toggle accordion with multi-select support
const toggleAccordion = (index) => {
  if (multiSelectMode.value) {
    // Toggle individual item without affecting others
    accordionData.value[index].open = !accordionData.value[index].open;
  } else {
    // Single item open mode
    accordionData.value = accordionData.value.map((item, i) => ({
      ...item,
      open: i === index ? !item.open : false
    }));
  }
};

// Expand all
const expandAll = () => {
  accordionData.value.forEach(item => {
    item.open = true;
  });
};

// Collapse all
const collapseAll = () => {
  accordionData.value.forEach(item => {
    item.open = false;
  });
};

// Add new accordion item
const addNewAccordion = () => {
  const newItem = {
    id: Date.now(),
    title: "New Question",
    content: "This is a new accordion item. Click to edit and customize the content.",
    category: "general",
    tags: ["new"],
    open: false,
    lastModified: new Date().toISOString().split('T')[0]
  };
  accordionData.value.unshift(newItem);
};

// Delete accordion item
const deleteAccordion = (index) => {
  accordionData.value.splice(index, 1);
};
</script>

<template>
  <div class="app">
    <header>
      <h1>📋 Advanced Vue Accordions</h1>
      <p>Interactive FAQ with Search, Filters & Multi-Select</p>
    </header>

    <main>
      <!-- Controls Section -->
      <div class="controls-section">
        <div class="search-container">
          <input
            v-model="searchTerm"
            placeholder="🔍 Search questions..."
            class="search-input"
          />
        </div>

        <div class="filter-controls">
          <select v-model="selectedCategory" class="category-filter">
            <option v-for="category in categories" :key="category" :value="category">
              {{ category === 'all' ? '🌍 All Categories' : `📁 ${category}` }}
            </option>
          </select>

          <button
            @click="multiSelectMode = !multiSelectMode"
            :class="['mode-toggle', { active: multiSelectMode }]"
          >
            {{ multiSelectMode ? '📂 Multi-Select' : '📄 Single-Select' }}
          </button>
        </div>

        <div class="action-controls">
          <button @click="expandAll" class="action-btn expand">
            📖 Expand All
          </button>
          <button @click="collapseAll" class="action-btn collapse">
            📕 Collapse All
          </button>
          <button @click="addNewAccordion" class="action-btn add">
            ➕ Add New
          </button>
        </div>
      </div>

      <!-- Statistics -->
      <div class="stats-section">
        <div class="stat-card">
          <span class="stat-number">{{ filteredItems.length }}</span>
          <span class="stat-label">Total Items</span>
        </div>
        <div class="stat-card">
          <span class="stat-number">{{ accordionData.filter(item => item.open).length }}</span>
          <span class="stat-label">Open Items</span>
        </div>
        <div class="stat-card">
          <span class="stat-number">{{ searchTerm.length > 0 ? '🔍' : '📋' }}</span>
          <span class="stat-label">{{ searchTerm.length > 0 ? 'Filtered' : 'Showing All' }}</span>
        </div>
      </div>

      <!-- Accordions Container -->
      <div class="accordion-container">
        <div
          v-for="(item, index) in filteredItems"
          :key="item.id"
          :class="['accordion', { open: item.open, highlighted: searchTerm && (item.title.toLowerCase().includes(searchTerm.toLowerCase()) || item.content.toLowerCase().includes(searchTerm.toLowerCase())) }]"
        >
          <!-- Accordion Header -->
          <div
            class="accordion-header"
            @click="toggleAccordion(accordionData.value.indexOf(item))"
          >
            <div class="header-left">
              <span class="accordion-icon">{{ item.open ? '📖' : '📄' }}</span>
              <h3 class="accordion-title">{{ item.title }}</h3>
            </div>
            <div class="header-right">
              <span class="category-badge">{{ item.category }}</span>
              <span class="toggle-icon">{{ item.open ? '▼' : '▶' }}</span>
            </div>
          </div>

          <!-- Accordion Content -->
          <div
            v-show="item.open"
            class="accordion-content"
          >
            <div class="content-inner">
              <p class="content-text">{{ item.content }}</p>

              <!-- Tags -->
              <div class="tags-container">
                <span
                  v-for="tag in item.tags"
                  :key="tag"
                  class="tag"
                >
                  #{{ tag }}
                </span>
              </div>

              <!-- Metadata -->
              <div class="content-metadata">
                <span class="metadata-item">📅 {{ item.lastModified }}</span>
                <span class="metadata-item">🏷️ {{ item.category }}</span>
              </div>

              <!-- Delete Button -->
              <button
                @click.stop="deleteAccordion(accordionData.value.indexOf(item))"
                class="delete-btn"
              >
                🗑️ Delete
              </button>
            </div>
          </div>
        </div>

        <!-- No Results Message -->
        <div v-if="filteredItems.length === 0" class="no-results">
          <div class="no-results-icon">🔍</div>
          <h3>No Results Found</h3>
          <p>Try adjusting your search or filters to find what you're looking for.</p>
        </div>
      </div>
    </main>

    <footer>
      <div class="footer-content">
        <p>💡 Tips: Use multi-select mode to open multiple items at once</p>
        <p>Made with ❤️ using Vue.js 3 Composition API</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: white;
}

header {
  text-align: center;
  padding: 40px 20px 30px;
}

header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  font-size: 1.1em;
  opacity: 0.9;
}

main {
  max-width: 900px;
  margin: 0 auto;
  padding: 0 20px 40px;
}

/* Controls Section */
.controls-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  margin-bottom: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.search-container {
  margin-bottom: 20px;
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
}

.search-input::placeholder {
  color: rgba(255,255,255,0.7);
}

.search-input:focus {
  outline: none;
  background: rgba(255,255,255,0.3);
  box-shadow: 0 0 20px rgba(255,255,255,0.3);
}

.filter-controls {
  display: flex;
  gap: 15px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.category-filter {
  flex: 1;
  min-width: 200px;
  padding: 12px 15px;
  border: none;
  border-radius: 8px;
  background: rgba(255,255,255,0.2);
  color: white;
  font-size: 1em;
}

.category-filter option {
  background: #667eea;
  color: white;
}

.mode-toggle {
  padding: 12px 20px;
  border: none;
  border-radius: 8px;
  background: rgba(255,255,255,0.2);
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.mode-toggle.active {
  background: #4CAF50;
  transform: scale(1.05);
}

.action-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.action-btn {
  padding: 10px 15px;
  border: none;
  border-radius: 8px;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease;
}

.action-btn.expand { background: #2196F3; }
.action-btn.collapse { background: #ff9800; }
.action-btn.add { background: #4CAF50; }

.action-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
}

/* Statistics Section */
.stats-section {
  display: flex;
  gap: 20px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.stat-card {
  flex: 1;
  min-width: 150px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 20px;
  border-radius: 12px;
  text-align: center;
  border: 1px solid rgba(255,255,255,0.2);
}

.stat-number {
  display: block;
  font-size: 2em;
  font-weight: bold;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 0.9em;
  opacity: 0.8;
}

/* Accordion Styles */
.accordion-container {
  margin-bottom: 30px;
}

.accordion {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  margin-bottom: 15px;
  overflow: hidden;
  border: 1px solid rgba(255,255,255,0.2);
  transition: all 0.3s ease;
}

.accordion:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0,0,0,0.2);
}

.accordion.highlighted {
  border-color: #4CAF50;
  box-shadow: 0 0 20px rgba(76, 175, 80, 0.3);
}

.accordion-header {
  padding: 20px 25px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.3s ease;
  user-select: none;
}

.accordion-header:hover {
  background: rgba(255,255,255,0.1);
}

.header-left {
  display: flex;
  align-items: center;
  gap: 15px;
  flex: 1;
}

.accordion-icon {
  font-size: 1.5em;
  transition: transform 0.3s ease;
}

.accordion.open .accordion-icon {
  transform: scale(1.2);
}

.accordion-title {
  font-size: 1.2em;
  font-weight: 600;
  margin: 0;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 15px;
}

.category-badge {
  background: rgba(255,255,255,0.2);
  padding: 5px 10px;
  border-radius: 15px;
  font-size: 0.8em;
  font-weight: 500;
}

.toggle-icon {
  font-size: 1.2em;
  transition: transform 0.3s ease;
}

.accordion.open .toggle-icon {
  transform: rotate(180deg);
}

.accordion-content {
  border-top: 1px solid rgba(255,255,255,0.1);
}

.content-inner {
  padding: 25px;
}

.content-text {
  line-height: 1.6;
  margin-bottom: 20px;
  font-size: 1.05em;
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 15px;
}

.tag {
  background: rgba(255,255,255,0.2);
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 0.8em;
}

.content-metadata {
  display: flex;
  gap: 15px;
  margin-bottom: 15px;
  font-size: 0.9em;
  opacity: 0.7;
}

.delete-btn {
  background: rgba(244, 67, 54, 0.8);
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.delete-btn:hover {
  background: #d32f2f;
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
}

.no-results h3 {
  margin-bottom: 10px;
}

/* Footer */
footer {
  text-align: center;
  padding: 30px 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

.footer-content p {
  margin-bottom: 10px;
  opacity: 0.8;
}

/* Responsive Design */
@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .filter-controls {
    flex-direction: column;
  }

  .action-controls {
    flex-direction: column;
  }

  .stats-section {
    flex-direction: column;
  }

  .accordion-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

  .header-left {
    width: 100%;
  }

  .header-right {
    width: 100%;
    justify-content: space-between;
  }

  .content-metadata {
    flex-direction: column;
    gap: 5px;
  }
}
</style>
```

### 📋 Step 4: Jalankan Aplikasi (1 Menit)

```bash
# Start development server
npm run dev
```

**Buka browser:** `http://localhost:5173`

🎉 **Selesai! Aplikasi Advanced Accordions Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis
3. **`v-model`** - Two-way data binding
4. **`@click`** - Event handling
5. **`v-for`** - List rendering
6. **`v-show`** - Conditional display
7. **`:class`** - Dynamic class binding

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed } from "vue";
const accordionData = ref([...]);  // Data accordion
const searchTerm = ref("");        // Search input
const selectedCategory = ref("");  // Filter kategori

// 2. Computed Properties
const filteredItems = computed(() => { /* filter logic */ });

// 3. Functions
const toggleAccordion = () => { /* toggle logic */ };
const expandAll = () => { /* expand all */ };
const addNewAccordion = () => { /* add new */ };
</script>

<template>
<!-- 4. Tampilan HTML -->
</template>

<style scoped>
/* 5. Styling CSS */
</style>
```

### 🔄 Logika Accordion

```javascript
// Toggle dengan multi-select support
const toggleAccordion = (index) => {
  if (multiSelectMode.value) {
    // Mode multi-select: toggle individual item
    accordionData.value[index].open = !accordionData.value[index].open;
  } else {
    // Mode single-select: tutup yang lain
    accordionData.value = accordionData.value.map((item, i) => ({
      ...item,
      open: i === index ? !item.open : false
    }));
  }
};
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. API Integration
```vue
<script setup>
const fetchAccordionData = async () => {
  try {
    const response = await fetch('https://api.example.com/faq');
    const data = await response.json();
    accordionData.value = data.map(item => ({ ...item, open: false }));
  } catch (error) {
    console.error('Error:', error);
  }
};

onMounted(fetchAccordionData);
</script>
```

### 2. Rich Text Editor
```vue
<script setup>
const editMode = ref(false);
const editingIndex = ref(-1);

const startEdit = (index) => {
  editMode.value = true;
  editingIndex.value = index;
};

const saveEdit = (index, newContent) => {
  accordionData.value[index].content = newContent;
  editMode.value = false;
  editingIndex.value = -1;
};
</script>
```

### 3. Drag & Drop Reordering
```vue
<script setup>
const draggedIndex = ref(null);

const onDragStart = (index) => {
  draggedIndex.value = index;
};

const onDrop = (dropIndex) => {
  if (draggedIndex.value !== null && draggedIndex.value !== dropIndex) {
    const draggedItem = accordionData.value[draggedIndex.value];
    accordionData.value.splice(draggedIndex.value, 1);
    accordionData.value.splice(dropIndex, 0, draggedItem);
  }
  draggedIndex.value = null;
};
</script>
```

### 4. Export/Import Functionality
```vue
<script setup>
const exportToJSON = () => {
  const dataStr = JSON.stringify(accordionData.value, null, 2);
  const dataBlob = new Blob([dataStr], { type: 'application/json' });
  const url = URL.createObjectURL(dataBlob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'accordion-data.json';
  link.click();
};

const importFromJSON = (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = (e) => {
    accordionData.value = JSON.parse(e.target.result);
  };
  reader.readAsText(file);
};
</script>
```

### 5. Theme System
```vue
<script setup>
const themes = ['light', 'dark', 'colorful', 'minimal'];
const currentTheme = ref('dark');

const themeClasses = computed(() => ({
  'theme-light': currentTheme.value === 'light',
  'theme-dark': currentTheme.value === 'dark',
  'theme-colorful': currentTheme.value === 'colorful',
  'theme-minimal': currentTheme.value === 'minimal'
}));
</script>
```

### 6. Animation Libraries
```vue
<script setup>
import { useTransition } from '@vueuse/core';

const height = ref(0);
const heightTransition = useTransition(height);

const toggleAccordion = (index) => {
  // ... existing logic
  if (item.open) {
    height.value = contentElement.scrollHeight;
  } else {
    height.value = 0;
  }
};
</script>
```

## 🎨 Customization Ideas

### 1. Color Schemes
```css
/* Theme Variations */
.theme-colorful {
  background: linear-gradient(135deg, #ff6b6b, #4ecdc4, #45b7d1);
}

.theme-minimal {
  background: #ffffff;
  color: #333333;
}
```

### 2. Icon Libraries
```vue
<script setup>
// Using Font Awesome
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faChevronDown, faChevronRight } from '@fortawesome/free-solid-svg-icons';
</script>

<template>
  <FontAwesomeIcon :icon="item.open ? faChevronDown : faChevronRight" />
</template>
```

### 3. Advanced Animations
```css
.accordion-content {
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
  transform-origin: top;
}

.accordion-enter-active,
.accordion-leave-active {
  transition: all 0.3s ease;
}

.accordion-enter-from,
.accordion-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Accordion tidak berfungsi | Cek reactive data dan event handlers |
| Search tidak working | Verify computed property logic |
| Multi-select mode error | Check toggle logic implementation |
| Styling berantakan | Cek CSS selectors dan specificity |
| Responsive design issues | Test di berbagai screen sizes |

## 🚀 Next Steps

1. **Database Integration** - Connect to backend API
2. **User Authentication** - Login/register functionality
3. **Content Management** - Edit/delete permissions
4. **Analytics** - Track user interactions
5. **Mobile App** - Convert to PWA
6. **Advanced Search** - Full-text search with filters
7. **Collaboration** - Multiple users editing

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Accordions yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) | [VueUse](https://vueuse.org/)