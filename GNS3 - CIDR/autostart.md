# Konfigurasi Autostart GNS3 - CIDR

Tambahkan konfigurasi berikut pada setiap node di `/etc/network/interfaces`.

## Router

### Cyberia

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Wired

```ini
auto eth0 eth1 eth2 eth3

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
iface eth3 inet manual
    up /bin/bash /root/script.sh
```

### Psyche

```ini
auto eth0 eth1

iface eth0 inet manual
iface eth1 inet manual
    up /bin/bash /root/script.sh
```

### Copland

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Schumann

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Aceta

```ini
auto eth0 eth1

iface eth0 inet manual
iface eth1 inet manual
    up /bin/bash /root/script.sh
```

### Navi

```ini
auto eth0 eth1 eth2 eth3

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
iface eth3 inet manual
    up /bin/bash /root/script.sh
```

### Tachibana

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Deus

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Protocol7

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Knights

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Phantoma

```ini
auto eth0 eth1 eth2

iface eth0 inet manual
iface eth1 inet manual
iface eth2 inet manual
    up /bin/bash /root/script.sh
```

### Shibuya

```ini
auto eth0 eth1

iface eth0 inet manual
iface eth1 inet manual
    up /bin/bash /root/script.sh
```

## Client

Semua client menggunakan `eth0`. Gunakan konfigurasi berikut pada setiap client:

```ini
auto eth0

iface eth0 inet manual
    up /bin/bash /root/script.sh
```
