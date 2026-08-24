
---

# 🛡️ NetworkWalks B082 — Week 2 · Footprinting & Network Scanning
*🎓 Educational · ✅ Authorized · 🐧 Kali Linux · 🗺️ Zenmap · Week 2*

This repository documents Week 2 of my Cybersecurity & Ethical Hacking internship with NetworkWalks Academy (Batch B082). Week 2 is made up of two parts:

- 🔍 **PM1 — Footprinting & Reconnaissance:** collecting publicly available information about a target website (networkwalks.com) using six Kali Linux tools.
- 🖥️ **PM5 — Network Scanning with Zenmap:** identifying the live devices connected to my own home network.

Together, these cover the first two stages of a penetration test — learning about a target from the outside (footprinting), then mapping a network from within (scanning).

## ⚠️ Authorization & Scope

All work documented here was carried out under a written Letter of Authorization issued for the NetworkWalks B082 internship.

**✅ In scope:**
- 🌐 networkwalks.com — the client's website and DNS records (footprinting)
- 🏠 My own home LAN, which I own and control (scanning)

**🚫 Out of scope / not performed:** No exploitation or attacks were carried out — only passive information gathering and host discovery. The signed authorization is available at `docs/Letter-of-Authorization.pdf`.

> ⚠️ Please don't replicate any of this against a target you don't own or lack authorization to test. Scanning without permission can be illegal, even when no harm results.

**📌 A note on addresses:** All IP and MAC addresses shown belong to my own private home network — a lab environment under my control. Private addresses (192.168.x.x) and device MACs are only meaningful within that local network.

## 📋 Overview of Findings

### 🔍 PM1 — Footprinting networkwalks.com (public information)

| Item | Finding |
|---|---|
| 🏢 Registrar | GoDaddy — registered 2019, valid until 2027 |
| 👤 Owner | Hidden behind a privacy service (Domains By Proxy) |
| 🖥️ Hosting | HostGator (ns6135 / ns6136.hostgator.com) |
| 🌐 Server IP | 192.232.216.135 |
| ⚙️ Web server | Apache, with an Nginx caching layer |
| 📝 CMS / plugin | WordPress + WP Download Manager |
| 🔓 Exposed endpoint | WordPress REST API at /wp-json/ |
| 🔥 Firewall | ModSecurity (SpiderLabs) — the site is protected |
| 📡 DNS software | BIND 9.16.23 |
| ✉️ Mail | mail.networkwalks.com, managed through cPanel |

### 🖧 PM5 — Scanning my own home network (host discovery)

| Item | Finding |
|---|---|
| 🌐 Subnet scanned | 192.168.143.0/24 |
| ✅ Live hosts found | 1 (my own laptop) |

**💡 In short:** starting from nothing but a domain name, PM1 exposed the target's hosting provider, IP address, web stack, mail configuration, and firewall — all sourced from public information. PM5 then mapped a live network and turned up five devices, several of which were actively concealing their identity via MAC randomization.

## 🔎 PM1 — Footprinting & Reconnaissance

I used six Kali Linux tools to build a complete public profile of networkwalks.com without interacting with the site in any harmful way.

| # | Tool | Purpose |
|---|---|---|
| 1️⃣ | `whois` | 🏢 Retrieve the domain's registration details |
| 2️⃣ | `whatweb` | 🔬 Fingerprint the site's web technologies (see note) |
| 3️⃣ | `nslookup` | 🌐 Resolve the domain to its IP address |
| 4️⃣ | `curl -I` | 📄 Inspect the HTTP response headers |
| 5️⃣ | `wafw00f` | 🔥 Detect a Web Application Firewall |
| 6️⃣ | `dnsrecon` | 📡 Enumerate all DNS records |

**📸 Evidence**

- 🏢 `whois` — domain registration details (GoDaddy registrar, HostGator name servers)

  *whois output*

- 🌐 `nslookup` — domain resolved to IP (192.232.216.135)

  *nslookup output*

- 📄 `curl -I` — HTTP headers (Apache, WordPress, WP Download Manager cookie, /wp-json/)

  *curl headers output*

- 🔥 `wafw00f` — firewall detection (site sits behind ModSecurity)

  *wafw00f output*

- 📡 `dnsrecon` — DNS records (name servers, BIND version, MX, SPF, cPanel records)

  *dnsrecon output*

## 🖧 PM5 — Network Scanning with Zenmap

The goal here was to identify the live devices on my home network, along with their IP and MAC addresses, and export a network topology diagram.

Since Zenmap is no longer supported on modern Kali Linux (it relies on deprecated Python 2), I installed the official Nmap + Zenmap package on Windows instead and scanned my LAN from there.

**⌨️ Scan command:**
```
nmap -sn 192.168.143.0/24
```
*(a ping scan — identifies live hosts without scanning ports)*

**✅ Result — 1 live host found:**

| IP | MAC | Device |
|---|---|---|
192.168.143.0| | unknown 

The scan picked up  my own laptop — a deliberate privacy feature in modern phones and laptops that prevents Nmap from identifying their manufacturer. My laptop appeared as "up" but without a MAC address (since a device can't ARP itself), so I retrieved its MAC separately using `ipconfig /all`.

**📸 Evidence**

- 🖥️ Zenmap installed and running on Windows

  *Zenmap installed*

- 📊 Ping scan — Nmap output (live hosts and MAC addresses)

  *Nmap output*

- 🗺️ Network topology (exported as PDF — see `output/Topology.pdf`)

  *Network topology*

## 💡 What I Learned

- 🔗 Footprinting and scanning are two halves of the same reconnaissance phase — one gathers external, public information about a target, the other actively maps a network from within.
- 🧰 No single tool is indispensable. When whatweb failed, the other tools still surfaced the same information.
- 🌐 A single domain name can reveal a surprising amount — from networkwalks.com alone, I uncovered the hosting provider, IP address, web stack, mail setup, and firewall.
- 🏠 Real-world networks don't behave like lab environments — my home network scan turned up devices actively masking their identity through MAC randomization, something a controlled lab setup wouldn't typically show.

## 🛠️ Problems Faced & Solutions

| Problem | Solution |
|---|---|
| ⚠️ verification of zenmap hosts | ✅ Verified the same network host address after typing the ipconfig command |


```

## 📄 Full Report

The complete penetration testing report covering both modules — including the disclaimer, methodology, findings, risk analysis, recommendations, and supporting evidence — is available here:
[Terry Nyambe penetration testing report.docx](https://github.com/user-attachments/files/31382861/Terry.Nyambe.penetration.testing.report.docx)
 

## ⚖️ Disclaimer

🎓 For educational and research purposes only. All activity documented here was conducted against targets I own or was explicitly authorized to test, under the Letter of Authorization included in this repository. ⚠️ Unauthorized scanning or reconnaissance can carry serious legal consequences.
