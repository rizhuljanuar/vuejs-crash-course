# Progress Steps - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Progress Steps adalah aplikasi web yang menampilkan komponen progress indicator (indikator kemajuan) interaktif menggunakan Vue.js 3 dengan Composition API. Aplikasi ini mendemonstrasikan bagaimana membuat progress steps yang umum digunakan dalam multi-step forms, wizards, atau proses yang berurutan. Project ini sangat berguna untuk memahami state management dan user flow navigation.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- Array manipulation dan indexing
- Conditional rendering dan class binding
- Event handling untuk navigation
- State management untuk progress tracking
- Dynamic styling berdasarkan state
- Component-based architecture
- User experience design untuk progress indicators

## 📋 Fitur Aplikasi

1. **Visual Progress Bar**: Indikator visual untuk menunjukkan progress
2. **Active Step Highlighting**: Menandai step yang sedang aktif
3. **Navigation Controls**: Tombol Previous/Next untuk navigasi
4. **Disabled States**: Otomatis disable button di batasan step
5. **Dynamic Styling**: Perubahan visual berdasarkan state
6. **Responsive Design**: Layout yang bekerja di semua device sizes
7. **Step Completion Tracking**: Melacak kemajuan proses

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)

## 📂 Struktur Project

```
6. Progress Steps/
├── App.vue                              # Komponen utama aplikasi
├── components/
│   └── ProgressSteps.vue                # Komponen untuk progress steps
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
npm create vue@latest progress-steps-app
cd progress-steps-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create progress-steps-app
cd progress-steps-app
```

### Langkah 3: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style scoped>`**: Bagian CSS untuk styling

### Langkah 4: Membuat Komponen ProgressSteps

**File: `components/ProgressSteps.vue`**

#### Bagian Script Setup
```vue
<script setup>
import { ref } from "vue";

// Array untuk menyimpan nama-nama step
const steps = ref(['Step 1', 'Step 2', 'Step 3']);

// Reactive variable untuk melacak step saat ini
const currentStep = ref(0);

// Fungsi untuk pindah ke step berikutnya
const nextStep = () => {
  // Validasi apakah masih ada step berikutnya
  if (currentStep.value < steps.value.length - 1) {
    currentStep.value++;
  }
};

// Fungsi untuk pindah ke step sebelumnya
const prevStep = () => {
  // Validasi apakah masih ada step sebelumnya
  if (currentStep.value > 0) {
    currentStep.value--;
  }
};

// Computed property untuk persentase progress
const progressPercentage = computed(() => {
  return ((currentStep.value + 1) / steps.value.length) * 100;
});

// Computed property untuk status navigasi
const canGoNext = computed(() => {
  return currentStep.value < steps.value.length - 1;
});

const canGoPrev = computed(() => {
  return currentStep.value > 0;
});
</script>
```

#### Penjelasan Kode:

1. **`import { ref } from "vue"`**
   - Mengimpor `ref` untuk membuat reactive variables
   - `ref` membuat data yang otomatis update UI saat berubah

2. **`const steps = ref([...])`**
   - Array reactive untuk menyimpan nama steps
   - Bisa dengan mudah ditambah atau diubah

3. **`const currentStep = ref(0)`**
   - Reactive variable untuk melacak step aktif
   - Dimulai dari 0 (step pertama)

4. **`nextStep()` Function**
   - Validasi boundary (jangan melebihi total steps)
   - Increment `currentStep` untuk maju
   - Boundary checking untuk mencegah error

5. **`prevStep()` Function**
   - Validasi boundary (jangan kurang dari 0)
   - Decrement `currentStep` untuk mundur
   - Boundary checking untuk mencegah error

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="progress-container">
    <!-- Progress Steps Visual -->
    <div class="progress-bar">
      <div
        v-for="(step, index) in steps"
        :key="index"
        :class="getStepClasses(index)"
        class="step-item"
        @click="goToStep(index)"
      >
        <div class="step-number">{{ index + 1 }}</div>
        <div class="step-title">{{ step }}</div>
        <div v-if="index !== steps.length - 1" class="step-connector"></div>
      </div>
    </div>

    <!-- Progress Information -->
    <div class="progress-info">
      <div class="progress-stats">
        <span class="current-step">Step {{ currentStep + 1 }} of {{ steps.length }}</span>
        <span class="progress-percentage">{{ progressPercentage }}%</span>
      </div>
      <div class="progress-bar-linear">
        <div
          class="progress-fill"
          :style="{ width: progressPercentage + '%' }"
        ></div>
      </div>
    </div>

    <!-- Navigation Controls -->
    <div class="controls">
      <button
        @click="prevStep"
        :disabled="!canGoPrev"
        class="btn btn-prev"
      >
        ← Previous
      </button>
      <button
        @click="nextStep"
        :disabled="!canGoNext"
        class="btn btn-next"
      >
        Next →
      </button>
    </div>

    <!-- Current Step Content -->
    <div class="step-content">
      <h3>{{ steps[currentStep] }}</h3>
      <div class="content-area">
        <div v-if="currentStep === 0">
          <h4>Welcome to Step 1</h4>
          <p>This is the first step of our progress indicator. Click 'Next' to continue.</p>
          <ul>
            <li>Complete required information</li>
            <li>Review your inputs</li>
            <li>Click next to proceed</li>
          </ul>
        </div>
        <div v-else-if="currentStep === 1">
          <h4>Step 2 - In Progress</h4>
          <p>You're now on the second step. You can go back to step 1 or continue to step 3.</p>
          <ul>
            <li>Verify step 1 completion</li>
            <li>Complete current step requirements</li>
            <li>Proceed to next step</li>
          </ul>
        </div>
        <div v-else>
          <h4>Step 3 - Final Step</h4>
          <p>This is the final step. You can go back to review previous steps or complete the process.</p>
          <ul>
            <li>Review all previous steps</li>
            <li>Confirm all information</li>
            <li>Complete the process</li>
          </ul>
        </div>
      </div>
    </div>

    <!-- Quick Actions -->
    <div class="quick-actions">
      <button @click="resetSteps" class="btn btn-reset">
        🔄 Reset Progress
      </button>
      <button @click="addStep" class="btn btn-add">
        ➕ Add Step
      </button>
      <button @click="removeLastStep" class="btn btn-remove" :disabled="steps.length <= 1">
        ➖ Remove Last Step
      </button>
    </div>
  </div>
</template>
```

#### Penjelasan Template:

1. **Progress Steps Visual**
   - `v-for` untuk rendering setiap step
   - Dynamic class binding untuk active states
   - Click handlers untuk direct navigation

2. **Progress Information**
   - Menampilkan step saat ini dan total
   - Progress percentage calculation
   - Linear progress bar visual

3. **Navigation Controls**
   - Previous/Next buttons dengan disabled states
   - Event handlers untuk navigasi
   - Visual feedback untuk disabled buttons

4. **Step Content**
   - Dynamic content berdasarkan step aktif
   - `v-if` untuk conditional rendering
   - Informasi yang relevan untuk setiap step

### Langkah 6: Tambah Helper Functions
**Edit `src/components/ProgressSteps.vue` - Tambah functions:**
```vue
<script setup>
import { ref, computed } from "vue";

// ... (kode sebelumnya)

// Helper function untuk mendapatkan class setiap step
const getStepClasses = (index) => {
  return {
    'step-active': index === currentStep.value,
    'step-completed': index < currentStep.value,
    'step-upcoming': index > currentStep.value
  };
};

// Function untuk langsung menuju ke step tertentu
const goToStep = (index) => {
  currentStep.value = index;
};

// Function untuk reset progress ke awal
const resetSteps = () => {
  currentStep.value = 0;
};

// Function untuk menambah step baru
const addStep = () => {
  const newStepNumber = steps.value.length + 1;
  steps.value.push(`Step ${newStepNumber}`);
};

// Function untuk menghapus step terakhir
const removeLastStep = () => {
  if (steps.value.length > 1) {
    steps.value.pop();
    // Jika current step melebihi total step baru, reset ke step terakhir
    if (currentStep.value >= steps.value.length) {
      currentStep.value = steps.value.length - 1;
    }
  }
};

// Computed property untuk status keseluruhan
const isCompleted = computed(() => {
  return currentStep.value === steps.value.length - 1;
});

// Computed property untuk step status text
const stepStatusText = computed(() => {
  if (isCompleted.value) {
    return '✅ Process Completed!';
  }
  return `📝 Step ${currentStep.value + 1} of ${steps.value.length}`;
});
</script>
```

#### Penjelasan Functions:

1. **`getStepClasses(index)`**
   - Mengembalikan object class untuk styling dinamis
   - Active, completed, atau upcoming states
   - Untuk visual feedback yang jelas

2. **`goToStep(index)`**
   - Langsung navigasi ke step tertentu
   - Useful untuk quick navigation
   - Memperbaiki user experience

3. **`resetSteps()`**
   - Reset progress ke step pertama
   - Berguna untuk restart proses
   - Memulai ulang state aplikasi

4. **`addStep()` & `removeLastStep()`**
   - Dynamic step management
   - Demonstrasi array manipulation
   - Boundary checking untuk validasi

### Langkah 7: Styling dengan CSS

```vue
<style scoped>
/* Main Container */
.progress-container {
  max-width: 800px;
  margin: 50px auto;
  padding: 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 20px;
  box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  color: white;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* Progress Steps Visual */
.progress-bar {
  display: flex;
  position: relative;
  margin-bottom: 30px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 20px;
  backdrop-filter: blur(10px);
}

.step-item {
  flex: 1;
  text-align: center;
  padding: 15px 10px;
  position: relative;
  cursor: pointer;
  transition: all 0.3s ease;
  border-radius: 10px;
  margin: 0 5px;
}

.step-item:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: translateY(-2px);
}

.step-number {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 10px;
  font-weight: bold;
  font-size: 1.1em;
  transition: all 0.3s ease;
}

.step-title {
  font-weight: 600;
  font-size: 0.9em;
  margin-bottom: 5px;
}

.step-connector {
  position: absolute;
  top: 50%;
  right: -50%;
  width: 100%;
  height: 2px;
  background: rgba(255, 255, 255, 0.3);
  transform: translateY(-50%);
  z-index: 1;
}

/* Step States */
.step-active {
  background: rgba(255, 255, 255, 0.3);
  transform: scale(1.05);
}

.step-active .step-number {
  background: #4CAF50;
  color: white;
}

.step-completed {
  opacity: 0.7;
}

.step-completed .step-number {
  background: #2196F3;
  color: white;
}

.step-upcoming {
  opacity: 0.5;
}

.step-upcoming .step-number {
  background: rgba(255, 255, 255, 0.2);
  color: rgba(255, 255, 255, 0.7);
}

/* Progress Information */
.progress-info {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 25px;
  border-radius: 15px;
  margin-bottom: 30px;
}

.progress-stats {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.current-step {
  font-size: 1.2em;
  font-weight: 600;
}

.progress-percentage {
  font-size: 1.2em;
  font-weight: 600;
  color: #4CAF50;
}

.progress-bar-linear {
  width: 100%;
  height: 8px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 4px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #4CAF50, #8BC34A);
  transition: width 0.5s ease;
  border-radius: 4px;
}

/* Navigation Controls */
.controls {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 30px;
}

.btn {
  padding: 15px 30px;
  font-size: 1.1em;
  font-weight: 600;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
}

.btn:active:not(:disabled) {
  transform: translateY(0);
}

.btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.btn-prev {
  background: #ff6b6b;
  color: white;
}

.btn-prev:hover:not(:disabled) {
  background: #ff5252;
}

.btn-next {
  background: #4CAF50;
  color: white;
}

.btn-next:hover:not(:disabled) {
  background: #45a049;
}

/* Step Content */
.step-content {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  margin-bottom: 30px;
}

.step-content h3 {
  margin-bottom: 20px;
  font-size: 1.8em;
  text-align: center;
}

.content-area {
  background: rgba(255, 255, 255, 0.05);
  padding: 20px;
  border-radius: 10px;
}

.content-area h4 {
  margin-bottom: 15px;
  color: #4CAF50;
}

.content-area p {
  margin-bottom: 15px;
  line-height: 1.6;
}

.content-area ul {
  list-style: none;
  padding: 0;
}

.content-area li {
  margin-bottom: 10px;
  padding-left: 20px;
  position: relative;
}

.content-area li:before {
  content: "→";
  position: absolute;
  left: 0;
  color: #4CAF50;
}

/* Quick Actions */
.quick-actions {
  display: flex;
  justify-content: center;
  gap: 15px;
  flex-wrap: wrap;
}

.btn-reset {
  background: #ff9800;
  color: white;
}

.btn-reset:hover {
  background: #f57c00;
}

.btn-add {
  background: #2196F3;
  color: white;
}

.btn-add:hover {
  background: #1976D2;
}

.btn-remove {
  background: #f44336;
  color: white;
}

.btn-remove:hover:not(:disabled) {
  background: #da190b;
}

.btn-remove:disabled {
  background: #ccc;
  color: #666;
}

/* Responsive Design */
@media (max-width: 768px) {
  .progress-container {
    margin: 20px;
    padding: 20px;
  }

  .progress-bar {
    padding: 10px 5px;
    flex-direction: column;
  }

  .step-item {
    margin: 5px 0;
  }

  .step-connector {
    display: none;
  }

  .controls {
    flex-direction: column;
  }

  .quick-actions {
    flex-direction: column;
  }

  .btn {
    width: 100%;
  }

  .progress-stats {
    flex-direction: column;
    gap: 10px;
    text-align: center;
  }
}
</style>
```

#### Penjelasan CSS:

1. **Modern Glassmorphism Design**
   - `backdrop-filter: blur()` untuk efek kaca
   - Gradient backgrounds untuk visual appeal
   - Shadow effects untuk depth perception

2. **Interactive Elements**
   - Hover states dengan transform dan shadow
   - Smooth transitions untuk semua interactions
   - Active states dengan visual indicators

3. **Step Visual States**
   - Active (sedang dibuka)
   - Completed (sudah dilalui)
   - Upcoming (masih akan datang)
   - Clear visual hierarchy

4. **Responsive Design**
   - Mobile-friendly layout
   - Flexbox untuk flexible arrangements
   - Optimized untuk touch devices

### Langkah 8: Membuat App.vue

**File: `App.vue`**
```vue
<script setup>
import ProgressSteps from './components/ProgressSteps.vue'
</script>

<template>
  <div id="app">
    <header>
      <h1>📊 Vue.js Progress Steps</h1>
      <p>Interactive Progress Indicator Component</p>
    </header>

    <main>
      <ProgressSteps />

      <div class="features-section">
        <h2>✨ Features Demonstrated:</h2>
        <ul>
          <li>🔄 Reactive state management</li>
          <li>📈 Progress tracking</li>
          <li>🎯 Step navigation</li>
          <li>🎨 Dynamic styling</li>
          <li>📱 Responsive design</li>
          <li>⚡ Interactive controls</li>
        </ul>
      </div>

      <div class="learn-more">
        <h2>📚 What You'll Learn:</h2>
        <div class="learning-grid">
          <div class="learning-item">
            <h3>🔧 Vue.js Concepts</h3>
            <p>Reactive data, computed properties, event handling</p>
          </div>
          <div class="learning-item">
            <h3>📊 Progress Patterns</h3>
            <p>Step tracking, progress indicators, user flow</p>
          </div>
          <div class="learning-item">
            <h3>🎨 UI/UX Design</h3>
            <p>Visual feedback, state indicators, animations</p>
          </div>
          <div class="learning-item">
            <h3>⚡ Advanced Features</h3>
            <p>Dynamic step management, validation, integration</p>
          </div>
        </div>
      </div>
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js 3</p>
      <p>Master progress tracking patterns for multi-step processes</p>
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
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
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
  flex-direction: column;
  gap: 40px;
}

.features-section, .learn-more {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(10px);
  padding: 30px;
  border-radius: 15px;
  border: 1px solid rgba(255,255,255,0.2);
}

.features-section h2, .learn-more h2 {
  text-align: center;
  margin-bottom: 20px;
}

.features-section ul {
  list-style: none;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.features-section li {
  background: rgba(255,255,255,0.1);
  padding: 15px;
  border-radius: 8px;
  border: 1px solid rgba(255,255,255,0.1);
  color: white;
}

.learning-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}

.learning-item {
  background: rgba(255,255,255,0.1);
  padding: 20px;
  border-radius: 10px;
  border: 1px solid rgba(255,255,255,0.1);
  color: white;
}

.learning-item h3 {
  margin-bottom: 10px;
  color: #4CAF50;
}

footer {
  text-align: center;
  margin-top: 40px;
  padding-top: 20px;
  border-top: 1px solid rgba(255,255,255,0.2);
  opacity: 0.8;
}

footer p {
  margin-bottom: 5px;
}

@media (max-width: 768px) {
  header h1 {
    font-size: 2em;
  }

  .features-section ul,
  .learning-grid {
    grid-template-columns: 1fr;
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
   - Buka alamat yang ditampilkan di terminal
   - Aplikasi Progress Steps siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API
- Menggunakan `<script setup>` untuk syntax yang lebih ringkas
- Organisasi logika yang lebih modular

### 2. Reactive References (`ref`)
- Membuat data reactive yang otomatis update UI
- State management untuk current step

### 3. Computed Properties
- Menghitung progress percentage secara otomatis
- Validasi status navigasi

### 4. Event Handling
- Click events untuk navigasi
- Direct step selection

### 5. Conditional Rendering
- Menampilkan konten berdasarkan step aktif
- Dynamic class binding untuk styling

### 6. Array Manipulation
- Menambah dan menghapus steps
- Boundary checking untuk validasi

### 7. Component Architecture
- Single-file components structure
- Props dan events untuk communication

## 🚀 Enhancement Ideas

Setelah menyelesaikan basic progress steps, Anda bisa menambahkan fitur:

### 1. Step Validation
```vue
<script setup>
const stepValidation = ref({
  0: false, // Step 1 tidak valid
  1: false, // Step 2 tidak valid
  2: false  // Step 3 tidak valid
});

const isStepValid = (stepIndex) => {
  return stepValidation.value[stepIndex];
};

const nextStep = () => {
  if (isStepValid(currentStep.value)) {
    if (currentStep.value < steps.value.length - 1) {
      currentStep.value++;
    }
  } else {
    alert('Please complete current step before proceeding');
  }
};
</script>
```

### 2. Step Content Management
```vue
<script setup>
const stepContent = ref({
  0: { title: "Personal Information", content: "Basic info" },
  1: { title: "Account Setup", content: "Account details" },
  2: { title: "Confirmation", content: "Review and submit" }
});

const updateStepContent = (stepIndex, content) => {
  stepContent.value[stepIndex] = content;
};
</script>
```

### 3. Progress Persistence
```vue
<script setup>
import { watch } from 'vue';

// Save progress ke localStorage
watch(currentStep, (newStep) => {
  localStorage.setItem('currentStep', newStep.toString());
});

// Load progress dari localStorage
const loadSavedProgress = () => {
  const saved = localStorage.getItem('currentStep');
  if (saved) {
    currentStep.value = parseInt(saved);
  }
};

onMounted(loadSavedProgress);
</script>
```

### 4. Animation dan Transitions
```vue
<script setup>
import { ref, watch, nextTick } from 'vue';

const isTransitioning = ref(false);

const animateStepChange = async (fromStep, toStep) => {
  isTransitioning.value = true;

  await nextTick();

  // Animation logic here

  setTimeout(() => {
    isTransitioning.value = false;
  }, 300);
};

watch(currentStep, (newStep, oldStep) => {
  if (oldStep !== undefined) {
    animateStepChange(oldStep, newStep);
  }
});
</script>
```

### 5. Multiple Progress Tracks
```vue
<script setup>
const progressTracks = ref({
  main: { steps: ['Setup', 'Config', 'Deploy'], current: 0 },
  secondary: { steps: ['Test', 'Verify', 'Launch'], current: 0 }
});

const switchTrack = (trackName) => {
  activeTrack.value = trackName;
};

const activeTrack = ref('main');

const currentTrack = computed(() => progressTracks.value[activeTrack.value]);
</script>
```

### 6. Progress Callbacks
```vue
<script setup>
const stepCallbacks = ref({
  0: () => console.log('Step 1 started'),
  1: () => console.log('Step 2 started'),
  2: () => console.log('Step 3 started')
});

const executeStepCallback = (stepIndex) => {
  if (stepCallbacks.value[stepIndex]) {
    stepCallbacks.value[stepIndex]();
  }
};

watch(currentStep, (newStep) => {
  executeStepCallback(newStep);
});
</script>
```

## 🔧 Common Issues & Troubleshooting

### Issue 1: Step navigation tidak berfungsi
**Solution**:
- Pastikan `ref()` didefinisikan dengan benar
- Cek boundary checking di fungsi navigation
- Verify event handler terhubung dengan benar

### Issue 2: Styling tidak berfungsi
**Solution**:
- Pastikan `scoped` CSS didefinisikan
- Cek class names di template dan CSS
- Verify CSS specificity

### Issue 3: Progress percentage tidak update
**Solution**:
- Pastikan computed property didefinisikan dengan benar
- Cek dependency tracking
- Verify reactive data update

### Issue 4: Step states tidak muncul
**Solution**:
- Pastikan conditional rendering logic benar
- Cek class binding syntax
- Verify computed properties

### Issue 5: Dynamic steps tidak berfungsi
**Solution**:
- Pastikan array manipulation methods benar
- Cek boundary validation
- Verify reactivity update

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Vue.js Components Guide](https://vuejs.org/guide/essentials/component-basics.html)
- [CSS Progress Bars](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/progress)
- [Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [UX Design for Progress Indicators](https://www.nngroup.com/articles/progress-indicators/)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Implementasi komponen ProgressSteps
- [ ] Tambahkan reactive data untuk steps
- [ ] Implementasi navigasi fungsi
- [ ] Tambahkan computed properties
- [ ] Implementasi helper functions
- [ ] Tambahkan visual styling
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Progress Steps yang fully functional dengan Vue.js! Project ini memberikan pemahaman tentang:
- State management untuk multi-step processes
- User flow navigation patterns
- Visual feedback systems
- Dynamic content rendering
- Component-based architecture
- Interactive UI patterns

---

**Happy Coding! 🚀**