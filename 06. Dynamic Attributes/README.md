# 🎯 Tutorial Vue.js: Dynamic Attributes untuk Pemula

## 📖 Pengenalan Dynamic Attributes

**Dynamic Attributes** adalah fitur powerful dalam Vue.js yang memungkinkan Anda mengikat多个 atribut sekaligus dari sebuah object. Ini sangat berguna ketika Anda memiliki banyak atribut yang ingin diikat secara dinamis ke sebuah elemen HTML.

## 🤔 Apa itu Dynamic Attributes?

**Dynamic Attributes** adalah teknik untuk mengikat seluruh properti dari sebuah JavaScript object ke atribut HTML menggunakan sintaks khusus.

## ❓ Pertanyaan: Apa Fungsi Dynamic Attributes?

Dynamic Attributes memiliki beberapa fungsi utama yang sangat berguna:

### **1. 🔧 Simplifikasi Kode**
- **Fungsi:** Menggabungkan banyak atribut menjadi satu object
- **Manfaat:** Mengurangi repetisi dan membuat kode lebih clean
- **Contoh:** Daripada menulis 5 v-bind terpisah, gunakan 1 dynamic attributes

### **2. 📦 Grouping Related Properties**
- **Fungsi:** Mengelompokkan atribut yang terkait dalam satu object
- **Manfaat:** Lebih mudah mengelola atribut yang sejenis
- **Contoh:** Semua atribut gambar (src, alt, width, height) dalam satu object

### **3. 🔄 Reusability**
- **Fungsi:** Membuat object attributes yang bisa digunakan kembali
- **Manfaat:** Konsistensi dan DRY principle
- **Contoh:** Satu button attributes object untuk semua button di aplikasi

### **4. 🎯 Dynamic Configuration**
- **Fungsi:** Mengubah banyak atribut sekaligus berdasarkan kondisi
- **Manfaat:** Lebih mudah mengubah konfigurasi secara dinamis
- **Contoh:** Theme switching, form validation states

### **5. 🛡️ Type Safety**
- **Fungsi:** Struktur object yang jelas untuk atribut
- **Manfaat:** Mengurangi typo dan membuat debugging lebih mudah
- **Contoh:** Object structure validation untuk required attributes

**Sintaks Dasar:**
```html
<!-- Full syntax -->
<element v-bind="attributeObject"></element>

<!-- Shorthand syntax (Vue 3.3+) -->
<element :="attributeObject"></element>
```

## ❓ Pertanyaan: Gimana Cara Penggunaan Dynamic Attributes?

Dynamic Attributes sangat mudah digunakan dan sangat powerful! Berikut adalah cara-cara penggunaannya:

### 1. **Mengikat Multiple Image Attributes**
```vue
<script setup>
const imageInfo = {
  src: 'https://picsum.photos/seed/demo/400/300.jpg',
  alt: 'Gambar demo',
  width: 400,
  height: 300,
  loading: 'lazy',
  decoding: 'async'
}
</script>

<template>
  <!-- Mengikat semua atribut sekaligus -->
  <img :="imageInfo" />
</template>
```

### 2. **Mengikat Link Attributes**
```vue
<script setup>
const linkInfo = {
  href: 'https://vuejs.org',
  target: '_blank',
  rel: 'noopener noreferrer',
  title: 'Vue.js Documentation',
  class: 'external-link'
}
</script>

<template>
  <a :="linkInfo">Vue.js</a>
</template>
```

### 3. **Mengikat Form Input Attributes**
```vue
<script setup>
const inputAttrs = {
  type: 'email',
  name: 'user-email',
  id: 'email',
  placeholder: 'Enter your email',
  required: true,
  maxlength: 50,
  autocomplete: 'email'
}
</script>

<template>
  <input :="inputAttrs" />
</template>
```

### 4. **Mengikat Button Attributes dengan Class**
```vue
<script setup>
const buttonAttrs = {
  type: 'submit',
  disabled: false,
  class: ['btn', 'btn-primary', 'btn-lg'],
  'aria-label': 'Submit form',
  'data-action': 'submit'
}
</script>

<template>
  <button :="buttonAttrs">Submit</button>
</template>
```

## ❓ Pertanyaan: Kapan Dynamic Attributes Ini Cocok Digunakan?

Dynamic Attributes sangat cocok digunakan dalam situasi berikut:

### ✅ **Sangat Cocok Digunakan Untuk:**

1. **Elemen dengan Banyak Atribut Terkait**
   - Image dengan src, alt, width, height, loading, decoding
   - Link dengan href, target, rel, title, class
   - Form input dengan type, name, id, placeholder, required, pattern

2. **Komponen yang Reusable**
   - Button dengan berbagai variant dan state
   - Card dengan berbagai konfigurasi
   - Form fields dengan validasi dan styling

3. **Dynamic Content Generation**
   - Gallery images dengan ukuran berbeda
   - Product cards dengan atribut berbeda
   - User profiles dengan data dinamis

4. **Configuration Objects**
   - Theme configurations
   - Layout settings
   - Component props bundling

