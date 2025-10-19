# 📦 Tutorial Vue.js: ref() untuk Pemula

## 📖 Pengenalan ref()

**`ref()`** adalah API Vue 3 yang digunakan untuk membuat reactive reference untuk primitive types (string, number, boolean) dan juga objects. Ini adalah cara paling umum untuk membuat reactive data dalam Vue Composition API.

## 🤔 Apa itu ref()?

**`ref()`** adalah function yang membuat wrapper object reactive di sekitar nilai. Nilai aktual disimpan dalam property `.value` dari ref object.

**Sintaks Dasar:**
```javascript
import { ref } from 'vue'

const count = ref(0)        // Number ref
const name = ref('Vue')     // String ref
const isVisible = ref(true) // Boolean ref
```

## 📁 Struktur Proyek

```
10. ref()/
├── README.md                      # Dokumentasi ini
└── src/
    ├── components/
    │   └── MyRefComponent.vue     # Komponen dengan contoh ref()
    └── App.vue                    # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `MyRefComponent.vue`

```vue
<script setup>
import { reactive, ref } from 'vue'
let friends = reactive([ref('Jordan'), ref('Alex'), ref('HuXn')])
</script>

<template>
  <h1>1. {{ friends[0] }}</h1>
  <h1>2. {{ friends[1] }}</h1>
  <h1>3. {{ friends[2] }}</h1>

  <button @click="friends[0] = '🤝'">Hand Shake</button>
  <button @click="friends[1] = '🤝'">Hand Shake</button>
  <button @click="friends[2] = '🤝'">Hand Shake</button>
</template>
```

**Penjelasan:**
- `ref()` membuat reactive reference untuk string
- Array `friends` dibuat dengan `reactive()` berisi ref objects
- Template otomatis "unwraps" ref values (tidak perlu `.value` di template)
- Perubahan pada ref akan memicu re-render

## 📖 Konsep Fundamental ref()

### 1. **ref() vs reactive()**

#### **ref() - Untuk Semua Tipe Data**
```javascript
import { ref } from 'vue'

// ✅ BENAR - ref untuk primitive types
const count = ref(0)           // Number
const name = ref('Vue')        // String
const isVisible = ref(true)    // Boolean
const data = ref(null)         // Null

// ✅ BENAR - ref juga untuk objects
const user = ref({
  name: 'John',
  age: 25
})

// ✅ BENAR - ref untuk arrays
const items = ref([1, 2, 3, 4, 5])
```

#### **reactive() - Hanya untuk Objects**
```javascript
import { reactive } from 'vue'

// ✅ BENAR - reactive untuk objects
const state = reactive({
  count: 0,
  name: 'Vue'
})

// ✅ BENAR - reactive untuk arrays
const list = reactive([1, 2, 3, 4, 5])

// ❌ SALAH - reactive tidak bekerja dengan primitives
const count = reactive(0)     // Tidak akan bekerja!
```

### 2. **Cara Mengakses ref Values**

#### **Di JavaScript/Script**
```javascript
const count = ref(0)

// Mengakses nilai: gunakan .value
console.log(count.value)  // 0

// Mengubah nilai: gunakan .value
count.value = 5
console.log(count.value)  // 5
```

#### **Di Template**
```vue
<template>
  <!-- Di template, Vue otomatis unwraps .value -->
  <p>Count: {{ count }}</p>        <!-- Tidak perlu count.value -->

  <!-- Event handlers juga otomatis unwrapped -->
  <button @click="count++">Add</button>  <!-- Tidak perlu count.value++ -->
</template>
```

### 3. **ref dengan Objects dan Arrays**

```javascript
// ref dengan object
const user = ref({
  name: 'John',
  age: 25,
  hobbies: ['coding', 'reading']
})

// Mengakses nested properties
console.log(user.value.name)        // 'John'
console.log(user.value.hobbies[0])   // 'coding'

// Mengubah nested properties
user.value.age = 26
user.value.hobbies.push('gaming')

// ref dengan array
const todos = ref([
  { id: 1, text: 'Learn Vue.js', completed: false },
  { id: 2, text: 'Build apps', completed: false }
])

// Mengakses array items
console.log(todos.value[0].text)   // 'Learn Vue.js'

// Mengubah array
todos.value.push({ id: 3, text: 'Master Vue.js', completed: false })
todos.value[0].completed = true
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan `ref()`:

### Enhanced Demo: ref() Showcase

```vue
<script setup>
import { ref, computed, watch, onMounted } from 'vue'

// 1. Primitive Types with ref()
const count = ref(0)
const message = ref('Hello, Vue.js!')
const isVisible = ref(true)
const temperature = ref(25)
const score = ref(0)

// 2. String manipulation with ref()
const firstName = ref('John')
const lastName = ref('Doe')
const searchQuery = ref('')
const userInput = ref('')

// 3. Boolean logic with ref()
const isLoggedIn = ref(false)
const hasNotifications = ref(true)
const isDarkMode = ref(false)
const isLoading = ref(false)

// 4. Object with ref()
const userProfile = ref({
  id: 1,
  name: 'John Doe',
  email: 'john.doe@example.com',
  avatar: 'https://picsum.photos/seed/user1/100/100.jpg',
  bio: 'Vue.js enthusiast and web developer.',
  stats: {
    followers: 150,
    following: 200,
    posts: 42
  },
  preferences: {
    theme: 'light',
    language: 'en',
    notifications: {
      email: true,
      push: false,
      sms: true
    }
  }
})

// 5. Array with ref()
const shoppingList = ref([
  { id: 1, name: 'Milk', quantity: 2, price: 15000, completed: false },
  { id: 2, name: 'Bread', quantity: 1, price: 25000, completed: true },
  { id: 3, name: 'Eggs', quantity: 12, price: 30000, completed: false }
])

const todos = ref([
  { id: 1, text: 'Learn Vue.js ref()', completed: true, priority: 'high' },
  { id: 2, text: 'Build amazing applications', completed: false, priority: 'medium' },
  { id: 3, text: 'Master Vue.js ecosystem', completed: false, priority: 'low' }
])

const tags = ref(['Vue.js', 'JavaScript', 'Frontend', 'Web Development'])

// 6. Complex state with ref()
const gameSession = ref({
  currentPlayer: {
    id: 1,
    name: 'Player 1',
    score: 0,
    lives: 3,
    level: 1,
    position: { x: 0, y: 0 }
  },
  enemies: [
    { id: 1, name: 'Dragon', health: 100, attack: 20 },
    { id: 2, name: 'Goblin', health: 50, attack: 10 }
  ],
  items: [
    { id: 1, name: 'Sword', damage: 15, owned: false },
    { id: 2, name: 'Shield', defense: 10, owned: true }
  ],
  gameStats: {
    totalKills: 0,
    totalDamage: 0,
    playTime: 0
  }
})

// 7. Form data with ref()
const formData = ref({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: '',
  gender: '',
  terms: false,
  newsletter: true
})

const formErrors = ref({
  username: '',
  email: '',
  password: '',
  confirmPassword: ''
})

// 8. API state with ref()
const apiData = ref({
  users: [],
  posts: [],
  loading: false,
  error: null,
  lastUpdated: null
})

const searchResults = ref([])
const isSearching = ref(false)

// Computed properties
const fullName = computed(() => `${firstName.value} ${lastName.value}`)

const userAgeGroup = computed(() => {
  const age = parseInt(formData.value.age)
  if (age < 18) return 'Minor'
  if (age < 65) return 'Adult'
  return 'Senior'
})

const shoppingTotal = computed(() => {
  return shoppingList.value.reduce((total, item) => {
    return total + (item.price * item.quantity)
  }, 0)
})

const completedTodos = computed(() => {
  return todos.value.filter(todo => todo.completed).length
})

const highPriorityTodos = computed(() => {
  return todos.value.filter(todo => todo.priority === 'high' && !todo.completed)
})

const filteredTodos = computed(() => {
  if (!searchQuery.value) return todos.value
  return todos.value.filter(todo =>
    todo.text.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

const playerPower = computed(() => {
  const player = gameSession.value.currentPlayer
  const ownedItems = gameSession.value.items.filter(item => item.owned)
  const itemBonus = ownedItems.reduce((total, item) => {
    return total + (item.damage || item.defense || 0)
  }, 0)
  return player.score + player.level * 10 + itemBonus
})

const formIsValid = computed(() => {
  return formData.value.username.length >= 3 &&
         formData.value.email.includes('@') &&
         formData.value.password.length >= 6 &&
         formData.value.password === formData.value.confirmPassword &&
         formData.value.age >= 18 &&
         formData.value.terms
})

// Watchers
watch(
  () => count.value,
  (newCount) => {
    if (newCount >= 100) {
      message.value = 'Congratulations! You reached 100! 🎉'
    } else if (newCount >= 50) {
      message.value = 'Halfway there! Keep going! 💪'
    }
  }
)

watch(
  () => temperature.value,
  (newTemp) => {
    if (newTemp > 30) {
      message.value = `It's hot! ${newTemp}°C 🌡️`
    } else if (newTemp < 10) {
      message.value = `It's cold! ${newTemp}°C ❄️`
    }
  }
)

// Event handlers
function incrementCount() {
  count.value++
}

function decrementCount() {
  count.value--
}

function resetCount() {
  count.value = 0
  message.value = 'Counter reset!'
}

function randomizeCount() {
  count.value = Math.floor(Math.random() * 200)
}

function updateMessage() {
  const messages = [
    'Vue.js is awesome! 🚀',
    'Learning Composition API! 📚',
    'Building reactive apps! ⚡',
    'ref() makes everything reactive! 🔄'
  ]
  message.value = messages[Math.floor(Math.random() * messages.length)]
}

function toggleVisibility() {
  isVisible.value = !isVisible.value
}

function adjustTemperature(delta) {
  temperature.value = Math.max(-20, Math.min(50, temperature.value + delta))
}

function changeFirstName() {
  const names = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve']
  firstName.value = names[Math.floor(Math.random() * names.length)]
}

function changeLastName() {
  const names = ['Smith', 'Johnson', 'Williams', 'Brown', 'Jones']
  lastName.value = names[Math.floor(Math.random() * names.length)]
}

function login() {
  isLoggedIn.value = true
  hasNotifications.value = true
}

function logout() {
  isLoggedIn.value = false
  hasNotifications.value = false
}

function toggleTheme() {
  isDarkMode.value = !isDarkMode.value
  userProfile.value.preferences.theme = isDarkMode.value ? 'dark' : 'light'
}

function simulateLoading() {
  isLoading.value = true
  setTimeout(() => {
    isLoading.value = false
  }, 2000)
}

function updateProfile() {
  userProfile.value.name = fullName.value
  userProfile.value.stats.followers += Math.floor(Math.random() * 10)
}

function addToShoppingList() {
  const newItem = {
    id: shoppingList.value.length + 1,
    name: `Item ${shoppingList.value.length + 1}`,
    quantity: 1,
    price: Math.floor(Math.random() * 50000) + 5000,
    completed: false
  }
  shoppingList.value.push(newItem)
}

function toggleShoppingItem(itemId) {
  const item = shoppingList.value.find(item => item.id === itemId)
  if (item) {
    item.completed = !item.completed
  }
}

function removeShoppingItem(itemId) {
  const index = shoppingList.value.findIndex(item => item.id === itemId)
  if (index > -1) {
    shoppingList.value.splice(index, 1)
  }
}

function addTodo() {
  if (userInput.value.trim()) {
    const newTodo = {
      id: todos.value.length + 1,
      text: userInput.value,
      completed: false,
      priority: 'medium'
    }
    todos.value.push(newTodo)
    userInput.value = ''
  }
}

function toggleTodo(todoId) {
  const todo = todos.value.find(todo => todo.id === todoId)
  if (todo) {
    todo.completed = !todo.completed
  }
}

function removeTodo(todoId) {
  const index = todos.value.findIndex(todo => todo.id === todoId)
  if (index > -1) {
    todos.value.splice(index, 1)
  }
}

function addTag() {
  if (userInput.value.trim() && !tags.value.includes(userInput.value.trim())) {
    tags.value.push(userInput.value.trim())
    userInput.value = ''
  }
}

function removeTag(tagToRemove) {
  const index = tags.value.indexOf(tagToRemove)
  if (index > -1) {
    tags.value.splice(index, 1)
  }
}

function levelUpPlayer() {
  gameSession.value.currentPlayer.level++
  gameSession.value.currentPlayer.score += 100
}

function damagePlayer(amount) {
  gameSession.value.currentPlayer.lives = Math.max(0, gameSession.value.currentPlayer.lives - amount)
}

function movePlayer(direction) {
  const player = gameSession.value.currentPlayer.position
  switch (direction) {
    case 'up':
      player.y = Math.max(0, player.y - 1)
      break
    case 'down':
      player.y = Math.min(10, player.y + 1)
      break
    case 'left':
      player.x = Math.max(0, player.x - 1)
      break
    case 'right':
      player.x = Math.min(10, player.x + 1)
      break
  }
}

function acquireItem(itemId) {
  const item = gameSession.value.items.find(item => item.id === itemId)
  if (item && !item.owned) {
    item.owned = true
    gameSession.value.currentPlayer.score += 50
  }
}

function simulateApiCall() {
  apiData.value.loading = true
  apiData.value.error = null

  setTimeout(() => {
    apiData.value.users = [
      { id: 1, name: 'John Doe', email: 'john@example.com' },
      { id: 2, name: 'Jane Smith', email: 'jane@example.com' }
    ]
    apiData.value.posts = [
      { id: 1, title: 'Vue.js Tutorial', userId: 1 },
      { id: 2, title: 'Composition API Guide', userId: 2 }
    ]
    apiData.value.loading = false
    apiData.value.lastUpdated = new Date().toISOString()
  }, 1500)
}

function searchUsers() {
  if (searchQuery.value.trim()) {
    isSearching.value = true
    setTimeout(() => {
      searchResults.value = [
        { id: 1, name: `${searchQuery.value} Result 1` },
        { id: 2, name: `${searchQuery.value} Result 2` }
      ]
      isSearching.value = false
    }, 500)
  }
}

function validateForm() {
  formErrors.value.username = formData.value.username.length < 3
    ? 'Username must be at least 3 characters'
    : ''

  formErrors.value.email = !formData.value.email.includes('@')
    ? 'Please enter a valid email'
    : ''

  formErrors.value.password = formData.value.password.length < 6
    ? 'Password must be at least 6 characters'
    : ''

  formErrors.value.confirmPassword = formData.value.password !== formData.value.confirmPassword
    ? 'Passwords do not match'
    : ''
}

// Initialize on mount
onMounted(() => {
  console.log('Component mounted with ref() examples!')
})
</script>

<template>
  <div class="ref-demo" :class="{ 'dark-theme': isDarkMode }">
    <h1>📦 Vue.js ref() Showcase</h1>

    <!-- 1. Primitive Types with ref() -->
    <section class="demo-section">
      <h2>1️⃣ Primitive Types with ref()</h2>
      <div class="demo-content">
        <div class="counter-demo">
          <h3>Counter: {{ count }}</h3>
          <p class="message">{{ message }}</p>
          <div class="counter-controls">
            <button @click="incrementCount">+</button>
            <button @click="decrementCount">-</button>
            <button @click="resetCount">Reset</button>
            <button @click="randomizeCount">Random</button>
          </div>
        </div>

        <div class="visibility-demo">
          <p v-if="isVisible" class="visible-content">✨ This content is visible!</p>
          <p v-else class="hidden-content">🚫 Content is hidden</p>
          <button @click="toggleVisibility">
            {{ isVisible ? 'Hide' : 'Show' }} Content
          </button>
        </div>

        <div class="temperature-demo">
          <h3>Temperature: {{ temperature }}°C</h3>
          <div class="temp-controls">
            <button @click="adjustTemperature(-5)">-5°C</button>
            <button @click="adjustTemperature(-1)">-1°C</button>
            <button @click="adjustTemperature(1)">+1°C</button>
            <button @click="adjustTemperature(5)">+5°C</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 2. String Manipulation with ref() -->
    <section class="demo-section">
      <h2>2️⃣ String Manipulation with ref()</h2>
      <div class="demo-content">
        <div class="name-demo">
          <h3>Full Name: {{ fullName }}</h3>
          <p>First: {{ firstName }} | Last: {{ lastName }}</p>
          <div class="name-controls">
            <button @click="changeFirstName">Random First Name</button>
            <button @click="changeLastName">Random Last Name</button>
          </div>
        </div>

        <div class="message-demo">
          <p>Current Message: {{ message }}</p>
          <button @click="updateMessage">Change Message</button>
        </div>

        <div class="search-demo">
          <input
            v-model="searchQuery"
            placeholder="Search todos..."
            @input="searchUsers"
          />
          <p>Searching: {{ searchQuery || 'Type to search...' }}</p>
          <div v-if="isSearching" class="searching">Searching...</div>
          <div v-if="searchResults.length > 0" class="search-results">
            <p v-for="result in searchResults" :key="result.id">
              {{ result.name }}
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. Boolean Logic with ref() -->
    <section class="demo-section">
      <h2>3️⃣ Boolean Logic with ref()</h2>
      <div class="demo-content">
        <div class="auth-demo">
          <div v-if="isLoggedIn" class="logged-in">
            <h3>Welcome back! 👋</h3>
            <p>You have {{ hasNotifications ? 'new' : 'no' }} notifications</p>
            <button @click="logout">Logout</button>
          </div>
          <div v-else class="logged-out">
            <h3>Please login</h3>
            <button @click="login">Login</button>
          </div>
        </div>

        <div class="theme-demo">
          <p>Current theme: {{ isDarkMode ? 'Dark 🌙' : 'Light ☀️' }}</p>
          <button @click="toggleTheme">Toggle Theme</button>
        </div>

        <div class="loading-demo">
          <button @click="simulateLoading">
            {{ isLoading ? 'Loading...' : 'Simulate Loading' }}
          </button>
          <div v-if="isLoading" class="loading-spinner">⏳ Processing...</div>
        </div>
      </div>
    </section>

    <!-- 4. Object with ref() -->
    <section class="demo-section">
      <h2>4️⃣ Object with ref()</h2>
      <div class="demo-content">
        <div class="profile-demo">
          <div class="profile-header">
            <img :src="userProfile.avatar" :alt="userProfile.name" class="avatar" />
            <div class="profile-info">
              <h3>{{ userProfile.name }}</h3>
              <p>{{ userProfile.email }}</p>
              <p>{{ userProfile.bio }}</p>
            </div>
          </div>

          <div class="profile-stats">
            <div class="stat">
              <span class="stat-number">{{ userProfile.stats.followers }}</span>
              <span class="stat-label">Followers</span>
            </div>
            <div class="stat">
              <span class="stat-number">{{ userProfile.stats.following }}</span>
              <span class="stat-label">Following</span>
            </div>
            <div class="stat">
              <span class="stat-number">{{ userProfile.stats.posts }}</span>
              <span class="stat-label">Posts</span>
            </div>
          </div>

          <div class="profile-controls">
            <button @click="updateProfile">Update Profile</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. Array with ref() -->
    <section class="demo-section">
      <h2>5️⃣ Array with ref()</h2>
      <div class="demo-content">
        <div class="shopping-demo">
          <h3>Shopping List</h3>
          <div class="shopping-list">
            <div
              v-for="item in shoppingList"
              :key="item.id"
              class="shopping-item"
              :class="{ completed: item.completed }"
            >
              <input
                type="checkbox"
                :checked="item.completed"
                @change="toggleShoppingItem(item.id)"
              />
              <span class="item-name">{{ item.name }}</span>
              <span class="item-quantity">×{{ item.quantity }}</span>
              <span class="item-price">
                {{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(item.price) }}
              </span>
              <button @click="removeShoppingItem(item.id)" class="remove-btn">×</button>
            </div>
          </div>

          <div class="shopping-total">
            <h3>Total: {{ new Intl.NumberFormat('id-ID', {
              style: 'currency',
              currency: 'IDR'
            }).format(shoppingTotal) }}</h3>
          </div>

          <button @click="addToShoppingList" class="add-item-btn">Add Item</button>
        </div>

        <div class="todos-demo">
          <h3>Todo List ({{ completedTodos }}/{{ todos.length }} completed)</h3>

          <div class="todo-input">
            <input
              v-model="userInput"
              placeholder="Enter new todo..."
              @keyup.enter="addTodo"
            />
            <button @click="addTodo">Add</button>
          </div>

          <div class="todo-list">
            <div
              v-for="todo in filteredTodos"
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
              <span class="todo-priority" :class="todo.priority">{{ todo.priority }}</span>
              <button @click="removeTodo(todo.id)" class="remove-todo">×</button>
            </div>
          </div>

          <div v-if="highPriorityTodos.length > 0" class="high-priority-alert">
            ⚠️ {{ highPriorityTodos.length }} high priority todos pending
          </div>
        </div>

        <div class="tags-demo">
          <h3>Tags</h3>
          <div class="tag-input">
            <input
              v-model="userInput"
              placeholder="Add tag..."
              @keyup.enter="addTag"
            />
            <button @click="addTag">Add Tag</button>
          </div>
          <div class="tags-list">
            <span
              v-for="tag in tags"
              :key="tag"
              class="tag"
            >
              {{ tag }}
              <button @click="removeTag(tag)" class="remove-tag">×</button>
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Complex State with ref() -->
    <section class="demo-section">
      <h2>6️⃣ Complex State with ref()</h2>
      <div class="demo-content">
        <div class="game-demo">
          <h3>Game Session</h3>

          <div class="player-info">
            <h4>{{ gameSession.currentPlayer.name }}</h4>
            <p>Score: {{ gameSession.currentPlayer.score }}</p>
            <p>Level: {{ gameSession.currentPlayer.level }}</p>
            <p>Lives: {{ gameSession.currentPlayer.lives }}</p>
            <p>Position: ({{ gameSession.currentPlayer.position.x }}, {{ gameSession.currentPlayer.position.y }})</p>
            <p>Total Power: {{ playerPower }}</p>
          </div>

          <div class="game-controls">
            <div class="control-group">
              <h5>Actions</h5>
              <button @click="levelUpPlayer">Level Up</button>
              <button @click="damagePlayer(1)">Take Damage</button>
            </div>

            <div class="control-group">
              <h5>Movement</h5>
              <div class="movement-controls">
                <button @click="movePlayer('up')">↑</button>
                <div>
                  <button @click="movePlayer('left')">←</button>
                  <button @click="movePlayer('right')">→</button>
                </div>
                <button @click="movePlayer('down')">↓</button>
              </div>
            </div>

            <div class="control-group">
              <h5>Items</h5>
              <div v-for="item in gameSession.items" :key="item.id" class="item">
                <span :class="{ owned: item.owned }">{{ item.name }}</span>
                <span v-if="item.damage">+{{ item.damage }} ATK</span>
                <span v-if="item.defense">+{{ item.defense }} DEF</span>
                <button
                  v-if="!item.owned"
                  @click="acquireItem(item.id)"
                  :disabled="gameSession.currentPlayer.score < 50"
                >
                  Acquire (50 pts)
                </button>
              </div>
            </div>
          </div>

          <div class="enemies">
            <h4>Enemies</h4>
            <div v-for="enemy in gameSession.enemies" :key="enemy.id" class="enemy">
              <h5>{{ enemy.name }}</h5>
              <p>Health: {{ enemy.health }}/100</p>
              <p>Attack: {{ enemy.attack }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 7. Form Data with ref() -->
    <section class="demo-section">
      <h2>7️⃣ Form Data with ref()</h2>
      <div class="demo-content">
        <form @submit.prevent class="registration-form">
          <div class="form-group">
            <label>Username:</label>
            <input
              v-model="formData.username"
              placeholder="Enter username"
              @input="validateForm"
            />
            <span v-if="formErrors.username" class="error">{{ formErrors.username }}</span>
          </div>

          <div class="form-group">
            <label>Email:</label>
            <input
              v-model="formData.email"
              type="email"
              placeholder="Enter email"
              @input="validateForm"
            />
            <span v-if="formErrors.email" class="error">{{ formErrors.email }}</span>
          </div>

          <div class="form-group">
            <label>Password:</label>
            <input
              v-model="formData.password"
              type="password"
              placeholder="Enter password"
              @input="validateForm"
            />
            <span v-if="formErrors.password" class="error">{{ formErrors.password }}</span>
          </div>

          <div class="form-group">
            <label>Confirm Password:</label>
            <input
              v-model="formData.confirmPassword"
              type="password"
              placeholder="Confirm password"
              @input="validateForm"
            />
            <span v-if="formErrors.confirmPassword" class="error">{{ formErrors.confirmPassword }}</span>
          </div>

          <div class="form-group">
            <label>Age:</label>
            <input
              v-model.number="formData.age"
              type="number"
              min="18"
              max="100"
              placeholder="Enter age"
            />
            <span>Age group: {{ userAgeGroup }}</span>
          </div>

          <div class="form-group">
            <label>Gender:</label>
            <select v-model="formData.gender">
              <option value="">Select gender</option>
              <option value="male">Male</option>
              <option value="female">Female</option>
              <option value="other">Other</option>
            </select>
          </div>

          <div class="form-group">
            <label>
              <input v-model="formData.terms" type="checkbox" />
              I agree to the terms and conditions
            </label>
          </div>

          <div class="form-group">
            <label>
              <input v-model="formData.newsletter" type="checkbox" />
              Subscribe to newsletter
            </label>
          </div>

          <div class="form-status">
            <p>Form is {{ formIsValid ? 'valid ✅' : 'invalid ❌' }}</p>
            <div v-if="formIsValid" class="success-message">
              Ready to submit! 🎉
            </div>
          </div>
        </form>
      </div>
    </section>

    <!-- 8. API State with ref() -->
    <section class="demo-section">
      <h2>8️⃣ API State with ref()</h2>
      <div class="demo-content">
        <div class="api-demo">
          <button @click="simulateApiCall" :disabled="apiData.loading">
            {{ apiData.loading ? 'Loading...' : 'Simulate API Call' }}
          </button>

          <div v-if="apiData.loading" class="api-loading">
            <p>Fetching data from API...</p>
          </div>

          <div v-if="apiData.error" class="api-error">
            <p>Error: {{ apiData.error }}</p>
          </div>

          <div v-if="apiData.users.length > 0" class="api-data">
            <h4>Users ({{ apiData.users.length }}):</h4>
            <div v-for="user in apiData.users" :key="user.id" class="user-item">
              <strong>{{ user.name }}</strong> - {{ user.email }}
            </div>

            <h4>Posts ({{ apiData.posts.length }}):</h4>
            <div v-for="post in apiData.posts" :key="post.id" class="post-item">
              <strong>{{ post.title }}</strong> (User ID: {{ post.userId }})
            </div>

            <p v-if="apiData.lastUpdated">
              Last updated: {{ new Date(apiData.lastUpdated).toLocaleString() }}
            </p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style scoped>
.ref-demo {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  transition: background-color 0.3s ease, color 0.3s ease;
}

.dark-theme {
  background-color: #1a1a1a;
  color: #ecf0f1;
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

.dark-theme .demo-section {
  background: #2c2c2c;
  box-shadow: 0 2px 10px rgba(255, 255, 255, 0.1);
}

.demo-section h2 {
  color: #2c3e50;
  margin-bottom: 15px;
  font-size: 1.3em;
}

.dark-theme .demo-section h2 {
  color: #ecf0f1;
}

.demo-content {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* Counter Demo */
.counter-demo {
  text-align: center;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 10px;
}

.counter-demo h3 {
  font-size: 2em;
  margin: 0;
}

.message {
  font-style: italic;
  margin: 10px 0;
  min-height: 20px;
}

.counter-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  flex-wrap: wrap;
}

/* Visibility Demo */
.visibility-demo {
  text-align: center;
  padding: 20px;
}

.visible-content {
  color: #2ecc71;
  font-weight: bold;
}

.hidden-content {
  color: #e74c3c;
  font-style: italic;
}

/* Temperature Demo */
.temperature-demo {
  text-align: center;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.temp-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 10px;
  flex-wrap: wrap;
}

/* Name Demo */
.name-demo {
  text-align: center;
  padding: 20px;
  background-color: #e3f2fd;
  border-radius: 8px;
}

.name-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 10px;
}

/* Search Demo */
.search-demo {
  padding: 20px;
}

.search-demo input {
  width: 100%;
  max-width: 400px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  margin-bottom: 10px;
}

.searching {
  color: #f39c12;
  font-style: italic;
}

.search-results {
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 4px;
  margin-top: 10px;
}

/* Auth Demo */
.auth-demo {
  text-align: center;
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 8px;
}

.logged-in {
  color: #2ecc71;
}

.logged-out {
  color: #e74c3c;
}

/* Theme Demo */
.theme-demo {
  text-align: center;
  padding: 20px;
}

/* Loading Demo */
.loading-demo {
  text-align: center;
  padding: 20px;
}

.loading-spinner {
  color: #f39c12;
  font-style: italic;
  margin-top: 10px;
}

/* Profile Demo */
.profile-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.profile-header {
  display: flex;
  align-items: center;
  gap: 20px;
  margin-bottom: 20px;
}

.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
}

.profile-info h3 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.profile-info p {
  margin: 5px 0;
  color: #6c757d;
}

.profile-stats {
  display: flex;
  gap: 30px;
  margin-bottom: 20px;
  justify-content: center;
}

.stat {
  text-align: center;
}

.stat-number {
  display: block;
  font-size: 1.5em;
  font-weight: bold;
  color: #42b883;
}

.stat-label {
  font-size: 0.9em;
  color: #6c757d;
}

.profile-controls {
  text-align: center;
}

/* Shopping Demo */
.shopping-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.shopping-list {
  margin-bottom: 20px;
}

.shopping-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background-color: white;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  margin-bottom: 5px;
}

.shopping-item.completed {
  opacity: 0.6;
  text-decoration: line-through;
}

.item-name {
  flex: 1;
}

.item-quantity {
  color: #6c757d;
}

.item-price {
  font-weight: bold;
  color: #42b883;
}

.remove-btn {
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  cursor: pointer;
  font-size: 16px;
}

.shopping-total {
  text-align: right;
  margin-bottom: 15px;
  font-size: 1.2em;
  color: #42b883;
}

.add-item-btn {
  width: 100%;
  padding: 10px;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* Todo Demo */
.todos-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.todo-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.todo-list {
  margin-bottom: 15px;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  background-color: white;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  margin-bottom: 5px;
}

.todo-item.completed {
  opacity: 0.6;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
}

.todo-text {
  flex: 1;
}

.todo-priority {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
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

.remove-todo {
  background: none;
  border: none;
  color: #dc3545;
  font-size: 18px;
  cursor: pointer;
  padding: 0;
  width: 20px;
}

.high-priority-alert {
  background-color: #fff3cd;
  color: #856404;
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ffeaa7;
  text-align: center;
}

/* Tags Demo */
.tags-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.tag-input {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.tag-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.tags-list {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.tag {
  background-color: #007bff;
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  display: flex;
  align-items: center;
  gap: 5px;
}

.remove-tag {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  font-size: 14px;
  padding: 0;
}

/* Game Demo */
.game-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.player-info {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
}

.game-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
  margin-bottom: 20px;
}

.control-group h5 {
  margin: 0 0 10px 0;
  color: #495057;
}

.movement-controls {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 5px;
  width: fit-content;
}

.item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 5px;
  border-radius: 4px;
}

.item.owned {
  background-color: #d4edda;
  color: #155724;
}

.enemies {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
}

.enemy {
  background-color: white;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 10px;
  border: 1px solid #ffeaa7;
}

/* Form Styles */
.registration-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.dark-theme .form-group input,
.dark-theme .form-group select {
  background-color: #3a3a3a;
  border-color: #555;
  color: #ecf0f1;
}

.error {
  color: #dc3545;
  font-size: 12px;
  margin-top: 5px;
}

.form-status {
  text-align: center;
  padding: 15px;
  background-color: #d4edda;
  border: 1px solid #c3e6cb;
  border-radius: 4px;
  margin-top: 15px;
}

.success-message {
  color: #155724;
  font-weight: bold;
}

/* API Demo */
.api-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.api-loading {
  color: #f39c12;
  font-style: italic;
  text-align: center;
  margin-top: 15px;
}

.api-error {
  color: #dc3545;
  background-color: #f8d7da;
  padding: 10px;
  border-radius: 4px;
  margin-top: 15px;
}

.api-data {
  margin-top: 15px;
}

.user-item,
.post-item {
  background-color: white;
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 5px;
  border: 1px solid #dee2e6;
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
  .ref-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .profile-header {
    flex-direction: column;
    text-align: center;
  }

  .profile-stats {
    gap: 15px;
  }

  .game-controls {
    grid-template-columns: 1fr;
  }

  .counter-controls,
  .name-controls,
  .temp-controls {
    flex-direction: column;
  }
}
</style>

## ❓ Pertanyaan: Apa Fungsi ref()?

`ref()` memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat penting dan serbaguna:

### 1. **Reaktivitas untuk Semua Jenis Data** (Primitive & Non-Primitive)
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 1: Membuat primitif menjadi reaktif
const count = ref(0)           // Number primitif
const name = ref('Vue.js')     // String primitif
const isVisible = ref(true)    // Boolean primitif
const price = ref(99.99)       // Number primitif
</script>

<template>
  <!-- Semua data primitif ini akan otomatis update di template -->
  <div>Count: {{ count }}</div>
  <div>Name: {{ name }}</div>
  <div>Visible: {{ isVisible }}</div>
  <div>Price: ${{ price }}</div>
</template>
```

**Keuntungan:**
- ✅ **Satu API untuk semua data** - tidak perlu pikirkan jenis data
- ✅ **Konsistensi** - cara yang sama untuk semua data reaktif
- ✅ **Simple** - mudah dipahami dan digunakan

### 2. **Reaktivitas untuk Kompleks Data**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 2: Membuat objek dan array menjadi reaktif
const user = ref({
  id: 1,
  name: 'John Doe',
  email: 'john@example.com'
})

const items = ref([
  { id: 1, name: 'Item 1' },
  { id: 2, name: 'Item 2' }
])

const updateUser = () => {
  // Mengubah seluruh objek
  user.value = {
    id: 2,
    name: 'Jane Smith',
    email: 'jane@example.com'
  }
}
</script>

<template>
  <div>{{ user.name }} - {{ user.email }}</div>
  <div>Total items: {{ items.length }}</div>
</template>
```

**Keuntungan:**
- ✅ **Replacement support** - bisa ganti seluruh objek/array
- ✅ **Reaktivitas penuh** - semua property terdeteksi perubahan
- ✅ **Memory efficient** - hanya satu reference untuk track perubahan

### 3. **Template Integration Otomatis**
```vue
<script setup>
import { ref } from 'vue'

const message = ref('Hello World')
const count = ref(0)

const increment = () => {
  count.value++  // Perlu .value di JavaScript
}
</script>

<template>
  <!-- Fungsi 3: Otomatis unwrap di template -->
  <h1>{{ message }}</h1>        <!-- Tidak perlu .value -->
  <p>Count: {{ count }}</p>     <!-- Tidak perlu .value -->

  <button @click="increment">
    Increment {{ count }}       <!-- Tidak perlu .value -->
  </button>
</template>
```

**Keuntungan:**
- ✅ **Clean template** - tidak perlu `.value` di HTML
- ✅ **Auto-unwrap** - Vue otomatis unwrap ref di template
- ✅ **Developer friendly** - template lebih mudah dibaca

### 4. **DOM Element References**
```vue
<script setup>
import { ref, onMounted } from 'vue'

// Fungsi 4: Reference ke DOM elements
const inputRef = ref(null)
const divRef = ref(null)

onMounted(() => {
  // Akses langsung ke DOM element
  inputRef.value.focus()
  console.log(divRef.value.textContent)
})

const highlightDiv = () => {
  divRef.value.style.backgroundColor = 'yellow'
}
</script>

<template>
  <input ref="inputRef" placeholder="Auto focus on mount">
  <div ref="divRef" @click="highlightDiv">Click to highlight</div>
</template>
```

**Keuntungan:**
- ✅ **Direct DOM access** - akses langsung ke elements
- ✅ **No jQuery needed** - vanilla Vue.js DOM manipulation
- ✅ **Safe** - hanya accessible setelah component mounted

### 5. **Component Communication**
```vue
<!-- ParentComponent.vue -->
<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const childRef = ref(null)

const callChildMethod = () => {
  // Fungsi 5: Akses methods dan data child component
  childRef.value.doSomething()
  console.log(childRef.value.childData)
}
</script>

<template>
  <ChildComponent ref="childRef" />
  <button @click="callChildMethod">Call Child Method</button>
</template>
```

```vue
<!-- ChildComponent.vue -->
<script setup>
import { ref, defineExpose } from 'vue'

const childData = ref('Hello from child!')

const doSomething = () => {
  console.log('Child method called!')
}

defineExpose({
  childData,
  doSomething
})
</script>
```

**Keuntungan:**
- ✅ **Parent-child communication** - akses methods child component
- ✅ **Component control** - parent bisa kontrol child behavior
- ✅ **Encapsulation** - child bisa expose yang dibutuhkan saja

## 🎯 Best Practices untuk Menggunakan ref()

### 1. **Gunakan ref() untuk Data Primitif**
```vue
<script setup>
import { ref } from 'vue'

// ✅ GOOD: ref() untuk primitif
const count = ref(0)
const name = ref('Vue.js')
const isVisible = ref(true)

// ❌ AVOID: reactive() untuk primitif (tidak akan bekerja)
// const count = reactive(0)  // ERROR!
</script>
```

### 2. **Pilih ref() vs reactive() dengan Bijak**
```vue
<script setup>
import { ref, reactive } from 'vue'

// ✅ Gunakan ref() untuk replacement
const user = ref({ name: 'John' })
const updateUser = () => {
  user.value = { name: 'Jane' }  // ✅ Replacement OK
}

// ✅ Gunakan reactive() untuk nested mutations
const profile = reactive({ name: 'John' })
const updateProfile = () => {
  profile.name = 'Jane'  // ✅ Direct mutation
}
</script>
```

### 3. **TypeScript Integration**
```vue
<script setup lang="ts">
import { ref } from 'vue'

// ✅ Explicit typing
const count = ref<number>(0)
const name = ref<string>('Vue.js')
const items = ref<Array<{ id: number; name: string }>>([])

// ✅ Interface types
interface User {
  id: number
  name: string
}

const user = ref<User>({
  id: 1,
  name: 'John Doe'
})
</script>
```

## 🎉 Kesimpulan

`ref()` adalah API yang **sangat penting** dan **serbaguna** dalam Vue.js yang memberikan:

- ✅ **Reaktivitas untuk semua jenis data** - termasuk primitif
- ✅ **Akses yang mudah** - melalui property `.value`
- ✅ **Template otomatis** - tidak perlu `.value` di template
- ✅ **Cara yang jelas** - untuk bekerja dengan data reaktif
- ✅ **Kompatibilitas TypeScript** - yang baik
- ✅ **Performa yang optimal** - dalam berbagai kasus

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang `ref()`
- API Reference Vue.js
- Vue.js Composition API Guide

---

**🔥 Tips Pro:**
- Gunakan `ref()` untuk **semua jenis data** saat mulai belajar Vue.js
- Jangan lupa `.value` di JavaScript/TypeScript tapi **jangan gunakan** di template
- Pilih `ref()` ketika tidak yakin antara `ref()` dan `reactive()`
- Manfaatkan TypeScript untuk type safety yang lebih baik

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation