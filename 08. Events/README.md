# 🎯 Tutorial Vue.js: Event Handling untuk Pemula

## 📖 Pengenalan Event Handling

**Event Handling** adalah kemampuan Vue.js untuk merespons interaksi pengguna seperti klik, input, hover, dan lainnya. Vue menyediakan directive `v-on` untuk menangani event dengan cara yang clean dan reaktif.

## ❓ Pertanyaan: Apa Fungsi Event Handling di Vue.js?

Event Handling memiliki beberapa fungsi utama yang sangat penting:

### **1. 🖱️ User Interaction Response**
- **Fungsi:** Menangani interaksi user (klik, ketik, hover, scroll)
- **Manfaat:** Membuat aplikasi interaktif dan responsif
- **Contoh:** Tombol yang merespons klik, form yang memproses input

### **2. 🔄 State Management**
- **Fungsi:** Mengubah data/state berdasarkan user action
- **Manfaat:** Mengupdate UI secara real-time
- **Contoh:** Counter yang bertambah saat tombol ditekan

### **3. 📝 Form Processing**
- **Fungsi:** Menangani submit form, validation, input handling
- **Manfaat:** Mengumpulkan dan memproses data user
- **Contoh:** Login form, contact form, search box

### **4. 🎮 Application Control**
- **Fungsi:** Mengontrol alur aplikasi (navigation, modals, dropdowns)
- **Manfaat:** Mengatur UX flow dan user journey
- **Contoh:** Menu dropdown, modal popup, page navigation

### **5. 🎨 Dynamic UI Updates**
- **Fungsi:** Mengubah tampilan berdasarkan event/aksi
- **Manfaat:** Membuat UI yang dinamis dan engaging
- **Contoh:** Toggle dark mode, show/hide content, animations

### **6. 🔗 Component Communication**
- **Fungsi:** Komunikasi antar komponen via custom events
- **Manfaat:** Membangun arsitektur component yang terpisah
- **Contoh:** Child component mengirim data ke parent

## 🤔 Apa itu Event Handling?

**Event Handling** adalah proses mendengarkan dan merespons event yang terjadi pada DOM elements, seperti:
- User mengklik tombol
- User mengetik di input field
- User menghover elemen
- Form disubmit
- Keyboard ditekan

## 📁 Struktur Proyek

```
08. Events/
├── README.md                    # Dokumentasi ini
└── src/
    ├── components/
    │   └── MyEvent.vue          # Komponen dengan contoh event handling
    └── App.vue                  # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `MyEvent.vue`

```vue
<!-- Event handling dengan form submit -->
<script setup>
const submitHandler = (e) => {
  e.preventDefault()
  alert('valid credentials')
}
</script>

<template>
  <form v-on:submit="submitHandler">
    <input type="text" placeholder="Please enter your name" />
    <input type="text" placeholder="Please enter your email" />
    <button type="submit">Submit</button>
  </form>
</template>
```

**Penjelasan:**
- `v-on:submit` adalah directive untuk menangani event submit
- `submitHandler` adalah fungsi yang dipanggil ketika form disubmit
- `e.preventDefault()` mencegah behavior default form (refresh halaman)

## ❓ Pertanyaan: Gimana Cara Penggunaan Event Handling di Vue.js?

Event Handling di Vue.js sangat mudah digunakan dengan berbagai cara:

### **1. Basic Event Handling**
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
const message = ref('')

function increment() {
  count.value++
  message.value = `Count is now: ${count.value}`
}

function sayHello() {
  alert('Hello from Vue.js!')
}
</script>

<template>
  <div>
    <!-- Basic click event -->
    <button @click="increment">Increment ({{ count }})</button>

    <!-- Call function directly -->
    <button @click="sayHello">Say Hello</button>

    <!-- Inline expression -->
    <button @click="count++">Inline Increment</button>

    <!-- Display message -->
    <p v-if="message">{{ message }}</p>
  </div>
</template>
```

### **2. Event with Parameters**
```vue
<script setup>
import { ref } from 'vue'

const userName = ref('')

function greet(name) {
  alert(`Hello, ${name}!`)
}

function updateUser(newName) {
  userName.value = newName
}

function handleClick(event, message) {
  console.log('Event:', event)
  console.log('Message:', message)
  console.log('Target element:', event.target)
}
</script>

<template>
  <div>
    <!-- Pass parameters to function -->
    <button @click="greet('John')">Greet John</button>
    <button @click="greet('Jane')">Greet Jane</button>

    <!-- Pass the event object -->
    <button @click="handleClick($event, 'Button clicked!')">
      Click with Event
    </button>

    <!-- Update reactive data -->
    <input
      @input="updateUser($event.target.value)"
      placeholder="Enter your name"
    />
    <p>User: {{ userName }}</p>
  </div>
</template>
```

