# Panduan Koneksi Firebase & Firestore Cloud Leaderboard

Project **Super Ultra Kawai Helix Game Waow** telah berhasil dihubungkan ke Firebase dengan konfigurasi Anda.

---

## 1. Konfigurasi yang Digunakan
Konfigurasi Firebase web app berikut telah aktif di dalam [`index.html`](file:///c:/Users/Teacher/Documents/GAME-FOR-EVERYONE-FIRST-GAME-/index.html):
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

## 2. Fitur Firebase yang Diaktifkan
1. **Real-time Global Leaderboard**:
   - Tab **🌍 Player Leaderboard** otomatis mengambil data pemain dari Firebase Cloud Firestore secara langsung.
   - Setiap kali pemain kalah (*Game Over*) atau menang (*Victory*), skor dan nama pemain otomatis dikirim ke Firestore.
2. **Koleksi Firestore**:
   - `leaderboard_normal`: Menyimpan skor untuk *Normal Mode*.
   - `leaderboard_endless`: Menyimpan skor untuk *Endless Mode*.
3. **Graceful Offline Fallback**:
   - Jika perangkat pemain sedang offline atau belum memiliki akses internet, game tetap berjalan lancar 100% menggunakan penyimpanan lokal (`localStorage`).
4. **Status Indikator**:
   - Terdapat lencana status koneksi Firebase di **Menu Utama** dan catatan sinkronisasi di layar **Leaderboard**.

---

## 3. Langkah Pengaturan di Firebase Console (Jika belum dibuat)
Agar Firebase Firestore dapat menerima dan membaca data skor:

1. Buka [Firebase Console](https://console.firebase.google.com/) dan pilih project **game-for-everyone-1a93e**.
2. Di menu sebelah kiri, klik **Build** > **Firestore Database**.
3. Jika belum membuat database:
   - Klik **Create Database**.
   - Pilih lokasi server terdekat (misal: `asia-southeast1` / `asia-southeast2`).
   - Pilih **Start in test mode** untuk pengujian langsung.
4. Di tab **Rules**, pastikan aturan Firestore mengizinkan pembacaan & penulisan (untuk mode development/game publik):
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

Database Firestore Anda kini siap merekam skor pemain dari seluruh dunia secara realtime! 🚀