### ❌ **Kurang Cocok Digunakan Untuk:**

1. **Single Attribute**
   ```vue
   <!-- ❌ Tidak perlu dynamic attributes untuk satu atribut -->
   <img :="{ src: imageUrl }" />

   <!-- ✅ Lebih baik v-bind biasa -->
   <img :src="imageUrl" />
   ```

2. **Mixed Attribute Types**
   ```vue
   <!-- ❌ Menggabungkan atribut yang tidak terkait -->
   const mixedAttrs = {
     src: 'image.jpg',    // Image attribute
     href: 'link.html',   // Link attribute
     disabled: false      // Button attribute
   }
   ```

3. **Static Attributes**
   ```vue
   <!-- ❌ Tidak perlu untuk atribut statis -->
   const staticAttrs = { href: 'https://google.com' }

   <!-- ✅ Lebih baik HTML biasa -->
   <a href="https://google.com">Google</a>
   ```

## 📚 Contoh Praktis Berdasarkan Kode Sumber

Berdasarkan kode di `HelloWorld.vue`, ini adalah contoh nyata penggunaan Dynamic Attributes:

```vue
<script setup>
// Dynamic Binding With Multiple Attributes
const imageInfo = {
  src: 'https://images.unsplash.com/photo-1699646034253-0ebaa551d226?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  alt: 'Just a random picture',
  width: 400,
  height: 400
}
</script>

<template>
  <div>
    <!-- Mengikat semua atribut gambar sekaligus -->
    <img :="imageInfo" />
  </div>
</template>
```

**Penjelasan:**
- Object `imageInfo` berisi semua atribut yang dibutuhkan untuk gambar
- `:="imageInfo"` secara otomatis mengikat semua properti object ke atribut HTML
- Hasil rendering: `<img src="..." alt="..." width="400" height="400">`

## 🔥 Perbandingan: V-Bind Biasa vs Dynamic Attributes

### **V-Bind Biasa (Individual Binding)**
```vue
<script setup>
const imageSrc = 'image.jpg'
const imageAlt = 'Description'
const imageWidth = 300
const imageHeight = 200
</script>

<template>
  <img
    :src="imageSrc"
    :alt="imageAlt"
    :width="imageWidth"
    :height="imageHeight"
  />
</template>
```

### **Dynamic Attributes (Object Binding)**
```vue
<script setup>
const imageInfo = {
  src: 'image.jpg',
  alt: 'Description',
  width: 300,
  height: 200
}
</script>

<template>
  <img :="imageInfo" />
</template>
```

**Keuntungan Dynamic Attributes:**
✅ Kode lebih clean dan singkat
✅ Group atribut yang terkait
✅ Lebih mudah di-maintain
✅ Dapat digunakan untuk komponen reusable

## 🎯 Contoh Real-World

### 1. **Product Card E-Commerce**
```vue
<script setup>
const getProductAttributes = (product) => ({
  'data-product-id': product.id,
  'data-price': product.price,
  'data-in-stock': product.inStock,
  class: [
    'product-card',
    {
      'out-of-stock': !product.inStock,
      'featured-product': product.featured
    }
  ],
  'aria-label': `${product.name}, ${product.inStock ? 'in stock' : 'out of stock'}`
})

const product = ref({
  id: 1,
  name: 'Laptop',
  price: 15000000,
  inStock: true,
  featured: true
})
</script>

<template>
  <div :="getProductAttributes(product)">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price }}</p>
  </div>
</template>
```

### 2. **Dynamic Form Generator**
```vue
<script setup>
const getInputAttributes = (field) => ({
  type: field.type,
  name: field.name,
  id: field.name,
  required: field.required,
  placeholder: `Enter your ${field.label.toLowerCase()}...`,
  'data-field-type': field.type,
  'aria-required': field.required,
  autocomplete: field.type === 'password' ? 'new-password' : field.type
})

const formFields = ref([
  { name: 'username', type: 'text', label: 'Username', required: true },
  { name: 'email', type: 'email', label: 'Email', required: true },
  { name: 'password', type: 'password', label: 'Password', required: true }
])
</script>

<template>
  <form>
    <div v-for="field in formFields" :key="field.name">
      <label :for="field.name">{{ field.label }}</label>
      <input :="getInputAttributes(field)" />
    </div>
  </form>
</template>
```

### 3. **Button Component dengan Multiple States**
```vue
<script setup>
const getButtonAttributes = (variant, size, disabled) => ({
  type: 'button',
  disabled: disabled,
  class: [
    'btn',
    `btn-${variant}`,
    `btn-${size}`,
    { 'btn-disabled': disabled }
  ],
  'aria-disabled': disabled,
  'data-variant': variant,
  'data-size': size
})

const buttonVariant = ref('primary')
const buttonSize = ref('medium')
const isDisabled = ref(false)
</script>

<template>
  <button :="getButtonAttributes(buttonVariant, buttonSize, isDisabled)">
    Click me
  </button>
</template>
```

## 💡 Tips dan Best Practices

