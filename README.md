<div align="center">

# 🌐 Networking Labs

**Hands-on Cisco networking notes: from basic router setup to OSPF, network services, and wireless.**

![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-1BA0D7?logo=cisco&logoColor=white)
![Labs](https://img.shields.io/badge/Labs-7-blue)
![Notes](https://img.shields.io/badge/Notes-Markdown-lightgrey?logo=markdown)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![License](https://img.shields.io/badge/License-MIT-yellow)

[Overview](#-overview) •
[Labs](#-labs) •
[Quick Reference](#-quick-reference) •
[Getting Started](#-getting-started) •
[Repository Structure](#-repository-structure) •
[Contributing](#-contributing)

</div>

---

## 📖 Overview

This repository contains revision notes and configuration walkthroughs for a series of seven networking labs, built and tested in **Cisco Packet Tracer**. Each lab explains the *why* behind the configuration as well as the commands, making it useful for coursework, certification prep (e.g. CCNA), and quick revision.

**Topics covered**

- Router CLI configuration and device hardening
- Serial links between routers
- Static routing and default routes
- Dynamic routing with RIP and OSPF
- Network services: HTTP/HTTPS, DNS, FTP, Email, DHCP
- Wireless access points and IoT devices

> **Golden rule:** every device on the same LAN must share the same subnet.

---

## 🧪 Labs

| # | Lab | Key Topics |
|:-:|-----|------------|
| 1 | Simple Router Configuration | Hostname, passwords, banners, interfaces, default gateway |
| 2 | Connecting 2 Routers | Serial links (NIM-2T), IP addressing, TTL behavior |
| 3 | Static Routing | Static routes, hop counting, loopback, default route |
| 4 | Dynamic Routing (RIP) | RIP v1 vs v2, passive interfaces, 15-hop limit |
| 5 | OSPF | Process ID, wildcard masks, area 0 |
| 6 | HTTPS, FTP, DNS, Email, DHCP | Server services, DHCP relay, APIPA |
| 7 | IoT Devices | Access point, WPA2-PSK, wireless clients |

### Router access methods

1. **CLI**: directly on the router.
2. **Console cable**: connect a PC to the router's console port (RS-232) for management access.

---

## ⚡ Quick Reference

<details>
<summary><b>Basic router setup</b></summary>

```text
enable
configure terminal
hostname <name>
enable secret <password>
service password-encryption
banner motd "<text>"
copy running-config startup-config    # or: wr
```
</details>

<details>
<summary><b>Console and remote access</b></summary>

```text
line console 0
 password <password>
 login

line vty 0 4
 password <password>
 login
```
</details>

<details>
<summary><b>Interface configuration</b></summary>

```text
interface <type> <number>
 ip address <ip> <subnet-mask>
 no shutdown
```
</details>

<details>
<summary><b>Static and default routes</b></summary>

```text
ip route <network-id> <subnet-mask> <next-hop | exit-interface>
ip route 0.0.0.0 0.0.0.0 <next-hop | exit-interface>
```
</details>

<details>
<summary><b>RIP v2</b></summary>

```text
router rip
 version 2
 no auto-summary
 network <directly-connected-network>
 passive-interface <interface>
 default-information originate         # on the router holding the default route
```
</details>

<details>
<summary><b>OSPF</b></summary>

```text
router ospf <process-id>
 network <network-id> <wildcard-mask> area 0
 passive-interface <interface>
```
</details>

<details>
<summary><b>DHCP relay</b></summary>

```text
interface <interface-facing-clients>
 ip helper-address <dhcp-server-ip>
```
</details>

<details>
<summary><b>Verification</b></summary>

```text
show ip route
show ip protocols
```

| Code | Meaning |
|:----:|---------|
| `C` | Connected |
| `S` / `S*` | Static / static default |
| `R` / `R*` | RIP / RIP default |
| `O` | OSPF |
</details>

### Ports at a glance

| Protocol | Port(s) | Purpose |
|----------|---------|---------|
| FTP | 20 (data), 21 (control) | File transfer |
| SMTP | 25 | Sending email |
| POP3 | 110 | Receiving email |
| DHCP | 67 (server), 68 (client), UDP | Automatic IP assignment |

### RIP vs OSPF

| Feature | RIP v1 | RIP v2 | OSPF |
|---------|--------|--------|------|
| Type | Distance-vector | Distance-vector | Link-state |
| Metric | Hop count | Hop count | Cost |
| Max hops | 15 | 15 | No hop limit |
| Updates | Broadcast | Multicast (224.0.0.9) | Event-driven |
| Subnet support | Classful | Classless | Classless |
| Authentication | No | Yes | Yes |
| Administrative distance | 120 | 120 | 110 |

---

## 🚀 Getting Started

### Prerequisites

- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) (free with a Cisco Networking Academy account)
- A Markdown viewer. The notes were written in Obsidian, but GitHub renders them fine.

### Usage

```bash
# Clone the repository
git clone https://github.com/<your-username>/networking-labs.git
cd networking-labs
```

1. Open [`Networking_Labs.md`](./Networking_Labs.md) and follow the labs in order. Later labs build on earlier ones (for example, DHCP relay requires working OSPF).
2. Recreate each topology in Packet Tracer.
3. Apply the commands and verify with `show ip route`, `ping`, and `tracert`.

---

## 📁 Repository Structure

```text
networking-labs/
├── README.md              # Project overview and quick reference
├── Networking_Labs.md     # Full lab notes
├── images/                # Topology diagrams and screenshots
├── LICENSE
└── .gitignore
```

---

## 🤝 Contributing

Corrections and improvements are welcome.

1. Fork the repository
2. Create a branch: `git checkout -b fix/describe-your-change`
3. Commit your changes: `git commit -m "Describe your change"`
4. Push the branch and open a Pull Request

---

## 📄 License

Distributed under the MIT License. See [`LICENSE`](./LICENSE) for details.

---

## 👤 Author

**Your Name**
[GitHub](https://github.com/ibrahimkobe) • [LinkedIn](https://www.linkedin.com/in/ibrahim-abukobe/)

<div align="center">

⭐ If these notes helped you, consider giving the repo a star.

</div>
