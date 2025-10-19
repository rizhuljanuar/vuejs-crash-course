# Accordions - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Accordions adalah aplikasi web yang menampilkan konten dalam format accordion (collapsible panels). Aplikasi ini dibuat menggunakan Vue.js 3 dengan Composition API dan memungkinkan user untuk mengexpand dan collapse konten dengan smooth animations. Project ini sangat cocok untuk FAQ, dokumentasi, atau konten yang terorganisir dalam kategori.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- List rendering dengan `v-for`
- Conditional rendering dengan `v-show`
- Event handling untuk user interactions
- Dynamic styling dan animations
- Array manipulation methods
- State management patterns
- Component-based architecture
- Responsive design principles

## 📋 Fitur Aplikasi

1. **Expandable/Collapsible Panels**: Panel yang bisa dibuka dan ditutup
2. **Single Item Open**: Hanya satu panel yang bisa dibuka pada satu waktu
3. **Smooth Transitions**: Animasi halus saat membuka/menutup
4. **Visual Indicators**: Arrow icons yang menunjukkan state panel
5. **Responsive Design**: Layout yang bekerja di semua device sizes
6. **Hover Effects**: Visual feedback saat user hover
7. **Content Organization**: Konten terstruktur dengan rapi

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)

## 📂 Struktur Project

```
4. Accordions/
├── App.vue                              # Komponen utama aplikasi
├── components/
│   └── AccordionComponent.vue           # Komponen untuk accordion
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
npm create vue@latest accordion-app
cd accordion-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create accordion-app
cd accordion-app
```

### Langkah 3: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style scoped>`**: Bagian CSS untuk styling

### Langkah 4: Membuat Komponen AccordionComponent

**File: `components/AccordionComponent.vue`**

#### Bagian Script Setup
```vue
<script setup>
import { ref } from "vue";

// Data accordion items dengan title, content, dan state
const items = ref([
  {
    title: "What is HTML?",
    content: "The HyperText Markup Language or HTML is the standard markup language for documents designed to be displayed in a web browser. It defines the content and structure of web content. It is often assisted by technologies such as Cascading Style Sheets and scripting languages such as JavaScript.",
    open: false,  // State untuk menunjukkan apakah panel terbuka
  },
  {
    title: "What is CSS?",
    content: "Cascading Style Sheets is a style sheet language used for specifying the presentation and styling of a document written in a markup language such as HTML or XML. CSS is a cornerstone technology of the World Wide Web, alongside HTML and JavaScript.",
    open: false,
  },
  {
    title: "What is JavaScript?",
    content: "JavaScript, often abbreviated as JS, is a programming language and core technology of the World Wide Web, alongside HTML and CSS. As of 2023, 98.7% of websites use JavaScript on the client side for webpage behavior, often incorporating third-party libraries.",
    open: false,
  },
]);

// Fungsi untuk toggle accordion item
const toggleAccordion = (index) => {
  // Map melalui semua items dan update state
  items.value = items.value.map((item, i) => ({
    ...item,  // Copy existing properties
    open: i === index ? !item.open : false,  // Toggle selected, close others
  }));
};
</script>
```

#### Penjelasan Kode:

1. **`import { ref } from "vue"`**
   - Mengimpor `ref` untuk membuat reactive variables
   - `ref` membuat data yang otomatis update UI saat berubah

2. **`const items = ref([...])`**
   - Array reactive untuk menyimpan data accordion
   - Setiap item memiliki properti: `title`, `content`, dan `open`
   - Properti `open` menentukan visibility panel

3. **`toggleAccordion(index)` Function**
   - Menerima parameter `index` untuk menentukan item mana yang dipilih
   - Menggunakan `map()` untuk membuat array baru
   - Menggunakan spread operator `...item` untuk copy properties
   - Logic: Toggle item yang dipilih, tutup semua item lain

