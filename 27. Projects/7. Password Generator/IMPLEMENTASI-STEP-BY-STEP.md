# 📋 Implementasi Step-by-Step: Password Generator Vue.js

## 🎯 Panduan Lengkap Dari Nol Hingga Production

### 📝 Overview
Panduan ini akan memandu Anda langkah demi langkah dalam membuat aplikasi Password Generator yang lengkap dengan Vue.js 3, mulai dari setup project hingga deployment dengan fitur-fitur security dan user experience yang terbaik.

---

## 🔥 Langkah 1: Persiapan Environment

### 1.1 Install Node.js & npm
```bash
# Cek versi Node.js (minimal 14)
node --version

# Cek versi npm
npm --version

# Jika belum ada, download dari https://nodejs.org/
```

### 1.2 Pilih Development Tool
**Opsi A: Vite (Recommended - Lebih Cepat)**
```bash
npm create vue@latest password-generator-project
```

**Opsi B: Vue CLI (Traditional)**
```bash
npm install -g @vue/cli
vue create password-generator-project
```

### 1.3 Setup Project
```bash
# Masuk ke folder project
cd password-generator-project

# Install dependencies
npm install

# Start development server
npm run dev
```

---

## 🏗️ Langkah 2: Struktur Project Awal

### 2.1 Bersihkan File Default
**Hapus file yang tidak diperlukan:**
- `src/components/HelloWorld.vue`
- `src/assets/` (kosongkan)
- `src/components/TheWelcome.vue`
- `src/components/icons/`

### 2.2 Struktur Folder Baru
```
src/
├── App.vue                 # Main application component
├── main.js                 # Application entry point
├── components/
│   ├── PasswordGenerator.vue   # Password generator component
│   ├── StrengthIndicator.vue   # Password strength component
│   └── PasswordHistory.vue     # Password history component
├── assets/
│   └── styles/
│       └── main.css        # Global styles
├── utils/
│   ├── passwordUtils.js        # Password utility functions
│   ├── storageUtils.js         # Storage helper functions
│   └── validationUtils.js      # Validation functions
└── composables/
    ├── usePassword.js          # Password composable
    ├── useClipboard.js         # Clipboard composable
    └── useLocalStorage.js      # LocalStorage composable
```

---

## 🧩 Langkah 3: Membuat Komponen Dasar

### 3.1 Update main.js
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import './assets/styles/main.css'

createApp(App).mount('#app')
```

### 3.2 Membuat PasswordUtils
**File: `src/utils/passwordUtils.js`**
```javascript
// Character sets untuk password generation
export const CHARACTER_SETS = {
  LOWERCASE: 'abcdefghijklmnopqrstuvwxyz',
  UPPERCASE: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
  NUMBERS: '0123456789',
  SYMBOLS: '!@#$%^&*()_+-=[]{}|;:,.<>?/~`',
  SIMILAR: 'ilLoO01',
  AMBIGUOUS: '{}[]()\/\'"`~,;.<>'
};

// Generate password dengan options
export const generatePassword = (options = {}) => {
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

  // Build character set
  let chars = CHARACTER_SETS.LOWERCASE;

  if (includeUppercase) chars += CHARACTER_SETS.UPPERCASE;
  if (includeNumbers) chars += CHARACTER_SETS.NUMBERS;
  if (includeSymbols) chars += CHARACTER_SETS.SYMBOLS;
  if (customCharacters) chars += customCharacters;

  // Exclude similar characters
  if (excludeSimilar) {
    chars = chars.split('').filter(char =>
      !CHARACTER_SETS.SIMILAR.includes(char)
    ).join('');
  }

  // Exclude ambiguous characters
  if (excludeAmbiguous) {
    chars = chars.split('').filter(char =>
      !CHARACTER_SETS.AMBIGUOUS.includes(char)
    ).join('');
  }

  if (chars.length === 0) {
    throw new Error('Tidak ada karakter yang tersedia');
  }

  // Generate password
  let password = '';
  const usedChars = new Set();

  for (let i = 0; i < length; i++) {
    let randomIndex;

    if (excludeDuplicates) {
      let attempts = 0;
      do {
        randomIndex = Math.floor(Math.random() * chars.length);
        attempts++;
      } while (usedChars.has(chars[randomIndex]) && attempts < 100);

      if (attempts >= 100) {
        usedChars.clear();
      }
    } else {
      randomIndex = Math.floor(Math.random() * chars.length);
    }

    const char = chars[randomIndex];
    password += char;

    if (excludeDuplicates) {
      usedChars.add(char);
    }
  }

  // Ensure starts with letter if required
  if (startWithLetter && !/^[a-zA-Z]/.test(password)) {
    const letters = chars.match(/[a-zA-Z]/g);
    if (letters && letters.length > 0) {
      const firstChar = letters[Math.floor(Math.random() * letters.length)];
      password = firstChar + password.slice(1);
    }
  }

  return password;
};

// Password strength analysis
export const analyzePasswordStrength = (password) => {
  if (!password) return { score: 0, strength: 'none', strengthText: 'Tidak ada password' };

  let score = 0;
  const feedback = [];

  // Length analysis
  if (password.length >= 16) {
    score += 4;
  } else if (password.length >= 12) {
    score += 3;
  } else if (password.length >= 8) {
    score += 2;
  } else if (password.length >= 6) {
    score += 1;
    feedback.push('Password terlalu pendek (minimal 8 karakter)');
  } else {
    feedback.push('Password sangat pendek (minimal 6 karakter)');
  }

  // Character variety analysis
  const hasLowercase = /[a-z]/.test(password);
  const hasUppercase = /[A-Z]/.test(password);
  const hasNumbers = /\d/.test(password);
  const hasSymbols = /[^a-zA-Z0-9]/.test(password);

  if (hasLowercase) score += 1;
  else feedback.push('Tambahkan huruf kecil');

  if (hasUppercase) score += 1;
  else feedback.push('Tambahkan huruf besar');

  if (hasNumbers) score += 1;
  else feedback.push('Tambahkan angka');

  if (hasSymbols) score += 2;
  else feedback.push('Tambahkan simbol');

  // Pattern analysis
  const commonPatterns = [
    /123456/, /password/i, /qwerty/i, /abc123/i,
    /admin/i, /letmein/i, /welcome/i
  ];

  if (commonPatterns.some(pattern => pattern.test(password))) {
    score -= 2;
    feedback.push('Hindari pola password yang umum');
  }

  // Repeated characters analysis
  const repeatedChars = password.match(/(.)\1{2,}/g);
  if (repeatedChars) {
    score -= 1;
    feedback.push('Hindari karakter yang berulang');
  }

  // Determine strength
  let strength, strengthText;
  if (score >= 8) {
    strength = 'very-strong';
    strengthText = 'Sangat Kuat';
  } else if (score >= 6) {
    strength = 'strong';
    strengthText = 'Kuat';
  } else if (score >= 4) {
    strength = 'medium';
    strengthText = 'Sedang';
  } else if (score >= 2) {
    strength = 'weak';
    strengthText = 'Lemah';
  } else {
    strength = 'very-weak';
    strengthText = 'Sangat Lemah';
  }

  return {
    score: Math.max(0, Math.min(10, score)),
    strength,
    strengthText,
    feedback
  };
};

// Calculate password entropy
export const calculatePasswordEntropy = (password) => {
  if (!password) return 0;

  let charsetSize = 0;

  if (/[a-z]/.test(password)) charsetSize += 26;
  if (/[A-Z]/.test(password)) charsetSize += 26;
  if (/\d/.test(password)) charsetSize += 10;
  if (/[^a-zA-Z0-9]/.test(password)) charsetSize += 32;

  const entropy = password.length * Math.log2(charsetSize);
  return Math.round(entropy);
};

// Validate password against requirements
export const validatePasswordRequirements = (password, requirements) => {
  const errors = [];

  if (!requirements) return { isValid: true, errors };

  const {
    minLength = 8,
    maxLength = 128,
    requireUppercase = false,
    requireLowercase = false,
    requireNumbers = false,
    requireSymbols = false
  } = requirements;

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

  return {
    isValid: errors.length === 0,
    errors
  };
};

// Generate multiple passwords with different criteria
export const generatePasswordVariations = (baseOptions = {}) => {
  const variations = [];

  // Standard strong password
  variations.push({
    name: 'Strong Password',
    password: generatePassword({
      ...baseOptions,
      length: 16,
      includeUppercase: true,
      includeNumbers: true,
      includeSymbols: true
    })
  });

  // PIN code
  variations.push({
    name: 'PIN Code',
    password: generatePassword({
      length: 4,
      includeUppercase: false,
      includeNumbers: true,
      includeSymbols: false
    })
  });

  // Passphrase-like
  variations.push({
    name: 'Memorable Password',
    password: generatePassword({
      length: 12,
      includeUppercase: true,
      includeNumbers: true,
      includeSymbols: false,
      excludeSimilar: true,
      excludeAmbiguous: true
    })
  });

  // Maximum security
  variations.push({
    name: 'Maximum Security',
    password: generatePassword({
      length: 32,
      includeUppercase: true,
      includeNumbers: true,
      includeSymbols: true,
      excludeDuplicates: true
    })
  });

  return variations.map(variation => ({
    ...variation,
    strength: analyzePasswordStrength(variation.password),
    entropy: calculatePasswordEntropy(variation.password)
  }));
};

export default {
  generatePassword,
  analyzePasswordStrength,
  calculatePasswordEntropy,
  validatePasswordRequirements,
  generatePasswordVariations,
  CHARACTER_SETS
};
```

### 3.3 Membuat Storage Utils
**File: `src/utils/storageUtils.js`**
```javascript
// LocalStorage helper functions
export const storage = {
  // Get item from localStorage
  get(key, defaultValue = null) {
    try {
      const item = localStorage.getItem(key);
      return item ? JSON.parse(item) : defaultValue;
    } catch (error) {
      console.error('Error getting from localStorage:', error);
      return defaultValue;
    }
  },

  // Set item to localStorage
  set(key, value) {
    try {
      localStorage.setItem(key, JSON.stringify(value));
      return true;
    } catch (error) {
      console.error('Error setting to localStorage:', error);
      return false;
    }
  },

  // Remove item from localStorage
  remove(key) {
    try {
      localStorage.removeItem(key);
      return true;
    } catch (error) {
      console.error('Error removing from localStorage:', error);
      return false;
    }
  },

  // Clear all localStorage
  clear() {
    try {
      localStorage.clear();
      return true;
    } catch (error) {
      console.error('Error clearing localStorage:', error);
      return false;
    }
  },

  // Get storage usage
  getUsage() {
    try {
      let total = 0;
      for (let key in localStorage) {
        if (localStorage.hasOwnProperty(key)) {
          total += localStorage[key].length + key.length;
        }
      }
      return {
        used: total,
        usedKB: (total / 1024).toFixed(2),
        available: 5 * 1024 * 1024, // ~5MB typical limit
        availableKB: ((5 * 1024 * 1024 - total) / 1024).toFixed(2)
      };
    } catch (error) {
      console.error('Error calculating storage usage:', error);
      return { used: 0, usedKB: '0', available: 0, availableKB: '0' };
    }
  }
};