### **3. Form Events**
```vue
<script setup>
import { reactive } from 'vue'

const formData = reactive({
  username: '',
  email: '',
  password: ''
})

const errors = reactive({
  username: '',
  email: ''
})

function validateUsername() {
  if (formData.username.length < 3) {
    errors.username = 'Username must be at least 3 characters'
  } else {
    errors.username = ''
  }
}

function validateEmail() {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(formData.email)) {
    errors.email = 'Please enter a valid email'
  } else {
    errors.email = ''
  }
}

function submitForm(event) {
  event.preventDefault() // Prevent default form submission

  // Validate all fields
  validateUsername()
  validateEmail()

  // Check if there are any errors
  if (!errors.username && !errors.email) {
    alert(`Form submitted! Username: ${formData.username}, Email: ${formData.email}`)
  }
}

function resetForm() {
  formData.username = ''
  formData.email = ''
  formData.password = ''
  errors.username = ''
  errors.email = ''
}
</script>

<template>
  <form @submit="submitForm">
    <div>
      <label>Username:</label>
      <input
        v-model="formData.username"
        @input="validateUsername"
        placeholder="Enter username"
        required
      />
      <p v-if="errors.username" style="color: red;">{{ errors.username }}</p>
    </div>

    <div>
      <label>Email:</label>
      <input
        v-model="formData.email"
        @input="validateEmail"
        type="email"
        placeholder="Enter email"
        required
      />
      <p v-if="errors.email" style="color: red;">{{ errors.email }}</p>
    </div>

    <div>
      <label>Password:</label>
      <input
        v-model="formData.password"
        type="password"
        placeholder="Enter password"
        required
      />
    </div>

    <button type="submit">Submit</button>
    <button type="button" @click="resetForm">Reset</button>
  </form>
</template>
```

### **4. Keyboard Events**
```vue
<script setup>
import { ref } from 'vue'

const keyPressed = ref('')
const keyCode = ref('')
const inputText = ref('')

function handleKeyDown(event) {
  keyPressed.value = event.key
  keyCode.value = event.keyCode

  // Special key handling
  if (event.key === 'Enter') {
    console.log('Enter pressed!')
  }

  if (event.ctrlKey && event.key === 's') {
    event.preventDefault()
    console.log('Ctrl+S - Save triggered!')
  }
}

function handleKeyPress(event) {
  // Only allow letters
  if (!/^[a-zA-Z]$/.test(event.key)) {
    event.preventDefault()
  }
}

function handleKeyUp(event) {
  console.log('Key released:', event.key)
}
</script>

<template>
  <div>
    <!-- Basic keyboard events -->
    <input
      @keydown="handleKeyDown"
      @keypress="handleKeyPress"
      @keyup="handleKeyUp"
      placeholder="Type here..."
      v-model="inputText"
    />

    <p>Last key pressed: {{ keyPressed }} ({{ keyCode }})</p>
    <p>Input text: {{ inputText }}</p>

    <!-- Specific key events -->
    <input
      @keyup.enter="alert('Enter pressed!')"
      placeholder="Press Enter for alert"
    />

    <input
      @keyup.ctrl.s="alert('Ctrl+S pressed!')"
      placeholder="Press Ctrl+S"
    />

    <input
      @keyup.esc="inputText = ''"
      placeholder="Press Escape to clear"
      v-model="inputText"
    />
  </div>
</template>
```

