# 📋 Tutorial Vue.js: Working With Collections & Lists untuk Pemula

## 📖 Pengenalan Working With Collections & Lists

**Working With Collections & Lists** adalah kemampuan Vue.js untuk melakukan iterasi (looping) melalui array dan object untuk menampilkan data secara dinamis. Ini adalah fitur fundamental yang akan Anda gunakan dalam hampir setiap aplikasi Vue.js.

## 🤔 Apa itu v-for?

**`v-for`** adalah directive Vue.js yang digunakan untuk merender list of elements berdasarkan data array atau object. Ini mirip dengan loop dalam pemrograman tradisional, tetapi dengan reactivity built-in.

**Sintaks Dasar:**
```vue
<!-- Array iteration -->
<li v-for="item in items" :key="item.id">{{ item.name }}</li>

<!-- Array with index -->
<li v-for="(item, index) in items" :key="item.id">{{ index }}: {{ item.name }}</li>

<!-- Object iteration -->
<li v-for="(value, key) in object" :key="key">{{ key }}: {{ value }}</li>

<!-- Object with index -->
<li v-for="(value, key, index) in object" :key="key">{{ index }}. {{ key }}: {{ value }}</li>

<!-- Number range -->
<li v-for="n in 10" :key="n">{{ n }}</li>
```

## 📁 Struktur Proyek

```
13. Working With Collections & Lists/
├── README.md                                            # Dokumentasi ini
└── src/
    ├── components/
    │   └── IterationsComponent.vue                     # Komponen dengan contoh v-for
    └── App.vue                                           # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `IterationsComponent.vue`

```vue
<template>
  <ul v-for="number in 5" :key="number">
    <li>{{ number }}</li>
  </ul>
</template>
```

**Penjelasan:**
- `v-for="number in 5"` akan menghasilkan loop dari 1 sampai 5
- `:key="number"` adalah attribute yang penting untuk performance
- Vue akan merender elemen `<ul>` dan `<li>` untuk setiap iterasi

## 📖 Konsep Fundamental v-for

### 1. **Array Iteration - Basic**
```vue
<script setup>
const fruits = ['Apple', 'Banana', 'Orange', 'Mango']
</script>

<template>
  <ul>
    <li v-for="fruit in fruits" :key="fruit">{{ fruit }}</li>
  </ul>
</template>
```

### 2. **Array Iteration - With Index**
```vue
<script setup>
const fruits = ['Apple', 'Banana', 'Orange', 'Mango']
</script>

<template>
  <ul>
    <li v-for="(fruit, index) in fruits" :key="fruit">
      {{ index + 1 }}. {{ fruit }}
    </li>
  </ul>
</template>
```

### 3. **Object Iteration**
```vue
<script setup>
const user = {
  name: 'John Doe',
  age: 25,
  email: 'john@example.com',
  city: 'New York'
}
</script>

<template>
  <ul>
    <li v-for="(value, key) in user" :key="key">
      {{ key }}: {{ value }}
    </li>
  </ul>
</template>
```

### 4. **Object Array Iteration dengan Destructuring**
```vue
<script setup>
const users = [
  { id: 1, name: 'Alice', age: 25 },
  { id: 2, name: 'Bob', age: 30 },
  { id: 3, name: 'Charlie', age: 35 }
]
</script>

<template>
  <ul>
    <li v-for="{ id, name, age } in users" :key="id">
      {{ name }} is {{ age }} years old
    </li>
  </ul>
</template>
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan v-for:

### Enhanced Demo: Working With Collections & Lists Showcase

```vue
<script setup>
import { ref, computed } from 'vue'

// 1. Basic Array Iteration
const fruits = ref(['🍎 Apple', '🍌 Banana', '🍊 Orange', '🥭 Mango', '🍇 Grapes'])

// 2. Number Range Iteration
const rating = ref(5)

// 3. Object Array Iteration
const users = ref([
  { id: 1, name: 'Alice Johnson', age: 28, email: 'alice@example.com', role: 'admin', isActive: true, joinDate: '2022-01-15' },
  { id: 2, name: 'Bob Smith', age: 32, email: 'bob@example.com', role: 'user', isActive: true, joinDate: '2022-03-20' },
  { id: 3, name: 'Charlie Brown', age: 24, email: 'charlie@example.com', role: 'user', isActive: false, joinDate: '2023-01-10' },
  { id: 4, name: 'Diana Wilson', age: 29, email: 'diana@example.com', role: 'moderator', isActive: true, joinDate: '2021-11-05' },
  { id: 5, name: 'Eve Davis', age: 35, email: 'eve@example.com', role: 'user', isActive: true, joinDate: '2023-02-28' }
])

// 4. Nested Array Iteration
const categories = ref([
  {
    name: 'Fruits',
    items: ['🍎 Apple', '🍌 Banana', '🍊 Orange', '🥝 Kiwi'],
    colors: ['red', 'yellow', 'orange', 'brown']
  },
  {
    name: 'Vegetables',
    items: ['🥕 Carrot', '🥦 Broccoli', '🥒 Cucumber', '🍅 Tomato'],
    colors: ['orange', 'green', 'green', 'red']
  },
  {
    name: 'Proteins',
    items: ['🥩 Beef', '🍗 Chicken', '🐟 Fish', '🥚 Eggs'],
    colors: ['red', 'white', 'white', 'white']
  }
])

// 5. Object Iteration
const userProfile = ref({
  personalInfo: {
    firstName: 'John',
    lastName: 'Doe',
    age: 25,
    birthDate: '1998-05-15',
    gender: 'male'
  },
  contactInfo: {
    email: 'john.doe@example.com',
    phone: '+1 234 567 8900',
    address: '123 Main St',
    city: 'New York',
    country: 'USA'
  },
  preferences: {
    theme: 'dark',
    language: 'en',
    timezone: 'UTC-5',
    notifications: {
      email: true,
      push: false,
      sms: true
    }
  },
  stats: {
    loginCount: 42,
    lastLogin: '2024-01-15',
    accountCreated: '2022-01-01',
    posts: 156,
    followers: 1200
  }
})

// 6. Complex Data Structure
const products = ref([
  {
    id: 1,
    name: 'Laptop Pro',
    category: 'Electronics',
    price: 15000000,
    inStock: true,
    rating: 4.5,
    reviews: [
      { id: 1, user: 'Alice', rating: 5, comment: 'Excellent product!' },
      { id: 2, user: 'Bob', rating: 4, comment: 'Good value for money' },
      { id: 3, user: 'Charlie', rating: 4, comment: 'Very satisfied' }
    ],
    specifications: {
      cpu: 'Intel Core i7',
      ram: '16GB',
      storage: '512GB SSD',
      display: '15.6 inch',
      weight: '1.8kg'
    }
  },
  {
    id: 2,
    name: 'Wireless Mouse',
    category: 'Accessories',
    price: 250000,
    inStock: true,
    rating: 4.2,
    reviews: [
      { id: 4, user: 'Diana', rating: 5, comment: 'Very comfortable!' },
      { id: 5, user: 'Eve', rating: 4, comment: 'Good battery life' }
    ],
    specifications: {
      type: 'Wireless',
      connectivity: 'Bluetooth 5.0',
      battery: '6 months',
      dpi: '1600',
      buttons: '6 programmable'
    }
  },
  {
    id: 3,
    name: 'Mechanical Keyboard',
    category: 'Accessories',
    price: 750000,
    inStock: false,
    rating: 4.8,
    reviews: [
      { id: 6, user: 'Frank', rating: 5, comment: 'Amazing typing experience!' },
      { id: 7, user: 'Grace', rating: 5, comment: 'Love the RGB lighting' },
      { id: 8, user: 'Henry', rating: 4, comment: 'A bit loud but great' }
    ],
    specifications: {
      type: 'Mechanical',
      switches: 'Cherry MX Red',
      layout: 'TKL (87 keys)',
      backlight: 'RGB',
      cable: 'USB-C detachable'
    }
  }
])

// 7. Todo List with Categories
const todoCategories = ref([
  {
    id: 1,
    name: 'Work',
    color: '#3498db',
    todos: [
      { id: 1, text: 'Complete project proposal', completed: true, priority: 'high' },
      { id: 2, text: 'Review code changes', completed: false, priority: 'medium' },
      { id: 3, text: 'Team meeting preparation', completed: false, priority: 'high' }
    ]
  },
  {
    id: 2,
    name: 'Personal',
    color: '#e74c3c',
    todos: [
      { id: 4, text: 'Grocery shopping', completed: false, priority: 'medium' },
      { id: 5, text: 'Call mom', completed: true, priority: 'low' },
      { id: 6, text: 'Exercise', completed: false, priority: 'medium' }
    ]
  },
  {
    id: 3,
    name: 'Learning',
    color: '#f39c12',
    todos: [
      { id: 7, text: 'Read Vue.js documentation', completed: true, priority: 'high' },
      { id: 8, text: 'Practice JavaScript exercises', completed: false, priority: 'medium' },
      { id: 9, text: 'Watch tutorial videos', completed: false, priority: 'low' }
    ]
  }
])

// 8. Music Playlist
const playlist = ref([
  {
    id: 1,
    title: 'Bohemian Rhapsody',
    artist: 'Queen',
    album: 'A Night at the Opera',
    duration: '5:55',
    year: 1975,
    genre: 'Rock',
    isFavorite: true
  },
  {
    id: 2,
    title: 'Stairway to Heaven',
    artist: 'Led Zeppelin',
    album: 'Led Zeppelin IV',
    duration: '8:02',
    year: 1971,
    genre: 'Rock',
    isFavorite: true
  },
  {
    id: 3,
    title: 'Hotel California',
    artist: 'Eagles',
    album: 'Hotel California',
    duration: '6:31',
    year: 1976,
    genre: 'Rock',
    isFavorite: false
  },
  {
    id: 4,
    title: 'Sweet Child O\' Mine',
    artist: 'Guns N\' Roses',
    album: 'Appetite for Destruction',
    duration: '5:56',
    year: 1987,
    genre: 'Rock',
    isFavorite: true
  }
])

// 9. Blog Posts with Tags
const blogPosts = ref([
  {
    id: 1,
    title: 'Getting Started with Vue.js',
    author: 'John Doe',
    date: '2024-01-15',
    content: 'Learn the basics of Vue.js...',
    tags: ['Vue.js', 'JavaScript', 'Frontend', 'Tutorial'],
    likes: 42,
    comments: [
      { id: 1, author: 'Alice', text: 'Great article!', date: '2024-01-16' },
      { id: 2, author: 'Bob', text: 'Very helpful', date: '2024-01-17' }
    ]
  },
  {
    id: 2,
    title: 'Advanced JavaScript Techniques',
    author: 'Jane Smith',
    date: '2024-01-10',
    content: 'Master advanced JavaScript concepts...',
    tags: ['JavaScript', 'Advanced', 'Programming'],
    likes: 28,
    comments: [
      { id: 3, author: 'Charlie', text: 'Mind blowing!', date: '2024-01-11' }
    ]
  }
])

// 10. Restaurant Menu
const restaurantMenu = ref([
  {
    category: 'Appetizers',
    items: [
      { id: 1, name: 'Spring Rolls', price: 25000, description: 'Crispy vegetable rolls', allergens: ['gluten'], isVegetarian: true },
      { id: 2, name: 'Chicken Wings', price: 35000, description: 'Spicy buffalo wings', allergens: ['dairy'], isVegetarian: false },
      { id: 3, name: 'Bruschetta', price: 30000, description: 'Toasted bread with tomatoes', allergens: ['gluten'], isVegetarian: true }
    ]
  },
  {
    category: 'Main Courses',
    items: [
      { id: 4, name: 'Grilled Salmon', price: 85000, description: 'Fresh Atlantic salmon', allergens: ['fish'], isVegetarian: false },
      { id: 5, name: 'Pasta Carbonara', price: 45000, description: 'Classic Italian pasta', allergens: ['gluten', 'dairy', 'eggs'], isVegetarian: false },
      { id: 6, name: 'Vegetable Curry', price: 40000, description: 'Mixed vegetables in curry sauce', allergens: ['nuts'], isVegetarian: true }
    ]
  },
  {
    category: 'Desserts',
    items: [
      { id: 7, name: 'Chocolate Cake', price: 25000, description: 'Rich chocolate cake', allergens: ['gluten', 'dairy', 'eggs'], isVegetarian: true },
      { id: 8, name: 'Ice Cream Sundae', price: 20000, description: 'Vanilla ice cream with toppings', allergens: ['dairy'], isVegetarian: true }
    ]
  }
])

// Computed properties
const activeUsers = computed(() => users.value.filter(user => user.isActive))
const totalUsers = computed(() => users.value.length)
const adminUsers = computed(() => users.value.filter(user => user.role === 'admin'))
const averageAge = computed(() => {
  const total = users.value.reduce((sum, user) => sum + user.age, 0)
  return Math.round(total / users.value.length)
})

const totalProducts = computed(() => products.value.length)
const inStockProducts = computed(() => products.value.filter(p => p.inStock))
const outOfStockProducts = computed(() => products.value.filter(p => !p.inStock))
const averageRating = computed(() => {
  const total = products.value.reduce((sum, p) => sum + p.rating, 0)
  return (total / products.value.length).toFixed(1)
})

const totalTodos = computed(() => {
  return todoCategories.value.reduce((total, category) => total + category.todos.length, 0)
})
const completedTodos = computed(() => {
  return todoCategories.value.reduce((total, category) => {
    return total + category.todos.filter(todo => todo.completed).length
  }, 0)
})

const totalDuration = computed(() => {
  const totalSeconds = playlist.value.reduce((total, song) => {
    const [minutes, seconds] = song.duration.split(':').map(Number)
    return total + (minutes * 60 + seconds)
  }, 0)
  const totalMinutes = Math.floor(totalSeconds / 60)
  const remainingSeconds = totalSeconds % 60
  return `${totalMinutes}:${remainingSeconds.toString().padStart(2, '0')}`
})

const totalLikes = computed(() => blogPosts.value.reduce((total, post) => total + post.likes, 0))
const totalComments = computed(() => blogPosts.value.reduce((total, post) => total + post.comments.length, 0))

// Event handlers
function addFruit() {
  const newFruits = ['🍓 Strawberry', '🥑 Avocado', '🍍 Pineapple', '🥝 Kiwi']
  const randomFruit = newFruits[Math.floor(Math.random() * newFruits.length)]
  fruits.value.push(randomFruit)
}

function removeFruit(index) {
  fruits.value.splice(index, 1)
}

function updateRating(newRating) {
  rating.value = Math.max(1, Math.min(10, newRating))
}

function addUser() {
  const newId = Math.max(...users.value.map(u => u.id)) + 1
  const names = ['Frank Miller', 'Grace Lee', 'Henry Wilson', 'Iris Chen', 'Jack Brown']
  const randomName = names[Math.floor(Math.random() * names.length)]

  users.value.push({
    id: newId,
    name: randomName,
    age: Math.floor(Math.random() * 40) + 20,
    email: randomName.toLowerCase().replace(' ', '.') + '@example.com',
    role: ['user', 'moderator', 'admin'][Math.floor(Math.random() * 3)],
    isActive: Math.random() > 0.3,
    joinDate: new Date().toISOString().split('T')[0]
  })
}

function toggleUserStatus(userId) {
  const user = users.value.find(u => u.id === userId)
  if (user) {
    user.isActive = !user.isActive
  }
}

function addCategory() {
  const newCategoryName = prompt('Enter category name:')
  if (newCategoryName) {
    categories.value.push({
      name: newCategoryName,
      items: [],
      colors: []
    })
  }
}

function addItemToCategory(categoryIndex) {
  const newItem = prompt('Enter item name:')
  if (newItem && categories.value[categoryIndex]) {
    categories.value[categoryIndex].items.push(newItem)
  }
}

function addTodo(categoryId) {
  const todoText = prompt('Enter todo text:')
  if (todoText) {
    const category = todoCategories.value.find(c => c.id === categoryId)
    if (category) {
      const newId = Math.max(...category.todos.map(t => t.id), 0) + 1
      category.todos.push({
        id: newId,
        text: todoText,
        completed: false,
        priority: 'medium'
      })
    }
  }
}

function toggleTodo(categoryId, todoId) {
  const category = todoCategories.value.find(c => c.id === categoryId)
  if (category) {
    const todo = category.todos.find(t => t.id === todoId)
    if (todo) {
      todo.completed = !todo.completed
    }
  }
}

function toggleFavorite(songId) {
  const song = playlist.value.find(s => s.id === songId)
  if (song) {
    song.isFavorite = !song.isFavorite
  }
}

function addSong() {
  const songs = [
    { title: 'Imagine', artist: 'John Lennon', duration: '3:03' },
    { title: 'Yesterday', artist: 'The Beatles', duration: '2:05' },
    { title: 'Hey Jude', artist: 'The Beatles', duration: '7:11' }
  ]
  const randomSong = songs[Math.floor(Math.random() * songs.length)]
  const newId = Math.max(...playlist.value.map(s => s.id), 0) + 1

  playlist.value.push({
    id: newId,
    ...randomSong,
    album: 'Unknown',
    year: new Date().getFullYear(),
    genre: 'Pop',
    isFavorite: false
  })
}

function likePost(postId) {
  const post = blogPosts.value.find(p => p.id === postId)
  if (post) {
    post.likes++
  }
}

function addComment(postId) {
  const commentText = prompt('Enter your comment:')
  if (commentText) {
    const post = blogPosts.value.find(p => p.id === postId)
    if (post) {
      const newId = Math.max(...post.comments.map(c => c.id), 0) + 1
      post.comments.push({
        id: newId,
        author: 'Anonymous',
        text: commentText,
        date: new Date().toISOString().split('T')[0]
      })
    }
  }
}
</script>

<template>
  <div class="collections-demo">
    <h1>📋 Vue.js Collections & Lists Showcase</h1>

    <!-- 1. Basic Array Iteration -->
    <section class="demo-section">
      <h2>1️⃣ Basic Array Iteration</h2>
      <div class="demo-content">
        <div class="fruits-demo">
          <h3>Fruits Collection</h3>
          <div class="fruits-grid">
            <div v-for="(fruit, index) in fruits" :key="fruit" class="fruit-item">
              <span class="fruit-emoji">{{ fruit.split(' ')[0] }}</span>
              <span class="fruit-name">{{ fruit.split(' ')[1] }}</span>
              <button @click="removeFruit(index)" class="remove-btn">×</button>
            </div>
          </div>
          <button @click="addFruit" class="add-btn">Add Random Fruit</button>
        </div>
      </div>
    </section>

    <!-- 2. Number Range Iteration -->
    <section class="demo-section">
      <h2>2️⃣ Number Range Iteration</h2>
      <div class="demo-content">
        <div class="rating-demo">
          <h3>Star Rating ({{ rating }}/10)</h3>
          <div class="stars-container">
            <div v-for="star in 10" :key="star" class="star-container">
              <span
                :class="['star', { filled: star <= rating }]"
                @click="updateRating(star)"
              >
                ⭐
              </span>
            </div>
          </div>
          <div class="rating-controls">
            <button @click="updateRating(rating - 1)">⬇️ Decrease</button>
            <button @click="updateRating(rating + 1)">⬆️ Increase</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. Object Array Iteration -->
    <section class="demo-section">
      <h2>3️⃣ Object Array Iteration</h2>
      <div class="demo-content">
        <div class="users-demo">
          <h3>User Management System</h3>

          <div class="users-stats">
            <div class="stat-card">
              <h4>Total Users</h4>
              <span class="stat-number">{{ totalUsers }}</span>
            </div>
            <div class="stat-card">
              <h4>Active Users</h4>
              <span class="stat-number active">{{ activeUsers.length }}</span>
            </div>
            <div class="stat-card">
              <h4>Admins</h4>
              <span class="stat-number admin">{{ adminUsers.length }}</span>
            </div>
            <div class="stat-card">
              <h4>Average Age</h4>
              <span class="stat-number">{{ averageAge }}</span>
            </div>
          </div>

          <div class="users-list">
            <div
              v-for="user in users"
              :key="user.id"
              class="user-card"
              :class="{ inactive: !user.isActive }"
            >
              <div class="user-avatar">
                <span class="avatar-text">{{ user.name.charAt(0) }}</span>
              </div>
              <div class="user-info">
                <h4>{{ user.name }}</h4>
                <p>{{ user.email }}</p>
                <p>Age: {{ user.age }} | Role: {{ user.role }}</p>
                <p>Joined: {{ user.joinDate }}</p>
              </div>
              <div class="user-actions">
                <button
                  @click="toggleUserStatus(user.id)"
                  :class="{ active: user.isActive }"
                >
                  {{ user.isActive ? 'Deactivate' : 'Activate' }}
                </button>
              </div>
            </div>
          </div>

          <div class="users-actions">
            <button @click="addUser">Add Random User</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 4. Nested Array Iteration -->
    <section class="demo-section">
      <h2>4️⃣ Nested Array Iteration</h2>
      <div class="demo-content">
        <div class="categories-demo">
          <h3>Food Categories</h3>

          <div class="categories-container">
            <div
              v-for="(category, categoryIndex) in categories"
              :key="category.name"
              class="category-card"
            >
              <h4>{{ category.name }}</h4>

              <div class="items-list">
                <h5>Items:</h5>
                <div v-for="(item, itemIndex) in category.items" :key="itemIndex" class="item">
                  <span class="item-name">{{ item }}</span>
                  <span
                    class="color-dot"
                    :style="{ backgroundColor: category.colors[itemIndex] }"
                  ></span>
                </div>
              </div>

              <button @click="addItemToCategory(categoryIndex)" class="add-item-btn">
                Add Item
              </button>
            </div>
          </div>

          <div class="categories-actions">
            <button @click="addCategory">Add Category</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. Object Iteration -->
    <section class="demo-section">
      <h2>5️⃣ Object Iteration</h2>
      <div class="demo-content">
        <div class="profile-demo">
          <h3>User Profile Information</h3>

          <div
            v-for="(section, sectionName) in userProfile"
            :key="sectionName"
            class="profile-section"
          >
            <h4>{{ formatSectionName(sectionName) }}</h4>
            <div
              v-for="(value, key) in section"
              :key="key"
              class="profile-item"
            >
              <span class="item-key">{{ formatKey(key) }}:</span>
              <span class="item-value">{{ formatValue(value) }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Complex Data Structure -->
    <section class="demo-section">
      <h2>6️⃣ Complex Data Structure</h2>
      <div class="demo-content">
        <div class="products-demo">
          <h3>Product Catalog</h3>

          <div class="products-stats">
            <div class="stat-card">
              <h4>Total Products</h4>
              <span class="stat-number">{{ totalProducts }}</span>
            </div>
            <div class="stat-card">
              <h4>In Stock</h4>
              <span class="stat-number in-stock">{{ inStockProducts.length }}</span>
            </div>
            <div class="stat-card">
              <h4>Out of Stock</h4>
              <span class="stat-number out-of-stock">{{ outOfStockProducts.length }}</span>
            </div>
            <div class="stat-card">
              <h4>Avg Rating</h4>
              <span class="stat-number">{{ averageRating }}</span>
            </div>
          </div>

          <div class="products-grid">
            <div
              v-for="product in products"
              :key="product.id"
              class="product-card"
              :class="{ 'out-of-stock': !product.inStock }"
            >
              <div class="product-header">
                <h4>{{ product.name }}</h4>
                <span class="product-category">{{ product.category }}</span>
              </div>

              <div class="product-price">
                {{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(product.price) }}
              </div>

              <div class="product-rating">
                <span class="rating-stars">
                  <span v-for="star in 5" :key="star" class="rating-star">
                    {{ star <= product.rating ? '⭐' : '☆' }}
                  </span>
                </span>
                <span class="rating-value">({{ product.rating }})</span>
              </div>

              <div class="product-specs">
                <h5>Specifications:</h5>
                <div
                  v-for="(value, key) in product.specifications"
                  :key="key"
                  class="spec-item"
                >
                  <span class="spec-key">{{ key }}:</span>
                  <span class="spec-value">{{ value }}</span>
                </div>
              </div>

              <div class="product-reviews">
                <h5>Recent Reviews:</h5>
                <div
                  v-for="review in product.reviews.slice(0, 2)"
                  :key="review.id"
                  class="review-item"
                >
                  <div class="review-header">
                    <span class="review-author">{{ review.user }}</span>
                    <span class="review-rating">{{ review.rating }}/5</span>
                  </div>
                  <p class="review-comment">{{ review.comment }}</p>
                </div>
              </div>

              <div class="product-status">
                <span
                  v-if="product.inStock"
                  class="status-badge in-stock"
                >
                  ✅ In Stock
                </span>
                <span
                  v-else
                  class="status-badge out-of-stock"
                >
                  ❌ Out of Stock
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 7. Todo List with Categories -->
    <section class="demo-section">
      <h2>7️⃣ Todo List with Categories</h2>
      <div class="demo-content">
        <div class="todos-demo">
          <h3>Task Management System</h3>

          <div class="todos-stats">
            <div class="stat-card">
              <h4>Total Tasks</h4>
              <span class="stat-number">{{ totalTodos }}</span>
            </div>
            <div class="stat-card">
              <h4>Completed</h4>
              <span class="stat-number completed">{{ completedTodos }}</span>
            </div>
            <div class="stat-card">
              <h4>Progress</h4>
              <span class="stat-number">
                {{ totalTodos > 0 ? Math.round((completedTodos / totalTodos) * 100) : 0 }}%
              </span>
            </div>
          </div>

          <div class="progress-bar">
            <div
              class="progress-fill"
              :style="{ width: (totalTodos > 0 ? (completedTodos / totalTodos) * 100 : 0) + '%' }"
            ></div>
          </div>

          <div class="todo-categories">
            <div
              v-for="category in todoCategories"
              :key="category.id"
              class="todo-category"
              :style="{ borderColor: category.color }"
            >
              <h4 :style="{ color: category.color }">{{ category.name }}</h4>

              <div class="todo-list">
                <div
                  v-for="todo in category.todos"
                  :key="todo.id"
                  class="todo-item"
                  :class="{ completed: todo.completed }"
                >
                  <input
                    type="checkbox"
                    :checked="todo.completed"
                    @change="toggleTodo(category.id, todo.id)"
                  />
                  <span class="todo-text">{{ todo.text }}</span>
                  <span
                    class="todo-priority"
                    :class="todo.priority"
                  >
                    {{ todo.priority }}
                  </span>
                </div>
              </div>

              <div class="category-actions">
                <button @click="addTodo(category.id)" class="add-todo-btn">
                  Add Task
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 8. Music Playlist -->
    <section class="demo-section">
      <h2>8️⃣ Music Playlist</h2>
      <div class="demo-content">
        <div class="playlist-demo">
          <h3>My Favorite Songs</h3>

          <div class="playlist-info">
            <p><strong>Total Songs:</strong> {{ playlist.length }}</p>
            <p><strong>Total Duration:</strong> {{ totalDuration }}</p>
            <p><strong>Favorite Songs:</strong> {{ playlist.filter(s => s.isFavorite).length }}</p>
          </div>

          <div class="playlist-table">
            <table>
              <thead>
                <tr>
                  <th>#</th>
                  <th>Favorite</th>
                  <th>Title</th>
                  <th>Artist</th>
                  <th>Album</th>
                  <th>Duration</th>
                  <th>Year</th>
                  <th>Genre</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(song, index) in playlist"
                  :key="song.id"
                  class="song-row"
                  :class="{ favorite: song.isFavorite }"
                >
                  <td>{{ index + 1 }}</td>
                  <td>
                    <button
                      @click="toggleFavorite(song.id)"
                      class="favorite-btn"
                      :class="{ active: song.isFavorite }"
                    >
                      {{ song.isFavorite ? '❤️' : '🤍' }}
                    </button>
                  </td>
                  <td class="song-title">{{ song.title }}</td>
                  <td>{{ song.artist }}</td>
                  <td>{{ song.album }}</td>
                  <td>{{ song.duration }}</td>
                  <td>{{ song.year }}</td>
                  <td>{{ song.genre }}</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="playlist-actions">
            <button @click="addSong">Add Song</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 9. Blog Posts with Tags -->
    <section class="demo-section">
      <h2>9️⃣ Blog Posts with Tags</h2>
      <div class="demo-content">
        <div class="blog-demo">
          <h3>Latest Blog Posts</h3>

          <div class="blog-stats">
            <div class="stat-card">
              <h4>Total Posts</h4>
              <span class="stat-number">{{ blogPosts.length }}</span>
            </div>
            <div class="stat-card">
              <h4>Total Likes</h4>
              <span class="stat-number">{{ totalLikes }}</span>
            </div>
            <div class="stat-card">
              <h4>Total Comments</h4>
              <span class="stat-number">{{ totalComments }}</span>
            </div>
          </div>

          <div class="blog-list">
            <article
              v-for="post in blogPosts"
              :key="post.id"
              class="blog-post"
            >
              <div class="post-header">
                <h2>{{ post.title }}</h2>
                <div class="post-meta">
                  <span class="author">By {{ post.author }}</span>
                  <span class="date">{{ post.date }}</span>
                </div>
              </div>

              <div class="post-content">
                <p>{{ post.content }}</p>
              </div>

              <div class="post-tags">
                <span
                  v-for="tag in post.tags"
                  :key="tag"
                  class="tag"
                >
                  #{{ tag }}
                </span>
              </div>

              <div class="post-stats">
                <button @click="likePost(post.id)" class="like-btn">
                  👍 {{ post.likes }} Likes
                </button>
                <span class="comment-count">
                  💬 {{ post.comments.length }} Comments
                </span>
              </div>

              <div class="post-comments">
                <h4>Comments:</h4>
                <div
                  v-for="comment in post.comments"
                  :key="comment.id"
                  class="comment"
                >
                  <div class="comment-header">
                    <span class="comment-author">{{ comment.author }}</span>
                    <span class="comment-date">{{ comment.date }}</span>
                  </div>
                  <p class="comment-text">{{ comment.text }}</p>
                </div>

                <div class="add-comment">
                  <button @click="addComment(post.id)" class="add-comment-btn">
                    Add Comment
                  </button>
                </div>
              </div>
            </article>
          </div>
        </div>
      </div>
    </section>

    <!-- 10. Restaurant Menu -->
    <section class="demo-section">
      <h2>🔟 Restaurant Menu</h2>
      <div class="demo-content">
        <div class="menu-demo">
          <h3>Restaurant Menu</h3>

          <div class="menu-sections">
            <div
              v-for="section in restaurantMenu"
              :key="section.category"
              class="menu-section"
            >
              <h4>{{ section.category }}</h4>

              <div class="menu-items">
                <div
                  v-for="item in section.items"
                  :key="item.id"
                  class="menu-item"
                >
                  <div class="item-header">
                    <h5>{{ item.name }}</h5>
                    <span class="item-price">
                      {{ new Intl.NumberFormat('id-ID', {
                        style: 'currency',
                        currency: 'IDR'
                      }).format(item.price) }}
                    </span>
                  </div>

                  <p class="item-description">{{ item.description }}</p>

                  <div class="item-details">
                    <div class="dietary-info">
                      <span
                        v-if="item.isVegetarian"
                        class="dietary-badge vegetarian"
                      >
                        🥬 Vegetarian
                      </span>
                      <span
                        v-for="allergen in item.allergens"
                        :key="allergen"
                        class="allergen-badge"
                      >
                        ⚠️ {{ allergen }}
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// Helper functions for formatting
function formatSectionName(sectionName) {
  return sectionName
    .replace(/([A-Z])/g, ' $1')
    .replace(/^./, str => str.toUpperCase())
}

function formatKey(key) {
  return key
    .replace(/([A-Z])/g, ' $1')
    .replace(/^./, str => str.toUpperCase())
}

function formatValue(value) {
  if (typeof value === 'object' && value !== null) {
    return JSON.stringify(value, null, 2)
  }
  return String(value)
}
</script>

<style scoped>
.collections-demo {
  max-width: 1200px;
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

/* Fruits Demo */
.fruits-demo {
  text-align: center;
}

.fruits-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  margin: 20px 0;
}

.fruit-item {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 12px;
  background-color: #f8f9fa;
  border-radius: 20px;
  border: 1px solid #dee2e6;
  transition: all 0.3s ease;
}

.fruit-item:hover {
  background-color: #e3f2fd;
  border-color: #42b883;
}

.fruit-emoji {
  font-size: 1.2em;
}

.fruit-name {
  font-weight: 500;
}

.remove-btn {
  background: none;
  border: none;
  color: #dc3545;
  cursor: pointer;
  font-size: 16px;
  padding: 0;
  margin-left: 5px;
}

.add-btn {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

/* Rating Demo */
.rating-demo {
  text-align: center;
}

.stars-container {
  display: flex;
  justify-content: center;
  gap: 5px;
  margin: 20px 0;
}

.star-container {
  cursor: pointer;
}

.star {
  font-size: 1.5em;
  transition: all 0.2s ease;
}

.star.filled {
  color: #ffd700;
}

.star:not(.filled) {
  color: #ddd;
}

.star:hover {
  transform: scale(1.2);
}

.rating-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
}

/* Users Demo */
.users-demo {
  text-align: center;
}

.users-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.stat-card {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.stat-card h4 {
  margin: 0 0 10px 0;
  color: #495057;
  font-size: 0.9em;
}

.stat-number {
  font-size: 1.5em;
  font-weight: bold;
  color: #42b883;
}

.stat-number.active {
  color: #28a745;
}

.stat-number.admin {
  color: #dc3545;
}

.users-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.user-card {
  background-color: #ffffff;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 15px;
  text-align: left;
  display: flex;
  align-items: center;
  gap: 15px;
  transition: all 0.3s ease;
}

.user-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.user-card.inactive {
  opacity: 0.7;
  background-color: #f8f9fa;
}

.user-avatar {
  flex-shrink: 0;
}

.avatar-text {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #42b883;
  color: white;
  font-size: 1.2em;
  font-weight: bold;
}

.user-info {
  flex: 1;
}

.user-info h4 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.user-info p {
  margin: 2px 0;
  font-size: 0.9em;
  color: #6c757d;
}

.user-actions {
  flex-shrink: 0;
}

.user-actions button {
  padding: 6px 12px;
  font-size: 0.8em;
}

.user-actions button.active {
  background-color: #dc3545;
}

.users-actions {
  text-align: center;
}

/* Categories Demo */
.categories-demo {
  text-align: center;
}

.categories-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.category-card {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  text-align: left;
}

.category-card h4 {
  margin: 0 0 15px 0;
  color: #2c3e50;
  text-align: center;
}

.items-list h5 {
  margin: 0 0 10px 0;
  color: #495057;
  font-size: 0.9em;
}

.item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 5px 10px;
  margin: 5px 0;
  background-color: white;
  border-radius: 4px;
}

.item-name {
  flex: 1;
}

.color-dot {
  width: 12px;
  height: 12px;
  border-radius: 50%;
  margin-left: 10px;
}

.add-item-btn {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9em;
  margin-top: 10px;
}

.categories-actions {
  text-align: center;
}

/* Profile Demo */
.profile-demo {
  max-width: 800px;
  margin: 0 auto;
}

.profile-section {
  background-color: #f8f9fa;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
}

.profile-section h4 {
  margin: 0 0 15px 0;
  color: #42b883;
  border-bottom: 2px solid #42b883;
  padding-bottom: 5px;
}

.profile-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #e9ecef;
}

.profile-item:last-child {
  border-bottom: none;
}

.item-key {
  font-weight: bold;
  color: #495057;
  min-width: 150px;
}

.item-value {
  color: #212529;
  word-break: break-all;
}

/* Products Demo */
.products-demo {
  text-align: center;
}

.products-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.product-card {
  background-color: #ffffff;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  text-align: left;
  transition: all 0.3s ease;
}

.product-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.product-card.out-of-stock {
  opacity: 0.7;
  background-color: #f8f9fa;
}

.product-header {
  margin-bottom: 15px;
}

.product-header h4 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.product-category {
  font-size: 0.8em;
  color: #6c757d;
  background-color: #e9ecef;
  padding: 2px 8px;
  border-radius: 12px;
}

.product-price {
  font-size: 1.3em;
  font-weight: bold;
  color: #42b883;
  margin-bottom: 10px;
}

.product-rating {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.rating-stars {
  display: flex;
  gap: 2px;
}

.rating-star {
  font-size: 0.9em;
}

.rating-value {
  font-size: 0.8em;
  color: #6c757d;
}

.product-specs,
.product-reviews {
  margin-bottom: 15px;
}

.product-specs h5,
.product-reviews h5 {
  margin: 0 0 10px 0;
  color: #495057;
  font-size: 0.9em;
}

.spec-item {
  display: flex;
  justify-content: space-between;
  padding: 4px 0;
  font-size: 0.8em;
}

.spec-key {
  color: #6c757d;
}

.spec-value {
  color: #212529;
  font-weight: 500;
}

.review-item {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.review-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
}

.review-author {
  font-weight: bold;
  color: #2c3e50;
}

.review-rating {
  color: #ffc107;
}

.review-comment {
  margin: 0;
  font-size: 0.8em;
  color: #495057;
}

.product-status {
  text-align: center;
}

.status-badge {
  display: inline-block;
  padding: 4px 12px;
  border-radius: 12px;
  font-size: 0.8em;
  font-weight: bold;
}

.status-badge.in-stock {
  background-color: #d4edda;
  color: #155724;
}

.status-badge.out-of-stock {
  background-color: #f8d7da;
  color: #721c24;
}

/* Todos Demo */
.todos-demo {
  text-align: center;
}

.todos-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.progress-bar {
  width: 100%;
  height: 20px;
  background-color: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  margin-bottom: 20px;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #42b883, #369870);
  transition: width 0.3s ease;
}

.todo-categories {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.todo-category {
  background-color: #ffffff;
  border: 2px solid #42b883;
  border-radius: 8px;
  padding: 20px;
  text-align: left;
}

.todo-category h4 {
  margin: 0 0 15px 0;
  text-align: center;
}

.todo-list {
  margin-bottom: 15px;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 0;
  border-bottom: 1px solid #e9ecef;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #6c757d;
}

.todo-text {
  flex: 1;
  text-align: left;
}

.todo-priority {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.7em;
  font-weight: bold;
}

.todo-priority.high {
  background-color: #dc3545;
  color: white;
}

.todo-priority.medium {
  background-color: #ffc107;
  color: #212529;
}

.todo-priority.low {
  background-color: #28a745;
  color: white;
}

.category-actions {
  text-align: center;
}

.add-todo-btn {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 0.9em;
}

/* Playlist Demo */
.playlist-demo {
  text-align: center;
}

.playlist-info {
  background-color: #f8f9fa;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.playlist-info p {
  margin: 5px 0;
  font-weight: 500;
}

.playlist-table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.playlist-table th,
.playlist-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #dee2e6;
}

.playlist-table th {
  background-color: #f8f9fa;
  font-weight: bold;
  color: #495057;
}

.playlist-table tbody tr:hover {
  background-color: #f8f9fa;
}

.playlist-table tbody tr.favorite {
  background-color: #fff3cd;
}

.song-title {
  font-weight: bold;
  color: #2c3e50;
}

.favorite-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.2em;
  padding: 0;
}

.favorite-btn.active {
  color: #e74c3c;
}

.playlist-actions {
  text-align: center;
}

/* Blog Demo */
.blog-demo {
  text-align: center;
}

.blog-stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.blog-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 20px;
}

.blog-post {
  background-color: #ffffff;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  text-align: left;
}

.post-header {
  margin-bottom: 15px;
}

.post-header h2 {
  margin: 0 0 10px 0;
  color: #2c3e50;
}

.post-meta {
  display: flex;
  gap: 15px;
  font-size: 0.9em;
  color: #6c757d;
}

.post-content {
  margin-bottom: 15px;
}

.post-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
  margin-bottom: 15px;
}

.tag {
  background-color: #e3f2fd;
  color: #1976d2;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.8em;
}

.post-stats {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.like-btn {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
}

.comment-count {
  font-size: 0.8em;
  color: #6c757d;
}

.post-comments {
  border-top: 1px solid #e9ecef;
  padding-top: 15px;
}

.post-comments h4 {
  margin: 0 0 10px 0;
  color: #495057;
  font-size: 0.9em;
}

.comment {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 8px;
}

.comment-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 5px;
}

.comment-author {
  font-weight: bold;
  color: #2c3e50;
  font-size: 0.8em;
}

.comment-date {
  font-size: 0.8em;
  color: #6c757d;
}

.comment-text {
  margin: 0;
  font-size: 0.8em;
  color: #495057;
}

.add-comment {
  text-align: center;
  margin-top: 10px;
}

.add-comment-btn {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 0.8em;
}

/* Menu Demo */
.menu-demo {
  text-align: center;
}

.menu-sections {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
}

.menu-section {
  background-color: #ffffff;
  border: 1px solid #dee2e6;
  border-radius: 8px;
  padding: 20px;
  text-align: left;
}

.menu-section h4 {
  margin: 0 0 20px 0;
  color: #2c3e50;
  text-align: center;
  font-size: 1.2em;
  border-bottom: 2px solid #42b883;
  padding-bottom: 10px;
}

.menu-item {
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 1px solid #e9ecef;
}

.menu-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
  padding-bottom: 0;
}

.item-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.item-header h5 {
  margin: 0;
  color: #2c3e50;
}

.item-price {
  font-size: 1.1em;
  font-weight: bold;
  color: #42b883;
}

.item-description {
  color: #6c757d;
  margin-bottom: 10px;
  font-size: 0.9em;
}

.item-details {
  margin-top: 10px;
}

.dietary-info {
  display: flex;
  flex-wrap: wrap;
  gap: 5px;
}

.dietary-badge,
.allergen-badge {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 0.7em;
  font-weight: bold;
}

.dietary-badge.vegetarian {
  background-color: #d4edda;
  color: #155724;
}

.allergen-badge {
  background-color: #f8d7da;
  color: #721c24;
}

/* Global button styles */
button {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 14px;
}

button:hover {
  background-color: #369870;
  transform: translateY(-2px);
}

button:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
  transform: none;
}

/* Responsive Design */
@media (max-width: 768px) {
  .collections-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .users-stats,
  .products-stats,
  .todos-stats,
  .blog-stats {
    grid-template-columns: 1fr;
  }

  .users-list,
  .products-grid,
  .todo-categories,
  .blog-list {
    grid-template-columns: 1fr;
  }

  .categories-container,
  .menu-sections {
    grid-template-columns: 1fr;
  }

  .playlist-table {
    font-size: 0.8em;
  }

  .playlist-table th,
  .playlist-table td {
    padding: 8px 4px;
  }
}
</style>

## ❓ Pertanyaan: Apa Fungsi Working With Collections & Lists?

Working With Collections & Lists (v-for) memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk mengelola data dinamis:

### 1. **Dynamic Content Generation**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 1: Generate content secara dinamis dari data
const menuItems = ref([
  { id: 1, name: 'Home', icon: '🏠', path: '/' },
  { id: 2, name: 'About', icon: '📖', path: '/about' },
  { id: 3, name: 'Services', icon: '⚙️', path: '/services' },
  { id: 4, name: 'Contact', icon: '📧', path: '/contact' }
])

const addMenuItem = () => {
  const newId = menuItems.value.length + 1
  menuItems.value.push({
    id: newId,
    name: `Page ${newId}`,
    icon: '📄',
    path: `/page-${newId}`
  })
}
</script>

<template>
  <nav class="navigation">
    <!-- Generate menu items secara dinamis -->
    <router-link
      v-for="item in menuItems"
      :key="item.id"
      :to="item.path"
      class="nav-item"
    >
      <span class="icon">{{ item.icon }}</span>
      <span class="name">{{ item.name }}</span>
    </router-link>
  </nav>

  <button @click="addMenuItem">Add Menu Item</button>
</template>
```

