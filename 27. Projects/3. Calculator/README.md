# Calculator - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Calculator adalah aplikasi web kalkulator sederhana yang dibuat menggunakan Vue.js 3 dengan Composition API. Aplikasi ini meniru fungsi kalkulator standar dengan operasi aritmatika dasar (penjumlahan, pengurangan, perkalian, pembagian) dan fitur-fitur tambahan seperti display yang dinamis dan error handling.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- Computed properties untuk dynamic class binding
- Event handling yang kompleks
- Form input management
- String manipulation dan eval() function
- Conditional rendering dan class binding
- Grid layout dengan CSS
- Error handling dalam aplikasi Vue.js

## 📋 Fitur Aplikasi

1. **Operasi Aritmatika Dasar**: +, -, ×, ÷
2. **Input Numbers**: 0-9 dan decimal point (.)
3. **Dynamic Display**: Font size menyesuaikan panjang angka
4. **Clear Function**: Reset kalkulator
5. **Error Handling**: Menampilkan "Error" untuk invalid operations
6. **Real-time Update**: Display langsung update saat input
7. **Responsive Design**: Layout yang rapi dan konsisten

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)

## 📂 Struktur Project

```
3. Calculator/
├── App.vue                              # Komponen utama aplikasi
├── components/
│   └── AmazingCalculator.vue            # Komponen untuk kalkulator
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
npm create vue@latest calculator-app
cd calculator-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create calculator-app
cd calculator-app
```

### Langkah 3: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style>`**: Bagian CSS untuk styling

### Langkah 4: Membuat Komponen AmazingCalculator

**File: `components/AmazingCalculator.vue`**

#### Bagian Script Setup
```vue
<script setup>
import { ref, computed } from "vue";

// Display value - current calculation state
const display = ref("0");

// Fungsi untuk menambah input ke display
const appendToDisplay = (value) => {
  if (display.value === "0" && value !== ".") {
    // Jika display = 0 dan input bukan ".", ganti dengan input baru
    display.value = value;
  } else {
    // Tambahkan input ke display yang ada
    display.value += value;
  }
};

// Fungsi untuk melakukan perhitungan
const calculate = () => {
  try {
    // Gunakan eval() untuk menghitung ekspresi matematika
    display.value = eval(display.value).toString();
  } catch (error) {
    // Handle error untuk invalid mathematical expressions
    display.value = "Error";
  }
};

// Computed property untuk dynamic class binding
const displayClass = computed(() => {
  // Kembalikan class 'small-text' jika display terlalu panjang
  return display.value.length > 12 ? "small-text" : "";
});

// Fungsi untuk mereset kalkulator
const clearDisplay = () => {
  display.value = "0";
};
</script>
```

#### Penjelasan Kode:

1. **`import { ref, computed } from "vue"`**
   - Mengimpor `ref` untuk membuat reactive variables
   - Mengimpor `computed` untuk computed properties

2. **`const display = ref("0")`**
   - Reactive variable untuk menyimpan nilai display
   - Nilai awalnya adalah "0"

3. **`appendToDisplay(value)` Function**
   - Menambahkan input (angka atau operator) ke display
   - Menghandle special case untuk angka 0
   - Menghandle input decimal point (.)

4. **`calculate()` Function**
   - Menggunakan `eval()` untuk menghitung ekspresi matematika
   - Menggunakan try-catch untuk error handling
   - Mengconvert hasil ke string untuk display

5. **`displayClass` Computed Property**
   - Menghitung class CSS secara dinamis
   - Mengurangi font size jika display terlalu panjang

6. **`clearDisplay()` Function**
   - Reset display ke nilai awal "0"

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="calculator-container">
    <h1>🧮 Amazing Calculator</h1>

    <div class="calculator">
      <!-- Display Input -->
      <input
        v-model="display"
        :class="displayClass"
        readonly
        class="calculator-display"
      />

      <!-- Number and Operator Buttons -->
      <div class="buttons">
        <!-- Row 1 -->
        <button @click="appendToDisplay('7')" class="number">7</button>
        <button @click="appendToDisplay('8')" class="number">8</button>
        <button @click="appendToDisplay('9')" class="number">9</button>
        <button @click="appendToDisplay('/')" class="operator">÷</button>

        <!-- Row 2 -->
        <button @click="appendToDisplay('4')" class="number">4</button>
        <button @click="appendToDisplay('5')" class="number">5</button>
        <button @click="appendToDisplay('6')" class="number">6</button>
        <button @click="appendToDisplay('*')" class="operator">×</button>

        <!-- Row 3 -->
        <button @click="appendToDisplay('1')" class="number">1</button>
        <button @click="appendToDisplay('2')" class="number">2</button>
        <button @click="appendToDisplay('3')" class="number">3</button>
        <button @click="appendToDisplay('-')" class="operator">−</button>

        <!-- Row 4 -->
        <button @click="appendToDisplay('0')" class="number">0</button>
        <button @click="appendToDisplay('.')" class="decimal">.</button>
        <button @click="calculate()" class="equals">=</button>
        <button @click="appendToDisplay('+')" class="operator">+</button>
      </div>

      <!-- Clear Button -->
      <button @click="clearDisplay" class="clear-button">C</button>
    </div>

    <!-- Instructions -->
    <div class="instructions">
      <h3>Cara Penggunaan:</h3>
      <ul>
        <li>Klik tombol angka untuk memasukkan bilangan</li>
        <li>Klik operator (+, -, ×, ÷) untuk operasi matematika</li>
        <li>Klik "=" untuk menghitung hasil</li>
        <li>Klik "C" untuk menghapus semua</li>
      </ul>
    </div>
  </div>
</template>
```

#### Penjelasan Template:

1. **`v-model="display"`**
   - Two-way data binding dengan input display
   - Display otomatis update saat nilai berubah

2. **`:class="displayClass"`**
   - Dynamic class binding
   - Class dihitung dari computed property

3. **`readonly`**
   - Input tidak bisa diedit langsung oleh user
   - Hanya bisa diupdate melalui tombol

4. **`@click="functionName(parameter)"`**
   - Event listener untuk setiap tombol
   - Memanggil fungsi dengan parameter yang sesuai

5. **CSS Classes**
   - `number` untuk tombol angka
   - `operator` untuk tombol operator
   - `decimal` untuk titik desimal
   - `equals` untuk tombol sama dengan
   - `clear-button` untuk tombol clear

### Langkah 6: Styling dengan CSS

```vue
<style>
/* Base Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.calculator-container {
  text-align: center;
  color: white;
}

h1 {
  font-size: 3em;
  margin-bottom: 30px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.calculator {
  max-width: 350px;
  margin: 0 auto;
  padding: 30px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
  border: 1px solid rgba(255,255,255,0.2);
}

.calculator-display {
  width: 100%;
  padding: 20px;
  margin-bottom: 20px;
  font-size: 2em;
  text-align: right;
  background: rgba(0,0,0,0.3);
  border: none;
  border-radius: 10px;
  color: white;
  font-family: 'Courier New', monospace;
  transition: all 0.3s ease;
}

.calculator-display.small-text {
  font-size: 1.5em;
}

.calculator-display:focus {
  outline: none;
  box-shadow: 0 0 10px rgba(255,255,255,0.3);
}

.buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  margin-bottom: 15px;
}

button {
  padding: 20px;
  font-size: 1.3em;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

button:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

/* Button Styles */
.number {
  background: rgba(255,255,255,0.2);
  color: white;
}

.number:hover {
  background: rgba(255,255,255,0.3);
}

.operator {
  background: #4CAF50;
  color: white;
}

.operator:hover {
  background: #45a049;
}

.decimal {
  background: rgba(255,255,255,0.2);
  color: white;
}

.decimal:hover {
  background: rgba(255,255,255,0.3);
}

.equals {
  background: #2196F3;
  color: white;
}

.equals:hover {
  background: #1976D2;
}

.clear-button {
  width: 100%;
  padding: 15px;
  font-size: 1.2em;
  background: #f44336;
  color: white;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}

.clear-button:hover {
  background: #da190b;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

.instructions {
  margin-top: 30px;
  padding: 20px;
  background: rgba(255,255,255,0.1);
  border-radius: 10px;
  max-width: 350px;
  margin-left: auto;
  margin-right: auto;
}

.instructions h3 {
  margin-bottom: 15px;
  color: white;
}

.instructions ul {
  list-style: none;
  text-align: left;
}

.instructions li {
  margin: 8px 0;
  padding-left: 20px;
  position: relative;
}

.instructions li:before {
  content: "▸";
  position: absolute;
  left: 0;
  color: #4CAF50;
}

/* Responsive Design */
@media (max-width: 480px) {
  .calculator {
    max-width: 90%;
    padding: 20px;
  }

  .calculator-display {
    font-size: 1.5em;
    padding: 15px;
  }

  .calculator-display.small-text {
    font-size: 1.2em;
  }

  button {
    padding: 15px;
    font-size: 1.1em;
  }

  h1 {
    font-size: 2em;
  }
}
</style>
```

#### Penjelasan CSS:

1. **Modern Glassmorphism Design**
   - `backdrop-filter: blur()` untuk efek kaca
   - `rgba()` untuk transparansi
   - Gradient background untuk visual appeal

2. **Grid Layout**
   - `display: grid` untuk layout tombol
   - `grid-template-columns: repeat(4, 1fr)` untuk 4 kolom
   - `gap` untuk spacing antar tombol

3. **Interactive Buttons**
   - `:hover` untuk efek hover
   - `:active` untuk efek klik
   - `transform` untuk animasi smooth

4. **Responsive Design**
   - Media queries untuk mobile devices
   - Font size yang menyesuaikan
   - Layout yang fleksibel

### Langkah 7: Membuat App.vue

**File: `App.vue`**
```vue
<script setup>
import AmazingCalculator from './components/AmazingCalculator.vue'
</script>

<template>
  <div id="app">
    <header>
      <h1>🚀 Vue.js Projects</h1>
      <p>Project 3: Calculator Application</p>
    </header>

    <main>
      <AmazingCalculator />
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js 3</p>
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
  background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
  min-height: 100vh;
  color: white;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
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
  align-items: center;
  justify-content: center;
}

footer {
  text-align: center;
  margin-top: 40px;
  opacity: 0.7;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
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
   - Aplikasi Calculator siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API
- Menggunakan `<script setup>` untuk syntax yang lebih ringkas
- Organisasi logika yang lebih modular

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- String manipulation untuk display management

### 3. Computed Properties
- Menghitung nilai secara dinamis berdasarkan data lain
- Dynamic class binding untuk styling yang responsif

### 4. Event Handling
- Multiple event listeners untuk berbagai tombol
- Parameter passing dalam event handlers

### 5. Template Syntax
- `v-model` untuk two-way data binding
- `:class` untuk dynamic class binding
- Event handling dengan `@click`

### 6. JavaScript Integration
- `eval()` function untuk mathematical expressions
- Error handling dengan try-catch
- String manipulation methods

## 🚀 Enhancement Ideas

Setelah menyelesaikan basic calculator, Anda bisa menambahkan fitur:

### 1. Advanced Operations
```vue
<script setup>
const appendToDisplay = (value) => {
  // ... existing logic
};

const calculateSquare = () => {
  try {
    const num = eval(display.value);
    display.value = (num * num).toString();
  } catch (error) {
    display.value = "Error";
  }
};

const calculateSquareRoot = () => {
  try {
    const num = eval(display.value);
    display.value = Math.sqrt(num).toString();
  } catch (error) {
    display.value = "Error";
  }
};
</script>
```

### 2. Memory Functions
```vue
<script setup>
const memory = ref(0);

const memoryStore = () => {
  memory.value = eval(display.value);
};

const memoryRecall = () => {
  display.value = memory.value.toString();
};

const memoryClear = () => {
  memory.value = 0;
};
</script>
```

### 3. History Feature
```vue
<script setup>
const history = ref([]);

const calculate = () => {
  try {
    const expression = display.value;
    const result = eval(expression);

    // Add to history
    history.value.unshift({
      expression: expression,
      result: result.toString()
    });

    display.value = result.toString();
  } catch (error) {
    display.value = "Error";
  }
};
</script>
```

### 4. Keyboard Support
```vue
<script setup>
import { onMounted, onUnmounted } from "vue";

const handleKeyPress = (event) => {
  const key = event.key;

  if (key >= '0' && key <= '9') {
    appendToDisplay(key);
  } else if (key === '.') {
    appendToDisplay('.');
  } else if (key === '+' || key === '-' || key === '*' || key === '/') {
    appendToDisplay(key);
  } else if (key === 'Enter' || key === '=') {
    calculate();
  } else if (key === 'Escape' || key === 'c' || key === 'C') {
    clearDisplay();
  }
};

onMounted(() => {
  window.addEventListener('keydown', handleKeyPress);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyPress);
});
</script>
```

### 5. Scientific Calculator Mode
```vue
<script setup>
const isScientific = ref(false);

const calculateSin = () => {
  try {
    const num = eval(display.value);
    display.value = Math.sin(num * Math.PI / 180).toString();
  } catch (error) {
    display.value = "Error";
  }
};

const calculateCos = () => {
  try {
    const num = eval(display.value);
    display.value = Math.cos(num * Math.PI / 180).toString();
  } catch (error) {
    display.value = "Error";
  }
};
</script>
```

## 🔧 Common Issues & Troubleshooting

### Issue 1: Calculator tidak berfungsi
**Solution**:
- Cek console untuk error
- Pastikan `eval()` function bekerja dengan benar
- Validasi input sebelum menggunakan `eval()`

### Issue 2: Display tidak update
**Solution**:
- Pastikan `ref()` didefinisikan dengan benar
- Cek `v-model` binding
- Verifikasi reactive data update

### Issue 3: Error handling tidak berfungsi
**Solution**:
- Pastikan try-catch block mengelilingi `eval()`
- Tambahkan validation untuk input
- Handle edge cases (division by zero, dll)

### Issue 4: Styling berantakan di mobile
**Solution**:
- Tambahkan media queries
- Gunakan responsive units (%, vw, vh)
- Test di berbagai device sizes

### Issue 5: Performance issues
**Solution**:
- Optimize computed properties
- Hindari unnecessary re-renders
- Gunakan `debounce` untuk input yang complex

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Vue.js Computed Properties Guide](https://vuejs.org/guide/essentials/computed.html)
- [Vite Documentation](https://vitejs.dev/)
- [MDN Web Docs](https://developer.mozilla.org/)
- [CSS Grid Guide](https://css-tricks.com/snippets/css/complete-guide-grid/)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Implementasi komponen AmazingCalculator
- [ ] Tambahkan reactive display
- [ ] Implementasi tombol angka dan operator
- [ ] Tambahkan fungsi calculate
- [ ] Implementasi error handling
- [ ] Tambahkan computed properties
- [ ] Styling aplikasi yang menarik
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Calculator yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:
- Complex state management
- Mathematical operations
- Event handling patterns
- Form input management
- Error handling strategies
- Dynamic styling

---

**Happy Coding! 🚀**