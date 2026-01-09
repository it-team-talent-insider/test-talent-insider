# Mobile / Frontend Developer Technical Test
**Project: Property Super App**

## 🎯 Objective
Membangun aplikasi mobile/web yang responsif, performant, dan memiliki User Experience (UX) premium untuk pencarian properti. Kandidat diharapkan mengimplementasikan fitur **Map Clustering**, **Infinite Scroll**, dan **Optimized Search** menggunakan API yang telah disediakan.

---

## 📱 Fitur Utama (Requirements)

### 1. Authentication
*   **Splash Screen**: Tampilan awal aplikasi.
*   **Login & Register**: 
    *   Validasi input (Email format, Password min 8 char).
    *   Auto-login setelah registrasi sukses.
    *   Simpan Token (JWT) di storage lokal secara aman.

### 2. Home & Property List (Infinite Scroll)
*   **Tampilan List**: Menampilkan daftar properti dalam bentuk Card.
*   **Infinite Scroll**: Tidak boleh load semua data sekaligus. Gunakan **Cursor Pagination** yang disediakan API.
*   **Filter & Sort**:
    *   Filter berdasarkan `Type` (Rumah, Apartemen, Ruko).
    *   Filter based on `Status` (New, Second).
    *   Range Harga (`price_min`, `price_max`).

### 3. Smart Search & Location Filter
*   **Search Bar**: Pencarian properti berdasarkan nama.
*   **Location Dropdown**: 
    *   Dropdown "Pilih Kota" yang datanya diambil dari API.
    *   Dropdown ini **wajib** menggunakan **Infinite Scroll** (karena data kota bisa ribuan) dan fitur **Search** (ketik nama kota).

### 4. Interactive Map & Clustering (CORE FEATURE) 🌟
Ini adalah fitur terpenting. Aplikasi harus mampu menangani 5.000+ pin properti di peta tanpa lag.
*   **Map Interface**: Google Maps / Mapbox / Leaflet.
*   **Client-Side Clustering Logic**:
    1.  Kandidat harus mendeteksi pergerakan peta (Pan/Zoom).
    2.  Ambil koordinat batas layar (**SouthWest** & **NorthEast**).
    3.  Panggil API `/locations/cluster` untuk mendapatkan ID properti di area tersebut.
    4.  Panggil API `/properties/search` menggunakan ID yang didapat untuk mengambil detail properti (gunakan `view_mode=simple`).
    5.  Render Marker di peta.

### 5. Add Property Form
*   **Input Data**: Nama, Harga, Deskripsi, Tipe, Alamat.
*   **Upload Image**: Pilih gambar dari galeri/kamera, convert ke **Base64 string**, dan kirim ke API.

---

## 🔗 API Integration Guide

**Documentation (Swagger)**: [https://app.swaggerhub.com/apis/ADMIN_186/test-talent-insider/1.0.0](https://app.swaggerhub.com/apis/ADMIN_186/test-talent-insider/1.0.0)
**Design (Figma)**: [https://www.figma.com/design/rxWBpijXFKlC56dF4nmEaC/Test-Mobile-Developer---Talent-Insider?node-id=0-1&t=djmeOacipM86E2zz-1](https://www.figma.com/design/rxWBpijXFKlC56dF4nmEaC/Test-Mobile-Developer---Talent-Insider?node-id=0-1&t=djmeOacipM86E2zz-1)

**Base URL**: `https://api-test.linkedinindonesia.com/api`

### A. Authentication
*   `POST /register`: Kirim `email`, `password`, `first_name`, `last_name`, `phone`.
*   `POST /login`: Kirim `email`, `password`.

### B. Property Data (Infinite Scroll)
*   `GET /properties`
    *   **Params**: `cursor` (untuk next page), `search`, `type`, `status`.
    *   **Tips**: Gunakan `next_page_url` atau `next_cursor` dari response JSON untuk load page selanjutnya.

### C. Location / City Dropdown
*   `GET /locations/cities`
    *   **Params**: `search` (ketik nama kota), `cursor` (scroll ke bawah load more).
    *   **Output**: List unik nama kota.

### D. Map Clustering Flow (High Performance)
Untuk menangani ribuan data, jangan panggil `GET /properties` biasa. Gunakan flow ini:

1.  **Step 1: Get Property IDs in Viewport**
    *   Endpoint: `POST /locations/cluster`
    *   Body:
        ```json
        {
          "bounds": [
             {
               "sw_latitude": -6.300, "sw_longitude": 106.700,
               "ne_latitude": -6.100, "ne_longitude": 106.900
             }
          ]
        }
        ```
    *   Response: Array of Property IDs (Contoh: `[101, 105, 209, ...]`)

2.  **Step 2: Fetch Data by IDs**
    *   Endpoint: `POST /properties/search`
    *   Body:
        ```json
        {
          "ids": [101, 105, 209, ...],
          "view_mode": "simple"  <-- PENTING: Gunakan 'simple' agar response ringan
        }
        ```
    *   Response: List properti ringkas (ID, Nama, Harga, Koordinat/Alamat) untuk dirender jadi Marker.

---

## 🏆 Evaluation Criteria

1.  **Code Structure**: Clean Architecture, Separation of Concerns (MVVM/BLoC/Repository Pattern).
2.  **Performance**:
    *   Aplikasi tidak boleh "freeze" saat load map.
    *   Infinite scroll berjalan mulus (60fps).
3.  **UI/UX**: Desain "Premium", transisi halus, loading state yang jelas (Shimmer effect).
4.  **Error Handling**: Menangani kondisi offline atau server error dengan baik.

---

## 📦 Submission
*   Source code diupload ke GitHub/GitLab.
*   Video demo singkat aplikasi (.mp4 atau link YouTube/Drive).
*   APK/IPA file (Optional).