### **5. Mouse Events**
```vue
<script setup>
import { ref, reactive } from 'vue'

const mousePosition = reactive({ x: 0, y: 0 })
const isHovering = ref(false)
const clickCount = ref(0)
const isDragging = ref(false)

function handleMouseMove(event) {
  mousePosition.x = event.clientX
  mousePosition.y = event.clientY
}

function handleMouseEnter() {
  isHovering.value = true
}

function handleMouseLeave() {
  isHovering.value = false
}

function handleClick(event) {
  clickCount.value++
  console.log('Clicked at:', event.clientX, event.clientY)
}

function handleRightClick(event) {
  event.preventDefault() // Prevent context menu
  alert('Right clicked!')
}

function handleDragStart(event) {
  isDragging.value = true
  event.dataTransfer.effectAllowed = 'move'
}

function handleDragEnd(event) {
  isDragging.value = false
}
</script>

<template>
  <div>
    <!-- Mouse move tracking -->
    <div
      @mousemove="handleMouseMove"
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave"
      :style="{
        width: '300px',
        height: '200px',
        border: '2px solid #ccc',
        backgroundColor: isHovering ? '#e3f2fd' : '#f5f5f5',
        cursor: 'crosshair'
      }"
    >
      <p>Mouse position: {{ mousePosition.x }}, {{ mousePosition.y }}</p>
      <p>Hovering: {{ isHovering ? 'Yes' : 'No' }}</p>
    </div>

    <!-- Click events -->
    <button @click="handleClick">Click me! ({{ clickCount }} clicks)</button>
    <button @click.left="alert('Left click!')">Left Click</button>
    <button @click.right="handleRightClick">Right Click</button>
    <button @click.middle="alert('Middle click!')">Middle Click</button>

    <!-- Drag and drop -->
    <div
      draggable="true"
      @dragstart="handleDragStart"
      @dragend="handleDragEnd"
      :style="{
        width: '100px',
        height: '50px',
        backgroundColor: '#4caf50',
        color: 'white',
        display: 'flex',
        alignItems: 'center',
        justifyContent: 'center',
        cursor: isDragging ? 'grabbing' : 'grab',
        opacity: isDragging ? 0.5 : 1
      }"
    >
      Drag me!
    </div>
  </div>
</template>
```

### **6. Event Modifiers**
```vue
<script setup>
import { ref } from 'vue'

const message = ref('')
const linkClicked = ref('')

function showMessage(text) {
  message.value = text
}

function handleOuterClick() {
  message.value = 'Outer div clicked'
}

function handleInnerClick() {
  message.value = 'Inner button clicked'
}

function handleOnceClick() {
  message.value = 'This will only work once!'
}
</script>

<template>
  <div>
    <!-- .prevent - Prevent default behavior -->
    <div>
      <h3>.prevent Modifier</h3>
      <a
        href="https://vuejs.org"
        @click.prevent="linkClicked = 'Link click prevented!'"
      >
        Click me (won't navigate)
      </a>
      <p v-if="linkClicked">{{ linkClicked }}</p>
    </div>

    <!-- .stop - Stop event propagation -->
    <div>
      <h3>.stop Modifier</h3>
      <div
        @click="handleOuterClick"
        style="padding: 20px; background: #f0f0f0; margin: 10px 0;"
      >
        Outer Div (click me)
        <button
          @click.stop="handleInnerClick"
          style="margin: 10px;"
        >
          Inner Button (propagation stopped)
        </button>
      </div>
    </div>

    <!-- .once - Only trigger once -->
    <div>
      <h3>.once Modifier</h3>
      <button @click.once="handleOnceClick">
        Click me (only works once)
      </button>
    </div>

    <!-- .self - Only trigger if event.target is the element itself -->
    <div>
      <h3>.self Modifier</h3>
      <div
        @click.self="message = 'Div clicked (not child)'"
        style="padding: 20px; background: #e0e0e0; margin: 10px 0;"
      >
        Div Container (with .self modifier)
        <button style="margin: 5px;">
          Child Button
        </button>
      </div>
    </div>

    <!-- .capture - Use capture mode -->
    <div>
      <h3>.capture Modifier</h3>
      <div
        @click.capture="message = 'Outer div clicked (capture)'"
        style="padding: 20px; background: #d0d0d0; margin: 10px 0;"
      >
        Outer Div (capture mode)
        <button @click="message = 'Inner button clicked'">
          Inner Button
        </button>
      </div>
    </div>

    <!-- .passive - Improve performance for scroll events -->
    <div>
      <h3>.passive Modifier</h3>
      <div
        @wheel.passive="message = 'Wheel scrolled (passive)'"
        style="height: 100px; overflow: auto; background: #f5f5f5; padding: 10px;"
      >
        Scroll here with mouse wheel
        <div style="height: 200px; background: linear-gradient(to bottom, #fff, #ccc);">
          Long content
        </div>
      </div>
    </div>

    <p v-if="message"><strong>Message:</strong> {{ message }}</p>
  </div>
</template>
```