**Keuntungan:**
- ✅ **Scalable UI** - mudah menambah/hapus content tanpa ubah template
- ✅ **Data-driven** - UI mengikuti struktur data
- ✅ **Maintainable** - perubahan data otomatis update tampilan

### 2. **Data Display & Formatting**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 2: Display data dengan berbagai format
const products = ref([
  { id: 1, name: 'Laptop', price: 15000000, category: 'Electronics', rating: 4.5 },
  { id: 2, name: 'Mouse', price: 250000, category: 'Accessories', rating: 4.2 },
  { id: 3, name: 'Keyboard', price: 750000, category: 'Accessories', rating: 4.8 }
])

const formatPrice = (price) => {
  return new Intl.NumberFormat('id-ID', {
    style: 'currency',
    currency: 'IDR'
  }).format(price)
}

const renderStars = (rating) => {
  const fullStars = Math.floor(rating)
  const hasHalfStar = rating % 1 !== 0
  return '⭐'.repeat(fullStars) + (hasHalfStar ? '✨' : '')
}
</script>

<template>
  <div class="product-list">
    <!-- Display data dengan formatting kustom -->
    <div v-for="product in products" :key="product.id" class="product-card">
      <h3>{{ product.name }}</h3>
      <p class="category">{{ product.category }}</p>
      <p class="price">{{ formatPrice(product.price) }}</p>
      <p class="rating">{{ renderStars(product.rating) }} ({{ product.rating }})</p>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Consistent formatting** - format data yang seragam