// Password history management
export const passwordHistory = {
  // Get password history
  get(limit = 10) {
    const history = storage.get('passwordHistory', []);
    return history.slice(0, limit);
  },

  // Add password to history
  add(password, metadata = {}) {
    const history = this.get();
    const entry = {
      id: Date.now().toString(),
      password,
      timestamp: new Date().toISOString(),
      strength: metadata.strength || 'unknown',
      length: password.length,
      entropy: metadata.entropy || 0,
      ...metadata
    };

    history.unshift(entry);

    // Keep only last N entries
    const maxHistory = storage.get('settings.maxHistory', 50);
    const trimmedHistory = history.slice(0, maxHistory);

    storage.set('passwordHistory', trimmedHistory);
    return entry;
  },

  // Remove password from history
  remove(id) {
    const history = this.get();
    const filteredHistory = history.filter(item => item.id !== id);
    storage.set('passwordHistory', filteredHistory);
    return filteredHistory;
  },

  // Clear all history
  clear() {
    storage.remove('passwordHistory');
    return true;
  },

  // Export history
  export() {
    const history = this.get();
    const exportData = {
      version: '1.0',
      exportedAt: new Date().toISOString(),
      count: history.length,
      passwords: history
    };
    return exportData;
  },

  // Import history
  import(data) {
    try {
      if (!data.passwords || !Array.isArray(data.passwords)) {
        throw new Error('Invalid import data format');
      }

      const existingHistory = this.get();
      const mergedHistory = [...data.passwords, ...existingHistory];

      // Remove duplicates and sort by timestamp
      const uniqueHistory = mergedHistory.filter((item, index, arr) =>
        arr.findIndex(i => i.password === item.password) === index
      ).sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));

      storage.set('passwordHistory', uniqueHistory);
      return uniqueHistory.length;
    } catch (error) {
      console.error('Error importing history:', error);
      throw error;
    }
  }
};

// Settings management
export const settings = {
  // Get all settings
  getAll() {
    return storage.get('settings', {
      passwordLength: 12,
      includeUppercase: true,
      includeNumbers: true,
      includeSymbols: true,
      excludeSimilar: false,
      excludeAmbiguous: false,
      excludeDuplicates: false,
      startWithLetter: false,
      customCharacters: '',
      maxHistory: 50,
      autoCopy: false,
      showStrength: true
    });
  },

  // Get specific setting
  get(key) {
    const allSettings = this.getAll();
    return allSettings[key];
  },

  // Set specific setting
  set(key, value) {
    const allSettings = this.getAll();
    allSettings[key] = value;
    storage.set('settings', allSettings);
    return allSettings;
  },

  // Update multiple settings
  update(newSettings) {
    const allSettings = { ...this.getAll(), ...newSettings };
    storage.set('settings', allSettings);
    return allSettings;
  },

  // Reset to defaults
  reset() {
    storage.remove('settings');
    return this.getAll();
  }
};

// Backup and restore functionality
export const backup = {
  // Create backup
  create() {
    const backupData = {
      version: '1.0',
      timestamp: new Date().toISOString(),
      settings: settings.getAll(),
      history: passwordHistory.get(),
      storageUsage: storage.getUsage()
    };
    return backupData;
  },

  // Restore from backup
  restore(backupData) {
    try {
      if (!backupData.version) {
        throw new Error('Invalid backup format');
      }

      // Restore settings
      if (backupData.settings) {
        settings.update(backupData.settings);
      }

      // Restore history
      if (backupData.history && Array.isArray(backupData.history)) {
        storage.set('passwordHistory', backupData.history);
      }

      return true;
    } catch (error) {
      console.error('Error restoring backup:', error);
      throw error;
    }
  },

  // Export backup to file
  exportToFile(filename = 'password-generator-backup.json') {
    const backupData = this.create();
    const blob = new Blob([JSON.stringify(backupData, null, 2)], {
      type: 'application/json'
    });

    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  }
};

export default {
  storage,
  passwordHistory,
  settings,
  backup
};
```

### 3.4 Membuat Composables
**File: `src/composables/usePassword.js`**
```javascript
import { ref, computed, watch } from 'vue';
import { generatePassword, analyzePasswordStrength, calculatePasswordEntropy } from '../utils/passwordUtils';
import { settings, passwordHistory } from '../utils/storageUtils';

export function usePassword() {
  // Reactive state
  const passwordLength = ref(12);
  const includeUppercase = ref(true);
  const includeNumbers = ref(true);
  const includeSymbols = ref(true);
  const excludeSimilar = ref(false);
  const excludeAmbiguous = ref(false);
  const excludeDuplicates = ref(false);
  const startWithLetter = ref(false);
  const customCharacters = ref('');

  // Generated password
  const generatedPassword = ref('');
  const isGenerating = ref(false);
  const lastGeneratedAt = ref(null);

  // Computed properties
  const passwordOptions = computed(() => ({
    length: passwordLength.value,
    includeUppercase: includeUppercase.value,
    includeNumbers: includeNumbers.value,
    includeSymbols: includeSymbols.value,
    excludeSimilar: excludeSimilar.value,
    excludeAmbiguous: excludeAmbiguous.value,
    excludeDuplicates: excludeDuplicates.value,
    startWithLetter: startWithLetter.value,
    customCharacters: customCharacters.value
  }));

  const passwordStrength = computed(() => {
    if (!generatedPassword.value) return null;
    return analyzePasswordStrength(generatedPassword.value);
  });

  const passwordEntropy = computed(() => {
    if (!generatedPassword.value) return 0;
    return calculatePasswordEntropy(generatedPassword.value);
  });

  const canGenerate = computed(() => {
    return passwordLength.value >= 4 &&
           passwordLength.value <= 32 &&
           (
             includeUppercase.value ||
             includeNumbers.value ||
             includeSymbols.value ||
             customCharacters.value.length > 0
           );
  });

  // Methods
  const generateNewPassword = async (options = null) => {
    if (!canGenerate.value && !options) {
      throw new Error('Cannot generate password with current settings');
    }

    isGenerating.value = true;

    try {
      // Add small delay for better UX
      await new Promise(resolve => setTimeout(resolve, 100));

      const finalOptions = options || passwordOptions.value;
      const newPassword = generatePassword(finalOptions);

      generatedPassword.value = newPassword;
      lastGeneratedAt.value = new Date().toISOString();

      // Add to history
      const strength = analyzePasswordStrength(newPassword);
      const entropy = calculatePasswordEntropy(newPassword);

      passwordHistory.add(newPassword, {
        strength: strength.strength,
        score: strength.score,
        entropy,
        options: finalOptions
      });

      // Save current settings
      saveSettings();

      return newPassword;
    } catch (error) {
      console.error('Error generating password:', error);
      throw error;
    } finally {
      isGenerating.value = false;
    }
  };

  const generateMultiplePasswords = async (count = 5) => {
    const passwords = [];

    for (let i = 0; i < count; i++) {
      try {
        const password = await generateNewPassword();
        passwords.push({
          password,
          strength: passwordStrength.value,
          entropy: passwordEntropy.value,
          timestamp: lastGeneratedAt.value
        });
      } catch (error) {
        console.error(`Error generating password ${i + 1}:`, error);
      }
    }

    return passwords;
  };

  const validateSettings = () => {
    const errors = [];

    if (passwordLength.value < 4) {
      errors.push('Panjang password minimal 4 karakter');
    }

    if (passwordLength.value > 32) {
      errors.push('Panjang password maksimal 32 karakter');
    }

    if (!includeUppercase.value &&
        !includeNumbers.value &&
        !includeSymbols.value &&
        customCharacters.value.length === 0) {
      errors.push('Pilih setidaknya satu jenis karakter');
    }

    if (customCharacters.value && customCharacters.value.length > 0) {
      const uniqueChars = new Set(customCharacters.value).size;
      if (uniqueChars < customCharacters.value.length) {
        errors.push('Karakter kustom tidak boleh duplikat');
      }
    }

    return {
      isValid: errors.length === 0,
      errors
    };
  };

  const saveSettings = () => {
    settings.update({
      passwordLength: passwordLength.value,
      includeUppercase: includeUppercase.value,
      includeNumbers: includeNumbers.value,
      includeSymbols: includeSymbols.value,
      excludeSimilar: excludeSimilar.value,
      excludeAmbiguous: excludeAmbiguous.value,
      excludeDuplicates: excludeDuplicates.value,
      startWithLetter: startWithLetter.value,
      customCharacters: customCharacters.value
    });
  };

  const loadSettings = () => {
    const savedSettings = settings.getAll();

    passwordLength.value = savedSettings.passwordLength || 12;
    includeUppercase.value = savedSettings.includeUppercase !== false;
    includeNumbers.value = savedSettings.includeNumbers !== false;
    includeSymbols.value = savedSettings.includeSymbols !== false;
    excludeSimilar.value = savedSettings.excludeSimilar || false;
    excludeAmbiguous.value = savedSettings.excludeAmbiguous || false;
    excludeDuplicates.value = savedSettings.excludeDuplicates || false;
    startWithLetter.value = savedSettings.startWithLetter || false;
    customCharacters.value = savedSettings.customCharacters || '';
  };

  const resetSettings = () => {
    passwordLength.value = 12;
    includeUppercase.value = true;
    includeNumbers.value = true;
    includeSymbols.value = true;
    excludeSimilar.value = false;
    excludeAmbiguous.value = false;
    excludeDuplicates.value = false;
    startWithLetter.value = false;
    customCharacters.value = '';

    settings.reset();
  };

  // Auto-save settings when they change
  watch([
    passwordLength,
    includeUppercase,
    includeNumbers,
    includeSymbols,
    excludeSimilar,
    excludeAmbiguous,
    excludeDuplicates,
    startWithLetter,
    customCharacters
  ], () => {
    saveSettings();
  }, { deep: true });

  // Load settings on composable creation
  loadSettings();

  return {
    // State
    passwordLength,
    includeUppercase,
    includeNumbers,
    includeSymbols,
    excludeSimilar,
    excludeAmbiguous,
    excludeDuplicates,
    startWithLetter,
    customCharacters,
    generatedPassword,
    isGenerating,
    lastGeneratedAt,

    // Computed
    passwordOptions,
    passwordStrength,
    passwordEntropy,
    canGenerate,

    // Methods
    generateNewPassword,
    generateMultiplePasswords,
    validateSettings,
    saveSettings,
    loadSettings,
    resetSettings
  };
}
```

**File: `src/composables/useClipboard.js`**
```javascript
import { ref } from 'vue';

export function useClipboard() {
  const copiedText = ref('');
  const isCopied = ref(false);
  const error = ref(null);

  const copy = async (text) => {
    try {
      error.value = null;

      // Try modern Clipboard API first
      if (navigator.clipboard && window.isSecureContext) {
        await navigator.clipboard.writeText(text);
      } else {
        // Fallback for older browsers
        await fallbackCopyTextToClipboard(text);
      }

      copiedText.value = text;
      isCopied.value = true;

      // Reset copied state after 2 seconds
      setTimeout(() => {
        isCopied.value = false;
      }, 2000);

      return true;
    } catch (err) {
      error.value = err.message || 'Failed to copy text';
      console.error('Copy to clipboard failed:', err);
      return false;
    }
  };

  const fallbackCopyTextToClipboard = (text) => {
    return new Promise((resolve, reject) => {
      const textArea = document.createElement('textarea');
      textArea.value = text;

      // Avoid scrolling to bottom
      textArea.style.top = '0';
      textArea.style.left = '0';
      textArea.style.position = 'fixed';

      document.body.appendChild(textArea);
      textArea.focus();
      textArea.select();

      try {
        const successful = document.execCommand('copy');
        if (successful) {
          resolve();
        } else {
          reject(new Error('Fallback copy failed'));
        }
      } catch (err) {
        reject(err);
      }

      document.body.removeChild(textArea);
    });
  };

  const copyPassword = async (password) => {
    if (!password) {
      error.value = 'No password to copy';
      return false;
    }

    return await copy(password);
  };

  const clearError = () => {
    error.value = null;
  };

  return {
    copiedText,
    isCopied,
    error,
    copy,
    copyPassword,
    clearError
  };
}
```

---

## 🎨 Langkah 4: Membuat Komponen UI

### 4.1 PasswordGenerator Component
**File: `src/components/PasswordGenerator.vue`**
```vue
<script setup>
import { ref, computed, onMounted } from 'vue';
import { usePassword } from '../composables/usePassword';
import { useClipboard } from '../composables/useClipboard';
import { passwordHistory } from '../utils/storageUtils';

