# Password Generator - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Password Generator adalah aplikasi web yang memungkinkan pengguna untuk membuat password yang aman dan kuat secara acak menggunakan Vue.js 3 dengan Composition API. Aplikasi ini mendemonstrasikan konsep dasar Vue.js seperti reactive data, event handling, dan dynamic styling. Project ini sangat cocok untuk pemula yang ingin mempelajari fundamental Vue.js sambil membuat aplikasi yang praktis dan bermanfaat.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- Event handling dengan `@click` dan `v-model`
- Conditional rendering dengan `v-if`
- Form handling dan user input validation
- Dynamic styling dan CSS transitions
- Component-based architecture
- Random generation algorithms
- Security best practices untuk password creation
- User experience design untuk utility applications

## 📋 Fitur Aplikasi

1. **Dynamic Password Generation**: Membuat password acak dengan kriteria tertentu
2. **Customizable Length**: Mengatur panjang password (4-32 karakter)
3. **Character Options**: Memilih jenis karakter (uppercase, numbers, symbols)
4. **Real-time Updates**: Password dibuat secara instan saat tombol diklik
5. **Copy to Clipboard**: Mudah menyalin password yang dihasilkan (enhanced)
6. **Password Strength Indicator**: Menunjukkan kekuatan password (enhanced)
7. **Generate History**: Menyimpan history password yang dibuat (enhanced)
8. **Responsive Design**: Layout yang bekerja di semua device sizes
9. **Visual Feedback**: Animasi dan transisi yang halus
10. **Accessibility Features**: Keyboard navigation dan screen reader support

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)

## 📂 Struktur Project

```
7. Password Generator/
├── App.vue                                    # Komponen utama aplikasi
├── components/
│   └── PasswordGenerator.vue                  # Komponen password generator
├── assets/
│   └── styles/
│       └── main.css                          # Global styles
├── utils/
│   └── passwordUtils.js                      # Helper functions untuk password
└── README.md                                  # Dokumentasi project
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
npm create vue@latest password-generator-app
cd password-generator-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create password-generator-app
cd password-generator-app
```

### Langkah 3: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style scoped>`**: Bagian CSS untuk styling

### Langkah 4: Membuat Komponen PasswordGenerator

**File: `components/PasswordGenerator.vue`**

#### Bagian Script Setup - Reactive Data Management
```vue
<script setup>
import { ref, computed, onMounted } from 'vue';

// Reactive data untuk form inputs
const passwordLength = ref(12);
const includeUppercase = ref(true);
const includeNumbers = ref(true);
const includeSymbols = ref(true);
const includeSimilar = ref(false);
const includeAmbiguous = ref(false);

// Reactive data untuk password hasil
const generatedPassword = ref('');
const passwordHistory = ref([]);
const copiedToClipboard = ref(false);
const showToast = ref(false);
const toastMessage = ref('');

// Advanced options
const excludeDuplicates = ref(false);
const startWithLetter = ref(false);
const customCharacters = ref('');

// Computed properties untuk validasi dan kalkulasi
const passwordStrength = computed(() => {
  let strength = 0;

  if (passwordLength.value >= 12) strength += 2;
  else if (passwordLength.value >= 8) strength += 1;

  if (includeUppercase.value) strength += 1;
  if (includeNumbers.value) strength += 1;
  if (includeSymbols.value) strength += 2;
  if (!includeSimilar.value) strength += 1;
  if (!includeAmbiguous.value) strength += 1;

  if (strength <= 2) return 'weak';
  if (strength <= 4) return 'medium';
  if (strength <= 6) return 'strong';
  return 'very-strong';
});

const strengthColor = computed(() => {
  const colors = {
    'weak': '#e74c3c',
    'medium': '#f39c12',
    'strong': '#27ae60',
    'very-strong': '#2ecc71'
  };
  return colors[passwordStrength.value];
});

const strengthText = computed(() => {
  const texts = {
    'weak': 'Lemah',
    'medium': 'Sedang',
    'strong': 'Kuat',
    'very-strong': 'Sangat Kuat'
  };
  return texts[passwordStrength.value];
});

// Character sets untuk password generation
const getCharacterSets = () => {
  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const uppercaseChars = includeUppercase.value ? "ABCDEFGHIJKLMNOPQRSTUVWXYZ" : "";
  const numberChars = includeNumbers.value ? "0123456789" : "";
  const symbolChars = includeSymbols.value ? "!@#$%^&*()_+-=[]{}|;:,.<>?/~`" : "";

  // Similar characters (optional exclusion)
  const similarChars = "ilLoO01";
  const ambiguousChars = "{}[]()/'\"`~,;.<>";

  let allChars = lowercaseChars + uppercaseChars + numberChars + symbolChars;

  // Add custom characters if provided
  if (customCharacters.value) {
    allChars += customCharacters.value;
  }

  // Remove similar characters if excluded
  if (!includeSimilar.value) {
    allChars = allChars.split('').filter(char => !similarChars.includes(char)).join('');
  }

  // Remove ambiguous characters if excluded
  if (!includeAmbiguous.value) {
    allChars = allChars.split('').filter(char => !ambiguousChars.includes(char)).join('');
  }

  return allChars;
};

