# 🔄 Dynamic Component - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja dynamic components untuk merender komponen secara dinamis dan switch antar komponen dengan mudah dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Dynamic Component?](#-apa-itu-dynamic-component)
- [🎮 Cara Menggunakan Dynamic Component](#-cara-mengunakan-dynamic-component)
- [⚡ Basic Dynamic Component](#-basic-dynamic-component)
- [🔄 Component Switching](#-component-switching)
- [🎯 Advanced Patterns](#-advanced-patterns)
- [📦 Dynamic dengan Async Components](#-dynamic-dengan-async-components)
- [🔧 Performance Considerations](#-performance-considerations)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Dynamic Component?

**Dynamic Component** adalah fitur Vue.js yang memungkinkan kita untuk merender komponen yang berbeda secara dinamis berdasarkan data atau kondisi tertentu. Ini sangat berguna untuk membuat interfaces yang dapat berubah seperti tabs, modals, atau wizard forms.

### Konsep Dasar Dynamic Component

```vue
<template>
  <!-- Dynamic component dengan :is -->
  <component :is="currentComponent" />
</template>

<script setup>
import { ref } from 'vue'
import ComponentA from './ComponentA.vue'
import ComponentB from './ComponentB.vue'

const currentComponent = ref(ComponentA)
</script>
```

### Mengapa Dynamic Component Penting?

1. **🔄 Component Switching:** Mudah switch antar komponen
2. **📦 Code Organization:** Pisahkan logic ke komponen terpisah
3. **🎯 Conditional Rendering:** Render komponen berdasarkan kondisi
4. **🚀 Performance:** Load komponen hanya saat dibutuhkan
5. **📱 Flexible UI:** Membuat UI yang dinamis dan adaptif

### Static vs Dynamic Rendering

**❌ Static Rendering (Tanpa Dynamic Component):**
```vue
<template>
  <ComponentA v-if="tab === 'A'" />
  <ComponentB v-if="tab === 'B'" />
  <ComponentC v-if="tab === 'C'" />
</template>
```

**✅ Dynamic Rendering (Dengan Dynamic Component):**
```vue
<template>
  <component :is="currentComponent" />
</template>
```

## 🎮 Cara Menggunakan Dynamic Component

### 1. Import Komponen

```vue
<script setup>
import { ref } from 'vue'
import ComponentA from './ComponentA.vue'
import ComponentB from './ComponentB.vue'
</script>
```

### 2. Buat Object Mapping

```vue
<script setup>
const components = {
  ComponentA,
  ComponentB
}
</script>
```

### 3. Gunakan :is Directive

```vue
<template>
  <component :is="components[activeTab]" />
</template>
```

## ⚡ Basic Dynamic Component

### Tab Navigation Example

**Tanpa Dynamic Component:**
```vue
<!-- Without Dynamic Component - Repetitive v-if -->
<script setup>
import { ref } from 'vue'
import ComponentOne from './ComponentOne.vue'
import ComponentTwo from './ComponentTwo.vue'
import ComponentThree from './ComponentThree.vue'

const currentTab = ref('One')
</script>

<template>
  <!-- Tanpa Dynamic Component -->
  <button @click="currentTab = 'One'">One</button>
  <button @click="currentTab = 'Two'">Two</button>
  <button @click="currentTab = 'Three'">Three</button>

  <ComponentOne v-if="currentTab === 'One'" />
  <ComponentTwo v-if="currentTab === 'Two'" />
  <ComponentThree v-if="currentTab === 'Three'" />
</template>
```

**Dengan Dynamic Component:**
```vue
<!-- With Dynamic Component - Clean dan efisien -->
<script setup>
import { ref } from 'vue'
import ComponentOne from './ComponentOne.vue'
import ComponentTwo from './ComponentTwo.vue'
import ComponentThree from './ComponentThree.vue'

const currentTab = ref('ComponentOne')

const tabs = {
  ComponentOne: ComponentOne,
  ComponentTwo: ComponentTwo,
  ComponentThree: ComponentThree
}
</script>

<template>
  <!-- Dengan Dynamic Component -->
  <button @click="currentTab = 'ComponentOne'">What is HTML</button>
  <button @click="currentTab = 'ComponentTwo'">What is CSS</button>
  <button @click="currentTab = 'ComponentThree'">What is JS</button>

  <component :is="tabs[currentTab]" />
</template>
```

### Complete Dynamic Component Demo

```vue
<script setup>
import { ref, computed } from 'vue'

// Import all components
import HomePage from './components/HomePage.vue'
import AboutPage from './components/AboutPage.vue'
import ContactPage from './components/ContactPage.vue'
import SettingsPage from './components/SettingsPage.vue'

// Component mapping
const componentMap = {
  home: HomePage,
  about: AboutPage,
  contact: ContactPage,
  settings: SettingsPage
}

// Reactive state
const currentPage = ref('home')

// Computed properties
const currentComponent = computed(() => {
  return componentMap[currentPage.value]
})

const navigationItems = computed(() => {
  return Object.keys(componentMap).map(key => ({
    key,
    label: key.charAt(0).toUpperCase() + key.slice(1),
    icon: getIcon(key)
  }))
})

// Methods
const navigateTo = (page) => {
  currentPage.value = page
}

const getIcon = (page) => {
  const icons = {
    home: '🏠',
    about: 'ℹ️',
    contact: '📧',
    settings: '⚙️'
  }
  return icons[page] || '📄'
}

// Track navigation history
const navigationHistory = ref([])
const addToHistory = (page) => {
  navigationHistory.value.unshift({
    page,
    timestamp: new Date().toLocaleTimeString()
  })
  // Keep only last 10 entries
  if (navigationHistory.value.length > 10) {
    navigationHistory.value = navigationHistory.value.slice(0, 10)
  }
}

// Watch for navigation changes
watch(currentPage, (newPage) => {
  addToHistory(newPage)
})
</script>

<template>
  <div class="dynamic-demo">
    <h2>Dynamic Component Navigation</h2>

    <!-- Navigation -->
    <nav class="navigation">
      <div class="nav-items">
        <button
          v-for="item in navigationItems"
          :key="item.key"
          @click="navigateTo(item.key)"
          :class="{ active: currentPage === item.key }"
          class="nav-button"
        >
          <span class="icon">{{ item.icon }}</span>
          <span class="label">{{ item.label }}</span>
        </button>
      </div>

      <div class="current-page">
        Current: <strong>{{ currentPage }}</strong>
      </div>
    </nav>

    <!-- Dynamic Component Area -->
    <div class="component-container">
      <Transition name="fade" mode="out-in">
        <component :is="currentComponent" />
      </Transition>
    </div>

    <!-- Navigation History -->
    <div class="history-section">
      <h4>Navigation History:</h4>
      <div v-if="navigationHistory.length === 0" class="no-history">
        No navigation yet
      </div>
      <div v-else class="history-list">
        <div
          v-for="(item, index) in navigationHistory"
          :key="index"
          class="history-item"
        >
          <span class="icon">{{ getIcon(item.page) }}</span>
          <span class="page">{{ item.label }}</span>
          <span class="time">{{ item.timestamp }}</span>
        </div>
      </div>
    </div>

    <div class="comparison">
      <h3>Static vs Dynamic Rendering</h3>
      <div class="comparison-grid">
        <div class="comparison-item">
          <h4>❌ Static (v-if)</h4>
          <pre><code>&lt;ComponentA v-if="tab === 'A'" /&gt;
&lt;ComponentB v-if="tab === 'B'" /&gt;
&lt;ComponentC v-if="tab === 'C'" /&gt;</code></pre>
          <ul>
            <li>Repetitive code</li>
            <li>Hard to maintain</li>
            <li>Must import all components</li>
            <li>Poor scalability</li>
          </ul>
        </div>

        <div class="comparison-item">
          <h4>✅ Dynamic (component :is)</h4>
          <pre><code>&lt;component :is="components[tab]" /&gt;</code></pre>
          <ul>
            <li>Clean code</li>
            <li>Easy to maintain</li>
            <li>Flexible mapping</li>
            <li>Better scalability</li>
          </ul>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>💡 Dynamic Component Benefits:</h4>
      <ul>
        <li><strong>Clean Code:</strong> Menghindari repetitive v-if statements</li>
        <li><strong>Flexibility:</strong> Mudah menambah/mengurangi komponen</li>
        <li><strong>Maintainability:</strong> Centralized component mapping</li>
        <li><strong>Performance:</strong> Component lifecycle management</li>
        <li><strong>Type Safety:</strong> Better TypeScript support</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.dynamic-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.navigation {
  margin-bottom: 30px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
  border: 1px solid #bfdbfe;
}

.nav-items {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.nav-button {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  background: white;
  border: 2px solid #dbeafe;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.nav-button:hover {
  border-color: #3b82f6;
  background: #eff6ff;
}

.nav-button.active {
  border-color: #3b82f6;
  background: #3b82f6;
  color: white;
}

.icon {
  font-size: 18px;
}

.label {
  font-weight: 500;
}

.current-page {
  text-align: center;
  color: #1e40af;
  font-weight: 500;
}

.component-container {
  margin-bottom: 30px;
  min-height: 300px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

/* Transition animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.history-section {
  margin-bottom: 30px;
}

.history-section h4 {
  margin-bottom: 15px;
  color: #374151;
}

.no-history {
  text-align: center;
  padding: 20px;
  color: #9ca3af;
  font-style: italic;
  background: #f8fafc;
  border-radius: 8px;
}

.history-list {
  background: #f8fafc;
  border-radius: 8px;
  padding: 15px;
  max-height: 200px;
  overflow-y: auto;
}

.history-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px;
  margin-bottom: 5px;
  background: white;
  border-radius: 6px;
  border-left: 3px solid #3b82f6;
}

.history-item:last-child {
  margin-bottom: 0;
}

.history-item .icon {
  font-size: 16px;
}

.history-item .page {
  flex: 1;
  font-weight: 500;
  color: #374151;
}

.history-item .time {
  font-size: 12px;
  color: #6b7280;
  font-family: monospace;
}

.comparison {
  margin-bottom: 30px;
}

.comparison h3 {
  text-align: center;
  color: #374151;
  margin-bottom: 20px;
}

.comparison-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.comparison-item {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.comparison-item h4 {
  margin-top: 0;
  margin-bottom: 15px;
  text-align: center;
}

.comparison-item pre {
  background: #1e293b;
  color: #e2e8f0;
  padding: 15px;
  border-radius: 6px;
  font-family: monospace;
  font-size: 12px;
  overflow-x: auto;
  margin-bottom: 15px;
}

.comparison-item ul {
  margin: 0;
  padding-left: 20px;
}

.comparison-item li {
  margin: 5px 0;
  color: #374151;
}

.info-box {
  background: #dbeafe;
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.info-box h4 {
  margin-top: 0;
  color: #1e40af;
  margin-bottom: 15px;
}

.info-box ul {
  margin: 15px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 8px 0;
  color: #1e3a8a;
}

.info-box strong {
  color: #1e40af;
}
</style>
```

## 🔄 Component Switching

### Conditional Component Based on State

```vue
<script setup>
import { ref, computed } from 'vue'

// Import different components
import LoadingComponent from './components/LoadingComponent.vue'
import ErrorComponent from './components/ErrorComponent.vue'
import SuccessComponent from './components/SuccessComponent.vue'
import EmptyComponent from './components/EmptyComponent.vue'

// State management
const loadingState = ref('loading') // loading, error, success, empty
const data = ref(null)
const errorMessage = ref('')

// Simulate async operation
const loadData = async () => {
  loadingState.value = 'loading'
  data.value = null
  errorMessage.value = ''

  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 2000))

    // Simulate random success/error
    if (Math.random() > 0.3) {
      data.value = { message: 'Data loaded successfully!' }
      loadingState.value = 'success'
    } else {
      throw new Error('Failed to load data')
    }
  } catch (error) {
    errorMessage.value = error.message
    loadingState.value = 'error'
  }
}

// Component mapping based on state
const componentMap = {
  loading: LoadingComponent,
  error: ErrorComponent,
  success: SuccessComponent,
  empty: EmptyComponent
}

// Computed untuk current component
const currentComponent = computed(() => {
  return componentMap[loadingState.value]
})

// Reset function
const reset = () => {
  loadingState.value = 'empty'
  data.value = null
  errorMessage.value = ''
}
</script>

<template>
  <div class="state-switching-demo">
    <h2>State-based Component Switching</h2>

    <div class="controls">
      <button @click="loadData" :disabled="loadingState === 'loading'">
        {{ loadingState === 'loading' ? 'Loading...' : 'Load Data' }}
      </button>
      <button @click="reset">Reset</button>
    </div>

    <!-- Dynamic component based on state -->
    <div class="component-display">
      <Transition name="fade" mode="out-in">
        <component
          :is="currentComponent"
          :data="data"
          :error="errorMessage"
          @retry="loadData"
        />
      </Transition>
    </div>

    <div class="state-info">
      <h4>Current State: {{ loadingState }}</h4>
      <div class="state-grid">
        <div
          v-for="(component, state) in componentMap"
          :key="state"
          :class="{ active: loadingState === state }"
          class="state-item"
        >
          <span class="state-name">{{ state.toUpperCase() }}</span>
          <span class="component-name">{{ component.__name }}</span>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>🎯 State Management Pattern:</h4>
      <ul>
        <li><strong>Loading State:</strong> Show loading spinner/skeleton</li>
        <li><strong>Error State:</strong> Show error message with retry option</li>
        <li><strong>Success State:</strong> Show success message or data</li>
        <li><strong>Empty State:</strong> Show empty state message</li>
        <li><strong>Dynamic Switching:</strong> Components change based on state</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.state-switching-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  justify-content: center;
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

.controls button:hover:not(:disabled) {
  background: #059669;
}

.controls button:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.component-display {
  margin-bottom: 30px;
  min-height: 200px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.state-info h4 {
  margin-bottom: 15px;
  color: #065f46;
  text-align: center;
}

.state-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.state-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 15px;
  background: white;
  border: 2px solid #d1fae5;
  border-radius: 6px;
  transition: all 0.3s ease;
}

.state-item.active {
  border-color: #10b981;
  background: #d1fae5;
}

.state-name {
  font-weight: bold;
  color: #065f46;
}

.component-name {
  font-family: monospace;
  font-size: 12px;
  color: #047857;
}

.info-box {
  background: #d1fae5;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.info-box h4 {
  margin-top: 0;
  color: #065f46;
  margin-bottom: 10px;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #047857;
}

.info-box strong {
  color: #065f46;
}

/* Fade transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
```

## 🎯 Advanced Patterns

### Dynamic Component dengan Props Passing

```vue
<script setup>
import { ref, computed } from 'vue'

// Components dengan berbagai props
import UserProfile from './components/UserProfile.vue'
import ProductCard from './components/ProductCard.vue'
import NewsArticle from './components/NewsArticle.vue'

// Data untuk berbagai tipe content
const currentContentType = ref('user')
const activeItem = ref(null)

// Sample data
const userData = {
  name: 'John Doe',
  email: 'john@example.com',
  avatar: 'https://via.placeholder.com/100'
}

const productData = {
  id: 1,
  name: 'Awesome Product',
  price: 99.99,
  image: 'https://via.placeholder.com/200',
  description: 'This is an amazing product!'
}

const articleData = {
  id: 1,
  title: 'Breaking News',
  content: 'Lorem ipsum dolor sit amet...',
  author: 'Jane Smith',
  publishDate: new Date().toISOString()
}

// Component mapping
const componentMap = {
  user: UserProfile,
  product: ProductCard,
  article: NewsArticle
}

// Dynamic props untuk setiap komponen
const currentComponent = computed(() => {
  return componentMap[currentContentType.value]
})

const currentProps = computed(() => {
  switch (currentContentType.value) {
    case 'user':
      return { user: userData }
    case 'product':
      return { product: productData }
    case 'article':
      return { article: articleData }
    default:
      return {}
  }
})

// Methods
const switchContentType = (type) => {
  currentContentType.value = type
}

const handleItemClick = (item) => {
  activeItem.value = item
  console.log('Item clicked:', item)
}
</script>

<template>
  <div class="advanced-demo">
    <h2>Dynamic Component with Props</h2>

    <!-- Content Type Selector -->
    <div class="content-selector">
      <h4>Select Content Type:</h4>
      <div class="type-buttons">
        <button
          v-for="(_, type) in componentMap"
          :key="type"
          @click="switchContentType(type)"
          :class="{ active: currentContentType === type }"
          class="type-btn"
        >
          {{ type.charAt(0).toUpperCase() + type.slice(1) }}
        </button>
      </div>
    </div>

    <!-- Dynamic Component dengan Props -->
    <div class="component-display">
      <Transition name="slide" mode="out-in">
        <component
          :is="currentComponent"
          v-bind="currentProps"
          @item-click="handleItemClick"
        />
      </Transition>
    </div>

    <!-- Props Display -->
    <div class="props-display">
      <h4>Current Props:</h4>
      <pre><code>{{ JSON.stringify(currentProps, null, 2) }}</code></pre>
    </div>

    <div class="pattern-explanation">
      <h4>🔧 Advanced Pattern Features:</h4>
      <div class="pattern-grid">
        <div class="pattern-item">
          <h5>Dynamic Props</h5>
          <p>Setiap komponen menerima props yang berbeda</p>
        </div>
        <div class="pattern-item">
          <h5>Event Handling</h5>
          <p>Events dari komponen dinamis ditangani</p>
        </div>
        <div class="pattern-item">
          <h5>Type Safety</h5>
          <p>Pastikan props sesuai dengan komponen</p>
        </div>
        <div class="pattern-item">
          <h5>Transitions</h5>
          <p>Smooth transitions antar komponen</p>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>💡 Use Cases for Advanced Pattern:</h4>
      <ul>
        <li><strong>Content Management:</strong> Different content types (articles, products, users)</li>
        <li><strong>Form Wizards:</strong> Multi-step forms dengan komponen berbeda</li>
        <li><strong>Dashboard Widgets:</strong> Various widget types</li>
        <li><strong>Media Galleries:</strong> Different media display types</li>
        <li><strong>Data Visualization:</strong> Different chart types</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.advanced-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.content-selector {
  margin-bottom: 30px;
  padding: 20px;
  background: #faf5ff;
  border-radius: 8px;
}

.content-selector h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #6b21a8;
}

.type-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.type-btn {
  padding: 10px 20px;
  background: white;
  border: 2px solid #d8b4fe;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
  color: #6b21a8;
}

.type-btn:hover {
  background: #f3e8ff;
  border-color: #8b5cf6;
}

.type-btn.active {
  background: #8b5cf6;
  color: white;
  border-color: #8b5cf6;
}

.component-display {
  margin-bottom: 30px;
  min-height: 300px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.props-display {
  margin-bottom: 30px;
}

.props-display h4 {
  margin-bottom: 10px;
  color: #374151;
}

.props-display pre {
  background: #1e293b;
  color: #e2e8f0;
  padding: 15px;
  border-radius: 6px;
  font-family: monospace;
  font-size: 12px;
  overflow-x: auto;
}

.pattern-explanation {
  margin-bottom: 30px;
}

.pattern-explanation h4 {
  margin-bottom: 15px;
  color: #6b21a8;
}

.pattern-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.pattern-item {
  background: #f3e8ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.pattern-item h5 {
  margin-top: 0;
  margin-bottom: 8px;
  color: #6b21a8;
}

.pattern-item p {
  margin: 0;
  color: #6b21a8;
  font-size: 14px;
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
  margin-bottom: 10px;
}

.info-box ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #6b21a8;
}

.info-box strong {
  color: #6b21a8;
}

/* Slide transition */
.slide-enter-active,
.slide-leave-active {
  transition: all 0.3s ease;
}

.slide-enter-from {
  opacity: 0;
  transform: translateX(-20px);
}

.slide-leave-to {
  opacity: 0;
  transform: translateX(20px);
}
</style>
```

### Dynamic Component dengan Keep-Alive

```vue
<script setup>
import { ref, computed } from 'vue'

// Import components
import FormStep1 from './components/FormStep1.vue'
import FormStep2 from './components/FormStep2.vue'
import FormStep3 from './components/FormStep3.vue'
import FormComplete from './components/FormComplete.vue'

// State management
const currentStep = ref(1)
const formData = ref({
  name: '',
  email: '',
  phone: '',
  address: '',
  preferences: []
})

const totalSteps = 4

// Component mapping
const stepComponents = {
  1: FormStep1,
  2: FormStep2,
  3: FormStep3,
  4: FormComplete
}

// Computed properties
const currentComponent = computed(() => {
  return stepComponents[currentStep.value]
})

const canGoNext = computed(() => {
  // Validate current step
  switch (currentStep.value) {
    case 1:
      return formData.value.name && formData.value.email
    case 2:
      return formData.value.phone
    case 3:
      return formData.value.address
    default:
      return false
  }
})

const canGoBack = computed(() => {
  return currentStep.value > 1
})

// Methods
const nextStep = () => {
  if (canGoNext.value && currentStep.value < totalSteps) {
    currentStep.value++
  }
}

const prevStep = () => {
  if (canGoBack.value) {
    currentStep.value--
  }
}

const resetForm = () => {
  currentStep.value = 1
  formData.value = {
    name: '',
    email: '',
    phone: '',
    address: '',
    preferences: []
  }
}

const submitForm = () => {
  currentStep.value = 4 // Complete step
}

// Update form data
const updateFormData = (data) => {
  Object.assign(formData.value, data)
}
</script>

<template>
  <div class="wizard-demo">
    <h2>Dynamic Component with Keep-Alive</h2>

    <!-- Progress Bar -->
    <div class="progress-bar">
      <div class="steps">
        <div
          v-for="step in totalSteps"
          :key="step"
          :class="{ active: currentStep === step, completed: currentStep > step }"
          class="step"
        >
          <span class="step-number">{{ step }}</span>
          <span class="step-label">Step {{ step }}</span>
        </div>
      </div>
      <div class="progress-line" :style="{ width: (currentStep - 1) / (totalSteps - 1) * 100 + '%' }"></div>
    </div>

    <!-- Dynamic Component dengan Keep-Alive -->
    <div class="form-container">
      <KeepAlive>
        <component
          :is="currentComponent"
          :form-data="formData"
          @update="updateFormData"
          @submit="submitForm"
        />
      </KeepAlive>
    </div>

    <!-- Navigation Buttons -->
    <div class="navigation-buttons">
      <button
        @click="prevStep"
        :disabled="!canGoBack"
        class="nav-btn back-btn"
      >
        ← Previous
      </button>

      <button
        @click="resetForm"
        class="nav-btn reset-btn"
      >
        Reset
      </button>

      <button
        @click="nextStep"
        :disabled="!canGoNext"
        class="nav-btn next-btn"
      >
        {{ currentStep === totalSteps ? 'Complete' : 'Next →' }}
      </button>
    </div>

    <!-- Form Data Display -->
    <div class="form-data-display">
      <h4>Form Data Preview:</h4>
      <pre><code>{{ JSON.stringify(formData, null, 2) }}</code></pre>
    </div>

    <div class="keep-alive-info">
      <h4>🔄 Keep-Alive Benefits:</h4>
      <ul>
        <li><strong>State Preservation:</strong> Form data preserved between steps</li>
        <li><strong>No Re-rendering:</strong> Components maintain their state</li>
        <li><strong>Performance:</strong> Better performance for complex forms</li>
        <li><strong>User Experience:</strong> No data loss when switching back</li>
        <li><strong>Form Validation:</strong> Validation state maintained</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.wizard-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #ef4444;
  border-radius: 8px;
}

.progress-bar {
  margin-bottom: 30px;
  position: relative;
}

.steps {
  display: flex;
  justify-content: space-between;
  position: relative;
  z-index: 2;
}

.step {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  width: 80px;
}

.step-number {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background: #e5e7eb;
  color: #6b7280;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 14px;
  margin-bottom: 5px;
  transition: all 0.3s ease;
}

.step-label {
  font-size: 12px;
  color: #6b7280;
  transition: all 0.3s ease;
}

.step.active .step-number {
  background: #ef4444;
  color: white;
}

.step.active .step-label {
  color: #ef4444;
  font-weight: bold;
}

.step.completed .step-number {
  background: #10b981;
  color: white;
}

.step.completed .step-label {
  color: #10b981;
}

.progress-line {
  position: absolute;
  top: 15px;
  left: 40px;
  right: 40px;
  height: 2px;
  background: #e5e7eb;
  z-index: 1;
}

.form-container {
  margin-bottom: 30px;
  min-height: 300px;
  padding: 20px;
  background: #fef2f2;
  border-radius: 8px;
  border: 1px solid #fecaca;
}

.navigation-buttons {
  display: flex;
  justify-content: space-between;
  margin-bottom: 30px;
}

.nav-btn {
  padding: 10px 20px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
}

.back-btn {
  background: #6b7280;
  color: white;
}

.back-btn:hover:not(:disabled) {
  background: #4b5563;
}

.reset-btn {
  background: #f59e0b;
  color: white;
}

.reset-btn:hover {
  background: #d97706;
}

.next-btn {
  background: #10b981;
  color: white;
}

.next-btn:hover:not(:disabled) {
  background: #059669;
}

.nav-btn:disabled {
  background: #d1d5db;
  cursor: not-allowed;
}

.form-data-display {
  margin-bottom: 30px;
}

.form-data-display h4 {
  margin-bottom: 10px;
  color: #374151;
}

.form-data-display pre {
  background: #1e293b;
  color: #e2e8f0;
  padding: 15px;
  border-radius: 6px;
  font-family: monospace;
  font-size: 12px;
  overflow-x: auto;
}

.keep-alive-info {
  background: #fef2f2;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ef4444;
}

.keep-alive-info h4 {
  margin-top: 0;
  color: #991b1b;
  margin-bottom: 10px;
}

.keep-alive-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.keep-alive-info li {
  margin: 5px 0;
  color: #7f1d1d;
}

.keep-alive-info strong {
  color: #991b1b;
}
</style>
```

## 📦 Dynamic dengan Async Components

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Async components untuk lazy loading
const LazyComponentA = defineAsyncComponent(() =>
  import('./components/LazyComponentA.vue')
)

const LazyComponentB = defineAsyncComponent(() =>
  import('./components/LazyComponentB.vue')
)

const LazyComponentC = defineAsyncComponent(() =>
  import('./components/LazyComponentC.vue')
)

// Regular components
import RegularComponentA from './components/RegularComponentA.vue'
import RegularComponentB from './components/RegularComponentB.vue'

// State
const useAsync = ref(true)
const currentType = ref('A')
const loadingTime = ref(0)

// Component mapping
const asyncComponents = {
  A: LazyComponentA,
  B: LazyComponentB,
  C: LazyComponentC
}

const regularComponents = {
  A: RegularComponentA,
  B: RegularComponentB
}

// Computed untuk memilih component
const currentComponent = computed(() => {
  const components = useAsync.value ? asyncComponents : regularComponents
  return components[currentType.value]
})

// Methods
const toggleAsyncMode = () => {
  useAsync.value = !useAsync.value
}

const switchComponent = (type) => {
  const startTime = performance.now()
  currentType.value = type

  // Simulate loading time
  if (useAsync.value) {
    setTimeout(() => {
      loadingTime.value = performance.now() - startTime
    }, 500)
  }
}
</script>

<template>
  <div class="async-demo">
    <h2>Dynamic Components with Async Loading</h2>

    <!-- Controls -->
    <div class="controls">
      <div class="async-toggle">
        <label>
          <input
            v-model="useAsync"
            type="checkbox"
          />
          Use Async Components
        </label>
      </div>

      <div class="component-selector">
        <h4>Select Component:</h4>
        <div class="component-buttons">
          <button
            @click="switchComponent('A')"
            :class="{ active: currentType === 'A' }"
            class="comp-btn"
          >
            Component A
          </button>
          <button
            @click="switchComponent('B')"
            :class="{ active: currentType === 'B' }"
            class="comp-btn"
          >
            Component B
          </button>
          <button
            v-if="useAsync"
            @click="switchComponent('C')"
            :class="{ active: currentType === 'C' }"
            class="comp-btn"
          >
            Component C
          </button>
        </div>
      </div>
    </div>

    <!-- Dynamic Component dengan Suspense -->
    <div class="component-display">
      <Suspense>
        <template #default>
          <component :is="currentComponent" />
        </template>

        <template #fallback>
          <div class="loading-state">
            <div class="loading-spinner"></div>
            <p>Loading {{ useAsync ? 'async' : 'regular' }} component...</p>
          </div>
        </template>
      </Suspense>
    </div>

    <!-- Performance Metrics -->
    <div class="metrics">
      <h4>Performance Metrics:</h4>
      <div class="metric-grid">
        <div class="metric-item">
          <span class="metric-label">Mode:</span>
          <span class="metric-value">{{ useAsync ? 'Async' : 'Regular' }}</span>
        </div>
        <div class="metric-item">
          <span class="metric-label">Component:</span>
          <span class="metric-value">{{ currentType }}</span>
        </div>
        <div class="metric-item" v-if="loadingTime > 0">
          <span class="metric-label">Load Time:</span>
          <span class="metric-value">{{ loadingTime.toFixed(2) }}ms</span>
        </div>
      </div>
    </div>

    <!-- Comparison -->
    <div class="comparison">
      <h3>Async vs Regular Components</h3>
      <div class="comparison-table">
        <div class="table-header">
          <div class="header-cell">Feature</div>
          <div class="header-cell">Regular</div>
          <div class="header-cell">Async</div>
        </div>
        <div class="table-row">
          <div class="cell">Initial Load</div>
          <div class="cell">Immediate</div>
          <div class="cell">On Demand</div>
        </div>
        <div class="table-row">
          <div class="cell">Bundle Size</div>
          <div class="cell">Larger</div>
          <div class="cell">Smaller</div>
        </div>
        <div class="table-row">
          <div class="cell">Memory Usage</div>
          <div class="cell">Higher</div>
          <div class="cell">Optimal</div>
        </div>
        <div class="table-row">
          <div class="cell">User Experience</div>
          <div class="cell">Instant</div>
          <div class="cell">Loading Required</div>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>💡 When to Use Each Approach:</h4>
      <div class="use-cases">
        <div class="use-case">
          <h5>Regular Components:</h5>
          <ul>
            <li>Critical above-the-fold content</li>
            <li>Small, frequently used components</li>
            <li>Components with minimal dependencies</li>
            <li>Components needed immediately</li>
          </ul>
        </div>
        <div class="use-case">
          <h5>Async Components:</h5>
          <ul>
            <li>Large, complex components</li>
            <li>Below-the-fold content</li>
            <li>Components with heavy dependencies</li>
            <li>Components accessed conditionally</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.async-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.controls {
  margin-bottom: 30px;
}

.async-toggle {
  margin-bottom: 20px;
}

.async-toggle label {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: 500;
  color: #075985;
}

.async-toggle input[type="checkbox"] {
  width: 20px;
  height: 20px;
}

.component-selector h4 {
  margin-bottom: 10px;
  color: #0c4a6e;
}

.component-buttons {
  display: flex;
  gap: 10px;
}

.comp-btn {
  padding: 10px 20px;
  background: white;
  border: 2px solid #bae6fd;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
  color: #0c4a6e;
}

.comp-btn:hover {
  background: #f0f9ff;
  border-color: #06b6d4;
}

.comp-btn.active {
  background: #06b6d4;
  color: white;
  border-color: #06b6d4;
}

.component-display {
  margin-bottom: 30px;
  min-height: 200px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
  border: 1px solid #bae6fd;
}

.loading-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 150px;
  color: #0c4a6e;
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e0f2fe;
  border-top: 4px solid #06b6d4;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.metrics {
  margin-bottom: 30px;
}

.metrics h4 {
  margin-bottom: 15px;
  color: #0c4a6e;
}

.metric-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
}

.metric-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  background: white;
  border-radius: 6px;
  border: 1px solid #bae6fd;
}

.metric-label {
  font-weight: 500;
  color: #0c4a6e;
}

.metric-value {
  font-weight: bold;
  color: #06b6d4;
}

.comparison {
  margin-bottom: 30px;
}

.comparison h3 {
  text-align: center;
  color: #0c4a6e;
  margin-bottom: 20px;
}

.comparison-table {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.table-header {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  background: #06b6d4;
  color: white;
  font-weight: bold;
}

.header-cell {
  padding: 12px;
  text-align: center;
}

.table-row {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  border-bottom: 1px solid #e2e8f0;
}

.table-row:last-child {
  border-bottom: none;
}

.cell {
  padding: 12px;
  text-align: center;
  border-right: 1px solid #e2e8f0;
}

.cell:last-child {
  border-right: none;
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
  margin-bottom: 15px;
}

.use-cases {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.use-case h5 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #0c4a6e;
}

.use-case ul {
  margin: 0;
  padding-left: 20px;
}

.use-case li {
  margin: 5px 0;
  color: #075985;
}
</style>
```

## 🔧 Performance Considerations

### Performance Best Practices

```vue
<script setup>
import { ref, shallowRef } from 'vue'

// ✅ Good: Use shallowRef untuk component yang tidak re-render sering
const heavyComponent = shallowRef(null)

// ✅ Good: Cache component definitions
const componentCache = new Map()

const getComponent = (name) => {
  if (!componentCache.has(name)) {
    componentCache.set(name, () => import(`./components/${name}.vue`))
  }
  return componentCache.get(name)
}

// ✅ Good: Conditional component loading
const shouldLoadHeavyComponent = ref(false)

const dynamicComponent = computed(() => {
  if (shouldLoadHeavyComponent.value) {
    return heavyComponent.value
  }
  return LightweightComponent
})

// ✅ Good: Preload critical components
const preloadCriticalComponents = async () => {
  const criticalComponents = ['Header', 'Navigation', 'Footer']

  await Promise.all(
    criticalComponents.map(name => import(`./components/${name}.vue`))
  )
}
</script>
```

### Memory Management

```vue
<script setup>
import { ref, onUnmounted } from 'vue'

// ✅ Good: Cleanup unused components
const currentComponent = ref(null)
const componentRefs = ref(new Map())

const switchComponent = async (componentName) => {
  // Cleanup previous component
  if (currentComponent.value) {
    // Trigger cleanup if component has cleanup method
    if (componentRefs.value.has(currentComponent.value)) {
      const prevComponent = componentRefs.value.get(currentComponent.value)
      if (prevComponent.cleanup) {
        await prevComponent.cleanup()
      }
    }
  }

  // Load new component
  try {
    const componentModule = await import(`./components/${componentName}.vue`)
    currentComponent.value = componentModule.default
    componentRefs.value.set(componentName, componentModule)
  } catch (error) {
    console.error(`Failed to load component ${componentName}:`, error)
  }
}

// Cleanup on unmount
onUnmounted(() => {
  componentRefs.value.clear()
})
</script>
```

## 🚀 Best Practices

### 1. Use Component Mapping Object

```vue
<script setup>
// ✅ Good: Centralized component mapping
const components = {
  home: () => import('./components/HomePage.vue'),
  about: () => import('./components/AboutPage.vue'),
  contact: () => import('./components/ContactPage.vue')
}

const currentComponent = computed(() => {
  const loader = components[currentPage.value]
  return defineAsyncComponent(loader)
})

// ❌ Bad: Hardcoded imports
import HomePage from './components/HomePage.vue'
import AboutPage from './components/AboutPage.vue'
import ContactPage from './components/ContactPage.vue'

const currentComponent = computed(() => {
  switch (currentPage.value) {
    case 'home': return HomePage
    case 'about': return AboutPage
    case 'contact': return ContactPage
  }
})
</script>
```

### 2. Use Keep-Alive untuk State Preservation

```vue
<template>
  <!-- ✅ Good: Preserve component state -->
  <KeepAlive>
    <component :is="currentComponent" />
  </KeepAlive>

  <!-- ❌ Bad: Component loses state on switch -->
  <component :is="currentComponent" />
</template>
```

### 3. Handle Loading States

```vue
<template>
  <!-- ✅ Good: Handle loading with Suspense -->
  <Suspense>
    <template #default>
      <component :is="currentComponent" />
    </template>
    <template #fallback>
      <LoadingSpinner />
    </template>
  </Suspense>
</template>
```

### 4. Use Appropriate Transitions

```vue
<template>
  <!-- ✅ Good: Smooth transitions -->
  <Transition name="fade" mode="out-in">
    <component :is="currentComponent" />
  </Transition>

  <!-- ❌ Bad: Abrupt changes -->
  <component :is="currentComponent" />
</template>
```

## 💡 Tips dan Trik

### 1. Dynamic Component Registry

```vue
<script setup>
import { ref } from 'vue'

// Dynamic component registry
const componentRegistry = ref(new Map())

const registerComponent = (name, loader) => {
  componentRegistry.value.set(name, loader)
}

const unregisterComponent = (name) => {
  componentRegistry.value.delete(name)
}

const getComponent = (name) => {
  return componentRegistry.value.get(name)
}

// Usage
registerComponent('custom', () => import('./CustomComponent.vue'))
</script>
```

### 2. Component Factory Pattern

```vue
<script setup>
import { ref } from 'vue'

const createComponent = (type) => {
  return defineAsyncComponent(() =>
    import(`./components/${type}.vue`)
  )
}

// Dynamic component creation
const components = ref({
  chart: createComponent('Chart'),
  table: createComponent('Table'),
  form: createComponent('Form')
})
</script>
```

### 3. Component Pool for Performance

```vue
<script setup>
import { ref } from 'vue'

class ComponentPool {
  constructor(createFn, maxSize = 5) {
    this.createFn = createFn
    this.maxSize = maxSize
    this.pool = []
    this.active = new Set()
  }

  async get() {
    if (this.pool.length > 0) {
      const component = this.pool.pop()
      this.active.add(component)
      return component
    }

    // Create new instance if pool is empty
    const component = await this.createFn()
    this.active.add(component)
    return component
  }

  release(component) {
    if (this.active.has(component)) {
      this.active.delete(component)

      if (this.pool.length < this.maxSize) {
        this.pool.push(component)
      }
    }
  }
}

// Usage
const componentPool = new ComponentPool(() => import('./HeavyComponent.vue'))
</script>
```

---

## 🎉 Kesimpulan

Dynamic Components adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🔄 Component Switching:** Mudah switch antar komponen secara dinamis
2. **📦 Code Organization:** Pisahkan logic ke komponen yang terfokus
3. **🎯 Conditional Rendering:** Render komponen berdasarkan kondisi
4. **🚀 Performance Optimization:** Load komponen sesuai kebutuhan
5. **📱 Flexible UI:** Membuat interface yang adaptif dan dinamis

### Poin Penting:

- Gunakan `:is` directive untuk dynamic component rendering
- Manfaatkan `Keep-Alive` untuk mempertahankan state
- Gunakan `Suspense` untuk handle loading states
- Buat component mapping object untuk maintainability
- Pertimbangkan performance untuk heavy components

### Kapan Menggunakan Dynamic Components:

- ✅ Tab navigation dan wizard forms
- ✅ Content management systems
- ✅ Dashboard widgets
- ✅ Conditional layouts
- ✅ Modal and popup systems

### Kapan Menghindari:

- ❌ Simple static layouts
- ❌ Single component applications
- ❌ Performance-critical sections
- ❌ SEO-critical content
- ❌ Components yang selalu visible

### Pertanya Tambahan:

#### **Bagaimana Cara Penggunaannya?**

1. **Definisi Komponen:** Import semua komponen yang mungkin di-render
2. **Buat Mapping:** Buat object mapping untuk komponen-komponen tersebut
3. **State Management:** Buat reactive state untuk menentukan komponen aktif
4. **Render:** Gunakan `<component :is="currentComponent" />` untuk render
5. **Opsional:** Tambahkan `Keep-Alive`, `Suspense`, dan transitions

#### **Kapan Ini Cocok Digunakan?**

1. **✅ Navigasi & Routing:** Tab systems, menu navigation
2. **✅ Form Wizards:** Multi-step forms dengan komponen berbeda
3. **✅ Content Management:** Berbagai tipe konten (artikel, produk, user profiles)
4. **✅ Dashboard Systems:** Widget yang dapat diubah-ubah
5. **✅ Modal Systems:** Berbagai jenis modal dan popup
6. **✅ Component Libraries:** UI component library dengan variasi
7. **✅ A/B Testing:** Render komponen berbeda untuk testing
8. **✅ Feature Flags:** Enable/disable features berdasarkan kondisi

### Best Practice Summary:

- ✅ **Organization:** Gunakan mapping object untuk maintainability
- ✅ **Performance:** Async loading untuk komponen berat
- ✅ **State Management:** Preserve state dengan Keep-Alive
- ✅ **User Experience:** Smooth transitions dan loading states
- ✅ **Error Handling:** Handle component loading errors
- ✅ **Memory Management:** Cleanup unused components

Dengan memahami dynamic components, Anda sudah bisa membuat aplikasi Vue.js yang fleksibel, performant, dan user-friendly! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan dynamic components dalam berbagai skenario untuk melihat manfaatnya langsung!