# 🧮 Tutorial Vue.js: Computed Properties untuk Pemula

## 📖 Pengenalan Computed Properties

**Computed Properties** adalah fitur Vue.js yang memungkinkan Anda membuat derived data (data yang dihitung dari data lain) secara otomatis dan efisien. Computed properties akan cache hasilnya dan hanya recompute ketika dependencies berubah.

## 🤔 Apa itu Computed Properties?

**Computed Properties** adalah properties yang dihitung secara dinamis berdasarkan nilai-nilai reactive lainnya. Mereka seperti functions, tetapi dengan caching otomatis untuk performa yang lebih baik.

**Sintaks Dasar:**
```javascript
import { computed } from 'vue'

const computedProperty = computed(() => {
  // Logic untuk menghitung nilai
  return derivedValue
})
```

## 📁 Struktur Proyek

```
11. Computed Properties/
├── README.md                             # Dokumentasi ini
└── src/
    ├── components/
    │   └── ComputedProperties.vue         # Komponen dengan contoh computed properties
    └── App.vue                            # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `ComputedProperties.vue`

```vue
<script setup>
import { ref, computed } from 'vue'
const counter = ref(0)

const squaredCounter = computed(() => counter.value ** 2)

const incrementCounter = () => {
  counter.value++
}
</script>

<template>
  <p>Counter: {{ counter }}</p>
  <p>Squared Counter: {{ squaredCounter }}</p>
  <button @click="incrementCounter">Increment Counter</button>
</template>
```

**Penjelasan:**
- `computed()` membuat computed property yang otomatis update
- `squaredCounter` akan menghitung kuadrat dari `counter`
- Hasil akan di-cache dan hanya recompute ketika `counter` berubah
- Di template, computed properties digunakan seperti data biasa

## 📖 Konsep Fundamental Computed Properties

### 1. **Computed vs Methods**

#### **Computed Properties (Disarankan)**
```javascript
const fullName = computed(() => {
  return firstName.value + ' ' + lastName.value
})
```
- ✅ **Cached** - Hasil disimpan, hanya recompute jika dependencies berubah
- ✅ **Reactive** - Otomatis update di template
- ✅ **Clean syntax** - Tidak perlu dipanggil seperti function

#### **Methods (Tidak cached)**
```javascript
function getFullName() {
  return firstName.value + ' ' + lastName.value
}
```
- ❌ **No caching** - Dipanggil ulang setiap render
- ❌ **Less performant** untuk operasi yang kompleks
- ✅ **Useful** untuk actions/event handlers

### 2. **Writable Computed Properties**

```javascript
const firstName = ref('John')
const lastName = ref('Doe')

// Read-only computed
const fullName = computed(() => firstName.value + ' ' + lastName.value)

// Writable computed
const fullNameWritable = computed({
  get() {
    return firstName.value + ' ' + lastName.value
  },
  set(newValue) {
    const names = newValue.split(' ')
    firstName.value = names[0]
    lastName.value = names[names.length - 1]
  }
})
```

### 3. **Computed dengan Reactive Object**

```javascript
const user = ref({
  firstName: 'John',
  lastName: 'Doe',
  age: 25
})

const userSummary = computed(() => {
  return `${user.value.firstName} (${user.value.age} years old)`
})
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan computed properties:

### Enhanced Demo: Computed Properties Showcase