// Main password generation function
const generatePassword = () => {
  const allChars = getCharacterSets();

  // Validate if we have any characters to use
  if (allChars.length === 0) {
    showError('Tidak ada karakter yang tersedia. Pilih setidaknya satu opsi karakter.');
    return;
  }

  // Validate password length
  if (passwordLength.value < 4 || passwordLength.value > 32) {
    showError('Panjang password harus antara 4 dan 32 karakter.');
    return;
  }

  let password = '';
  const usedChars = new Set();

  // Generate password character by character
  for (let i = 0; i < passwordLength.value; i++) {
    let randomIndex;

    if (excludeDuplicates.value) {
      // Find unused character
      let attempts = 0;
      do {
        randomIndex = Math.floor(Math.random() * allChars.length);
        attempts++;
      } while (usedChars.has(allChars[randomIndex]) && attempts < 100);

      if (attempts >= 100) {
        // Can't find unique character, reset used chars
        usedChars.clear();
        randomIndex = Math.floor(Math.random() * allChars.length);
      }

      usedChars.add(allChars[randomIndex]);
    } else {
      randomIndex = Math.floor(Math.random() * allChars.length);
    }

    password += allChars[randomIndex];
  }

  // Ensure password starts with letter if required
  if (startWithLetter.value && !/^[a-zA-Z]/.test(password)) {
    const letters = allChars.match(/[a-zA-Z]/);
    if (letters && letters.length > 0) {
      const firstChar = letters[Math.floor(Math.random() * letters.length)];
      password = firstChar + password.slice(1);
    }
  }

  generatedPassword.value = password;

  // Add to history
  addToHistory(password);

  // Reset copied state
  copiedToClipboard.value = false;

  // Show success message
  showSuccess('Password berhasil dibuat!');
};

// Copy password to clipboard
const copyToClipboard = async () => {
  if (!generatedPassword.value) {
    showError('Tidak ada password untuk disalin.');
    return;
  }

  try {
    await navigator.clipboard.writeText(generatedPassword.value);
    copiedToClipboard.value = true;
    showSuccess('Password berhasil disalin ke clipboard!');

    // Reset copied state after 2 seconds
    setTimeout(() => {
      copiedToClipboard.value = false;
    }, 2000);
  } catch (err) {
    // Fallback for older browsers
    const textArea = document.createElement('textarea');
    textArea.value = generatedPassword.value;
    document.body.appendChild(textArea);
    textArea.select();
    document.execCommand('copy');
    document.body.removeChild(textArea);

    copiedToClipboard.value = true;
    showSuccess('Password berhasil disalin ke clipboard!');

    setTimeout(() => {
      copiedToClipboard.value = false;
    }, 2000);
  }
};

// Add password to history
const addToHistory = (password) => {
  const timestamp = new Date().toLocaleString();
  passwordHistory.value.unshift({
    password,
    timestamp,
    strength: passwordStrength.value,
    length: password.length
  });

  // Keep only last 10 passwords
  if (passwordHistory.value.length > 10) {
    passwordHistory.value = passwordHistory.value.slice(0, 10);
  }

  // Save to localStorage
  saveToLocalStorage();
};

// Clear history
const clearHistory = () => {
  if (confirm('Apakah Anda yakin ingin menghapus semua history password?')) {
    passwordHistory.value = [];
    saveToLocalStorage();
    showSuccess('History password berhasil dihapus.');
  }
};

// Save to localStorage
const saveToLocalStorage = () => {
  try {
    localStorage.setItem('passwordHistory', JSON.stringify(passwordHistory.value));
    localStorage.setItem('passwordSettings', JSON.stringify({
      passwordLength: passwordLength.value,
      includeUppercase: includeUppercase.value,
      includeNumbers: includeNumbers.value,
      includeSymbols: includeSymbols.value,
      includeSimilar: includeSimilar.value,
      includeAmbiguous: includeAmbiguous.value,
      excludeDuplicates: excludeDuplicates.value,
      startWithLetter: startWithLetter.value,
      customCharacters: customCharacters.value
    }));
  } catch (error) {
    console.error('Error saving to localStorage:', error);
  }
};

// Load from localStorage
const loadFromLocalStorage = () => {
  try {
    // Load history
    const savedHistory = localStorage.getItem('passwordHistory');
    if (savedHistory) {
      passwordHistory.value = JSON.parse(savedHistory);
    }

    // Load settings
    const savedSettings = localStorage.getItem('passwordSettings');
    if (savedSettings) {
      const settings = JSON.parse(savedSettings);
      passwordLength.value = settings.passwordLength || 12;
      includeUppercase.value = settings.includeUppercase !== false;
      includeNumbers.value = settings.includeNumbers !== false;
      includeSymbols.value = settings.includeSymbols !== false;
      includeSimilar.value = settings.includeSimilar || false;
      includeAmbiguous.value = settings.includeAmbiguous || false;
      excludeDuplicates.value = settings.excludeDuplicates || false;
      startWithLetter.value = settings.startWithLetter || false;
      customCharacters.value = settings.customCharacters || '';
    }
  } catch (error) {
    console.error('Error loading from localStorage:', error);
  }
};

// Toast notification functions
const showSuccess = (message) => {
  showToast.value = true;
  toastMessage.value = message;
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
};

const showError = (message) => {
  showToast.value = true;
  toastMessage.value = message;
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
};