### **7. Multiple Events**
```vue
<script setup>
import { ref } from 'vue'

const logs = ref([])

function addLog(message) {
  logs.value.unshift(`${new Date().toLocaleTimeString()}: ${message}`)
  if (logs.value.length > 10) {
    logs.value.pop()
  }
}

function handleMultipleEvents(event) {
  addLog(`${event.type} event triggered`)
}

function handleMouseEvents(event) {
  addLog(`Mouse ${event.type} at (${event.clientX}, ${event.clientY})`)
}
</script>

<template>
  <div>
    <!-- Multiple events on same element -->
    <button
      @click="addLog('Button clicked')"
      @mouseenter="addLog('Mouse entered')"
      @mouseleave="addLog('Mouse left')"
      @focus="addLog('Button focused')"
      @blur="addLog('Button blurred')"
    >
      Hover and Click Me
    </button>

    <!-- Multiple events with same handler -->
    <div
      @click="handleMultipleEvents"
      @dblclick="handleMultipleEvents"
      @contextmenu="handleMultipleEvents"
      style="padding: 20px; background: #f0f0f0; margin: 10px 0;"
    >
      Click, Double-click, or Right-click here
    </div>

    <!-- Multiple mouse events -->
    <div
      @mousemove="handleMouseEvents"
      @mousedown="handleMouseEvents"
      @mouseup="handleMouseEvents"
      style="height: 100px; background: #e0e0e0; padding: 10px; cursor: crosshair;"
    >
      Move and click mouse here
    </div>

    <!-- Event logs -->
    <div style="margin-top: 20px;">
      <h4>Event Logs:</h4>
      <div style="height: 200px; overflow-y: auto; background: #f5f5f5; padding: 10px;">
        <div v-for="(log, index) in logs" :key="index" style="margin: 2px 0;">
          {{ log }}
        </div>
      </div>
    </div>
  </div>
</template>
```

## 📖 Konsep Fundamental Event Handling

### 1. **Sintaks Event Handling**

#### **Full Syntax: `v-on:`**
```vue
<button v-on:click="doSomething">Click me</button>
```

#### **Shorthand Syntax: `@`**
```vue
<button @click="doSomething">Click me</button>
```

**💡 Tips:** Gunakan shorthand syntax (`@`) karena lebih umum dan lebih ringkas.

## ❓ Pertanyaan: Kapan Event Handling di Vue.js Ini Cocok Digunakan?

Event Handling di Vue.js cocok digunakan dalam situasi berikut:

### ✅ **Sangat Cocok Digunakan Untuk:**

1. **Interactive User Interfaces**
   - Form dengan validation dan feedback
   - Dashboard dengan charts dan controls
   - Shopping carts dengan add/remove items
   - Game interfaces dengan controls

2. **Single Page Applications (SPAs)**
   - Navigation dengan dynamic routing
   - Modal dialogs dan popups
   - Tab switching dan accordions
   - Drag and drop interfaces

3. **Real-time Applications**
   - Chat applications dengan instant messaging
   - Live dashboards dengan auto-refresh
   - Collaboration tools dengan real-time updates
   - Notification systems

4. **Data-driven Applications**
   - Admin panels dengan CRUD operations
   - Search functionality dengan filters
   - Data tables dengan sorting dan pagination
   - Form wizards dengan multi-step processes

5. **Mobile-responsive Applications**
   - Touch gestures untuk mobile devices
   - Swipe actions untuk mobile UI
   - Progressive Web Apps (PWAs)
   - Hybrid applications

### ❌ **Kurang Cocok Digunakan Untuk:**

1. **Static Content Websites**
   ```html
   <!-- ❌ Terlalu kompleks untuk static content -->
   <button @click="alert('Hello')">Click me</button>

   <!-- ✅ Lebih baik HTML biasa untuk static sites -->
   <button onclick="alert('Hello')">Click me</button>
   ```

2. **Simple Display Pages**
   - Portfolio websites tanpa interaksi kompleks
   - Landing pages dengan minimal user interaction
   - Documentation websites

3. **Non-interactive Applications**
   - Data visualization tanpa controls
   - Static presentation slides
   - Information-only websites

### 2. **Reactivity dalam Event Handling**

```vue
<!-- ❌ SALAH - Tidak reaktif -->
<script setup>
let count = 0  // Variable biasa, tidak reaktif
</script>

<template>
  <button @click="count++">Count: {{ count }}</button>
  <!-- Tidak akan bekerja karena count tidak reaktif -->
</template>

<!-- ✅ BENAR - Menggunakan ref() -->
<script setup>
import { ref } from 'vue'
const count = ref(0)  // Reactive data
</script>

<template>
  <button @click="count++">Count: {{ count }}</button>
  <!-- Akan bekerja karena count reaktif -->
</template>
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai jenis event handling:

### Enhanced Demo: Event Handling Showcase

```vue
<script setup>
import { ref, computed, reactive } from 'vue'