```vue
<script setup>
import { ref, computed, watch } from 'vue'

// 1. Basic Computed Properties
const counter = ref(0)
const baseNumber = ref(5)

const squaredCounter = computed(() => counter.value ** 2)
const cubedCounter = computed(() => counter.value ** 3)
const multipliedResult = computed(() => counter.value * baseNumber.value)

// 2. String Computed Properties
const firstName = ref('John')
const lastName = ref('Doe')
const title = ref('Mr.')
const company = ref('Tech Corp')

const fullName = computed(() => `${title.value} ${firstName.value} ${lastName.value}`)
const email = computed(() => `${firstName.value.toLowerCase()}.${lastName.value.toLowerCase()}@${company.value.toLowerCase().replace(' ', '')}.com`)
const initials = computed(() => `${firstName.value[0]}${lastName.value[0]}`)
const formalName = computed(() => `${title.value} ${lastName.value}`)

// 3. Array Computed Properties
const numbers = ref([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
const filterThreshold = ref(5)

const evenNumbers = computed(() => numbers.value.filter(n => n % 2 === 0))
const oddNumbers = computed(() => numbers.value.filter(n => n % 2 !== 0))
const numbersAboveThreshold = computed(() => numbers.value.filter(n => n > filterThreshold.value))
const sumOfNumbers = computed(() => numbers.value.reduce((sum, n) => sum + n, 0))
const averageOfNumbers = computed(() => sumOfNumbers.value / numbers.value.length)
const maxNumber = computed(() => Math.max(...numbers.value))
const minNumber = computed(() => Math.min(...numbers.value))

// 4. User Profile Computed Properties
const userProfile = ref({
  firstName: 'Jane',
  lastName: 'Smith',
  birthYear: 1990,
  currentYear: 2024,
  height: 165, // cm
  weight: 60,  // kg
  interests: ['coding', 'reading', 'traveling'],
  address: {
    street: '123 Main St',
    city: 'New York',
    country: 'USA',
    zipCode: '10001'
  }
})

const age = computed(() => userProfile.value.currentYear - userProfile.value.birthYear)
const bmi = computed(() => {
  const heightInMeters = userProfile.value.height / 100
  return (userProfile.value.weight / (heightInMeters ** 2)).toFixed(1)
})
const bmiCategory = computed(() => {
  const bmiValue = parseFloat(bmi.value)
  if (bmiValue < 18.5) return 'Underweight'
  if (bmiValue < 25) return 'Normal'
  if (bmiValue < 30) return 'Overweight'
  return 'Obese'
})
const fullAddress = computed(() => {
  const addr = userProfile.value.address
  return `${addr.street}, ${addr.city}, ${addr.country} ${addr.zipCode}`
})
const interestsList = computed(() => userProfile.value.interests.join(', '))
const isMillennial = computed(() => {
  const year = userProfile.value.birthYear
  return year >= 1981 && year <= 1996
})

// 5. Shopping Cart Computed Properties
const shoppingCart = ref([
  {
    id: 1,
    name: 'Laptop',
    price: 15000000,
    quantity: 1,
    category: 'Electronics',
    discount: 10,
    tax: 10
  },
  {
    id: 2,
    name: 'Mouse',
    price: 250000,
    quantity: 2,
    category: 'Accessories',
    discount: 0,
    tax: 10
  },
  {
    id: 3,
    name: 'Keyboard',
    price: 750000,
    quantity: 1,
    category: 'Accessories',
    discount: 15,
    tax: 10
  }
])

const cartItems = computed(() => shoppingCart.value.filter(item => item.quantity > 0))
const cartItemCount = computed(() => cartItems.value.reduce((total, item) => total + item.quantity, 0))
const subtotal = computed(() => {
  return cartItems.value.reduce((total, item) => total + (item.price * item.quantity), 0)
})
const totalDiscount = computed(() => {
  return cartItems.value.reduce((total, item) => {
    const itemTotal = item.price * item.quantity
    return total + (itemTotal * item.discount / 100)
  }, 0)
})
const totalTax = computed(() => {
  const taxableAmount = subtotal.value - totalDiscount.value
  return cartItems.value.reduce((total, item) => {
    const itemTotal = item.price * item.quantity
    const discountedTotal = itemTotal - (itemTotal * item.discount / 100)
    return total + (discountedTotal * item.tax / 100)
  }, 0)
})
const totalAmount = computed(() => subtotal.value - totalDiscount.value + totalTax.value)
const averageItemPrice = computed(() => {
  return cartItemCount.value > 0 ? subtotal.value / cartItemCount.value : 0
})

// 6. Todo List Computed Properties
const todos = ref([
  { id: 1, text: 'Learn Vue.js', completed: true, priority: 'high', createdAt: '2024-01-01' },
  { id: 2, text: 'Build amazing apps', completed: false, priority: 'medium', createdAt: '2024-01-02' },
  { id: 3, text: 'Master JavaScript', completed: true, priority: 'high', createdAt: '2024-01-03' },
  { id: 4, text: 'Learn TypeScript', completed: false, priority: 'low', createdAt: '2024-01-04' },
  { id: 5, text: 'Deploy to production', completed: false, priority: 'medium', createdAt: '2024-01-05' }
])

const completedTodos = computed(() => todos.value.filter(todo => todo.completed))
const incompleteTodos = computed(() => todos.value.filter(todo => !todo.completed))
const completionRate = computed(() => {
  return todos.value.length > 0 ? Math.round((completedTodos.value.length / todos.value.length) * 100) : 0
})
const highPriorityTodos = computed(() => todos.value.filter(todo => todo.priority === 'high'))
const incompleteHighPriorityTodos = computed(() => {
  return todos.value.filter(todo => todo.priority === 'high' && !todo.completed)
})
const todoStats = computed(() => ({
  total: todos.value.length,
  completed: completedTodos.value.length,
  incomplete: incompleteTodos.value.length,
  highPriority: highPriorityTodos.value.length,
  completionRate: completionRate.value
}))

// 7. Form Validation Computed Properties
const formData = ref({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: '',
  phone: ''
})

const usernameError = computed(() => {
  if (formData.value.username.length === 0) return 'Username is required'
  if (formData.value.username.length < 3) return 'Username must be at least 3 characters'
  if (formData.value.username.length > 20) return 'Username must be less than 20 characters'
  return ''
})

const emailError = computed(() => {
  const email = formData.value.email
  if (email.length === 0) return 'Email is required'
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(email)) return 'Please enter a valid email address'
  return ''
})

const passwordError = computed(() => {
  const password = formData.value.password
  if (password.length === 0) return 'Password is required'
  if (password.length < 6) return 'Password must be at least 6 characters'
  if (!/[A-Z]/.test(password)) return 'Password must contain at least one uppercase letter'
  if (!/[0-9]/.test(password)) return 'Password must contain at least one number'
  return ''
})

const confirmPasswordError = computed(() => {
  if (formData.value.confirmPassword.length === 0) return 'Please confirm your password'
  if (formData.value.password !== formData.value.confirmPassword) return 'Passwords do not match'
  return ''
})

const ageError = computed(() => {
  const age = parseInt(formData.value.age)
  if (formData.value.age.length === 0) return 'Age is required'
  if (isNaN(age) || age < 18) return 'You must be at least 18 years old'
  if (age > 120) return 'Please enter a valid age'
  return ''
})

const phoneError = computed(() => {
  const phone = formData.value.phone
  if (phone.length === 0) return 'Phone number is required'
  const phoneRegex = /^[\d\s\-\+\(\)]+$/
  if (!phoneRegex.test(phone)) return 'Please enter a valid phone number'
  if (phone.replace(/\D/g, '').length < 10) return 'Phone number must be at least 10 digits'
  return ''
})

const formErrors = computed(() => ({
  username: usernameError.value,
  email: emailError.value,
  password: passwordError.value,
  confirmPassword: confirmPasswordError.value,
  age: ageError.value,
  phone: phoneError.value
}))

const formIsValid = computed(() => {
  return Object.values(formErrors.value).every(error => error === '')
})

// 8. Search and Filter Computed Properties
const searchQuery = ref('')
const selectedCategory = ref('all')
const selectedPriority = ref('all')
const sortBy = ref('date')

const filteredAndSortedTodos = computed(() => {
  let filtered = todos.value

  // Filter by search query
  if (searchQuery.value) {
    filtered = filtered.filter(todo =>
      todo.text.toLowerCase().includes(searchQuery.value.toLowerCase())
    )
  }

  // Filter by priority
  if (selectedPriority.value !== 'all') {
    filtered = filtered.filter(todo => todo.priority === selectedPriority.value)
  }

  // Filter by completion status
  if (selectedCategory.value === 'completed') {
    filtered = filtered.filter(todo => todo.completed)
  } else if (selectedCategory.value === 'incomplete') {
    filtered = filtered.filter(todo => !todo.completed)
  }

  // Sort
  return filtered.sort((a, b) => {
    if (sortBy.value === 'date') {
      return new Date(b.createdAt) - new Date(a.createdAt)
    } else if (sortBy.value === 'priority') {
      const priorityOrder = { high: 3, medium: 2, low: 1 }
      return priorityOrder[b.priority] - priorityOrder[a.priority]
    }
    return 0
  })
})

// 9. Currency and Formatting Computed Properties
const exchangeRate = ref(15000) // 1 USD = 15000 IDR
const prices = ref([
  { id: 1, name: 'Product A', usdPrice: 10 },
  { id: 2, name: 'Product B', usdPrice: 25 },
  { id: 3, name: 'Product C', usdPrice: 50 }
])

const formattedPrices = computed(() => {
  return prices.value.map(item => ({
    ...item,
    idrPrice: item.usdPrice * exchangeRate.value,
    formattedUsd: new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(item.usdPrice),
    formattedIdr: new Intl.NumberFormat('id-ID', {
      style: 'currency',
      currency: 'IDR'
    }).format(item.usdPrice * exchangeRate.value)
  }))
})

const totalInUSD = computed(() => {
  return prices.value.reduce((total, item) => total + item.usdPrice, 0)
})

const totalInIDR = computed(() => {
  return totalInUSD.value * exchangeRate.value
})

// 10. Performance Optimization Computed Properties
const largeDataSet = ref(Array.from({ length: 10000 }, (_, i) => ({
  id: i + 1,
  value: Math.random() * 100,
  category: ['A', 'B', 'C'][Math.floor(Math.random() * 3)]
})))

const expensiveComputation = computed(() => {
  // Simulate expensive computation
  console.log('Expensive computation running...')
  return largeDataSet.value
    .filter(item => item.value > 50)
    .reduce((sum, item) => sum + item.value, 0)
    .toFixed(2)
})

// Writable computed properties
const writableName = computed({
  get() {
    return `${firstName.value} ${lastName.value}`
  },
  set(newValue) {
    const names = newValue.trim().split(' ')
    firstName.value = names[0] || ''
    lastName.value = names.slice(1).join(' ') || ''
  }
})

// Event handlers
function incrementCounter() {
  counter.value++
}

function decrementCounter() {
  counter.value--
}

function addNumber() {
  const newNumber = Math.floor(Math.random() * 20) + 1
  numbers.value.push(newNumber)
}

function removeNumber(index) {
  numbers.value.splice(index, 1)
}

function updateBaseNumber(newBase) {
  baseNumber.value = newBase
}

function changeUserAge() {
  userProfile.value.birthYear--
}

function addToCart(item) {
  const existingItem = shoppingCart.value.find(cartItem => cartItem.id === item.id)
  if (existingItem) {
    existingItem.quantity++
  } else {
    shoppingCart.value.push({ ...item, quantity: 1 })
  }
}

function updateCartItemQuantity(itemId, newQuantity) {
  const item = shoppingCart.value.find(item => item.id === itemId)
  if (item) {
    item.quantity = Math.max(0, newQuantity)
  }
}

function addTodo(text) {
  const newTodo = {
    id: todos.value.length + 1,
    text,
    completed: false,
    priority: 'medium',
    createdAt: new Date().toISOString().split('T')[0]
  }
  todos.value.push(newTodo)
}

function toggleTodo(todoId) {
  const todo = todos.value.find(t => t.id === todoId)
  if (todo) {
    todo.completed = !todo.completed
  }
}

function updateExchangeRate(newRate) {
  exchangeRate.value = newRate
}

function addRandomData() {
  largeDataSet.value.push({
    id: largeDataSet.value.length + 1,
    value: Math.random() * 100,
    category: ['A', 'B', 'C'][Math.floor(Math.random() * 3)]
  })
}
</script>

<template>
  <div class="computed-demo">
    <h1>🧮 Vue.js Computed Properties Showcase</h1>

    <!-- 1. Basic Computed Properties -->
    <section class="demo-section">
      <h2>1️⃣ Basic Computed Properties</h2>
      <div class="demo-content">
        <div class="counter-demo">
          <h3>Counter Operations</h3>
          <p>Counter: <strong>{{ counter }}</strong></p>
          <p>Squared: <strong>{{ squaredCounter }}</strong></p>
          <p>Cubed: <strong>{{ cubedCounter }}</strong></p>
          <p>Multiplied by {{ baseNumber }}: <strong>{{ multipliedResult }}</strong></p>

          <div class="controls">
            <button @click="incrementCounter">Increment</button>
            <button @click="decrementCounter">Decrement</button>
          </div>

          <div class="base-controls">
            <label>Base Number:</label>
            <button @click="updateBaseNumber(2)">2</button>
            <button @click="updateBaseNumber(5)">5</button>
            <button @click="updateBaseNumber(10)">10</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 2. String Computed Properties -->
    <section class="demo-section">
      <h2>2️⃣ String Computed Properties</h2>
      <div class="demo-content">
        <div class="name-demo">
          <div class="input-group">
            <label>Title:</label>
            <input v-model="title" placeholder="Mr./Ms./Dr." />
          </div>
          <div class="input-group">
            <label>First Name:</label>
            <input v-model="firstName" placeholder="First name" />
          </div>
          <div class="input-group">
            <label>Last Name:</label>
            <input v-model="lastName" placeholder="Last name" />
          </div>
          <div class="input-group">
            <label>Company:</label>
            <input v-model="company" placeholder="Company name" />
          </div>

          <div class="results">
            <p><strong>Full Name:</strong> {{ fullName }}</p>
            <p><strong>Email:</strong> {{ email }}</p>
            <p><strong>Initials:</strong> {{ initials }}</p>
            <p><strong>Formal Name:</strong> {{ formalName }}</p>
          </div>

          <div class="writable-demo">
            <label>Writable Full Name:</label>
            <input v-model="writableName" placeholder="Enter full name" />
            <p>Changing this will update first and last name automatically!</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 3. Array Computed Properties -->
    <section class="demo-section">
      <h2>3️⃣ Array Computed Properties</h2>
      <div class="demo-content">
        <div class="array-demo">
          <h3>Number Analysis</h3>
          <div class="controls">
            <button @click="addNumber">Add Random Number</button>
          </div>

          <div class="numbers-display">
            <p><strong>All Numbers:</strong> {{ numbers.join(', ') }}</p>
          </div>

          <div class="filter-controls">
            <label>Filter Threshold:</label>
            <input
              v-model.number="filterThreshold"
              type="range"
              min="1"
              max="20"
            />
            <span>{{ filterThreshold }}</span>
          </div>

          <div class="array-results">
            <div class="result-group">
              <h4>Filtered Results:</h4>
              <p><strong>Even Numbers:</strong> {{ evenNumbers.join(', ') }}</p>
              <p><strong>Odd Numbers:</strong> {{ oddNumbers.join(', ') }}</p>
              <p><strong>Above {{ filterThreshold }}:</strong> {{ numbersAboveThreshold.join(', ') }}</p>
            </div>

            <div class="result-group">
              <h4>Statistics:</h4>
              <p><strong>Sum:</strong> {{ sumOfNumbers }}</p>
              <p><strong>Average:</strong> {{ averageOfNumbers.toFixed(2) }}</p>
              <p><strong>Maximum:</strong> {{ maxNumber }}</p>
              <p><strong>Minimum:</strong> {{ minNumber }}</p>
            </div>
          </div>

          <div class="number-list">
            <h4>Manage Numbers:</h4>
            <div
              v-for="(number, index) in numbers"
              :key="index"
              class="number-item"
            >
              <span>{{ number }}</span>
              <button @click="removeNumber(index)">Remove</button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 4. User Profile Computed Properties -->
    <section class="demo-section">
      <h2>4️⃣ User Profile Computed Properties</h2>
      <div class="demo-content">
        <div class="profile-demo">
          <h3>User Profile Analysis</h3>

          <div class="profile-inputs">
            <div class="input-group">
              <label>First Name:</label>
              <input v-model="userProfile.firstName" />
            </div>
            <div class="input-group">
              <label>Last Name:</label>
              <input v-model="userProfile.lastName" />
            </div>
            <div class="input-group">
              <label>Birth Year:</label>
              <input v-model.number="userProfile.birthYear" type="number" />
            </div>
            <div class="input-group">
              <label>Current Year:</label>
              <input v-model.number="userProfile.currentYear" type="number" />
            </div>
            <div class="input-group">
              <label>Height (cm):</label>
              <input v-model.number="userProfile.height" type="number" />
            </div>
            <div class="input-group">
              <label>Weight (kg):</label>
              <input v-model.number="userProfile.weight" type="number" />
            </div>
          </div>

          <div class="profile-results">
            <h4>Computed Information:</h4>
            <p><strong>Age:</strong> {{ age }} years old</p>
            <p><strong>BMI:</strong> {{ bmi }} ({{ bmiCategory }})</p>
            <p><strong>Full Address:</strong> {{ fullAddress }}</p>
            <p><strong>Interests:</strong> {{ interestsList }}</p>
            <p><strong>Is Millennial:</strong> {{ isMillennial ? 'Yes ✅' : 'No ❌' }}</p>
          </div>

          <div class="profile-actions">
            <button @click="changeUserAge">Age -1 Year</button>
          </div>
        </div>
      </div>
    </section>

    <!-- 5. Shopping Cart Computed Properties -->
    <section class="demo-section">
      <h2>5️⃣ Shopping Cart Computed Properties</h2>
      <div class="demo-content">
        <div class="cart-demo">
          <h3>Shopping Cart Analysis</h3>

          <div class="available-products">
            <h4>Available Products:</h4>
            <div
              v-for="product in [
                { id: 4, name: 'Monitor', price: 3000000 },
                { id: 5, name: 'Headphones', price: 1500000 },
                { id: 6, name: 'Webcam', price: 800000 }
              ]"
              :key="product.id"
              class="product-item"
            >
              <span>{{ product.name }} - {{ new Intl.NumberFormat('id-ID', {
                style: 'currency',
                currency: 'IDR'
              }).format(product.price) }}</span>
              <button @click="addToCart(product)">Add to Cart</button>
            </div>
          </div>

          <div class="cart-items">
            <h4>Cart Items:</h4>
            <div v-if="cartItems.length === 0" class="empty-cart">
              Your cart is empty
            </div>
            <div
              v-for="item in cartItems"
              :key="item.id"
              class="cart-item"
            >
              <div class="item-info">
                <h5>{{ item.name }}</h5>
                <p>Price: {{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(item.price) }}</p>
                <p>Discount: {{ item.discount }}%</p>
                <p>Tax: {{ item.tax }}%</p>
              </div>
              <div class="item-controls">
                <label>Quantity:</label>
                <input
                  :value="item.quantity"
                  @input="updateCartItemQuantity(item.id, parseInt($event.target.value))"
                  type="number"
                  min="0"
                />
              </div>
            </div>
          </div>

          <div class="cart-summary">
            <h4>Cart Summary:</h4>
            <div class="summary-grid">
              <div class="summary-item">
                <span>Items Count:</span>
                <strong>{{ cartItemCount }}</strong>
              </div>
              <div class="summary-item">
                <span>Subtotal:</span>
                <strong>{{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(subtotal) }}</strong>
              </div>
              <div class="summary-item">
                <span>Discount:</span>
                <strong>-{{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(totalDiscount) }}</strong>
              </div>
              <div class="summary-item">
                <span>Tax:</span>
                <strong>{{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(totalTax) }}</strong>
              </div>
              <div class="summary-item total">
                <span>Total Amount:</span>
                <strong>{{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(totalAmount) }}</strong>
              </div>
              <div class="summary-item">
                <span>Average Item Price:</span>
                <strong>{{ new Intl.NumberFormat('id-ID', {
                  style: 'currency',
                  currency: 'IDR'
                }).format(averageItemPrice) }}</strong>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 6. Todo List Computed Properties -->
    <section class="demo-section">
      <h2>6️⃣ Todo List Computed Properties</h2>
      <div class="demo-content">
        <div class="todo-demo">
          <h3>Todo List Analysis</h3>

          <div class="todo-input">
            <input
              v-model="newTodoText"
              placeholder="Enter new todo..."
              @keyup.enter="addTodo(newTodoText)"
            />
            <button @click="addTodo(newTodoText)">Add Todo</button>
          </div>

          <div class="todo-stats">
            <h4>Statistics:</h4>
            <div class="stats-grid">
              <div class="stat-item">
                <span>Total:</span>
                <strong>{{ todoStats.total }}</strong>
              </div>
              <div class="stat-item">
                <span>Completed:</span>
                <strong>{{ todoStats.completed }}</strong>
              </div>
              <div class="stat-item">
                <span>Incomplete:</span>
                <strong>{{ todoStats.incomplete }}</strong>
              </div>
              <div class="stat-item">
                <span>High Priority:</span>
                <strong>{{ todoStats.highPriority }}</strong>
              </div>
              <div class="stat-item">
                <span>Completion Rate:</span>
                <strong>{{ todoStats.completionRate }}%</strong>
              </div>
              <div class="stat-item">
                <span>Incomplete High Priority:</span>
                <strong>{{ incompleteHighPriorityTodos.length }}</strong>
              </div>
            </div>
          </div>

          <div class="progress-bar">
            <div class="progress-fill" :style="{ width: completionRate + '%' }">
              {{ completionRate }}%
            </div>
          </div>

          <div class="todo-list">
            <h4>All Todos:</h4>
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
              <span class="todo-priority" :class="todo.priority">{{ todo.priority }}</span>
              <span class="todo-date">{{ todo.createdAt }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 7. Form Validation Computed Properties -->
    <section class="demo-section">
      <h2>7️⃣ Form Validation Computed Properties</h2>
      <div class="demo-content">
        <div class="form-demo">
          <h3>Real-time Form Validation</h3>

          <form @submit.prevent class="validation-form">
            <div class="form-group">
              <label>Username:</label>
              <input v-model="formData.username" placeholder="Enter username" />
              <span v-if="usernameError" class="error">{{ usernameError }}</span>
            </div>

            <div class="form-group">
              <label>Email:</label>
              <input v-model="formData.email" type="email" placeholder="Enter email" />
              <span v-if="emailError" class="error">{{ emailError }}</span>
            </div>

            <div class="form-group">
              <label>Password:</label>
              <input v-model="formData.password" type="password" placeholder="Enter password" />
              <span v-if="passwordError" class="error">{{ passwordError }}</span>
            </div>

            <div class="form-group">
              <label>Confirm Password:</label>
              <input v-model="formData.confirmPassword" type="password" placeholder="Confirm password" />
              <span v-if="confirmPasswordError" class="error">{{ confirmPasswordError }}</span>
            </div>

            <div class="form-group">
              <label>Age:</label>
              <input v-model="formData.age" type="number" placeholder="Enter age" />
              <span v-if="ageError" class="error">{{ ageError }}</span>
            </div>

            <div class="form-group">
              <label>Phone:</label>
              <input v-model="formData.phone" placeholder="Enter phone number" />
              <span v-if="phoneError" class="error">{{ phoneError }}</span>
            </div>

            <div class="form-status">
              <h4>Form Status:</h4>
              <p :class="{ valid: formIsValid, invalid: !formIsValid }">
                Form is {{ formIsValid ? 'VALID ✅' : 'INVALID ❌' }}
              </p>
            </div>
          </form>
        </div>
      </div>
    </section>

    <!-- 8. Search and Filter Computed Properties -->
    <section class="demo-section">
      <h2>8️⃣ Search and Filter Computed Properties</h2>
      <div class="demo-content">
        <div class="search-demo">
          <h3>Advanced Todo Filtering</h3>

          <div class="search-controls">
            <div class="control-group">
              <label>Search:</label>
              <input v-model="searchQuery" placeholder="Search todos..." />
            </div>

            <div class="control-group">
              <label>Status:</label>
              <select v-model="selectedCategory">
                <option value="all">All</option>
                <option value="completed">Completed</option>
                <option value="incomplete">Incomplete</option>
              </select>
            </div>

            <div class="control-group">
              <label>Priority:</label>
              <select v-model="selectedPriority">
                <option value="all">All</option>
                <option value="high">High</option>
                <option value="medium">Medium</option>
                <option value="low">Low</option>
              </select>
            </div>

            <div class="control-group">
              <label>Sort By:</label>
              <select v-model="sortBy">
                <option value="date">Date</option>
                <option value="priority">Priority</option>
              </select>
            </div>
          </div>

          <div class="filtered-results">
            <h4>Filtered Results ({{ filteredAndSortedTodos.length }}):</h4>
            <div
              v-for="todo in filteredAndSortedTodos"
              :key="todo.id"
              class="filtered-todo"
              :class="{ completed: todo.completed }"
            >
              <span class="todo-text">{{ todo.text }}</span>
              <span class="todo-priority" :class="todo.priority">{{ todo.priority }}</span>
              <span class="todo-date">{{ todo.createdAt }}</span>
            </div>

            <div v-if="filteredAndSortedTodos.length === 0" class="no-results">
              No todos found matching your criteria
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 9. Currency and Formatting Computed Properties -->
    <section class="demo-section">
      <h2>9️⃣ Currency and Formatting Computed Properties</h2>
      <div class="demo-content">
        <div class="currency-demo">
          <h3>Currency Conversion and Formatting</h3>

          <div class="exchange-controls">
            <label>Exchange Rate (1 USD to IDR):</label>
            <input
              v-model.number="exchangeRate"
              type="number"
              step="1000"
            />
          </div>

          <div class="formatted-prices">
            <h4>Formatted Prices:</h4>
            <div
              v-for="item in formattedPrices"
              :key="item.id"
              class="price-item"
            >
              <h5>{{ item.name }}</h5>
              <p>USD: {{ item.formattedUsd }}</p>
              <p>IDR: {{ item.formattedIdr }}</p>
            </div>
          </div>

          <div class="totals">
            <h4>Totals:</h4>
            <p><strong>Total USD:</strong> {{ new Intl.NumberFormat('en-US', {
              style: 'currency',
              currency: 'USD'
            }).format(totalInUSD) }}</p>
            <p><strong>Total IDR:</strong> {{ new Intl.NumberFormat('id-ID', {
              style: 'currency',
              currency: 'IDR'
            }).format(totalInIDR) }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 10. Performance Optimization Computed Properties -->
    <section class="demo-section">
      <h2>🔟 Performance Optimization Computed Properties</h2>
      <div class="demo-content">
        <div class="performance-demo">
          <h3>Caching and Performance</h3>

          <div class="dataset-info">
            <p><strong>Dataset Size:</strong> {{ largeDataSet.length }} items</p>
            <p><strong>Expensive Computation Result:</strong> {{ expensiveComputation }}</p>
            <p><em>Check the console to see when the expensive computation runs (only when dependencies change)</em></p>
          </div>

          <div class="performance-controls">
            <button @click="addRandomData">Add Random Data</button>
            <button @click="largeDataSet.push({ ...largeDataSet[0], id: largeDataSet.length + 1 })">
              Trigger Recomputation
            </button>
          </div>

          <div class="performance-explanation">
            <h4>How Caching Works:</h4>
            <ul>
              <li>Computed properties cache their results</li>
              <li>They only recompute when dependencies change</li>
              <li>This provides better performance than methods</li>
              <li>Check browser console to see when computation runs</li>
            </ul>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// Additional reactive data for template
const newTodoText = ref('')
</script>

<style scoped>
.computed-demo {
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

/* Counter Demo */
.counter-demo {
  text-align: center;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 10px;
}

.counter-demo p {
  margin: 5px 0;
  font-size: 1.1em;
}

.controls, .base-controls {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 15px;
  flex-wrap: wrap;
}

/* Name Demo */
.name-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
}

.input-group label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.input-group input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

.results {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin: 15px 0;
}

.results p {
  margin: 5px 0;
}

.writable-demo {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #ffeaa7;
}

/* Array Demo */
.array-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.numbers-display {
  background-color: #e8f5e8;
  padding: 10px;
  border-radius: 4px;
  margin: 10px 0;
  word-break: break-all;
}

.filter-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 15px 0;
}

.array-results {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  margin: 15px 0;
}

.result-group {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.result-group h4 {
  margin: 0 0 10px 0;
  color: #495057;
}

.result-group p {
  margin: 5px 0;
}

.number-list {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.number-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 10px;
  margin: 5px 0;
  background-color: #f8f9fa;
  border-radius: 4px;
}

/* Profile Demo */
.profile-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.profile-inputs {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.profile-results {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin: 15px 0;
}

.profile-results h4 {
  margin: 0 0 10px 0;
  color: #1976d2;
}

.profile-results p {
  margin: 5px 0;
}

.profile-actions {
  text-align: center;
}

/* Cart Demo */
.cart-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.available-products {
  margin-bottom: 20px;
}

.product-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 5px 0;
  background-color: white;
  border-radius: 4px;
  border: 1px solid #dee2e6;
}

.cart-items {
  margin-bottom: 20px;
}

.cart-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  margin: 10px 0;
  background-color: white;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.item-info h5 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.item-info p {
  margin: 2px 0;
  font-size: 12px;
  color: #6c757d;
}

.item-controls {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
}

.item-controls input {
  width: 60px;
  text-align: center;
}

.cart-summary {
  background-color: #d4edda;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #c3e6cb;
}

.cart-summary h4 {
  margin: 0 0 15px 0;
  color: #155724;
}

.summary-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 10px;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 12px;
  background-color: white;
  border-radius: 4px;
}

.summary-item.total {
  grid-column: 1 / -1;
  background-color: #28a745;
  color: white;
  font-weight: bold;
  font-size: 1.1em;
}

.empty-cart {
  text-align: center;
  padding: 40px;
  color: #6c757d;
  font-style: italic;
}

/* Todo Demo */
.todo-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.todo-input {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.todo-input input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.todo-stats {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
}

.todo-stats h4 {
  margin: 0 0 10px 0;
  color: #1976d2;
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 10px;
}

.stat-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 12px;
  background-color: white;
  border-radius: 4px;
  text-align: center;
}

.progress-bar {
  width: 100%;
  height: 20px;
  background-color: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  margin: 15px 0;
}

.progress-fill {
  height: 100%;
  background: linear-gradient(90deg, #28a745, #20c997);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  transition: width 0.3s ease;
}

.todo-list {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.todo-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  margin: 5px 0;
  border-radius: 4px;
  transition: background-color 0.2s;
}

.todo-item:hover {
  background-color: #f8f9fa;
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

.todo-date {
  font-size: 12px;
  color: #6c757d;
}

/* Form Demo */
.form-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.validation-form {
  background-color: white;
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

.error {
  color: #dc3545;
  font-size: 12px;
  margin-top: 5px;
  display: block;
}

.form-status {
  text-align: center;
  padding: 15px;
  margin-top: 20px;
  border-radius: 8px;
}

.form-status.valid {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.form-status.invalid {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

/* Search Demo */
.search-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.search-controls {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.control-group {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.control-group label {
  font-weight: bold;
  color: #2c3e50;
}

.control-group input,
.control-group select {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.filtered-results {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.filtered-todo {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px;
  margin: 5px 0;
  border-radius: 4px;
  background-color: #f8f9fa;
}

.no-results {
  text-align: center;
  padding: 40px;
  color: #6c757d;
  font-style: italic;
}

/* Currency Demo */
.currency-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.exchange-controls {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}

.exchange-controls label {
  font-weight: bold;
}

.exchange-controls input {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 150px;
}

.formatted-prices {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-bottom: 20px;
}

.price-item {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.price-item h5 {
  margin: 0 0 10px 0;
  color: #2c3e50;
}

.price-item p {
  margin: 5px 0;
  font-weight: bold;
}

.totals {
  background-color: #d4edda;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #c3e6cb;
}

.totals h4 {
  margin: 0 0 10px 0;
  color: #155724;
}

.totals p {
  margin: 5px 0;
  font-size: 1.1em;
}

/* Performance Demo */
.performance-demo {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
}

.dataset-info {
  background-color: #fff3cd;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #ffeaa7;
  margin-bottom: 15px;
}

.dataset-info p {
  margin: 5px 0;
}

.dataset-info strong {
  color: #856404;
}

.performance-controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.performance-explanation {
  background-color: #e3f2fd;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #bbdefb;
}

.performance-explanation h4 {
  margin: 0 0 10px 0;
  color: #1976d2;
}

.performance-explanation ul {
  margin: 0;
  padding-left: 20px;
}

.performance-explanation li {
  margin: 5px 0;
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
  .computed-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .profile-inputs,
  .stats-grid,
  .summary-grid,
  .search-controls {
    grid-template-columns: 1fr;
  }

  .array-results {
    grid-template-columns: 1fr;
  }

  .controls,
  .base-controls {
    flex-direction: column;
  }
}
</style>

## ❓ Pertanyaan: Apa Fungsi Computed Properties?

Computed Properties memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk mengelola data kompleks:

### 1. **Caching Otomatis untuk Performa**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 1: Caching otomatis untuk performa optimal
const basePrice = ref(100)
const quantity = ref(5)
const discount = ref(10)

// Computed dengan caching - hanya recompute jika dependencies berubah
const totalPrice = computed(() => {
  console.log('Computing total price...') // Hanya berjalan saat dependencies berubah
  return (basePrice.value * quantity.value) - (basePrice.value * quantity.value * discount.value / 100)
})

// Method tanpa caching - selalu berjalan setiap render
const getTotalPrice = () => {
  console.log('Method called every time...') // Berjalan setiap render
  return (basePrice.value * quantity.value) - (basePrice.value * quantity.value * discount.value / 100)
}
</script>

<template>
  <div>Price: {{ totalPrice }}</div>
  <div>Method: {{ getTotalPrice() }}</div>
</template>
```