// Password strength checker
const checkPasswordStrength = (password) => {
  if (!password) return 0;

  let score = 0;

  // Length bonus
  if (password.length >= 12) score += 3;
  else if (password.length >= 8) score += 2;
  else if (password.length >= 6) score += 1;

  // Character variety bonus
  if (/[a-z]/.test(password)) score += 1;
  if (/[A-Z]/.test(password)) score += 1;
  if (/[0-9]/.test(password)) score += 1;
  if (/[^a-zA-Z0-9]/.test(password)) score += 2;

  return Math.min(score, 10);
};

// Reset to default settings
const resetSettings = () => {
  if (confirm('Apakah Anda yakin ingin reset ke pengaturan default?')) {
    passwordLength.value = 12;
    includeUppercase.value = true;
    includeNumbers.value = true;
    includeSymbols.value = true;
    includeSimilar.value = false;
    includeAmbiguous.value = false;
    excludeDuplicates.value = false;
    startWithLetter.value = false;
    customCharacters.value = '';

    showSuccess('Pengaturan berhasil direset ke default.');
  }
};

// Generate password on mount
onMounted(() => {
  loadFromLocalStorage();
  if (!generatedPassword.value) {
    generatePassword();
  }
});
</script>
```

#### Penjelasan Kode Script:

1. **`import { ref, computed, onMounted } from 'vue'`**
   - Mengimpor fungsi-fungsi Vue.js yang diperlukan
   - `ref` untuk membuat reactive variables
   - `computed` untuk derived state
   - `onMounted` untuk lifecycle hooks

2. **Reactive Variables dengan `ref()`**
   - `passwordLength`: Mengatur panjang password
   - `includeUppercase/Numbers/Symbols`: Opsi karakter
   - `generatedPassword`: Password yang dihasilkan
   - `passwordHistory`: History password yang dibuat

3. **Computed Properties**
   - `passwordStrength`: Menghitung kekuatan password
   - `strengthColor/Text`: Warna dan teks berdasarkan kekuatan

4. **Main Functions**
   - `generatePassword()`: Fungsi utama untuk membuat password
   - `copyToClipboard()`: Menyalin password ke clipboard
   - `addToHistory()`: Menyimpan password ke history

5. **Helper Functions**
   - `saveToLocalStorage()`: Menyimpan data ke browser storage
   - `loadFromLocalStorage()`: Memuat data dari browser storage
   - `showSuccess()/showError()`: Menampilkan notifikasi

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="password-generator">
    <!-- Header -->
    <header class="generator-header">
      <h1>🔐 Password Generator</h1>
      <p>Buat password yang aman dan kuat dengan mudah</p>
    </header>

    <!-- Main Content -->
    <main class="generator-main">
      <!-- Generated Password Display -->
      <section class="password-display">
        <div class="password-output">
          <div class="password-text" :class="{ 'copied': copiedToClipboard }">
            {{ generatedPassword || 'Klik Generate untuk membuat password' }}
          </div>
          <button
            @click="copyToClipboard"
            class="copy-button"
            :disabled="!generatedPassword"
            :class="{ 'copied': copiedToClipboard }"
          >
            <span v-if="copiedToClipboard">✓</span>
            <span v-else>📋</span>
          </button>
        </div>

        <!-- Password Strength Indicator -->
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

      <!-- Password Settings -->
      <section class="password-settings">
        <h2>⚙️ Pengaturan Password</h2>

        <!-- Password Length -->
        <div class="setting-group">
          <label for="passwordLength">
            Panjang Password: <span class="value-display">{{ passwordLength }}</span>
          </label>
          <input
            id="passwordLength"
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
          <h3>📝 Karakter yang Disertakan</h3>

          <div class="checkbox-group">
            <label class="checkbox-item">
              <input type="checkbox" v-model="includeUppercase" />
              <span class="checkmark"></span>
              <span>Huruf Besar (A-Z)</span>
            </label>

            <label class="checkbox-item">
              <input type="checkbox" v-model="includeNumbers" />
              <span class="checkmark"></span>
              <span>Angka (0-9)</span>
            </label>

            <label class="checkbox-item">
              <input type="checkbox" v-model="includeSymbols" />
              <span class="checkmark"></span>
              <span>Simbol (!@#$%^&*)</span>
            </label>
          </div>
        </div>

        <!-- Advanced Options -->
        <details class="advanced-options">
          <summary>🔧 Opsi Lanjutan</summary>

          <div class="advanced-settings">
            <div class="checkbox-group">
              <label class="checkbox-item">
                <input type="checkbox" v-model="includeSimilar" />
                <span class="checkmark"></span>
                <span>Sertakan karakter mirip (i, l, 1, L, o, 0, O)</span>
              </label>

              <label class="checkbox-item">
                <input type="checkbox" v-model="includeAmbiguous" />
                <span class="checkmark"></span>
                <span>Sertakan karakter ambigu ({} [] () / ' " ` ~ , ; . < >)</span>
              </label>

              <label class="checkbox-item">
                <input type="checkbox" v-model="excludeDuplicates" />
                <span class="checkmark"></span>
                <span>Exclude karakter duplikat</span>
              </label>

              <label class="checkbox-item">
                <input type="checkbox" v-model="startWithLetter" />
                <span class="checkmark"></span>
                <span>Mulai dengan huruf</span>
              </label>
            </div>

            <!-- Custom Characters -->
            <div class="setting-group">
              <label for="customCharacters">
                Karakter Kustom (opsional):
              </label>
              <input
                id="customCharacters"
                type="text"
                v-model="customCharacters"
                placeholder="Tambahkan karakter kustom..."
                class="text-input"
              />
            </div>
          </div>
        </details>

        <!-- Action Buttons -->
        <div class="action-buttons">
          <button @click="generatePassword" class="generate-btn primary">
            🎲 Generate Password
          </button>

          <button @click="resetSettings" class="reset-btn secondary">
            🔄 Reset Pengaturan
          </button>
        </div>
      </section>

      <!-- Password History -->
      <section v-if="passwordHistory.length > 0" class="password-history">
        <div class="history-header">
          <h3>📜 History Password</h3>
          <button @click="clearHistory" class="clear-history-btn">
            🗑️ Hapus Semua
          </button>
        </div>

        <div class="history-list">
          <div
            v-for="(item, index) in passwordHistory"
            :key="index"
            class="history-item"
            :class="item.strength"
          >
            <div class="history-password" @click="copyPassword(item.password)">
              {{ item.password }}
            </div>
            <div class="history-meta">
              <span class="history-length">{{ item.length }} karakter</span>
              <span class="history-strength" :style="{ color: getStrengthColor(item.strength) }">
                {{ getStrengthText(item.strength) }}
              </span>
              <span class="history-time">{{ formatTime(item.timestamp) }}</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Security Tips -->
      <section class="security-tips">
        <h3>🛡️ Tips Keamanan Password</h3>
        <ul class="tips-list">
          <li>🔒 Gunakan password minimal 12 karakter untuk keamanan optimal</li>
          <li>🔡 Kombinasikan huruf besar, kecil, angka, dan simbol</li>
          <li>🚫 Hindari menggunakan informasi pribadi seperti nama atau tanggal lahir</li>
          <li>🔄 Ganti password secara berkala (setiap 3-6 bulan)</li>
          <li>🔐 Gunakan password berbeda untuk setiap akun</li>
          <li>📱 Pertimbangkan menggunakan password manager untuk menyimpan password</li>
        </ul>
      </section>
    </main>

    <!-- Toast Notification -->
    <div v-if="showToast" class="toast-notification" :class="toastMessage.includes('berhasil') ? 'success' : 'error'">
      {{ toastMessage }}
    </div>
  </div>
