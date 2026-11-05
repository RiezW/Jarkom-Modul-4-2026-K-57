## 1. Router Cyberia

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

# Enable IP forwarding
sysctl -w net.ipv4.ip_forward=1

# eth0 -> NAT1
dhclient eth0 2>/dev/null || true

# eth1 -> Switch 1 (A1)
ip addr add 10.92.0.1/25 dev eth1

# eth2 -> Wired (A2)
ip addr add 10.92.0.129/30 dev eth2

# NAT masquerade
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Route to internal network
ip route add 10.92.0.0/17 via 10.92.0.130 dev eth2
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null
iptables -t nat -F POSTROUTING


# eth0 -> NAT1
dhclient eth0 2>/dev/null || true

# eth1 -> Switch 1 (A1)
ip addr replace 10.92.28.1/25 dev eth1

# eth2 -> Wired (A2)
ip addr replace 10.92.28.145/30 dev eth2

# NAT masquerade
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Seluruh jaringan internal (I1)
ip route replace 10.92.0.0/19 via 10.92.28.146 dev eth2
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

## 2. Router Wired

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Cyberia (A2)
ip addr add 10.92.0.130/30 dev eth0

# eth1 -> Switch 2 (A3)
ip addr add 10.92.8.1/29 dev eth1

# eth2 -> Navi (A10)
ip addr add 10.92.36.1/30 dev eth2

# eth3 -> Tachibana (A14)
ip addr add 10.92.49.1/30 dev eth3

# Default route to Cyberia
ip route add default via 10.92.0.129

# Psyche (A4)
ip route add 10.92.12.0/22 via 10.92.8.2

# Copland (A5 + A6)
ip route add 10.92.16.0/21 via 10.92.8.3

# Schumann (A7 + A8)
ip route add 10.92.20.0/28 via 10.92.8.4

# Aceta (A9)
ip route add 10.92.32.0/22 via 10.92.8.4

# Navi LANs (A11 + A12)
ip route add 10.92.40.0/22 via 10.92.36.2

# Navi LAN (A13)
ip route add 10.92.48.0/24 via 10.92.36.2

# Tachibana LAN (A15)
ip route add 10.92.56.0/23 via 10.92.49.2

# Deus and downstream networks
ip route add 10.92.64.0/19 via 10.92.49.2
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Cyberia (A2)
ip addr replace 10.92.28.146/30 dev eth0

# eth1 -> Switch 2 (A3)
ip addr replace 10.92.28.129/29 dev eth1

# eth2 -> Navi (A10)
ip addr replace 10.92.28.137/30 dev eth2

# eth3 -> Tachibana (A14)
ip addr replace 10.92.28.141/30 dev eth3

# Default route ke Cyberia
ip route replace default via 10.92.28.145

# Schumann + Aceta (C4)
ip route replace 10.92.0.0/21 via 10.92.28.132

# Psyche (A4)
ip route replace 10.92.8.0/22 via 10.92.28.130

# Copland (B7)
ip route replace 10.92.12.0/22 via 10.92.28.131

# Tachibana dan seluruh cabang bawah (G1)
ip route replace 10.92.16.0/21 via 10.92.28.142

# Navi (C6)
ip route replace 10.92.24.0/22 via 10.92.28.138
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

## 3. Router Cabang Kiri (Switch 2)

### 3.1. Router Psyche

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.8.2/29 dev eth0

# eth1 -> Switch 3 (A4)
ip addr add 10.92.12.1/22 dev eth1

# Default route to Wired
ip route add default via 10.92.8.1
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Switch 2 (A3)
ip addr replace 10.92.28.130/29 dev eth0

# eth1 -> Switch 3 (A4)
ip addr replace 10.92.8.1/22 dev eth1

# Default route ke Wired
ip route replace default via 10.92.28.129
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 3.2. Router Copland

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.8.3/29 dev eth0

# eth1 -> Juri (A5)
ip addr add 10.92.16.1/26 dev eth1

# eth2 -> Switch 4 (A6)
ip addr add 10.92.18.1/23 dev eth2

# Default route to Wired
ip route add default via 10.92.8.1
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Switch 2 (A3)
ip addr replace 10.92.28.131/29 dev eth0

# eth1 -> Juri (A5)
ip addr replace 10.92.14.1/26 dev eth1

# eth2 -> Switch 4 (A6)
ip addr replace 10.92.12.1/23 dev eth2

# Default route ke Wired
ip route replace default via 10.92.28.129
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 3.3. Router Schumann

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.8.4/29 dev eth0