**Keuntungan:**
- ✅ **Performa optimal** - hanya recompute saat dependencies berubah
- ✅ **Memory efficient** - hasil disimpan dalam cache
- ✅ **No redundant calculations** - menghindari perhitungan ulang yang tidak perlu

### 2. **Derived Data yang Reaktif**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 2: Otomatis update saat data sumber berubah
const firstName = ref('John')
const lastName = ref('Doe')
const age = ref(25)

// Data derived yang otomatis reaktif
const fullName = computed(() => `${firstName.value} ${lastName.value}`)
const isAdult = computed(() => age.value >= 18)
const displayName = computed(() => {
  return isAdult.value ? fullName.value : `${firstName.value} (Minor)`
})

const updateName = () => {
  firstName.value = 'Jane'
  // fullName dan displayName otomatis update!
}
</script>

<template>
  <div>{{ displayName }}</div>
  <div>Adult: {{ isAdult ? 'Yes' : 'No' }}</div>
</template>
```

**Keuntungan:**
- ✅ **Automatic updates** - data derived otomatis update
- ✅ **Single source of truth** - hanya ubah data sumber
- ✅ **Reactive chain** - computed properties bisa chain ke computed lain

### 3. **Complex Data Transformation**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 3: Transformasi data kompleks dengan mudah
const products = ref([
  { id: 1, name: 'Laptop', price: 15000000, category: 'electronics' },
  { id: 2, name: 'Mouse', price: 250000, category: 'accessories' },
  { id: 3, name: 'Keyboard', price: 750000, category: 'accessories' }
])

const searchQuery = ref('')
const selectedCategory = ref('all')

// Transformasi data kompleks
const filteredProducts = computed(() => {
  return products.value
    .filter(product => {
      const matchesSearch = product.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      const matchesCategory = selectedCategory.value === 'all' || product.category === selectedCategory.value
      return matchesSearch && matchesCategory
    })
    .map(product => ({
      ...product,
      formattedPrice: new Intl.NumberFormat('id-ID', {
        style: 'currency',
        currency: 'IDR'
      }).format(product.price)
    }))
    .sort((a, b) => a.price - b.price)
})
</script>

<template>
  <div v-for="product in filteredProducts" :key="product.id">
    {{ product.name }} - {{ product.formattedPrice }}
  </div>
</template>
```

**Keuntungan:**
- ✅ **Data transformation** - ubah format data tanpa mengubah data asli
- ✅ **Filtering & sorting** - logika kompleks di computed properties
- ✅ **Formatting** - tampilan data yang konsisten

### 4. **Form Validation Real-time**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 4: Validation yang real-time dan reaktif
const formData = ref({
  email: '',
  password: '',
  confirmPassword: ''
})

const emailError = computed(() => {
  const email = formData.value.email
  if (!email) return 'Email is required'
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(email)) return 'Please enter a valid email'
  return ''
})

const passwordError = computed(() => {
  const password = formData.value.password
  if (!password) return 'Password is required'
  if (password.length < 6) return 'Password must be at least 6 characters'
  if (!/[A-Z]/.test(password)) return 'Password must contain uppercase letter'
  return ''
})

const formIsValid = computed(() => {
  return !emailError.value && !passwordError.value && formData.value.password === formData.value.confirmPassword
})
</script>

<template>
  <form>
    <input v-model="formData.email" placeholder="Email" />
    <span v-if="emailError" class="error">{{ emailError }}</span>

    <input v-model="formData.password" type="password" placeholder="Password" />
    <span v-if="passwordError" class="error">{{ passwordError }}</span>

    <button :disabled="!formIsValid">Submit</button>
  </form>
</template>
```

**Keuntungan:**
- ✅ **Real-time validation** - error muncul saat user mengetik
- ✅ **Reactive form state** - button otomatis enable/disable
- ✅ **Clean validation logic** - logic terpisah dari template

### 5. **Data Aggregation & Analytics**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 5: Aggregasi data untuk analytics
const sales = ref([
  { product: 'Laptop', amount: 15000000, category: 'Electronics', month: 'January' },
  { product: 'Mouse', amount: 250000, category: 'Accessories', month: 'January' },
  { product: 'Keyboard', amount: 750000, category: 'Accessories', month: 'February' },
  { product: 'Monitor', amount: 3000000, category: 'Electronics', month: 'February' }
])

// Aggregasi data
const totalSales = computed(() => {
  return sales.value.reduce((total, sale) => total + sale.amount, 0)
})

const salesByCategory = computed(() => {
  return sales.value.reduce((acc, sale) => {
    acc[sale.category] = (acc[sale.category] || 0) + sale.amount
    return acc
  }, {})
})

const averageSaleAmount = computed(() => {
  return sales.value.length > 0 ? totalSales.value / sales.value.length : 0
})

const bestSellingProduct = computed(() => {
  return sales.value.reduce((best, current) =>
    current.amount > best.amount ? current : best
  , sales.value[0])
})
</script>

<template>
  <div>Total Sales: {{ totalSales.toLocaleString('id-ID') }}</div>
  <div>Average Sale: {{ averageSaleAmount.toLocaleString('id-ID') }}</div>
  <div>Best Product: {{ bestSellingProduct.product }}</div>
</template>
```

