# UTS-AR
# AR Visualisasi Hardware Komputer 3D dan Fungsinya

Aplikasi **Marker-Based Augmented Reality** untuk Android yang menampilkan model 3D **13 komponen hardware laptop** (7 komponen internal dan 6 komponen luar/antarmuka) di atas kartu marker. Pengguna bisa memutar dan memperbesar model, lalu membaca fungsi tiap komponen.

> Tugas UTS mata kuliah **Augmented Reality untuk Pendidikan**, Universitas Darussalam Gontor.
> Dosen pengampu: Faisal Reza Pradhana, M.Kom

| Anggota | NIM |
|---|---|
| Akhmad Amin Aziza | 452024611055 |
| Rosyid Muzaki Putro | 452024611058 |

**Kelas:** TI5A2 · **Kelompok:** NamaKelompok

---

## Daftar Isi
1. [Fitur](#fitur)
2. [Memasang APK (pengguna)](#memasang-apk-pengguna)
3. [Cara Menggunakan Aplikasi](#cara-menggunakan-aplikasi)
4. [Daftar Kartu Marker](#daftar-kartu-marker)
5. [Prasyarat Pengembangan](#prasyarat-pengembangan)
6. [Menjalankan dari Source Code](#menjalankan-dari-source-code)
7. [Model 3D (Blender)](#model-3d-blender)
8. [Struktur Proyek](#struktur-proyek)
9. [Cara Kerja Teknis](#cara-kerja-teknis)
10. [Troubleshooting](#troubleshooting)
11. [Aset & Lisensi](#aset--lisensi)
12. [Penggunaan GenAI](#penggunaan-genai)
13. [Riwayat Commit](#riwayat-commit)

---

## Fitur

| Fitur | Keterangan |
|---|---|
| Izin kamera | Meminta izin kamera. Bila ditolak, muncul pesan beserta tombol *Coba Lagi* dan *Buka Pengaturan* |
| Pindai marker | Image tracking 13 kartu (AR Foundation + ARCore) |
| Model 3D hardware | 13 model buatan sendiri di Blender muncul di atas kartu dengan skala yang menyesuaikan ukuran kartu |
| Rotasi | Geser 1 jari untuk memutar 360° (kiri/kanan) dan memiringkan model (atas/bawah) |
| Zoom | Cubit 2 jari (pinch), dibatasi 0,5×–3× |
| Info fungsi | Ketuk model atau tombol **(i)** untuk membuka pop-up berisi nama, fungsi, dan spesifikasi |
| Marker hilang | Model disembunyikan dan muncul petunjuk *"Arahkan kamera ke kartu marker"* |
| Reset | Mengembalikan rotasi dan skala model ke kondisi awal |

---

## Memasang APK (pengguna)

**Syarat perangkat:** Android 7.0 (API 24) atau lebih baru dan mendukung **Google Play Services for AR (ARCore)**. Daftar perangkat yang didukung ada di <https://developers.google.com/ar/devices>.

1. Unduh `UTS_AR_NamaKelompok_TI5A2.apk` dari [Google Drive](LINK-APK).
2. Salin APK ke ponsel, lalu buka file tersebut.
3. Bila diminta, izinkan **Instal dari sumber tidak dikenal** untuk aplikasi pengelola file atau browser.
4. Ketuk **Instal**, lalu **Buka**.
5. Saat pertama kali masuk ke layar pindai, pilih **Izinkan** pada permintaan izin kamera.
6. Bila diminta memasang atau memperbarui **Google Play Services for AR**, ikuti petunjuknya.

Cara lain lewat USB (mode developer aktif):

```bash
adb install -r UTS_AR_NamaKelompok_TI5A2.apk
```

---

## Cara Menggunakan Aplikasi

1. Buka aplikasi, lalu ketuk **Mulai Pindai** di Beranda.
2. Izinkan akses kamera.
3. Arahkan kamera ke salah satu kartu marker dari jarak ±20–50 cm dengan cahaya yang cukup.
4. Model 3D muncul di atas kartu beserta label namanya.
5. Geser 1 jari untuk memutar, cubit 2 jari untuk zoom.
6. Ketuk model atau tombol **(i)** untuk membaca fungsi komponen.
7. Ketuk **Reset** untuk mengembalikan posisi model, atau **Kembali** untuk ke Beranda.

---

## Daftar Kartu Marker

File siap cetak: [`Markers/UTS_AR_Kartu_Marker.pdf`](Markers/UTS_AR_Kartu_Marker.pdf) (A4, 4 kartu per halaman). Gambar target untuk Unity ada di `Markers/marker_png/`.

| No | Kategori | ID Marker (Reference Image) | Komponen | Model |
|---|---|---|---|---|
| 01 | Internal | `marker_cpu` | Prosesor (CPU) | `HW_cpu.fbx` |
| 02 | Internal | `marker_motherboard` | Motherboard | `HW_motherboard.fbx` |
| 03 | Internal | `marker_ram` | RAM | `HW_ram.fbx` |
| 04 | Internal | `marker_storage` | Penyimpanan (SSD/HDD) | `HW_storage.fbx` |
| 05 | Internal | `marker_gpu` | GPU (Kartu Grafis) | `HW_gpu.fbx` |
| 06 | Internal | `marker_cooling` | Sistem Pendingin | `HW_cooling.fbx` |
| 07 | Internal | `marker_battery` | Baterai | `HW_battery.fbx` |
| 08 | Eksternal | `marker_screen` | Layar (LCD/LED) | `HW_screen.fbx` |
| 09 | Eksternal | `marker_keyboard` | Keyboard & Touchpad | `HW_keyboard.fbx` |
| 10 | Eksternal | `marker_speaker` | Speaker | `HW_speaker.fbx` |
| 11 | Eksternal | `marker_webcam` | Webcam & Mikrofon | `HW_webcam.fbx` |
| 12 | Eksternal | `marker_chassis` | Casing & Engsel | `HW_chassis.fbx` |
| 13 | Eksternal | `marker_ports` | Port & Konektivitas | `HW_ports.fbx` |

- Ukuran kartu **10 × 10 cm**, gambar marker **8 × 8 cm**, margin putih **1 cm**.
- Dicetak di art carton 260 gsm dengan laminasi doff.
- Ukuran fisik di Reference Image Library: **0,08 m**.
- Semua gambar mendapat skor `arcoreimg` 85–100 (minimal yang disarankan 75).

Kartu bisa dibuat ulang dengan `python tools/make_markers.py Markers` (butuh Python 3 + Pillow).

---

## Prasyarat Pengembangan

| Komponen | Versi |
|---|---|
| Unity Editor | 2022.3 LTS + modul **Android Build Support** (SDK, NDK, OpenJDK) |
| AR Foundation | 5.1.x |
| Google ARCore XR Plugin | 5.1.x |
| TextMeshPro | bawaan Unity |
| Input System (opsional) | 1.7+ (skrip mendukung Input System baru maupun lama) |
| Git + Git LFS | untuk file model 3D (.fbx/.glb) dan tekstur |

---

## Menjalankan dari Source Code

### 1. Clone repositori
```bash
git lfs install
git clone URL-REPOSITORI
```
Buka folder hasil clone lewat **Unity Hub › Add › Add project from disk**.

### 2. Pengaturan Player (Edit › Project Settings)
- **XR Plug-in Management › Android:** centang **ARCore**.
- **Player › Android › Other Settings:**
  - Graphics APIs: **OpenGLES3** (hapus Vulkan bila kamera tampil hitam)
  - Minimum API Level: **Android 7.0 (API 24)**
  - Scripting Backend: **IL2CPP**, Target Architectures: **ARM64**
  - Active Input Handling: **Input System Package (New)** atau **Both**
- **XR Plug-in Management › ARCore:** Requirement = **Required**.

### 3. Susunan scene `ARScan`
```
ARScan
├── AR Session            ← NONAKTIFKAN di Inspector (diaktifkan oleh CameraPermissionHandler)
├── XR Origin (Mobile AR)
│   ├── Camera Offset / Main Camera (tag: MainCamera)
│   └── [komponen] AR Tracked Image Manager
│        - Serialized Library : HardwareReferenceLibrary
│        - Max Moving Images  : 2  (Max Number Of Moving Images)
├── Managers
│   ├── ARHardwareManager      (isi hardwareEntries + referensi UI)
│   └── CameraPermissionHandler (isi AR Session + panel izin)
├── Canvas
│   ├── ScanHintPanel     "Arahkan kamera ke kartu marker"
│   ├── LabelPanel        TMP_Text nama komponen
│   ├── InfoButton (i)    OnClick → ARHardwareManager.ShowInfoForActive
│   ├── ResetButton       OnClick → ARHardwareManager.ResetActiveModel
│   ├── BackButton        OnClick → ARHardwareManager.BackToHome
│   ├── InfoPopup         Judul, Fungsi, Spesifikasi + tombol X → CloseInfo
│   └── PermissionPanel   Pesan + Coba Lagi → CheckAndRequest, Pengaturan → OpenAppSettings
└── EventSystem
```

### 4. Reference Image Library
1. **Create › XR › Reference Image Library**, beri nama `HardwareReferenceLibrary`.
2. Tambahkan 13 gambar dari `Markers/marker_png/`. Kolom **Name** harus sama dengan ID di tabel marker (misalnya `marker_cpu`).
3. Centang **Specify Size** dan isi **0,08 m** (lebar gambar cetak).
4. Untuk setiap tekstur marker, *Read/Write Enabled* tidak wajib, tetapi **Compression = None** disarankan.

### 5. ARHardwareManager
- Saat komponen ditambahkan, daftar 13 komponen beserta teks fungsinya terisi otomatis. Bisa juga lewat klik kanan komponen › **Isi Data Default Hardware**.
- Isi slot **Prefab** tiap entri dengan model `HW_<nama>` dari folder `Assets/Models`.
- `Fit To Marker` mengatur lebar model relatif terhadap kartu (1 = selebar gambar marker).
- `TouchInteraction` tidak perlu ditambahkan ke prefab karena dipasang otomatis.

### 6. Build APK
**File › Build Settings ›** atur urutan scene `Home` (0), `ARScan` (1) **› Switch Platform (Android) › Build**.
Beri nama `UTS_AR_NamaKelompok_TI5A2.apk`.

---

## Model 3D (Blender)

- `Blender/AR_Hardware_Laptop.blend` berisi 13 model di collection `AR_Hardware`. Tiap model punya Empty induk `HW_<nama>` dan bagian-bagian bernama, misalnya `Pendingin_Heat_Pipe_0`.
- Model dibangun lewat skrip `tools/build_hardware_models.py`. Buka **Blender › Scripting › Open › Run Script** untuk membangun ulang model dan mengekspor FBX ke folder `Models_FBX/`.
- Pengaturan ekspor FBX: Apply Scalings = FBX All, Forward = -Z, Up = Y, Apply Transform aktif, sehingga model berdiri tegak di Unity tanpa rotasi tambahan.
- Di Unity, seret `Models_FBX/*.fbx` ke `Assets/Models`, lalu isi slot **Prefab** tiap entri ARHardwareManager dengan model `HW_<nama>` yang sesuai.

---

## Struktur Proyek

```
Blender/AR_Hardware_Laptop.blend     sumber model 3D
Models_FBX/HW_*.fbx                  13 model hasil ekspor
Markers/                             kartu cetak, gambar target, PDF
tools/                               make_markers.py, build_hardware_models.py, hardware_data.py
Assets/
├── Scenes/            Home.unity, ARScan.unity
├── Scripts/
│   ├── ARHardwareManager.cs        tracking, spawn model, UI marker terdeteksi/hilang, pop-up info
│   ├── TouchInteraction.cs         rotasi 1 jari, pinch zoom 2 jari, tap model
│   ├── CameraPermissionHandler.cs  izin kamera, pesan bila ditolak, cek dukungan ARCore
│   └── MainMenu.cs                 tombol Beranda
├── Models/            13 model HW_*.fbx (dari Models_FBX)
├── Markers/           13 gambar marker + HardwareReferenceLibrary.asset
└── UI/                ikon, font
```

---

## Cara Kerja Teknis

1. **Deteksi:** `ARTrackedImageManager` mencocokkan titik fitur pada frame kamera dengan gambar di Reference Image Library.
2. **Pembacaan ID:** `trackedImage.referenceImage.name` (misalnya `marker_gpu`) dipakai `ARHardwareManager` untuk memilih prefab.
3. **Pose 3D:** ARCore menghitung posisi dan orientasi kartu (6DoF) dari fitur yang cocok dan ukuran fisik 0,08 m. Model dipasang sebagai *child* objek marker sehingga ikut bergerak bersama kartu.
4. **Skala:** lebar model disesuaikan otomatis dengan lebar kartu (`fitToMarker`), dipusatkan, dan alasnya diletakkan di permukaan kartu.
5. **Marker hilang:** bila `trackingState` bukan `Tracking` (Limited/None), model disembunyikan dan `ScanHintPanel` ditampilkan. Model muncul lagi saat status kembali `Tracking`.

---

## Troubleshooting

| Masalah | Solusi |
|---|---|
| Layar kamera hitam | Pastikan izin kamera diberikan, set Graphics API ke OpenGLES3, dan gunakan perangkat yang mendukung ARCore |
| Marker tidak terdeteksi | Tambah cahaya, kurangi pantulan (pakai kertas doff), jaga jarak 20–50 cm, dan pastikan nama di library sama dengan ID di `hardwareEntries` |
| Model terlalu besar/kecil | Pastikan *Specify Size* di library sama dengan lebar cetak sebenarnya, lalu sesuaikan `Fit To Marker` |
| Model miring/terbalik | Isi `Rotation Offset` pada entri hardware (misalnya X = -90 untuk model dari Blender) |
| Sentuhan tidak berfungsi | Pastikan ada **EventSystem** dan **Main Camera** dengan tag MainCamera, serta Active Input Handling sudah sesuai |
| Peringatan "trackedImagesChanged is obsolete" | Muncul di AR Foundation 6.x dan aman diabaikan, atau gunakan AR Foundation 5.1 |

---

## Aset & Lisensi

| Aset | Sumber | Lisensi |
|---|---|---|
| 13 model 3D (`Models_FBX/`, `Blender/`) | Dibuat sendiri dengan skrip Blender `tools/build_hardware_models.py` | Milik kelompok (bebas dipakai untuk pendidikan) |
| 13 gambar kartu marker (`Markers/`) | Dibuat sendiri dengan `tools/make_markers.py` | Milik kelompok |
| Font Inter (pada kartu) | [rsms.me/inter](https://rsms.me/inter/) | SIL Open Font License 1.1 |
| Materi fungsi komponen | swalayankomputer.com, idntimes.com, erablue.id, scribd.com (lihat referensi di PPT) | Diparafrasekan untuk pendidikan |

Kode sumber proyek ini dibuat untuk keperluan akademik.

---

## Penggunaan GenAI

| Alat | Keperluan |
|---|---|
| Claude (Anthropic) | Draf teks presentasi, skrip C# (ARHardwareManager, TouchInteraction, CameraPermissionHandler, MainMenu), skrip pembuat kartu marker, skrip modeling Blender, dan README. Semua hasil ditinjau dan diuji kelompok di perangkat. |

---

## Riwayat Commit

Setiap anggota melakukan commit dari akunnya masing-masing. Konvensi pesan commit:

- `feat: tambah tracking marker`
- `fix: model hilang saat marker tertutup`
- `docs: update README`