# eth1 -> Akashic (A7)
ip addr add 10.92.20.1/29 dev eth1

# eth2 -> Aceta (A8)
ip addr add 10.92.20.9/30 dev eth2

# Default route to Wired
ip route add default via 10.92.8.1

# Route to Aceta LAN (A9)
ip route add 10.92.32.0/22 via 10.92.20.10
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Switch 2 (A3)
ip addr replace 10.92.28.132/29 dev eth0

# eth1 -> Akashic (A7)
ip addr replace 10.92.4.1/29 dev eth1

# eth2 -> Aceta (A8)
ip addr replace 10.92.4.9/30 dev eth2

# Default route ke Wired
ip route replace default via 10.92.28.129

# LAN Aceta (A9)
ip route replace 10.92.0.0/22 via 10.92.4.10
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 3.4. Router Aceta

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Schumann (A8)
ip addr add 10.92.20.10/30 dev eth0

# eth1 -> Switch 5 (A9)
ip addr add 10.92.32.1/22 dev eth1

# Default route to Schumann
ip route add default via 10.92.20.9
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Schumann (A8)
ip addr replace 10.92.4.10/30 dev eth0

# eth1 -> Switch 5 (A9)
ip addr replace 10.92.0.1/22 dev eth1

# Default route ke Schumann
ip route replace default via 10.92.4.9
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

## 4. Router Cabang Kanan (Navi)

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Wired (A10)
ip addr add 10.92.36.2/30 dev eth0

# eth1 -> Switch 6 (A11)
ip addr add 10.92.40.1/24 dev eth1

# eth2 -> Switch 7 (A12)
ip addr add 10.92.41.1/26 dev eth2

# eth3 -> Switch 8 (A13)
ip addr add 10.92.48.1/24 dev eth3

# Default route to Wired
ip route add default via 10.92.36.1
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Wired (A10)
ip addr replace 10.92.28.138/30 dev eth0

# eth1 -> Switch 6 (A11)
ip addr replace 10.92.24.1/24 dev eth1

# eth2 -> Switch 7 (A12)
ip addr replace 10.92.26.1/26 dev eth2

# eth3 -> Switch 8 (A13)
ip addr replace 10.92.25.1/24 dev eth3

# Default route ke Wired
ip route replace default via 10.92.28.137
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

## 5. Router Cabang Bawah (Tachibana–Shibuya)

### 5.1. Router Tachibana

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Wired (A14)
ip addr add 10.92.49.2/30 dev eth0

# eth1 -> Switch 9 (A15)
ip addr add 10.92.56.1/23 dev eth1

# eth2 -> Deus (A16)
ip addr add 10.92.58.1/30 dev eth2

# Default route to Wired
ip route add default via 10.92.49.1

# Route to Deus and downstream networks
ip route add 10.92.64.0/19 via 10.92.58.2
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Wired (A14)
ip addr replace 10.92.28.142/30 dev eth0

# eth1 -> Switch 9 (A15)
ip addr replace 10.92.20.1/23 dev eth1

# eth2 -> Deus (A16)
ip addr replace 10.92.18.129/30 dev eth2

# Default route ke Wired
ip route replace default via 10.92.28.141

# Deus dan seluruh cabang bawah (F1)
ip route replace 10.92.16.0/22 via 10.92.18.130
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 5.2. Router Deus

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Tachibana (A16)
ip addr add 10.92.58.2/30 dev eth0

# eth1 -> Switch 10 (A17)
ip addr add 10.92.64.1/25 dev eth1

# eth2 -> Protocol7 (A18)
ip addr add 10.92.64.129/30 dev eth2

# Default route to Tachibana
ip route add default via 10.92.58.1

# Protocol7 LAN and link (A19 + A20)
ip route add 10.92.66.0/23 via 10.92.64.130

# Knights and downstream networks
ip route add 10.92.72.0/21 via 10.92.64.130

# Shibuya LAN
ip route add 10.92.80.0/26 via 10.92.64.130
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Tachibana (A16)
ip addr replace 10.92.18.130/30 dev eth0

# eth1 -> Switch 10 (A17)
ip addr replace 10.92.18.1/25 dev eth1

# eth2 -> Protocol7 (A18)
ip addr replace 10.92.18.133/30 dev eth2

# Default route ke Tachibana
ip route replace default via 10.92.18.129

# Protocol7 dan seluruh cabang bawah (E1)
ip route replace 10.92.16.0/23 via 10.92.18.134
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 5.3. Router Protocol7

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Deus (A18)
ip addr add 10.92.64.130/30 dev eth0

# eth1 -> Switch 11 (A19)
ip addr add 10.92.66.1/26 dev eth1

# eth2 -> Knights (A20)
ip addr add 10.92.66.65/30 dev eth2

# Default route to Deus
ip route add default via 10.92.64.129

# Knights and downstream networks
ip route add 10.92.72.0/21 via 10.92.66.66

# Shibuya LAN
ip route add 10.92.80.0/26 via 10.92.66.66
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Deus (A18)
ip addr replace 10.92.18.134/30 dev eth0

# eth1 -> Switch 11 (A19)
ip addr replace 10.92.17.1/26 dev eth1

# eth2 -> Knights (A20)
ip addr replace 10.92.17.65/30 dev eth2

# Default route ke Deus
ip route replace default via 10.92.18.133

# Knights dan seluruh cabang bawah (D1)
ip route replace 10.92.16.0/24 via 10.92.17.66
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 5.4. Router Knights

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Protocol7 (A20)
ip addr add 10.92.66.66/30 dev eth0

# eth1 -> Eri (A21)
ip addr add 10.92.72.1/29 dev eth1

# eth2 -> Phantoma (A22)
ip addr add 10.92.72.9/30 dev eth2

# Default route to Protocol7
ip route add default via 10.92.66.65

# Phantoma LAN and Shibuya link (A23 + A24)
ip route add 10.92.72.64/26 via 10.92.72.10

# Shibuya LAN (A25)
ip route add 10.92.80.0/26 via 10.92.72.10
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Protocol7 (A20)
ip addr replace 10.92.17.66/30 dev eth0

# eth1 -> Eri (A21)
ip addr replace 10.92.16.129/29 dev eth1

# eth2 -> Phantoma (A22)
ip addr replace 10.92.16.137/30 dev eth2

# Default route ke Protocol7
ip route replace default via 10.92.17.65

# Phantoma dan seluruh cabang bawah (C1)
ip route replace 10.92.16.0/25 via 10.92.16.138
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

### 5.5. Router Phantoma

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Knights (A22)
ip addr add 10.92.72.10/30 dev eth0

# eth1 -> Switch 13 (A23)
ip addr add 10.92.72.65/27 dev eth1

# eth2 -> Shibuya (A24)
ip addr add 10.92.72.97/30 dev eth2

# Default route to Knights
ip route add default via 10.92.72.9

# Route to Shibuya LAN (A25)
ip route add 10.92.80.0/26 via 10.92.72.98
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Knights (A22)
ip addr replace 10.92.16.138/30 dev eth0

# eth1 -> Switch 13 (A23)
ip addr replace 10.92.16.65/27 dev eth1

# eth2 -> Shibuya (A24)
ip addr replace 10.92.16.97/30 dev eth2

# Default route ke Knights
ip route replace default via 10.92.16.137

# LAN Shibuya (A25)
ip route replace 10.92.16.0/26 via 10.92.16.98
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```

revisi ke 2
```bash
ip addr flush dev eth1
ip addr flush dev eth2

# eth1 -> Shibuya (A24)
ip addr replace 10.92.16.97/30 dev eth1

# eth2 -> Switch 13 (A23)
ip addr replace 10.92.16.65/27 dev eth2

ip route replace 10.92.16.0/26 via 10.92.16.98
```

### 5.6. Router Shibuya

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Phantoma (A24)
ip addr add 10.92.72.98/30 dev eth0

# eth1 -> Switch 12 (A25)
ip addr add 10.92.80.1/26 dev eth1

# Default route to Phantoma
ip route add default via 10.92.72.97
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```


> ### Revisi — skema IP final
>
> Jalankan blok di bawah ini **sebagai pengganti** script di atas.
> Baris `flush` wajib ada: alamat skema lama berbeda prefix dari yang baru,
> sehingga `ip addr replace` saja tidak akan menghapusnya.

```bash
nano /root/script.sh
```

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# Hapus alamat & rute skema lama
for i in $(ls /sys/class/net | grep -v lo); do ip addr flush dev $i; done
ip route flush table main 2>/dev/null


# eth0 -> Phantoma (A24)
ip addr replace 10.92.16.98/30 dev eth0

# eth1 -> Switch 12 (A25)
ip addr replace 10.92.16.1/26 dev eth1

# Default route ke Phantoma
ip route replace default via 10.92.16.97
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

Verifikasi:
```bash
ip -br addr
ip route
```