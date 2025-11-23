# To-Do List Activity App

> Repository ini berisi proyek Flutter untuk aplikasi _To-Do List Activity App_ yang dikembangkan sebagai tugas/portofolio.

---

## Daftar Tim

> **Silakan ganti placeholder di bawah dengan nama lengkap dan NIM anggota tim yang sebenarnya.**

- Anggota 1: Nama Lengkap - NIM
- Anggota 2: Nama Lengkap - NIM
- Anggota 3: Nama Lengkap - NIM

---

## Pembagian Tugas

> **Silakan sesuaikan tugas nyata tiap anggota.**

- UI & Desain: Nama A
- Implementasi Login & SharedPreferences: Nama B
- State management (Provider) & Feature Activity: Nama C
- Dokumentasi & README: Nama D

---

## Struktur Folder (ringkasan)

```
/lib
  ├─ main.dart
  ├─ pages/
  │   ├─ register_page.dart
  │   ├─ login_page.dart
  │   └─ activity_page.dart
  ├─ providers/
  │   ├─ theme_provider.dart
  │   └─ activity_provider.dart
  └─ widgets/
assets/
  └─ screenshots/
pubspec.yaml
README.md
```

> (Catatan: file yang Anda kirim berada dalam satu file `main.dart`. Di repo sebaiknya pisahkan menjadi beberapa file sesuai struktur di atas.)

---

## Cara Menjalankan Proyek (local)

1. Pastikan Flutter SDK sudah terpasang.
2. Clone repository:

   ```bash
   git clone <repo-url>
   cd <repo-folder>
   flutter pub get
   flutter run
   ```

3. Jika ingin menjalankan di emulator/simulator, jalankan emulator lebih dulu.

---

## Penjelasan Fitur

### 1. Fitur Login

- **Tujuan:** Mengizinkan pengguna masuk ke aplikasi menggunakan email & password yang sebelumnya didaftarkan.
- **Implementasi saat ini:**

  - Pada halaman Register, aplikasi menyimpan `email`, `password`, dan `fullName` ke `SharedPreferences`.
  - Pada halaman Login, aplikasi membaca nilai `email` dan `password` dari `SharedPreferences` dan membandingkannya dengan input pengguna.
  - Setelah login sukses, aplikasi menyimpan `lastLogin` di `SharedPreferences` dan mengarahkan pengguna ke halaman Activity.

- **Kelemahan/Limitasi:**

  - Autentikasi hanya bersifat lokal (tidak ada server). Data tersimpan plain di SharedPreferences (hindari untuk data sensitif di aplikasi produksi).

### 2. Fitur Light / Dark Mode

- **Tujuan:** Memungkinkan pengguna beralih antara tema terang dan gelap.
- **Implementasi:**

  - `ThemeProvider` (ChangeNotifier) menyimpan boolean `isDark`.
  - Nilai `isDark` dibaca dari `SharedPreferences` pada inisialisasi provider (`_load()`), sehingga preferensi tema dipertahankan antar sesi.
  - Perubahan tema melalui `toggle()` akan menyimpan nilai baru ke `SharedPreferences` dan memanggil `notifyListeners()`.
  - `MaterialApp` menggunakan `themeMode: themeProv.isDark ? ThemeMode.dark : ThemeMode.light` sehingga tema berubah secara global.

### 3. Implementasi SharedPreferences

- Paket yang digunakan: `shared_preferences`.
- Data yang disimpan:

  - `email`, `password`, `fullName` (pada registrasi)
  - `lastLogin` (pada login sukses)
  - `isDark` (preference tema)

- **Catatan keamanan:** `SharedPreferences` cocok untuk menyimpan preferensi dan data ringan, bukan data sensitif yang harus terenkripsi.

### 4. Implementasi SQLite

- **Status saat ini:** _BELUM diimplementasikan_ di kode yang diberikan. Saat ini daftar kegiatan (activities) disimpan dalam memori menggunakan `ActivityProvider` (list in-memory) sehingga data akan hilang saat aplikasi dimulai ulang.

- **Saran implementasi SQLite (langkah singkat):**

  1. Tambahkan dependency di `pubspec.yaml`:

     ```yaml
     dependencies:
       sqflite: ^2.0.0+4
       path: ^1.8.0
     ```

  2. Buat helper database (`lib/services/db_helper.dart`) yang menangani inisialisasi DB dan operasi CRUD.
  3. Buat model `Activity` (class) dengan field `id`, `text`, `done`.
  4. Modifikasi `ActivityProvider` agar saat `addActivity`, `deleteActivity`, `toggleDone`, dan `clearAll` juga melakukan operasi ke database.
  5. Saat provider diinisialisasi, baca daftar dari database dan isi `_activities`.

- **Contoh alur singkat implementasi:**

  - `DBHelper.openDB()` → return instance DB
  - `DBHelper.insertActivity(Activity)` → insert
  - `DBHelper.getAllActivities()` → membaca semua activity saat start
  - `DBHelper.updateActivity(Activity)` → toggle done
  - `DBHelper.deleteActivityById(id)` → delete

---

## Screenshot (minimal 5 halaman)

Letakkan screenshot di folder `/assets/screenshots/` lalu sertakan jalur tersebut di `pubspec.yaml`.

Screenshot yang diminta minimal (contoh nama file):

- `login_page.png` — halaman Login
- `register_page.png` — halaman Register
- `home_page.png` — halaman Activity/Home
- `add_item_page.png` — tampilan saat menambah item atau dialog input
- `light_mode.png` — tampilan aplikasi di Light Mode
- `dark_mode.png` — tampilan aplikasi di Dark Mode (opsional)

**Cara mengambil screenshot di emulator:**

- Android Studio emulator: `More` → `Screenshot`.
- iOS Simulator: `File` → `Save Screen`.

---

## Bagaimana Aplikasi Menyimpan Data (penjelasan)

- **SharedPreferences**: digunakan untuk menyimpan preferensi dan data registrasi sederhana (`email`, `fullName`, `password`, `isDark`, `lastLogin`). Data ini bertahan antar sesi aplikasi di perangkat.
- **Activities (saat ini)**: Disimpan di memory menggunakan `ActivityProvider` pada runtime. Jika aplikasi ditutup atau di-restart, daftar akan hilang.
- **Rekomendasi**: Untuk persistensi yang benar, implementasikan SQLite (`sqflite`) seperti dijelaskan di atas, atau gunakan opsi penyimpanan berbasis file / backend (API) jika ingin sinkronisasi antar perangkat.

---

## Petunjuk Pengumpulan (GitHub link)

1. Buat repository baru di GitHub (mis: `todo-activity-app`).
2. Tambahkan semua file proyek Flutter ke folder repo lokal.
3. Pastikan `pubspec.yaml` mencantumkan dependency yang digunakan (`provider`, `shared_preferences`, dan jika implementasi SQLite: `sqflite`, `path`).
4. Tambahkan folder `assets/screenshots/` dengan minimal 5 screenshot.
5. Commit & push:

   ```bash
   git init
   git add .
   git commit -m "Initial commit - ToDo Activity App"
   git branch -M main
   git remote add origin https://github.com/<username>/<repo>.git
   git push -u origin main
   ```

6. Kirim link GitHub repository sebagai luaran.

---

## Catatan Tambahan & Checklist Sebelum Mengumpulkan

- [ ] Ganti placeholder nama & NIM di bagian 'Daftar Tim'.
- [ ] Pisahkan file `main.dart` menjadi beberapa file sesuai struktur folder untuk keterbacaan.
- [ ] Jika diminta, implementasikan SQLite dan lakukan migration data (opsional)
- [ ] Tambahkan screenshot ke `assets/screenshots` dan pastikan path di `pubspec.yaml` sudah benar.
- [ ] Periksa `README.md` dan lengkapi deskripsi tugas tiap anggota.

---

Jika kamu mau, aku bisa:

- Mengubah `ActivityProvider` agar terhubung ke SQLite (aku bisa buatkan contoh file `db_helper.dart` dan ubah provider),
- Memecah `main.dart` menjadi file-file terpisah (pages/providers),
- Menyusun commit message & langkah git yang rapi sebelum push.

Tulis saja opsi mana yang ingin kamu minta dan aku akan langsung buatkan filenya di repo (atau contoh kodenya) di sini.
