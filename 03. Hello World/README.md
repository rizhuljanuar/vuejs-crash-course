# 🎓 Tutorial Vue.js: Hello World untuk Pemula

## 📖 Pengenalan Vue.js Dasar

Selamat datang di tutorial Vue.js pertama Anda! Dokumentasi ini akan memandu Anda langkah demi langkah untuk memahami dasar-dasar Vue.js melalui proyek "Hello World" yang sederhana.

## 🎯 Apa yang Akan Anda Pelajari?

1. **Struktur dasar komponen Vue.js**
2. **Script setup syntax** (Vue 3 terbaru)
3. **Template rendering** dengan HTML
4. **Scoped styling** untuk CSS
5. **Import dan export komponen**
6. **Konsep reactivity** dasar

## 📁 Struktur Proyek

```
03. Hello World/
├── README.md           # Dokumentasi ini
└── src/               # Folder source code
    ├── components/    # Folder komponen
    │   └── HelloWorld.vue  # Komponen Hello World
    └── App.vue        # Komponen utama aplikasi
```

## 🔍 Analisis Kode Sumber

### 1. Komponen `HelloWorld.vue`

Mari kita pelajari komponen pertama Anda:

```vue
<script setup>
// JavaScript logic for your component
</script>

<template>
  <!-- HTML template for your component -->
</template>

<style scoped>
/* Styles specific to your component */
</style>
```

#### 📝 Penjelasan Setiap Bagian:

**🔹 `<script setup>`**
- Bagian JavaScript dari komponen
- Menggunakan **Composition API** dari Vue 3
- `setup` attribute membuat kode lebih ringkas
- Tempat mendefinisikan logika, data, dan fungsi

**🔹 `<template>`**
- Bagian HTML dari komponen
- Mendefinisikan struktur tampilan
- Bisa mengandung data dinamis
- Sintaks mirip HTML tapi lebih powerful

**🔹 `<style scoped>`**
- Bagian CSS dari komponen
- `scoped` attribute membuat CSS hanya berlaku untuk komponen ini
- Mencegah konflik CSS dengan komponen lain

### 2. Komponen `App.vue`

Komponen utama yang menampilkan HelloWorld:

```vue
<script setup>
import HelloWorld from '@/components/HelloWorld.vue'
</script>

<template>
  <div>
    <HelloWorld />
  </div>
</template>
```

#### 📝 Penjelasan:

**🔹 `import HelloWorld`**
- Mengimpor komponen HelloWorld dari folder components
- `@` adalah alias untuk folder `src/`
- Ekstensi `.vue` bisa dihilangkan dalam import

**🔹 `<HelloWorld />`**
- Menggunakan komponen yang sudah diimport
- Self-closing tag untuk komponen tanpa children
- Vue akan merender komponen HelloWorld di sini

## 🚀 Membuat Hello World yang Lebih Menarik

Mari kita tingkatkan komponen HelloWorld untuk menampilkan pesan yang lebih interaktif:

### Versi 1: Hello World Sederhana

**📄 `src/components/HelloWorld.vue`**

```vue
<script setup>
// Definisikan variabel message
const message = 'Hello, World!'
</script>

<template>
  <h1>{{ message }}</h1>
</template>

<style scoped>
h1 {
  color: #42b883; /* Warna khas Vue.js */
  text-align: center;
  font-family: Arial, sans-serif;
}
</style>
```

**Penjelasan:**
- `{{ message }}` adalah **template interpolation**
- Vue akan mengganti `{{ message }}` dengan nilai variabel `message`
- Data di Vue bersifat **reactive** - jika berubah, tampilan akan otomatis update

### Versi 2: Hello World Interaktif

Mari tambahkan interaksi dengan tombol:

```vue
<script setup>
import { ref } from 'vue'

// Gunakan ref untuk membuat reactive data
const message = ref('Hello, World!')
const count = ref(0)

// Fungsi untuk mengubah pesan
function changeMessage() {
  message.value = 'Hello, Vue.js!'
}

// Fungsi untuk menambah counter
function increment() {
  count.value++
}
</script>

<template>
  <div class="hello-container">
    <h1>{{ message }}</h1>

    <div class="counter">
      <p>Counter: {{ count }}</p>
      <button @click="increment">Tambah (+1)</button>
    </div>

    <div class="actions">
      <button @click="changeMessage">Ubah Pesan</button>
    </div>
  </div>
</template>

<style scoped>
.hello-container {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #42b883;
  font-size: 2.5em;
  margin-bottom: 30px;
}

.counter {
  margin: 20px 0;
  padding: 15px;
  background-color: #f5f5f5;
  border-radius: 5px;
}

.counter p {
  font-size: 1.2em;
  margin: 0 0 15px 0;
}

button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1em;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #369870;
}

.actions {
  margin-top: 20px;
}
</style>
```

**Konsep Baru yang Dipelajari:**

1. **`ref()`** - Membuat reactive data
2. **`.value`** - Mengakses nilai ref di JavaScript
3. **`@click`** - Event listener untuk klik
4. **Reactivity** - Tampilan otomatis update saat data berubah

### Versi 3: Hello World dengan Input User

Mari tambahkan input untuk nama user:

```vue
<script setup>
import { ref, computed } from 'vue'

// Reactive data
const name = ref('')
const showMessage = ref(false)

// Computed property
const greeting = computed(() => {
  if (name.value.trim() === '') {
    return 'Hello, World!'
  }
  return `Hello, ${name.value}!`
})

// Fungsi
function submitName() {
  if (name.value.trim() !== '') {
    showMessage.value = true
  }
}

function reset() {
  name.value = ''
  showMessage.value = false
}
</script>

<template>
  <div class="hello-container">
    <!-- Input Section -->
    <div v-if="!showMessage" class="input-section">
      <h2>Masukkan Nama Anda</h2>
      <input
        v-model="name"
        placeholder="Ketik nama Anda..."
        @keyup.enter="submitName"
      />
      <button
        @click="submitName"
        :disabled="name.trim() === ''"
      >
        Sapa Saya!
      </button>
    </div>

    <!-- Greeting Section -->
    <div v-else class="greeting-section">
      <h1>{{ greeting }}</h1>
      <p>Selamat datang di dunia Vue.js! 🎉</p>
      <button @click="reset">Coba Lagi</button>
    </div>
  </div>
</template>

<style scoped>
.hello-container {
  max-width: 500px;
  margin: 50px auto;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.input-section h2 {
  text-align: center;
  margin-bottom: 20px;
}

input {
  width: 100%;
  padding: 12px;
  margin-bottom: 15px;
  border: 2px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
  box-sizing: border-box;
}

input:focus {
  outline: none;
  border-color: #42b883;
}

button {
  width: 100%;
  background-color: #42b883;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1.1em;
  transition: all 0.3s;
}

button:hover:not(:disabled) {
  background-color: #369870;
  transform: translateY(-2px);
}

button:disabled {
  background-color: #cccccc;
  cursor: not-allowed;
}

.greeting-section {
  text-align: center;
}

.greeting-section h1 {
  font-size: 2.5em;
  margin-bottom: 20px;
}

.greeting-section p {
  font-size: 1.2em;
  margin-bottom: 25px;
}
</style>
```

**Konsep Baru:**

1. **`v-model`** - Two-way data binding untuk input
2. **`v-if` / `v-else`** - Conditional rendering
3. **`computed()`** - Properties yang dihitung otomatis
4. **`:disabled`** - Dynamic attribute binding
5. **`@keyup.enter`** - Event listener untuk tombol Enter

## 🎯 Konsep Vue.js yang Sudah Dipelajari

### 1. **Komponen Vue (`.vue` files)**
Struktur 3 bagian: `<script>`, `<template>`, `<style>`

### 2. **Composition API (`<script setup>`)**
- Cara modern menulis komponen Vue
- Lebih ringkas dan mudah dipahami
- Organisasi kode yang lebih baik

### 3. **Reactivity System**
- **`ref()`** - Membuat data reactive
- **`computed()`** - Properties yang dihitung
- **Auto-update** - Tampilan berubah otomatis

### 4. **Template Syntax**
- **`{{ }}`** - Text interpolation
- **`v-if`** - Conditional rendering
- **`v-model`** - Two-way binding
- **`@click`** - Event handling

### 5. **Styling**
- **Scoped CSS** - Styles hanya untuk komponen ini
- **Dynamic styling** - Styles bisa berubah berdasarkan data

## 🛠️ Cara Menjalankan Proyek

### Setup Awal
```bash
# Masuk ke folder proyek
cd "03. Hello World"

# Install dependencies (pilih salah satu)
npm install          # atau
yarn install         # atau
pnpm install         # atau
bun install
```

### Development Server
```bash
# Jalankan development server
npm run dev          # atau
yarn dev             # atau
pnpm dev             # atau
bun dev
```

Buka browser dan akses `http://localhost:5173` (atau port yang ditampilkan di terminal).

## 🎚️ Tingkat Kesulitan Progressif

### 🟢 Level 1: Basic (Versi 1)
- Menampilkan teks statis
- Memahami struktur komponen dasar
- Menggunakan variabel di template

### 🟡 Level 2: Intermediate (Versi 2)
- Event handling (`@click`)
- Reactive data dengan `ref()`
- Fungsi JavaScript di komponen
- Conditional class dan styling

### 🔴 Level 3: Advanced (Versi 3)
- Form handling dengan `v-model`
- Conditional rendering dengan `v-if`
- Computed properties
- Dynamic attributes
- Complex interactions

## 💡 Tips untuk Pemula

### 1. **Memahami Reactivity**
```javascript
// ❌ Salah - tidak akan update tampilan
let message = 'Hello'

// ✅ Benar - reactive data
const message = ref('Hello')
message.value = 'New message' // Tampilan akan update
```

### 2. **Event Handling**
```vue
<!-- Cara 1: Inline function -->
<button @click="count++">Tambah</button>

<!-- Cara 2: Method call -->
<button @click="increment">Tambah</button>
```

### 3. **Styling Best Practices**
```vue
<style scoped>
/* Gunakan scoped untuk menghindari konflik CSS */
.component-name {
  /* Gunakan BEM naming convention */
}
</style>
```

### 4. **Debugging Tips**
- Gunakan `console.log()` untuk melihat nilai variabel
- Install Vue DevTools di browser
- Periksa console untuk error messages

## 🚀 Next Steps

Setelah memahami dasar-dasar ini, Anda bisa melanjutkan ke:

1. **Component Props** - Mengirim data ke komponen lain
2. **Component Events** - Komunikasi dari child ke parent
3. **List Rendering** - Menampilkan data array dengan `v-for`
4. **Vue Router** - Navigasi antar halaman
5. **State Management** - Mengelola data global dengan Pinia

## 🔗 Referensi Tambahan

- [Dokumentasi Resmi Vue.js](https://vuejs.org/)
- [Vue.js Style Guide](https://vuejs.org/style-guide/)
- [Vue DevTools](https://devtools.vuejs.org/)
- [Vue School - Free Tutorials](https://vueschool.io/lessons)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Vue.js pertama Anda! 🎊

Dari sini, Anda sudah memahami konsep fundamental Vue.js dan siap untuk melanjutkan ke topik yang lebih advanced. Teruslah berlatih dan jangan ragu untuk bereksperimen dengan kode!

---

*"Simplicity is the ultimate sophistication." - Leonardo da Vinci*

Vue.js dirancang untuk sederhana namun powerful. Nikmati perjalanan belajar Anda! 🚀