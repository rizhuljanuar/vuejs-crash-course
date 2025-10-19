# 🚀 Panduan Cepat: Calculator Vue.js

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
npm create vue@latest calculator-app

# Masuk ke folder project
cd calculator-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

// Display value - state kalkulator
const display = ref("0");
const memory = ref(0);
const history = ref([]);

// Tambah input ke display
const appendToDisplay = (value) => {
  if (display.value === "0" && value !== ".") {
    display.value = value;
  } else {
    display.value += value;
  }
};

// Hitung hasil
const calculate = () => {
  try {
    const expression = display.value;
    const result = eval(expression);

    // Tambah ke history
    history.value.unshift({
      expression: expression,
      result: result.toString(),
      timestamp: new Date().toLocaleTimeString()
    });

    display.value = result.toString();
  } catch (error) {
    display.value = "Error";
  }
};

// Clear display
const clearDisplay = () => {
  display.value = "0";
};

// Clear everything (AC)
const allClear = () => {
  display.value = "0";
  memory.value = 0;
};

// Delete last digit
const deleteLast = () => {
  if (display.value.length > 1) {
    display.value = display.value.slice(0, -1);
  } else {
    display.value = "0";
  }
};

// Memory functions
const memoryStore = () => {
  try {
    memory.value = eval(display.value);
  } catch (error) {
    memory.value = 0;
  }
};

const memoryRecall = () => {
  display.value = memory.value.toString();
};

const memoryClear = () => {
  memory.value = 0;
};

// Computed untuk dynamic class
const displayClass = computed(() => {
  if (display.value.length > 20) return "extra-small";
  if (display.value.length > 12) return "small-text";
  return "";
});

// Keyboard support
const handleKeyPress = (event) => {
  const key = event.key;

  if (key >= '0' && key <= '9') appendToDisplay(key);
  else if (key === '.') appendToDisplay('.');
  else if (key === '+' || key === '-' || key === '*' || key === '/') appendToDisplay(key);
  else if (key === 'Enter' || key === '=') calculate();
  else if (key === 'Escape') allClear();
  else if (key === 'Backspace') deleteLast();
};

onMounted(() => {
  window.addEventListener('keydown', handleKeyPress);
});

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyPress);
});
</script>

<template>
  <div class="app">
    <header>
      <h1>🧮 Vue Calculator Pro</h1>
      <p>Advanced Calculator with Memory & History</p>
    </header>

    <main>
      <div class="calculator-container">
        <!-- Display -->
        <div class="display-container">
          <input
            v-model="display"
            :class="displayClass"
            readonly
            class="calculator-display"
          />
          <div class="memory-indicator" v-if="memory !== 0">
            M: {{ memory }}
          </div>
        </div>

        <!-- Calculator Buttons -->
        <div class="calculator">
          <!-- Row 1: Memory & Clear -->
          <div class="button-row">
            <button @click="memoryStore" class="memory-btn">MS</button>
            <button @click="memoryRecall" class="memory-btn">MR</button>
            <button @click="memoryClear" class="memory-btn">MC</button>
            <button @click="deleteLast" class="function-btn">⌫</button>
            <button @click="allClear" class="clear-btn">AC</button>
          </div>

          <!-- Row 2 -->
          <div class="button-row">
            <button @click="appendToDisplay('7')" class="number">7</button>
            <button @click="appendToDisplay('8')" class="number">8</button>
            <button @click="appendToDisplay('9')" class="number">9</button>
            <button @click="appendToDisplay('/')" class="operator">÷</button>
            <button @click="calculate" class="equals">=</button>
          </div>

          <!-- Row 3 -->
          <div class="button-row">
            <button @click="appendToDisplay('4')" class="number">4</button>
            <button @click="appendToDisplay('5')" class="number">5</button>
            <button @click="appendToDisplay('6')" class="number">6</button>
            <button @click="appendToDisplay('*')" class="operator">×</button>
          </div>

          <!-- Row 4 -->
          <div class="button-row">
            <button @click="appendToDisplay('1')" class="number">1</button>
            <button @click="appendToDisplay('2')" class="number">2</button>
            <button @click="appendToDisplay('3')" class="number">3</button>
            <button @click="appendToDisplay('-')" class="operator">−</button>
          </div>

          <!-- Row 5 -->
          <div class="button-row">
            <button @click="appendToDisplay('0')" class="number zero">0</button>
            <button @click="appendToDisplay('.')" class="decimal">.</button>
            <button @click="appendToDisplay('+')" class="operator">+</button>
          </div>
        </div>
      </div>

      <!-- History Panel -->
      <div class="history-panel">
        <h3>📜 History</h3>
        <div class="history-list">
          <div v-if="history.length === 0" class="empty-history">
            No calculations yet
          </div>
          <div
            v-for="(item, index) in history.slice(0, 10)"
            :key="index"
            class="history-item"
          >
            <div class="history-expression">{{ item.expression }}</div>
            <div class="history-result">= {{ item.result }}</div>
            <div class="history-time">{{ item.timestamp }}</div>
          </div>
        </div>
        <button @click="history = []" class="clear-history-btn" v-if="history.length > 0">
          Clear History
        </button>
      </div>
    </main>

    <footer>
      <p>Keyboard: Numbers, Operators, Enter/=, Backspace, Escape</p>
      <p>Made with ❤️ using Vue.js 3</p>
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
  display: flex;
  flex-direction: column;
}

header {
  text-align: center;
  padding: 30px 20px;
}

header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

header p {
  opacity: 0.9;
  font-size: 1.1em;
}

main {
  flex: 1;
  display: flex;
  gap: 30px;
  justify-content: center;
  align-items: flex-start;
  padding: 0 20px;
  max-width: 1000px;
  margin: 0 auto;
}

.calculator-container {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
}

.display-container {
  margin-bottom: 20px;
}

.calculator-display {
  width: 100%;
  padding: 20px;
  font-size: 2em;
  text-align: right;
  background: rgba(0,0,0,0.3);
  border: none;
  border-radius: 10px;
  color: white;
  font-family: 'Courier New', monospace;
  margin-bottom: 10px;
}

.calculator-display.small-text {
  font-size: 1.5em;
}

.calculator-display.extra-small {
  font-size: 1.2em;
}

.memory-indicator {
  text-align: right;
  font-size: 0.9em;
  opacity: 0.8;
  color: #4CAF50;
}

.calculator {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.button-row {
  display: flex;
  gap: 10px;
}

button {
  flex: 1;
  padding: 20px;
  font-size: 1.2em;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: bold;
  min-height: 60px;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0,0,0,0.2);
}

button:active {
  transform: translateY(0);
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

.equals {
  background: #2196F3;
  color: white;
}

.equals:hover {
  background: #1976D2;
}

.clear-btn {
  background: #f44336;
  color: white;
}

.clear-btn:hover {
  background: #da190b;
}

.function-btn {
  background: #FF9800;
  color: white;
}

.function-btn:hover {
  background: #F57C00;
}

.memory-btn {
  background: #9C27B0;
  color: white;
}

.memory-btn:hover {
  background: #7B1FA2;
}

.decimal {
  background: rgba(255,255,255,0.2);
  color: white;
}

.decimal:hover {
  background: rgba(255,255,255,0.3);
}

.zero {
  flex: 2;
}

/* History Panel */
.history-panel {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 20px;
  border: 1px solid rgba(255,255,255,0.2);
  box-shadow: 0 15px 35px rgba(0,0,0,0.2);
  width: 250px;
  max-height: 400px;
  overflow-y: auto;
}

.history-panel h3 {
  margin-bottom: 15px;
  text-align: center;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.history-item {
  background: rgba(255,255,255,0.1);
  padding: 10px;
  border-radius: 8px;
  font-size: 0.9em;
}

.history-expression {
  font-family: 'Courier New', monospace;
  margin-bottom: 5px;
}

.history-result {
  font-weight: bold;
  color: #4CAF50;
  margin-bottom: 5px;
}

.history-time {
  font-size: 0.8em;
  opacity: 0.7;
}

.empty-history {
  text-align: center;
  opacity: 0.7;
  padding: 20px;
}

.clear-history-btn {
  width: 100%;
  padding: 10px;
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  transition: background 0.3s;
}

.clear-history-btn:hover {
  background: rgba(255,255,255,0.3);
}

footer {
  text-align: center;
  padding: 20px;
  opacity: 0.8;
  border-top: 1px solid rgba(255,255,255,0.2);
}

/* Responsive Design */
@media (max-width: 768px) {
  main {
    flex-direction: column;
    align-items: center;
    padding: 0 10px;
  }

  .calculator-container {
    width: 100%;
    max-width: 350px;
  }

  .history-panel {
    width: 100%;
    max-width: 350px;
    margin-top: 20px;
  }

  button {
    padding: 15px;
    font-size: 1em;
    min-height: 50px;
  }

  .calculator-display {
    font-size: 1.5em;
    padding: 15px;
  }

  .calculator-display.small-text {
    font-size: 1.2em;
  }

  header h1 {
    font-size: 2em;
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

🎉 **Selesai! Aplikasi Calculator Pro Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis
3. **`onMounted/onUnmounted`** - Lifecycle hooks
4. **`v-model`** - Two-way data binding
5. **`@click`** - Event handling
6. **`{{ }}`** - Tampilkan data di template

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed } from "vue";
const display = ref("0");    // Display kalkulator
const memory = ref(0);       // Memory kalkulator
const history = ref([]);     // History perhitungan

// 2. Functions
const appendToDisplay = () => { /* tambah input */ };
const calculate = () => { /* hitung hasil */ };
const clearDisplay = () => { /* reset display */ };

// 3. Computed Properties
const displayClass = computed(() => { /* class dinamis */ });

// 4. Lifecycle Hooks
onMounted(() => { /* setup keyboard listener */ });
</script>

<template>
<!-- 5. Tampilan HTML -->
</template>

<style scoped>
/* 6. Styling CSS */
</style>
```

### 🧮 Logika Kalkulator

```javascript
// Tambah input ke display
const appendToDisplay = (value) => {
  if (display.value === "0" && value !== ".") {
    display.value = value;     // Ganti 0 dengan input baru
  } else {
    display.value += value;    // Tambah ke display
  }
};

// Hitung hasil dengan eval()
const calculate = () => {
  try {
    const result = eval(display.value);  // Hitung ekspresi
    display.value = result.toString();   // Tampilkan hasil
  } catch (error) {
    display.value = "Error";             // Handle error
  }
};
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Scientific Mode
```vue
<script setup>
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

### 2. Advanced Functions
```vue
<script setup>
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

### 3. Percentage & Sign Change
```vue
<script setup>
const calculatePercentage = () => {
  try {
    const num = eval(display.value);
    display.value = (num / 100).toString();
  } catch (error) {
    display.value = "Error";
  }
};

const toggleSign = () => {
  if (display.value !== "0") {
    if (display.value.startsWith('-')) {
      display.value = display.value.slice(1);
    } else {
      display.value = '-' + display.value;
    }
  }
};
</script>
```

### 4. Sound Effects
```vue
<script setup>
const playSound = () => {
  const audio = new Audio('data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBj+a2/LDciUFLIHO8tiJNwgZaLvt559NEAxQp+PwtmMcBjiR1/LMeSwFJHfH8N2QQAoUXrTp66hVFApGn+DyvmwhBSt+yvLTgjMGHm7A7+OZURE');
  audio.play();
};

const appendToDisplay = (value) => {
  playSound();
  // ... existing logic
};
</script>
```

### 5. Themes
```vue
<script setup>
const themes = ref(['default', 'dark', 'neon', 'retro']);
const currentTheme = ref('default');

const themeClasses = computed(() => ({
  'theme-dark': currentTheme.value === 'dark',
  'theme-neon': currentTheme.value === 'neon',
  'theme-retro': currentTheme.value === 'retro'
}));
</script>

<template>
  <div class="app" :class="themeClasses">
    <!-- calculator content -->
  </div>
</template>
```

## 🎨 Customization Ideas

### 1. Color Schemes
```css
/* Dark Theme */
.theme-dark {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

/* Neon Theme */
.theme-neon {
  background: linear-gradient(135deg, #00ff88 0%, #00d4ff 100%);
}
```

### 2. Button Animations
```css
button {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

button:active {
  transform: scale(0.95);
}
```

### 3. Display Animations
```css
.calculator-display {
  transition: all 0.2s ease;
}

.calculator-display:focus {
  box-shadow: 0 0 20px rgba(255,255,255,0.5);
}
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Kalkulator tidak berfungsi | Cek `eval()` function dan console error |
| Display tidak update | Pastikan `ref()` reactive data benar |
| Keyboard tidak berfungsi | Cek event listener di `onMounted` |
| Styling berantakan | Cek CSS selectors dan responsive design |
| Memory functions error | Validasi sebelum menggunakan `eval()` |

## 🚀 Next Steps

1. **Scientific Calculator** - Tambah fungsi trigonometri
2. **Unit Converter** - Konversi panjang, berat, suhu
3. **Graphing Calculator** - Plot fungsi matematika
4. **Calculator App** - Progressive Web App (PWA)
5. **Voice Calculator** - Voice input & output
6. **Multi-base Calculator** - Binary, Hex, Octal

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Calculator Pro yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)