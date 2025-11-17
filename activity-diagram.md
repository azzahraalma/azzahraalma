# Activity Diagram - Aplikasi Manajemen Keuangan

## Diagram Aktivitas

```mermaid
graph TD
    Start([Start]) --> InputLogin[User: Input Login / Pilih Registrasi]
    InputLogin --> ValidasiAkun{System: Validasi Akun}
    ValidasiAkun -->|Registrasi| SimpanAkun[System: Simpan Data Akun]
    ValidasiAkun -->|Login Valid| MasukAplikasi[User: Masuk ke Aplikasi]
    SimpanAkun --> MasukAplikasi
    
    MasukAplikasi --> TampilHomepage[System: Tampilkan Homepage]
    TampilHomepage --> MenuUtama{User: Pilih Menu}
    
    %% Fork - Profil
    MenuUtama -->|Pilih Profil| AmbilDataProfil[System: Ambil Data Profil]
    AmbilDataProfil --> TampilProfil[System: Tampilkan Profil]
    TampilProfil --> MenuUtama
    
    %% Fork - Transaksi
    MenuUtama -->|Pilih Transaksi| TampilMenuTransaksi[System: Tampilkan Menu Transaksi]
    TampilMenuTransaksi --> TambahEditHapus[User: Tambah/Edit/Hapus Transaksi]
    TambahEditHapus --> SimpanTransaksi[System: Simpan Data Transaksi]
    SimpanTransaksi --> UpdateTampilan[System: Update Tampilan Transaksi]
    UpdateTampilan --> MenuUtama
    
    %% Fork - Budget
    MenuUtama -->|Pilih Budget| TampilMenuBudget[System: Tampilkan Menu Budget]
    TampilMenuBudget --> AturBudget[User: Atur Prioritas & Batas]
    AturBudget --> SimpanBudget[System: Simpan Data Budget]
    SimpanBudget --> CekOverlimit{System: Cek Overlimit?}
    CekOverlimit -->|Ya| KirimNotifOverlimit[Notification: Kirim Notifikasi Overlimit]
    CekOverlimit -->|Tidak| MenuUtama
    KirimNotifOverlimit --> MenuUtama
    
    %% Fork - Tabungan
    MenuUtama -->|Pilih Tabungan| TampilMenuTabungan[System: Tampilkan Menu Tabungan]
    TampilMenuTabungan --> SimpanTarget[User: Simpan Target Tabungan]
    SimpanTarget --> SimpanDataTabungan[System: Simpan Data Tabungan]
    SimpanDataTabungan --> CekJadwal{System: Cek Jadwal Reminder?}
    CekJadwal -->|Waktunya| KirimReminder[Notification: Kirim Reminder Tabungan]
    CekJadwal -->|Belum| MenuUtama
    KirimReminder --> MenuUtama
    
    %% Fork - Laporan
    MenuUtama -->|Pilih Laporan| GenerateLaporan[System: Generate Laporan]
    GenerateLaporan --> TampilGrafik[System: Tampilkan Grafik/Rangkuman]
    TampilGrafik --> MenuUtama
    
    %% Logout
    MenuUtama -->|Logout| HentikanSesi[System: Hentikan Sesi]
    HentikanSesi --> End([End])
    
    %% Styling
    classDef userAction fill:#e1f5ff,stroke:#01579b,stroke-width:2px
    classDef systemAction fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef notificationAction fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef decision fill:#fff9c4,stroke:#f57f17,stroke-width:2px
    
    class InputLogin,TambahEditHapus,AturBudget,SimpanTarget userAction
    class ValidasiAkun,SimpanAkun,MasukAplikasi,TampilHomepage,AmbilDataProfil,TampilProfil,TampilMenuTransaksi,SimpanTransaksi,UpdateTampilan,TampilMenuBudget,SimpanBudget,CekOverlimit,TampilMenuTabungan,SimpanDataTabungan,CekJadwal,GenerateLaporan,TampilGrafik,HentikanSesi systemAction
    class KirimNotifOverlimit,KirimReminder notificationAction
    class MenuUtama decision
```

## Penjelasan Swimlane

### 🔵 User Actions (Biru)
- Input login/registrasi
- Pilih menu (Profil, Transaksi, Budget, Tabungan, Laporan)
- Tambah/Edit/Hapus data
- Atur prioritas dan batas budget
- Simpan target tabungan
- Logout

### 🟠 System Actions (Orange)
- Validasi akun
- Simpan data (akun, transaksi, budget, tabungan)
- Tampilkan halaman (Homepage, Profil, Menu)
- Update tampilan
- Cek kondisi (overlimit, jadwal reminder)
- Generate laporan
- Hentikan sesi

### 🟣 Notification Actions (Ungu)
- Kirim notifikasi overlimit budget
- Kirim reminder tabungan

## Alur Utama

1. **Login/Registrasi** → Validasi → Masuk ke aplikasi
2. **Homepage** → Pilih salah satu menu:
   - **Profil**: Lihat dan kelola profil pengguna
   - **Transaksi**: Tambah, edit, atau hapus transaksi keuangan
   - **Budget**: Atur budget dengan notifikasi overlimit
   - **Tabungan**: Set target dengan reminder otomatis
   - **Laporan**: Lihat grafik dan rangkuman keuangan
3. **Logout** → Keluar dari aplikasi

## Fork & Join Pattern

Diagram ini menggunakan pola **Fork-Join** di mana setelah masuk ke homepage, user dapat memilih berbagai menu (fork), dan setelah selesai dengan menu tersebut, kembali ke menu utama (join) untuk memilih menu lain atau logout.
