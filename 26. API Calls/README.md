# 🌐 API Calls - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara melakukan API calls (HTTP requests) dalam Vue.js untuk mengambil data dari server dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu API Calls?](#-apa-itu-api-calls)
- [🎮 Cara Menggunakan API Calls](#-cara-menggunakan-api-calls)
- [⚡ Fetch API](#-fetch-api)
- [🔧 Axios vs Fetch](#axios-vs-fetch)
- [🔄 Async/Await Pattern](#-asyncawait-pattern)
- [📊 Error Handling](#-error-handling)
- [🎯 Advanced API Patterns](#-advanced-api-patterns)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu API Calls?

**API Calls** adalah proses permintaan data dari server atau eksternal API (Application Programming Interface) ke aplikasi Vue.js. Ini memungkinkan aplikasi untuk berkomunikasi dengan backend services dan mengambil/mengirim data secara dinamis.

### Konsep Dasar API Calls

```javascript
// API Call Process:
// 1. Request (Client → Server)
// 2. Processing (Server)
// 3. Response (Server → Client)
// 4. Update UI (Vue.js Reactive)
```

### Mengapa API Calls Penting?

1. **🌐 Data Fetching:** Mengambil data dari database atau external services
2. **📤 Data Submission:** Mengirim form data ke server
3. **🔄 Real-time Updates:** Mendapatkan data real-time dari server
4. **🎯 Dynamic Content:** Update content tanpa page reload
5. **📱 External Integration:** Integrasi dengan third-party services

### Jenis-jenis API Calls

| Method | Kegunaan | Example |
|--------|----------|---------|
| **GET** | Mengambil data | `/users`, `/products` |
| **POST** | Membuat data | `/users`, `/orders` |
| **PUT** | Update data | `/users/1` |
| **PATCH** | Update partial | `/users/1` |
| **DELETE** | Hapus data | `/users/1` |

## 🎮 Cara Menggunakan API Calls

### 1. Import Libraries

```vue
<script setup>
// Import fetch (built-in)
// Import axios (third-party library)
import axios from 'axios'
</script>
```

### 2. Make Request

```vue
<script setup>
// Using fetch
const fetchData = async () => {
  const response = await fetch('/api/data')
  const data = await response.json()
}

// Using axios
const fetchData = async () => {
  const response = await axios.get('/api/data')
  const data = response.data
}
</script>
```

### 3. Handle Response

```vue
<script setup>
const data = ref(null)
const error = ref(null)

const fetchData = async () => {
  try {
    const response = await fetch('/api/data')
    if (!response.ok) throw new Error('Network response was not ok')
    data.value = await response.json()
  } catch (err) {
    error.value = err.message
  }
}
</script>
```

## ⚡ Fetch API

### Basic Fetch Usage

```vue
<script setup>
import { ref } from 'vue'

const data = ref(null)
const loading = ref(false)
const error = ref(null)

const fetchData = () => {
  loading.value = true
  error.value = null

  // Make GET request
  fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then((response) => {
      // Check response status
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`)
      }

      // Parse JSON response
      return response.json()
    })
    .then((responseData) => {
      // Update reactive data
      data.value = responseData
    })
    .catch((err) => {
      // Handle errors
      error.value = err.message
      console.error('Error fetching data:', err)
    })
    .finally(() => {
      // Always executed
      loading.value = false
    })
}
</script>

<template>
  <div class="fetch-demo">
    <h2>Using Fetch API</h2>

    <div class="controls">
      <button @click="fetchData" :disabled="loading">
        {{ loading ? 'Loading...' : 'Fetch Data' }}
      </button>
    </div>

    <div class="result">
      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>Loading data...</p>
      </div>

      <div v-else-if="error" class="error">
        <h3>❌ Error:</h3>
        <p>{{ error }}</p>
        <button @click="fetchData">Retry</button>
      </div>

      <div v-else-if="data" class="success">
        <h3>✅ Success!</h3>
        <div class="data-display">
          <h4>Todo Item:</h4>
          <p><strong>ID:</strong> {{ data.id }}</p>
          <p><strong>Title:</strong> {{ data.title }}</p>
          <p><strong>Completed:</strong> {{ data.completed }}</p>
        </div>
      </div>

      <div v-else class="empty">
        <p>No data yet. Click the button to fetch!</p>
      </div>
    </div>

    <div class="info-box">
      <h4>📝 Fetch API Features:</h4>
      <ul>
        <li><strong>Built-in:</strong> Tidak perlu install library</li>
li><strong>Promise-based:</strong> Modern async/await support</li>
        <strong><strong>Stream support:</strong> Server-Sent Events</li>
        <strong><strong>CORS:</strong> Default CORS handling</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.fetch-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 12px 24px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.controls button:hover:not(:disabled) {
  background: #2563eb;
}

.controls button:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.result {
  min-height: 200px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top: 4px solid #3b82f6;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error {
  text-align: center;
  color: #dc2626;
}

.error h3 {
  margin-top: 0;
  color: #dc2626;
}

.error p {
  margin: 10px 0;
  color: #991b1b;
}

.error button {
  margin-top: 10px;
  padding: 6px 12px;
  background: #dc2626;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.success {
  color: #166534;
}

.success h3 {
  margin-top: 0;
  color: #166534;
}

.data-display {
  background: white;
  padding: 15px;
  border-radius: 6px;
  margin-top: 15px;
  border-left: 4px solid #10b981;
}

.data-display h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #065f46;
}

.data-display p {
  margin: 5px 0;
  color: #374151;
}

.empty {
  text-align: center;
  color: #6b7280;
  font-style: italic;
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
  color: #1e3a8a;
}

.info-box strong {
  color: #1e40af;
}
</style>
```

### Advanced Fetch dengan POST Request

```vue
<script setup>
import { ref } from 'vue'

const formData = ref({
  title: '',
  body: '',
  userId: 1
})

const result = ref(null)
const loading = ref(false)
const error = ref(null)

const submitData = async () => {
  if (!formData.value.title || !formData.value.body) {
    error.value = 'Please fill all fields'
    return
  }

  loading.value = true
  error.value = null

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(formData.value)
    })

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }

    const data = await response.json()
    result.value = data

    // Reset form
    formData.value = { title: '', body: '', userId: 1 }
  } catch (err) {
    error.value = err.message
    console.error('Error submitting data:', err)
  } finally {
    loading.value = false
  }
}

const handleInput = (field, value) => {
  formData.value[field] = value
  error.value = null
}
</script>

<template>
  <div class="post-demo">
    <h2>POST Request with Fetch</h2>

    <div class="form-section">
      <h3>Create New Post</h3>

      <div class="form-group">
        <label for="title">Title:</label>
        <input
          id="title"
          v-model="formData.title"
          @input="handleInput('title', $event.target.value)"
          placeholder="Enter post title"
          class="form-input"
        />
      </div>

      <div class="form-group">
        <label for="body">Body:</label>
        <textarea
          id="body"
          v-model="formData.body"
          @input="handleInput('body', $event.target.value)"
          placeholder="Enter post body"
          class="form-textarea"
        ></textarea>
      </div>

      <div class="form-group">
        <label for="userId">User ID:</label>
        <input
          id="userId"
          v-model.number="formData.userId"
          @input="handleInput('userId', parseInt($event.target.value))"
          type="number"
          class="form-input"
        />
      </div>

      <button @click="submitData" :disabled="loading" class="submit-btn">
        {{ loading ? 'Submitting...' : 'Submit Post' }}
      </button>
    </div>

    <div class="result-section">
      <h3>Result:</h3>
      <div v-if="loading" class="loading">
        <p>Submitting post...</p>
      </div>

      <div v-else-if="error" class="error">
        <p><strong>Error:</strong> {{ error }}</p>
        <button @click="submitData">Try Again</button>
      </div>

      <div v-else-if="result" class="success">
        <p><strong>Post Created!</strong></p>
        <div class="post-data">
          <p><strong>ID:</strong> {{ result.id }}</p>
          <p><strong>Title:</strong> {{ result.title }}</p>
          <p><strong>Body:</strong> {{ result.body }}</p>
          <p><strong>User ID:</strong> {{ result.userId }}</p>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>📋 POST Request Pattern:</h4>
      <ol>
        <li><strong>Method:</strong> `method: 'POST'`</li>
        <li><strong>Headers:</strong> `Content-Type: 'application/json'`</li>
        <li><strong>Body:</strong> `JSON.stringify(data)`</li>
        <li><strong>Response:</strong> Parse JSON response</li>
      </ol>
    </div>
  </div>
</template>

