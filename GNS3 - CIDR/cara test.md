# GNS3 Router and Client Testing

## 1. Check automatic configuration

On each router and client:

```bash
ip -br addr
ip route
```

Confirm the expected interface addresses and routes.

**Cyberia:** Do not configure this route:

```bash
ip route add 10.92.0.0/17 via 10.92.0.130
```

It incorrectly sends the entire parent network back to Wired.

## 2. Test router-to-router connections

Run each ping from the router in the first column.

| Router | Neighbor IP | Command |
|---|---|---|
| Cyberia | 10.92.0.130 | `ping -c 4 10.92.0.130` |
| Wired | 10.92.0.129 | `ping -c 4 10.92.0.129` |
| Psyche | 10.92.8.1 | `ping -c 4 10.92.8.1` |
| Copland | 10.92.8.1 | `ping -c 4 10.92.8.1` |
| Schumann | 10.92.8.1 | `ping -c 4 10.92.8.1` |
| Aceta | 10.92.20.9 | `ping -c 4 10.92.20.9` |
| Navi | 10.92.36.1 | `ping -c 4 10.92.36.1` |
| Tachibana | 10.92.49.1 | `ping -c 4 10.92.49.1` |
| Deus | 10.92.58.1 | `ping -c 4 10.92.58.1` |
| Protocol7 | 10.92.64.129 | `ping -c 4 10.92.64.129` |
| Knights | 10.92.66.65 | `ping -c 4 10.92.66.65` |
| Phantoma | 10.92.72.9 | `ping -c 4 10.92.72.9` |
| Shibuya | 10.92.72.97 | `ping -c 4 10.92.72.97` |

Expected: replies received, ideally `0% packet loss`.

## 3. Check client IP addresses and gateways

Run on each client:

```bash
ip -br addr
ip route
```

| Client | Expected IP/prefix | Gateway |
|---|---|---|
| JJ | 10.92.0.2/25 | 10.92.0.1 |
| MyuMyu | 10.92.0.3/25 | 10.92.0.1 |
| Kari | 10.92.12.2/22 | 10.92.12.1 |
| Lin | 10.92.12.3/22 | 10.92.12.1 |
| Juri | 10.92.16.2/26 | 10.92.16.1 |
| Masayuki | 10.92.18.2/23 | 10.92.18.1 |
| Akashic | 10.92.20.2/29 | 10.92.20.1 |
| Omnipresence | 10.92.32.2/22 | 10.92.32.1 |
| Denpa | 10.92.32.3/22 | 10.92.32.1 |
| Alice | 10.92.40.2/24 | 10.92.40.1 |
| Reika | 10.92.40.3/24 | 10.92.40.1 |
| Taro | 10.92.41.2/26 | 10.92.41.1 |
| Yasuo | 10.92.48.2/24 | 10.92.48.1 |
| Miho | 10.92.48.3/24 | 10.92.48.1 |
| Kuma | 10.92.48.4/24 | 10.92.48.1 |
| Collective | 10.92.56.2/23 | 10.92.56.1 |
| Ghost | 10.92.56.3/23 | 10.92.56.1 |
| Lilith | 10.92.64.2/25 | 10.92.64.1 |
| Specter | 10.92.64.3/25 | 10.92.64.1 |
| Chisa | 10.92.66.2/26 | 10.92.66.1 |
| Hodgeson | 10.92.66.3/26 | 10.92.66.1 |
| Eri | 10.92.72.2/29 | 10.92.72.1 |
| Semi | 10.92.72.66/27 | 10.92.72.65 |
| Lain | 10.92.80.2/26 | 10.92.80.1 |
| Mika | 10.92.80.3/26 | 10.92.80.1 |

Ping each client's gateway. Example on JJ:

```bash
ping -c 4 10.92.0.1
```

## 4. Test client connectivity

**Same LAN — JJ to MyuMyu:**

```bash
ping -c 4 10.92.0.3
```

**Different LAN — JJ to Kari:**

```bash
ping -c 4 10.92.12.2
```

**Reverse — Kari to JJ:**

```bash
ping -c 4 10.92.0.2
```

**End-to-end — JJ to Lain:**

```bash
ping -c 4 10.92.80.2
```

**Reverse — Lain to JJ:**

```bash
ping -c 4 10.92.0.2
```

Expected: all tests receive replies.

## 5. Troubleshooting

```bash
ip -br addr
ip link
ip route
ip route get <destination-IP>
ip neigh
ping -c 4 <destination-IP>
```

- Missing IP: check script path, interface name, and startup hook.
- Gateway unreachable: check client IP/prefix, gateway IP, and GNS3 link.
- Remote LAN unreachable: check routes and next hops on intermediate routers.
- One-way ping: check return routes.
- Duplicate route/address errors: check whether the startup script ran more than once.

## 6. Test checklist

- [ ] All routers start with correct IP addresses.
- [ ] All router-to-router links pass ping tests.
- [ ] All clients have correct IP addresses and default gateways.
- [ ] All clients can ping their gateways.
- [ ] Same-LAN client ping succeeds.
- [ ] JJ can ping Kari.
- [ ] Kari can ping JJ.
- [ ] JJ can ping Lain.
- [ ] Lain can ping JJ.
