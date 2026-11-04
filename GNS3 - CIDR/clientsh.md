# Konfigurasi Client Script GNS3 - CIDR

Berikut daftar konfigurasi IP untuk setiap node client yang digunakan pada topologi GNS3.

## 1. Subnet A1 (Switch 1)
Gateway: `10.92.0.1`

### Node: JJ
```bash
#!/bin/bash
ip addr add 10.92.0.2/25 dev eth0
ip route add default via 10.92.0.1
```

### Node: MyuMyu
```bash
#!/bin/bash
ip addr add 10.92.0.3/25 dev eth0
ip route add default via 10.92.0.1
```

## 2. Subnet A4 (Switch 3)
Gateway: `10.92.4.1`

### Node: Kari
```bash
#!/bin/bash
ip addr add 10.92.4.2/22 dev eth0
ip route add default via 10.92.4.1
```

### Node: Lin
```bash
#!/bin/bash
ip addr add 10.92.4.3/22 dev eth0
ip route add default via 10.92.4.1
```

## 3. Subnet A5 & A6 (Copland)
Gateway: `10.92.16.1` dan `10.92.18.1`

### Node: Juri (Subnet A5)
```bash
#!/bin/bash
ip addr add 10.92.16.2/26 dev eth0
ip route add default via 10.92.16.1
```

### Node: Masayuki (Subnet A6)
```bash
#!/bin/bash
ip addr add 10.92.18.2/23 dev eth0
ip route add default via 10.92.18.1
```

## 4. Subnet A7 (Schumann)
Gateway: `10.92.20.1`

### Node: Akashic (Server)
```bash
#!/bin/bash
ip addr add 10.92.20.2/29 dev eth0
ip route add default via 10.92.20.1
```

## 5. Subnet A9 (Switch 5 / Aceta)
Gateway: `10.92.32.1`

### Node: Omnipresence
```bash
#!/bin/bash
ip addr add 10.92.32.2/22 dev eth0
ip route add default via 10.92.32.1
```

### Node: Denpa
```bash
#!/bin/bash
ip addr add 10.92.32.3/22 dev eth0
ip route add default via 10.92.32.1
```

## 6. Subnet A11, A12, A13 (Navi)
Gateway: `10.92.48.1`, `10.92.49.1`, `10.92.50.1`

### Node: Alice (Subnet A11)
```bash
#!/bin/bash
ip addr add 10.92.48.2/24 dev eth0
ip route add default via 10.92.48.1
```

### Node: Reika (Subnet A11)
```bash
#!/bin/bash
ip addr add 10.92.48.3/24 dev eth0
ip route add default via 10.92.48.1
```

### Node: Taro (Subnet A12)
```bash
#!/bin/bash
ip addr add 10.92.49.2/26 dev eth0
ip route add default via 10.92.49.1
```

### Node: Yasuo (Subnet A13)
```bash
#!/bin/bash
ip addr add 10.92.50.2/24 dev eth0
ip route add default via 10.92.50.1
```

### Node: Miho (Subnet A13)
```bash
#!/bin/bash
ip addr add 10.92.50.3/24 dev eth0
ip route add default via 10.92.50.1
```

### Node: Kuma (Subnet A13)
```bash
#!/bin/bash
ip addr add 10.92.50.4/24 dev eth0
ip route add default via 10.92.50.1
```

## 7. Subnet A15 (Switch 9 / Tachibana)
Gateway: `10.92.52.1`

### Node: Collective
```bash
#!/bin/bash
ip addr add 10.92.52.2/23 dev eth0
ip route add default via 10.92.52.1
```

### Node: Ghost
```bash
#!/bin/bash
ip addr add 10.92.52.3/23 dev eth0
ip route add default via 10.92.52.1
```

## 8. Subnet A17 (Switch 10 / Deus)
Gateway: `10.92.64.1`

### Node: Lilith
```bash
#!/bin/bash
ip addr add 10.92.64.2/25 dev eth0
ip route add default via 10.92.64.1
```

### Node: Specter
```bash
#!/bin/bash
ip addr add 10.92.64.3/25 dev eth0
ip route add default via 10.92.64.1
```

## 9. Subnet A19 (Switch 11 / Protocol7)
Gateway: `10.92.65.1`

### Node: Chisa
```bash
#!/bin/bash
ip addr add 10.92.65.2/26 dev eth0
ip route add default via 10.92.65.1
```

### Node: Hodgeson
```bash
#!/bin/bash
ip addr add 10.92.65.3/26 dev eth0
ip route add default via 10.92.65.1
```

## 10. Subnet A21 (Knights)
Gateway: `10.92.66.1`

### Node: Eri
```bash
#!/bin/bash
ip addr add 10.92.66.2/30 dev eth0
ip route add default via 10.92.66.1
```

## 11. Subnet A23 (Switch 13 / Phantoma)
Gateway: `10.92.66.17`

### Node: Semi
```bash
#!/bin/bash
ip addr add 10.92.66.18/28 dev eth0
ip route add default via 10.92.66.17
```

## 12. Subnet A25 (Switch 12 / Shibuya)
Gateway: `10.92.68.1`

### Node: Lain
```bash
#!/bin/bash
ip addr add 10.92.68.2/26 dev eth0
ip route add default via 10.92.68.1
```

### Node: Mika
```bash
#!/bin/bash
ip addr add 10.92.68.3/26 dev eth0
ip route add default via 10.92.68.1
```

## Petunjuk eksekusi di GNS3
Simpan setiap skrip sebagai berkas `/root/script.sh` pada masing-masing node Debian, lalu jalankan perintah berikut:

```bash
chmod +x /root/script.sh
```
```bash
bash /root/script.sh
```