<style scoped>
.post-demo {
  padding: 20px;
  max-width: 600px;
  margin: 0 auto;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.form-section {
  margin-bottom: 30px;
  padding: 20px;
  background: #f0fdf4;
  border-radius: 8px;
  border: 1px solid #bbf7d0;
}

.form-section h3 {
  margin-top: 0;
  color: #065f46;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: 500;
  color: #065f46;
}

.form-input, .form-textarea {
  width: 100%;
  padding: 10px;
  border: 2px solid #d1fae5;
  border-radius: 4px;
  font-size: 14px;
  transition: border-color 0.3s;
}

.form-input:focus, .form-textarea:focus {
  outline: none;
  border-color: #10b981;
}

.form-textarea {
  min-height: 100px;
  resize: vertical;
}

.submit-btn {
  padding: 12px 24px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.submit-btn:hover:not(:disabled) {
  background: #059669;
}

.submit-btn:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.result-section {
  margin-bottom: 20px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.result-section h3 {
  margin-top: 0;
  color: #065f46;
}

.loading, .error, .success {
  text-align: center;
  padding: 20px;
  border-radius: 8px;
}

.loading {
  color: #6b7280;
}

.error {
  color: #dc2626;
}

.success {
  color: #166534;
}

.post-data {
  background: white;
  padding: 15px;
  border-radius: 6px;
  margin-top: 10px;
  border-left: 4px solid #10b981;
}

.post-data p {
  margin: 5px 0;
  color: #374151;
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

.info-box ol {
  margin: 10px 0;
  padding-left: 20px;
}

.info-box li {
  margin: 5px 0;
  color: #047857;
}
</style>
```

## 🔧 Axios vs Fetch

### Perbanding Fetch dan Axios

| Fitur | Fetch | Axios |
|-------|-------|------|
| **Installation** | Built-in | Perlu install |
| **Browser Support** | Modern browsers | Semua browser |
| **JSON Parsing** | Manual | Otomatis |
| **Error Handling** | Manual check | Otomatis |
| **Request/Response** | Manual | Otomatis |
| **Interceptors** | Manual | Built-in |
| **TimeOut** | Manual | Built-in |
| **Cancel Token** | Manual | Built-in |
| **CSRF Protection** | Manual | Built-in |

### Installation Axios

```bash
# Menggunakan npm
npm install axios

# Menggunakan yarn
yarn add axios

# Menggunakan CDN
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### Axios Basic Usage

```vue
<script setup>
import { ref } from 'vue'
import axios from 'axios'

const data = ref(null)
const loading = ref(false)
const error = ref(null)

const fetchData = async () => {
  loading.value = true
  error.value = null

  try {
    // Axios handles JSON parsing automatically
    const response = await axios.get('https://jsonplaceholder.typicode.com/todos/1')
    data.value = response.data

    // Axios provides response metadata
    console.log('Status:', response.status)
    console.log('Headers:', response.headers)
  } catch (err) {
    error.value = err.message
    console.error('Error fetching data:', err)
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="axios-demo">
    <h2>Using Axios</h2>

    <div class="controls">
      <button @click="fetchData" :disabled="loading">
        {{ loading ? 'Loading...' : 'Fetch Data' }}
      </button>
    </div>

    <div class="result">
      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>Loading data with Axios...</p>
      </div>

      <div v-else-if="error" class="error">
        <h3>❌ Error:</h3>
        <p>{{ error }}</p>
        <button @click="fetchData">Retry</button>
      </div>

      <div v-else-if="data" class="success">
        <h3>✅ Success!</h3>
        <div class="data-display">
          <h4>Todo Item:</h4>
          <p><strong>ID:</strong> {{ data.id }}</p>
          <p><strong>Title:</strong> {{ data.title }}</p>
          <p><strong>Completed:</strong> {{ data.completed }}</p>
          <p><strong>User ID:</strong> {{ data.userId }}</p>
        </div>
      </div>

      <div v-else class="empty">
        <p>No data yet. Click the button to fetch!</p>
      </div>
    </div>

    <div class="comparison">
      <h3>Fetch vs Axios Comparison:</h3>
      <div class="comparison-table">
        <div class="table-header">
          <div class="header-cell">Feature</div>
          <div class="header-cell">Fetch</div>
          <div class="header-cell">Axios</div>
        </div>
        <div class="table-row">
          <div class="cell">Installation</div>
          <div class="cell">Built-in</div>
          <div class="cell">npm install axios</div>
        </div>
        <div class="table-row">
          <div class="cell">JSON Parsing</div>
          <div class="cell">response.json()</div>
          <div class="cell">Automatic</div>
        </div>
        <div class="table-row">
          <div class="cell">Error Handling</div>
          <div class="cell">response.ok check</div>
          <div class="cell">try/catch block</div>
        </div>
        <div class="table-row">
          <div class="cell">Headers</div>
          <div class="cell">response.headers</div>
          <div class="cell">response.headers</div>
        </div>
        <div class="table-row">
          <div class="cell">Timeout</div>
   <div class="cell">AbortController</div>
          <div class="cell">timeout config</div>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>🚀 Axios Advantages:</h4>
      <ul>
        <li><strong>Automatic JSON Parsing:</strong> Tidak perlu manual parsing</li>
        <li><strong>Error Handling:</strong> Better error messages</li>
        <li><strong>Request/Response Interceptors:</strong> Built-in support</li>
        <li><strong>Timeout Support:</strong> Konfigurasi mudah</li>
        <li><strong>Cancel Tokens:</strong> Pembatalan request</li>
        <li><strong>CSRF Protection:</strong> Built-in CSRF token</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.axios-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.controls {
  text-align: center;
  margin-bottom: 20px;
}

.controls button {
  padding: 12px 24px;
  background: #8b5cf6;
  color:  white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.controls button:hover:not(:disabled) {
  background: #7c3aed;
}

.controls button:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.result {
  min-height: 200px;
  padding: 20px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
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
  0% { transform: 0deg; }
  100% { transform: 360deg; }
}

.error {
  text-align: center;
  color: #dc2626;
}

.error h3 {
  margin-top: 0;
  color: #dc2626;
}

.error p {
  margin: 10px 0;
  color: #991b1b;
}

.error button {
  margin-top: 10px;
  padding: 6px 12px;
  background: #dc2626;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.success {
  color: #166534;
}

.success h3 {
  margin-top: 0;
  color: #166534;
}

.data-display {
  background: white;
  padding: 15px;
  border-radius: 6px;
  margin-top: 15px;
  border-left: 4px solid #8b5cf6;
}

.data-display h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #6b21a8;
}

.data-display p {
  margin: 5px 0;
  color: #374151;
}

.empty {
  text-align: center;
  color: #6b7280;
  font-style: italic;
}

.comparison {
  margin-bottom: 20px;
}

.comparison h3 {
  text-align: center;
  color: #6b21a8;
  margin-bottom: 15px;
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
  background: #6b21a8;
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
  background: #e9d5ff;
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
  color: #4c1d95;
}

.info-box strong {
  color: #6b21a8;
}
</style>
```

## 🔄 Async/Await Pattern

### Understanding Async/Await

```vue
<script setup>
import { ref } from 'vue'

// ❌ Tanpa async/await (Promise Chaining)
const fetchDataWithoutAsync = () => {
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => {
      console.log(data)
    })
    .catch(error => {
      console.error(error)
    })
}

// ✅ Dengan async/await (Cleaner Code)
const fetchDataWithAsync = async () => {
  try {
    const response = await fetch('https://api.example.com/data')
    const data = await response.json()
    console.log(data)
  } catch (error) {
    console.error(error)
  }
}
</script>
```

### Async/Await dengan Error Handling

```vue
<script setup>
import { ref } from 'vue'

const users = ref([])
const loading = ref(false)
const error = ref(null)

const fetchUsers = async () => {
  loading.value = true
  error.value = null

  try {
    // Simulate network delay
    await new Promise(resolve => setTimeout(resolve, 1000))

    // Fetch users
    const response = await fetch('https://jsonplaceholder.typicode.com/users')

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }

    const data = await response.json()
    users.value = data

    // Filter active users
    const activeUsers = users.value.filter(user => user.status === 'active')
    console.log('Active users:', activeUsers)

  } catch (err) {
    error.value = `Failed to fetch users: ${err.message}`

    // Show user-friendly error message
    console.error('Fetch error:', err)
  } finally {
    loading.value = false
  }
}

const createUser = async (userData) => {
  try {
    const response = await fetch('https://api.example.com/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(userData)
    })

    if (!response.ok) {
      throw new Error('Failed to create user')
    }

    const newUser = await response.json()

    // Add to local array
    users.value.push(newUser)

    return newUser
  } catch (error) {
    console.error('Create user error:', error)
    throw error
  }
}
</script>

<template>
  <div class="async-demo">
    <h2>Async/Await Pattern</h2>

    <div class="user-management">
      <div class="controls">
        <button @click="fetchUsers" :disabled="loading">
          {{ loading ? 'Loading Users...' : 'Fetch Users' }}
        </button>

        <button @click="createNewUser">Create User</button>
      </div>

      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>Loading users...</p>
      </div>

      <div v-else-if="error" class="error">
        <h3>❌ Error:</h3>
        <p>{{ error }}</p>
        <button @click="fetchUsers">Retry</button>
      </div>

      <div v-else>
        <div class="user-list">
          <div
            v-for="user in users"
            :key="user.id"
            class="user-card"
          >
            <div class="user-avatar">
              {{ user.name.charAt(0).toUpperCase() }}
            </div>
            <div class="user-info">
              <h4>{{ user.name }}</h4>
              <p>{{ user.email }}</p>
            </div>
          </div>
        </div>

        <div v-if="users.length === 0" class="empty-state">
          <p>No users found</p>
        </div>
      </div>
    </div>

    <div class="code-example">
      <h3>Async/Await Code Example:</h3>
      <pre><code>const fetchUsers = async () => {
  try {
    loading.value = true

    // Multiple async operations
    const response = await fetch('/api/users')
    const users = await response.json()

    // Process data
    const activeUsers = users.filter(user => user.status === 'active')

    // Update UI
    users.value = activeUsers

    // Another async operation
    const stats = await fetch('/api/stats')
    console.log('Stats:', await stats.json())

  } catch (error) {
    console.error('Error:', error)
  } finally {
    loading.value = false
  }
}</code></pre>
    </div>

    <div class="info-box">
      <h4>🔄 Async/Await Benefits:</h4>
      <ul>
        <li><strong>Readable Code:</strong> Terlihat seperti synchronous code</li>
        <li><strong>Error Handling:</strong> Try/catch blocks</li>
        <li><strong>Sequential Execution:</strong> Operasi berurutan yang jelas</li>
        <li><strong>Resource Management:</strong> Lebih mudah dikelola</li>
        <li><strong>Debugging:** Lebih mudah debug dengan stack traces</li>
      </ul>
    </div>

    <div class="best-practices">
      <h4>💡 Best Practices:</h4>
      <ul>
        <li>Gunakan try/catch untuk error handling</li>
        <li>Gunakan loading states untuk UX yang baik</li>
        <li>Implement retry mechanisms</li>
        <li>Cache responses untuk performance</li>
        <li>Validate data sebelum mengirim</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.async-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.user-management {
  margin-bottom: 30px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.controls button {
  padding: 10px 20px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.controls button:hover:not(:disabled) {
  background: #d97706;
}

.controls button:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #6b7280;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top: 4px solid #f59e0b;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: 0deg; }
  100% { transform: 360deg; }
}

.error {
  text-align: center;
  padding: 20px;
  color: #dc2626;
}

.error h3 {
  margin-top: 0;
  color: #dc2626;
}

.error p {
  margin: 10px 0;
  color: #991b1b;
}

.error button {
  margin-top: 10px;
  padding: 6px 12px;
  background: #dc2626;
  color: 15
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.user-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 15px;
}

.user-card {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 15px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s ease;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.user-avatar {
  width: 40px;
  height: 4
  border-radius: 50%;
  background: #f59e0b;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  font-size: 18px;
}

.user-info h4 {
  margin: 0 0 5px 0;
  color: #1f2937;
}

.user-info p {
  margin: 0;
  color: #4b5563;
  font-size: 14px;
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: #6b7280;
  font-style: italic;
}

.code-example {
  margin-bottom: 30px;
}

.code-example h3 {
  margin-bottom: 15px;
  color: #374151;
}

.code-example pre {
  background: #1e293b;
  color: #e2e8f0;
  padding: 20px;
  border-radius: 8px;
  overflow-x: auto;
}

.code-example code {
  font-family: monospace;
  font-size: 14px;
  line-height: 1.4;
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

.info-box strong {
  color: #92400e;
}

.best-practices {
  background: #f9fafb;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
}

.best-practices h4 {
  margin-top: 0;
  color: #92400e;
}

.best-practices ul {
  margin: 10px 0;
  padding-left: 20px;
}

.best-practices li {
  margin: 5px 0;
  color: #78350f;
}
</style>
```

## 📊 Error Handling

### Comprehensive Error Handling

```vue
<script setup>
import { ref } from 'vue'

const data = ref(null)
const loading = ref(false)
const error = ref(null)

// Error types
const errorTypes = {
  NETWORK: 'Network Error - Failed to connect to server',
  TIMEOUT: 'Timeout Error - Request took too long',
  VALIDATION: 'Validation Error - Invalid data format',
  AUTHORIZATION: 'Authorization Error - Not authenticated',
  SERVER_ERROR: 'Server Error - Server returned error'
}

const fetchData = async () => {
  loading.value = true
  error.value = null

  try {
    // Set timeout
    const controller = new AbortController()
    const timeoutId = setTimeout(() => {
      controller.abort()
    }, 10000)

    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1', {
      signal: controller.signal
    })

    clearTimeout(timeoutId)

    if (!response.ok) {
      // Handle different HTTP status codes
      if (response.status === 404) {
        throw new Error('Resource not found')
      } else if (response.status === 401) {
        throw new Error('Unauthorized access')
      } else if (response.status >= 500) {
        throw new Error('Server error')
      } else {
        throw new Error(`HTTP Error: ${response.status}`)
      }
    }

    const data = await response.json()
    data.value = data
  } catch (err) {
    // Different error types
    if (err.name === 'AbortError') {
      error.value = errorTypes.TIMEOUT
    } else if (err.message.includes('Failed to fetch')) {
      error.value = errorTypes.NETWORK
    } else if (err.message.includes('404')) {
      error.value = errorTypes.VALIDATION
    } else if (err.message.includes('401')) {
      error.value = errorTypes.AUTHORIZATION
    } else {
      error.value = err.message || 'Unknown error occurred'
    }

    console.error('Detailed error:', err)
  } finally {
    loading.value = false
  }
}

const retryRequest = async () => {
  await fetchData()
}
</script>

<template>
  <div class="error-demo">
    <h2>Comprehensive Error Handling</h2>

    <div class="controls">
      <button @click="fetchData" :disabled="loading">
        {{ loading ? 'Loading...' : 'Fetch Data' }}
      </button>
      <button @click="retryRequest" v-if="error">
        Retry
      </button>
    </div>

    <div class="result">
      <div v-if="loading" class="loading">
        <div class="spinner"></div>
        <p>Fetching data...</p>
      </div>

      <div v-else-if="error" class="error">
        <h3>❌ {{ getErrorType(error) }}</h3>
        <p>{{ error }}</p>

        <div class="error-details">
          <h4>Error Details:</h4>
          <ul>
            <li><strong>Type:</strong> {{ getErrorType(error) }}</li>
            <li><strong>Message:</strong> {{ error }}</li>
            <li><strong>Timestamp:</strong> {{ new Date().toLocaleTimeString() }}</li>
          </ul>
        </div>

        <div class="error-actions">
          <button @click="retryRequest">Retry Request</button>
          <button @click="error = null">Clear Error</button>
        </div>
      </div>

      <div v-else-if="data" class="success">
        <h3>✅ Success!</h3>
        <div class="data-display">
          <h4>Todo Item:</h4>
          <p><strong>ID:</strong> {{ data.id }}</p>
          <p><strong>Title:</strong> {{ data.title }}</p>
          <p><strong>Completed:</strong> {{ data.completed }}</p>
        </div>
      </div>

      <div v-else class="empty">
        <p>No data yet. Click the button to fetch!</p>
      </div>
    </div>

    <div class="error-types">
      <h4>📋 Error Types:</h4>
      <div class="error-grid">
        <div class="error-item">
          <span class="error-type">{{ errorTypes.NETWORK }}</span>
          <p>Network connection issues, server down</p>
        </div>
        <div class="error-item">
          <span class="error-type">{{ errorTypes.TIMEOUT }}</span>
          <p>Request takes too long</p>
        </div>
        <div class="error-item">
          <span class="error-type">{{ errorTypes.VALIDATION }}</span>
          <p>Invalid data format or validation</p>
        </div>
        <div class="error-item">
          <span class="error-type">{{ errorTypes.AUTHORIZATION }}</span>
          <p>Authentication issues</p>
        </div>
        <div class="error-item">
          <span class="error-type">{{ errorTypes.SERVER_ERROR }}</span>
          <p>Server-side errors</p>
        </div>
      </div>
    </div>

    <div class="info-box">
      <h4>🛡️ Error Handling Best Practices:</h4>
      <ul>
        <li><strong>User-Friendly Messages:</strong> Show clear error messages to users</li>
        <li><strong>Retry Mechanism:</strong> Provide retry options for temporary errors</li>
        <li><strong>Timeout Handling:</strong> Set appropriate timeouts for requests</li>
        <strong><strong>Status Code Check:</strong> Handle different HTTP status codes</li>
        <strong>Logging:</strong> Log detailed error information</li>
        <li><strong>Fallback Options:</strong> Provide fallback content when possible</li>
      </ul>
    </div>
  </div>
</template>

<script>
// Helper function untuk error type detection
const getErrorType = (error) => {
  if (error.name === 'AbortError') {
    return errorTypes.TIMEOUT
  } else if (error.message?.includes('Failed to fetch')) {
    return errorTypes.NETWORK
  } else if (error.message?.includes('404')) {
    return errorTypes.VALIDATION
  } else if (error.message?.includes('401')) {
    return errorTypes.AUTHORIZATION
  } else if (error.status >= 500) {
    return errorTypes.SERVER_ERROR
  }
  return error.message || 'Unknown error'
}
</script>

<style scoped>
.error-demo {
  padding: 20px;
  max-width: 700px;
  margin: 0 auto;
  border: 2px solid #ef4444;
  border-radius: 8px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.controls button {
  padding: 12px 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.controls button:hover:not(:disabled) {
  background: #dc2626;
}

.controls button:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.result {
  min-height: 200px;
  padding: 20px;
  background: #fef2f2;
  border-radius: 8px;
  border: 1px solid #fecaca;
}

.loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top: 4px solid #ef4444;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

@keyframes spin {
  0% { transform: 0deg; }
  100% { transform: 360deg; }
}

.error {
  text-align: center;
  color: #dc2626;
}

.error h3 {
  margin-top: 0;
  color: #dc2626;
}

.error p {
  margin: 10px 0;
  color: #991b1b;
}

.error-details {
  background: #fee2e2;
  padding: 15px;
  border-radius: 6px;
  margin: 15px 0;
}

.error-details h4 {
  margin-top: 0;
  color: #991b1b;
}

.error-details ul {
  margin: 10px 0;
  padding-left: 20px;
}

.error-details li {
  margin: 5px 0;
  color: #7f1d1d;
}

.error-actions {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.error-actions button {
  padding: 6px 12px;
  background: #dc2626;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.success {
  color: #166534;
}

.success h3 {
  margin-top: 0;
  color: #166534;
}

.data-display {
  background: white;
  padding: 15px;
  border-radius: 6px;
  margin-top: 15px;
  border-left: 4px solid #10b981;
}

.data-display h4 {
  margin-top: 0;
  margin-bottom: 10px;
  color: #065f46;
}

.data-display p {
  margin: 5px 0;
  color: #374151;
}

.empty {
  text-align: center;
  color: #6b7280;
  font-style: italic;
}

.error-types {
  margin-bottom: 30px;
}

.error-types h4 {
  color: #374151;
  margin-bottom: 15px;
}

.error-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.error-item {
  background: #fef2f2;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #ef4444;
}

.error-type {
  font-weight: bold;
  color: #dc2626;
  margin-bottom: 5px;
}

.error-item p {
  margin: 0;
  color: #7f1d1d;
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

## 🎯 Advanced API Patterns

### API Service Composition

```vue
<script setup>
import { ref, reactive } from 'vue'

// API Service class
class ApiService {
  constructor(baseURL = 'https://jsonplaceholder.typicode.com') {
    this.baseURL = baseURL
  }

  // Generic GET request
  async get(endpoint, params = {}) {
    const url = new URL(endpoint, this.baseURL)
    Object.keys(params).forEach(key => {
      url.searchParams.set(key, params[key])
    })

    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }

    return response.json()
  }

  // GET specific resource
  async getById(endpoint, id) {
    return this.get(`${endpoint}/${id}`)
  }

  // POST request
  async post(endpoint, data) {
    const response = await fetch(`${this.baseURL}${endpoint}`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    })

    if (!response.ok) {
      throw new Error(`Failed to POST: ${response.status}`)
    }

    return response.json()
  }

  // PUT request
  async put(endpoint, id, data) {
    return this.post(`${endpoint}/${id}`, data)
  }

  // DELETE request
  async delete(endpoint, id) {
    const response = await fetch(`${this.baseURL}${endpoint}/${id}`, {
      method: 'DELETE'
    })

    if (!response.ok) {
      throw new Error(`Failed to DELETE: ${response.status}`)
    }

    return response.json()
  }
}

// Composable untuk API service
const useApiService = () => {
  const apiService = new ApiService()

  const users = ref([])
  const loading = ref(false)
  const error = ref(null)

  const fetchUsers = async () => {
    loading.value = true
    error.value = null
    try {
      users.value = await apiService.get('todos')
    } catch (err) {
      error.value = err.message
    } finally {
      loading.value = false
    }
  }

  const createUser = async (userData) => {
    try {
      const newUser = await apiService.post('users', userData)
      users.value.push(newUser)
      return newUser
    } catch (err) {
      throw err
    }
  }

  const updateUser = async (id, userData) => {
    try {
      return await apiService.put(`users/${id}`, userData)
    } catch (err) {
      throw err
    }
  }

  const deleteUser = async (id) => {
    try {
      await apiService.delete(`users/${id}`)
      users.value = users.value.filter(user => user.id !== id)
    } catch (err) {
      throw err
    }
  }

  return {
    users,
    loading,
    error,
    fetchUsers,
    createUser,
    updateUser,
    deleteUser
  }
}

// Gunakan di component
const {
  users,
  loading,
  error,
  fetchUsers,
  createUser,
  updateUser,
  deleteUser
} = useApiService()
</script>

<template>
  <div class="api-service-demo">
    <h2>API Service Pattern</h2>

    <div class="user-stats">
      <div class="stat-item">
        <span class="stat-value">{{ users.length }}</span>
        <span class="stat-label">Total Users</span>
      </div>
      <div class="stat-item">
        <span class="stat-value">{{ loading ? 'Yes' : 'No' }}</span>
        <span class="stat-label">Loading</span>
      </div>
      <div class="stat-item">
        <span class="stat-value">{{ error ? 'Yes' : 'No' }}</span>
        <span class="stat-label">Has Error</span>
      </div>
    </div>

    <div class="user-actions">
      <button @click="fetchUsers" :disabled="loading">
        {{ loading ? 'Loading...' : 'Fetch Users' }}
      </button>

      <button @click="createSampleUser" :disabled="loading">
        Create Sample User
      </button>
    </div>

    <div v-if="loading" class="loading">
      <div class="spinner"></div>
      <p>Loading users...</p>
    </div>

    <div v-else-if="error" class="error">
      <h3>❌ Error: {{ error }}</h3>
      <button @click="fetchUsers">Retry</button>
    </div>

    <div v-else class="user-list">
      <div
        v-for="user in users"
        :key="user.id"
        class="user-card"
      >
        <div class="user-header">
          <h3>{{ user.name }}</h3>
          <span class="user-id">ID: {{ user.id }}</span>
        </div>
        <div class="user-actions">
          <button @click="updateUser(user.id, { name: 'Updated User' })">
            Update
          </button>
          <button @click="deleteUser(user.id)" class="danger">
            Delete
          </button>
        </div>
      </div>

      <div v-if="users.length === 0" class="empty-state">
        <p>No users found</p>
      </div>
    </div>

    <div class="service-info">
      <h4>🔧 Service Pattern Benefits:</h4>
      <ul>
        <li><strong>Centralized Logic:</strong> Semua API logic di satu tempat</li>
        <strong><strong>Reusable:</strong> Dapat digunakan di multiple components</li>
        <strong><strong>Testable:</strong> Mudah di-unit test</li>
        <strong>State Management:</strong> Reactive state management</li>
        <strong>Error Handling:</strong> Centralized error handling</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.api-service-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #06b6d4;
  border-radius: 8px;
}

.user-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 30px;
  padding: 20px;
  background: #f0f9ff;
  border-radius: 8px;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  background: white;
  border-radius: 6px;
  border-left: 4px solid #06b6d4;
}

