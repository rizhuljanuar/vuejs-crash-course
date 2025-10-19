# Dokumentasi Struktur Folder Proyek Vue.js

## Panduan Lengkap Struktur Folder Vue.js + Vite

Dokumentasi ini menjelaskan struktur folder dan file dalam proyek Vue.js yang dibuat dengan Vite. Proyek ini menggunakan Vue 3 dengan setup modern yang optimal untuk pengembangan aplikasi web.

## 📁 Struktur Folder Lengkap

```
my-vue/
├── .eslintrc.cjs              # Konfigurasi ESLint untuk kualitas kode
├── .gitignore                 # File dan folder yang diabaikan oleh Git
├── .prettierrc.json           # Konfigurasi Prettier untuk formatting kode
├── index.html                 # File HTML utama (entry point)
├── jsconfig.json              # Konfigurasi JavaScript untuk IDE
├── package.json               # Konfigurasi proyek dan dependencies
├── package-lock.json          # Lock file untuk npm (versi dependencies)
├── vite.config.js             # Konfigurasi Vite build tool
└── src/                       # Folder source code utama
    ├── assets/                # Asset statis (gambar, CSS, font)
    ├── components/            # Komponen Vue reusable
    │   └── icons/             # Komponen icon
    ├── App.vue                # Komponen root aplikasi
    └── main.js                # Entry point aplikasi Vue
```

## 📋 Penjelasan Setiap File

### 1. **File Konfigurasi Utama**

#### 📄 `package.json`
File konfigurasi utama proyek yang berisi:
- **Informasi proyek**: nama, versi, deskripsi
- **Scripts**: perintah untuk development, build, testing
- **Dependencies**: library yang dibutuhkan aplikasi
- **DevDependencies**: tools untuk development

```json
{
  "name": "my-vue",
  "version": "0.0.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint . --ext .vue,.js,.jsx,.cjs,.mjs --fix --ignore-path .gitignore",
    "format": "prettier --write src/"
  }
}
```

#### 📄 `vite.config.js`
Konfigurasi build tool Vite:
- Plugin Vue
- Server development
- Build optimization
- Alias paths

```javascript
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})
```

#### 📄 `index.html`
File HTML utama yang berisi:
- Container untuk aplikasi Vue (`<div id="app"></div>`)
- Meta tags dan title
- Link ke CSS/JS external

### 2. **File Kualitas Kode**

#### 📄 `.eslintrc.cjs`
Konfigurasi ESLint untuk:
- Deteksi error dan warning
- Formatting otomatis
- Aturan coding style

#### 📄 `.prettierrc.json`
Konfigurasi Prettier untuk:
- Konsistensi formatting kode
- Indentasi, quotes, semicolon
- Line break dan spacing

### 3. **Folder `src/` - Source Code Utama**

#### 📁 `src/assets/`
Berisi file statis:
- **Gambar**: PNG, JPG, SVG, ICO
- **Styles**: CSS, SCSS, LESS
- **Font**: TTF, WOFF, WOFF2
- **Data JSON**: Konfigurasi atau data statis

#### 📁 `src/components/`
Komponen Vue reusable:
- **Atomic components**: Button, Input, Card
- **Molecular components**: Form, Navbar
- **Organism components**: Header, Sidebar
- **Template components**: Layout, Pages

#### 📁 `src/components/icons/`
Komponen icon SVG:
- **IconDocumentation.vue** - Icon dokumentasi
- **IconSupport.vue** - Icon support
- **IconTooling.vue** - Icon tools
- **IconEcosystem.vue** - Icon ecosystem
- **IconCommunity.vue** - Icon community

#### 📄 `src/App.vue`
Komponen root aplikasi:
- Layout utama
- Router view (jika menggunakan Vue Router)
- Global styles
- State management initialization

#### 📄 `src/main.js`
Entry point aplikasi:
- Inisialisasi Vue app
- Registration plugins
- Global components
- Mount aplikasi ke DOM

```javascript
import { createApp } from 'vue'
import './assets/main.css'
import App from './App.vue'

createApp(App).mount('#app')
```

## 🚀 Setup dan Perintah

### Menggunakan npm (Default)

```bash
# Install dependencies
npm install

# Development server
npm run dev

# Build untuk produksi
npm run build

# Preview build produksi
npm run preview

# Linting kode
npm run lint

# Format kode
npm run format
```

### Menggunakan Yarn

```bash
# Install dependencies
yarn install

# Development server
yarn dev

# Build untuk produksi
yarn build

# Preview build produksi
yarn preview

# Linting kode
yarn lint

# Format kode
yarn format
```

### Menggunakan pnpm

```bash
# Install dependencies
pnpm install

# Development server
pnpm dev

# Build untuk produksi
pnpm build

# Preview build produksi
pnpm preview

# Linting kode
pnpm lint

# Format kode
pnpm format
```

### Menggunakan Bun (Package Manager Modern)

```bash
# Install dependencies
bun install

# Development server
bun dev

# Build untuk produksi
bun run build

# Preview build produksi
bun run preview

# Linting kode
bun run lint

# Format kode
bun run format
```

**Keuntungan menggunakan Bun:**
- 🚀 Instalasi dependencies 10x lebih cepat
- 💾 Hemat memori dan ruang disk
- ⚡ Hot reload lebih responsif
- 🔧 All-in-one JavaScript toolkit

## 🛠️ Setup IDE yang Direkomendasikan

### Visual Studio Code
Install extensions berikut:
1. **Volar** - Vue language support
2. **TypeScript Vue Plugin (Volar)** - TypeScript support
3. **ESLint** - Linting integration
4. **Prettier** - Code formatting
5. **Vue VSCode Snippets** - Vue snippets

### VS Code Settings
Tambahkan ke `.vscode/settings.json`:
```json
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

## 📦 Dependencies

### Dependencies Utama
- **`vue` (^3.3.11)** - Framework Vue.js 3

### DevDependencies
- **`@vitejs/plugin-vue`** - Plugin Vue untuk Vite
- **`vite` (^5.0.10)** - Build tool modern
- **`eslint`** - Linting JavaScript
- **`eslint-plugin-vue`** - Linting khusus Vue
- **`prettier`** - Code formatter
- **`@vue/eslint-config-prettier`** - Integrasi ESLint + Prettier

## 🔄 Alur Kerja Development

### 1. Setup Awal
```bash
# Clone proyek
git clone <repository-url>

# Masuk ke folder
cd my-vue

# Install dependencies (pilih salah satu)
npm install          # atau
yarn install         # atau
pnpm install         # atau
bun install
```

### 2. Development
```bash
# Jalankan development server
npm run dev          # atau
yarn dev             # atau
pnpm dev             # atau
bun dev
```

### 3. Build untuk Produksi
```bash
# Build aplikasi
npm run build        # atau
yarn build           # atau
pnpm build           # atau
bun run build

# Preview hasil build
npm run preview      # atau
yarn preview         # atau
pnpm preview         # atau
bun run preview
```

## 🎯 Best Practices

### 1. **Struktur Folder**
- Gunakan **kebab-case** untuk nama folder
- Kelompokkan komponen berdasarkan fitur
- Pisahkan components, views, dan services

### 2. **Penamaan File**
- Komponen: **PascalCase** (ExampleComponent.vue)
- File lain: **kebab-case** (example-service.js)
- Assets: **lowercase** dengan deskripsi jelas

### 3. **Organisasi Kode**
- Satu komponen per file
- Export default untuk komponen utama
- Named exports untuk utilities

### 4. **Asset Management**
- Import assets dengan path relatif
- Gunakan `@` alias untuk `src/` folder
- Optimize gambar dengan format modern

## 🔧 Konfigurasi Tambahan

### Environment Variables
Buat file `.env.local`:
```
VITE_API_URL=http://localhost:3000
VITE_APP_TITLE=My Vue App
```

### Path Aliases
Di `vite.config.js`:
```javascript
resolve: {
  alias: {
    '@': fileURLToPath(new URL('./src', import.meta.url)),
    '@components': fileURLToPath(new URL('./src/components', import.meta.url)),
    '@assets': fileURLToPath(new URL('./src/assets', import.meta.url))
  }
}
```

### Proxy Configuration
Di `vite.config.js`:
```javascript
server: {
  proxy: {
    '/api': {
      target: 'http://localhost:3000',
      changeOrigin: true
    }
  }
}
```

## 🚀 Next Steps

Setelah memahami struktur folder ini, Anda dapat:

1. **Menambah komponen baru** di `src/components/`
2. **Membuat routes** dengan Vue Router
3. **Menambah state management** dengan Pinia
4. **Integrasi API** untuk backend connection
5. **Setup testing** dengan Vitest
6. **Deploy** ke Vercel, Netlify, atau hosting lain

---

## 📚 Referensi

- [Dokumentasi Vue.js](https://vuejs.org/)
- [Dokumentasi Vite](https://vitejs.dev/)
- [Vue Style Guide](https://vuejs.org/style-guide/)
- [Bun Documentation](https://bun.sh/docs)

## 💡 Tips Tambahan

- Gunakan **Bun** untuk development yang lebih cepat
- Aktifkan **Hot Module Replacement** (HMR) untuk development efisien
- Setup **Git hooks** untuk pre-commit linting
- Gunakan **TypeScript** untuk type safety yang lebih baik
- Implement **testing** sejak awal development