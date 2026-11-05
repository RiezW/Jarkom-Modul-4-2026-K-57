# GNS3 Router and Client Testing — Skema IP Final

Blok induk: `10.92.0.0/19` (10.92.0.0 – 10.92.31.255)

## 1. Check configuration

On each router and client:

```bash
ip -br addr
ip route
```

Confirm the expected interface addresses and routes.

**Penting — pastikan tidak ada sisa skema lama.** Prefix berikut sudah tidak dipakai dan tidak boleh muncul di output `ip route` atau `ip -br addr`:

```
10.92.0.0/17   10.92.32.x   10.92.36.x   10.92.40.x   10.92.41.x
10.92.48.x     10.92.49.x   10.92.56.x   10.92.58.x   10.92.64.x
10.92.66.x     10.92.72.x   10.92.80.x
```

Kalau masih ada, jalankan:

```bash
ip addr flush dev <interface>
ip route flush table main
```

lalu jalankan ulang script-nya.

**Cyberia:** route berikut sekarang BENAR dan memang harus ada:

```bash
ip route replace 10.92.0.0/19 via 10.92.28.146 dev eth2
```

Prefix `/19` ini sudah tepat sebatas blok yang dialokasikan. Jangan pakai `/17` seperti skema lama — itu melebihi blok dan ikut menarik alamat di luar jaringan ini.

## 2. Test router-to-router connections

Run each ping from the router in the first column.

| Router | Link | Neighbor IP | Command |
|---|---|---|---|
| Cyberia | A2 → Wired | 10.92.28.146 | `ping -c 4 10.92.28.146` |
| Wired | A2 → Cyberia | 10.92.28.145 | `ping -c 4 10.92.28.145` |
| Wired | A3 → Psyche | 10.92.28.130 | `ping -c 4 10.92.28.130` |
| Wired | A3 → Copland | 10.92.28.131 | `ping -c 4 10.92.28.131` |
| Wired | A3 → Schumann | 10.92.28.132 | `ping -c 4 10.92.28.132` |
| Wired | A10 → Navi | 10.92.28.138 | `ping -c 4 10.92.28.138` |
| Wired | A14 → Tachibana | 10.92.28.142 | `ping -c 4 10.92.28.142` |
| Psyche | A3 → Wired | 10.92.28.129 | `ping -c 4 10.92.28.129` |
| Copland | A3 → Wired | 10.92.28.129 | `ping -c 4 10.92.28.129` |
| Schumann | A3 → Wired | 10.92.28.129 | `ping -c 4 10.92.28.129` |
| Schumann | A8 → Aceta | 10.92.4.10 | `ping -c 4 10.92.4.10` |
| Aceta | A8 → Schumann | 10.92.4.9 | `ping -c 4 10.92.4.9` |
| Navi | A10 → Wired | 10.92.28.137 | `ping -c 4 10.92.28.137` |
| Tachibana | A14 → Wired | 10.92.28.141 | `ping -c 4 10.92.28.141` |
| Tachibana | A16 → Deus | 10.92.18.130 | `ping -c 4 10.92.18.130` |
| Deus | A16 → Tachibana | 10.92.18.129 | `ping -c 4 10.92.18.129` |
| Deus | A18 → Protocol7 | 10.92.18.134 | `ping -c 4 10.92.18.134` |
| Protocol7 | A18 → Deus | 10.92.18.133 | `ping -c 4 10.92.18.133` |
| Protocol7 | A20 → Knights | 10.92.17.66 | `ping -c 4 10.92.17.66` |
| Knights | A20 → Protocol7 | 10.92.17.65 | `ping -c 4 10.92.17.65` |
| Knights | A22 → Phantoma | 10.92.16.138 | `ping -c 4 10.92.16.138` |
| Phantoma | A22 → Knights | 10.92.16.137 | `ping -c 4 10.92.16.137` |
| Phantoma | A24 → Shibuya | 10.92.16.98 | `ping -c 4 10.92.16.98` |
| Shibuya | A24 → Phantoma | 10.92.16.97 | `ping -c 4 10.92.16.97` |

Expected: replies received, ideally `0% packet loss`.

Catatan untuk A3: Psyche, Copland, dan Schumann berada di satu segmen bersama lewat Switch 2, jadi ketiganya juga harus bisa saling ping langsung:

```bash
# dari Psyche
ping -c 4 10.92.28.131
ping -c 4 10.92.28.132
```

## 3. Check router interface addresses

| Router | Interface | Subnet | Expected IP/prefix |
|---|---|---|---|
| Cyberia | eth0 | NAT | DHCP |
| Cyberia | eth1 | A1 | 10.92.28.1/25 |
| Cyberia | eth2 | A2 | 10.92.28.145/30 |
| Wired | eth0 | A2 | 10.92.28.146/30 |
| Wired | eth1 | A3 | 10.92.28.129/29 |
| Wired | eth2 | A10 | 10.92.28.137/30 |
| Wired | eth3 | A14 | 10.92.28.141/30 |
| Psyche | eth0 | A3 | 10.92.28.130/29 |
| Psyche | eth1 | A4 | 10.92.8.1/22 |
| Copland | eth0 | A3 | 10.92.28.131/29 |
| Copland | eth1 | A5 | 10.92.14.1/26 |
| Copland | eth2 | A6 | 10.92.12.1/23 |
| Schumann | eth0 | A3 | 10.92.28.132/29 |
| Schumann | eth1 | A7 | 10.92.4.1/29 |
| Schumann | eth2 | A8 | 10.92.4.9/30 |
| Aceta | eth0 | A8 | 10.92.4.10/30 |
| Aceta | eth1 | A9 | 10.92.0.1/22 |
| Navi | eth0 | A10 | 10.92.28.138/30 |
| Navi | eth1 | A11 | 10.92.24.1/24 |
| Navi | eth2 | A12 | 10.92.26.1/26 |
| Navi | eth3 | A13 | 10.92.25.1/24 |
| Tachibana | eth0 | A14 | 10.92.28.142/30 |
| Tachibana | eth1 | A15 | 10.92.20.1/23 |
| Tachibana | eth2 | A16 | 10.92.18.129/30 |
| Deus | eth0 | A16 | 10.92.18.130/30 |
| Deus | eth1 | A17 | 10.92.18.1/25 |
| Deus | eth2 | A18 | 10.92.18.133/30 |
| Protocol7 | eth0 | A18 | 10.92.18.134/30 |
| Protocol7 | eth1 | A19 | 10.92.17.1/26 |
| Protocol7 | eth2 | A20 | 10.92.17.65/30 |
| Knights | eth0 | A20 | 10.92.17.66/30 |
| Knights | eth1 | A21 | 10.92.16.129/29 |
| Knights | eth2 | A22 | 10.92.16.137/30 |
| Phantoma | eth0 | A22 | 10.92.16.138/30 |
| Phantoma | eth1 | A23 | 10.92.16.65/27 |
| Phantoma | eth2 | A24 | 10.92.16.97/30 |
| Shibuya | eth0 | A24 | 10.92.16.98/30 |
| Shibuya | eth1 | A25 | 10.92.16.1/26 |

## 4. Check routing tables

Jumlah rute statis yang benar per router:

| Router | Default via | Rute statis tambahan |
|---|---|---|
| Cyberia | NAT (DHCP) | `10.92.0.0/19 via 10.92.28.146` |
| Wired | 10.92.28.145 | `10.92.0.0/21 via 10.92.28.132`<br>`10.92.8.0/22 via 10.92.28.130`<br>`10.92.12.0/22 via 10.92.28.131`<br>`10.92.16.0/21 via 10.92.28.142`<br>`10.92.24.0/22 via 10.92.28.138` |
| Psyche | 10.92.28.129 | — |
| Copland | 10.92.28.129 | — |
| Schumann | 10.92.28.129 | `10.92.0.0/22 via 10.92.4.10` |
| Aceta | 10.92.4.9 | — |
| Navi | 10.92.28.137 | — |
| Tachibana | 10.92.28.141 | `10.92.16.0/22 via 10.92.18.130` |
| Deus | 10.92.18.129 | `10.92.16.0/23 via 10.92.18.134` |
| Protocol7 | 10.92.18.133 | `10.92.16.0/24 via 10.92.17.66` |
| Knights | 10.92.17.65 | `10.92.16.0/25 via 10.92.16.138` |
| Phantoma | 10.92.16.137 | `10.92.16.0/26 via 10.92.16.98` |
| Shibuya | 10.92.16.97 | — |

Kalau ada router yang punya rute lebih banyak dari daftar ini, kemungkinan ada sisa skema lama.

## 5. Check client IP addresses and gateways

Run on each client:

```bash
ip -br addr
ip route
```

| Client | Subnet | Expected IP/prefix | Gateway |
|---|---|---|---|
| JJ | A1 | 10.92.28.2/25 | 10.92.28.1 |
| MyuMyu | A1 | 10.92.28.3/25 | 10.92.28.1 |
| Karl | A4 | 10.92.8.2/22 | 10.92.8.1 |
| Lin | A4 | 10.92.8.3/22 | 10.92.8.1 |
| Juri | A5 | 10.92.14.2/26 | 10.92.14.1 |
| Masayuki | A6 | 10.92.12.2/23 | 10.92.12.1 |
| Akashic | A7 | 10.92.4.2/29 | 10.92.4.1 |
| Omnipresence | A9 | 10.92.0.2/22 | 10.92.0.1 |
| Denpa | A9 | 10.92.0.3/22 | 10.92.0.1 |
| Alice | A11 | 10.92.24.2/24 | 10.92.24.1 |
| Reika | A11 | 10.92.24.3/24 | 10.92.24.1 |
| Taro | A12 | 10.92.26.2/26 | 10.92.26.1 |
| Yasuo | A13 | 10.92.25.2/24 | 10.92.25.1 |
| Miho | A13 | 10.92.25.3/24 | 10.92.25.1 |
| Kuma | A13 | 10.92.25.4/24 | 10.92.25.1 |
| Collective | A15 | 10.92.20.2/23 | 10.92.20.1 |
| Ghost | A15 | 10.92.20.3/23 | 10.92.20.1 |
| Lilith | A17 | 10.92.18.2/25 | 10.92.18.1 |
| Specter | A17 | 10.92.18.3/25 | 10.92.18.1 |
| Chisa | A19 | 10.92.17.2/26 | 10.92.17.1 |
| Hodgeson | A19 | 10.92.17.3/26 | 10.92.17.1 |
| Eri | A21 | 10.92.16.130/29 | 10.92.16.129 |
| Semi | A23 | 10.92.16.66/27 | 10.92.16.65 |
| Lain | A25 | 10.92.16.2/26 | 10.92.16.1 |
| Mika | A25 | 10.92.16.3/26 | 10.92.16.1 |

Ping each client's gateway. Example on JJ:

```bash
ping -c 4 10.92.28.1
```

## 6. Test client connectivity

**Same LAN — JJ to MyuMyu:**

```bash
ping -c 4 10.92.28.3
```

**Different LAN, one hop — JJ to Karl:**

```bash
ping -c 4 10.92.8.2
```

**Reverse — Karl to JJ:**

```bash
ping -c 4 10.92.28.2
```

**Deepest branch — JJ to Lain:**

```bash
ping -c 4 10.92.16.2
```

**Reverse — Lain to JJ:**

```bash
ping -c 4 10.92.28.2
```

**Cabang terjauh ke cabang terjauh — Karl to Mika:**

```bash
ping -c 4 10.92.16.3
```

Jalur ini melewati Psyche → Wired → Tachibana → Deus → Protocol7 → Knights → Phantoma → Shibuya, jadi sekaligus menguji seluruh rantai summary.

**Reverse — Mika to Karl:**

```bash
ping -c 4 10.92.8.2
```

**Cabang samping — Omnipresence to Taro:**

```bash
ping -c 4 10.92.26.2
```

Menguji jalur Aceta → Schumann → Wired → Navi.

Expected: all tests receive replies.

## 7. Verify route aggregation

Pengujian ini membuktikan summary bekerja, bukan sekadar konektivitas. Jalankan dari client:

```bash
# dari Karl, lihat jalur yang dipilih menuju Mika
ip route get 10.92.16.3

# dari Wired, pastikan tujuan cabang bawah lewat satu prefix /21
ip route get 10.92.16.3
ip route get 10.92.20.5
```

Dari Wired, kedua tujuan di atas harus keluar lewat `10.92.28.142` (Tachibana) dengan rute `10.92.16.0/21` — bukan lewat rute per-subnet terpisah.

```bash
# dari Wired, cabang Schumann dan Aceta harus lewat satu prefix /21
ip route get 10.92.0.5
ip route get 10.92.4.2
```

Keduanya harus keluar lewat `10.92.28.132` (Schumann) dengan rute `10.92.0.0/21`.

## 8. Troubleshooting

```bash
ip -br addr
ip link
ip route
ip route get <destination-IP>
ip neigh
ping -c 4 <destination-IP>
```

- **Missing IP:** check script path, interface name, and startup hook.
- **Interface punya dua alamat:** script lama dan baru dijalankan tanpa flush. Jalankan `ip addr flush dev <interface>` lalu script baru.
- **Gateway unreachable:** check client IP/prefix, gateway IP, and GNS3 link.
- **Remote LAN unreachable:** check routes and next hops on intermediate routers.
- **One-way ping:** check return routes — biasanya rute balik di salah satu router di tengah jalur belum ada.
- **Duplicate route/address errors:** check whether the startup script ran more than once.
- **Ping ke cabang bawah gagal dari Wired, tapi ke Tachibana sukses:** periksa rute `10.92.16.0/22` di Tachibana.
- **Forwarding mati:** pastikan `sysctl net.ipv4.ip_forward` bernilai 1 di semua router.

## 9. Test checklist

- [ ] Tidak ada sisa alamat atau rute dari skema lama di node mana pun.
- [ ] All routers start with correct IP addresses.
- [ ] All router-to-router links pass ping tests.
- [ ] Psyche, Copland, dan Schumann saling ping di segmen A3.
- [ ] Jumlah rute statis tiap router sesuai tabel bagian 4.
- [ ] All clients have correct IP addresses and default gateways.
- [ ] All clients can ping their gateways.
- [ ] Same-LAN client ping succeeds (JJ ↔ MyuMyu).
- [ ] JJ can ping Karl, and Karl can ping JJ.
- [ ] JJ can ping Lain, and Lain can ping JJ.
- [ ] Karl can ping Mika, and Mika can ping Karl.
- [ ] Omnipresence can ping Taro.
- [ ] `ip route get` membuktikan summary dipakai, bukan rute per-subnet.