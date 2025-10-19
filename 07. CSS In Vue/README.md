# 🎨 Tutorial Vue.js: CSS in Vue untuk Pemula

## 📖 Pengenalan CSS di Vue.js

Vue.js menyediakan berbagai cara untuk mengelola CSS dalam aplikasi Anda. Dari global styles hingga scoped styles dan CSS Modules, Vue memberikan fleksibilitas penuh untuk mengorganisir styling dengan cara yang clean dan maintainable.

## ❓ Pertanyaan: Apa Fungsi CSS di Vue.js?

CSS di Vue.js memiliki beberapa fungsi utama yang sangat penting:

### **1. 🎨 Styling dan Visual Design**
- **Fungsi:** Mengatur tampilan visual komponen (warna, ukuran, layout, dll)
- **Manfaat:** Membuat aplikasi menarik dan user-friendly
- **Contoh:** Button styling, card layout, theme colors

### **2. 📦 Scoped Styling**
- **Fungsi:** Membatasi CSS hanya berlaku untuk komponen tertentu
- **Manfaat:** Menghindari konflik CSS antar komponen
- **Contoh:** Class `.button` di komponen A tidak mempengaruhi komponen B

### **3. 🔄 Dynamic Styling**
- **Fungsi:** Mengubah style berdasarkan data atau kondisi
- **Manfaat:** Membuat UI yang interaktif dan responsif
- **Contoh:** Error state (border merah), success state (border hijau)

### **4. 🎯 Component-Based Styling**
- **Fungsi:** Mengelola CSS bersamaan dengan komponen Vue
- **Manfaat:** Lebih mudah maintain dan reuse styling
- **Contoh:** Setiap komponen memiliki style sendiri

### **5. 📱 Responsive Design**
- **Fungsi:** Membuat layout yang adaptif untuk berbagai ukuran layar
- **Manfaat:** User experience yang baik di desktop, tablet, dan mobile
- **Contoh:** Grid layout yang berubah sesuai screen size

### **6. 🌈 Theme Management**
- **Fungsi:** Mengelola theme (dark mode, light mode, custom themes)
- **Manfaat:** Konsistensi visual dan user preference
- **Contoh:** Toggle dark/light mode dengan CSS variables

## ❓ Pertanyaan: Gimana Cara Penggunaan CSS di Vue.js?

CSS di Vue.js sangat mudah digunakan dengan berbagai metode:

### **1. Global Styles (Tanpa Scoped)**
```vue
<script setup>
// Tidak perlu setup khusus
</script>

<template>
  <h1 class="title">Global Title</h1>
  <p class="text">Global Text</p>
</template>

<style>
/* Global styles - berlaku untuk seluruh aplikasi */
.title {
  color: #e74c3c;
  font-size: 2em;
}

.text {
  color: #2c3e50;
  line-height: 1.6;
}
</style>
```

### **2. Scoped Styles (Local Component)**
```vue
<script setup>
// Component logic here
</script>

<template>
  <h1 class="title">Scoped Title</h1>
  <p class="text">Scoped Text</p>
  <div class="global-in-scoped">Global Class in Scoped Component</div>
</template>

<style scoped>
/* Scoped styles - hanya berlaku untuk komponen ini */
.title {
  color: #3498db;
  font-size: 1.5em;
}

.text {
  color: #2ecc71;
  padding: 10px;
  background: #f8f9fa;
}

/* Gunakan :global() untuk class global dalam scoped style */
:global(.global-in-scoped) {
  color: #9b59b6;
  font-weight: bold;
}
</style>
```

### **3. CSS Modules**
```vue
<script setup>
// Import CSS modules
import { useCssModule } from 'vue'

// Menggunakan default module
const $style = useCssModule()

// Atau menggunakan named module
const myClasses = useCssModule('myClasses')
</script>

<template>
  <!-- Default module -->
  <h1 :class="$style.title">CSS Modules Title</h1>
  <p :class="$style.text">CSS Modules Text</p>

  <!-- Named module -->
  <div :class="myClasses.container">Named Module Container</div>
</template>

<style module>
/* Default CSS module */
.title {
  color: #e67e22;
  font-size: 1.8em;
}

.text {
  color: #d35400;
  background: #fef5e7;
  padding: 10px;
}
</style>

<style module="myClasses">
/* Named CSS module */
.container {
  background: linear-gradient(45deg, #667eea, #764ba2);
  color: white;
  padding: 20px;
  border-radius: 8px;
}
</style>
```

### **4. Multiple Style Blocks**
```vue
<script setup>
// Component logic
</script>

<template>
  <div>
    <h1 class="global-title">Global Styled Title</h1>
    <p class="scoped-text">Scoped Styled Text</p>
    <div :class="$style.moduleBox">CSS Module Box</div>
  </div>
</template>

<!-- Global Styles -->
<style>
.global-title {
  color: #e74c3c;
  text-align: center;
}
</style>

<!-- Scoped Styles -->
<style scoped>
.scoped-text {
  color: #3498db;
  font-style: italic;
}
</style>

<!-- CSS Modules -->
<style module>
.moduleBox {
  background: #2ecc71;
  color: white;
  padding: 15px;
  border-radius: 5px;
}
</style>
```

### **5. Dynamic CSS Classes**
```vue
<script setup>
import { ref, computed } from 'vue'

const isActive = ref(true)
const hasError = ref(false)
const theme = ref('dark')

const dynamicClasses = computed(() => ({
  'container': true,
  'active': isActive.value,
  'error': hasError.value,
  'theme-dark': theme.value === 'dark',
  'theme-light': theme.value === 'light'
}))
</script>

<template>
  <!-- Object syntax -->
  <div :class="dynamicClasses">
    Dynamic Classes Container
  </div>

  <!-- Array syntax -->
  <div :class="[
    'base-class',
    { 'active': isActive },
    { 'error': hasError }
  ]">
    Array Syntax Container
  </div>

  <!-- String interpolation -->
  <div :class="`theme-${theme} ${isActive ? 'active' : ''}`">
    String Interpolation Container
  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  border-radius: 8px;
}

.active {
  background-color: #d4edda;
  border: 2px solid #28a745;
}

.error {
  background-color: #f8d7da;
  border: 2px solid #dc3545;
}

.theme-dark {
  background-color: #2c3e50;
  color: white;
}

.theme-light {
  background-color: #f8f9fa;
  color: #2c3e50;
}
</style>
```

### **6. CSS Variables (Custom Properties)**
```vue
<script setup>
import { ref } from 'vue'

const primaryColor = ref('#42b883')
const textColor = ref('#2c3e50')
const borderRadius = ref('8px')

function changeTheme() {
  primaryColor.value = primaryColor.value === '#42b883' ? '#e74c3c' : '#42b883'
  textColor.value = textColor.value === '#2c3e50' ? '#ffffff' : '#2c3e50'
}
</script>

<template>
  <div
    class="theme-container"
    :style="{
      '--primary-color': primaryColor,
      '--text-color': textColor,
      '--border-radius': borderRadius
    }"
  >
    <h1>Theme with CSS Variables</h1>
    <button @click="changeTheme">Change Theme</button>
  </div>
</template>

<style scoped>
.theme-container {
  background-color: var(--primary-color);
  color: var(--text-color);
  border-radius: var(--border-radius);
  padding: 20px;
  transition: all 0.3s ease;
}

.theme-container h1 {
  margin: 0 0 15px 0;
}

.theme-container button {
  background: white;
  color: var(--primary-color);
  border: none;
  padding: 10px 20px;
  border-radius: var(--border-radius);
  cursor: pointer;
  font-weight: bold;
}
</style>
```

## ❓ Pertanyaan: Kapan CSS di Vue.js Ini Cocok Digunakan?

CSS di Vue.js cocok digunakan dalam situasi berikut:

### ✅ **Sangat Cocok Digunakan Untuk:**

1. **Component-Based Development**
   - Setiap komponen memiliki styling sendiri
   - Reusable components dengan consistent styling
   - Isolated styling untuk mencegah konflik

2. **Large Applications**
   - Banyak komponen dengan styling berbeda
   - Team development dengan modular CSS
   - Maintainable codebase dengan organized styles

3. **Dynamic UI/UX**
   - Theme switching (dark/light mode)
   - Interactive states (hover, active, disabled)
   - Responsive layouts dengan conditional styling

4. **Design Systems**
   - Consistent design patterns
   - Reusable style components
   - Brand guideline implementation

5. **Progressive Enhancement**
   - Base styles dengan global CSS
   - Component-specific styles dengan scoped CSS
   - Advanced styling dengan CSS Modules

### ❌ **Kurang Cocok Digunakan Untuk:**

1. **Simple Static Websites**
   ```html
   <!-- ❌ Terlalu kompleks untuk halaman statis sederhana -->
   <style scoped>
   .title { color: red; }
   </style>

   <!-- ✅ Lebih baik CSS biasa -->
   <link rel="stylesheet" href="styles.css">
   ```

2. **Rapid Prototyping**
   - Gunakan framework CSS seperti Tailwind, Bootstrap
   - Vue CSS lebih untuk production apps

3. **Non-Vue Projects**
   - Fitur khusus Vue tidak bekerja di plain HTML/CSS

## 🎯 Jenis-Jenis CSS di Vue.js

1. **Global Styles** - CSS yang berlaku untuk seluruh aplikasi
2. **Scoped Styles** - CSS yang hanya berlaku untuk komponen tertentu
3. **CSS Modules** - CSS dengan class names yang di-generate secara otomatis
4. **Combined Styles** - Menggabungkan berbagai jenis CSS

## 📁 Struktur Proyek

```
07. CSS In Vue/
├── README.md                           # Dokumentasi ini
└── src/
    ├── components/
    │   ├── GlobalStyles.vue            # Contoh global styles
    │   ├── LocalStyles.vue             # Contoh scoped styles
    │   ├── CombinedStyles.vue          # Contoh combined styles
    │   └── ModuleStyle.vue             # Contoh CSS Modules
    └── App.vue                         # Komponen utama
```

## 🔍 Analisis Kode Sumber

### 1. **GlobalStyles.vue** - Global Styles

```vue
<script setup></script>

<template>
  <h1 class="text-red">Global Styling</h1>
  <p class="local">Trying Local Scope In This component (not gonna work)</p>
</template>

<style>
.text-red {
  color: red;
}
</style>
```

**Penjelasan:**
- `<style>` tanpa `scoped` akan membuat CSS berlaku secara global
- Class `.text-red` akan mempengaruhi semua elemen dengan class tersebut di seluruh aplikasi
- Class `.local` tidak akan bekerja karena di-definisikan di komponen lain

### 2. **LocalStyles.vue** - Scoped Styles

```vue
<script></script>

<template>
  <div class="local">Second Component</div>
  <h1 class="still-global">Is it still global?</h1>
</template>

<style scoped>
.local {
  color: blueviolet;
}

/* The :global() pseudo selector allows us to use a certain class globally even in a scoped style. */
:global(.still-global) {
  color: aquamarine;
}
</style>
```

**Penjelasan:**
- `<style scoped>` membuat CSS hanya berlaku untuk komponen ini
- Class `.local` hanya akan mempengaruhi elemen dalam komponen ini
- `:global(.still-global)` membuat class tertentu tetap global meskipun dalam scoped style

### 3. **ModuleStyle.vue** - CSS Modules

```vue
<template>
  <h1 :class="myClasses.anotherClass">Using Customized Modules</h1>
</template>

<style module="myClasses">
.anotherClass {
  background: seagreen;
  color: white;
}
</style>
```

**Penjelasan:**
- `<style module>` mengaktifkan CSS Modules
- `module="myClasses"` memberikan nama khusus untuk module
- Class names akan di-generate secara unik untuk menghindari konflik

### 4. **CombinedStyles.vue** - Multiple Style Blocks

```vue
<template>
  <h2 class="g-1">Another Global Style</h2>
  <h2 class="l-1">Another Local Style</h2>
</template>

<!-- Global Styling -->
<style>
.g-1 {
  background: fuchsia;
  color: white;
}
</style>

<!-- Local Styling -->
<style scoped>
.l-1 {
  background: black;
  color: white;
}
</style>
```

**Penjelasan:**
- Vue memungkinkan multiple `<style>` blocks dalam satu komponen
- Bisa menggabungkan global dan scoped styles dalam satu file

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai teknik CSS di Vue:

### Enhanced Demo: CSS in Vue Showcase

```vue
<script setup>
import { ref, computed } from 'vue'

// Reactive state untuk demo
const currentTheme = ref('light')
const activeStyleType = ref('scoped')
const primaryColor = ref('#42b883')
const textSize = ref('16px')
const borderRadius = ref('8px')

// Computed properties untuk dynamic styling
const themeClasses = computed(() => ({
  'dark-theme': currentTheme.value === 'dark',
  'light-theme': currentTheme.value === 'light',
  'blue-theme': currentTheme.value === 'blue'
}))

const demoStyles = computed(() => ({
  '--primary-color': primaryColor.value,
  '--text-size': textSize.value,
  '--border-radius': borderRadius.value
}))

// Functions untuk interaksi
function toggleTheme() {
  const themes = ['light', 'dark', 'blue']
  const currentIndex = themes.indexOf(currentTheme.value)
  currentTheme.value = themes[(currentIndex + 1) % themes.length]
}

function changeStyleType(type) {
  activeStyleType.value = type
}

function updateColor(color) {
  primaryColor.value = color
}
</script>

<template>
  <div class="css-demo" :class="themeClasses" :style="demoStyles">
    <h1>🎨 Vue.js CSS Showcase</h1>

    <!-- Theme Controls -->
    <section class="control-section">
      <h2>🎨 Theme Controls</h2>
      <button @click="toggleTheme" class="theme-button">
        Current Theme: {{ currentTheme.toUpperCase() }}
      </button>

      <div class="color-controls">
        <label>Primary Color:</label>
        <input
          type="color"
          :value="primaryColor"
          @input="updateColor($event.target.value)"
        />
        <span>{{ primaryColor }}</span>
      </div>

      <div class="style-controls">
        <label>Text Size:</label>
        <input
          type="range"
          min="12"
          max="24"
          v-model="textSize"
        />
        <span>{{ textSize }}</span>
      </div>

      <div class="radius-controls">
        <label>Border Radius:</label>
        <input
          type="range"
          min="0"
          max="20"
          v-model="borderRadius"
        />
        <span>{{ borderRadius }}</span>
      </div>
    </section>

    <!-- Global Styles Demo -->
    <section class="demo-section">
      <h2>1️⃣ Global Styles</h2>
      <div class="demo-content">
        <p class="global-text">This text uses global styles</p>
        <button class="global-button">Global Button</button>
        <div class="global-card">Global Card</div>
      </div>
    </section>

    <!-- Scoped Styles Demo -->
    <section class="demo-section">
      <h2>2️⃣ Scoped Styles</h2>
      <div class="demo-content">
        <p class="scoped-text">This text uses scoped styles</p>
        <button class="scoped-button">Scoped Button</button>
        <div class="scoped-card">Scoped Card</div>
        <p class="global-in-scoped">
          This uses :global() selector in scoped styles
        </p>
      </div>
    </section>

    <!-- CSS Modules Demo -->
    <section class="demo-section">
      <h2>3️⃣ CSS Modules</h2>
      <div class="demo-content">
        <p :class="$style.moduleText">This text uses CSS Modules</p>
        <button :class="$style.moduleButton">CSS Modules Button</button>
        <div :class="$style.moduleCard">CSS Modules Card</div>
      </div>
    </section>

    <!-- Dynamic Class Binding -->
    <section class="demo-section">
      <h2>4️⃣ Dynamic Class Binding</h2>
      <div class="demo-content">
        <div :class="['dynamic-box', { active: true, disabled: false }]">
          Dynamic Box with Classes
        </div>

        <div :class="{
          'dynamic-box': true,
          'active': currentTheme === 'dark',
          'disabled': currentTheme === 'light'
        }">
          Conditional Dynamic Box
        </div>

        <div :class="`dynamic-box theme-${currentTheme}`">
          String Concatenation Dynamic Box
        </div>
      </div>
    </section>

    <!-- Inline Styles -->
    <section class="demo-section">
      <h2>5️⃣ Inline Styles</h2>
      <div class="demo-content">
        <div :style="{
          backgroundColor: primaryColor,
          color: 'white',
          padding: '20px',
          borderRadius: borderRadius,
          fontSize: textSize
        }">
          Dynamic Inline Style Box
        </div>

        <div :style="demoStyles">
          Computed Inline Style Box
        </div>
      </div>
    </section>

    <!-- CSS Variables -->
    <section class="demo-section">
      <h2>6️⃣ CSS Variables</h2>
      <div class="demo-content">
        <div class="css-variables-box">
          CSS Variables Box
        </div>

        <div class="variables-demo">
          <p>Primary Color: {{ primaryColor }}</p>
          <p>Text Size: {{ textSize }}</p>
          <p>Border Radius: {{ borderRadius }}</p>
        </div>
      </div>
    </section>

    <!-- Responsive Design -->
    <section class="demo-section">
      <h2>7️⃣ Responsive Design</h2>
      <div class="demo-content">
        <div class="responsive-grid">
          <div class="grid-item">Item 1</div>
          <div class="grid-item">Item 2</div>
          <div class="grid-item">Item 3</div>
          <div class="grid-item">Item 4</div>
        </div>

        <div class="responsive-text">
          Resize your browser to see responsive behavior!
        </div>
      </div>
    </section>

    <!-- Animations and Transitions -->
    <section class="demo-section">
      <h2>8️⃣ Animations and Transitions</h2>
      <div class="demo-content">
        <div class="animated-box">
          Animated Box (Hover me)
        </div>

        <div class="transition-box">
          Transition Box (Click me)
        </div>

        <div class="keyframe-animation">
          Keyframe Animation
        </div>
      </div>
    </section>

    <!-- Pseudo-classes and Pseudo-elements -->
    <section class="demo-section">
      <h2>9️⃣ Pseudo-classes and Pseudo-elements</h2>
      <div class="demo-content">
        <button class="pseudo-button">Hover and Click Me</button>

        <div class="pseudo-card">
          Card with ::before and ::after
        </div>

        <p class="pseudo-text">
          Text with ::first-line and ::first-letter styling
        </p>
      </div>
    </section>

    <!-- CSS Grid and Flexbox -->
    <section class="demo-section">
      <h2>🔟 CSS Grid and Flexbox</h2>
      <div class="demo-content">
        <div class="flexbox-container">
          <div class="flex-item">Flex 1</div>
          <div class="flex-item">Flex 2</div>
          <div class="flex-item">Flex 3</div>
        </div>

        <div class="grid-container">
          <div class="grid-item">Grid 1</div>
          <div class="grid-item">Grid 2</div>
          <div class="grid-item">Grid 3</div>
          <div class="grid-item">Grid 4</div>
          <div class="grid-item">Grid 5</div>
          <div class="grid-item">Grid 6</div>
        </div>
      </div>
    </section>
  </div>
</template>

<!-- Global Styles -->
<style>
/* Base styles untuk seluruh aplikasi */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  line-height: 1.6;
  transition: all 0.3s ease;
}

/* Global utility classes */
.global-text {
  color: #e74c3c;
  font-weight: bold;
  font-size: 18px;
}

.global-button {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.global-button:hover {
  background-color: #c0392b;
  transform: translateY(-2px);
}

.global-card {
  background-color: #ecf0f1;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #e74c3c;
  margin-top: 10px;
}

/* Global-in-scoped classes */
.global-in-scoped {
  color: #9b59b6;
  font-style: italic;
  text-decoration: underline;
}
</style>

<!-- Scoped Styles -->
<style scoped>
.css-demo {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  min-height: 100vh;
}

h1 {
  text-align: center;
  color: var(--primary-color, #42b883);
  margin-bottom: 30px;
  font-size: 2.5em;
}

.control-section {
  background: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 30px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.control-section h2 {
  margin-bottom: 15px;
  color: #2c3e50;
}

.theme-button {
  background: var(--primary-color, #42b883);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s ease;
  margin-bottom: 15px;
}

.theme-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(66, 184, 131, 0.3);
}

.color-controls,
.style-controls,
.radius-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 10px;
}

.color-controls input[type="color"],
.style-controls input[type="range"],
.radius-controls input[type="range"] {
  cursor: pointer;
}

.demo-section {
  background: white;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.demo-content {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Scoped specific styles */
.scoped-text {
  color: #3498db;
  font-weight: bold;
  font-size: 18px;
}

.scoped-button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.scoped-button:hover {
  background-color: #2980b9;
  transform: translateY(-2px);
}

.scoped-card {
  background-color: #e8f4fd;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #3498db;
  margin-top: 10px;
}

.dynamic-box {
  padding: 20px;
  border-radius: var(--border-radius, 8px);
  background-color: #f8f9fa;
  border: 2px solid #dee2e6;
  text-align: center;
  transition: all 0.3s ease;
}

.dynamic-box.active {
  background-color: #d4edda;
  border-color: #28a745;
  color: #155724;
}

.dynamic-box.disabled {
  background-color: #f8d7da;
  border-color: #dc3545;
  color: #721c24;
}

.css-variables-box {
  padding: 20px;
  background-color: var(--primary-color, #42b883);
  color: white;
  border-radius: var(--border-radius, 8px);
  font-size: var(--text-size, 16px);
  text-align: center;
  transition: all 0.3s ease;
}

.variables-demo {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.variables-demo p {
  margin: 5px 0;
  font-family: monospace;
}

.responsive-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.responsive-text {
  font-size: 16px;
  text-align: center;
  padding: 20px;
  background-color: #e3f2fd;
  border-radius: 8px;
}

.animated-box {
  padding: 20px;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  color: white;
  border-radius: 8px;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
}

.animated-box:hover {
  transform: scale(1.05) rotate(2deg);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.2);
}

.transition-box {
  padding: 20px;
  background-color: #ff9ff3;
  color: white;
  border-radius: 8px;
  text-align: center;
  cursor: pointer;
  transition: all 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}

.transition-box:active {
  transform: scale(0.95);
  background-color: #ee5a6f;
}

.keyframe-animation {
  padding: 20px;
  background-color: #54a0ff;
  color: white;
  border-radius: 8px;
  text-align: center;
  animation: pulse 2s infinite;
}

.pseudo-button {
  background-color: #00d2d3;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  position: relative;
  overflow: hidden;
  transition: all 0.3s ease;
}

.pseudo-button:hover {
  background-color: #00a8a9;
  transform: translateY(-2px);
}

.pseudo-button:active {
  transform: translateY(0);
}

.pseudo-button::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  width: 0;
  height: 0;
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  transform: translate(-50%, -50%);
  transition: width 0.6s, height 0.6s;
}

.pseudo-button:hover::before {
  width: 300px;
  height: 300px;
}

.pseudo-card {
  background-color: #feca57;
  color: #2c3e50;
  padding: 20px;
  border-radius: 8px;
  position: relative;
  overflow: hidden;
}

.pseudo-card::before {
  content: '⭐';
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 24px;
}

.pseudo-card::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 4px;
  background: linear-gradient(90deg, #ff6b6b, #4ecdc4, #45b7d1);
}

.pseudo-text {
  font-size: 18px;
  line-height: 1.8;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.pseudo-text::first-line {
  color: #e74c3c;
  font-weight: bold;
}

.pseudo-text::first-letter {
  font-size: 3em;
  float: left;
  line-height: 1;
  margin-right: 8px;
  color: #3498db;
  font-weight: bold;
}

.flexbox-container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 15px;
  margin-bottom: 20px;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.flex-item {
  flex: 1;
  padding: 15px;
  background-color: #3498db;
  color: white;
  text-align: center;
  border-radius: 5px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.grid-item {
  padding: 15px;
  background-color: #2ecc71;
  color: white;
  text-align: center;
  border-radius: 5px;
}

/* Theme styles */
.dark-theme {
  background-color: #1a1a1a;
  color: #ecf0f1;
}

.dark-theme .control-section,
.dark-theme .demo-section {
  background-color: #2c2c2c;
  color: #ecf0f1;
}

.dark-theme .demo-section h2,
.dark-theme .control-section h2 {
  color: #ecf0f1;
}

.light-theme {
  background-color: #f8f9fa;
  color: #2c3e50;
}

.blue-theme {
  background-color: #e3f2fd;
  color: #1565c0;
}

.blue-theme .control-section,
.blue-theme .demo-section {
  background-color: #bbdefb;
  color: #1565c0;
}

/* Animations */
@keyframes pulse {
  0% {
    transform: scale(1);
    box-shadow: 0 0 0 0 rgba(84, 160, 255, 0.7);
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 0 0 10px rgba(84, 160, 255, 0);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 0 0 rgba(84, 160, 255, 0);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .css-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .control-section,
  .demo-section {
    padding: 15px;
  }

  .flexbox-container {
    flex-direction: column;
  }

  .grid-container {
    grid-template-columns: repeat(2, 1fr);
  }

  .responsive-grid {
    grid-template-columns: 1fr;
  }
}

@media (max-width: 480px) {
  h1 {
    font-size: 1.5em;
  }

  .grid-container {
    grid-template-columns: 1fr;
  }

  .color-controls,
  .style-controls,
  .radius-controls {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>

<!-- CSS Modules -->
<style module>
.moduleText {
  color: #e67e22;
  font-weight: bold;
  font-size: 18px;
  padding: 10px;
  background-color: #fef5e7;
  border-radius: 5px;
  border: 2px solid #e67e22;
}

.moduleButton {
  background-color: #e67e22;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.moduleButton:hover {
  background-color: #d35400;
  transform: translateY(-2px);
}

.moduleCard {
  background-color: #fef5e7;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #e67e22;
  margin-top: 10px;
  transition: all 0.3s ease;
}

.moduleCard:hover {
  box-shadow: 0 4px 12px rgba(230, 126, 34, 0.2);
}
</style>