- ✅ **Rich display** - berbagai cara menampilkan data
- ✅ **Localization ready** - mudah adaptasi untuk berbagai bahasa

### 3. **Interactive List Management**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 3: Manajemen list interaktif
const todos = ref([
  { id: 1, text: 'Learn Vue.js', completed: true, priority: 'high' },
  { id: 2, text: 'Build amazing app', completed: false, priority: 'medium' },
  { id: 3, text: 'Master JavaScript', completed: false, priority: 'high' }
])

const newTodoText = ref('')

const addTodo = () => {
  if (newTodoText.value.trim()) {
    todos.value.push({
      id: Date.now(),
      text: newTodoText.value,
      completed: false,
      priority: 'medium'
    })
    newTodoText.value = ''
  }
}

const toggleTodo = (id) => {
  const todo = todos.value.find(t => t.id === id)
  if (todo) todo.completed = !todo.completed
}

const removeTodo = (id) => {
  const index = todos.value.findIndex(t => t.id === id)
  if (index > -1) todos.value.splice(index, 1)
}

const clearCompleted = () => {
  todos.value = todos.value.filter(todo => !todo.completed)
}
</script>

<template>
  <div class="todo-manager">
    <!-- Form untuk menambah todo -->
    <div class="todo-form">
      <input
        v-model="newTodoText"
        @keyup.enter="addTodo"
        placeholder="Enter new todo..."
      />
      <button @click="addTodo">Add Todo</button>
    </div>

    <!-- List todos interaktif -->
    <div class="todo-list">
      <div
        v-for="todo in todos"
        :key="todo.id"
        class="todo-item"
        :class="{ completed: todo.completed }"
      >
        <input
          type="checkbox"
          :checked="todo.completed"
          @change="toggleTodo(todo.id)"
        />
        <span class="todo-text">{{ todo.text }}</span>
        <span class="priority" :class="todo.priority">{{ todo.priority }}</span>
        <button @click="removeTodo(todo.id)" class="delete-btn">×</button>
      </div>
    </div>

    <!-- Actions untuk list management -->
    <div class="todo-actions">
      <button @click="clearCompleted">Clear Completed</button>
      <span>Total: {{ todos.length }} | Completed: {{ todos.filter(t => t.completed).length }}</span>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **CRUD operations** - Create, Read, Update, Delete data
