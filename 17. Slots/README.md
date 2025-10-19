# 🎰 Slots - Panduan Lengkap Vue.js

> **🎯 Tujuan:** Memahami cara kerja slots untuk membuat komponen yang fleksibel dan reusable dalam Vue.js dengan pendekatan yang mudah dipahami untuk pemula.

## 📚 Daftar Isi

- [🔍 Apa itu Slots?](#-apa-itu-slots)
- [🎮 Cara Menggunakan Slots](#-cara-menggunakan-slots)
- [🎯 Default Slots](#-default-slots)
- [🏷️ Named Slots](#️-named-slots)
- [🎪 Fallback Content](#-fallback-content)
- [🔧 Scoped Slots](#-scoped-slots)
- [💫 Dynamic Slots](#-dynamic-slots)
- [🚀 Best Practices](#-best-practices)
- [💡 Tips dan Trik](#-tips-dan-trik)

## 🔍 Apa itu Slots?

**Slots** adalah mekanisme dalam Vue.js yang memungkinkan parent component untuk menyisipkan konten ke dalam child component. Slots membuat komponen menjadi lebih fleksibel dan reusable karena konten dapat ditentukan saat penggunaan.

### Konsep Dasar Slots

```vue
<!-- Parent Component menyisipkan konten -->
<ChildComponent>
  <h1>Konten ini akan masuk ke slot</h1>
  <p>Dan ini juga</p>
</ChildComponent>

<!-- Child Component menyediakan slot -->
<script setup>
</script>

<template>
  <div class="card">
    <h2>Card Title</h2>
    <slot> <!-- Konten dari parent akan muncul di sini --> </slot>
  </div>
</template>
```

### Mengapa Slots Penting?

1. **🔄 Flexibility:** Komponen dapat digunakan dengan konten berbeda
2. **📦 Reusability:** Satu komponen untuk banyak use case
3. **🎯 Composition:** Membangun UI dari komponen-komponen kecil
4. **🛡️ Separation of Concerns:** Memisahkan struktur dan konten

## 🎮 Cara Menggunakan Slots

### 1. Basic Slot Usage

```vue
<!-- ParentComponent.vue -->
<script setup>
import ChildComponent from './ChildComponent.vue'
</script>

<template>
  <ChildComponent>
    <h1>Hello from Parent!</h1>
    <p>This content is passed via slot.</p>
  </ChildComponent>
</template>

<!-- ChildComponent.vue -->
<script setup>
</script>

<template>
  <div class="container">
    <h2>Child Component</h2>
    <div class="slot-content">
      <slot></slot> <!-- Slot outlet -->
    </div>
  </div>
</template>

<style scoped>
.container {
  border: 2px solid #42b883;
  padding: 20px;
  border-radius: 8px;
}

.slot-content {
  background: #f0f9ff;
  padding: 15px;
  border-radius: 4px;
}
</style>
```

### 2. Render Output

Hasil dari contoh di atas:

```html
<div class="container">
  <h2>Child Component</h2>
  <div class="slot-content">
    <h1>Hello from Parent!</h1>
    <p>This content is passed via slot.</p>
  </div>
</div>
```

## 🎯 Default Slots

Default slots adalah slot tunggal yang paling umum digunakan.

### Contoh Default Slot

```vue
<!-- CardComponent.vue -->
<script setup>
</script>

<template>
  <div class="card">
    <div class="card-header">
      <h3>Card Header</h3>
    </div>
    <div class="card-body">
      <slot></slot>
    </div>
    <div class="card-footer">
      <small>Card Footer</small>
    </div>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  overflow: hidden;
  margin: 20px 0;
}

.card-header {
  background: #f8fafc;
  padding: 15px;
  border-bottom: 1px solid #e2e8f0;
}

.card-body {
  padding: 20px;
}

.card-footer {
  background: #f8fafc;
  padding: 10px 15px;
  border-top: 1px solid #e2e8f0;
}
</style>
```

### Penggunaan Default Slot

```vue
<script setup>
import CardComponent from './CardComponent.vue'
</script>

<template>
  <!-- Card dengan text content -->
  <CardComponent>
    <p>Ini adalah konten card yang sederhana.</p>
    <p>Bisa berisi beberapa paragraf.</p>
  </CardComponent>

  <!-- Card dengan complex content -->
  <CardComponent>
    <h4>User Profile</h4>
    <img src="/avatar.jpg" alt="User Avatar" />
    <ul>
      <li>Name: John Doe</li>
      <li>Email: john@example.com</li>
      <li>Role: Developer</li>
    </ul>
  </CardComponent>

  <!-- Card dengan form -->
  <CardComponent>
    <form @submit.prevent="handleSubmit">
      <input v-model="name" placeholder="Your name" />
      <button type="submit">Submit</button>
    </form>
  </CardComponent>
</template>
```

## 🏷️ Named Slots

Named slots memungkinkan kita memiliki multiple slots dengan nama yang berbeda.

### Basic Named Slots

```vue
<!-- LayoutComponent.vue -->
<script setup>
</script>

<template>
  <div class="layout">
    <header class="header">
      <slot name="header"></slot>
    </header>

    <main class="main">
      <slot></slot> <!-- Default slot -->
    </main>

    <aside class="sidebar">
      <slot name="sidebar"></slot>
    </aside>

    <footer class="footer">
      <slot name="footer"></slot>
    </footer>
  </div>
</template>

<style scoped>
.layout {
  display: grid;
  grid-template-columns: 1fr 300px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  gap: 20px;
  padding: 20px;
}

.header {
  grid-column: 1 / -1;
  background: #1e293b;
  color: white;
  padding: 20px;
  border-radius: 8px;
}

.main {
  background: white;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.sidebar {
  background: #f8fafc;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}

.footer {
  grid-column: 1 / -1;
  background: #f1f5f9;
  padding: 15px 20px;
  border-radius: 8px;
  text-align: center;
}
</style>
```

### Penggunaan Named Slots

```vue
<script setup>
import LayoutComponent from './LayoutComponent.vue'
</script>

<template>
  <LayoutComponent>
    <!-- Named slot dengan v-slot directive -->
    <template v-slot:header>
      <h1>My Awesome Website</h1>
      <nav>
        <a href="#home">Home</a>
        <a href="#about">About</a>
        <a href="#contact">Contact</a>
      </nav>
    </template>

    <!-- Default slot (tanpa nama) -->
    <main>
      <h2>Welcome to Main Content</h2>
      <p>This is the main content area.</p>
      <p>It uses the default slot.</p>
    </main>

    <!-- Named slot dengan shorthand syntax -->
    <template #sidebar>
      <h3>Sidebar</h3>
      <ul>
        <li>Link 1</li>
        <li>Link 2</li>
        <li>Link 3</li>
      </ul>

      <div class="ads">
        <p>Advertisement Space</p>
      </div>
    </template>

    <template #footer>
      <p>&copy; 2024 My Website. All rights reserved.</p>
      <div>
        <a href="#privacy">Privacy Policy</a> |
        <a href="#terms">Terms of Service</a>
      </div>
    </template>
  </LayoutComponent>
</template>
```

## 🎪 Fallback Content

Fallback content adalah konten default yang akan ditampilkan jika tidak ada konten yang diberikan ke slot.

### Component dengan Fallback Content

```vue
<!-- AlertComponent.vue -->
<script setup>
const props = defineProps({
  type: {
    type: String,
    default: 'info',
    validator: (value) => ['info', 'success', 'warning', 'error'].includes(value)
  }
})
</script>

<template>
  <div :class="['alert', `alert-${type}`]">
    <div class="alert-icon">
      <slot name="icon">
        <!-- Default icon based on type -->
        <span v-if="type === 'info'">ℹ️</span>
        <span v-else-if="type === 'success'">✅</span>
        <span v-else-if="type === 'warning'">⚠️</span>
        <span v-else-if="type === 'error'">❌</span>
      </slot>
    </div>

    <div class="alert-content">
      <slot>
        <!-- Fallback content jika tidak ada konten -->
        <p>This is a default {{ type }} message.</p>
      </slot>
    </div>

    <div class="alert-close">
      <slot name="close">
        <!-- Default close button -->
        <button @click="$emit('close')" class="close-btn">×</button>
      </slot>
    </div>
  </div>
</template>

<style scoped>
.alert {
  display: flex;
  align-items: flex-start;
  padding: 15px;
  border-radius: 8px;
  margin: 10px 0;
  gap: 10px;
}

.alert-info {
  background: #e0f2fe;
  border: 1px solid #0284c7;
  color: #0c4a6e;
}

.alert-success {
  background: #dcfce7;
  border: 1px solid #16a34a;
  color: #14532d;
}

.alert-warning {
  background: #fef3c7;
  border: 1px solid #d97706;
  color: #78350f;
}

.alert-error {
  background: #fee2e2;
  border: 1px solid #dc2626;
  color: #7f1d1d;
}

.alert-icon {
  flex-shrink: 0;
  font-size: 20px;
}

.alert-content {
  flex-grow: 1;
}

.alert-close {
  flex-shrink: 0;
}

.close-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
```

### Penggunaan dengan dan tanpa Fallback

```vue
<script setup>
import AlertComponent from './AlertComponent.vue'
</script>

<template>
  <!-- Menggunakan fallback content -->
  <AlertComponent type="info" />

  <!-- Menggunakan custom content -->
  <AlertComponent type="success">
    <h4>Success!</h4>
    <p>Your changes have been saved successfully.</p>
  </AlertComponent>

  <!-- Menggunakan named slots -->
  <AlertComponent type="warning">
    <template #icon>
      🚨
    </template>

    <h4>Warning!</h4>
    <p>Please review your changes before proceeding.</p>

    <template #close>
      <button @click="handleClose" class="custom-close">
        Dismiss
      </button>
    </template>
  </AlertComponent>

  <!-- Tidak ada konten, fallback yang muncul -->
  <AlertComponent type="error" />
</template>

<script setup>
const handleClose = () => {
  console.log('Alert closed')
}
</script>

<style scoped>
.custom-close {
  background: #ef4444;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}
</style>
```

## 🔧 Scoped Slots

Scoped slots memungkinkan child component untuk memberikan data ke parent component melalui slot.

### Basic Scoped Slot

```vue
<!-- UserListComponent.vue -->
<script setup>
const users = [
  { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com', role: 'Moderator' }
]
</script>

<template>
  <div class="user-list">
    <h3>User List</h3>
    <div class="users">
      <!-- Slot dengan props -->
      <slot v-for="user in users" :key="user.id" :user="user" :index="users.indexOf(user)">
        <!-- Fallback rendering -->
        <div class="user-card">
          <h4>{{ user.name }}</h4>
          <p>{{ user.email }}</p>
          <span class="role">{{ user.role }}</span>
        </div>
      </slot>
    </div>
  </div>
</template>

<style scoped>
.user-list {
  padding: 20px;
}

.users {
  display: grid;
  gap: 15px;
}

.user-card {
  background: white;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.role {
  display: inline-block;
  background: #3b82f6;
  color: white;
  padding: 2px 8px;
  border-radius: 12px;
  font-size: 12px;
}
</style>
```

### Menggunakan Scoped Slots

```vue
<script setup>
import UserListComponent from './UserListComponent.vue'

const handleUserClick = (user) => {
  console.log('User clicked:', user)
}

const getRoleColor = (role) => {
  const colors = {
    'Admin': '#dc2626',
    'Moderator': '#f59e0b',
    'User': '#10b981'
  }
  return colors[role] || '#6b7280'
}
</script>

<template>
  <!-- Menggunakan scoped slot dengan destructuring -->
  <UserListComponent>
    <template #default="{ user, index }">
      <div class="custom-user-card" @click="handleUserClick(user)">
        <div class="user-header">
          <h4>{{ user.name }}</h4>
          <span
            class="role-badge"
            :style="{ backgroundColor: getRoleColor(user.role) }"
          >
            {{ user.role }}
          </span>
        </div>
        <p class="email">{{ user.email }}</p>
        <div class="user-meta">
          <small>User ID: {{ user.id }}</small>
          <small>Index: {{ index }}</small>
        </div>
      </div>
    </template>
  </UserListComponent>

  <!-- Contoh lain dengan rendering yang berbeda -->
  <UserListComponent>
    <template #default="{ user }">
      <div class="table-row">
        <span>{{ user.id }}</span>
        <span>{{ user.name }}</span>
        <span>{{ user.email }}</span>
        <span>{{ user.role }}</span>
      </div>
    </template>
  </UserListComponent>
</template>

<style scoped>
.custom-user-card {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 20px;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.2s;
}

.custom-user-card:hover {
  transform: translateY(-2px);
}

.user-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.user-header h4 {
  margin: 0;
}

.role-badge {
  color: white;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
}

.email {
  margin: 10px 0;
  opacity: 0.9;
}

.user-meta {
  display: flex;
  justify-content: space-between;
  font-size: 12px;
  opacity: 0.8;
}

.table-row {
  display: grid;
  grid-template-columns: 50px 1fr 1fr 100px;
  gap: 15px;
  padding: 15px;
  background: #f8fafc;
  border-radius: 8px;
  border: 1px solid #e2e8f0;
}
</style>
```

### Advanced Scoped Slots dengan Multiple Data

```vue
<!-- DataTable.vue -->
<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  data: {
    type: Array,
    required: true
  },
  columns: {
    type: Array,
    required: true
  }
})

const sortBy = ref(null)
const sortOrder = ref('asc')

const sortedData = computed(() => {
  if (!sortBy.value) return props.data

  return [...props.data].sort((a, b) => {
    const aVal = a[sortBy.value]
    const bVal = b[sortBy.value]

    if (aVal < bVal) return sortOrder.value === 'asc' ? -1 : 1
    if (aVal > bVal) return sortOrder.value === 'asc' ? 1 : -1
    return 0
  })
})

const handleSort = (column) => {
  if (sortBy.value === column) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortBy.value = column
    sortOrder.value = 'asc'
  }
}
</script>

<template>
  <div class="data-table">
    <table>
      <thead>
        <tr>
          <th v-for="column in columns" :key="column.key" @click="handleSort(column.key)">
            {{ column.label }}
            <span v-if="sortBy === column.key" class="sort-indicator">
              {{ sortOrder === 'asc' ? '↑' : '↓' }}
            </span>
          </th>
        </tr>
      </thead>
      <tbody>
        <!-- Scoped slot dengan multiple props -->
        <slot
          v-for="(row, index) in sortedData"
          :key="row.id || index"
          :row="row"
          :index="index"
          :columns="columns"
          :is-even="index % 2 === 0"
        >
          <tr :class="{ even: index % 2 === 0 }">
            <td v-for="column in columns" :key="column.key">
              {{ row[column.key] }}
            </td>
          </tr>
        </slot>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.data-table {
  width: 100%;
  overflow-x: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #e2e8f0;
}

th {
  background: #f8fafc;
  font-weight: 600;
  cursor: pointer;
  user-select: none;
}

th:hover {
  background: #f1f5f9;
}

.sort-indicator {
  margin-left: 5px;
  font-weight: bold;
}

.even {
  background: #f8fafc;
}
</style>
```

## 💫 Dynamic Slots

Dynamic slots memungkinkan kita untuk menggunakan nama slot secara dinamis.

### Component dengan Dynamic Slots

```vue
<!-- DynamicComponent.vue -->
<script setup>
const props = defineProps({
  slotNames: {
    type: Array,
    default: () => ['default']
  }
})
</script>

<template>
  <div class="dynamic-container">
    <div v-for="slotName in slotNames" :key="slotName" class="slot-section">
      <h3>{{ slotName }} Slot</h3>
      <div class="slot-content">
        <slot :name="slotName">
          <p>Fallback content for {{ slotName }} slot</p>
        </slot>
      </div>
    </div>
  </div>
</template>

<style scoped>
.dynamic-container {
  border: 2px solid #e2e8f0;
  border-radius: 8px;
  padding: 20px;
}

.slot-section {
  margin-bottom: 20px;
}

.slot-section:last-child {
  margin-bottom: 0;
}

.slot-content {
  background: #f8fafc;
  padding: 15px;
  border-radius: 4px;
  border: 1px dashed #cbd5e1;
}

h3 {
  margin-top: 0;
  color: #475569;
}
</style>
```

### Menggunakan Dynamic Slots

```vue
<script setup>
import { ref } from 'vue'
import DynamicComponent from './DynamicComponent.vue'

const dynamicSlots = ref(['header', 'content', 'footer'])
const newSlotName = ref('')

const addSlot = () => {
  if (newSlotName.value && !dynamicSlots.value.includes(newSlotName.value)) {
    dynamicSlots.value.push(newSlotName.value)
    newSlotName.value = ''
  }
}
</script>

<template>
  <div class="dynamic-demo">
    <div class="controls">
      <input
        v-model="newSlotName"
        placeholder="Enter slot name"
        @keyup.enter="addSlot"
      />
      <button @click="addSlot">Add Slot</button>
    </div>

    <DynamicComponent :slot-names="dynamicSlots">
      <template #header>
        <h2>Dynamic Header Content</h2>
        <p>This header was added dynamically!</p>
      </template>

      <template #content>
        <div class="content-box">
          <h4>Main Content</h4>
          <p>Dynamic slots make components super flexible!</p>
        </div>
      </template>

      <template #footer>
        <footer>
          <small>Dynamic footer with slot content</small>
        </footer>
      </template>

      <!-- Slots untuk nama yang mungkin ditambahkan -->
      <template v-for="slotName in dynamicSlots" :key="slotName" #[slotName]>
        <div v-if="!['header', 'content', 'footer'].includes(slotName)">
          <p>Content for dynamically added slot: <strong>{{ slotName }}</strong></p>
        </div>
      </template>
    </DynamicComponent>
  </div>
</template>

<style scoped>
.dynamic-demo {
  max-width: 600px;
  margin: 0 auto;
}

.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.controls input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #d1d5db;
  border-radius: 4px;
}

.controls button {
  padding: 8px 16px;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.content-box {
  background: #dbeafe;
  padding: 15px;
  border-radius: 4px;
  border-left: 4px solid #3b82f6;
}

footer {
  text-align: center;
  color: #6b7280;
}
</style>
```

## 🚀 Best Practices

### 1. Gunakan Fallback Content yang Bermanfaat

```vue
<script setup>
// ✅ Berikan fallback yang berguna
</script>

<template>
  <div class="modal">
    <slot name="header">
      <h2>Modal Title</h2>
    </slot>

    <slot>
      <p>Modal content goes here...</p>
    </slot>

    <slot name="footer">
      <button @click="$emit('close')">Close</button>
    </slot>
  </div>
</template>
```

### 2. Beri Nama Slot yang Deskriptif

```vue
<script setup>
// ✅ Gunakan nama yang jelas dan deskriptif
</script>

<template>
  <div class="page-layout">
    <slot name="page-header"></slot>
    <slot name="page-sidebar"></slot>
    <slot name="page-content"></slot>
    <slot name="page-footer"></slot>
  </div>
</template>

<!-- ❌ Hindari nama yang tidak jelas -->
<template>
  <div class="layout">
    <slot name="a"></slot>
    <slot name="b"></slot>
    <slot name="c"></slot>
  </div>
</template>
```

### 3. Validasi Slot Props

```vue
<script setup>
// ✅ Validasi data yang diberikan ke scoped slots
const props = defineProps({
  users: {
    type: Array,
    required: true,
    validator: (users) => {
      return users.every(user =>
        user.id && user.name && user.email
      )
    }
  }
})
</script>

<template>
  <div class="user-list">
    <slot
      v-for="user in users"
      :key="user.id"
      :user="user"
      :is-admin="user.role === 'admin'"
      :display-name="user.name.toUpperCase()"
    >
    </slot>
  </div>
</template>
```

### 4. Gunakan Conditional Slots

```vue
<script setup>
const props = defineProps({
  showActions: Boolean,
  user: Object
})
</script>

<template>
  <div class="user-card">
    <div class="user-info">
      <slot name="user-info" :user="user">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
      </slot>
    </div>

    <!-- Conditional slot -->
    <div v-if="showActions" class="user-actions">
      <slot name="actions" :user="user">
        <button>Edit</button>
        <button>Delete</button>
      </slot>
    </div>
  </div>
</template>
```

## 💡 Tips dan Trik

### 1. Slot dengan Transisi

```vue
<script setup>
</script>

<template>
  <div class="slotted-component">
    <transition name="fade" mode="out-in">
      <slot></slot>
    </transition>
  </div>
</template>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
```

### 2. Recursive Slots

```vue
<!-- TreeComponent.vue -->
<script setup>
const props = defineProps({
  nodes: Array
})
</script>

<template>
  <div class="tree">
    <div v-for="node in nodes" :key="node.id" class="tree-node">
      <slot name="node" :node="node">
        <span>{{ node.label }}</span>
      </slot>

      <!-- Recursive slot untuk children -->
      <div v-if="node.children && node.children.length" class="tree-children">
        <TreeComponent :nodes="node.children">
          <template #node="{ node: childNode }">
            <slot name="node" :node="childNode"></slot>
          </template>
        </TreeComponent>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tree-node {
  margin-left: 20px;
  padding: 5px 0;
}

.tree-children {
  margin-left: 20px;
  border-left: 1px solid #e2e8f0;
}
</style>
```

### 3. Slot Composition

```vue
<!-- CardWrapper.vue -->
<script setup>
</script>

<template>
  <div class="card-wrapper">
    <div class="card-background">
      <slot name="background">
        <div class="default-bg"></div>
      </slot>
    </div>

    <div class="card-content">
      <slot name="header"></slot>
      <slot></slot>
      <slot name="footer"></slot>
    </div>
  </div>
</template>

<style scoped>
.card-wrapper {
  position: relative;
  border-radius: 12px;
  overflow: hidden;
}

.card-background {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1;
}

.card-content {
  position: relative;
  z-index: 2;
  padding: 20px;
}
</style>
```

### 4. Slot dengan Render Functions

```vue
<script setup>
import { h } from 'vue'

const props = defineProps({
  level: {
    type: Number,
    default: 1,
    validator: (value) => value >= 1 && value <= 6
  }
})

// Dynamic heading level dengan slot
const renderHeading = () => {
  return h(`h${props.level}`, {}, [
    h('slot')
  ])
}
</script>

<template>
  <component :is="`h${level}`">
    <slot></slot>
  </component>
</template>
```

---

## 🎉 Kesimpulan

Slots adalah fitur powerful dalam Vue.js yang memungkinkan:

1. **🔄 Flexible Components** - Komponen dapat digunakan dengan berbagai konten
2. **📦 Content Distribution** - Mendistribusikan konten dengan efektif
3. **🎯 Composition** - Membangun UI dari komponen-komponen kecil
4. **🛡️ Encapsulation** - Memisahkan struktur dan konten
5. **📊 Reusability** - Maksimalkan penggunaan kembali komponen

### Poin Penting:

- Default slots untuk konten tunggal
- Named slots untuk multiple konten area
- Fallback content untuk konten default
- Scoped slots untuk data sharing antar komponen
- Dynamic slots untuk fleksibilitas maksimal

### Lanjutan:

- Renderless components dengan scoped slots
- Component composition patterns
- Advanced slot techniques
- Performance considerations dengan slots

Dengan memahami slots, Anda sudah bisa membuat komponen Vue.js yang sangat fleksibel dan reusable! 🚀

---

**🎯 Ready for Practice?** Coba buat komponen-komponen dengan berbagai jenis slot patterns untuk memperkuat pemahaman Anda!

## ❓ Pertanyaan: Apa Fungsi Slots?

Slots memiliki 5 fungsi utama dalam Vue.js yang membuatnya sangat powerful untuk komponen yang fleksibel:

### 1. **Content Distribution & Component Composition**
```vue
<!-- BaseLayout.vue - Komponen layout yang fleksibel -->
<script setup>
const props = defineProps({
  showSidebar: {
    type: Boolean,
    default: true
  },
  theme: {
    type: String,
    default: 'light',
    validator: (value) => ['light', 'dark'].includes(value)
  }
})
</script>

<template>
  <!-- Fungsi 1: Mendistribusikan konten ke berbagai area -->
  <div class="base-layout" :class="[theme, { 'has-sidebar': showSidebar }]">
    <!-- Header slot untuk branding dan navigation -->
    <header class="layout-header">
      <slot name="header">
        <div class="default-header">
          <h1>My Application</h1>
          <nav>
            <a href="#home">Home</a>
            <a href="#about">About</a>
          </nav>
        </div>
      </slot>
    </header>

    <div class="layout-main">
      <!-- Sidebar slot untuk navigation dan widgets -->
      <aside v-if="showSidebar" class="layout-sidebar">
        <slot name="sidebar">
          <div class="default-sidebar">
            <h3>Navigation</h3>
            <ul>
              <li><a href="#dashboard">Dashboard</a></li>
              <li><a href="#profile">Profile</a></li>
              <li><a href="#settings">Settings</a></li>
            </ul>
          </div>
        </slot>
      </aside>

      <!-- Main content slot -->
      <main class="layout-content">
        <slot>
          <div class="default-content">
            <h2>Welcome to the Application</h2>
            <p>This is the default content area.</p>
          </div>
        </slot>
      </main>
    </div>

    <!-- Footer slot untuk footer information -->
    <footer class="layout-footer">
      <slot name="footer">
        <div class="default-footer">
          <p>&copy; 2024 My Application. All rights reserved.</p>
        </div>
      </slot>
    </footer>
  </div>
</template>

<style scoped>
.base-layout {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  font-family: Arial, sans-serif;
}

.base-layout.dark {
  background: #1a1a1a;
  color: white;
}

.layout-header {
  background: #2c3e50;
  color: white;
  padding: 1rem 2rem;
}

.layout-main {
  flex: 1;
  display: flex;
  min-height: 0;
}

.layout-sidebar {
  width: 250px;
  background: #ecf0f1;
  padding: 1rem;
  border-right: 1px solid #bdc3c7;
}

.layout-content {
  flex: 1;
  padding: 2rem;
  overflow-y: auto;
}

.layout-footer {
  background: #34495e;
  color: white;
  padding: 1rem 2rem;
  text-align: center;
}

.has-sidebar .layout-content {
  max-width: calc(100% - 250px);
}
</style>

<!-- Menggunakan BaseLayout dengan konten kustom -->
<script setup>
import BaseLayout from './BaseLayout.vue'
</script>

<template>
  <!-- Layout dengan konten kustom di semua slot -->
  <BaseLayout show-sidebar theme="light">
    <!-- Custom header -->
    <template #header>
      <div class="custom-header">
        <div class="logo">
          <img src="/logo.png" alt="Logo" />
          <h1>Vue.js Dashboard</h1>
        </div>
        <div class="user-menu">
          <button>Profile</button>
          <button>Logout</button>
        </div>
      </div>
    </template>

    <!-- Custom sidebar -->
    <template #sidebar>
      <div class="custom-sidebar">
        <div class="user-profile">
          <img src="/avatar.jpg" alt="User" />
          <span>John Doe</span>
        </div>
        <nav class="main-nav">
          <a href="#dashboard" class="active">📊 Dashboard</a>
          <a href="#analytics">📈 Analytics</a>
          <a href="#reports">📄 Reports</a>
          <a href="#team">👥 Team</a>
        </nav>
        <div class="widget-area">
          <h4>Quick Actions</h4>
          <button>+ New Project</button>
          <button>📧 Send Email</button>
        </div>
      </div>
    </template>

    <!-- Custom main content -->
    <main>
      <h2>Dashboard Overview</h2>
      <div class="dashboard-grid">
        <div class="stat-card">
          <h3>Total Users</h3>
          <p class="stat-number">1,234</p>
          <span class="trend positive">+12%</span>
        </div>
        <div class="stat-card">
          <h3>Revenue</h3>
          <p class="stat-number">$45,678</p>
          <span class="trend positive">+8%</span>
        </div>
        <div class="stat-card">
          <h3>Active Projects</h3>
          <p class="stat-number">42</p>
          <span class="trend negative">-3%</span>
        </div>
      </div>
    </main>

    <!-- Custom footer -->
    <template #footer>
      <div class="custom-footer">
        <div class="footer-links">
          <a href="#privacy">Privacy Policy</a>
          <a href="#terms">Terms of Service</a>
          <a href="#support">Support</a>
        </div>
        <div class="footer-info">
          <span>Version 2.1.0</span>
          <span>Last updated: {{ new Date().toLocaleDateString() }}</span>
        </div>
      </div>
    </template>
  </BaseLayout>
</template>
```

**Keuntungan:**
- ✅ **Flexible composition** - bangun layout dari berbagai konten
- ✅ **Content separation** - pisahkan struktur dari konten
- ✅ **Reusable layouts** - satu layout untuk banyak halaman
- ✅ **Consistent structure** - struktur yang konsisten dengan konten variatif

### 2. **Component Customization & Theming**
```vue
<!-- CardComponent.vue - Komponen card yang dapat dikustomisasi -->
<script setup>
const props = defineProps({
  variant: {
    type: String,
    default: 'default',
    validator: (value) => ['default', 'elevated', 'outlined', 'filled'].includes(value)
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large'].includes(value)
  },
  isClickable: {
    type: Boolean,
    default: false
  },
  showHeader: {
    type: Boolean,
    default: true
  },
  showFooter: {
    type: Boolean,
    default: false
  }
})

const cardClasses = computed(() => ({
  [`card-${props.variant}`]: true,
  [`card-${props.size}`]: true,
  'card-clickable': props.isClickable
}))
</script>

<template>
  <!-- Fungsi 2: Komponen yang dapat dikustomisasi melalui slots -->
  <div class="card" :class="cardClasses" @click="isClickable ? $emit('click') : null">
    <!-- Header slot dengan fallback -->
    <div v-if="showHeader" class="card-header">
      <slot name="header">
        <div class="default-header">
          <h3>Card Title</h3>
          <span class="card-subtitle">Subtitle</span>
        </div>
      </slot>
    </div>

    <!-- Action area slot -->
    <div v-if="$slots.actions" class="card-actions">
      <slot name="actions"></slot>
    </div>

    <!-- Main content slot -->
    <div class="card-content">
      <slot>
        <div class="default-content">
          <p>Card content goes here...</p>
        </div>
      </slot>
    </div>

    <!-- Extra content slot untuk additional info -->
    <div v-if="$slots.extra" class="card-extra">
      <slot name="extra"></slot>
    </div>

    <!-- Footer slot -->
    <div v-if="showFooter || $slots.footer" class="card-footer">
      <slot name="footer">
        <div class="default-footer">
          <small>Last updated: {{ new Date().toLocaleDateString() }}</small>
        </div>
      </slot>
    </div>

    <!-- Overlay slot untuk loading atau status -->
    <div v-if="$slots.overlay" class="card-overlay">
      <slot name="overlay"></slot>
    </div>
  </div>
</template>

<style scoped>
.card {
  border-radius: 12px;
  background: white;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

.card-default {
  border: 1px solid #e2e8f0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.card-elevated {
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  border: none;
}

.card-outlined {
  border: 2px solid #3b82f6;
  box-shadow: none;
}

.card-filled {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
}

.card-small {
  padding: 1rem;
}

.card-medium {
  padding: 1.5rem;
}

.card-large {
  padding: 2rem;
}

.card-clickable {
  cursor: pointer;
}

.card-clickable:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.card-header {
  margin-bottom: 1rem;
}

.card-actions {
  margin-bottom: 1rem;
  display: flex;
  gap: 0.5rem;
}

.card-content {
  margin-bottom: 1rem;
}

.card-extra {
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
}

.card-footer {
  padding-top: 1rem;
  border-top: 1px solid #e2e8f0;
  font-size: 0.875rem;
  color: #64748b;
}

.card-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}
</style>

<!-- Berbagai variasi penggunaan CardComponent -->
<script setup>
import { ref } from 'vue'
import CardComponent from './CardComponent.vue'

const isLoading = ref(false)
const simulateLoading = () => {
  isLoading.value = true
  setTimeout(() => isLoading.value = false, 2000)
}
</script>

<template>
  <div class="card-showcase">
    <!-- Basic card dengan default styling -->
    <CardComponent size="small">
      <h4>Simple Card</h4>
      <p>Ini adalah card sederhana dengan default styling.</p>
    </CardComponent>

    <!-- Elevated card dengan custom header -->
    <CardComponent variant="elevated">
      <template #header>
        <div class="custom-header">
          <span class="badge">Premium</span>
          <h3>Premium Features</h3>
        </div>
      </template>

      <p>Card dengan elevasi dan header kustom.</p>

      <template #footer>
        <button>Learn More</button>
      </template>
    </CardComponent>

    <!-- Clickable card dengan actions -->
    <CardComponent
      variant="outlined"
      :is-clickable="true"
      @click="console.log('Card clicked!')"
    >
      <template #actions>
        <button class="icon-btn">❤️</button>
        <button class="icon-btn">🔄</button>
        <button class="icon-btn">📤</button>
      </template>

      <h4>Interactive Card</h4>
      <p>Card ini dapat diklik dan memiliki action buttons.</p>

      <template #extra>
        <div class="stats">
          <span>👀 1.2k views</span>
          <span>💬 45 comments</span>
        </div>
      </template>
    </CardComponent>

    <!-- Filled card dengan loading overlay -->
    <CardComponent variant="filled" size="large">
      <template #header>
        <h3>Analytics Dashboard</h3>
        <button @click="simulateLoading">🔄 Refresh</button>
      </template>

      <div class="chart-container">
        <div class="chart-placeholder">
          📊 Chart akan ditampilkan di sini
        </div>
      </div>

      <template #overlay v-if="isLoading">
        <div class="loading-spinner">
          <div class="spinner"></div>
          <p>Loading data...</p>
        </div>
      </template>

      <template #footer>
        <div class="chart-controls">
          <button>Last 7 days</button>
          <button>Last month</button>
          <button>Last year</button>
        </div>
      </template>
    </CardComponent>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Visual customization** - berbagai tema dan gaya visual
- ✅ **Layout flexibility** - struktur konten yang dapat disesuaikan
- ✅ **Interactive elements** - tombol dan aksi yang dapat dikustomisasi
- ✅ **State handling** - overlay untuk loading dan status

### 3. **Data Passing & Component Logic Separation**
```vue
<!-- UserTable.vue - Komponen tabel dengan scoped slots -->
<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  users: {
    type: Array,
    required: true,
    validator: (users) => {
      return users.every(user =>
        user.id &&
        user.name &&
        user.email
      )
    }
  },
  loading: {
    type: Boolean,
    default: false
  },
  selectable: {
    type: Boolean,
    default: false
  },
  sortable: {
    type: Boolean,
    default: true
  }
})

const emit = defineEmits(['selection-change', 'row-click', 'sort'])

// Internal state dan logic
const selectedUsers = ref([])
const sortColumn = ref('')
const sortDirection = ref('asc')

// Computed properties
const sortedUsers = computed(() => {
  if (!sortColumn.value || !props.sortable) return props.users

  return [...props.users].sort((a, b) => {
    const aVal = a[sortColumn.value]
    const bVal = b[sortColumn.value]

    if (aVal < bVal) return sortDirection.value === 'asc' ? -1 : 1
    if (aVal > bVal) return sortDirection.value === 'asc' ? 1 : -1
    return 0
  })
})

// Fungsi 3: Memisahkan logic component dari presentation
const handleSort = (column) => {
  if (!props.sortable) return

  if (sortColumn.value === column) {
    sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortColumn.value = column
    sortDirection.value = 'asc'
  }

  emit('sort', { column, direction: sortDirection.value })
}

const handleRowClick = (user, index) => {
  emit('row-click', { user, index })
}

const handleSelection = (user, isSelected) => {
  if (isSelected) {
    selectedUsers.value.push(user.id)
  } else {
    const index = selectedUsers.value.indexOf(user.id)
    if (index > -1) selectedUsers.value.splice(index, 1)
  }
  emit('selection-change', selectedUsers.value)
}

const isUserSelected = (user) => selectedUsers.value.includes(user.id)

const formatDate = (date) => {
  return new Date(date).toLocaleDateString()
}

const getUserStatus = (user) => {
  if (user.isActive) return { text: 'Active', class: 'status-active' }
  return { text: 'Inactive', class: 'status-inactive' }
}
</script>

<template>
  <div class="user-table">
    <!-- Loading state -->
    <div v-if="loading" class="loading-overlay">
      <div class="spinner"></div>
      <p>Loading users...</p>
    </div>

    <!-- Table dengan scoped slots untuk custom rendering -->
    <table class="table">
      <thead>
        <tr>
          <!-- Select all checkbox -->
          <th v-if="selectable" class="select-column">
            <input
              type="checkbox"
              :checked="selectedUsers.length === users.length"
              @change="toggleSelectAll"
            />
          </th>

          <!-- Sortable headers -->
          <th
            v-for="column in ['name', 'email', 'role']"
            :key="column"
            @click="handleSort(column)"
            :class="{ sortable }"
          >
            {{ column.charAt(0).toUpperCase() + column.slice(1) }}
            <span v-if="sortColumn === column" class="sort-indicator">
              {{ sortDirection === 'asc' ? '↑' : '↓' }}
            </span>
          </th>

          <!-- Additional columns slot -->
          <slot name="headers"></slot>
        </tr>
      </thead>

      <tbody>
        <!-- Main row slot dengan data passing -->
        <tr
          v-for="(user, index) in sortedUsers"
          :key="user.id"
          @click="handleRowClick(user, index)"
          :class="{ selected: isUserSelected(user) }"
        >
          <!-- Selection checkbox -->
          <td v-if="selectable" class="select-column">
            <input
              type="checkbox"
              :checked="isUserSelected(user)"
              @change="handleSelection(user, $event.target.checked)"
              @click.stop
            />
          </td>

          <!-- Scoped slot untuk row customization -->
          <slot
            name="row"
            :user="user"
            :index="index"
            :isSelected="isUserSelected(user)"
            :formatDate="formatDate"
            :getUserStatus="getUserStatus"
          >
            <!-- Default row rendering -->
            <td>{{ user.name }}</td>
            <td>{{ user.email }}</td>
            <td>
              <span :class="getUserStatus(user).class">
                {{ getUserStatus(user).text }}
              </span>
            </td>
          </slot>

          <!-- Action column slot -->
          <slot name="actions" :user="user" :index="index">
            <td class="actions">
              <button @click.stop="editUser(user)">Edit</button>
              <button @click.stop="deleteUser(user)" class="danger">Delete</button>
            </td>
          </slot>
        </tr>
      </tbody>
    </table>

    <!-- Empty state slot -->
    <div v-if="users.length === 0 && !loading" class="empty-state">
      <slot name="empty">
        <div class="default-empty">
          <h3>No users found</h3>
          <p>Start by adding your first user.</p>
          <button @click="$emit('add-user')">Add User</button>
        </div>
      </slot>
    </div>

    <!-- Footer slot untuk pagination atau actions -->
    <div v-if="$slots.footer" class="table-footer">
      <slot name="footer" :selectedCount="selectedUsers.length" :totalCount="users.length"></slot>
    </div>
  </div>
</template>

<style scoped>
.user-table {
  position: relative;
}

.loading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.9);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

.table {
  width: 100%;
  border-collapse: collapse;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

th, td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #e2e8f0;
}

th {
  background: #f8fafc;
  font-weight: 600;
  color: #374151;
}

th.sortable {
  cursor: pointer;
  user-select: none;
}

th.sortable:hover {
  background: #f1f5f9;
}

.sort-indicator {
  margin-left: 5px;
  font-weight: bold;
}

.select-column {
  width: 50px;
}

tr:hover {
  background: #f8fafc;
}

tr.selected {
  background: #dbeafe;
}

.status-active {
  color: #059669;
  font-weight: 500;
}

.status-inactive {
  color: #6b7280;
}

.actions {
  display: flex;
  gap: 8px;
}

.actions button {
  padding: 4px 8px;
  border: 1px solid #d1d5db;
  background: white;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
}

.actions button.danger {
  border-color: #ef4444;
  color: #ef4444;
}

.empty-state {
  padding: 3rem;
  text-align: center;
  background: #f8fafc;
  border-radius: 8px;
}

.table-footer {
  margin-top: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>

<!-- Menggunakan UserTable dengan custom rendering -->
<script setup>
import { ref } from 'vue'
import UserTable from './UserTable.vue'

const users = ref([
  { id: 1, name: 'John Doe', email: 'john@example.com', role: 'admin', isActive: true, joinDate: '2023-01-15' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'user', isActive: false, joinDate: '2023-03-20' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com', role: 'moderator', isActive: true, joinDate: '2023-02-10' }
])

const handleUserAction = (action, user) => {
  console.log(`${action} user:`, user.name)
}

const handleSort = ({ column, direction }) => {
  console.log(`Sorting by ${column} in ${direction} order`)
}
</script>

<template>
  <div class="table-demo">
    <h2>User Management Table</h2>

    <UserTable
      :users="users"
      :selectable="true"
      :sortable="true"
      @row-click="handleUserAction('click', $event.user)"
      @sort="handleSort"
    >
      <!-- Custom row rendering -->
      <template #row="{ user, formatDate, getUserStatus }">
        <td>
          <div class="user-cell">
            <img :src="`/avatars/${user.id}.jpg` :alt="user.name" class="avatar" />
            <div class="user-info">
              <strong>{{ user.name }}</strong>
              <small>ID: #{{ user.id.toString().padStart(4, '0') }}</small>
            </div>
          </div>
        </td>
        <td>
          <div class="email-cell">
            <span>{{ user.email }}</span>
            <small>Joined {{ formatDate(user.joinDate) }}</small>
          </div>
        </td>
        <td>
          <span :class="['role-badge', user.role]">
            {{ user.role.toUpperCase() }}
          </span>
        </td>
      </template>

      <!-- Custom actions -->
      <template #actions="{ user }">
        <td class="custom-actions">
          <button @click="handleUserAction('view', user)" class="view-btn">
            👁️ View
          </button>
          <button @click="handleUserAction('edit', user)" class="edit-btn">
            ✏️ Edit
          </button>
          <button @click="handleUserAction('message', user)" class="message-btn">
            💬 Message
          </button>
        </td>
      </template>

      <!-- Custom footer -->
      <template #footer="{ selectedCount, totalCount }">
        <div class="custom-footer">
          <span>{{ selectedCount }} of {{ totalCount }} selected</span>
          <div class="bulk-actions">
            <button :disabled="selectedCount === 0">Delete Selected</button>
            <button :disabled="selectedCount === 0">Export Selected</button>
          </div>
        </div>
      </template>
    </UserTable>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Logic separation** - logic terpisah dari presentation
- ✅ **Data passing** - pass data ke parent dengan scoped slots
- ✅ **Custom rendering** - parent kontrol cara data ditampilkan
- ✅ **Reusability** - satu komponen untuk berbagai display needs

### 4. **Component Extension & Plugin Patterns**
```vue
<!-- ModalComponent.vue - Modal yang dapat diekstensi -->
<script setup>
const props = defineProps({
  isOpen: {
    type: Boolean,
    default: false
  },
  size: {
    type: String,
    default: 'medium',
    validator: (value) => ['small', 'medium', 'large', 'fullscreen'].includes(value)
  },
  closable: {
    type: Boolean,
    default: true
  },
  showBackdrop: {
    type: Boolean,
    default: true
  },
  persistent: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['close', 'confirm', 'cancel'])

// Fungsi 4: Komponen yang dapat diekstensi melalui slots
const modalClasses = computed(() => ({
  [`modal-${props.size}`]: true,
  'modal-open': props.isOpen
}))

const handleBackdropClick = () => {
  if (!props.persistent && props.closable) {
    emit('close')
  }
}

const handleKeydown = (event) => {
  if (event.key === 'Escape' && props.closable) {
    emit('close')
  }
}

watch(() => props.isOpen, (isOpen) => {
  if (isOpen) {
    document.addEventListener('keydown', handleKeydown)
    document.body.style.overflow = 'hidden'
  } else {
    document.removeEventListener('keydown', handleKeydown)
    document.body.style.overflow = ''
  }
})
</script>

<template>
  <!-- Modal dengan berbagai slot untuk ekstensi -->
  <teleport to="body">
    <div v-if="isOpen" class="modal-wrapper">
      <!-- Backdrop slot -->
      <div v-if="showBackdrop" class="modal-backdrop" @click="handleBackdropClick">
        <slot name="backdrop">
          <div class="default-backdrop"></div>
        </slot>
      </div>

      <!-- Modal container -->
      <div class="modal-container" :class="modalClasses">
        <!-- Header slot -->
        <div v-if="$slots.header" class="modal-header">
          <slot name="header">
            <h3>Modal Title</h3>
            <button v-if="closable" @click="$emit('close')" class="close-btn">×</button>
          </slot>
        </div>

        <!-- Content slot -->
        <div class="modal-content" :class="{ 'has-header': $slots.header }">
          <slot></slot>
        </div>

        <!-- Footer slot -->
        <div v-if="$slots.footer" class="modal-footer">
          <slot name="footer" :close="() => $emit('close')">
            <button @click="$emit('cancel')">Cancel</button>
            <button @click="$emit('confirm')" class="primary">Confirm</button>
          </slot>
        </div>

        <!-- Extension slots untuk positioning -->
        <div v-if="$slots['top-left']" class="modal-extension top-left">
          <slot name="top-left"></slot>
        </div>
        <div v-if="$slots['top-right']" class="modal-extension top-right">
          <slot name="top-right"></slot>
        </div>
        <div v-if="$slots['bottom-left']" class="modal-extension bottom-left">
          <slot name="bottom-left"></slot>
        </div>
        <div v-if="$slots['bottom-right']" class="modal-extension bottom-right">
          <slot name="bottom-right"></slot>
        </div>
      </div>
    </div>
  </teleport>
</template>

<style scoped>
.modal-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 1000;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-backdrop {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
}

.modal-container {
  position: relative;
  background: white;
  border-radius: 12px;
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
  max-height: 90vh;
  overflow: hidden;
  z-index: 1;
}

.modal-small {
  width: 400px;
}

.modal-medium {
  width: 600px;
}

.modal-large {
  width: 800px;
}

.modal-fullscreen {
  width: 90vw;
  height: 90vh;
}

.modal-header {
  padding: 1.5rem;
  border-bottom: 1px solid #e2e8f0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.modal-content {
  padding: 1.5rem;
  overflow-y: auto;
  max-height: 60vh;
}

.modal-content.has-header {
  padding-top: 1rem;
}

.modal-footer {
  padding: 1.5rem;
  border-top: 1px solid #e2e8f0;
  display: flex;
  justify-content: flex-end;
  gap: 0.75rem;
}

.modal-extension {
  position: absolute;
  width: 40px;
  height: 40px;
  background: white;
  border: 2px solid #e2e8f0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2;
}

.top-left {
  top: -20px;
  left: -20px;
}

.top-right {
  top: -20px;
  right: -20px;
}

.bottom-left {
  bottom: -20px;
  left: -20px;
}

.bottom-right {
  bottom: -20px;
  right: -20px;
}

.close-btn {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  color: #6b7280;
}

.close-btn:hover {
  color: #374151;
}
</style>

<!-- Berbagai ekstensi ModalComponent -->
<script setup>
import { ref } from 'vue'
import ModalComponent from './ModalComponent.vue'

const modals = ref({
  basic: false,
  form: false,
  confirm: false,
  wizard: false,
  custom: false
})

const formData = ref({
  name: '',
  email: '',
  message: ''
})

const wizardStep = ref(1)
</script>

<template>
  <div class="modal-showcase">
    <h2>Modal Extensions Showcase</h2>

    <!-- Basic modal dengan slot kustom -->
    <button @click="modals.basic = true">Open Basic Modal</button>
    <ModalComponent v-model:isOpen="modals.basic" size="small">
      <template #header>
        <h3>👋 Welcome!</h3>
      </template>
      <p>This is a basic modal with custom header.</p>
      <p>Modal components can be easily customized using slots.</p>
    </ModalComponent>

    <!-- Form modal dengan validation -->
    <button @click="modals.form = true">Open Form Modal</button>
    <ModalComponent v-model:isOpen="modals.form" size="medium">
      <template #header>
        <h3>📝 Contact Form</h3>
      </template>

      <form @submit.prevent>
        <div class="form-group">
          <label>Name</label>
          <input v-model="formData.name" placeholder="Your name" />
        </div>
        <div class="form-group">
          <label>Email</label>
          <input v-model="formData.email" type="email" placeholder="Your email" />
        </div>
        <div class="form-group">
          <label>Message</label>
          <textarea v-model="formData.message" placeholder="Your message"></textarea>
        </div>
      </form>

      <template #footer="{ close }">
        <button @click="close">Cancel</button>
        <button @click="handleSubmit" class="primary">Send Message</button>
      </template>
    </ModalComponent>

    <!-- Confirmation modal dengan custom styling -->
    <button @click="modals.confirm = true">Open Confirmation</button>
    <ModalComponent v-model:isOpen="modals.confirm" :persistent="true">
      <template #header>
        <h3>⚠️ Confirm Action</h3>
      </template>

      <div class="confirm-content">
        <div class="warning-icon">⚠️</div>
        <p>Are you sure you want to delete this item?</p>
        <p class="warning-text">This action cannot be undone.</p>
      </div>

      <template #footer>
        <button @click="modals.confirm = false">Cancel</button>
        <button @click="confirmDelete" class="danger">Delete</button>
      </template>
    </ModalComponent>

    <!-- Wizard modal dengan multiple steps -->
    <button @click="modals.wizard = true">Open Wizard</button>
    <ModalComponent v-model:isOpen="modals.wizard" size="large">
      <template #header>
        <h3>🧙 Setup Wizard</h3>
        <div class="wizard-progress">
          <span :class="{ active: wizardStep >= 1 }">1</span>
          <span :class="{ active: wizardStep >= 2 }">2</span>
          <span :class="{ active: wizardStep >= 3 }">3</span>
        </div>
      </template>

      <div class="wizard-content">
        <div v-if="wizardStep === 1" class="wizard-step">
          <h4>Step 1: Basic Information</h4>
          <p>Enter your basic profile information.</p>
        </div>
        <div v-if="wizardStep === 2" class="wizard-step">
          <h4>Step 2: Preferences</h4>
          <p>Customize your experience preferences.</p>
        </div>
        <div v-if="wizardStep === 3" class="wizard-step">
          <h4>Step 3: Review & Confirm</h4>
          <p>Review your information before completing setup.</p>
        </div>
      </div>

      <template #footer>
        <button v-if="wizardStep > 1" @click="wizardStep--">Previous</button>
        <button v-if="wizardStep < 3" @click="wizardStep++" class="primary">Next</button>
        <button v-else @click="completeWizard" class="primary">Complete</button>
      </template>
    </ModalComponent>

    <!-- Custom modal dengan extension slots -->
    <button @click="modals.custom = true">Open Custom Modal</button>
    <ModalComponent v-model:isOpen="modals.custom" size="large">
      <!-- Extension slots -->
      <template #top-left>
        <span>📌</span>
      </template>
      <template #top-right>
        <span>⭐</span>
      </template>

      <template #header>
        <h3>🎨 Custom Styled Modal</h3>
      </template>

      <div class="custom-content">
        <div class="feature-grid">
          <div class="feature-card">
            <span>🚀</span>
            <h4>Fast Performance</h4>
            <p>Lightning fast modal rendering</p>
          </div>
          <div class="feature-card">
            <span>🎯</span>
            <h4>Precise Control</h4>
            <p>Fine-grained customization options</p>
          </div>
          <div class="feature-card">
            <span>💎</span>
            <h4>Premium Quality</h4>
            <p>Beautiful and accessible design</p>
          </div>
        </div>
      </div>

      <template #footer>
        <button @click="modals.custom = false">Close</button>
        <button @click="savePreferences" class="primary">Save Preferences</button>
      </template>
    </ModalComponent>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Component extension** - tambah fitur tanpa modifikasi core
- ✅ **Plugin architecture** - parent dapat "plugin-in" functionality
- ✅ **Flexible styling** - kustomisasi visual yang lengkap
- ✅ **Advanced patterns** - wizard, multi-step forms, dll

### 5. **Renderless Components & Logic Reusability**
```vue
<!-- DataFetcher.vue - Renderless component untuk data fetching -->
<script setup>
import { ref, onMounted } from 'vue'

const props = defineProps({
  url: {
    type: String,
    required: true
  },
  method: {
    type: String,
    default: 'GET'
  },
  autoFetch: {
    type: Boolean,
    default: true
  },
  cache: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits(['success', 'error', 'loading'])

// Fungsi 5: Renderless component untuk logic reuse
const data = ref(null)
const loading = ref(false)
const error = ref(null)
const cache = ref(new Map())

const fetchData = async () => {
  // Check cache first
  if (props.cache && cache.value.has(props.url)) {
    data.value = cache.value.get(props.url)
    emit('success', data.value)
    return data.value
  }

  loading.value = true
  error.value = null
  emit('loading', true)

  try {
    const response = await fetch(props.url, { method: props.method })

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }

    const result = await response.json()
    data.value = result

    // Cache the result
    if (props.cache) {
      cache.value.set(props.url, result)
    }

    emit('success', result)
    return result
  } catch (err) {
    error.value = err.message
    emit('error', err)
    throw err
  } finally {
    loading.value = false
    emit('loading', false)
  }
}

const refetch = () => {
  if (props.cache) {
    cache.value.delete(props.url)
  }
  return fetchData()
}

// Auto fetch
if (props.autoFetch) {
  onMounted(fetchData)
}

// Expose methods dan data ke parent
defineExpose({
  data,
  loading,
  error,
  fetchData,
  refetch
})
</script>

<template>
  <!-- Renderless component - hanya expose logic, tidak render UI -->
  <slot
    :data="data"
    :loading="loading"
    :error="error"
    :fetchData="fetchData"
    :refetch="refetch"
    :isSuccess="!loading && !error && data !== null"
    :isEmpty="!loading && !error && data !== null && (Array.isArray(data) ? data.length === 0 : Object.keys(data).length === 0)"
  >
    <!-- Default slot content -->
    <div v-if="loading" class="default-loading">
      <div class="spinner"></div>
      <p>Loading...</p>
    </div>

    <div v-else-if="error" class="default-error">
      <h3>Error</h3>
      <p>{{ error }}</p>
      <button @click="fetchData">Retry</button>
    </div>

    <div v-else-if="data" class="default-data">
      <pre>{{ JSON.stringify(data, null, 2) }}</pre>
    </div>
  </slot>
</template>

<style scoped>
.default-loading,
.default-error,
.default-data {
  padding: 2rem;
  text-align: center;
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e2e8f0;
  border-top: 4px solid #3b82f6;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>

<!-- Menggunakan DataFetcher untuk berbagai use cases -->
<script setup>
import { ref } from 'vue'
import DataFetcher from './DataFetcher.vue'

const apiUrl = ref('https://jsonplaceholder.typicode.com/users')
const refreshKey = ref(0)
</script>

<template>
  <div class="renderless-demo">
    <h2>Renderless Component Examples</h2>

    <!-- User list dengan custom loading -->
    <DataFetcher :url="apiUrl" :cache="true" :key="refreshKey">
      <template #default="{ data, loading, error, refetch, isSuccess, isEmpty }">
        <div class="user-list-container">
          <div class="list-header">
            <h3>User List</h3>
            <button @click="refetch">🔄 Refresh</button>
          </div>

          <!-- Custom loading state -->
          <div v-if="loading" class="custom-loading">
            <div class="loading-cards">
              <div v-for="i in 3" :key="i" class="skeleton-card">
                <div class="skeleton-avatar"></div>
                <div class="skeleton-content">
                  <div class="skeleton-line"></div>
                  <div class="skeleton-line short"></div>
                </div>
              </div>
            </div>
          </div>

          <!-- Custom error state -->
          <div v-else-if="error" class="custom-error">
            <div class="error-card">
              <span class="error-icon">⚠️</span>
              <h4>Failed to load users</h4>
              <p>{{ error }}</p>
              <button @click="refetch" class="retry-btn">Try Again</button>
            </div>
          </div>

          <!-- Custom data rendering -->
          <div v-else-if="isSuccess && !isEmpty" class="user-grid">
            <div v-for="user in data" :key="user.id" class="user-card">
              <div class="user-avatar">
                <img :src="`https://i.pravatar.cc/150?img=${user.id}` :alt="user.name" />
              </div>
              <div class="user-info">
                <h4>{{ user.name }}</h4>
                <p>{{ user.email }}</p>
                <span class="user-company">{{ user.company.name }}</span>
              </div>
              <div class="user-actions">
                <button>View Profile</button>
                <button>Send Message</button>
              </div>
            </div>
          </div>

          <!-- Empty state -->
          <div v-else-if="isEmpty" class="empty-state">
            <div class="empty-icon">👥</div>
            <h4>No users found</h4>
            <p>There are no users to display.</p>
          </div>
        </div>
      </template>
    </DataFetcher>

    <!-- Another use case: Simple status indicator -->
    <DataFetcher :url="apiUrl + '/1'">
      <template #default="{ data, loading, error, isSuccess }">
        <div class="status-indicator">
          <div class="status-dot" :class="{
            'loading': loading,
            'success': isSuccess,
            'error': error
          }"></div>
          <span>
            {{ loading ? 'Checking API...' : isSuccess ? 'API Online' : 'API Offline' }}
          </span>
        </div>
      </template>
    </DataFetcher>

    <!-- Another use case: Form data population -->
    <DataFetcher :url="apiUrl + '/1'">
      <template #default="{ data, loading, error }">
        <form class="user-form" @submit.prevent>
          <fieldset :disabled="loading || error">
            <legend>Edit User</legend>

            <div v-if="loading" class="form-loading">
              Loading user data...
            </div>

            <div v-else-if="error" class="form-error">
              Failed to load user data
            </div>

            <template v-else-if="data">
              <div class="form-group">
                <label>Name</label>
                <input :value="data.name" />
              </div>
              <div class="form-group">
                <label>Email</label>
                <input :value="data.email" type="email" />
              </div>
              <div class="form-group">
                <label>Phone</label>
                <input :value="data.phone" />
              </div>
              <div class="form-group">
                <label>Website</label>
                <input :value="data.website" />
              </div>
            </template>

            <div class="form-actions">
              <button type="submit" :disabled="loading || error">Save Changes</button>
            </div>
          </fieldset>
        </form>
      </template>
    </DataFetcher>
  </div>
</template>
```

**Keuntungan:**
- ✅ **Logic reuse** - gunakan logic yang sama di berbagai komponen
- ✅ **Presentation separation** - logic terpisah dari UI
- ✅ **Testability** - logic dapat diuji secara independen
- ✅ **Flexibility** - parent kontrol bagaimana data ditampilkan

## 🎯 Best Practices untuk Slots

### 1. **Provide Meaningful Fallback Content**
```vue
<template>
  <!-- ✅ GOOD: Berikan fallback yang berguna -->
  <slot name="header">
    <h2>Default Title</h2>
  </slot>

  <!-- ❌ AVOID: Fallback yang tidak berguna -->
  <slot name="content">
    <div></div> <!-- Empty fallback -->
  </slot>
</template>
```

### 2. **Use Descriptive Slot Names**
```vue
<template>
  <!-- ✅ GOOD: Nama yang deskriptif -->
  <slot name="user-avatar"></slot>
  <slot name="user-info"></slot>
  <slot name="user-actions"></slot>

  <!-- ❌ AVOID: Nama yang tidak jelas -->
  <slot name="slot1"></slot>
  <slot name="data"></slot>
</template>
```

### 3. **Validate Scoped Slot Props**
```vue
<script setup>
// ✅ GOOD: Validasi data sebelum passing ke slot
const props = defineProps({
  users: {
    type: Array,
    required: true,
    validator: (users) => users.every(user => user.id && user.name)
  }
})
</script>

<template>
  <slot v-for="user in users" :user="user" :isValid="true"></slot>
</template>
```

## 🎉 Kesimpulan

Slots adalah fitur **fundamental dan powerful** dalam Vue.js yang memberikan:

- ✅ **Content distribution** - distribusikan konten dengan fleksibel
- ✅ **Component customization** - kustomisasi tampilan dan behavior
- ✅ **Data passing** - scoped slots untuk sharing data
- ✅ **Extension patterns** - plugin architecture dan component extension
- ✅ **Logic reuse** - renderless components untuk reusable logic

**Referensi Lengkap:**
- Dokumentasi resmi Vue.js tentang Slots
- Vue.js Component Slots Guide
- Vue.js Composition API Documentation

---

**🔥 Tips Pro:**
- Selalu sediakan **fallback content** yang meaningful
- Gunakan **descriptive slot names** untuk clarity
- Manfaatkan **scoped slots** untuk data sharing
- Implement **conditional slots** untuk optional content
- Gunakan **renderless components** untuk logic reuse

**Referensi:**
- Vue.js Official Documentation
- Vue.js GitHub Repository
- Vue.js Style Guide