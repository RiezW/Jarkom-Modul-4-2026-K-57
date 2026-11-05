# Jarkom-Modul-4-2026-K-57

| | |
|---|---|
| **Nama** | : Riezco Eka Bayu Witantra & Sulthan Daffa Al Hasyimi |
| **NRP** | : 5027251057 & 5027251091 |
| **Kelompok** | : K-57 |
| **Mata Kuliah** | : JarKom |

---

## Ringkasan

Praktikum Modul 4 membangun jaringan **The Wired** berdasarkan topologi dan kebutuhan host yang diberikan. Soal meminta perhitungan alamat dan routing seefisien mungkin, serta memastikan semua node terhubung dan dapat saling melakukan ping. Metode yang digunakan berbeda untuk tiap platform: **VLSM di Cisco Packet Tracer** dan **CIDR di GNS3**.

Laporan ini membagi dokumentasi menjadi dua bagian: **1. CIDR** untuk pengerjaan GNS3 dan **2. VLSM** untuk pengerjaan Cisco Packet Tracer.

![Topologi jaringan](Assets/GNS3/topologi_lingkaran.png)

![Topologi level B](Assets/GNS3/level-B.png)

![Topologi level C](Assets/GNS3/level-C.png)

![Topologi level D](Assets/GNS3/level-D.png)

![Topologi level E](Assets/GNS3/level-E.png)

![Topologi level F](Assets/GNS3/level-F.png)

![Topologi level G](Assets/GNS3/level-G.png)

![Topologi level H](Assets/GNS3/level-H.png)

![Topologi level I](Assets/GNS3/level-I.png)

## 1. CIDR

### 1.1 Berkas Dokumentasi

| Lokasi | Isi |
|---|---|
| [`GNS3 - CIDR/script.sh-router.md`](<GNS3 - CIDR/script.sh-router.md>) | Script konfigurasi setiap router |
| [`GNS3 - CIDR/clientsh.md`](<GNS3 - CIDR/clientsh.md>) | Script IP dan gateway setiap client |
| [`GNS3 - CIDR/autostart.md`](<GNS3 - CIDR/autostart.md>) | Konfigurasi autostart |
| [`GNS3 - CIDR/cara test.md`](<GNS3 - CIDR/cara test.md>) | Checklist dan langkah pengujian |
| `Assets/GNS3/` | Gambar topologi, pembagian subnet, dan bukti pengujian |

### 1.2 Topologi dan Perencanaan Alamat

Topologi terdiri dari Cyberia sebagai router yang mengarah ke NAT, Wired sebagai router inti, dan router-router cabang. Jumlah kebutuhan host pada setiap subnet mencakup node akhir dan interface router yang terhubung ke subnet tersebut.

![Topologi GNS3](Assets/GNS3/topologi.png)

![Pembagian subnet dan kebutuhan host](Assets/GNS3/pembagianip.png)

| Subnet | Network/prefix | Kebutuhan host | Gateway atau endpoint link | Node pada LAN |
|---|---|---:|---|---|
| A1 | `10.92.28.0/25` | 126 | Cyberia `10.92.28.1` | JJ, MyuMyu |
| A2 | `10.92.28.144/30` | 2 | Cyberia `.145`, Wired `.146` | Link Cyberia-Wired |
| A3 | `10.92.28.128/29` | 4 | Wired `.129`, Psyche `.130`, Copland `.131`, Schumann `.132` | Link router cabang |
| A4 | `10.92.8.0/22` | 881 | Psyche `10.92.8.1` | Kari, Lin |
| A5 | `10.92.14.0/26` | 32 | Copland `10.92.14.1` | Juri |
| A6 | `10.92.12.0/23` | 301 | Copland `10.92.12.1` | Masayuki |
| A7 | `10.92.4.0/29` | 4 | Schumann `10.92.4.1` | Akashic |
| A8 | `10.92.4.8/30` | 2 | Schumann `.9`, Aceta `.10` | Link Schumann-Aceta |
| A9 | `10.92.0.0/22` | 514 | Aceta `10.92.0.1` | Omnipresence, Denpa |
| A10 | `10.92.28.136/30` | 2 | Wired `.137`, Navi `.138` | Link Wired-Navi |
| A11 | `10.92.24.0/24` | 254 | Navi `10.92.24.1` | Alice, Reika |
| A12 | `10.92.26.0/26` | 58 | Navi `10.92.26.1` | Taro |
| A13 | `10.92.25.0/24` | 143 | Navi `10.92.25.1` | Yasuo, Miho, Kuma |
| A14 | `10.92.28.140/30` | 2 | Wired `.141`, Tachibana `.142` | Link Wired-Tachibana |
| A15 | `10.92.20.0/23` | 267 | Tachibana `10.92.20.1` | Collective, Ghost |
| A16 | `10.92.18.128/30` | 2 | Tachibana `.129`, Deus `.130` | Link Tachibana-Deus |
| A17 | `10.92.18.0/25` | 99 | Deus `10.92.18.1` | Lilith, Specter |
| A18 | `10.92.18.132/30` | 2 | Deus `.133`, Protocol7 `.134` | Link Deus-Protocol7 |
| A19 | `10.92.17.0/26` | 33 | Protocol7 `10.92.17.1` | Chisa, Hodgeson |
| A20 | `10.92.17.64/30` | 2 | Protocol7 `.65`, Knights `.66` | Link Protocol7-Knights |
| A21 | `10.92.16.128/29` | 3 | Knights `10.92.16.129` | Eri |
| A22 | `10.92.16.136/30` | 2 | Knights `.137`, Phantoma `.138` | Link Knights-Phantoma |
| A23 | `10.92.16.64/27` | 15 | Phantoma `10.92.16.65` | Semi |
| A24 | `10.92.16.96/30` | 2 | Phantoma `.97`, Shibuya `.98` | Link Phantoma-Shibuya |
| A25 | `10.92.16.0/26` | 62 | Shibuya `10.92.16.1` | Lain, Mika |

### 1.3 Perhitungan CIDR dan Routing

Prefix dipilih berdasarkan kebutuhan host pada tiap subnet. Untuk subnet LAN, jumlah alamat usable harus mencukupi host dan interface router; untuk link point-to-point digunakan subnet kecil seperti `/30`. Setelah subnet individual ditentukan, jaringan cabang diringkas menggunakan prefix bersama agar router inti tidak perlu menyimpan satu route untuk setiap LAN.

![Pengelompokan subnet CIDR](Assets/GNS3/penggabungancidr1.png)

![Ringkasan CIDR tingkat atas](Assets/GNS3/penggabungancidr2.png)

Prefix pada gambar menunjukkan penggabungan bertingkat. Tabel berikut merangkum route yang direncanakan dari hasil penggabungan:

| Router | Tujuan | Route ringkasan | Next hop |
|---|---|---|---|
| Wired | A9 dan A7-A8 (C4) | `10.92.0.0/21` | Schumann `10.92.28.132` |
| Wired | A5-A6 (B7) | `10.92.12.0/22` | Copland `10.92.28.131` |
| Wired | A11-A13 (C6) | `10.92.24.0/22` | Navi `10.92.28.138` |
| Wired | Cabang Tachibana (G1) | `10.92.16.0/21` | Tachibana `10.92.28.142` |
| Tachibana | A17 dan cabang di bawah Deus (C2) | `10.92.18.0/24` | Deus `10.92.18.130` |
| Deus | A19-A25 (E1) | `10.92.16.0/23` | Protocol7 `10.92.18.134` |
| Protocol7 | A21-A25 (D1) | `10.92.16.0/24` | Knights `10.92.17.66` |
| Knights | A23-A25 (C1) | `10.92.16.0/25` | Phantoma `10.92.16.138` |

Router cabang menggunakan default route ke router induknya, sedangkan Wired menyimpan route menuju cabang. Cyberia menjadi jalur keluar ke NAT; NAT masquerade dan IP forwarding dikonfigurasi di router tersebut.

![Pemetaan subnet dan route](Assets/GNS3/subnetroute.png)

### 1.4 Implementasi Konfigurasi

Script router mengaktifkan IP forwarding, memasang alamat pada interface, menambahkan route, dan mengatur NAT pada Cyberia. Script client memasang alamat statis dan default gateway sesuai subnetnya. Namun, script router dan client yang tersedia masih memakai skema IP sebelumnya; jangan jalankan sebelum konfigurasi tersebut diperbarui mengikuti tabel dan gambar terbaru.

1. Pada node, edit file konfigurasi dengan `nano /root/script.sh` dan masukkan script yang sesuai.
2. Beri izin eksekusi dengan `chmod +x /root/script.sh`.
3. Jalankan konfigurasi dengan `bash /root/script.sh`.
4. Konfigurasi autostart dijelaskan di [`GNS3 - CIDR/autostart.md`](<GNS3 - CIDR/autostart.md>).

Script dan alamat lengkap per node tersedia pada [konfigurasi router](<GNS3 - CIDR/script.sh-router.md>) dan [konfigurasi client](<GNS3 - CIDR/clientsh.md>). Sebelum menjalankan script kembali, periksa alamat dan route yang sudah terpasang agar tidak terjadi penambahan duplikat.

### 1.5 Pengujian

Pengujian menggunakan `ip -br addr`, `ip route`, dan `ping -c 4 <alamat-tujuan>`. Screenshot terbaru menunjukkan empat ping berhasil tanpa packet loss:

| Sumber | Tujuan | Hasil |
|---|---|---|
| JJ | MyuMyu (`10.92.28.3`) | 4 paket diterima, 0% packet loss |
| Karl | JJ (`10.92.28.2`) | 4 paket diterima, 0% packet loss |
| Schumann | Aceta (`10.92.4.10`) | 4 paket diterima, 0% packet loss |
| Wired | Cyberia (`10.92.28.145`) | 4 paket diterima, 0% packet loss |

![Ping JJ ke MyuMyu](Assets/GNS3/jj-myumyu.png)

![Ping Karl ke JJ](Assets/GNS3/karl-jj.png)

![Ping Schumann ke Aceta](Assets/GNS3/schumann-aceta.png)

![Ping Wired ke Cyberia](Assets/GNS3/wired-cyberia.png)

Screenshot pemeriksaan route Wired memperlihatkan alamat interface dan route ringkasan, sedangkan pemeriksaan Shibuya memperlihatkan alamat interface serta default route:

![Alamat dan route Wired](Assets/GNS3/wired.png)

![Alamat dan route Shibuya](Assets/GNS3/shibuya.png)

Bukti ini mengonfirmasi koneksi yang diuji dan konfigurasi pada Wired serta Shibuya, tetapi belum mencakup pengujian semua node. Checklist di [`GNS3 - CIDR/cara test.md`](<GNS3 - CIDR/cara test.md>) masih memakai alamat lama dan perlu diperbarui sebelum pengujian lanjutan.

### 1.6 Kesimpulan

Perancangan GNS3 membagi prefix `10.92.0.0/19` menjadi 25 subnet dengan ukuran sesuai jumlah host dan kebutuhan link ant-router. Static route dan route summarization direncanakan untuk menghubungkan cabang melalui Wired, sementara Cyberia menyediakan koneksi ke NAT. Sebelum konfigurasi atau pengujian, selaraskan script dan checklist dengan skema terbaru.

## 2. VLSM
