# 📢 Component Events - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja component events untuk komunikasi dua arah antar komponen dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Component Events?](#-apa-itu-component-events)
- [🎮 Cara Menggunakan Events](#-cara-menggunakan-events)
- [📡 Emitting Events](#-emitting-events)
- [👂 Listening to Events](#-listening-to-events)
- [🎯 Event Arguments](#-event-arguments)
- [✅ Event Validation](#-event-validation)
- [🔧 Advanced Event Patterns](#-advanced-event-patterns)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Component Events?

**Component Events** adalah cara bagi child component untuk berkomunikasi dengan parent component. Jika props mengalir dari parent ke child (one-way data flow), maka events mengalir dari child ke parent.

### Konsep Dasar Events

```
Parent Component
    ↓ (props data)
Child Component
    ↑ (emit events)
Parent Component
```

### Mengapa Events Penting?

1. **🔄 Two-Way Communication:** Child bisa memberi tahu parent tentang perubahan
2. **🎯 User Interactions:** Menangani interaksi user dalam child component
3. **📊 Data Flow:** Mempertahankan flow data yang jelas
4. **🛡️ Decoupling:** Parent dan child tetap independent

## 🎮 Cara Menggunakan Events

### 1. Deklarasi Events

Child component perlu mendeklarasikan events yang bisa di-emit:

```vue
<script setup>
// Cara 1: Array syntax (sederhana)
defineEmits(['click', 'submit', 'update'])

// Cara 2: Object syntax (dengan validasi)
defineEmits({
  click: null,
  submit: (payload) => {
    // Validasi payload
    return payload && typeof payload.name === 'string'
  }
})
</script>
```

### 2. Emitting Events

Child component bisa emit events menggunakan `$emit`:

```vue
<template>
  <button @click="handleClick">Click Me</button>
  <button @click="handleSubmit">Submit Form</button>
</template>

<script setup>
const emit = defineEmits(['click', 'submit'])

const handleClick = () => {
  emit('click')
}

const handleSubmit = () => {
  emit('submit', { name: 'John', age: 25 })
}
</script>
```

### 3. Listening to Events

Parent component bisa listen events dari child:

```vue
<script setup>
import ChildComponent from './ChildComponent.vue'

const handleClick = () => {
  console.log('Button clicked!')
}

const handleSubmit = (data) => {
  console.log('Form submitted:', data)
}
</script>

<template>
  <ChildComponent @click="handleClick" @submit="handleSubmit" />
</template>
```

## 📡 Emitting Events

### Basic Emit

```vue
<template>
  <!-- Emit tanpa arguments -->
  <button @click="$emit('increment')">+</button>

  <!-- Emit dengan arguments -->
  <button @click="$emit('update', newValue)">Update</button>
</template>

<script setup>
const emit = defineEmits(['increment', 'update'])

const handleIncrement = () => {
  emit('increment')
}

const handleUpdate = (value) => {
  emit('update', value)
}
</script>
```

### Emit dengan Multiple Arguments

```vue
<script setup>
const emit = defineEmits(['user-action'])

const performAction = () => {
  const user = { name: 'John', age: 25 }
  const action = 'login'
  const timestamp = new Date()

  emit('user-action', user, action, timestamp)
}
</script>

<template>
  <button @click="performAction">Perform Action</button>
</template>
```

### Emit dengan Object Payload

```vue
<script setup>
const emit = defineEmits(['form-submit'])

const submitForm = () => {
  const formData = {
    username: username.value,
    email: email.value,
    password: password.value
  }

  emit('form-submit', formData)
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <!-- form fields -->
  </form>
</template>
```

## 👂 Listening to Events

### Inline Handlers

```vue
<template>
  <!-- Direct method call -->
  <ChildComponent @click="count++" />

  <!-- Method with arguments -->
  <ChildComponent @update="value = $event" />

  <!-- Multiple arguments -->
  <ChildComponent @user-data="handleUserData($event)" />
</template>
```

### Method Handlers

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

const handleClick = () => {
  count.value++
  console.log('Clicked:', count.value)
}

const handleUpdate = (newValue) => {
  console.log('Updated to:', newValue)
}

const handleUserData = (user, action, timestamp) => {
  console.log('User:', user)
  console.log('Action:', action)
  console.log('Time:', timestamp)
}
</script>

<template>
  <ChildComponent
    @click="handleClick"
    @update="handleUpdate"
    @user-data="handleUserData"
  />
</template>
```

### Event Modifiers

Vue.js menyediakan event modifiers untuk menangani events dengan lebih baik:

```vue
<template>
  <!-- Event modifiers -->
  <ChildComponent
    @submit.prevent="handleSubmit"
    @click.stop="handleClick"
    @keyup.enter="handleEnter"
    @once="handleOnce"
  />
</template>
```

## 🎯 Event Arguments

### Single Argument

```vue
<!-- Child Component -->
<script setup>
const emit = defineEmits(['update-value'])

const updateValue = () => {
  emit('update-value', 42)
}
</script>

<!-- Parent Component -->
<script setup>
const handleUpdate = (value) => {
  console.log('New value:', value) // 42
}
</script>

<template>
  <ChildComponent @update-value="handleUpdate" />
</template>
```

### Multiple Arguments

```vue
<!-- Child Component -->
<script setup>
const emit = defineEmits(['user-info'])

const sendUserInfo = () => {
  emit('user-info', 'John Doe', 25, 'john@example.com')
}
</script>

<!-- Parent Component -->
<script setup>
const handleUserInfo = (name, age, email) => {
  console.log('Name:', name)    // John Doe
  console.log('Age:', age)      // 25
  console.log('Email:', email)  // john@example.com
}
</script>

<template>
  <ChildComponent @user-info="handleUserInfo" />
</template>
```

### Object Arguments

```vue
<!-- Child Component -->
<script setup>
const emit = defineEmits(['form-data'])

const submitForm = () => {
  const data = {
    username: 'johndoe',
    email: 'john@example.com',
    preferences: {
      theme: 'dark',
      notifications: true
    }
  }

  emit('form-data', data)
}
</script>

<!-- Parent Component -->
<script setup>
const handleFormData = (formData) => {
  console.log('Username:', formData.username)
  console.log('Email:', formData.email)
  console.log('Theme:', formData.preferences.theme)
}
</script>

<template>
  <ChildComponent @form-data="handleFormData" />
</template>
```

## ✅ Event Validation

Event validation memastikan data yang di-emit sesuai dengan yang diharapkan.

### Basic Validation

```vue
<script setup>
const emit = defineEmits({
  // Event tanpa validasi
  click: null,

  // Event dengan validasi return boolean
  submit: (payload) => {
    if (!payload || typeof payload !== 'object') {
      console.warn('Submit payload must be an object')
      return false
    }
    return true
  },

  // Event dengan validasi kompleks
  'update-user': ({ username, email, age }) => {
    if (!username || typeof username !== 'string') {
      console.log('Please enter a valid username')
      return false
    }
    if (!email || !email.includes('@')) {
      console.log('Please enter a valid email')
      return false
    }
    if (!age || age < 18 || age > 100) {
      console.log('Age must be between 18 and 100')
      return false
    }
    return true
  }
})

const handleSubmit = (formData) => {
  emit('submit', formData)
}

const updateUser = (userData) => {
  emit('update-user', userData)
}
</script>
```

### Advanced Validation with Console Logs

```vue
<script setup>
const emit = defineEmits({
  'user-login': ({ username, password }) => {
    // Validasi username
    if (!username) {
      console.log('❌ Username is required')
      return false
    }
    if (username.length < 3) {
      console.log('❌ Username must be at least 3 characters')
      return false
    }

    // Validasi password
    if (!password) {
      console.log('❌ Password is required')
      return false
    }
    if (password.length < 8) {
      console.log('❌ Password must be at least 8 characters')
      return false
    }

    console.log('✅ User login data is valid')
    return true
  }
})

const handleLogin = (username, password) => {
  emit('user-login', { username, password })
}
</script>
```

### Type-Safe Events with TypeScript

```vue
<script setup lang="ts">
interface UserInfo {
  username: string
  email: string
  age: number
}

interface Emits {
  (e: 'user-info', user: UserInfo): void
  (e: 'update', value: string): void
  (e: 'delete', id: number): void
}

const emit = defineEmits<Emits>()

const updateUserInfo = (user: UserInfo) => {
  emit('user-info', user)
}
</script>
```

## 🔧 Advanced Event Patterns

### Custom Event Bus

Untuk komunikasi antar komponen yang tidak parent-child:

```vue
<!-- eventBus.js -->
import { ref } from 'vue'

export const eventBus = ref({
  events: {},

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = []
    }
    this.events[event].push(callback)
  },

  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach(callback => callback(...args))
    }
  },

  off(event, callback) {
    if (this.events[event]) {
      this.events[event] = this.events[event].filter(cb => cb !== callback)
    }
  }
})

<!-- Component A -->
<script setup>
import { eventBus } from './eventBus'

const triggerEvent = () => {
  eventBus.value.emit('custom-event', { data: 'Hello from Component A' })
}
</script>

<!-- Component B -->
<script setup>
import { onMounted, onUnmounted } from 'vue'
import { eventBus } from './eventBus'

const handleCustomEvent = (data) => {
  console.log('Received event:', data)
}

onMounted(() => {
  eventBus.value.on('custom-event', handleCustomEvent)
})

onUnmounted(() => {
  eventBus.value.off('custom-event', handleCustomEvent)
})
</script>
```

### V-Model with Custom Events

```vue
<!-- CustomInput.vue -->
<script setup>
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const updateValue = (event) => {
  emit('update:modelValue', event.target.value)
}
</script>

<template>
  <input
    :value="modelValue"
    @input="updateValue"
  />
</template>

<!-- Parent Component -->
<script setup>
import { ref } from 'vue'

const searchText = ref('')
</script>

<template>
  <CustomInput v-model="searchText" />
  <p>Search: {{ searchText }}</p>
</template>
```

### Multiple V-Model

```vue
<!-- UserForm.vue -->
<script setup>
const props = defineProps({
  firstName: String,
  lastName: String
})

const emit = defineEmits([
  'update:firstName',
  'update:lastName'
])

const updateFirstName = (event) => {
  emit('update:firstName', event.target.value)
}

const updateLastName = (event) => {
  emit('update:lastName', event.target.value)
}
</script>

<template>
  <input
    :value="firstName"
    @input="updateFirstName"
    placeholder="First Name"
  />
  <input
    :value="lastName"
    @input="updateLastName"
    placeholder="Last Name"
  />
</template>

<!-- Parent Component -->
<script setup>
import { ref } from 'vue'

const firstName = ref('')
const lastName = ref('')
</script>

<template>
  <UserForm
    v-model:firstName="firstName"
    v-model:lastName="lastName"
  />
  <p>Full Name: {{ firstName }} {{ lastName }}</p>
</template>
```

## 🚀 Best Practices

### 1. Event Naming Conventions

```vue
<script setup>
// ✅ Gunakan kebab-case untuk event names
const emit = defineEmits([
  'user-clicked',
  'form-submitted',
  'data-updated',
  'modal-closed'
])

// ❌ Hindari camelCase untuk event names
const emit = defineEmits([
  'userClicked',      // Kurang konsisten
  'formSubmitted'     // Kurang konsisten
])
</script>
```

### 2. Always Declare Events

```vue
<script setup>
// ✅ Selalu deklarasikan events
const emit = defineEmits(['click', 'submit', 'update'])

// ✅ Dengan validasi
const emit = defineEmits({
  submit: (payload) => validatePayload(payload)
})

// ❌ Hindari emit tanpa deklarasi
// (meskipun Vue memungkinkan, kurang baik untuk maintainability)
</script>
```

### 3. Payload Consistency

```vue
<script setup>
// ✅ Payload yang konsisten
const emit = defineEmits(['user-update'])

const updateUser = (user) => {
  emit('user-update', {
    id: user.id,
    name: user.name,
    email: user.email
  })
}

// ❌ Payload yang tidak konsisten
const updateUser = (user) => {
  if (user.isAdmin) {
    emit('user-update', user)              // Full object
  } else {
    emit('user-update', user.name)         // String saja
  }
}
</script>
```

### 4. Error Handling

```vue
<script setup>
const emit = defineEmits(['submit'])

const handleSubmit = async (formData) => {
  try {
    // Validasi data
    if (!validateForm(formData)) {
      emit('submit-error', { message: 'Invalid form data' })
      return
    }

    // Kirim data
    const result = await api.submit(formData)
    emit('submit-success', result)

  } catch (error) {
    emit('submit-error', { message: error.message })
  }
}
</script>
```

## 💡 Tips dan Trik

### 1. Event Debouncing

```vue
<script setup>
import { ref } from 'vue'

const emit = defineEmits(['search'])
const searchQuery = ref('')
const debounceTimer = ref(null)

const handleSearch = () => {
  clearTimeout(debounceTimer.value)
  debounceTimer.value = setTimeout(() => {
    emit('search', searchQuery.value)
  }, 300)
}
</script>

<template>
  <input
    v-model="searchQuery"
    @input="handleSearch"
    placeholder="Search..."
  />
</template>
```

### 2. Event Bubbling Control

```vue
<script setup>
const emit = defineEmits(['item-click'])

const handleItemClick = (event) => {
  // Stop event bubbling
  event.stopPropagation()

  emit('item-click', {
    id: event.target.dataset.id,
    timestamp: Date.now()
  })
}
</script>

<template>
  <div @click="handleItemClick">
    <button data-id="1">Item 1</button>
    <button data-id="2">Item 2</button>
  </div>
</template>
```

### 3. Event Composition

```vue
<script setup>
const emit = defineEmits(['complex-action'])

const handleComplexAction = () => {
  // Gabungkan multiple events
  const actionData = {
    type: 'user-action',
    payload: {
      user: getCurrentUser(),
      action: 'click',
      metadata: {
        timestamp: Date.now(),
        sessionId: getSessionId()
      }
    }
  }

  emit('complex-action', actionData)
}
</script>
```

### 4. Async Event Handling

```vue
<script setup>
const emit = defineEmits(['data-loaded'])

const loadData = async () => {
  try {
    const data = await fetchData()
    emit('data-loaded', { success: true, data })
  } catch (error) {
    emit('data-loaded', { success: false, error: error.message })
  }
}
</script>
```

---

## 🎉 Kesimpulan

Component Events adalah fitur fundamental dalam Vue.js yang memungkinkan:

1. **📡 Two-Way Communication** - Child bisa berkomunikasi dengan parent
2. **🎯 User Interactions** - Menangani interaksi user dengan efektif
3. **🔄 Event-Driven Architecture** - Membangun aplikasi yang reaktif
4. **🛡️ Data Validation** - Memastikan data yang valid dikirim
5. **📊 Clean Architecture** - Memisahkan concerns antar komponen

### Poin Penting:

- Events selalu mengalir dari child ke parent
- Selalu deklarasikan events untuk maintainability
- Gunakan event validation untuk keamanan
- Berikan payload yang konsisten dan terstruktur
- Handle error dengan baik dalam event handlers

### Lanjutan:

- Provide/inject untuk komunikasi yang lebih kompleks
- Event bus untuk komunikasi antar komponen yang tidak berhubungan
- Composition functions untuk event logic yang reusable
- Type-safe events dengan TypeScript

Dengan memahami component events, Anda sudah bisa membangun aplikasi Vue.js yang interaktif dan terstruktur dengan baik! 🚀

---

**🎯 Ready for Practice?** Coba buat komponen-komponen dengan berbagai jenis events untuk memperkuat pemahaman Anda!

## ❓ Pertanyaan: Apa Fungsi Component Events?

Component Events memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk komunikasi antar komponen:

### 1. **Two-Way Communication (Child to Parent)**
```vue
<!-- CounterComponent.vue - Child yang berkomunikasi ke parent -->
<script setup>
const emit = defineEmits(['increment', 'decrement', 'reset'])

// Fungsi 1: Child memberi tahu parent tentang perubahan
const count = ref(0)
const maxValue = 10

const increment = () => {
  if (count.value < maxValue) {
    count.value++
    emit('increment', {
      value: count.value,
      timestamp: Date.now(),
      action: 'increment'
    })
  } else {
    emit('max-reached', {
      value: count.value,
      message: 'Maximum value reached'
    })
  }
}

const decrement = () => {
  if (count.value > 0) {
    count.value--
    emit('decrement', {
      value: count.value,
      timestamp: Date.now(),
      action: 'decrement'
    })
  } else {
    emit('min-reached', {
      value: count.value,
      message: 'Minimum value reached'
    })
  }
}

const reset = () => {
  count.value = 0
  emit('reset', {
    value: count.value,
    action: 'reset',
    previousValue: count.value
  })
}
</script>

<template>
  <div class="counter">
    <h3>Counter: {{ count }}</h3>
    <div class="controls">
      <button @click="decrement" :disabled="count === 0">-</button>
      <button @click="reset">Reset</button>
      <button @click="increment" :disabled="count >= maxValue">+</button>
    </div>
    <div class="progress">
      <div class="progress-bar" :style="{ width: (count / maxValue * 100) + '%' }"></div>
    </div>
  </div>
</template>

<!-- ParentComponent.vue - Parent yang menerima events -->
<script setup>
import { ref } from 'vue'
import CounterComponent from './CounterComponent.vue'

const eventHistory = ref([])
const currentValue = ref(0)
const notifications = ref([])

const handleIncrement = (data) => {
  currentValue.value = data.value
  addToHistory('increment', data)
  showNotification(`Incremented to ${data.value}`, 'success')
}

const handleDecrement = (data) => {
  currentValue.value = data.value
  addToHistory('decrement', data)
  showNotification(`Decremented to ${data.value}`, 'info')
}

const handleReset = (data) => {
  currentValue.value = data.value
  addToHistory('reset', data)
  showNotification('Counter reset to 0', 'warning')
}

const handleMaxReached = (data) => {
  addToHistory('max-reached', data)
  showNotification(data.message, 'error')
}

const handleMinReached = (data) => {
  addToHistory('min-reached', data)
  showNotification(data.message, 'error')
}

const addToHistory = (eventType, data) => {
  eventHistory.value.unshift({
    event: eventType,
    data,
    time: new Date().toLocaleTimeString()
  })
  if (eventHistory.value.length > 10) {
    eventHistory.value.pop()
  }
}

const showNotification = (message, type) => {
  notifications.value.push({ message, type, id: Date.now() })
  setTimeout(() => {
    notifications.value = notifications.value.filter(n => n.id !== notifications.value[0]?.id)
  }, 3000)
}
</script>

<template>
  <div class="parent-component">
    <h1>Parent Component</h1>
    <p>Current Value: <strong>{{ currentValue }}</strong></p>

    <!-- Notifications -->
    <div class="notifications">
      <div
        v-for="notification in notifications"
        :key="notification.id"
        :class="['notification', notification.type]"
      >
        {{ notification.message }}
      </div>
    </div>

    <!-- Child component dengan event listeners -->
    <CounterComponent
      @increment="handleIncrement"
      @decrement="handleDecrement"
      @reset="handleReset"
      @max-reached="handleMaxReached"
      @min-reached="handleMinReached"
    />

    <!-- Event history -->
    <div class="event-history">
      <h3>Event History:</h3>
      <div v-for="event in eventHistory" :key="event.time" class="history-item">
        <span class="time">{{ event.time }}</span>
        <span class="event">{{ event.event }}</span>
        <span class="value">Value: {{ event.data.value }}</span>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Bidirectional communication** - child bisa memberi informasi ke parent
- ✅ **Real-time updates** - parent langsung tahu perubahan di child
- ✅ **Data synchronization** - state tetap sinkron antar komponen
- ✅ **Event tracking** - mudah trace dan debug komunikasi

### 2. **User Interaction Handling**
```vue
<!-- InteractiveButton.vue - Komponen untuk handling user interactions -->
<script setup>
const emit = defineEmits([
  'button-click',
  'long-press',
  'double-click',
  'hover-start',
  'hover-end',
  'focus-gained',
  'focus-lost'
])

const isPressed = ref(false)
const isHovered = ref(false)
const pressTimer = ref(null)
const clickCount = ref(0)
const lastClickTime = ref(0)

// Fungsi 2: Menangani berbagai jenis user interactions
const handleMouseDown = () => {
  isPressed.value = true

  // Detect long press
  pressTimer.value = setTimeout(() => {
    emit('long-press', {
      duration: 1000,
      timestamp: Date.now(),
      buttonType: 'primary'
    })
  }, 1000)
}

const handleMouseUp = () => {
  isPressed.value = false
  clearTimeout(pressTimer.value)

  // Detect double click
  const currentTime = Date.now()
  if (currentTime - lastClickTime.value < 300) {
    clickCount.value++
    if (clickCount.value === 2) {
      emit('double-click', {
        clickCount: clickCount.value,
        timestamp: currentTime
      })
      clickCount.value = 0
    }
  } else {
    clickCount.value = 1
  }
  lastClickTime.value = currentTime

  emit('button-click', {
    clickType: clickCount.value === 1 ? 'single' : 'double',
    timestamp: currentTime,
    wasPressed: isPressed.value
  })
}

const handleMouseEnter = () => {
  isHovered.value = true
  emit('hover-start', {
    timestamp: Date.now(),
    element: 'button'
  })
}

const handleMouseLeave = () => {
  isHovered.value = false
  emit('hover-end', {
    timestamp: Date.now(),
    duration: Date.now() - (hoverStartTime.value || Date.now())
  })
}

const handleFocus = () => {
  emit('focus-gained', {
    timestamp: Date.now(),
    source: 'keyboard'
  })
}

const handleBlur = () => {
  emit('focus-lost', {
    timestamp: Date.now(),
    duration: Date.now() - focusStartTime.value
  })
}

const hoverStartTime = ref(null)
const focusStartTime = ref(null)

watch(isHovered, (newValue) => {
  if (newValue) {
    hoverStartTime.value = Date.now()
  }
})

// Keyboard accessibility
const handleKeydown = (event) => {
  if (event.key === 'Enter' || event.key === ' ') {
    event.preventDefault()
    handleMouseUp()
    emit('button-click', {
      clickType: 'keyboard',
      key: event.key,
      timestamp: Date.now()
    })
  }
}
</script>

<template>
  <button
    class="interactive-button"
    :class="{
      'is-pressed': isPressed,
      'is-hovered': isHovered
    }"
    @mousedown="handleMouseDown"
    @mouseup="handleMouseUp"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
    @focus="handleFocus"
    @blur="handleBlur"
    @keydown="handleKeydown"
  >
    <slot>Click Me!</slot>
  </button>
</template>

<!-- InteractionDemo.vue - Parent yang menangani interactions -->
<script setup>
import { ref } from 'vue'
import InteractiveButton from './InteractiveButton.vue'

const interactionLog = ref([])
const stats = ref({
  clicks: 0,
  doubleClicks: 0,
  longPresses: 0,
  hovers: 0
})

const logInteraction = (type, data) => {
  const logEntry = {
    type,
    data,
    time: new Date().toLocaleTimeString()
  }
  interactionLog.value.unshift(logEntry)
  if (interactionLog.value.length > 20) {
    interactionLog.value.pop()
  }
  updateStats(type)
}

const updateStats = (type) => {
  switch (type) {
    case 'button-click':
      stats.value.clicks++
      break
    case 'double-click':
      stats.value.doubleClicks++
      break
    case 'long-press':
      stats.value.longPresses++
      break
    case 'hover-start':
      stats.value.hovers++
      break
  }
}

const clearLog = () => {
  interactionLog.value = []
  stats.value = {
    clicks: 0,
    doubleClicks: 0,
    longPresses: 0,
    hovers: 0
  }
}
</script>

<template>
  <div class="interaction-demo">
    <h1>User Interaction Demo</h1>

    <!-- Stats dashboard -->
    <div class="stats">
      <h3>Interaction Stats:</h3>
      <div class="stat-item">Clicks: {{ stats.clicks }}</div>
      <div class="stat-item">Double Clicks: {{ stats.doubleClicks }}</div>
      <div class="stat-item">Long Presses: {{ stats.longPresses }}</div>
      <div class="stat-item">Hovers: {{ stats.hovers }}</div>
    </div>

    <!-- Interactive button dengan semua event listeners -->
    <InteractiveButton
      @button-click="logInteraction('button-click', $event)"
      @double-click="logInteraction('double-click', $event)"
      @long-press="logInteraction('long-press', $event)"
      @hover-start="logInteraction('hover-start', $event)"
      @hover-end="logInteraction('hover-end', $event)"
      @focus-gained="logInteraction('focus-gained', $event)"
      @focus-lost="logInteraction('focus-lost', $event)"
    >
      Interact with Me!
    </InteractiveButton>

    <!-- Interaction log -->
    <div class="interaction-log">
      <h3>Interaction Log:</h3>
      <button @click="clearLog">Clear Log</button>
      <div class="log-entries">
        <div
          v-for="entry in interactionLog"
          :key="entry.time + entry.type"
          class="log-entry"
        >
          <span class="time">{{ entry.time }}</span>
          <span class="type">{{ entry.type }}</span>
          <span class="data">{{ JSON.stringify(entry.data) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Rich interactions** - menangani berbagai jenis user interactions
- ✅ **Accessibility support** - support keyboard dan screen readers
- ✅ **Event tracking** - log dan analisis user behavior
- ✅ **Responsive feedback** - memberikan feedback real-time

### 3. **Form Data Management & Validation**
```vue
<!-- SmartForm.vue - Form dengan advanced event handling -->
<script setup>
const emit = defineEmits([
  'field-change',
  'field-focus',
  'field-blur',
  'form-submit',
  'form-reset',
  'validation-error',
  'submit-success',
  'submit-error'
])

const formData = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  terms: false
})

const fieldErrors = ref({})
const isSubmitting = ref(false)

// Fungsi 3: Form validation dan management dengan events
const validators = {
  username: (value) => {
    if (!value) return 'Username is required'
    if (value.length < 3) return 'Username must be at least 3 characters'
    if (!/^[a-zA-Z0-9_]+$/.test(value)) return 'Username can only contain letters, numbers, and underscores'
    return null
  },
  email: (value) => {
    if (!value) return 'Email is required'
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!emailRegex.test(value)) return 'Please enter a valid email'
    return null
  },
  password: (value) => {
    if (!value) return 'Password is required'
    if (value.length < 8) return 'Password must be at least 8 characters'
    if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(value)) return 'Password must contain uppercase, lowercase, and number'
    return null
  },
  confirmPassword: (value) => {
    if (!value) return 'Please confirm your password'
    if (value !== formData.password) return 'Passwords do not match'
    return null
  }
}

const validateField = (fieldName, value) => {
  const error = validators[fieldName]?.(value)
  fieldErrors.value[fieldName] = error

  emit('field-change', {
    fieldName,
    value,
    error,
    isValid: !error,
    timestamp: Date.now()
  })

  return !error
}

const handleFieldChange = (fieldName, event) => {
  const value = event.target.type === 'checkbox' ? event.target.checked : event.target.value
  formData[fieldName] = value
  validateField(fieldName, value)
}

