# 🚀 Panduan Cepat: Password Generator Vue.js

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
npm create vue@latest password-generator-app

# Masuk ke folder project
cd password-generator-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref, computed, onMounted } from 'vue';

// Reactive data
const passwordLength = ref(12);
const includeUppercase = ref(true);
const includeNumbers = ref(true);
const includeSymbols = ref(true);
const generatedPassword = ref('');
const copiedToClipboard = ref(false);
const passwordHistory = ref([]);

// Advanced options
const excludeSimilar = ref(false);
const excludeDuplicates = ref(false);
const startWithLetter = ref(false);

// Computed properties
const passwordStrength = computed(() => {
  if (!generatedPassword.value) return 0;

  let strength = 0;
  const password = generatedPassword.value;

  // Length bonus
  if (password.length >= 16) strength += 3;
  else if (password.length >= 12) strength += 2;
  else if (password.length >= 8) strength += 1;

  // Character variety bonus
  if (/[a-z]/.test(password)) strength += 1;
  if (/[A-Z]/.test(password)) strength += 1;
  if (/\d/.test(password)) strength += 1;
  if (/[^a-zA-Z0-9]/.test(password)) strength += 2;

  return Math.min(strength, 10);
});

const strengthColor = computed(() => {
  const strength = passwordStrength.value;
  if (strength <= 3) return '#e74c3c';
  if (strength <= 6) return '#f39c12';
  if (strength <= 8) return '#27ae60';
  return '#2ecc71';
});

const strengthText = computed(() => {
  const strength = passwordStrength.value;
  if (strength <= 3) return 'Lemah';
  if (strength <= 6) return 'Sedang';
  if (strength <= 8) return 'Kuat';
  return 'Sangat Kuat';
});

// Character sets
const getCharacterSets = () => {
  let chars = "abcdefghijklmnopqrstuvwxyz";

  if (includeUppercase.value) chars += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  if (includeNumbers.value) chars += "0123456789";
  if (includeSymbols.value) chars += "!@#$%^&*()_+-=[]{}|;:,.<>?/~`";

  // Exclude similar characters
  if (!excludeSimilar.value) {
    chars = chars.replace(/[ilLoO01]/g, '');
  }

  return chars;
};

// Main functions
const generatePassword = () => {
  const allChars = getCharacterSets();

  if (allChars.length === 0) {
    alert('Pilih setidaknya satu jenis karakter!');
    return;
  }

  let password = '';
  const usedChars = new Set();

  for (let i = 0; i < passwordLength.value; i++) {
    let randomIndex;

    if (excludeDuplicates.value) {
      let attempts = 0;
      do {
        randomIndex = Math.floor(Math.random() * allChars.length);
        attempts++;
      } while (usedChars.has(allChars[randomIndex]) && attempts < 100);

      if (attempts >= 100) usedChars.clear();
      usedChars.add(allChars[randomIndex]);
    } else {
      randomIndex = Math.floor(Math.random() * allChars.length);
    }

    password += allChars[randomIndex];
  }

  // Start with letter if required
  if (startWithLetter.value && !/^[a-zA-Z]/.test(password)) {
    const letters = allChars.match(/[a-zA-Z]/);
    if (letters && letters.length > 0) {
      const firstChar = letters[Math.floor(Math.random() * letters.length)];
      password = firstChar + password.slice(1);
    }
  }

  generatedPassword.value = password;
  addToHistory(password);
  copiedToClipboard.value = false;
};

const copyToClipboard = async () => {
  if (!generatedPassword.value) return;

  try {
    await navigator.clipboard.writeText(generatedPassword.value);
    copiedToClipboard.value = true;
    setTimeout(() => copiedToClipboard.value = false, 2000);
  } catch (err) {
    // Fallback
    const textArea = document.createElement('textarea');
    textArea.value = generatedPassword.value;
    document.body.appendChild(textArea);
    textArea.select();
    document.execCommand('copy');
    document.body.removeChild(textArea);
    copiedToClipboard.value = true;
    setTimeout(() => copiedToClipboard.value = false, 2000);
  }
};

