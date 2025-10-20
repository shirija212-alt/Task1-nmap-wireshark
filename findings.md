# Findings & Risk Analysis

## 1️⃣ Live Hosts Identified
| IP | Status | Notes |
|----|--------|-------|
| 10.161.135.24 | responsive | DNS (UDP/53) open |
| 10.161.135.67 | responsive | HTTPS (TCP/443) open |
| 10.161.135.185 | responsive | no open ports |
| 10.161.135.85 | scanner | Kali VM |

## 2️⃣ Service Exposure Summary
| Host | Port | Service | Risk Level | Why |
|------|------|----------|------------|-------|
| 10.161.135.67 | 443/tcp | HTTPS | Medium | Potential SSL misconfig/leakage |
| 10.161.135.24 | 53/udp | DNS | Medium | Metadata leakage / MITM risk |
| 10.161.135.85 | none | none | Low | No exposure |

## 3️⃣ Attack Surface Assessment
1. DNS response traffic proves name resolution inside LAN.
2. HTTPS server reachable means internal web surface exists.
3. No firewall prompts suggests internal traffic allowed laterally.

## 4️⃣ Mitigation Recommendations
| Control | Action |
|---------|---------|
| Network Segmentation | isolate DNS on mgmt/VLAN |
| Firewall Restriction | limit 443 to known systems |
| Logging | enable DNS query logging |
| TLS security | verify certificate chain/security |

## 5️⃣ Conclusion
The network has **two visible service exposures** (443 + 53) verified **not just by Nmap but by packet-level Wireshark proof**, demonstrating accurate reconnaissance methodology.
