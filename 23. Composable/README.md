# 🧩 Composable - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja composables untuk logic reuse dan code organization dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Composable?](#-apa-itu-composable)
- [🎮 Cara Menggunakan Composable](#-cara-menggunakan-composable)
- [⚡ Basic Composable](#-basic-composable)
- [🔄 Logic Reuse Pattern](#-logic-reuse-pattern)
- [🎯 Advanced Composable](#-advanced-composable)
- [📦 Composable dengan Params](#-composable-dengan-params)
- [🔧 Composable Ecosystem](#-composable-ecosystem)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Composable?

**Composable** adalah fungsi JavaScript yang menggunakan Vue.js Composition API untuk mengelola stateful logic. Composable memungkinkan kita untuk mengekstrak dan reuse logic antar komponen tanpa harus mengulang kode.

### Konsep Dasar Composable

```javascript
// Composable function (biasanya diawali dengan 'use')
import { ref } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)

  const increment = () => count.value++
  const decrement = () => count.value--

  return { count, increment, decrement }
}
```

### Mengapa Composable Penting?

1. **🔄 Logic Reuse:** Gunakan logic yang sama di multiple komponen
2. **📦 Code Organization:** Pisahkan logic dari template
3. **🎯 Single Responsibility:** Setiap composable punya satu tanggung jawab
4. **🧪 Testability:** Logic lebih mudah di-test
5. **📚 Maintainability:** Kode lebih mudah di-maintain

## ❓ Pertanyaan: Apa Fungsi Composable?

Composable memiliki 5 fungsi utama yang sangat penting dalam pengembangan aplikasi Vue.js modern:

### Fungsi 1: Logic Reuse dan Code Organization

Composable memungkinkan kita untuk mengekstrak logic yang dapat digunakan kembali di berbagai komponen, mengurangi duplikasi kode dan meningkatkan organisasi kode.

```javascript
// composables/useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0, step = 1) {
  const count = ref(initialValue)

  const increment = () => count.value += step
  const decrement = () => count.value -= step
  const reset = () => count.value = initialValue

  const isEven = computed(() => count.value % 2 === 0)
  const isPositive = computed(() => count.value > 0)
  const doubled = computed(() => count.value * 2)

  return {
    count,
    increment,
    decrement,
    reset,
    isEven,
    isPositive,
    doubled
  }
}
```

```vue
<!-- Component 1: Counter Display -->
<script setup>
import { useCounter } from './composables/useCounter'

const counter = useCounter(0, 1)
</script>

<template>
  <div class="counter-display">
    <h2>Simple Counter</h2>
    <p>Count: {{ counter.count }}</p>
    <p>Is Even: {{ counter.isEven ? 'Yes' : 'No' }}</p>
    <p>Is Positive: {{ counter.isPositive ? 'Yes' : 'No' }}</p>
    <p>Doubled: {{ counter.doubled }}</p>
    <div class="controls">
      <button @click="counter.increment">+</button>
      <button @click="counter.decrement">-</button>
      <button @click="counter.reset">Reset</button>
    </div>
  </div>
</template>

<style scoped>
.counter-display {
  padding: 20px;
  border: 2px solid #3b82f6;
  border-radius: 8px;
  text-align: center;
}

.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 15px;
}

.controls button {
  padding: 8px 16px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

```vue
<!-- Component 2: Step Counter -->
<script setup>
import { useCounter } from './composables/useCounter'

const stepCounter = useCounter(10, 5)
const customCounter = useCounter(100, 10)
</script>

<template>
  <div class="step-counter-demo">
    <h2>Step Counter dengan Custom Step</h2>

    <div class="counter-section">
      <h3>Step 5 Counter (Start: 10)</h3>
      <p>Count: {{ stepCounter.count }}</p>
      <div class="controls">
        <button @click="stepCounter.increment">+5</button>
        <button @click="stepCounter.decrement">-5</button>
        <button @click="stepCounter.reset">Reset</button>
      </div>
    </div>

    <div class="counter-section">
      <h3>Step 10 Counter (Start: 100)</h3>
      <p>Count: {{ customCounter.count }}</p>
      <div class="controls">
        <button @click="customCounter.increment">+10</button>
        <button @click="customCounter.decrement">-10</button>
        <button @click="customCounter.reset">Reset</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.step-counter-demo {
  padding: 20px;
  border: 2px solid #10b981;
  border-radius: 8px;
}

.counter-section {
  margin-bottom: 20px;
  padding: 15px;
  background: #f0fdf4;
  border-radius: 6px;
}

.controls {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.controls button {
  padding: 6px 12px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
</style>
```

### Fungsi 2: State Management dan Reactive Logic

Composable sangat berguna untuk mengelola state kompleks dan logic reaktif yang dapat digunakan kembali di berbagai komponen.

```javascript
// composables/usePagination.js
import { ref, computed, watch } from 'vue'

export function usePagination(items, itemsPerPage = 10) {
  const currentPage = ref(1)

  const totalPages = computed(() => {
    return Math.ceil(items.value.length / itemsPerPage)
  })

  const paginatedItems = computed(() => {
    const start = (currentPage.value - 1) * itemsPerPage
    const end = start + itemsPerPage
    return items.value.slice(start, end)
  })

  const hasNextPage = computed(() => currentPage.value < totalPages.value)
  const hasPrevPage = computed(() => currentPage.value > 1)

  const nextPage = () => {
    if (hasNextPage.value) currentPage.value++
  }

  const prevPage = () => {
    if (hasPrevPage.value) currentPage.value--
  }

  const goToPage = (page) => {
    if (page >= 1 && page <= totalPages.value) {
      currentPage.value = page
    }
  }

  const goToFirstPage = () => currentPage.value = 1
  const goToLastPage = () => currentPage.value = totalPages.value

  // Reset ke halaman 1 jika items berubah
  watch(items, () => {
    currentPage.value = 1
  })

  return {
    currentPage,
    totalPages,
    paginatedItems,
    hasNextPage,
    hasPrevPage,
    nextPage,
    prevPage,
    goToPage,
    goToFirstPage,
    goToLastPage
  }
}
```

```vue
<script setup>
import { ref } from 'vue'
import { usePagination } from './composables/usePagination'

// Sample data
const products = ref([
  { id: 1, name: 'Laptop', price: 999, category: 'Electronics' },
  { id: 2, name: 'Mouse', price: 29, category: 'Electronics' },
  { id: 3, name: 'Keyboard', price: 79, category: 'Electronics' },
  { id: 4, name: 'Monitor', price: 299, category: 'Electronics' },
  { id: 5, name: 'Book', price: 19, category: 'Books' },
  { id: 6, name: 'Notebook', price: 9, category: 'Books' },
  { id: 7, name: 'Pen', price: 2, category: 'Books' },
  { id: 8, name: 'Backpack', price: 49, category: 'Accessories' },
  { id: 9, name: 'Headphones', price: 149, category: 'Electronics' },
  { id: 10, name: 'Coffee Mug', price: 12, category: 'Accessories' },
  { id: 11, name: 'Desk Lamp', price: 35, category: 'Accessories' },
  { id: 12, name: 'Phone Case', price: 15, category: 'Accessories' },
  { id: 13, name: 'Webcam', price: 89, category: 'Electronics' },
  { id: 14, name: 'Notebook Stand', price: 25, category: 'Accessories' },
  { id: 15, name: 'USB Cable', price: 8, category: 'Electronics' }
])

const itemsPerPage = ref(5)
const pagination = usePagination(products, itemsPerPage)

const changeItemsPerPage = (newSize) => {
  itemsPerPage.value = newSize
}
</script>

<template>
  <div class="pagination-demo">
    <h2>📄 Pagination dengan Composable</h2>

    <div class="controls">
      <label>Items per page:</label>
      <select @change="changeItemsPerPage($event.target.value)" class="items-select">
        <option value="3">3</option>
        <option value="5" selected>5</option>
        <option value="10">10</option>
        <option value="15">15</option>
      </select>
    </div>

    <div class="products-grid">
      <div
        v-for="product in pagination.paginatedItems"
        :key="product.id"
        class="product-card"
      >
        <h4>{{ product.name }}</h4>
        <p class="price">${{ product.price }}</p>
        <span class="category">{{ product.category }}</span>
      </div>
    </div>

    <div class="pagination-info">
      <p>
        Page {{ pagination.currentPage }} of {{ pagination.totalPages }}
        ({{ products.length }} total items)
      </p>
    </div>

    <div class="pagination-controls">
      <button
        @click="pagination.goToFirstPage"
        :disabled="!pagination.hasPrevPage"
        class="page-btn"
      >
        First
      </button>

      <button
        @click="pagination.prevPage"
        :disabled="!pagination.hasPrevPage"
        class="page-btn"
      >
        Previous
      </button>

      <div class="page-numbers">
        <button
          v-for="page in pagination.totalPages"
          :key="page"
          @click="pagination.goToPage(page)"
          :class="{ active: pagination.currentPage === page }"
          class="page-number"
        >
          {{ page }}
        </button>
      </div>

      <button
        @click="pagination.nextPage"
        :disabled="!pagination.hasNextPage"
        class="page-btn"
      >
        Next
      </button>

      <button
        @click="pagination.goToLastPage"
        :disabled="!pagination.hasNextPage"
        class="page-btn"
      >
        Last
      </button>
    </div>
  </div>
</template>

<style scoped>
.pagination-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.controls {
  margin-bottom: 20px;
  text-align: center;
}

.items-select {
  padding: 5px 10px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  margin-left: 10px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.product-card {
  padding: 15px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
  text-align: center;
}

.product-card h4 {
  margin: 0 0 10px 0;
  color: #1f2937;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #059669;
  margin: 5px 0;
}

.category {
  display: inline-block;
  padding: 2px 8px;
  background: #e5e7eb;
  color: #374151;
  border-radius: 12px;
  font-size: 12px;
}

.pagination-info {
  text-align: center;
  margin-bottom: 20px;
  color: #6b7280;
}

.pagination-controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.page-btn {
  padding: 8px 12px;
  background: #f3f4f6;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s;
}

.page-btn:hover:not(:disabled) {
  background: #e5e7eb;
}

.page-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.page-numbers {
  display: flex;
  gap: 5px;
}

.page-number {
  width: 36px;
  height: 36px;
  padding: 0;
  background: #f3f4f6;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
}

.page-number:hover {
  background: #e5e7eb;
}

.page-number.active {
  background: #3b82f6;
  color: white;
  border-color: #3b82f6;
}
</style>
```

### Fungsi 3: API Integration dan Data Fetching

Composable sangat ideal untuk mengelola API calls, data fetching, dan error handling dengan cara yang konsisten dan dapat digunakan kembali.

```javascript
// composables/useApi.js
import { ref, onMounted, onUnmounted } from 'vue'

export function useApi(apiUrl, options = {}) {
  const data = ref(null)
  const loading = ref(false)
  const error = ref(null)
  const controller = ref(null)

  const defaultOptions = {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
    },
    ...options
  }

  const fetchData = async (url = apiUrl, requestOptions = defaultOptions) => {
    if (loading.value) return

    loading.value = true
    error.value = null

    try {
      // Cancel previous request if exists
      if (controller.value) {
        controller.value.abort()
      }

      controller.value = new AbortController()

      const response = await fetch(url, {
        ...requestOptions,
        signal: controller.value.signal
      })

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }

      const result = await response.json()
      data.value = result

      return result
    } catch (err) {
      if (err.name !== 'AbortError') {
        error.value = err.message
        console.error('API Error:', err)
      }
      throw err
    } finally {
      loading.value = false
    }
  }

  const postData = async (payload, url = apiUrl) => {
    return await fetchData(url, {
      ...defaultOptions,
      method: 'POST',
      body: JSON.stringify(payload)
    })
  }

  const putData = async (payload, url = apiUrl) => {
    return await fetchData(url, {
      ...defaultOptions,
      method: 'PUT',
      body: JSON.stringify(payload)
    })
  }

  const deleteData = async (url = apiUrl) => {
    return await fetchData(url, {
      ...defaultOptions,
      method: 'DELETE'
    })
  }

  const cancelRequest = () => {
    if (controller.value) {
      controller.value.abort()
    }
  }

  // Cleanup on unmount
  onUnmounted(() => {
    cancelRequest()
  })

  return {
    data,
    loading,
    error,
    fetchData,
    postData,
    putData,
    deleteData,
    cancelRequest
  }
}
```

```vue
<script setup>
import { ref } from 'vue'
import { useApi } from './composables/useApi'

// API endpoints
const usersApi = useApi('https://jsonplaceholder.typicode.com/users')
const postsApi = useApi('https://jsonplaceholder.typicode.com/posts')
const photosApi = useApi('https://jsonplaceholder.typicode.com/photos')

// State untuk form
const newPost = ref({
  title: '',
  body: '',
  userId: 1
})

const editingUser = ref(null)
const activeTab = ref('users')

// Fetch data
const fetchUsers = () => usersApi.fetchData()
const fetchPosts = () => postsApi.fetchData()
const fetchPhotos = () => photosApi.fetchData()

// Create new post
const createPost = async () => {
  if (!newPost.value.title || !newPost.value.body) {
    alert('Please fill all fields')
    return
  }

  try {
    await postsApi.postData(newPost.value)
    alert('Post created successfully!')
    newPost.value = { title: '', body: '', userId: 1 }
    fetchPosts() // Refresh posts
  } catch (error) {
    alert('Failed to create post')
  }
}

// Update user
const startEditUser = (user) => {
  editingUser.value = { ...user }
}

const updateUser = async () => {
  if (!editingUser.value) return

  try {
    const userId = editingUser.value.id
    await usersApi.putData(editingUser.value, `https://jsonplaceholder.typicode.com/users/${userId}`)
    alert('User updated successfully!')
    editingUser.value = null
    fetchUsers() // Refresh users
  } catch (error) {
    alert('Failed to update user')
  }
}

const cancelEdit = () => {
  editingUser.value = null
}

// Delete operations
const deleteUser = async (userId) => {
  if (!confirm('Are you sure you want to delete this user?')) return

  try {
    await usersApi.deleteData(`https://jsonplaceholder.typicode.com/users/${userId}`)
    alert('User deleted successfully!')
    fetchUsers() // Refresh users
  } catch (error) {
    alert('Failed to delete user')
  }
}

// Load initial data
fetchUsers()
</script>

<template>
  <div class="api-demo">
    <h2>🌐 API Integration dengan Composable</h2>

    <div class="tabs">
      <button
        v-for="tab in ['users', 'posts', 'photos']"
        :key="tab"
        @click="activeTab = tab"
        :class="{ active: activeTab === tab }"
        class="tab-btn"
      >
        {{ tab.charAt(0).toUpperCase() + tab.slice(1) }}
      </button>
    </div>

    <!-- Users Tab -->
    <div v-if="activeTab === 'users'" class="tab-content">
      <div class="actions">
        <button @click="fetchUsers" :disabled="usersApi.loading" class="action-btn">
          {{ usersApi.loading ? 'Loading...' : 'Refresh Users' }}
        </button>
      </div>

      <div v-if="usersApi.error" class="error-message">
        Error: {{ usersApi.error }}
      </div>

      <div v-if="usersApi.loading" class="loading">Loading users...</div>

      <div v-else-if="usersApi.data" class="users-grid">
        <div
          v-for="user in usersApi.data"
          :key="user.id"
          class="user-card"
        >
          <div v-if="editingUser?.id === user.id" class="edit-form">
            <input v-model="editingUser.name" placeholder="Name" class="edit-input" />
            <input v-model="editingUser.email" placeholder="Email" class="edit-input" />
            <input v-model="editingUser.phone" placeholder="Phone" class="edit-input" />
            <div class="edit-actions">
              <button @click="updateUser" class="save-btn">Save</button>
              <button @click="cancelEdit" class="cancel-btn">Cancel</button>
            </div>
          </div>
          <div v-else class="user-info">
            <h4>{{ user.name }}</h4>
            <p>{{ user.email }}</p>
            <p>{{ user.phone }}</p>
            <div class="user-actions">
              <button @click="startEditUser(user)" class="edit-btn">Edit</button>
              <button @click="deleteUser(user.id)" class="delete-btn">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Posts Tab -->
    <div v-if="activeTab === 'posts'" class="tab-content">
      <div class="actions">
        <button @click="fetchPosts" :disabled="postsApi.loading" class="action-btn">
          {{ postsApi.loading ? 'Loading...' : 'Refresh Posts' }}
        </button>
      </div>

      <div class="create-post">
        <h3>Create New Post</h3>
        <input
          v-model="newPost.title"
          placeholder="Post title"
          class="post-input"
        />
        <textarea
          v-model="newPost.body"
          placeholder="Post content"
          class="post-textarea"
        ></textarea>
        <button @click="createPost" class="create-btn">Create Post</button>
      </div>

      <div v-if="postsApi.error" class="error-message">
        Error: {{ postsApi.error }}
      </div>

      <div v-if="postsApi.loading" class="loading">Loading posts...</div>

      <div v-else-if="postsApi.data" class="posts-list">
        <div
          v-for="post in postsApi.data.slice(0, 5)"
          :key="post.id"
          class="post-card"
        >
          <h4>{{ post.title }}</h4>
          <p>{{ post.body }}</p>
        </div>
      </div>
    </div>

    <!-- Photos Tab -->
    <div v-if="activeTab === 'photos'" class="tab-content">
      <div class="actions">
        <button @click="fetchPhotos" :disabled="photosApi.loading" class="action-btn">
          {{ photosApi.loading ? 'Loading...' : 'Refresh Photos' }}
        </button>
      </div>

      <div v-if="photosApi.error" class="error-message">
        Error: {{ photosApi.error }}
      </div>

      <div v-if="photosApi.loading" class="loading">Loading photos...</div>

      <div v-else-if="photosApi.data" class="photos-grid">
        <img
          v-for="photo in photosApi.data.slice(0, 12)"
          :key="photo.id"
          :src="photo.thumbnailUrl"
          :alt="photo.title"
          class="photo-thumbnail"
        />
      </div>
    </div>

    <div class="api-info">
      <h4>🔧 Composable Features:</h4>
      <ul>
        <li><strong>Loading States:</strong> Otomatis mengelola loading states</li>
        <li><strong>Error Handling:</strong> Centralized error management</li>
        <li><strong>Request Cancellation:</strong> Membatalkan request yang tidak perlu</li>
        <li><strong>CRUD Operations:</strong> GET, POST, PUT, DELETE methods</li>
        <li><strong>Cleanup:</strong> Otomatis cleanup saat component unmount</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.api-demo {
  padding: 20px;
  max-width: 1000px;
  margin: 0 auto;
  border: 2px solid #f59e0b;
  border-radius: 8px;
}

.tabs {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.tab-btn {
  padding: 10px 20px;
  background: #f3f4f6;
  border: 2px solid #e5e7eb;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
}

.tab-btn.active {
  background: #f59e0b;
  color: white;
  border-color: #f59e0b;
}

.tab-content {
  min-height: 300px;
}

.actions {
  margin-bottom: 20px;
}

.action-btn {
  padding: 8px 16px;
  background: #f59e0b;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.action-btn:disabled {
  background: #9ca3af;
  cursor: not-allowed;
}

.error-message {
  background: #fef2f2;
  color: #dc2626;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 15px;
}

.loading {
  text-align: center;
  padding: 20px;
  color: #6b7280;
}

.users-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 15px;
}

.user-card {
  padding: 15px;
  background: #fffbeb;
  border: 1px solid #fbbf24;
  border-radius: 8px;
}

.user-info h4 {
  margin: 0 0 10px 0;
  color: #92400e;
}

.user-info p {
  margin: 5px 0;
  color: #78350f;
  font-size: 14px;
}

.user-actions {
  display: flex;
  gap: 10px;
  margin-top: 10px;
}

.edit-btn, .delete-btn {
  padding: 4px 8px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.edit-btn {
  background: #3b82f6;
  color: white;
}

.delete-btn {
  background: #ef4444;
  color: white;
}

.edit-form {
  padding: 10px;
  background: white;
  border-radius: 4px;
}

.edit-input {
  display: block;
  width: 100%;
  margin-bottom: 8px;
  padding: 6px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
}

.edit-actions {
  display: flex;
  gap: 8px;
}

.save-btn, .cancel-btn {
  padding: 4px 8px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.save-btn {
  background: #10b981;
  color: white;
}

.cancel-btn {
  background: #6b7280;
  color: white;
}

.create-post {
  background: #f0fdf4;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.create-post h3 {
  margin-top: 0;
  color: #166534;
}

.post-input, .post-textarea {
  display: block;
  width: 100%;
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
}

.post-textarea {
  min-height: 80px;
  resize: vertical;
}

.create-btn {
  padding: 8px 16px;
  background: #10b981;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.posts-list {
  display: grid;
  gap: 15px;
}

.post-card {
  padding: 15px;
  background: #f8fafc;
  border-left: 4px solid #10b981;
  border-radius: 4px;
}

.post-card h4 {
  margin: 0 0 10px 0;
  color: #1f2937;
}

.post-card p {
  margin: 0;
  color: #6b7280;
  line-height: 1.5;
}

.photos-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 15px;
}

.photo-thumbnail {
  width: 100%;
  height: 150px;
  object-fit: cover;
  border-radius: 8px;
  border: 2px solid #e5e7eb;
}

.api-info {
  background: #fef3c7;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #f59e0b;
  margin-top: 20px;
}

.api-info h4 {
  margin-top: 0;
  color: #92400e;
}

.api-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.api-info li {
  margin: 5px 0;
  color: #92400e;
  font-size: 14px;
}
</style>
```

### Fungsi 4: Event Handling dan Lifecycle Management

Composable dapat mengelola event listeners dan lifecycle hooks secara terpusat, memungkinkan cleanup otomatis dan pengelolaan event yang konsisten.

```javascript
// composables/useKeyboardShortcuts.js
import { onMounted, onUnmounted } from 'vue'

export function useKeyboardShortcuts(shortcuts) {
  const handleKeyDown = (event) => {
    const { key, ctrlKey, shiftKey, altKey } = event

    // Find matching shortcut
    for (const [shortcut, callback] of Object.entries(shortcuts)) {
      const [modifiers, actualKey] = shortcut.split('+')

      const hasModifiers =
        (modifiers.includes('ctrl') && ctrlKey) ||
        (modifiers.includes('shift') && shiftKey) ||
        (modifiers.includes('alt') && altKey) ||
        modifiers.length === 0

      if (hasModifiers && key.toLowerCase() === actualKey.toLowerCase()) {
        event.preventDefault()
        callback(event)
        break
      }
    }
  }

  onMounted(() => {
    window.addEventListener('keydown', handleKeyDown)
  })

  onUnmounted(() => {
    window.removeEventListener('keydown', handleKeyDown)
  })
}
```

```javascript
// composables/useWindowEvents.js
import { ref, onMounted, onUnmounted } from 'vue'

export function useWindowEvents() {
  const windowSize = ref({
    width: window.innerWidth,
    height: window.innerHeight
  })

  const scrollPosition = ref({
    x: window.scrollX,
    y: window.scrollY
  })

  const isOnline = ref(navigator.onLine)
  const isVisible = ref(!document.hidden)

  // Event handlers
  const handleResize = () => {
    windowSize.value = {
      width: window.innerWidth,
      height: window.innerHeight
    }
  }

  const handleScroll = () => {
    scrollPosition.value = {
      x: window.scrollX,
      y: window.scrollY
    }
  }

  const handleOnline = () => isOnline.value = true
  const handleOffline = () => isOnline.value = false
  const handleVisibilityChange = () => isVisible.value = !document.hidden

  // Utility methods
  const scrollTo = (x, y) => window.scrollTo(x, y)
  const scrollToTop = () => window.scrollTo(0, 0)
  const scrollBy = (x, y) => window.scrollBy(x, y)

  // Event listeners
  onMounted(() => {
    window.addEventListener('resize', handleResize)
    window.addEventListener('scroll', handleScroll)
    window.addEventListener('online', handleOnline)
    window.addEventListener('offline', handleOffline)
    document.addEventListener('visibilitychange', handleVisibilityChange)
  })

  onUnmounted(() => {
    window.removeEventListener('resize', handleResize)
    window.removeEventListener('scroll', handleScroll)
    window.removeEventListener('online', handleOnline)
    window.removeEventListener('offline', handleOffline)
    document.removeEventListener('visibilitychange', handleVisibilityChange)
  })

  return {
    windowSize,
    scrollPosition,
    isOnline,
    isVisible,
    scrollTo,
    scrollToTop,
    scrollBy
  }
}
```

```vue
<script setup>
import { ref } from 'vue'
import { useKeyboardShortcuts } from './composables/useKeyboardShortcuts'
import { useWindowEvents } from './composables/useWindowEvents'

// Window events
const { windowSize, scrollPosition, isOnline, isVisible, scrollToTop } = useWindowEvents()

// Counter untuk demo
const counter = ref(0)
const message = ref('Press Ctrl+S to save, Ctrl+R to reset, Ctrl+I to increment')

// Keyboard shortcuts
useKeyboardShortcuts({
  'ctrl+s': () => {
    message.value = 'Document saved!'
    setTimeout(() => message.value = 'Press Ctrl+S to save, Ctrl+R to reset, Ctrl+I to increment', 2000)
  },
  'ctrl+r': () => {
    counter.value = 0
    message.value = 'Counter reset!'
    setTimeout(() => message.value = 'Press Ctrl+S to save, Ctrl+R to reset, Ctrl+I to increment', 2000)
  },
  'ctrl+i': () => {
    counter.value++
    message.value = `Counter incremented to ${counter.value}`
    setTimeout(() => message.value = 'Press Ctrl+S to save, Ctrl+R to reset, Ctrl+I to increment', 2000)
  },
  'ctrl+shift+u': () => {
    scrollToTop()
    message.value = 'Scrolled to top!'
    setTimeout(() => message.value = 'Press Ctrl+S to save, Ctrl+R to reset, Ctrl+I to increment', 2000)
  }
})

// Manual increment/decrement for mobile
const increment = () => counter.value++
const decrement = () => counter.value--
const reset = () => counter.value = 0
</script>

<template>
  <div class="events-demo">
    <h2>⌨️ Event Handling & Lifecycle Management</h2>

    <div class="status-grid">
      <div class="status-card">
        <h3>🖥️ Window Size</h3>
        <p>Width: {{ windowSize.width }}px</p>
        <p>Height: {{ windowSize.height }}px</p>
        <p>Aspect: {{ (windowSize.width / windowSize.height).toFixed(2) }}</p>
      </div>

      <div class="status-card">
        <h3>📜 Scroll Position</h3>
        <p>X: {{ scrollPosition.x }}px</p>
        <p>Y: {{ scrollPosition.y }}px</p>
        <button @click="scrollToTop" class="scroll-btn">Scroll to Top</button>
      </div>

      <div class="status-card">
        <h3>🌐 Network Status</h3>
        <p :class="{ online: isOnline, offline: !isOnline }">
          {{ isOnline ? '✅ Online' : '❌ Offline' }}
        </p>
      </div>

      <div class="status-card">
        <h3>👁️ Page Visibility</h3>
        <p :class="{ visible: isVisible, hidden: !isVisible }">
          {{ isVisible ? '👁️ Visible' : '😴 Hidden' }}
        </p>
      </div>
    </div>

    <div class="counter-section">
      <h3>🔢 Counter with Keyboard Shortcuts</h3>
      <div class="counter-display">
        <div class="counter-value">{{ counter }}</div>
      </div>
      <div class="counter-controls">
        <button @click="decrement" class="counter-btn decrement">-</button>
        <button @click="reset" class="counter-btn reset">Reset</button>
        <button @click="increment" class="counter-btn increment">+</button>
      </div>
      <div class="message-display">
        {{ message }}
      </div>
    </div>

    <div class="shortcuts-info">
      <h4>⌨️ Available Shortcuts:</h4>
      <ul>
        <li><kbd>Ctrl+S</kbd> - Save document</li>
        <li><kbd>Ctrl+R</kbd> - Reset counter</li>
        <li><kbd>Ctrl+I</kbd> - Increment counter</li>
        <li><kbd>Ctrl+Shift+U</kbd> - Scroll to top</li>
      </ul>
    </div>

    <div class="events-info">
      <h4>🔧 Composable Features:</h4>
      <ul>
        <li><strong>Auto Setup:</strong> Event listeners mounted automatically</li>
        <li><strong>Auto Cleanup:</strong> Listeners removed on unmount</li>
        <li><strong>Reactive State:</strong> Window properties update in real-time</li>
        <li><strong>Utility Methods:</strong> Convenient helper functions</li>
        <li><strong>Keyboard Support:</strong> Cross-browser compatible shortcuts</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.events-demo {
  padding: 20px;
  max-width: 900px;
  margin: 0 auto;
  border: 2px solid #8b5cf6;
  border-radius: 8px;
}

.status-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.status-card {
  background: #faf5ff;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #e9d5ff;
  text-align: center;
}

.status-card h3 {
  margin-top: 0;
  color: #6b21a8;
}

.status-card p {
  margin: 5px 0;
  color: #581c87;
}

.scroll-btn {
  margin-top: 10px;
  padding: 6px 12px;
  background: #8b5cf6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.online {
  color: #059669;
}

.offline {
  color: #dc2626;
}

.visible {
  color: #059669;
}

.hidden {
  color: #dc2626;
}

.counter-section {
  background: #f3e8ff;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
  text-align: center;
}

.counter-section h3 {
  margin-top: 0;
  color: #6b21a8;
}

.counter-display {
  margin: 20px 0;
}

.counter-value {
  font-size: 48px;
  font-weight: bold;
  color: #6b21a8;
  display: inline-block;
  min-width: 100px;
}

.counter-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 15px;
}

.counter-btn {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: none;
  font-size: 20px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s;
}

.counter-btn.increment {
  background: #10b981;
  color: white;
}

.counter-btn.decrement {
  background: #ef4444;
  color: white;
}

.counter-btn.reset {
  background: #6b7280;
  color: white;
}

.counter-btn:hover {
  transform: scale(1.1);
}

.message-display {
  padding: 10px;
  background: white;
  border-radius: 4px;
  color: #6b21a8;
  font-weight: 500;
  min-height: 20px;
}

.shortcuts-info {
  background: #f0f9ff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.shortcuts-info h4 {
  margin-top: 0;
  color: #0c4a6e;
}

.shortcuts-info ul {
  margin: 10px 0;
  padding-left: 20px;
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.shortcuts-info li {
  color: #0c4a6e;
}

kbd {
  background: #e0f2fe;
  padding: 2px 6px;
  border-radius: 3px;
  border: 1px solid #bae6fd;
  font-family: monospace;
  font-size: 12px;
}

.events-info {
  background: #faf5ff;
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #8b5cf6;
}

.events-info h4 {
  margin-top: 0;
  color: #6b21a8;
}

.events-info ul {
  margin: 10px 0;
  padding-left: 20px;
}

.events-info li {
  margin: 5px 0;
  color: #6b21a8;
  font-size: 14px;
}
</style>
```

### Fungsi 5: Utility Functions dan Cross-cutting Concerns

Composable sangat efektif untuk mengelola utility functions dan cross-cutting concerns seperti theming, notifications, dan local storage.

```javascript
// composables/useTheme.js
import { ref, watch, onMounted } from 'vue'

export function useTheme() {
  const theme = ref('light')
  const systemPreference = ref('light')

  const availableThemes = ['light', 'dark', 'auto']

  const themeColors = {
    light: {
      primary: '#3b82f6',
      secondary: '#10b981',
      background: '#ffffff',
      surface: '#f8fafc',
      text: '#1f2937'
    },
    dark: {
      primary: '#60a5fa',
      secondary: '#34d399',
      background: '#1f2937',
      surface: '#374151',
      text: '#f9fafb'
    }
  }

  const currentColors = computed(() => {
    const activeTheme = theme.value === 'auto' ? systemPreference.value : theme.value
    return themeColors[activeTheme]
  })

  const setTheme = (newTheme) => {
    if (availableThemes.includes(newTheme)) {
      theme.value = newTheme
      localStorage.setItem('theme', newTheme)
      applyTheme(newTheme === 'auto' ? systemPreference.value : newTheme)
    }
  }

  const applyTheme = (themeName) => {
    const root = document.documentElement
    const colors = themeColors[themeName]

    Object.entries(colors).forEach(([key, value]) => {
      root.style.setProperty(`--color-${key}`, value)
    })

    root.setAttribute('data-theme', themeName)
  }

  const toggleTheme = () => {
    const newTheme = theme.value === 'light' ? 'dark' : 'light'
    setTheme(newTheme)
  }

  const detectSystemTheme = () => {
    if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
      systemPreference.value = 'dark'
    } else {
      systemPreference.value = 'light'
    }
  }

  // Initialize
  onMounted(() => {
    // Load saved theme
    const savedTheme = localStorage.getItem('theme')
    if (savedTheme && availableThemes.includes(savedTheme)) {
      theme.value = savedTheme
    }

    // Detect system preference
    detectSystemTheme()

    // Listen for system theme changes
    if (window.matchMedia) {
      const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)')
      mediaQuery.addListener(detectSystemTheme)
    }

    // Apply initial theme
    applyTheme(theme.value === 'auto' ? systemPreference.value : theme.value)
  })

  // Watch for auto theme changes
  watch([theme, systemPreference], () => {
    if (theme.value === 'auto') {
      applyTheme(systemPreference.value)
    }
  })

  return {
    theme,
    systemPreference,
    availableThemes,
    currentColors,
    setTheme,
    toggleTheme,
    detectSystemTheme
  }
}
```

```javascript
// composables/useLocalStorage.js
import { ref, watchEffect } from 'vue'

export function useLocalStorage(key, defaultValue) {
  const storedValue = ref(defaultValue)

  // Load from localStorage on mount
  if (typeof window !== 'undefined') {
    try {
      const item = window.localStorage.getItem(key)
      if (item) {
        storedValue.value = JSON.parse(item)
      }
    } catch (error) {
      console.error(`Error loading from localStorage:`, error)
    }
  }

  // Watch for changes and save to localStorage
  watchEffect(() => {
    if (typeof window !== 'undefined') {
      try {
        window.localStorage.setItem(key, JSON.stringify(storedValue.value))
      } catch (error) {
        console.error(`Error saving to localStorage:`, error)
      }
    }
  })

  // Utility functions
  const clearStorage = () => {
    if (typeof window !== 'undefined') {
      try {
        window.localStorage.removeItem(key)
        storedValue.value = defaultValue
      } catch (error) {
        console.error(`Error clearing localStorage:`, error)
      }
    }
  }

  return {
    storedValue,
    clearStorage
  }
}
```

```vue
<script setup>
import { ref, computed } from 'vue'
import { useTheme } from './composables/useTheme'
import { useLocalStorage } from './composables/useLocalStorage'

// Theme management
const { theme, systemPreference, availableThemes, currentColors, setTheme, toggleTheme } = useTheme()

// Local storage demo
const userData = useLocalStorage('userData', {
  name: 'Guest User',
  preferences: {
    notifications: true,
    autoSave: false,
    language: 'en'
  }
})

const notes = useLocalStorage('notes', [])

// Form data
const noteInput = ref('')
const userName = ref(userData.storedValue.value.name)
const notifications = ref(userData.storedValue.value.preferences.notifications)
const autoSave = ref(userData.storedValue.value.preferences.autoSave)

// Update functions
const updateUserName = () => {
  userData.storedValue.value.name = userName.value
}

const toggleNotifications = () => {
  userData.storedValue.value.preferences.notifications = !userData.storedValue.value.preferences.notifications
}

const toggleAutoSave = () => {
  userData.storedValue.value.preferences.autoSave = !userData.storedValue.value.preferences.autoSave
}

const addNote = () => {
  if (noteInput.value.trim()) {
    notes.storedValue.value.push({
      id: Date.now(),
      text: noteInput.value,
      timestamp: new Date().toISOString(),
      theme: theme.value
    })
    noteInput.value = ''
  }
}

const deleteNote = (index) => {
  notes.storedValue.value.splice(index, 1)
}

const clearUserData = () => {
  userData.clearStorage()
  userName.value = userData.storedValue.value.name
  notifications.value = userData.storedValue.value.preferences.notifications
  autoSave.value = userData.storedValue.value.preferences.autoSave
}

const clearNotes = () => {
  notes.clearStorage()
}

// Computed values
const isDarkMode = computed(() => {
  return theme.value === 'dark' || (theme.value === 'auto' && systemPreference.value === 'dark')
})
</script>

<template>
  <div class="theme-demo" :data-theme="isDarkMode ? 'dark' : 'light'">
    <h2>🎨 Theme & Local Storage Management</h2>

    <div class="theme-controls">
      <h3>Theme Settings</h3>
      <div class="theme-selector">
        <button
          v-for="themeOption in availableThemes"
          :key="themeOption"
          @click="setTheme(themeOption)"
          :class="{ active: theme === themeOption }"
          class="theme-btn"
        >
          {{ themeOption.charAt(0).toUpperCase() + themeOption.slice(1) }}
        </button>
        <button @click="toggleTheme" class="theme-btn toggle">
          Toggle ({{ theme === 'light' ? '🌙' : '☀️' }})
        </button>
      </div>

      <div class="theme-info">
        <p><strong>Current Theme:</strong> {{ theme }}</p>
        <p><strong>System Preference:</strong> {{ systemPreference }}</p>
        <p><strong>Is Dark Mode:</strong> {{ isDarkMode ? 'Yes' : 'No' }}</p>
      </div>
    </div>

    <div class="storage-demo">
      <h3>Local Storage Demo</h3>

      <!-- User Data Section -->
      <div class="storage-section">
        <h4>User Data</h4>
        <div class="form-group">
          <label>Name:</label>
          <input
            v-model="userName"
            @blur="updateUserName"
            placeholder="Enter your name"
            class="form-input"
          />
        </div>

        <div class="preferences">
          <h5>Preferences:</h5>
          <label class="checkbox-label">
            <input
              type="checkbox"
              v-model="notifications"
              @change="toggleNotifications"
            />
            Enable Notifications
          </label>
          <label class="checkbox-label">
            <input
              type="checkbox"
              v-model="autoSave"
              @change="toggleAutoSave"
            />
            Auto-save
          </label>
        </div>

        <div class="storage-info">
          <p><strong>Saved Name:</strong> {{ userData.storedValue.name }}</p>
          <p><strong>Notifications:</strong> {{ userData.storedValue.preferences.notifications ? 'Enabled' : 'Disabled' }}</p>
          <p><strong>Auto-save:</strong> {{ userData.storedValue.preferences.autoSave ? 'Enabled' : 'Disabled' }}</p>
        </div>

        <button @click="clearUserData" class="clear-btn">Clear User Data</button>
      </div>

      <!-- Notes Section -->
      <div class="storage-section">
        <h4>Notes</h4>
        <div class="note-form">
          <input
            v-model="noteInput"
            placeholder="Enter a note..."
            class="form-input"
            @keypress.enter="addNote"
          />
          <button @click="addNote" class="add-btn">Add Note</button>
        </div>

        <div class="notes-list">
          <div v-if="notes.storedValue.length === 0" class="empty-notes">
            No notes yet. Add your first note above!
          </div>
          <div
            v-for="(note, index) in notes.storedValue.slice().reverse()"
            :key="note.id"
            class="note-item"
          >
            <div class="note-content">
              <p>{{ note.text }}</p>
              <small class="note-meta">
                {{ new Date(note.timestamp).toLocaleString() }} | Theme: {{ note.theme }}
              </small>
            </div>
            <button @click="deleteNote(notes.storedValue.length - 1 - index)" class="delete-note-btn">
              ×
            </button>
          </div>
        </div>

        <div class="notes-info">
          <p><strong>Total Notes:</strong> {{ notes.storedValue.length }}</p>
        </div>

        <button @click="clearNotes" class="clear-btn">Clear All Notes</button>
      </div>
    </div>

    <div class="composable-features">
      <h4>🔧 Advanced Composable Features:</h4>
      <ul>
        <li><strong>Persistence:</strong> Data survives page reloads</li>
        <li><strong>Auto-sync:</strong> Changes saved automatically</li>
        <strong>Theme Integration:</strong> Theme affects stored data</li>
        <li><strong>Error Handling:</strong> Graceful fallback for storage errors</li>
        <li><strong>Type Safety:</strong> Maintains data structure integrity</li>
        <li><strong>Cleanup:</strong> Easy data management and clearing</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.theme-demo {
  padding: 20px;
  max-width: 800px;
  margin: 0 auto;
  border: 2px solid #3b82f6;
  border-radius: 8px;
}

.theme-controls {
  background: var(--color-surface);
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.theme-controls h3 {
  margin-top: 0;
  color: var(--color-text);
}

.theme-selector {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.theme-btn {
  padding: 8px 16px;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.3s;
}

.theme-btn:hover {
  opacity: 0.9;
}

.theme-btn.active {
  background: var(--color-secondary);
}

.theme-btn.toggle {
  background: var(--color-text);
}

.theme-info p {
  margin: 5px 0;
  color: var(--color-text);
}

.storage-demo {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.storage-section {
  background: var(--color-surface);
  padding: 20px;
  border-radius: 8px;
}

.storage-section h4 {
  margin-top: 0;
  color: var(--color-text);
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  color: var(--color-text);
  font-weight: 500;
}

.form-input {
  width: 100%;
  padding: 8px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
  background: var(--color-background);
  color: var(--color-text);
}

.preferences {
  margin-bottom: 15px;
}

.preferences h5 {
  margin-bottom: 10px;
  color: var(--color-text);
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 8px;
  color: var(--color-text);
  cursor: pointer;
}

.storage-info {
  background: var(--color-background);
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 15px;
}

.storage-info p {
  margin: 3px 0;
  color: var(--color-text);
  font-size: 14px;
}

.clear-btn {
  padding: 6px 12px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.clear-btn:hover {
  background: #dc2626;
}

.note-form {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.add-btn {
  padding: 8px 12px;
  background: var(--color-primary);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.notes-list {
  max-height: 200px;
  overflow-y: auto;
  margin-bottom: 15px;
}

.empty-notes {
  text-align: center;
  color: #9ca3af;
  font-style: italic;
  padding: 20px;
}

.note-item {
  background: var(--color-background);
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 8px;
  border-left: 3px solid var(--color-primary);
  position: relative;
}

.note-content p {
  margin: 0 0 5px 0;
  color: var(--color-text);
}

.note-meta {
  color: #6b7280;
  font-size: 12px;
}

.delete-note-btn {
  position: absolute;
  top: 5px;
  right: 5px;
  width: 20px;
  height: 20px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  font-size: 12px;
  line-height: 1;
}

.notes-info p {
  margin: 5px 0;
  color: var(--color-text);
  font-size: 14px;
}

.composable-features {
  background: var(--color-surface);
  padding: 15px;
  border-radius: 8px;
  border-left: 4px solid #3b82f6;
}

.composable-features h4 {
  margin-top: 0;
  color: var(--color-text);
}

.composable-features ul {
  margin: 10px 0;
  padding-left: 20px;
}

.composable-features li {
  margin: 5px 0;
  color: var(--color-text);
  font-size: 14px;
}

/* Dark mode overrides */
[data-theme="dark"] .theme-demo {
  border-color: #60a5fa;
}

[data-theme="dark"] .theme-btn.toggle {
  background: #f3f4f6;
  color: #1f2937;
}
</style>
```

### Composable vs Mixins vs Options API

| Aspect | Composable | Mixins | Options API |
|--------|------------|--------|-------------|
| **Logic Reuse** | ✅ Excellent | ⚠️ Conflict-prone | ❌ Limited |
| **Type Safety** | ✅ Full TypeScript | ❌ Poor | ⚠️ Limited |
| **Dependencies** | ✅ Explicit | ❌ Implicit | ❌ Mixed |
| **Naming** | ✅ Clear | ⚠️ Conflicts | ❌ Scattered |
| **Debugging** | ✅ Easy | ❌ Hard | ⚠️ Medium |

## 🎮 Cara Menggunakan Composable

### 1. Buat Composable Function

```javascript
// composables/useCounter.js
import { ref } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)

  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initialValue

  return { count, increment, decrement, reset }
}
```

### 2. Import di Component

```vue
<script setup>
import { useCounter } from './composables/useCounter'
</script>
```

### 3. Gunakan di Component

```vue
<script setup>
import { useCounter } from './composables/useCounter'

const counter = useCounter(10)
</script>

<template>
  <div>
    <p>Count: {{ counter.count }}</p>
    <button @click="counter.increment">+</button>
    <button @click="counter.decrement">-</button>
  </div>
</template>
```

## ⚡ Basic Composable

### Counter Composable

```javascript
// composables/useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)

  const increment = () => count.value++
  const decrement = () => count.value--
  const reset = () => count.value = initialValue

  // Computed properties
  const isEven = computed(() => count.value % 2 === 0)
  const isPositive = computed(() => count.value > 0)

  // Methods dengan validation
  const incrementBy = (amount) => {
    if (typeof amount === 'number') {
      count.value += amount
    }
  }

  const decrementBy = (amount) => {
    if (typeof amount === 'number') {
      count.value -= amount
    }
  }

  return {
    // State
    count,
    // Computed
    isEven,
    isPositive,
    // Methods
    increment,
    decrement,
    reset,
    incrementBy,
    decrementBy
  }
}
```

```vue
<!-- components/CounterComponent.vue -->
<script setup>
import { useCounter } from '../shared/useCounter'

// Gunakan composable
const counter = useCounter(10)
</script>

<template>
  <div class="counter-component">
    <h3>Counter Component</h3>

    <div class="counter-display">
      <span class="count">{{ counter.count }}</span>
      <div class="status">
        <span v-if="counter.isEven" class="even">Even</span>
        <span v-else class="odd">Odd</span>
        <span v-if="counter.isPositive" class="positive">Positive</span>
        <span v-else class="zero">Zero</span>
      </div>
    </div>

    <div class="controls">
      <button @click="counter.decrement">-</button>
      <button @click="counter.increment">+</button>
      <button @click="counter.reset">Reset</button>
    </div>

    <div class="advanced-controls">
      <input
        v-model.number="incrementAmount"
        type="number"
        placeholder="Amount"
      />
      <button @click="counter.incrementBy(incrementAmount)">Add</button>
      <button @click="counter.decrementBy(incrementAmount)">Subtract</button>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

const incrementAmount = ref(1)
</script>

<style scoped>
.counter-component {
  padding: 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  max-width: 300px;
  margin: 0 auto;
}

.counter-component h3 {
  margin-top: 0;
  text-align: center;
  color: #1f2937;
}

.counter-display {
  text-align: center;
  margin-bottom: 20px;
}

.count {
  display: block;
  font-size: 48px;
  font-weight: bold;
  color: #3b82f6;
  margin-bottom: 10px;
}

.status {
  display: flex;
  gap: 8px;
  justify-content: center;
}

.even, .odd, .positive, .zero {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
}

.even { background: #dcfce7; color: #166534; }
.odd { background: #fef3c7; color: #92400e; }
.positive { background: #dbeafe; color: #1e40af; }
.zero { background: #f3f4f6; color: #374151; }

.controls {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 15px;
}

.controls button {
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 8px;
  background: #3b82f6;
  color: white;
  font-size: 18px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.controls button:hover {
  background: #2563eb;
  transform: scale(1.05);
}

.advanced-controls {
  display: flex;
  gap: 8px;
  align-items: center;
}

.advanced-controls input {
  width: 80px;
  padding: 6px;
  border: 2px solid #e5e7eb;
  border-radius: 4px;
  text-align: center;
}

.advanced-controls button {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  background: #10b981;
  color: white;
  font-size: 12px;
  cursor: pointer;
}

.advanced-controls button:hover {
  background: #059669;
}
</style>
```

### Todo List Composable

```javascript
// composables/useTodoList.js
import { ref, computed } from 'vue'

export function useTodoList() {
  const todos = ref([])
  const newTodoText = ref('')
  const filter = ref('all') // all, active, completed

  // Computed properties
  const filteredTodos = computed(() => {
    switch (filter.value) {
      case 'active':
        return todos.value.filter(todo => !todo.completed)
      case 'completed':
        return todos.value.filter(todo => todo.completed)
      default:
        return todos.value
    }
  })

  const todoCount = computed(() => todos.value.length)
  const completedCount = computed(() =>
    todos.value.filter(todo => todo.completed).length
  )
  const activeCount = computed(() =>
    todos.value.filter(todo => !todo.completed).length
  )
  const completionRate = computed(() => {
    if (todoCount.value === 0) return 0
    return Math.round((completedCount.value / todoCount.value) * 100)
  })

  // Methods
  const addTodo = () => {
    if (newTodoText.value.trim() === '') return

    todos.value.push({
      id: Date.now(),
      text: newTodoText.value.trim(),
      completed: false,
      createdAt: new Date()
    })

    newTodoText.value = ''
  }

  const removeTodo = (id) => {
    const index = todos.value.findIndex(todo => todo.id === id)
    if (index > -1) {
      todos.value.splice(index, 1)
    }
  }

  const toggleTodo = (id) => {
    const todo = todos.value.find(todo => todo.id === id)
    if (todo) {
      todo.completed = !todo.completed
    }
  }

  const updateTodoText = (id, newText) => {
    const todo = todos.value.find(todo => todo.id === id)
    if (todo && newText.trim() !== '') {
      todo.text = newText.trim()
    }
  }

  const clearCompleted = () => {
    todos.value = todos.value.filter(todo => !todo.completed)
  }

  const markAllComplete = () => {
    todos.value.forEach(todo => {
      todo.completed = true
    })
  }

  const markAllActive = () => {
    todos.value.forEach(todo => {
      todo.completed = false
    })
  }

  const setFilter = (newFilter) => {
    filter.value = newFilter
  }

  return {
    // State
    todos,
    newTodoText,
    filter,
    // Computed
    filteredTodos,
    todoCount,
    completedCount,
    activeCount,
    completionRate,
    // Methods
    addTodo,
    removeTodo,
    toggleTodo,
    updateTodoText,
    clearCompleted,
    markAllComplete,
    markAllActive,
    setFilter
  }
}
```

```vue
<!-- components/TodoList.vue -->
<script setup>
import { useTodoList } from '../shared/Todo'

// Gunakan composable
const todoList = useTodoList()

// Add todos on Enter key
const handleKeydown = (event) => {
  if (event.key === 'Enter') {
    todoList.addTodo()
  }
}

// Computed untuk status messages
const statusMessage = computed(() => {
  if (todoList.todoCount === 0) {
    return 'No todos yet. Add one above!'
  }
  return `${todoList.activeCount} active, ${todoList.completedCount} completed`
})
</script>

<template>
  <div class="todo-list">
    <h2>📝 Todo List</h2>

    <!-- Add new todo -->
    <div class="add-todo">
      <input
        v-model="todoList.newTodoText"
        @keydown="handleKeydown"
        placeholder="What needs to be done?"
        class="todo-input"
      />
      <button @click="todoList.addTodo" class="add-btn">
        Add
      </button>
    </div>

    <!-- Filter tabs -->
    <div class="filter-tabs">
      <button
        v-for="tab in ['all', 'active', 'completed']"
        :key="tab"
        @click="todoList.setFilter(tab)"
        :class="{ active: todoList.filter === tab }"
        class="filter-tab"
      >
        {{ tab.charAt(0).toUpperCase() + tab.slice(1) }}
        <span class="count">
          {{ tab === 'all' ? todoList.todoCount :
             tab === 'active' ? todoList.activeCount :
             todoList.completedCount }}
        </span>
      </button>
    </div>

    <!-- Todo items -->
    <div class="todos">
      <div
        v-for="todo in todoList.filteredTodos"
        :key="todo.id"
        class="todo-item"
        :class="{ completed: todo.completed }"
      >
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="todoList.toggleTodo(todo.id)"
          class="todo-checkbox"
        />

        <span
          @dblclick="editTodo(todo)"
          class="todo-text"
        >
          {{ todo.text }}
        </span>

        <button
          @click="todoList.removeTodo(todo.id)"
          class="remove-btn"
        >
          ×
        </button>
      </div>

      <div v-if="todoList.filteredTodos.length === 0" class="empty-state">
        <p>No {{ todoList.filter }} todos</p>
      </div>
    </div>

    <!-- Statistics and actions -->
    <div v-if="todoList.todoCount > 0" class="todo-stats">
      <div class="progress-bar">
        <div
          class="progress-fill"
          :style="{ width: todoList.completionRate + '%' }"
        ></div>
      </div>
      <div class="stats-info">
        <span>{{ statusMessage }}</span>
        <span>{{ todoList.completionRate }}% complete</span>
      </div>

      <div class="bulk-actions">
        <button
          v-if="todoList.activeCount > 0"
          @click="todoList.markAllComplete"
          class="bulk-btn"
        >
          Mark all complete
        </button>
        <button
          v-if="todoList.completedCount > 0"
          @click="todoList.markAllActive"
          class="bulk-btn"
        >
          Mark all active
        </button>
        <button
          v-if="todoList.completedCount > 0"
          @click="todoList.clearCompleted"
          class="bulk-btn danger"
        >
          Clear completed
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue'

const editingTodo = ref(null)
const editText = ref('')

const editTodo = (todo) => {
  editingTodo.value = todo.id
  editText.value = todo.text
}

const saveEdit = (todoId) => {
  if (editText.value.trim() !== '') {
    todoList.updateTodoText(todoId, editText.value)
  }
  editingTodo.value = null
  editText.value = ''
}

const cancelEdit = () => {
  editingTodo.value = null
  editText.value = ''
}
</script>

<style scoped>
.todo-list {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0,0,0,0.1);
}

.todo-list h2 {
  text-align: center;
  margin-top: 0;
  color: #1f2937;
  margin-bottom: 20px;
}

.add-todo {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input {
  flex: 1;
  padding: 12px;
  border: 2px solid #e5e7eb;
  border-radius: 8px;
  font-size: 16px;
  transition: border-color 0.3s;
}

.todo-input:focus {
  outline: none;
  border-color: #3b82f6;
}

.add-btn {
  padding: 12px 20px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.3s;
}

.add-btn:hover {
  background: #2563eb;
}

.filter-tabs {
  display: flex;
  margin-bottom: 20px;
  border-bottom: 2px solid #e5e7eb;
}

.filter-tab {
  padding: 10px 16px;
  background: transparent;
  border: none;
  border-bottom: 2px solid transparent;
  cursor: pointer;
  color: #6b7280;
  transition: all 0.3s;
  display: flex;
  align-items: center;
  gap: 5px;
}

.filter-tab:hover {
  color: #374151;
  background: #f9fafb;
}

.filter-tab.active {
  color: #3b82f6;
  border-bottom-color: #3b82f6;
}

.count {
  background: #e5e7eb;
  color: #374151;
  padding: 2px 6px;
  border-radius: 10px;
  font-size: 12px;
  font-weight: 500;
}

.filter-tab.active .count {
  background: #dbeafe;
  color: #1e40af;
}

.todos {
  margin-bottom: 20px;
}

.todo-item {
  display: flex;
  align-items: center;
  padding: 12px;
  margin-bottom: 8px;
  background: #f9fafb;
  border-radius: 8px;
  transition: all 0.3s ease;
}

.todo-item:hover {
  background: #f3f4f6;
}

.todo-item.completed {
  opacity: 0.6;
}

.todo-checkbox {
  margin-right: 12px;
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.todo-text {
  flex: 1;
  cursor: pointer;
  color: #1f2937;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #6b7280;
}

.remove-btn {
  padding: 4px 8px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  opacity: 0;
  transition: opacity 0.3s;
}

.todo-item:hover .remove-btn {
  opacity: 1;
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: #9ca3af;
  font-style: italic;
}

.todo-stats {
  padding-top: 20px;
  border-top: 2px solid #e5e7eb;
}

.progress-bar {
  height: 8px;
  background: #e5e7eb;
  border-radius: 4px;
  margin-bottom: 10px;
  overflow: hidden;
}

.progress-fill {
  height: 100%;
  background: #10b981;
  transition: width 0.3s ease;
}

.stats-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
  font-size: 14px;
  color: #6b7280;
}

.bulk-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.bulk-btn {
  padding: 6px 12px;
  background: #6b7280;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: background-color 0.3s;
}

.bulk-btn:hover {
  background: #4b5563;
}

.bulk-btn.danger {
  background: #ef4444;
}

.bulk-btn.danger:hover {
  background: #dc2626;
}
</style>
```

## 🔄 Logic Reuse Pattern

### Problem: Code Duplication

```vue
<!-- ❌ Tanpa Composable - Duplication di multiple components -->
<!-- Component A.vue -->
<script setup>
import { ref } from 'vue'

const counterA = ref(0)
const incrementA = () => counterA.value++
const decrementA = () => counterA.value--
</script>

<!-- Component B.vue -->
<script setup>
import { ref } from 'vue'

const counterB = ref(0)
const incrementB = () => counterB.value++
const decrementB = () => counterB.value--
</script>
```

### Solution: Composable Pattern

```vue
<!-- ✅ Dengan Composable - Logic reuse -->
<!-- Component A.vue -->
<script setup>
import { useCounter } from './composables/useCounter'
const counterA = useCounter()
</script>

<!-- Component B.vue -->
<script setup>
import { useCounter } from './composables/useCounter'
const counterB = useCounter(5) // Custom initial value
</script>
```

### Before and After Comparison

```vue
<!-- BEFORE: Tanpa Composable -->
<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// User data logic
const user = ref(null)
const isLoggedIn = computed(() => !!user.value)
const login = async (credentials) => {
  // Login logic
}
const logout = () => {
  user.value = null
}

// Window size logic
const windowSize = ref({ width: 0, height: 0 })
const isMobile = computed(() => windowSize.value.width < 768)

const updateWindowSize = () => {
  windowSize.value = {
    width: window.innerWidth,
    height: window.innerHeight
  }
}

onMounted(() => {
  updateWindowSize()
  window.addEventListener('resize', updateWindowSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateWindowSize)
})
</script>
```

```vue
<!-- AFTER: Dengan Composable -->
<script setup>
import { useAuth } from './composables/useAuth'
import { useWindowSize } from './composables/useWindowSize'

// Logic dipisah ke composables
const { user, isLoggedIn, login, logout } = useAuth()
const { windowSize, isMobile } = useWindowSize()
</script>
```

## 🎯 Advanced Composable

### API Fetching Composable

```javascript
// composables/useApi.js
import { ref, computed } from 'vue'

export function useApi(url, options = {}) {
  const data = ref(null)
  const error = ref(null)
  const loading = ref(false)

  const execute = async (fetchOptions = {}) => {
    loading.value = true
    error.value = null

    try {
      const response = await fetch(url, {
        ...options,
        ...fetchOptions
      })

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`)
      }

      data.value = await response.json()
      return data.value
    } catch (err) {
      error.value = err
      throw err
    } finally {
      loading.value = false
    }
  }

  // Computed properties
  const isSuccess = computed(() => !loading.value && !error.value && data.value !== null)
  const isError = computed(() => !loading.value && error.value !== null)
  const isEmpty = computed(() => !loading.value && !error.value && data.value === null)

  return {
    data,
    error,
    loading,
    isSuccess,
    isError,
    isEmpty,
    execute
  }
}

// Composable untuk GET requests
export function useFetch(url, options = {}) {
  return useApi(url, { method: 'GET', ...options })
}

// Composable untuk POST requests
export function usePost(url, options = {}) {
  return useApi(url, { method: 'POST', ...options })
}
```

### Local Storage Composable

```javascript
// composables/useLocalStorage.js
import { ref, watch } from 'vue'

export function useLocalStorage(key, defaultValue) {
  const storedValue = localStorage.getItem(key)
  const value = ref(storedValue ? JSON.parse(storedValue) : defaultValue)

  // Watch untuk update localStorage
  watch(value, (newValue) => {
    if (newValue === null || newValue === undefined) {
      localStorage.removeItem(key)
    } else {
      localStorage.setItem(key, JSON.stringify(newValue))
    }
  }, { deep: true })

  // Method untuk clear
  const clear = () => {
    value.value = defaultValue
  }

  return {
    value,
    clear
  }
}

// Specific local storage composables
export function useTheme() {
  const { value: theme, clear } = useLocalStorage('theme', 'light')

  const toggleTheme = () => {
    theme.value = theme.value === 'light' ? 'dark' : 'light'
  }

  return {
    theme,
    toggleTheme,
    clear
  }
}

export function useUserPreferences() {
  const { value: preferences, clear } = useLocalStorage('userPreferences', {
    language: 'en',
    notifications: true,
    autoSave: false
  })

  const updatePreference = (key, value) => {
    preferences.value[key] = value
  }

  return {
    preferences,
    updatePreference,
    clear
  }
}
```

### Form Validation Composable

```javascript
// composables/useForm.js
import { ref, computed } from 'vue'

export function useForm(initialValues = {}, validationRules = {}) {
  const values = ref({ ...initialValues })
  const errors = ref({})
  const touched = ref({})
  const isSubmitting = ref(false)

  // Validation function
  const validate = (field = null) => {
    const fieldsToValidate = field ? [field] : Object.keys(validationRules)

    fieldsToValidate.forEach(fieldName => {
      const value = values.value[fieldName]
      const rules = validationRules[fieldName]

      if (!rules) return

      errors.value[fieldName] = null

      for (const rule of rules) {
        const result = rule(value, values.value)
        if (result !== true) {
          errors.value[fieldName] = result
          break
        }
      }
    })

    return Object.keys(errors.value).every(key => !errors.value[key])
  }

  // Computed properties
  const isValid = computed(() => {
    return Object.keys(validationRules).every(key => !errors.value[key])
  })

  const hasErrors = computed(() => {
    return Object.keys(errors.value).some(key => errors.value[key])
  })

  // Methods
  const setValue = (field, value) => {
    values.value[field] = value
    touched.value[field] = true

    if (errors.value[field]) {
      validate(field)
    }
  }

  const setValues = (newValues) => {
    Object.assign(values.value, newValues)
    Object.keys(newValues).forEach(field => {
      touched.value[field] = true
    })
    validate()
  }

  const reset = () => {
    values.value = { ...initialValues }
    errors.value = {}
    touched.value = {}
    isSubmitting.value = false
  }

  const submit = async (callback) => {
    isSubmitting.value = true

    const isValid = validate()
    if (!isValid) {
      isSubmitting.value = false
      return false
    }

    try {
      await callback(values.value)
      return true
    } catch (error) {
      console.error('Form submission error:', error)
      return false
    } finally {
      isSubmitting.value = false
    }
  }

  return {
    values,
    errors,
    touched,
    isSubmitting,
    isValid,
    hasErrors,
    validate,
    setValue,
    setValues,
    reset,
    submit
  }
}

// Validation rules
export const required = (message = 'This field is required') => (value) => {
  if (value === null || value === undefined || value === '') {
    return message
  }
  return true
}

export const minLength = (min, message) => (value) => {
  if (value && value.length < min) {
    return message || `Must be at least ${min} characters`
  }
  return true
}

export const email = (message = 'Invalid email address') => (value) => {
  if (value && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
    return message
  }
  return true
}
```

## 📦 Composable dengan Params

### Flexible Configuration

```javascript
// composables/useCounter.js
export function useCounter(options = {}) {
  const {
    initialValue = 0,
    min = Number.NEGATIVE_INFINITY,
    max = Number.POSITIVE_INFINITY,
    step = 1
  } = options

  const count = ref(initialValue)

  const increment = () => {
    const newValue = count.value + step
    if (newValue <= max) {
      count.value = newValue
    }
  }

  const decrement = () => {
    const newValue = count.value - step
    if (newValue >= min) {
      count.value = newValue
    }
  }

  const setValue = (value) => {
    if (value >= min && value <= max) {
      count.value = value
    }
  }

  const isAtMin = computed(() => count.value <= min)
  const isAtMax = computed(() => count.value >= max)

  return {
    count,
    increment,
    decrement,
    setValue,
    isAtMin,
    isAtMax
  }
}
```

### Penggunaan dengan Berbagai Config

```vue
<script setup>
// Counter dengan default settings
const defaultCounter = useCounter()

// Counter dengan custom settings
const limitedCounter = useCounter({
  initialValue: 10,
  min: 0,
  max: 20,
  step: 2
})

// Percentage counter
const percentageCounter = useCounter({
  initialValue: 50,
  min: 0,
  max: 100,
  step: 5
})
</script>
```

## 🔧 Composable Ecosystem

### Building Composable Chain

```javascript
// composables/useAuth.js
export function useAuth() {
  const user = ref(null)
  const token = ref(null)

  const login = async (credentials) => {
    // Login logic
    const response = await api.login(credentials)
    user.value = response.user
    token.value = response.token
  }

  const logout = () => {
    user.value = null
    token.value = null
  }

  const isAuthenticated = computed(() => !!user.value)

  return {
    user,
    token,
    isAuthenticated,
    login,
    logout
  }
}

// composables/usePermissions.js
export function usePermissions(auth) {
  const permissions = ref([])

  const hasPermission = (permission) => {
    return permissions.value.includes(permission)
  }

  const hasRole = (role) => {
    return auth.user.value?.role === role
  }

  const canAccess = (requiredPermissions = []) => {
    if (!auth.isAuthenticated.value) return false

    return requiredPermissions.every(permission =>
      hasPermission(permission)
    )
  }

  return {
    permissions,
    hasPermission,
    hasRole,
    canAccess
  }
}

// composables/useUserDashboard.js
export function useUserDashboard() {
  const auth = useAuth()
  const permissions = usePermissions(auth)

  const dashboardData = ref(null)
  const loading = ref(false)

  const loadDashboard = async () => {
    if (!auth.isAuthenticated.value) return

    loading.value = true
    try {
      const data = await api.getDashboardData(auth.user.value.id)
      dashboardData.value = data
    } catch (error) {
      console.error('Failed to load dashboard:', error)
    } finally {
      loading.value = false
    }
  }

  return {
    auth,
    permissions,
    dashboardData,
    loading,
    loadDashboard
  }
}
```

## 🚀 Best Practices

### 1. Naming Conventions

```javascript
// ✅ Gunakan 'use' prefix untuk composables
export function useCounter() { }
export function useAuth() { }
export function useLocalStorage() { }

// ❌ Hindari tanpa prefix
export function counter() { }
export function auth() { }
```

### 2. Return Consistent API

```javascript
// ✅ Return object dengan consistent structure
export function useCounter() {
  return {
    // State
    count: ref(0),
    // Computed
    isEven: computed(() => count.value % 2 === 0),
    // Methods
    increment: () => count.value++,
    decrement: () => count.value--
  }
}
```

### 3. Handle Dependencies

```javascript
// ✅ Explicit dependencies
export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  // ... logic
  return { count }
}

// ✅ Accept parameters untuk flexibility
export function useApi(baseUrl, defaultOptions = {}) {
  // ... implementation
}
```

### 4. Error Handling

```javascript
// ✅ Good error handling
export function useApi(url) {
  const data = ref(null)
  const error = ref(null)

  const execute = async () => {
    try {
      const response = await fetch(url)
      data.value = await response.json()
    } catch (err) {
      error.value = err
      console.error('API Error:', err)
    }
  }

  return { data, error, execute }
}
```

## 💡 Tips dan Trik

### 1. Composable Testing

```javascript
// tests/useCounter.test.js
import { useCounter } from '../composables/useCounter'

describe('useCounter', () => {
  it('should initialize with default value', () => {
    const { count } = useCounter()
    expect(count.value).toBe(0)
  })

  it('should increment correctly', () => {
    const { count, increment } = useCounter(5)
    increment()
    expect(count.value).toBe(6)
  })
})
```

### 2. Composable Composition

```javascript
// composables/useUserSession.js
export function useUserSession() {
  const auth = useAuth()
  const theme = useTheme()
  const preferences = useUserPreferences()

  return {
    auth,
    theme,
    preferences
  }
}
```

### 3. Reactive Parameters

```javascript
// composables/useDebounce.js
export function useDebounce(value, delay = 300) {
  const debouncedValue = ref(value.value)

  watch(value, (newValue) => {
    const timer = setTimeout(() => {
      debouncedValue.value = newValue
    }, delay)

    return () => clearTimeout(timer)
  })

  return debouncedValue
}
```

---

## 🎉 Kesimpulan

Composables adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🔄 Logic Reuse:** Gunakan logic yang sama di multiple komponen
2. **📦 Code Organization:** Pisahkan logic dari presentation
3. **🎯 Single Responsibility:** Setiap composable punya satu fokus
4. **🧪 Testability:** Logic mudah di-testing secara isolated
5. **📚 Maintainability:** Kode lebih mudah dimaintain dan dipahami

### Poin Penting:

- Gunakan prefix 'use' untuk naming
- Return object dengan state, computed, dan methods
- Handle error dengan baik
- Test composables secara terpisah
- Compose composables untuk logic kompleks

### Kapan Menggunakan Composables:

- ✅ Logic yang digunakan di multiple komponen
- ✅ Complex state management
- ✅ API calls dan data fetching
- ✅ Form logic dan validation
- ✅ Browser API integrations

### Kapan Menghindari:

- ❌ Simple component-specific logic
- ❌ Logic yang hanya digunakan sekali
- ❌ Pure presentation logic

Dengan memahami composables, Anda sudah bisa membangun aplikasi Vue.js yang modular, maintainable, dan reusable! 🚀

---

**🎯 Ready for Practice?** Coba buat composables untuk berbagai use cases dalam aplikasi Anda untuk melihat manfaatnya langsung!