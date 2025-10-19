# 📋 Props dan Validators - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja props dan validators untuk komunikasi antar komponen dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Props?](#-apa-itu-props)
- [🎮 Cara Menggunakan Props](#-cara-menggunakan-props)
- [⚙️ Static vs Dynamic Props](#️-static-vs-dynamic-props)
- [🔒 Props Immutability](#-props-immutability)
- [✅ Prop Validation](#-prop-validation)
- [🎯 Complex Props](#-complex-props)
- [🔧 Custom Validators](#-custom-validators)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Props?

**Props** (properties) adalah cara utama untuk mengirim data dari komponen parent ke komponen child dalam Vue.js. Props bersifat **one-way data flow** - data mengalir dari atas ke bawah, tidak bisa diubah dari child ke parent.

### Konsep Dasar Props

```vue
<!-- Parent Component -->
<template>
  <ChildComponent message="Hello World!" />
</template>

<!-- Child Component -->
<script setup>
const props = defineProps(['message'])
</script>

<template>
  <p>{{ props.message }}</p>
</template>
```

### Mengapa Props Penting?

1. **🔄 Reusability:** Komponen dapat digunakan kembali dengan data berbeda
2. **📊 Komunikasi:** Cara aman untuk berkomunikasi antar komponen
3. **🎯 Prediktabilitas:** Data flow yang jelas dan terstruktur
4. **🛡️ Keamanan:** Child tidak bisa mengubah data parent secara tidak sengaja

## 🎮 Cara Menggunakan Props

### 1. Definisi Props Dasar

```vue
<script setup>
// Cara 1: Array syntax (sederhana)
const props = defineProps(['name', 'age', 'email'])

// Cara 2: Object syntax (dengan validasi)
const props = defineProps({
  name: String,
  age: Number,
  email: String
})
</script>
```

### 2. Mengakses Props dalam Template

```vue
<template>
  <!-- Langsung menggunakan nama prop -->
  <h1>{{ name }}</h1>
  <p>Age: {{ age }}</p>

  <!-- Atau dengan props prefix -->
  <h1>{{ props.name }}</h1>
  <p>Age: {{ props.age }}</p>
</template>
```

### 3. Mengakses Props dalam Script

```vue
<script setup>
const props = defineProps(['name', 'age'])

// Dalam JavaScript, selalu gunakan props prefix
console.log(props.name)     // ✅ Benar
console.log(props.age)      // ✅ Benar
console.log(name)           // ❌ Error (undefined dalam script)
</script>
```

## ⚙️ Static vs Dynamic Props

### Static Props

Static props adalah nilai yang ditulis langsung sebagai string dalam template.

```vue
<template>
  <!-- Static props - nilai tetap -->
  <UserCard name="John Doe" age="25" city="Jakarta" />
</template>
```

**✅ Static Props cocok untuk:**
- Data yang tidak berubah
- Konfigurasi default
- Label teks statis

### Dynamic Props

Dynamic props menggunakan v-bind (:) untuk mengirim data dari variabel atau expression.

```vue
<script setup>
import { ref } from 'vue'

const userName = ref('Alex')
const userAge = ref(20)
const isActive = ref(true)
</script>

<template>
  <!-- Dynamic props - dari variabel -->
  <UserCard :name="userName" :age="userAge" :active="isActive" />

  <!-- Dynamic props - dari expression -->
  <UserCard :name="user.firstName + ' ' + user.lastName" />
  <UserCard :age="2024 - user.birthYear" />
</template>
```

**✅ Dynamic Props cocok untuk:**
- Data yang berubah (reactive)
- Perhitungan atau expression
- Data dari API atau database

## 🔒 Props Immutability

Props bersifat **immutable** (tidak bisa diubah) dari dalam komponen child.

### ❌ JANGAN Melakukan Ini:

```vue
<script setup>
const props = defineProps(['name', 'age'])

// ERROR: Props tidak bisa diubah langsung!
props.name = 'New Name'     // ❌ Tidak boleh
props.age = 25             // ❌ Tidak boleh
</script>
```

### ✅ LAKUKAN Ini:

```vue
<script setup>
import { ref, computed } from 'vue'

const props = defineProps(['name', 'age'])

// 1. Buat data lokal jika perlu modifikasi
const localName = ref(props.name)

// 2. Gunakan computed untuk turunan data
const displayName = computed(() => {
  return props.name.toUpperCase()
})

// 3. Emit event untuk meminta parent mengubah data
const updateName = (newName) => {
  emit('update-name', newName)
}
</script>
```

## ✅ Prop Validation

Prop validation memastikan props yang diterima komponen sesuai dengan yang diharapkan.

### Basic Validation

```vue
<script setup>
// Validasi tipe data
defineProps({
  name: String,        // Harus string
  age: Number,         // Harus number
  isActive: Boolean,   // Harus boolean
  tags: Array,         // Harus array
  profile: Object      // Harus object
})
</script>
```

### Advanced Validation

```vue
<script setup>
defineProps({
  // Prop wajib
  name: {
    type: String,
    required: true
  },

  // Prop dengan default value
  age: {
    type: Number,
    default: 0
  },

  // Default value untuk object/array
  userInfo: {
    type: Object,
    default: () => ({
      name: 'Guest',
      role: 'user'
    })
  },

  // Multiple type options
  value: {
    type: [String, Number],
    default: ''
  },

  // Custom validator
  email: {
    type: String,
    validator: (value) => {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)
    }
  }
})
</script>
```

### Built-in Type Validators

Vue.js menyediakan beberapa built-in types:

- `String` - Tipe data string
- `Number` - Tipe data number
- `Boolean` - Tipe data boolean
- `Array` - Tipe data array
- `Object` - Tipe data object
- `Date` - Tipe data date
- `Function` - Tipe data function
- `Symbol` - Tipe data symbol

## 🎯 Complex Props

Props bisa menerima tipe data kompleks seperti array dan object.

### Array Props

```vue
<!-- Parent Component -->
<script setup>
const friends = ['Alex', 'John', 'Jordan', 'Sarah']
const skills = ['JavaScript', 'Vue.js', 'CSS', 'HTML']
</script>

<template>
  <UserProfile :friends="friends" :skills="skills" />
</template>

<!-- Child Component -->
<script setup>
defineProps({
  friends: Array,
  skills: Array
})
</script>

<template>
  <div>
    <h3>Friends:</h3>
    <ul v-for="(friend, index) in friends" :key="index">
      <li>{{ friend }}</li>
    </ul>

    <h3>Skills:</h3>
    <div v-for="(skill, index) in skills" :key="index">
      <span class="skill-tag">{{ skill }}</span>
    </div>
  </div>
</template>
```

### Object Props

```vue
<!-- Parent Component -->
<script setup>
const userInfo = {
  name: 'Alex Johnson',
  age: 25,
  email: 'alex@example.com',
  address: {
    street: 'Jl. Sudirman 123',
    city: 'Jakarta',
    country: 'Indonesia'
  },
  hobbies: ['Coding', 'Reading', 'Gaming']
}
</script>

<template>
  <UserCard :user-info="userInfo" />
</template>

<!-- Child Component -->
<script setup>
defineProps({
  userInfo: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class="user-card">
    <h2>{{ userInfo.name }}</h2>
    <p>Age: {{ userInfo.age }}</p>
    <p>Email: {{ userInfo.email }}</p>

    <h4>Address:</h4>
    <p>{{ userInfo.address.street }}</p>
    <p>{{ userInfo.address.city }}, {{ userInfo.address.country }}</p>

    <h4>Hobbies:</h4>
    <div v-for="hobby in userInfo.hobbies" :key="hobby">
      {{ hobby }}
    </div>
  </div>
</template>
```

### Nested Object Validation

```vue
<script setup>
defineProps({
  // Nested object validation
  user: {
    type: Object,
    required: true,
    validator: (value) => {
      return (
        typeof value.name === 'string' &&
        typeof value.age === 'number' &&
        value.age >= 0 &&
        Array.isArray(value.hobbies)
      )
    }
  }
})
</script>
```

## 🔧 Custom Validators

Custom validators memungkinkan kita membuat aturan validasi yang kompleks.

### Basic Custom Validator

```vue
<script setup>
defineProps({
  // Validasi nama yang diizinkan
  name: {
    type: String,
    validator: (value) => {
      // Hanya izinkan nama tertentu
      return ['HuXn', 'Jordan', 'John', 'Sarah'].includes(value)
    }
  },

  // Validasi rentang umur
  age: {
    type: Number,
    validator: (value) => {
      // Umur harus antara 18 dan 65
      return value >= 18 && value <= 65
    }
  },

  // Validasi password strength
  password: {
    type: String,
    validator: (value) => {
      // Password minimal 8 karakter
      return value.length >= 8
    }
  }
})
</script>
```

### Advanced Custom Validators

```vue
<script setup>
defineProps({
  // Validasi email dengan regex
  email: {
    type: String,
    validator: (value) => {
      const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      return emailRegex.test(value)
    }
  },

  // Validasi URL
  website: {
    type: String,
    validator: (value) => {
      try {
        new URL(value)
        return true
      } catch {
        return false
      }
    }
  },

  // Validasi array dengan kondisi tertentu
  tags: {
    type: Array,
    validator: (value) => {
      // Maksimal 5 tags
      if (value.length > 5) return false

      // Setiap tag harus string dan tidak kosong
      return value.every(tag =>
        typeof tag === 'string' && tag.trim().length > 0
      )
    }
  },

  // Validasi object dengan struktur tertentu
  address: {
    type: Object,
    validator: (value) => {
      const requiredFields = ['street', 'city', 'zipCode']
      return requiredFields.every(field => field in value)
    }
  }
})
</script>
```

### Validator dengan Pesan Error

Karena custom validators tidak support pesan error kustom, kita bisa menggunakan computed properties untuk validasi tambahan:

```vue
<script setup>
import { computed } from 'vue'

const props = defineProps({
  username: {
    type: String,
    validator: (value) => {
      // Basic validation
      return value.length >= 3
    }
  }
})

// Additional validation with custom error messages
const usernameError = computed(() => {
  if (props.username.length < 3) {
    return 'Username minimal 3 karakter'
  }
  if (!/^[a-zA-Z0-9_]+$/.test(props.username)) {
    return 'Username hanya boleh mengandung huruf, angka, dan underscore'
  }
  return null
})
</script>

<template>
  <div>
    <input v-model="username" />
    <p v-if="usernameError" class="error">
      {{ usernameError }}
    </p>
  </div>
</template>
```

## 🚀 Best Practices

### 1. Naming Conventions

```vue
<!-- ✅ Gunakan camelCase untuk nama props di script -->
<script setup>
defineProps({
  userName: String,
  isActive: Boolean,
  userAge: Number
})
</script>

<!-- ✅ Gunakan kebab-case saat passing props -->
<template>
  <UserProfile
    :user-name="userName"
    :is-active="isActive"
    :user-age="userAge"
  />
</template>
```

### 2. Default Values

```vue
<script setup>
defineProps({
  // ✅ Berikan default value yang reasonable
  title: {
    type: String,
    default: 'Untitled'
  },

  // ✅ Gunakan function untuk object/array default
  config: {
    type: Object,
    default: () => ({
      theme: 'light',
      language: 'en'
    })
  },

  // ✅ Required untuk props penting
  id: {
    type: String,
    required: true
  }
})
</script>
```

### 3. Type Safety

```vue
<script setup>
// ✅ Selalu tentukan tipe data
defineProps({
  count: Number,           // Spesifik
  status: Boolean,         // Jelas
  items: Array,           // Untuk collection

  // ✅ Multiple types jika diperlukan
  value: [String, Number],

  // ❌ Hindari tanpa tipe
  data: null  // Kurang jelas
})
</script>
```

### 4. Props vs Data

```vue
<script setup>
import { ref } from 'vue'

const props = defineProps({
  initialValue: {
    type: String,
    default: ''
  }
})

// ✅ Gunakan props untuk data dari parent
// ✅ Gunakan ref/emits untuk data lokal yang bisa berubah
const localValue = ref(props.initialValue)

// ✅ Emit event untuk mengkomunikasikan perubahan
const emit = defineEmits(['update'])

const handleChange = () => {
  emit('update', localValue.value)
}
</script>
```

## 💡 Tips dan Trik

### 1. Props Destructuring

```vue
<script setup>
// ✅ Destucturing props untuk kemudahan akses
const { name, age, email } = defineProps({
  name: String,
  age: Number,
  email: String
})

// Gunakan langsung tanpa props prefix
console.log(name)    // Bisa diakses langsung
console.log(age)     // Bisa diakses langsung
</script>
```

### 2. Reactive Props

```vue
<script setup>
import { watch } from 'vue'

const props = defineProps(['userId'])

// ✅ Watch perubahan props
watch(() => props.userId, (newId, oldId) => {
  console.log(`User ID changed from ${oldId} to ${newId}`)
  // Load data user baru
})
</script>
```

### 3. Props Validation Debug

```vue
<script setup>
// ✅ Debug props development
const props = defineProps({
  data: Object
})

// Log props untuk debugging (hanya di development)
if (process.env.NODE_ENV === 'development') {
  console.log('Props received:', props)
}
</script>
```

### 4. Type Props dengan TypeScript

```vue
<script setup lang="ts">
interface User {
  id: number
  name: string
  email: string
  role: 'admin' | 'user' | 'guest'
}

interface Props {
  user: User
  isLoading?: boolean
  onUpdate: (user: User) => void
}

defineProps<Props>()
</script>
```

### 5. Fallback Values

```vue
<script setup>
const props = defineProps({
  primaryColor: {
    type: String,
    default: '#007bff'
  },

  fontSize: {
    type: [String, Number],
    default: '16px'
  }
})

// ✅ Computed untuk fallback values
const computedStyles = computed(() => ({
  color: props.primaryColor || '#333',
  fontSize: typeof props.fontSize === 'number'
    ? `${props.fontSize}px`
    : props.fontSize
}))
</script>
```

---

## 🎉 Kesimpulan

Props dan validators adalah fitur fundamental dalam Vue.js yang memungkinkan:

1. **📡 Komunikasi Antar Komponen** - Cara aman untuk mengirim data
2. **🔍 Validasi Data** - Memastikan data yang diterima sesuai harapan
3. **🔄 Reusability** - Komponen dapat digunakan dengan berbagai data
4. **🛡️ Type Safety** - Mencegah bug dengan validasi tipe data
5. **📚 Maintainability** - Code yang lebih terstruktur dan mudah dipahami

### Poin Penting:

- Props bersifat **one-way** dari parent ke child
- Props **immutable** - tidak bisa diubah dari child
- Selalu gunakan **prop validation** untuk aplikasi production
- Berikan **default values** yang reasonable
- Gunakan **custom validators** untuk validasi kompleks

### Lanjutan:

- Component `emits` untuk komunikasi child ke parent
- Provide/inject untuk dependency injection
- Props untuk component composition patterns
- TypeScript untuk type safety lebih baik

Dengan memahami props dan validators, Anda sudah bisa membuat komponen Vue.js yang robust, reusable, dan maintainable! 🚀

---

**🎯 Ready for Practice?** Coba buat komponen-komponen dengan berbagai jenis props dan validasi untuk memperkuat pemahaman Anda!

## ❓ Pertanyaan: Apa Fungsi Props and Validators?

Props and Validators memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk komunikasi antar komponen:

### 1. **Data Flow Management (One-Way Communication)**
```vue
<!-- ParentComponent.vue -->
<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

// Fungsi 1: Mengelola aliran data dari parent ke child
const user = ref({
  name: 'John Doe',
  age: 25,
  email: 'john@example.com',
  role: 'developer'
})

const theme = ref('light')
const settings = ref({
  notifications: true,
  autoSave: false,
  fontSize: 16
})

const updateUser = () => {
  // Parent mengubah data, child otomatis ter-update
  user.value.age++
}
</script>

<template>
  <div class="parent-component">
    <h1>Parent Component</h1>
    <button @click="updateUser">Update User Age</button>

    <!-- Data mengalir dari parent ke child secara otomatis -->
    <ChildComponent
      :user-data="user"
      :theme="theme"
      :settings="settings"
    />
  </div>
</template>

<!-- ChildComponent.vue -->
<script setup>
// Child menerima data dari parent
const props = defineProps({
  userData: {
    type: Object,
    required: true
  },
  theme: {
    type: String,
    default: 'light'
  },
  settings: {
    type: Object,
    default: () => ({})
  }
})

// Data props otomatis reactive - perubahan di parent langsung terlihat di child
const userDisplayName = computed(() =>
  `${props.userData.name} (${props.userData.age} years old)`
)
</script>

<template>
  <div class="child-component" :class="theme">
    <h2>Child Component</h2>
    <p>{{ userDisplayName }}</p>
    <p>Email: {{ userData.email }}</p>
    <p>Role: {{ userData.role }}</p>

    <!-- Settings dari parent -->
    <div v-if="settings.notifications" class="notification">
      🔔 Notifications enabled
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Predictable data flow** - data selalu mengalir dari atas ke bawah
- ✅ **Reactive updates** - perubahan di parent otomatis terlihat di child
- ✅ **Centralized state** - state management yang terorganisir
- ✅ **Debugging friendly** - mudah trace sumber data

### 2. **Component Reusability & Customization**
```vue
<!-- CardComponent.vue - Komponen yang sangat reusable -->
<script setup>
const props = defineProps({
  title: {
    type: String,
    required: true
  },
  content: {
    type: String,
    default: 'No content provided'
  },
  variant: {
    type: String,
    default: 'primary',
    validator: (value) => ['primary', 'secondary', 'success', 'warning'].includes(value)
  },
  isCollapsible: {
    type: Boolean,
    default: false
  },
  showFooter: {
    type: Boolean,
    default: true
  },
  footerText: {
    type: String,
    default: ''
  },
  badgeCount: {
    type: Number,
    default: 0
  }
})

const isExpanded = ref(true)
const toggleCollapse = () => {
  isExpanded.value = !isExpanded.value
}

const cardClasses = computed(() => ({
  'card': true,
  [`card-${props.variant}`]: true,
  'card-collapsible': props.isCollapsible
}))
</script>

<template>
  <div :class="cardClasses">
    <div class="card-header">
      <h3>{{ title }}</h3>
      <span v-if="badgeCount > 0" class="badge">{{ badgeCount }}</span>
      <button
        v-if="isCollapsible"
        @click="toggleCollapse"
        class="collapse-btn"
      >
        {{ isExpanded ? '▼' : '▲' }}
      </button>
    </div>

    <div v-if="isExpanded || !isCollapsible" class="card-body">
      <p>{{ content }}</p>
      <slot><!-- Additional content --></slot>
    </div>

    <div v-if="showFooter && (footerText || $slots.footer)" class="card-footer">
      <slot name="footer">
        <span>{{ footerText }}</span>
      </slot>
    </div>
  </div>
</template>

<!-- Menggunakan CardComponent dengan berbagai konfigurasi -->
<script setup>
import CardComponent from './CardComponent.vue'
</script>

<template>
  <!-- Primary card dengan badge -->
  <CardComponent
    title="User Profile"
    content="This is user profile information"
    variant="primary"
    :badge-count="5"
  />

  <!-- Secondary card yang collapsible -->
  <CardComponent
    title="Settings"
    content="Application settings"
    variant="secondary"
    :is-collapsible="true"
    show-footer
    footer-text="Last updated: 2 days ago"
  />

  <!-- Success card tanpa footer -->
  <CardComponent
    title="Success Message"
    content="Operation completed successfully"
    variant="success"
    :show-footer="false"
  />

  <!-- Warning card dengan custom footer -->
  <CardComponent
    title="Warning"
    content="Please review this information"
    variant="warning"
    show-footer
  >
    <template #footer>
      <button>Dismiss</button>
      <button>Review</button>
    </template>
  </CardComponent>
</template>
```

**Keuntungan:**
- ✅ **Highly reusable** - satu komponen untuk berbagai use cases
- ✅ **Configurable** - berbagai opsi konfigurasi
- ✅ **Consistent UI** - design yang seragam dengan variasi
- ✅ **Easy maintenance** - perubahan di satu tempat affect semua usage

### 3. **Data Validation & Type Safety**
```vue
<!-- UserProfile.vue - Komponen dengan validasi ketat -->
<script setup>
const props = defineProps({
  // Validasi tipe data yang ketat
  user: {
    type: Object,
    required: true,
    validator: (user) => {
      // Validasi struktur object
      if (!user.id || typeof user.id !== 'string') return false
      if (!user.name || typeof user.name !== 'string') return false
      if (!user.email || typeof user.email !== 'string') return false
      if (typeof user.age !== 'number' || user.age < 0 || user.age > 150) return false
      if (!Array.isArray(user.skills) || user.skills.length === 0) return false
      return true
    }
  },

  // Validasi enum values
  role: {
    type: String,
    default: 'user',
    validator: (role) => ['admin', 'moderator', 'user', 'guest'].includes(role)
  },

  // Validasi array dengan kondisi kompleks
  permissions: {
    type: Array,
    default: () => [],
    validator: (permissions) => {
      // Maksimal 10 permissions
      if (permissions.length > 10) return false

      // Setiap permission harus string valid
      const validPermissions = ['read', 'write', 'delete', 'admin', 'moderate']
      return permissions.every(perm =>
        typeof perm === 'string' && validPermissions.includes(perm)
      )
    }
  },

  // Validasi date
  lastLogin: {
    type: [String, Date],
    validator: (value) => {
      if (!value) return true // Optional

      const date = value instanceof Date ? value : new Date(value)
      return !isNaN(date.getTime()) && date <= new Date()
    }
  },

  // Validasi numeric dengan range
  activityScore: {
    type: Number,
    default: 0,
    validator: (score) => score >= 0 && score <= 100
  }
})

// Computed untuk validasi tambahan dengan error messages
const validationErrors = computed(() => {
  const errors = []

  if (props.user.name.length < 2) {
    errors.push('User name must be at least 2 characters')
  }

  if (props.user.age < 18) {
    errors.push('User must be at least 18 years old')
  }

  if (props.permissions.length > 5) {
    errors.push('Too many permissions assigned')
  }

  return errors
})

const isValidProfile = computed(() => validationErrors.value.length === 0)
</script>

<template>
  <div class="user-profile" :class="{ invalid: !isValidProfile }">
    <h2>{{ user.name }}</h2>
    <p>Role: {{ role }}</p>
    <p>Age: {{ user.age }}</p>
    <p>Activity Score: {{ activityScore }}/100</p>

    <!-- Error messages jika validasi gagal -->
    <div v-if="!isValidProfile" class="validation-errors">
      <h4>⚠️ Validation Issues:</h4>
      <ul>
        <li v-for="error in validationErrors" :key="error">{{ error }}</li>
      </ul>
    </div>

    <!-- Success state jika validasi berhasil -->
    <div v-else class="validation-success">
      ✅ Profile is valid and complete
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Type safety** - mencegah bugs dari tipe data yang salah
- ✅ **Data integrity** - memastikan data yang diterima valid
- ✅ **Early error detection** - error terdeteksi di development time
- ✅ **Self-documenting** - props definition mendeskripsikan expected data

### 4. **Component Composition & API Design**
```vue
<!-- DataTable.vue - Komponen yang dapat dikomposisi -->
<script setup>
const props = defineProps({
  // Data yang akan ditampilkan
  data: {
    type: Array,
    required: true,
    validator: (data) => Array.isArray(data) && data.length > 0
  },

  // Konfigurasi kolom
  columns: {
    type: Array,
    required: true,
    validator: (columns) => {
      return columns.every(col =>
        col.key &&
        col.label &&
        typeof col.key === 'string' &&
        typeof col.label === 'string'
      )
    }
  },

  // Konfigurasi fitur
  features: {
    type: Object,
    default: () => ({
      sortable: true,
      filterable: true,
      pagination: true,
      selectable: false,
      exportable: false
    })
  },

  // Styling options
  size: {
    type: String,
    default: 'medium',
    validator: (size) => ['small', 'medium', 'large'].includes(size)
  },

  // Callback functions
  onRowClick: {
    type: Function,
    default: () => {}
  },

  onSort: {
    type: Function,
    default: () => {}
  },

  onFilter: {
    type: Function,
    default: () => {}
  }
})

// Internal state management
const currentPage = ref(1)
const sortBy = ref('')
const sortDirection = ref('asc')
const filters = ref({})
const selectedRows = ref([])

// Computed properties untuk data processing
const processedData = computed(() => {
  let result = [...props.data]

  // Apply filters
  Object.entries(filters.value).forEach(([key, value]) => {
    if (value) {
      result = result.filter(item =>
        String(item[key]).toLowerCase().includes(value.toLowerCase())
      )
    }
  })

  // Apply sorting
  if (sortBy.value) {
    result.sort((a, b) => {
      const aVal = a[sortBy.value]
      const bVal = b[sortBy.value]
      const modifier = sortDirection.value === 'asc' ? 1 : -1
      return (aVal > bVal ? 1 : -1) * modifier
    })
  }

  return result
})

// Event handlers
const handleRowClick = (row, index) => {
  props.onRowClick(row, index)

  if (props.features.selectable) {
    const selectedIndex = selectedRows.value.findIndex(item => item.id === row.id)
    if (selectedIndex > -1) {
      selectedRows.value.splice(selectedIndex, 1)
    } else {
      selectedRows.value.push(row)
    }
  }
}

const handleSort = (column) => {
  if (sortBy.value === column.key) {
    sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortBy.value = column.key
    sortDirection.value = 'asc'
  }
  props.onSort(column.key, sortDirection.value)
}
</script>

<template>
  <div class="data-table" :class="`table-${size}`">
    <!-- Table controls -->
    <div v-if="features.filterable || features.exportable" class="table-controls">
      <input
        v-if="features.filterable"
        v-model="filters.search"
        placeholder="Search..."
        class="search-input"
      />

      <button v-if="features.exportable" @click="exportData">
        Export Data
      </button>
    </div>

    <!-- Table header -->
    <table>
      <thead>
        <tr>
          <th v-if="features.selectable">Select</th>
          <th
            v-for="column in columns"
            :key="column.key"
            @click="features.sortable ? handleSort(column) : null"
            :class="{ sortable: features.sortable }"
          >
            {{ column.label }}
            <span v-if="sortBy === column.key" class="sort-indicator">
              {{ sortDirection === 'asc' ? '↑' : '↓' }}
            </span>
          </th>
        </tr>
      </thead>

      <!-- Table body -->
      <tbody>
        <tr
          v-for="(row, index) in processedData"
          :key="row.id || index"
          @click="handleRowClick(row, index)"
          :class="{ selected: selectedRows.includes(row) }"
        >
          <td v-if="features.selectable">
            <input
              type="checkbox"
              :checked="selectedRows.includes(row)"
              @change.stop
            />
          </td>
          <td v-for="column in columns" :key="column.key">
            {{ row[column.key] }}
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Pagination -->
    <div v-if="features.pagination" class="pagination">
      <button :disabled="currentPage === 1" @click="currentPage--">
        Previous
      </button>
      <span>Page {{ currentPage }}</span>
      <button @click="currentPage++">
        Next
      </button>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Flexible API** - komponen dapat dikonfigurasi untuk berbagai kebutuhan
- ✅ **Composable** - dapat dikombinasikan dengan komponen lain
- ✅ **Extensible** - mudah menambah fitur baru
- ✅ **Clear contract** - API yang jelas untuk developer

### 5. **Development Experience & Debugging**
```vue
<!-- EnhancedUserCard.vue - Komponen dengan excellent developer experience -->
<script setup>
const props = defineProps({
  // Required props dengan deskripsi jelas
  user: {
    type: Object,
    required: true,
    validator: (user) => {
      if (!user.id) {
        console.warn('[UserCard] User object must have an id property')
        return false
      }
      if (!user.name) {
        console.warn('[UserCard] User object must have a name property')
        return false
      }
      return true
    }
  },

  // Optional props dengan default yang reasonable
  showDetails: {
    type: Boolean,
    default: true
  },

  theme: {
    type: String,
    default: 'default',
    validator: (theme) => ['default', 'dark', 'colorful'].includes(theme)
  },

  // Advanced props untuk debugging
  debug: {
    type: Boolean,
    default: false
  },

  // Callback untuk parent communication
  onUpdate: {
    type: Function,
    default: () => {}
  }
})

// Debug logging di development mode
if (props.debug || process.env.NODE_ENV === 'development') {
  console.log('[UserCard] Props received:', props)
  console.log('[UserCard] User data:', JSON.stringify(props.user, null, 2))
}

// Computed dengan error handling yang baik
const userDisplayName = computed(() => {
  try {
    return props.user.name || 'Unknown User'
  } catch (error) {
    console.error('[UserCard] Error getting user name:', error)
    return 'Error loading name'
  }
})

const userAge = computed(() => {
  const age = props.user.age
  if (typeof age !== 'number') {
    console.warn('[UserCard] Invalid age value:', age)
    return 'N/A'
  }
  return age
})

// Watch untuk debugging perubahan props
watch(() => props.user, (newUser, oldUser) => {
  if (props.debug) {
    console.log('[UserCard] User changed:', { old: oldUser, new: newUser })
  }
  props.onUpdate(newUser)
}, { deep: true })

// Error boundary simulation
const hasError = ref(false)
const errorMessage = ref('')

const handleCardClick = () => {
  try {
    // Simulate some operation
    if (!props.user.id) {
      throw new Error('User ID is required')
    }
    console.log('[UserCard] Card clicked for user:', props.user.id)
  } catch (error) {
    hasError.value = true
    errorMessage.value = error.message
    console.error('[UserCard] Error:', error)
  }
}
</script>

<template>
  <div class="user-card" :class="theme" @click="handleCardClick">
    <!-- Error display -->
    <div v-if="hasError" class="error-state">
      <h3>⚠️ Error</h3>
      <p>{{ errorMessage }}</p>
      <button @click.stop="hasError = false">Retry</button>
    </div>

    <!-- Normal state -->
    <div v-else>
      <!-- Debug info di development mode -->
      <div v-if="debug" class="debug-info">
        <h4>Debug Info:</h4>
        <pre>{{ JSON.stringify(user, null, 2) }}</pre>
        <p>Component: UserCard</p>
        <p>Props: {{ Object.keys($props).join(', ') }}</p>
      </div>

      <!-- User content -->
      <div class="user-avatar">
        <img
          :src="`https://picsum.photos/seed/${user.id}/100/100.jpg`"
          :alt="userDisplayName"
          @error="console.error('[UserCard] Failed to load avatar')"
        />
      </div>

      <div class="user-info">
        <h3>{{ userDisplayName }}</h3>

        <div v-if="showDetails" class="user-details">
          <p>Age: {{ userAge }}</p>
          <p v-if="user.email">Email: {{ user.email }}</p>
          <p v-if="user.role">Role: {{ user.role }}</p>
        </div>
      </div>

      <!-- Dev tools button di development mode -->
      <button
        v-if="process.env.NODE_ENV === 'development'"
        @click.stop="$emit('toggle-debug')"
        class="debug-toggle"
      >
        {{ debug ? 'Hide Debug' : 'Show Debug' }}
      </button>
    </div>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Better debugging** - tools untuk memudahkan development
- ✅ **Error boundaries** - graceful error handling
- ✅ **Development warnings** - helpful warning messages
- ✅ **Console logging** - detailed logging untuk troubleshooting

## 🎯 Best Practices untuk Props and Validators

### 1. **Always Use Props Validation**
```vue
<script setup>
// ✅ GOOD: Selalu validasi props
const props = defineProps({
  user: {
    type: Object,
    required: true,
    validator: (user) => user.id && user.name
  }
})

// ❌ AVOID: Props tanpa validasi
const props = defineProps(['user'])
</script>
```

### 2. **Provide Meaningful Defaults**
```vue
<script setup>
// ✅ GOOD: Default yang meaningful
const props = defineProps({
  theme: {
    type: String,
    default: 'light'
  },
  items: {
    type: Array,
    default: () => []
  }
})

// ❌ AVOID: Default yang tidak helpful
const props = defineProps({
  theme: {
    type: String,
    default: null
  }
})
</script>
```

### 3. **Use Props for Data, Emits for Events**
```vue
<script setup>
// ✅ GOOD: Props untuk data, emits untuk events
const props = defineProps(['value'])
const emit = defineEmits(['update'])

const handleChange = () => {
  emit('update', newValue)
}

// ❌ AVOID: Mengubah props langsung
const props = defineProps(['value'])

const handleChange = () => {
  props.value = newValue // Error!
}
</script>
```

## 🎉 Kesimpulan

Props and Validators adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Structured data flow** - komunikasi yang terorganisir antar komponen
- ✅ **Component reusability** - komponen yang dapat digunakan kembali
- ✅ **Type safety** - validasi data yang ketat dan reliable
- ✅ **Flexible composition** - API yang dapat dikonfigurasi
- ✅ **Better DX** - development experience yang menyenangkan

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Props
- Vue.js Component API Guide
- Vue.js Style Guide

---

**🔥 Tips Pro:**
- Selalu gunakan **prop validation** untuk aplikasi production
- Berikan **default values** yang reasonable dan meaningful
- Gunakan **custom validators** untuk validasi data kompleks
- Manfaatkan **props destructuring** untuk kode yang lebih clean
- Tambahkan **debug logging** untuk kemudahan troubleshooting

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Composition API Documentation