# 📚 Implementasi Step-by-Step Calculator Vue.js

## 🎯 Panduan Detail Membuat Calculator dari Nol

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
npm create vue@latest calculator-app

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
cd calculator-app

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
calculator-app/
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
    <h1>🧮 Calculator</h1>
    <p>Membuat kalkulator dengan Vue.js</p>
  </div>
</template>

<script setup>
// Kosongkan script untuk sementara
</script>

<style>
/* Kosongkan style untuk sementara */
</style>
```

**Result:** Aplikasi sekarang menampilkan judul "Calculator"

---

### 📋 Step 3: Implementasi Dasar Display

#### 3.1 Tambahkan Reactive Display
**Edit `src/App.vue` - Script section:**
```vue
<script setup>
import { ref } from "vue";

// 1. State untuk display kalkulator
const display = ref("0");

// 2. Debug info
const debugInfo = ref({
  displayLength: 0,
  lastAction: "none"
});
</script>
```

#### 3.2 Tampilkan Display di UI
**Edit `src/App.vue` - Template section:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <!-- Calculator Display -->
    <div class="calculator">
      <div class="display-container">
        <input
          v-model="display"
          readonly
          class="calculator-display"
        />
      </div>

      <!-- Debug Info -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Type: {{ typeof display }}</p>
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
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

#app {
  text-align: center;
}

h1 {
  margin-bottom: 30px;
  color: #333;
}

.calculator {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

.display-container {
  margin-bottom: 20px;
}

.calculator-display {
  width: 100%;
  padding: 20px;
  font-size: 2em;
  text-align: right;
  border: 2px solid #ddd;
  border-radius: 5px;
  background: #f9f9f9;
}

.debug-section {
  background: #e8f4f8;
  padding: 15px;
  border-radius: 5px;
  text-align: left;
  font-size: 0.9em;
}

.debug-section h3 {
  margin-bottom: 10px;
  color: #2c3e50;
}
</style>
```

**Result:** Sekarang Anda bisa melihat display calculator dan debug info!

---

### 📋 Step 4: Implementasi Tombol Angka

#### 4.1 Tambah Fungsi Input Angka
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
import { ref } from "vue";

const display = ref("0");

// Fungsi untuk menambah input ke display
const appendToDisplay = (value) => {
  // Debug: Log setiap input
  console.log("Input received:", value);
  console.log("Current display:", display.value);

  if (display.value === "0" && value !== ".") {
    // Jika display = "0" dan input bukan ".", ganti dengan input baru
    display.value = value;
    console.log("Replaced 0 with:", value);
  } else {
    // Tambahkan input ke display yang ada
    display.value += value;
    console.log("Appended to display:", display.value);
  }
};
</script>
```

#### 4.2 Tambah Tombol Angka
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <div class="calculator">
      <!-- Display -->
      <div class="display-container">
        <input
          v-model="display"
          readonly
          class="calculator-display"
        />
      </div>

      <!-- Number Buttons -->
      <div class="buttons">
        <!-- Row 1 -->
        <div class="button-row">
          <button @click="appendToDisplay('7')">7</button>
          <button @click="appendToDisplay('8')">8</button>
          <button @click="appendToDisplay('9')">9</button>
          <button @click="appendToDisplay('/')">÷</button>
        </div>

        <!-- Row 2 -->
        <div class="button-row">
          <button @click="appendToDisplay('4')">4</button>
          <button @click="appendToDisplay('5')">5</button>
          <button @click="appendToDisplay('6')">6</button>
          <button @click="appendToDisplay('*')">×</button>
        </div>

        <!-- Row 3 -->
        <div class="button-row">
          <button @click="appendToDisplay('1')">1</button>
          <button @click="appendToDisplay('2')">2</button>
          <button @click="appendToDisplay('3')">3</button>
          <button @click="appendToDisplay('-')">−</button>
        </div>

        <!-- Row 4 -->
        <div class="button-row">
          <button @click="appendToDisplay('0')">0</button>
          <button @click="appendToDisplay('.')">.</button>
          <button @click="appendToDisplay('+')">+</button>
        </div>
      </div>

      <!-- Debug Section -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Test: Click some buttons!</p>
      </div>
    </div>
  </div>
</template>
```