4. **Single Item Open Pattern**
   - Hanya satu accordion yang bisa terbuka pada satu waktu
   - Memberikan user experience yang lebih clean
   - Menghindari clutter dengan terlalu banyak panel terbuka

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="accordion-container">
    <!-- Loop melalui semua accordion items -->
    <div
      v-for="(item, index) in items"
      :key="index"
      class="accordion"
    >
      <!-- Header yang bisa diklik untuk toggle -->
      <div
        class="accordion-header"
        @click="toggleAccordion(index)"
      >
        <span class="header-text">{{ item.title }}</span>
        <!-- Arrow icon yang berubah berdasarkan state -->
        <span class="arrow-icon">{{ item.open ? "▼" : "▶" }}</span>
      </div>

      <!-- Content yang muncul/hilang berdasarkan state -->
      <div
        v-show="item.open"
        class="accordion-content"
      >
        <div class="content-inner">
          {{ item.content }}
        </div>
      </div>
    </div>
  </div>
</template>
```

#### Penjelasan Template:

1. **`v-for="(item, index) in items"`**
   - Looping untuk setiap item dalam array `items`
   - `item` adalah object data accordion
   - `index` adalah posisi item dalam array
   - `:key="index"` untuk unique identifier (wajib untuk v-for)

2. **`@click="toggleAccordion(index)"`**
   - Event listener untuk click pada header
   - Memanggil fungsi `toggleAccordion` dengan parameter index
   - Menangani user interaction untuk membuka/menutup panel

3. **`v-show="item.open"`**
   - Conditional rendering berdasarkan properti `open`
   - `v-show` menggunakan `display: none/block` untuk visibility
   - Content muncul jika `item.open` bernilai `true`

4. **Dynamic Arrow Icon**
   - `{{ item.open ? "▼" : "▶" }}`
   - Ternary operator untuk conditional display
   - Arrow down (▼) jika panel terbuka
   - Arrow right (▶) jika panel tertutup

### Langkah 6: Styling dengan CSS

```vue
<style scoped>
/* Container utama untuk accordion */
.accordion-container {
  max-width: 800px;
  margin: 50px auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* Individual accordion item */
.accordion {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  margin-bottom: 12px;
  transition: all 0.3s ease;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.accordion:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

/* Header section */
.accordion-header {
  background: linear-gradient(135deg, #3498db 0%, #2980b9 100%);
  color: white;
  padding: 20px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.3s ease;
  user-select: none; /* Mencegah text selection */
}

.accordion-header:hover {
  background: linear-gradient(135deg, #2980b9 0%, #21618c 100%);
}

.accordion-header:active {
  transform: translateY(1px);
}

/* Header text styling */
.header-text {
  font-weight: 600;
  font-size: 1.1em;
}

/* Arrow icon styling */
.arrow-icon {
  font-size: 1.2em;
  transition: transform 0.3s ease;
  color: rgba(255, 255, 255, 0.8);
}

/* Content section */
.accordion-content {
  background-color: #f8f9fa;
  border-top: 1px solid #e0e0e0;
  overflow: hidden;
  transition: all 0.3s ease;
}

.content-inner {
  padding: 20px;
  line-height: 1.6;
  color: #333;
  font-size: 1em;
}

/* Animation untuk smooth height transition */
.accordion-content {
  max-height: 0;
  opacity: 0;
  transition: max-height 0.3s ease, opacity 0.3s ease;
}

.accordion-content[style] {
  max-height: 1000px;
  opacity: 1;
}

/* Responsive design */
@media (max-width: 768px) {
  .accordion-container {
    margin: 20px 10px;
    padding: 10px;
  }

  .accordion-header {
    padding: 15px;
  }

  .header-text {
    font-size: 1em;
  }

  .content-inner {
    padding: 15px;
    font-size: 0.9em;
  }
}

/* Focus styles untuk accessibility */
.accordion-header:focus {
  outline: 2px solid #ff6b6b;
  outline-offset: 2px;
}

/* Animation class untuk panel yang terbuka */
.accordion.active .accordion-header {
  background: linear-gradient(135deg, #2ecc71 0%, #27ae60 100%);
}
</style>
```

#### Penjelasan CSS:

1. **Modern Gradient Design**
   - Menggunakan `linear-gradient` untuk header yang menarik
   - Shadow effects untuk depth perception
   - Smooth transitions untuk semua interactions

2. **Interactive Elements**
   - `:hover` states untuk visual feedback
   - `:active` states untuk click feedback
   - `transition` untuk smooth animations

3. **Responsive Design**
   - Media queries untuk mobile devices
   - Flexible layout yang menyesuaikan screen size
   - Optimized padding dan font sizes

4. **Accessibility Features**
   - Focus states untuk keyboard navigation
   - `user-select: none` untuk mencegah text selection pada header
   - Semantic HTML structure

### Langkah 7: Membuat App.vue

**File: `App.vue`**
```vue
<script setup>
import AccordionComponent from './components/AccordionComponent.vue'
</script>

<template>
  <div id="app">
    <header>
      <h1>📋 Vue.js Accordions</h1>
      <p>Interactive FAQ with Smooth Animations</p>
    </header>

    <main>
      <div class="section-intro">
        <h2>About This Project</h2>
        <p>This accordion component demonstrates Vue.js fundamentals including reactive data, event handling, and conditional rendering. Click on any question below to reveal the answer.</p>
      </div>

      <AccordionComponent />

      <div class="features-section">
        <h3>✨ Features Demonstrated:</h3>
        <ul>
          <li>🔄 Reactive data management</li>
          <li>🖱️ Event handling</li>
          <li>📱 Conditional rendering</li>
          <li>🎨 Dynamic styling</li>
          <li>📊 Array manipulation</li>
          <li>🎭 Component architecture</li>
        </ul>
      </div>
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js 3</p>
    </footer>
  </div>
</template>

<style>
/* Global styles */
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
  max-width: 1000px;
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
  gap: 40px;
}

.section-intro {
  text-align: center;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.section-intro h2 {
  margin-bottom: 15px;
  color: white;
}

.section-intro p {
  line-height: 1.6;
  opacity: 0.9;
}

.features-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.features-section h3 {
  margin-bottom: 20px;
  color: white;
  text-align: center;
}

.features-section ul {
  list-style: none;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.features-section li {
  background: rgba(255,255,255,0.1);
  padding: 15px;
  border-radius: 8px;
  border: 1px solid rgba(255,255,255,0.1);
}

footer {
  text-align: center;
  margin-top: 40px;
  opacity: 0.7;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
}

@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .features-section ul {
    grid-template-columns: 1fr;
  }
}
</style>
```

### Langkah 8: Menjalankan Aplikasi

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Jalankan Development Server**
   ```bash
   npm run dev
   ```

3. **Buka Browser**
   - Buka alamat yang ditampilkan di terminal
   - Aplikasi Accordions siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API
- Menggunakan `<script setup>` untuk syntax yang lebih ringkas
- Organisasi logika yang lebih modular dan maintainable

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- State management untuk accordion items

### 3. List Rendering
- `v-for` untuk rendering dynamic lists
- `:key` untuk optimal performance dan identity tracking

### 4. Conditional Rendering
- `v-show` untuk conditional display
- Dynamic visibility berdasarkan data state

### 5. Event Handling
- `@click` untuk user interactions
- Parameter passing dalam event handlers

### 6. Array Manipulation
- `map()` method untuk state updates
- Immutable updates patterns

### 7. Component Architecture
- Single-file components structure
- Props dan events untuk communication

## 🚀 Enhancement Ideas

Setelah menyelesaikan basic accordion, Anda bisa menambahkan fitur:

### 1. Multiple Items Open
```vue
<script setup>
const toggleAccordion = (index) => {
  // Toggle individual item without closing others
  items.value[index].open = !items.value[index].open;
};
</script>
```

### 2. Search Functionality
```vue
<script setup>
const searchTerm = ref('');

const filteredItems = computed(() => {
  return items.value.filter(item =>
    item.title.toLowerCase().includes(searchTerm.value.toLowerCase()) ||
    item.content.toLowerCase().includes(searchTerm.value.toLowerCase())
  );
});
</script>

<template>
  <div class="search-container">
    <input
      v-model="searchTerm"
      placeholder="Search accordions..."
      class="search-input"
    />
  </div>

  <div v-for="(item, index) in filteredItems" :key="index" class="accordion">
    <!-- ... accordion template -->
  </div>
</template>
```

### 3. Categories & Filtering
```vue
<script setup>
const categories = ref(['all', 'technology', 'design', 'business']);
const selectedCategory = ref('all');

const filteredItems = computed(() => {
  if (selectedCategory.value === 'all') {
    return items.value;
  }
  return items.value.filter(item => item.category === selectedCategory.value);
});
</script>
```

### 4. Smooth Height Animations
```vue
<script setup>
import { ref, nextTick } from "vue";

const contentHeight = ref({});

const toggleAccordion = async (index) => {
  const item = items.value[index];

  if (item.open) {
    // Close animation
    contentHeight.value[index] = item.contentHeight + 'px';
    await nextTick();
    contentHeight.value[index] = '0px';
  } else {
    // Open animation
    contentHeight.value[index] = '0px';
    await nextTick();
    contentHeight.value[index] = 'auto';
  }

  items.value[index].open = !items.value[index].open;
};
</script>

<template>
  <div
    class="accordion-content"
    :style="{ height: contentHeight[index] || '0px' }"
  >
    <div ref="contentRef">{{ item.content }}</div>
  </div>
</template>
```

### 5. Keyboard Navigation
```vue
<script setup>
import { onMounted } from "vue";

const handleKeyDown = (event, index) => {
  switch (event.key) {
    case 'Enter':
    case ' ':
      toggleAccordion(index);
      event.preventDefault();
      break;
    case 'ArrowDown':
      focusNextAccordion(index);
      event.preventDefault();
      break;
    case 'ArrowUp':
      focusPreviousAccordion(index);
      event.preventDefault();
      break;
  }
};
</script>

<template>
  <div
    class="accordion-header"
    @click="toggleAccordion(index)"
    @keydown="handleKeyDown($event, index)"
    tabindex="0"
  >
    {{ item.title }}
  </div>
</template>
```

### 6. API Integration
```vue
<script setup>
import { ref, onMounted } from "vue";

const items = ref([]);
const loading = ref(true);

const fetchAccordionData = async () => {
  try {
    const response = await fetch('https://api.example.com/faq');
    const data = await response.json();
    items.value = data.map(item => ({ ...item, open: false }));
  } catch (error) {
    console.error('Error fetching accordion data:', error);
  } finally {
    loading.value = false;
  }
};

onMounted(fetchAccordionData);
</script>
```

## 🔧 Common Issues & Troubleshooting

### Issue 1: Accordion tidak berfungsi
**Solution**:
- Pastikan `ref()` didefinisikan dengan benar
- Cek event handler `@click` terhubung dengan function
- Verify reactive data update

### Issue 2: Multiple accordions terbuka bersamaan
**Solution**:
- Pastikan logic `toggleAccordion` menutup item lain
- Gunakan `map()` untuk immutable array updates
- Check state management logic

### Issue 3: Animasi tidak smooth
**Solution**:
- Tambahkan CSS transitions
- Gunakan `v-show` untuk smooth height transitions
- Optimize reflow operations

### Issue 4: Styling berantakan di mobile
**Solution**:
- Tambahkan media queries
- Gunakan responsive units
- Test di berbagai device sizes

### Issue 5: Keyboard navigation tidak berfungsi
**Solution**:
- Tambahkan `tabindex` untuk focusable elements
- Implement keyboard event handlers
- Test keyboard accessibility

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Vue.js Conditional Rendering Guide](https://vuejs.org/guide/essentials/conditional.html)
- [Vue.js List Rendering Guide](https://vuejs.org/guide/essentials/list.html)
- [CSS Transitions Guide](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions)
- [Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Implementasi komponen AccordionComponent
- [ ] Tambahkan reactive data untuk items
- [ ] Implementasi toggle function
- [ ] Tambahkan list rendering dengan v-for
- [ ] Implementasi conditional rendering dengan v-show
- [ ] Tambahkan dynamic styling dan animations
- [ ] Styling aplikasi yang menarik
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Accordions yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:
- State management patterns
- User interaction handling
- Dynamic content rendering
- Component-based architecture
- Modern CSS animations
- Responsive design principles

---

**Happy Coding! 🚀**