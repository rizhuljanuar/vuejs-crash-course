# 📚 Implementasi Step-by-Step Todo List Vue.js

## 🎯 Panduan Detail Membuat Todo List dari Nol

### 📋 Step 1: Setup Environment Development

#### 1.1 Install Node.js
```bash
# Download Node.js dari https://nodejs.org/
# Pilih versi LTS (Recommended for Most Users)

# Verifikasi instalasi
node --version
npm --version
```

#### 1.2 Buat Project Vue dengan Vite
```bash
# Buat project baru
npm create vue@latest todo-list-app

# Saat ditanya, pilih:
# ✅ TypeScript? No
# ✅ JSX Support? No
# ✅ Vue Router? No
# ✅ Pinia? No
# ✅ Vitest? No
# ✅ E2E Testing? No
# ✅ ESLint? Yes
# ✅ Prettier? Yes

# Masuk ke folder project
cd todo-list-app

# Install dependencies
npm install
```

#### 1.3 Jalankan Development Server
```bash
npm run dev
```
**Result:** Aplikasi Vue default berjalan di `http://localhost:5173`

---

### 📋 Step 2: Persiapan Struktur Project

#### 2.1 Lihat Struktur Folder
```
todo-list-app/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   ├── App.vue
│   └── main.js
├── package.json
└── README.md
```

#### 2.2 Bersihkan Default Project
**Edit `src/App.vue`:**
```vue
<!-- Hapus semua isi dan ganti dengan template kosong -->
<template>
  <div id="app">
    <h1>Todo List App</h1>
  </div>
</template>

<script setup>
// Kosongkan script untuk sementara
</script>

<style>
/* Kosongkan style untuk sementara */
</style>
```

**Result:** Aplikasi sekarang menampilkan judul "Todo List App"

---

### 📋 Step 3: Implementasi Dasar Todo List

#### 3.1 Tambahkan Reactive Data
**Edit `src/App.vue` - Script section:**
```vue
<script setup>
import { ref } from "vue";

// 1. Reactive variables
const newTask = ref("");        // Input untuk task baru
const tasks = ref([]);          // Array untuk menyimpan tasks
</script>
```

**Penjelasan:**
- `ref()` membuat variable reactive (otomatis update UI)
- `newTask` untuk menyimpan input dari user
- `tasks` untuk menyimpan daftar semua tasks

#### 3.2 Buat UI Input Task
**Edit `src/App.vue` - Template section:**
```vue
<template>
  <div id="app">
    <h1>📝 Todo List</h1>

    <!-- Input Section -->
    <div class="input-section">
      <input
        type="text"
        v-model="newTask"
        placeholder="Tambah task baru..."
        @keyup.enter="addTask"
      />
      <button @click="addTask">➕ Tambah</button>
    </div>

    <!-- Debug Info (sementara) -->
    <div class="debug">
      <p>Input: {{ newTask }}</p>
      <p>Tasks: {{ tasks }}</p>
    </div>
  </div>
</template>
```

**Penjelasan:**
- `v-model="newTask"`: Two-way binding dengan input
- `@keyup.enter="addTask"`: Jalankan function saat Enter ditekan
- `@click="addTask"`: Jalankan function saat tombol diklik
- `{{ }}`: Menampilkan data di template

#### 3.3 Implementasi Fungsi Add Task
**Edit `src/App.vue` - Tambah function:**
```vue
<script setup>
import { ref } from "vue";

const newTask = ref("");
const tasks = ref([]);

// Fungsi untuk menambah task
const addTask = () => {
  // Validasi: input tidak boleh kosong
  if (newTask.value.trim() === "") {
    alert("Task tidak boleh kosong!");
    return;
  }

  // Tambah task ke array
  tasks.value.push(newTask.value);

  // Kosongkan input
  newTask.value = "";

  console.log("Task ditambahkan:", tasks.value);
};
</script>
```

**Result:** Sekarang bisa menambah task dan melihat hasilnya di debug info

---

### 📋 Step 4: Tampilkan Daftar Tasks

#### 4.1 Buat Task List Display
**Edit `src/App.vue` - Template section:**
```vue
<template>
  <div id="app">
    <h1>📝 Todo List</h1>

    <div class="input-section">
      <input
        type="text"
        v-model="newTask"
        placeholder="Tambah task baru..."
        @keyup.enter="addTask"
      />
      <button @click="addTask">➕ Tambah</button>
    </div>

    <!-- Task List -->
    <div class="task-list">
      <h3>Daftar Tasks ({{ tasks.length }})</h3>

      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span>{{ index + 1 }}. {{ task }}</span>
        </li>
      </ul>

      <!-- Empty State -->
      <div v-if="tasks.length === 0" class="empty-state">
        🎯 Belum ada tasks. Tambah task pertama!
      </div>
    </div>
  </div>
</template>
```