#### 4.3 Tambah Styling untuk Tombol
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.buttons {
  margin-bottom: 20px;
}

.button-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

button {
  flex: 1;
  padding: 20px;
  font-size: 1.2em;
  border: 1px solid #ddd;
  background: #f9f9f9;
  border-radius: 5px;
  cursor: pointer;
  transition: background 0.2s;
}

button:hover {
  background: #e9e9e9;
}

button:active {
  background: #d9d9d9;
}
</style>
```

**Result:** Sekarang tombol angka berfungsi dan bisa menginput ke display!

---

### 📋 Step 5: Implementasi Fungsi Hitung

#### 5.1 Tambah Fungsi Calculate
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
import { ref } from "vue";

const display = ref("0");

const appendToDisplay = (value) => {
  // ... (function sebelumnya)
};

// Fungsi untuk menghitung hasil
const calculate = () => {
  try {
    console.log("Calculating:", display.value);

    // Gunakan eval() untuk menghitung ekspresi matematika
    const result = eval(display.value);

    console.log("Result:", result);

    // Update display dengan hasil
    display.value = result.toString();

    console.log("Updated display:", display.value);

  } catch (error) {
    // Handle error untuk invalid mathematical expressions
    console.error("Calculation error:", error);
    display.value = "Error";
  }
};
</script>
```

#### 5.2 Tambah Tombol Equals dan Clear
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <div class="calculator">
      <!-- Display -->
      <div class="display-container">
        <input
          v-model="display"
          readonly
          class="calculator-display"
        />
      </div>

      <!-- Buttons -->
      <div class="buttons">
        <!-- Row 1 -->
        <div class="button-row">
          <button @click="appendToDisplay('7')">7</button>
          <button @click="appendToDisplay('8')">8</button>
          <button @click="appendToDisplay('9')">9</button>
          <button @click="appendToDisplay('/')">÷</button>
        </div>

        <!-- Row 2 -->
        <div class="button-row">
          <button @click="appendToDisplay('4')">4</button>
          <button @click="appendToDisplay('5')">5</button>
          <button @click="appendToDisplay('6')">6</button>
          <button @click="appendToDisplay('*')">×</button>
        </div>

        <!-- Row 3 -->
        <div class="button-row">
          <button @click="appendToDisplay('1')">1</button>
          <button @click="appendToDisplay('2')">2</button>
          <button @click="appendToDisplay('3')">3</button>
          <button @click="appendToDisplay('-')">−</button>
        </div>

        <!-- Row 4 -->
        <div class="button-row">
          <button @click="appendToDisplay('0')">0</button>
          <button @click="appendToDisplay('.')">.</button>
          <button @click="calculate" class="equals-button">=</button>
          <button @click="appendToDisplay('+')">+</button>
        </div>
      </div>

      <!-- Clear Button -->
      <button @click="display = '0'" class="clear-button">
        Clear
      </button>

      <!-- Debug Section -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Try: 2 + 3 = 5</p>
      </div>
    </div>
  </div>
</template>
```

#### 5.3 Tambah Styling untuk Tombol Spesial
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.equals-button {
  background: #4CAF50;
  color: white;
  font-weight: bold;
}

.equals-button:hover {
  background: #45a049;
}

.clear-button {
  width: 100%;
  padding: 15px;
  background: #f44336;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1.1em;
  margin-bottom: 20px;
}

.clear-button:hover {
  background: #da190b;
}
</style>
```

**Result:** Kalkulator sekarang bisa melakukan perhitungan dasar!

---

### 📋 Step 6: Implementasi Dynamic Display Sizing