.stat-value {
  font-weight: bold;
  color: #0c4a6e;
}

.stat-label {
  color: #0c4a6e;
}

.user-actions {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
}

.user-actions button {
  padding: 8px 16px;
  background: #06b6d4;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.user-actions button:hover:not(:disabled) {
  background: #0891b2;
}

.user-actions button.danger {
  background: #ef4444;
}

.user-actions button.danger:hover {
  background: #dc2626;
}

.loading {
  text-align: center;
  padding: 40px;
  color: #6b7280;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e5e7eb;
  border-top: 4px solid #06b6d4;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-bottom: 15px;
}

.user-list {
  margin-bottom: 20px;
}

.user-card {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin-bottom: 15px;
  transition: transform 0.2s ease;
}

.user-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.user-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.user-header h3 {
  margin: 0;
  color: #374151;
}

.user-id {
  font-family: monospace;
  font-size: 12px;
  color: #6b7280;
}

.user-actions {
  display: flex;
  gap: 5px;
}

.danger {
  background: #ef4444;
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: #6b7280;
  font-style: italic;
}

.service-info {
  background: #e0f2fe;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #06b6d4;
}

.service-info h4 {
  margin-top: 0;
  color: #0c4a6e;
  margin-bottom: 10px;
}

.service-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.service-info li {
  margin: 5px 0;
  color: #075985;
}
</style>
```

### Request/Response Interceptors

```vue
<script setup>
import { ref } from 'vue'
import axios from 'axios'

