# Todo List Project - Panduan Lengkap untuk Pemula

## 📝 Deskripsi Project

Project Todo List adalah aplikasi web sederhana yang dibuat menggunakan Vue.js 3 dengan Composition API. Aplikasi ini memungkinkan pengguna untuk menambah, menampilkan, dan menghapus tugas (tasks) dalam daftar to-do mereka.

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan project ini, Anda akan memahami:
- Konsep dasar Vue.js 3 dan Composition API
- Cara menggunakan reactive references (`ref`)
- Event handling di Vue.js
- Conditional rendering dan list rendering
- Styling dengan CSS Scoped di Vue.js
- Struktur project Vue.js yang sederhana

## 📋 Fitur Aplikasi

1. **Menambah Task**: Pengguna dapat menambahkan tugas baru melalui input field
2. **Menampilkan Task**: Semua tugas ditampilkan dalam bentuk list
3. **Menghapus Task**: Setiap task memiliki tombol remove untuk menghapusnya
4. **Responsive Design**: Aplikasi memiliki tampilan yang bersih dan responsif

## 🛠️ Persyaratan Sistem

Sebelum memulai, pastikan Anda telah menginstall:
- Node.js (versi 14 atau lebih tinggi)
- npm (Node Package Manager) atau yarn
- Text editor (Visual Studio Code direkomendasikan)
- Browser modern (Chrome, Firefox, Safari, atau Edge)

## 📂 Struktur Project

```
1. Todo List/
├── App.vue              # Komponen utama aplikasi
├── components/
│   └── TodoList.vue     # Komponen untuk fungsi Todo List
└── README.md           # Dokumentasi project
```

## 🚀 Langkah-Langkah Pembuatan Project

### Langkah 1: Setup Environment