#### 6.1 Tambah Computed Property
**Edit `src/App.vue` - Import dan tambahkan computed:**
```vue
<script setup>
import { ref, computed } from "vue"; // Tambah computed

const display = ref("0");

// ... (functions sebelumnya)

// Computed property untuk dynamic class binding
const displayClass = computed(() => {
  const length = display.value.length;

  console.log("Display length:", length);

  if (length > 15) {
    return "extra-small";
  } else if (length > 12) {
    return "small-text";
  } else if (length > 8) {
    return "medium-text";
  }

  return "";
});
</script>
```

#### 6.2 Update Template dengan Dynamic Class
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <div class="calculator">
      <!-- Display dengan dynamic class -->
      <div class="display-container">
        <input
          v-model="display"
          :class="displayClass"
          readonly
          class="calculator-display"
        />
        <div class="debug-info">
          Class: {{ displayClass || "default" }}
        </div>
      </div>

      <!-- ... (buttons tetap sama) -->

      <!-- Debug Section -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Class: {{ displayClass || "default" }}</p>
        <p>Test: Enter long numbers!</p>
      </div>
    </div>
  </div>
</template>
```

#### 6.3 Tambah CSS Classes
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.calculator-display {
  /* ... (existing styles) */
  transition: font-size 0.3s ease;
}

.calculator-display.medium-text {
  font-size: 1.5em;
}

.calculator-display.small-text {
  font-size: 1.2em;
}

.calculator-display.extra-small {
  font-size: 1em;
}

.debug-info {
  font-size: 0.8em;
  color: #666;
  margin-top: 5px;
}
</style>
```

**Result:** Font size display otomatis menyesuaikan dengan panjang angka!

---

### 📋 Step 7: Implementasi Clear Functions

#### 7.1 Tambah Clear Functions
**Edit `src/App.vue` - Tambah functions:**
```vue
<script setup>
import { ref, computed } from "vue";

const display = ref("0");

// ... (functions sebelumnya)

// Clear display (reset ke 0)
const clearDisplay = () => {
  console.log("Clearing display");
  display.value = "0";
};

// Clear all (reset everything)
const clearAll = () => {
  console.log("Clearing all");
  display.value = "0";
  // Tambahkan state lain jika ada
};

// Delete last digit (backspace)
const deleteLast = () => {
  console.log("Deleting last digit");
  if (display.value.length > 1) {
    display.value = display.value.slice(0, -1);
  } else {
    display.value = "0";
  }
};
</script>
```

#### 7.2 Update Tombol Clear
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <div class="calculator">
      <!-- Display -->
      <div class="display-container">
        <input
          v-model="display"
          :class="displayClass"
          readonly
          class="calculator-display"
        />
      </div>

      <!-- Function Buttons Row -->
      <div class="function-row">
        <button @click="clearAll" class="function-btn">AC</button>
        <button @click="deleteLast" class="function-btn">⌫</button>
        <button @click="calculate" class="equals-button">=</button>
      </div>

      <!-- Number Buttons -->
      <div class="buttons">
        <!-- ... (number buttons tetap sama) -->
      </div>

      <!-- Debug Section -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Test clear functions!</p>
      </div>
    </div>
  </div>
</template>
```

#### 7.3 Tambah Styling Function Buttons
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.function-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.function-btn {
  flex: 1;
  padding: 15px;
  background: #FF9800;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.function-btn:hover {
  background: #F57C00;
}

.equals-button {
  flex: 1;
  background: #4CAF50;
  color: white;
  font-weight: bold;
}

.equals-button:hover {
  background: #45a049;
}
</style>
```

**Result:** Kalkulator sekarang memiliki multiple clear functions!

---

### 📋 Step 8: Advanced Styling & Animations

#### 8.1 Implementasi Modern Design
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
  display: flex;
  justify-content: center;
  align-items: center;
}

#app {
  text-align: center;
}

h1 {
  margin-bottom: 30px;
  color: white;
  font-size: 3em;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.calculator {
  max-width: 400px;
  margin: 0 auto;
  padding: 30px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
  border: 1px solid rgba(255,255,255,0.2);
}

.display-container {
  margin-bottom: 20px;
}

.calculator-display {
  width: 100%;
  padding: 20px;
  font-size: 2em;
  text-align: right;
  border: none;
  border-radius: 10px;
  background: rgba(0,0,0,0.3);
  color: white;
  font-family: 'Courier New', monospace;
  transition: all 0.3s ease;
}

.calculator-display:focus {
  outline: none;
  box-shadow: 0 0 15px rgba(255,255,255,0.3);
}

.function-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.function-btn, .equals-button {
  flex: 1;
  padding: 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
  color: white;
}

.function-btn {
  background: #FF9800;
}

.function-btn:hover {
  background: #F57C00;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

.equals-button {
  background: #4CAF50;
}

.equals-button:hover {
  background: #45a049;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

.buttons {
  margin-bottom: 20px;
}

.button-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

button {
  flex: 1;
  padding: 20px;
  font-size: 1.2em;
  border: none;
  border-radius: 10px;
  background: rgba(255,255,255,0.2);
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(5px);
}

button:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

button:active {
  transform: translateY(0);
}

.debug-section {
  background: rgba(255,255,255,0.1);
  padding: 15px;
  border-radius: 10px;
  text-align: left;
  font-size: 0.9em;
  color: white;
}

.debug-section h3 {
  margin-bottom: 10px;
  color: white;
}
</style>
```

**Result:** Kalkulator sekarang memiliki modern glassmorphism design!

---

### 📋 Step 9: Error Handling & Validation

#### 9.1 Implementasi Error Handling
**Edit `src/App.vue` - Update functions:**
```vue
<script setup>
import { ref, computed } from "vue";

const display = ref("0");
const lastError = ref("");

const appendToDisplay = (value) => {
  try {
    // Clear error jika ada
    if (display.value === "Error") {
      display.value = "0";
    }

    // Validate input
    if (value === '.') {
      // Cek apakah sudah ada titik desimal
      const parts = display.value.split(/[+\-*/]/);
      const lastNumber = parts[parts.length - 1];
      if (lastNumber.includes('.')) {
        return; // Jangan tambahkan titik jika sudah ada
      }
    }

    // Validate operator
    if (['+', '-', '*', '/'].includes(value)) {
      const lastChar = display.value[display.value.length - 1];
      if (['+', '-', '*', '/'].includes(lastChar)) {
        // Ganti operator terakhir
        display.value = display.value.slice(0, -1) + value;
        return;
      }
    }

    if (display.value === "0" && value !== ".") {
      display.value = value;
    } else {
      display.value += value;
    }

    lastError.value = ""; // Clear error

  } catch (error) {
    lastError.value = "Input error: " + error.message;
    console.error("Input error:", error);
  }
};

const calculate = () => {
  try {
    console.log("Calculating:", display.value);

    // Validate expression
    if (!display.value || display.value === "0") {
      throw new Error("No expression to calculate");
    }

    // Check for invalid characters
    if (!/^[0-9+\-*/.() ]+$/.test(display.value)) {
      throw new Error("Invalid characters in expression");
    }

    // Check for division by zero
    if (display.value.includes('/0')) {
      throw new Error("Division by zero");
    }

    const result = eval(display.value);

    // Handle infinite or NaN results
    if (!isFinite(result)) {
      throw new Error("Invalid result");
    }

    display.value = result.toString();
    lastError.value = "";

    console.log("Calculation successful:", result);

  } catch (error) {
    console.error("Calculation error:", error);
    display.value = "Error";
    lastError.value = error.message;
  }
};

const clearDisplay = () => {
  display.value = "0";
  lastError.value = "";
};

const clearAll = () => {
  display.value = "0";
  lastError.value = "";
};

const deleteLast = () => {
  if (display.value === "Error") {
    display.value = "0";
    lastError.value = "";
    return;
  }

  if (display.value.length > 1) {
    display.value = display.value.slice(0, -1);
  } else {
    display.value = "0";
  }
  lastError.value = "";
};
</script>
```

#### 9.2 Tambah Error Display
**Edit `src/App.vue` - Template:**
```vue
<template>
  <div id="app">
    <h1>🧮 Calculator</h1>

    <div class="calculator">
      <!-- Display dengan error indicator -->
      <div class="display-container">
        <input
          v-model="display"
          :class="[displayClass, { error: display === 'Error' }]"
          readonly
          class="calculator-display"
        />
        <div v-if="lastError" class="error-message">
          ⚠️ {{ lastError }}
        </div>
      </div>

      <!-- ... (buttons tetap sama) -->

      <!-- Debug Section dengan error info -->
      <div class="debug-section">
        <h3>Debug Info:</h3>
        <p>Display: {{ display }}</p>
        <p>Length: {{ display.length }}</p>
        <p>Last Error: {{ lastError || 'None' }}</p>
        <p>Test: Try invalid operations!</p>
      </div>
    </div>
  </div>
</template>
```

#### 9.3 Tambah Error Styling
**Edit `src/App.vue` - Tambah CSS:**
```vue
<style>
/* ... (styles sebelumnya tetap sama) */

.calculator-display.error {
  background: rgba(244, 67, 54, 0.3);
  color: #ffcccc;
  animation: shake 0.5s;
}

.error-message {
  color: #ffcccc;
  font-size: 0.9em;
  margin-top: 5px;
  min-height: 20px;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}
</style>
```

**Result:** Kalkulator sekarang memiliki robust error handling!

---

### 📋 Step 10: Refactor ke Component Architecture

#### 10.1 Buat Component CalculatorDisplay
**Buat file `src/components/CalculatorDisplay.vue`:**
```vue
<template>
  <div class="display-container">
    <input
      :value="modelValue"
      :class="[displayClass, { error: modelValue === 'Error' }]"
      readonly
      class="calculator-display"
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <div v-if="error" class="error-message">
      ⚠️ {{ error }}
    </div>
  </div>
</template>

<script setup>
// Props
defineProps({
  modelValue: {
    type: String,
    required: true
  },
  displayClass: {
    type: String,
    default: ""
  },
  error: {
    type: String,
    default: ""
  }
});

// Events
defineEmits(['update:modelValue']);
</script>

<style scoped>
.display-container {
  margin-bottom: 20px;
}

.calculator-display {
  width: 100%;
  padding: 20px;
  font-size: 2em;
  text-align: right;
  border: none;
  border-radius: 10px;
  background: rgba(0,0,0,0.3);
  color: white;
  font-family: 'Courier New', monospace;
  transition: all 0.3s ease;
}

.calculator-display:focus {
  outline: none;
  box-shadow: 0 0 15px rgba(255,255,255,0.3);
}

.calculator-display.error {
  background: rgba(244, 67, 54, 0.3);
  color: #ffcccc;
  animation: shake 0.5s;
}

.error-message {
  color: #ffcccc;
  font-size: 0.9em;
  margin-top: 5px;
  min-height: 20px;
}

@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-5px); }
  75% { transform: translateX(5px); }
}
</style>
```

#### 10.2 Buat Component CalculatorButtons
**Buat file `src/components/CalculatorButtons.vue`:**
```vue
<template>
  <div class="calculator-buttons">
    <!-- Function Row -->
    <div class="function-row">
      <button @click="$emit('clear-all')" class="function-btn">AC</button>
      <button @click="$emit('delete-last')" class="function-btn">⌫</button>
      <button @click="$emit('calculate')" class="equals-button">=</button>
    </div>

    <!-- Number Buttons -->
    <div class="buttons">
      <!-- Row 1 -->
      <div class="button-row">
        <button @click="$emit('input', '7')" class="number">7</button>
        <button @click="$emit('input', '8')" class="number">8</button>
        <button @click="$emit('input', '9')" class="number">9</button>
        <button @click="$emit('input', '/')" class="operator">÷</button>
      </div>

      <!-- Row 2 -->
      <div class="button-row">
        <button @click="$emit('input', '4')" class="number">4</button>
        <button @click="$emit('input', '5')" class="number">5</button>
        <button @click="$emit('input', '6')" class="number">6</button>
        <button @click="$emit('input', '*')" class="operator">×</button>
      </div>

      <!-- Row 3 -->
      <div class="button-row">
        <button @click="$emit('input', '1')" class="number">1</button>
        <button @click="$emit('input', '2')" class="number">2</button>
        <button @click="$emit('input', '3')" class="number">3</button>
        <button @click="$emit('input', '-')" class="operator">−</button>
      </div>

      <!-- Row 4 -->
      <div class="button-row">
        <button @click="$emit('input', '0')" class="number zero">0</button>
        <button @click="$emit('input', '.')" class="decimal">.</button>
        <button @click="$emit('input', '+')" class="operator">+</button>
      </div>
    </div>
  </div>
</template>

<script setup>
// Events
defineEmits([
  'input',
  'calculate',
  'clear-all',
  'delete-last'
]);
</script>

<style scoped>
.function-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

.function-btn, .equals-button {
  flex: 1;
  padding: 20px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s ease;
  color: white;
}

.function-btn {
  background: #FF9800;
}

.function-btn:hover {
  background: #F57C00;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

.equals-button {
  background: #4CAF50;
}

.equals-button:hover {
  background: #45a049;
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

.buttons {
  margin-bottom: 20px;
}

.button-row {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

button {
  flex: 1;
  padding: 20px;
  font-size: 1.2em;
  border: none;
  border-radius: 10px;
  background: rgba(255,255,255,0.2);
  color: white;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(5px);
}

button:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

button:active {
  transform: translateY(0);
}

.zero {
  flex: 2;
}

.number:hover {
  background: rgba(255,255,255,0.25);
}

.operator:hover {
  background: rgba(76, 175, 80, 0.3);
}

.decimal:hover {
  background: rgba(255,255,255,0.25);
}
</style>
```

#### 10.3 Update App.vue (Main Component)
**Edit `src/App.vue` menjadi:**
```vue
<script setup>
import { ref, computed } from "vue";
import CalculatorDisplay from './components/CalculatorDisplay.vue';
import CalculatorButtons from './components/CalculatorButtons.vue';

// Reactive data
const display = ref("0");
const lastError = ref("");

// Computed property
const displayClass = computed(() => {
  const length = display.value.length;
  if (length > 15) return "extra-small";
  if (length > 12) return "small-text";
  if (length > 8) return "medium-text";
  return "";
});

// Methods
const appendToDisplay = (value) => {
  // ... (function dari step sebelumnya)
};

const calculate = () => {
  // ... (function dari step sebelumnya)
};

const clearDisplay = () => {
  display.value = "0";
  lastError.value = "";
};

const clearAll = () => {
  display.value = "0";
  lastError.value = "";
};

const deleteLast = () => {
  // ... (function dari step sebelumnya)
};
</script>

<template>
  <div id="app">
    <header>
      <h1>🧮 Vue Calculator</h1>
      <p>Modern Calculator with Component Architecture</p>
    </header>

    <main>
      <div class="calculator">
        <CalculatorDisplay
          v-model="display"
          :display-class="displayClass"
          :error="lastError"
        />

        <CalculatorButtons
          @input="appendToDisplay"
          @calculate="calculate"
          @clear-all="clearAll"
          @delete-last="deleteLast"
        />

        <div class="debug-section">
          <h3>Component Architecture:</h3>
          <p>Display Component: ✅</p>
          <p>Buttons Component: ✅</p>
          <p>State Management: ✅</p>
          <p>Events Communication: ✅</p>
        </div>
      </div>
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
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
}

#app {
  max-width: 500px;
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

.calculator {
  width: 100%;
  max-width: 400px;
  padding: 30px;
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
  border: 1px solid rgba(255,255,255,0.2);
}

.debug-section {
  background: rgba(255,255,255,0.1);
  padding: 15px;
  border-radius: 10px;
  text-align: left;
  font-size: 0.9em;
  margin-top: 20px;
}

.debug-section h3 {
  margin-bottom: 10px;
  color: white;
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

**Result:** Aplikasi sekarang menggunakan component-based architecture yang modular!

---

### 📋 Step 11: Final Testing & Deployment

#### 11.1 Test Semua Fitur
**Coba semua fungsi:**
- ✅ Input angka 0-9
- ✅ Operasi dasar (+, -, *, /)
- ✅ Decimal point handling
- ✅ Dynamic display sizing
- ✅ Error handling (division by zero, invalid expressions)
- ✅ Clear functions (AC, backspace)
- ✅ Component communication
- ✅ Responsive design

#### 11.2 Debug Issues
**Common issues & fixes:**
```vue
<!-- Issue: Component tidak menerima props -->
<!-- Fix: Pastikan props definition benar -->
defineProps({
  modelValue: { type: String, required: true }
});

<!-- Issue: Events tidak diterima -->
<!-- Fix: Pastikan emit definition benar -->
defineEmits(['input', 'calculate']);

<!-- Issue: Reactive data tidak update -->
<!-- Fix: Pastikan ref() dan computed() benar -->
const display = ref("0");
const displayClass = computed(() => { /* logic */ });
```

#### 11.3 Build untuk Production
```bash
# Build aplikasi
npm run build

# Preview build
npm run preview

# Hasilnya ada di folder /dist
```

#### 11.4 Deployment
```bash
# 1. Push ke GitHub
git add .
git commit -m "Complete Vue Calculator with Component Architecture"
git push origin main

# 2. Deploy ke Netlify/Vercel
# - Connect GitHub repository
# - Auto deploy dari main branch
# - Settings: Build command = npm run build
# - Settings: Publish directory = dist
```

---

## 🎉 Hasil Akhir

✅ **Aplikasi Calculator Lengkap dengan:**
- Basic arithmetic operations (+, -, *, /)
- Dynamic display sizing
- Advanced error handling
- Multiple clear functions
- Component-based architecture
- Modern glassmorphism design
- Responsive layout
- Production-ready code

## 📚 Yang Telah Dipelajari

1. **Vue.js Fundamentals**
   - Composition API (`<script setup>`)
   - Reactive data (`ref()`)
   - Computed properties
   - Props & Events system
   - Component architecture

2. **JavaScript Concepts**
   - Mathematical expressions with `eval()`
   - String manipulation
   - Error handling dengan try-catch
   - Event handling patterns
   - Data validation

3. **Advanced Concepts**
   - Component communication
   - State management
   - Dynamic class binding
   - Form input handling
   - CSS animations

## 🚀 Next Steps

1. **Advanced Features** - Memory functions, scientific operations
2. **Keyboard Support** - Full keyboard integration
3. **History Feature** - Calculation history
4. **Themes System** - Multiple color schemes
5. **Mobile App** - Convert to mobile application
6. **API Integration** - Currency converter, unit converter
7. **Graphing Calculator** - Mathematical function plotting

---

**🎯 Mission Accomplished!**
Anda berhasil membuat aplikasi Calculator yang fully functional dengan Vue.js 3 dan component-based architecture!

**💡 Pro Tip:** Lanjutkan dengan menambah advanced features atau mulai project Vue.js lainnya!

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) | [MDN Web Docs](https://developer.mozilla.org/)