// Request interceptor
const setupInterceptors = () => {
  // Request interceptor
  axios.interceptors.request.use(
    (config) => {
      // Add auth token
      const token = localStorage.getItem('authToken')
      if (token) {
        config.headers.Authorization = `Bearer ${token}`
      }

      // Add request timestamp
      config.metadata = {
        ...config.metadata,
        requestTimestamp: new Date().toISOString()
      }

      console.log('Request:', config)
      return config
    },
    (error) => {
      console.error('Request error:', error)
      return Promise.reject(error)
    }
  )

  // Response interceptor
  axios.interceptors.response.use(
    (response) => {
      // Log response
      console.log('Response:', response)

      // Transform response data
      if (response.data && response.data.items) {
        response.data.processedItems = response.data.items.map(item => ({
          ...item,
          processed: true
        }))
      }

      return response
    },
    (error) => {
      console.error('Response error:', error)
      return Promise.reject(error)
    }
  )
}

// Setup interceptors on mount
setupInterceptors()
</script>

const data = ref(null)
const loading = ref(false)

const fetchData = async () => {
  loading.value = true
  error.value = null

  try {
    // Request akan melalui interceptor
    const response = await axios.get('/api/data')
    data.value = response.data
  } catch (error) {
    error.value = error.message
  } finally {
    loading.value = false
  }
}
</script>
```

## 🚀 Best Practices

### 1. Environment Configuration

```vue
<script setup>
// ✅ Environment-based API configuration
const apiBaseURL = import.meta.env.VITE_API_URL || 'http://localhost:3000/api'

