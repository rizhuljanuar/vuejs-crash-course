# 🎯 Custom Directives - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara membuat dan menggunakan custom directives untuk manipulasi DOM secara langsung dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Custom Directives?](#-apa-itu-custom-directives)
- [🎮 Cara Menggunakan Custom Directives](#-cara-menggunakan-custom-directives)
- [⚡ Basic Custom Directive](#-basic-custom-directive)
- [🔄 Directive dengan Values](#-directive-dengan-values)
- [🎯 Directive dengan Arguments](#-directive-dengan-arguments)
- [🔧 Advanced Directive Hooks](#-advanced-directive-hooks)
- [📦 Directive dengan Modifiers](#-directive-dengan-modifiers)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Custom Directives?

**Custom Directives** adalah fitur Vue.js yang memungkinkan kita untuk membuat custom HTML directives untuk manipulasi DOM secara langsung. Directive adalah cara untuk menginstruksikan Vue untuk melakukan sesuatu pada DOM elements.

### Konsep Dasar Custom Directives

```javascript
// Custom directive definition
const vMyDirective = {
  mounted(el, binding) {
    // Logic yang dijalankan saat element dimount
    el.textContent = 'Hello from custom directive!'
  }
}
```

### Mengapa Custom Directives Penting?

1. **🎯 DOM Manipulation:** Manipulasi DOM secara langsung
2. **🔄 Reusability:** Logic yang dapat digunakan kembali
3. **⚡ Performance:** Efisien untuk manipulasi DOM yang kompleks
4. **🎨 Styling Logic:** Mengelola styling yang kompleks
5. **📱 Integration:** Integrasi dengan library pihak ketiga

### Built-in Directives vs Custom Directives

| Directive | Type | Kegunaan |
|-----------|------|----------|
| `v-if` | Built-in | Conditional rendering |
| `v-for` | Built-in | List rendering |
| `v-model` | Built-in | Two-way binding |
| `v-show` | Built-in | Conditional display |
| `v-custom` | Custom | Logic custom kita |

### Lifecycle Directive Hooks

```javascript
const vMyDirective = {
  // Called before bound element's attributes
  // or event listeners are applied
  created(el, binding, vnode, prevVnode) {},

  // Called right after the bound element has been inserted into its parent DOM
  mounted(el, binding, vnode, prevVnode) {},

  // Called before the component is updated
  beforeUpdate(el, binding, vnode, prevVnode) {},

  // Called after the component has been updated
  updated(el, binding, vnode, prevVnode) {},

  // Called before the bound element's parent component is unmounted
  beforeUnmount(el, binding, vnode, prevVnode) {},

  // Called after the bound element's parent component is unmounted
  unmounted(el, binding, vnode, prevVnode) {}
}
```

## 🎮 Cara Menggunakan Custom Directives

### 1. Definisikan Directive

```vue
<script setup>
// Local directive
const vHighlight = {
  mounted(el) {
    el.style.backgroundColor = 'yellow'
  }
}
</script>
```

### 2. Gunakan di Template

```vue
<template>
  <p v-highlight>This text will be highlighted!</p>
</template>
```

### 3. Global Directive Registration

```javascript
// main.js
import { createApp } from 'vue'

const app = createApp(App)

// Global directive registration
app.directive('highlight', {
  mounted(el) {
    el.style.backgroundColor = 'yellow'
  }
})
```

## ⚡ Basic Custom Directive

### Simple Text Formatting Directive

```vue
<script setup>
// Basic custom directive untuk formatting text
const vFormatDiv = {
  mounted(el) {
    el.style.fontSize = '4rem'
    el.style.fontStyle = 'italic'
    el.style.color = 'teal'
  }
}

// Function shorthand untuk simple directives
const vFontSize = (el) => {
  el.style.fontSize = '2rem'
}
</script>

<template>
  <div class="directive-demo">
    <h2>Basic Custom Directives</h2>

    <div class="example-section">
      <h3>Text Formatting Directive:</h3>
      <div v-format-div>Using simple custom directive</div>
      <p>Text di atas diubah oleh v-format-div directive</p>
    </div>

    <div class="example-section">
      <h3>Function Shorthand:</h3>
      <h1 v-font-size>Using Function Short Hand</h1>
      <p>Directive di atas menggunakan function shorthand</p>
    </div>

    <div class="info-box">
      <h4>📝 Apa yang terjadi:</h4>
      <ul>
        <li><strong>v-format-div:</strong> Mengubah font size, style, dan warna</li>
        <li><strong>v-font-size:</strong> Hanya mengubah font size</li>
        <li>Logic dijalankan saat element dimount ke DOM</li>
        <li>Manipulasi CSS secara langsung via JavaScript</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.directive-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
}

.example-section h3 {
  margin-top: 0;
  color: #0c4a6e;
}

.example-section p {
  color: #075985;
  font-style: italic;
  margin-top: 10px;
}

.info-box {
  background: #e0f2fe;
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

### Focus Directive

```vue
<script setup>
// Auto-focus directive
const vFocus = {
  mounted(el) {
    el.focus()
  }
}

// Highlight on focus directive
const vHighlightOnFocus = {
  mounted(el) {
    const originalBorder = el.style.border

    const handleFocus = () => {
      el.style.border = '3px solid #3b82f6'
      el.style.boxShadow = '0 0 0 3px rgba(59, 130, 246, 0.1)'
    }

    const handleBlur = () => {
      el.style.border = originalBorder
      el.style.boxShadow = 'none'
    }

    el.addEventListener('focus', handleFocus)
    el.addEventListener('blur', handleBlur)

    // Cleanup saat element di-unmount
    el._cleanup = () => {
      el.removeEventListener('focus', handleFocus)
      el.removeEventListener('blur', handleBlur)
    }
  },
  unmounted(el) {
    if (el._cleanup) {
      el._cleanup()
    }
  }
}
</script>

<template>
  <div class="focus-demo">
    <h2>Focus Directives</h2>

    <div class="example-section">
      <h3>Auto-Focus Directive:</h3>
      <input
        v-focus
        type="text"
        placeholder="This input will be auto-focused"
        class="input-field"
      />
      <p>Input akan otomatis fokus saat component dimuat</p>
    </div>

    <div class="example-section">
      <h3>Highlight on Focus Directive:</h3>
      <textarea
        v-highlight-on-focus
        placeholder="Type something here and see the highlight"
        class="textarea-field"
      ></textarea>
      <p>Textarea akan highlight saat fokus</p>
    </div>

    <div class="info-box">
      <h4>🎯 Use Cases:</h4>
      <ul>
        <li><strong>Auto-focus:</strong> Form fields yang perlu immediate attention</li>
        <li><strong>Visual feedback:</strong> Enhanced user experience</li>
        <li><strong>Accessibility:</strong> Better navigation support</li>
        <li><strong>Guidance:</strong> Mengarahkan user attention</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.focus-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
  padding: 20px;
  background: #faf5ff;
  border-radius: 8px;
}

.example-section h3 {
  margin-top: 0;
  color: #6b21a8;
}

.input-field, .textarea-field {
  width: 100%;
  padding: 10px;
  border: 2px solid #e5e7eb;
  border-radius: 6px;
  font-size: 14px;
  transition: all 0.3s ease;
}

.textarea-field {
  min-height: 80px;
  resize: vertical;
}

.example-section p {
  margin-top: 10px;
  color: #6b21a8;
  font-style: italic;
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

## 🔄 Directive dengan Values

### Dynamic Value Directive

```vue
<script setup>
import { ref } from 'vue'

const fontSize = ref(16)

// Directive dengan dynamic value
const vFormatDiv = {
  mounted(el, binding) {
    el.style.fontSize = `${binding.value}px`
    el.style.border = `${binding.value}px solid teal`
  },
  updated(el, binding) {
    el.style.fontSize = `${binding.value}px`
    el.style.border = `${binding.value}px solid teal`
  }
}
</script>

<template>
  <div class="value-demo">
    <h2>Directives with Values</h2>

    <div class="example-section">
      <h3>Dynamic Font Size Directive:</h3>
      <div v-format-div="fontSize">This text changes size!</div>

      <div class="controls">
        <label for="font-slider">Font Size: {{ fontSize }}px</label>
        <input
          id="font-slider"
          v-model.number="fontSize"
          type="range"
          min="10"
          max="50"
          class="slider"
        />
      </div>

      <div class="button-controls">
        <button @click="fontSize = 10">Small</button>
        <button @click="fontSize = 20">Medium</button>
        <button @click="fontSize = 30">Large</button>
        <button @click="fontSize = 40">Extra Large</button>
      </div>
    </div>

    <div class="info-box">
      <h4>📊 Binding Object Properties:</h4>
      <p>Directive menerima <code>binding</code> object dengan properties:</p>
      <ul>
        <li><strong>value:</strong> Nilai yang diberikan ke directive</li>
        <li><strong>oldValue:</strong> Nilai sebelumnya (di updated hook)</li>
        <li><strong>arg:</strong> Argument dari directive</li>
        <li><strong>modifiers:</strong> Modifiers yang digunakan</li>
        <li><strong>instance:</strong> Component instance</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.value-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
  padding: 20px;
  background: #fffbeb;
  border-radius: 8px;
}

.example-section h3 {
  margin-top: 0;
  color: #92400e;
}

.controls {
  margin: 20px 0;
}

.controls label {
  display: block;
  margin-bottom: 10px;
  color: #92400e;
  font-weight: 500;
}

.slider {
  width: 100%;
  margin-bottom: 20px;
}

.button-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.button-controls button {
  padding: 8px 16px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.button-controls button:hover {
  background: #d97706;
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

.info-box p {
  margin: 10px 0;
  color: #78350f;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #78350f;
}

.info-box code {
  background: #fbbf24;
  color: #78350f;
  padding: 2px 6px;
  border-radius: 3px;
  font-family: monospace;
  font-size: 14px;
}
</style>
```

## 🎯 Directive dengan Arguments

### Argument-based Color Directive

```vue
<script setup>
// Directive dengan arguments untuk color
const vFormatDiv = {
  mounted(el, binding) {
    switch (binding.arg) {
      case 'orange':
        el.style.color = 'orange'
        break
      case 'pink':
        el.style.color = 'pink'
        break
      case 'purple':
        el.style.color = 'purple'
        break
      case 'teal':
        el.style.color = 'teal'
        break
      default:
        el.style.color = 'black'
    }

    el.style.fontSize = '2rem'
    el.style.fontWeight = 'bold'
    el.style.padding = '10px'
    el.style.borderRadius = '8px'
  }
}
</script>

<template>
  <div class="argument-demo">
    <h2>Directives with Arguments</h2>

    <div class="example-section">
      <h3>Color Directive with Arguments:</h3>
      <div class="color-examples">
        <h1 v-format-div:default>This text will be black</h1>
        <h1 v-format-div:orange>This text will be orange</h1>
        <h1 v-format-div:pink>This text will be pink</h1>
        <h1 v-format-div:purple>This text will be purple</h1>
        <h1 v-format-div:teal>This text will be teal</h1>
      </div>
    </div>

    <div class="syntax-explanation">
      <h4>📝 Syntax:</h4>
      <div class="syntax-example">
        <code>v-directive-name:argument="value"</code>
        <ul>
          <li><strong>directive-name:</strong> Nama directive</li>
          <li><strong>argument:</strong> Argument yang dikirim (optional)</li>
          <li><strong>value:</strong> Value yang dikirim (optional)</li>
        </ul>
      </div>
    </div>

    <div class="info-box">
      <h4>🎯 Use Cases for Arguments:</h4>
      <ul>
        <li><strong>Color themes:</strong> Different color variations</li>
        <li><strong>Sizes:</strong> Size variations (small, medium, large)</li>
        <li><strong>Positions:</strong> Position variations (top, bottom, left, right)</li>
        <li><strong>Variants:</strong> Different style variants</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.argument-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #ec4899;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
  padding: 20px;
  background: #fdf2f8;
  border-radius: 8px;
}

.example-section h3 {
  margin-top: 0;
  color: #9f1239;
}

.color-examples {
  display: grid;
  gap: 15px;
  margin: 20px 0;
}

.color-examples h1 {
  margin: 0;
  text-align: center;
  background: white;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.syntax-explanation {
  margin-bottom: 20px;
}

.syntax-explanation h4 {
  color: #9f1239;
}

.syntax-example {
  background: white;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ec4899;
}

.syntax-example code {
  display: block;
  background: #fce7f3;
  color: #9f1239;
  padding: 8px 12px;
  border-radius: 4px;
  font-family: monospace;
  font-size: 14px;
  margin-bottom: 10px;
}

.syntax-example ul {
  margin: 0;
  padding-left: 20px;
}

.syntax-example li {
  margin: 5px 0;
  color: #831843;
}

.info-box {
  background: #fdf2f8;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ec4899;
}

.info-box h4 {
  margin-top: 0;
  color: #9f1239;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #831843;
}
</style>
```

## 🔧 Advanced Directive Hooks

### Complete Lifecycle Directive

```vue
<script setup>
import { ref } from 'vue'

const message = ref('')
const logs = ref([])

// Complete directive dengan semua lifecycle hooks
const vLifecycleLogger = {
  // Called before element is mounted
  created(el, binding) {
    addLog('created', `Element created for directive: ${binding.arg || 'no-arg'}`)
    el.dataset.created = Date.now()
  },

  // Called after element is mounted
  mounted(el, binding) {
    addLog('mounted', `Element mounted at ${new Date().toLocaleTimeString()}`)

    // Add initial styling
    el.style.padding = '10px'
    el.style.border = '2px solid #3b82f6'
    el.style.borderRadius = '4px'
    el.style.backgroundColor = '#dbeafe'
  },

  // Called before element is updated
  beforeUpdate(el, binding) {
    addLog('beforeUpdate', `Element about to update`)
  },

  // Called after element is updated
  updated(el, binding) {
    addLog('updated', `Element updated with value: ${binding.value}`)

    // Update styling based on value
    if (binding.value) {
      el.style.backgroundColor = binding.value.color || '#dbeafe'
      el.style.fontSize = `${binding.value.fontSize || 14}px`
    }
  },

  // Called before element is unmounted
  beforeUnmount(el, binding) {
    addLog('beforeUnmount', `Element about to unmount`)
  },

  // Called after element is unmounted
  unmounted(el, binding) {
    addLog('unmounted', `Element unmounted`)
  }
}

const addLog = (event, message) => {
  logs.value.unshift({
    event,
    message,
    timestamp: new Date().toLocaleTimeString()
  })

  // Keep only last 10 logs
  if (logs.value.length > 10) {
    logs.value = logs.value.slice(0, 10)
  }
}

const directiveValue = ref({
  color: '#dcfce7',
  fontSize: 16
})

const showElement = ref(true)

const updateDirective = () => {
  // Random color
  const colors = ['#dcfce7', '#fef3c7', '#fee2e2', '#e0e7ff', '#f3e8ff']
  directiveValue.value = {
    color: colors[Math.floor(Math.random() * colors.length)],
    fontSize: Math.floor(Math.random() * 20) + 12
  }
}

const toggleElement = () => {
  showElement.value = !showElement.value
}

const clearLogs = () => {
  logs.value = []
}
</script>

<template>
  <div class="lifecycle-demo">
    <h2>Advanced Directive Hooks</h2>

    <div class="example-section">
      <h3>Complete Lifecycle Logger:</h3>

      <div class="controls">
        <button @click="updateDirective">Update Directive Value</button>
        <button @click="toggleElement">
          {{ showElement ? 'Hide' : 'Show' }} Element
        </button>
        <button @click="clearLogs">Clear Logs</button>
      </div>

      <div v-if="showElement" class="demo-element">
        <p v-lifecycle_logger:text="directiveValue">
          This element demonstrates all directive lifecycle hooks!
        </p>
      </div>

      <div class="logs-section">
        <h4>📋 Lifecycle Logs:</h4>
        <div v-if="logs.length === 0" class="no-logs">
          No logs yet. Try the buttons above!
        </div>
        <div v-else class="logs">
          <div
            v-for="(log, index) in logs"
            :key="index"
            class="log-entry"
            :class="log.event"
          >
            <span class="timestamp">{{ log.timestamp }}</span>
            <span class="event">{{ log.event.toUpperCase() }}</span>
            <span class="message">{{ log.message }}</span>
          </div>
        </div>
      </div>
    </div>

    <div class="hooks-info">
      <h4>🔄 Directive Hooks Explained:</h4>
      <div class="hooks-grid">
        <div class="hook-item">
          <h5>created</h5>
          <p>Sebelum element dimount, setup awal</p>
        </div>
        <div class="hook-item">
          <h5>mounted</h5>
          <p>Saat element dimount ke DOM</p>
        </div>
        <div class="hook-item">
          <h5>beforeUpdate</h5>
          <p>Sebelum update saat data berubah</p>
        </div>
        <div class="hook-item">
          <h5>updated</h5>
          <p>Sesudah update selesai</p>
        </div>
        <div class="hook-item">
          <h5>beforeUnmount</h5>
          <p>Sebelum element dihapus</p>
        </div>
        <div class="hook-item">
          <h5>unmounted</h5>
          <p>Sesudah element dihapus</p>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>💡 Best Practices:</h4>
      <ul>
        <li>Gunakan <strong>mounted</strong> untuk DOM manipulation awal</li>
        <li>Gunakan <strong>updated</strong> untuk reaksi terhadap perubahan</li>
        <li>Gunakan <strong>unmounted</strong> untuk cleanup event listeners</li>
        <li>Gunakan <strong>created</strong> untuk setup non-DOM</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.lifecycle-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.controls button {
  padding: 8px 16px;
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

.demo-element {
  margin: 20px 0;
  padding: 20px;
  text-align: center;
  background: #f0fdf4;
  border-radius: 8px;
}

.logs-section {
  margin: 20px 0;
}

.logs-section h4 {
  margin-bottom: 10px;
  color: #065f46;
}

.no-logs {
  text-align: center;
  padding: 20px;
  color: #6b7280;
  font-style: italic;
  background: #f8fafc;
  border-radius: 8px;
}

.logs {
  max-height: 300px;
  overflow-y: auto;
  background: #f8fafc;
  border-radius: 8px;
  padding: 10px;
}

.log-entry {
  display: grid;
  grid-template-columns: 80px 100px 1fr;
  gap: 10px;
  padding: 8px;
  margin-bottom: 5px;
  background: white;
  border-radius: 4px;
  font-family: monospace;
  font-size: 12px;
}

.timestamp {
  color: #64748b;
}

.event {
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 3px;
}

.created { background: #dbeafe; color: #1e40af; }
.mounted { background: #dcfce7; color: #166534; }
.beforeUpdate { background: #fef3c7; color: #92400e; }
.updated { background: #e0e7ff; color: #3730a3; }
.beforeUnmount { background: #fee2e2; color: #991b1b; }
.unmounted { background: #f3f4f6; color: #374151; }

.message {
  color: #374151;
}

.hooks-info {
  margin-bottom: 30px;
}

.hooks-info h4 {
  margin-bottom: 15px;
  color: #065f46;
}

.hooks-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.hook-item {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.hook-item h5 {
  margin-top: 0;
  margin-bottom: 5px;
  color: #065f46;
}

.hook-item p {
  margin: 0;
  color: #047857;
  font-size: 14px;
}

.info-box {
  background: #dcfce7;
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

## 📦 Directive dengan Modifiers

### Click Outside Directive dengan Modifiers

```vue
<script setup>
import { ref } from 'vue'

const isModalOpen = ref(false)
const isDropdownOpen = ref(false)
const clickCount = ref(0)

// Click outside directive dengan modifiers
const vClickOutside = {
  mounted(el, binding) {
    el._clickOutsideHandler = (event) => {
      // Check if click is outside the element
      if (!(el === event.target || el.contains(event.target))) {
        // Check modifiers
        const { modifiers } = binding

        // .prevent modifier
        if (modifiers.prevent) {
          event.preventDefault()
        }

        // .stop modifier
        if (modifiers.stop) {
          event.stopPropagation()
        }

        // Call the handler function
        binding.value(event)
      }
    }

    document.addEventListener('click', el._clickOutsideHandler)
  },

  unmounted(el) {
    document.removeEventListener('click', el._clickOutsideHandler)
  }
}

const openModal = () => {
  isModalOpen.value = true
  clickCount.value = 0
}

const closeModal = () => {
  isModalOpen.value = false
}

const openDropdown = () => {
  isDropdownOpen.value = true
}

const closeDropdown = () => {
  isDropdownOpen.value = false
}

const handleModalClick = () => {
  clickCount.value++
}
</script>

<template>
  <div class="modifier-demo">
    <h2>Directives with Modifiers</h2>

    <div class="example-section">
      <h3>Click Outside with Modifiers:</h3>

      <div class="demo-grid">
        <!-- Modal Example -->
        <div class="demo-item">
          <h4>Modal (with .prevent modifier)</h4>
          <button @click="openModal">Open Modal</button>

          <div v-if="isModalOpen" v-click-outside.prevent="closeModal" class="modal">
            <h3>Modal Dialog</h3>
            <p>Click outside to close (default behavior prevented)</p>
            <p>Click count inside: {{ clickCount }}</p>
            <button @click="handleModalClick">Click Me</button>
            <button @click="closeModal">Close</button>
          </div>
        </div>

        <!-- Dropdown Example -->
        <div class="demo-item">
          <h4>Dropdown (without modifier)</h4>
          <button @click="openDropdown">Toggle Dropdown</button>

          <div v-if="isDropdownOpen" v-click-outside="closeDropdown" class="dropdown">
            <p>Dropdown content</p>
            <a href="#option1">Option 1</a>
            <a href="#option2">Option 2</a>
            <a href="#option3">Option 3</a>
          </div>
        </div>
      </div>
    </div>

    <div class="syntax-explanation">
      <h4>📝 Modifier Syntax:</h4>
      <div class="syntax-examples">
        <div class="syntax-item">
          <code>v-directive.modifier="handler"</code>
          <p>Gunakan single modifier</p>
        </div>
        <div class="syntax-item">
          <code>v-directive.modifier1.modifier2="handler"</code>
          <p>Gunakan multiple modifiers</p>
        </div>
        <div class="syntax-item">
          <code>binding.modifiers</code>
          <p>Access modifiers in directive hook</p>
        </div>
      </div>
    </div>

    <div class="common-modifiers">
      <h4>🔧 Common Modifiers:</h4>
      <div class="modifier-list">
        <div class="modifier">
          <code>.prevent</code>
          <span>Call event.preventDefault()</span>
        </div>
        <div class="modifier">
          <code>.stop</code>
          <span>Call event.stopPropagation()</span>
        </div>
        <div class="modifier">
          <code>.once</code>
          <span>Execute only once</span>
        </div>
        <div class="modifier">
          <code>.capture</code>
          <span>Use capture mode</span>
        </div>
        <div class="modifier">
          <code>.passive</code>
          <span>Use passive listener</span>
        </div>
        <div class="modifier">
          <code>.self</code>
          <span>Only trigger if element itself</span>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>💡 Custom Modifiers:</h4>
      <p>Anda juga bisa membuat custom modifiers:</p>
      <ul>
        <li>Access via <code>binding.modifiers.customModifier</code></li>
        <li>Check existence dengan <code>if (binding.modifiers.customModifier)</code></li>
        <li>Pass data via modifier: <code>v-directive.modifier.value="handler"</code></li>
        <li>Great for directive variations and options</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.modifier-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #6366f1;
  border-radius: 8px;
}

.example-section {
  margin-bottom: 30px;
}

.example-section h3 {
  margin-bottom: 20px;
  color: #4338ca;
}

.demo-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
}

.demo-item {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.demo-item h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #4338ca;
}

.modal {
  margin-top: 15px;
  padding: 20px;
  background: white;
  border: 2px solid #6366f1;
  border-radius: 8px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.1);
}

.modal h3 {
  margin-top: 0;
  color: #4338ca;
}

.modal button {
  margin: 5px;
  padding: 5px 10px;
  background: #6366f1;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.dropdown {
  margin-top: 15px;
  padding: 15px;
  background: white;
  border: 2px solid #6366f1;
  border-radius: 8px;
}

.dropdown a {
  display: block;
  padding: 5px 0;
  color: #4338ca;
  text-decoration: none;
}

.dropdown a:hover {
  text-decoration: underline;
}

.syntax-explanation {
  margin-bottom: 30px;
}

.syntax-explanation h4 {
  color: #4338ca;
  margin-bottom: 15px;
}

.syntax-examples {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #6366f1;
}

.syntax-item {
  margin-bottom: 15px;
}

.syntax-item:last-child {
  margin-bottom: 0;
}

.syntax-item code {
  display: block;
  background: #1e293b;
  color: #e2e8f0;
  padding: 10px;
  border-radius: 4px;
  font-family: monospace;
  margin-bottom: 5px;
}

.syntax-item p {
  margin: 0;
  color: #475569;
  font-size: 14px;
}

.common-modifiers {
  margin-bottom: 30px;
}

.common-modifiers h4 {
  color: #4338ca;
  margin-bottom: 15px;
}

.modifier-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.modifier {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background: #f8fafc;
  border-radius: 6px;
  border-left: 3px solid #6366f1;
}

.modifier code {
  background: #e0e7ff;
  color: #4338ca;
  padding: 3px 8px;
  border-radius: 4px;
  font-family: monospace;
  font-size: 12px;
  font-weight: bold;
}

.modifier span {
  color: #475569;
  font-size: 14px;
}

.info-box {
  background: #e0e7ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #6366f1;
}

.info-box h4 {
  margin-top: 0;
  color: #4338ca;
}

.info-box p {
  margin: 10px 0;
  color: #4338ca;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #3730a3;
}

.info-box code {
  background: #c7d2fe;
  color: #4338ca;
  padding: 2px 6px;
  border-radius: 3px;
  font-family: monospace;
  font-size: 14px;
}
</style>
```

## 🚀 Best Practices

### 1. Gunakan Directives untuk DOM Manipulation

```vue
<script setup>
// ✅ Good: DOM manipulation
const vHighlight = {
  mounted(el, binding) {
    el.style.backgroundColor = binding.value || 'yellow'
  }
}

// ❌ Bad: Complex logic dalam directive
const vComplexLogic = {
  mounted(el, binding) {
    // Don't put API calls or complex logic here
    fetch('/api/data').then(...) // Bad!
  }
}
</script>
```

### 2. Clean Up Event Listeners

```vue
<script setup>
// ✅ Good: Proper cleanup
const vClickOutside = {
  mounted(el, binding) {
    el._handler = (event) => {
      if (!el.contains(event.target)) {
        binding.value(event)
      }
    }
    document.addEventListener('click', el._handler)
  },
  unmounted(el) {
    document.removeEventListener('click', el._handler)
    delete el._handler
  }
}
</script>
```

### 3. Use Appropriate Hooks

```vue
<script setup>
// ✅ Good: Use right hooks
const vDirective = {
  created(el, binding) {
    // Non-DOM setup
    el.dataset.initialized = 'true'
  },
  mounted(el, binding) {
    // DOM manipulation
    el.style.color = 'red'
  },
  unmounted(el) {
    // Cleanup
    delete el.dataset.initialized
  }
}
</script>
```

### 4. Keep Directives Simple

```vue
<script setup>
// ✅ Good: Single responsibility
const vHighlight = {
  mounted(el, binding) {
    el.style.backgroundColor = binding.value
  }
}

// ✅ Good: Specific purpose
const vAutoFocus = {
  mounted(el) {
    el.focus()
  }
}
</script>
```

## 💡 Tips dan Trik

### 1. Directive Composition

```vue
<script setup>
// Compose multiple directives
const vCombined = {
  mounted(el, binding) {
    // Auto focus
    el.focus()

    // Highlight
    el.style.backgroundColor = binding.value.color || 'yellow'

    // Add border
    el.style.border = `2px solid ${binding.value.borderColor || 'blue'}`
  }
}
</script>
```

### 2. Directive dengan Default Values

```vue
<script setup>
const vFormat = {
  mounted(el, binding) => {
    const options = {
      color: 'black',
      fontSize: '16px',
      fontWeight: 'normal',
      ...binding.value // Override with provided values
    }

    Object.assign(el.style, options)
  }
}
</script>

<template>
  <div v-format="{ color: 'red', fontSize: '20px' }">Formatted text</div>
  <div v-format>Default formatting</div>
</template>
```

### 3. Directive untuk Third-party Libraries

```vue
<script setup>
// Integration dengan external library
const vTooltip = {
  mounted(el, binding) {
    // Initialize tooltip library
    new Tooltip(el, {
      title: binding.value,
      placement: 'top'
    })
  },
  unmounted(el) {
    // Cleanup
    if (el._tooltip) {
      el._tooltip.destroy()
    }
  }
}
</script>
```

---

## 🎉 Kesimpulan

Custom Directives adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🎯 DOM Manipulation:** Manipulasi DOM secara langsung dan efisien
2. **🔄 Reusability:** Logic yang dapat digunakan kembali di multiple elements
3. **⚡ Performance:** Optimal untuk manipulasi DOM yang kompleks
4. **🎨 Styling Logic:** Mengelola styling dan animations yang kompleks
5. **📱 Integration:** Integrasi dengan library JavaScript pihak ketiga

### Poin Penting:

- Directive digunakan untuk DOM manipulation langsung
- Gunakan hooks yang tepat (mounted, updated, unmounted)
- Selalu cleanup event listeners di unmounted hook
- Keep directives single responsibility
- Gunakan arguments dan modifiers untuk flexibility

### Kapan Menggunakan Custom Directives:

- ✅ DOM manipulation yang kompleks
- ✅ Styling logic yang reusable
- ✅ Integrasi dengan third-party libraries
- ✅ Performance optimizations
- ✅ Animations dan transitions

### Kapan Menghindari Directives:

- ❌ Complex business logic
- ❌ API calls atau data fetching
- ❌ Simple CSS changes (gunakan CSS classes)
- ❌ Logic yang bisa di-handle dengan built-in directives

### Pertanyaan Tambahan:

#### **Bagaimana Cara Penggunaannya?**

1. **Definisi:** Buat directive object dengan hooks yang diperlukan
2. **Registrasi:** Registrasi secara global (app.directive) atau local
3. **Penggunaan:** Gunakan di template dengan syntax `v-directive-name`
4. **Parameters:** Kirim values, arguments, dan modifiers sesuai kebutuhan

#### **Kapan Ini Cocok Digunakan?**

1. **✅ DOM Manipulation:** Ketika perlu manipulasi DOM secara langsung
2. **✅ Reusable Logic:** Ketika logic DOM digunakan di multiple places
3. **✅ Performance:** Untuk optimasi manipulasi DOM yang kompleks
4. **✅ Styling Logic:** Untuk styling dinamis yang kompleks
5. **✅ Third-party Integration:** Untuk integrasi library JavaScript

Dengan memahami custom directives, Anda sudah bisa membuat manipulasi DOM yang powerful dan reusable dalam aplikasi Vue.js! 🚀

---

**🎯 Ready for Practice?** Coba buat custom directives untuk berbagai use cases seperti animations, tooltips, focus management, dan lainnya!