</template>
```

#### Penjelasan Template:

1. **Header Section**
   - Menampilkan judul dan deskripsi aplikasi
   - Branding dengan emoji untuk visual appeal

2. **Password Display**
   - Menampilkan password yang dihasilkan
   - Copy to clipboard functionality
   - Visual strength indicator dengan animasi

3. **Settings Section**
   - Range slider untuk panjang password
   - Checkboxes untuk opsi karakter
   - Advanced options dalam collapsible section

4. **History Section**
   - Menampilkan 10 password terakhir
   - Metadata untuk setiap password (panjang, kekuatan, waktu)
   - Clear history functionality

5. **Security Tips**
   - Tips keamanan untuk user education
   - Best practices untuk password management

### Langkah 6: Advanced CSS Styling

```vue
<style scoped>
/* Main Container */
.password-generator {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
}

/* Header Styles */
.generator-header {
  text-align: center;
  margin-bottom: 40px;
  padding: 30px 0;
}

.generator-header h1 {
  font-size: 2.5em;
  margin-bottom: 10px;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}

.generator-header p {
  font-size: 1.1em;
  opacity: 0.9;
}

/* Main Content */
.generator-main {
  display: flex;
  flex-direction: column;
  gap: 30px;
}

/* Password Display Section */
.password-display {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.password-output {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-bottom: 20px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 15px;
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

.copy-button {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  padding: 10px 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.2em;
}

.copy-button:hover:not(:disabled) {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

.copy-button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.copy-button.copied {
  background: #4CAF50;
  color: white;
}

/* Strength Indicator */
.strength-indicator {
  margin-top: 15px;
}

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
  font-size: 0.9em;
  text-align: right;
}

/* Settings Section */
.password-settings {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.password-settings h2 {
  margin-bottom: 25px;
  text-align: center;
  font-size: 1.8em;
}

.setting-group {
  margin-bottom: 25px;
}

.setting-group label {
  display: block;
  margin-bottom: 10px;
  font-weight: 600;
  font-size: 1.1em;
}

.value-display {
  color: #4CAF50;
  font-weight: 700;
  font-size: 1.2em;
}

/* Range Slider */
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
  transition: all 0.3s ease;
}

.range-slider::-webkit-slider-thumb:hover {
  transform: scale(1.2);
  background: #45a049;
}

.range-slider::-moz-range-thumb {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #4CAF50;
  cursor: pointer;
  transition: all 0.3s ease;
}

.range-labels {
  display: flex;
  justify-content: space-between;
  font-size: 0.9em;
  opacity: 0.8;
}

/* Checkbox Group */
.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.checkbox-item {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  padding: 10px;
  border-radius: 8px;
  transition: background 0.3s ease;
}

.checkbox-item:hover {
  background: rgba(255,255,255,0.1);
}

.checkbox-item input[type="checkbox"] {
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
  position: relative;
}

.checkbox-item input[type="checkbox"]:checked + .checkmark {
  background: #4CAF50;
  border-color: #4CAF50;
}

.checkbox-item input[type="checkbox"]:checked + .checkmark::after {
  content: '✓';
  color: white;
  font-weight: bold;
  font-size: 14px;
}

/* Advanced Options */
.advanced-options {
  margin-top: 20px;
}

.advanced-options summary {
  cursor: pointer;
  font-weight: 600;
  font-size: 1.1em;
  padding: 10px;
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  transition: background 0.3s ease;
}

.advanced-options summary:hover {
  background: rgba(255,255,255,0.2);
}

.advanced-settings {
  margin-top: 20px;
  padding: 20px;
  background: rgba(0,0,0,0.1);
  border-radius: 10px;
}

/* Text Input */
.text-input {
  width: 100%;
  padding: 12px 15px;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  color: white;
  font-size: 1em;
  transition: all 0.3s ease;
}

.text-input::placeholder {
  color: rgba(255,255,255,0.6);
}

.text-input:focus {
  outline: none;
  border-color: #4CAF50;
  box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.2);
}

/* Action Buttons */
.action-buttons {
  display: flex;
  gap: 15px;
  margin-top: 30px;
  flex-wrap: wrap;
}

.generate-btn {
  flex: 1;
  background: linear-gradient(135deg, #4CAF50, #45a049);
  color: white;
  border: none;
  border-radius: 10px;
  padding: 15px 25px;
  font-size: 1.1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}

.generate-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.3);
}

