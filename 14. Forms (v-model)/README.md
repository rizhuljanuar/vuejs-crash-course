# 📝 Tutorial Vue.js: Forms (v-model) untuk Pemula

## 📖 Pengenalan v-model

**v-model** adalah directive Vue.js yang digunakan untuk membuat two-way data binding pada form elements. Ini memungkinkan Anda untuk menghubungkan data JavaScript secara otomatis dengan nilai input form, sehingga perubahan di form akan langsung memperbarui data dan sebaliknya.

## 🤔 Apa itu v-model?

**v-model** adalah syntactic sugar untuk mempermudah two-way binding antara data dan form elements. Ini menggabungkan `v-bind:value` dan `v-on:input` menjadi satu directive yang lebih sederhana.

**Sintaks Dasar:**
```vue
<!-- Equivalen dengan -->
<input :value="message" @input="message = $event.target.value" />

<!-- Menggunakan v-model (lebih sederhana) -->
<input v-model="message" />
```

## 📁 Struktur Proyek

```
14. Forms (v-model)/
├── README.md                              # Dokumentasi ini
└── src/
    ├── components/
    │   └── FormComponent.vue           # Komponen dengan contoh v-model
    └── App.vue                             # Komponen utama
```

## 🔍 Analisis Kode Sumber

### Komponen `FormComponent.vue`

```vue
<script setup>
import { reactive } from 'vue'
const formData = reactive({ username: '', email: '', password: '' })
</script>

<template>
  <form @submit.prevent="alert('form submitted')">
    <input type="text" placeholder="Please enter your name." v-model="formData.username" />
    <input type="email" placeholder="Please enter your email." v-model="formData.email" />
    <input type="password" placeholder="Please enter your password." v-model="formData.password" />
    <button type="submit">Submit</button>
  </form>

  <h1>{{ formData.username }}</h1>
  <h1>{{ formData.email }}</h1>
  <h1>{{ formData.password }}</h1>
</template>
```

**Penjelasan:**
- `reactive()` membuat object formData yang reaktif
- `v-model="formData.username"` mengikat input text ke property username
- Perubahan di input akan langsung memperbarui formData
- Perubahan di formData akan langsung memperbarui input

## 📖 Konsep Fundamental v-model

### 1. **Two-Way Binding**
```vue
<script setup>
import { ref } from 'vue'
const message = ref('Hello Vue!')

// Ketika user mengetik di input:
// message.value akan otomatis diupdate
// Dan sebaliknya, jika message.value diubah di script, input akan terupdate
</script>

<template>
  <input v-model="message" />
  <p>You typed: {{ message }}</p>
</template>
```

### 2. **v-model dengan Berbagai Input Types**

#### **Text Input**
```vue
<input v-model="username" />
```

#### **Textarea**
```vue
<textarea v-model="description"></textarea>
```

#### **Checkbox**
```vue
<input type="checkbox" v-model="isActive" />

<!-- Untuk boolean values -->
<input type="checkbox" v-model="isActive" />
<!-- isActive = true/false -->

<!-- Untuk array values -->
<input type="checkbox" value="apple" v-model="fruits" />
<input type="checkbox" value="banana" v-model="fruits" />
<!-- fruits = ['apple', 'banana'] -->
```

#### **Radio Buttons**
```vue
<input type="radio" value="male" v-model="gender" />
<input type="radio" value="female" v-model="gender" />
<!-- gender = 'male' atau 'female' -->
```

#### **Select Dropdown**
```vue
<select v-model="selectedCountry">
  <option value="id">Indonesia</option>
  <option value="my">Malaysia</option>
  <option value="sg">Singapore</option>
</select>
<!-- selectedCountry = 'id', 'my', atau 'sg' -->

<!-- Multiple select -->
<select v-model="selectedCountries" multiple>
  <option value="id">Indonesia</option>
  <option value="my">Malaysia</option>
  <option value="sg">Singapore</option>
</select>
<!-- selectedCountries = ['id', 'my', 'sg'] -->
```

### 3. **Modifiers v-model**

#### **.lazy**
```vue
<!-- Default: update on every input event -->
<input v-model="message" />

<!-- Dengan .lazy: update only on change event (lose focus) -->
<input v-model.lazy="message" />
```

#### **.number**
```vue
<!-- Default: string value -->
<input type="number" v-model="age" /> <!-- age adalah string -->

<!-- Dengan .number: otomatis konversi ke number -->
<input type="number" v-model.number="age" /> <!-- age adalah number -->
```

#### **.trim**
```vue
<!-- Default: termasuk whitespace -->
<input v-model="username" /> <!-- username bisa "  john  " -->

<!-- Dengan .trim: otomatis hapus whitespace -->
<input v-model.trim="username" /> <!-- username menjadi "john" -->
```

## 🚀 Contoh Lengkap dan Interaktif

Mari kita buat komponen yang mendemonstrasikan berbagai penggunaan v-model:

### Enhanced Demo: Forms (v-model) Showcase

```vue
<script setup>
import { ref, reactive, computed } from 'vue'

// 1. Basic Text Inputs
const username = ref('JohnDoe')
const email = ref('john@example.com')
const searchQuery = ref('')

// 2. Textarea
const bio = ref('I love Vue.js!')
const message = ref('')

// 3. Password Input
const password = ref('')
const confirmPassword = ref('')

// 4. Checkbox - Boolean
const isActive = ref(true)
const acceptsTerms = ref(false)
const newsletter = ref(true)

// 5. Checkbox - Array
const hobbies = ref(['coding', 'reading', 'gaming'])
const skills = ref([])

// 6. Radio Buttons
const gender = ref('male')
const experience = ref('beginner')

// 7. Select Dropdown
const country = ref('id')
const city = ref('')
const languages = ref(['en', 'id'])

// 8. Multiple Select
const favoriteColors = ref(['blue', 'green'])
const interests = ref(['technology', 'design'])

// 9. Number Input
const age = ref(25)
const quantity = ref(1)
const price = ref(100)

// 10. Range Input
const volume = ref(50)
const brightness = ref(75)
const rating = ref(3)

// 11. Complex Form Object
const userProfile = reactive({
  personal: {
    firstName: 'John',
    lastName: 'Doe',
    dateOfBirth: '1990-01-01',
    phone: '+62812345678',
    avatar: ''
  },
  preferences: {
    theme: 'light',
    language: 'en',
    timezone: 'Asia/Jakarta',
    notifications: {
      email: true,
      push: false,
      sms: true
    }
  },
  security: {
    password: '',
    twoFactorEnabled: false,
    sessionTimeout: 30
  }
})

// 12. Shopping Cart
const cart = reactive({
  items: [],
  couponCode: '',
  shippingMethod: 'standard',
  paymentMethod: 'credit_card'
})

// 13. Product Filter
const productFilter = reactive({
  category: 'all',
  priceRange: [0, 1000],
  inStockOnly: true,
  sortBy: 'name'
})

// 14. Survey Form
const survey = reactive({
  satisfaction: 5,
  recommendations: '',
  wouldRecommend: true,
  improvements: []
})

// 15. Dynamic Fields
const dynamicFields = ref([
  { id: 1, label: 'Full Name', type: 'text', value: '', required: true },
  { id: 2, label: 'Email', type: 'email', value: '', required: true },
  { id: 3, label: 'Age', type: 'number', value: '', required: false }
])

// Computed properties
const fullName = computed(() => `${userProfile.personal.firstName} ${userProfile.personal.lastName}`)
const isFormValid = computed(() => {
  return username.value.length >= 3 &&
         email.value.includes('@') &&
         password.value.length >= 6 &&
         password.value === confirmPassword.value
})

const cartTotal = computed(() => {
  return cart.items.reduce((total, item) => total + (item.price * item.quantity), 0)
})

const filteredProducts = computed(() => {
  // Simulasi products untuk demo
  const products = [
    { id: 1, name: 'Laptop', price: 500, category: 'electronics', inStock: true },
    { id: 2, name: 'Mouse', price: 50, category: 'accessories', inStock: true },
    { id: 3, name: 'Keyboard', price: 100, category: 'accessories', inStock: false }
  ]

  return products.filter(product => {
    const inPriceRange = product.price >= productFilter.priceRange[0] && product.price <= productFilter.priceRange[1]
    const categoryMatch = productFilter.category === 'all' || product.category === productFilter.category
    const stockMatch = !productFilter.inStockOnly || product.inStock

    return inPriceRange && categoryMatch && stockMatch
  })
})

// Event handlers
function submitForm() {
  console.log('Form submitted:', {
    username: username.value,
    email: email.value,
    password: password.value,
    profile: userProfile,
    cart: cart
  })
  alert('Form submitted successfully! Check console for data.')
}

function resetForm() {
  username.value = ''
  email.value = ''
  password.value = ''
  confirmPassword.value = ''
  Object.assign(userProfile.personal, {
    firstName: 'John',
    lastName: 'Doe',
    dateOfBirth: '1990-01-01',
    phone: '+62812345678',
    avatar: ''
  })
}

function addToCart(product) {
  const existingItem = cart.items.find(item => item.id === product.id)
  if (existingItem) {
    existingItem.quantity++
  } else {
    cart.items.push({
      id: product.id,
      name: product.name,
      price: product.price,
      quantity: 1
    })
  }
}

function removeFromCart(itemId) {
  const index = cart.items.findIndex(item => item.id === itemId)
  if (index > -1) {
    cart.items.splice(index, 1)
  }
}

function addDynamicField() {
  const newId = Math.max(...dynamicFields.value.map(f => f.id)) + 1
  const newField = {
    id: newId,
    label: `Field ${newId}`,
    type: 'text',
    value: '',
    required: false
  }
  dynamicFields.value.push(newField)
}

function removeDynamicField(fieldId) {
  const index = dynamicFields.value.findIndex(f => f.id === fieldId)
  if (index > -1) {
    dynamicFields.value.splice(index, 1)
  }
}

function submitSurvey() {
  console.log('Survey submitted:', survey)
  alert('Thank you for your feedback!')
}
</script>

<template>
  <div class="v-model-demo">
    <h1>📝 Vue.js v-model Showcase</h1>

    <!-- 1. Basic Text Inputs -->
    <section class="demo-section">
      <h2>1️⃣ Basic Text Inputs</h2>
      <div class="demo-content">
        <div class="input-group">
          <label for="username">Username:</label>
          <input
            id="username"
            v-model="username"
            placeholder="Enter your username"
            class="form-input"
          />
          <p>Current value: <strong>{{ username || '(empty)' }}</strong></p>
        </div>

        <div class="input-group">
          <label for="email">Email:</label>
          <input
            id="email"
            v-model="email"
            type="email"
            placeholder="Enter your email"
            class="form-input"
          />
          <p>Current value: <strong>{{ email || '(empty)' }}</strong></p>
        </div>

        <div class="input-group">
          <label for="search">Search:</label>
          <input
            id="search"
            v-model="searchQuery"
            placeholder="Search here..."
            class="form-input search-input"
          />
          <p>Searching for: <strong>{{ searchQuery || '(nothing)' }}</strong></p>
        </div>
      </div>
    </section>

    <!-- 2. Textarea -->
    <section class="demo-section">
      <h2>2️⃣ Textarea</h2>
      <div class="demo-content">
        <div class="input-group">
          <label for="bio">Bio:</label>
          <textarea
            id="bio"
            v-model="bio"
            placeholder="Tell us about yourself..."
            rows="4"
            class="form-textarea"
          ></textarea>
          <p>Character count: <strong>{{ bio.length }}</strong></p>
        </div>

        <div class="input-group">
          <label for="message">Message:</label>
          <textarea
            id="message"
            v-model="message"
            placeholder="Type your message here..."
            rows="3"
            class="form-textarea"
          ></textarea>
          <p>Message preview: {{ message || '(empty)' }}</p>
        </div>
      </div>
    </section>

    <!-- 3. Password Input -->
    <section class="demo-section">
      <h2>3️⃣ Password Input</h2>
      <div class="demo-content">
        <div class="input-group">
          <label for="password">Password:</label>
          <input
            id="password"
            v-model="password"
            type="password"
            placeholder="Enter your password"
            class="form-input"
          />
          <p>Password length: <strong>{{ password.length }}</strong></p>
        </div>

        <div class="input-group">
          <label for="confirmPassword">Confirm Password:</label>
          <input
            id="confirmPassword"
            v-model="confirmPassword"
            type="password"
            placeholder="Confirm your password"
            class="form-input"
          />
          <p v-if="password && confirmPassword" class="password-match">
            Passwords {{ password === confirmPassword ? '✅ Match' : '❌ Do not match' }}
          </p>
        </div>
      </div>
    </section>

    <!-- 4. Checkbox - Boolean -->
    <section class="demo-section">
      <h2>4️⃣ Checkbox (Boolean Values)</h2>
      <div class="demo-content">
        <div class="checkbox-group">
          <label>
            <input type="checkbox" v-model="isActive" />
            Active Status: {{ isActive ? '✅ Active' : '❌ Inactive' }}
          </label>

          <label>
            <input type="checkbox" v-model="acceptsTerms" />
            Accept Terms: {{ acceptsTerms ? '✅ Accepted' : '❌ Not Accepted' }}
          </label>

          <label>
            <input type="checkbox" v-model="newsletter" />
            Newsletter: {{ newsletter ? '✅ Subscribed' : '❌ Not Subscribed' }}
          </label>
        </div>
      </div>
    </section>

    <!-- 5. Checkbox - Array -->
    <section class="demo-section">
      <h2>5️⃣ Checkbox (Array Values)</h2>
      <div class="demo-content">
        <div class="checkbox-group">
          <h4>Hobbies:</h4>
          <label v-for="hobby in ['coding', 'reading', 'gaming', 'traveling', 'cooking']" :key="hobby">
            <input
              type="checkbox"
              :value="hobby"
              v-model="hobbies"
            />
            {{ hobby }}
          </label>
          <p>Selected: <strong>{{ hobbies.join(', ') || '(none)' }}</p>
        </div>

        <div class="checkbox-group">
          <h4>Skills:</h4>
          <label v-for="skill in ['JavaScript', 'Vue.js', 'Python', 'React', 'Node.js']" :key="skill">
            <input
              type="checkbox"
              :value="skill"
              v-model="skills"
            />
            {{ skill }}
          </label>
          <p>Selected: <strong>{{ skills.join(', ') || '(none)' }}</p>
        </div>
      </div>
    </section>

    <!-- 6. Radio Buttons -->
    <section class="demo-section">
      <h2>6️⃣ Radio Buttons</h2>
      <div class="demo-content">
        <div class="radio-group">
          <h4>Gender:</h4>
          <label v-for="gender in ['male', 'female', 'other']" :key="gender">
            <input
              type="radio"
              :value="gender"
              v-model="gender"
              name="gender"
            />
            {{ gender.charAt(0).toUpperCase() + gender.slice(1) }}
          </label>
          <p>Selected: <strong>{{ gender }}</strong></p>
        </div>

        <div class="radio-group">
          <h4>Experience Level:</h4>
          <label v-for="level in ['beginner', 'intermediate', 'advanced']" :key="level">
            <input
              type="radio"
              :value="level"
              v-model="experience"
              name="experience"
            />
            {{ level.charAt(0).toUpperCase() + level.slice(1) }}
          </label>
          <p>Selected: <strong>{{ experience }}</strong></p>
        </div>
      </div>
    </section>

    <!-- 7. Select Dropdown -->
    <section class="demo-section">
      <h2>7️⃣ Select Dropdown</h2>
      <div class="demo-content">
        <div class="select-group">
          <label for="country">Country:</label>
          <select id="country" v-model="country" class="form-select">
            <option value="">Select a country</option>
            <option value="id">🇮🇩 Indonesia</option>
            <option value="my">🇲🇾 Malaysia</option>
            <option value="sg">🇸🇬 Singapore</option>
            <option value="th">🇹🇹 Thailand</option>
            <option value="ph">🇵🇭 Philippines</option>
          </select>
          <p>Selected: <strong>{{ country || '(none)' }}</p>
        </div>

        <div class="select-group">
          <label for="city">City:</label>
          <select id="city" v-model="city" class="form-select" :disabled="!country">
            <option value="">Select a city</option>
            <option v-if="country === 'id'" value="jakarta">Jakarta</option>
            <option v-if="country === 'id'" value="surabaya">Surabaya</option>
            <option v-if="country === 'id'" value="bandung">Bandung</option>
            <option v-if="country === 'my'" value="kuala-lumpur">Kuala Lumpur</option>
            <option v-if="country === 'sg'" value="singapore">Singapore</option>
          </select>
          <p v-if="city">Selected: <strong>{{ city }}</strong></p>
          <p v-else>Please select a country first</p>
        </div>
      </div>
    </section>

    <!-- 8. Multiple Select -->
    <section class="demo-section">
      <h2>8️⃣ Multiple Select</h2>
      <div class="demo-content">
        <div class="select-group">
          <label for="languages">Languages:</label>
          <select
            id="languages"
            v-model="languages"
            multiple
            class="form-select"
            size="4"
          >
            <option value="en">English</option>
            <option value="id">Bahasa Indonesia</option>
            <option value="zh">中文</option>
            <option value="es">Español</option>
            <option value="fr">Français</option>
            <option value="de">Deutsch</option>
          </select>
          <p>Selected: <strong>{{ languages.join(', ') || '(none)' }}</p>
        </div>

        <div class="select-group">
          <label for="colors">Favorite Colors:</label>
          <select
            id="colors"
            v-model="favoriteColors"
            multiple
            class="form-select"
            size="5"
          >
            <option value="red">🔴 Red</option>
            <option value="blue">🔵 Blue</option>
            <option value="green">🟢 Green</option>
            <option value="yellow">🟡 Yellow</option>
            <option value="purple">🟣 Purple</option>
            <option value="orange">🟠 Orange</option>
            <option value="black">⚫ Black</option>
            <option value="white">⚪ White</option>
          </select>
          <p>Selected: <strong>{{ favoriteColors.map(c => c + ' ' + getColorEmoji(c)).join(', ') || '(none)' }}</strong></p>
        </div>
      </div>
    </section>

    <!-- 9. Number Input -->
    <section class="demo-section">
      <h2>9️⃣ Number Input</h2>
      <div class="demo-content">
        <div class="input-group">
          <label for="age">Age:</label>
          <input
            id="age"
            v-model.number="age"
            type="number"
            min="1"
            max="120"
            class="form-input"
          />
          <p>Age: <strong>{{ age }}</strong> years old</p>
        </div>

        <div class="input-group">
          <label for="quantity">Quantity:</label>
          <input
            id="quantity"
            v-model.number="quantity"
            type="number"
            min="1"
            class="form-input"
          />
          <p>Quantity: <strong>{{ quantity }}</strong> items</p>
          <p>Total: <strong>{{ quantity * 100 }}</strong> (assuming $100 per item)</p>
        </div>

        <div class="input-group">
          <label for="price">Price:</label>
          <input
            id="price"
            v-model.number="price"
            type="number"
            min="0"
            step="0.01"
            class="form-input"
          />
          <p>Price: <strong>${{ price }}</strong></p>
        </div>
      </div>
    </section>

    <!-- 10. Range Input -->
    <section class="demo-section">
      <h2>🔟 Range Input</h2>
      <div class="demo-content">
        <div class="input-group">
          <label for="volume">Volume: {{ volume }}%</label>
          <input
            id="volume"
            v-model="volume"
            type="range"
            min="0"
            max="100"
            class="form-range"
          />
          <div class="volume-bar">
            <div class="volume-fill" :style="{ width: volume + '%' }"></div>
          </div>
        </div>

        <div class="input-group">
          <label for="brightness">Brightness: {{ brightness }}%</label>
          <input
            id="brightness"
            v-model="brightness"
            type="range"
            min="0"
            max="100"
            class="form-range"
          />
          <div class="brightness-box" :style="{ filter: `brightness(${brightness}%)` }">
            <p>This box brightness changes with the slider!</p>
          </div>
        </div>

        <div class="input-group">
          <label for="rating">Rating: {{ rating }} stars</label>
          <input
            id="rating"
            v-model.number="rating"
            type="range"
            min="1"
            max="5"
            class="form-range"
          />
          <div class="rating-stars">
            <span v-for="star in 5" :key="star" class="star">
              {{ star <= rating ? '⭐' : '☆' }}
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- 11. Complex Form Object -->
    <section class="demo-section">
      <h2>🔷 Complex Form Object</h2>
      <div class="demo-content">
        <div class="complex-form">
          <h3>User Profile</h3>

          <div class="form-section">
            <h4>Personal Information</h4>
            <div class="input-group">
              <label>First Name:</label>
              <input v-model="userProfile.personal.firstName" class="form-input" />
            </div>
            <div class="input-group">
              <label>Last Name:</label>
              <input v-model="userProfile.personal.lastName" class="form-input" />
            </div>
            <div class="input-group">
              <label>Date of Birth:</label>
              <input
                v-model="userProfile.personal.dateOfBirth"
                type="date"
                class="form-input"
              />
            </div>
            <div class="input-group">
              <label>Phone:</label>
              <input v-model="userProfile.personal.phone" class="form-input" />
            </div>
            <p>Full Name: <strong>{{ fullName }}</strong></p>
          </div>

          <div class="form-section">
            <h4>Preferences</h4>
            <div class="input-group">
              <label>Theme:</label>
              <select v-model="userProfile.preferences.theme" class="form-select">
                <option value="light">Light</option>
                <option value="dark">Dark</option>
                <option value="auto">Auto</option>
              </select>
            </div>
            <div class="input-group">
              <label>Language:</label>
              <select v-model="userProfile.preferences.language" class="form-select">
                <option value="en">English</option>
                <option value="id">Bahasa Indonesia</option>
                <option value="zh">中文</option>
              </select>
            </div>
            <div class="input-group">
              <label>Timezone:</label>
              <select v-model="userProfile.preferences.timezone" class="form-select">
                <option value="Asia/Jakarta">Asia/Jakarta</option>
                <option value="Asia/Singapore">Asia/Singapore</option>
                <option value="UTC">UTC</option>
              </select>
            </div>
          </div>

          <div class="form-section">
            <h4>Notifications</h4>
            <div class="checkbox-group">
              <label>
                <input type="checkbox" v-model="userProfile.preferences.notifications.email" />
                Email notifications
              </label>
              <label>
                <input type="checkbox" v-model="userProfile.preferences.notifications.push" />
                Push notifications
              </label>
              <label>
                <input type="checkbox" v-model="userProfile.preferences.notifications.sms" />
                SMS notifications
              </label>
            </div>
          </div>

          <div class="form-section">
            <h4>Security</h4>
            <div class="input-group">
              <label>Password:</label>
              <input
                v-model="userProfile.security.password"
                type="password"
                placeholder="Enter password"
                class="form-input"
              />
            </div>
            <div class="input-group">
              <label>Two-Factor Authentication:</label>
              <label>
                <input
                  type="checkbox"
                  v-model="userProfile.security.twoFactorEnabled"
                />
                Enable 2FA
              </label>
            </div>
            <div class="input-group">
              <label>Session Timeout (minutes):</label>
              <input
                v-model.number="userProfile.security.sessionTimeout"
                type="number"
                min="5"
                max="120"
                class="form-input"
              />
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 12. Shopping Cart -->
    <section class="demo-section">
      <h2>🛒 Shopping Cart with v-model</h2>
      <div class="demo-content">
        <div class="shopping-cart">
          <h3>Shopping Cart</h3>

          <div class="cart-items">
            <div v-if="cart.items.length === 0" class="empty-cart">
              Your cart is empty
            </div>
            <div
              v-for="item in cart.items"
              :key="item.id"
              class="cart-item"
            >
              <div class="item-info">
                <h4>{{ item.name }}</h4>
                <p class="price">${{ item.price }}</p>
              </div>
              <div class="item-controls">
                <button @click="item.quantity--">-</button>
                <input
                  v-model.number="item.quantity"
                  type="number"
                  min="1"
                  class="quantity-input"
                />
                <button @click="item.quantity++">+</button>
                <button @click="removeFromCart(item.id)" class="remove-btn">Remove</button>
              </div>
            </div>
          </div>

          <div class="cart-summary">
            <div class="summary-item">
              <span>Subtotal:</span>
              <span>${{ cartTotal }}</span>
            </div>
            <div class="summary-item">
              <span>Coupon:</span>
              <input
                v-model="cart.couponCode"
                placeholder="Enter coupon code"
                class="form-input"
              />
            </div>
            <div class="summary-item">
              <span>Shipping:</span>
              <select v-model="cart.shippingMethod" class="form-select">
                <option value="standard">Standard (5-7 days)</option>
                <option value="express">Express (2-3 days)</option>
                <option value="overnight">Overnight (1 day)</option>
              </select>
            </div>
            <div class="summary-item total">
              <span>Total:</span>
              <span>${{ cartTotal }}</span>
            </div>
            <div class="summary-item">
              <span>Payment:</span>
              <select v-model="cart.paymentMethod" class="form-select">
                <option value="credit_card">Credit Card</option>
                <option value="debit_card">Debit Card</option>
                <option value="paypal">PayPal</option>
                <option value="bank_transfer">Bank Transfer</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 13. Product Filter -->
    <section class="demo-section">
      <h2>🔍 Product Filter with v-model</h2>
      <div class="demo-content">
        <div class="product-filter">
          <h3>Filter Products</h3>

          <div class="filter-group">
            <label>Category:</label>
            <select v-model="productFilter.category" class="form-select">
              <option value="all">All Categories</option>
              <option value="electronics">Electronics</option>
              <option value="accessories">Accessories</option>
              <option value="books">Books</option>
            </select>
          </div>

          <div class="filter-group">
            <label>Price Range: ${{ productFilter.priceRange[0] }} - ${{ productFilter.priceRange[1] }}</label>
            <div class="range-controls">
              <input
                v-model.number="productFilter.priceRange[0]"
                type="range"
                min="0"
                max="1000"
                step="50"
                class="form-range"
              />
              <input
                v-model.number="productFilter.priceRange[1]"
                type="range"
                min="0"
                max="1000"
                step="50"
                class="form-range"
              />
            </div>
          </div>

          <div class="filter-group">
            <label>
              <input
                type="checkbox"
                v-model="productFilter.inStockOnly"
              />
              In Stock Only
            </label>
          </div>

          <div class="filter-group">
            <label>Sort By:</label>
            <select v-model="productFilter.sortBy" class="form-select">
              <option value="name">Name</option>
              <option value="price">Price (Low to High)</option>
              <option value="price-desc">Price (High to Low)</option>
            </select>
          </div>

          <div class="filtered-products">
            <h4>Filtered Products ({{ filteredProducts.length }})</h4>
            <div
              v-for="product in filteredProducts"
              :key="product.id"
              class="product-item"
            >
              <span>{{ product.name }}</span>
              <span class="price">${{ product.price }}</span>
              <span class="category">{{ product.category }}</span>
              <span class="stock">{{ product.inStock ? 'In Stock' : 'Out of Stock' }}</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- 14. Survey Form -->
    <section class="demo-section">
      <h2>📊 Survey Form</h2>
      <div class="demo-content">
        <div class="survey-form">
          <h3>Customer Satisfaction Survey</h3>

          <div class="survey-group">
            <label>Overall Satisfaction:</label>
            <div class="rating-slider">
              <input
                v-model.number="survey.satisfaction"
                type="range"
                min="1"
                max="10"
                class="form-range"
              />
              <span>{{ survey.satisfaction }}/10</span>
            </div>
          </div>

          <div class="survey-group">
            <label for="recommendations">Recommendations:</label>
            <textarea
              id="recommendations"
              v-model="survey.recommendations"
              placeholder="What would you recommend?"
              rows="3"
              class="form-textarea"
            ></textarea>
          </div>

          <div class="survey-group">
            <label>Would you recommend us?</label>
            <div class="radio-group">
              <label>
                <input
                  type="radio"
                  :value="true"
                  v-model="survey.wouldRecommend"
                  name="recommend"
                />
                Yes
              </label>
              <label>
                <input
                  type="radio"
                  :value="false"
                  v-model="survey.wouldRecommend"
                  name="recommend"
                />
                No
              </label>
            </div>
            <p>Your answer: {{ survey.wouldRecommend ? 'Yes' : 'No' }}</p>
          </div>

          <div class="survey-group">
            <label>Areas for Improvement:</label>
            <div class="checkbox-group">
              <label v-for="improvement in ['Customer Service', 'Product Quality', 'Pricing', 'Website Design', 'Delivery Speed']" :key="improvement">
                <input
                  type="checkbox"
                  :value="improvement"
                  v-model="survey.improvements"
                />
                {{ improvement }}
              </label>
            </div>
            <p>Selected improvements: {{ survey.improvements.join(', ') || '(none)' }}</p>
          </div>

          <button @click="submitSurvey" class="submit-survey">
            Submit Survey
          </button>
        </div>
      </div>
    </section>

    <!-- 15. Dynamic Fields -->
    <section class="demo-section">
      <h2>🔧 Dynamic Form Fields</h2>
      <div class="demo-content">
        <div class="dynamic-form">
          <h3>Dynamic Form Builder</h3>

          <div class="dynamic-fields">
            <div
              v-for="field in dynamicFields"
              :key="field.id"
              class="dynamic-field"
            >
              <div class="field-header">
                <label>
                  {{ field.label }}
                  <span v-if="field.required" class="required">*</span>
                </label>
                <button
                  @click="removeDynamicField(field.id)"
                  class="remove-field-btn"
                  v-if="dynamicFields.length > 1"
                >
                  ×
                </button>
              </div>

              <input
                v-if="field.type === 'text'"
                v-model="field.value"
                type="text"
                :placeholder="`Enter ${field.label.toLowerCase()}`"
                class="form-input"
                :required="field.required"
              />

              <input
                v-else-if="field.type === 'email'"
                v-model="field.value"
                type="email"
                :placeholder="`Enter ${field.label.toLowerCase()}`"
                class="form-input"
                :required="field.required"
              />

              <input
                v-else-if="field.type === 'number'"
                v-model.number="field.value"
                type="number"
                :placeholder="`Enter ${field.label.toLowerCase()}`"
                class="form-input"
                :required="field.required"
              />
            </div>

            <button @click="addDynamicField" class="add-field-btn">
              Add Field
            </button>
          </div>

          <div class="form-actions">
            <button @click="resetForm" class="reset-btn">Reset Form</button>
            <button @click="submitForm" :disabled="!isFormValid" class="submit-btn">
              {{ isFormValid ? 'Submit Form ✅' : 'Please complete the form ❌' }}
            </button>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// Helper function for color emoji
function getColorEmoji(color) {
  const colorEmojis = {
    red: '🔴',
    blue: '🔵',
    green: '🟢',
    yellow: '🟡',
    purple: '🟣',
    orange: '🟠',
    black: '⚫',
    white: '⚪'
  }
  return colorEmojis[color] || '🎨'
}
</script>

<style scoped>
.v-model-demo {
  max-width: 1000px;
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

/* Input Styles */
.input-group {
  text-align: left;
  margin-bottom: 15px;
}

.input-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.form-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 14px;
}

.form-input:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 2px rgba(66, 184, 131, 0.2);
}

.form-textarea {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 14px;
  resize: vertical;
  min-height: 80px;
}

.form-textarea:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 2px rgba(66, 184, 131, 0.2);
}

.form-select {
  width: 100%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-size: 14px;
  background-color: white;
}

.form-select:focus {
  outline: none;
  border-color: #42b883;
  box-shadow: 0 0 0 2px rgba(66, 184, 131, 0.2);
}

.form-range {
  width: 100%;
  margin-top: 10px;
}

/* Checkbox Styles */
.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.checkbox-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.checkbox-group input[type="checkbox"] {
  margin: 0;
}

/* Radio Styles */
.radio-group {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.radio-group label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
}

.radio-group input[type="radio"] {
  margin: 0;
}

/* Range Styles */
.volume-bar {
  width: 100%;
  height: 20px;
  background-color: #e9ecef;
  border-radius: 10px;
  overflow: hidden;
  margin-top: 10px;
}

.volume-fill {
  height: 100%;
  background-color: #42b883;
  transition: width 0.3s ease;
}

.brightness-box {
  padding: 20px;
  border-radius: 8px;
  background-color: #f8f9fa;
  margin-top: 10px;
  text-align: center;
  border: 1px solid #dee2e6;
}

.rating-stars {
  font-size: 1.2em;
  margin-top: 10px;
}

.star {
  cursor: pointer;
}

/* Complex Form Styles */
.complex-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.complex-form h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.form-section {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 15px;
  border: 1px solid #e9ecef;
}

.form-section h4 {
  margin: 0 0 10px 0;
  color: #495057;
  border-bottom: 2px solid #42b883;
  padding-bottom: 5px;
}

/* Shopping Cart Styles */
.shopping-cart {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.shopping-cart h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.cart-items {
  margin-bottom: 20px;
}

.cart-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px;
  background-color: white;
  border-radius: 8px;
  margin-bottom: 10px;
  border: 1px solid #dee2e6;
}

.cart-item:last-child {
  margin-bottom: 0;
}

.item-info {
  flex: 1;
}

.item-info h4 {
  margin: 0 0 5px 0;
  color: #2c3e50;
}

.item-info .price {
  margin: 0;
  color: #42b883;
  font-weight: bold;
}

.item-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-input {
  width: 60px;
  text-align: center;
}

.remove-btn {
  background-color: #dc3545;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  cursor: pointer;
  font-size: 12px;
}

.remove-btn:hover {
  background-color: #c82333;
}

.cart-summary {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.summary-item {
  display: flex;
  justify-content: space-between;
  padding: 8px 0;
  border-bottom: 1px solid #e9ecef;
}

.summary-item:last-child {
  border-bottom: none;
}

.summary-item.total {
  font-weight: bold;
  color: #42b883;
  font-size: 1.1em;
}

/* Product Filter Styles */
.product-filter {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.product-filter h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.filter-group {
  margin-bottom: 15px;
}

.filter-group label {
  display: block;
  margin-bottom: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.range-controls {
  display: flex;
  gap: 10px;
  margin-top: 5px;
}

.filtered-products {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
  margin-top: 15px;
}

.filtered-products h4 {
  margin: 0 0 10px 0;
  color: #495057;
}

.product-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  margin: 5px 0;
  background-color: #f8f9fa;
  border-radius: 4px;
}

.product-name {
  font-weight: bold;
  color: #2c3e50;
}

.product-price {
  color: #42b883;
  font-weight: bold;
}

.product-category {
  font-size: 0.8em;
  color: #6c757d;
  background-color: #e9ecef;
  padding: 2px 8px;
  border-radius: 12px;
}

.product-stock {
  font-size: 0.8em;
  padding: 2px 8px;
  border-radius: 12px;
}

.product-stock.in-stock {
  background-color: #d4edda;
  color: #155724;
}

.product-stock.out-of-stock {
  background-color: #f8d7da;
  color: #721c24;
}

/* Survey Form Styles */
.survey-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.survey-form h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.survey-group {
  margin-bottom: 20px;
}

.survey-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: bold;
  color: #2c3e50;
}

.rating-slider {
  display: flex;
  align-items: center;
  gap: 15px;
}

.rating-slider input {
  flex: 1;
}

/* Dynamic Form Styles */
.dynamic-form {
  background-color: #f8f9fa;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #dee2e6;
}

.dynamic-form h3 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

.dynamic-fields {
  margin-bottom: 20px;
}

.dynamic-field {
  background-color: white;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 10px;
  border: 1px solid #dee2e6;
}

.field-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
}

.field-header label {
  display: flex;
  align-items: center;
  gap: 5px;
  font-weight: bold;
  color: #2c3e50;
}

.required {
  color: #dc3545;
}

.remove-field-btn {
  background: none;
  border: none;
  color: #dc3545;
  font-size: 16px;
  cursor: pointer;
  padding: 0;
  margin-left: 10px;
}

.add-field-btn,
.reset-btn,
.submit-btn,
.submit-survey {
  background-color: #42b883;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 14px;
}

.add-field-btn:hover,
.reset-btn:hover,
.submit-btn:hover,
.submit-survey:hover {
  background-color: #369870;
  transform: translateY(-2px);
}

.submit-btn:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
  transform: none;
}

.reset-btn {
  background-color: #6c757d;
}

.submit-survey {
  background-color: #28a745;
}

/* Password Match Indicator */
.password-match {
  margin-top: 5px;
  padding: 8px 12px;
  border-radius: 4px;
}

.password-match.success {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.password-match.error {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

/* Form Actions */
.form-actions {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-top: 20px;
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
  .v-model-demo {
    padding: 10px;
  }

  h1 {
    font-size: 2em;
  }

  .demo-section {
    padding: 15px;
  }

  .form-actions {
    flex-direction: column;
  }

  .cart-item {
    flex-direction: column;
    gap: 10px;
    text-align: left;
  }

  .item-controls {
    justify-content: center;
  }

  .range-controls {
    flex-direction: column;
  }

  .summary-item {
    flex-direction: column;
    text-align: left;
    gap: 5px;
  }
}
</style>

## ❓ Pertanyaan: Apa Fungsi Forms (v-model)?

Forms (v-model) memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk mengelola data form:

### 1. **Two-Way Data Binding**
```vue
<script setup>
import { ref } from 'vue'

// Fungsi 1: Sync data dua arah antara form dan state
const message = ref('Hello Vue!')
const username = ref('')
const password = ref('')

// Perubahan di form otomatis update ref
// Perubahan di ref otomatis update form
</script>

<template>
  <div class="form-demo">
    <!-- Input dan state sync secara real-time -->
    <input v-model="message" placeholder="Type something..." />
    <p>You typed: {{ message }}</p>

    <!-- Data binding untuk form fields -->
    <input v-model="username" placeholder="Username" />
    <input v-model="password" type="password" placeholder="Password" />

    <!-- Preview data yang ter-sync -->
    <div class="preview">
      <h4>Live Preview:</h4>
      <p>Username: {{ username }}</p>
      <p>Password length: {{ password.length }}</p>
    </div>

    <!-- Programmatic update juga sync ke form -->
    <button @click="message = 'Updated from script!'">
      Update Message Programmatically
    </button>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Real-time sync** - perubahan langsung terlihat
- ✅ **Automatic updates** - tidak perlu manual sync
- ✅ **Bidirectional** - form → data dan data → form

### 2. **Form State Management**
```vue
<script setup>
import { reactive, computed } from 'vue'

// Fungsi 2: Centralized form state management
const form = reactive({
  personal: {
    firstName: '',
    lastName: '',
    email: '',
    phone: ''
  },
  preferences: {
    newsletter: true,
    notifications: false,
    theme: 'light'
  },
  security: {
    password: '',
    confirmPassword: '',
    twoFactor: false
  }
})

// Computed untuk derived state
const fullName = computed(() =>
  `${form.personal.firstName} ${form.personal.lastName}`.trim()
)

const isPasswordValid = computed(() =>
  form.security.password.length >= 8 &&
  form.security.password === form.security.confirmPassword
)

const isFormComplete = computed(() => {
  return form.personal.firstName &&
         form.personal.lastName &&
         form.personal.email &&
         isPasswordValid.value
})
</script>

<template>
  <form class="user-registration">
    <!-- Nested object binding -->
    <fieldset>
      <legend>Personal Information</legend>
      <input v-model="form.personal.firstName" placeholder="First Name" />
      <input v-model="form.personal.lastName" placeholder="Last Name" />
      <input v-model="form.personal.email" type="email" placeholder="Email" />
      <input v-model="form.personal.phone" placeholder="Phone" />
    </fieldset>

    <fieldset>
      <legend>Preferences</legend>
      <label>
        <input type="checkbox" v-model="form.preferences.newsletter" />
        Subscribe to newsletter
      </label>
      <label>
        <input type="checkbox" v-model="form.preferences.notifications" />
        Enable notifications
      </label>
      <select v-model="form.preferences.theme">
        <option value="light">Light Theme</option>
        <option value="dark">Dark Theme</option>
        <option value="auto">Auto Theme</option>
      </select>
    </fieldset>

    <!-- Real-time validation feedback -->
    <div class="form-status">
      <p>Full Name: <strong>{{ fullName || '(Not provided)' }}</strong></p>
      <p>Password Valid: <strong>{{ isPasswordValid ? '✅' : '❌' }}</strong></p>
      <p>Form Complete: <strong>{{ isFormComplete ? '✅ Ready to Submit' : '❌ Incomplete' }}</strong></p>
    </div>

    <button type="submit" :disabled="!isFormComplete">
      {{ isFormComplete ? 'Submit Registration' : 'Complete the Form' }}
    </button>
  </form>
</template>
```

**Keuntungan:**
- ✅ **Centralized state** - semua data form dalam satu objek
- ✅ **Computed validation** - real-time form validation
- ✅ **Nested structure** - support form yang kompleks
- ✅ **Reactive UI** - button status otomatis update

### 3. **Type-Safe Form Handling dengan Modifiers**
```vue
<script setup>
import { ref, computed } from 'vue'

// Fungsi 3: Type-safe handling dengan v-model modifiers
const userAge = ref(25)           // .number modifier
const userEmail = ref('')        // .trim modifier
const userBio = ref('')          // .lazy modifier
const searchQuery = ref('')      // .trim + .lazy combination

// Type validation
const isAgeValid = computed(() => {
  return typeof userAge.value === 'number' && userAge.value >= 18 && userAge.value <= 100
})

const isEmailValid = computed(() => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(userEmail.value)
})

// Search functionality dengan lazy loading
const searchResults = ref([])
const isSearching = ref(false)

const performSearch = async () => {
  if (!searchQuery.value.trim()) return

  isSearching.value = true
  // Simulasi API call
  setTimeout(() => {
    searchResults.value = [
      `Result for "${searchQuery.value}" - 1`,
      `Result for "${searchQuery.value}" - 2`,
      `Result for "${searchQuery.value}" - 3`
    ]
    isSearching.value = false
  }, 500)
}
</script>

<template>
  <div class="form-modifiers-demo">
    <h3>v-model Modifiers Demo</h3>

    <!-- .number - otomatis konversi ke number -->
    <div class="form-group">
      <label for="age">Age (with .number modifier):</label>
      <input
        id="age"
        v-model.number="userAge"
        type="number"
        min="1"
        max="100"
      />
      <p>Type: {{ typeof userAge }}, Value: {{ userAge }}</p>
      <p class="validation" :class="{ valid: isAgeValid, invalid: !isAgeValid }">
        {{ isAgeValid ? '✅ Valid age' : '❌ Invalid age (must be 18-100)' }}
      </p>
    </div>

    <!-- .trim - otomatis hapus whitespace -->
    <div class="form-group">
      <label for="email">Email (with .trim modifier):</label>
      <input
        id="email"
        v-model.trim="userEmail"
        type="email"
        placeholder="your@email.com"
      />
      <p>Raw: "{{ userEmail }}" (length: {{ userEmail.length }})</p>
      <p class="validation" :class="{ valid: isEmailValid, invalid: !isEmailValid }">
        {{ isEmailValid ? '✅ Valid email' : '❌ Invalid email format' }}
      </p>
    </div>

    <!-- .lazy - update hanya saat lose focus -->
    <div class="form-group">
      <label for="bio">Bio (with .lazy modifier):</label>
      <textarea
        id="bio"
        v-model.lazy="userBio"
        placeholder="Tell us about yourself..."
        rows="3"
      ></textarea>
      <p>Bio updates when you click outside (on change event)</p>
      <p>Current bio: "{{ userBio }}"</p>
    </div>

    <!-- Combined modifiers -->
    <div class="form-group">
      <label for="search">Search (with .trim + .lazy modifiers):</label>
      <input
        id="search"
        v-model.trim.lazy="searchQuery"
        placeholder="Search... (press Enter or click outside)"
        @change="performSearch"
      />
      <p>Search query: "{{ searchQuery }}" (trimmed, lazy updated)</p>

      <div v-if="isSearching" class="searching">
        Searching...
      </div>

      <ul v-else-if="searchResults.length > 0" class="search-results">
        <li v-for="result in searchResults" :key="result">
          {{ result }}
        </li>
      </ul>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Type safety** - otomatis konversi tipe data
- ✅ **Data cleaning** - otomatis trim whitespace
- ✅ **Performance optimization** - lazy updates untuk expensive operations
- ✅ **Input validation** - built-in data sanitization

### 4. **Complex Form Controls & Collections**
```vue
<script setup>
import { ref, reactive, computed } from 'vue'

// Fungsi 4: Handle complex form controls
const survey = reactive({
  // Multiple checkbox values
  interests: [],

  // Radio button selection
  experience: 'beginner',

  // Multiple select
  languages: [],

  // Single select
  country: '',

  // Nested form sections
  contact: {
    email: '',
    phone: '',
    preferredMethod: 'email'
  }
})

// Dynamic options
const availableInterests = [
  'Web Development', 'Mobile Development', 'Data Science',
  'AI/ML', 'DevOps', 'UI/UX Design', 'Blockchain'
]

const countries = [
  { code: 'id', name: 'Indonesia' },
  { code: 'us', name: 'United States' },
  { code: 'uk', name: 'United Kingdom' },
  { code: 'jp', name: 'Japan' }
]

const languages = [
  { code: 'en', name: 'English' },
  { code: 'id', name: 'Bahasa Indonesia' },
  { code: 'zh', name: 'Chinese' },
  { code: 'es', name: 'Spanish' }
]

// Computed properties
const selectedInterestsCount = computed(() => survey.interests.length)
const hasContactInfo = computed(() =>
  survey.contact.email || survey.contact.phone
)
const canSubmit = computed(() =>
  survey.interests.length > 0 &&
  survey.experience &&
  survey.country &&
  hasContactInfo.value
)

// Dynamic field management
const customSkills = ref([
  { id: 1, name: 'JavaScript', level: 5 },
  { id: 2, name: 'Vue.js', level: 4 }
])

const addSkill = () => {
  const newId = Math.max(...customSkills.value.map(s => s.id)) + 1
  customSkills.value.push({
    id: newId,
    name: '',
    level: 3
  })
}

const removeSkill = (id) => {
  const index = customSkills.value.findIndex(s => s.id === id)
  if (index > -1 && customSkills.value.length > 1) {
    customSkills.value.splice(index, 1)
  }
}
</script>

<template>
  <form class="complex-form">
    <h3>Technical Skills Survey</h3>

    <!-- Multiple Checkbox -->
    <fieldset>
      <legend>Areas of Interest (Select multiple)</legend>
      <div class="checkbox-grid">
        <label v-for="interest in availableInterests" :key="interest">
          <input
            type="checkbox"
            :value="interest"
            v-model="survey.interests"
          />
          {{ interest }}
        </label>
      </div>
      <p>Selected: {{ selectedInterestsCount }} interests</p>
    </fieldset>

    <!-- Radio Buttons -->
    <fieldset>
      <legend>Experience Level</legend>
      <div class="radio-group">
        <label v-for="level in ['beginner', 'intermediate', 'advanced', 'expert']" :key="level">
          <input
            type="radio"
            :value="level"
            v-model="survey.experience"
            name="experience"
          />
          {{ level.charAt(0).toUpperCase() + level.slice(1) }}
        </label>
      </div>
      <p>Current level: <strong>{{ survey.experience }}</strong></p>
    </fieldset>

    <!-- Select Dropdowns -->
    <fieldset>
      <legend>Location & Languages</legend>
      <div class="select-group">
        <label for="country">Country:</label>
        <select id="country" v-model="survey.country">
          <option value="">Select country</option>
          <option v-for="country in countries" :key="country.code" :value="country.code">
            {{ country.name }}
          </option>
        </select>
      </div>

      <div class="select-group">
        <label for="languages">Languages (Select multiple):</label>
        <select id="languages" v-model="survey.languages" multiple size="4">
          <option v-for="lang in languages" :key="lang.code" :value="lang.code">
            {{ lang.name }}
          </option>
        </select>
        <p>Selected: {{ survey.languages.map(code => languages.find(l => l.code === code)?.name).join(', ') }}</p>
      </div>
    </fieldset>

    <!-- Nested Form Controls -->
    <fieldset>
      <legend>Contact Information</legend>
      <div class="contact-form">
        <input v-model="survey.contact.email" type="email" placeholder="Email" />
        <input v-model="survey.contact.phone" placeholder="Phone" />

        <label>Preferred Contact Method:</label>
        <div class="radio-group">
          <label>
            <input type="radio" value="email" v-model="survey.contact.contactMethod" />
            Email
          </label>
          <label>
            <input type="radio" value="phone" v-model="survey.contact.contactMethod" />
            Phone
          </label>
        </div>

        <p class="contact-status">
          {{ hasContactInfo ? '✅ Contact info provided' : '❌ Please provide contact info' }}
        </p>
      </div>
    </fieldset>

    <!-- Dynamic Form Fields -->
    <fieldset>
      <legend>Skills Assessment</legend>
      <div class="dynamic-skills">
        <div v-for="skill in customSkills" :key="skill.id" class="skill-item">
          <input v-model="skill.name" placeholder="Skill name" />

          <label>Level:</label>
          <input
            v-model.number="skill.level"
            type="range"
            min="1"
            max="5"
            class="skill-range"
          />
          <span class="skill-level">{{ skill.level }}/5</span>

          <button
            type="button"
            @click="removeSkill(skill.id)"
            :disabled="customSkills.length === 1"
            class="remove-skill"
          >
            ×
          </button>
        </div>

        <button type="button" @click="addSkill" class="add-skill">
          + Add Skill
        </button>
      </div>
    </fieldset>

    <!-- Form Status -->
    <div class="form-summary">
      <h4>Form Summary:</h4>
      <ul>
        <li>Interests: {{ survey.interests.join(', ') || '(None selected)' }}</li>
        <li>Experience: {{ survey.experience }}</li>
        <li>Country: {{ countries.find(c => c.code === survey.country)?.name || '(Not selected)' }}</li>
        <li>Languages: {{ survey.languages.length }} language(s)</li>
        <li>Contact: {{ survey.contact[survey.contact.contactMethod] || '(Not provided)' }}</li>
        <li>Skills: {{ customSkills.length }} skill(s)</li>
      </ul>
    </div>

    <button type="submit" :disabled="!canSubmit">
      {{ canSubmit ? 'Submit Survey ✅' : 'Complete the form ❌' }}
    </button>
  </form>
</template>
```

**Keuntungan:**
- ✅ **Multiple input types** - checkbox, radio, select, range
- ✅ **Array binding** - multi-select dengan v-model array
- ✅ **Dynamic fields** - add/remove form elements
- ✅ **Nested structures** - complex form organization

### 5. **Real-Time Form Validation & User Experience**
```vue
<script setup>
import { ref, reactive, computed } from 'vue'

// Fungsi 5: Real-time validation dengan user feedback
const formData = reactive({
  username: '',
  email: '',
  password: '',
  confirmPassword: '',
  age: '',
  terms: false
})

// Validation states
const validation = reactive({
  username: {
    isValid: false,
    message: '',
    touched: false
  },
  email: {
    isValid: false,
    message: '',
    touched: false
  },
  password: {
    isValid: false,
    message: '',
    touched: false
  },
  confirmPassword: {
    isValid: false,
    message: '',
    touched: false
  },
  age: {
    isValid: false,
    message: '',
    touched: false
  }
})

// Real-time validation functions
const validateUsername = (username) => {
  validation.username.touched = true

  if (!username) {
    validation.username.isValid = false
    validation.username.message = 'Username is required'
    return
  }

  if (username.length < 3) {
    validation.username.isValid = false
    validation.username.message = 'Username must be at least 3 characters'
    return
  }

  if (!/^[a-zA-Z0-9_]+$/.test(username)) {
    validation.username.isValid = false
    validation.username.message = 'Username can only contain letters, numbers, and underscores'
    return
  }

  validation.username.isValid = true
  validation.username.message = '✅ Username is available'
}

const validateEmail = (email) => {
  validation.email.touched = true

  if (!email) {
    validation.email.isValid = false
    validation.email.message = 'Email is required'
    return
  }

  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  if (!emailRegex.test(email)) {
    validation.email.isValid = false
    validation.email.message = 'Please enter a valid email address'
    return
  }

  validation.email.isValid = true
  validation.email.message = '✅ Valid email format'
}

const validatePassword = (password) => {
  validation.password.touched = true

  if (!password) {
    validation.password.isValid = false
    validation.password.message = 'Password is required'
    return
  }

  if (password.length < 8) {
    validation.password.isValid = false
    validation.password.message = 'Password must be at least 8 characters'
    return
  }

  if (!/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/.test(password)) {
    validation.password.isValid = false
    validation.password.message = 'Password must contain uppercase, lowercase, and number'
    return
  }

  validation.password.isValid = true
  validation.password.message = '✅ Strong password'
}

const validateConfirmPassword = (confirmPassword) => {
  validation.confirmPassword.touched = true

  if (!confirmPassword) {
    validation.confirmPassword.isValid = false
    validation.confirmPassword.message = 'Please confirm your password'
    return
  }

  if (confirmPassword !== formData.password) {
    validation.confirmPassword.isValid = false
    validation.confirmPassword.message = 'Passwords do not match'
    return
  }

  validation.confirmPassword.isValid = true
  validation.confirmPassword.message = '✅ Passwords match'
}

const validateAge = (age) => {
  validation.age.touched = true

  if (!age) {
    validation.age.isValid = false
    validation.age.message = 'Age is required'
    return
  }

  const ageNum = Number(age)
  if (isNaN(ageNum) || ageNum < 18 || ageNum > 120) {
    validation.age.isValid = false
    validation.age.message = 'Age must be between 18 and 120'
    return
  }

  validation.age.isValid = true
  validation.age.message = '✅ Valid age'
}

// Watch untuk real-time validation
watch(() => formData.username, validateUsername)
watch(() => formData.email, validateEmail)
watch(() => formData.password, (newPassword) => {
  validatePassword(newPassword)
  if (formData.confirmPassword) {
    validateConfirmPassword(formData.confirmPassword)
  }
})
watch(() => formData.confirmPassword, validateConfirmPassword)
watch(() => formData.age, validateAge)

// Computed untuk form status
const isFormValid = computed(() => {
  return validation.username.isValid &&
         validation.email.isValid &&
         validation.password.isValid &&
         validation.confirmPassword.isValid &&
         validation.age.isValid &&
         formData.terms
})

const validationProgress = computed(() => {
  const fields = ['username', 'email', 'password', 'confirmPassword', 'age']
  const validFields = fields.filter(field => validation[field].isValid)
  return Math.round((validFields.length / fields.length) * 100)
})
</script>

<template>
  <form class="validated-form" @submit.prevent="console.log('Form submitted!')">
    <h3>Registration Form with Real-Time Validation</h3>

    <!-- Progress indicator -->
    <div class="progress-container">
      <div class="progress-bar">
        <div class="progress-fill" :style="{ width: validationProgress + '%' }"></div>
      </div>
      <p>Form Completion: {{ validationProgress }}%</p>
    </div>

    <!-- Username Field -->
    <div class="form-field" :class="{
      'valid': validation.username.isValid,
      'invalid': validation.username.touched && !validation.username.isValid
    }">
      <label for="username">Username:</label>
      <input
        id="username"
        v-model="formData.username"
        @input="validateUsername(formData.username)"
        placeholder="Enter username"
        class="form-input"
      />
      <div class="validation-message" v-if="validation.username.touched">
        {{ validation.username.message }}
      </div>
    </div>

    <!-- Email Field -->
    <div class="form-field" :class="{
      'valid': validation.email.isValid,
      'invalid': validation.email.touched && !validation.email.isValid
    }">
      <label for="email">Email:</label>
      <input
        id="email"
        v-model="formData.email"
        @input="validateEmail(formData.email)"
        type="email"
        placeholder="Enter email"
        class="form-input"
      />
      <div class="validation-message" v-if="validation.email.touched">
        {{ validation.email.message }}
      </div>
    </div>

    <!-- Password Field -->
    <div class="form-field" :class="{
      'valid': validation.password.isValid,
      'invalid': validation.password.touched && !validation.password.isValid
    }">
      <label for="password">Password:</label>
      <input
        id="password"
        v-model="formData.password"
        @input="validatePassword(formData.password)"
        type="password"
        placeholder="Enter password"
        class="form-input"
      />
      <div class="validation-message" v-if="validation.password.touched">
        {{ validation.password.message }}
      </div>
    </div>

    <!-- Confirm Password Field -->
    <div class="form-field" :class="{
      'valid': validation.confirmPassword.isValid,
      'invalid': validation.confirmPassword.touched && !validation.confirmPassword.isValid
    }">
      <label for="confirmPassword">Confirm Password:</label>
      <input
        id="confirmPassword"
        v-model="formData.confirmPassword"
        @input="validateConfirmPassword(formData.confirmPassword)"
        type="password"
        placeholder="Confirm password"
        class="form-input"
      />
      <div class="validation-message" v-if="validation.confirmPassword.touched">
        {{ validation.confirmPassword.message }}
      </div>
    </div>

    <!-- Age Field -->
    <div class="form-field" :class="{
      'valid': validation.age.isValid,
      'invalid': validation.age.touched && !validation.age.isValid
    }">
      <label for="age">Age:</label>
      <input
        id="age"
        v-model.number="formData.age"
        @input="validateAge(formData.age)"
        type="number"
        min="18"
        max="120"
        placeholder="Enter age"
        class="form-input"
      />
      <div class="validation-message" v-if="validation.age.touched">
        {{ validation.age.message }}
      </div>
    </div>

    <!-- Terms Checkbox -->
    <div class="form-field">
      <label class="checkbox-label">
        <input type="checkbox" v-model="formData.terms" />
        I agree to the terms and conditions
      </label>
    </div>

    <!-- Submit Button -->
    <button type="submit" :disabled="!isFormValid" class="submit-btn">
      {{ isFormValid ? 'Create Account ✅' : 'Complete the form ❌' }}
    </button>

    <!-- Form Status -->
    <div class="form-status" :class="{ valid: isFormValid }">
      {{ isFormValid ? '✅ Form is ready to submit!' : '⚠️ Please complete all required fields' }}
    </div>
  </form>
</template>
```

**Keuntungan:**
- ✅ **Real-time feedback** - validation saat user mengetik
- ✅ **User guidance** - pesan error yang jelas dan helpful
- ✅ **Progress tracking** - visual form completion indicator
- ✅ **Better UX** - prevent submission form yang invalid

## 🎯 Best Practices untuk Forms (v-model)

### 1. **Struktur Data yang Terorganisir**
```vue
<script setup>
// ✅ GOOD: Organized form structure
const userForm = reactive({
  personal: {
    firstName: '',
    lastName: '',
    email: ''
  },
  preferences: {
    newsletter: false,
    theme: 'light'
  }
})

// ❌ AVOID: Flat structure untuk complex forms
const formData = reactive({
  firstName: '',
  lastName: '',
  email: '',
  newsletter: false,
  theme: 'light'
})
</script>
```

### 2. **Gunakan Modifiers yang Tepat**
```vue
<template>
  <!-- ✅ GOOD: Gunakan modifiers yang sesuai -->
  <input v-model.number="age" type="number" />
  <input v-model.trim="username" />
  <textarea v-model.lazy="description" />
  <input v-model.trim.lazy="searchQuery" />

  <!-- ❌ AVOID: Tidak gunakan modifiers yang tidak perlu -->
  <input v-model.number="username" /> <!-- username seharusnya string -->
  <input v-model.trim="age" /> <!-- number seharusnya tidak di-trim -->
</template>
```

### 3. **Real-Time Validation**
```vue
<script setup>
// ✅ GOOD: Real-time validation dengan computed
const isEmailValid = computed(() => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
  return emailRegex.test(email.value)
})

const isFormValid = computed(() => {
  return username.value.length >= 3 && isEmailValid.value
})
</script>

<template>
  <!-- ✅ GOOD: Real-time feedback -->
  <input v-model="email" />
  <div v-if="email && !isEmailValid" class="error">
    Please enter a valid email
  </div>

  <button :disabled="!isFormValid">Submit</button>
</template>
```

## 🎉 Kesimpulan

Forms (v-model) adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Two-way binding** - sync otomatis antara form dan data
- ✅ **State management** - centralized form state organization
- ✅ **Type safety** - automatic type conversion dengan modifiers
- ✅ **Complex controls** - support untuk berbagai input types
- ✅ **Real-time validation** - instant user feedback dan UX improvement

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Form Input Bindings
- Vue.js Template Syntax Guide
- Vue.js Forms Best Practices

---

**🔥 Tips Pro:**
- Gunakan **reactive()** untuk form objects yang kompleks
- Manfaatkan **v-model modifiers** (.number, .trim, .lazy) untuk data quality
- Implement **real-time validation** untuk better UX
- Organisasi form data dalam **nested structure** untuk maintainability
- Gunakan **computed properties** untuk derived state dan validation logic

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Style Guide