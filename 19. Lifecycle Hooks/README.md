# 🔄 Lifecycle Hooks - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami lifecycle hooks Vue.js untuk mengontrol siklus hidup komponen dari creation hingga destruction dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Lifecycle Hooks?](#-apa-itu-lifecycle-hooks)
- [🎮 Lifecycle Diagram](#-lifecycle-diagram)
- [⚡ Vue 3 Lifecycle Hooks](#-vue-3-lifecycle-hooks)
- [🏗️ Creation Hooks](#️-creation-hooks)
- [🔄 Mounting Hooks](#-mounting-hooks)
- [🔄 Updating Hooks](#-updating-hooks)
- [💀 Destruction Hooks](#-destruction-hooks)
- [🔧 Advanced Usage](#-advanced-usage)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Lifecycle Hooks?

**Lifecycle Hooks** adalah metode khusus yang memungkinkan kita untuk menjalankan kode pada tahapan tertentu dalam lifecycle komponen Vue.js. Setiap komponen melalui serangkaian tahapan dari creation hingga destruction.

### Konsep Dasar Lifecycle

```
1. Creation Phase   → Komponen dibuat
2. Mounting Phase   → Komponen dimasukkan ke DOM
3. Updating Phase   → Komponen diperbarui saat data berubah
4. Destruction Phase → Komponen dihapus dari DOM
```

### Mengapa Lifecycle Hooks Penting?

1. **⚡ Initialization:** Setup awal komponen
2. **🌐 API Calls:** Mengambil data dari server
3. **🎯 DOM Manipulation:** Mengakses dan mengubah DOM
4. **🔄 Event Listeners:** Setup dan cleanup event listeners
5. **💾 Resource Management:** Mengelola memory dan resources

## 🎮 Lifecycle Diagram

### Vue 3 Lifecycle Flow

```
Creation Phase:
┌─────────────────┐
│   setup()       │ ← Component setup
└─────────────────┘
          ↓
┌─────────────────┐
│ onBeforeMount() │ ← Sebelum mount
└─────────────────┘
          ↓
┌─────────────────┐
│   onMounted()   │ ← Setelah mount
└─────────────────┘

Updating Phase:
┌─────────────────┐
│ onBeforeUpdate()│ ← Sebelum update
└─────────────────┘
          ↓
┌─────────────────┐
│   onUpdated()   │ ← Setelah update
└─────────────────┘

Destruction Phase:
┌─────────────────┐
│onBeforeUnmount()│ ← Sebelum unmount
└─────────────────┘
          ↓
┌─────────────────┐
│  onUnmounted()  │ ← Setelah unmount
└─────────────────┘
```

## ⚡ Vue 3 Lifecycle Hooks

### List of All Hooks

```vue
<script setup>
import {
  onBeforeMount,
  onMounted,
  onBeforeUpdate,
  onUpdated,
  onBeforeUnmount,
  onUnmounted,
  onErrorCaptured,
  onRenderTracked,
  onRenderTriggered
} from 'vue'
</script>
```

### Hook Overview

| Hook | Phase | Description |
|------|-------|-------------|
| `onBeforeMount` | Mounting | Sebelum komponen dimount ke DOM |
| `onMounted` | Mounting | Setelah komponen dimount ke DOM |
| `onBeforeUpdate` | Updating | Sebelum DOM diperbarui |
| `onUpdated` | Updating | Setelah DOM diperbarui |
| `onBeforeUnmount` | Destruction | Sebelum komponen dihapus dari DOM |
| `onUnmounted` | Destruction | Setelah komponen dihapus dari DOM |
| `onErrorCaptured` | Error | Menangkap error dari child components |
| `onRenderTracked` | Debug | Tracking reactive dependencies |
| `onRenderTriggered` | Debug | Tracking render triggers |

## 🏗️ Creation Hooks

### onBeforeMount

```vue
<script setup>
import { ref, onBeforeMount } from 'vue'

const data = ref(null)
const loading = ref(true)

onBeforeMount(() => {
  console.log('Component is about to be mounted')
  console.log('DOM is not available yet')

  // Tidak bisa akses DOM elements di sini
  // const el = document.querySelector('.my-element') // null

  // Bisa setup data awal
  console.log('Setting up initial data...')
})
</script>

<template>
  <div class="my-element">
    <h1>Component Content</h1>
    <p v-if="loading">Loading...</p>
    <p v-else>{{ data }}</p>
  </div>
</template>
```

### onMounted

```vue
<script setup>
import { ref, onMounted } from 'vue'

const users = ref([])
const message = ref('Hello World!')
const isComponentMounted = ref(true)

onMounted(() => {
  console.log('Component has been mounted')
  console.log('DOM is now available')

  // ✅ Bisa akses DOM elements
  const title = document.querySelector('h1')
  if (title) {
    title.style.color = '#3b82f6'
  }

  // ✅ Bisa fetch data dari API
  fetchUsers()

  // ✅ Bisa setup event listeners
  window.addEventListener('resize', handleResize)

  // ✅ Bisa start timers atau intervals
  startTimer()
})

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    const data = await response.json()
    users.value = data
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const handleResize = () => {
  console.log('Window resized')
}

const startTimer = () => {
  setInterval(() => {
    console.log('Timer tick')
  }, 1000)
}
</script>

<template>
  <div v-if="isComponentMounted">
    <h1>{{ message }}</h1>

    <div class="users-section">
      <h2>Users List</h2>
      <div v-if="users.length === 0">Loading users...</div>
      <ul v-else>
        <li v-for="user in users" :key="user.id">
          {{ user.name }} - {{ user.email }}
        </li>
      </ul>
    </div>

    <div class="actions">
      <button @click="updateMessage">Update Message</button>
      <button @click="unmountComponent">Unmount Component</button>
    </div>
  </div>
  <div v-else>
    <p>Component has been unmounted</p>
  </div>
</template>

<style scoped>
h1 {
  transition: color 0.3s;
}

.users-section {
  margin: 20px 0;
  padding: 15px;
  background: #f8fafc;
  border-radius: 8px;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  padding: 8px;
  margin: 4px 0;
  background: white;
  border-radius: 4px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.actions {
  margin-top: 20px;
  display: flex;
  gap: 10px;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  background: #3b82f6;
  color: white;
}

button:hover {
  background: #2563eb;
}
</style>
```

## 🔄 Updating Hooks

### onBeforeUpdate

```vue
<script setup>
import { ref, onBeforeUpdate } from 'vue'

const count = ref(0)
const previousCount = ref(0)
const updateHistory = ref([])

onBeforeUpdate(() => {
  console.log('Component is about to update')
  console.log('Current count:', count.value)

  // Simpan sebelumnya
  previousCount.value = count.value

  // Log perubahan
  updateHistory.value.push({
    from: previousCount.value,
    to: count.value,
    timestamp: new Date().toLocaleTimeString()
  })

  // Bisa akses DOM sebelum update
  const element = document.querySelector('.count-display')
  if (element) {
    console.log('DOM element before update:', element.textContent)
  }
})

const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}

const reset = () => {
  count.value = 0
  updateHistory.value = []
}
</script>

<template>
  <div class="counter-demo">
    <h2>Counter Demo</h2>

    <div class="count-display">
      Current Count: <strong>{{ count }}</strong>
    </div>

    <div class="previous-count">
      Previous Count: {{ previousCount }}
    </div>

    <div class="controls">
      <button @click="decrement">-</button>
      <button @click="increment">+</button>
      <button @click="reset">Reset</button>
    </div>

    <div class="update-history">
      <h3>Update History</h3>
      <div v-if="updateHistory.length === 0" class="no-history">
        No updates yet
      </div>
      <div v-else>
        <div
          v-for="(update, index) in updateHistory"
          :key="index"
          class="history-item"
        >
          {{ update.timestamp }}: {{ update.from }} → {{ update.to }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.counter-demo {
  padding: 20px;
  max-width: 400px;
  margin: 0 auto;
  border: 2px solid #e2e8f0;
  border-radius: 8px;
}

.count-display {
  font-size: 24px;
  text-align: center;
  margin: 20px 0;
  padding: 15px;
  background: #dbeafe;
  border-radius: 8px;
}

.previous-count {
  text-align: center;
  color: #64748b;
  margin-bottom: 15px;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.controls button {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  font-weight: bold;
}

.update-history {
  border-top: 1px solid #e2e8f0;
  padding-top: 20px;
}

.history-item {
  padding: 5px 10px;
  margin: 2px 0;
  background: #f1f5f9;
  border-radius: 4px;
  font-family: monospace;
  font-size: 14px;
}

.no-history {
  text-align: center;
  color: #94a3b8;
  font-style: italic;
}
</style>
```

### onUpdated

```vue
<script setup>
import { ref, onUpdated } from 'vue'

const items = ref([
  { id: 1, text: 'Item 1', color: '#3b82f6' },
  { id: 2, text: 'Item 2', color: '#10b981' },
  { id: 3, text: 'Item 3', color: '#f59e0b' }
])

const updateCount = ref(0)

onUpdated(() => {
  console.log('Component has been updated')
  updateCount.value++

  // ✅ DOM sudah ter-update di sini
  const elements = document.querySelectorAll('.list-item')
  console.log(`Found ${elements.length} updated elements`)

  // Bakukan animasi atau manipulasi DOM
  elements.forEach((el, index) => {
    el.style.animation = `slideIn 0.3s ease-out ${index * 0.1}s`
  })
})

const addItem = () => {
  const newId = Math.max(...items.value.map(item => item.id)) + 1
  const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6']
  const randomColor = colors[Math.floor(Math.random() * colors.length)]

  items.value.push({
    id: newId,
    text: `Item ${newId}`,
    color: randomColor
  })
}

const removeItem = (id) => {
  const index = items.value.findIndex(item => item.id === id)
  if (index > -1) {
    items.value.splice(index, 1)
  }
}

const shuffleItems = () => {
  items.value = [...items.value].sort(() => Math.random() - 0.5)
}
</script>

<template>
  <div class="list-demo">
    <h2>Dynamic List Demo</h2>

    <div class="stats">
      <p>Total Items: {{ items.length }}</p>
      <p>Update Count: {{ updateCount }}</p>
    </div>

    <div class="controls">
      <button @click="addItem">Add Item</button>
      <button @click="shuffleItems">Shuffle</button>
    </div>

    <div class="list-container">
      <div
        v-for="item in items"
        :key="item.id"
        class="list-item"
        :style="{ borderColor: item.color }"
      >
        <span class="item-text">{{ item.text }}</span>
        <button
          class="remove-btn"
          @click="removeItem(item.id)"
        >
          ×
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.list-demo {
  padding: 20px;
  max-width: 500px;
  margin: 0 auto;
}

.stats {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
  padding: 10px;
  background: #f8fafc;
  border-radius: 8px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  justify-content: center;
}

.list-container {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border: 2px solid;
  border-radius: 8px;
  background: white;
  transition: all 0.3s ease;
}

.item-text {
  font-weight: 500;
}

.remove-btn {
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.remove-btn:hover {
  background: #dc2626;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
```

## 💀 Destruction Hooks

### onBeforeUnmount

```vue
<script setup>
import { ref, onBeforeUnmount } from 'vue'

const timerId = ref(null)
const eventListeners = ref([])
const messages = ref([])

onBeforeUnmount(() => {
  console.log('Component is about to be unmounted')

  // Cleanup timers
  if (timerId.value) {
    clearInterval(timerId.value)
    console.log('Timer cleaned up')
  }

  // Cleanup event listeners
  eventListeners.value.forEach(({ element, event, handler }) => {
    element.removeEventListener(event, handler)
  })
  console.log('Event listeners cleaned up')

  // Cleanup resources
  messages.value = []
  console.log('Component data cleaned up')
})

const addLogMessage = (message) => {
  messages.value.push({
    text: message,
    timestamp: new Date().toLocaleTimeString()
  })
}

const startTimer = () => {
  if (timerId.value) return

  timerId.value = setInterval(() => {
    addLogMessage('Timer tick')
  }, 2000)

  addLogMessage('Timer started')
}

const stopTimer = () => {
  if (timerId.value) {
    clearInterval(timerId.value)
    timerId.value = null
    addLogMessage('Timer stopped')
  }
}

const addMouseTracker = () => {
  const handler = (e) => {
    addLogMessage(`Mouse: (${e.clientX}, ${e.clientY})`)
  }

  document.addEventListener('mousemove', handler)
  eventListeners.value.push({
    element: document,
    event: 'mousemove',
    handler
  })

  addLogMessage('Mouse tracker added')
}

const removeMouseTracker = () => {
  const index = eventListeners.value.findIndex(
    listener => listener.event === 'mousemove'
  )

  if (index > -1) {
    const { element, event, handler } = eventListeners.value[index]
    element.removeEventListener(event, handler)
    eventListeners.value.splice(index, 1)
    addLogMessage('Mouse tracker removed')
  }
}
</script>

<template>
  <div class="cleanup-demo">
    <h2>Component Cleanup Demo</h2>

    <div class="controls">
      <button @click="startTimer">Start Timer</button>
      <button @click="stopTimer">Stop Timer</button>
      <button @click="addMouseTracker">Add Mouse Tracker</button>
      <button @click="removeMouseTracker">Remove Mouse Tracker</button>
    </div>

    <div class="log-container">
      <h3>Activity Log</h3>
      <div class="log-messages">
        <div
          v-for="(message, index) in messages"
          :key="index"
          class="log-message"
        >
          <span class="timestamp">{{ message.timestamp }}</span>
          <span class="text">{{ message.text }}</span>
        </div>
      </div>
    </div>

    <div class="note">
      <p><strong>Note:</strong> Check console when unmounting component to see cleanup in action!</p>
    </div>
  </div>
</template>

<style scoped>
.cleanup-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
}

.controls {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}

.log-container {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  overflow: hidden;
}

.log-container h3 {
  margin: 0;
  padding: 15px;
  background: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.log-messages {
  max-height: 300px;
  overflow-y: auto;
  padding: 10px;
}

.log-message {
  display: flex;
  gap: 10px;
  padding: 5px 0;
  border-bottom: 1px solid #f1f5f9;
}

.timestamp {
  font-family: monospace;
  color: #64748b;
  font-size: 12px;
  min-width: 80px;
}

.text {
  color: #334155;
}

.note {
  margin-top: 20px;
  padding: 15px;
  background: #fef3c7;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.note p {
  margin: 0;
  color: #92400e;
}
</style>
```

### onUnmounted

```vue
<script setup>
import { ref, onUnmounted } from 'vue'

const unmountTime = ref(null)
const cleanupActions = ref([])

onUnmounted(() => {
  console.log('Component has been unmounted')

  unmountTime.value = new Date().toLocaleTimeString()
  cleanupActions.value.push('Component unmounted')

  // Final cleanup actions
  console.log('All cleanup actions completed')
  console.log('Component fully destroyed')
})

const addCleanupLog = (action) => {
  cleanupActions.value.push({
    action,
    timestamp: new Date().toLocaleTimeString()
  })
}
</script>

<template>
  <div class="unmount-demo">
    <h2>Unmount Lifecycle Demo</h2>

    <div class="status">
      <p>Component Status: <span class="active">Active</span></p>
      <p>Current Time: {{ new Date().toLocaleTimeString() }}</p>
    </div>

    <div class="actions">
      <button @click="addCleanupLog('Manual action logged')">
        Log Manual Action
      </button>
    </div>

    <div class="cleanup-log">
      <h3>Cleanup Actions Log</h3>
      <div v-if="cleanupActions.length === 0" class="empty">
        No cleanup actions yet
      </div>
      <div v-else>
        <div
          v-for="(item, index) in cleanupActions"
          :key="index"
          class="log-item"
        >
          <span class="time">{{ item.timestamp || item }}</span>
          <span class="action">{{ item.action || item }}</span>
        </div>
      </div>
    </div>

    <div v-if="unmountTime" class="unmount-info">
      <h3>Component Unmounted!</h3>
      <p>Unmount Time: {{ unmountTime }}</p>
    </div>
  </div>
</template>

<style scoped>
.unmount-demo {
  padding: 20px;
  border: 2px solid #10b981;
  border-radius: 8px;
  max-width: 500px;
  margin: 0 auto;
}

.status {
  background: #dcfce7;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.active {
  color: #16a34a;
  font-weight: bold;
}

.actions {
  margin-bottom: 20px;
  text-align: center;
}

.cleanup-log {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 15px;
}

.log-item {
  display: flex;
  justify-content: space-between;
  padding: 5px 0;
  border-bottom: 1px solid #f1f5f9;
}

.time {
  font-family: monospace;
  color: #64748b;
  font-size: 12px;
}

.action {
  color: #334155;
}

.empty {
  text-align: center;
  color: #94a3b8;
  font-style: italic;
}

.unmount-info {
  margin-top: 20px;
  padding: 15px;
  background: #fee2e2;
  border-radius: 8px;
  border-left: 4px solid #ef4444;
}

.unmount-info h3 {
  margin: 0 0 10px 0;
  color: #dc2626;
}
</style>
```

## 🔧 Advanced Usage

### Error Handling with onErrorCaptured

```vue
<script setup>
import { ref, onErrorCaptured } from 'vue'

const errors = ref([])
const showChild = ref(true)

onErrorCaptured((error, instance, info) => {
  console.error('Error captured:', error)
  console.error('Component instance:', instance)
  console.error('Error info:', info)

  errors.value.push({
    message: error.message,
    info,
    timestamp: new Date().toLocaleTimeString()
  })

  // Return false untuk mencegah error propagate
  return false
})

const clearErrors = () => {
  errors.value = []
}

const toggleChild = () => {
  showChild.value = !showChild.value
}
</script>

<template>
  <div class="error-boundary-demo">
    <h2>Error Boundary Demo</h2>

    <div class="controls">
      <button @click="toggleChild">
        {{ showChild ? 'Hide' : 'Show' }} Child Component
      </button>
      <button @click="clearErrors">Clear Errors</button>
    </div>

    <div v-if="showChild">
      <ErrorChild />
    </div>

    <div v-if="errors.length > 0" class="error-log">
      <h3>Caught Errors</h3>
      <div
        v-for="(error, index) in errors"
        :key="index"
        class="error-item"
      >
        <p><strong>Error:</strong> {{ error.message }}</p>
        <p><strong>Info:</strong> {{ error.info }}</p>
        <p><strong>Time:</strong> {{ error.timestamp }}</p>
      </div>
    </div>
  </div>
</template>

<script>
// Error Child Component yang sengaja dibuat error
const ErrorChild = {
  template: `
    <div class="error-child">
      <h3>Error Child Component</h3>
      <button @click="causeError">Cause Error</button>
      <button @click="causeTypeError">Cause Type Error</button>
    </div>
  `,
  methods: {
    causeError() {
      // Error yang disengaja
      this.nonExistentMethod()
    },
    causeTypeError() {
      // Type error
      const obj = null
      console.log(obj.someProperty)
    }
  }
}
</script>

<style scoped>
.error-boundary-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.error-child {
  padding: 15px;
  background: #f8fafc;
  border-radius: 8px;
  margin-bottom: 20px;
}

.error-child button {
  margin-right: 10px;
  padding: 5px 10px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.error-log {
  border: 1px solid #ef4444;
  border-radius: 8px;
  padding: 15px;
  background: #fef2f2;
}

.error-item {
  margin-bottom: 10px;
  padding: 10px;
  background: white;
  border-radius: 4px;
  border-left: 4px solid #ef4444;
}

.error-item p {
  margin: 5px 0;
  font-size: 14px;
}
</style>
```

### Debug Hooks (Development Only)

```vue
<script setup>
import { ref, onRenderTracked, onRenderTriggered } from 'vue'

const debugInfo = ref([])
const count = ref(0)

onRenderTracked((e) => {
  if (process.env.NODE_ENV === 'development') {
    debugInfo.value.push({
      type: 'TRACKED',
      key: e.key,
      target: e.target,
      timestamp: new Date().toLocaleTimeString()
    })
  }
})

onRenderTriggered((e) => {
  if (process.env.NODE_ENV === 'development') {
    debugInfo.value.push({
      type: 'TRIGGERED',
      key: e.key,
      oldValue: e.oldValue,
      newValue: e.newValue,
      timestamp: new Date().toLocaleTimeString()
    })
  }
})

const increment = () => {
  count.value++
}

const clearDebug = () => {
  debugInfo.value = []
}
</script>

<template>
  <div class="debug-demo">
    <h2>Debug Hooks Demo</h2>

    <div class="counter">
      <p>Count: {{ count }}</p>
      <button @click="increment">Increment</button>
    </div>

    <div class="debug-controls">
      <button @click="clearDebug">Clear Debug Info</button>
    </div>

    <div class="debug-info">
      <h3>Render Debug Info (Development Only)</h3>
      <div v-if="debugInfo.length === 0" class="empty">
        No debug information yet. Try incrementing the counter.
      </div>
      <div v-else>
        <div
          v-for="(info, index) in debugInfo"
          :key="index"
          class="debug-item"
          :class="info.type.toLowerCase()"
        >
          <span class="type">{{ info.type }}</span>
          <span class="time">{{ info.timestamp }}</span>
          <span class="key">{{ info.key }}</span>
          <span v-if="info.oldValue !== undefined" class="change">
            {{ info.oldValue }} → {{ info.newValue }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.debug-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
}

.counter {
  text-align: center;
  margin-bottom: 20px;
  padding: 15px;
  background: #dbeafe;
  border-radius: 8px;
}

.debug-controls {
  text-align: center;
  margin-bottom: 20px;
}

.debug-info {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  overflow: hidden;
}

.debug-info h3 {
  margin: 0;
  padding: 15px;
  background: #f8fafc;
  border-bottom: 1px solid #e2e8f0;
}

.debug-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 15px;
  border-bottom: 1px solid #f1f5f9;
  font-family: monospace;
  font-size: 12px;
}

.debug-item:last-child {
  border-bottom: none;
}

.tracked {
  background: #f0fdf4;
}

.triggered {
  background: #fef3c7;
}

.type {
  font-weight: bold;
  min-width: 80px;
}

.time {
  color: #64748b;
  min-width: 80px;
}

.key {
  color: #3b82f6;
  min-width: 100px;
}

.change {
  color: #f59e0b;
  font-weight: bold;
}

.empty {
  padding: 20px;
  text-align: center;
  color: #94a3b8;
  font-style: italic;
}
</style>
```

## 🚀 Best Practices

### 1. Use Hooks Appropriately

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const data = ref(null)

// ✅ Gunakan onMounted untuk API calls
onMounted(async () => {
  try {
    const response = await fetch('/api/data')
    data.value = await response.json()
  } catch (error) {
    console.error('Failed to fetch data:', error)
  }
})

// ✅ Gunakan onUnmounted untuk cleanup
onUnmounted(() => {
  // Cleanup resources
  data.value = null
})
</script>
```

### 2. Avoid Heavy Operations in onBeforeMount

```vue
<script setup>
import { ref, onBeforeMount, onMounted } from 'vue'

const data = ref(null)

// ❌ Jangan lakukan heavy operations di onBeforeMount
onBeforeMount(() => {
  // Don't do this!
  // const response = await fetch('/api/data') // Bad practice
})

// ✅ Lakukan di onMounted
onMounted(async () => {
  const response = await fetch('/api/data')
  data.value = await response.json()
})
</script>
```

### 3. Cleanup Resources Properly

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const timerId = ref(null)
const eventListenerAttached = ref(false)

onMounted(() => {
  // Start timer
  timerId.value = setInterval(() => {
    console.log('Timer tick')
  }, 1000)

  // Add event listener
  window.addEventListener('resize', handleResize)
  eventListenerAttached.value = true
})

onUnmounted(() => {
  // Cleanup timer
  if (timerId.value) {
    clearInterval(timerId.value)
  }

  // Cleanup event listener
  if (eventListenerAttached.value) {
    window.removeEventListener('resize', handleResize)
  }
})

const handleResize = () => {
  console.log('Window resized')
}
</script>
```

### 4. Don't Access DOM Before onMounted

```vue
<script setup>
import { onBeforeMount, onMounted } from 'vue'

// ❌ Jangan akses DOM di onBeforeMount
onBeforeMount(() => {
  const element = document.querySelector('.my-element') // null!
})

// ✅ Akses DOM di onMounted
onMounted(() => {
  const element = document.querySelector('.my-element') // Works!
  if (element) {
    element.style.color = 'red'
  }
})
</script>
```

## 💡 Tips dan Trik

### 1. Multiple Hooks of Same Type

```vue
<script setup>
import { onMounted } from 'vue'

// ✅ Bisa memiliki multiple onMounted hooks
onMounted(() => {
  console.log('First mount handler')
  setupComponent()
})

onMounted(() => {
  console.log('Second mount handler')
  startAnalytics()
})

onMounted(() => {
  console.log('Third mount handler')
  initializePlugins()
})
</script>
```

### 2. Cleanup Function Pattern

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  const handleResize = () => console.log('Resized')
  const timer = setInterval(() => console.log('Tick'), 1000)

  // Return cleanup function
  return () => {
    window.removeEventListener('resize', handleResize)
    clearInterval(timer)
  }
})
</script>
```

### 3. Conditional Lifecycle Logic

```vue
<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  autoLoad: {
    type: Boolean,
    default: false
  }
})

const data = ref(null)

onMounted(() => {
  if (props.autoLoad) {
    loadData()
  }
})

const loadData = async () => {
  // Load data logic
}
</script>
```

### 4. Performance Monitoring

```vue
<script setup>
import { onMounted, onBeforeUnmount } from 'vue'

onMounted(() => {
  console.time('component-mount')
  console.log('Component mounting started')

  // Component initialization

  console.timeEnd('component-mount')
  console.log('Component mounted successfully')
})

onBeforeUnmount(() => {
  console.log('Component about to unmount')
  console.time('component-unmount')
})

onUnmounted(() => {
  console.timeEnd('component-unmount')
  console.log('Component unmounted successfully')
})
</script>
```

---

## 🎉 Kesimpulan

Lifecycle hooks adalah fitur fundamental dalam Vue.js yang memungkinkan:

1. **⚡ Component Initialization:** Setup awal komponen
2. **🌐 DOM Manipulation:** Akses dan modifikasi DOM
3. **🔄 Data Management:** Fetch data dan manage state
4. **💾 Resource Management:** Setup dan cleanup resources
5. **🐛 Error Handling:** Menangkap dan handle errors

### Poin Penting:

- `onMounted` untuk DOM access dan API calls
- `onUnmounted` untuk cleanup resources
- `onUpdated` untuk post-update operations
- `onErrorCaptured` untuk error boundaries
- Debug hooks hanya untuk development

### Lifecycle Flow Summary:

```
setup() → onBeforeMount() → onMounted() →
[onBeforeUpdate() → onUpdated()] →
onBeforeUnmount() → onUnmounted()
```

### Best Practices:

- Gunakan hooks yang tepat untuk tugas yang tepat
- Cleanup resources di onUnmounted
- Jangan akses DOM sebelum onMounted
- Hindari heavy operations di onBeforeMount
- Monitor performance dengan timing logs

Dengan memahami lifecycle hooks, Anda sudah bisa membangun aplikasi Vue.js yang robust dan efisien! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan berbagai lifecycle hooks dalam komponen-komponen Anda untuk melihat kapan dan bagaimana mereka dieksekusi!

## ❓ Pertanyaan: Apa Fungsi Lifecycle Hooks?

Lifecycle Hooks memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk component management:

### 1. **Component Initialization & Setup**
```vue
<script setup>
import { ref, onBeforeMount, onMounted } from 'vue'

// Fungsi 1: Menginisialisasi komponen dan setup resources
const userData = ref(null)
const isLoading = ref(true)
const error = ref(null)
const componentStats = reactive({
  mountTime: null,
  initTime: Date.now(),
  apiCalls: 0,
  domElements: 0
})

// Setup preparation sebelum mount
onBeforeMount(() => {
  console.log('🚀 Component initialization started')
  componentStats.initTime = Date.now()

  // Prepare initial data structure
  userData.value = {
    profile: null,
    preferences: {
      theme: 'light',
      notifications: true,
      language: 'en'
    },
    activity: {
      lastLogin: null,
      sessions: [],
      stats: {}
    }
  }

  // Set initial loading state
  isLoading.value = true
  error.value = null

  console.log('📋 Data structure prepared')
  console.log('⏰ Initialization completed in:', Date.now() - componentStats.initTime, 'ms')
})

// Setup setelah DOM ready
onMounted(async () => {
  console.log('🌐 Component mounted, DOM ready')
  componentStats.mountTime = Date.now()

  try {
    // 1. Setup DOM interactions
    const headerElement = document.querySelector('.component-header')
    if (headerElement) {
      headerElement.style.animation = 'fadeIn 0.5s ease-in'
      componentStats.domElements++
    }

    // 2. Load user data dari API
    await loadUserData()

    // 3. Setup event listeners
    setupEventListeners()

    // 4. Initialize plugins dan libraries
    initializePlugins()

    // 5. Start background processes
    startBackgroundProcesses()

    console.log('✅ Component fully initialized')
    console.log('⏱️ Total mount time:', Date.now() - componentStats.mountTime, 'ms')

  } catch (err) {
    console.error('❌ Initialization failed:', err)
    error.value = err.message
  } finally {
    isLoading.value = false
  }
})

const loadUserData = async () => {
  console.log('📡 Loading user data...')
  componentStats.apiCalls++

  // Simulasi API call dengan realistic delays
  await new Promise(resolve => setTimeout(resolve, 800))

  // Simulasi user data
  userData.value.profile = {
    id: 123,
    name: 'John Doe',
    email: 'john@example.com',
    avatar: 'https://picsum.photos/seed/user123/200/200.jpg',
    joinDate: '2023-01-15',
    status: 'active'
  }

  userData.value.activity.lastLogin = new Date()
  userData.value.activity.sessions = [
    { start: '2024-01-15T09:00:00Z', duration: 120 },
    { start: '2024-01-14T14:30:00Z', duration: 90 }
  ]

  userData.value.activity.stats = {
    totalSessions: 42,
    totalDuration: 3600,
    averageSession: 85,
    lastWeekSessions: 7
  }

  console.log('✅ User data loaded successfully')
}

const setupEventListeners = () => {
  console.log('🔧 Setting up event listeners...')

  // Window resize listener
  const handleResize = () => {
    console.log('📏 Window resized')
    updateLayout()
  }
  window.addEventListener('resize', handleResize)

  // Keyboard shortcuts
  const handleKeydown = (e) => {
    if (e.ctrlKey && e.key === 's') {
      e.preventDefault()
      saveUserData()
    }
  }
  document.addEventListener('keydown', handleKeydown)

  // Before unload listener
  const handleBeforeUnload = (e) => {
    if (hasUnsavedChanges()) {
      e.preventDefault()
      e.returnValue = 'You have unsaved changes'
    }
  }
  window.addEventListener('beforeunload', handleBeforeUnload)

  console.log('✅ Event listeners configured')
}

const initializePlugins = () => {
  console.log('🔌 Initializing plugins...')

  // Initialize analytics
  if (typeof gtag !== 'undefined') {
    gtag('config', 'GA_MEASUREMENT_ID', {
      user_id: userData.value.profile?.id
    })
  }

  // Initialize tooltips
  if (typeof tippy !== 'undefined') {
    tippy('[data-tooltip]', {
      theme: 'light',
      animation: 'scale'
    })
  }

  console.log('✅ Plugins initialized')
}

const startBackgroundProcesses = () => {
  console.log('⏰ Starting background processes...')

  // Auto-save process
  setInterval(() => {
    if (userData.value && hasUnsavedChanges()) {
      saveUserData()
    }
  }, 30000) // Every 30 seconds

  // Activity tracking
  setInterval(() => {
    updateActivityStats()
  }, 60000) // Every minute

  console.log('✅ Background processes started')
}

const updateLayout = () => {
  // Responsive layout adjustments
  const width = window.innerWidth
  console.log('📐 Layout updated for width:', width)
}

const saveUserData = () => {
  console.log('💾 Saving user data...')
  localStorage.setItem('userData', JSON.stringify(userData.value))
}

const hasUnsavedChanges = () => {
  // Check for unsaved changes logic
  return false // Simplified for demo
}

const updateActivityStats = () => {
  console.log('📊 Updating activity stats...')
  // Update activity statistics
}
</script>

<template>
  <div class="user-dashboard">
    <header class="component-header">
      <h1>User Dashboard</h1>
      <div class="stats">
        <span>Mount Time: {{ componentStats.mountTime ? new Date(componentStats.mountTime).toLocaleTimeString() : 'N/A' }}</span>
        <span>API Calls: {{ componentStats.apiCalls }}</span>
        <span>DOM Elements: {{ componentStats.domElements }}</span>
      </div>
    </header>

    <!-- Loading state -->
    <div v-if="isLoading" class="loading-state">
      <div class="spinner"></div>
      <p>Initializing dashboard...</p>
    </div>

    <!-- Error state -->
    <div v-else-if="error" class="error-state">
      <h3>⚠️ Initialization Failed</h3>
      <p>{{ error }}</p>
      <button @click="$router.go(0)">Retry</button>
    </div>

    <!-- Main content -->
    <div v-else-if="userData?.profile" class="dashboard-content">
      <div class="user-profile">
        <img :src="userData.profile.avatar" :alt="userData.profile.name" class="avatar" />
        <div class="user-info">
          <h2>{{ userData.profile.name }}</h2>
          <p>{{ userData.profile.email }}</p>
          <p>Member since: {{ new Date(userData.profile.joinDate).toLocaleDateString() }}</p>
          <span class="status" :class="userData.profile.status">
            {{ userData.profile.status.toUpperCase() }}
          </span>
        </div>
      </div>

      <div class="activity-stats">
        <h3>Activity Statistics</h3>
        <div class="stats-grid">
          <div class="stat-card">
            <span class="number">{{ userData.activity.stats.totalSessions }}</span>
            <span class="label">Total Sessions</span>
          </div>
          <div class="stat-card">
            <span class="number">{{ userData.activity.stats.averageSession }}m</span>
            <span class="label">Avg Session</span>
          </div>
          <div class="stat-card">
            <span class="number">{{ userData.activity.stats.lastWeekSessions }}</span>
            <span class="label">This Week</span>
          </div>
        </div>
      </div>

      <div class="preferences">
        <h3>Preferences</h3>
        <div class="preference-item">
          <label>Theme:</label>
          <span>{{ userData.preferences.theme }}</span>
        </div>
        <div class="preference-item">
          <label>Notifications:</label>
          <span>{{ userData.preferences.notifications ? 'Enabled' : 'Disabled' }}</span>
        </div>
        <div class="preference-item">
          <label>Language:</label>
          <span>{{ userData.preferences.language.toUpperCase() }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.user-dashboard {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.component-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.stats {
  display: flex;
  gap: 20px;
  font-size: 14px;
}

.loading-state {
  text-align: center;
  padding: 40px;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 20px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-20px); }
  to { opacity: 1; transform: translateY(0); }
}

.user-profile {
  display: flex;
  align-items: center;
  gap: 20px;
  padding: 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  margin-bottom: 30px;
}

.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
}

.user-info h2 {
  margin: 0 0 10px 0;
  color: #333;
}

.user-info p {
  margin: 5px 0;
  color: #666;
}

.status {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
}

.status.active {
  background: #d4edda;
  color: #155724;
}

.activity-stats {
  margin-bottom: 30px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 20px;
  margin-top: 15px;
}

.stat-card {
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  text-align: center;
}

.stat-card .number {
  display: block;
  font-size: 24px;
  font-weight: bold;
  color: #3498db;
  margin-bottom: 5px;
}

.stat-card .label {
  font-size: 14px;
  color: #666;
}

.preferences {
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
}

.preference-item {
  display: flex;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #dee2e6;
}

.preference-item:last-child {
  border-bottom: none;
}

.preference-item label {
  font-weight: 500;
  color: #495057;
}
</style>
```

**Keuntungan:**
- ✅ **Controlled initialization** - setup yang terstruktur dan bertahap
- ✅ **Resource management** - pengelolaan resources yang optimal
- ✅ **Error handling** - penanganan error yang graceful
- ✅ **Performance tracking** - monitoring initialization performance

### 2. **DOM Manipulation & Side Effects**
```vue
<script setup>
import { ref, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount } from 'vue'

// Fungsi 2: Manipulasi DOM dan manage side effects
const canvas = ref(null)
const chartData = ref([])
const animationFrameId = ref(null)
const resizeObserver = ref(null)
const intersectionObserver = ref(null)
const scrollPosition = ref(0)
const windowSize = ref({ width: 0, height: 0 })
const isElementVisible = ref(false)

// Chart animation state
const chartState = reactive({
  isAnimating: false,
  currentFrame: 0,
  totalFrames: 60,
  animationDuration: 2000,
  startTime: null
})

// Initialize canvas dan chart
onMounted(() => {
  console.log('🎨 Initializing canvas and animations...')

  // 1. Setup canvas
  if (canvas.value) {
    const ctx = canvas.value.getContext('2d')
    canvas.value.width = canvas.value.offsetWidth
    canvas.value.height = canvas.value.offsetHeight

    // 2. Initialize chart data
    initializeChartData()

    // 3. Start chart animation
    startChartAnimation()

    // 4. Setup resize observer
    setupResizeObserver()

    // 5. Setup intersection observer
    setupIntersectionObserver()

    // 6. Add window scroll listener
    window.addEventListener('scroll', handleScroll)

    // 7. Add window resize listener
    window.addEventListener('resize', handleResize)
    handleResize() // Initial call

    console.log('✅ Canvas and animations initialized')
  }
})

// Handle DOM updates
onBeforeUpdate(() => {
  console.log('🔄 About to update DOM...')

  // Pause animations during update
  if (chartState.isAnimating) {
    pauseAnimation()
  }
})

onUpdated(() => {
  console.log('✅ DOM updated, resuming operations...')

  // Resume animations after update
  if (chartState.isAnimating) {
    resumeAnimation()
  }

  // Redraw chart if needed
  if (canvas.value && chartData.value.length > 0) {
    redrawChart()
  }
})

// Cleanup resources
onBeforeUnmount(() => {
  console.log('🧹 Cleaning up DOM and side effects...')

  // 1. Cancel animation frame
  if (animationFrameId.value) {
    cancelAnimationFrame(animationFrameId.value)
    console.log('✅ Animation frame cancelled')
  }

  // 2. Disconnect resize observer
  if (resizeObserver.value) {
    resizeObserver.value.disconnect()
    console.log('✅ Resize observer disconnected')
  }

  // 3. Disconnect intersection observer
  if (intersectionObserver.value) {
    intersectionObserver.value.disconnect()
    console.log('✅ Intersection observer disconnected')
  }

  // 4. Remove event listeners
  window.removeEventListener('scroll', handleScroll)
  window.removeEventListener('resize', handleResize)
  console.log('✅ Event listeners removed')

  // 5. Clear canvas
  if (canvas.value) {
    const ctx = canvas.value.getContext('2d')
    ctx.clearRect(0, 0, canvas.value.width, canvas.value.height)
    console.log('✅ Canvas cleared')
  }
})

const initializeChartData = () => {
  // Generate random chart data
  const dataPoints = 12
  const data = []

  for (let i = 0; i < dataPoints; i++) {
    data.push({
      label: `Month ${i + 1}`,
      value: Math.floor(Math.random() * 100) + 20,
      color: `hsl(${i * 30}, 70%, 50%)`
    })
  }

  chartData.value = data
  console.log('📊 Chart data initialized:', data.length, 'points')
}

const startChartAnimation = () => {
  chartState.isAnimating = true
  chartState.currentFrame = 0
  chartState.startTime = performance.now()

  animateChart()
}

const animateChart = () => {
  const currentTime = performance.now()
  const elapsed = currentTime - chartState.startTime
  const progress = Math.min(elapsed / chartState.animationDuration, 1)

  // Calculate current frame
  chartState.currentFrame = Math.floor(progress * chartState.totalFrames)

  // Draw chart
  drawChart(progress)

  // Continue animation if not complete
  if (progress < 1) {
    animationFrameId.value = requestAnimationFrame(animateChart)
  } else {
    chartState.isAnimating = false
    console.log('✅ Chart animation completed')
  }
}

const drawChart = (progress = 1) => {
  if (!canvas.value) return

  const ctx = canvas.value.getContext('2d')
  const width = canvas.value.width
  const height = canvas.value.height

  // Clear canvas
  ctx.clearRect(0, 0, width, height)

  // Draw background
  ctx.fillStyle = '#f8f9fa'
  ctx.fillRect(0, 0, width, height)

  // Calculate dimensions
  const padding = 40
  const chartWidth = width - (padding * 2)
  const chartHeight = height - (padding * 2)
  const barWidth = chartWidth / chartData.value.length

  // Draw bars with animation
  chartData.value.forEach((data, index) => {
    const x = padding + (index * barWidth)
    const barHeight = (data.value / 120) * chartHeight * progress
    const y = height - padding - barHeight

    // Draw bar
    ctx.fillStyle = data.color
    ctx.fillRect(x + barWidth * 0.1, y, barWidth * 0.8, barHeight)

    // Draw value label
    if (progress === 1) {
      ctx.fillStyle = '#333'
      ctx.font = '12px Arial'
      ctx.textAlign = 'center'
      ctx.fillText(data.value, x + barWidth / 2, y - 5)
    }

    // Draw label
    ctx.fillStyle = '#666'
    ctx.font = '10px Arial'
    ctx.textAlign = 'center'
    ctx.fillText(data.label, x + barWidth / 2, height - padding + 20)
  })

  // Draw axes
  ctx.strokeStyle = '#333'
  ctx.lineWidth = 2
  ctx.beginPath()
  ctx.moveTo(padding, padding)
  ctx.lineTo(padding, height - padding)
  ctx.lineTo(width - padding, height - padding)
  ctx.stroke()
}

const redrawChart = () => {
  drawChart(1) // Draw completed chart
}

const pauseAnimation = () => {
  if (animationFrameId.value) {
    cancelAnimationFrame(animationFrameId.value)
    animationFrameId.value = null
  }
}

const resumeAnimation = () => {
  if (chartState.isAnimating && !animationFrameId.value) {
    animateChart()
  }
}

const setupResizeObserver = () => {
  resizeObserver.value = new ResizeObserver((entries) => {
    entries.forEach(entry => {
      const { width, height } = entry.contentRect
      console.log('📏 Canvas resized:', width, 'x', height)

      if (canvas.value) {
        canvas.value.width = width
        canvas.value.height = height
        redrawChart()
      }
    })
  })

  resizeObserver.value.observe(canvas.value)
}

const setupIntersectionObserver = () => {
  intersectionObserver.value = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      isElementVisible.value = entry.isIntersecting

      if (entry.isIntersecting) {
        console.log('👁️ Chart is visible')
        if (!chartState.isAnimating) {
          startChartAnimation()
        }
      } else {
        console.log('🙈 Chart is hidden')
        pauseAnimation()
      }
    })
  }, {
    threshold: 0.1
  })

  intersectionObserver.value.observe(canvas.value)
}

