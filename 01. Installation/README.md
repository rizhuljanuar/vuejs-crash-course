# Dokumentasi Instalasi Vue.js

## Panduan Lengkap Instalasi Vue.js

Dokumentasi ini akan memandu Anda melalui proses instalasi Vue.js dengan berbagai metode yang tersedia. Vue.js adalah framework JavaScript progresif untuk membangun antarmuka pengguna yang interaktif.

## Prasyarat

Sebelum memulai instalasi Vue.js, pastikan Anda telah menginstal:
- **Node.js** (versi 18.0 atau lebih tinggi)
- **npm** (biasanya sudah terinstal bersama Node.js)
- **Git** (untuk kontrol versi)

### Opsi Package Manager

Anda dapat menggunakan salah satu dari package manager berikut:
- **npm** (Node Package Manager) - default
- **yarn** - alternatif yang lebih cepat
- **pnpm** - hemat ruang disk
- **Bun** - package manager JavaScript modern yang cepat

## Metode Instalasi

### 1. Menggunakan npm (Default)

#### Langkah 1: Buat Proyek Vue Baru
```bash
npm create vue@latest
```

#### Langkah 2: Install Dependencies
Setelah proyek dibuat, masuk ke direktori proyek:
```bash
cd <nama-proyek-anda>
npm install
```

### 2. Menggunakan Yarn

#### Langkah 1: Install Yarn (jika belum ada)
```bash
npm install -g yarn
```

#### Langkah 2: Buat Proyek Vue Baru
```bash
yarn create vue
```

#### Langkah 3: Install Dependencies
```bash
cd <nama-proyek-anda>
yarn install
```

### 3. Menggunakan pnpm

#### Langkah 1: Install pnpm (jika belum ada)
```bash
npm install -g pnpm
```

#### Langkah 2: Buat Proyek Vue Baru
```bash
pnpm create vue
```

#### Langkah 3: Install Dependencies
```bash
cd <nama-proyek-anda>
pnpm install
```

### 4. Menggunakan Bun (Package Manager Modern)

**Bun** adalah package manager JavaScript modern yang sangat cepat, hemat memori, dan all-in-one untuk JavaScript & TypeScript.

#### Langkah 1: Install Bun
```bash
# Di Linux/macOS
curl -fsSL https://bun.sh/install | bash

# Atau menggunakan npm
npm install -g bun

# Atau menggunakan Homebrew (macOS)
brew tap oven-sh/bun
brew install bun
```

#### Langkah 2: Buat Proyek Vue Baru dengan Bun
```bash
bun create vue
```

#### Langkah 3: Install Dependencies dengan Bun
```bash
cd <nama-proyek-anda>
bun install
```

#### Keuntungan Menggunakan Bun:
- **10x lebih cepat** dari npm
- **Hemat memori** dan ruang disk
- **All-in-one tool**: package manager, bundler, test runner, dan runtime
- **Kompatibel** dengan package npm yang sudah ada
- **Instalasi dependencies** yang sangat cepat

## Konfigurasi Proyek

Setelah menjalankan perintah create vue, Anda akan diminta untuk mengkonfigurasi proyek:

```
✔ Project name: … <nama-proyek-anda>
✔ Add TypeScript? … No / Yes
✔ Add JSX Support? … No / Yes
✔ Add Vue Router for Single Page Application development? … No / Yes
✔ Add Pinia for state management? … No / Yes
✔ Add Vitest for Unit Testing? … No / Yes
✔ Add an End-to-End Testing Solution? › No
✔ Add ESLint for code quality? … No / Yes
```

### Rekomendasi Konfigurasi:
- **TypeScript**: Yes (untuk type safety yang lebih baik)
- **JSX Support**: Yes (jika Anda terbiasa dengan React)
- **Vue Router**: Yes (untuk aplikasi dengan multiple pages)
- **Pinia**: Yes (untuk state management modern)
- **Vitest**: Yes (untuk unit testing)
- **ESLint**: Yes (untuk menjaga kualitas kode)

## Menjalankan Proyek

Setelah instalasi selesai, jalankan proyek dengan perintah berikut:

### Menggunakan npm
```bash
npm run dev
```

### Menggunakan Yarn
```bash
yarn dev
```

### Menggunakan pnpm
```bash
pnpm dev
```

### Menggunakan Bun
```bash
bun run dev
```

## Build untuk Produksi

### Menggunakan npm
```bash
npm run build
```

### Menggunakan Yarn
```bash
yarn build
```

### Menggunakan pnpm
```bash
pnpm build
```

### Menggunakan Bun
```bash
bun run build
```

## Struktur Direktori Proyek

Setelah instalasi, struktur direktori Anda akan terlihat seperti ini:

```
nama-proyek/
├── public/              # File statis
│   └── favicon.ico
├── src/                 # Source code
│   ├── assets/          # Asset seperti gambar, CSS
│   ├── components/      # Komponen Vue
│   ├── App.vue          # Komponen utama
│   └── main.js          # Entry point
├── .gitignore           # File yang diabaikan Git
├── index.html           # File HTML utama
├── package.json         # Konfigurasi proyek
└── README.md            # Dokumentasi proyek
```

## Tips Tambahan

### 1. Verifikasi Instalasi
Untuk memastikan Vue.js terinstal dengan benar:
```bash
# Cek versi package manager
npm --version  # atau yarn --version, pnpm --version, bun --version

# Cek versi Node.js
node --version
```

### 2. Update Dependencies
```bash
# npm
npm update

# yarn
yarn upgrade

# pnpm
pnpm update

# bun
bun update
```

### 3. Install Package Tambahan
```bash
# npm
npm install <nama-package>

# yarn
yarn add <nama-package>

# pnpm
pnpm add <nama-package>

# bun
bun add <nama-package>
```

## Troubleshooting

### Masalah Umum:

1. **EACCES Error (Permission Denied)**
   ```bash
   sudo chown -R $(whoami) ~/.npm
   # Atau gunakan nvm untuk mengelola Node.js
   ```

2. **Port Sudah Digunakan**
   ```bash
   # Kill process di port 3000
   sudo kill -9 $(lsof -ti :3000)
   ```

3. **Network Timeout**
   ```bash
   npm config set registry https://registry.npmjs.org/
   # Atau gunakan mirror lokal
   npm config set registry https://npm.taobao.org/
   ```

### Bantuan Tambahan:
- [Dokumentasi Resmi Vue.js](https://vuejs.org/guide/quick-start.html)
- [Dokumentasi Bun](https://bun.sh/docs)
- [Forum Vue.js](https://forum.vuejs.org/)

---

## Ringkasan Perintah

| Aksi | npm | yarn | pnpm | bun |
|------|-----|------|------|-----|
| Buat Proyek | `npm create vue@latest` | `yarn create vue` | `pnpm create vue` | `bun create vue` |
| Install Deps | `npm install` | `yarn install` | `pnpm install` | `bun install` |
| Run Dev | `npm run dev` | `yarn dev` | `pnpm dev` | `bun run dev` |
| Build | `npm run build` | `yarn build` | `pnpm build` | `bun run build` |
| Add Package | `npm install <pkg>` | `yarn add <pkg>` | `pnpm add <pkg>` | `bun add <pkg>` |

Pilih package manager yang sesuai dengan kebutuhan dan preferensi Anda. **Bun** direkomendasikan untuk performa tercepat, sedangkan **npm** adalah pilihan yang paling stabil dan kompatibel.