// Use composables
const {
  passwordLength,
  includeUppercase,
  includeNumbers,
  includeSymbols,
  excludeSimilar,
  excludeAmbiguous,
  excludeDuplicates,
  startWithLetter,
  customCharacters,
  generatedPassword,
  isGenerating,
  passwordStrength,
  passwordEntropy,
  canGenerate,
  generateNewPassword,
  generateMultiplePasswords,
  validateSettings,
  resetSettings
} = usePassword();

const {
  copyPassword,
  isCopied,
  error: clipboardError
} = useClipboard();

// Local state
const showAdvanced = ref(false);
const showHistory = ref(false);
const showToast = ref(false);
const toastMessage = ref('');
const multiplePasswords = ref([]);

// Computed properties
const strengthColor = computed(() => {
  const colors = {
    'very-weak': '#e74c3c',
    'weak': '#e67e22',
    'medium': '#f39c12',
    'strong': '#27ae60',
    'very-strong': '#2ecc71'
  };
  return passwordStrength.value ? colors[passwordStrength.value.strength] : '#ccc';
});

const strengthPercentage = computed(() => {
  return passwordStrength.value ? (passwordStrength.value.score / 10) * 100 : 0;
});

const history = computed(() => {
  return passwordHistory.get(10);
});

// Methods
const handleGenerate = async () => {
  try {
    await generateNewPassword();
    showSuccess('Password berhasil dibuat!');
  } catch (error) {
    showError('Gagal membuat password: ' + error.message);
  }
};

const handleCopy = async () => {
  if (!generatedPassword.value) return;

  try {
    const success = await copyPassword(generatedPassword.value);
    if (success) {
      showSuccess('Password berhasil disalin!');
    } else {
      showError('Gagal menyalin password');
    }
  } catch (error) {
    showError('Gagal menyalin password: ' + error.message);
  }
};

const handleGenerateMultiple = async () => {
  try {
    multiplePasswords.value = await generateMultiplePasswords(5);
    showSuccess('Berhasil membuat 5 password!');
  } catch (error) {
    showError('Gagal membuat password: ' + error.message);
  }
};

const copyFromHistory = async (password) => {
  try {
    const success = await copyPassword(password);
    if (success) {
      showSuccess('Password dari history berhasil disalin!');
    }
  } catch (error) {
    showError('Gagal menyalin password');
  }
};

const clearHistory = () => {
  if (confirm('Apakah Anda yakin ingin menghapus semua history?')) {
    passwordHistory.clear();
    showSuccess('History berhasil dihapus!');
  }
};

const showSuccess = (message) => {
  toastMessage.value = message;
  showToast.value = true;
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
};

const showError = (message) => {
  toastMessage.value = message;
  showToast.value = true;
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
};

const formatTime = (timestamp) => {
  const date = new Date(timestamp);
  return date.toLocaleTimeString('id-ID', {
    hour: '2-digit',
    minute: '2-digit'
  });
};

const formatDate = (timestamp) => {
  const date = new Date(timestamp);
  return date.toLocaleDateString('id-ID');
};

// Generate initial password
onMounted(async () => {
  await handleGenerate();
});
</script>

<template>
  <div class="password-generator">
    <!-- Toast Notification -->
    <div v-if="showToast" class="toast" :class="{ 'error': toastMessage.includes('Gagal') }">
      {{ toastMessage }}
    </div>

    <!-- Header -->
    <header class="generator-header">
      <h1>🔐 Password Generator</h1>
      <p>Buat password yang aman dan kuat dengan mudah</p>
    </header>

    <!-- Password Display -->
    <section class="password-display">
      <div class="password-output">
        <div class="password-text" :class="{ 'copied': isCopied }">
          {{ generatedPassword || 'Klik Generate untuk membuat password' }}
        </div>
        <button
          @click="handleCopy"
          class="copy-button"
          :disabled="!generatedPassword"
          :class="{ 'copied': isCopied }"
        >
          <span v-if="isCopied">✓</span>
          <span v-else>📋</span>
        </button>
      </div>

      <!-- Strength Indicator -->
      <div v-if="generatedPassword" class="strength-indicator">
        <div class="strength-bar">
          <div
            class="strength-fill"
            :style="{
              width: strengthPercentage + '%',
              backgroundColor: strengthColor
            }"
          ></div>
        </div>
        <div class="strength-info">
          <span class="strength-text" :style="{ color: strengthColor }">
            Kekuatan: {{ passwordStrength?.strengthText }}
          </span>
          <span class="entropy-text">
            Entropy: {{ passwordEntropy }} bits
          </span>
        </div>
      </div>
    </section>

    <!-- Settings -->
    <section class="settings-section">
      <h2>⚙️ Pengaturan Password</h2>

      <!-- Password Length -->
      <div class="setting-group">
        <label for="passwordLength">
          Panjang Password: <strong>{{ passwordLength }}</strong>
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
        <h3>Karakter yang Disertakan:</h3>

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
      <details class="advanced-options" v-model:open="showAdvanced">
        <summary>🔧 Opsi Lanjutan</summary>

        <div class="advanced-settings">
          <div class="checkbox-group">
            <label class="checkbox-item">
              <input type="checkbox" v-model="excludeSimilar" />
              <span class="checkmark"></span>
              <span>Exclude karakter mirip (i, l, 1, L, o, 0, O)</span>
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
        <button
          @click="handleGenerate"
          :disabled="!canGenerate || isGenerating"
          class="btn-primary"
        >
          <span v-if="isGenerating">🔄 Membuat...</span>
          <span v-else>🎲 Generate Password</span>
        </button>

        <button @click="handleGenerateMultiple" class="btn-secondary">
          📦 Generate 5 Password
        </button>

        <button @click="resetSettings" class="btn-tertiary">
          🔄 Reset
        </button>
      </div>
    </section>

    <!-- Multiple Passwords -->
    <section v-if="multiplePasswords.length > 0" class="multiple-passwords">
      <h3>📦 Password yang Dibuat:</h3>
      <div class="password-list">
        <div
          v-for="(item, index) in multiplePasswords"
          :key="index"
          class="password-item"
          @click="copyPassword(item.password)"
        >
          <div class="password-content">
            <span class="password-text">{{ item.password }}</span>
            <div class="password-meta">
              <span class="strength-badge" :style="{ backgroundColor: strengthColor }">
                {{ item.strength.strengthText }}
              </span>
              <span class="entropy-badge">
                {{ item.entropy }} bits
              </span>
            </div>
          </div>
          <button class="copy-small-btn">📋</button>
        </div>
      </div>
    </section>

    <!-- History -->
    <section v-if="history.length > 0" class="history-section">
      <div class="history-header">
        <h3 @click="showHistory = !showHistory">
          📜 History Password ({{ history.length }})
        </h3>
        <button @click="clearHistory" class="clear-history-btn">
          🗑️ Hapus Semua
        </button>
      </div>

      <div v-if="showHistory" class="history-list">
        <div
          v-for="(item, index) in history"
          :key="item.id || index"
          class="history-item"
          @click="copyFromHistory(item.password)"
        >
          <div class="history-password">{{ item.password }}</div>
          <div class="history-meta">
            <span class="history-length">{{ item.length }} karakter</span>
            <span class="history-strength" :style="{ color: getStrengthColor(item.strength) }">
              {{ getStrengthText(item.strength) }}
            </span>
            <span class="history-time" :title="formatDate(item.timestamp)">
              {{ formatTime(item.timestamp) }}
            </span>
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
        <li>🚫 Hindari menggunakan informasi pribadi dalam password</li>
        <li>🔄 Ganti password secara berkala (setiap 3-6 bulan)</li>
        <li>🔐 Gunakan password berbeda untuk setiap akun penting</li>
        <li>📱 Pertimbangkan menggunakan password manager untuk menyimpan password</li>
      </ul>
    </section>
  </div>
</template>

<style scoped>
/* Styles akan ditambahkan di bagian selanjutnya */
</style>
```

### 4.2 CSS Styling untuk PasswordGenerator
```vue
<style scoped>
/* Main Container */
.password-generator {
  max-width: 700px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  color: white;
}

/* Toast Notification */
.toast {
  position: fixed;
  top: 20px;
  right: 20px;
  padding: 15px 20px;
  border-radius: 10px;
  background: #4CAF50;
  color: white;
  font-weight: 600;
  z-index: 1000;
  animation: slideIn 0.3s ease;
  max-width: 300px;
}

.toast.error {
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

/* Header */
.generator-header {
  text-align: center;
  margin-bottom: 30px;
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

.copy-button {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 8px;
  padding: 10px 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1.2em;
  color: white;
  min-width: 50px;
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

.strength-info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.strength-text {
  font-weight: 600;
}

.entropy-text {
  font-size: 0.9em;
  opacity: 0.8;
}

/* Settings */
.settings-section h2 {
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
  font-size: 1.1em;
}

.setting-group strong {
  color: #4CAF50;
  font-weight: 700;
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
  gap: 12px;
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
  padding: 12px;
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  transition: background 0.3s ease;
  margin-bottom: 15px;
}

.advanced-options summary:hover {
  background: rgba(255,255,255,0.2);
}

.advanced-settings {
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
  margin-top: 25px;
  flex-wrap: wrap;
}

.btn-primary, .btn-secondary, .btn-tertiary {
  flex: 1;
  padding: 15px 20px;
  border: none;
  border-radius: 10px;
  font-size: 1em;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 150px;
}

.btn-primary {
  background: linear-gradient(135deg, #4CAF50, #45a049);
  color: white;
}

.btn-primary:disabled {
  background: #666;
  cursor: not-allowed;
  opacity: 0.6;
}

.btn-secondary {
  background: rgba(255,255,255,0.2);
  color: white;
  border: 2px solid rgba(255,255,255,0.3);
}

.btn-tertiary {
  background: transparent;
  color: white;
  border: 2px solid rgba(255,255,255,0.3);
}

.btn-primary:hover:not(:disabled),
.btn-secondary:hover,
.btn-tertiary:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 15px rgba(0,0,0,0.3);
}

/* Multiple Passwords */
.multiple-passwords h3 {
  margin-bottom: 20px;
  color: #4CAF50;
}

.password-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.password-item {
  display: flex;
  align-items: center;
  gap: 15px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.password-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateX(5px);
}

.password-content {
  flex: 1;
}

.password-meta {
  display: flex;
  gap: 10px;
  margin-top: 5px;
}

.strength-badge, .entropy-badge {
  font-size: 0.8em;
  padding: 2px 8px;
  border-radius: 4px;
  background: rgba(255,255,255,0.2);
}

.copy-small-btn {
  background: rgba(255,255,255,0.2);
  border: none;
  border-radius: 6px;
  padding: 8px 12px;
  cursor: pointer;
  font-size: 1.1em;
  transition: all 0.3s ease;
}

.copy-small-btn:hover {
  background: rgba(255,255,255,0.3);
}

/* History */
.history-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.history-header h3 {
  margin: 0;
  cursor: pointer;
  transition: color 0.3s ease;
}

.history-header h3:hover {
  color: #4CAF50;
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
  max-height: 300px;
  overflow-y: auto;
}

.history-item {
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  padding: 12px;
  margin-bottom: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  border-left: 4px solid;
}

.history-item:hover {
  background: rgba(0,0,0,0.3);
  transform: translateX(5px);
}

.history-item.very-strong {
  border-left-color: #2ecc71;
}

.history-item.strong {
  border-left-color: #27ae60;
}

.history-item.medium {
  border-left-color: #f39c12;
}

.history-item.weak {
  border-left-color: #e67e22;
}

.history-item.very-weak {
  border-left-color: #e74c3c;
}

.history-password {
  font-family: 'Courier New', monospace;
  font-size: 1.1em;
  font-weight: 600;
  margin-bottom: 8px;
}

.history-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.85em;
  opacity: 0.8;
  flex-wrap: wrap;
  gap: 5px;
}

.history-length {
  background: rgba(255,255,255,0.2);
  padding: 2px 6px;
  border-radius: 4px;
}

/* Security Tips */
.security-tips h3 {
  margin-bottom: 15px;
  text-align: center;
  color: #4CAF50;
}

.tips-list {
  list-style: none;
  padding: 0;
}

.tips-list li {
  padding: 12px 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
  display: flex;
  align-items: flex-start;
  gap: 10px;
  line-height: 1.5;
}

.tips-list li:last-child {
  border-bottom: none;
}

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
    align-items: flex-start;
    gap: 3px;
  }

  .strength-info {
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
  }

  .password-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 5px;
  }

  .password-item {
    flex-direction: column;
    align-items: stretch;
    gap: 10px;
  }

  .copy-small-btn {
    align-self: flex-end;
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

/* High Contrast Mode */
@media (prefers-contrast: high) {
  .password-generator {
    background: #000;
    color: #fff;
  }

  section {
    border: 2px solid #fff;
  }
}

/* Focus Styles */
button:focus,
input:focus,
summary:focus {
  outline: 3px solid #4CAF50;
  outline-offset: 2px;
}
</style>
```

---

## 📱 Langkah 5: Responsive Design & Accessibility

### 5.1 Mobile-First CSS
```css
/* Base styles mobile-first */
.password-generator {
  max-width: 100%;
  padding: 1rem;
}

/* Tablet styles */
@media (min-width: 768px) {
  .password-generator {
    max-width: 700px;
    margin: 0 auto;
    padding: 2rem;
  }
}

/* Desktop styles */
@media (min-width: 1024px) {
  .password-generator {
    padding: 3rem;
  }

  .action-buttons {
    flex-direction: row;
  }
}
```

### 5.2 Touch-Friendly Interactions
```vue
<script setup>
// Touch gesture support
const touchStartX = ref(0);
const touchEndX = ref(0);

const handleTouchStart = (e) => {
  touchStartX.value = e.changedTouches[0].screenX;
};

const handleTouchEnd = (e) => {
  touchEndX.value = e.changedTouches[0].screenX;
  handleSwipe();
};

const handleSwipe = () => {
  const swipeDistance = touchEndX.value - touchStartX.value;

  if (Math.abs(swipeDistance) > 50) {
    if (swipeDistance > 0) {
      // Swipe right - copy password
      handleCopy();
    } else {
      // Swipe left - generate new password
      handleGenerate();
    }
  }
};
</script>

<template>
  <div
    class="password-generator"
    @touchstart="handleTouchStart"
    @touchend="handleTouchEnd"
  >
    <!-- content -->
  </div>
</template>
```

### 5.3 Accessibility Features
```vue
<template>
  <div class="password-generator">
    <!-- ARIA labels and roles -->
    <main role="main" aria-label="Password Generator">
      <section aria-label="Generated Password">
        <div
          role="textbox"
          aria-label="Generated password"
          aria-live="polite"
          class="password-text"
          tabindex="0"
        >
          {{ generatedPassword }}
        </div>

        <button
          @click="handleCopy"
          :aria-label="isCopied ? 'Password copied to clipboard' : 'Copy password to clipboard'"
          :aria-pressed="isCopied"
          class="copy-button"
        >
          {{ isCopied ? '✓' : '📋' }}
        </button>
      </section>

      <!-- Keyboard navigation -->
      <section aria-label="Password Settings">
        <label for="passwordLength">
          Password Length:
          <span aria-live="polite">{{ passwordLength }}</span>
        </label>
        <input
          id="passwordLength"
          type="range"
          v-model="passwordLength"
          min="4"
          max="32"
          aria-describedby="length-description"
        />
        <div id="length-description" class="sr-only">
          Use arrow keys to adjust password length between 4 and 32 characters
        </div>
      </section>
    </main>
  </div>
</template>

<style scoped>
/* Screen reader only content */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* High contrast mode support */
@media (prefers-contrast: high) {
  .password-generator {
    background: #000;
    color: #fff;
  }

  .btn-primary {
    border: 3px solid #fff;
  }
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
</style>
```

---

## 🧪 Langkah 6: Testing & Quality Assurance

### 6.1 Manual Testing Checklist
```markdown
## Testing Checklist

### Core Functionality
- [ ] Password generation works with all settings
- [ ] Copy to clipboard functions correctly
- [ ] Password strength indicator updates properly
- [ ] Settings are saved to localStorage
- [ ] History saves and displays correctly
- [ ] Multiple password generation works

### Edge Cases
- [ ] Minimum length (4) works
- [ ] Maximum length (32) works
- [ ] No character types selected (should show error)
- [ ] Custom characters work correctly
- [ ] Exclude duplicates works
- [ ] Exclude similar characters works

### User Experience
- [ ] Responsive design works on mobile
- [ ] Touch gestures work on mobile
- [ ] Keyboard navigation works
- [ ] Screen reader compatibility
- [ ] High contrast mode support
- [ ] Reduced motion support

### Performance
- [ ] Fast password generation
- [ ] Smooth animations
- [ ] No memory leaks
- [ ] Efficient localStorage usage
```

### 6.2 Unit Tests (Optional)
**File: `tests/passwordUtils.test.js`**
```javascript
import { describe, it, expect } from 'vitest';
import { generatePassword, analyzePasswordStrength } from '../src/utils/passwordUtils.js';

describe('Password Utils', () => {
  describe('generatePassword', () => {
    it('should generate password with default options', () => {
      const password = generatePassword();
      expect(password).toHaveLength(12);
    });

    it('should generate password with custom length', () => {
      const password = generatePassword({ length: 8 });
      expect(password).toHaveLength(8);
    });

    it('should include uppercase when specified', () => {
      const password = generatePassword({
        length: 100,
        includeUppercase: true,
        includeNumbers: false,
        includeSymbols: false
      });
      expect(password).toMatch(/[A-Z]/);
    });

    it('should exclude similar characters when specified', () => {
      const password = generatePassword({
        length: 100,
        excludeSimilar: true
      });
      expect(password).not.toMatch(/[ilLoO01]/);
    });
  });

  describe('analyzePasswordStrength', () => {
    it('should return weak for simple password', () => {
      const result = analyzePasswordStrength('abc');
      expect(result.strength).toBe('very-weak');
    });

    it('should return strong for complex password', () => {
      const result = analyzePasswordStrength('Abc123!@#');
      expect(result.strength).toMatch(/strong|very-strong/);
    });
  });
});
```

---

## 🚀 Langkah 7: Deployment & Production

### 7.1 Build Configuration
**File: `vite.config.js`**
```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          'vendor': ['vue'],
          'utils': ['./src/utils/passwordUtils.js']
        }
      }
    },
    minify: 'terser',
    sourcemap: false,
    chunkSizeWarningLimit: 1000
  },
  server: {
    host: true,
    port: 3000
  },
  preview: {
    port: 4173,
    host: true
  }
})
```

### 7.2 Environment Variables
**File: `.env.production`**
```
VITE_APP_TITLE=Password Generator
VITE_APP_VERSION=1.0.0
VITE_MAX_PASSWORD_LENGTH=64
VITE_DEFAULT_PASSWORD_LENGTH=16
```

### 7.3 Deployment Scripts
**File: `package.json` (add scripts)**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "build:analyze": "vite build --mode analyze",
    "deploy:netlify": "npm run build && netlify deploy --prod --dir=dist",
    "deploy:vercel": "npm run build && vercel --prod",
    "test": "vitest",
    "test:coverage": "vitest --coverage"
  }
}
```

### 7.4 PWA Configuration (Optional)
**File: `vite.config.js` (add PWA plugin)**
```javascript
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    vue(),
    VitePWA({
      registerType: 'autoUpdate',
      includeAssets: ['favicon.ico', 'apple-touch-icon.png'],
      manifest: {
        name: 'Password Generator',
        short_name: 'PassGen',
        description: 'Generate secure passwords',
        theme_color: '#4CAF50',
        background_color: '#ffffff',
        display: 'standalone',
        icons: [
          {
            src: 'icon-192x192.png',
            sizes: '192x192',
            type: 'image/png'
          },
          {
            src: 'icon-512x512.png',
            sizes: '512x512',
            type: 'image/png'
          }
        ]
      }
    })
  ]
})
```

---

## ✅ Final Review & Checklist

### Implementation Complete Checklist
- [ ] Basic password generation functionality
- [ ] Advanced generation options
- [ ] Password strength indicator
- [ ] Copy to clipboard functionality
- [ ] Password history with localStorage
- [ ] Responsive design for all devices
- [ ] Touch and keyboard navigation
- [ ] Accessibility features
- [ ] Performance optimization
- [ ] Production build configuration
- [ ] Deployment setup
- [ ] Documentation complete

### Code Quality Checklist
- [ ] Vue.js 3 Composition API best practices
- [ ] Component reusability and modularity
- [ ] Proper error handling
- [ ] TypeScript-ready structure (optional)
- [ ] Code follows naming conventions
- [ ] Proper file organization
- [ ] Efficient use of computed properties
- [ ] Memory leak prevention

### Security Checklist
- [ ] No sensitive data in localStorage
- [ ] Proper input validation
- [ ] HTTPS requirement for clipboard API
- [ ] No hardcoded secrets
- [ ] Secure random number generation
- [ ] XSS prevention

### User Experience Checklist
- [ ] Intuitive interface design
- [ ] Clear visual feedback
- [ ] Fast response times
- [ ] Accessible to all users
- [ ] Mobile-friendly
- [ ] Helpful error messages

---

## 🎉 Selamat! Implementation Selesai

Anda telah berhasil membuat aplikasi Password Generator yang lengkap dengan:

✅ **Advanced password generation algorithms**
✅ **Real-time strength analysis**
✅ **Clipboard integration with fallbacks**
✅ **Password history with persistence**
✅ **Responsive and accessible design**
✅ **Touch and keyboard navigation**
✅ **Performance optimization**
✅ **Production-ready deployment**

### Next Steps
1. **Add password breach checking** with HaveIBeenPwned API
2. **Implement password templates** for common use cases
3. **Add export/import functionality** for settings and history
4. **Create browser extension** version
5. **Add biometric authentication** for accessing history
6. **Implement multi-language support**

### Resources for Further Learning
- [Vue.js Documentation](https://vuejs.org/)
- [Web Security Guidelines](https://owasp.org/)
- [Clipboard API Documentation](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API)
- [Web App Manifest](https://developer.mozilla.org/en-US/docs/Web/Manifest)
- [PWA Best Practices](https://web.dev/progressive-web-apps/)

---

**Happy Coding! 🚀**