const handleScroll = () => {
  scrollPosition.value = window.pageYOffset
}

const handleResize = () => {
  windowSize.value = {
    width: window.innerWidth,
    height: window.innerHeight
  }
}

const regenerateData = () => {
  initializeChartData()
  startChartAnimation()
}

const toggleAnimation = () => {
  if (chartState.isAnimating) {
    pauseAnimation()
  } else {
    resumeAnimation()
  }
}
</script>

<template>
  <div class="chart-container">
    <header class="chart-header">
      <h2>Interactive Chart with DOM Manipulation</h2>
      <div class="controls">
        <button @click="regenerateData">🔄 Regenerate Data</button>
        <button @click="toggleAnimation">
          {{ chartState.isAnimating ? '⏸️ Pause' : '▶️ Play' }}
        </button>
      </div>
    </header>

    <div class="chart-info">
      <div class="info-item">
        <span>Window Size:</span>
        <span>{{ windowSize.width }} x {{ windowSize.height }}</span>
      </div>
      <div class="info-item">
        <span>Scroll Position:</span>
        <span>{{ scrollPosition }}px</span>
      </div>
      <div class="info-item">
        <span>Chart Visible:</span>
        <span>{{ isElementVisible ? 'Yes' : 'No' }}</span>
      </div>
      <div class="info-item">
        <span>Animation:</span>
        <span>{{ chartState.isAnimating ? 'Running' : 'Stopped' }}</span>
      </div>
    </div>

    <div class="canvas-wrapper">
      <canvas ref="canvas" class="chart-canvas"></canvas>
    </div>

    <div class="chart-legend">
      <h3>Chart Data Points</h3>
      <div class="legend-items">
        <div
          v-for="(item, index) in chartData"
          :key="index"
          class="legend-item"
        >
          <div class="color-box" :style="{ backgroundColor: item.color }"></div>
          <span>{{ item.label }}: {{ item.value }}</span>
        </div>
      </div>
    </div>

    <div class="instructions">
      <h3>DOM Manipulation Features:</h3>
      <ul>
        <li>📏 <strong>Resize Observer:</strong> Chart automatically resizes with window</li>
        <li>👁️ <strong>Intersection Observer:</strong> Animation pauses when hidden</li>
        <li>📜 <strong>Scroll Tracking:</strong> Monitors scroll position</li>
        <li>🎨 <strong>Canvas Animation:</strong> Smooth chart animations with requestAnimationFrame</li>
        <li>🧹 <strong>Resource Cleanup:</strong> Proper cleanup on unmount</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.chart-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.chart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
}