const addToHistory = (password) => {
  const timestamp = new Date().toLocaleString();
  passwordHistory.value.unshift({
    password,
    timestamp,
    strength: passwordStrength.value,
    length: password.length
  });

  if (passwordHistory.value.length > 10) {
    passwordHistory.value = passwordHistory.value.slice(0, 10);
  }

  saveToLocalStorage();
};

const saveToLocalStorage = () => {
  try {
    localStorage.setItem('passwordHistory', JSON.stringify(passwordHistory.value));
    localStorage.setItem('passwordSettings', JSON.stringify({
      passwordLength: passwordLength.value,
      includeUppercase: includeUppercase.value,
      includeNumbers: includeNumbers.value,
      includeSymbols: includeSymbols.value
    }));
  } catch (error) {
    console.error('Error saving to localStorage:', error);
  }
};

const loadFromLocalStorage = () => {
  try {
    const savedHistory = localStorage.getItem('passwordHistory');
    if (savedHistory) {
      passwordHistory.value = JSON.parse(savedHistory);
    }

    const savedSettings = localStorage.getItem('passwordSettings');
    if (savedSettings) {
      const settings = JSON.parse(savedSettings);
      passwordLength.value = settings.passwordLength || 12;
      includeUppercase.value = settings.includeUppercase !== false;
      includeNumbers.value = settings.includeNumbers !== false;
      includeSymbols.value = settings.includeSymbols !== false;
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
};

const clearHistory = () => {
  if (confirm('Hapus semua history password?')) {
    passwordHistory.value = [];
    localStorage.removeItem('passwordHistory');
  }
};

const resetSettings = () => {
  if (confirm('Reset ke pengaturan default?')) {
    passwordLength.value = 12;
    includeUppercase.value = true;
    includeNumbers.value = true;
    includeSymbols.value = true;
    excludeSimilar.value = false;
    excludeDuplicates.value = false;
    startWithLetter.value = false;
  }
};

const getStrengthPercentage = () => {
  return (passwordStrength.value / 10) * 100;
};

const formatTime = (timestamp) => {
  const date = new Date(timestamp);
  return date.toLocaleTimeString('id-ID', {
    hour: '2-digit',
    minute: '2-digit'
  });
};

// Initialize
onMounted(() => {
  loadFromLocalStorage();
  if (!generatedPassword.value) {
    generatePassword();
  }
});
</script>

<template>
  <div class="password-generator">
    <!-- Header -->
    <header class="header">
      <h1>🔐 Password Generator</h1>
      <p>Buat password aman dan kuat dengan mudah</p>
    </header>

    <!-- Password Display -->
    <section class="password-display">
      <div class="password-output">
        <div class="password-text" :class="{ 'copied': copiedToClipboard }">
          {{ generatedPassword || 'Klik Generate untuk membuat password' }}
        </div>
        <button
          @click="copyToClipboard"
          class="copy-btn"
          :disabled="!generatedPassword"
          :class="{ 'copied': copiedToClipboard }"
        >
          {{ copiedToClipboard ? '✓' : '📋' }}
        </button>
      </div>

      <!-- Strength Indicator -->
      <div class="strength-indicator">
        <div class="strength-bar">
          <div
            class="strength-fill"
            :style="{ width: getStrengthPercentage() + '%', backgroundColor: strengthColor }"
          ></div>
        </div>
        <div class="strength-text" :style="{ color: strengthColor }">
          Kekuatan: {{ strengthText }}
        </div>
      </div>
    </section>

    <!-- Settings -->
    <section class="settings">
      <h2>⚙️ Pengaturan Password</h2>

      <!-- Password Length -->
      <div class="setting-group">
        <label>
          Panjang Password: <strong>{{ passwordLength }}</strong>
        </label>
        <input
          type="range"
          v-model="passwordLength"
          min="4"
          max="32"
          class="range-slider"
        />
        <div class="range-labels">
          <span>4</span>
          <span>32</span>
        </div>
      </div>

      <!-- Character Options -->
      <div class="setting-group">
        <h3>Karakter yang Disertakan:</h3>

        <label class="checkbox-label">
          <input type="checkbox" v-model="includeUppercase" />
          <span class="checkmark"></span>
          Huruf Besar (A-Z)
        </label>

        <label class="checkbox-label">
          <input type="checkbox" v-model="includeNumbers" />
          <span class="checkmark"></span>
          Angka (0-9)
        </label>

        <label class="checkbox-label">
          <input type="checkbox" v-model="includeSymbols" />
          <span class="checkmark"></span>
          Simbol (!@#$%^&*)
        </label>
      </div>

      <!-- Advanced Options -->
      <details class="advanced-options">
        <summary>🔧 Opsi Lanjutan</summary>

        <div class="advanced-settings">
          <label class="checkbox-label">
            <input type="checkbox" v-model="excludeSimilar" />
            <span class="checkmark"></span>
            Exclude karakter mirip (i, l, 1, L, o, 0, O)
          </label>

          <label class="checkbox-label">
            <input type="checkbox" v-model="excludeDuplicates" />
            <span class="checkmark"></span>
            Exclude karakter duplikat
          </label>

          <label class="checkbox-label">
            <input type="checkbox" v-model="startWithLetter" />
            <span class="checkmark"></span>
            Mulai dengan huruf
          </label>
        </div>
      </details>

      <!-- Action Buttons -->
      <div class="action-buttons">
        <button @click="generatePassword" class="btn-primary">
          🎲 Generate Password
        </button>
        <button @click="resetSettings" class="btn-secondary">
          🔄 Reset
        </button>
      </div>
    </section>

    <!-- History -->
    <section v-if="passwordHistory.length > 0" class="history">
      <div class="history-header">
        <h3>📜 History Password</h3>
        <button @click="clearHistory" class="clear-btn">
          🗑️ Hapus
        </button>
      </div>

      <div class="history-list">
        <div
          v-for="(item, index) in passwordHistory"
          :key="index"
          class="history-item"
          @click="copyToClipboard"
        >
          <div class="history-password">{{ item.password }}</div>
          <div class="history-meta">
            <span class="history-length">{{ item.length }} karakter</span>
            <span class="history-time">{{ formatTime(item.timestamp) }}</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Security Tips -->
    <section class="tips">
      <h3>🛡️ Tips Keamanan</h3>
      <ul>
        <li>🔒 Gunakan password minimal 12 karakter</li>
        <li>🔡 Kombinasikan huruf besar, kecil, angka, dan simbol</li>
        <li>🚫 Hindari informasi pribadi dalam password</li>
        <li>🔄 Ganti password secara berkala</li>
        <li>🔐 Gunakan password berbeda untuk setiap akun</li>
      </ul>
    </section>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.password-generator {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
}

/* Header */
.header {
  text-align: center;
  margin-bottom: 30px;
  padding: 30px 0;
}

.header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.header p {
  font-size: 1.1em;
  opacity: 0.9;
}

/* Sections */
section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 25px;
  margin-bottom: 20px;
  border: 1px solid rgba(255,255,255,0.2);
}

/* Password Display */
.password-output {
  display: flex;
  align-items: center;
  gap: 15px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 15px;
  margin-bottom: 15px;
}

.password-text {
  flex: 1;
  font-family: 'Courier New', monospace;
  font-size: 1.2em;
  font-weight: 600;
  word-break: break-all;
  color: #4CAF50;
  transition: all 0.3s ease;
}

.password-text.copied {
  color: #2ecc71;
  transform: scale(1.02);
}

.copy-btn {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  padding: 10px 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.2em;
  color: white;
}

.copy-btn:hover:not(:disabled) {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.copy-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.copy-btn.copied {
  background: #4CAF50;
}

/* Strength Indicator */
.strength-bar {
  width: 100%;
  height: 8px;
  background: rgba(255,255,255,0.2);
  border-radius: 4px;
  overflow: hidden;
  margin-bottom: 10px;
}

.strength-fill {
  height: 100%;
  transition: all 0.5s ease;
  border-radius: 4px;
}

.strength-text {
  font-weight: 600;
  text-align: right;
}

/* Settings */
.settings h2 {
  text-align: center;
  margin-bottom: 25px;
  font-size: 1.5em;
}

.setting-group {
  margin-bottom: 20px;
}

.setting-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: 600;
}

.range-slider {
  width: 100%;
  height: 8px;
  border-radius: 4px;
  background: rgba(255,255,255,0.2);
  outline: none;
  margin-bottom: 5px;
  -webkit-appearance: none;
}

.range-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #4CAF50;
  cursor: pointer;
}

.range-labels {
  display: flex;
  justify-content: space-between;
  font-size: 0.9em;
  opacity: 0.8;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 10px;
  transition: background 0.3s ease;
}

.checkbox-label:hover {
  background: rgba(255,255,255,0.1);
}

.checkbox-label input[type="checkbox"] {
  display: none;
}

.checkmark {
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255,255,255,0.5);
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.checkbox-label input[type="checkbox"]:checked + .checkmark {
  background: #4CAF50;
  border-color: #4CAF50;
}

.checkbox-label input[type="checkbox"]:checked + .checkmark::after {
  content: '✓';
  color: white;
  font-weight: bold;
}

/* Advanced Options */
.advanced-options {
  margin-top: 20px;
}

.advanced-options summary {
  cursor: pointer;
  font-weight: 600;
  padding: 10px;
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  margin-bottom: 15px;
}

.advanced-settings {
  padding: 15px;
  background: rgba(0,0,0,0.1);
  border-radius: 10px;
}

/* Action Buttons */
.action-buttons {
  display: flex;
  gap: 15px;
  margin-top: 25px;
}

.btn-primary, .btn-secondary {
  flex: 1;
  padding: 15px 20px;
  border: none;
  border-radius: 10px;
  font-size: 1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-primary {
  background: linear-gradient(135deg, #4CAF50, #45a049);
  color: white;
}

.btn-secondary {
  background: rgba(255,255,255,0.2);
  color: white;
  border: 2px solid rgba(255,255,255,0.3);
}

.btn-primary:hover, .btn-secondary:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0,0,0,0.3);
}

/* History */
.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.clear-btn {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 8px 15px;
  cursor: pointer;
  font-size: 0.9em;
}

.history-list {
  max-height: 250px;
  overflow-y: auto;
}

.history-item {
  background: rgba(0,0,0,0.2);
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.history-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateX(5px);
}

.history-password {
  font-family: 'Courier New', monospace;
  font-weight: 600;
  margin-bottom: 5px;
}

.history-meta {
  display: flex;
  justify-content: space-between;
  font-size: 0.85em;
  opacity: 0.8;
}

/* Tips */
.tips h3 {
  margin-bottom: 15px;
  text-align: center;
}

.tips ul {
  list-style: none;
}

.tips li {
  padding: 8px 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
  display: flex;
  align-items: center;
  gap: 10px;
}

.tips li:last-child {
  border-bottom: none;
}

/* Responsive Design */
@media (max-width: 768px) {
  .password-generator {
    padding: 15px;
    margin: 10px;
  }

  .header h1 {
    font-size: 2em;
  }

  .password-output {
    flex-direction: column;
    gap: 10px;
  }

  .action-buttons {
    flex-direction: column;
  }

  .history-header {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }

  .history-meta {
    flex-direction: column;
    gap: 3px;
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

🎉 **Selesai! Aplikasi Password Generator Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data reactive
2. **`computed()`** - Hitung nilai otomatis (strength calculation)
3. **`v-model`** - Two-way data binding
4. **`@click`** - Event handling
5. **`v-if`** - Conditional rendering
6. **`:style`** - Dynamic styling

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & State
import { ref, computed, onMounted } from "vue";
const passwordLength = ref(12);        // Reactive variable
const includeUppercase = ref(true);   // Checkbox state
const generatedPassword = ref('');    // Generated password

// 2. Computed Properties
const passwordStrength = computed(() => { /* calculate strength */ });

// 3. Functions
const generatePassword = () => { /* generate logic */ };
const copyToClipboard = () => { /* copy logic */ };
</script>

<template>
<!-- 4. Tampilan HTML -->
</template>

<style scoped>
/* 5. Styling CSS */
</style>
```

### 🔄 Logika Password Generation

```javascript
const generatePassword = () => {
  // 1. Build character set
  let chars = "abcdefghijklmnopqrstuvwxyz";
  if (includeUppercase.value) chars += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  if (includeNumbers.value) chars += "0123456789";
  if (includeSymbols.value) chars += "!@#$%^&*()";

  // 2. Generate random password
  let password = "";
  for (let i = 0; i < passwordLength.value; i++) {
    const randomIndex = Math.floor(Math.random() * chars.length);
    password += chars[randomIndex];
  }

  // 3. Update reactive state
  generatedPassword.value = password;
};
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Password Strength Analysis
```vue
<script setup>
const checkPasswordBreached = async (password) => {
  // Simulate API call to check if password has been breached
  const commonPasswords = ['123456', 'password', 'qwerty'];
  return commonPasswords.includes(password.toLowerCase());
};
</script>
```

### 2. Multiple Password Generation
```vue
<script setup>
const generateMultiplePasswords = (count = 5) => {
  const passwords = [];
  for (let i = 0; i < count; i++) {
    generatePassword();
    passwords.push({
      password: generatedPassword.value,
      strength: passwordStrength.value
    });
  }
  return passwords;
};
</script>
```

### 3. Password Export
```vue
<script setup>
const exportPasswords = () => {
  const data = {
    passwords: passwordHistory.value,
    exportedAt: new Date().toISOString()
  };

  const blob = new Blob([JSON.stringify(data, null, 2)], {
    type: 'application/json'
  });

  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'passwords.json';
  a.click();
};
</script>
```

### 4. Custom Character Sets
```vue
<script setup>
const customCharacters = ref('');
const includeCustom = ref(false);

const getCharacterSets = () => {
  let chars = "abcdefghijklmnopqrstuvwxyz";

  if (includeUppercase.value) chars += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  if (includeNumbers.value) chars += "0123456789";
  if (includeSymbols.value) chars += "!@#$%^&*()";
  if (includeCustom.value && customCharacters.value) {
    chars += customCharacters.value;
  }

  return chars;
};
</script>
```

### 5. Password Templates
```vue
<script setup>
const templates = ref([
  {
    name: 'Strong Password',
    settings: { length: 16, uppercase: true, numbers: true, symbols: true }
  },
  {
    name: 'PIN Code',
    settings: { length: 4, uppercase: false, numbers: true, symbols: false }
  },
  {
    name: 'Simple Password',
    settings: { length: 8, uppercase: true, numbers: true, symbols: false }
  }
]);

const applyTemplate = (template) => {
  passwordLength.value = template.settings.length;
  includeUppercase.value = template.settings.uppercase;
  includeNumbers.value = template.settings.numbers;
  includeSymbols.value = template.settings.symbols;
  generatePassword();
};
</script>
```

## 🎨 Customization Ideas

### 1. Theme Variations
```css
/* Dark Theme */
.dark-theme {
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 100%);
}

/* Neon Theme */
.neon-theme {
  background: #0a0a0a;
  box-shadow: 0 0 20px rgba(0, 255, 255, 0.5);
}
```

### 2. Animations
```vue
<script setup>
import { ref, watch } from 'vue';

const isGenerating = ref(false);

const generatePassword = async () => {
  isGenerating.value = true;

  // Add animation delay
  await new Promise(resolve => setTimeout(resolve, 500));

  // Generate password logic...

  isGenerating.value = false;
};
</script>

<style>
@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.generating {
  animation: pulse 1s infinite;
}
</style>
```

### 3. Sound Effects
```vue
<script setup>
const playSound = (type) => {
  const audio = new Audio(`/sounds/${type}.mp3`);
  audio.play().catch(e => console.log('Sound play failed:', e));
};

const copyToClipboard = () => {
  // Copy logic...
  playSound('copy');
};

const generatePassword = () => {
  // Generate logic...
  playSound('generate');
};
</script>
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Password tidak ter-generate | Cek opsi karakter, pastikan ada yang dipilih |
| Copy to clipboard tidak bekerja | Pastikan HTTPS, cek browser permissions |
| LocalStorage tidak bekerja | Cek storage quota, browser permissions |
| Strength indicator tidak akurat | Review calculation algorithm |

## 🚀 Next Steps

1. **Password Analysis** - Add detailed entropy calculation
2. **Security Check** - Integrate with breach databases
3. **Batch Generation** - Generate multiple passwords at once
4. **Import/Export** - Save/load password configurations
5. **API Integration** - Connect to password management services
6. **Mobile App** - Convert to PWA or mobile app

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Password Generator yang lengkap dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/) | [Security Guidelines](https://owasp.org/) | [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)