const handleFieldFocus = (fieldName) => {
  emit('field-focus', {
    fieldName,
    value: formData[fieldName],
    timestamp: Date.now()
  })
}

const handleFieldBlur = (fieldName) => {
  emit('field-blur', {
    fieldName,
    value: formData[fieldName],
    error: fieldErrors.value[fieldName],
    isValid: !fieldErrors.value[fieldName],
    timestamp: Date.now()
  })
}

const validateForm = () => {
  let isValid = true
  Object.keys(formData).forEach(fieldName => {
    const fieldValid = validateField(fieldName, formData[fieldName])
    if (!fieldValid) isValid = false
  })

  if (!isValid) {
    emit('validation-error', {
      errors: fieldErrors.value,
      message: 'Please fix the errors before submitting',
      timestamp: Date.now()
    })
  }

  return isValid
}

const handleSubmit = async () => {
  if (!validateForm()) return

  isSubmitting.value = true

  try {
    // Simulasi API call
    await new Promise(resolve => setTimeout(resolve, 2000))

    emit('submit-success', {
      data: { ...formData },
      message: 'Form submitted successfully!',
      timestamp: Date.now()
    })

    // Reset form on success
    resetForm()
  } catch (error) {
    emit('submit-error', {
      error: error.message,
      message: 'Failed to submit form',
      timestamp: Date.now()
    })
  } finally {
    isSubmitting.value = false
  }
}

