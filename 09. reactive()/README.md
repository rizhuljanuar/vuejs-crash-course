# 🔄 Tutorial Vue.js: reactive() untuk Pemula

## 📖 Pengenalan reactive()

**`reactive()`** adalah API Vue 3 yang digunakan untuk membuat objek JavaScript menjadi reaktif. Artinya, setiap perubahan pada objek tersebut akan secara otomatis memicu update pada UI yang menggunakannya.

## 🤔 Apa itu reactive()?

**`reactive()`** adalah function yang mengubah object biasa menjadi reactive object. Ini adalah bagian dari Vue Composition API yang powerful untuk mengelola state yang kompleks.

## ❓ Pertanyaan: Apa Fungsi reactive()?

reactive() memiliki beberapa fungsi utama yang sangat penting:

### **1. 🔧 Object Reactivity**
- **Fungsi:** Mengubah object JavaScript biasa menjadi reactive object
- **Manfaat:** Setiap perubahan pada object akan memicu update UI secara otomatis
- **Contoh:** Form data, user profiles, application state

### **2. 🔄 Nested Reactivity**
- **Fungsi:** Membuat semua nested properties menjadi reaktif
- **Manfaat:** Perubahan pada deep nested object akan tetap terdeteksi
- **Contoh:** `user.profile.settings.theme` changes trigger reactivity

### **3. 📦 Array Operations**
- **Fungsi:** Membuat array methods (push, pop, splice) menjadi reaktif
- **Manfaat:** Manipulasi array akan memicu re-render
- **Contoh:** Shopping cart, todo lists, data tables

### **4. 🎯 State Management**
- **Fungsi:** Mengelola complex application state dalam single object
- **Manfaat:** Lebih mudah mengelola state yang terkait
- **Contoh:** Game state, form data, user sessions

### **5. 🛡️ Type Consistency**
- **Fungsi:** Mendukung object dengan structure yang konsisten
- **Manfaat:** TypeScript integration dan error prevention
- **Contoh:** API response data, configuration objects

**Sintaks Dasar:**
```javascript
import { reactive } from 'vue'

const state = reactive({
  property1: value1,
  property2: value2
})
```

## 📁 Struktur Proyek

```
09. reactive()/
├── README.md                           # Dokumentasi ini
└── src/
    ├── components/
    │   └── MyReactiveComponent.vue     # Komponen dengan contoh reactive()
    └── App.vue                         # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `MyReactiveComponent.vue`

```vue
<script setup>
import { reactive } from 'vue'
let initialState = reactive({ val: { count: 0 }, user: ['HuXn', 'Jordan'] })

const changeUsers = () => {
  initialState.user[0] = 'michel'
  initialState.user[1] = 'philip'
}
</script>

<template>
  <h1>Current Count: {{ initialState.val.count }}</h1>
  <h2>Users: {{ initialState.user }}</h2>

  <button @click="initialState.val.count += 10">Add 10</button>
  <button @click="changeUsers">Change Users</button>
  <button @click="initialState.user.push('John Doe')">Add One More User</button>
</template>
```

**Penjelasan:**
- `reactive()` membuat object `initialState` menjadi reaktif
- Nested object `val.count` dan array `user` juga menjadi reaktif
- Perubahan pada nested properties akan memicu re-render
- Array methods seperti `push()` juga bekerja dengan reaktif

## 📖 Konsep Fundamental reactive()

### 1. **reactive() vs ref()**

#### **reactive() - Untuk Objects**
```javascript
import { reactive } from 'vue'

// ✅ BENAR - reactive untuk objects
const state = reactive({
  count: 0,
  name: 'Vue',
  user: {
    id: 1,
    email: 'user@example.com'
  }
})

// ✅ BENAR - reactive untuk arrays
const list = reactive([1, 2, 3, 4, 5])

// ❌ SALAH - reactive tidak bekerja dengan primitive types
const count = reactive(0)  // Tidak akan bekerja!
```

#### **ref() - Untuk Primitive Types**
```javascript
import { ref } from 'vue'

// ✅ BENAR - ref untuk primitive types
const count = ref(0)
const name = ref('Vue')
const isVisible = ref(true)

// ✅ BENAR - ref juga bisa untuk objects
const user = ref({
  id: 1,
  name: 'John'
})
```

### 2. **Nested Reactivity**

```javascript
const state = reactive({
  user: {
    profile: {
      name: 'John',
      settings: {
        theme: 'dark',
        notifications: true
      }
    }
  }
})

// Semua nested properties tetap reaktif
state.user.profile.name = 'Jane'  // ✅ Reaktif
state.user.profile.settings.theme = 'light'  // ✅ Reaktif
```

### 3. **Array Reactivity**

```javascript
const todos = reactive([
  { id: 1, text: 'Learn Vue.js', completed: false },
  { id: 2, text: 'Build amazing apps', completed: false }
])

// Array methods yang reaktif:
todos.push({ id: 3, text: 'Master Vue.js', completed: false })  // ✅
todos.pop()  // ✅
todos.splice(0, 1)  // ✅
todos.sort()  // ✅
todos.reverse()  // ✅

// Direct index assignment juga reaktif:
todos[0] = { id: 1, text: 'Updated task', completed: true }  // ✅
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan `reactive()`:

### Enhanced Demo: reactive() Showcase

```vue
<script setup>
import { reactive, computed, watch } from 'vue'

// 1. Basic reactive object
const counter = reactive({
  count: 0,
  step: 1,
  maxCount: 100,
  minCount: 0
})

// 2. User profile reactive object
const userProfile = reactive({
  personal: {
    firstName: 'John',
    lastName: 'Doe',
    age: 25,
    email: 'john.doe@example.com'
  },
  preferences: {
    theme: 'light',
    language: 'en',
    notifications: {
      email: true,
      push: false,
      sms: true
    }
  },
  stats: {
    loginCount: 42,
    lastLogin: new Date().toISOString(),
    isActive: true
  }
})

// 3. Shopping cart reactive array
const shoppingCart = reactive([
  {
    id: 1,
    name: 'Laptop',
    price: 15000000,
    quantity: 1,
    category: 'Electronics'
  },
  {
    id: 2,
    name: 'Mouse',
    price: 250000,
    quantity: 2,
    category: 'Accessories'
  }
])

// 4. Todo list reactive array
const todos = reactive([
  {
    id: 1,
    text: 'Learn Vue.js reactive()',
    completed: true,
    priority: 'high',
    createdAt: new Date().toISOString()
  },
  {
    id: 2,
    text: 'Build amazing applications',
    completed: false,
    priority: 'medium',
    createdAt: new Date().toISOString()
  }
])

// 5. Form data reactive object
const formData = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  preferences: {
    newsletter: false,
    updates: true,
    marketing: false
  },
  errors: {
    username: '',
    email: '',
    password: ''
  }
})

// 6. Game state reactive object
const gameState = reactive({
  player: {
    name: 'Player 1',
    score: 0,
    level: 1,
    lives: 3,
    position: { x: 0, y: 0 },
    inventory: ['sword', 'shield']
  },
  enemies: [
    { name: 'Dragon', health: 100, damage: 20 },
    { name: 'Goblin', health: 50, damage: 10 }
  ],
  settings: {
    difficulty: 'medium',
    soundVolume: 80,
    musicVolume: 60
  }
})

// Computed properties
const fullName = computed(() => {
  return `${userProfile.personal.firstName} ${userProfile.personal.lastName}`
})

const cartTotal = computed(() => {
  return shoppingCart.reduce((total, item) => {
    return total + (item.price * item.quantity)
  }, 0)
})

const completedTodos = computed(() => {
  return todos.filter(todo => todo.completed).length
})

const formIsValid = computed(() => {
  return formData.username.length >= 3 &&
         formData.email.includes('@') &&
         formData.password.length >= 6 &&
         formData.password === formData.confirmPassword
})

const playerStats = computed(() => {
  return {
    totalPower: gameState.player.score * gameState.player.level,
    isAlive: gameState.player.lives > 0,
    inventoryCount: gameState.player.inventory.length
  }
})

// Watchers
watch(
  () => counter.count,
  (newCount, oldCount) => {
    if (newCount >= counter.maxCount) {
      counter.count = counter.maxCount
    } else if (newCount <= counter.minCount) {
      counter.count = counter.minCount
    }
  }
)

watch(
  () => userProfile.personal.age,
  (newAge) => {
    if (newAge < 18) {
      userProfile.stats.isActive = false
    } else {
      userProfile.stats.isActive = true
    }
  }
)

// Event handlers
function incrementCounter() {
  counter.count += counter.step
}

function decrementCounter() {
  counter.count -= counter.step
}

function resetCounter() {
  counter.count = 0
}

function randomizeCounter() {
  counter.count = Math.floor(Math.random() * counter.maxCount)
}

function updateUserName() {
  const names = ['Alice', 'Bob', 'Charlie', 'Diana', 'Eve']
  const randomName = names[Math.floor(Math.random() * names.length)]
  userProfile.personal.firstName = randomName
}

function ageUser() {
  userProfile.personal.age++
}

function toggleTheme() {
  userProfile.preferences.theme = userProfile.preferences.theme === 'light' ? 'dark' : 'light'
}

function toggleNotification(type) {
  userProfile.preferences.notifications[type] = !userProfile.preferences.notifications[type]
}

function addToCart() {
  const newItem = {
    id: shoppingCart.length + 1,
    name: `Product ${shoppingCart.length + 1}`,
    price: Math.floor(Math.random() * 1000000) + 100000,
    quantity: 1,
    category: 'Misc'
  }
  shoppingCart.push(newItem)
}

function updateCartItemQuantity(itemId, newQuantity) {
  const item = shoppingCart.find(item => item.id === itemId)
  if (item) {
    item.quantity = Math.max(1, newQuantity)
  }
}

function removeFromCart(itemId) {
  const index = shoppingCart.findIndex(item => item.id === itemId)
  if (index > -1) {
    shoppingCart.splice(index, 1)
  }
}

function addTodo() {
  if (formData.username) {
    const newTodo = {
      id: todos.length + 1,
      text: formData.username,
      completed: false,
      priority: 'medium',
      createdAt: new Date().toISOString()
    }
    todos.push(newTodo)
    formData.username = ''
  }
}

function toggleTodo(todoId) {
  const todo = todos.find(todo => todo.id === todoId)
  if (todo) {
    todo.completed = !todo.completed
  }
}

function removeTodo(todoId) {
  const index = todos.findIndex(todo => todo.id === todoId)
  if (index > -1) {
    todos.splice(index, 1)
  }
}

function increasePlayerScore() {
  gameState.player.score += 10
}

function levelUp() {
  gameState.player.level++
}

function damagePlayer(amount) {
  gameState.player.lives = Math.max(0, gameState.player.lives - amount)
}

function movePlayer(direction) {
  switch (direction) {
    case 'up':
      gameState.player.position.y = Math.max(0, gameState.player.position.y - 1)
      break
    case 'down':
      gameState.player.position.y = Math.min(10, gameState.player.position.y + 1)
      break
    case 'left':
      gameState.player.position.x = Math.max(0, gameState.player.position.x - 1)
      break
    case 'right':
      gameState.player.position.x = Math.min(10, gameState.player.position.x + 1)
      break
  }
}

function addToInventory(item) {
  if (!gameState.player.inventory.includes(item)) {
    gameState.player.inventory.push(item)
  }
}

function removeFromInventory(item) {
  const index = gameState.player.inventory.indexOf(item)
  if (index > -1) {
    gameState.player.inventory.splice(index, 1)
  }
}

function damageEnemy(enemyIndex, damage) {
  if (gameState.enemies[enemyIndex]) {
    gameState.enemies[enemyIndex].health = Math.max(0, gameState.enemies[enemyIndex].health - damage)
  }
}
</script>

<template>
  <div class="reactive-demo">
    <h1>🔄 Vue.js reactive() Showcase</h1>

    <!-- 1. Basic Counter with reactive() -->
    <section class="demo-section">
      <h2>1️⃣ Basic Counter with reactive()</h2>
      <div class="demo-content">
        <div class="counter-display">
          <h3>Count: {{ counter.count }}</h3>
          <p>Step: {{ counter.step }}</p>
          <p>Range: {{ counter.minCount }} - {{ counter.maxCount }}</p>
        </div>

        <div class="counter-controls">
          <button @click="incrementCounter">Increment (+{{ counter.step }})</button>
          <button @click="decrementCounter">Decrement (-{{ counter.step }})</button>
          <button @click="resetCounter">Reset</button>
          <button @click="randomizeCounter">Random</button>
        </div>

        <div class="step-controls">
          <label>Step: </label>
          <button @click="counter.step = 1">1</button>
          <button @click="counter.step = 5">5</button>
          <button @click="counter.step = 10">10</button>
        </div>
      </div>
    </section>

    <!-- 2. Nested Object Reactivity -->
    <section class="demo-section">
      <h2>2️⃣ Nested Object Reactivity</h2>
      <div class="demo-content">
        <div class="user-profile">
          <h3>User Profile</h3>
          <p><strong>Name:</strong> {{ fullName }}</p>
          <p><strong>Age:</strong> {{ userProfile.personal.age }}</p>
          <p><strong>Email:</strong> {{ userProfile.personal.email }}</p>
          <p><strong>Theme:</strong> {{ userProfile.preferences.theme }}</p>
          <p><strong>Active:</strong> {{ userProfile.stats.isActive ? 'Yes ✅' : 'No ❌' }}</p>

          <div class="notification-settings">
            <h4>Notifications:</h4>
            <label>
              <input
                type="checkbox"
                :checked="userProfile.preferences.notifications.email"
                @change="toggleNotification('email')"
              />
              Email
            </label>
            <label>
              <input
                type="checkbox"
                :checked="userProfile.preferences.notifications.push"
                @change="toggleNotification('push')"
              />
              Push
            </label>
            <label>
              <input
                type="checkbox"
                :checked="userProfile.preferences.notifications.sms"
                @change="toggleNotification('sms')"
              />
              SMS
            </label>
          </div>

          <div class="profile-controls">
            <button @click="updateUserName">Random Name</button>
            <button @click="ageUser">Age +1</button>
            <button @click="toggleTheme">Toggle Theme</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. Array Reactivity - Shopping Cart -->
    <section class="demo-section">
      <h2>3️⃣ Array Reactivity - Shopping Cart</h2>
      <div class="demo-content">
        <div class="shopping-cart">
          <h3>Shopping Cart</h3>
          <div v-if="shoppingCart.length === 0" class="empty-cart">
            Your cart is empty
          </div>

          <div v-for="item in shoppingCart" :key="item.id" class="cart-item">
            <div class="item-info">
              <h4>{{ item.name }}</h4>
              <p>Category: {{ item.category }}</p>
              <p>Price: {{ new Intl.NumberFormat('id-ID', {
                style: 'currency',
                currency: 'IDR'
              }).format(item.price) }}</p>
            </div>

            <div class="item-controls">
              <div class="quantity-control">
                <button @click="updateCartItemQuantity(item.id, item.quantity - 1)">-</button>
                <span>{{ item.quantity }}</span>
                <button @click="updateCartItemQuantity(item.id, item.quantity + 1)">+</button>
              </div>

              <button @click="removeFromCart(item.id)" class="remove-btn">Remove</button>
            </div>
          </div>

          <div class="cart-total">
            <h3>Total: {{ new Intl.NumberFormat('id-ID', {
              style: 'currency',
              currency: 'IDR'
            }).format(cartTotal) }}</h3>
          </div>

          <button @click="addToCart" class="add-to-cart-btn">Add Random Item</button>
        </div>
      </div>
    </section>

    <!-- 4. Todo List with reactive() -->
    <section class="demo-section">
      <h2>4️⃣ Todo List with reactive()</h2>
      <div class="demo-content">
        <div class="todo-app">
          <h3>Todo List ({{ completedTodos }}/{{ todos.length }} completed)</h3>

          <div class="todo-input">
            <input
              v-model="formData.username"
              placeholder="Enter new todo..."
              @keyup.enter="addTodo"
            />
            <button @click="addTodo">Add Todo</button>
          </div>

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
              <span class="todo-priority">{{ todo.priority }}</span>
              <button @click="removeTodo(todo.id)" class="remove-todo">×</button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. Complex Game State -->
    <section class="demo-section">
      <h2>5️⃣ Complex Game State</h2>
      <div class="demo-content">
        <div class="game-state">
          <h3>Game State</h3>

          <div class="player-info">
            <h4>Player: {{ gameState.player.name }}</h4>
            <p>Score: {{ gameState.player.score }}</p>
            <p>Level: {{ gameState.player.level }}</p>
            <p>Lives: {{ gameState.player.lives }}</p>
            <p>Position: ({{ gameState.player.position.x }}, {{ gameState.player.position.y }})</p>

            <div class="inventory">
              <h5>Inventory:</h5>
              <span v-for="item in gameState.player.inventory" :key="item" class="inventory-item">
                {{ item }}
                <button @click="removeFromInventory(item)" class="remove-item">×</button>
              </span>
            </div>
          </div>

          <div class="player-stats">
            <h4>Stats:</h4>
            <p>Total Power: {{ playerStats.totalPower }}</p>
            <p>Status: {{ playerStats.isAlive ? 'Alive ✅' : 'Dead ❌' }}</p>
            <p>Items: {{ playerStats.inventoryCount }}</p>
          </div>

          <div class="game-controls">
            <div class="control-group">
              <h5>Actions:</h5>
              <button @click="increasePlayerScore">+10 Score</button>
              <button @click="levelUp">Level Up</button>
              <button @click="damagePlayer(1)">Take Damage</button>
            </div>

            <div class="control-group">
              <h5>Movement:</h5>
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
              <h5>Add Items:</h5>
              <button @click="addToInventory('potion')">Potion</button>
              <button @click="addToInventory('armor')">Armor</button>
              <button @click="addToInventory('magic-scroll')">Magic Scroll</button>
            </div>
          </div>

          <div class="enemies">
            <h4>Enemies:</h4>
            <div v-for="(enemy, index) in gameState.enemies" :key="index" class="enemy">
              <h5>{{ enemy.name }}</h5>
              <p>Health: {{ enemy.health }} / 100</p>
              <button @click="damageEnemy(index, 10)">Attack (-10)</button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Form Validation with reactive() -->
    <section class="demo-section">
      <h2>6️⃣ Form Validation with reactive()</h2>
      <div class="demo-content">
        <form @submit.prevent class="validation-form">
          <div class="form-group">
            <label>Username:</label>
            <input
              v-model="formData.username"
              placeholder="Enter username"
              @input="formData.errors.username = formData.username.length < 3 ? 'Username must be at least 3 characters' : ''"
            />
            <span v-if="formData.errors.username" class="error">{{ formData.errors.username }}</span>
          </div>

          <div class="form-group">
            <label>Email:</label>
            <input
              v-model="formData.email"
              type="email"
              placeholder="Enter email"
              @input="formData.errors.email = !formData.email.includes('@') ? 'Invalid email' : ''"
            />
            <span v-if="formData.errors.email" class="error">{{ formData.errors.email }}</span>
          </div>

          <div class="form-group">
            <label>Password:</label>
            <input
              v-model="formData.password"
              type="password"
              placeholder="Enter password"
              @input="validatePassword"
            />
            <span v-if="formData.errors.password" class="error">{{ formData.errors.password }}</span>
          </div>

          <div class="form-group">
            <label>Confirm Password:</label>
            <input
              v-model="formData.confirmPassword"
              type="password"
              placeholder="Confirm password"
              @input="validatePassword"
            />
          </div>

          <div class="form-group">
            <label>Preferences:</label>
            <div class="checkbox-group">
              <label>
                <input v-model="formData.preferences.newsletter" type="checkbox" />
                Newsletter
              </label>
              <label>
                <input v-model="formData.preferences.updates" type="checkbox" />
                Updates
              </label>
              <label>
                <input v-model="formData.preferences.marketing" type="checkbox" />
                Marketing
              </label>
            </div>
          </div>

          <div class="form-status">
            <p>Form is {{ formIsValid ? 'valid ✅' : 'invalid ❌' }}</p>
          </div>
        </form>
      </div>
    </section>
  </div>
</template>

<script>
function validatePassword() {
  if (this.formData.password.length < 6) {
    this.formData.errors.password = 'Password must be at least 6 characters'
  } else if (this.formData.password !== this.formData.confirmPassword) {
    this.formData.errors.password = 'Passwords do not match'
  } else {
    this.formData.errors.password = ''
  }
}
</script>

<style scoped>
.reactive-demo {
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

/* User Profile Styles */
.user-profile {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.notification-settings {
  margin: 15px 0;
}

.notification-settings label {
  display: block;
  margin: 5px 0;
}

.profile-controls {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

/* Shopping Cart Styles */
.shopping-cart {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.empty-cart {
  text-align: center;
  padding: 40px;
  color: #6c757d;
  font-style: italic;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  border: 1px solid #dee2e6;
  border-radius: 5px;
  margin-bottom: 10px;
  background-color: white;
}

.item-info h4 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.item-info p {
  margin: 2px 0;
  font-size: 14px;
  color: #6c757d;
}

.quantity-control {
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-control button {
  padding: 5px 10px;
  font-size: 12px;
}

.remove-btn {
  background-color: #dc3545;
  margin-left: 10px;
}

.remove-btn:hover {
  background-color: #c82333;
}

.cart-total {
  margin-top: 15px;
  padding-top: 15px;
  border-top: 2px solid #dee2e6;
  text-align: right;
}

.cart-total h3 {
  margin: 0;
  color: #42b883;
  font-size: 1.5em;
}

.add-to-cart-btn {
  width: 100%;
  margin-top: 15px;
  padding: 15px;
  font-size: 16px;
}

/* Todo List Styles */
.todo-app {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
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
  max-height: 300px;
  overflow-y: auto;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border: 1px solid #dee2e6;
  border-radius: 4px;
  margin-bottom: 5px;
  background-color: white;
}

.todo-item.completed .todo-text {
  text-decoration: line-through;
  color: #6c757d;
}

.todo-text {
  flex: 1;
}

.todo-priority {
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
  background-color: #ffc107;
  color: #212529;
}

.remove-todo {
  background: none;
  border: none;
  color: #dc3545;
  font-size: 18px;
  cursor: pointer;
  padding: 0;
  width: 20px;
  height: 20px;
}

/* Game State Styles */
.game-state {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.player-info,
.player-stats {
  margin-bottom: 20px;
}

.inventory {
  margin: 10px 0;
}

.inventory-item {
  display: inline-block;
  background-color: #007bff;
  color: white;
  padding: 4px 8px;
  border-radius: 12px;
  margin: 2px;
  font-size: 12px;
}

.remove-item {
  background: none;
  border: none;
  color: white;
  cursor: pointer;
  margin-left: 5px;
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

.movement-controls button {
  padding: 10px;
  font-size: 16px;
}

.enemies {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
}

.enemy {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 5px;
  border: 1px solid #ffeaa7;
}

.enemy h5 {
  margin: 0 0 10px 0;
  color: #856404;
}

/* Form Validation Styles */
.validation-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
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

.form-group input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.checkbox-group {
  display: flex;
  gap: 15px;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 5px;
  font-weight: normal;
}

.error {
  color: #dc3545;
  font-size: 12px;
  margin-top: 5px;
}

.form-status {
  text-align: center;
  padding: 10px;
  background-color: #d4edda;
  border: 1px solid #c3e6cb;
  border-radius: 4px;
  margin-top: 15px;
}

/* Responsive Design */
@media (max-width: 768px) {
  .reactive-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .cart-item {
    flex-direction: column;
    align-items: flex-start;
    gap: 10px;
  }

  .item-controls {
    width: 100%;
    justify-content: space-between;
  }

  .game-controls {
    grid-template-columns: 1fr;
  }

  .enemies {
    grid-template-columns: 1fr;
  }
}
</style>