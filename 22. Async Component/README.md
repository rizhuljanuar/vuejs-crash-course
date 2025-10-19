# ⚡ Async Component - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja async components untuk loading komponen secara dinamis dan meningkatkan performa aplikasi Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Async Component?](#-apa-itu-async-component)
- [🎮 Cara Menggunakan Async Component](#-cara-menggunakan-async-component)
- [⚡ Basic Async Component](#-basic-async-component)
- [🔄 Conditional Loading](#-conditional-loading)
- [🎯 Loading dan Error States](#-loading-dan-error-states)
- [🔧 Advanced Async Patterns](#-advanced-async-patterns)
- [📊 Performance Benefits](#-performance-benefits)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Async Component?

**Async Component** adalah fitur dalam Vue.js yang memungkinkan kita untuk memuat komponen secara asynchronous (tidak langsung) saat dibutuhkan. Ini sangat berguna untuk mengoptimalkan performa aplikasi dengan memisahkan bundle dan mengurangi initial load time.

### Konsep Dasar Async Component

```javascript
// Sync component (dimuat langsung)
import MyComponent from './MyComponent.vue'

// Async component (dimuat saat dibutuhkan)
const AsyncComponent = defineAsyncComponent(() => import('./MyComponent.vue'))
```

### Mengapa Async Component Penting?

1. **⚡ Performance:** Mengurangi initial bundle size
2. **🚀 Faster Loading:** Halaman load lebih cepat
3. **📦 Code Splitting:** Memisahkan kode otomatis
4. **🎯 Lazy Loading:** Load komponen hanya saat needed
5. **📱 Better UX:** Pengalaman user lebih baik

## ❓ Pertanyaan: Apa Fungsi Async Component?

Async Component memiliki 5 fungsi utama yang sangat penting dalam pengembangan aplikasi Vue.js modern:

### Fungsi 1: Optimasi Performa dan Code Splitting

Async Component memungkinkan kita untuk membagi kode aplikasi menjadi bagian-bagian yang lebih kecil dan memuatnya hanya saat dibutuhkan, mengurangi bundle size awal dan mempercepat loading halaman.

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Async components untuk berbagai fitur aplikasi
const UserProfile = defineAsyncComponent(() => import('./components/UserProfile.vue'))
const Analytics = defineAsyncComponent(() => import('./components/Analytics.vue'))
const AdminPanel = defineAsyncComponent(() => import('./components/AdminPanel.vue'))
const Settings = defineAsyncComponent(() => import('./components/Settings.vue'))

const activeSection = ref('profile')

const sections = [
  { id: 'profile', name: 'User Profile', component: UserProfile },
  { id: 'analytics', name: 'Analytics', component: Analytics },
  { id: 'admin', name: 'Admin Panel', component: AdminPanel },
  { id: 'settings', name: 'Settings', component: Settings }
]

const currentComponent = computed(() => {
  const section = sections.find(s => s.id === activeSection.value)
  return section ? section.component : null
})

// Simulasi bundle size tracking
const bundleInfo = ref({
  initialBundle: '150KB',
  asyncBundles: {
    profile: '45KB',
    analytics: '120KB',
    admin: '80KB',
    settings: '35KB'
  },
  loadedBundles: []
})

const trackBundleLoad = (componentName) => {
  if (!bundleInfo.value.loadedBundles.includes(componentName)) {
    bundleInfo.value.loadedBundles.push(componentName)
    console.log(`Bundle loaded: ${componentName}`)
  }
}
</script>

<template>
  <div class="code-splitting-demo">
    <h2>⚡ Performance Optimization & Code Splitting</h2>

    <div class="bundle-info">
      <h3>📦 Bundle Information</h3>
      <div class="bundle-stats">
        <div class="initial-bundle">
          <strong>Initial Bundle:</strong> {{ bundleInfo.initialBundle }}
        </div>
        <div class="async-bundles">
          <strong>Async Bundles:</strong>
          <ul>
            <li v-for="(size, name) in bundleInfo.asyncBundles" :key="name">
              {{ name }}: {{ size }}
              <span v-if="bundleInfo.loadedBundles.includes(name)" class="loaded-indicator">
                ✅ Loaded
              </span>
            </li>
          </ul>
        </div>
      </div>
    </div>

    <div class="navigation">
      <h3>🧭 Navigate Sections</h3>
      <div class="nav-buttons">
        <button
          v-for="section in sections"
          :key="section.id"
          @click="activeSection = section.id; trackBundleLoad(section.id)"
          :class="{ active: activeSection === section.id }"
          class="nav-btn"
        >
          {{ section.name }}
        </button>
      </div>
    </div>

    <div class="component-display">
      <h3>📱 Current Section: {{ sections.find(s => s.id === activeSection)?.name }}</h3>
      <div class="component-container">
        <Suspense>
          <template #default>
            <component :is="currentComponent" />
          </template>
          <template #fallback>
            <div class="loading-placeholder">
              <div class="spinner"></div>
              <p>Loading {{ sections.find(s => s.id === activeSection)?.name }}...</p>
              <div class="bundle-progress">
                Downloading {{ bundleInfo.asyncBundles[activeSection] }} bundle
              </div>
            </div>
          </template>
        </Suspense>
      </div>
    </div>

    <div class="performance-benefits">
      <h4>🚀 Performance Benefits:</h4>
      <ul>
        <li><strong>Reduced Initial Load:</strong> Hanya 150KB di awal vs 430KB total</li>
        <li><strong>Faster Time to Interactive:</strong> Halaman interaktif lebih cepat</li>
        <li><strong>On-Demand Loading:</strong> Code dimuat hanya saat dibutuhkan</li>
        <li><strong>Better Caching:</strong> Bundle yang sudah dimuat dapat di-cache</li>
        <li><strong>Network Efficiency:</strong> Transfer data lebih sedikit di awal</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.code-splitting-demo {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.bundle-info {
  background: #eff6ff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  border-left: 4px solid #3b82f6;
}

.bundle-info h3 {
  margin-top: 0;
  color: #1e40af;
}

.bundle-stats {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 15px;
}

.initial-bundle {
  font-weight: 500;
  color: #1e40af;
}

.async-bundles ul {
  margin: 5px 0;
  padding-left: 20px;
}

.async-bundles li {
  margin: 5px 0;
  color: #3730a3;
  font-size: 14px;
}

.loaded-indicator {
  color: #059669;
  font-weight: 500;
}

.navigation {
  margin-bottom: 20px;
}

.navigation h3 {
  margin-bottom: 10px;
  color: #374151;
}

.nav-buttons {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.nav-btn {
  padding: 10px 15px;
  background: #f3f4f6;
  border: 2px solid #d1d5db;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
}

.nav-btn:hover {
  background: #e5e7eb;
}

.nav-btn.active {
  background: #3b82f6;
  color: white;
  border-color: #3b82f6;
}

.component-display {
  margin-bottom: 20px;
}

.component-display h3 {
  margin-bottom: 15px;
  color: #374151;
}

.component-container {
  min-height: 200px;
  border: 2px dashed #d1d5db;
  border-radius: 8px;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.loading-placeholder {
  text-align: center;
  color: #6b7280;
}

.spinner {
  width: 32px;
  height: 32px;
  border: 3px solid #e5e7eb;
  border-top: 3px solid #3b82f6;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.bundle-progress {
  margin-top: 10px;
  padding: 8px 12px;
  background: #dbeafe;
  border-radius: 4px;
  font-size: 14px;
  color: #1e40af;
}

.performance-benefits {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.performance-benefits h4 {
  margin-top: 0;
  color: #065f46;
}

.performance-benefits ul {
  margin: 10px 0;
  padding-left: 20px;
}

.performance-benefits li {
  margin: 5px 0;
  color: #065f46;
  font-size: 14px;
}
</style>
```

### Fungsi 2: Loading dan Error States Management

Async Component menyediakan mekanisme untuk mengelola loading states dan error handling dengan elegan menggunakan Suspense component dan error boundaries.

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Async components dengan berbagai konfigurasi
const FastComponent = defineAsyncComponent(() => import('./components/FastComponent.vue'))

const SlowComponent = defineAsyncComponent({
  loader: () => import('./components/SlowComponent.vue'),
  loadingComponent: () => import('./components/LoadingSpinner.vue'),
  delay: 200,
  timeout: 3000
})

const ErrorComponent = defineAsyncComponent({
  loader: () => import('./components/ErrorComponent.vue'),
  errorComponent: () => import('./components/ErrorFallback.vue'),
  onError(error, retry, fail) {
    console.error('Component failed to load:', error)
    // Retry logic
    setTimeout(retry, 2000)
  }
})

const AdminComponent = defineAsyncComponent({
  loader: () => import('./components/AdminComponent.vue'),
  loadingComponent: { template: '<div class="admin-loading">Loading Admin Panel...</div>' },
  errorComponent: { template: '<div class="admin-error">Failed to load admin panel</div>' },
  delay: 100,
  timeout: 5000
})

const selectedComponent = ref('fast')
const loadingHistory = ref([])
const errorHistory = ref([])

const components = {
  fast: {
    name: 'Fast Component',
    description: 'Memuat dengan cepat (< 500ms)',
    component: FastComponent,
    expectedLoadTime: '< 500ms'
  },
  slow: {
    name: 'Slow Component',
    description: 'Memuat dengan lambat (2-3 detik)',
    component: SlowComponent,
    expectedLoadTime: '2-3s'
  },
  error: {
    name: 'Error Component',
    description: 'Akan mengalami error',
    component: ErrorComponent,
    expectedLoadTime: 'Error'
  },
  admin: {
    name: 'Admin Component',
    description: 'Panel administrasi',
    component: AdminComponent,
    expectedLoadTime: '1-2s'
  }
}

const currentComponentInfo = computed(() => {
  return components[selectedComponent.value]
})

const trackLoadingStart = (componentName) => {
  loadingHistory.value.push({
    component: componentName,
    startTime: new Date().toLocaleTimeString(),
    status: 'loading'
  })
}

const trackLoadingComplete = (componentName, duration) => {
  const lastEntry = loadingHistory.value.findLast(entry => entry.component === componentName)
  if (lastEntry) {
    lastEntry.endTime = new Date().toLocaleTimeString()
    lastEntry.duration = duration
    lastEntry.status = 'completed'
  }
}

const trackError = (componentName, error) => {
  errorHistory.value.push({
    component: componentName,
    error: error.message,
    timestamp: new Date().toLocaleTimeString(),
    retryCount: 0
  })
}

const retryComponent = (componentName) => {
  selectedComponent.value = componentName
  const errorEntry = errorHistory.value.findLast(entry => entry.component === componentName)
  if (errorEntry) {
    errorEntry.retryCount++
  }
}

const clearHistory = () => {
  loadingHistory.value = []
  errorHistory.value = []
}
</script>

<template>
  <div class="loading-error-demo">
    <h2>🔄 Loading & Error States Management</h2>

    <div class="component-selector">
      <h3>🎯 Select Component to Load</h3>
      <div class="component-options">
        <div
          v-for="(info, key) in components"
          :key="key"
          @click="selectedComponent = key; trackLoadingStart(info.name)"
          :class="{ active: selectedComponent === key }"
          class="component-option"
        >
          <div class="option-header">
            <h4>{{ info.name }}</h4>
            <span class="load-time">{{ info.expectedLoadTime }}</span>
          </div>
          <p>{{ info.description }}</p>
        </div>
      </div>
    </div>

    <div class="component-display">
      <h3>📱 Component Display</h3>
      <div class="component-container">
        <Suspense @resolve="trackLoadingComplete(currentComponentInfo.name, 500)">
          <template #default>
            <component
              :is="currentComponentInfo.component"
              @error="(e) => trackError(currentComponentInfo.name, e)"
            />
          </template>
          <template #fallback>
            <div class="loading-state">
              <div class="loading-animation">
                <div class="pulse-dot"></div>
                <div class="pulse-dot"></div>
                <div class="pulse-dot"></div>
              </div>
              <h4>Loading {{ currentComponentInfo.name }}...</h4>
              <p>Expected load time: {{ currentComponentInfo.expectedLoadTime }}</p>
              <div class="progress-bar">
                <div class="progress-fill"></div>
              </div>
            </div>
          </template>
        </Suspense>
      </div>
    </div>

    <div class="state-tracker">
      <h3>📊 Loading History</h3>
      <div class="history-container">
        <div v-if="loadingHistory.length === 0" class="empty-state">
          No components loaded yet. Select a component above.
        </div>
        <div v-else class="history-list">
          <div
            v-for="(entry, index) in loadingHistory"
            :key="index"
            :class="entry.status"
            class="history-item"
          >
            <div class="history-header">
              <span class="component-name">{{ entry.component }}</span>
              <span class="status-indicator">
                {{ entry.status === 'loading' ? '⏳' : '✅' }}
              </span>
            </div>
            <div class="history-details">
              <span>Start: {{ entry.startTime }}</span>
              <span v-if="entry.endTime">End: {{ entry.endTime }}</span>
              <span v-if="entry.duration">Duration: {{ entry.duration }}ms</span>
            </div>
          </div>
        </div>
      </div>

      <h3>❌ Error History</h3>
      <div class="error-container">
        <div v-if="errorHistory.length === 0" class="empty-state">
          No errors occurred yet.
        </div>
        <div v-else class="error-list">
          <div
            v-for="(error, index) in errorHistory"
            :key="index"
            class="error-item"
          >
            <div class="error-header">
              <span class="component-name">{{ error.component }}</span>
              <button
                @click="retryComponent(error.component)"
                class="retry-btn"
              >
                Retry ({{ error.retryCount }})
              </button>
            </div>
            <div class="error-details">
              <p class="error-message">{{ error.error }}</p>
              <span class="error-time">{{ error.timestamp }}</span>
            </div>
          </div>
        </div>
      </div>

      <button @click="clearHistory" class="clear-btn">Clear History</button>
    </div>

    <div class="loading-patterns">
      <h4>🎨 Loading Patterns:</h4>
      <ul>
        <li><strong>Suspense:</strong> Vue's built-in loading state management</li>
        <li><strong>Loading Component:</strong> Custom loading component untuk async</li>
        <li><strong>Error Component:</strong> Fallback UI saat loading gagal</li>
        <li><strong>Delay:</strong> Prevent flicker untuk fast loading</li>
        <li><strong>Timeout:</strong> Mencegah loading terlalu lama</li>
        <li><strong>Error Handling:</strong> Retry logic dan error recovery</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.loading-error-demo {
  padding: 20px;
  max-width: 1000px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.component-selector {
  margin-bottom: 20px;
}

.component-selector h3 {
  margin-bottom: 15px;
  color: #374151;
}

.component-options {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.component-option {
  padding: 15px;
  border: 2px solid #e5e7eb;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s;
}

.component-option:hover {
  border-color: #f59e0b;
  background: #fffbeb;
}

.component-option.active {
  border-color: #f59e0b;
  background: #fef3c7;
}

.option-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.option-header h4 {
  margin: 0;
  color: #374151;
}

.load-time {
  font-size: 12px;
  color: #6b7280;
  background: #f3f4f6;
  padding: 2px 6px;
  border-radius: 4px;
}

.component-option p {
  margin: 0;
  font-size: 14px;
  color: #6b7280;
}

.component-display {
  margin-bottom: 20px;
}

.component-display h3 {
  margin-bottom: 15px;
  color: #374151;
}

.component-container {
  min-height: 200px;
  border: 2px dashed #d1d5db;
  border-radius: 8px;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.loading-state {
  text-align: center;
  color: #6b7280;
}

.loading-animation {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-bottom: 15px;
}

.pulse-dot {
  width: 8px;
  height: 8px;
  background: #f59e0b;
  border-radius: 50%;
  animation: pulse 1.4s infinite ease-in-out both;
}

.pulse-dot:nth-child(1) { animation-delay: -0.32s; }
.pulse-dot:nth-child(2) { animation-delay: -0.16s; }
.pulse-dot:nth-child(3) { animation-delay: 0s; }

@keyframes pulse {
  0%, 80%, 100% {
    transform: scale(0);
    opacity: 0.5;
  }
  40% {
    transform: scale(1);
    opacity: 1;
  }
}

.loading-state h4 {
  margin: 10px 0;
  color: #374151;
}

.progress-bar {
  width: 200px;
  height: 4px;
  background: #e5e7eb;
  border-radius: 2px;
  margin: 15px auto 0;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #f59e0b, #d97706);
  border-radius: 2px;
  animation: progress 2s ease-in-out infinite;
}

@keyframes progress {
  0% { width: 0%; }
  50% { width: 70%; }
  100% { width: 100%; }
}

.state-tracker {
  margin-bottom: 20px;
}

.state-tracker h3 {
  margin-bottom: 10px;
  color: #374151;
}

.history-container, .error-container {
  margin-bottom: 15px;
}

.empty-state {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
  padding: 20px;
  background: #f9fafb;
  border-radius: 6px;
}

.history-item, .error-item {
  padding: 10px;
  border-radius: 6px;
  margin-bottom: 8px;
}

.history-item.loading {
  background: #fef3c7;
  border-left: 4px solid #f59e0b;
}

.history-item.completed {
  background: #ecfdf5;
  border-left: 4px solid #10b981;
}

.error-item {
  background: #fef2f2;
  border-left: 4px solid #ef4444;
}

.history-header, .error-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 5px;
}

.component-name {
  font-weight: 500;
  color: #374151;
}

.status-indicator {
  font-size: 16px;
}

.retry-btn {
  padding: 4px 8px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}

.history-details, .error-details {
  font-size: 12px;
  color: #6b7280;
}

.error-message {
  margin: 5px 0;
  color: #dc2626;
}

.error-time {
  font-size: 11px;
}

.clear-btn {
  padding: 8px 16px;
  background: #6b7280;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.loading-patterns {
  background: #fffbeb;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.loading-patterns h4 {
  margin-top: 0;
  color: #92400e;
}

.loading-patterns ul {
  margin: 10px 0;
  padding-left: 20px;
}

.loading-patterns li {
  margin: 5px 0;
  color: #92400e;
  font-size: 14px;
}
</style>
```

### Fungsi 3: Conditional dan Dynamic Loading

Async Component mendukung loading berdasarkan kondisi tertentu, user permissions, atau dynamic imports, memungkinkan aplikasi yang lebih fleksibel dan efisien.

```vue
<script setup>
import { ref, computed, defineAsyncComponent } from 'vue'

const currentUser = ref({
  role: 'guest', // guest, user, admin, premium
  permissions: ['read'],
  subscription: 'free' // free, premium, enterprise
})

// Conditional async components based on user role
const GuestComponent = defineAsyncComponent(() => import('./components/GuestComponent.vue'))
const UserComponent = defineAsyncComponent(() => import('./components/UserComponent.vue'))
const AdminComponent = defineAsyncComponent(() => import('./components/AdminComponent.vue'))
const PremiumComponent = defineAsyncComponent(() => import('./components/PremiumComponent.vue'))

// Conditional components based on permissions
const ReadOnlyComponent = defineAsyncComponent(() => import('./components/ReadOnlyComponent.vue'))
const EditComponent = defineAsyncComponent(() => import('./components/EditComponent.vue'))
const DeleteComponent = defineAsyncComponent(() => import('./components/DeleteComponent.vue'))

// Dynamic feature flags
const ExperimentalFeature = defineAsyncComponent(() =>
  import('./components/ExperimentalFeature.vue')
)

const BetaFeature = defineAsyncComponent(() =>
  import('./components/BetaFeature.vue')
)

// State management
const featureFlags = ref({
  experimentalMode: false,
  betaFeatures: true,
  advancedMode: false
})

const loadingContext = ref('auth')
const loadingHistory = ref([])

// Computed properties untuk conditional components
const authComponent = computed(() => {
  switch (currentUser.value.role) {
    case 'admin':
      return AdminComponent
    case 'premium':
      return PremiumComponent
    case 'user':
      return UserComponent
    default:
      return GuestComponent
  }
})

const permissionComponent = computed(() => {
  const permissions = currentUser.value.permissions
  if (permissions.includes('delete')) return DeleteComponent
  if (permissions.includes('edit')) return EditComponent
  return ReadOnlyComponent
})

const canAccessExperimental = computed(() => {
  return featureFlags.value.experimentalMode &&
         ['admin', 'premium'].includes(currentUser.value.role)
})

const canAccessBeta = computed(() => {
  return featureFlags.value.betaFeatures &&
         ['user', 'admin', 'premium'].includes(currentUser.value.role)
})

// User simulation functions
const simulateLogin = (role) => {
  currentUser.value = {
    role: role,
    permissions: role === 'admin' ? ['read', 'edit', 'delete'] :
                role === 'premium' ? ['read', 'edit'] :
                role === 'user' ? ['read'] : [],
    subscription: role === 'premium' ? 'premium' :
                  role === 'admin' ? 'enterprise' : 'free'
  }

  trackLoading(`Login as ${role}`)
}

const toggleFeature = (feature) => {
  featureFlags.value[feature] = !featureFlags.value[feature]
  trackLoading(`Toggle ${feature}`)
}

const trackLoading = (context) => {
  loadingContext.value = context
  loadingHistory.value.push({
    context: context,
    timestamp: new Date().toLocaleTimeString(),
    userRole: currentUser.value.role,
    featureFlags: { ...featureFlags.value }
  })
}
</script>

<template>
  <div class="conditional-loading-demo">
    <h2>🎯 Conditional & Dynamic Loading</h2>

    <div class="user-simulator">
      <h3>👤 User Role Simulator</h3>
      <div class="role-selector">
        <button
          v-for="role in ['guest', 'user', 'premium', 'admin']"
          :key="role"
          @click="simulateLogin(role)"
          :class="{ active: currentUser.role === role }"
          class="role-btn"
        >
          {{ role.charAt(0).toUpperCase() + role.slice(1) }}
        </button>
      </div>
      <div class="current-user">
        <p><strong>Current Role:</strong> {{ currentUser.role }}</p>
        <p><strong>Permissions:</strong> {{ currentUser.permissions.join(', ') || 'None' }}</p>
        <p><strong>Subscription:</strong> {{ currentUser.subscription }}</p>
      </div>
    </div>

    <div class="feature-flags">
      <h3>🚩 Feature Flags</h3>
      <div class="flag-controls">
        <label class="flag-toggle">
          <input
            type="checkbox"
            v-model="featureFlags.experimentalMode"
            @change="toggleFeature('experimentalMode')"
          />
          Experimental Mode
        </label>
        <label class="flag-toggle">
          <input
            type="checkbox"
            v-model="featureFlags.betaFeatures"
            @change="toggleFeature('betaFeatures')"
          />
          Beta Features
        </label>
        <label class="flag-toggle">
          <input
            type="checkbox"
            v-model="featureFlags.advancedMode"
            @change="toggleFeature('advancedMode')"
          />
          Advanced Mode
        </label>
      </div>
    </div>

    <div class="component-showcase">
      <h3>📱 Component Showcase</h3>

      <!-- Auth-based components -->
      <div class="component-section">
        <h4>🔐 Authentication-based Component</h4>
        <div class="component-container">
          <Suspense>
            <template #default>
              <component :is="authComponent" />
            </template>
            <template #fallback>
              <div class="conditional-loading">
                <div class="loading-text">Loading {{ currentUser.role }} component...</div>
              </div>
            </template>
          </Suspense>
        </div>
      </div>

      <!-- Permission-based components -->
      <div class="component-section">
        <h4>🛡️ Permission-based Component</h4>
        <div class="component-container">
          <Suspense>
            <template #default>
              <component :is="permissionComponent" />
            </template>
            <template #fallback>
              <div class="conditional-loading">
                <div class="loading-text">Loading permission-based component...</div>
              </div>
            </template>
          </Suspense>
        </div>
      </div>

      <!-- Feature-based components -->
      <div class="component-section">
        <h4>🚀 Feature-based Components</h4>
        <div class="feature-components">
          <div v-if="canAccessExperimental" class="feature-item">
            <h5>Experimental Feature</h5>
            <div class="component-container small">
              <Suspense>
                <template #default>
                  <ExperimentalFeature />
                </template>
                <template #fallback>
                  <div class="mini-loading">Loading experimental...</div>
                </template>
              </Suspense>
            </div>
          </div>

          <div v-if="canAccessBeta" class="feature-item">
            <h5>Beta Feature</h5>
            <div class="component-container small">
              <Suspense>
                <template #default>
                  <BetaFeature />
                </template>
                <template #fallback>
                  <div class="mini-loading">Loading beta...</div>
                </template>
              </Suspense>
            </div>
          </div>

          <div v-if="!canAccessExperimental && !canAccessBeta" class="no-features">
            <p>No features available. Change user role or enable feature flags.</p>
          </div>
        </div>
      </div>
    </div>

    <div class="loading-history">
      <h3>📊 Loading History</h3>
      <div class="history-list">
        <div v-if="loadingHistory.length === 0" class="empty-state">
          No loading events yet. Change user role or toggle features.
        </div>
        <div v-else>
          <div
            v-for="(entry, index) in loadingHistory.slice().reverse()"
            :key="index"
            class="history-entry"
          >
            <div class="history-header">
              <span class="context">{{ entry.context }}</span>
              <span class="timestamp">{{ entry.timestamp }}</span>
            </div>
            <div class="history-details">
              <span class="role">Role: {{ entry.userRole }}</span>
              <span class="flags">Features: {{ Object.keys(entry.featureFlags).filter(f => entry.featureFlags[f]).join(', ') || 'None' }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="conditional-info">
      <h4>🎯 Conditional Loading Benefits:</h4>
      <ul>
        <li><strong>Role-based Access:</strong> Load components sesuai user role</li>
        <li><strong>Permission Control:</strong> Dynamic loading berdasarkan permissions</li>
        <li><strong>Feature Flags:</strong> Toggle features tanpa deployment</li>
        <li><strong>A/B Testing:</strong> Load different components untuk testing</li>
        <li><strong>Progressive Enhancement:</strong> Load advanced features gradually</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.conditional-loading-demo {
  padding: 20px;
  max-width: 1000px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.user-simulator {
  background: #faf5ff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.user-simulator h3 {
  margin-top: 0;
  color: #6b21a8;
}

.role-selector {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.role-btn {
  padding: 8px 16px;
  background: #f3e8ff;
  border: 2px solid #d8b4fe;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
}

.role-btn:hover {
  background: #e9d5ff;
}

.role-btn.active {
  background: #8b5cf6;
  color: white;
  border-color: #8b5cf6;
}

.current-user {
  font-size: 14px;
  color: #6b21a8;
}

.current-user p {
  margin: 5px 0;
}

.feature-flags {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.feature-flags h3 {
  margin-top: 0;
  color: #166534;
}

.flag-controls {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.flag-toggle {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  color: #166534;
}

.component-showcase {
  margin-bottom: 20px;
}

.component-showcase h3 {
  margin-bottom: 20px;
  color: #374151;
}

.component-section {
  margin-bottom: 25px;
}

.component-section h4 {
  margin-bottom: 10px;
  color: #475569;
}

.component-container {
  min-height: 120px;
  border: 2px dashed #d1d5db;
  border-radius: 8px;
  padding: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 10px;
}

.component-container.small {
  min-height: 80px;
  padding: 10px;
}

.conditional-loading {
  text-align: center;
  color: #6b7280;
}

.loading-text {
  font-style: italic;
}

.mini-loading {
  font-size: 12px;
  color: #9ca3af;
  font-style: italic;
}

.feature-components {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 15px;
}

.feature-item h5 {
  margin: 0 0 10px 0;
  color: #374151;
  font-size: 14px;
}

.no-features {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
  padding: 20px;
  background: #f9fafb;
  border-radius: 6px;
}

.loading-history {
  margin-bottom: 20px;
}

.loading-history h3 {
  margin-bottom: 10px;
  color: #374151;
}

.history-list {
  max-height: 200px;
  overflow-y: auto;
  border: 1px solid #e5e7eb;
  border-radius: 6px;
  padding: 10px;
}

.empty-state {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
}

.history-entry {
  padding: 8px;
  border-bottom: 1px solid #f3f4f6;
}

.history-entry:last-child {
  border-bottom: none;
}

.history-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
}

.context {
  font-weight: 500;
  color: #374151;
}

.timestamp {
  font-size: 12px;
  color: #6b7280;
}

.history-details {
  display: flex;
  gap: 15px;
  font-size: 12px;
  color: #6b7280;
}

.conditional-info {
  background: #fef3c7;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.conditional-info h4 {
  margin-top: 0;
  color: #92400e;
}

.conditional-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.conditional-info li {
  margin: 5px 0;
  color: #92400e;
  font-size: 14px;
}
</style>
```

### Fungsi 4: Route-based Code Splitting

Async Component sangat penting untuk routing applications di mana setiap route dimuat sebagai bundle terpisah, memungkinkan navigasi antar halaman yang cepat dan efisien.

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Async components untuk setiap route
const Home = defineAsyncComponent(() => import('./views/Home.vue'))
const About = defineAsyncComponent(() => import('./views/About.vue'))
const Products = defineAsyncComponent(() => import('./views/Products.vue'))
const Dashboard = defineAsyncComponent(() => import('./views/Dashboard.vue'))
const Profile = defineAsyncComponent(() => import('./views/Profile.vue'))
const Settings = defineAsyncComponent(() => import('./views/Settings.vue'))

// Current route state
const currentRoute = ref('home')
const routeHistory = ref([])
const isLoading = ref(false)

// Route definitions dengan metadata
const routes = {
  home: {
    name: 'Home',
    component: Home,
    description: 'Landing page utama',
    bundleSize: '45KB',
    loadTime: '< 500ms'
  },
  about: {
    name: 'About',
    component: About,
    description: 'Halaman tentang kami',
    bundleSize: '30KB',
    loadTime: '< 300ms'
  },
  products: {
    name: 'Products',
    component: Products,
    description: 'Daftar produk',
    bundleSize: '120KB',
    loadTime: '1-2s'
  },
  dashboard: {
    name: 'Dashboard',
    component: Dashboard,
    description: 'Dashboard analitik',
    bundleSize: '200KB',
    loadTime: '2-3s'
  },
  profile: {
    name: 'Profile',
    component: Profile,
    description: 'Profil pengguna',
    bundleSize: '80KB',
    loadTime: '< 1s'
  },
  settings: {
    name: 'Settings',
    component: Settings,
    description: 'Pengaturan aplikasi',
    bundleSize: '60KB',
    loadTime: '< 800ms'
  }
}

const currentRouteInfo = computed(() => {
  return routes[currentRoute.value]
})

const loadedRoutes = ref([])

// Navigation functions
const navigateTo = async (routeName) => {
  if (routeName === currentRoute.value || isLoading.value) return

  isLoading.value = true

  try {
    // Simulate route change delay
    await new Promise(resolve => setTimeout(resolve, 300))

    currentRoute.value = routeName

    // Track loaded route
    if (!loadedRoutes.value.includes(routeName)) {
      loadedRoutes.value.push(routeName)
    }

    // Add to history
    routeHistory.value.push({
      route: routeName,
      timestamp: new Date().toLocaleTimeString(),
      loadTime: Math.random() * 2000 + 200
    })

    // Keep only last 10 entries
    if (routeHistory.value.length > 10) {
      routeHistory.value = routeHistory.value.slice(-10)
    }
  } finally {
    isLoading.value = false
  }
}

const goBack = () => {
  if (routeHistory.value.length > 1) {
    const previousRoute = routeHistory.value[routeHistory.value.length - 2].route
    navigateTo(previousRoute)
    routeHistory.value.pop()
  }
}

const clearHistory = () => {
  routeHistory.value = []
}

// Calculate total bundle size
const totalBundleLoaded = computed(() => {
  return loadedRoutes.value.reduce((total, routeName) => {
    const size = parseInt(routes[routeName]?.bundleSize.replace('KB', '') || 0)
    return total + size
  }, 0)
})

const totalPossibleBundle = computed(() => {
  return Object.values(routes).reduce((total, route) => {
    const size = parseInt(route.bundleSize.replace('KB', ''))
    return total + size
  }, 0)
})
</script>

<template>
  <div class="route-splitting-demo">
    <h2>🛣️ Route-based Code Splitting</h2>

    <div class="navigation-bar">
      <h3>🧭 Navigation</h3>
      <div class="nav-buttons">
        <button
          v-for="(route, key) in routes"
          :key="key"
          @click="navigateTo(key)"
          :class="{ active: currentRoute === key, loading: isLoading && currentRoute !== key }"
          :disabled="isLoading"
          class="nav-btn"
        >
          {{ route.name }}
        </button>
      </div>
      <div class="navigation-controls">
        <button @click="goBack" :disabled="routeHistory.length <= 1" class="control-btn">
          ← Back
        </button>
        <button @click="clearHistory" class="control-btn">
          Clear History
        </button>
      </div>
    </div>

    <div class="route-info">
      <h3>📊 Route Information</h3>
      <div class="info-grid">
        <div class="info-item">
          <strong>Current Route:</strong> {{ currentRouteInfo.name }}
        </div>
        <div class="info-item">
          <strong>Description:</strong> {{ currentRouteInfo.description }}
        </div>
        <div class="info-item">
          <strong>Bundle Size:</strong> {{ currentRouteInfo.bundleSize }}
        </div>
        <div class="info-item">
          <strong>Expected Load Time:</strong> {{ currentRouteInfo.loadTime }}
        </div>
      </div>
    </div>

    <div class="content-area">
      <h3>📱 Page Content: {{ currentRouteInfo.name }}</h3>
      <div class="content-container">
        <Suspense>
          <template #default>
            <component :is="currentRouteInfo.component" />
          </template>
          <template #fallback>
            <div class="route-loading">
              <div class="loading-animation">
                <div class="route-spinner"></div>
              </div>
              <h4>Loading {{ currentRouteInfo.name }}...</h4>
              <p>Downloading {{ currentRouteInfo.bundleSize }} bundle</p>
              <div class="route-progress">
                <div class="progress-fill"></div>
              </div>
            </div>
          </template>
        </Suspense>
      </div>
    </div>

    <div class="bundle-metrics">
      <h3>📦 Bundle Metrics</h3>
      <div class="metrics-grid">
        <div class="metric-card">
          <h4>Loaded Routes</h4>
          <div class="loaded-list">
            <div v-if="loadedRoutes.length === 0" class="empty-state">
              No routes loaded yet
            </div>
            <div v-else>
              <span
                v-for="route in loadedRoutes"
                :key="route"
                :class="{ current: route === currentRoute }"
                class="loaded-route"
              >
                {{ routes[route].name }}
              </span>
            </div>
          </div>
        </div>

        <div class="metric-card">
          <h4>Bundle Size Analysis</h4>
          <div class="size-info">
            <p><strong>Loaded:</strong> {{ totalBundleLoaded }}KB</p>
            <p><strong>Total Possible:</strong> {{ totalPossibleBundle }}KB</p>
            <p><strong>Saved:</strong> {{ totalPossibleBundle - totalBundleLoaded }}KB</p>
            <div class="progress-bar">
              <div
                class="size-progress"
                :style="{ width: (totalBundleLoaded / totalPossibleBundle * 100) + '%' }"
              ></div>
            </div>
          </div>
        </div>

        <div class="metric-card">
          <h4>Navigation History</h4>
          <div class="history-list">
            <div v-if="routeHistory.length === 0" class="empty-state">
              No navigation yet
            </div>
            <div v-else>
              <div
                v-for="(entry, index) in routeHistory.slice().reverse()"
                :key="index"
                class="history-entry"
              >
                <span class="route-name">{{ routes[entry.route].name }}</span>
                <span class="timestamp">{{ entry.timestamp }}</span>
                <span class="load-time">{{ Math.round(entry.loadTime) }}ms</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="route-benefits">
      <h4>🚀 Route-based Splitting Benefits:</h4>
      <ul>
        <li><strong>Per-route Loading:</strong> Hanya memuat bundle untuk route yang dikunjungi</li>
        <li><strong>Faster Navigation:</strong> Route yang sudah dimuat tidak perlu download lagi</li>
        <li><strong>Better Caching:</strong> Setiap route dapat di-cache secara terpisah</li>
        <li><strong>Progressive Loading:</strong> User dapat langsung mengakses halaman utama</li>
        <li><strong>Reduced Memory:</strong> Tidak semua route dimuat di memory secara bersamaan</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.route-splitting-demo {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.navigation-bar {
  background: #ecfdf5;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.navigation-bar h3 {
  margin-top: 0;
  color: #065f46;
}

.nav-buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.nav-btn {
  padding: 10px 15px;
  background: #f0fdf4;
  border: 2px solid #86efac;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
  font-weight: 500;
}

.nav-btn:hover:not(:disabled) {
  background: #dcfce7;
}

.nav-btn.active {
  background: #10b981;
  color: white;
  border-color: #10b981;
}

.nav-btn.loading:not(:disabled) {
  opacity: 0.6;
  cursor: not-allowed;
}

.nav-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.navigation-controls {
  display: flex;
  gap: 10px;
}

.control-btn {
  padding: 8px 12px;
  background: #6b7280;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.control-btn:disabled {
  background: #d1d5db;
  cursor: not-allowed;
}

.route-info {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.route-info h3 {
  margin-top: 0;
  color: #065f46;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.info-item {
  padding: 8px;
  background: white;
  border-radius: 4px;
  font-size: 14px;
}

.content-area {
  margin-bottom: 20px;
}

.content-area h3 {
  margin-bottom: 15px;
  color: #374151;
}

.content-container {
  min-height: 300px;
  border: 2px dashed #d1d5db;
  border-radius: 8px;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.route-loading {
  text-align: center;
  color: #6b7280;
}

.loading-animation {
  margin-bottom: 15px;
}

.route-spinner {
  width: 40px;
  height: 40px;
  border: 3px solid #e5e7eb;
  border-top: 3px solid #10b981;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.route-loading h4 {
  margin: 10px 0;
  color: #374151;
}

.route-progress {
  width: 250px;
  height: 6px;
  background: #e5e7eb;
  border-radius: 3px;
  margin: 15px auto 0;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #10b981, #059669);
  border-radius: 3px;
  animation: progress 2s ease-in-out infinite;
}

@keyframes progress {
  0% { width: 0%; }
  50% { width: 70%; }
  100% { width: 100%; }
}

.bundle-metrics {
  margin-bottom: 20px;
}

.bundle-metrics h3 {
  margin-bottom: 15px;
  color: #374151;
}

.metrics-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 15px;
}

.metric-card {
  background: #f9fafb;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
}

.metric-card h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.loaded-list {
  margin-bottom: 10px;
}

.empty-state {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
  font-size: 14px;
}

.loaded-route {
  display: inline-block;
  margin: 3px;
  padding: 4px 8px;
  background: #e5e7eb;
  border-radius: 4px;
  font-size: 12px;
}

.loaded-route.current {
  background: #10b981;
  color: white;
}

.size-info p {
  margin: 5px 0;
  font-size: 14px;
  color: #374151;
}

.progress-bar {
  width: 100%;
  height: 8px;
  background: #e5e7eb;
  border-radius: 4px;
  margin-top: 10px;
  overflow: hidden;
}

.size-progress {
  height: 100%;
  background: #10b981;
  border-radius: 4px;
  transition: width 0.3s ease;
}

.history-list {
  max-height: 150px;
  overflow-y: auto;
}

.history-entry {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 0;
  border-bottom: 1px solid #f3f4f6;
  font-size: 12px;
}

.history-entry:last-child {
  border-bottom: none;
}

.route-name {
  font-weight: 500;
  color: #374151;
}

.timestamp, .load-time {
  color: #6b7280;
}

.route-benefits {
  background: #ecfdf5;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.route-benefits h4 {
  margin-top: 0;
  color: #065f46;
}

.route-benefits ul {
  margin: 10px 0;
  padding-left: 20px;
}

.route-benefits li {
  margin: 5px 0;
  color: #065f46;
  font-size: 14px;
}
</style>
```

### Fungsi 5: Preloading dan Prefetching Strategies

Async Component mendukung berbagai strategi preloading untuk mengoptimalkan user experience dengan memuat komponen sebelum dibutuhkan, seperti saat user hover atau prediksi navigasi.

```vue
<script setup>
import { ref, defineAsyncComponent, onMounted } from 'vue'

// Base async components
const Home = defineAsyncComponent(() => import('./views/Home.vue'))
const Products = defineAsyncComponent(() => import('./views/Products.vue'))
const Dashboard = defineAsyncComponent(() => import('./views/Dashboard.vue'))
const Profile = defineAsyncComponent(() => import('./views/Profile.vue'))

// Preloading state management
const preloadedComponents = ref(new Set())
const hoveredComponents = ref(new Set())
const prefetchProgress = ref({})
const loadingStrategies = ref({
  immediate: [],
  onHover: [],
  onIdle: [],
  onVisible: []
})

// Component definitions dengan preloading strategies
const componentConfigs = {
  home: {
    name: 'Home',
    component: Home,
    priority: 'high',
    strategy: 'immediate',
    description: 'Landing page - dimuat langsung'
  },
  products: {
    name: 'Products',
    component: Products,
    priority: 'medium',
    strategy: 'onHover',
    description: 'Products - dimuat saat hover'
  },
  dashboard: {
    name: 'Dashboard',
    component: Dashboard,
    priority: 'low',
    strategy: 'onIdle',
    description: 'Dashboard - dimuat saat browser idle'
  },
  profile: {
    name: 'Profile',
    component: Profile,
    priority: 'medium',
    strategy: 'onVisible',
    description: 'Profile - dimuat saat visible'
  }
}

// Preloading functions
const preloadComponent = async (componentKey, strategy) => {
  if (preloadedComponents.value.has(componentKey)) return

  try {
    prefetchProgress.value[componentKey] = 0

    // Simulate progressive loading
    const loadingSteps = [25, 50, 75, 100]
    for (const progress of loadingSteps) {
      prefetchProgress.value[componentKey] = progress
      await new Promise(resolve => setTimeout(resolve, 200))
    }

    // Load the component
    const config = componentConfigs[componentKey]
    await config.component

    preloadedComponents.value.add(componentKey)
    loadingStrategies.value[strategy].push(componentKey)

    console.log(`Component ${componentKey} preloaded via ${strategy}`)
  } catch (error) {
    console.error(`Failed to preload ${componentKey}:`, error)
  } finally {
    delete prefetchProgress.value[componentKey]
  }
}

// Strategy: Immediate preloading
const immediatePreload = () => {
  const immediateComponents = Object.entries(componentConfigs)
    .filter(([_, config]) => config.strategy === 'immediate')

  immediateComponents.forEach(([key, _]) => {
    preloadComponent(key, 'immediate')
  })
}

// Strategy: On hover preloading
const handleComponentHover = (componentKey) => {
  hoveredComponents.value.add(componentKey)

  if (componentConfigs[componentKey].strategy === 'onHover') {
    // Delay preloading to avoid unnecessary loads
    setTimeout(() => {
      if (hoveredComponents.value.has(componentKey)) {
        preloadComponent(componentKey, 'onHover')
      }
    }, 500)
  }
}

const handleComponentLeave = (componentKey) => {
  hoveredComponents.value.delete(componentKey)
}

// Strategy: On idle preloading
const setupIdlePreloading = () => {
  if ('requestIdleCallback' in window) {
    requestIdleCallback(() => {
      const idleComponents = Object.entries(componentConfigs)
        .filter(([_, config]) => config.strategy === 'onIdle')

      idleComponents.forEach(([key, _]) => {
        preloadComponent(key, 'onIdle')
      })
    })
  } else {
    // Fallback for browsers without requestIdleCallback
    setTimeout(() => {
      const idleComponents = Object.entries(componentConfigs)
        .filter(([_, config]) => config.strategy === 'onIdle')

      idleComponents.forEach(([key, _]) => {
        preloadComponent(key, 'onIdle')
      })
    }, 2000)
  }
}

// Strategy: On visible preloading (Intersection Observer)
const setupVisiblePreloading = () => {
  if ('IntersectionObserver' in window) {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          const componentKey = entry.target.dataset.component
          if (componentConfigs[componentKey]?.strategy === 'onVisible') {
            preloadComponent(componentKey, 'onVisible')
            observer.unobserve(entry.target)
          }
        }
      })
    }, { threshold: 0.1 })

    // Observe all visible preload elements
    document.querySelectorAll('[data-component]').forEach(el => {
      observer.observe(el)
    })
  }
}

// Manual preloading
const manualPreload = (componentKey) => {
  preloadComponent(componentKey, 'manual')
}

const clearPreloaded = () => {
  preloadedComponents.value.clear()
  hoveredComponents.value.clear()
  prefetchProgress.value = {}
  loadingStrategies.value = {
    immediate: [],
    onHover: [],
    onIdle: [],
    onVisible: []
  }
}

// Stats
const preloadingStats = computed(() => {
  const total = Object.keys(componentConfigs).length
  const loaded = preloadedComponents.value.size
  const inProgress = Object.keys(prefetchProgress.value).length

  return {
    total,
    loaded,
    inProgress,
    percentage: Math.round((loaded / total) * 100)
  }
})

// Initialize strategies
onMounted(() => {
  immediatePreload()
  setupIdlePreloading()
  setupVisiblePreloading()
})
</script>

<template>
  <div class="preloading-demo">
    <h2>⚡ Preloading & Prefetching Strategies</h2>

    <div class="stats-overview">
      <h3>📊 Preloading Statistics</h3>
      <div class="stats-grid">
        <div class="stat-card">
          <h4>Total Components</h4>
          <div class="stat-value">{{ preloadingStats.total }}</div>
        </div>
        <div class="stat-card">
          <h4>Preloaded</h4>
          <div class="stat-value">{{ preloadingStats.loaded }}</div>
        </div>
        <div class="stat-card">
          <h4>In Progress</h4>
          <div class="stat-value">{{ preloadingStats.inProgress }}</div>
        </div>
        <div class="stat-card">
          <h4>Progress</h4>
          <div class="stat-value">{{ preloadingStats.percentage }}%</div>
          <div class="progress-bar">
            <div
              class="progress-fill"
              :style="{ width: preloadingStats.percentage + '%' }"
            ></div>
          </div>
        </div>
      </div>
    </div>

    <div class="component-strategies">
      <h3>🎯 Component Preloading Strategies</h3>
      <div class="strategies-grid">
        <div
          v-for="(config, key) in componentConfigs"
          :key="key"
          :data-component="key"
          @mouseenter="handleComponentHover(key)"
          @mouseleave="handleComponentLeave(key)"
          :class="{
            'preloaded': preloadedComponents.has(key),
            'hovered': hoveredComponents.has(key),
            'loading': prefetchProgress[key] !== undefined
          }"
          class="component-card"
        >
          <div class="card-header">
            <h4>{{ config.name }}</h4>
            <div class="priority-badge" :class="config.priority">
              {{ config.priority }}
            </div>
          </div>

          <p class="strategy-desc">{{ config.description }}</p>

          <div class="strategy-info">
            <span class="strategy-label">Strategy: {{ config.strategy }}</span>
          </div>

          <div class="loading-status">
            <div v-if="preloadedComponents.has(key)" class="status-success">
              ✅ Preloaded
            </div>
            <div v-else-if="prefetchProgress[key] !== undefined" class="status-loading">
              <div class="mini-progress">
                <div
                  class="mini-progress-fill"
                  :style="{ width: prefetchProgress[key] + '%' }"
                ></div>
              </div>
              Loading... {{ prefetchProgress[key] }}%
            </div>
            <div v-else class="status-pending">
              ⏳ Not preloaded
            </div>
          </div>

          <button
            @click="manualPreload(key)"
            :disabled="preloadedComponents.has(key)"
            class="preload-btn"
          >
            {{ preloadedComponents.has(key) ? 'Preloaded' : 'Preload Now' }}
          </button>
        </div>
      </div>
    </div>

    <div class="strategy-breakdown">
      <h3>📋 Strategy Breakdown</h3>
      <div class="breakdown-grid">
        <div class="strategy-section">
          <h4>🚀 Immediate</h4>
          <p>Components loaded immediately on mount</p>
          <div class="component-list">
            <span
              v-for="comp in loadingStrategies.immediate"
              :key="comp"
              class="component-tag success"
            >
              {{ componentConfigs[comp].name }}
            </span>
            <span v-if="loadingStrategies.immediate.length === 0" class="empty-tag">
              None yet
            </span>
          </div>
        </div>

        <div class="strategy-section">
          <h4>👆 On Hover</h4>
          <p>Components loaded when user hovers</p>
          <div class="component-list">
            <span
              v-for="comp in loadingStrategies.onHover"
              :key="comp"
              class="component-tag success"
            >
              {{ componentConfigs[comp].name }}
            </span>
            <span v-if="loadingStrategies.onHover.length === 0" class="empty-tag">
              None yet
            </span>
          </div>
        </div>

        <div class="strategy-section">
          <h4>😴 On Idle</h4>
          <p>Components loaded during browser idle time</p>
          <div class="component-list">
            <span
              v-for="comp in loadingStrategies.onIdle"
              :key="comp"
              class="component-tag success"
            >
              {{ componentConfigs[comp].name }}
            </span>
            <span v-if="loadingStrategies.onIdle.length === 0" class="empty-tag">
              None yet
            </span>
          </div>
        </div>

        <div class="strategy-section">
          <h4>👁️ On Visible</h4>
          <p>Components loaded when scrolled into view</p>
          <div class="component-list">
            <span
              v-for="comp in loadingStrategies.onVisible"
              :key="comp"
              class="component-tag success"
            >
              {{ componentConfigs[comp].name }}
            </span>
            <span v-if="loadingStrategies.onVisible.length === 0" class="empty-tag">
              None yet
            </span>
          </div>
        </div>
      </div>
    </div>

    <div class="controls">
      <button @click="clearPreloaded" class="clear-btn">
        Clear All Preloaded
      </button>
    </div>

    <div class="preloading-benefits">
      <h4>🎯 Preloading Benefits:</h4>
      <ul>
        <li><strong>Instant Navigation:</strong> Components already loaded when needed</li>
        <li><strong>Smart Resource Management:</strong> Load components during optimal times</li>
        <li><strong>Better UX:</strong> No loading delays for preloaded components</li>
        <li><strong>Network Optimization:</strong> Use idle bandwidth effectively</li>
        <li><strong>Predictive Loading:</strong> Anticipate user behavior</li>
        <li><strong>Progressive Enhancement:</strong> Critical vs non-critical separation</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.preloading-demo {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.stats-overview {
  background: #eff6ff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.stats-overview h3 {
  margin-top: 0;
  color: #1e40af;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
}

.stat-card {
  background: white;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  border: 1px solid #dbeafe;
}

.stat-card h4 {
  margin: 0 0 10px 0;
  color: #374151;
  font-size: 14px;
}

.stat-value {
  font-size: 24px;
  font-weight: bold;
  color: #3b82f6;
}

.progress-bar {
  width: 100%;
  height: 6px;
  background: #e5e7eb;
  border-radius: 3px;
  margin-top: 8px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #3b82f6;
  border-radius: 3px;
  transition: width 0.3s ease;
}

.component-strategies {
  margin-bottom: 20px;
}

.component-strategies h3 {
  margin-bottom: 15px;
  color: #374151;
}

.strategies-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 15px;
}

.component-card {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  border: 2px solid #e2e8f0;
  cursor: pointer;
  transition: all 0.3s;
}

.component-card:hover {
  border-color: #3b82f6;
  background: #eff6ff;
}

.component-card.preloaded {
  border-color: #10b981;
  background: #ecfdf5;
}

.component-card.hovered {
  border-color: #f59e0b;
  background: #fffbeb;
}

.component-card.loading {
  border-color: #8b5cf6;
  background: #faf5ff;
}

.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.card-header h4 {
  margin: 0;
  color: #374151;
}

.priority-badge {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
}

.priority-badge.high {
  background: #fecaca;
  color: #dc2626;
}

.priority-badge.medium {
  background: #fed7aa;
  color: #ea580c;
}

.priority-badge.low {
  background: #e0e7ff;
  color: #4f46e5;
}

.strategy-desc {
  margin: 8px 0;
  font-size: 14px;
  color: #6b7280;
}

.strategy-info {
  margin-bottom: 10px;
}

.strategy-label {
  font-size: 12px;
  color: #374151;
  background: #e5e7eb;
  padding: 2px 6px;
  border-radius: 4px;
}

.loading-status {
  margin-bottom: 10px;
  min-height: 20px;
}

.status-success {
  color: #059669;
  font-weight: 500;
  font-size: 14px;
}

.status-loading {
  color: #7c3aed;
  font-size: 14px;
}

.mini-progress {
  width: 100%;
  height: 4px;
  background: #e5e7eb;
  border-radius: 2px;
  margin: 5px 0;
  overflow: hidden;
}

.mini-progress-fill {
  height: 100%;
  background: #7c3aed;
  border-radius: 2px;
  transition: width 0.3s ease;
}

.status-pending {
  color: #6b7280;
  font-size: 14px;
}

.preload-btn {
  width: 100%;
  padding: 8px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.preload-btn:hover:not(:disabled) {
  background: #2563eb;
}

.preload-btn:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.strategy-breakdown {
  margin-bottom: 20px;
}

.strategy-breakdown h3 {
  margin-bottom: 15px;
  color: #374151;
}

.breakdown-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.strategy-section {
  background: #f9fafb;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e5e7eb;
}

.strategy-section h4 {
  margin-top: 0;
  margin-bottom: 5px;
  color: #374151;
}

.strategy-section p {
  margin: 0 0 10px 0;
  font-size: 14px;
  color: #6b7280;
}

.component-list {
  min-height: 30px;
}

.component-tag {
  display: inline-block;
  margin: 3px;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 500;
}

.component-tag.success {
  background: #dcfce7;
  color: #166534;
}

.empty-tag {
  color: #9ca3af;
  font-style: italic;
  font-size: 12px;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.clear-btn {
  padding: 10px 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.clear-btn:hover {
  background: #dc2626;
}

.preloading-benefits {
  background: #eff6ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.preloading-benefits h4 {
  margin-top: 0;
  color: #1e40af;
}

.preloading-benefits ul {
  margin: 10px 0;
  padding-left: 20px;
}

.preloading-benefits li {
  margin: 5px 0;
  color: #1e40af;
  font-size: 14px;
}
</style>
```

### Sync vs Async Component

| Aspect | Sync Component | Async Component |
|--------|----------------|-----------------|
| **Loading Time** | Load semua di awal | Load saat dibutuhkan |
| **Bundle Size** | Besar | Kecil (di awal) |
| **Performance** | Lambat di awal | Cepat di awal |
| **User Experience** | Tunggu lama | Load progresif |
| **Memory Usage** | Tinggi | Optimal |

## 🎮 Cara Menggunakan Async Component

### 1. Import defineAsyncComponent

```vue
<script setup>
import { defineAsyncComponent } from 'vue'
</script>
```

### 2. Define Async Component

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// Basic async component
const AsyncComponent = defineAsyncComponent(() =>
  import('./AsyncComponent.vue')
)
</script>
```

### 3. Gunakan di Template

```vue
<template>
  <div>
    <AsyncComponent />
  </div>
</template>
```

## ⚡ Basic Async Component

### Simple Async Loading

```vue
<!-- App.vue -->
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Async component definition
const RenderFriends = defineAsyncComponent(() =>
  import('./components/RenderFriends.vue')
)

const showFriends = ref(false)

const toggleFriendList = () => {
  showFriends.value = !showFriends.value
}
</script>

<template>
  <div class="async-demo">
    <h2>Basic Async Component</h2>

    <div class="toggle-section">
      <button @click="toggleFriendList" class="toggle-btn">
        {{ showFriends ? 'Hide' : 'Show' }} Friend List
      </button>
    </div>

    <div v-if="showFriends" class="friends-section">
      <!-- Async component hanya dimuat saat showFriends = true -->
      <RenderFriends />
    </div>

    <div class="info-box">
      <h4>⚡ What's happening:</h4>
      <ul>
        <li>Komponen <strong>RenderFriends.vue</strong> tidak dimuat di awal</li>
        <li>Komponen hanya di-load saat tombol diklik pertama kali</li>
        <li>Setelah di-load, komponen akan di-cache</li>
        <li>Mengurangi initial bundle size</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.async-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.toggle-section {
  text-align: center;
  margin-bottom: 20px;
}

.toggle-btn {
  padding: 12px 24px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s ease;
}

.toggle-btn:hover {
  background: #2563eb;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.3);
}

.friends-section {
  margin-top: 20px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
  border: 1px solid #bfdbfe;
}

.info-box {
  margin-top: 20px;
  padding: 15px;
  background: #dbeafe;
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
  color: #1e3a8a;
}
</style>
```

```vue
<!-- components/RenderFriends.vue -->
<script setup>
import { ref } from 'vue'

const friends = ref(['HuXn', 'Jordan', 'Alex', 'Maroon 5'])

// Simulate data loading
const isLoading = ref(true)

setTimeout(() => {
  isLoading.value = false
}, 1000)
</script>

<template>
  <div class="friends-list">
    <h3>Friends List</h3>

    <div v-if="isLoading" class="loading">
      <p>Loading friends...</p>
    </div>

    <div v-else>
      <div
        v-for="(friend, index) in friends"
        :key="index"
        class="friend-item"
      >
        <span class="friend-icon">👤</span>
        <span class="friend-name">{{ friend }}</span>
      </div>
    </div>

    <div class="friend-stats">
      <p>Total friends: {{ friends.length }}</p>
    </div>
  </div>
</template>

<style scoped>
.friends-list {
  text-align: center;
}

.friends-list h3 {
  margin-top: 0;
  color: #1e40af;
}

.loading {
  padding: 20px;
  color: #64748b;
  font-style: italic;
}

.friend-item {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  padding: 10px;
  margin: 8px 0;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.friend-item:hover {
  transform: translateY(-2px);
}

.friend-icon {
  font-size: 20px;
}

.friend-name {
  font-weight: 500;
  color: #374151;
}

.friend-stats {
  margin-top: 15px;
  padding: 10px;
  background: #e0f2fe;
  border-radius: 6px;
  color: #0c4a6e;
  font-size: 14px;
}
</style>
```

### Multiple Async Components

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

// Multiple async components
const UserProfile = defineAsyncComponent(() =>
  import('./components/UserProfile.vue')
)

const UserSettings = defineAsyncComponent(() =>
  import('./components/UserSettings.vue')
)

const UserActivity = defineAsyncComponent(() =>
  import('./components/UserActivity.vue')
)

const activeTab = ref('profile')

const tabs = [
  { id: 'profile', name: 'Profile', component: UserProfile },
  { id: 'settings', name: 'Settings', component: UserSettings },
  { id: 'activity', name: 'Activity', component: UserActivity }
]

const currentComponent = computed(() => {
  const tab = tabs.find(t => t.id === activeTab.value)
  return tab ? tab.component : null
})
</script>

<template>
  <div class="multi-async-demo">
    <h2>Multiple Async Components</h2>

    <div class="tab-navigation">
      <button
        v-for="tab in tabs"
        :key="tab.id"
        @click="activeTab = tab.id"
        :class="{ active: activeTab === tab.id }"
        class="tab-btn"
      >
        {{ tab.name }}
      </button>
    </div>

    <div class="tab-content">
      <!-- Dynamic component dengan async loading -->
      <component :is="currentComponent" v-if="currentComponent" />
    </div>

    <div class="performance-info">
      <h4>📊 Performance Benefits:</h4>
      <ul>
        <li>Hanya komponen aktif yang dimuat</li>
        <li>Tab lain dimuat saat dibuka pertama kali</li>
        <li>Komponen yang sudah dimuat di-cache</li>
        <li>Menghemat bandwidth dan memory</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.multi-async-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.tab-navigation {
  display: flex;
  gap: 5px;
  margin-bottom: 20px;
  border-bottom: 2px solid #e5e7eb;
}

.tab-btn {
  padding: 10px 20px;
  background: transparent;
  border: none;
  border-bottom: 2px solid transparent;
  cursor: pointer;
  font-size: 14px;
  color: #6b7280;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  color: #374151;
  background: #f9fafb;
}

.tab-btn.active {
  color: #10b981;
  border-bottom-color: #10b981;
  background: #f0fdf4;
}

.tab-content {
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  min-height: 300px;
}

.performance-info {
  margin-top: 20px;
  padding: 15px;
  background: #d1fae5;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.performance-info h4 {
  margin-top: 0;
  color: #065f46;
}

.performance-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.performance-info li {
  margin: 5px 0;
  color: #047857;
}
</style>
```

## 🔄 Conditional Loading

### Dynamic Component Loading

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

const componentType = ref('welcome')
const isLoading = ref(false)

// Async component mapping
const asyncComponents = {
  welcome: defineAsyncComponent(() => import('./components/WelcomeComponent.vue')),
  dashboard: defineAsyncComponent(() => import('./components/DashboardComponent.vue')),
  profile: defineAsyncComponent(() => import('./components/ProfileComponent.vue')),
  settings: defineAsyncComponent(() => import('./components/SettingsComponent.vue'))
}

const currentComponent = computed(() => {
  return asyncComponents[componentType.value] || null
})

const loadComponent = async (type) => {
  if (type !== componentType.value) {
    isLoading.value = true
    componentType.value = type

    // Simulate loading delay
    setTimeout(() => {
      isLoading.value = false
    }, 500)
  }
}
</script>

<template>
  <div class="conditional-async-demo">
    <h2>Conditional Async Loading</h2>

    <div class="component-selector">
      <h4>Pilih Component:</h4>
      <div class="button-grid">
        <button
          v-for="(_, type) in asyncComponents"
          :key="type"
          @click="loadComponent(type)"
          :class="{ active: componentType === type }"
          class="component-btn"
        >
          {{ type.charAt(0).toUpperCase() + type.slice(1) }}
        </button>
      </div>
    </div>

    <div class="component-display">
      <div v-if="isLoading" class="loading-state">
        <div class="spinner"></div>
        <p>Loading component...</p>
      </div>

      <div v-else-if="currentComponent" class="component-wrapper">
        <Suspense>
          <template #default>
            <component :is="currentComponent" />
          </template>
          <template #fallback>
            <div class="loading-fallback">
              <p>Loading {{ componentType }}...</p>
            </div>
          </template>
        </Suspense>
      </div>
    </div>

    <div class="info-panel">
      <h4>🔄 Loading Behavior:</h4>
      <ul>
        <li>Component dimuat hanya saat dipilih</li>
        <li>Loading state ditampilkan selama proses</li>
        <li>Suspense fallback untuk async loading</li>
        <li>Component di-cache setelah pertama kali dimuat</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.conditional-async-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.component-selector h4 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #6b21a8;
}

.button-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 10px;
  margin-bottom: 30px;
}

.component-btn {
  padding: 10px 15px;
  background: #f3e8ff;
  border: 2px solid #d8b4fe;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  color: #6b21a8;
  font-weight: 500;
}

.component-btn:hover {
  background: #e9d5ff;
  transform: translateY(-1px);
}

.component-btn.active {
  background: #8b5cf6;
  color: white;
  border-color: #8b5cf6;
}

.component-display {
  margin-bottom: 20px;
  min-height: 200px;
}

.loading-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  color: #6b7280;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top: 4px solid #8b5cf6;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.component-wrapper {
  padding: 20px;
  background: #faf5ff;
  border-radius: 8px;
  border: 1px solid #e9d5ff;
}

.loading-fallback {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background: #f3e8ff;
  border-radius: 8px;
  color: #6b21a8;
  font-style: italic;
}

.info-panel {
  background: #f3e8ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.info-panel h4 {
  margin-top: 0;
  color: #6b21a8;
}

.info-panel ul {
  margin: 10px 0;
  padding-left: 20px;
}

.info-panel li {
  margin: 5px 0;
  color: #6b21a8;
}
</style>
```

## 🎯 Loading dan Error States

### Advanced Async dengan Suspense

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

const showComponent = ref(false)

// Async component dengan error handling
const AdvancedComponent = defineAsyncComponent({
  loader: () => import('./components/AdvancedComponent.vue'),
  loadingComponent: () => import('./components/LoadingComponent.vue'),
  errorComponent: () => import('./components/ErrorComponent.vue'),
  delay: 200,
  timeout: 3000
})

const loadWithSuspense = () => {
  showComponent.value = true
}
</script>

<template>
  <div class="suspense-demo">
    <h2>Async dengan Suspense</h2>

    <div class="controls">
      <button @click="loadWithSuspense" class="load-btn">
        Load Advanced Component
      </button>
    </div>

    <div v-if="showComponent" class="suspense-wrapper">
      <Suspense>
        <template #default>
          <AdvancedComponent />
        </template>

        <template #fallback>
          <div class="fallback-content">
            <div class="loading-animation">
              <div class="bounce-1"></div>
              <div class="bounce-2"></div>
              <div class="bounce-3"></div>
            </div>
            <p>Loading advanced component...</p>
          </div>
        </template>
      </Suspense>
    </div>

    <div class="features-grid">
      <div class="feature-card">
        <h4>⚡ Lazy Loading</h4>
        <p>Component hanya dimuat saat dibutuhkan</p>
      </div>

      <div class="feature-card">
        <h4>🔄 Loading State</h4>
        <p>Suspense fallback ditampilkan selama loading</p>
      </div>

      <div class="feature-card">
        <h4>❌ Error Handling</h4>
        <p>Error handling jika component gagal dimuat</p>
      </div>

      <div class="feature-card">
        <h4>⏱️ Timeout</h4>
        <p>Timeout protection untuk loading terlalu lama</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.suspense-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.controls {
  text-align: center;
  margin-bottom: 30px;
}

.load-btn {
  padding: 12px 24px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s ease;
}

.load-btn:hover {
  background: #d97706;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(245, 158, 11, 0.3);
}

.suspense-wrapper {
  margin-bottom: 30px;
  min-height: 200px;
}

.fallback-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 40px;
  background: #fef3c7;
  border-radius: 8px;
  color: #92400e;
}

.loading-animation {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.bounce-1, .bounce-2, .bounce-3 {
  width: 12px;
  height: 12px;
  background: #f59e0b;
  border-radius: 50%;
  animation: bounce 1.4s infinite ease-in-out both;
}

.bounce-1 { animation-delay: -0.32s; }
.bounce-2 { animation-delay: -0.16s; }

@keyframes bounce {
  0%, 80%, 100% {
    transform: scale(0);
  }
  40% {
    transform: scale(1);
  }
}

.features-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.feature-card {
  background: #fffbeb;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #fed7aa;
  text-align: center;
}

.feature-card h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #92400e;
}

.feature-card p {
  margin: 0;
  color: #78350f;
  font-size: 14px;
}
</style>
```

### Error Boundary untuk Async Components

```vue
<script setup>
import { ref, defineAsyncComponent, onErrorCaptured } from 'vue'

const loadComponent = ref(null)
const hasError = ref(false)
const errorMessage = ref('')

// Async component yang mungkin error
const ProblematicComponent = defineAsyncComponent({
  loader: () => import('./components/ProblematicComponent.vue'),
  onError(error) {
    console.error('Component loading failed:', error)
    errorMessage.value = `Failed to load component: ${error.message}`
    hasError.value = true
  }
})

const tryLoad = (componentName) => {
  hasError.value = false
  errorMessage.value = ''

  // Simulate different loading scenarios
  if (componentName === 'good') {
    loadComponent.value = defineAsyncComponent(() => import('./components/GoodComponent.vue'))
  } else if (componentName === 'bad') {
    loadComponent.value = defineAsyncComponent(() => import('./components/NonExistentComponent.vue'))
  } else if (componentName === 'timeout') {
    loadComponent.value = defineAsyncComponent({
      loader: () => new Promise(resolve => setTimeout(resolve, 5000)),
      timeout: 2000
    })
  }
}

// Error boundary
onErrorCaptured((error) => {
  console.error('Error captured:', error)
  errorMessage.value = `Component error: ${error.message}`
  hasError.value = true
  return false // Prevent error from propagating
})
</script>

<template>
  <div class="error-boundary-demo">
    <h2>Error Handling dengan Async Components</h2>

    <div class="test-controls">
      <h4>Test Scenarios:</h4>
      <div class="button-group">
        <button @click="tryLoad('good')" class="good-btn">
          ✅ Load Good Component
        </button>
        <button @click="tryLoad('bad')" class="bad-btn">
          ❌ Load Bad Component
        </button>
        <button @click="tryLoad('timeout')" class="timeout-btn">
          ⏱️ Load with Timeout
        </button>
      </div>
    </div>

    <div class="component-area">
      <div v-if="hasError" class="error-display">
        <h3>⚠️ Error Occurred</h3>
        <p>{{ errorMessage }}</p>
        <button @click="hasError = false" class="retry-btn">
          Try Again
        </button>
      </div>

      <div v-else-if="loadComponent" class="suspense-area">
        <Suspense>
          <template #default>
            <component :is="loadComponent" />
          </template>
          <template #fallback>
            <div class="loading-state">
              <p>Loading component...</p>
            </div>
          </template>
        </Suspense>
      </div>

      <div v-else class="placeholder">
        <p>Select a test scenario to load a component</p>
      </div>
    </div>

    <div class="error-strategies">
      <h4>🛡️ Error Handling Strategies:</h4>
      <div class="strategy-grid">
        <div class="strategy-item">
          <h5>Async Component Error</h5>
          <p>Handle errors dalam defineAsyncComponent</p>
        </div>
        <div class="strategy-item">
          <h5>Suspense Fallback</h5>
          <p>Fallback component untuk loading states</p>
        </div>
        <div class="strategy-item">
          <h5>Error Boundary</h5>
          <p>onErrorCaptured untuk catch errors</p>
        </div>
        <div class="strategy-item">
          <h5>User Feedback</h5>
          <p>Clear error messages dan retry options</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.error-boundary-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #ef4444;
  border-radius: 8px;
}

.test-controls h4 {
  margin-top: 0;
  color: #991b1b;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.good-btn, .bad-btn, .timeout-btn {
  padding: 10px 15px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.3s ease;
}

.good-btn {
  background: #10b981;
  color: white;
}

.bad-btn {
  background: #ef4444;
  color: white;
}

.timeout-btn {
  background: #f59e0b;
  color: white;
}

.good-btn:hover { background: #059669; }
.bad-btn:hover { background: #dc2626; }
.timeout-btn:hover { background: #d97706; }

.component-area {
  min-height: 200px;
  margin-bottom: 30px;
}

.error-display {
  padding: 20px;
  background: #fef2f2;
  border: 2px solid #fecaca;
  border-radius: 8px;
  text-align: center;
  color: #991b1b;
}

.error-display h3 {
  margin-top: 0;
  color: #dc2626;
}

.retry-btn {
  margin-top: 15px;
  padding: 8px 16px;
  background: #dc2626;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.suspense-area {
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
}

.loading-state {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100px;
  color: #64748b;
  font-style: italic;
}

.placeholder {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100px;
  color: #9ca3af;
  font-style: italic;
  background: #f8fafc;
  border-radius: 8px;
}

.error-strategies h4 {
  margin-top: 0;
  color: #991b1b;
}

.strategy-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.strategy-item {
  background: #fef2f2;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ef4444;
}

.strategy-item h5 {
  margin-top: 0;
  margin-bottom: 5px;
  color: #dc2626;
  font-size: 14px;
}

.strategy-item p {
  margin: 0;
  color: #7f1d1d;
  font-size: 12px;
}
</style>
```

## 🔧 Advanced Async Patterns

### Dynamic Import dengan Parameters

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

const theme = ref('light')
const userRole = ref('user')

// Dynamic async component dengan parameters
const loadThemedComponent = (themeName) => {
  return defineAsyncComponent(() =>
    import(`./components/themes/${themeName}/ThemeComponent.vue`)
  )
}

const loadRoleBasedComponent = (role) => {
  return defineAsyncComponent(() =>
    import(`./components/roles/${role}/DashboardComponent.vue`)
  )
}

const currentThemeComponent = computed(() => {
  return loadThemedComponent(theme.value)
})

const currentRoleComponent = computed(() => {
  return loadRoleBasedComponent(userRole.value)
})

const changeTheme = (newTheme) => {
  theme.value = newTheme
}

const changeRole = (newRole) => {
  userRole.value = newRole
}
</script>

<template>
  <div class="dynamic-async-demo">
    <h2>Dynamic Async Component Loading</h2>

    <div class="controls-section">
      <div class="control-group">
        <h4>Theme Selection:</h4>
        <div class="theme-buttons">
          <button
            @click="changeTheme('light')"
            :class="{ active: theme === 'light' }"
            class="theme-btn light"
          >
            ☀️ Light
          </button>
          <button
            @click="changeTheme('dark')"
            :class="{ active: theme === 'dark' }"
            class="theme-btn dark"
          >
            🌙 Dark
          </button>
          <button
            @click="changeTheme('colorful')"
            :class="{ active: theme === 'colorful' }"
            class="theme-btn colorful"
          >
            🌈 Colorful
          </button>
        </div>
      </div>

      <div class="control-group">
        <h4>User Role:</h4>
        <div class="role-buttons">
          <button
            @click="changeRole('user')"
            :class="{ active: userRole === 'user' }"
            class="role-btn"
          >
            👤 User
          </button>
          <button
            @click="changeRole('admin')"
            :class="{ active: userRole === 'admin' }"
            class="role-btn"
          >
            👑 Admin
          </button>
          <button
            @click="changeRole('moderator')"
            :class="{ active: userRole === 'moderator' }"
            class="role-btn"
          >
            🛡️ Moderator
          </button>
        </div>
      </div>
    </div>

    <div class="components-display">
      <div class="component-section">
        <h3>Theme Component:</h3>
        <Suspense>
          <template #default>
            <component :is="currentThemeComponent" />
          </template>
          <template #fallback>
            <div class="component-loading">
              <p>Loading {{ theme }} theme...</p>
            </div>
          </template>
        </Suspense>
      </div>

      <div class="component-section">
        <h3>Role Dashboard:</h3>
        <Suspense>
          <template #default>
            <component :is="currentRoleComponent" />
          </template>
          <template #fallback>
            <div class="component-loading">
              <p>Loading {{ userRole }} dashboard...</p>
            </div>
          </template>
        </Suspense>
      </div>
    </div>

    <div class="advanced-info">
      <h4>🚀 Advanced Features:</h4>
      <ul>
        <li>Dynamic import dengan template literals</li>
        <li>Component loading berdasarkan parameters</li>
        <li>Code splitting otomatis per theme/role</li>
        <li>Reactive component switching</li>
        <li>Optimal memory usage dengan lazy loading</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.dynamic-async-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.controls-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 30px;
}

.control-group h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #0c4a6e;
}

.theme-buttons, .role-buttons {
  display: flex;
  gap: 8px;
}

.theme-btn, .role-btn {
  padding: 8px 12px;
  border: 2px solid #bae6fd;
  background: white;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 12px;
}

.theme-btn.active, .role-btn.active {
  background: #0891b2;
  color: white;
  border-color: #0891b2;
}

.theme-btn.light.active { background: #fbbf24; border-color: #fbbf24; }
.theme-btn.dark.active { background: #374151; border-color: #374151; }
.theme-btn.colorful.active { background: #8b5cf6; border-color: #8b5cf6; }

.components-display {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 30px;
}

.component-section {
  background: #f0f9ff;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #bae6fd;
}

.component-section h3 {
  margin-top: 0;
  margin-bottom: 15px;
  color: #0c4a6e;
  text-align: center;
}

.component-loading {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 80px;
  background: #e0f2fe;
  border-radius: 6px;
  color: #075985;
  font-style: italic;
}

.advanced-info {
  background: #ecfeff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #06b6d4;
}

.advanced-info h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.advanced-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.advanced-info li {
  margin: 5px 0;
  color: #075985;
}
</style>
```

## 📊 Performance Benefits

### Bundle Size Comparison

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

const showAnalysis = ref(false)
const bundleData = ref({
  sync: {
    size: '2.4 MB',
    loadTime: '3.2s',
    components: 15
  },
  async: {
    size: '450 KB',
    loadTime: '0.8s',
    components: 3,
    lazyComponents: 12
  }
})

// Heavy async component untuk demonstrasi
const AnalyticsComponent = defineAsyncComponent(() =>
  import('./components/AnalyticsComponent.vue')
)

const ChartComponent = defineAsyncComponent(() =>
  import('./components/ChartComponent.vue')
)

const ReportsComponent = defineAsyncComponent(() =>
  import('./components/ReportsComponent.vue')
)

const availableComponents = [
  { name: 'Analytics', component: AnalyticsComponent },
  { name: 'Charts', component: ChartComponent },
  { name: 'Reports', component: ReportsComponent }
]

const loadedComponents = ref([])

const loadComponent = async (comp) => {
  const start = performance.now()

  // Simulate loading dan track performance
  try {
    await comp.component
    const loadTime = performance.now() - start

    loadedComponents.value.push({
      name: comp.name,
      loadTime: `${loadTime.toFixed(2)}ms`,
      timestamp: new Date().toLocaleTimeString()
    })
  } catch (error) {
    console.error('Failed to load component:', error)
  }
}
</script>

<template>
  <div class="performance-demo">
    <h2>Performance Benefits</h2>

    <div class="toggle-analysis">
      <button @click="showAnalysis = !showAnalysis" class="analysis-btn">
        {{ showAnalysis ? 'Hide' : 'Show' }} Bundle Analysis
      </button>
    </div>

    <div v-if="showAnalysis" class="bundle-comparison">
      <h3>📊 Bundle Size Comparison</h3>

      <div class="comparison-grid">
        <div class="comparison-card sync">
          <h4>🔴 Sync Loading</h4>
          <div class="metric">
            <span class="label">Bundle Size:</span>
            <span class="value">{{ bundleData.sync.size }}</span>
          </div>
          <div class="metric">
            <span class="label">Load Time:</span>
            <span class="value">{{ bundleData.sync.loadTime }}</span>
          </div>
          <div class="metric">
            <span class="label">All Components:</span>
            <span class="value">{{ bundleData.sync.components }}</span>
          </div>
        </div>

        <div class="comparison-card async">
          <h4>✅ Async Loading</h4>
          <div class="metric">
            <span class="label">Initial Bundle:</span>
            <span class="value">{{ bundleData.async.size }}</span>
          </div>
          <div class="metric">
            <span class="label">Initial Load:</span>
            <span class="value">{{ bundleData.async.loadTime }}</span>
          </div>
          <div class="metric">
            <span class="label">Core Components:</span>
            <span class="value">{{ bundleData.async.components }}</span>
          </div>
          <div class="metric">
            <span class="label">Lazy Components:</span>
            <span class="value">{{ bundleData.async.lazyComponents }}</span>
          </div>
        </div>
      </div>

      <div class="improvement-stats">
        <h4>🚀 Performance Improvements:</h4>
        <div class="stats-grid">
          <div class="stat-item">
            <span class="stat-value">81% ↓</span>
            <span class="stat-label">Bundle Size</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">75% ↓</span>
            <span class="stat-label">Load Time</span>
          </div>
          <div class="stat-item">
            <span class="stat-value">4x ↑</span>
            <span class="stat-label">Loading Speed</span>
          </div>
        </div>
      </div>
    </div>

    <div class="component-testing">
      <h3>🧪 Test Async Loading</h3>
      <p>Click buttons below to see async components loading in real-time:</p>

      <div class="component-buttons">
        <button
          v-for="comp in availableComponents"
          :key="comp.name"
          @click="loadComponent(comp)"
          :disabled="loadedComponents.some(c => c.name === comp.name)"
          class="load-component-btn"
        >
          {{ comp.name }}
        </button>
      </div>

      <div v-if="loadedComponents.length > 0" class="loading-log">
        <h4>📋 Loading Log:</h4>
        <div
          v-for="(comp, index) in loadedComponents"
          :key="index"
          class="log-entry"
        >
          <span class="component-name">{{ comp.name }}</span>
          <span class="load-time">{{ comp.loadTime }}</span>
          <span class="timestamp">{{ comp.timestamp }}</span>
        </div>
      </div>
    </div>

    <div class="benefits-summary">
      <h4>💡 Key Benefits:</h4>
      <ul>
        <li><strong>Faster Initial Load:</strong> Users see content faster</li>
        <li><strong>Reduced Bandwidth:</strong> Only load what's needed</li>
        <li><strong>Better UX:</strong> Progressive loading experience</li>
        <li><strong>SEO Friendly:</strong> Faster page loads improve rankings</li>
        <li><strong>Memory Efficient:</strong> Components loaded only when used</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.performance-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.toggle-analysis {
  text-align: center;
  margin-bottom: 20px;
}

.analysis-btn {
  padding: 10px 20px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.analysis-btn:hover {
  background: #059669;
}

.bundle-comparison {
  margin-bottom: 30px;
}

.bundle-comparison h3 {
  text-align: center;
  color: #065f46;
  margin-bottom: 20px;
}

.comparison-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin-bottom: 30px;
}

.comparison-card {
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.comparison-card.sync {
  background: #fef2f2;
  border: 2px solid #fecaca;
}

.comparison-card.async {
  background: #f0fdf4;
  border: 2px solid #bbf7d0;
}

.comparison-card h4 {
  margin-top: 0;
  margin-bottom: 15px;
}

.comparison-card.sync h4 {
  color: #dc2626;
}

.comparison-card.async h4 {
  color: #16a34a;
}

.metric {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 8px 12px;
  background: white;
  border-radius: 4px;
}

.label {
  font-weight: 500;
  color: #374151;
}

.value {
  font-weight: bold;
  color: #1f2937;
}

.improvement-stats h4 {
  text-align: center;
  color: #065f46;
  margin-bottom: 15px;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 15px;
}

.stat-item {
  text-align: center;
  padding: 15px;
  background: #dcfce7;
  border-radius: 8px;
}

.stat-value {
  display: block;
  font-size: 24px;
  font-weight: bold;
  color: #16a34a;
  margin-bottom: 5px;
}

.stat-label {
  font-size: 12px;
  color: #15803d;
}

.component-testing {
  margin-bottom: 30px;
}

.component-testing h3 {
  color: #374151;
  margin-bottom: 10px;
}

.component-testing p {
  color: #6b7280;
  margin-bottom: 15px;
}

.component-buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.load-component-btn {
  padding: 8px 16px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

.load-component-btn:hover:not(:disabled) {
  background: #2563eb;
}

.load-component-btn:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.loading-log {
  background: #f8fafc;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.loading-log h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #374151;
}

.log-entry {
  display: grid;
  grid-template-columns: 1fr auto auto;
  gap: 10px;
  padding: 8px;
  margin: 5px 0;
  background: white;
  border-radius: 4px;
  font-family: monospace;
  font-size: 12px;
}

.component-name {
  font-weight: bold;
  color: #1f2937;
}

.load-time {
  color: #059669;
}

.timestamp {
  color: #6b7280;
}

.benefits-summary {
  background: #f0fdf4;
  padding: 20px;
  border-radius: 8px;
  border-left: 4px solid #10b981;
}

.benefits-summary h4 {
  margin-top: 0;
  color: #065f46;
}

.benefits-summary ul {
  margin: 15px 0;
  padding-left: 20px;
}

.benefits-summary li {
  margin: 8px 0;
  color: #047857;
}

.benefits-summary strong {
  color: #065f46;
}
</style>
```

## 🚀 Best Practices

### 1. Gunakan untuk Component Berat

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// ✅ Good: Heavy components dengan async loading
const ChartComponent = defineAsyncComponent(() =>
  import('./components/HeavyChartComponent.vue')
)

const DashboardComponent = defineAsyncComponent(() =>
  import('./components/ComplexDashboard.vue')
)

// ❌ Bad: Simple components tidak perlu async
const SimpleButton = defineAsyncComponent(() =>
  import('./components/SimpleButton.vue') // Overkill!
)
</script>
```

### 2. Error Handling yang Baik

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// ✅ Good: Error handling dengan fallback
const RobustComponent = defineAsyncComponent({
  loader: () => import('./components/ImportantComponent.vue'),
  loadingComponent: LoadingSpinner,
  errorComponent: ErrorDisplay,
  delay: 200,
  timeout: 10000
})
</script>
```

### 3. Gunakan Suspense untuk Loading States

```vue
<template>
  <!-- ✅ Good: Suspense untuk loading states -->
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <LoadingSpinner />
    </template>
  </Suspense>
</template>
```

### 4. Bundle Splitting yang Strategis

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// ✅ Good: Group related components
const AdminComponents = defineAsyncComponent(() =>
  import(/* webpackChunkName: "admin" */ './components/admin/AdminPanel.vue')
)

const UserComponents = defineAsyncComponent(() =>
  import(/* webpackChunkName: "user" */ './components/user/UserDashboard.vue')
)
</script>
```

## 💡 Tips dan Trik

### 1. Preload Critical Components

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

// Preload component saat idle
const preloadComponent = () => {
  import('./components/CriticalComponent.vue')
}

// Preload saat user interaction
const handleMouseEnter = () => {
  preloadComponent()
}
</script>

<template>
  <div @mouseenter="handleMouseEnter">
    <AsyncComponent />
  </div>
</template>
```

### 2. Dynamic Import dengan Fallback

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

const loadComponent = (componentName) => {
  return defineAsyncComponent({
    loader: () => import(`./components/${componentName}.vue`)
      .catch(() => import('./components/FallbackComponent.vue')),
    loadingComponent: LoadingSpinner
  })
}
</script>
```

### 3. Conditional Loading dengan Caching

```vue
<script setup>
import { ref, defineAsyncComponent } from 'vue'

const componentCache = new Map()

const getCachedComponent = (name) => {
  if (!componentCache.has(name)) {
    componentCache.set(name, defineAsyncComponent(() =>
      import(`./components/${name}.vue`)
    ))
  }
  return componentCache.get(name)
}
</script>
```

---

## 🎉 Kesimpulan

Async Components adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **⚡ Performance Optimization:** Mengurangi initial bundle size
2. **🚀 Faster Loading:** Load time yang lebih cepat
3. **📦 Code Splitting:** Otomatis memisahkan kode
4. **🎯 Lazy Loading:** Load komponen sesuai kebutuhan
5. **📱 Better UX:** Pengalaman user yang lebih baik

### Poin Penting:

- Gunakan `defineAsyncComponent()` untuk async loading
- Suspense untuk handling loading states
- Error handling untuk failed loads
- Strategic component splitting
- Performance monitoring dan optimization

### Kapan Menggunakan Async Components:

- ✅ Large/complex components
- ✅ Route-based components
- ✅ Modal/popup components
- ✅ Admin panel components
- ✅ Report/chart components

### Kapan Menghindari:

- ❌ Small/simple components
- ❌ Critical above-the-fold components
- ❌ Frequently accessed components
- ❌ Components dengan minimal dependencies

Dengan memahami async components, Anda sudah bisa membangun aplikasi Vue.js yang performa dan user-friendly! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan async components dalam berbagai skenario untuk melihat perbedaan performa secara langsung!