**Keuntungan:**
- ✅ **Data aggregation** - hitung total, rata-rata, dll secara otomatis
- ✅ **Analytics dashboard** - tampilan metrics yang update real-time
- ✅ **Business insights** - analisis data yang kompleks

## 🎯 Best Practices untuk Menggunakan Computed Properties

### 1. **Gunakan Computed untuk Derived Data**
```vue
<script setup>
import { ref, computed } from 'vue'

const items = ref([1, 2, 3, 4, 5])

// ✅ GOOD: Computed untuk derived data
const evenItems = computed(() => items.value.filter(item => item % 2 === 0))
const itemCount = computed(() => items.value.length)

// ❌ AVOID: Computed untuk actions
const addItem = computed(() => {  // Ini seharusnya method!
  return () => items.value.push(items.value.length + 1)
})
</script>
```

### 2. **Hindari Side Effects**
```vue
<script setup>
import { ref, computed } from 'vue'

const user = ref({ name: 'John', score: 100 })

// ✅ GOOD: Murni computation
const userLevel = computed(() => {
  const score = user.value.score
  if (score >= 1000) return 'Expert'
  if (score >= 500) return 'Advanced'
  return 'Beginner'
})

// ❌ AVOID: Side effects di computed
const userLevelWithSideEffect = computed(() => {
  const level = userLevel.value
  console.log('User level:', level)  // Side effect!
  return level
})
</script>
```

### 3. **Writable Computed Properties**
```vue
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

// ✅ Writable computed untuk two-way binding
const fullName = computed({
  get() {
    return `${firstName.value} ${lastName.value}`
  },
  set(newValue) {
    const names = newValue.trim().split(' ')
    firstName.value = names[0] || ''
    lastName.value = names.slice(1).join(' ') || ''
  }
})
</script>

<template>
  <input v-model="fullName" placeholder="Enter full name" />
  <!-- Mengubah ini akan update firstName dan lastName -->
</template>
```

## 🎉 Kesimpulan

Computed Properties adalah fitur **sangat powerful** dalam Vue.js yang memberikan:

- ✅ **Performa optimal** - caching otomatis untuk menghindari perhitungan ulang
- ✅ **Reaktif otomatis** - derived data yang update saat dependencies berubah
- ✅ **Clean code** - logic terpisah dari template
- ✅ **Powerful transformations** - manipulasi data yang kompleks
- ✅ **Real-time validation** - form validation yang responsif

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Computed Properties
- Vue.js Reactivity API Guide
- Vue.js Performance Best Practices

---

**🔥 Tips Pro:**
- Gunakan computed properties untuk **derived data** yang memerlukan caching
- Gunakan methods untuk **actions** dan **event handlers**
- Hindari side effects di dalam computed properties
- Manfaatkan writable computed untuk two-way binding yang kompleks
- Computed properties chain ke computed properties lain untuk logic yang kompleks

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation