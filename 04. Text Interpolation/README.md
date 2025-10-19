# 📝 Tutorial Vue.js: Text Interpolation untuk Pemula

## 🎯 Pengenalan Text Interpolation

Text Interpolation adalah fitur fundamental dalam Vue.js yang memungkinkan Anda menampilkan data dinamis di dalam template HTML. Ini adalah cara pertama dan paling umum untuk "menghubungkan" data JavaScript dengan tampilan HTML.

## 🤔 Apa itu Text Interpolation?

**Text Interpolation** adalah teknik untuk menyisipkan nilai JavaScript ke dalam HTML menggunakan sintaks double curly braces: `{{ }}`.

**Sintaks Dasar:**
```html
{{ expressionJavaScript }}
```

## 📁 Struktur Proyek

```
04. Text Interpolation/
├── README.md                    # Dokumentasi ini
└── src/
    ├── components/
    │   └── HelloWorld.vue       # Komponen dengan contoh interpolation
    └── App.vue                  # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `HelloWorld.vue`

```vue
<script setup>
const myMessage = 'Yellow World'
const myNumber = 100

function add(x, y) {
  return x + y
}
</script>

<template>
  <div>
    <h1>{{ myMessage }}</h1>
    <h1>{{ myNumber }}</h1>
    <h1>2 + 2 = {{ 2 + 2 }}</h1>
    <h1>Add Two Numbers: {{ add(2, 2) }}</h1>
    <!-- <p>Not allowed: {{ const myName = 'huxn' }}</p> ❌ -->
  </div>
</template>
```

## 📖 Penjelasan Mendalam

### 1. **Variable Interpolation**
```vue
<script setup>
const myMessage = 'Yellow World'
const myNumber = 100
</script>

<template>
  <h1>{{ myMessage }}</h1>  <!-- Menampilkan: Yellow World -->
  <h1>{{ myNumber }}</h1>   <!-- Menampilkan: 100 -->
</template>
```

**Yang terjadi:**
- Vue mengganti `{{ myMessage }}` dengan nilai variabel `myMessage`
- Vue mengganti `{{ myNumber }}` dengan nilai variabel `myNumber`
- Perubahan pada variabel akan otomatis update tampilan (reactive)

### 2. **Expression Interpolation**
```vue
<template>
  <h1>2 + 2 = {{ 2 + 2 }}</h1>  <!-- Menampilkan: 2 + 2 = 4 -->
  <h1>{{ 10 * 5 }}</h1>          <!-- Menampilkan: 50 -->
  <h1>{{ 'Hello' + ' ' + 'World' }}</h1>  <!-- Menampilkan: Hello World -->
</template>
```

**Yang bisa dilakukan:**
- Operasi matematika: `+`, `-`, `*`, `/`
- String concatenation: `+`
- Ternary operator: `condition ? value1 : value2`
- Method calls: `methodName()`

### 3. **Function Call Interpolation**
```vue
<script setup>
function add(x, y) {
  return x + y
}

function greet(name) {
  return `Hello, ${name}!`
}

function isAdult(age) {
  return age >= 18 ? 'Dewasa' : 'Anak-anak'
}
</script>

<template>
  <h1>Add Two Numbers: {{ add(2, 2) }}</h1>           <!-- Menampilkan: 4 -->
  <h1>{{ greet('Vue.js') }}</h1>                      <!-- Menampilkan: Hello, Vue.js! -->
  <h1>Status: {{ isAdult(20) }}</h1>                  <!-- Menampilkan: Status: Dewasa -->
</template>
```

## ⚠️ Yang TAK Boleh Dilakukan

### 1. **Statement Tidak Diperbolehkan**
```vue
<!-- ❌ SALAH - Ini tidak akan bekerja -->
<p>{{ const myName = 'huxn' }}</p>
<p>{{ if (condition) { return 'yes' } }}</p>
<p>{{ for (let i = 0; i < 5; i++) { console.log(i) } }}</p>
```

**Mengapa salah?** Interpolation hanya menerima **expression**, bukan **statement**.

### 2. **Global Object Tidak Dapat Diakses Langsung**
```vue
<!-- ❌ SALAH - Tidak bisa mengakses global objects -->
<p>{{ console.log('hello') }}</p>
<p>{{ window.location }}</p>
```

## ✅ Yang Boleh Dilakukan

### 1. **JavaScript Expressions**
```vue
<script setup>
const user = {
  name: 'John Doe',
  age: 25,
  isAdmin: true
}

const numbers = [1, 2, 3, 4, 5]

const isLoggedIn = true
</script>

<template>
  <!-- Object property access -->
  <p>Name: {{ user.name }}</p>
  <p>Age: {{ user.age }}</p>

  <!-- Array access -->
  <p>First number: {{ numbers[0] }}</p>
  <p>Last number: {{ numbers[numbers.length - 1] }}</p>

  <!-- Ternary operator -->
  <p>Status: {{ isLoggedIn ? 'Logged In' : 'Logged Out' }}</p>

  <!-- Logical expressions -->
  <p>Can edit: {{ isLoggedIn && user.isAdmin }}</p>
</template>
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang lebih kompleks untuk mendemonstrasikan berbagai jenis interpolation:

### Versi Enhanced: Text Interpolation Showcase

```vue
<script setup>
import { ref, computed } from 'vue'

// Reactive data
const name = ref('Vue.js Developer')
const age = ref(25)
const score = ref(85)
const isLoggedIn = ref(true)
const items = ref(['Apple', 'Banana', 'Orange'])

// Computed properties
const status = computed(() => {
  return age.value >= 18 ? 'Dewasa' : 'Anak-anak'
})

const grade = computed(() => {
  if (score.value >= 90) return 'A'
  if (score.value >= 80) return 'B'
  if (score.value >= 70) return 'C'
  return 'D'
})

const greeting = computed(() => {
  const hour = new Date().getHours()
  if (hour < 12) return 'Good Morning'
  if (hour < 18) return 'Good Afternoon'
  return 'Good Evening'
})

// Functions
function formatCurrency(amount) {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR'
  }).format(amount)
}

function calculateDiscount(price, discount) {
  return price - (price * discount / 100)
}

function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1)
}

// Update functions untuk demo
function updateAge() {
  age.value = Math.floor(Math.random() * 30) + 10
}

function updateScore() {
  score.value = Math.floor(Math.random() * 40) + 60
}

function toggleLogin() {
  isLoggedIn.value = !isLoggedIn.value
}
</script>

<template>
  <div class="interpolation-demo">
    <h1>🎓 Vue.js Text Interpolation Demo</h1>

    <!-- 1. Basic Variable Interpolation -->
    <section class="demo-section">
      <h2>1️⃣ Basic Variable Interpolation</h2>
      <p>Nama: {{ name }}</p>
      <p>Umur: {{ age }} tahun</p>
      <p>Score: {{ score }} poin</p>

      <div class="actions">
        <button @click="updateAge">Random Umur</button>
        <button @click="updateScore">Random Score</button>
      </div>
    </section>

    <!-- 2. Mathematical Expressions -->
    <section class="demo-section">
      <h2>2️⃣ Mathematical Expressions</h2>
      <p>10 + 5 = {{ 10 + 5 }}</p>
      <p>100 * 2 = {{ 100 * 2 }}</p>
      <p>Score + 15 = {{ score + 15 }}</p>
      <p>Umur * 12 bulan = {{ age * 12 }} bulan</p>
    </section>

    <!-- 3. String Operations -->
    <section class="demo-section">
      <h2>3️⃣ String Operations</h2>
      <p>"Hello" + " " + "World" = {{ "Hello" + " " + "World" }}</p>
      <p>Nama uppercase: {{ name.toUpperCase() }}</p>
      <p>Nama capitalize: {{ capitalize(name) }}</p>
      <p>Panjang nama: {{ name.length }} karakter</p>
    </section>

    <!-- 4. Ternary Operator -->
    <section class="demo-section">
      <h2>4️⃣ Conditional Logic (Ternary Operator)</h2>
      <p>Status: {{ age >= 18 ? 'Dewasa' : 'Anak-anak' }}</p>
      <p>Grade: {{ score >= 80 ? 'Lulus' : 'Remedial' }}</p>
      <p>Login: {{ isLoggedIn ? '✅ Logged In' : '❌ Logged Out' }}</p>

      <button @click="toggleLogin">Toggle Login Status</button>
    </section>

    <!-- 5. Function Calls -->
    <section class="demo-section">
      <h2>5️⃣ Function Calls</h2>
      <p>Harga: {{ formatCurrency(2500000) }}</p>
      <p>Discount 20%: {{ formatCurrency(calculateDiscount(100000, 20)) }}</p>
      <p>Sapaan: {{ greeting('John') }}!</p>
      <p>Current greeting: {{ greeting }}, {{ name }}!</p>
    </section>

    <!-- 6. Computed Properties -->
    <section class="demo-section">
      <h2>6️⃣ Computed Properties</h2>
      <p>Status usia: {{ status }}</p>
      <p>Grade nilai: {{ grade }}</p>
      <p>Waktu sekarang: {{ greeting }}</p>
    </section>

    <!-- 7. Array Operations -->
    <section class="demo-section">
      <h2>7️⃣ Array Operations</h2>
      <p>Items: {{ items.join(', ') }}</p>
      <p>Jumlah items: {{ items.length }}</p>
      <p>Item pertama: {{ items[0] }}</p>
      <p>Item terakhir: {{ items[items.length - 1] }}</p>
    </section>

    <!-- 8. Complex Expressions -->
    <section class="demo-section">
      <h2>8️⃣ Complex Expressions</h2>
      <p>Final score: {{ score + (age >= 18 ? 10 : 5) }}</p>
      <p>Display name: {{ isLoggedIn ? name : 'Guest' }}</p>
      <p>Message: {{ greeting }}, {{ capitalize(name) }}! Anda {{ status }}.</p>
    </section>
  </div>
</template>

<style scoped>
.interpolation-demo {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #42b883;
  margin-bottom: 30px;
  font-size: 2.5em;
}

.demo-section {
  background: #f8f9fa;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
  border-left: 4px solid #42b883;
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.demo-section p {
  margin: 8px 0;
  padding: 8px;
  background: white;
  border-radius: 5px;
  font-family: 'Courier New', monospace;
  border-left: 3px solid #3498db;
}

.actions {
  margin-top: 15px;
}

button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 8px 16px;
  margin-right: 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9em;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #369870;
}
</style>
```

## 📚 Jenis-Jenis Expression yang Bisa Digunakan

### 1. **Literals**
```vue
<p>{{ 'Hello World' }}</p>      <!-- String literal -->
<p>{{ 42 }}</p>                  <!-- Number literal -->
<p>{{ true }}</p>                <!-- Boolean literal -->
<p>{{ null }}</p>                <!-- Null literal -->
<p>{{ undefined }}</p>           <!-- Undefined literal -->
```

### 2. **Variables**
```vue
<script setup>
const message = 'Hello'
const count = 10
const isActive = true
</script>

<template>
  <p>{{ message }}</p>
  <p>{{ count }}</p>
  <p>{{ isActive }}</p>
</template>
```

### 3. **Mathematical Operations**
```vue
<p>{{ 5 + 3 }}</p>               <!-- 8 -->
<p>{{ 10 - 4 }}</p>              <!-- 6 -->
<p>{{ 3 * 4 }}</p>               <!-- 12 -->
<p>{{ 15 / 3 }}</p>              <!-- 5 -->
<p>{{ 17 % 5 }}</p>              <!-- 2 (modulo) -->
<p>{{ 2 ** 3 }}</p>              <!-- 8 (exponent) -->
```

### 4. **String Operations**
```vue
<p>{{ 'Hello' + ' ' + 'World' }}</p>
<p>{{ 'Vue.js'.toUpperCase() }}</p>
<p>{{ 'Vue.js'.length }}</p>
<p>{{ 'Vue.js'.substring(0, 3) }}</p>
```

### 5. **Logical Operations**
```vue
<script setup>
const isLoggedIn = true
const isAdmin = false
const user = { name: 'John' }
</script>

<template>
  <p>{{ isLoggedIn && isAdmin }}</p>        <!-- false -->
  <p>{{ isLoggedIn || isAdmin }}</p>        <!-- true -->
  <p>{{ !isLoggedIn }}</p>                  <!-- false -->
  <p>{{ user && user.name }}</p>            <!-- John -->
</template>
```

### 6. **Comparison Operations**
```vue
<script setup>
const age = 25
const score = 85
</script>

<template>
  <p>{{ age >= 18 }}</p>                     <!-- true -->
  <p>{{ score > 90 }}</p>                    <!-- false -->
  <p>{{ age === 25 }}</p>                    <!-- true -->
  <p>{{ score !== 100 }}</p>                 <!-- true -->
</template>
```

### 7. **Ternary Operator**
```vue
<script setup>
const age = 20
const isLoggedIn = true
</script>

<template>
  <p>{{ age >= 18 ? 'Dewasa' : 'Anak-anak' }}</p>
  <p>{{ isLoggedIn ? 'Welcome back!' : 'Please login' }}</p>
</template>
```

### 8. **Function Calls**
```vue
<script setup>
function add(a, b) { return a + b }
function formatDate(date) {
  return new Date(date).toLocaleDateString()
}
</script>

<template>
  <p>{{ add(5, 3) }}</p>
  <p>{{ formatDate('2024-01-01') }}</p>
</template>
```

## 🎯 Best Practices

### 1. **Keep it Simple**
```vue
<!-- ✅ Baik -->
<p>{{ user.name }}</p>

<!-- ❌ Hindari logic yang terlalu kompleks -->
<p>{{ users.filter(u => u.age >= 18 && u.isActive).map(u => u.name).join(', ') }}</p>

<!-- ✅ Lebih baik dengan computed property -->
<p>{{ adultUsers }}</p>
```

### 2. **Use Computed Properties for Complex Logic**
```vue
<script setup>
import { computed } from 'vue'

const price = ref(100)
const quantity = ref(5)
const discount = ref(10)

// ✅ Computed property untuk logic yang kompleks
const totalPrice = computed(() => {
  const subtotal = price.value * quantity.value
  const discountAmount = subtotal * (discount.value / 100)
  return subtotal - discountAmount
})
</script>

<template>
  <p>Total: {{ totalPrice }}</p>
</template>
```

### 3. **Avoid Side Effects**
```vue
<script setup>
const count = ref(0)

// ❌ Jangan panggil fungsi dengan side effects di template
function increment() {
  count.value++  // Side effect!
}
</script>

<template>
  <!-- ❌ Jangan lakukan ini -->
  <p>{{ increment() }}</p>

  <!-- ✅ Gunakan event handler -->
  <button @click="increment">Increment</button>
  <p>{{ count }}</p>
</template>
```

## 🛠️ Cara Menjalankan Proyek

```bash
# Masuk ke folder proyek
cd "04. Text Interpolation"

# Install dependencies
npm install

# Jalankan development server
npm run dev
```

## 🎪 Latihan Praktis

### Latihan 1: Personal Info Card
Buat komponen yang menampilkan informasi personal:

```vue
<script setup>
const firstName = ref('John')
const lastName = ref('Doe')
const age = ref(25)
const hobby = ref('Programming'
</script>

<template>
  <div class="profile-card">
    <h2>{{ firstName + ' ' + lastName }}</h2>
    <p>Age: {{ age }} years old</p>
    <p>Status: {{ age >= 18 ? 'Adult' : 'Minor' }}</p>
    <p>Hobby: {{ hobby.toUpperCase() }}</p>
  </div>
</template>
```

### Latihan 2: Shopping Cart
Buat kalkulasi shopping cart sederhana:

```vue
<script setup>
const items = ref([
  { name: 'Book', price: 50000, quantity: 2 },
  { name: 'Pen', price: 10000, quantity: 5 }
])

const subtotal = computed(() =>
  items.value.reduce((total, item) => total + (item.price * item.quantity), 0)
)

const tax = computed(() => subtotal.value * 0.1)
const total = computed(() => subtotal.value + tax.value)
</script>

<template>
  <div class="cart">
    <h3>Shopping Cart</h3>
    <p>Subtotal: {{ subtotal }}</p>
    <p>Tax (10%): {{ tax }}</p>
    <p><strong>Total: {{ total }}</strong></p>
  </div>
</template>
```

## 🚀 Lanjutan

Setelah memahami text interpolation, Anda bisa melanjutkan ke:

1. **Directive Binding** - `v-bind`, `v-model`, `v-if`, dll.
2. **Event Handling** - `@click`, `@submit`, dll.
3. **List Rendering** - `v-for` untuk menampilkan array
4. **Component Props** - Mengirim data ke komponen lain
5. **State Management** - Pinia untuk data global

## 🔗 Referensi

- [Vue.js Documentation - Text Interpolation](https://vuejs.org/guide/essentials/template-syntax.html#text-interpolation)
- [JavaScript Expressions Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)

---

## 🎉 Kesimpulan

Text Interpolation adalah fitur fundamental Vue.js yang memungkinkan Anda:

✅ Menampilkan data dinamis di template
✅ Menggunakan JavaScript expressions
✅ Memanggil fungsi dan menghitung nilai
✅ Membuat tampilan yang reaktif dan interaktif

**Ingat:** `{{ expression }}` adalah jendela Anda ke dunia JavaScript dalam template Vue.js. Gunakan dengan bijak untuk membuat aplikasi yang dinamis dan menarik!

*Happy coding with Vue.js! 🚀*