### 1. **Gunakan Reactive Objects**
```vue
<script setup>
import { reactive } from 'vue'

// ✅ Good - Reactive object
const imageAttrs = reactive({
  src: 'image.jpg',
  alt: 'Description',
  width: 300
})

// ❌ Avoid - Non-reactive object
const imageAttrs = {
  src: 'image.jpg',
  alt: 'Description',
  width: 300
}
</script>
```

### 2. **Gabungkan dengan Computed Properties**
```vue
<script setup>
import { computed } from 'vue'

const isLoading = ref(false)
const isActive = ref(true)

const buttonAttrs = computed(() => ({
  type: 'button',
  disabled: isLoading.value || !isActive.value,
  class: {
    'btn': true,
    'btn-loading': isLoading.value,
    'btn-active': isActive.value
  },
  'aria-busy': isLoading.value
}))
</script>

<template>
  <button :="buttonAttrs">
    {{ isLoading ? 'Loading...' : 'Click me' }}
  </button>
</template>
```

### 3. **Combine dengan Individual Bindings**
```vue
<script setup>
const baseAttrs = reactive({
  type: 'button',
  class: 'btn'
})

const isDisabled = ref(false)
const variant = ref('primary')
</script>

<template>
  <!-- ✅ Combine dynamic attributes with individual bindings -->
  <button :="baseAttrs" :disabled="isDisabled" :data-variant="variant">
    Click me
  </button>
</template>
```

## 📁 Struktur Proyek

```
06. Dynamic Attributes/
├── README.md                    # Dokumentasi ini
└── src/
    ├── components/
    │   └── HelloWorld.vue       # Komponen dengan contoh dynamic attributes
    └── App.vue                  # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `HelloWorld.vue`

```vue
<script setup>
// Dynamic Binding With Multiple Attributes
const imageInfo = {
  src: 'https://images.unsplash.com/photo-1699646034253-0ebaa551d226?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  alt: 'Just a random picture',
  width: 400,
  height: 400
}
</script>

<template>
  <div>
    <!-- <img v-bind="imageInfo" /> -->
    <img :="imageInfo" />
  </div>
</template>
```

## 📖 Penjelasan Mendalam

### 1. **Bagaimana Dynamic Attributes Bekerja**

Ketika Anda menggunakan `:="object"`, Vue akan:
1. Mengambil setiap properti dari object
2. Mengubahnya menjadi atribut HTML
3. Mengikat nilai properti ke atribut tersebut

**Contoh:**
```javascript
const imageInfo = {
  src: 'image.jpg',
  alt: 'Description',
  width: 300,
  height: 200
}
```

Hasil rendering:
```html
<img src="image.jpg" alt="Description" width="300" height="200">
```

### 2. **Sintaks Full vs Shorthand**

#### **Full Syntax: `v-bind:`**
```vue
<!-- Full syntax - kompatibel dengan semua versi Vue -->
<img v-bind="imageInfo" />
```

#### **Shorthand Syntax: `:`**
```vue
<!-- Shorthand syntax - Vue 3.3+ -->
<img :="imageInfo" />
```

**💡 Tips:** Gunakan shorthand syntax (`:`) untuk kode yang lebih clean jika Anda menggunakan Vue 3.3 atau lebih baru.

## 🚀 Contoh Lengkap dan Komprehensif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan dynamic attributes:

### Enhanced Demo: Dynamic Attributes Showcase

```vue
<script setup>
import { ref, computed, reactive } from 'vue'

// 1. Image Attributes Object
const imageInfo = reactive({
  src: 'https://picsum.photos/seed/vuejs/400/300.jpg',
  alt: 'Vue.js Dynamic Attributes Demo',
  width: 400,
  height: 300,
  loading: 'lazy',
  decoding: 'async'
})

// 2. Link Attributes Object
const linkInfo = reactive({
  href: 'https://vuejs.org',
  target: '_blank',
  rel: 'noopener noreferrer',
  title: 'Visit Vue.js Documentation',
  download: false
})

// 3. Button Attributes Object
const buttonInfo = reactive({
  type: 'button',
  disabled: false,
  'aria-label': 'Click me to perform action',
  'data-action': 'submit',
  role: 'button'
})

// 4. Form Input Attributes Object
const inputInfo = reactive({
  type: 'text',
  placeholder: 'Enter your name...',
  maxlength: 50,
  required: true,
  autocomplete: 'name',
  spellcheck: false
})

// 5. Video Attributes Object
const videoInfo = reactive({
  src: 'https://www.w3schools.com/html/mov_bbb.mp4',
  width: 400,
  height: 300,
  controls: true,
  autoplay: false,
  muted: true,
  loop: false,
  poster: 'https://picsum.photos/seed/video-poster/400/300.jpg'
})

// 6. Iframe Attributes Object
const iframeInfo = reactive({
  src: 'https://www.youtube.com/embed/dQw4w9WgXcQ',
  width: 560,
  height: 315,
  frameborder: 0,
  allow: 'accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture',
  allowfullscreen: true,
  loading: 'lazy'
})

// 7. Meta Attributes Object (for SEO)
const metaInfo = reactive({
  charset: 'UTF-8',
  name: 'viewport',
  content: 'width=device-width, initial-scale=1.0'
})

// 8. Dynamic Product Card Attributes
const productCards = reactive([
  {
    id: 1,
    name: 'Laptop',
    price: 15000000,
    inStock: true,
    featured: true,
    discount: 10
  },
  {
    id: 2,
    name: 'Mouse',
    price: 250000,
    inStock: false,
    featured: false,
    discount: 0
  },
  {
    id: 3,
    name: 'Keyboard',
    price: 750000,
    inStock: true,
    featured: true,
    discount: 15
  }
])

// Reactive state
const currentImageIndex = ref(0)
const isDarkMode = ref(false)
const buttonState = ref('normal') // normal, loading, disabled
const inputType = ref('text')

// Computed properties
const currentImage = computed(() => {
  const seeds = ['vuejs', 'dynamic', 'attributes', 'demo', 'showcase']
  return {
    ...imageInfo,
    src: `https://picsum.photos/seed/${seeds[currentImageIndex.value]}/400/300.jpg`,
    alt: `Dynamic Image ${currentImageIndex.value + 1}`
  }
})

const currentButtonInfo = computed(() => {
  const states = {
    normal: { ...buttonInfo, disabled: false, 'aria-label': 'Normal button' },
    loading: { ...buttonInfo, disabled: true, 'aria-label': 'Loading, please wait', 'data-loading': 'true' },
    disabled: { ...buttonInfo, disabled: true, 'aria-label': 'Button is disabled' }
  }
  return states[buttonState.value]
})

const currentInputInfo = computed(() => ({
  ...inputInfo,
  type: inputType.value,
  placeholder: `Enter your ${inputType.value}...`
}))

// Functions
function changeImage() {
  currentImageIndex.value = (currentImageIndex.value + 1) % 5
}

function toggleDarkMode() {
  isDarkMode.value = !isDarkMode.value
}

function changeButtonState(state) {
  buttonState.value = state
}

function changeInputType(type) {
  inputType.value = type
}

function updateImageSize(newWidth, newHeight) {
  imageInfo.width = newWidth
  imageInfo.height = newHeight
}

function changeLinkTarget() {
  linkInfo.target = linkInfo.target === '_blank' ? '_self' : '_blank'
  linkInfo.title = linkInfo.target === '_blank'
    ? 'Opens in new tab'
    : 'Opens in same tab'
}

function getProductAttributes(product) {
  return {
    'data-product-id': product.id,
    'data-price': product.price,
    'data-in-stock': product.inStock,
    'data-featured': product.featured,
    'data-discount': product.discount,
    class: [
      'product-card',
      {
        'out-of-stock': !product.inStock,
        'featured-product': product.featured,
        'has-discount': product.discount > 0
      }
    ]
  }
}
</script>

