# 🚀 Panduan Cepat: Todo List Vue.js untuk Pemula

## 🎯 Target: 5 Menit Memahami & Membuat Aplikasi

### 📋 Step 1: Persiapan (1 Menit)

**Install Node.js (belum ada?)**
```bash
# Cek apakah Node.js sudah terinstall
node --version

# Belum ada? Download di nodejs.org
```

### 📋 Step 2: Setup Project (2 Menit)

```bash
# Buat project baru dengan Vite
npm create vue@latest todo-app

# Masuk ke folder project
cd todo-app

# Install dependencies
npm install
```

### 📋 Step 3: Ganti Kode (1 Menit)

**Hapus semua kode di `src/App.vue` dan ganti dengan:**

```vue
<script setup>
import { ref } from "vue";

const newTask = ref("");
const tasks = ref([]);

const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
};
</script>

<template>
  <div class="todo-app">
    <h1>📝 Todo List Saya</h1>

    <div class="input-section">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Tambah tugas baru..."
      />
      <button @click="addTask">➕ Tambah</button>
    </div>

    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index">
        ✅ {{ task }}
        <button @click="removeTask(index)">🗑️ Hapus</button>
      </li>
    </ul>

    <div v-if="tasks.length === 0" class="empty-state">
      🎯 Belum ada tugas. Tambah tugas pertama!
    </div>
  </div>
</template>

<style scoped>
.todo-app {
  max-width: 500px;
  margin: 50px auto;
  padding: 30px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 15px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.2);
  color: white;
}

h1 {
  text-align: center;
  margin-bottom: 30px;
  font-size: 2.5em;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
}

input {
  flex: 1;
  padding: 15px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
}

button {
  padding: 15px 20px;
  border: none;
  border-radius: 8px;
  background: #4CAF50;
  color: white;
  cursor: pointer;
  font-weight: bold;
  transition: all 0.3s;
}

button:hover {
  background: #45a049;
  transform: translateY(-2px);
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-list li {
  background: rgba(255,255,255,0.1);
  margin: 10px 0;
  padding: 15px;
  border-radius: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  backdrop-filter: blur(10px);
}

.task-list li button {
  background: #f44336;
  padding: 8px 15px;
}

.task-list li button:hover {
  background: #da190b;
}

.empty-state {
  text-align: center;
  padding: 40px;
  font-size: 1.2em;
  opacity: 0.8;
}
</style>
```

### 📋 Step 4: Jalankan Aplikasi (1 Menit)

```bash
# Start development server
npm run dev
```

**Buka browser:** `http://localhost:5173`

🎉 **Selesai! Aplikasi Todo List Anda siap digunakan!**

## 🔍 Penjelasan Singkat Kode

### ✨ Konsep Utama Vue.js

1. **`ref()`** - Membuat data yang bisa berubah otomatis
2. **`v-model`** - Hubungkan input dengan data
3. **`v-for`** - Tampilkan data array
4. **`@click`** - Tangani klik tombol
5. **`@keyup.enter`** - Tangani tombol Enter

### 📝 Struktur Kode

```vue
<script setup>
// 1. Import & Data
import { ref } from "vue";
const newTask = ref("");   // Input user
const tasks = ref([]);     // Daftar tugas

// 2. Fungsi
const addTask = () => { /* tambah tugas */ };
const removeTask = (index) => { /* hapus tugas */ };
</script>

<template>
<!-- 3. Tampilan HTML -->
</template>

<style scoped>
/* 4. Styling CSS */
</style>
```

## 🎯 Fitur Tambahan (Coba tambahkan!)

### 1. Mark as Complete
```vue
// Tambah di script setup
const toggleComplete = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
};

// Modifikasi addTask
const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      text: newTask.value,
      completed: false
    });
    newTask.value = "";
  }
};
```

### 2. Filter Tasks
```vue
// Tambah di script setup
const filter = ref('all');

const filteredTasks = computed(() => {
  if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed);
  }
  if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed);
  }
  return tasks.value;
});
```

### 3. Local Storage
```vue
// Tambah di script setup
const saveTasks = () => {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
};

const loadTasks = () => {
  const saved = localStorage.getItem('tasks');
  if (saved) {
    tasks.value = JSON.parse(saved);
  }
};

// Load saat pertama kali
loadTasks();

// Save setiap ada perubahan
watch(tasks, saveTasks, { deep: true });
```

## 🐞 Masalah Umum & Solusi

| Masalah | Solusi |
|---------|--------|
| Input tidak bisa mengetik | Cek `v-model` sudah benar |
| Tombol tidak berfungsi | Cek `@click` function ada |
| List tidak muncul | Cek `v-for` dan `:key` |
| Style tidak berubah | Cek `scoped` CSS |

## 🚀 Next Steps

1. **Component System** - Pecah jadi beberapa komponen
2. **Vue Router** - Tambah halaman baru
3. **Pinia** - State management
4. **API Integration** - Hubung ke backend
5. **Testing** - Unit & E2E testing

---

**🎯 Goal 5 Menit Tercapai!**
Sekarang Anda punya aplikasi Todo List yang berfungsi dengan Vue.js!

**💡 Tip:** Lanjutkan dengan menambah fitur-fitur tambahan di atas untuk memperdalam pemahaman Vue.js.

**🔗 Resources:** [Vue.js Docs](https://vuejs.org/) | [Vite Docs](https://vitejs.dev/)