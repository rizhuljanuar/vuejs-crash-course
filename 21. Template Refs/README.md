# 📌 Template Refs - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja template refs untuk mengakses DOM elements dan component instances secara langsung dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Template Refs?](#-apa-itu-template-refs)
- [🎮 Cara Menggunakan Template Refs](#-cara-menggunakan-template-refs)
- [⚡ Basic Template Ref](#-basic-template-ref)
- [🔄 Ref dengan Function](#-ref-dengan-function)
- [🎯 Component Refs](#-component-refs)
- [🔧 Ref dengan Watchers](#-ref-dengan-watchers)
- [🚀 Advanced Ref Patterns](#-advanced-ref-patterns)
- [💡 Best Practices](#-best-practices)
- [🛡️ Safety Considerations](#-safety-considerations)

## 🔍 Apa itu Template Refs?

**Template Refs** adalah fitur dalam Vue.js yang memungkinkan kita untuk mendapatkan referensi langsung ke DOM elements atau component instances. Dengan refs, kita bisa mengakses dan memanipulasi elements secara programmatic.

### Konsep Dasar Template Refs

```vue
<template>
  <!-- Template ref dengan ref attribute -->
  <input ref="myInput" />
  <div ref="myDiv">Content</div>
  <MyComponent ref="myComponent" />
</template>

<script setup>
// JavaScript ref untuk menyimpan referensi
const myInput = ref(null)
const myDiv = ref(null)
const myComponent = ref(null)
</script>
```

### Mengapa Template Refs Penting?

1. **🎯 Direct DOM Access:** Mengakses DOM elements langsung
2. **⚡ Focus Management:** Mengatur focus pada input elements
3. **🎨 Animations:** Membuat custom animations
4. **📏 Measurements:** Mendapatkan ukuran dan posisi elements
5. **🔧 Component Control:** Mengakses methods dan data child components
6. **📱 Integration:** Integrasi dengan library pihak ketiga

## ❓ Pertanyaan: Apa Fungsi Template Refs?

Template Refs memiliki 5 fungsi utama yang sangat penting dalam pengembangan aplikasi Vue.js:

### Fungsi 1: Mengakses dan Memanipulasi DOM Elements Langsung

Template Refs memungkinkan kita untuk mengakses elemen DOM secara langsung dan melakukan manipulasi yang tidak mungkin dilakukan melalui template Vue biasa.

```vue
<script setup>
import { ref, onMounted } from 'vue'

const inputElement = ref(null)
const buttonElement = ref(null)
const messageElement = ref(null)

const focusInput = () => {
  // Mengakses input element langsung
  inputElement.value?.focus()
  inputElement.value?.select()
}

const changeButtonColor = () => {
  // Mengubah style button secara langsung
  buttonElement.value.style.backgroundColor = '#3b82f6'
  buttonElement.value.style.color = 'white'
}

const updateMessage = () => {
  // Mengubah konten elemen langsung
  messageElement.value.textContent = 'DOM element berhasil dimanipulasi!'
  messageElement.value.style.color = '#16a34a'
}

const getElementInfo = () => {
  if (inputElement.value) {
    console.log('Input type:', inputElement.value.type)
    console.log('Input value:', inputElement.value.value)
    console.log('Input placeholder:', inputElement.value.placeholder)
  }
}

onMounted(() => {
  console.log('DOM elements siap dimanipulasi')
  getElementInfo()
})
</script>

<template>
  <div class="dom-manipulation-demo">
    <h2>🎯 Direct DOM Access & Manipulation</h2>

    <div class="form-section">
      <input
        ref="inputElement"
        type="text"
        placeholder="Ketik sesuatu..."
        class="styled-input"
      />

      <button
        ref="buttonElement"
        @click="focusInput"
        class="action-button"
      >
        Focus Input
      </button>

      <div ref="messageElement" class="message-display">
        Pesan akan muncul di sini
      </div>
    </div>

    <div class="controls">
      <button @click="changeButtonColor" class="control-btn">
        Ubah Warna Button
      </button>
      <button @click="updateMessage" class="control-btn">
        Update Pesan
      </button>
      <button @click="getElementInfo" class="control-btn">
        Log Element Info
      </button>
    </div>

    <div class="info-box">
      <h4>💡 Yang Bisa Dilakukan dengan Template Refs:</h4>
      <ul>
        <li><strong>Focus management:</strong> Mengatur focus otomatis pada input</li>
        <li><strong>Style manipulation:</strong> Mengubah CSS properties langsung</li>
        <li><strong>Content modification:</strong> Mengubah teks atau HTML content</li>
        <li><strong>Property access:</strong> Membaca properties seperti .value, .type, .id</li>
        <li><strong>Method calls:</strong> Memanggil DOM methods seperti .focus(), .click(), .select()</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.dom-manipulation-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.form-section {
  display: flex;
  flex-direction: column;
  gap: 15px;
  margin-bottom: 20px;
}

.styled-input {
  padding: 12px;
  border: 2px solid #d1d5db;
  border-radius: 6px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.styled-input:focus {
  outline: none;
  border-color: #3b82f6;
}

.action-button {
  padding: 10px 20px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
}

.action-button:hover {
  background: #2563eb;
}

.message-display {
  padding: 15px;
  background: #f8fafc;
  border: 2px solid #e2e8f0;
  border-radius: 6px;
  text-align: center;
  font-weight: 500;
  color: #475569;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.control-btn {
  padding: 8px 16px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.control-btn:hover {
  background: #059669;
}

.info-box {
  background: #dbeafe;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.info-box h4 {
  margin-top: 0;
  color: #1e40af;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #1e40af;
}
</style>
```

### Fungsi 2: Mengelola Focus dan User Interaction

Template Refs sangat berguna untuk mengelola focus pada form elements dan meningkatkan user experience melalui navigasi yang intuitif.

```vue
<script setup>
import { ref, onMounted, nextTick } from 'vue'

const firstNameInput = ref(null)
const lastNameInput = ref(null)
const emailInput = ref(null)
const passwordInput = ref(null)
const submitButton = ref(null)

const formData = ref({
  firstName: '',
  lastName: '',
  email: '',
  password: ''
})

const focusField = (fieldRef) => {
  // Focus pada field tertentu
  fieldRef.value?.focus()
  fieldRef.value?.select()
}

const moveToNextField = (currentField, nextField) => {
  if (currentField.value?.value.length > 2) {
    nextField.value?.focus()
  }
}

const validateAndFocus = (fieldRef, fieldName) => {
  const field = fieldRef.value
  if (!field?.value.trim()) {
    field?.focus()
    field?.classList.add('error')
    return false
  }
  field?.classList.remove('error')
  return true
}

const handleSubmit = async () => {
  // Validasi setiap field dan focus pada yang error
  let isValid = true

  if (!validateAndFocus(firstNameInput, 'firstName')) isValid = false
  else if (!validateAndFocus(lastNameInput, 'lastName')) isValid = false
  else if (!validateAndFocus(emailInput, 'email')) isValid = false
  else if (!validateAndFocus(passwordInput, 'password')) isValid = false

  if (isValid) {
    submitButton.value?.focus()
    alert('Form berhasil disubmit!')
  }
}

const setupKeyboardNavigation = () => {
  // Enter key untuk pindah ke field berikutnya
  firstNameInput.value?.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {
      e.preventDefault()
      moveToNextField(firstNameInput, lastNameInput)
    }
  })

  lastNameInput.value?.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {
      e.preventDefault()
      moveToNextField(lastNameInput, emailInput)
    }
  })

  emailInput.value?.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {
      e.preventDefault()
      moveToNextField(emailInput, passwordInput)
    }
  })

  passwordInput.value?.addEventListener('keydown', (e) => {
    if (e.key === 'Enter') {
      e.preventDefault()
      handleSubmit()
    }
  })
}

// Auto focus pada field pertama saat component dimuat
onMounted(() => {
  nextTick(() => {
    firstNameInput.value?.focus()
    setupKeyboardNavigation()
  })
})
</script>

<template>
  <div class="focus-management-demo">
    <h2>⚡ Focus Management & User Interaction</h2>

    <form @submit.prevent="handleSubmit" class="registration-form">
      <div class="form-group">
        <label for="firstName">First Name:</label>
        <input
          ref="firstNameInput"
          id="firstName"
          v-model="formData.firstName"
          type="text"
          placeholder="Masukkan nama depan"
          class="form-input"
        />
      </div>

      <div class="form-group">
        <label for="lastName">Last Name:</label>
        <input
          ref="lastNameInput"
          id="lastName"
          v-model="formData.lastName"
          type="text"
          placeholder="Masukkan nama belakang"
          class="form-input"
        />
      </div>

      <div class="form-group">
        <label for="email">Email:</label>
        <input
          ref="emailInput"
          id="email"
          v-model="formData.email"
          type="email"
          placeholder="email@example.com"
          class="form-input"
        />
      </div>

      <div class="form-group">
        <label for="password">Password:</label>
        <input
          ref="passwordInput"
          id="password"
          v-model="formData.password"
          type="password"
          placeholder="Minimal 8 karakter"
          class="form-input"
        />
      </div>

      <button
        ref="submitButton"
        type="submit"
        class="submit-button"
      >
        Submit Form
      </button>
    </form>

    <div class="focus-controls">
      <h4>🎯 Quick Focus Controls:</h4>
      <button @click="focusField(firstNameInput)" class="focus-btn">
        Focus First Name
      </button>
      <button @click="focusField(lastNameInput)" class="focus-btn">
        Focus Last Name
      </button>
      <button @click="focusField(emailInput)" class="focus-btn">
        Focus Email
      </button>
      <button @click="focusField(passwordInput)" class="focus-btn">
        Focus Password
      </button>
    </div>

    <div class="instructions">
      <h4>⌨️ Keyboard Navigation:</h4>
      <ul>
        <li><strong>Enter:</strong> Pindah ke field berikutnya</li>
        <li><strong>Tab:</strong> Navigasi normal</li>
        <li><strong>Shift+Tab:</strong> Navigasi mundur</li>
        <li><strong>Auto-focus:</strong> Field pertama otomatis fokus saat load</li>
        <li><strong>Auto-validation:</strong> Focus otomatis pada field yang kosong saat submit</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.focus-management-demo {
  padding: 20px;
  max-width: 500px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.registration-form {
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
  color: #374151;
}

.form-input {
  width: 100%;
  padding: 10px;
  border: 2px solid #d1d5db;
  border-radius: 6px;
  font-size: 14px;
  transition: border-color 0.3s;
}

.form-input:focus {
  outline: none;
  border-color: #10b981;
  box-shadow: 0 0 0 3px rgba(16, 185, 129, 0.1);
}

.form-input.error {
  border-color: #ef4444;
  box-shadow: 0 0 0 3px rgba(239, 68, 68, 0.1);
}

.submit-button {
  width: 100%;
  padding: 12px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s;
}

.submit-button:hover {
  background: #059669;
}

.focus-controls {
  margin-bottom: 20px;
  text-align: center;
}

.focus-controls h4 {
  margin-bottom: 10px;
  color: #374151;
}

.focus-btn {
  margin: 5px;
  padding: 6px 12px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.focus-btn:hover {
  background: #2563eb;
}

.instructions {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.instructions h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #065f46;
}

.instructions ul {
  margin: 0;
  padding-left: 20px;
}

.instructions li {
  margin: 5px 0;
  color: #065f46;
  font-size: 14px;
}
</style>
```

### Fungsi 3: Integrasi dengan Library Pihak Ketiga

Template Refs sangat penting untuk mengintegrasikan Vue.js dengan library JavaScript eksternal seperti charts, maps, sliders, dan lainnya.

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

// Refs untuk integrasi library eksternal
const chartCanvas = ref(null)
const sliderElement = ref(null)
const mapContainer = ref(null)
const datePickerElement = ref(null)
const richTextEditor = ref(null)

// State untuk menyimpan instances
let chartInstance = null
let sliderInstance = null
let mapInstance = null
let pickerInstance = null
let editorInstance = null

const chartData = ref({
  labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May'],
  datasets: [{
    label: 'Sales Data',
    data: [12, 19, 3, 5, 2],
    backgroundColor: ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6']
  }]
})

// Simulasi Chart.js integration
const initializeChart = () => {
  if (!chartCanvas.value) return

  // Simulasi inisialisasi chart library
  const ctx = chartCanvas.value.getContext('2d')

  // Mock chart instance (dalam real implementation, ini akan menjadi Chart.js instance)
  chartInstance = {
    update: () => {
      console.log('Chart updated with new data:', chartData.value)
      // Dalam implementasi nyata: chartInstance.data = chartData.value
    },
    destroy: () => {
      console.log('Chart destroyed')
    }
  }

  console.log('Chart initialized on canvas element')
}

// Simulasi Range Slider integration
const initializeSlider = () => {
  if (!sliderElement.value) return

  // Mock slider setup
  sliderInstance = {
    getValue: () => sliderElement.value?.value,
    setValue: (value) => {
      if (sliderElement.value) {
        sliderElement.value = value
        dispatchSliderEvent(value)
      }
    },
    destroy: () => {
      sliderElement.value?.removeEventListener('input', handleSliderChange)
    }
  }

  sliderElement.value?.addEventListener('input', handleSliderChange)
  console.log('Slider initialized')
}

// Simulasi Leaflet/Mapbox integration
const initializeMap = () => {
  if (!mapContainer.value) return

  // Mock map instance
  mapInstance = {
    setCenter: (lat, lng) => {
      console.log(`Map center set to: ${lat}, ${lng}`)
      mapContainer.value.innerHTML = `
        <div style="padding: 20px; text-align: center; background: #e0f2fe;">
          📍 Map Center: ${lat}, ${lng}
        </div>
      `
    },
    addMarker: (lat, lng, title) => {
      console.log(`Marker added: ${title} at ${lat}, ${lng}`)
    },
    destroy: () => {
      console.log('Map destroyed')
    }
  }

  // Set initial map view
  mapInstance.setCenter(-6.2088, 106.8456) // Jakarta
  console.log('Map initialized')
}

// Simulasi Date Picker integration
const initializeDatePicker = () => {
  if (!datePickerElement.value) return

  // Mock date picker setup
  pickerInstance = {
    getDate: () => datePickerElement.value?.value,
    setDate: (date) => {
      if (datePickerElement.value) {
        datePickerElement.value.value = date
      }
    },
    destroy: () => {
      console.log('Date picker destroyed')
    }
  }

  console.log('Date picker initialized')
}

// Simulasi Rich Text Editor integration
const initializeTextEditor = () => {
  if (!richTextEditor.value) return

  // Mock rich text editor
  editorInstance = {
    getContent: () => richTextEditor.value?.innerHTML,
    setContent: (content) => {
      if (richTextEditor.value) {
        richTextEditor.value.innerHTML = content
      }
    },
    insertText: (text) => {
      if (richTextEditor.value) {
        richTextEditor.value.innerHTML += text
      }
    },
    destroy: () => {
      console.log('Text editor destroyed')
    }
  }

  // Set initial content
  editorInstance.setContent('<p><strong>Welcome!</strong> Start typing here...</p>')
  console.log('Rich text editor initialized')
}

// Event handlers
const handleSliderChange = (event) => {
  console.log('Slider value changed:', event.target.value)
}

const updateChartData = () => {
  // Update dengan data random
  const newData = chartData.value.datasets[0].data.map(() =>
    Math.floor(Math.random() * 20) + 1
  )
  chartData.value.datasets[0].data = newData
  chartInstance?.update()
}

const changeMapLocation = (city, lat, lng) => {
  mapInstance?.setCenter(lat, lng)
  mapInstance?.addMarker(lat, lng, city)
}

const setTodayDate = () => {
  const today = new Date().toISOString().split('T')[0]
  pickerInstance?.setDate(today)
}

const insertEditorText = () => {
  editorInstance?.insertText(' <em>inserted text</em> ')
}

// Cleanup all instances
const cleanupInstances = () => {
  chartInstance?.destroy()
  sliderInstance?.destroy()
  mapInstance?.destroy()
  pickerInstance?.destroy()
  editorInstance?.destroy()
}

onMounted(() => {
  // Initialize all integrations
  initializeChart()
  initializeSlider()
  initializeMap()
  initializeDatePicker()
  initializeTextEditor()
})

onUnmounted(() => {
  cleanupInstances()
})
</script>

<template>
  <div class="library-integration-demo">
    <h2>📚 Third-Party Library Integration</h2>

    <div class="integration-grid">
      <!-- Chart Integration -->
      <div class="integration-item">
        <h4>📊 Chart Integration</h4>
        <canvas ref="chartCanvas" width="200" height="150" class="chart-canvas"></canvas>
        <button @click="updateChartData" class="integration-btn">
          Update Chart Data
        </button>
      </div>

      <!-- Slider Integration -->
      <div class="integration-item">
        <h4>🎚️ Range Slider</h4>
        <input
          ref="sliderElement"
          type="range"
          min="0"
          max="100"
          value="50"
          class="range-slider"
        />
        <div class="slider-value">Value: {{ sliderElement?.value || 50 }}</div>
      </div>

      <!-- Map Integration -->
      <div class="integration-item">
        <h4>🗺️ Map Integration</h4>
        <div ref="mapContainer" class="map-container"></div>
        <div class="map-controls">
          <button @click="changeMapLocation('Jakarta', -6.2088, 106.8456)" class="map-btn">
            Jakarta
          </button>
          <button @click="changeMapLocation('Surabaya', -7.2575, 112.7521)" class="map-btn">
            Surabaya
          </button>
        </div>
      </div>

      <!-- Date Picker Integration -->
      <div class="integration-item">
        <h4>📅 Date Picker</h4>
        <input
          ref="datePickerElement"
          type="date"
          class="date-picker"
        />
        <button @click="setTodayDate" class="integration-btn">
          Set Today
        </button>
      </div>

      <!-- Rich Text Editor Integration -->
      <div class="integration-item">
        <h4>✏️ Rich Text Editor</h4>
        <div
          ref="richTextEditor"
          contenteditable="true"
          class="rich-text-editor"
        ></div>
        <button @click="insertEditorText" class="integration-btn">
          Insert Text
        </button>
      </div>
    </div>

    <div class="integration-info">
      <h4>🔧 Integration Benefits:</h4>
      <ul>
        <li><strong>Direct Access:</strong> Mengakses DOM elements untuk library initialization</li>
        <li><strong>Instance Management:</strong> Menyimpan dan mengelola library instances</li>
        <li><strong>Cleanup:</strong> Proper cleanup saat component unmount</li>
        <li><strong>Event Handling:</strong> Menghubungkan library events dengan Vue reactivity</li>
        <li><strong>Performance:</strong> Optimized integration dengan lifecycle hooks</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.library-integration-demo {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.integration-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.integration-item {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.integration-item h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #475569;
  text-align: center;
}

.chart-canvas {
  width: 100%;
  height: 150px;
  background: #e0f2fe;
  border: 1px solid #0ea5e9;
  border-radius: 4px;
  margin-bottom: 10px;
}

.range-slider {
  width: 100%;
  margin-bottom: 10px;
}

.slider-value {
  text-align: center;
  color: #475569;
  font-size: 14px;
}

.map-container {
  width: 100%;
  height: 150px;
  background: #e0f2fe;
  border: 1px solid #0ea5e9;
  border-radius: 4px;
  margin-bottom: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #0c4a6e;
  font-weight: 500;
}

.map-controls {
  display: flex;
  gap: 10px;
}

.map-btn {
  flex: 1;
  padding: 6px;
  background: #0ea5e9;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}

.date-picker {
  width: 100%;
  padding: 8px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  margin-bottom: 10px;
}

.rich-text-editor {
  width: 100%;
  min-height: 100px;
  padding: 10px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  background: white;
  margin-bottom: 10px;
}

.rich-text-editor:focus {
  outline: none;
  border-color: #8b5cf6;
}

.integration-btn {
  width: 100%;
  padding: 8px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.integration-btn:hover {
  background: #7c3aed;
}

.integration-info {
  background: #faf5ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.integration-info h4 {
  margin-top: 0;
  color: #6b21a8;
}

.integration-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.integration-info li {
  margin: 5px 0;
  color: #6b21a8;
  font-size: 14px;
}
</style>
```

### Fungsi 4: Mengakses Component Methods dan Data

Template Refs memungkinkan parent component untuk mengakses methods, data, dan computed properties dari child components secara langsung.

```vue
<script setup>
import { ref } from 'vue'

// Refs untuk child components
const childCounter = ref(null)
const childForm = ref(null)
const childModal = ref(null)
const childTimer = ref(null)

// Data untuk demonstrasi
const message = ref('Parent component message')

// Methods untuk mengakses child components
const incrementChildCounter = () => {
  childCounter.value?.increment()
}

const decrementChildCounter = () => {
  childCounter.value?.decrement()
}

const resetChildCounter = () => {
  childCounter.value?.reset()
}

const getChildCounterData = () => {
  if (childCounter.value) {
    const data = childCounter.value.getCounterData()
    console.log('Child counter data:', data)
    alert(`Current count: ${data.count}, Clicks: ${data.totalClicks}`)
  }
}

const submitChildForm = () => {
  childForm.value?.submit()
}

const clearChildForm = () => {
  childForm.value?.clear()
}

const getChildFormData = () => {
  if (childForm.value) {
    const data = childForm.value.getFormData()
    console.log('Child form data:', data)
    alert(`Form data: ${JSON.stringify(data, null, 2)}`)
  }
}

const openChildModal = () => {
  childModal.value?.open()
}

const closeChildModal = () => {
  childModal.value?.close()
}

const getModalState = () => {
  if (childModal.value) {
    const isOpen = childModal.value.getIsOpen()
    console.log('Modal is open:', isOpen)
    alert(`Modal is ${isOpen ? 'open' : 'closed'}`)
  }
}

const startChildTimer = () => {
  childTimer.value?.start()
}

const stopChildTimer = () => {
  childTimer.value?.stop()
}

const getChildTimerInfo = () => {
  if (childTimer.value) {
    const info = childTimer.value.getTimerInfo()
    console.log('Child timer info:', info)
    alert(`Timer: ${info.isRunning ? 'Running' : 'Stopped'}, Elapsed: ${info.elapsed}s`)
  }
}
</script>

<template>
  <div class="component-access-demo">
    <h2>🔗 Component Methods & Data Access</h2>

    <div class="demo-grid">
      <!-- Child Counter Component -->
      <div class="demo-section">
        <h3>🔢 Child Counter</h3>
        <ChildCounter ref="childCounter" />

        <div class="controls">
          <button @click="incrementChildCounter" class="control-btn success">
            Increment
          </button>
          <button @click="decrementChildCounter" class="control-btn danger">
            Decrement
          </button>
          <button @click="resetChildCounter" class="control-btn warning">
            Reset
          </button>
          <button @click="getChildCounterData" class="control-btn info">
            Get Data
          </button>
        </div>
      </div>

      <!-- Child Form Component -->
      <div class="demo-section">
        <h3>📝 Child Form</h3>
        <ChildForm ref="childForm" />

        <div class="controls">
          <button @click="submitChildForm" class="control-btn success">
            Submit Form
          </button>
          <button @click="clearChildForm" class="control-btn warning">
            Clear Form
          </button>
          <button @click="getChildFormData" class="control-btn info">
            Get Form Data
          </button>
        </div>
      </div>

      <!-- Child Modal Component -->
      <div class="demo-section">
        <h3>🪟 Child Modal</h3>
        <ChildModal ref="childModal" />

        <div class="controls">
          <button @click="openChildModal" class="control-btn success">
            Open Modal
          </button>
          <button @click="closeChildModal" class="control-btn danger">
            Close Modal
          </button>
          <button @click="getModalState" class="control-btn info">
            Get State
          </button>
        </div>
      </div>

      <!-- Child Timer Component -->
      <div class="demo-section">
        <h3>⏱️ Child Timer</h3>
        <ChildTimer ref="childTimer" />

        <div class="controls">
          <button @click="startChildTimer" class="control-btn success">
            Start Timer
          </button>
          <button @click="stopChildTimer" class="control-btn danger">
            Stop Timer
          </button>
          <button @click="getChildTimerInfo" class="control-btn info">
            Get Info
          </button>
        </div>
      </div>
    </div>

    <div class="explanation">
      <h4>🎯 Component Access Benefits:</h4>
      <ul>
        <li><strong>Method Invocation:</strong> Memanggil methods child component dari parent</li>
        <li><strong>Data Access:</strong> Mengakses data dan computed properties child</li>
        <li><strong>State Control:</strong> Mengontrol state child component secara eksternal</li>
        <li><strong>Validation:</strong> Memvalidasi data child component sebelum submit</li>
        <li><strong>Integration:</strong> Menghubungkan multiple child components</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.component-access-demo {
  padding: 20px;
  max-width: 1000px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.demo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.demo-section {
  background: #fef3c7;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #f59e0b;
}

.demo-section h3 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #92400e;
  text-align: center;
}

.controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(100px, 1fr));
  gap: 10px;
  margin-top: 15px;
}

.control-btn {
  padding: 8px 12px;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
}

.control-btn.success {
  background: #10b981;
  color: white;
}

.control-btn.success:hover {
  background: #059669;
}

.control-btn.danger {
  background: #ef4444;
  color: white;
}

.control-btn.danger:hover {
  background: #dc2626;
}

.control-btn.warning {
  background: #f59e0b;
  color: white;
}

.control-btn.warning:hover {
  background: #d97706;
}

.control-btn.info {
  background: #3b82f6;
  color: white;
}

.control-btn.info:hover {
  background: #2563eb;
}

.explanation {
  background: #fffbeb;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.explanation h4 {
  margin-top: 0;
  color: #92400e;
}

.explanation ul {
  margin: 10px 0;
  padding-left: 20px;
}

.explanation li {
  margin: 5px 0;
  color: #92400e;
  font-size: 14px;
}
</style>

### Fungsi 5: Mengukur Element Dimensions dan Position

Template Refs memungkinkan kita untuk mendapatkan informasi ukuran dan posisi elemen secara akurat, yang sangat berguna untuk responsive design dan layout calculations.

```vue
<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'

// Refs untuk elements yang akan diukur
const boxElement = ref(null)
const containerElement = ref(null)
const viewportElement = ref(null)
const scrollableElement = ref(null)
const imageElement = ref(null)

// State untuk menyimpan measurement data
const measurements = ref({
  box: {
    width: 0,
    height: 0,
    offsetWidth: 0,
    offsetHeight: 0,
    clientWidth: 0,
    clientHeight: 0,
    scrollWidth: 0,
    scrollHeight: 0
  },
  container: {
    offsetTop: 0,
    offsetLeft: 0,
    scrollTop: 0,
    scrollLeft: 0
  },
  viewport: {
    innerWidth: 0,
    innerHeight: 0,
    clientWidth: 0,
    clientHeight: 0
  },
  scrollable: {
    scrollTop: 0,
    scrollHeight: 0,
    clientHeight: 0,
    scrollPercentage: 0
  },
  image: {
    naturalWidth: 0,
    naturalHeight: 0,
    currentWidth: 0,
    currentHeight: 0,
    aspectRatio: 0
  }
})

// ResizeObserver untuk tracking perubahan ukuran
let resizeObserver = null

const measureBoxElement = () => {
  if (!boxElement.value) return

  const box = boxElement.value

  measurements.value.box = {
    width: box.getBoundingClientRect().width,
    height: box.getBoundingClientRect().height,
    offsetWidth: box.offsetWidth,
    offsetHeight: box.offsetHeight,
    clientWidth: box.clientWidth,
    clientHeight: box.clientHeight,
    scrollWidth: box.scrollWidth,
    scrollHeight: box.scrollHeight
  }
}

const measureContainerPosition = () => {
  if (!containerElement.value) return

  const container = containerElement.value

  measurements.value.container = {
    offsetTop: container.offsetTop,
    offsetLeft: container.offsetLeft,
    scrollTop: container.scrollTop,
    scrollLeft: container.scrollLeft
  }
}

const measureViewport = () => {
  measurements.value.viewport = {
    innerWidth: window.innerWidth,
    innerHeight: window.innerHeight,
    clientWidth: document.documentElement.clientWidth,
    clientHeight: document.documentElement.clientHeight
  }
}

const measureScrollableElement = () => {
  if (!scrollableElement.value) return

  const element = scrollableElement.value
  const scrollPercentage = (element.scrollTop / (element.scrollHeight - element.clientHeight)) * 100

  measurements.value.scrollable = {
    scrollTop: element.scrollTop,
    scrollHeight: element.scrollHeight,
    clientHeight: element.clientHeight,
    scrollPercentage: isNaN(scrollPercentage) ? 0 : scrollPercentage
  }
}

const measureImageElement = () => {
  if (!imageElement.value) return

  const img = imageElement.value

  measurements.value.image = {
    naturalWidth: img.naturalWidth,
    naturalHeight: img.naturalHeight,
    currentWidth: img.width,
    currentHeight: img.height,
    aspectRatio: img.naturalWidth / img.naturalHeight
  }
}

const measureAllElements = () => {
  measureBoxElement()
  measureContainerPosition()
  measureViewport()
  measureScrollableElement()
  measureImageElement()
}

const scrollToTop = () => {
  scrollableElement.value?.scrollTo({ top: 0, behavior: 'smooth' })
}

const scrollToBottom = () => {
  scrollableElement.value?.scrollTo({
    top: scrollableElement.value.scrollHeight,
    behavior: 'smooth'
  })
}

const scrollToPercentage = (percentage) => {
  const element = scrollableElement.value
  if (element) {
    const targetScroll = (element.scrollHeight - element.clientHeight) * (percentage / 100)
    element.scrollTo({ top: targetScroll, behavior: 'smooth' })
  }
}

const resizeBox = () => {
  if (boxElement.value) {
    const randomWidth = Math.floor(Math.random() * 200) + 100
    const randomHeight = Math.floor(Math.random() * 150) + 50
    boxElement.value.style.width = randomWidth + 'px'
    boxElement.value.style.height = randomHeight + 'px'
  }
}

const centerElement = () => {
  if (containerElement.value && boxElement.value) {
    const containerRect = containerElement.value.getBoundingClientRect()
    const boxRect = boxElement.value.getBoundingClientRect()

    const centerX = (containerRect.width - boxRect.width) / 2
    const centerY = (containerRect.height - boxRect.height) / 2

    boxElement.value.style.position = 'absolute'
    boxElement.value.style.left = centerX + 'px'
    boxElement.value.style.top = centerY + 'px'
  }
}

const setupResizeObserver = () => {
  if (!resizeObserver) {
    resizeObserver = new ResizeObserver(() => {
      nextTick(() => {
        measureAllElements()
      })
    })
  }

  // Observe multiple elements
  if (boxElement.value) resizeObserver.observe(boxElement.value)
  if (containerElement.value) resizeObserver.observe(containerElement.value)
  if (viewportElement.value) resizeObserver.observe(viewportElement.value)
}

const cleanupResizeObserver = () => {
  if (resizeObserver) {
    resizeObserver.disconnect()
    resizeObserver = null
  }
}

// Event listeners
const handleScroll = () => {
  measureScrollableElement()
}

const handleResize = () => {
  measureAllElements()
}

onMounted(() => {
  nextTick(() => {
    measureAllElements()
    setupResizeObserver()
  })

  // Add event listeners
  window.addEventListener('resize', handleResize)
  if (scrollableElement.value) {
    scrollableElement.value.addEventListener('scroll', handleScroll)
  }
})

onUnmounted(() => {
  cleanupResizeObserver()
  window.removeEventListener('resize', handleResize)
  if (scrollableElement.value) {
    scrollableElement.value.removeEventListener('scroll', handleScroll)
  }
})
</script>

<template>
  <div class="measurements-demo">
    <h2>📏 Element Dimensions & Position Measurements</h2>

    <div class="measurements-grid">
      <!-- Box Element Measurements -->
      <div class="measurement-section">
        <h4>📦 Box Element</h4>
        <div ref="boxElement" class="measurement-box">
          Dynamic Box
        </div>
        <div class="measurement-data">
          <p><strong>getBoundingClientRect:</strong></p>
          <p>Width: {{ measurements.box.width.toFixed(1) }}px</p>
          <p>Height: {{ measurements.box.height.toFixed(1) }}px</p>
          <p><strong>Offset Dimensions:</strong></p>
          <p>Width: {{ measurements.box.offsetWidth }}px</p>
          <p>Height: {{ measurements.box.offsetHeight }}px</p>
        </div>
        <button @click="resizeBox" class="measurement-btn">
          Resize Box
        </button>
      </div>

      <!-- Container Position -->
      <div class="measurement-section">
        <h4>📍 Container Position</h4>
        <div ref="containerElement" class="position-container">
          <div ref="boxElement" class="inner-box">
            Inner Box
          </div>
        </div>
        <div class="measurement-data">
          <p>Offset Top: {{ measurements.container.offsetTop }}px</p>
          <p>Offset Left: {{ measurements.container.offsetLeft }}px</p>
          <p>Scroll Top: {{ measurements.container.scrollTop }}px</p>
        </div>
        <button @click="centerElement" class="measurement-btn">
          Center Element
        </button>
      </div>

      <!-- Viewport Measurements -->
      <div class="measurement-section">
        <h4>🖥️ Viewport</h4>
        <div ref="viewportElement" class="viewport-display">
          <p>Window Size</p>
        </div>
        <div class="measurement-data">
          <p>Inner Width: {{ measurements.viewport.innerWidth }}px</p>
          <p>Inner Height: {{ measurements.viewport.innerHeight }}px</p>
          <p>Client Width: {{ measurements.viewport.clientWidth }}px</p>
          <p>Client Height: {{ measurements.viewport.clientHeight }}px</p>
        </div>
      </div>

      <!-- Scrollable Element -->
      <div class="measurement-section">
        <h4>📜 Scrollable Element</h4>
        <div
          ref="scrollableElement"
          class="scrollable-content"
          @scroll="handleScroll"
        >
          <div class="long-content">
            <p>Scrollable content...</p>
            <div v-for="i in 20" :key="i" class="content-item">
              Item {{ i }}
            </div>
          </div>
        </div>
        <div class="measurement-data">
          <p>Scroll Top: {{ measurements.scrollable.scrollTop }}px</p>
          <p>Scroll Height: {{ measurements.scrollable.scrollHeight }}px</p>
          <p>Client Height: {{ measurements.scrollable.clientHeight }}px</p>
          <p>Progress: {{ measurements.scrollable.scrollPercentage.toFixed(1) }}%</p>
        </div>
        <div class="scroll-controls">
          <button @click="scrollToTop" class="scroll-btn">Top</button>
          <button @click="scrollToPercentage(50)" class="scroll-btn">50%</button>
          <button @click="scrollToBottom" class="scroll-btn">Bottom</button>
        </div>
      </div>

      <!-- Image Measurements -->
      <div class="measurement-section">
        <h4>🖼️ Image Element</h4>
        <div class="image-container">
          <img
            ref="imageElement"
            src="https://via.placeholder.com/300x200/3b82f6/ffffff?text=Sample+Image"
            alt="Sample image"
            class="measurement-image"
          />
        </div>
        <div class="measurement-data">
          <p>Natural Width: {{ measurements.image.naturalWidth }}px</p>
          <p>Natural Height: {{ measurements.image.naturalHeight }}px</p>
          <p>Current Width: {{ measurements.image.currentWidth }}px</p>
          <p>Current Height: {{ measurements.image.currentHeight }}px</p>
          <p>Aspect Ratio: {{ measurements.image.aspectRatio.toFixed(2) }}</p>
        </div>
      </div>
    </div>

    <div class="measurement-info">
      <h4>🔍 Measurement Techniques:</h4>
      <ul>
        <li><strong>getBoundingClientRect():</strong> Mendapatkan posisi dan ukuran relatif terhadap viewport</li>
        <li><strong>offsetWidth/Height:</strong> Ukuran termasuk padding dan border</li>
        <li><strong>clientWidth/Height:</strong> Ukuran tanpa scrollbar</li>
        <li><strong>scrollWidth/Height:</strong> Total ukuran termasuk content yang tidak terlihat</li>
        <li><strong>ResizeObserver:</strong> Monitor perubahan ukuran secara real-time</li>
        <li><strong>naturalWidth/Height:</strong> Ukuran asli image sebelum di-resize</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.measurements-demo {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.measurements-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.measurement-section {
  background: #f0fdfa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #14b8a6;
}

.measurement-section h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #0f766e;
  text-align: center;
}

.measurement-box {
  width: 150px;
  height: 100px;
  background: #14b8a6;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  margin: 0 auto 15px;
  transition: all 0.3s ease;
}

.position-container {
  position: relative;
  height: 150px;
  background: #e2e8f0;
  border-radius: 4px;
  margin-bottom: 15px;
}

.inner-box {
  position: absolute;
  width: 80px;
  height: 60px;
  background: #14b8a6;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  font-size: 12px;
  top: 10px;
  left: 10px;
}

.viewport-display {
  height: 100px;
  background: linear-gradient(135deg, #14b8a6, #0d9488);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 4px;
  margin-bottom: 15px;
  font-weight: 500;
}

.scrollable-content {
  height: 120px;
  overflow-y: auto;
  border: 2px solid #14b8a6;
  border-radius: 4px;
  margin-bottom: 15px;
  background: white;
}

.long-content {
  padding: 10px;
}

.content-item {
  padding: 8px;
  margin: 5px 0;
  background: #f0fdfa;
  border-radius: 4px;
  text-align: center;
}

.image-container {
  text-align: center;
  margin-bottom: 15px;
}

.measurement-image {
  max-width: 100%;
  height: auto;
  border-radius: 4px;
  border: 2px solid #14b8a6;
}

.measurement-data {
  background: white;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 15px;
  font-size: 14px;
}

.measurement-data p {
  margin: 3px 0;
  color: #374151;
}

.measurement-btn {
  width: 100%;
  padding: 8px;
  background: #14b8a6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.measurement-btn:hover {
  background: #0d9488;
}

.scroll-controls {
  display: flex;
  gap: 5px;
}

.scroll-btn {
  flex: 1;
  padding: 6px;
  background: #0d9488;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.scroll-btn:hover {
  background: #0f766e;
}

.measurement-info {
  background: #ecfeff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #06b6d4;
}

.measurement-info h4 {
  margin-top: 0;
  color: #0e7490;
}

.measurement-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.measurement-info li {
  margin: 5px 0;
  color: #0e7490;
  font-size: 14px;
}
</style>
```

### Template Refs vs Selectors

| Method | Template Refs | Query Selectors |
|--------|---------------|-----------------|
| **Performance** | ✅ Optimal | ❌ Slower |
| **Reactivity** | ✅ Reactive | ❌ Static |
| **Type Safety** | ✅ Type-safe | ❌ String-based |
| **Vue Integration** | ✅ Native | ❌ External |
| **Scope** | ✅ Component-scoped | ❌ Global |

## 🎮 Cara Menggunakan Template Refs

### 1. Deklarasi Ref di Script

```vue
<script setup>
import { ref } from 'vue'

// Deklarasi ref untuk menyimpan referensi
const myElement = ref(null)
const myComponent = ref(null)
</script>
```

### 2. Gunakan di Template

```vue
<template>
  <!-- Hubungkan ref dengan element -->
  <input ref="myElement" />
  <MyComponent ref="myComponent" />
</template>
```

### 3. Akses Ref di Script

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  // Akses DOM element
  console.log(myElement.value) // <input> element

  // Akses component instance
  console.log(myComponent.value) // Component instance
})
</script>
```

## ⚡ Basic Template Ref

### Simple Input Focus

```vue
<script setup>
import { onMounted, ref } from 'vue'

const inputRef = ref('')
const message = ref('')

onMounted(() => {
  // Setelah component di-mount, kita bisa akses DOM element
  console.log('Input element:', inputRef.value)

  // Auto-focus input saat component dimuat
  inputRef.value.focus()

  // Set initial value
  inputRef.value.value = 'Hello Vue!'

  // Add event listener
  inputRef.value.addEventListener('input', handleInput)
})

const handleInput = (event) => {
  message.value = `You typed: ${event.target.value}`
}

const clearInput = () => {
  inputRef.value.value = ''
  inputRef.value.focus()
  message.value = ''
}

const getInputValue = () => {
  alert(`Current value: ${inputRef.value.value}`)
}
</script>

<template>
  <div class="basic-ref-demo">
    <h2>Basic Template Ref</h2>

    <div class="input-section">
      <label for="myInput">Enter your message:</label>
      <input
        id="myInput"
        ref="inputRef"
        type="text"
        placeholder="Auto-focused input"
      />
    </div>

    <div class="message-display">
      <p v-if="message" class="message">{{ message }}</p>
      <p v-else class="placeholder">Start typing to see the message...</p>
    </div>

    <div class="controls">
      <button @click="clearInput">Clear & Focus</button>
      <button @click="getInputValue">Show Current Value</button>
    </div>

    <div class="info-box">
      <h4>ℹ️ What's happening:</h4>
      <ul>
        <li>Input is auto-focused when component loads</li>
        <li>Initial value "Hello Vue!" is set programmatically</li>
        <li>Input events are handled via direct DOM access</li>
        <li>Buttons manipulate input directly</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.basic-ref-demo {
  padding: 20px;
  max-width: 500px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.input-section {
  margin-bottom: 20px;
}

.input-section label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #374151;
}

.input-section input {
  width: 100%;
  padding: 10px;
  border: 2px solid #d1d5db;
  border-radius: 4px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.input-section input:focus {
  outline: none;
  border-color: #3b82f6;
  box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
}

.message-display {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  text-align: center;
  min-height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.message {
  color: #1e40af;
  font-weight: 500;
}

.placeholder {
  color: #9ca3af;
  font-style: italic;
}

.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 8px 16px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.controls button:hover {
  background: #2563eb;
}

.info-box {
  background: #dbeafe;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.info-box h4 {
  margin-top: 0;
  color: #1e40af;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #1e40af;
}
</style>
```

### Multiple Element Refs

```vue
<script setup>
import { onMounted, ref } from 'vue'

const titleRef = ref(null)
const paragraphRef = ref(null)
const buttonRef = ref(null)
const listRef = ref(null)

onMounted(() => {
  console.log('All elements mounted:', {
    title: titleRef.value,
    paragraph: paragraphRef.value,
    button: buttonRef.value,
    list: listRef.value
  })

  // Manipulasi multiple elements
  animateElements()
})

const animateElements = () => {
  // Animate title
  if (titleRef.value) {
    titleRef.value.style.color = '#3b82f6'
    titleRef.value.style.transform = 'scale(1.1)'
    setTimeout(() => {
      titleRef.value.style.transform = 'scale(1)'
    }, 300)
  }

  // Style paragraph
  if (paragraphRef.value) {
    paragraphRef.value.style.background = 'linear-gradient(90deg, #dbeafe, #bfdbfe)'
    paragraphRef.value.style.padding = '15px'
    paragraphRef.value.style.borderRadius = '8px'
  }

  // Add button animation
  if (buttonRef.value) {
    buttonRef.value.addEventListener('click', () => {
      buttonRef.value.style.transform = 'scale(0.95)'
      setTimeout(() => {
        buttonRef.value.style.transform = 'scale(1)'
      }, 100)
    })
  }

  // Style list items
  if (listRef.value) {
    const items = listRef.value.querySelectorAll('li')
    items.forEach((item, index) => {
      item.style.opacity = '0'
      item.style.transform = 'translateX(-20px)'

      setTimeout(() => {
        item.style.transition = 'all 0.3s ease'
        item.style.opacity = '1'
        item.style.transform = 'translateX(0)'
      }, index * 100)
    })
  }
}

const restyleElements = () => {
  // Reset semua styles
  if (titleRef.value) {
    titleRef.value.style.color = '#1f2937'
    titleRef.value.style.transform = 'scale(1)'
  }

  if (paragraphRef.value) {
    paragraphRef.value.style.background = 'transparent'
    paragraphRef.value.style.padding = '0'
    paragraphRef.value.style.borderRadius = '0'
  }

  // Random colors untuk list items
  if (listRef.value) {
    const items = listRef.value.querySelectorAll('li')
    const colors = ['#ef4444', '#f59e0b', '#10b981', '#3b82f6', '#8b5cf6']

    items.forEach((item) => {
      const randomColor = colors[Math.floor(Math.random() * colors.length)]
      item.style.background = randomColor + '20'
      item.style.borderLeft = `4px solid ${randomColor}`
    })
  }
}
</script>

<template>
  <div class="multiple-ref-demo">
    <h2 ref="titleRef">Multiple Element Refs</h2>

    <p ref="paragraphRef">
      This demo shows how to use template refs with multiple DOM elements.
      Each element has its own ref and can be manipulated independently.
    </p>

    <div class="button-section">
      <button ref="buttonRef" @click="restyleElements">
        Restyle Elements
      </button>
    </div>

    <div class="list-section">
      <h3>Features:</h3>
      <ul ref="listRef">
        <li>Direct DOM manipulation</li>
        <li>Individual element control</li>
        <li>Animation capabilities</li>
        <li>Event handling</li>
        <li>Style modifications</li>
      </ul>
    </div>

    <div class="info-box">
      <h4>🔍 Element References:</h4>
      <div class="ref-list">
        <span class="ref-item">titleRef → h2 element</span>
        <span class="ref-item">paragraphRef → p element</span>
        <span class="ref-item">buttonRef → button element</span>
        <span class="ref-item">listRef → ul element</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.multiple-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #1f2937;
  transition: all 0.3s ease;
}

p {
  margin-bottom: 20px;
  line-height: 1.6;
  color: #4b5563;
  transition: all 0.3s ease;
}

.button-section {
  text-align: center;
  margin-bottom: 30px;
}

.button-section button {
  padding: 12px 24px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.button-section button:hover {
  background: #7c3aed;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(139, 92, 246, 0.3);
}

.list-section {
  margin-bottom: 20px;
}

.list-section h3 {
  color: #374151;
  margin-bottom: 10px;
}

.list-section ul {
  list-style: none;
  padding: 0;
}

.list-section li {
  padding: 10px 15px;
  margin-bottom: 5px;
  background: #f8fafc;
  border-radius: 6px;
  transition: all 0.3s ease;
  border-left: 4px solid #8b5cf6;
}

.info-box {
  background: #f3e8ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.info-box h4 {
  margin-top: 0;
  color: #6b21a8;
}

.ref-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 10px;
}

.ref-item {
  background: #e9d5ff;
  color: #6b21a8;
  padding: 5px 10px;
  border-radius: 15px;
  font-size: 12px;
  font-family: monospace;
}
</style>
```

## 🔄 Ref dengan Function

### Dynamic Ref Functions

```vue
<script setup>
import { ref } from 'vue'

const elementInfo = ref([])
const clickCount = ref(0)

// Function ref yang dipanggil untuk setiap element
const elementRefFunction = (el) => {
  if (el) {
    console.log('Element mounted:', el)
    console.log('Text content:', el.textContent)

    // Add element info ke array
    elementInfo.value.push({
      tag: el.tagName,
      text: el.textContent,
      class: el.className,
      id: el.id
    })

    // Manipulasi element
    el.style.padding = '10px'
    el.style.margin = '5px 0'
    el.style.borderRadius = '6px'
    el.style.cursor = 'pointer'
    el.style.transition = 'all 0.3s ease'

    // Add click handler
    el.addEventListener('click', () => {
      clickCount.value++
      el.style.background = `hsl(${clickCount.value * 30}, 70%, 80%)`
      el.style.transform = 'scale(1.05)'

      setTimeout(() => {
        el.style.transform = 'scale(1)'
      }, 200)
    })
  }
}

const resetElements = () => {
  elementInfo.value = []
  clickCount.value = 0
}
</script>

<template>
  <div class="function-ref-demo">
    <h2>Function Refs</h2>

    <div class="elements-container">
      <h3 :ref="elementRefFunction">Click me! (Heading)</h3>
      <p :ref="elementRefFunction">Click this paragraph!</p>
      <div :ref="elementRefFunction">Click this div!</div>
      <span :ref="elementRefFunction">Click this span!</span>
    </div>

    <div class="stats">
      <h4>Statistics:</h4>
      <p>Elements tracked: {{ elementInfo.length }}</p>
      <p>Total clicks: {{ clickCount }}</p>
    </div>

    <div class="element-list">
      <h4>Tracked Elements:</h4>
      <div v-if="elementInfo.length === 0" class="empty">
        No elements tracked yet. The function ref will be called when elements mount.
      </div>
      <div v-else>
        <div
          v-for="(info, index) in elementInfo"
          :key="index"
          class="element-info"
        >
          <span class="tag">{{ info.tag }}</span>
          <span class="text">{{ info.text }}</span>
        </div>
      </div>
    </div>

    <div class="controls">
      <button @click="resetElements">Reset Tracking</button>
    </div>

    <div class="info-box">
      <h4>🎯 How Function Refs Work:</h4>
      <ul>
        <li>Function dipanggil saat element mount/unmount</li>
        <li>Parameter pertama adalah element reference</li>
        <li>Bisa digunakan untuk dynamic element tracking</li>
        <li>Cocok untuk lists atau dynamic content</li>
        <li>Bisa melakukan setup saat element dibuat</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.function-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.elements-container {
  margin: 20px 0;
  padding: 20px;
  background: #f0fdf4;
  border-radius: 8px;
}

.elements-container > * {
  background: white;
  border: 1px solid #d1fae5;
  display: block;
  text-align: center;
  font-weight: 500;
}

.stats {
  background: #dcfce7;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.stats h4 {
  margin-top: 0;
  color: #166534;
}

.stats p {
  margin: 5px 0;
  color: #15803d;
}

.element-list {
  margin-bottom: 20px;
}

.element-list h4 {
  margin-bottom: 10px;
  color: #374151;
}

.empty {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
}

.element-info {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px;
  margin-bottom: 5px;
  background: #f8fafc;
  border-radius: 6px;
}

.tag {
  background: #10b981;
  color: white;
  padding: 3px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-family: monospace;
  font-weight: bold;
}

.text {
  color: #374151;
  font-size: 14px;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 10px 20px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.controls button:hover {
  background: #059669;
}

.info-box {
  background: #ecfdf5;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.info-box h4 {
  margin-top: 0;
  color: #065f46;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #047857;
}
</style>
```

### Ref untuk Dynamic Lists

```vue
<script setup>
import { ref, reactive } from 'vue'

const items = reactive([
  { id: 1, text: 'First Item', color: '#3b82f6' },
  { id: 2, text: 'Second Item', color: '#10b981' },
  { id: 3, text: 'Third Item', color: '#f59e0b' }
])

const itemRefs = ref([])

// Function ref untuk setiap item dalam list
const setItemRef = (el) => {
  if (el) {
    itemRefs.value.push(el)
  }
}

const addItem = () => {
  const colors = ['#ef4444', '#8b5cf6', '#06b6d4', '#84cc16']
  const newId = Math.max(...items.map(item => item.id)) + 1

  items.push({
    id: newId,
    text: `Item ${newId}`,
    color: colors[Math.floor(Math.random() * colors.length)]
  })
}

const removeItem = (id) => {
  const index = items.findIndex(item => item.id === id)
  if (index > -1) {
    items.splice(index, 1)
  }
}

const highlightAll = () => {
  itemRefs.value.forEach((ref, index) => {
    setTimeout(() => {
      ref.style.transform = 'scale(1.1)'
      ref.style.boxShadow = '0 4px 12px rgba(0,0,0,0.15)'

      setTimeout(() => {
        ref.style.transform = 'scale(1)'
        ref.style.boxShadow = 'none'
      }, 300)
    }, index * 100)
  })
}

const randomizeColors = () => {
  const colors = ['#ef4444', '#f59e0b', '#10b981', '#3b82f6', '#8b5cf6', '#06b6d4']

  itemRefs.value.forEach(ref => {
    const randomColor = colors[Math.floor(Math.random() * colors.length)]
    ref.style.background = randomColor
    ref.style.borderColor = randomColor
  })
}
</script>

<template>
  <div class="list-ref-demo">
    <h2>Dynamic List Refs</h2>

    <div class="controls">
      <button @click="addItem">Add Item</button>
      <button @click="highlightAll">Highlight All</button>
      <button @click="randomizeColors">Random Colors</button>
    </div>

    <div class="items-list">
      <div
        v-for="(item, index) in items"
        :key="item.id"
        :ref="setItemRef"
        class="list-item"
        :style="{ borderColor: item.color }"
        @click="removeItem(item.id)"
      >
        <span class="item-text">{{ item.text }}</span>
        <span class="remove-hint">Click to remove</span>
      </div>
    </div>

    <div class="stats">
      <p>Total items: {{ items.length }}</p>
      <p>Tracked refs: {{ itemRefs.length }}</p>
    </div>

    <div class="info-box">
      <h4>📋 List Ref Management:</h4>
      <p>Each list item gets its own ref through the function ref mechanism.</p>
      <p>Vue automatically manages the refs array when items are added/removed.</p>
    </div>
  </div>
</template>

<style scoped>
.list-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  justify-content: center;
}

.controls button {
  padding: 8px 16px;
  background: #06b6d4;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.controls button:hover {
  background: #0891b2;
  transform: translateY(-1px);
}

.items-list {
  margin-bottom: 20px;
}

.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  margin-bottom: 10px;
  background: white;
  border: 2px solid;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.list-item:hover {
  transform: translateX(5px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.item-text {
  font-weight: 500;
  color: #1f2937;
}

.remove-hint {
  font-size: 12px;
  color: #6b7280;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.list-item:hover .remove-hint {
  opacity: 1;
}

.stats {
  display: flex;
  justify-content: space-around;
  padding: 15px;
  background: #f0f9ff;
  border-radius: 8px;
  margin-bottom: 20px;
}

.stats p {
  margin: 0;
  color: #0c4a6e;
  font-weight: 500;
}

.info-box {
  background: #ecfeff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #06b6d4;
}

.info-box h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.info-box p {
  margin: 8px 0;
  color: #075985;
}
</style>
```

## 🎯 Component Refs

### Basic Component Ref

```vue
<!-- MyComponent.vue -->
<script setup>
import { ref } from 'vue'

const count = ref(10)
const message = ref('Hello from child!')

const increment = () => count.value++
const decrement = () => count.value--

const updateMessage = (newMessage) => {
  message.value = newMessage
}

// Expose methods dan data ke parent
defineExpose({
  count,
  message,
  increment,
  decrement,
  updateMessage
})
</script>

<template>
  <div class="child-component">
    <h3>Child Component</h3>
    <p>Count: {{ count }}</p>
    <p>Message: {{ message }}</p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>

<style scoped>
.child-component {
  padding: 20px;
  background: #fef3c7;
  border-radius: 8px;
  border: 2px solid #f59e0b;
  text-align: center;
}

.child-component button {
  margin: 0 5px;
  padding: 5px 10px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

```vue
<!-- Parent Component -->
<script setup>
import { ref, onMounted } from 'vue'
import MyComponent from './MyComponent.vue'

const childRef = ref(null)
const parentMessage = ref('')

onMounted(() => {
  console.log('Child component ref:', childRef.value)

  // Akses child component methods
  if (childRef.value) {
    childRef.value.increment()
    childRef.value.updateMessage('Updated from parent!')
  }
})

const callChildMethods = () => {
  if (childRef.value) {
    // Call child methods
    childRef.value.increment()
    childRef.value.increment()

    // Update child data
    childRef.value.updateMessage('Called from parent button!')

    parentMessage.value = 'Child methods called successfully!'
  }
}

const getChildData = () => {
  if (childRef.value) {
    const data = {
      count: childRef.value.count,
      message: childRef.value.message
    }

    parentMessage.value = `Child data: Count=${data.count}, Message="${data.message}"`
    console.log('Child data:', data)
  }
}
</script>

<template>
  <div class="component-ref-demo">
    <h2>Component Refs</h2>

    <!-- Child component dengan ref -->
    <MyComponent ref="childRef" />

    <div class="parent-controls">
      <h3>Parent Controls</h3>

      <div class="button-group">
        <button @click="callChildMethods">Call Child Methods</button>
        <button @click="getChildData">Get Child Data</button>
      </div>

      <div class="message-display">
        <p v-if="parentMessage" class="message">{{ parentMessage }}</p>
        <p v-else class="placeholder">Click buttons to interact with child component</p>
      </div>

      <div class="info-box">
        <h4>🔗 Parent-Child Communication:</h4>
        <ul>
          <li>Parent gets reference to child via ref attribute</li>
          <li>Child exposes methods/data with defineExpose()</li>
          <li>Parent can call child methods directly</li>
          <li>Parent can access child reactive data</li>
          <li>Maintains component encapsulation</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.component-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.parent-controls {
  margin-top: 30px;
  padding: 20px;
  background: #fffbeb;
  border-radius: 8px;
  border: 1px solid #fed7aa;
}

.parent-controls h3 {
  margin-top: 0;
  color: #92400e;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.button-group button {
  padding: 10px 16px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button-group button:hover {
  background: #d97706;
}

.message-display {
  padding: 15px;
  background: white;
  border-radius: 6px;
  margin-bottom: 20px;
  text-align: center;
  min-height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.message {
  color: #92400e;
  font-weight: 500;
}

.placeholder {
  color: #9ca3af;
  font-style: italic;
}

.info-box {
  background: #fef3c7;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.info-box h4 {
  margin-top: 0;
  color: #92400e;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #78350f;
}
</style>
```

### Multiple Component Refs

```vue
<!-- CounterComponent.vue -->
<script setup>
import { ref } from 'vue'

const props = defineProps({
  initialValue: {
    type: Number,
    default: 0
  },
  label: {
    type: String,
    required: true
  }
})

const count = ref(props.initialValue)

const increment = () => count.value++
const decrement = () => count.value--
const reset = () => count.value = props.initialValue
const getValue = () => count.value

defineExpose({
  count,
  label: props.label,
  increment,
  decrement,
  reset,
  getValue
})
</script>

<template>
  <div class="counter-component">
    <h4>{{ label }}</h4>
    <div class="counter-display">{{ count }}</div>
    <div class="counter-controls">
      <button @click="decrement">-</button>
      <button @click="reset">Reset</button>
      <button @click="increment">+</button>
    </div>
  </div>
</template>

<style scoped>
.counter-component {
  padding: 15px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  text-align: center;
}

.counter-component h4 {
  margin-top: 0;
  color: #374151;
}

.counter-display {
  font-size: 24px;
  font-weight: bold;
  color: #3b82f6;
  margin: 10px 0;
}

.counter-controls button {
  margin: 0 5px;
  padding: 5px 10px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

```vue
<!-- Parent Component -->
<script setup>
import { ref, onMounted } from 'vue'
import CounterComponent from './CounterComponent.vue'

const counterRefs = ref([])
const totalSum = ref(0)
const lastAction = ref('')

const setCounterRef = (el) => {
  if (el) {
    counterRefs.value.push(el)
  }
}

// Calculate total sum
const calculateSum = () => {
  totalSum.value = counterRefs.value.reduce((sum, counter) => {
    return sum + counter.getValue()
  }, 0)
}

// Increment all counters
const incrementAll = () => {
  counterRefs.value.forEach(counter => {
    counter.increment()
  })
  calculateSum()
  lastAction.value = 'Incremented all counters'
}

// Decrement all counters
const decrementAll = () => {
  counterRefs.value.forEach(counter => {
    counter.decrement()
  })
  calculateSum()
  lastAction.value = 'Decremented all counters'
}

// Reset all counters
const resetAll = () => {
  counterRefs.value.forEach(counter => {
    counter.reset()
  })
  calculateSum()
  lastAction.value = 'Reset all counters'
}

// Random operation
const randomOperation = () => {
  counterRefs.value.forEach(counter => {
    const operations = [counter.increment, counter.decrement, counter.reset]
    const randomOp = operations[Math.floor(Math.random() * operations.length)]
    randomOp()
  })
  calculateSum()
  lastAction.value = 'Performed random operations'
}

onMounted(() => {
  calculateSum()
  lastAction.value = 'Component mounted'
})
</script>

<template>
  <div class="multi-component-demo">
    <h2>Multiple Component Refs</h2>

    <div class="counters-grid">
      <CounterComponent
        ref="setCounterRef"
        label="Counter A"
        :initial-value="5"
      />
      <CounterComponent
        ref="setCounterRef"
        label="Counter B"
        :initial-value="10"
      />
      <CounterComponent
        ref="setCounterRef"
        label="Counter C"
        :initial-value="15"
      />
      <CounterComponent
        ref="setCounterRef"
        label="Counter D"
        :initial-value="0"
      />
    </div>

    <div class="master-controls">
      <h3>Master Controls</h3>

      <div class="stats">
        <p><strong>Total Sum:</strong> {{ totalSum }}</p>
        <p><strong>Counters:</strong> {{ counterRefs.length }}</p>
        <p><strong>Last Action:</strong> {{ lastAction }}</p>
      </div>

      <div class="button-grid">
        <button @click="incrementAll">Increment All</button>
        <button @click="decrementAll">Decrement All</button>
        <button @click="resetAll">Reset All</button>
        <button @click="randomOperation">Random</button>
      </div>

      <div class="individual-controls">
        <h4>Individual Controls:</h4>
        <div class="individual-buttons">
          <button
            v-for="(counter, index) in counterRefs"
            :key="index"
            @click="counter.increment()"
            class="individual-btn"
          >
            {{ counter.label }} +
          </button>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>🎛️ Multiple Component Management:</h4>
      <p>Each counter component is tracked via function refs. The parent can:</p>
      <ul>
        <li>Call methods on all components simultaneously</li>
        <li>Access individual component data</li>
        <li>Perform calculations across components</li>
        <li>Maintain individual component control</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.multi-component-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.counters-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.master-controls {
  background: #eff6ff;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #bfdbfe;
}

.master-controls h3 {
  margin-top: 0;
  color: #1e40af;
}

.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
  padding: 15px;
  background: white;
  border-radius: 6px;
}

.stats p {
  margin: 0;
  color: #374151;
}

.button-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 10px;
  margin-bottom: 20px;
}

.button-grid button {
  padding: 10px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button-grid button:hover {
  background: #2563eb;
}

.individual-controls {
  margin-bottom: 20px;
}

.individual-controls h4 {
  margin-bottom: 10px;
  color: #374151;
}

.individual-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.individual-btn {
  padding: 6px 12px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: background-color 0.3s;
}

.individual-btn:hover {
  background: #059669;
}

.info-box {
  background: #dbeafe;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.info-box h4 {
  margin-top: 0;
  color: #1e40af;
}

.info-box p {
  margin: 10px 0;
  color: #1e40af;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #1e3a8a;
}
</style>
```

## 🔧 Ref dengan Watchers

### Ref dengan WatchEffect

```vue
<script setup>
import { ref, watchEffect } from 'vue'

const inputRef = ref()
const message = ref('')
const isVisible = ref(true)

// WatchEffect untuk monitor ref changes
watchEffect(() => {
  console.log('WatchEffect triggered')
  console.log('InputRef value:', inputRef.value)

  if (inputRef.value) {
    console.log('Element is available:', inputRef.value)

    // Auto-focus saat element tersedia
    inputRef.value.focus()

    // Add event listener
    inputRef.value.addEventListener('input', handleInput)

    // Set placeholder
    inputRef.value.placeholder = 'Type something...'

    message.value = 'Input element is ready and focused!'
  } else {
    console.log('Element not available yet or unmounted')
    message.value = 'Input element is not available'
  }
})

const handleInput = (event) => {
  message.value = `You typed: ${event.target.value}`
}

const toggleVisibility = () => {
  isVisible.value = !isVisible.value

  if (!isVisible.value) {
    message.value = 'Input element hidden'
  }
}

const getInputInfo = () => {
  if (inputRef.value) {
    const info = {
      tagName: inputRef.value.tagName,
      type: inputRef.value.type,
      placeholder: inputRef.value.placeholder,
      value: inputRef.value.value,
      hasFocus: document.activeElement === inputRef.value
    }

    message.value = `Input info: ${JSON.stringify(info, null, 2)}`
  }
}
</script>

<template>
  <div class="watch-ref-demo">
    <h2>Refs with Watchers</h2>

    <div class="visibility-toggle">
      <button @click="toggleVisibility">
        {{ isVisible ? 'Hide' : 'Show' }} Input
      </button>
    </div>

    <div v-if="isVisible" class="input-section">
      <label for="watchedInput">Watched Input:</label>
      <input
        id="watchedInput"
        ref="inputRef"
        type="text"
      />
    </div>

    <div class="message-section">
      <h4>Message:</h4>
      <div class="message-display">{{ message }}</div>
    </div>

    <div class="controls">
      <button @click="getInputInfo">Get Input Info</button>
    </div>

    <div class="info-box">
      <h4>👁️ WatchEffect Behavior:</h4>
      <ul>
        <li>WatchEffect runs immediately when component mounts</li>
        <li>Re-runs whenever inputRef.value changes</li>
        <li>Tracks ref availability (null → element → null)</li>
        <li>Useful for setup logic that depends on DOM elements</li>
        <li>Automatically handles cleanup when element is removed</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.watch-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.visibility-toggle {
  text-align: center;
  margin-bottom: 20px;
}

.visibility-toggle button {
  padding: 10px 20px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.visibility-toggle button:hover {
  background: #7c3aed;
}

.input-section {
  margin-bottom: 20px;
  padding: 20px;
  background: #f3e8ff;
  border-radius: 8px;
}

.input-section label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #6b21a8;
}

.input-section input {
  width: 100%;
  padding: 10px;
  border: 2px solid #d8b4fe;
  border-radius: 4px;
  font-size: 16px;
}

.message-section h4 {
  margin-bottom: 10px;
  color: #374151;
}

.message-display {
  padding: 15px;
  background: #faf5ff;
  border-radius: 6px;
  min-height: 50px;
  font-family: monospace;
  font-size: 14px;
  color: #6b21a8;
  white-space: pre-wrap;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 8px 16px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.info-box {
  background: #f3e8ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.info-box h4 {
  margin-top: 0;
  color: #6b21a8;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #6b21a8;
}
</style>
```

### Advanced Watcher dengan Ref

```vue
<script setup>
import { ref, watch, nextTick } from 'vue'

const canvasRef = ref()
const isDrawing = ref(false)
const drawingData = ref([])
const stats = ref({
  strokes: 0,
  points: 0,
  lastAction: 'Ready to draw'
})

// Watch canvas ref untuk setup
watch(canvasRef, async (canvas) => {
  if (canvas) {
    console.log('Canvas element available')

    // Wait for next tick to ensure canvas is fully mounted
    await nextTick()

    // Setup canvas context
    const ctx = canvas.getContext('2d')
    ctx.strokeStyle = '#3b82f6'
    ctx.lineWidth = 2
    ctx.lineCap = 'round'

    // Set canvas size
    canvas.width = canvas.offsetWidth
    canvas.height = canvas.offsetHeight

    // Add event listeners
    setupCanvasEvents(canvas, ctx)

    stats.value.lastAction = 'Canvas initialized'
  }
}, { immediate: true })

const setupCanvasEvents = (canvas, ctx) => {
  let currentPath = []

  const startDrawing = (e) => {
    isDrawing.value = true
    const rect = canvas.getBoundingClientRect()
    const x = e.clientX - rect.left
    const y = e.clientY - rect.top

    currentPath = [{ x, y }]
    ctx.beginPath()
    ctx.moveTo(x, y)

    stats.value.lastAction = 'Started drawing'
  }

  const draw = (e) => {
    if (!isDrawing.value) return

    const rect = canvas.getBoundingClientRect()
    const x = e.clientX - rect.left
    const y = e.clientY - rect.top

    currentPath.push({ x, y })
    ctx.lineTo(x, y)
    ctx.stroke()

    stats.value.points++
  }

  const stopDrawing = () => {
    if (!isDrawing.value) return

    isDrawing.value = false

    if (currentPath.length > 0) {
      drawingData.value.push({
        points: [...currentPath],
        timestamp: Date.now()
      })
      stats.value.strokes++
      stats.value.lastAction = `Stroke ${stats.value.strokes} completed`
    }

    currentPath = []
  }

  canvas.addEventListener('mousedown', startDrawing)
  canvas.addEventListener('mousemove', draw)
  canvas.addEventListener('mouseup', stopDrawing)
  canvas.addEventListener('mouseout', stopDrawing)
}

const clearCanvas = () => {
  if (canvasRef.value) {
    const ctx = canvasRef.value.getContext('2d')
    ctx.clearRect(0, 0, canvasRef.value.width, canvasRef.value.height)

    drawingData.value = []
    stats.value.strokes = 0
    stats.value.points = 0
    stats.value.lastAction = 'Canvas cleared'
  }
}

const downloadCanvas = () => {
  if (canvasRef.value) {
    const link = document.createElement('a')
    link.download = 'drawing.png'
    link.href = canvasRef.value.toDataURL()
    link.click()

    stats.value.lastAction = 'Drawing downloaded'
  }
}
</script>

<template>
  <div class="canvas-ref-demo">
    <h2>Canvas with Ref Watchers</h2>

    <div class="canvas-container">
      <canvas
        ref="canvasRef"
        class="drawing-canvas"
        width="400"
        height="300"
      ></canvas>
    </div>

    <div class="stats">
      <h4>Drawing Stats:</h4>
      <div class="stats-grid">
        <div class="stat-item">
          <span class="label">Strokes:</span>
          <span class="value">{{ stats.strokes }}</span>
        </div>
        <div class="stat-item">
          <span class="label">Points:</span>
          <span class="value">{{ stats.points }}</span>
        </div>
        <div class="stat-item">
          <span class="label">Last Action:</span>
          <span class="value">{{ stats.lastAction }}</span>
        </div>
      </div>
    </div>

    <div class="controls">
      <button @click="clearCanvas">Clear Canvas</button>
      <button @click="downloadCanvas">Download Image</button>
    </div>

    <div class="info-box">
      <h4>🎨 Canvas Ref Management:</h4>
      <ul>
        <li>Watcher monitors canvas element availability</li>
        <li>Canvas context setup happens when element is ready</li>
        <li>Event listeners are added after canvas is mounted</li>
        <li>Uses nextTick to ensure DOM is fully updated</li>
        <li>Drawing data is tracked separately for stats</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.canvas-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #ef4444;
  border-radius: 8px;
}

.canvas-container {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
}

.drawing-canvas {
  border: 2px solid #d1d5db;
  border-radius: 8px;
  background: white;
  cursor: crosshair;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.stats {
  background: #fef2f2;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.stats h4 {
  margin-top: 0;
  color: #991b1b;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 10px;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  background: white;
  border-radius: 4px;
  border: 1px solid #fecaca;
}

.label {
  font-weight: 500;
  color: #7f1d1d;
}

.value {
  font-weight: bold;
  color: #dc2626;
}

.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 10px 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.controls button:hover {
  background: #dc2626;
}

.info-box {
  background: #fef2f2;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ef4444;
}

.info-box h4 {
  margin-top: 0;
  color: #991b1b;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #7f1d1d;
}
</style>
```

## 🚀 Advanced Ref Patterns

### Conditional Refs

```vue
<script setup>
import { ref, watch, computed } from 'vue'

const showInput = ref(true)
const inputType = ref('text')
const inputValue = ref('')
const elementRef = ref()
const elementInfo = ref({})

// Computed untuk menentukan element yang akan dirender
const currentElement = computed(() => {
  if (!showInput.value) return null

  switch (inputType.value) {
    case 'text':
      return 'input'
    case 'textarea':
      return 'textarea'
    case 'select':
      return 'select'
    default:
      return 'input'
  }
})

// Watch element ref changes
watch(elementRef, (element) => {
  if (element) {
    elementInfo.value = {
      tagName: element.tagName,
      type: element.type || 'N/A',
      id: element.id || 'N/A',
      className: element.className || 'None',
      hasFocus: document.activeElement === element
    }

    // Auto-focus
    setTimeout(() => element.focus(), 100)
  } else {
    elementInfo.value = { message: 'No element rendered' }
  }
})

const toggleElement = () => {
  showInput.value = !showInput.value
}

const changeType = (type) => {
  inputType.value = type
}

const getElementInfo = () => {
  if (elementRef.value) {
    const styles = window.getComputedStyle(elementRef.value)
    alert(`Element Info:\n${JSON.stringify(elementInfo.value, null, 2)}\n\nDisplay: ${styles.display}`)
  }
}
</script>

<template>
  <div class="conditional-ref-demo">
    <h2>Conditional Template Refs</h2>

    <div class="controls">
      <div class="control-group">
        <h4>Toggle Element:</h4>
        <button @click="toggleElement">
          {{ showInput ? 'Hide' : 'Show' }} Element
        </button>
      </div>

      <div class="control-group">
        <h4>Element Type:</h4>
        <div class="type-buttons">
          <button
            @click="changeType('text')"
            :class="{ active: inputType === 'text' }"
          >
            Text Input
          </button>
          <button
            @click="changeType('textarea')"
            :class="{ active: inputType === 'textarea' }"
          >
            Textarea
          </button>
          <button
            @click="changeType('select')"
            :class="{ active: inputType === 'select' }"
          >
            Select
          </button>
        </div>
      </div>
    </div>

    <div class="element-container">
      <!-- Conditional rendering dengan ref yang sama -->
      <input
        v-if="showInput && currentElement === 'input'"
        ref="elementRef"
        v-model="inputValue"
        type="text"
        placeholder="Text input"
        class="form-element"
      />

      <textarea
        v-else-if="showInput && currentElement === 'textarea'"
        ref="elementRef"
        v-model="inputValue"
        placeholder="Textarea input"
        class="form-element"
      ></textarea>

      <select
        v-else-if="showInput && currentElement === 'select'"
        ref="elementRef"
        v-model="inputValue"
        class="form-element"
      >
        <option value="">Select an option</option>
        <option value="option1">Option 1</option>
        <option value="option2">Option 2</option>
        <option value="option3">Option 3</option>
      </select>

      <div v-else class="no-element">
        No element rendered
      </div>
    </div>

    <div class="info-display">
      <h4>Element Information:</h4>
      <div class="info-content">
        <div v-if="elementInfo.message" class="info-message">
          {{ elementInfo.message }}
        </div>
        <div v-else class="info-details">
          <p><strong>Tag:</strong> {{ elementInfo.tagName }}</p>
          <p><strong>Type:</strong> {{ elementInfo.type }}</p>
          <p><strong>ID:</strong> {{ elementInfo.id }}</p>
          <p><strong>Class:</strong> {{ elementInfo.className }}</p>
          <p><strong>Has Focus:</strong> {{ elementInfo.hasFocus ? 'Yes' : 'No' }}</p>
        </div>
      </div>

      <button @click="getElementInfo" class="info-btn">
        Get Detailed Info
      </button>
    </div>

    <div class="info-box">
      <h4>🔄 Conditional Ref Behavior:</h4>
      <ul>
        <li>Same ref name used across different element types</li>
        <li>Ref value changes when different element renders</li>
        <li>Watcher tracks ref availability and type changes</li>
        <li>Useful for dynamic form elements</li>
        <li>Maintains focus across element changes</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.conditional-ref-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.controls {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.control-group h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #0c4a6e;
}

.control-group button {
  padding: 8px 16px;
  background: #06b6d4;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.control-group button:hover {
  background: #0891b2;
}

.type-buttons {
  display: flex;
  gap: 8px;
}

.type-buttons button {
  flex: 1;
  padding: 6px 12px;
  background: #e0f2fe;
  color: #0c4a6e;
  border: 1px solid #bae6fd;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

.type-buttons button.active {
  background: #06b6d4;
  color: white;
  border-color: #06b6d4;
}

.element-container {
  min-height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
}

.form-element {
  width: 100%;
  max-width: 300px;
  padding: 10px;
  border: 2px solid #0ea5e9;
  border-radius: 6px;
  font-size: 16px;
}

.no-element {
  color: #64748b;
  font-style: italic;
}

.info-display {
  background: #ecfeff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.info-display h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.info-content {
  margin-bottom: 15px;
}

.info-message {
  text-align: center;
  color: #64748b;
  font-style: italic;
  padding: 10px;
}

.info-details p {
  margin: 5px 0;
  color: #0c4a6e;
}

.info-btn {
  width: 100%;
  padding: 8px;
  background: #0ea5e9;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.info-box {
  background: #f0f9ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #06b6d4;
}

.info-box h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #075985;
}
</style>
```

## 💡 Best Practices

### 1. Initialize Refs dengan null

```vue
<script setup>
import { ref } from 'vue'

// ✅ Good: Initialize dengan null
const myRef = ref(null)

// ❌ Bad: Initialize tanpa value
const myRef = ref()
</script>
```

### 2. Check Ref Availability

```vue
<script setup>
import { onMounted } from 'vue'

const elementRef = ref(null)

onMounted(() => {
  // ✅ Always check if ref exists
  if (elementRef.value) {
    elementRef.value.focus()
  }
})
</script>
```

### 3. Use with WatchEffect untuk Setup Logic

```vue
<script setup>
import { ref, watchEffect } from 'vue'

const canvasRef = ref(null)

watchEffect(() => {
  if (canvasRef.value) {
    // Setup canvas context
    const ctx = canvasRef.value.getContext('2d')
    // ... setup logic
  }
})
</script>
```

### 4. Cleanup Event Listeners

```vue
<script setup>
import { ref, onUnmounted } from 'vue'

const elementRef = ref(null)
const handleResize = () => console.log('Resized')

onMounted(() => {
  if (elementRef.value) {
    window.addEventListener('resize', handleResize)
  }
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})
</script>
```

### 5. Avoid Template Refs untuk Simple Operations

```vue
<template>
  <!-- ❌ Hindari refs untuk simple value updates -->
  <input ref="inputRef" />
  <button @click="inputRef.value.value = 'Hello'">Set Value</button>

  <!-- ✅ Gunakan v-model untuk simple operations -->
  <input v-model="inputValue" />
  <button @click="inputValue = 'Hello'">Set Value</button>
</template>
```

## 🛡️ Safety Considerations

### 1. Ref Tidak Tersedia

```vue
<script setup>
import { ref } from 'vue'

const myRef = ref(null)

const safeOperation = () => {
  // ✅ Selalu cek availability
  if (myRef.value) {
    myRef.value.focus()
  } else {
    console.warn('Element not available')
  }
}
</script>
```

### 2. Lifecycle Considerations

```vue
<script setup>
import { ref, onMounted, nextTick } from 'vue'

const myRef = ref(null)

onMounted(async () => {
  // ✅ Tunggu nextTick untuk DOM updates
  await nextTick()

  if (myRef.value) {
    // Safe to access DOM
  }
})
</script>
```

### 3. Conditional Rendering

```vue
<script setup>
import { ref, watch } from 'vue'

const showElement = ref(true)
const elementRef = ref(null)

watch(elementRef, (element) => {
  if (element) {
    // Element tersedia
  } else {
    // Element tidak dirender (v-if=false)
  }
})
</script>

<template>
  <input v-if="showElement" ref="elementRef" />
</template>
```

---

## 🎉 Kesimpulan

Template Refs adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🎯 Direct DOM Access:** Akses langsung ke DOM elements
2. **⚡ Component Control:** Kontrol penuh atas component instances
3. **🎨 Custom Manipulations:** Manipulasi DOM yang kompleks
4. **📱 Third-party Integration:** Integrasi dengan library eksternal
5. **🔧 Advanced Patterns:** Implementasi patterns yang kompleks

### Poin Penting:

- Gunakan `ref(null)` untuk inisialisasi
- Check availability sebelum penggunaan
- Gunakan `onMounted` atau `watchEffect` untuk setup
- Cleanup resources di `onUnmounted`
- Gunakan `defineExpose()` untuk component refs

### Kapan Menggunakan Template Refs:

- ✅ Focus management
- ✅ Animasi custom
- ✅ Canvas/graphics manipulation
- ✅ Third-party library integration
- ✅ Component method calls
- ✅ DOM measurements

### Kapan Menghindari Template Refs:

- ❌ Simple form handling (gunakan v-model)
- ❌ Basic styling (gunakan CSS classes)
- ❌ Simple DOM manipulations (Vue directives)

Dengan memahami template refs, Anda sudah bisa membangun aplikasi Vue.js dengan kontrol penuh atas DOM dan components! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan berbagai ref patterns dalam aplikasi Anda untuk melihat kapan dan bagaimana mereka bekerja!