<template>
  <div class="dynamic-attributes-demo" :class="{ 'dark-theme': isDarkMode }">
    <h1>🎯 Vue.js Dynamic Attributes Demo</h1>

    <!-- 1. Image Dynamic Attributes -->
    <section class="demo-section">
      <h2>1️⃣ Image Dynamic Attributes</h2>
      <div class="demo-content">
        <div class="image-container">
          <img :="currentImage" class="dynamic-image" />
        </div>

        <div class="controls">
          <button @click="changeImage">Change Image</button>
          <div class="size-controls">
            <button @click="updateImageSize(300, 200)">Small (300x200)</button>
            <button @click="updateImageSize(400, 300)">Medium (400x300)</button>
            <button @click="updateImageSize(600, 400)">Large (600x400)</button>
          </div>
        </div>

        <div class="attribute-display">
          <h4>Current Attributes:</h4>
          <pre>{{ JSON.stringify(currentImage, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 2. Link Dynamic Attributes -->
    <section class="demo-section">
      <h2>2️⃣ Link Dynamic Attributes</h2>
      <div class="demo-content">
        <a :="linkInfo" class="dynamic-link">
          Vue.js Documentation
        </a>

        <button @click="changeLinkTarget">
          Toggle Target (Current: {{ linkInfo.target }})
        </button>

        <div class="attribute-display">
          <h4>Link Attributes:</h4>
          <pre>{{ JSON.stringify(linkInfo, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 3. Button Dynamic Attributes -->
    <section class="demo-section">
      <h2>3️⃣ Button Dynamic Attributes</h2>
      <div class="demo-content">
        <button :="currentButtonInfo" class="dynamic-button">
          {{ buttonState === 'loading' ? 'Loading...' : `Button (${buttonState})` }}
        </button>

        <div class="button-controls">
          <button @click="changeButtonState('normal')">Normal</button>
          <button @click="changeButtonState('loading')">Loading</button>
          <button @click="changeButtonState('disabled')">Disabled</button>
        </div>

        <div class="attribute-display">
          <h4>Button Attributes:</h4>
          <pre>{{ JSON.stringify(currentButtonInfo, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 4. Input Dynamic Attributes -->
    <section class="demo-section">
      <h2>4️⃣ Input Dynamic Attributes</h2>
      <div class="demo-content">
        <input :="currentInputInfo" class="dynamic-input" />

        <div class="input-controls">
          <button @click="changeInputType('text')">Text</button>
          <button @click="changeInputType('email')">Email</button>
          <button @click="changeInputType('password')">Password</button>
          <button @click="changeInputType('number')">Number</button>
          <button @click="changeInputType('tel')">Phone</button>
        </div>

        <div class="attribute-display">
          <h4>Input Attributes:</h4>
          <pre>{{ JSON.stringify(currentInputInfo, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 5. Video Dynamic Attributes -->
    <section class="demo-section">
      <h2>5️⃣ Video Dynamic Attributes</h2>
      <div class="demo-content">
        <video :="videoInfo" class="dynamic-video">
          Your browser does not support the video tag.
        </video>

        <div class="video-controls">
          <button @click="videoInfo.autoplay = !videoInfo.autoplay">
            Autoplay: {{ videoInfo.autoplay ? 'ON' : 'OFF' }}
          </button>
          <button @click="videoInfo.loop = !videoInfo.loop">
            Loop: {{ videoInfo.loop ? 'ON' : 'OFF' }}
          </button>
          <button @click="videoInfo.muted = !videoInfo.muted">
            Muted: {{ videoInfo.muted ? 'ON' : 'OFF' }}
          </button>
        </div>

        <div class="attribute-display">
          <h4>Video Attributes:</h4>
          <pre>{{ JSON.stringify(videoInfo, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 6. Product Cards with Dynamic Attributes -->
    <section class="demo-section">
      <h2>6️⃣ Product Cards with Dynamic Attributes</h2>
      <div class="demo-content">
        <div
          v-for="product in productCards"
          :key="product.id"
          :="getProductAttributes(product)"
          class="product-card-base"
        >
          <h3>{{ product.name }}</h3>
          <p>Price: {{ new Intl.NumberFormat('id-ID', {
            style: 'currency',
            currency: 'IDR'
          }).format(product.price) }}</p>
          <p v-if="product.discount > 0">
            Discount: {{ product.discount }}% OFF
          </p>
          <p>
            Status: {{ product.inStock ? '✅ In Stock' : '❌ Out of Stock' }}
          </p>
          <p v-if="product.featured">⭐ Featured Product</p>
        </div>
      </div>
    </section>

    <!-- 7. Complex Form with Dynamic Attributes -->
    <section class="demo-section">
      <h2>7️⃣ Complex Form with Dynamic Attributes</h2>
      <div class="demo-content">
        <form :="{
          action: '/submit',
          method: 'post',
          enctype: 'multipart/form-data',
          'data-form-id': 'dynamic-form'
        }" class="dynamic-form">

          <div
            v-for="field in [
              { label: 'Name', type: 'text', required: true },
              { label: 'Email', type: 'email', required: true },
              { label: 'Phone', type: 'tel', required: false },
              { label: 'Message', type: 'textarea', required: true }
            ]"
            :key="field.label"
            :="{
              class: 'form-group'
            }"
          >
            <label :="{
              for: field.label.toLowerCase(),
              'data-required': field.required
            }">
              {{ field.label }} {{ field.required ? '*' : '' }}
            </label>

            <input
              v-if="field.type !== 'textarea'"
              :="{
                type: field.type,
                name: field.label.toLowerCase(),
                id: field.label.toLowerCase(),
                required: field.required,
                placeholder: `Enter your ${field.label.toLowerCase()}...`,
                'data-field-type': field.type
              }"
            />

            <textarea
              v-else
              :="{
                name: field.label.toLowerCase(),
                id: field.label.toLowerCase(),
                required: field.required,
                placeholder: `Enter your ${field.label.toLowerCase()}...`,
                rows: 4,
                'data-field-type': field.type
              }"
            ></textarea>
          </div>

          <button :="{
            type: 'submit',
            class: 'submit-button',
            'data-loading': 'false'
          }">
            Submit Form
          </button>
        </form>
      </div>
    </section>

    <!-- 8. Iframe Dynamic Attributes -->
    <section class="demo-section">
      <h2>8️⃣ Iframe Dynamic Attributes</h2>
      <div class="demo-content">
        <iframe :="iframeInfo" class="dynamic-iframe">
          Your browser does not support iframes.
        </iframe>

        <div class="attribute-display">
          <h4>Iframe Attributes:</h4>
          <pre>{{ JSON.stringify(iframeInfo, null, 2) }}</pre>
        </div>
      </div>
    </section>

    <!-- 9. Theme Toggle -->
    <section class="demo-section">
      <h2>9️⃣ Global Theme Toggle</h2>
      <div class="demo-content">
        <button @click="toggleDarkMode" class="theme-toggle">
          {{ isDarkMode ? '☀️ Switch to Light Mode' : '🌙 Switch to Dark Mode' }}
        </button>
        <p>Current theme: <strong>{{ isDarkMode ? 'Dark Mode' : 'Light Mode' }}</strong></p>
      </div>
    </section>
  </div>
</template>

<style scoped>
.dynamic-attributes-demo {
  max-width: 1000px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  transition: all 0.3s ease;
}

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

.image-container {
  text-align: center;
}

.dynamic-image {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s;
}

.dynamic-image:hover {
  transform: scale(1.05);
}

.controls {
  display: flex;
  flex-direction: column;
  gap: 10px;
  align-items: center;
}

.size-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #369870;
}

.dynamic-link {
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

.dynamic-link:hover {
  background-color: #42b883;
  color: white;
}

.dynamic-button {
  padding: 12px 24px;
  font-size: 16px;
  margin-right: 10px;
}

.dynamic-button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.button-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.dynamic-input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
  width: 300px;
}

.dark-theme .dynamic-input {
  background-color: #3a3a3a;
  border-color: #555;
  color: #ecf0f1;
}

.input-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.dynamic-video {
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.video-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.product-card-base {
  background: #f8f9fa;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  margin-bottom: 10px;
  transition: all 0.3s;
}

.product-card-base:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.product-card-base.out-of-stock {
  opacity: 0.7;
  background-color: #fdf2f2;
}

.product-card-base.featured-product {
  border-color: #f39c12;
  background-color: #fff9e6;
}

.product-card-base.has-discount {
  border-color: #e74c3c;
}

.dark-theme .product-card-base {
  background-color: #3a3a3a;
  border-color: #555;
}

.dark-theme .product-card-base.out-of-stock {
  background-color: #3a1a1a;
}

.dark-theme .product-card-base.featured-product {
  background-color: #4a3a1a;
  border-color: #f39c12;
}

.dynamic-form {
  background: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
}

.dark-theme .dynamic-form {
  background-color: #3a3a3a;
  border-color: #555;
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
.form-group textarea {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.dark-theme .form-group input,
.dark-theme .form-group textarea {
  background-color: #4a4a4a;
  border-color: #666;
  color: #ecf0f1;
}

.submit-button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 12px 24px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.dynamic-iframe {
  border: none;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
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

.attribute-display {
  background-color: #f8f9fa;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 15px;
  margin-top: 15px;
}

.dark-theme .attribute-display {
  background-color: #3a3a3a;
  border-color: #555;
}

.attribute-display h4 {
  margin: 0 0 10px 0;
  color: #42b883;
}

pre {
  background-color: #2d2d2d;
  color: #f8f8f2;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
  font-size: 12px;
  margin: 0;
}
</style>
```

## 📚 Jenis-Jenis Dynamic Attributes

### 1. **Image Dynamic Attributes**
```vue
<script setup>
const imageAttrs = {
  src: 'https://example.com/image.jpg',
  alt: 'Description',
  width: 300,
  height: 200,
  loading: 'lazy',
  decoding: 'async'
}
</script>

<template>
  <img :="imageAttrs" />
</template>
```

### 2. **Link Dynamic Attributes**
```vue
<script setup>
const linkAttrs = {
  href: 'https://example.com',
  target: '_blank',
  rel: 'noopener noreferrer',
  title: 'Link title',
  download: false
}
</script>

<template>
  <a :="linkAttrs">Link Text</a>
</template>
```

### 3. **Form Input Dynamic Attributes**
```vue
<script setup>
const inputAttrs = {
  type: 'email',
  name: 'user-email',
  id: 'email',
  placeholder: 'Enter your email',
  required: true,
  maxlength: 50,
  autocomplete: 'email'
}
</script>

<template>
  <input :="inputAttrs" />
</template>
```

### 4. **Button Dynamic Attributes**
```vue
<script setup>
const buttonAttrs = {
  type: 'submit',
  disabled: false,
  'aria-label': 'Submit form',
  'data-action': 'submit',
  role: 'button'
}
</script>

<template>
  <button :="buttonAttrs">Submit</button>
</template>
```

### 5. **Media Dynamic Attributes (Video/Audio)**
```vue
<script setup>
const videoAttrs = {
  src: 'video.mp4',
  width: 600,
  height: 400,
  controls: true,
  autoplay: false,
  muted: true,
  loop: false,
  poster: 'poster.jpg'
}
</script>

<template>
  <video :="videoAttrs"></video>
</template>
```

### 6. **Iframe Dynamic Attributes**
```vue
<script setup>
const iframeAttrs = {
  src: 'https://example.com',
  width: 800,
  height: 600,
  frameborder: 0,
  allow: 'fullscreen',
  allowfullscreen: true,
  loading: 'lazy'
}
</script>

<template>
  <iframe :="iframeAttrs"></iframe>
</template>
```

## 🎯 Advanced Dynamic Attributes

### 1. **Computed Dynamic Attributes**
```vue
<script setup>
import { computed } from 'vue'

const isActive = ref(true)
const isLoading = ref(false)

const buttonAttrs = computed(() => ({
  type: 'button',
  disabled: isLoading.value,
  class: {
    'btn': true,
    'btn-active': isActive.value,
    'btn-loading': isLoading.value
  },
  'aria-busy': isLoading.value,
  'data-state': isLoading.value ? 'loading' : 'idle'
}))
</script>

<template>
  <button :="buttonAttrs">
    {{ isLoading ? 'Loading...' : 'Click me' }}
  </button>
</template>
```

### 2. **Conditional Dynamic Attributes**
```vue
<script setup>
import { computed } from 'vue'

const userType = ref('admin')
const permissions = ref(['read', 'write'])

const userAttrs = computed(() => {
  const baseAttrs = {
    'data-user-type': userType.value,
    class: 'user-card'
  }

  if (userType.value === 'admin') {
    return {
      ...baseAttrs,
      'data-admin': true,
      'data-permissions': permissions.value.join(',')
    }
  }

  return baseAttrs
})
</script>

<template>
  <div :="userAttrs">
    User Content
  </div>
</template>
```

### 3. **Dynamic Attributes with Functions**
```vue
<script setup>
const getLinkAttrs = (url, external = false) => ({
  href: url,
  target: external ? '_blank' : '_self',
  rel: external ? 'noopener noreferrer' : '',
  class: external ? 'external-link' : 'internal-link'
})

const getButtonAttrs = (variant = 'primary', size = 'medium') => ({
  type: 'button',
  class: [
    'btn',
    `btn-${variant}`,
    `btn-${size}`
  ],
  'data-variant': variant,
  'data-size': size
})
</script>

<template>
  <a :="getLinkAttrs('https://vuejs.org', true)">Vue.js</a>
  <button :="getButtonAttrs('secondary', 'large')">Click me</button>
</template>
```

## 🔥 Real-World Examples

### 1. **E-Commerce Product Card**
```vue
<script setup>
const product = ref({
  id: 1,
  name: 'Laptop',
  price: 15000000,
  inStock: true,
  featured: true,
  discount: 10
})

const getProductAttrs = (product) => ({
  'data-product-id': product.id,
  'data-price': product.price,
  'data-in-stock': product.inStock,
  'data-featured': product.featured,
  'data-discount': product.discount,
  class: [
    'product-card',
    {
      'out-of-stock': !product.inStock,
      'featured-product': product.featured,
      'has-discount': product.discount > 0
    }
  ],
  'aria-label': `${product.name}, ${product.inStock ? 'in stock' : 'out of stock'}`
})
</script>

<template>
  <div :="getProductAttrs(product)">
    <h3>{{ product.name }}</h3>
    <p>Price: {{ product.price }}</p>
  </div>
</template>
```

### 2. **Dynamic Form Generator**
```vue
<script setup>
const formFields = ref([
  { name: 'username', type: 'text', label: 'Username', required: true },
  { name: 'email', type: 'email', label: 'Email', required: true },
  { name: 'password', type: 'password', label: 'Password', required: true }
])

const getInputAttrs = (field) => ({
  type: field.type,
  name: field.name,
  id: field.name,
  required: field.required,
  placeholder: `Enter your ${field.label.toLowerCase()}...`,
  'data-field-type': field.type,
  'data-required': field.required,
  'aria-required': field.required,
  autocomplete: field.type === 'password' ? 'new-password' : field.type
})
</script>

<template>
  <form>
    <div v-for="field in formFields" :key="field.name">
      <label :for="field.name">{{ field.label }}</label>
      <input :="getInputAttrs(field)" />
    </div>
  </form>
</template>
```

## 🎯 Best Practices

### 1. **Use Reactive Objects**
```vue
<script setup>
import { reactive } from 'vue'

// ✅ Good - Reactive object
const imageAttrs = reactive({
  src: 'image.jpg',
  alt: 'Description',
  width: 300
})

// ❌ Avoid - Non-reactive object
const imageAttrs = {
  src: 'image.jpg',
  alt: 'Description',
  width: 300
}
</script>
```

### 2. **Combine with Other Bindings**
```vue
<script setup>
const baseAttrs = reactive({
  type: 'button',
  class: 'btn'
})

const isDisabled = ref(false)
const variant = ref('primary')
</script>

<template>
  <!-- ✅ Good - Combine dynamic attributes with individual bindings -->
  <button :="baseAttrs" :disabled="isDisabled" :data-variant="variant">
    Click me
  </button>

  <!-- ✅ Also good - Include everything in the object -->
  <button :="{
    ...baseAttrs,
    disabled: isDisabled,
    'data-variant': variant
  }">
    Click me
  </button>
</template>
```

### 3. **Use Computed Properties for Complex Logic**
```vue
<script setup>
import { computed } from 'vue'

const userRole = ref('admin')
const isLoading = ref(false)
const isActive = ref(true)

// ✅ Computed property for complex attribute logic
const buttonAttrs = computed(() => ({
  type: 'button',
  disabled: isLoading.value || !isActive.value,
  class: [
    'btn',
    `btn-${userRole.value}`,
    {
      'loading': isLoading.value,
      'disabled': !isActive.value
    }
  ],
  'aria-label': isLoading.value
    ? 'Loading, please wait'
    : `${userRole.value} action button`,
  'data-role': userRole.value,
  'data-state': isLoading.value ? 'loading' : 'ready'
}))
</script>

<template>
  <!-- ✅ Clean template with computed attributes -->
  <button :="buttonAttrs">
    {{ isLoading ? 'Loading...' : 'Action' }}
  </button>
</template>
```

### 4. **Avoid Overuse**
```vue
<script setup>
// ✅ Good - Use dynamic attributes for related properties
const imageAttrs = reactive({
  src: 'image.jpg',
  alt: 'Description',
  width: 300,
  height: 200,
  loading: 'lazy'
})

// ❌ Avoid - Don't group unrelated properties
const mixedAttrs = reactive({
  src: 'image.jpg',        // Image attribute
  href: 'link.html',       // Link attribute
  disabled: false,         // Button attribute
  maxlength: 50           // Input attribute
})
</script>
```

## 🛠️ Cara Menjalankan Proyek

```bash
# Masuk ke folder proyek
cd "06. Dynamic Attributes"

# Install dependencies
npm install

# Jalankan development server
npm run dev
```

## 🎪 Latihan Praktis

### Latihan 1: Dynamic Image Gallery
Buat image gallery dengan dynamic attributes:

```vue
<script setup>
import { ref, computed } from 'vue'

const images = ref([
  { id: 1, src: 'image1.jpg', alt: 'Image 1', featured: true },
  { id: 2, src: 'image2.jpg', alt: 'Image 2', featured: false },
  { id: 3, src: 'image3.jpg', alt: 'Image 3', featured: true }
])

const selectedImageId = ref(1)

const getImageAttrs = (image) => ({
  src: `https://picsum.photos/seed/${image.src}/300/200.jpg`,
  alt: image.alt,
  width: 300,
  height: 200,
  class: [
    'gallery-image',
    {
      'featured': image.featured,
      'selected': image.id === selectedImageId.value
    }
  ],
  'data-image-id': image.id,
  'data-featured': image.featured,
  loading: 'lazy',
  onClick: () => selectedImageId.value = image.id
})
</script>

<template>
  <div class="gallery">
    <img
      v-for="image in images"
      :key="image.id"
      :="getImageAttrs(image)"
    />
  </div>
</template>
```

### Latihan 2: Dynamic Form Builder
Buat form builder dengan dynamic attributes:

```vue
<script setup>
import { ref } from 'vue'

const formConfig = ref([
  {
    type: 'text',
    name: 'fullName',
    label: 'Full Name',
    required: true,
    placeholder: 'Enter your full name',
    maxLength: 50
  },
  {
    type: 'email',
    name: 'email',
    label: 'Email Address',
    required: true,
    placeholder: 'Enter your email',
    pattern: '[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}$'
  },
  {
    type: 'textarea',
    name: 'message',
    label: 'Message',
    required: true,
    placeholder: 'Enter your message',
    rows: 4,
    maxLength: 500
  }
])

const getFieldAttrs = (field) => ({
  type: field.type,
  name: field.name,
  id: field.name,
  required: field.required,
  placeholder: field.placeholder,
  maxLength: field.maxLength,
  pattern: field.pattern,
  rows: field.rows,
  class: 'form-field',
  'data-field-type': field.type,
  'data-required': field.required,
  'aria-required': field.required,
  'aria-invalid': 'false'
})
</script>

<template>
  <form>
    <div v-for="field in formConfig" :key="field.name" class="form-group">
      <label :for="field.name">{{ field.label }}</label>
      <input v-if="field.type !== 'textarea'" :="getFieldAttrs(field)" />
      <textarea v-else :="getFieldAttrs(field)"></textarea>
    </div>
  </form>
</template>
```

## 🚀 Lanjutan

Setelah memahami dynamic attributes, Anda bisa melanjutkan ke:

1. **Custom Directives** - Membuat directive sendiri
2. **Provide/Inject** - Dependency injection
3. **Teleport** - Render content di luar component hierarchy
4. **Suspense** - Handle async components
5. **Advanced State Management** - Pinia patterns

## 🔗 Referensi

- [Vue.js Documentation - Attribute Binding](https://vuejs.org/guide/essentials/template-syntax.html#attribute-bindings)
- [Vue.js Documentation - Dynamic Arguments](https://vuejs.org/guide/essentials/template-syntax.html#dynamic-arguments)

---

## 🎉 Kesimpulan

Dynamic Attributes adalah fitur yang sangat powerful dalam Vue.js yang memungkinkan Anda:

✅ Mengelola banyak atribut secara bersamaan
✅ Membuat kode yang lebih clean dan maintainable
✅ Menggabungkan atribut yang terkait dalam satu object
✅ Membuat komponen yang lebih reusable dan flexible
✅ Mengurangi repetisi kode

**Ingat:** Gunakan dynamic attributes ketika Anda memiliki beberapa atribut yang terkait dan ingin mengelolanya secara bersamaan. Ini akan membuat kode Anda lebih organized dan easier to maintain!

*Master dynamic attributes to build truly flexible and maintainable Vue.js components! 🚀*