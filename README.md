# Omnitoon Releases

[![Rilis terbaru](https://img.shields.io/github/v/release/uznbt/omnitoon-releases?label=rilis%20terbaru)](https://github.com/uznbt/omnitoon-releases/releases/latest)
[![Total unduhan](https://img.shields.io/github/downloads/uznbt/omnitoon-releases/total?label=unduhan)](https://github.com/uznbt/omnitoon-releases/releases)

Mirror publik paket rilis **Omnitoon**, aplikasi manga dan komik reader dengan
backend Rust. Repositori ini hanya berisi aset rilis; kode sumber dikelola
secara privat.

- Server lokal: `http://127.0.0.1:47821`
- Katalog Komiku, Wurmz, dan CGBUM sudah tertanam di setiap paket
- Android 7.0+ (`minSdk 24`), Windows 10/11 x64, Linux x86_64

## Unduhan terbaru

Buka halaman [rilis terbaru](https://github.com/uznbt/omnitoon-releases/releases/latest)
untuk melihat semua berkas dan checksum.

### Android

| Berkas | Keterangan | Ukuran |
| --- | --- | ---: |
| [Omnitoon-universal.apk](https://github.com/uznbt/omnitoon-releases/releases/latest/download/Omnitoon-universal.apk) | arm64-v8a + armeabi-v7a. Pilih ini kalau ragu. | ±100 MB |
| [Omnitoon-arm64-v8a.apk](https://github.com/uznbt/omnitoon-releases/releases/latest/download/Omnitoon-arm64-v8a.apk) | Split untuk HP 64-bit ARM. | ±51 MB |
| [Omnitoon-armeabi-v7a.apk](https://github.com/uznbt/omnitoon-releases/releases/latest/download/Omnitoon-armeabi-v7a.apk) | Split untuk HP 32-bit ARM. | ±50 MB |
| [Omnitoon-x86_64.apk](https://github.com/uznbt/omnitoon-releases/releases/latest/download/Omnitoon-x86_64.apk) | Eksperimental untuk emulator x86_64. | ±53 MB |

APK node membawa cloudflared dan token tunnel. Baca bagian
[Keamanan](#keamanan) sebelum memasangnya.

### Windows

| Berkas | Keterangan |
| --- | --- |
| `Omnitoon_<versi>_x64-setup.exe` | Installer NSIS |
| `Omnitoon_<versi>_x64_en-US.msi` | Installer MSI |
| `Omnitoon-<versi>-windows-x64-portable.zip` | Versi portable |
| [Omnitoon-desktop.exe](https://github.com/uznbt/omnitoon-releases/releases/latest/download/Omnitoon-desktop.exe) | Binary langsung tanpa installer |

Paket Windows belum ditandatangani. SmartScreen mungkin menampilkan
peringatan; pilih **More info** lalu **Run anyway** hanya jika berkas sudah
diverifikasi lewat checksum.

### Linux

| Berkas | Distro |
| --- | --- |
| `Omnitoon_<versi>_amd64.deb` | Debian, Ubuntu, Kali, Linux Mint |
| `Omnitoon-<versi>-1.x86_64.rpm` | Fedora, openSUSE |
| `Omnitoon_<versi>_amd64.AppImage` | Semua distro x86_64 |
| `Omnitoon-<versi>-linux-x86_64-portable.tar.gz` | Semua distro x86_64 |
| `omnitoon-bin-<versi>-1-x86_64.pkg.tar.zst` | Arch, Manjaro |

Contoh pemasangan (ganti `0.1.8` dengan versi terbaru):

```sh
# Debian/Ubuntu
sudo apt install ./Omnitoon_0.1.8_amd64.deb

# Fedora/openSUSE
sudo dnf install ./Omnitoon-0.1.8-1.x86_64.rpm

# Arch/Manjaro
sudo pacman -U omnitoon-bin-0.1.8-1-x86_64.pkg.tar.zst

# AppImage
chmod +x Omnitoon_0.1.8_amd64.AppImage
./Omnitoon_0.1.8_amd64.AppImage
```

## Memasang di Android

1. Unduh `Omnitoon-universal.apk` dari halaman rilis.
2. Izinkan browser atau pengelola berkas memasang aplikasi dari sumber tidak
   dikenal.
3. Buka APK dan tunggu proses pemasangan selesai.
4. Untuk pembaruan berikutnya, buka **Pengaturan → Periksa pembaruan**.

APK memakai keystore yang sama antar rilis supaya versi baru bisa dipasang di
atas versi lama. Jangan menimpa Omnitoon dengan APK yang ditandatangani kunci
berbeda.

Pengguna v0.1.7 perlu memasang v0.1.8 secara manual satu kali. Setelah itu
pembaruan dalam aplikasi memakai server lokal yang mendukung resume dan
verifikasi SHA-256.

## Verifikasi checksum

Setiap rilis menyertakan dua berkas checksum:

- [SHA256SUMS.txt](https://github.com/uznbt/omnitoon-releases/releases/latest/download/SHA256SUMS.txt) untuk semua aset rilis
- [SHA256SUMS-apk.txt](https://github.com/uznbt/omnitoon-releases/releases/latest/download/SHA256SUMS-apk.txt) khusus keempat APK Android

Linux dan macOS:

```sh
sha256sum -c SHA256SUMS.txt
sha256sum -c SHA256SUMS-apk.txt
```

Windows PowerShell:

```powershell
Get-FileHash .\Omnitoon-universal.apk -Algorithm SHA256
```

Bandingkan hasilnya dengan isi berkas checksum. Jangan pasang berkas yang
hash-nya tidak cocok.

## Pembaruan aplikasi

- **v0.1.8 dan setelahnya:** aplikasi meminta server lokal mengunduh APK dari
  repositori ini. Unduhan bisa dilanjutkan setelah terputus, disimpan per tag,
  diverifikasi SHA-256, lalu disajikan ke Android lewat loopback.
- **v0.1.7 dan sebelumnya:** pembaruan masih memakai jalur langsung GitHub.
  Jika unduhan terpotong, pasang APK v0.1.8 secara manual satu kali.

Endpoint pembaruan dan `/api/apk` hanya melayani loopback, bukan dari domain
publik.

## Keamanan

- APK node berisi token Cloudflare Tunnel yang tertanam. Token dienkripsi di
  dalam vault, tetapi tetap bisa diekstrak dari perangkat.
- Simpan APK hanya di perangkat tepercaya dan jangan mengunggahnya kembali ke
  hosting publik. Jika token bocor, segera ganti token tunnel.
- Tunnel tidak menyala otomatis. Aktifkan mirror node lewat **Pengaturan**
  saat ingin membagikan situs.
- Ingress Cloudflare harus diarahkan ke `http://127.0.0.1:8080`.
- Katalog `play.db.omz` tertanam di paket dan disemai ke folder data saat
  pertama dijalankan, jadi tidak perlu mengunduh database terpisah.

## Isi repositori

Repositori ini hanya berisi aset rilis. Setiap tag `v*` di repositori sumber
dicerminkan ke sini oleh workflow rilis. Jangan menambahkan kode sumber atau
berkas lain ke repositori ini.

## Lisensi

Omnitoon didistribusikan di bawah GNU GPL-3.0.