// 1. Basic Counter Example
const count = ref(0)
const step = ref(1)

// 2. Input Handling
const name = ref('')
const email = ref('')
const password = ref('')
const message = ref('')

// 3. Mouse Events
const mouseX = ref(0)
const mouseY = ref(0)
const isHovering = ref(false)
const clickCount = ref(0)
const clickPosition = reactive({ x: 0, y: 0 })

// 4. Keyboard Events
const keyPressed = ref('')
const keyCode = ref(0)
const ctrlPressed = ref(false)
const shiftPressed = ref(false)

// 5. Form Events
const formData = reactive({
  username: '',
  email: '',
  age: '',
  gender: '',
  interests: [],
  newsletter: false
})

// 6. Drag and Drop
const isDragging = ref(false)
const dragPosition = reactive({ x: 0, y: 0 })
const dropZoneActive = ref(false)

// 7. Window Events
const windowSize = reactive({ width: 0, height: 0 })
const scrollY = ref(0)

// 8. Custom Events
const customEventCount = ref(0)

// Computed properties
const countInfo = computed(() => {
  if (count.value < 10) return 'Just getting started!'
  if (count.value < 50) return 'Keep going!'
  if (count.value < 100) return 'Almost there!'
  return 'You are a master!'
})

const emailValidation = computed(() => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email.value) ? 'Valid email ✅' : 'Invalid email ❌'
})

const formIsValid = computed(() => {
  return formData.username.length >= 3 &&
         emailValidation.value.includes('Valid') &&
         formData.age >= 18
})

// Event handlers
function increment() {
  count.value += step.value
}

function decrement() {
  count.value -= step.value
}

function reset() {
  count.value = 0
  step.value = 1
}

function doubleCount() {
  count.value *= 2
}

function setCount(newValue) {
  count.value = newValue
}

function greetUser() {
  if (name.value.trim()) {
    message.value = `Hello, ${name.value}! Welcome to Vue.js!`
  } else {
    message.value = 'Please enter your name first!'
  }
}

function clearName() {
  name.value = ''
  message.value = ''
}

function handleMouseMove(event) {
  mouseX.value = event.clientX
  mouseY.value = event.clientY
}

function handleMouseEnter() {
  isHovering.value = true
}

function handleMouseLeave() {
  isHovering.value = false
}

function handleMouseClick(event) {
  clickCount.value++
  clickPosition.x = event.clientX
  clickPosition.y = event.clientY
}

function handleKeyDown(event) {
  keyPressed.value = event.key
  keyCode.value = event.keyCode
  ctrlPressed.value = event.ctrlKey
  shiftPressed.value = event.shiftKey
}

function handleFormSubmit(event) {
  event.preventDefault()
  if (formIsValid.value) {
    message.value = `Form submitted successfully! Welcome ${formData.username}!`
  } else {
    message.value = 'Please fill in all required fields correctly!'
  }
}

function handleDragStart(event) {
  isDragging.value = true
  event.dataTransfer.effectAllowed = 'move'
}

function handleDragEnd(event) {
  isDragging.value = false
}

function handleDragOver(event) {
  event.preventDefault()
  dropZoneActive.value = true
}

function handleDragLeave(event) {
  dropZoneActive.value = false
}

function handleDrop(event) {
  event.preventDefault()
  dropZoneActive.value = false
  dragPosition.x = event.clientX
  dragPosition.y = event.clientY
}

function updateWindowSize() {
  windowSize.width = window.innerWidth
  windowSize.height = window.innerHeight
}

function handleScroll() {
  scrollY.value = window.scrollY
}

function triggerCustomEvent() {
  customEventCount.value++
  // Custom event logic here
}

// Initialize window size on mount
if (typeof window !== 'undefined') {
  updateWindowSize()
  window.addEventListener('resize', updateWindowSize)
  window.addEventListener('scroll', handleScroll)
}
</script>

<template>
  <div class="event-handling-demo">
    <h1>🎯 Vue.js Event Handling Showcase</h1>

    <!-- 1. Basic Counter Events -->
    <section class="demo-section">
      <h2>1️⃣ Basic Counter Events</h2>
      <div class="demo-content">
        <div class="counter-display">
          <h3>Count: {{ count }}</h3>
          <p>{{ countInfo }}</p>
        </div>

        <div class="counter-controls">
          <button @click="increment">Increment (+{{ step }})</button>
          <button @click="decrement">Decrement (-{{ step }})</button>
          <button @click="doubleCount">Double (×2)</button>
          <button @click="reset">Reset</button>
        </div>

        <div class="step-controls">
          <label>Step: </label>
          <button @click="step = 1">1</button>
          <button @click="step = 5">5</button>
          <button @click="step = 10">10</button>
          <button @click="setCount(100)">Set to 100</button>
        </div>
      </div>
    </section>

    <!-- 2. Input Events -->
    <section class="demo-section">
      <h2>2️⃣ Input Events</h2>
      <div class="demo-content">
        <div class="input-group">
          <label>Your Name:</label>
          <input
            v-model="name"
            @input="message = ''"
            placeholder="Enter your name"
            @keyup.enter="greetUser"
          />
          <button @click="greetUser">Greet</button>
          <button @click="clearName">Clear</button>
        </div>

        <div v-if="message" class="message">
          {{ message }}
        </div>

        <div class="input-group">
          <label>Email:</label>
          <input
            v-model="email"
            @input="message = ''"
            placeholder="Enter your email"
          />
          <span class="validation">{{ emailValidation }}</span>
        </div>

        <div class="input-group">
          <label>Message:</label>
          <textarea
            v-model="message"
            placeholder="Type your message here..."
            @keyup.ctrl.enter="message += ' (Sent with Ctrl+Enter)'"
          ></textarea>
        </div>
      </div>
    </section>

    <!-- 3. Mouse Events -->
    <section class="demo-section">
      <h2>3️⃣ Mouse Events</h2>
      <div class="demo-content">
        <div
          class="mouse-tracker"
          @mousemove="handleMouseMove"
          @mouseenter="handleMouseEnter"
          @mouseleave="handleMouseLeave"
          @click="handleMouseClick"
          :class="{ hovering: isHovering }"
        >
          <h3>Mouse Tracker Area</h3>
          <p>Move your mouse here!</p>
          <p>Position: {{ mouseX }}, {{ mouseY }}</p>
          <p>Status: {{ isHovering ? 'Hovering' : 'Not hovering' }}</p>
          <p>Clicks: {{ clickCount }}</p>
          <p>Last click: {{ clickPosition.x }}, {{ clickPosition.y }}</p>
        </div>

        <div class="mouse-demo-buttons">
          <button @click="clickCount = 0">Reset Clicks</button>
          <button @click.right="alert('Right clicked!')" @click.left="alert('Left clicked!')">
            Click me (different actions)
          </button>
          <button @click.middle="alert('Middle clicked!')">
            Middle Click
          </button>
        </div>
      </div>
    </section>

    <!-- 4. Keyboard Events -->
    <section class="demo-section">
      <h2>4️⃣ Keyboard Events</h2>
      <div class="demo-content">
        <input
          type="text"
          placeholder="Press any key here..."
          @keydown="handleKeyDown"
          @keyup.ctrl.s="message = 'Ctrl+S pressed!'"
          @keyup.alt.f4="message = 'Alt+F4 pressed!'"
          class="keyboard-input"
        />

        <div class="keyboard-info">
          <p>Last key pressed: <strong>{{ keyPressed }}</strong></p>
          <p>Key code: <strong>{{ keyCode }}</strong></p>
          <p>Ctrl pressed: <strong>{{ ctrlPressed ? 'Yes' : 'No' }}</strong></p>
          <p>Shift pressed: <strong>{{ shiftPressed ? 'Yes' : 'No' }}</strong></p>
        </div>

        <div class="keyboard-shortcuts">
          <h4>Try these shortcuts:</h4>
          <ul>
            <li>Ctrl+S: Save message</li>
            <li>Alt+F4: Special message</li>
            <li>Enter in name field: Greet user</li>
            <li>Ctrl+Enter in message: Add special text</li>
          </ul>
        </div>
      </div>
    </section>

    <!-- 5. Form Events -->
    <section class="demo-section">
      <h2>5️⃣ Form Events</h2>
      <div class="demo-content">
        <form @submit="handleFormSubmit" class="sample-form">
          <div class="form-group">
            <label>Username:</label>
            <input
              v-model="formData.username"
              type="text"
              placeholder="Enter username"
              required
              @input="message = ''"
            />
          </div>

          <div class="form-group">
            <label>Email:</label>
            <input
              v-model="formData.email"
              type="email"
              placeholder="Enter email"
              required
              @input="message = ''"
            />
          </div>

          <div class="form-group">
            <label>Age:</label>
            <input
              v-model.number="formData.age"
              type="number"
              min="18"
              max="100"
              placeholder="Enter age"
              required
            />
          </div>

          <div class="form-group">
            <label>Gender:</label>
            <select v-model="formData.gender" required>
              <option value="">Select gender</option>
              <option value="male">Male</option>
              <option value="female">Female</option>
              <option value="other">Other</option>
            </select>
          </div>

          <div class="form-group">
            <label>
              <input
                v-model="formData.newsletter"
                type="checkbox"
              />
              Subscribe to newsletter
            </label>
          </div>

          <button type="submit">Submit Form</button>
        </form>

        <div class="form-info">
          <p>Form valid: <strong>{{ formIsValid ? 'Yes ✅' : 'No ❌' }}</strong></p>
          <div v-if="formData.username">
            <p>Username: {{ formData.username }}</p>
          </div>
          <div v-if="formData.age">
            <p>Age: {{ formData.age }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Drag and Drop Events -->
    <section class="demo-section">
      <h2>6️⃣ Drag and Drop Events</h2>
      <div class="demo-content">
        <div
          class="draggable-item"
          draggable="true"
          @dragstart="handleDragStart"
          @dragend="handleDragEnd"
          :class="{ dragging: isDragging }"
        >
          Drag me!
        </div>

        <div
          class="drop-zone"
          @dragover="handleDragOver"
          @dragleave="handleDragLeave"
          @drop="handleDrop"
          :class="{ active: dropZoneActive }"
        >
          <p>Drop zone {{ dropZoneActive ? '(Active!)' : '' }}</p>
          <p v-if="dragPosition.x">
            Last drop: {{ dragPosition.x }}, {{ dragPosition.y }}
          </p>
        </div>
      </div>
    </section>

    <!-- 7. Window Events -->
    <section class="demo-section">
      <h2>7️⃣ Window Events</h2>
      <div class="demo-content">
        <div class="window-info">
          <p>Window size: {{ windowSize.width }} × {{ windowSize.height }}</p>
          <p>Scroll position: {{ scrollY }}px</p>
        </div>

        <p>Try resizing the window or scrolling to see the values change!</p>
      </div>
    </section>

    <!-- 8. Event Modifiers -->
    <section class="demo-section">
      <h2>8️⃣ Event Modifiers</h2>
      <div class="demo-content">
        <div class="modifier-examples">
          <!-- Prevent default -->
          <div class="modifier-group">
            <h4>.prevent - Prevent default behavior</h4>
            <a href="https://vuejs.org" @click.prevent="message = 'Link click prevented!'">
              Click me (won't navigate)
            </a>
          </div>

          <!-- Stop propagation -->
          <div class="modifier-group">
            <h4>.stop - Stop event propagation</h4>
            <div @click="message = 'Outer div clicked'" class="outer-div">
              <button @click.stop="message = 'Inner button clicked (propagation stopped)'">
                Click me
              </button>
            </div>
          </div>

          <!-- Once -->
          <div class="modifier-group">
            <h4>.once - Only trigger once</h4>
            <button @click.once="message = 'This will only work once!'">
              Click me (only once)
            </button>
          </div>

          <!-- Passive -->
          <div class="modifier-group">
            <h4>.passive - Improve performance</h4>
            <div
              @wheel.passive="message = 'Wheel event (passive)'"
              class="wheel-area"
            >
              Scroll here with mouse wheel
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 9. Multiple Events -->
    <section class="demo-section">
      <h2>9️⃣ Multiple Events</h2>
      <div class="demo-content">
        <button
          @click="message = 'Clicked!'"
          @mouseenter="message = 'Mouse entered!'"
          @mouseleave="message = 'Mouse left!'"
          class="multi-event-button"
        >
          Hover and click me!
        </button>

        <input
          @focus="message = 'Input focused!'"
          @blur="message = 'Input blurred!'"
          @input="message = `Typing: ${$event.target.value}`"
          placeholder="Type here..."
          class="multi-event-input"
        />
      </div>
    </section>

    <!-- 10. Custom Events -->
    <section class="demo-section">
      <h2>🔟 Custom Events</h2>
      <div class="demo-content">
        <button @click="triggerCustomEvent">
          Trigger Custom Event ({{ customEventCount }} times)
        </button>

        <div class="custom-event-demo">
          <p>Custom events can be used for component communication</p>
          <p>This demonstrates basic custom event counting</p>
        </div>
      </div>
    </section>

    <!-- Message Display -->
    <div v-if="message" class="message-display">
      <p>{{ message }}</p>
      <button @click="message = ''" class="close-message">×</button>
    </div>
  </div>
</template>

<style scoped>
.event-handling-demo {
  max-width: 1000px;
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
  background: #ffffff;
  border-radius: 10px;
  padding: 20px;
  margin-bottom: 20px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  border-left: 4px solid #42b883;
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.demo-content {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

/* Counter Styles */
.counter-display {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  border-radius: 10px;
  text-align: center;
}

.counter-display h3 {
  font-size: 2em;
  margin: 0;
}

.counter-controls,
.step-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: center;
}

button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

button:hover {
  background-color: #369870;
  transform: translateY(-2px);
}

/* Input Styles */
.input-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.input-group label {
  font-weight: bold;
  color: #2c3e50;
}

input, textarea, select {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

textarea {
  min-height: 80px;
  resize: vertical;
}

.validation {
  font-size: 12px;
  margin-top: 5px;
}

.message {
  background-color: #e8f5e8;
  color: #2c3e50;
  padding: 15px;
  border-radius: 5px;
  border-left: 4px solid #42b883;
}

/* Mouse Events Styles */
.mouse-tracker {
  background-color: #f8f9fa;
  border: 2px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
  cursor: crosshair;
  transition: all 0.3s ease;
}

.mouse-tracker.hovering {
  background-color: #e3f2fd;
  border-color: #2196f3;
  transform: scale(1.02);
}

.mouse-demo-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  justify-content: center;
}

/* Keyboard Styles */
.keyboard-input {
  width: 100%;
  padding: 15px;
  font-size: 16px;
  border: 2px solid #ddd;
  border-radius: 8px;
}

.keyboard-info {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.keyboard-shortcuts {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ffc107;
}

.keyboard-shortcuts ul {
  margin: 0;
  padding-left: 20px;
}

/* Form Styles */
.sample-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
  margin-bottom: 15px;
}

.form-group label {
  font-weight: bold;
  color: #2c3e50;
}

.form-info {
  background-color: #e8f4fd;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #bee5eb;
}

/* Drag and Drop Styles */
.draggable-item {
  background-color: #28a745;
  color: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  cursor: grab;
  user-select: none;
  transition: all 0.3s ease;
}

.draggable-item.dragging {
  opacity: 0.5;
  cursor: grabbing;
  transform: rotate(5deg);
}

.drop-zone {
  background-color: #f8f9fa;
  border: 2px dashed #dee2e6;
  border-radius: 8px;
  padding: 40px;
  text-align: center;
  transition: all 0.3s ease;
}

.drop-zone.active {
  background-color: #e3f2fd;
  border-color: #2196f3;
  border-style: solid;
}

/* Window Events Styles */
.window-info {
  background-color: #6c757d;
  color: white;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
}

/* Event Modifiers Styles */
.modifier-examples {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.modifier-group {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.modifier-group h4 {
  margin: 0 0 10px 0;
  color: #495057;
}

.modifier-group a {
  color: #007bff;
  text-decoration: none;
}

.modifier-group a:hover {
  text-decoration: underline;
}

.outer-div {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 2px solid #28a745;
  text-align: center;
}

.wheel-area {
  background-color: #fff3cd;
  padding: 40px;
  border-radius: 8px;
  border: 2px solid #ffc107;
  text-align: center;
  cursor: ns-resize;
}

/* Multiple Events Styles */
.multi-event-button {
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 25px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.multi-event-button:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.multi-event-input {
  width: 100%;
  padding: 15px;
  font-size: 16px;
  border: 2px solid #007bff;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.multi-event-input:focus {
  outline: none;
  border-color: #0056b3;
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.25);
}

/* Custom Events Styles */
.custom-event-demo {
  background-color: #e7f3ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #007bff;
}

/* Message Display */
.message-display {
  position: fixed;
  top: 20px;
  right: 20px;
  background-color: #28a745;
  color: white;
  padding: 15px 20px;
  border-radius: 8px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  z-index: 1000;
  max-width: 300px;
  animation: slideIn 0.3s ease;
}

.close-message {
  background: none;
  border: none;
  color: white;
  font-size: 20px;
  cursor: pointer;
  padding: 0;
  margin-left: 10px;
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

/* Responsive Design */
@media (max-width: 768px) {
  .event-handling-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .counter-controls,
  .step-controls,
  .mouse-demo-buttons {
    flex-direction: column;
  }

  .keyboard-info {
    grid-template-columns: 1fr;
  }
}
</style>