**Penjelasan:**
- `v-for="(task, index) in tasks"`: Looping untuk setiap task
- `:key="index"`: Unique identifier untuk Vue (wajib untuk v-for)
- `v-if="tasks.length === 0"`: Tampilkan pesan jika tidak ada tasks
- `{{ tasks.length }}`: Jumlah total tasks

---

### 📋 Step 5: Implementasi Delete Task

#### 5.1 Tambah Tombol Delete
**Edit `src/App.vue` - Template (bagian v-for):**
```vue
<li v-for="(task, index) in tasks" :key="index">
  <span>{{ index + 1 }}. {{ task }}</span>
  <button
    class="delete-btn"
    @click="removeTask(index)"
  >
    🗑️ Hapus
  </button>
</li>
```

#### 5.2 Implementasi Fungsi Remove
**Edit `src/App.vue` - Script section:**
```vue
<script setup>
import { ref } from "vue";

const newTask = ref("");
const tasks = ref([]);

const addTask = () => {
  if (newTask.value.trim() === "") {
    alert("Task tidak boleh kosong!");
    return;
  }
  tasks.value.push(newTask.value);
  newTask.value = "";
};

// Fungsi untuk menghapus task
const removeTask = (index) => {
  // Konfirmasi sebelum hapus
  if (confirm(`Hapus task "${tasks.value[index]}"?`)) {
    // Hapus task dari array
    tasks.value.splice(index, 1);
  }
};
</script>
```

**Penjelasan:**
- `confirm()`: Konfirmasi dialog sebelum hapus
- `splice(index, 1)`: Hapus 1 element dari array pada posisi index

---

### 📋 Step 6: Styling Aplikasi

#### 6.1 Tambah Basic Styling
**Edit `src/App.vue` - Style section:**
```vue
<style>
/* Reset & Global Styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  background-color: #f5f5f5;
}

#app {
  max-width: 600px;
  margin: 50px auto;
  padding: 30px;
  background: white;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
  font-size: 2em;
}

.input-section {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
}

.input-section input {
  flex: 1;
  padding: 15px;
  border: 2px solid #ddd;
  border-radius: 5px;
  font-size: 16px;
}

.input-section input:focus {
  outline: none;
  border-color: #4CAF50;
}

.input-section button {
  padding: 15px 20px;
  background: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

.input-section button:hover {
  background: #45a049;
}

.task-list h3 {
  color: #333;
  margin-bottom: 15px;
}

.task-list ul {
  list-style: none;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px;
  margin: 10px 0;
  background: #f9f9f9;
  border-radius: 5px;
  border-left: 4px solid #4CAF50;
}

.delete-btn {
  background: #f44336;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 3px;
  cursor: pointer;
}

.delete-btn:hover {
  background: #da190b;
}

.empty-state {
  text-align: center;
  padding: 40px;
  color: #666;
  font-style: italic;
}
</style>
```

**Result:** Aplikasi sekarang memiliki tampilan yang profesional dan user-friendly

---

### 📋 Step 7: Testing & Validasi

#### 7.1 Test Semua Fitur
**Coba semua fungsi:**
- ✅ Menambah task baru
- ✅ Menambah task dengan tombol Enter
- ✅ Menampilkan daftar tasks
- ✅ Menampilkan jumlah tasks
- ✅ Menghapus task
- ✅ Validasi input kosong
- ✅ Empty state saat tidak ada tasks

#### 7.2 Debug Issues
**Common issues & fixes:**
```vue
<!-- Issue: Input tidak responsif -->
<!-- Fix: Pastikan v-model benar -->
<input v-model="newTask" />

<!-- Issue: Task tidak muncul -->
<!-- Fix: Pastikan v-for dan :key benar -->
<li v-for="(task, index) in tasks" :key="index">

<!-- Issue: Tombol tidak berfungsi -->
<!-- Fix: Pastikan function ada dan event handler benar -->
<button @click="addTask">
```

---

### 📋 Step 8: Refactor ke Component

#### 8.1 Buat Component TodoList
**Buat file `src/components/TodoList.vue`:**
```vue
<template>
  <div class="todo-list">
    <h2>📝 Todo List</h2>

    <div class="input-section">
      <input
        type="text"
        v-model="newTask"
        placeholder="Tambah task baru..."
        @keyup.enter="addTask"
      />
      <button @click="addTask">➕ Tambah</button>
    </div>

    <div class="tasks">
      <ul>
        <li v-for="(task, index) in tasks" :key="index">
          <span>{{ task }}</span>
          <button class="delete-btn" @click="removeTask(index)">
            🗑️
          </button>
        </li>
      </ul>

      <div v-if="tasks.length === 0" class="empty-state">
        🎯 Belum ada tasks
      </div>
    </div>
  </div>
</template>

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

<style scoped>
.todo-list {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

/* ... copy semua styles dari sebelumnya ... */
</style>
```

#### 8.2 Update App.vue
**Edit `src/App.vue` menjadi:**
```vue
<template>
  <div id="app">
    <header>
      <h1>🚀 Vue.js Todo App</h1>
      <p>Project pertama dengan Vue.js 3</p>
    </header>

    <main>
      <TodoList />
    </main>

    <footer>
      <p>Made with ❤️ using Vue.js</p>
    </footer>
  </div>
</template>

<script setup>
import TodoList from './components/TodoList.vue';
</script>

<style>
/* Global styles */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
}

#app {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

header {
  text-align: center;
  color: white;
  margin-bottom: 40px;
}

header h1 {
  font-size: 3em;
  margin-bottom: 10px;
}

footer {
  text-align: center;
  color: white;
  margin-top: 40px;
  opacity: 0.8;
}
</style>
```

**Result:** Aplikasi sekarang lebih terstruktur dengan component-based architecture!

---

### 📋 Step 9: Fitur Tambahan (Optional)

#### 9.1 Mark as Complete
```vue
<!-- Tambah di TodoList.vue -->
<template>
  <li v-for="(task, index) in tasks" :key="index">
    <span :class="{ completed: task.completed }">{{ task.text }}</span>
    <div class="task-actions">
      <button @click="toggleComplete(index)">
        {{ task.completed ? '↩️' : '✅' }}
      </button>
      <button class="delete-btn" @click="removeTask(index)">🗑️</button>
    </div>
  </li>
</template>

<script setup>
const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push({
      text: newTask.value,
      completed: false
    });
    newTask.value = "";
  }
};

const toggleComplete = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  opacity: 0.6;
}

.task-actions {
  display: flex;
  gap: 5px;
}
</style>
```

#### 9.2 Local Storage
```vue
<script setup>
import { ref, watch } from "vue";

// Load dari localStorage saat pertama kali
const savedTasks = localStorage.getItem('tasks');
const tasks = ref(savedTasks ? JSON.parse(savedTasks) : []);

// Save ke localStorage setiap ada perubahan
watch(tasks, (newTasks) => {
  localStorage.setItem('tasks', JSON.stringify(newTasks));
}, { deep: true });
</script>
```

---

### 📋 Step 10: Final Deployment

#### 10.1 Build untuk Production
```bash
# Build aplikasi
npm run build

# Hasilnya ada di folder /dist
```

#### 10.2 Deploy ke Netlify/Vercel
1. Push ke GitHub
2. Connect ke Netlify/Vercel
3. Auto deploy dari GitHub

---

## 🎉 Hasil Akhir

✅ **Aplikasi Todo List Lengkap dengan:**
- Tambah task baru
- Hapus task
- Mark task as complete (optional)
- Local storage persistence (optional)
- Responsive design
- Component-based architecture
- Clean code structure

## 📚 Yang Telah Dipelajari

1. **Vue.js Fundamentals**
   - Composition API (`<script setup>`)
   - Reactive data (`ref()`)
   - Directives (`v-model`, `v-for`, `v-if`)
   - Event handling (`@click`, `@keyup`)

2. **Modern Web Development**
   - Component-based architecture
   - Reactive programming
   - DOM manipulation dengan Vue
   - CSS styling dengan scoped styles

3. **Project Structure**
   - File organization
   - Component separation
   - Best practices

## 🚀 Next Steps

1. **Vue Router** - Multi-page aplikasi
2. **Pinia** - State management
3. **API Integration** - Backend connection
4. **Testing** - Unit & E2E testing
5. **Advanced Features** - Drag & drop, categories, dll

---

**🎯 Mission Accomplished!**
Anda berhasil membuat aplikasi Todo List yang fully functional dengan Vue.js!

**💡 Pro Tip:** Lanjutkan dengan menambah fitur-fitur menarik atau mulai project Vue.js lainnya!