.controls {
  display: flex;
  gap: 10px;
}

.controls button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background: #007bff;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

.controls button:hover {
  background: #0056b3;
}

.chart-info {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
  padding: 15px;
  background: #e9ecef;
  border-radius: 8px;
}

.info-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  background: white;
  border-radius: 4px;
  font-size: 14px;
}

.info-item span:first-child {
  font-weight: 500;
  color: #495057;
}

.info-item span:last-child {
  font-family: monospace;
  color: #007bff;
}

.canvas-wrapper {
  position: relative;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  overflow: hidden;
  margin-bottom: 20px;
}

.chart-canvas {
  display: block;
  width: 100%;
  height: 400px;
}

.chart-legend {
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  margin-bottom: 20px;
}

.legend-items {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
  margin-top: 15px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px;
  background: white;
  border-radius: 4px;
  font-size: 14px;
}

.color-box {
  width: 16px;
  height: 16px;
  border-radius: 2px;
}

.instructions {
  padding: 20px;
  background: #d1ecf1;
  border-left: 4px solid #0c5460;
  border-radius: 0 8px 8px 0;
}

.instructions h3 {
  margin: 0 0 15px 0;
  color: #0c5460;
}

.instructions ul {
  margin: 0;
  padding-left: 20px;
}

.instructions li {
  margin: 8px 0;
  color: #0c5460;
}

