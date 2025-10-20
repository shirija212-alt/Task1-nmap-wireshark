# Task 1 — Local Network Port Scanning & Reconnaissance

This task was completed as part of the Cyber Security Internship (Task 1: “Scan your local network for open ports”) to demonstrate basic reconnaissance skills using **Nmap** and packet-level verification using **Wireshark**.

---

## ✅ Objective
To identify live hosts and open ports/services in the local network (**10.161.135.0/24**) and understand the type of exposure they create, using both:
- **Active network scanning** (Nmap)
- **Packet-level verification** (Wireshark)

---

## ✅ Environment Details
| Component | Details |
|----------|---------|
| Scanner Host | Kali Linux (10.161.135.85) |
| Interface | `eth0` |
| Subnet | 10.161.135.0/24 |
| Nmap Version | 7.95 |
| Wireshark Version | GUI latest (default Kali repo) |

---

## ✅ Scanning Methodology

### 1️⃣ Discovery & Quick SYN Scan
```bash
sudo nmap -sS -Pn --open -T4 10.161.135.0/24 -oN scans/syn_scan.txt
```

### 2️⃣ Deep Scan on Specific Host (full 65535 TCP ports)
```bash
sudo nmap -sS -sV -O -Pn -p- 10.161.135.85 -oN scans/host_10.161.135.85_full.txt
```

### 3️⃣ UDP Top Ports Scan
```bash
sudo nmap -sU --top-ports 50 -Pn -T3 10.161.135.85/24 -oN scans/udp_top50.txt
```

---

## ✅ Key Results

| Host IP | Protocol | Port | State | Service |
|--------|----------|------|--------|---------|
| 10.161.135.24 | UDP | 53 | open | DNS resolver |
| 10.161.135.67 | TCP | 443 | open | HTTPS |
| 10.161.135.185 | TCP/UDP | - | closed | No service exposed |
| 10.161.135.85 (scanner) | - | - | - | Kali attacker node |

---

## ✅ Packet-Level Evidence (Wireshark)
Screenshots stored in `/screenshots`:

| Screenshot | What it Proves |
|------------|----------------|
| `wireshark_syn.png` | Scanner sent SYN packets (active scan) |
| `wireshark_synack.png` | 10.161.135.67 responded → OPEN TCP/443 |
| `wireshark_dns.png` | 10.161.135.24 responded → UDP/53 open |
| `scan_capture.pcapng` | Full packet capture of the scan |

Example SYN → SYN/ACK validation (extracted):
```
10.161.135.85  → 10.161.135.67  [SYN]
10.161.135.67  → 10.161.135.85  [SYN, ACK]  ✅ (Port 443 Open)
```

Example UDP DNS response:
```
10.161.135.85  → 10.161.135.24  [UDP 53 PTR Query]
10.161.135.24  → 10.161.135.85  [Standard Query Response] ✅ (DNS running)
```

---

## ✅ Interpretation
- TCP/443 open means there is a **web application / HTTPS service** running.
- UDP/53 open means a **DNS service** is available on the network.
- These services can leak metadata / hostnames (reverse DNS confirms internal queries).
- Scanner host (`10.161.135.85`) had **no open ports**, showing a hardened client system.

---

## ✅ Hardening Recommendations
| Risk | Recommendation |
|------|----------------|
| HTTPS not filtered internally | Restrict access using internal ACL/firewall |
| Open DNS internally | Limit queries to trusted devices |
| Discovery via PTR leaks | Harden DNS & disable unnecessary reverse lookup |
| No segmentation | Place DNS on management/VLAN |

---

## ✅ Repository Structure
```
task1/
├── README.md
├── findings.md
├── scan_evidence.md
├── scans/
│   ├─ host_10.161.135.85_full.txt
│   ├─ udp_top50.txt
│   ├─ tcp_small_test.txt
│   └─ commands.txt
└── screenshots/
    ├─ wireshark_syn.png
    ├─ wireshark_synack.png
    ├─ wireshark_dns.png
    ├─ scan_capture.pcapng
    └─ nmap_scan.png
```

## ✅ Legal Notice
Only devices inside **my own local network** were scanned. No unauthorized scanning was performed.

---

**Prepared by:**  
Shivas — Cybersecurity Student & Intern