- ✅ **Real-time updates** - perubahan langsung terlihat di UI
- ✅ **User interaction** - berbagai interaksi dengan list items

### 4. **Nested Data Structure Handling**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 4: Handle struktur data bersarang (nested)
const categories = ref([
  {
    name: 'Electronics',
    icon: '📱',
    subcategories: [
      {
        name: 'Phones',
        items: ['iPhone', 'Samsung Galaxy', 'Google Pixel']
      },
      {
        name: 'Laptops',
        items: ['MacBook', 'Dell XPS', 'ThinkPad']
      }
    ]
  },
  {
    name: 'Clothing',
    icon: '👕',
    subcategories: [
      {
        name: 'Men',
        items: ['T-Shirts', 'Jeans', 'Jackets']
      },
      {
        name: 'Women',
        items: ['Dresses', 'Tops', 'Skirts']
      }
    ]
  }
])

const addSubcategory = (categoryIndex) => {
  const name = prompt('Enter subcategory name:')
  if (name) {
    categories.value[categoryIndex].subcategories.push({
      name,
      items: []
    })
  }
}

const addItem = (categoryIndex, subcategoryIndex) => {
  const item = prompt('Enter item name:')
  if (item) {
    categories.value[categoryIndex].subcategories[subcategoryIndex].items.push(item)
  }
}
</script>

<template>
  <div class="nested-categories">
    <!-- Handle nested data structure -->
    <div v-for="(category, categoryIndex) in categories" :key="category.name" class="category">
      <h2>{{ category.icon }} {{ category.name }}</h2>

      <div class="subcategories">
        <div
          v-for="(subcategory, subIndex) in category.subcategories"
          :key="subcategory.name"
          class="subcategory"
        >
          <h3>{{ subcategory.name }}</h3>

          <ul class="items">
            <li v-for="item in subcategory.items" :key="item">
              {{ item }}
            </li>
          </ul>

          <div class="actions">
            <button @click="addItem(categoryIndex, subIndex)">Add Item</button>
          </div>
        </div>
      </div>

      <div class="category-actions">
        <button @click="addSubcategory(categoryIndex)">Add Subcategory</button>
      </div>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Complex data** - handle data structure yang kompleks
- ✅ **Hierarchical display** - tampilan data bertingkat
- ✅ **Flexible structure** - mudah menambah/hapus nested items

### 5. **Performance Optimization with Key Management**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 5: Optimasi performa dengan key management
const users = ref([
  { id: 'user_1', name: 'Alice', email: 'alice@example.com', status: 'active' },
  { id: 'user_2', name: 'Bob', email: 'bob@example.com', status: 'inactive' },
  { id: 'user_3', name: 'Charlie', email: 'charlie@example.com', status: 'active' }
])

const updateUserStatus = (userId, newStatus) => {
  const user = users.value.find(u => u.id === userId)
  if (user) user.status = newStatus
}

const addUser = () => {
  const newId = `user_${Date.now()}`
  users.value.push({
    id: newId,
    name: `User ${users.value.length + 1}`,
    email: `user${users.value.length + 1}@example.com`,
    status: 'active'
  })
}

const shuffleUsers = () => {
  // Vue akan efficiently re-render dengan key yang tepat
  users.value = [...users.value].sort(() => Math.random() - 0.5)
}
</script>

<template>
  <div class="user-management">
    <h3>User Management ({{ users.length }} users)</h3>

    <div class="user-controls">
      <button @click="addUser">Add User</button>
      <button @click="shuffleUsers">Shuffle Users</button>
    </div>

    <!-- Key management untuk performa optimal -->
    <div class="user-list">
      <div
        v-for="user in users"
        :key="user.id"  // ✅ Unik dan stable key
        class="user-card"
        :class="user.status"
      >
        <div class="user-info">
          <h4>{{ user.name }}</h4>
          <p>{{ user.email }}</p>
        </div>

        <div class="user-status">
          <span :class="['status-badge', user.status]">
            {{ user.status }}
          </span>
        </div>

        <div class="user-actions">
          <button
            @click="updateUserStatus(user.id, user.status === 'active' ? 'inactive' : 'active')"
          >
            Toggle Status
          </button>
        </div>
      </div>
    </div>

    <div class="performance-info">
      <p>✅ Using stable unique keys for optimal Vue performance</p>
      <p>✅ Vue can efficiently track, add, remove, and move items</p>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Efficient rendering** - Vue optimalkan DOM updates
- ✅ **Stable identity** - tracking item yang konsisten
- ✅ **Better performance** - minimalkan re-renders yang tidak perlu

## 🎯 Best Practices untuk Working With Collections & Lists

### 1. **Gunakan Key yang Unik dan Stable**
```vue
<template>
  <!-- ✅ GOOD: Unique dan stable keys -->
  <div v-for="user in users" :key="user.id">
    {{ user.name }}
  </div>

  <!-- ❌ AVOID: Index sebagai key (problematic saat list berubah) -->
  <div v-for="(user, index) in users" :key="index">
    {{ user.name }}
  </div>

  <!-- ✅ GOOD: Composite key untuk data unik -->
  <div v-for="item in items" :key="`${item.category}-${item.id}`">
    {{ item.name }}
  </div>
</template>
```

### 2. **Pisahkan Logic dari Template**
```vue
<script setup>
import { computed } from 'vue'

const todos = ref([
  { id: 1, text: 'Task 1', completed: false, priority: 'high' },
  { id: 2, text: 'Task 2', completed: true, priority: 'low' }
])

// ✅ GOOD: Computed properties untuk filtering dan sorting
const completedTodos = computed(() =>
  todos.value.filter(todo => todo.completed)
)

const highPriorityTodos = computed(() =>
  todos.value.filter(todo => todo.priority === 'high')
)

const sortedTodos = computed(() =>
  [...todos.value].sort((a, b) => b.priority.localeCompare(a.priority))
)
</script>

<template>
  <!-- Template tetap clean -->
  <div v-for="todo in sortedTodos" :key="todo.id">
    {{ todo.text }}
  </div>
</template>
```

### 3. **Hindari v-if dan v-for pada Element yang Sama**
```vue
<template>
  <!-- ❌ AVOID: v-if dan v-for pada element yang sama -->
  <div v-if="shouldShow" v-for="item in items" :key="item.id">
    {{ item.name }}
  </div>

  <!-- ✅ GOOD: Pisahkan dengan template wrapper -->
  <template v-if="shouldShow">
    <div v-for="item in items" :key="item.id">
      {{ item.name }}
    </div>
  </template>

  <!-- ✅ ATAU gunakan computed property -->
  <div v-for="item in visibleItems" :key="item.id">
    {{ item.name }}
  </div>
</template>
```

### 4. **Destructuring untuk Cleaner Code**
```vue
<script setup>
const users = ref([
  { id: 1, name: 'John', email: 'john@example.com', age: 25 },
  { id: 2, name: 'Jane', email: 'jane@example.com', age: 30 }
])
</script>

<template>
  <!-- ✅ GOOD: Object destructuring -->
  <div v-for="{ id, name, email, age } in users" :key="id">
    <h3>{{ name }}</h3>
    <p>{{ email }} ({{ age }} years old)</p>
  </div>

  <!-- ✅ GOOD: Array destructuring dengan index -->
  <div v-for="(user, index) in users" :key="user.id">
    <span>{{ index + 1 }}.</span>
    <span>{{ user.name }}</span>
  </div>
</template>
```

## 🎉 Kesimpulan

Working With Collections & Lists (v-for) adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Dynamic content generation** - generate UI dari data
- ✅ **Rich data display** - berbagai format tampilan data
- ✅ **Interactive management** - CRUD operations pada list
- ✅ **Nested data handling** - support struktur data kompleks
- ✅ **Performance optimization** - efficient rendering dengan proper key management

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang List Rendering
- Vue.js Template Syntax Guide
- Vue.js Performance Best Practices

---

**🔥 Tips Pro:**
- Selalu gunakan **unique dan stable keys** untuk optimal performance
- Manfaatkan **computed properties** untuk filtering dan sorting
- Pisahkan **business logic** dari template
- Gunakan **destructuring** untuk code yang lebih clean
- Pertimbangkan **virtual scrolling** untuk list yang sangat besar

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Style Guide