const apiClient = axios.create({
  baseURL: apiBaseURL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
})
</script>
```

### 2. Request Caching

```vue
<script setup>
// ✅ Simple cache dengan Map
const cache = new Map()

const cachedFetch = async (url, options = {}) => {
  const cacheKey = `${url}:${JSON.stringify(options)}`

  // Return cached data if available
  if (cache.has(cacheKey)) {
    return Promise.resolve(cache.get(cacheKey))
  }

  // Fetch and cache the result
  try {
    const response = await fetch(url, options)
    const data = await response.json()
    cache.set(cacheKey, data)
    return data
  } catch (error) {
    // Remove invalid cache
    cache.delete(cacheKey)
    throw error
  }
}
</script>
```

### 3. Request Debouncing

```vue
<script setup>
import { ref, debounce } from 'vue'

const searchQuery = ref('')
const searchResults = ref([])
const loading = ref(false)

// Debounced search function
const debouncedSearch = debounce(async (query) => {
  loading.value = true

  try {
    const response = await fetch(`/api/search?q=${query}`)
    const data = await response.json()
    searchResults.value = data.results
  } catch (error) {
    console.error('Search error:', error)
  } finally {
    loading.value = false
  }
}, 500) // 500ms debounce delay

const handleSearch = () => {
  debouncedSearch(searchQuery.value)
}
</script>

<template>
  <div class="debounce-demo">
    <input
      v-model="searchQuery"
      @input="handleSearch"
      placeholder="Search..."
      class="search-input"
    />

    <div v-if="loading" class="loading">
      Searching...
    </div>

    <div v-if="searchResults.length > 0" class="results">
      <div
        v-for="result in searchResults"
        :key="result.id"
        class="result-item"
      >
        {{ result.title }}
      </div>
    </div>
  </div>
</template>
```

### 4. Retry Logic

```vue
<script setup>
import { ref } from 'vue'

const data = ref(null)
const loading = ref(false)
const retryCount = ref(0)
const maxRetries = 3

const fetchData = async (retryCount = 0) => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos/1')

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }

    return response.json()
  } catch (error) {
      if (retryCount < maxRetries) {
        console.log(`Retry ${retryCount + 1}/${maxRetries}`)
        // Wait before retry
        await new Promise(resolve => setTimeout(resolve, 1000 * retryCount))
        return fetchData(retryCount + 1)
      }

      throw error
    }
  }
}

const fetchDataWithRetry = async () => {
  loading.value = true
  error.value = null
  retryCount.value = 0

  try {
    data.value = await fetchData(0)
  } catch (error) {
    error.value = `Failed after ${maxRetries} attempts`
    console.error('All retry attempts failed')
  } finally {
    loading.value = false
  }
}
</script>
```

---

## 💡 Tips dan Trik

### 1. Request/Response Transformation

```vue
<script setup>
// Transform response data
const transformResponse = (response) => {
  return {
    data: response.data,
    meta: {
      timestamp: response.headers['date'],
      processed: true
    }
  }
}