.reset-btn {
  background: rgba(255,255,255,0.2);
  color: white;
  border: 2px solid rgba(255,255,255,0.3);
  border-radius: 10px;
  padding: 15px 25px;
  font-size: 1.1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.reset-btn:hover {
  background: rgba(255,255,255,0.3);
  transform: translateY(-2px);
}

/* Password History */
.password-history {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.history-header h3 {
  margin: 0;
  font-size: 1.5em;
}

.clear-history-btn {
  background: #e74c3c;
  color: white;
  border: none;
  border-radius: 8px;
  padding: 8px 15px;
  cursor: pointer;
  font-size: 0.9em;
  transition: all 0.3s ease;
}

.clear-history-btn:hover {
  background: #c0392b;
}

.history-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-height: 300px;
  overflow-y: auto;
}

.history-item {
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 15px;
  border-left: 4px solid;
  transition: all 0.3s ease;
}

.history-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateX(5px);
}

.history-item.weak {
  border-left-color: #e74c3c;
}

.history-item.medium {
  border-left-color: #f39c12;
}

.history-item.strong {
  border-left-color: #27ae60;
}

.history-item.very-strong {
  border-left-color: #2ecc71;
}

.history-password {
  font-family: 'Courier New', monospace;
  font-size: 1.1em;
  font-weight: 600;
  margin-bottom: 8px;
  cursor: pointer;
  transition: color 0.3s ease;
}

.history-password:hover {
  color: #4CAF50;
}

.history-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.85em;
  opacity: 0.8;
}

.history-length {
  background: rgba(255,255,255,0.2);
  padding: 2px 8px;
  border-radius: 4px;
}

/* Security Tips */
.security-tips {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  border-radius: 15px;
  padding: 30px;
  border: 1px solid rgba(255,255,255,0.2);
}

.security-tips h3 {
  margin-bottom: 20px;
  text-align: center;
  font-size: 1.5em;
}

.tips-list {
  list-style: none;
  padding: 0;
}

.tips-list li {
  padding: 12px 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
  display: flex;
  align-items: center;
  gap: 10px;
}

.tips-list li:last-child {
  border-bottom: none;
}

/* Toast Notification */
.toast-notification {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 15px 20px;
  border-radius: 10px;
  color: white;
  font-weight: 600;
  z-index: 1000;
  animation: slideIn 0.3s ease;
  max-width: 300px;
}

.toast-notification.success {
  background: #4CAF50;
}

.toast-notification.error {
  background: #e74c3c;
}

@keyframes slideIn {
  from {
    transform: translateX(100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

/* Utility Classes */
.text-center { text-align: center; }
.mb-1 { margin-bottom: 0.5rem; }
.mb-2 { margin-bottom: 1rem; }
.mb-3 { margin-bottom: 1.5rem; }

/* Responsive Design */
@media (max-width: 768px) {
  .password-generator {
    padding: 15px;
    margin: 10px;
  }

  .generator-header h1 {
    font-size: 2em;
  }

  .password-output {
    flex-direction: column;
    gap: 10px;
  }

  .copy-button {
    align-self: stretch;
    text-align: center;
  }

  .action-buttons {
    flex-direction: column;
  }

  .history-header {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }

  .history-meta {
    flex-direction: column;
    gap: 5px;
    align-items: flex-start;
  }

  .tips-list li {
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
  }
}

/* Accessibility Features */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* High Contrast Mode */
@media (prefers-contrast: high) {
  .password-generator {
    background: #000;
    color: #fff;
  }

  .password-display,
  .password-settings,
  .password-history,
  .security-tips {
    border: 2px solid #fff;
  }
}

/* Focus Styles for Keyboard Navigation */
button:focus,
input:focus,
summary:focus {
  outline: 3px solid #4CAF50;
  outline-offset: 2px;
}
</style>
```

### Langkah 7: Helper Functions Implementation

**File: `utils/passwordUtils.js`**
```javascript
// Password strength analysis
export const analyzePasswordStrength = (password) => {
  const analysis = {
    score: 0,
    feedback: [],
    suggestions: []
  };

  if (!password) {
    return { ...analysis, strength: 'none', strengthText: 'Tidak ada password' };
  }

  // Length analysis
  if (password.length >= 16) {
    analysis.score += 4;
  } else if (password.length >= 12) {
    analysis.score += 3;
  } else if (password.length >= 8) {
    analysis.score += 2;
  } else if (password.length >= 6) {
    analysis.score += 1;
  } else {
    analysis.feedback.push('Password terlalu pendek');
    analysis.suggestions.push('Gunakan minimal 8 karakter');
  }

  // Character variety analysis
  const hasLowercase = /[a-z]/.test(password);
  const hasUppercase = /[A-Z]/.test(password);
  const hasNumbers = /\d/.test(password);
  const hasSymbols = /[^a-zA-Z0-9]/.test(password);

  if (hasLowercase) analysis.score += 1;
  else {
    analysis.feedback.push('Tidak ada huruf kecil');
    analysis.suggestions.push('Tambahkan huruf kecil (a-z)');
  }

  if (hasUppercase) analysis.score += 1;
  else {
    analysis.feedback.push('Tidak ada huruf besar');
    analysis.suggestions.push('Tambahkan huruf besar (A-Z)');
  }

  if (hasNumbers) analysis.score += 1;
  else {
    analysis.feedback.push('Tidak ada angka');
    analysis.suggestions.push('Tambahkan angka (0-9)');
  }

  if (hasSymbols) analysis.score += 2;
  else {
    analysis.feedback.push('Tidak ada simbol');
    analysis.suggestions.push('Tambahkan simbol (!@#$%^&*)');
  }

  // Pattern analysis
  const commonPatterns = [
    /123456/,
    /password/i,
    /qwerty/i,
    /abc123/i,
    /admin/i,
    /letmein/i,
    /welcome/i
  ];

  const hasCommonPattern = commonPatterns.some(pattern => pattern.test(password));
  if (hasCommonPattern) {
    analysis.score -= 2;
    analysis.feedback.push('Menggunakan pola yang umum');
    analysis.suggestions.push('Hindari pola password yang umum');
  }

  // Repeated characters analysis
  const repeatedChars = password.match(/(.)\1{2,}/g);
  if (repeatedChars) {
    analysis.score -= 1;
    analysis.feedback.push('Ada karakter yang berulang');
    analysis.suggestions.push('Hindari karakter yang berulang');
  }

  // Determine strength
  let strength, strengthText;
  if (analysis.score >= 8) {
    strength = 'very-strong';
    strengthText = 'Sangat Kuat';
  } else if (analysis.score >= 6) {
    strength = 'strong';
    strengthText = 'Kuat';
  } else if (analysis.score >= 4) {
    strength = 'medium';
    strengthText = 'Sedang';
  } else if (analysis.score >= 2) {
    strength = 'weak';
    strengthText = 'Lemah';
  } else {
    strength = 'very-weak';
    strengthText = 'Sangat Lemah';
  }

  return {
    ...analysis,
    strength,
    strengthText,
    score: Math.max(0, Math.min(10, analysis.score))
  };
};

// Generate password with specific requirements
export const generateSecurePassword = (options = {}) => {
  const {
    length = 12,
    includeUppercase = true,
    includeNumbers = true,
    includeSymbols = true,
    excludeSimilar = false,
    excludeAmbiguous = false,
    excludeDuplicates = false,
    startWithLetter = false,
    customCharacters = ''
  } = options;

  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const uppercaseChars = includeUppercase ? "ABCDEFGHIJKLMNOPQRSTUVWXYZ" : "";
  const numberChars = includeNumbers ? "0123456789" : "";
  const symbolChars = includeSymbols ? "!@#$%^&*()_+-=[]{}|;:,.<>?/~`" : "";

  let allChars = lowercaseChars + uppercaseChars + numberChars + symbolChars + customCharacters;

  // Exclude similar characters
  if (!excludeSimilar) {
    const similarChars = "ilLoO01";
    allChars = allChars.split('').filter(char => !similarChars.includes(char)).join('');
  }

  // Exclude ambiguous characters
  if (!excludeAmbiguous) {
    const ambiguousChars = "{}[]()/'\"`~,;.<>";
    allChars = allChars.split('').filter(char => !ambiguousChars.includes(char)).join('');
  }

  if (allChars.length === 0) {
    throw new Error('Tidak ada karakter yang tersedia untuk generate password');
  }

  let password = '';
  const usedChars = new Set();

  for (let i = 0; i < length; i++) {
    let randomIndex;

    if (excludeDuplicates) {
      let attempts = 0;
      do {
        randomIndex = Math.floor(Math.random() * allChars.length);
        attempts++;
      } while (usedChars.has(allChars[randomIndex]) && attempts < 100);

      if (attempts >= 100) {
        usedChars.clear();
        randomIndex = Math.floor(Math.random() * allChars.length);
      }

      usedChars.add(allChars[randomIndex]);
    } else {
      randomIndex = Math.floor(Math.random() * allChars.length);
    }

    password += allChars[randomIndex];
  }

  // Ensure password starts with letter if required
  if (startWithLetter && !/^[a-zA-Z]/.test(password)) {
    const letters = allChars.match(/[a-zA-Z]/);
    if (letters && letters.length > 0) {
      const firstChar = letters[Math.floor(Math.random() * letters.length)];
      password = firstChar + password.slice(1);
    }
  }

  return password;
};

// Password entropy calculation
export const calculatePasswordEntropy = (password) => {
  if (!password) return 0;

  let charsetSize = 0;

  if (/[a-z]/.test(password)) charsetSize += 26;
  if (/[A-Z]/.test(password)) charsetSize += 26;
  if (/\d/.test(password)) charsetSize += 10;
  if (/[^a-zA-Z0-9]/.test(password)) charsetSize += 32; // Approximate

  const entropy = password.length * Math.log2(charsetSize);
  return Math.round(entropy);
};

// Check if password has been pwned (simulation)
export const checkPasswordBreach = async (password) => {
  // This is a simulation - in real app, you'd use HaveIBeenPwned API
  const commonPasswords = [
    '123456', 'password', '123456789', '12345678', '12345',
    '1234567', '1234567890', 'qwerty', 'abc123', 'password123'
  ];

  return commonPasswords.includes(password.toLowerCase());
};

// Generate multiple passwords
export const generateMultiplePasswords = (count = 5, options = {}) => {
  const passwords = [];

  for (let i = 0; i < count; i++) {
    try {
      const password = generateSecurePassword(options);
      const strength = analyzePasswordStrength(password);

      passwords.push({
        password,
        strength: strength.strength,
        strengthText: strength.strengthText,
        entropy: calculatePasswordEntropy(password)
      });
    } catch (error) {
      console.error('Error generating password:', error);
    }
  }

  return passwords;
};

// Password validation rules
export const validatePasswordRequirements = (password, requirements = {}) => {
  const {
    minLength = 8,
    maxLength = 128,
    requireUppercase = false,
    requireLowercase = false,
    requireNumbers = false,
    requireSymbols = false,
    excludeSimilar = false,
    excludeAmbiguous = false
  } = requirements;

  const errors = [];

  if (password.length < minLength) {
    errors.push(`Password minimal ${minLength} karakter`);
  }

  if (password.length > maxLength) {
    errors.push(`Password maksimal ${maxLength} karakter`);
  }

  if (requireUppercase && !/[A-Z]/.test(password)) {
    errors.push('Password harus mengandung huruf besar');
  }

  if (requireLowercase && !/[a-z]/.test(password)) {
    errors.push('Password harus mengandung huruf kecil');
  }

  if (requireNumbers && !/\d/.test(password)) {
    errors.push('Password harus mengandung angka');
  }

  if (requireSymbols && !/[^a-zA-Z0-9]/.test(password)) {
    errors.push('Password harus mengandung simbol');
  }

  if (excludeSimilar) {
    const similarChars = /[ilLoO01]/;
    if (similarChars.test(password)) {
      errors.push('Password tidak boleh mengandung karakter mirip (i, l, L, o, O, 0, 1)');
    }
  }

  if (excludeAmbiguous) {
    const ambiguousChars = /[{}[\]()\/'"`~,;.<>]/;
    if (ambiguousChars.test(password)) {
      errors.push('Password tidak boleh mengandung karakter ambigu');
    }
  }

  return {
    isValid: errors.length === 0,
    errors
  };
};

export default {
  analyzePasswordStrength,
  generateSecurePassword,
  calculatePasswordEntropy,
  checkPasswordBreach,
  generateMultiplePasswords,
  validatePasswordRequirements
};
```

### Langkah 8: Update App.vue

**File: `App.vue`**
```vue
<script setup>
import PasswordGenerator from './components/PasswordGenerator.vue'
import { onMounted } from 'vue'

// Set page title
onMounted(() => {
  document.title = '🔐 Password Generator - Vue.js'
})
</script>

<template>
  <div id="app">
    <PasswordGenerator />

    <!-- Footer -->
    <footer class="app-footer">
      <div class="footer-content">
        <p>Dibuat dengan ❤️ menggunakan Vue.js 3</p>
        <p>© 2024 Password Generator - Keamanan password Anda adalah prioritas kami</p>
        <div class="footer-links">
          <a href="#" @click.prevent>Privacy Policy</a>
          <span>|</span>
          <a href="#" @click.prevent>Terms of Service</a>
          <span>|</span>
          <a href="#" @click.prevent>Security Guide</a>
        </div>
      </div>
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
  color: #333;
  line-height: 1.6;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

/* Footer Styles */
.app-footer {
  margin-top: auto;
  background: rgba(0, 0, 0, 0.2);
  color: white;
  text-align: center;
  padding: 30px 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-content {
  max-width: 800px;
  margin: 0 auto;
}

.footer-content p {
  margin-bottom: 10px;
  opacity: 0.9;
}

.footer-links {
  margin-top: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.footer-links a {
  color: #4CAF50;
  text-decoration: none;
  transition: color 0.3s ease;
}

.footer-links a:hover {
  color: #45a049;
  text-decoration: underline;
}

/* Responsive Design */
@media (max-width: 768px) {
  .footer-links {
    flex-direction: column;
    gap: 5px;
  }

  .footer-links span {
    display: none;
  }
}

/* Accessibility */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* Print Styles */
@media print {
  .app-footer {
    background: white;
    color: black;
    border-top: 1px solid #ccc;
  }

  .footer-links a {
    color: #4CAF50;
  }
}
</style>
```

### Langkah 9: Menjalankan Aplikasi

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Jalankan Development Server**
   ```bash
   npm run dev
   ```

3. **Buka Browser**
   - Buka alamat yang ditampilkan di terminal (biasanya `http://localhost:5173`)
   - Aplikasi Password Generator siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API dengan `<script setup>`
- Syntax yang lebih ringkas dan modern
- Reaktivitas yang lebih mudah dipahami
- Better TypeScript support

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- State management untuk form inputs
- Dynamic content rendering

### 3. Computed Properties
- Menghitung nilai derived dari reactive data
- Password strength calculation
- Automatic updates saat dependencies berubah

### 4. Event Handling
- `@click` untuk button interactions
- `@input` dan `v-model` untuk form binding
- Custom event handlers untuk copy functionality

### 5. Conditional Rendering
- `v-if` untuk menampilkan password yang sudah dibuat
- Dynamic class binding untuk styling
- Reactive UI updates

### 6. Lifecycle Hooks
- `onMounted` untuk initialization logic
- Load saved settings from localStorage
- Auto-generate password on mount

### 7. Component Architecture
- Single-file components structure
- Props dan events untuk communication
- Reusable component design

### 8. Browser APIs Integration
- Clipboard API untuk copy functionality
- LocalStorage untuk data persistence
- Navigator API untuk device detection

## 🔧 Enhancement Ideas

### 1. Password Strength Analysis
```vue
<script setup>
import { computed } from 'vue';

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
</script>
```

### 2. Password History
```vue
<script setup>
const passwordHistory = ref([]);

const addToHistory = (password) => {
  const timestamp = new Date().toLocaleString();
  passwordHistory.value.unshift({
    password,
    timestamp,
    strength: passwordStrength.value
  });

  // Keep only last 10 passwords
  if (passwordHistory.value.length > 10) {
    passwordHistory.value = passwordHistory.value.slice(0, 10);
  }
};
</script>
```

### 3. Advanced Options
```vue
<script setup>
const excludeSimilar = ref(false);
const excludeAmbiguous = ref(false);
const excludeDuplicates = ref(false);
const startWithLetter = ref(false);
const customCharacters = ref('');
</script>
```

### 4. Batch Password Generation
```vue
<script setup>
const generateMultiplePasswords = (count = 5) => {
  const passwords = [];

  for (let i = 0; i < count; i++) {
    generatePassword();
    passwords.push({
      password: generatedPassword.value,
      strength: passwordStrength.value,
      entropy: calculateEntropy(generatedPassword.value)
    });
  }

  return passwords;
};
</script>
```

### 5. Password Checker API Integration
```vue
<script setup>
const checkPasswordBreached = async (password) => {
  try {
    // Simulate API call to check if password has been breached
    const response = await fetch(`https://api.pwnedpasswords.com/range/${password.substring(0, 5)}`);
    // Process response...
    return false; // or true if breached
  } catch (error) {
    console.error('Error checking password breach:', error);
    return false;
  }
};
</script>
```

## 🐞 Common Issues & Troubleshooting

### Issue 1: Password tidak ter-generate
**Solution**:
- Pastikan ada setidaknya satu opsi karakter yang dipilih
- Cek panjang password (antara 4-32)
- Verify character sets tidak kosong

### Issue 2: Copy to clipboard tidak berfungsi
**Solution**:
- Pastikan browser mendukung Clipboard API
- Cek HTTPS requirement untuk clipboard API
- Implement fallback method untuk older browsers

### Issue 3: LocalStorage tidak bekerja
**Solution**:
- Cek browser storage permissions
- Handle quota exceeded error
- Implement graceful degradation

### Issue 4: Password strength indicator tidak akurat
**Solution**:
- Review strength calculation algorithm
- Add more complexity factors
- Test dengan berbagai password patterns

### Issue 5: Responsive design tidak bekerja
**Solution**:
- Cek CSS media queries
- Verify viewport meta tag
- Test pada berbagai device sizes

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Web Security Guidelines](https://owasp.org/)
- [Password Security Best Practices](https://www.cisa.gov/uscans)
- [Clipboard API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [LocalStorage Guide](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [CSS Grid and Flexbox](https://css-tricks.com/snippets/css/complete-guide-grid/)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Implementasi komponen PasswordGenerator
- [ ] Tambahkan reactive data untuk form inputs
- [ ] Implementasi password generation algorithm
- [ ] Tambahkan password strength indicator
- [ ] Implementasi copy to clipboard functionality
- [ ] Tambahkan password history
- [ ] Implementasi localStorage persistence
- [ ] Tambahkan advanced options
- [ ] Implementasi responsive design
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Password Generator yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:

- **Form handling dan user input validation**
- **Reactive state management**
- **Browser API integration**
- **Security best practices**
- **Data persistence dengan localStorage**
- **User experience design**
- **Component-based architecture**
- **Modern CSS dengan animations dan transitions**

---

**Happy Coding! 🚀**