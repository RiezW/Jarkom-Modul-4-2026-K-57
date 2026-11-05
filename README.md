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
| A4 | `10.92.8.0/22` | 881 | Psyche `10.92.8.1` | Karl, Lin |
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

![Pohon subnetting](Assets/GNS3/pohon%20subnetting.png)

Tabel berikut merangkum prefix dari setiap tahap penggabungan. Diagram level B-I di bawah memetakan hasil penggabungan tersebut ke topologi jaringan.

| Level | Hasil penggabungan | Network/prefix | Gabungan dari |
|---|---|---|---|
| B | B1 | `10.92.16.64/26` | A23 + A24 |
| B | B2 | `10.92.16.128/28` | A21 + A22 |
| B | B3 | `10.92.17.0/25` | A19 + A20 |
| B | B4 | `10.92.18.128/29` | A16 + A18 |
| B | B5 | `10.92.24.0/23` | A11 + A13 |
| B | B6 | `10.92.4.0/28` | A7 + A8 |
| B | B7 | `10.92.12.0/22` | A6 + A5 |
| B | B8 | `10.92.28.136/29` | A10 + A14 |
| C | C1 | `10.92.16.0/25` | A25 + B1 |
| C | C2 | `10.92.18.0/24` | A17 + B4 |
| C | C3 | `10.92.28.128/28` | A3 + B8 |
| C | C4 | `10.92.0.0/21` | A9 + B6 |
| C | C5 | `10.92.8.0/21` | A4 + B7 |
| C | C6 | `10.92.24.0/22` | B5 + A12 |
| D | D1 | `10.92.16.0/24` | C1 + B2 |
| D | D2 | `10.92.28.128/27` | C3 + A2 |
| D | D3 | `10.92.0.0/20` | C4 + C5 |
| E | E1 | `10.92.16.0/23` | D1 + B3 |
| E | E2 | `10.92.28.0/24` | A1 + D2 |
| F | F1 | `10.92.16.0/22` | E1 + C2 |
| F | F2 | `10.92.24.0/21` | C6 + E2 |
| G | G1 | `10.92.16.0/21` | F1 + A15 |
| H | H1 | `10.92.16.0/20` | G1 + F2 |
| I | I1 (ROOT) | `10.92.0.0/19` | D3 + H1 |

#### Visualisasi penggabungan pada topologi

![Topologi hasil penggabungan level B](Assets/GNS3/level-B.png)

![Topologi hasil penggabungan level C](Assets/GNS3/level-C.png)

![Topologi hasil penggabungan level D](Assets/GNS3/level-D.png)

![Topologi hasil penggabungan level E](Assets/GNS3/level-E.png)

![Topologi hasil penggabungan level F](Assets/GNS3/level-F.png)

![Topologi hasil penggabungan level G](Assets/GNS3/level-G.png)

![Topologi hasil penggabungan level H](Assets/GNS3/level-H.png)

![Topologi hasil penggabungan level I](Assets/GNS3/level-I.png)

![Pengelompokan subnet CIDR](Assets/GNS3/penggabungancidr1.png)

![Ringkasan CIDR tingkat atas](Assets/GNS3/penggabungancidr2.png)

Prefix pada gambar menunjukkan penggabungan bertingkat. Tabel berikut merangkum route yang direncanakan dari hasil penggabungan:

| Router | Tujuan | Route ringkasan | Next hop |
|---|---|---|---|
| Cyberia | Seluruh jaringan internal (I1) | `10.92.0.0/19` | Wired `10.92.28.146` |
| Wired | A9 dan A7-A8 (C4) | `10.92.0.0/21` | Schumann `10.92.28.132` |
| Wired | A4 (Psyche) | `10.92.8.0/22` | Psyche `10.92.28.130` |
| Wired | A5-A6 (B7) | `10.92.12.0/22` | Copland `10.92.28.131` |
| Wired | Cabang Tachibana (G1) | `10.92.16.0/21` | Tachibana `10.92.28.142` |
| Wired | A11-A13 (C6) | `10.92.24.0/22` | Navi `10.92.28.138` |
| Schumann | A9 (Aceta) | `10.92.0.0/22` | Aceta `10.92.4.10` |
| Tachibana | Cabang Deus ke bawah (F1) | `10.92.16.0/22` | Deus `10.92.18.130` |
| Deus | Cabang Protocol7 ke bawah (E1) | `10.92.16.0/23` | Protocol7 `10.92.18.134` |
| Protocol7 | Cabang Knights ke bawah (D1) | `10.92.16.0/24` | Knights `10.92.17.66` |
| Knights | Cabang Phantoma ke bawah (C1) | `10.92.16.0/25` | Phantoma `10.92.16.138` |
| Phantoma | A25 (Shibuya) | `10.92.16.0/26` | Shibuya `10.92.16.98` |

Total hanya 12 route statis untuk seluruh jaringan. Prefix ringkasan pada rantai Tachibana bersarang (`/21` → `/22` → `/23` → `/24` → `/25` → `/26`), sehingga tiap router pada rantai tersebut cukup menyimpan satu route menuju seluruh cabang di bawahnya.

Router cabang menggunakan default route ke router induknya, sedangkan Wired menyimpan route menuju cabang. Cyberia menjadi jalur keluar ke NAT; NAT masquerade dan IP forwarding dikonfigurasi di router tersebut.

![Pemetaan subnet dan route](Assets/GNS3/subnetroute.png)

### 1.4 Implementasi Konfigurasi

Script router mengaktifkan IP forwarding, memasang alamat pada interface, menambahkan route, dan mengatur NAT pada Cyberia. Script client memasang alamat statis dan default gateway sesuai subnetnya.

1. Pada node, edit file konfigurasi dengan `nano /root/script.sh` dan masukkan script yang sesuai.
2. Beri izin eksekusi dengan `chmod +x /root/script.sh`.
3. Jalankan konfigurasi dengan `bash /root/script.sh`.
4. Konfigurasi autostart dijelaskan di [`GNS3 - CIDR/autostart.md`](<GNS3 - CIDR/autostart.md>).

Script dan alamat lengkap per node tersedia pada [konfigurasi router](<GNS3 - CIDR/script.sh-router.md>) dan [konfigurasi client](<GNS3 - CIDR/clientsh.md>). Sebelum menjalankan script kembali, periksa alamat dan route yang sudah terpasang agar tidak terjadi penambahan duplikat.

### 1.5 Pengujian

Pengujian menggunakan `ip -br addr`, `ip route`, dan `ping -c 4 <alamat-tujuan>`. Screenshot terbaru menunjukkan empat ping berhasil tanpa packet loss:

| Sumber | Tujuan | Jalur yang diuji | Hasil |
|---|---|---|---|
| JJ | MyuMyu (`10.92.28.3`) | Satu LAN (A1) | 4 paket diterima, 0% packet loss |
| Wired | Cyberia (`10.92.28.145`) | Link antar-router (A2) | 4 paket diterima, 0% packet loss |
| Schumann | Aceta (`10.92.4.10`) | Link antar-router (A8) | 4 paket diterima, 0% packet loss |
| Karl | JJ (`10.92.28.2`) | Psyche → Wired → Cyberia | 4 paket diterima, 0% packet loss |
| Karl | Mika (`10.92.16.3`) | Psyche → Wired → Tachibana → Deus → Protocol7 → Knights → Phantoma → Shibuya | 4 paket diterima, 0% packet loss |

Pengujian Karl ke Mika melewati delapan router dan seluruh rantai prefix ringkasan dari `/21` hingga `/26`, sehingga sekaligus memverifikasi bahwa summarization pada setiap tingkat bekerja.

![Ping JJ ke MyuMyu](Assets/GNS3/jj-myumyu.png)

![Ping Karl ke JJ](Assets/GNS3/karl-jj.png)

![Ping Schumann ke Aceta](Assets/GNS3/schumann-aceta.png)

![Ping Wired ke Cyberia](Assets/GNS3/wired-cyberia.png)

Screenshot pemeriksaan route Wired memperlihatkan alamat interface dan route ringkasan, sedangkan pemeriksaan Shibuya memperlihatkan alamat interface serta default route:

![Alamat dan route Wired](Assets/GNS3/wired.png)

![Alamat dan route Shibuya](Assets/GNS3/shibuya.png)

Selain ping, efisiensi routing diverifikasi dengan `ip route get` untuk memastikan paket benar-benar melewati prefix ringkasan dan bukan route per-subnet. Dijalankan pada Wired:

```bash
ip route get 10.92.16.3     # tujuan di cabang Shibuya
ip route get 10.92.20.5     # tujuan di LAN Tachibana
```

Kedua tujuan keluar melalui next hop yang sama, yaitu Tachibana `10.92.28.142`, menggunakan satu route `10.92.16.0/21`. Hal yang sama berlaku untuk cabang Schumann:

```bash
ip route get 10.92.0.5      # LAN Aceta
ip route get 10.92.4.2      # LAN Schumann
```

Keduanya keluar melalui Schumann `10.92.28.132` dengan route `10.92.0.0/21`. Ini membuktikan Wired tidak menyimpan route terpisah untuk setiap LAN di cabang tersebut.

Bukti ini mengonfirmasi konektivitas pada LAN yang sama, antar-cabang, link antar-router, serta konfigurasi pada Wired dan Shibuya. Langkah pengujian lengkap beserta checklist tersedia di [`GNS3 - CIDR/cara test.md`](<GNS3 - CIDR/cara test.md>).

### 1.6 Kesimpulan

Perancangan GNS3 membagi prefix `10.92.0.0/19` menjadi 25 subnet dengan ukuran sesuai jumlah host dan kebutuhan link antar-router. Alokasi disusun mengikuti hierarki router sehingga setiap cabang menempati satu blok utuh dan dapat diringkas menjadi satu prefix. Hasilnya, seluruh jaringan hanya membutuhkan 12 route statis: Wired menyimpan lima route menuju cabang, rantai Tachibana sampai Phantoma masing-masing cukup satu route, dan router daun hanya menggunakan default route. Cyberia menyediakan koneksi keluar melalui NAT. Seluruh node telah dikonfigurasi dan diuji saling terhubung.

## 2. VLSM