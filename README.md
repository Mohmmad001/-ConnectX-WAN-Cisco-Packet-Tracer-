# ConnectX WAN (Cisco Packet Tracer)

A simulated enterprise WAN connecting a headquarters in Amman, Jordan to
branch offices in Irbid, Zarqa, Aqaba, Beirut and Cairo. Branches reach
shared services at HQ and each other through OSPF routing.

## Requirements the design meets

- Secure internal website access (`https://eis.connectx.com`)
- File sharing between branches
- Internal email and Wi-Fi for portable devices
- Static IPs for servers, printers and routers; DHCP for PCs and laptops
- Room for growth (up to 30 users per site) and future branches

## Topology

- **WAN:** routers in a ring, with point-to-point links from `100.0.0.0/8`
- **LAN:** star topology at every site (switch, wired PCs, printer, access point)
- **HQ:** five servers, wired only (no Wi-Fi in the server segment)
- **Loopbacks:** `50.0.0.0/8`. The HQ router's loopback is its OSPF router ID.

![Topology](docs/topology.png)

## Addressing plan

The `192.168.1.0/24` block is split into `/27` subnets (30 usable hosts each):

| Site | Network | Usable range |
|------|---------|--------------|
| Amman HQ | 192.168.1.0/27 | .1 – .30 |
| Irbid | 192.168.1.32/27 | .33 – .62 |
| Zarqa | 192.168.1.64/27 | .65 – .94 |
| Aqaba | 192.168.1.96/27 | .97 – .126 |
| Beirut | 192.168.1.128/27 | .129 – .158 |
| Cairo | 192.168.1.160/27 | .161 – .190 |
| Amman Office | 192.168.1.192/27 | .193 – .222 |

`192.168.1.224/27` is left unused for future branches.

**Static:** servers (`192.168.1.2 – .6`), router interfaces, printers.
**Dynamic (DHCP):** PCs and laptops.

## Routing

- **OSPF, single area (Area 0)** on all routers
- HQ router advertises a default route (`default-information originate`)
- OSPF was chosen over RIP for faster convergence, cost-based metrics and
  better scalability

## Servers (at HQ)

| Service | Purpose |
|---------|---------|
| DHCP | Automatic addressing for all sites |
| DNS | Resolves `eis.connectx.com` and internal names |
| Web (HTTP/HTTPS) | Internal information system |
| Email | Internal mail |
| FTP | File transfer between branches |

## Security

- Password-protected router access
- Segmented subnets per site
- No Wi-Fi in the server segment

## Testing

Connectivity (`ping`, `traceroute`), name resolution, DHCP leasing, and
service access from each branch were checked in Packet Tracer.

## Open it

1. Install **Cisco Packet Tracer 8.2** or later.
2. Open `ConnectX.pkt`.

## Possible improvements

- Redundant WAN links and a second HQ router
- Access lists and SSH instead of console/Telnet access
- OSPF authentication
- Multiple OSPF areas as the network grows