1. **Install Node.js**
   - Download Node.js dari [nodejs.org](https://nodejs.org/)
   - Pilih versi LTS (Long Term Support)
   - Ikuti proses instalasi sesuai sistem operasi Anda

2. **Verifikasi Instalasi**
   ```bash
   node --version
   npm --version
   ```

3. **Install Vue CLI** (opsional, jika ingin menggunakan template)
   ```bash
   npm install -g @vue/cli
   ```

### Langkah 2: Membuat Project Baru

**Cara 1: Menggunakan Vite (Direkomendasikan)**
```bash
npm create vue@latest todo-list-app
cd todo-list-app
npm install
```

**Cara 2: Menggunakan Vue CLI**
```bash
vue create todo-list-app
cd todo-list-app
```

### Langkah 3: Memahami Struktur File Vue

Sebuah file Vue memiliki 3 bagian utama:
1. **`<script setup>`**: Bagian logika JavaScript
2. **`<template>`**: Bagian HTML untuk menampilkan UI
3. **`<style scoped>`**: Bagian CSS untuk styling

### Langkah 4: Membuat Komponen TodoList

**File: `components/TodoList.vue`**

#### Bagian Script Setup
```vue
<script setup>
import { ref } from "vue";

// Reactive variables
const newTask = ref("");    // Untuk menyimpan input task baru
const tasks = ref([]);      // Array untuk menyimpan semua tasks

// Fungsi untuk menambah task
const addTask = () => {
  if (newTask.value.trim() !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

// Fungsi untuk menghapus task
const removeTask = (index) => {
  tasks.value.splice(index, 1);
};
</script>
```

#### Penjelasan Kode:

1. **`import { ref } from "vue"`**
   - Mengimpor `ref` dari Vue.js
   - `ref` digunakan untuk membuat reactive variables

2. **`const newTask = ref("")`**
   - Membuat variable reactive untuk menyimpan input task
   - Nilai awalnya adalah string kosong

3. **`const tasks = ref([])`**
   - Membuat array reactive untuk menyimpan daftar tasks
   - Nilai awalnya adalah array kosong

4. **`addTask()` Function**
   - Mengecek apakah input tidak kosong (menggunakan `trim()`)
   - Menambah task ke array jika valid
   - Mengosongkan input setelah menambah

5. **`removeTask()` Function**
   - Menerima parameter `index` untuk menentukan task mana yang akan dihapus
   - Menggunakan `splice()` untuk menghapus element dari array

### Langkah 5: Membuat Template HTML

```vue
<template>
  <div class="todo-app">
    <div class="task-input">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add a new task"
      />
      <button @click="addTask">Add To Do</button>
    </div>

    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" class="task-item">
        {{ task }}
        <button @click="removeTask(index)" class="remove-button">Remove</button>
      </li>
    </ul>
  </div>
</template>
```

#### Penjelasan Template:

1. **`v-model="newTask"`**
   - Two-way data binding antara input dan variable `newTask`

2. **`@keyup.enter="addTask"`**
   - Event listener untuk menjalankan `addTask()` saat tombol Enter ditekan

3. **`@click="addTask"`**
   - Event listener untuk menjalankan `addTask()` saat tombol diklik

4. **`v-for="(task, index) in tasks"`**
   - Looping untuk menampilkan setiap task dalam array
   - `task` adalah nilai task
   - `index` adalah posisi task dalam array

5. **`:key="index"`**
   - Memberikan unique key untuk setiap element dalam loop

6. **`@click="removeTask(index)"`**
   - Event listener untuk menghapus task dengan index tertentu

### Langkah 6: Styling dengan CSS

```vue
<style scoped>
.todo-app {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.task-input {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 8px;
  font-size: 14px;
}

button {
  padding: 8px 12px;
  font-size: 14px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 8px 0;
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.remove-button {
  padding: 6px 10px;
  font-size: 12px;
  background-color: #e74c3c;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.remove-button:hover {
  background-color: #c0392b;
}
</style>
```

#### Penjelasan CSS:

1. **`scoped`**
   - CSS hanya berlaku untuk komponen ini saja
   - Tidak akan mempengaruhi komponen lain

2. **`.todo-app`**
   - Container utama dengan center alignment dan shadow

3. **`.task-input`**
   - Flexbox layout untuk input dan button

4. **`.task-item`**
   - Styling untuk setiap item task dengan flexbox layout

### Langkah 7: Membuat App.vue

**File: `App.vue`**
```vue
<script setup>
import TodoList from './components/TodoList.vue'
</script>

<template>
  <TodoList />
</template>
```

#### Penjelasan App.vue:

1. **Import Komponen**
   - Mengimpor komponen `TodoList` dari folder components

2. **Render Komponen**
   - Menampilkan komponen `TodoList` dalam template

### Langkah 8: Menjalankan Aplikasi

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Jalankan Development Server**
   ```bash
   npm run dev
   ```

3. **Buka Browser**
   - Buka alamat yang ditampilkan di terminal (biasanya `http://localhost:5173`)
   - Aplikasi Todo List siap digunakan!

## 🧠 Konsep Vue.js yang Dipelajari

### 1. Composition API
- Menggunakan `<script setup>` untuk syntax yang lebih ringkas
- Mengorganisir logika dengan cara yang lebih modular

### 2. Reactive References (`ref`)
- Membuat variable reactive yang otomatis update UI saat berubah
- Mengakses nilai dengan `.value` (dalam JavaScript)

### 3. Event Handling
- `@click` untuk menangani click events
- `@keyup.enter` untuk menangani keyboard events

### 4. Data Binding
- `v-model` untuk two-way data binding
- `{{ }}` untuk one-way data display

### 5. List Rendering
- `v-for` untuk rendering array/list
- Pentingnya `:key` untuk performance

## 🔧 Common Issues & Troubleshooting

### Issue 1: Aplikasi tidak muncul
**Solution**: Pastikan development server berjalan dan buka alamat yang benar

### Issue 2: Tidak bisa menambah task
**Solution**: Cek console untuk error, pastikan `v-model` terhubung dengan benar

### Issue 3: Styling tidak berfungsi
**Solution**: Pastikan menggunakan `scoped` CSS atau cek selector CSS

### Issue 4: Error "ref is not defined"
**Solution**: Pastikan mengimport `ref` dari Vue dengan benar

## 🚀 Enhancement Ideas

Setelah menyelesaikan basic todo list, Anda bisa menambahkan fitur:

1. **Local Storage**: Simpan tasks di browser
2. **Task Completion**: Tandai task sebagai selesai
3. **Edit Task**: Edit task yang sudah ada
4. **Filter Tasks**: Filter task berdasarkan status
5. **Drag & Drop**: Ubah urutan task dengan drag & drop
6. **Categories**: Tambahkan kategori untuk task
7. **Due Dates**: Tambahkan tanggal deadline

## 📚 Sumber Belajar Tambahan

- [Vue.js Official Documentation](https://vuejs.org/)
- [Vue.js Composition API Guide](https://vuejs.org/guide/extras/composition-api-faq.html)
- [Vite Documentation](https://vitejs.dev/)
- [MDN Web Docs](https://developer.mozilla.org/)

## ✅ Checklist Selesai

- [ ] Setup environment development
- [ ] Buat project Vue baru
- [ ] Pahami struktur file Vue
- [ ] Implementasi komponen TodoList
- [ ] Tambahkan fungsi add task
- [ ] Tambahkan fungsi remove task
- [ ] Styling aplikasi
- [ ] Testing semua fitur
- [ ] Deploy (opsional)

## 🎉 Selamat!

Anda telah berhasil membuat aplikasi Todo List pertama dengan Vue.js! Project ini memberikan fondasi yang kuat untuk memahami konsep-konsep dasar Vue.js dan membangun aplikasi yang lebih kompleks di masa depan.

---

**Happy Coding! 🚀**