const fetchData = async () => {
  const response = await fetch('/api/data')
  const transformed = transformResponse(response)
  data.value = transformed.data
}
</script>
```

### 2. Multiple Concurrent Requests

```vue
<script setup>
const fetchAllData = async () => {
  const [users, posts, comments] = await Promise.all([
    fetch('/api/users'),
    fetch('/api/posts'),
    fetch('/api/comments')
  ])

  return {
    users: await users.json(),
    posts: await posts.json(),
    comments: await comments.json()
  }
}
</script>
```

### 3. Request Prioritization

```vue
<script setup>
const fetchWithPriority = async () => {
  // High priority: User data
  const userData = await fetch('/api/user')

  // Medium priority: Posts
  const postsData = await fetch('/api/posts')

  // Low priority: Comments
  const commentsData = await fetch('/api/comments')

  return {
    user: userData,
    posts: postsData,
    comments: commentsData
  }
}
</script>
```

---

## 🎉 Kesimpulan

API Calls adalah fitur fundamental dalam Vue.js yang memungkinkan aplikasi untuk berkomunikasi dengan server:

1. **🌐 Data Fetching:** Mengambil data dari database atau external services
2. **📤 Data Submission:** Mengirim form data ke server
3. **🔄 Real-time Updates:** Mendapatkan data real-time dari server
4. **📱� Dynamic Content:** Update content tanpa page reload
5. **📱 External Integration:** Integrasi dengan third-party services

### Poin Penting:

- **⚡ Reactive State:** Update UI secara otomatis
- **🔄 Error Handling:** Handle errors dengan baik
- **🚀 Performance:** Implement caching dan optimization
- **🔧 Security:** Implement authentication dan authorization
- **📊 Type Safety:** Validasi tipe data dengan TypeScript

### Kapan Menggunakan API Calls:

- **✅ Dashboard Systems:** Data dashboard dengan real-time updates
- **E-commerce:** Product catalogs, shopping carts, order management
- **Social Media:** Posts, comments, user interactions
- **Form Applications:** Contact forms, surveys, registrations
- **Data Visualization:** Charts, analytics, reports

### Kapan Menghindari:

- ❌ Static content yang tidak perlu di-update
- ❌ Simple state management (gunakan reactive refs instead)
- ❌ Client-side only computations
- ❌ Synchronous operations yang blocking

Dengan memahami API calls dengan baik, Anda sudah bisa membuat aplikasi Vue.js yang terhubung dengan backend services! 🚀

---

**🎯 Ready for Practice?** Coba implementasikan berbagai API patterns dalam aplikasi Anda untuk melihat kinerja langsung!