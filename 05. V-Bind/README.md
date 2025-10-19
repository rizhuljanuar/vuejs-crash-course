# 🔗 Tutorial Vue.js: V-Bind Directive untuk Pemula

## 🎯 Pengenalan V-Bind Directive

**V-Bind** adalah directive fundamental dalam Vue.js yang digunakan untuk mengikat (bind) data dari JavaScript ke atribut HTML. Ini memungkinkan Anda membuat atribut HTML yang dinamis dan reaktif berdasarkan data di komponen Vue Anda.

## 🤔 Apa itu V-Bind?

**V-Bind** adalah directive Vue.js yang memungkinkan Anda mengikat nilai atribut HTML ke ekspresi JavaScript.

**Sintaks Dasar:**
```html
<!-- Full syntax -->
<element v-bind:attribute="expression"></element>

<!-- Shorthand syntax (lebih umum digunakan) -->
<element :attribute="expression"></element>
```

## ❓ Pertanyaan: Gimana Cara Penggunaan V-Bind?

V-Bind sangat mudah digunakan! Berikut adalah cara-cara penggunaan V-Bind yang paling umum:

### 1. **Mengikat Link Dinamis**
```vue
<script setup>
const websiteUrl = 'https://vuejs.org'
const youtubeChannel = 'https://youtube.com/@huxnwebdev'
</script>

<template>
  <!-- Link ke website -->
  <a :href="websiteUrl">Vue.js Documentation</a>

  <!-- Link ke YouTube -->
  <a :href="youtubeChannel" target="_blank">HuXn WebDev Channel</a>
</template>
```

### 2. **Mengikat Gambar Dinamis**
```vue
<script setup>
const imageUrl = 'https://picsum.photos/seed/demo/300/200.jpg'
const imageAlt = 'Gambar demo'
const imageWidth = 300
const imageHeight = 200
</script>

<template>
  <img
    :src="imageUrl"
    :alt="imageAlt"
    :width="imageWidth"
    :height="imageHeight"
  />
</template>
```

### 3. **Mengikat CSS Classes Dinamis**
```vue
<script setup>
const isActive = ref(true)
const hasError = ref(false)
</script>

<template>
  <div :class="{
    'active': isActive,
    'error': hasError
  }">
    Content dengan class dinamis
  </div>
</template>
```

### 4. **Mengikat CSS Styles Dinamis**
```vue
<script setup>
const textColor = ref('red')
const fontSize = ref('16px')
</script>

<template>
  <p :style="{
    color: textColor,
    fontSize: fontSize
  }">
    Text dengan style dinamis
  </p>
</template>
```

## ❓ Pertanyaan: Kapan V-Bind Ini Cocok Digunakan?

V-Bind cocok digunakan dalam situasi berikut:

### ✅ **Sangat Cocok Digunakan Untuk:**

1. **Data yang Berubah Secara Dinamis**
   - Informasi user yang berubah (nama, avatar, status)
   - Data produk yang berubah (harga, stok, gambar)
   - Konfigurasi tema (warna, ukuran, layout)

2. **Rendering List Data**
   - Gallery gambar dengan URL berbeda
   - Daftar produk dengan atribut berbeda
   - Tabel dengan data dinamis

3. **Interaksi User**
   - Form validation (menampilkan error message)
   - Toggle states (active/inactive buttons)
   - Theme switching (dark/light mode)

4. **Conditional Attributes**
   - Menampilkan/hiding elements
   - Mengubah status berdasarkan kondisi
   - Mengatur accessibility attributes

### ❌ **Kurang Cocok Digunakan Untuk:**

1. **Static Values** - Gunakan HTML biasa untuk nilai yang tidak berubah
   ```html
   <!-- ❌ Tidak perlu v-bind -->
   <a :href="https://google.com">Google</a>

   <!-- ✅ Lebih baik HTML biasa -->
   <a href="https://google.com">Google</a>
   ```

2. **Simple Text Content** - Gunakan {{ }} untuk text, bukan v-bind
   ```html
   <!-- ❌ Salah -->
   <p :title="'Hello World'">Text</p>

   <!-- ✅ Benar untuk text content -->
   <p>Hello World</p>
   ```

## 📚 Contoh Praktis Berdasarkan Kode Sumber

Berdasarkan kode di `HelloWorld.vue`, ini adalah contoh nyata penggunaan V-Bind:

```vue
<script setup>
const myChannel = 'https://www.youtube.com/@huxnwebdev'
const amazingImage = 'https://images.unsplash.com/photo-1699646034253-0ebaa551d226?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
const altText = 'Just a random picture'
const imageWidth = 400
const imageHeight = 400
</script>

<template>
  <div>
    <!-- Link YouTube dengan v-bind -->
    <a :href="myChannel">HuXn WebDev Channel</a>

    <!-- Gambar dengan multiple v-bind -->
    <img
      :src="amazingImage"
      :alt="altText"
      :width="imageWidth"
      :height="imageHeight"
    />
  </div>
</template>
```

**Penjelasan:**
- `:href="myChannel"` - Mengikat URL link ke variabel JavaScript
- `:src="amazingImage"` - Mengikat source gambar ke variabel
- `:alt="altText"` - Mengikat alt text untuk accessibility
- `:width="imageWidth"` dan `:height="imageHeight"` - Mengikat ukuran gambar

## 🎯 Tips untuk Pemula

### 1. **Gunakan Shorthand Syntax**
```vue
<!-- ✅ Lebih singkat -->
<img :src="image" :alt="alt">

<!-- ❌ Lebih panjang -->
<img v-bind:src="image" v-bind:alt="alt">
```

### 2. **Gabungkan dengan Reactive Data**
```vue
<script setup>
const user = ref({
  name: 'John',
  avatar: 'avatar.jpg',
  isActive: true
})
</script>

<template>
  <div :class="{ active: user.isActive }">
    <img :src="user.avatar" :alt="user.name + ' avatar'">
  </div>
</template>
```

### 3. **Boolean Attributes**
```vue
<script setup>
const isDisabled = ref(false)
const isChecked = ref(true)
</script>

<template>
  <input :disabled="isDisabled" />
  <input type="checkbox" :checked="isChecked" />
</template>
```

## 📁 Struktur Proyek

```
05. V-Bind/
├── README.md                    # Dokumentasi ini
└── src/
    ├── components/
    │   └── HelloWorld.vue       # Komponen dengan contoh v-bind
    └── App.vue                  # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `HelloWorld.vue`

```vue
<script setup>
const myChannel = 'https://www.youtube.com/@huxnwebdev'
const amazingImage =
  'https://images.unsplash.com/photo-1699646034253-0ebaa551d226?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
const altText = 'Just a random picture'

const imageWidth = 400
const imageHeight = 400
</script>

<template>
  <div>
    <!-- Old way 👇 -->
    <!-- <a v-bind:href="myChannel">HuXn WebDev Channel</a> -->

    <!-- New way 👇 -->
    <a :href="myChannel">HuXn WebDev Channel</a>
    <img :src="amazingImage" :alt="altText" :width="imageWidth" :height="imageHeight" />
  </div>
</template>
```

## 📖 Penjelasan Mendalam

### 1. **Sintaks V-Bind**

#### **Full Syntax: `v-bind:`**
```vue
<!-- Full syntax - lebih verbose -->
<a v-bind:href="url">Link</a>
<img v-bind:src="imageSrc" v-bind:alt="altText">
```

#### **Shorthand Syntax: `:`**
```vue
<!-- Shorthand syntax - lebih ringkas -->
<a :href="url">Link</a>
<img :src="imageSrc" :alt="altText">
```

**💡 Tips:** Gunakan shorthand syntax (`:`) karena lebih umum dan lebih mudah dibaca.

### 2. **Dynamic Attributes**

#### **Link dinamis**
```vue
<script setup>
const websiteUrl = 'https://vuejs.org'
const linkText = 'Vue.js Documentation'
const isOpenInNewTab = true
</script>

<template>
  <!-- Basic link -->
  <a :href="websiteUrl">{{ linkText }}</a>

  <!-- Link dengan target dinamis -->
  <a :href="websiteUrl" :target="isOpenInNewTab ? '_blank' : '_self'">
    {{ linkText }}
  </a>
</template>
```

#### **Image dinamis**
```vue
<script setup>
const imageInfo = {
  src: 'https://example.com/image.jpg',
  alt: 'Contoh gambar',
  width: 300,
  height: 200
}
</script>

<template>
  <img
    :src="imageInfo.src"
    :alt="imageInfo.alt"
    :width="imageInfo.width"
    :height="imageInfo.height"
  />
</template>
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan v-bind:

### Enhanced Demo: V-Bind Showcase

```vue
<script setup>
import { ref, computed } from 'vue'

// Reactive data
const isDarkMode = ref(false)
const isActive = ref(true)
const buttonColor = ref('#42b883')
const textSize = ref('16px')
const borderRadius = ref('8px')
const isVisible = ref(true)
const disabled = ref(false)
const selectedIndex = ref(0)

const tabs = ref(['Home', 'About', 'Services', 'Contact'])
const products = ref([
  { name: 'Laptop', price: 10000000, inStock: true },
  { name: 'Mouse', price: 250000, inStock: false },
  { name: 'Keyboard', price: 750000, inStock: true }
])

// Computed properties
const themeClasses = computed(() => ({
  'dark-theme': isDarkMode.value,
  'light-theme': !isDarkMode.value
}))

const buttonStyle = computed(() => ({
  backgroundColor: buttonColor.value,
  fontSize: textSize.value,
  borderRadius: borderRadius.value,
  padding: '12px 24px',
  color: 'white',
  border: 'none',
  cursor: disabled.value ? 'not-allowed' : 'pointer',
  opacity: disabled.value ? 0.6 : 1
}))

const selectedProduct = computed(() => products.value[selectedIndex.value])

// Functions
function toggleTheme() {
  isDarkMode.value = !isDarkMode.value
}

function selectTab(index) {
  selectedIndex.value = index
}

function toggleButton() {
  isActive.value = !isActive.value
}

function changeColor() {
  const colors = ['#e74c3c', '#3498db', '#2ecc71', '#f39c12', '#9b59b6']
  const currentIndex = colors.indexOf(buttonColor.value)
  buttonColor.value = colors[(currentIndex + 1) % colors.length]
}
</script>

<template>
  <div class="v-bind-demo" :class="themeClasses">
    <h1>🔗 Vue.js V-Bind Directive Demo</h1>

    <!-- 1. Basic Attribute Binding -->
    <section class="demo-section">
      <h2>1️⃣ Basic Attribute Binding</h2>
      <div class="demo-content">
        <!-- Dynamic link -->
        <a
          :href="isDarkMode ? 'https://darkmode.com' : 'https://lightmode.com'"
          :target="isDarkMode ? '_blank' : '_self'"
          class="demo-link"
        >
          {{ isDarkMode ? '🌙 Dark Mode Site' : '☀️ Light Mode Site' }}
        </a>

        <!-- Dynamic image -->
        <img
          :src="isDarkMode
            ? 'https://picsum.photos/seed/dark/300/200.jpg'
            : 'https://picsum.photos/seed/light/300/200.jpg'"
          :alt="`Image for ${isDarkMode ? 'dark' : 'light'} mode`"
          :width="300"
          :height="200"
          class="demo-image"
        />
      </div>
    </section>

    <!-- 2. Class Binding -->
    <section class="demo-section">
      <h2>2️⃣ Class Binding</h2>
      <div class="demo-content">
        <button
          :class="{
            'active-button': isActive,
            'inactive-button': !isActive,
            'disabled': disabled
          }"
          :disabled="disabled"
          @click="toggleButton"
        >
          {{ isActive ? '✅ Active' : '❌ Inactive' }} Button
        </button>

        <button @click="disabled = !disabled" class="control-button">
          Toggle Disabled
        </button>
      </div>
    </section>

    <!-- 3. Style Binding -->
    <section class="demo-section">
      <h2>3️⃣ Style Binding</h2>
      <div class="demo-content">
        <button :style="buttonStyle" @click="changeColor">
          Dynamic Style Button
        </button>

        <div class="style-controls">
          <label>
            Font Size:
            <input
              type="range"
              min="12"
              max="24"
              v-model="textSize"
              @input="$event.target.style.setProperty('--font-size', textSize)"
            />
            {{ textSize }}
          </label>

          <label>
            Border Radius:
            <input
              type="range"
              min="0"
              max="20"
              v-model="borderRadius"
            />
            {{ borderRadius }}
          </label>
        </div>
      </div>
    </section>

    <!-- 4. Boolean Attribute Binding -->
    <section class="demo-section">
      <h2>4️⃣ Boolean Attribute Binding</h2>
      <div class="demo-content">
        <input
          type="checkbox"
          :checked="isVisible"
          @change="isVisible = $event.target.checked"
        />
        <label>Show content below</label>

        <div v-show="isVisible" class="conditional-content">
          <p>✨ Content ini muncul/hilang berdasarkan checkbox!</p>
          <input
            type="text"
            :disabled="!isVisible"
            placeholder="Input ini aktif jika content visible"
          />
        </div>
      </div>
    </section>

    <!-- 5. Dynamic Attribute Names -->
    <section class="demo-section">
      <h2>5️⃣ Dynamic Attribute Names</h2>
      <div class="demo-content">
        <button
          v-for="(tab, index) in tabs"
          :key="index"
          :class="['tab-button', { active: selectedIndex === index }]"
          :title="`Click to view ${tab}`"
          :aria-label="`${tab} tab, ${selectedIndex === index ? 'selected' : 'not selected'}`"
          @click="selectTab(index)"
        >
          {{ tab }}
        </button>

        <div class="tab-content">
          <h3>Selected: {{ tabs[selectedIndex] }}</h3>
          <p>Dynamic attributes like title, aria-label, and classes are bound here!</p>
        </div>
      </div>
    </section>

    <!-- 6. Object Property Binding -->
    <section class="demo-section">
      <h2>6️⃣ Object Property Binding</h2>
      <div class="demo-content">
        <div
          v-for="(product, index) in products"
          :key="index"
          :class="{
            'product-card': true,
            'out-of-stock': !product.inStock
          }"
          :data-product-id="index"
          :data-price="product.price"
        >
          <h4 :title="`Price: ${product.price}`">{{ product.name }}</h4>
          <p :style="{
            color: product.inStock ? '#2ecc71' : '#e74c3c',
            fontWeight: 'bold'
          }">
            {{ product.inStock ? '✅ In Stock' : '❌ Out of Stock' }}
          </p>
          <p :data-currency="product.price > 1000000 ? 'high' : 'low'">
            {{ new Intl.NumberFormat('id-ID', {
              style: 'currency',
              currency: 'IDR'
            }).format(product.price) }}
          </p>
        </div>
      </div>
    </section>

    <!-- 7. Form Element Binding -->
    <section class="demo-section">
      <h2>7️⃣ Form Element Binding</h2>
      <div class="demo-content">
        <div class="form-group">
          <label :for="'username-' + Date.now()">Username:</label>
          <input
            :id="'username-' + Date.now()"
            type="text"
            :placeholder="isDarkMode ? 'Enter username (dark mode)' : 'Enter username'"
            :maxlength="20"
            :required="true"
          />
        </div>

        <div class="form-group">
          <label for="user-email">Email:</label>
          <input
            id="user-email"
            type="email"
            placeholder="Enter your email"
            :pattern="[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}$"
            title="Please enter a valid email address"
          />
        </div>

        <div class="form-group">
          <label for="priority">Priority:</label>
          <select
            id="priority"
            :size="3"
            :multiple="false"
          >
            <option value="low" :selected="true">Low</option>
            <option value="medium">Medium</option>
            <option value="high">High</option>
          </select>
        </div>
      </div>
    </section>

    <!-- 8. Theme Toggle -->
    <section class="demo-section">
      <h2>8️⃣ Dynamic Theme Switcher</h2>
      <div class="demo-content">
        <button @click="toggleTheme" class="theme-toggle">
          {{ isDarkMode ? '☀️ Switch to Light' : '🌙 Switch to Dark' }}
        </button>
        <p>Current theme: <strong>{{ isDarkMode ? 'Dark Mode' : 'Light Mode' }}</strong></p>
      </div>
    </section>
  </div>
</template>

<style scoped>
.v-bind-demo {
  max-width: 900px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  transition: all 0.3s ease;
}

/* Light Theme */
.light-theme {
  background-color: #f8f9fa;
  color: #2c3e50;
}

/* Dark Theme */
.dark-theme {
  background-color: #1a1a1a;
  color: #ecf0f1;
}

h1 {
  text-align: center;
  color: #42b883;
  margin-bottom: 30px;
  font-size: 2.5em;
}

.demo-section {
  background: #ffffff;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
  border-left: 4px solid #42b883;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.dark-theme .demo-section {
  background: #2c2c2c;
  box-shadow: 0 2px 10px rgba(255, 255, 255, 0.1);
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.dark-theme .demo-section h2 {
  color: #ecf0f1;
}

.demo-content {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.demo-link {
  color: #42b883;
  text-decoration: none;
  font-weight: bold;
  padding: 10px;
  border: 2px solid #42b883;
  border-radius: 5px;
  transition: all 0.3s;
  display: inline-block;
  width: fit-content;
}

.demo-link:hover {
  background-color: #42b883;
  color: white;
}

.demo-image {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;
}

.demo-image:hover {
  transform: scale(1.05);
}

.active-button {
  background-color: #2ecc71;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.inactive-button {
  background-color: #e74c3c;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.control-button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
}

.style-controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 15px;
}

.style-controls label {
  display: flex;
  align-items: center;
  gap: 10px;
}

.style-controls input[type="range"] {
  flex: 1;
}

.conditional-content {
  background-color: #e8f5e8;
  padding: 15px;
  border-radius: 5px;
  margin-top: 10px;
}

.dark-theme .conditional-content {
  background-color: #1e3a1e;
}

.tab-button {
  background-color: #ecf0f1;
  border: 1px solid #bdc3c7;
  padding: 8px 16px;
  margin-right: 5px;
  cursor: pointer;
  border-radius: 5px 5px 0 0;
  transition: all 0.3s;
}

.tab-button.active {
  background-color: #42b883;
  color: white;
  border-color: #42b883;
}

.tab-content {
  background-color: #f8f9fa;
  padding: 20px;
  border: 1px solid #bdc3c7;
  border-radius: 0 5px 5px 5px;
}

.dark-theme .tab-content {
  background-color: #2c2c2c;
  border-color: #555;
}

.product-card {
  background-color: #ffffff;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #ddd;
  margin-bottom: 10px;
  transition: all 0.3s;
}

.product-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.out-of-stock {
  opacity: 0.7;
  background-color: #fdf2f2;
}

.dark-theme .product-card {
  background-color: #2c2c2c;
  border-color: #555;
}

.dark-theme .out-of-stock {
  background-color: #3a1a1a;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-bottom: 15px;
}

.form-group label {
  font-weight: bold;
}

.form-group input,
.form-group select {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.dark-theme .form-group input,
.dark-theme .form-group select {
  background-color: #3a3a3a;
  border-color: #555;
  color: #ecf0f1;
}

.theme-toggle {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 25px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
}

.theme-toggle:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(102, 126, 234, 0.6);
}
</style>
```

## 📚 Jenis-Jenis V-Bind

### 1. **Attribute Binding**
```vue
<!-- Single attribute -->
<img :src="imageSrc" :alt="imageAlt">

<!-- Multiple attributes -->
<a :href="url" :target="target" :title="title">Link</a>
```

### 2. **Class Binding**
```vue
<script setup>
const isActive = ref(true)
const error = ref(null)
const classObject = ref({
  active: true,
  'text-danger': false
})
</script>

<template>
  <!-- Object syntax -->
  <div :class="{ active: isActive, 'text-danger': error }"></div>

  <!-- Array syntax -->
  <div :class="[isActive ? 'active' : '', error ? 'text-danger' : '']"></div>

  <!-- Object from data -->
  <div :class="classObject"></div>

  <!-- Mixed syntax -->
  <div :class="[{ active: isActive }, errorClass]"></div>
</template>
```

### 3. **Style Binding**
```vue
<script setup>
const activeColor = ref('red')
const fontSize = ref(30)
const styleObject = ref({
  color: 'red',
  fontSize: '13px'
})
</script>

<template>
  <!-- Object syntax -->
  <div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

  <!-- Object from data -->
  <div :style="styleObject"></div>

  <!-- Array syntax (multiple style objects) -->
  <div :style="[baseStyles, overridingStyles]"></div>

  <!-- Auto-prefixing -->
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
</template>
```

### 4. **Boolean Attribute Binding**
```vue
<script setup>
const isDisabled = ref(false)
const isChecked = ref(true)
const isVisible = ref(true)
</script>

<template>
  <!-- Boolean attributes -->
  <input :disabled="isDisabled" />
  <input :checked="isChecked" type="checkbox" />
  <option :selected="isSelected">Option</option>

  <!-- Attributes that are removed when falsy -->
  <div :hidden="!isVisible">Content</div>
  <button :readonly="isReadOnly">Button</button>
</template>
```

### 5. **Dynamic Attribute Names**
```vue
<script setup>
const attributeName = ref('href')
const attributeValue = ref('https://vuejs.org')
</script>

<template>
  <!-- Dynamic attribute name -->
  <a :[attributeName]="attributeValue">Dynamic Link</a>

  <!-- Dynamic event name -->
  <button @[eventName]="doSomething">Dynamic Event</button>
</template>
```

## 🎯 Best Practices

### 1. **Use Shorthand Syntax**
```vue
<!-- ✅ Good - Shorthand -->
<img :src="imageSrc" :alt="imageAlt">

<!-- ❌ Avoid - Full syntax -->
<img v-bind:src="imageSrc" v-bind:alt="imageAlt">
```

### 2. **Prefer Computed Properties for Complex Logic**
```vue
<script setup>
import { computed } from 'vue'

const isActive = ref(true)
const hasError = ref(false)
const isWarning = ref(true)

// ✅ Computed property untuk complex logic
const buttonClasses = computed(() => ({
  'btn': true,
  'btn-active': isActive.value,
  'btn-error': hasError.value,
  'btn-warning': isWarning.value && !hasError.value
}))
</script>

<template>
  <!-- ✅ Clean template -->
  <button :class="buttonClasses">Click me</button>

  <!-- ❌ Complex logic in template -->
  <button :class="{
    'btn': true,
    'btn-active': isActive,
    'btn-error': hasError,
    'btn-warning': isWarning && !hasError
  }">Click me</button>
</template>
```

### 3. **Use Objects for Multiple Classes**
```vue
<script setup>
const classes = ref({
  'header': true,
  'header-fixed': isFixed.value,
  'header-transparent': isTransparent.value
})
</script>

<template>
  <!-- ✅ Object syntax -->
  <div :class="classes">Header</div>

  <!-- ❌ Multiple individual bindings -->
  <div
    :class="{ 'header': true }"
    :class="{ 'header-fixed': isFixed }"
    :class="{ 'header-transparent': isTransparent }"
  >Header</div>
</template>
```

### 4. **Be Careful with Style Binding**
```vue
<script setup>
// ✅ Use computed properties for complex styles
const buttonStyle = computed(() => ({
  backgroundColor: isActive.value ? '#42b883' : '#e74c3c',
  transform: isPressed.value ? 'scale(0.95)' : 'scale(1)',
  transition: 'all 0.2s ease'
}))
</script>

<template>
  <!-- ✅ Computed style -->
  <button :style="buttonStyle">Click me</button>

  <!-- ❌ Too much inline logic -->
  <button :style="{
    backgroundColor: isActive ? '#42b883' : '#e74c3c',
    transform: isPressed ? 'scale(0.95)' : 'scale(1)',
    transition: 'all 0.2s ease'
  }">Click me</button>
</template>
```

## 🛠️ Cara Menjalankan Proyek

```bash
# Masuk ke folder proyek
cd "05. V-Bind"

# Install dependencies
npm install

# Jalankan development server
npm run dev
```

## 🎪 Latihan Praktis

### Latihan 1: Dynamic Profile Card
Buat profile card dengan binding dinamis:

```vue
<script setup>
import { ref, computed } from 'vue'

const user = ref({
  name: 'John Doe',
  avatar: 'https://picsum.photos/seed/user1/150/150.jpg',
  role: 'Developer',
  isActive: true,
  joinDate: '2024-01-15'
})

const isDarkMode = ref(false)

const profileStyle = computed(() => ({
  backgroundColor: isDarkMode.value ? '#2c3e50' : '#ffffff',
  color: isDarkMode.value ? '#ecf0f1' : '#2c3e50',
  border: `2px solid ${user.value.isActive ? '#42b883' : '#e74c3c'}`
}))
</script>

<template>
  <div :class="['profile-card', { dark: isDarkMode }]" :style="profileStyle">
    <img :src="user.avatar" :alt="`${user.name} avatar`" />
    <h2>{{ user.name }}</h2>
    <p :class="{ 'status-active': user.isActive }">
      {{ user.isActive ? '🟢 Active' : '🔴 Inactive' }}
    </p>
    <button @click="isDarkMode = !isDarkMode">
      Toggle Theme
    </button>
  </div>
</template>
```

### Latihan 2: Interactive Form Validation
Buat form dengan binding dinamis untuk validasi:

```vue
<script setup>
import { ref, computed } from 'vue'

const formData = ref({
  name: '',
  email: '',
  password: ''
})

const errors = ref({
  name: false,
  email: false,
  password: false
})

const isFormValid = computed(() => {
  return formData.value.name.length >= 3 &&
         formData.value.email.includes('@') &&
         formData.value.password.length >= 6
})

const nameInputStyle = computed(() => ({
  borderColor: errors.value.name ? '#e74c3c' : formData.value.name ? '#42b883' : '#ddd'
}))
</script>

<template>
  <form :class="{ 'form-invalid': !isFormValid }">
    <input
      v-model="formData.name"
      :style="nameInputStyle"
      :placeholder="errors.value.name ? 'Name is required (min 3 chars)' : 'Enter your name'"
      :class="{ error: errors.value.name }"
    />

    <button
      type="submit"
      :disabled="!isFormValid"
      :class="{ active: isFormValid }"
    >
      Submit
    </button>
  </form>
</template>
```

## 🚀 Lanjutan

Setelah memahami v-bind, Anda bisa melanjutkan ke:

1. **V-Model** - Two-way data binding untuk forms
2. **V-If / V-Show** - Conditional rendering
3. **V-For** - List rendering
4. **Event Handling** - `@click`, `@submit`, dll.
5. **Component Props** - Mengirim data ke child components

## 🔗 Referensi

- [Vue.js Documentation - Attribute Binding](https://vuejs.org/guide/essentials/template-syntax.html#attribute-bindings)
- [Vue.js Documentation - Class and Style Bindings](https://vuejs.org/guide/essentials/class-and-style.html)

---

## 🎉 Kesimpulan

V-Bind adalah directive yang sangat powerful dalam Vue.js yang memungkinkan Anda:

✅ Membuat atribut HTML yang dinamis dan reaktif
✅ Mengikat data JavaScript ke HTML attributes
✅ Mengontrol classes dan styles secara dinamis
✅ Membuat UI yang interaktif dan responsive

**Ingat:** Gunakan shorthand syntax (`:`) untuk kode yang lebih clean, dan manfaatkan computed properties untuk logic yang kompleks. V-Bind adalah fondasi untuk membuat aplikasi Vue.js yang dinamis dan interaktif!

*Master v-bind to create truly dynamic Vue.js applications! 🚀*