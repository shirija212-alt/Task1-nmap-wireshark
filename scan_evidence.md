# Packet-Level Evidence (Wireshark)

This file includes human-readable evidence proving that the scanned ports are genuinely open by validating raw packets.

---

## ✅ TCP Evidence (SYN → SYN/ACK)

```
10.161.135.85  → 10.161.135.67   [SYN]   TCP Handshake Init
10.161.135.67  → 10.161.135.85   [SYN, ACK]   ✅ Port 443 OPEN
```

This matches the screenshot: `wireshark_synack.png`

---

## ✅ UDP Evidence (DNS)

```
10.161.135.85  → 10.161.135.24   [UDP 53] PTR Query
10.161.135.24  → 10.161.135.85   [DNS Response]  ✅ DNS running
```

This matches screenshot: `wireshark_dns.png`

---

## ✅ Additional Observations
- Multiple `no-response (open|filtered)` states → firewall or service filtering
- Closed ports return ICMP port unreachable on some hosts
- No TCP response for 10.161.135.85 (self-scan) → hardened scanner

---

This evidence **completes the chain of proof**:
`Finding → Nmap → Packet Capture → Validation`
