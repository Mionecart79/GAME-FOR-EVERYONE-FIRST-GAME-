# Panduan Lengkap Koneksi Firebase & Fitur Akun Game for Everyone

Project **Super Ultra Kawai Helix Game Waow (Game for Everyone)** telah berhasil diintegrasikan dengan Firebase Auth & Firestore menggunakan konfigurasi Anda.

---

## 1. Konfigurasi Firebase Aktif
Konfigurasi Firebase web app berikut telah aktif di dalam [`index.html`](file:///c:/Users/Student/Documents/GAME-FOR-EVERYONE-FIRST-GAME-/index.html):
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyBH4IBuZyo0fpbEM0K-mwnmbjRN2VWx31U",
  authDomain: "game-for-everyone-1a93e.firebaseapp.com",
  projectId: "game-for-everyone-1a93e",
  storageBucket: "game-for-everyone-1a93e.firebasestorage.app",
  messagingSenderId: "679469745511",
  appId: "1:679469745511:web:d7dde27545e102381a9bc2"
};
```

---

## 2. Fitur Firebase yang Telah Terpasang

### A. Sistem Akun, Akun Tamu & Keunikan Nama Pemain
1. **Pendaftaran & Login Akun Tetap**:
   - **Email & Password**: Pemain dapat mendaftarkan akun baru dengan password mereka sendiri.
   - **Google Sign-In**: Masuk cepat sekali klik menggunakan akun Google.
   - **Facebook Sign-In**: Masuk cepat sekali klik menggunakan akun Facebook.
2. **Akun Tamu (Tidak Tetap / Sementara)**:
   - Pemain dapat memilih **🎮 Main Tamu (Tidak Tetap)** kapan saja tanpa perlu mendaftar atau memasukkan email/password.
   - Terdapat tombol **⬅️ Kembali ke Menu Utama** yang jelas di bagian atas dan bawah layar akun sehingga pemain bisa kembali ke menu utama kapan saja.
   - Pemain yang sedang login juga dapat beralih ke Akun Tamu secara instan dengan tombol **⚡ Beralih ke Akun Tamu**.
3. **Paten Nama Pemain (Tidak Bisa Ditiru)**:
   - Nama pemain otomatis diverifikasi di Firestore collection `usernames`.
   - Jika nama sudah pernah didaftarkan oleh pemain lain, game akan menampilkan peringatan dan mencegah pemain lain menggunakan nama tersebut.
   - Pengecekan nama juga terjadi secara *real-time* saat pemain mengetik di layar **Kustomisasi Bola**.

### B. Sinkronisasi Profil Kustomisasi Bola (Cloud Save)
1. **Warna & Ekspresi Wajah Bola**:
   - Setiap perubahan warna bola dan ekspresi wajah (*happy*, *uwu*, *wink*, *derp*) otomatis tersimpan ke dokumen pemain di Firestore collection `users/{uid}`.
   - Kapan pun pemain login di perangkat atau browser lain, warna dan ekspresi bola mereka akan langsung dimuat secara otomatis!

### C. Real-time Leaderboard Murni (Mulai dari Kosong)
1. **Tanpa Data Dummy**:
   - Seluruh pemain contoh buatan telah dihapus 100%. Leaderboard dimulai dalam keadaan **benar-benar kosong**.
   - Saat ada pemain yang bermain (baik *Normal Mode* maupun *Endless Mode*), skor, lantai terjauh, max combo, warna, dan ekspresi bola akan dikirim ke Firestore dan ditampilkan secara otomatis.
2. **Koleksi Firestore**:
   - `leaderboard_normal`: Menyimpan rekor untuk Normal Mode (25 lantai).
   - `leaderboard_endless`: Menyimpan rekor untuk Endless Mode (tanpa batas).

---

## 3. Langkah Pengaturan di Firebase Console

Agar semua fitur autentikasi dan database berfungsi dengan lancar:

### Langkah 1: Aktifkan Firebase Authentication
1. Buka [Firebase Console](https://console.firebase.google.com/) dan pilih project **game-for-everyone-1a93e**.
2. Di menu sebelah kiri, klik **Build** > **Authentication** > **Get started**.
3. Di tab **Sign-in method**, aktifkan:
   - **Email/Password**: Klik enable lalu **Save**.
   - **Google**: Klik enable, pilih support email, lalu **Save**.
   - **Facebook** *(Opsional)*: Klik enable dan masukkan App ID serta App Secret dari Meta for Developers.

### Langkah 2: Buat & Atur Firestore Database
1. Di menu sebelah kiri, klik **Build** > **Firestore Database**.
2. Klik **Create database**, pilih lokasi terdekat (misal: `asia-southeast1` / `asia-southeast2`).
3. Pilih **Start in test mode** lalu klik **Create**.
4. Masuk ke tab **Rules**, gunakan aturan berikut agar game dapat membaca & menyimpan profil serta leaderboard:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```
5. Klik **Publish**.

---

## 4. Struktur Database di Firestore
- **`users/{uid}`**: Menyimpan data profil bola pemain (`name`, `color`, `face`, `updatedAt`).
- **`usernames/{nama_huruf_kecil}`**: Mengunci kepemilikan nama pemain (`uid`, `displayName`, `updatedAt`) agar nama tidak bisa diduplikasi.
- **`leaderboard_normal`**: Riwayat rekor skor Normal Mode pemain sedunia.
- **`leaderboard_endless`**: Riwayat rekor skor Endless Mode pemain sedunia.