.instructions strong {
  color: #0a3d4a;
}
</style>
```

**Keuntungan:**
- ✅ **Controlled DOM manipulation** - manipulasi DOM yang terstruktur
- ✅ **Resource management** - pengelolaan observers dan listeners
- ✅ **Performance optimization** - efficient animations dan rendering
- ✅ **Responsive behavior** - adaptasi terhadap perubahan ukuran dan visibility

### 3. **Data Synchronization & API Integration**
```vue
<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, watch } from 'vue'

// Fungsi 3: Sinkronisasi data dan integrasi API
const apiState = reactive({
  isConnected: false,
  lastSync: null,
  syncInterval: 30000, // 30 seconds
  retryCount: 0,
  maxRetries: 3
})

const syncStatus = ref('idle') // idle, syncing, success, error
const data = ref([])
const originalData = ref([])
const changes = ref([])
const isOnline = ref(navigator.onLine)
const syncLog = ref([])

// WebSocket connection untuk real-time updates
let websocket = null
let syncInterval = null

onMounted(async () => {
  console.log('🔄 Initializing data synchronization...')

  // 1. Check online status
  updateOnlineStatus()

  // 2. Setup online/offline listeners
  setupNetworkListeners()

  // 3. Load initial data
  await loadInitialData()

  // 4. Setup WebSocket connection
  setupWebSocket()

  // 5. Start periodic sync
  startPeriodicSync()

  // 6. Setup beforeunload listener
  setupBeforeUnloadListener()

  console.log('✅ Data synchronization initialized')
})

onBeforeUnmount(() => {
  console.log('🛑 Shutting down data synchronization...')

  // 1. Close WebSocket connection
  if (websocket) {
    websocket.close()
    websocket = null
  }

  // 2. Stop periodic sync
  if (syncInterval) {
    clearInterval(syncInterval)
    syncInterval = null
  }

  // 3. Save pending changes
  if (changes.value.length > 0) {
    savePendingChanges()
  }

  // 4. Remove network listeners
  window.removeEventListener('online', handleOnline)
  window.removeEventListener('offline', handleOffline)

  console.log('✅ Data synchronization shut down')
})

const loadInitialData = async () => {
  console.log('📥 Loading initial data...')
  syncStatus.value = 'syncing'

  try {
    // Try to load from cache first
    const cachedData = loadFromCache()
    if (cachedData) {
      data.value = cachedData
      originalData.value = JSON.parse(JSON.stringify(cachedData))
      console.log('📦 Loaded data from cache')
    }

    // Then fetch from API
    const response = await fetch('/api/data')
    if (response.ok) {
      const apiData = await response.json()
      data.value = apiData
      originalData.value = JSON.parse(JSON.stringify(apiData))
      saveToCache(apiData)

      apiState.isConnected = true
      apiState.lastSync = new Date()
      syncStatus.value = 'success'

      addLogEntry('Initial data loaded from API', 'success')
    } else {
      throw new Error(`API request failed: ${response.status}`)
    }
  } catch (error) {
    console.error('Failed to load initial data:', error)
    syncStatus.value = 'error'
    addLogEntry(`Failed to load data: ${error.message}`, 'error')

    // Retry logic
    if (apiState.retryCount < apiState.maxRetries) {
      apiState.retryCount++
      addLogEntry(`Retrying in 5 seconds... (${apiState.retryCount}/${apiState.maxRetries})`, 'warning')
      setTimeout(loadInitialData, 5000)
    }
  }
}

const setupWebSocket = () => {
  if (!isOnline.value) {
    console.log('📴 Offline - WebSocket connection skipped')
    return
  }

  try {
    websocket = new WebSocket('wss://api.example.com/realtime')

    websocket.onopen = () => {
      console.log('🔌 WebSocket connected')
      apiState.isConnected = true
      addLogEntry('WebSocket connection established', 'success')
    }

    websocket.onmessage = (event) => {
      const message = JSON.parse(event.data)
      handleRealtimeUpdate(message)
    }

    websocket.onclose = () => {
      console.log('🔌 WebSocket disconnected')
      apiState.isConnected = false
      addLogEntry('WebSocket connection lost', 'warning')

      // Try to reconnect after 5 seconds
      setTimeout(setupWebSocket, 5000)
    }

    websocket.onerror = (error) => {
      console.error('WebSocket error:', error)
      addLogEntry('WebSocket connection error', 'error')
    }
  } catch (error) {
    console.error('Failed to setup WebSocket:', error)
    addLogEntry(`WebSocket setup failed: ${error.message}`, 'error')
  }
}

const handleRealtimeUpdate = (message) => {
  console.log('📡 Real-time update received:', message)

  switch (message.type) {
    case 'data_update':
      updateDataItem(message.data)
      break
    case 'data_add':
      addDataItem(message.data)
      break
    case 'data_delete':
      deleteDataItem(message.id)
      break
    default:
      console.warn('Unknown message type:', message.type)
  }

  addLogEntry(`Real-time update: ${message.type}`, 'info')
}

const updateDataItem = (item) => {
  const index = data.value.findIndex(d => d.id === item.id)
  if (index !== -1) {
    data.value[index] = { ...data.value[index], ...item }
    trackChange('update', item)
  }
}

const addDataItem = (item) => {
  data.value.push(item)
  trackChange('add', item)
}

const deleteDataItem = (id) => {
  const index = data.value.findIndex(d => d.id === id)
  if (index !== -1) {
    const deletedItem = data.value[index]
    data.value.splice(index, 1)
    trackChange('delete', deletedItem)
  }
}

const trackChange = (type, item) => {
  changes.value.push({
    type,
    item,
    timestamp: new Date(),
    synced: false
  })
}

const startPeriodicSync = () => {
  syncInterval = setInterval(async () => {
    if (isOnline.value && changes.value.length > 0) {
      await syncChanges()
    }
  }, apiState.syncInterval)

  console.log('⏰ Periodic sync started')
}

const syncChanges = async () => {
  if (syncStatus.value === 'syncing') return

  console.log('🔄 Syncing changes to server...')
  syncStatus.value = 'syncing'

  try {
    const unsyncedChanges = changes.value.filter(c => !c.synced)

    if (unsyncedChanges.length === 0) {
      syncStatus.value = 'success'
      return
    }

    const response = await fetch('/api/sync', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ changes: unsyncedChanges })
    })

    if (response.ok) {
      // Mark changes as synced
      unsyncedChanges.forEach(change => {
        change.synced = true
        change.syncedAt = new Date()
      })

      apiState.lastSync = new Date()
      syncStatus.value = 'success'

      addLogEntry(`Synced ${unsyncedChanges.length} changes`, 'success')
    } else {
      throw new Error(`Sync failed: ${response.status}`)
    }
  } catch (error) {
    console.error('Sync failed:', error)
    syncStatus.value = 'error'
    addLogEntry(`Sync failed: ${error.message}`, 'error')
  }
}

const setupNetworkListeners = () => {
  window.addEventListener('online', handleOnline)
  window.addEventListener('offline', handleOffline)
}

const handleOnline = () => {
  isOnline.value = true
  console.log('🌐 Network connected')
  addLogEntry('Network connection restored', 'success')

  // Restart services
  setupWebSocket()
  if (changes.value.length > 0) {
    syncChanges()
  }
}

const handleOffline = () => {
  isOnline.value = false
  console.log('📴 Network disconnected')
  addLogEntry('Network connection lost', 'warning')

  // Close WebSocket
  if (websocket) {
    websocket.close()
  }
}

const updateOnlineStatus = () => {
  isOnline.value = navigator.onLine
}

const setupBeforeUnloadListener = () => {
  window.addEventListener('beforeunload', (e) => {
    if (changes.value.some(c => !c.synced)) {
      e.preventDefault()
      e.returnValue = 'You have unsaved changes'
    }
  })
}

const loadFromCache = () => {
  try {
    const cached = localStorage.getItem('appData')
    return cached ? JSON.parse(cached) : null
  } catch (error) {
    console.error('Failed to load from cache:', error)
    return null
  }
}

const saveToCache = (dataToSave) => {
  try {
    localStorage.setItem('appData', JSON.stringify(dataToSave))
  } catch (error) {
    console.error('Failed to save to cache:', error)
  }
}

const savePendingChanges = () => {
  try {
    const pendingChanges = changes.value.filter(c => !c.synced)
    if (pendingChanges.length > 0) {
      localStorage.setItem('pendingChanges', JSON.stringify(pendingChanges))
      console.log(`Saved ${pendingChanges.length} pending changes`)
    }
  } catch (error) {
    console.error('Failed to save pending changes:', error)
  }
}

const addLogEntry = (message, type = 'info') => {
  syncLog.value.unshift({
    message,
    type,
    timestamp: new Date()
  })

  // Keep only last 50 entries
  if (syncLog.value.length > 50) {
    syncLog.value = syncLog.value.slice(0, 50)
  }
}

const forceSync = () => {
  syncChanges()
}

const clearChanges = () => {
  changes.value = []
  originalData.value = JSON.parse(JSON.stringify(data.value))
  addLogEntry('Changes cleared', 'info')
}
</script>

