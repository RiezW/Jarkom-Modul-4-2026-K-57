## 1. Router Cyberia

```bash
#!/bin/bash

# Enable IP forwarding
sysctl -w net.ipv4.ip_forward=1
k
# Interface IP configuration
# eth0 terhubung ke NAT1 (DHCP / automatic dari NAT)
dhclient eth0 2>/dev/null || true

# eth1 -> Switch 1 (A1: 10.92.0.0/25)
ip addr add 10.92.0.1/25 dev eth1

# eth2 -> Wired (A2: 10.92.0.128/30)
ip addr add 10.92.0.129/30 dev eth2

# NAT masquerade ke internet
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Summary route CIDR menuju jaringan internal di bawah Wired (Block E2/E1)
ip route add 10.92.0.0/17 via 10.92.0.130 dev eth2 2>/dev/null || true
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

## 2. Router Wired

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Cyberia (A2: 10.92.0.128/30)
ip addr add 10.92.0.130/30 dev eth0

# eth1 -> Switch 2 (A3: 10.92.0.136/29)
ip addr add 10.92.0.137/29 dev eth1

# eth2 -> Navi (A10: 10.92.36.0/30)
ip addr add 10.92.36.1/30 dev eth2

# eth3 -> Tachibana (A14: 10.92.51.0/30)
ip addr add 10.92.51.1/30 dev eth3

# Default route ke Cyberia
ip route add default via 10.92.0.129

# Static route ke cabang kanan (Navi) - summary CIDR /22
ip route add 10.92.48.0/22 via 10.92.36.2

# Static route ke cabang bawah (Tachibana dan seterusnya) - summary CIDR /19
ip route add 10.92.52.0/19 via 10.92.51.2

# Static route ke cabang kiri di bawah Switch 2
# (Psyche, Copland, Schumann, Aceta)
ip route add 10.92.4.0/22 via 10.92.0.138
ip route add 10.92.16.0/20 via 10.92.0.139
ip route add 10.92.32.0/19 via 10.92.0.140
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

## 3. Router Cabang Kiri (Switch 2)

### 3.1. Router Psyche

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.0.138/29 dev eth0

# eth1 -> Switch 3 (A4: Kari & Lin)
ip addr add 10.92.4.1/22 dev eth1

# Default route ke Wired
ip route add default via 10.92.0.137
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 3.2. Router Copland

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.0.139/29 dev eth0

# eth1 -> Juri (A5)
ip addr add 10.92.16.1/26 dev eth1

# eth2 -> Switch 4 / Masayuki (A6)
ip addr add 10.92.18.1/23 dev eth2

# Default route ke Wired
ip route add default via 10.92.0.137
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 3.3. Router Schumann

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Switch 2 (A3)
ip addr add 10.92.0.140/29 dev eth0

# eth1 -> Akashic (A7)
ip addr add 10.92.20.1/29 dev eth1

# eth2 -> Aceta (A8)
ip addr add 10.92.20.9/30 dev eth2

# Default route ke Wired
ip route add default via 10.92.0.137

# Static route ke Aceta (A9)
ip route add 10.92.32.0/22 via 10.92.20.10
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 3.4. Router Aceta

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Schumann (A8)
ip addr add 10.92.20.10/30 dev eth0

# eth1 -> Switch 5 / Omnipresence, Denpa (A9)
ip addr add 10.92.32.1/22 dev eth1

# Default route ke Schumann
ip route add default via 10.92.20.9
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

## 4. Router Cabang Kanan (Navi)

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Wired (A10)
ip addr add 10.92.36.2/30 dev eth0

# eth1 -> Switch 6 / Alice, Reika (A11)
ip addr add 10.92.48.1/24 dev eth1

# eth2 -> Switch 7 / Taro (A12)
ip addr add 10.92.49.1/26 dev eth2

# eth3 -> Switch 8 / Yasuo, Miho, Kuma (A13)
ip addr add 10.92.50.1/24 dev eth3

# Default route ke Wired
ip route add default via 10.92.36.1
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

## 5. Router Cabang Bawah (Tachibana–Shibuya)

### 5.1. Router Tachibana

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Wired (A14)
ip addr add 10.92.51.2/30 dev eth0

# eth1 -> Switch 9 / Collective, Ghost (A15)
ip addr add 10.92.52.1/23 dev eth1

# eth2 -> Deus (A16)
ip addr add 10.92.54.1/30 dev eth2

# Default route ke Wired
ip route add default via 10.92.51.1

# Static route ke jalur Deus dan seterusnya (summary CIDR /20)
ip route add 10.92.64.0/20 via 10.92.54.2
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 5.2. Router Deus

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Tachibana (A16)
ip addr add 10.92.54.2/30 dev eth0

# eth1 -> Switch 10 / Lilith, Specter (A17)
ip addr add 10.92.64.1/25 dev eth1

# eth2 -> Protocol7 (A18)
ip addr add 10.92.64.129/30 dev eth2

# Default route ke Tachibana
ip route add default via 10.92.54.1

# Static route ke jalur Protocol7 dan seterusnya
ip route add 10.92.65.0/24 via 10.92.64.130
ip route add 10.92.66.0/23 via 10.92.64.130
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 5.3. Router Protocol7

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Deus (A18)
ip addr add 10.92.64.130/30 dev eth0

# eth1 -> Switch 11 / Chisa, Hodgeson (A19)
ip addr add 10.92.65.1/26 dev eth1

# eth2 -> Knights (A20)
ip addr add 10.92.65.65/30 dev eth2

# Default route ke Deus
ip route add default via 10.92.64.129

# Static route ke Knights dan seterusnya
ip route add 10.92.66.0/23 via 10.92.65.66
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 5.4. Router Knights

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Protocol7 (A20)
ip addr add 10.92.65.66/30 dev eth0

# eth1 -> Eri (A21)
ip addr add 10.92.66.1/30 dev eth1

# eth2 -> Phantoma (A22)
ip addr add 10.92.66.5/30 dev eth2

# Default route ke Protocol7
ip route add default via 10.92.65.65

# Static route ke Phantoma dan seterusnya
ip route add 10.92.66.16/28 via 10.92.66.6
ip route add 10.92.66.32/27 via 10.92.66.6
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 5.5. Router Phantoma

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Knights (A22)
ip addr add 10.92.66.6/30 dev eth0

# eth1 -> Shibuya (A24)
ip addr add 10.92.66.33/30 dev eth1

# eth2 -> Switch 13 / Semi (A23)
ip addr add 10.92.66.17/28 dev eth2

# Default route ke Knights
ip route add default via 10.92.66.5

# Static route ke Shibuya
ip route add 10.92.68.0/26 via 10.92.66.34
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```

### 5.6. Router Shibuya

```bash
#!/bin/bash

sysctl -w net.ipv4.ip_forward=1

# eth0 -> Phantoma (A24)
ip addr add 10.92.66.34/30 dev eth0

# eth1 -> Switch 12 / Lain, Mika (A25)
ip addr add 10.92.68.1/26 dev eth1

# Default route ke Phantoma
ip route add default via 10.92.66.33
```

Eksekusi pada router ini:
```bash
chmod +x /root/script.sh
bash /root/script.sh
```
