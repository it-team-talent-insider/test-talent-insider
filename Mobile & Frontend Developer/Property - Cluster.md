# Mobile / Frontend Developer Technical Test
**Position**: Mobile / Frontend Developer  
**Project**: Property Super App (Clone/Mini Version)  
**Time Limit**: 2 - 4 Days

---

## 📖 Scenario
Anda diminta untuk membangun sebuah aplikasi pencarian properti (Rumah, Apartemen, Ruko) yang modern dan performa tinggi. Fokus utama dari tes ini adalah kemampuan Anda dalam menangani **Data Besar (Large Datasets)**, **Optimasi Rendering Peta (Map Clustering)**, dan **User Experience (Infinite Scroll)** yang mulus.

---

## 🛠️ Resources (Sumber Daya)

| Resource | Link |
| :--- | :--- |
| **UI Design (Figma)** | [Klik disini untuk membuka Figma](https://www.figma.com/design/rxWBpijXFKlC56dF4nmEaC/Test-Mobile-Developer---Talent-Insider?node-id=0-1&t=djmeOacipM86E2zz-1) |
| **API Documentation** | [Klik disini untuk membuka SwaggerHub](https://app.swaggerhub.com/apis/ADMIN_186/test-talent-insider/1.0.0) |
| **Base URL API** | `https://api-test.linkedinindonesia.com/api` |

---

## 📱 Functional Requirements (Fitur Utama)

### 1. Authentication Module
*   **Splash Screen**: Tampilan pembuka yang elegan.
*   **Register**: 
    *   Form: First Name, Last Name, Email, Phone, Password.
    *   Validasi: Email harus valid, Password min. 8 karakter.
*   **Login**: Masuk menggunakan Email & Password.
*   **Session Management**: Simpan token (JWT) secara aman. User tidak perlu login ulang saat menutup aplikasi.

### 2. Home & Property List (Infinite Scroll)
*   **Tampilan List**: Tampilkan daftar properti dalam bentuk Card vertical.
*   **Infinite Scroll (Wajib)**: 
    *   Terapkan **Cursor Pagination** (bukan page number biasa).
    *   Load data per 10-20 item. Saat user scroll ke bawah, load data berikutnya otomatis.
*   **Filter & Sort**:
    *   Filter dropdown: `Type` (Rumah/Apartemen), `Status` (New/Second).
    *   Price Range: Input `Min Price` dan `Max Price`.

### 3. Smart Location & Search
*   **Search Bar**: Pencarian properti berdasarkan nama (Contoh: "Rumah Mewah").
*   **City Filter (Dropdown)**:
    *   Dropdown untuk memilih kota (Contoh: "Jakarta Selatan").
    *   **Tantangan**: Data kota berjumlah ribuan. Dropdown **HARUS** support **Search** (ketik nama kota) dan **Infinite Scroll** di dalam dropdown itu sendiri.
    *   API Endpoint: `GET /locations/cities`

### 4. Interactive Map & Clustering (🌟 Core Challenge)
Fitur ini memiliki bobot penilaian tertinggi. Aplikasi harus mampu me-render **5.000+ data properti** di peta tanpa lag.
*   **Map Engine**: Bebas (Google Maps SDK, Mapbox, atau Leaflet).
*   **Optimization**: Jangan me-load semua 5.000 marker sekaligus karena akan membuat HP nge-lag. Gunakan teknik **Server-Side Clustering** (Lihat panduan teknis di bawah).
*   **Interaction**:
    *   Saat user geser/zoom peta --> Load ulang marker sesuai area yang terlihat.
    *   Klik Cluster/Marker --> Tampilkan detail singkat properti.

### 5. Add Property Form
*   **Input**: Nama Properti, Harga, Deskripsi, Alamat Lengkap.
*   **Image Upload**: Ambil foto dari Galeri/Kamera, convert menjadi **Base64 String**, lalu kirim ke API.

---

## ⚙️ Technical Guide: Map Clustering Logic

Untuk mencapai performa tinggi, ikuti alur (flow) berikut:

1.  **Detect Map Movement**: Listen event saat user selesai menggeser (drag/pan) atau zoom peta.
2.  **Get Bounds**: Dapatkan koordinat batas layar HP user saat ini:
    *   `sw_latitude`, `sw_longitude` (Pojok Kiri Bawah)
    *   `ne_latitude`, `ne_longitude` (Pojok Kanan Atas)
3.  **Fetch Clusters (API 1)**: Panggil endpoint `POST /locations/cluster` dengan mengirim koordinat `bounds` tersebut. API ini akan mengembalikan **Array of Property IDs** yang ada di dalam layar saja.
4.  **Fetch Details (API 2)**: Gunakan ID yang didapat dari langkah 3, lalu panggil endpoint `POST /properties/search`.
    *   **Penting**: Sertakan parameter `view_mode: "simple"` pada body request agar response API ringan (hanya ID, Lat/Long, Harga, Nama).
5.  **Render Markers**: Gambar marker berdasarkan data yang didapat. Hapus marker yang sudah tidak terlihat di layar untuk menghemat memori.

---

## 🔝 Development Priority (Prioritas Pengerjaan)

Disarankan mengerjakan fitur dengan urutan berikut (Bobot terbesar pada Peta & List):

1.  **Map Clustering (Critical)**: Pastikan fitur ini smooth sebelum mengerjakan layer lainnya.
2.  **Infinite Scroll (High)**: Implementasi cursor pagination pada List & Dropdown Kota.
3.  **Authentication & Form**: Login, Register, dan Add Property.
4.  **UI Polish**: Detailing desain, animasi, dan transisi.

---

## 🏆 Evaluation Criteria (Penilaian)

Aplikasi Anda akan dinilai berdasarkan aspek berikut:

1.  **Performance (40%)**:
    *   Kelancaran Infinite Scroll (60 FPS).
    *   Kelancaran Map saat menangani ribuan data.
    *   Efisiensi penggunaan memori dan network request.
2.  **Code Quality (30%)**:
    *   Struktur kode yang rapi (Clean Architecture / MVVM / **Riverpod** / BLoC / Repository Pattern).
    *   Penamaan variable/fungsi yang jelas.
    *   Error Handling (Menangani respons error dari API / No Internet).
3.  **UI/UX (20%)**:
    *   Kesesuaian dengan desain Figma.
    *   User experience yang halus (Transisi, Loading State/Shimmer).
4.  **Bonus Points (10%)**:
    *   Unit Testing.
    *   Animation (Micro-interactions).
    *   Dark Mode support.

---

## 📦 Submission Format
1.  **Source Code**: Upload ke public repository (GitHub / GitLab).
2.  **Demo Video**: Rekam layar singkat (1-2 menit) yang menunjukkan fitur Register, Scroll List, Map Clustering, dan Search. Upload ke YouTube (Unlisted) atau sertakan file di repo.
3.  **APK (Android) / TestFlight (iOS)**: (Optional) Link instalasi aplikasi.

**Selamat Mengerjakan! Good Luck!**