<template>
  <div class="sync-dashboard">
    <header class="sync-header">
      <h2>Data Synchronization Dashboard</h2>
      <div class="status-indicators">
        <div class="indicator" :class="{ online: isOnline, offline: !isOnline }">
          {{ isOnline ? '🌐 Online' : '📴 Offline' }}
        </div>
        <div class="indicator" :class="syncStatus">
          {{ syncStatus.toUpperCase() }}
        </div>
        <div class="indicator">
          Changes: {{ changes.filter(c => !c.synced).length }}
        </div>
      </div>
    </header>

    <div class="sync-info">
      <div class="info-grid">
        <div class="info-item">
          <span>API Connected:</span>
          <span>{{ apiState.isConnected ? 'Yes' : 'No' }}</span>
        </div>
        <div class="info-item">
          <span>Last Sync:</span>
          <span>{{ apiState.lastSync ? new Date(apiState.lastSync).toLocaleTimeString() : 'Never' }}</span>
        </div>
        <div class="info-item">
          <span>Sync Interval:</span>
          <span>{{ apiState.syncInterval / 1000 }}s</span>
        </div>
        <div class="info-item">
          <span>Total Data:</span>
          <span>{{ data.length }}</span>
        </div>
      </div>
    </div>

    <div class="controls">
      <button @click="forceSync" :disabled="syncStatus === 'syncing' || !isOnline">
        🔄 Force Sync
      </button>
      <button @click="clearChanges" :disabled="changes.length === 0">
        🗑️ Clear Changes
      </button>
    </div>

    <div class="data-display">
      <h3>Data Items</h3>
      <div class="data-list">
        <div
          v-for="item in data.slice(0, 10)"
          :key="item.id"
          class="data-item"
          :class="{ changed: changes.some(c => c.item.id === item.id && !c.synced) }"
        >
          <span class="item-id">#{{ item.id }}</span>
          <span class="item-name">{{ item.name || 'Unnamed Item' }}</span>
          <span class="item-value">{{ item.value || 'N/A' }}</span>
        </div>
      </div>
      <p v-if="data.length > 10" class="more-items">
        ... and {{ data.length - 10 }} more items
      </p>
    </div>

    <div class="sync-log">
      <h3>Sync Log</h3>
      <div class="log-entries">
        <div
          v-for="entry in syncLog.slice(0, 20)"
          :key="entry.timestamp"
          class="log-entry"
          :class="entry.type"
        >
          <span class="timestamp">{{ new Date(entry.timestamp).toLocaleTimeString() }}</span>
          <span class="message">{{ entry.message }}</span>
        </div>
      </div>
    </div>

    <div class="features">
      <h3>Sync Features Demonstrated:</h3>
      <ul>
        <li>🔄 <strong>Periodic Sync:</strong> Automatic sync every 30 seconds</li>
        <li>🔌 <strong>WebSocket:</strong> Real-time updates</li>
        <li>📦 <strong>Caching:</strong> Local storage fallback</li>
        <li>📡 <strong>Online/Offline:</strong> Network status detection</li>
        <li>🛡️ <strong>Error Handling:</strong> Retry logic and error recovery</li>
        <li>💾 <strong>Pending Changes:</strong> Save unsynced changes</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.sync-dashboard {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.sync-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.status-indicators {
  display: flex;
  gap: 15px;
}

.indicator {
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: bold;
}

.indicator.online {
  background: #d4edda;
  color: #155724;
}

.indicator.offline {
  background: #f8d7da;
  color: #721c24;
}

.indicator.success {
  background: #d4edda;
  color: #155724;
}

.indicator.error {
  background: #f8d7da;
  color: #721c24;
}

.indicator.syncing {
  background: #fff3cd;
  color: #856404;
}

.sync-info {
  margin-bottom: 20px;
  padding: 15px;
  background: #f8f9fa;
  border-radius: 8px;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.info-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 12px;
  background: white;
  border-radius: 4px;
  font-size: 14px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.controls button {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  background: #007bff;
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

.controls button:hover:not(:disabled) {
  background: #0056b3;
}

.controls button:disabled {
  background: #6c757d;
  cursor: not-allowed;
}

.data-display {
  margin-bottom: 20px;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.data-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.data-item {
  display: grid;
  grid-template-columns: 80px 1fr 100px;
  gap: 10px;
  padding: 10px;
  background: #f8f9fa;
  border-radius: 4px;
  font-size: 14px;
  transition: background-color 0.2s;
}

.data-item.changed {
  background: #fff3cd;
  border-left: 4px solid #ffc107;
}

.item-id {
  font-family: monospace;
  color: #6c757d;
}

.item-name {
  font-weight: 500;
}

.item-value {
  text-align: right;
  color: #495057;
}

.more-items {
  text-align: center;
  color: #6c757d;
  font-style: italic;
  margin-top: 10px;
}

.sync-log {
  margin-bottom: 20px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
}

.log-entries {
  max-height: 300px;
  overflow-y: auto;
  margin-top: 10px;
}

.log-entry {
  display: flex;
  gap: 10px;
  padding: 8px;
  margin-bottom: 4px;
  background: white;
  border-radius: 4px;
  font-size: 14px;
}

.log-entry.success {
  border-left: 4px solid #28a745;
}

.log-entry.error {
  border-left: 4px solid #dc3545;
}

.log-entry.warning {
  border-left: 4px solid #ffc107;
}

.log-entry.info {
  border-left: 4px solid #17a2b8;
}

.timestamp {
  font-family: monospace;
  color: #6c757d;
  min-width: 80px;
}

.message {
  flex: 1;
  color: #495057;
}

.features {
  padding: 20px;
  background: #d1ecf1;
  border-left: 4px solid #0c5460;
  border-radius: 0 8px 8px 0;
}

.features h3 {
  margin: 0 0 15px 0;
  color: #0c5460;
}

.features ul {
  margin: 0;
  padding-left: 20px;
}

.features li {
  margin: 8px 0;
  color: #0c5460;
}
</style>
```

**Keuntungan:**
- ✅ **Real-time synchronization** - sinkronisasi data otomatis
- ✅ **Offline support** - fungsi offline dengan caching
- ✅ **Connection management** - manajemen koneksi WebSocket
- ✅ **Change tracking** - tracking perubahan data
- ✅ **Error recovery** - retry logic dan error handling

### 4. **Component Communication & Event Handling**
```vue
<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, defineExpose } from 'vue'

// Fungsi 4: Komunikasi antar komponen dan event handling
const componentId = ref(Math.random().toString(36).substr(2, 9))
const eventHistory = ref([])
const customEvents = ref([])
const globalEventBus = ref(null)
const parentListeners = ref(new Map())
const childComponents = ref(new Map())

// Component state
const state = reactive({
  isVisible: true,
  isActive: false,
  message: '',
  counter: 0,
  lastEvent: null,
  communicationMode: 'parent-child' // parent-child, siblings, global
})

// Component communication state
const communication = reactive({
  sentMessages: [],
  receivedMessages: [],
  activeChannels: [],
  subscriptions: []
})

onMounted(() => {
  console.log(`📡 Component ${componentId.value} - Initializing communication...`)

  // 1. Register with global event bus
  registerWithGlobalBus()

  // 2. Setup event listeners
  setupEventListeners()

  // 3. Initialize communication channels
  initializeChannels()

  // 4. Start periodic events
  startPeriodicEvents()

  // 5. Setup custom event emitters
  setupCustomEvents()

  // 6. Register with parent (if exists)
  registerWithParent()

  console.log(`✅ Component ${componentId.value} communication ready`)
})

onBeforeUnmount(() => {
  console.log(`🛑 Component ${componentId.value} - Cleaning up communication...`)

  // 1. Unregister from global bus
  if (globalEventBus.value) {
    globalEventBus.value.off('component:*', handleGlobalEvent)
  }

  // 2. Remove all parent listeners
  parentListeners.value.forEach((listener, event) => {
    window.removeEventListener(event, listener)
  })

  // 3. Notify other components
  notifyComponentUnloading()

  // 4. Cleanup child components
  cleanupChildComponents()

  // 5. Clear event history
  eventHistory.value = []
  customEvents.value = []

  console.log(`✅ Component ${componentId.value} communication cleaned up`)
})

const registerWithGlobalBus = () => {
  // Try to access global event bus (provided by parent or global)
  globalEventBus.value = inject('globalEventBus', null)

  if (globalEventBus.value) {
    globalEventBus.value.on('component:*', handleGlobalEvent)
    globalEventBus.value.emit('component:registered', {
      id: componentId.value,
      timestamp: new Date(),
      type: 'component'
    })
  }
}

const setupEventListeners = () => {
  // 1. Window events
  const handleVisibilityChange = () => {
    state.isVisible = !document.hidden
    emitEvent('visibility:change', { visible: state.isVisible })
  }

  const handleFocus = () => {
    state.isActive = true
    emitEvent('focus', { active: true })
  }

  const handleBlur = () => {
    state.isActive = false
    emitEvent('blur', { active: false })
  }

  const handleBeforeUnload = (e) => {
    emitEvent('before:unload', {
      hasUnsavedWork: communication.sentMessages.length > 0
    })
  }

  // 2. Custom events
  const handleCustomEvent = (e) => {
    handleIncomingEvent({
      type: 'custom',
      name: e.type,
      data: e.detail,
      source: 'window',
      timestamp: new Date()
    })
  }

  // 3. Add listeners
  window.addEventListener('visibilitychange', handleVisibilityChange)
  window.addEventListener('focus', handleFocus)
  window.addEventListener('blur', handleBlur)
  window.addEventListener('beforeunload', handleBeforeUnload)
  window.addEventListener('app:message', handleCustomEvent)

  // Store listeners for cleanup
  parentListeners.value.set('visibilitychange', handleVisibilityChange)
  parentListeners.value.set('focus', handleFocus)
  parentListeners.value.set('blur', handleBlur)
  parentListeners.value.set('beforeunload', handleBeforeUnload)
  parentListeners.value.set('app:message', handleCustomEvent)

  console.log('🔧 Event listeners configured')
}

const initializeChannels = () => {
  const channels = [
    'state:change',
    'message:send',
    'message:receive',
    'user:action',
    'system:notification',
    'component:interaction'
  ]

  channels.forEach(channel => {
    communication.activeChannels.push({
      name: channel,
      subscribed: true,
      timestamp: new Date()
    })
  })

  console.log('📡 Communication channels initialized:', channels)
}

const startPeriodicEvents = () => {
  // Emit periodic heartbeat
  const heartbeatInterval = setInterval(() => {
    emitEvent('heartbeat', {
      componentId: componentId.value,
      counter: state.counter++,
      timestamp: new Date()
    })
  }, 5000)

  // Cleanup on unmount
  onBeforeUnmount(() => {
    clearInterval(heartbeatInterval)
  })

  console.log('💓 Periodic events started')
}

const setupCustomEvents = () => {
  // Setup custom event types
  customEvents.value = [
    { name: 'data:update', description: 'Data update notification' },
    { name: 'user:login', description: 'User login event' },
    { name: 'error:occurred', description: 'Error notification' },
    { name: 'task:completed', description: 'Task completion' },
    { name: 'notification:show', description: 'Show notification' }
  ]

  console.log('🎯 Custom events configured')
}

const registerWithParent = () => {
  // Try to emit to parent component
  try {
    const parentEvent = new CustomEvent('component:mounted', {
      detail: {
        componentId: componentId.value,
        capabilities: ['emit', 'listen', 'communicate'],
        timestamp: new Date()
      }
    })
    window.dispatchEvent(parentEvent)
  } catch (error) {
    console.warn('Failed to register with parent:', error)
  }
}

const handleGlobalEvent = (eventData) => {
  if (eventData.componentId === componentId.value) {
    return // Ignore own events
  }

  handleIncomingEvent({
    type: 'global',
    name: eventData.name,
    data: eventData.data,
    source: eventData.source || 'global',
    timestamp: eventData.timestamp
  })
}

const handleIncomingEvent = (eventData) => {
  eventHistory.value.unshift({
    ...eventData,
    direction: 'incoming',
    componentId: componentId.value
  })

  if (eventHistory.value.length > 50) {
    eventHistory.value = eventHistory.value.slice(0, 50)
  }

  // Update state based on event
  switch (eventData.name) {
    case 'ping':
      emitEvent('pong', {
        from: componentId.value,
        to: eventData.source
      })
      break
    case 'state:change':
      if (eventData.data.target === componentId.value) {
        Object.assign(state, eventData.data.updates)
      }
      break
    case 'message:send':
      if (eventData.data.target === componentId.value) {
        communication.receivedMessages.push({
          ...eventData.data,
          receivedAt: new Date()
        })
      }
      break
  }
}

const emitEvent = (eventName, data = {}) => {
  const eventData = {
    name: eventName,
    data: {
      ...data,
      componentId: componentId.value,
      timestamp: new Date()
    },
    source: componentId.value,
    timestamp: new Date()
  }

  // Add to sent messages
  communication.sentMessages.push({
    ...eventData,
    direction: 'outgoing'
  })

  // Update last event
  state.lastEvent = eventData

  // Emit via different channels based on communication mode
  switch (state.communicationMode) {
    case 'global':
      emitGlobalEvent(eventData)
      break
    case 'parent-child':
      emitParentEvent(eventData)
      break
    case 'siblings':
      emitSiblingEvent(eventData)
      break
  }

  // Log event
  console.log(`📤 Component ${componentId.value} emitted:`, eventName, data)
}

const emitGlobalEvent = (eventData) => {
  if (globalEventBus.value) {
    globalEventBus.value.emit(eventData.name, eventData.data)
  }

  // Also dispatch custom event
  const customEvent = new CustomEvent(`app:${eventData.name}`, {
    detail: eventData.data
  })
  window.dispatchEvent(customEvent)
}

const emitParentEvent = (eventData) => {
  const parentEvent = new CustomEvent('child:event', {
    detail: eventData,
    bubbles: true
  })

  if (window.parent !== window) {
    window.parent.dispatchEvent(parentEvent)
  } else {
    window.dispatchEvent(parentEvent)
  }
}

const emitSiblingEvent = (eventData) => {
  const siblingEvent = new CustomEvent('sibling:event', {
    detail: eventData
  })
  window.dispatchEvent(siblingEvent)
}

const sendMessage = (targetId, message) => {
  emitEvent('message:send', {
    target: targetId,
    message: message,
    from: componentId.value
  })
}

const broadcastMessage = (message) => {
  emitEvent('message:broadcast', {
    message: message,
    from: componentId.value,
    broadcast: true
  })
}

const notifyComponentUnloading = () => {
  emitEvent('component:unloading', {
    componentId: componentId.value,
    reason: 'unmount',
    timestamp: new Date()
  })
}

const cleanupChildComponents = () => {
  childComponents.value.forEach((child, id) => {
    emitEvent('child:cleanup', {
      childId: id,
      parent: componentId.value
    })
  })
  childComponents.value.clear()
}

const changeCommunicationMode = (mode) => {
  state.communicationMode = mode
  emitEvent('communication:mode:change', { mode })
}

const clearEventHistory = () => {
  eventHistory.value = []
  communication.sentMessages = []
  communication.receivedMessages = []
}

// Expose methods for parent component
defineExpose({
  componentId,
  emitEvent,
  sendMessage,
  broadcastMessage,
  changeCommunicationMode,
  getState: () => ({ ...state })
})
</script>

<template>
  <div class="communication-hub">
    <header class="hub-header">
      <h2>Component Communication Hub</h2>
      <div class="component-info">
        <span>ID: {{ componentId }}</span>
        <span :class="{ active: state.isActive, inactive: !state.isActive }">
          {{ state.isActive ? 'Active' : 'Inactive' }}
        </span>
        <span :class="{ visible: state.isVisible, hidden: !state.isVisible }">
          {{ state.isVisible ? 'Visible' : 'Hidden' }}
        </span>
      </div>
    </header>

    <div class="communication-controls">
      <div class="mode-selector">
        <label>Communication Mode:</label>
        <select v-model="state.communicationMode" @change="changeCommunicationMode($event.target.value)">
          <option value="parent-child">Parent-Child</option>
          <option value="siblings">Siblings</option>
          <option value="global">Global Event Bus</option>
        </select>
      </div>

      <div class="action-buttons">
        <button @click="sendMessage('all', 'Hello from ' + componentId)">
          📤 Send Message
        </button>
        <button @click="broadcastMessage('Broadcast from ' + componentId)">
          📡 Broadcast
        </button>
        <button @click="emitEvent('ping')">
          🏓 Ping
        </button>
        <button @click="clearEventHistory">
          🗑️ Clear History
        </button>
      </div>
    </div>

    <div class="event-display">
      <div class="event-section">
        <h3>Event History</h3>
        <div class="event-list">
          <div
            v-for="event in eventHistory.slice(0, 10)"
            :key="event.timestamp + event.name"
            class="event-item"
            :class="event.direction"
          >
            <span class="direction-icon">
              {{ event.direction === 'incoming' ? '⬇️' : '⬆️' }}
            </span>
            <span class="event-name">{{ event.name }}</span>
            <span class="event-source">{{ event.source }}</span>
            <span class="event-time">
              {{ new Date(event.timestamp).toLocaleTimeString() }}
            </span>
          </div>
        </div>
        <p v-if="eventHistory.length === 0" class="no-events">No events yet</p>
      </div>

      <div class="communication-stats">
        <h3>Communication Statistics</h3>
        <div class="stats-grid">
          <div class="stat-item">
            <span class="label">Sent:</span>
            <span class="value">{{ communication.sentMessages.length }}</span>
          </div>
          <div class="stat-item">
            <span class="label">Received:</span>
            <span class="value">{{ communication.receivedMessages.length }}</span>
          </div>
          <div class="stat-item">
            <span class="label">Active Channels:</span>
            <span class="value">{{ communication.activeChannels.length }}</span>
          </div>
          <div class="stat-item">
            <span class="label">Heartbeat:</span>
            <span class="value">{{ state.counter }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="message-panel">
      <h3>Message Center</h3>
      <div class="message-input">
        <input
          v-model="state.message"
          placeholder="Type message to send..."
          @keyup.enter="sendMessage('all', state.message)"
        />
        <button @click="sendMessage('all', state.message)">Send</button>
      </div>
      <div class="message-list">
        <div
          v-for="(msg, index) in [...communication.sentMessages.slice(-3), ...communication.receivedMessages.slice(-3)]"
          :key="index"
          class="message-item"
          :class="msg.direction"
        >
          <span class="message-from">
            {{ msg.from || 'Unknown' }}
          </span>
          <span class="message-text">
            {{ msg.message || msg.name }}
          </span>
          <span class="message-time">
            {{ new Date(msg.timestamp || msg.receivedAt).toLocaleTimeString() }}
          </span>
        </div>
      </div>
    </div>

    <div class="features">
      <h3>Communication Features:</h3>
      <ul>
        <li>📡 <strong>Multi-mode Communication:</strong> Parent-child, siblings, global bus</li>
        <li>🎯 <strong>Custom Events:</strong> Custom event emitters and listeners</li>
        <li>📨 <strong>Message Passing:</strong> Direct and broadcast messaging</li>
        <li>💓 <strong>Heartbeat:</strong> Periodic status updates</li>
        <li>🔄 <strong>Event History:</strong> Complete event tracking</li>
        <li>🧹 <strong>Cleanup:</strong> Proper resource cleanup on unmount</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.communication-hub {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.hub-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.component-info {
  display: flex;
  gap: 15px;
  font-size: 14px;
}

.component-info span {
  padding: 4px 8px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.2);
}

.active {
  background: rgba(40, 167, 69, 0.3) !important;
}

.inactive {
  background: rgba(220, 53, 69, 0.3) !important;
}

.visible {
  background: rgba(23, 162, 184, 0.3) !important;
}

.hidden {
  background: rgba(108, 117, 125, 0.3) !important;
}

.communication-controls {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 15px;
  background: #f8f9fa;
  border-radius: 8px;
}

.mode-selector {
  display: flex;
  align-items: center;
  gap: 10px;
}

.mode-selector label {
  font-weight: 500;
}

.mode-selector select {
  padding: 6px 12px;
  border: 1px solid #ced4da;
  border-radius: 4px;
  background: white;
}

.action-buttons {
  display: flex;
  gap: 10px;
}

.action-buttons button {
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  background: #007bff;
  color: white;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.2s;
}

.action-buttons button:hover {
  background: #0056b3;
}

.event-display {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 20px;
  margin-bottom: 20px;
}

.event-section, .communication-stats {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  padding: 20px;
}

.event-section h3, .communication-stats h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.event-list {
  max-height: 200px;
  overflow-y: auto;
}

.event-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px;
  margin-bottom: 4px;
  border-radius: 4px;
  font-size: 14px;
}

.event-item.incoming {
  background: #d4edda;
  border-left: 4px solid #28a745;
}

.event-item.outgoing {
  background: #cce5ff;
  border-left: 4px solid #007bff;
}

.event-name {
  font-weight: 500;
  min-width: 120px;
}

.event-source {
  color: #6c757d;
  min-width: 80px;
}

.event-time {
  font-family: monospace;
  color: #6c757d;
  font-size: 12px;
}

.no-events {
  text-align: center;
  color: #6c757d;
  font-style: italic;
  padding: 20px;
}

.stats-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  background: #f8f9fa;
  border-radius: 4px;
  font-size: 14px;
}

.stat-item .label {
  color: #6c757d;
}

.stat-item .value {
  font-weight: bold;
  color: #495057;
}

.message-panel {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  padding: 20px;
  margin-bottom: 20px;
}

.message-panel h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.message-input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.message-input input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ced4da;
  border-radius: 4px;
}

.message-input button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background: #28a745;
  color: white;
  cursor: pointer;
}

.message-list {
  max-height: 150px;
  overflow-y: auto;
}

.message-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px;
  margin-bottom: 4px;
  border-radius: 4px;
  font-size: 14px;
}

.message-item.sent {
  background: #d1ecf1;
  border-left: 4px solid #17a2b8;
}

.message-item.received {
  background: #d4edda;
  border-left: 4px solid #28a745;
}

.message-from {
  font-weight: 500;
  min-width: 100px;
}

.message-text {
  flex: 1;
  text-align: center;
}

.message-time {
  font-family: monospace;
  color: #6c757d;
  font-size: 12px;
}

.features {
  padding: 20px;
  background: #e2e3e5;
  border-radius: 8px;
}

.features h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.features ul {
  margin: 0;
  padding-left: 20px;
}

.features li {
  margin: 8px 0;
  color: #495057;
}
</style>
```

**Keuntungan:**
- ✅ **Multi-channel communication** - berbagai cara komunikasi antar komponen
- ✅ **Event-driven architecture** - sistem berbasis event yang fleksibel
- ✅ **Real-time messaging** - messaging real-time antar komponen
- ✅ **Resource cleanup** - cleanup listeners dan connections yang proper
- ✅ **Debug-friendly** - logging dan tracking event yang lengkap

### 5. **Performance Monitoring & Debugging**
```vue
<script setup>
import { ref, reactive, onMounted, onBeforeUnmount, onBeforeUpdate, onUpdated } from 'vue'

// Fungsi 5: Monitoring performa dan debugging
const performanceMetrics = reactive({
  mountTime: null,
  renderCount: 0,
  updateCount: 0,
  memoryUsage: 0,
  lastUpdate: null,
  slowUpdates: [],
  renderTimes: [],
  componentSize: {
    domNodes: 0,
    memoryFootprint: 0
  }
})

const debugInfo = reactive({
  componentName: 'PerformanceMonitor',
  vueVersion: '3.x',
  browserInfo: '',
  timestamp: null,
  lifecycleEvents: [],
  errors: [],
  warnings: []
})

const monitoringConfig = reactive({
  enabled: true,
  logLevel: 'info', // debug, info, warn, error
  trackPerformance: true,
  trackMemory: true,
  maxLogEntries: 100
})

// Performance observer
let performanceObserver = null
let memoryObserver = null
let resizeObserver = null

onMounted(() => {
  console.log('📊 Initializing performance monitoring...')
  debugInfo.timestamp = new Date()
  debugInfo.browserInfo = navigator.userAgent

  // 1. Setup performance monitoring
  setupPerformanceMonitoring()

  // 2. Setup memory monitoring
  setupMemoryMonitoring()

  // 3. Setup DOM monitoring
  setupDOMMonitoring()

  // 4. Start metrics collection
  startMetricsCollection()

  // 5. Setup error tracking
  setupErrorTracking()

  // 6. Log initial state
  logDebugInfo('Component mounted', 'info')

  // 7. Start performance profiling
  startProfiling()

  console.log('✅ Performance monitoring initialized')
})

onBeforeUpdate(() => {
  if (monitoringConfig.enabled) {
    const startTime = performance.now()

    // Track update start
    debugInfo.lifecycleEvents.push({
      type: 'beforeUpdate',
      timestamp: new Date(),
      startTime,
      renderCount: performanceMetrics.renderCount
    })
  }
})

onUpdated(() => {
  if (monitoringConfig.enabled) {
    const endTime = performance.now()
    const lastEvent = debugInfo.lifecycleEvents[debugInfo.lifecycleEvents.length - 1]

    if (lastEvent && lastEvent.type === 'beforeUpdate') {
      const updateDuration = endTime - lastEvent.startTime
      performanceMetrics.updateCount++
      performanceMetrics.lastUpdate = new Date()

      // Track render times
      performanceMetrics.renderTimes.push({
        duration: updateDuration,
        timestamp: new Date(),
        type: 'update'
      })

      // Keep only last 50 render times
      if (performanceMetrics.renderTimes.length > 50) {
        performanceMetrics.renderTimes = performanceMetrics.renderTimes.slice(-50)
      }

      // Detect slow updates
      if (updateDuration > 16) { // 16ms = 60fps threshold
        performanceMetrics.slowUpdates.push({
          duration: updateDuration,
          timestamp: new Date(),
          type: 'slow_update'
        })

        logDebugInfo(`Slow update detected: ${updateDuration.toFixed(2)}ms`, 'warn')
      }

      // Update lifecycle event
      lastEvent.endTime = endTime
      lastEvent.duration = updateDuration

      logDebugInfo(`Update completed in ${updateDuration.toFixed(2)}ms`, 'debug')
    }
  }
})

onBeforeUnmount(() => {
  console.log('🛑 Shutting down performance monitoring...')

  // 1. Stop all observers
  if (performanceObserver) {
    performanceObserver.disconnect()
    performanceObserver = null
  }

  if (memoryObserver) {
    memoryObserver.disconnect()
    memoryObserver = null
  }

  if (resizeObserver) {
    resizeObserver.disconnect()
    resizeObserver = null
  }

  // 2. Generate final report
  generatePerformanceReport()

  // 3. Cleanup data
  debugInfo.lifecycleEvents = []
  performanceMetrics.renderTimes = []

  // 4. Log final state
  logDebugInfo('Performance monitoring shut down', 'info')

  console.log('✅ Performance monitoring shut down')
})

const setupPerformanceMonitoring = () => {
  if (!monitoringConfig.trackPerformance) return

  try {
    performanceObserver = new PerformanceObserver((list) => {
      list.getEntries().forEach((entry) => {
        if (entry.entryType === 'measure') {
          performanceMetrics.renderTimes.push({
            duration: entry.duration,
            timestamp: new Date(),
            type: entry.name
          })
        }
      })
    })

    performanceObserver.observe({ entryTypes: ['measure'] })

    // Mark performance points
    performance.mark('component-mounted')
    performance.mark('monitoring-started')

    logDebugInfo('Performance monitoring setup completed', 'info')
  } catch (error) {
    logDebugInfo(`Failed to setup performance monitoring: ${error.message}`, 'error')
  }
}

const setupMemoryMonitoring = () => {
  if (!monitoringConfig.trackMemory) return

  try {
    // Monitor memory usage
    memoryObserver = new PerformanceObserver((list) => {
      list.getEntries().forEach((entry) => {
        if (entry.entryType === 'measure' && entry.name.includes('memory')) {
          performanceMetrics.memoryUsage = parseFloat(entry.duration)
        }
      })
    })

    // Check for memory API availability
    if ('memory' in performance) {
      setInterval(() => {
        const memoryInfo = performance.memory
        performanceMetrics.memoryUsage = memoryInfo.usedJSHeapSize / 1024 / 1024 // MB

        // Log memory warnings
        if (performanceMetrics.memoryUsage > 100) {
          logDebugInfo(`High memory usage: ${performanceMetrics.memoryUsage.toFixed(2)}MB`, 'warn')
        }
      }, 5000)
    }

    logDebugInfo('Memory monitoring setup completed', 'info')
  } catch (error) {
    logDebugInfo(`Failed to setup memory monitoring: ${error.message}`, 'error')
  }
}

const setupDOMMonitoring = () => {
  try {
    // Monitor DOM mutations
    resizeObserver = new ResizeObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.target === $el) {
          performanceMetrics.componentSize.domNodes = countDOMNodes($el)
        }
      })
    })

    if ($el) {
      resizeObserver.observe($el)
      performanceMetrics.componentSize.domNodes = countDOMNodes($el)
    }

    logDebugInfo('DOM monitoring setup completed', 'info')
  } catch (error) {
    logDebugInfo(`Failed to setup DOM monitoring: ${error.message}`, 'error')
  }
}

const startMetricsCollection = () => {
  if (!monitoringConfig.enabled) return

  // Collect metrics every 2 seconds
  setInterval(() => {
    collectMetrics()
  }, 2000)
}

const collectMetrics = () => {
  try {
    // Memory metrics
    if (performance.memory) {
      const memory = performance.memory
      performanceMetrics.componentSize.memoryFootprint = memory.usedJSHeapSize
    }

    // Performance metrics
    const avgRenderTime = performanceMetrics.renderTimes.length > 0
      ? performanceMetrics.renderTimes.reduce((sum, r) => sum + r.duration, 0) / performanceMetrics.renderTimes.length
      : 0

    // Log performance summary
    logDebugInfo(`Performance: ${avgRenderTime.toFixed(2)}ms avg render time`, 'debug')
  } catch (error) {
    logDebugInfo(`Failed to collect metrics: ${error.message}`, 'error')
  }
}

const setupErrorTracking = () => {
  // Track unhandled errors
  const originalErrorHandler = window.onerror

  window.onerror = (message, source, lineno, colno, error) => {
    debugInfo.errors.push({
      message,
      source,
      line: lineno,
      column: colno,
      error: error?.stack,
      timestamp: new Date()
    })

    logDebugInfo(`Error: ${message}`, 'error')

    // Call original handler
    if (originalErrorHandler) {
      return originalErrorHandler(message, source, lineno, colno, error)
    }
  }

  // Track unhandled promise rejections
  const originalRejectionHandler = window.onunhandledrejection

  window.onunhandledrejection = (event) => {
    debugInfo.errors.push({
      message: event.reason?.message || 'Unhandled promise rejection',
      stack: event.reason?.stack,
      timestamp: new Date()
    })

    logDebugInfo(`Unhandled rejection: ${event.reason?.message}`, 'error')

    // Call original handler
    if (originalRejectionHandler) {
      return originalRejectionHandler(event)
    }
  }
}

const startProfiling = () => {
  if (!monitoringConfig.enabled) return

  // Mark component start
  performance.mark('component-profiling-start')

  // Profile component lifecycle
  setTimeout(() => {
    performance.mark('component-profiling-end')
    performance.measure('component-initialization', 'component-profiling-start', 'component-profiling-end')

    // Log profiling results
    const measures = performance.getEntriesByType('measure')
    const initMeasure = measures.find(m => m.name === 'component-initialization')

    if (initMeasure) {
      performanceMetrics.mountTime = initMeasure.duration
      logDebugInfo(`Component initialization: ${initMeasure.duration.toFixed(2)}ms`, 'info')
    }
  }, 100)
}

const countDOMNodes = (element) => {
  if (!element) return 0

  let count = 0
  const walker = document.createTreeWalker(
    element,
    NodeFilter.SHOW_ALL,
    null,
    false
  )

  while (walker.nextNode()) {
    count++
  }

  return count
}

const logDebugInfo = (message, level = 'info') => {
  if (!monitoringConfig.enabled) return

  const shouldLog = level === 'error' ||
    (level === 'warn' && ['warn', 'error', 'debug'].includes(monitoringConfig.logLevel)) ||
    (level === 'info' && ['info', 'warn', 'error', 'debug'].includes(monitoringConfig.logLevel)) ||
    (level === 'debug' && monitoringConfig.logLevel === 'debug')

  if (!shouldLog) return

  const logEntry = {
    message,
    level,
    timestamp: new Date(),
    component: debugInfo.componentName
  }

  // Add to appropriate category
  switch (level) {
    case 'error':
      debugInfo.errors.push(logEntry)
      console.error(`[${debugInfo.componentName}] ${message}`)
      break
    case 'warn':
      debugInfo.warnings.push(logEntry)
      console.warn(`[${debugInfo.componentName}] ${message}`)
      break
    case 'info':
      console.info(`[${debugInfo.componentName}] ${message}`)
      break
    case 'debug':
      console.log(`[${debugInfo.componentName}] ${message}`)
      break
  }

  // Keep log entries within limit
  if (debugInfo.errors.length > monitoringConfig.maxLogEntries) {
    debugInfo.errors = debugInfo.errors.slice(-monitoringConfig.maxLogEntries)
  }
  if (debugInfo.warnings.length > monitoringConfig.maxLogEntries) {
    debugInfo.warnings = debugInfo.warnings.slice(-monitoringConfig.maxLogEntries)
  }
}

const triggerSlowUpdate = () => {
  // Simulate slow update for testing
  const startTime = performance.now()

  // Busy work for testing
  let sum = 0
  for (let i = 0; i < 1000000; i++) {
    sum += Math.random()
  }

  const endTime = performance.now()
  const duration = endTime - startTime

  logDebugInfo(`Manual slow update: ${duration.toFixed(2)}ms`, 'warn')
}

const generatePerformanceReport = () => {
  const report = {
    componentName: debugInfo.componentName,
    monitoringDuration: Date.now() - debugInfo.timestamp.getTime(),
    totalRenders: performanceMetrics.renderCount,
    totalUpdates: performanceMetrics.updateCount,
    mountTime: performanceMetrics.mountTime,
    avgRenderTime: performanceMetrics.renderTimes.length > 0
      ? performanceMetrics.renderTimes.reduce((sum, r) => sum + r.duration, 0) / performanceMetrics.renderTimes.length
      : 0,
    slowUpdatesCount: performanceMetrics.slowUpdates.length,
    memoryUsage: performanceMetrics.memoryUsage,
    domNodes: performanceMetrics.componentSize.domNodes,
    errors: debugInfo.errors.length,
    warnings: debugInfo.warnings.length
  }

  console.log('📊 Performance Report:', report)
  return report
}

const clearMetrics = () => {
  performanceMetrics.renderTimes = []
  performanceMetrics.slowUpdates = []
  debugInfo.errors = []
  debugInfo.warnings = []
  debugInfo.lifecycleEvents = []

  logDebugInfo('Metrics cleared', 'info')
}

const toggleMonitoring = () => {
  monitoringConfig.enabled = !monitoringConfig.enabled
  logDebugInfo(`Monitoring ${monitoringConfig.enabled ? 'enabled' : 'disabled'}`, 'info')
}
</script>

<template>
  <div class="performance-monitor">
    <header class="monitor-header">
      <h2>Performance Monitor</h2>
      <div class="monitor-controls">
        <button @click="toggleMonitoring" :class="{ active: monitoringConfig.enabled }">
          {{ monitoringConfig.enabled ? '📊 On' : '📊 Off' }}
        </button>
        <button @click="triggerSlowUpdate">⏱️ Test Slow Update</button>
        <button @click="clearMetrics">🗑️ Clear Metrics</button>
      </div>
    </header>

    <div class="metrics-grid">
      <div class="metric-card">
        <h3>Performance Metrics</h3>
        <div class="metric-list">
          <div class="metric-item">
            <span>Mount Time:</span>
            <span>{{ performanceMetrics.mountTime ? performanceMetrics.mountTime.toFixed(2) + 'ms' : 'N/A' }}</span>
          </div>
          <div class="metric-item">
            <span>Total Renders:</span>
            <span>{{ performanceMetrics.renderCount }}</span>
          </div>
          <div class="metric-item">
            <span>Total Updates:</span>
            <span>{{ performanceMetrics.updateCount }}</span>
          </div>
          <div class="metric-item">
            <span>Slow Updates:</span>
            <span :class="{ warning: performanceMetrics.slowUpdates.length > 0 }">
              {{ performanceMetrics.slowUpdates.length }}
            </span>
          </div>
          <div class="metric-item">
            <span>Avg Render Time:</span>
            <span>
              {{
                performanceMetrics.renderTimes.length > 0
                  ? (performanceMetrics.renderTimes.reduce((sum, r) => sum + r.duration, 0) / performanceMetrics.renderTimes.length).toFixed(2) + 'ms'
                  : 'N/A'
              }}
            </span>
          </div>
        </div>
      </div>

      <div class="metric-card">
        <h3>Memory Usage</h3>
        <div class="metric-list">
          <div class="metric-item">
            <span>Memory Usage:</span>
            <span>{{ performanceMetrics.memoryUsage.toFixed(2) }} MB</span>
          </div>
          <div class="metric-item">
            <span>DOM Nodes:</span>
            <span>{{ performanceMetrics.componentSize.domNodes }}</span>
          </div>
          <div class="metric-item">
            <span>Memory Footprint:</span>
            <span>{{ (performanceMetrics.componentSize.memoryFootprint / 1024 / 1024).toFixed(2) }} MB</span>
          </div>
        </div>
      </div>

      <div class="metric-card">
        <h3>Debug Information</h3>
        <div class="metric-list">
          <div class="metric-item">
            <span>Component:</span>
            <span>{{ debugInfo.componentName }}</span>
          </div>
          <div class="metric-item">
            <span>Vue Version:</span>
            <span>{{ debugInfo.vueVersion }}</span>
          </div>
          <div class="metric-item">
            <span>Started:</span>
            <span>{{ debugInfo.timestamp ? new Date(debugInfo.timestamp).toLocaleTimeString() : 'N/A' }}</span>
          </div>
          <div class="metric-item">
            <span>Errors:</span>
            <span :class="{ error: debugInfo.errors.length > 0 }">{{ debugInfo.errors.length }}</span>
          </div>
          <div class="metric-item">
            <span>Warnings:</span>
            <span :class="{ warning: debugInfo.warnings.length > 0 }">{{ debugInfo.warnings.length }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="render-times-chart">
      <h3>Render Times History</h3>
      <div class="chart-container">
        <div
          v-for="(render, index) in performanceMetrics.renderTimes.slice(-20)"
          :key="index"
          class="render-bar"
          :style="{
            height: Math.min(100, (render.duration / 16) * 100) + '%',
            backgroundColor: render.duration > 16 ? '#ff6b6b' : '#4ecdc4'
          }"
          :title="`${render.duration.toFixed(2)}ms at ${new Date(render.timestamp).toLocaleTimeString()}`"
        ></div>
      </div>
      <div class="chart-legend">
        <span class="legend-item fast">≤16ms (60fps)</span>
        <span class="legend-item slow">>16ms (slow)</span>
      </div>
    </div>

    <div class="monitoring-config">
      <h3>Monitoring Configuration</h3>
      <div class="config-grid">
        <div class="config-item">
          <label>Enabled:</label>
          <input
            type="checkbox"
            v-model="monitoringConfig.enabled"
            @change="toggleMonitoring"
          />
        </div>
        <div class="config-item">
          <label>Track Performance:</label>
          <input type="checkbox" v-model="monitoringConfig.trackPerformance" />
        </div>
        <div class="config-item">
          <label>Track Memory:</label>
          <input type="checkbox" v-model="monitoringConfig.trackMemory" />
        </div>
        <div class="config-item">
          <label>Log Level:</label>
          <select v-model="monitoringConfig.logLevel">
            <option value="error">Error</option>
            <option value="warn">Warning</option>
            <option value="info">Info</option>
            <option value="debug">Debug</option>
          </select>
        </div>
      </div>
    </div>

    <div class="features">
      <h3>Performance Features Demonstrated:</h3>
      <ul>
        <li>📊 <strong>Performance Tracking:</strong> Render times and update metrics</li>
        <li>🧠 <strong>Memory Monitoring:</strong> JavaScript heap usage tracking</li>
        <li>🌐 <strong>DOM Monitoring:</strong> DOM node count and size tracking</li>
        <li>🔍 <strong>Error Tracking:</strong> Comprehensive error and warning logging</li>
        <li>📈 <strong>Real-time Metrics:</strong> Live performance data collection</li>
        <li>📊 <strong>Performance Report:</strong> Detailed performance analysis</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.performance-monitor {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.monitor-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
}

.monitor-controls {
  display: flex;
  gap: 10px;
}

.monitor-controls button {
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  cursor: pointer;
  transition: background-color 0.2s;
}

.monitor-controls button:hover {
  background: rgba(255, 255, 255, 0.3);
}

.monitor-controls button.active {
  background: rgba(40, 167, 69, 0.3);
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.metric-card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 20px;
}

.metric-card h3 {
  margin: 0 0 15px 0;
  color: #495057;
  border-bottom: 2px solid #e9ecef;
  padding-bottom: 10px;
}

.metric-list {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.metric-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
  font-size: 14px;
}

.metric-item span:first-child {
  color: #6c757d;
}

.metric-item span:last-child {
  font-weight: 500;
  color: #495057;
}

.metric-item span.warning {
  color: #ffc107;
}

.metric-item span.error {
  color: #dc3545;
}

.render-times-chart {
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
  padding: 20px;
  margin-bottom: 20px;
}

.render-times-chart h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.chart-container {
  display: flex;
  align-items: flex-end;
  height: 100px;
  gap: 2px;
  margin-bottom: 10px;
  padding: 10px;
  background: #f8f9fa;
  border-radius: 4px;
}

.render-bar {
  flex: 1;
  min-height: 2px;
  border-radius: 2px;
  transition: all 0.3s ease;
}

.chart-legend {
  display: flex;
  gap: 20px;
  font-size: 12px;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 5px;
}

.legend-item.fast {
  color: #4ecdc4;
}

.legend-item.slow {
  color: #ff6b6b;
}

.monitoring-config {
  background: #f8f9fa;
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 20px;
}

.monitoring-config h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.config-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.config-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.config-item label {
  font-weight: 500;
  color: #495057;
}

.config-item input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

.config-item select {
  padding: 4px 8px;
  border: 1px solid #ced4da;
  border-radius: 4px;
}

.features {
  padding: 20px;
  background: #e2e3e5;
  border-radius: 12px;
}

.features h3 {
  margin: 0 0 15px 0;
  color: #495057;
}

.features ul {
  margin: 0;
  padding-left: 20px;
}

.features li {
  margin: 8px 0;
  color: #495057;
}
</style>
```

**Keuntungan:**
- ✅ **Performance tracking** - monitoring performa real-time
- ✅ **Memory monitoring** - tracking penggunaan memory
- ✅ **Error tracking** - comprehensive error logging
- ✅ **Debug information** - debugging tools yang lengkap
- ✅ **Performance profiling** - analisis performa mendalam

## 🎯 Best Practices untuk Lifecycle Hooks

### 1. **Gunakan Hooks yang Tepat**
```vue
<script setup>
// ✅ Gunakan onMounted untuk DOM access
onMounted(() => {
  // Akses DOM elements
  const element = document.querySelector('.my-element')
})

// ✅ Gunakan onUnmounted untuk cleanup
onUnmounted(() => {
  // Cleanup resources
})
</script>
```

### 2. **Avoid Heavy Operations di onBeforeMount**
```vue
<script setup>
// ❌ Jangan lakukan heavy operations di onBeforeMount
onBeforeMount(() => {
  // Don't do this!
  // await fetch('/api/data') // Bad practice
})

// ✅ Lakukan di onMounted
onMounted(async () => {
  const response = await fetch('/api/data')
})
</script>
```

## 🎉 Kesimpulan

Lifecycle Hooks adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Component initialization** - setup yang terstruktur dan bertahap
- ✅ **DOM manipulation** - kontrol penuh atas DOM manipulation
- ✅ **Data synchronization** - sinkronisasi data dan API integration
- ✅ **Component communication** - komunikasi antar komponen yang efisien
- ✅ **Performance monitoring** - monitoring performa dan debugging

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Lifecycle Hooks
- Vue.js Composition API Guide
- Vue.js Performance Best Practices

---

**🔥 Tips Pro:**
- Gunakan **onMounted** untuk DOM access dan API calls
- Gunakan **onUnmounted** untuk cleanup resources
- Hindari **heavy operations** di onBeforeMount
- Implement **proper error handling** di semua hooks
- Monitor **performance** dengan lifecycle hooks

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation