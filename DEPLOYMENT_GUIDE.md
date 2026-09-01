# 📋 Panduan Deployment Sistem Absensi Kantor
## Google Apps Script Full-Stack Web Application

---

## 🚀 QUICK START (5 Menit)

### 1. **Buka Google Apps Script**
   - Kunjungi [script.google.com](https://script.google.com)
   - Klik **"New project"**
   - Beri nama: `Sistem Absensi Kantor`

### 2. **Copy-Paste Kode ke GAS**

Urutan file yang harus dicopy-paste:

#### **File 1: Kode.gs (Backend)**
```
1. Di project GAS, klik "+" untuk tambah file
2. Pilih "Script"
3. Ganti nama file dari "Untitled" menjadi "Kode"
4. Copy-paste seluruh isi dari 01_Kode.gs
5. Tekan Ctrl+S (Save)
```

#### **File 2: Index.html (Main HTML)**
```
1. Di sidebar kiri, klik "+" → "HTML"
2. Ganti nama menjadi "Index"
3. Copy-paste seluruh isi dari 02_Index.html
4. Tekan Ctrl+S
```

#### **File 3: Stylesheet.html (CSS)**
```
1. Klik "+" → "HTML"
2. Ganti nama menjadi "Stylesheet"
3. Copy-paste seluruh isi dari 03_Stylesheet.html
4. Tekan Ctrl+S
```

#### **File 4: JavaScript.html (Frontend Logic)**
```
1. Klik "+" → "HTML"
2. Ganti nama menjadi "JavaScript"
3. Copy-paste seluruh isi dari 04_JavaScript.html
4. Tekan Ctrl+S
```

**Struktur file di GAS harus terlihat seperti:**
```
📁 Sistem Absensi Kantor
  📄 Kode.gs
  📄 Index.html
  📄 Stylesheet.html
  📄 JavaScript.html
```

### 3. **Setup Environment**
```
1. Di GAS, pilih function "setupAppEnvironment" dari dropdown
2. Tekan tombol play (▶️) untuk jalankan
3. Tunggu hingga selesai (lihat di "Execution log")
4. Catat nilai:
   - Folder ID
   - Spreadsheet ID
```

✅ Jika berhasil, di Google Drive akan terbuat folder `SistemAbsensiKantor` dengan database Spreadsheet.

### 4. **Deploy as Web App**
```
1. Di GAS, klik "Deploy" → "New deployment"
2. Pilih type: "Web app"
3. Isi:
   - Description: "Sistem Absensi Kantor v1.0"
   - Execute as: [Akun kamu]
   - Who has access: "Anyone"
4. Klik "Deploy"
5. Salin URL yang diberikan (format: https://script.google.com/macros/d/...)
6. Bagikan URL ini ke guru & admin
```

✅ **Aplikasi sudah live!** Guru & admin bisa akses via link tersebut.

---

## 📊 DATABASE SETUP (Auto-Created)

Setelah `setupAppEnvironment()` selesai, akan terbuat struktur database:

### **Sheet: Data_Guru**
| ID | Nama | NIP | No_HP | Email | Foto_Wajah_URL | Status_Aktif | Tgl_Masuk | Role |
|---|---|---|---|---|---|---|---|---|
| UUID | Dr. Robert Chen | 19800512... | +62... | r.chen@... | (URL foto) | Aktif | dd/mm/yyyy | Guru |

### **Sheet: Rekam_Absensi**
| ID_Absensi | ID_Guru | Nama_Guru | Tanggal | Jam_Masuk | Jam_Keluar | Status_Verifikasi | Tipe_Verifikasi | Is_Lembur | Durasi_Lembur_Jam |
|---|---|---|---|---|---|---|---|---|---|

### **Sheet: Hari_Libur**
| ID | Tanggal | Keterangan | Tipe | Dibuat_Oleh |
|---|---|---|---|---|

### **Sheet: Akun_Pengguna**
| ID | Email | Nama_Lengkap | Role | Status |
|---|---|---|---|---|
| UUID | admin@school.com | Admin Super | Admin Super | Aktif |
| UUID | kepsek@school.com | Kepala Sekolah | Kepala Sekolah | Aktif |
| UUID | tu@school.com | Tata Usaha | TU | Aktif |

### **Sheet: AppConfig**
| Key | Value |
|---|---|
| appName | Sistem Absensi Kantor |
| whatsappProvider | twilio |
| whatsappApiKey | (Isi nanti) |

---

## 👥 SETUP AKUN PENGGUNA

### **1. Edit Akun_Pengguna Sheet**
   - Buka Spreadsheet `DB_Absensi_Kantor` di Google Drive
   - Edit tab `Akun_Pengguna`
   - Ganti email dengan akun Google Workspace sekolah:
     ```
     admin@sekolahku.sch.id → Admin Super
     kepsek@sekolahku.sch.id → Kepala Sekolah
     tu@sekolahku.sch.id → TU
     guru1@sekolahku.sch.id → Guru
     guru2@sekolahku.sch.id → Guru
     ```

### **2. Role-Based Access Control (RBAC)**

| Role | Dashboard | Absensi | Review | Data Guru | Hari Libur | Laporan | Pengaturan |
|---|---|---|---|---|---|---|---|
| **Guru** | ❌ | ✅ (Scan) | ❌ | ❌ | ❌ | ❌ | ✅ |
| **Admin** | ✅ (View) | ❌ | ✅ (Review) | ✅ (Read) | ❌ | ✅ | ✅ |
| **TU** | ✅ (View) | ❌ | ✅ (Review) | ✅ (Read) | ❌ | ✅ | ❌ |
| **Admin Super** | ✅ (View) | ❌ | ✅ (Review) | ✅ (Edit) | ✅ (Edit) | ✅ | ✅ (Full) |
| **Kepala Sekolah** | ✅ (View) | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |

---

## 🔐 KONFIGURASI WHATSAPP (Opsional)

Jika ingin enable notifikasi WhatsApp otomatis:

### **1. Setup Twilio Account**
   - Buka [twilio.com](https://www.twilio.com)
   - Daftar akun & dapatkan Twilio Phone Number
   - Ambil **Account SID** dan **Auth Token**
   - Setup WhatsApp Sandbox

### **2. Input ke AppConfig**
   - Buka Spreadsheet `DB_Absensi_Kantor`
   - Edit tab `AppConfig`
   - Isi:
     ```
     whatsappProvider = twilio
     whatsappApiKey = <Twilio Auth Token>
     ```

### **3. Test Notifikasi**
   - Guru scan absensi
   - Seharusnya dapat WA di nomor terdaftar:
     ```
     ✅ Absensi Berhasil
     Nama: Budi Santoso
     Tanggal: 1 September 2024
     Jam Masuk: 07:15
     Status: BERHASIL
     ```

---

## 🖥️ TESTING GUIDE

### **Test 1: Setup Berhasil**
```
1. Buka GAS project
2. Jalankan setupAppEnvironment()
3. Cek di Google Drive → Folder "SistemAbsensiKantor" ada
4. Buka Spreadsheet "DB_Absensi_Kantor"
5. Pastikan 5 tab sheet ada (Data_Guru, Rekam_Absensi, Hari_Libur, Akun_Pengguna, AppConfig)
```
✅ **Jika semua ada, setup berhasil!**

### **Test 2: Login dengan Akun Berbeda**
```
1. Deploy aplikasi sebagai Web App
2. Buka link deployment dengan 3 akun:
   - Admin Super → seharusnya lihat semua menu
   - Guru → seharusnya hanya lihat "Absensi" menu
   - Kepala Sekolah → seharusnya hanya lihat "Dashboard" & "Laporan"
3. Cek di console (F12 → Console) untuk debug
```

### **Test 3: CRUD Data Guru**
```
1. Login sebagai Admin Super
2. Klik "Data Guru"
3. Klik "Tambah Guru Baru"
4. Isi form (Nama, NIP, Email, No HP)
5. Klik "Simpan"
6. Periksa di Spreadsheet → Data_Guru sheet → ada guru baru
7. Edit atau Hapus guru
```

### **Test 4: Absensi Wajah**
```
1. Login sebagai Guru
2. Klik "Absensi"
3. Browser akan minta akses kamera → Terima
4. Klik "Verifikasi Wajah"
5. Cek di Spreadsheet → Rekam_Absensi sheet → ada record baru
```

### **Test 5: Review & Override**
```
1. Login sebagai Admin
2. Klik "Review"
3. Pilih tanggal
4. Klik "Detail" pada record absensi
5. Ubah status (Berhasil → Gagal atau sebaliknya)
6. Cek perubahan di Spreadsheet
```

### **Test 6: Laporan Bulanan**
```
1. Login sebagai Kepala Sekolah
2. Klik "Laporan"
3. Pilih Bulan & Tahun
4. Klik "Tampilkan Laporan"
5. Seharusnya tampil 2 grafik:
   - Bar chart Kehadiran Per Guru
   - Bar chart Jam Lembur Per Guru
6. Tabel detail di bawah
```

---

## 🐛 TROUBLESHOOTING

### **Error: "Spreadsheet tidak ditemukan"**
```
Solusi:
1. Jalankan ulang setupAppEnvironment()
2. Cek folder "SistemAbsensiKantor" di Drive
3. Cek permissions (aplikasi harus punya akses Drive)
```

### **Kamera tidak bisa diakses**
```
Solusi:
1. Browser permission: klik lock icon → Allow Camera
2. Test di browser lain (Chrome recommended)
3. Cek firewall/antivirus yang blokir kamera
4. Https required untuk webcam (GAS deployment auto https ✅)
```

### **Notifikasi WA tidak terkirim**
```
Solusi:
1. Cek Twilio API key di AppConfig benar
2. Cek nomor HP guru di Data_Guru valid & format +62
3. Twilio sandbox aktif & nomor guru sudah subscribe
4. Cek execution log di GAS untuk error details
```

### **Grafik Laporan tidak tampil**
```
Solusi:
1. Cek data di Rekam_Absensi ada
2. Browser console (F12) lihat error
3. Clear localStorage (F12 → Application → Clear)
4. Refresh page
```

### **Dark mode tidak berjalan**
```
Solusi:
1. localStorage mungkin disabled
2. Cek browser settings → Privacy → Local Storage: Allow
3. Atau disable di apploaderOverlay
```

---

## 📱 DEPLOYMENT UNTUK MOBILE

Aplikasi ini **responsive** dan bisa dibuka di smartphone:

### **1. Share URL Deployment**
   ```
   Bagikan link ini ke guru:
   https://script.google.com/macros/d/[PROJECT_ID]/usercopy/exec
   ```

### **2. Guru buka di mobile browser**
   - Safari (iPhone) atau Chrome (Android)
   - Klik "Add to Home Screen" untuk shortcut

### **3. Permissions yang perlu granted**
   - ✅ Camera (untuk facial recognition)
   - ✅ Location (opsional, untuk geo-tagging)
   - ✅ Microphone (opsional)

---

## 🔄 UPDATE & MAINTENANCE

### **Update Kode**
```
1. Buka GAS project → Edit file Kode.gs
2. Edit function yang ingin diubah
3. Save (Ctrl+S)
4. Redeploy:
   - Klik "Deploy" → "Manage deployments"
   - Klik deployment yang aktif
   - Klik "Update" (icon pensil)
   - Save
5. User tidak perlu clear cache, auto reload
```

### **Backup Data**
```
1. Monthly: Download Spreadsheet sebagai Excel
   - Di Sheet, klik "File" → "Download" → "Excel"
2. Store di cloud backup (Google Drive/Dropbox)
```

### **Monitor Performance**
```
Di GAS, buka "Execution log" untuk lihat:
- Berapa lama setiap function berjalan
- Error logs
- User activity (siapa login kapan)
```

---

## 🎯 TIPS PRODUCTION

### **1. Security**
   - ✅ GAS deployment auto HTTPS
   - ✅ RBAC built-in via role check
   - ✅ Never share Admin SuperPassword
   - ⚠️ Jangan share /dev URL, hanya /exec URL

### **2. Performance**
   - ✅ Cache data di localStorage (sudah built-in)
   - ⚠️ Jika >500 guru, consider migrasi ke Firebase
   - ⚠️ Jika traffic tinggi, enable GAS CacheService

### **3. Monitoring**
   - ⚠️ Setup Google Forms untuk feedback
   - ⚠️ Monitor execution log mingguan
   - ⚠️ Setup email alert untuk errors

---

## 📞 SUPPORT & DOCUMENTATION

- **PRD Lengkap:** `/prd-sistem-absensi-kantor.md`
- **Design System:** `/DESIGN.md`
- **Video Tutorial:** [Link tutorial lengkap]
- **FAQ:** [Link FAQ]

---

## 📝 VERSION HISTORY

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Sep 1, 2024 | Initial release |
| 1.1 (Planned) | Oct 2024 | Mobile app + SMS fallback |
| 2.0 (Planned) | Dec 2024 | Firebase migration + Analytics |

---

**Selamat! Aplikasi Sistem Absensi Kantor siap digunakan! 🎉**

Untuk support lebih lanjut, hubungi admin sekolah atau developer.