const resetForm = () => {
  Object.keys(formData).forEach(key => {
    formData[key] = key === 'terms' ? false : ''
  })
  fieldErrors.value = {}

  emit('form-reset', {
    timestamp: Date.now(),
    message: 'Form has been reset'
  })
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="smart-form">
    <h2>Smart Form with Event Handling</h2>

    <!-- Username field -->
    <div class="form-group" :class="{ 'has-error': fieldErrors.username }">
      <label for="username">Username:</label>
      <input
        id="username"
        type="text"
        :value="formData.username"
        @input="handleFieldChange('username', $event)"
        @focus="handleFieldFocus('username')"
        @blur="handleFieldBlur('username')"
        placeholder="Enter username"
      />
      <div v-if="fieldErrors.username" class="error-message">
        {{ fieldErrors.username }}
      </div>
    </div>

    <!-- Email field -->
    <div class="form-group" :class="{ 'has-error': fieldErrors.email }">
      <label for="email">Email:</label>
      <input
        id="email"
        type="email"
        :value="formData.email"
        @input="handleFieldChange('email', $event)"
        @focus="handleFieldFocus('email')"
        @blur="handleFieldBlur('email')"
        placeholder="Enter email"
      />
      <div v-if="fieldErrors.email" class="error-message">
        {{ fieldErrors.email }}
      </div>
    </div>

    <!-- Password field -->
    <div class="form-group" :class="{ 'has-error': fieldErrors.password }">
      <label for="password">Password:</label>
      <input
        id="password"
        type="password"
        :value="formData.password"
        @input="handleFieldChange('password', $event)"
        @focus="handleFieldFocus('password')"
        @blur="handleFieldBlur('password')"
        placeholder="Enter password"
      />
      <div v-if="fieldErrors.password" class="error-message">
        {{ fieldErrors.password }}
      </div>
    </div>

    <!-- Confirm password field -->
    <div class="form-group" :class="{ 'has-error': fieldErrors.confirmPassword }">
      <label for="confirmPassword">Confirm Password:</label>
      <input
        id="confirmPassword"
        type="password"
        :value="formData.confirmPassword"
        @input="handleFieldChange('confirmPassword', $event)"
        @focus="handleFieldFocus('confirmPassword')"
        @blur="handleFieldBlur('confirmPassword')"
        placeholder="Confirm password"
      />
      <div v-if="fieldErrors.confirmPassword" class="error-message">
        {{ fieldErrors.confirmPassword }}
      </div>
    </div>

    <!-- Terms checkbox -->
    <div class="form-group">
      <label>
        <input
          type="checkbox"
          :checked="formData.terms"
          @change="handleFieldChange('terms', $event)"
        />
        I agree to the terms and conditions
      </label>
    </div>

    <!-- Form actions -->
    <div class="form-actions">
      <button type="button" @click="resetForm">Reset Form</button>
      <button type="submit" :disabled="isSubmitting || !formData.terms">
        {{ isSubmitting ? 'Submitting...' : 'Submit Form' }}
      </button>
    </div>
  </form>
</template>

<!-- FormDemo.vue - Parent yang monitor form events -->
<script setup>
import { ref } from 'vue'
import SmartForm from './SmartForm.vue'

const eventLog = ref([])
const formStats = ref({
  fieldChanges: 0,
  validationErrors: 0,
  submitAttempts: 0,
  successfulSubmissions: 0
})

const logEvent = (eventType, data) => {
  const logEntry = {
    type: eventType,
    data,
    time: new Date().toLocaleTimeString()
  }
  eventLog.value.unshift(logEntry)
  if (eventLog.value.length > 15) {
    eventLog.value.pop()
  }
  updateStats(eventType)
}

const updateStats = (eventType) => {
  switch (eventType) {
    case 'field-change':
      formStats.value.fieldChanges++
      break
    case 'validation-error':
      formStats.value.validationErrors++
      break
    case 'form-submit':
      formStats.value.submitAttempts++
      break
    case 'submit-success':
      formStats.value.successfulSubmissions++
      break
  }
}

const handleSubmitSuccess = (data) => {
  logEvent('submit-success', data)
  alert(`Form submitted successfully!\n\nData: ${JSON.stringify(data.data, null, 2)}`)
}

const handleSubmitError = (data) => {
  logEvent('submit-error', data)
  alert(`Form submission failed: ${data.message}`)
}

const clearLog = () => {
  eventLog.value = []
  formStats.value = {
    fieldChanges: 0,
    validationErrors: 0,
    submitAttempts: 0,
    successfulSubmissions: 0
  }
}
</script>

<template>
  <div class="form-demo">
    <h1>Form Event Management Demo</h1>

    <!-- Form statistics -->
    <div class="stats">
      <h3>Form Statistics:</h3>
      <div class="stat-item">Field Changes: {{ formStats.fieldChanges }}</div>
      <div class="stat-item">Validation Errors: {{ formStats.validationErrors }}</div>
      <div class="stat-item">Submit Attempts: {{ formStats.submitAttempts }}</div>
      <div class="stat-item">Successful Submissions: {{ formStats.successfulSubmissions }}</div>
    </div>

    <!-- Smart form dengan comprehensive event handling -->
    <SmartForm
      @field-change="logEvent('field-change', $event)"
      @field-focus="logEvent('field-focus', $event)"
      @field-blur="logEvent('field-blur', $event)"
      @form-submit="logEvent('form-submit', $event)"
      @form-reset="logEvent('form-reset', $event)"
      @validation-error="logEvent('validation-error', $event)"
      @submit-success="handleSubmitSuccess"
      @submit-error="handleSubmitError"
    />

    <!-- Event log -->
    <div class="event-log">
      <h3>Form Event Log:</h3>
      <button @click="clearLog">Clear Log</button>
      <div class="log-entries">
        <div
          v-for="entry in eventLog"
          :key="entry.time + entry.type"
          class="log-entry"
          :class="entry.type"
        >
          <span class="time">{{ entry.time }}</span>
          <span class="type">{{ entry.type }}</span>
          <span class="data">{{ JSON.stringify(entry.data) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Real-time validation** - validation saat user mengetik
- ✅ **Comprehensive tracking** - monitoring semua form interactions
- ✅ **Error handling** - graceful error management
- ✅ **Submit workflow** - complete submission process dengan feedback

### 4. **State Synchronization & Updates**
```vue
<!-- SyncedList.vue - List yang sinkron dengan parent -->
<script setup>
const emit = defineEmits([
  'item-added',
  'item-removed',
  'item-updated',
  'item-selected',
  'items-reordered',
  'batch-update'
])

const props = defineProps({
  items: {
    type: Array,
    required: true
  },
  maxItems: {
    type: Number,
    default: 10
  },
  allowReorder: {
    type: Boolean,
    default: true
  }
})

const localItems = ref([...props.items])
const selectedItem = ref(null)
const dragOverItem = ref(null)

// Fungsi 4: Sinkronisasi state dengan parent melalui events
const addItem = (itemData) => {
  if (localItems.value.length >= props.maxItems) {
    emit('max-items-reached', {
      currentCount: localItems.value.length,
      maxItems: props.maxItems,
      attemptedItem: itemData
    })
    return false
  }

  const newItem = {
    id: Date.now(),
    ...itemData,
    createdAt: new Date().toISOString(),
    order: localItems.value.length
  }

  localItems.value.push(newItem)

  emit('item-added', {
    item: newItem,
    totalCount: localItems.value.length,
    timestamp: Date.now()
  })

  return newItem
}

const removeItem = (itemId) => {
  const itemIndex = localItems.value.findIndex(item => item.id === itemId)
  if (itemIndex === -1) return false

  const removedItem = localItems.value[itemIndex]
  localItems.value.splice(itemIndex, 1)

  // Update order for remaining items
  localItems.value.forEach((item, index) => {
    item.order = index
  })

  emit('item-removed', {
    item: removedItem,
    newCount: localItems.value.length,
    affectedItems: localItems.value.map(item => ({ id: item.id, order: item.order })),
    timestamp: Date.now()
  })

  return removedItem
}

const updateItem = (itemId, updates) => {
  const itemIndex = localItems.value.findIndex(item => item.id === itemId)
  if (itemIndex === -1) return false

  const oldItem = { ...localItems.value[itemIndex] }
  localItems.value[itemIndex] = {
    ...localItems.value[itemIndex],
    ...updates,
    updatedAt: new Date().toISOString()
  }

  emit('item-updated', {
    itemId,
    oldItem,
    newItem: localItems.value[itemIndex],
    changes: updates,
    timestamp: Date.now()
  })

  return localItems.value[itemIndex]
}

const selectItem = (itemId) => {
  const item = localItems.value.find(item => item.id === itemId)
  if (!item) return false

  const previousSelection = selectedItem.value
  selectedItem.value = item

  emit('item-selected', {
    item,
    previousSelection,
    selectionType: previousSelection ? 'change' : 'select',
    timestamp: Date.now()
  })

  return item
}

const reorderItems = (fromIndex, toIndex) => {
  if (!props.allowReorder || fromIndex === toIndex) return false

  const [movedItem] = localItems.value.splice(fromIndex, 1)
  localItems.value.splice(toIndex, 0, movedItem)

  // Update order for all items
  localItems.value.forEach((item, index) => {
    item.order = index
  })

  emit('items-reordered', {
    movedItem,
    fromIndex,
    toIndex,
    newOrder: localItems.value.map(item => ({ id: item.id, order: item.order })),
    timestamp: Date.now()
  })

  return true
}

const batchUpdate = (updates) => {
  const oldItems = [...localItems.value]
  let changedCount = 0

  updates.forEach(({ id, ...changes }) => {
    const itemIndex = localItems.value.findIndex(item => item.id === id)
    if (itemIndex !== -1) {
      localItems.value[itemIndex] = {
        ...localItems.value[itemIndex],
        ...changes,
        updatedAt: new Date().toISOString()
      }
      changedCount++
    }
  })

  emit('batch-update', {
    changedCount,
    totalItems: localItems.value.length,
    updates,
    oldItems,
    newItems: [...localItems.value],
    timestamp: Date.now()
  })

  return changedCount
}

// Drag and drop handlers
const handleDragStart = (event, item) => {
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('text/plain', item.id)
}

const handleDragOver = (event, item) => {
  event.preventDefault()
  dragOverItem.value = item
}

const handleDrop = (event, targetItem) => {
  event.preventDefault()
  const draggedItemId = event.dataTransfer.getData('text/plain')
  const draggedItemIndex = localItems.value.findIndex(item => item.id == draggedItemId)
  const targetIndex = localItems.value.findIndex(item => item.id === targetItem.id)

  if (draggedItemIndex !== -1 && targetIndex !== -1) {
    reorderItems(draggedItemIndex, targetIndex)
  }

  dragOverItem.value = null
}
</script>

<template>
  <div class="synced-list">
    <h3>Synced List ({{ localItems.length }}/{{ maxItems }} items)</h3>

    <!-- Add item controls -->
    <div class="add-controls">
      <button @click="addItem({ name: `Item ${localItems.length + 1}`, type: 'generated' })">
        Add Item
      </button>
      <button @click="batchUpdate(localItems.map(item => ({ id: item.id, status: 'updated' })))">
        Batch Update All
      </button>
    </div>

    <!-- List items -->
    <div class="list-items">
      <div
        v-for="(item, index) in localItems"
        :key="item.id"
        class="list-item"
        :class="{
          'selected': selectedItem?.id === item.id,
          'drag-over': dragOverItem?.id === item.id
        }"
        draggable="true"
        @click="selectItem(item.id)"
        @dragstart="handleDragStart($event, item)"
        @dragover="handleDragOver($event, item)"
        @drop="handleDrop($event, item)"
      >
        <div class="item-header">
          <span class="order">{{ item.order + 1 }}</span>
          <h4>{{ item.name }}</h4>
          <span class="type">{{ item.type }}</span>
        </div>
        <div class="item-controls">
          <button @click.stop="updateItem(item.id, { name: item.name + ' (Updated)' })">
            Update
          </button>
          <button @click.stop="removeItem(item.id)" class="remove">
            Remove
          </button>
        </div>
        <div class="item-meta">
          <small>Created: {{ new Date(item.createdAt).toLocaleTimeString() }}</small>
          <small v-if="item.updatedAt">Updated: {{ new Date(item.updatedAt).toLocaleTimeString() }}</small>
        </div>
      </div>
    </div>

    <!-- Selection info -->
    <div v-if="selectedItem" class="selection-info">
      <h4>Selected Item:</h4>
      <pre>{{ JSON.stringify(selectedItem, null, 2) }}</pre>
    </div>
  </div>
</template>

<!-- SyncDemo.vue - Parent yang sinkronisasi dengan list -->
<script setup>
import { ref, reactive } from 'vue'
import SyncedList from './SyncedList.vue'

const masterList = ref([
  { id: 1, name: 'Initial Item 1', type: 'initial', createdAt: new Date().toISOString() },
  { id: 2, name: 'Initial Item 2', type: 'initial', createdAt: new Date().toISOString() }
])

const syncLog = ref([])
const stats = reactive({
  totalAdded: 0,
  totalRemoved: 0,
  totalUpdated: 0,
  totalReordered: 0
})

const logSyncEvent = (eventType, data) => {
  const logEntry = {
    type: eventType,
    data,
    time: new Date().toLocaleTimeString()
  }
  syncLog.value.unshift(logEntry)
  if (syncLog.value.length > 20) {
    syncLog.value.pop()
  }
  updateStats(eventType, data)
}

const updateStats = (eventType, data) => {
  switch (eventType) {
    case 'item-added':
      stats.totalAdded++
      break
    case 'item-removed':
      stats.totalRemoved++
      break
    case 'item-updated':
      stats.totalUpdated++
      break
    case 'items-reordered':
      stats.totalReordered++
      break
  }
}

const handleItemAdded = (eventData) => {
  logSyncEvent('item-added', eventData)
  masterList.value.push(eventData.item)
}

const handleItemRemoved = (eventData) => {
  logSyncEvent('item-removed', eventData)
  const index = masterList.value.findIndex(item => item.id === eventData.item.id)
  if (index !== -1) {
    masterList.value.splice(index, 1)
  }
}

const handleItemUpdated = (eventData) => {
  logSyncEvent('item-updated', eventData)
  const index = masterList.value.findIndex(item => item.id === eventData.itemId)
  if (index !== -1) {
    masterList.value[index] = eventData.newItem
  }
}

const handleItemsReordered = (eventData) => {
  logSyncEvent('items-reordered', eventData)
  masterList.value.sort((a, b) => {
    const aOrder = eventData.newOrder.find(item => item.id === a.id)?.order ?? 0
    const bOrder = eventData.newOrder.find(item => item.id === b.id)?.order ?? 0
    return aOrder - bOrder
  })
}

const handleBatchUpdate = (eventData) => {
  logSyncEvent('batch-update', eventData)
  masterList.value = [...eventData.newItems]
}

const clearSyncLog = () => {
  syncLog.value = []
  Object.keys(stats).forEach(key => {
    stats[key] = 0
  })
}
</script>

<template>
  <div class="sync-demo">
    <h1>State Synchronization Demo</h1>

    <!-- Statistics -->
    <div class="stats">
      <h3>Synchronization Statistics:</h3>
      <div class="stat-item">Items Added: {{ stats.totalAdded }}</div>
      <div class="stat-item">Items Removed: {{ stats.totalRemoved }}</div>
      <div class="stat-item">Items Updated: {{ stats.totalUpdated }}</div>
      <div class="stat-item">Reorders: {{ stats.totalReordered }}</div>
    </div>

    <div class="demo-container">
      <!-- Child component dengan event handlers -->
      <div class="child-section">
        <h2>Child Component</h2>
        <SyncedList
          :items="masterList"
          :max-items="8"
          :allow-reorder="true"
          @item-added="handleItemAdded"
          @item-removed="handleItemRemoved"
          @item-updated="handleItemUpdated"
          @items-reordered="handleItemsReordered"
          @batch-update="handleBatchUpdate"
          @item-selected="logSyncEvent('item-selected', $event)"
        />
      </div>

      <!-- Parent state display -->
      <div class="parent-section">
        <h2>Parent Master List</h2>
        <div class="master-list">
          <div
            v-for="item in masterList"
            :key="item.id"
            class="master-item"
          >
            <span class="order">{{ masterList.indexOf(item) + 1 }}</span>
            <span class="name">{{ item.name }}</span>
            <span class="type">{{ item.type }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Sync log -->
    <div class="sync-log">
      <h3>Synchronization Log:</h3>
      <button @click="clearSyncLog">Clear Log</button>
      <div class="log-entries">
        <div
          v-for="entry in syncLog"
          :key="entry.time + entry.type"
          class="log-entry"
        >
          <span class="time">{{ entry.time }}</span>
          <span class="type">{{ entry.type }}</span>
          <span class="data">{{ JSON.stringify(entry.data) }}</span>
        </div>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **State synchronization** - parent dan child tetap sinkron
- ✅ **Real-time updates** - perubahan langsung terlihat di semua tempat
- ✅ **Batch operations** - support untuk multiple updates
- ✅ **Event tracking** - monitoring semua perubahan state

### 5. **Custom Event Patterns & Communication Flows**
```vue
<!-- EventHubComponent.vue - Komponen dengan advanced event patterns -->
<script setup>
const emit = defineEmits([
  'action-completed',
  'workflow-step',
  'error-occurred',
  'state-changed',
  'notification',
  'data-request',
  'data-response'
])

// Fungsi 5: Complex event patterns untuk komunikasi yang advanced
const workflowState = ref('idle')
const actionQueue = ref([])
const eventHistory = ref([])

const executeWorkflow = async (workflowType, data) => {
  try {
    // Step 1: Initialize workflow
    emit('workflow-step', {
      step: 'initialize',
      workflowType,
      data,
      timestamp: Date.now()
    })

    workflowState.value = 'processing'

    // Step 2: Validate data
    emit('workflow-step', {
      step: 'validate',
      data,
      timestamp: Date.now()
    })

    const isValid = await validateWorkflowData(workflowType, data)
    if (!isValid) {
      throw new Error('Data validation failed')
    }

    // Step 3: Process data
    emit('workflow-step', {
      step: 'process',
      data,
      timestamp: Date.now()
    })

    const result = await processWorkflowData(workflowType, data)

    // Step 4: Complete workflow
    emit('workflow-step', {
      step: 'complete',
      result,
      timestamp: Date.now()
    })

    emit('action-completed', {
      action: workflowType,
      result,
      duration: Date.now() - startTime,
      success: true
    })

    workflowState.value = 'completed'

  } catch (error) {
    emit('error-occurred', {
      error: error.message,
      workflowType,
      step: workflowState.value,
      timestamp: Date.now()
    })

    workflowState.value = 'error'

    emit('notification', {
      type: 'error',
      message: `Workflow failed: ${error.message}`,
      timestamp: Date.now()
    })
  }
}

const validateWorkflowData = async (workflowType, data) => {
  // Simulate validation
  await new Promise(resolve => setTimeout(resolve, 500))
  return data && data.id
}

const processWorkflowData = async (workflowType, data) => {
  // Simulate processing
  await new Promise(resolve => setTimeout(resolve, 1000))
  return {
    ...data,
    processed: true,
    processedAt: new Date().toISOString()
  }
}

const emitStateChange = (oldState, newState, context = {}) => {
  emit('state-changed', {
    oldState,
    newState,
    context,
    timestamp: Date.now()
  })

  eventHistory.value.push({
    type: 'state-change',
    from: oldState,
    to: newState,
    context,
    timestamp: Date.now()
  })
}

// Watch untuk state changes
watch(workflowState, (newState, oldState) => {
  emitStateChange(oldState, newState, { component: 'EventHubComponent' })
})

const requestExternalData = (requestId, query) => {
  emit('data-request', {
    requestId,
    query,
    timestamp: Date.now()
  })

  // Simulate async data request
  setTimeout(() => {
    emit('data-response', {
      requestId,
      data: { result: `Data for ${query}`, timestamp: Date.now() },
      success: true
    })
  }, 1500)
}

const addToQueue = (action, data) => {
  actionQueue.value.push({
    id: Date.now(),
    action,
    data,
    timestamp: Date.now()
  })

  emit('notification', {
    type: 'info',
    message: `Action "${action}" added to queue`,
    timestamp: Date.now()
  })
}

const processQueue = async () => {
  while (actionQueue.value.length > 0) {
    const { action, data } = actionQueue.value.shift()
    await executeWorkflow(action, data)
  }
}
</script>

<template>
  <div class="event-hub">
    <h2>Event Hub Component</h2>

    <div class="status">
      <p>Current State: <strong>{{ workflowState }}</strong></p>
      <p>Queue Length: {{ actionQueue.length }}</p>
      <p>Event History: {{ eventHistory.length }} events</p>
    </div>

    <div class="controls">
      <button @click="executeWorkflow('user-login', { id: 123, username: 'john' })">
        Execute Login Workflow
      </button>
      <button @click="executeWorkflow('data-sync', { id: 456, type: 'sync' })">
        Execute Sync Workflow
      </button>
      <button @click="addToQueue('user-login', { id: 789, username: 'jane' })">
        Add to Queue
      </button>
      <button @click="processQueue" :disabled="actionQueue.length === 0">
        Process Queue
      </button>
      <button @click="requestExternalData(Date.now(), 'user-data')">
        Request External Data
      </button>
    </div>
  </div>
</template>

<!-- EventPatternDemo.vue - Parent yang handles complex event patterns -->
<script setup>
import { ref, reactive } from 'vue'
import EventHubComponent from './EventHubComponent.vue'

const eventLog = ref([])
const workflowStatus = reactive({})
const notifications = ref([])
const dataCache = ref({})

const logEvent = (eventType, data, source = 'child') => {
  const logEntry = {
    type: eventType,
    data,
    source,
    time: new Date().toLocaleTimeString()
  }
  eventLog.value.unshift(logEntry)
  if (eventLog.value.length > 25) {
    eventLog.value.pop()
  }
}

const handleWorkflowStep = (eventData) => {
  logEvent('workflow-step', eventData)
  workflowStatus[eventData.workflowType] = {
    currentStep: eventData.step,
    lastUpdate: eventData.timestamp
  }
}

const handleActionCompleted = (eventData) => {
  logEvent('action-completed', eventData)

  const successMessage = `✅ ${eventData.action} completed in ${eventData.duration}ms`
  addNotification('success', successMessage)

  // Update workflow status
  workflowStatus[eventData.action] = {
    status: 'completed',
    lastUpdate: eventData.timestamp,
    result: eventData.result
  }
}

const handleErrorOccurred = (eventData) => {
  logEvent('error-occurred', eventData)

  const errorMessage = `❌ Error in ${eventData.workflowType}: ${eventData.error}`
  addNotification('error', errorMessage)

  // Update workflow status
  workflowStatus[eventData.workflowType] = {
    status: 'error',
    error: eventData.error,
    lastUpdate: eventData.timestamp
  }
}

const handleNotification = (eventData) => {
  logEvent('notification', eventData)
  addNotification(eventData.type, eventData.message)
}

const handleDataRequest = (eventData) => {
  logEvent('data-request', eventData)
  addNotification('info', `📡 Requesting data: ${eventData.query}`)
}

const handleDataResponse = (eventData) => {
  logEvent('data-response', eventData)

  if (eventData.success) {
    dataCache.value[eventData.requestId] = eventData.data
    addNotification('success', `✅ Data received for request ${eventData.requestId}`)
  } else {
    addNotification('error', `❌ Failed to get data for request ${eventData.requestId}`)
  }
}

const handleStateChanged = (eventData) => {
  logEvent('state-change', eventData)
  addNotification('info', `🔄 State changed from ${eventData.oldState} to ${eventData.newState}`)
}

const addNotification = (type, message) => {
  const notification = {
    id: Date.now() + Math.random(),
    type,
    message,
    timestamp: Date.now()
  }
  notifications.value.unshift(notification)

  // Auto-remove after 5 seconds
  setTimeout(() => {
    notifications.value = notifications.value.filter(n => n.id !== notification.id)
  }, 5000)
}

const clearEventLog = () => {
  eventLog.value = []
  Object.keys(workflowStatus).forEach(key => delete workflowStatus[key])
  notifications.value = []
  dataCache.value = {}
}
</script>

<template>
  <div class="event-pattern-demo">
    <h1>Advanced Event Patterns Demo</h1>

    <!-- Notifications -->
    <div class="notifications">
      <h3>Notifications:</h3>
      <div
        v-for="notification in notifications"
        :key="notification.id"
        :class="['notification', notification.type]"
      >
        {{ notification.message }}
      </div>
    </div>

    <!-- Workflow status -->
    <div class="workflow-status">
      <h3>Workflow Status:</h3>
      <div
        v-for="(status, workflow) in workflowStatus"
        :key="workflow"
        class="workflow-item"
      >
        <strong>{{ workflow }}:</strong>
        <span :class="['status', status.status || status.currentStep]">
          {{ status.status || status.currentStep }}
        </span>
        <small>{{ new Date(status.lastUpdate).toLocaleTimeString() }}</small>
      </div>
    </div>

    <!-- Event hub component -->
    <EventHubComponent
      @workflow-step="handleWorkflowStep"
      @action-completed="handleActionCompleted"
      @error-occurred="handleErrorOccurred"
      @state-changed="handleStateChanged"
      @notification="handleNotification"
      @data-request="handleDataRequest"
      @data-response="handleDataResponse"
    />

    <!-- Comprehensive event log -->
    <div class="event-log">
      <h3>Complete Event Log:</h3>
      <button @click="clearEventLog">Clear All</button>
      <div class="log-entries">
        <div
          v-for="entry in eventLog"
          :key="entry.time + entry.type + entry.source"
          class="log-entry"
          :class="[entry.type, entry.source]"
        >
          <span class="time">{{ entry.time }}</span>
          <span class="source">[{{ entry.source }}]</span>
          <span class="type">{{ entry.type }}</span>
          <span class="data">{{ JSON.stringify(entry.data) }}</span>
        </div>
      </div>
    </div>

    <!-- Data cache -->
    <div class="data-cache" v-if="Object.keys(dataCache).length > 0">
      <h3>Data Cache:</h3>
      <div class="cache-entries">
        <div
          v-for="(data, requestId) in dataCache"
          :key="requestId"
          class="cache-entry"
        >
          <strong>Request {{ requestId }}:</strong>
          <pre>{{ JSON.stringify(data, null, 2) }}</pre>
        </div>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Complex workflows** - support untuk multi-step processes
- ✅ **Event orchestration** - coordinate berbagai events
- ✅ **Error handling** - comprehensive error management
- ✅ **Data flow management** - track dan cache data flows

## 🎯 Best Practices untuk Component Events

### 1. **Always Declare Events**
```vue
<script setup>
// ✅ GOOD: Selalu deklarasikan events
const emit = defineEmits(['click', 'submit', 'update'])

// ✅ Dengan validasi
const emit = defineEmits({
  submit: (payload) => validatePayload(payload)
})

// ❌ AVOID: Emit tanpa deklarasi
const emit = defineEmits() // Kosong
</script>
```

### 2. **Use Consistent Event Names**
```vue
<script setup>
// ✅ GOOD: Kebab-case yang konsisten
const emit = defineEmits([
  'user-clicked',
  'form-submitted',
  'data-updated'
])

// ❌ AVOID: Tidak konsisten
const emit = defineEmits([
  'userClicked',
  'form-submitted',
  'dataUpdated'
])
</script>
```

### 3. **Provide Structured Payloads**
```vue
<script setup>
// ✅ GOOD: Payload yang terstruktur
const emit = defineEmits(['user-update'])

const updateUser = (user) => {
  emit('user-update', {
    id: user.id,
    changes: { name: user.name },
    timestamp: Date.now()
  })
}

// ❌ AVOID: Payload tidak terstruktur
const updateUser = (user) => {
  emit('user-update', user) // Terlalu banyak data
}
</script>
```

## 🎉 Kesimpulan

Component Events adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Two-way communication** - child dapat berkomunikasi dengan parent
- ✅ **Rich interactions** - menangani berbagai user interactions
- ✅ **Form management** - comprehensive form handling dengan events
- ✅ **State synchronization** - real-time state synchronization
- ✅ **Advanced patterns** - complex event orchestration

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Component Events
- Vue.js Event Handling Guide
- Vue.js Best Practices

---

**🔥 Tips Pro:**
- Selalu **deklarasikan events** untuk maintainability
- Gunakan **structured payloads** untuk data yang jelas
- Implement **event validation** untuk data integrity
- Manfaatkan **event modifiers** untuk better control
- Gunakan **composition patterns** untuk reusable event logic

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation