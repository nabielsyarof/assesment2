# To-Do List Activity App

> Repository ini berisi proyek Flutter untuk aplikasi _To-Do List Activity App_ yang dikembangkan sebagai tugas/portofolio.

---

## Daftar Tim

- Achmad Varis Abdussalam (2310130001)
- Muhammad Naufal Ammr Dzakwan (2310130010)
- Nabiel Syarof Azzaky (2310130012)
- M. Irfan Janur Gifari (2310130009)

---

## Pembagian Tugas

> **Silakan sesuaikan tugas nyata tiap anggota.**

- UI & Desain: Achmad Varis Abdussalam
- Implementasi Login & SharedPreferences: Muhammad Naufal Ammr Dzakwan
- State management (Provider) & Feature Activity: M. Irfan Janur Gifari
- Dokumentasi & README: Nabiel Syarof Azzaky

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

## Screenshot

Letakkan screenshot di folder `/assets/screenshots/` lalu sertakan jalur tersebut di `pubspec.yaml`.

Screenshot yang diminta minimal (contoh nama file):

- `login_page.png` — halaman Login
- `register_page.png` — halaman Register
- `home_page.png` — halaman Activity/Home
- `add_item_page.png` — tampilan saat menambah item atau dialog input
- `light_mode.png` — tampilan aplikasi di Light Mode
- `dark_mode.png` — tampilan aplikasi di Dark Mode (opsional)

## Bagaimana Aplikasi Menyimpan Data

- **SharedPreferences**: digunakan untuk menyimpan preferensi dan data registrasi sederhana (`email`, `fullName`, `password`, `isDark`, `lastLogin`). Data ini bertahan antar sesi aplikasi di perangkat.
- **Activities (saat ini)**: Disimpan di memory menggunakan `ActivityProvider` pada runtime. Jika aplikasi ditutup atau di-restart, daftar akan hilang.
- **Rekomendasi**: Untuk persistensi yang benar, implementasikan SQLite (`sqflite`) seperti dijelaskan di atas, atau gunakan opsi penyimpanan berbasis file / backend (API) jika ingin sinkronisasi antar perangkat.


6. Kirim link GitHub repository sebagai luaran.
https://github.com/parisid13/assesment2#
