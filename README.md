# 🧠 QUB DNS Architecture Analysis – CatGPT Case Study (CSC3064)

A security-focused evaluation of DNS service arrangements for a growing AI startup, conducted as part of the **CSC3064 Network Security Assessment** at Queen’s University Belfast.

---

## 📋 Assessment Overview

The project evaluates five DNS configuration models for the fictional company **CatGPT**, analyzing trade-offs between simplicity, control, and security. It covers:
- DNS service topologies and walkthroughs
- Firewall policy design for each setup
- CIA (Confidentiality, Integrity, Availability) implications
- DNS-based command & control detection strategies
- A final recommendation based on real-world best practices

---

## 🧱 Repository Contents

| Folder              | Description                                                  |
|---------------------|--------------------------------------------------------------|
| `slides/`           | The presentation slides used in the assessment video         |
| `analysis/`         | Discussion, solution and breakdown of each DNS setup         |
| `assessment-pdf/`   | Official assessment brief provided by QUB                    |
| `diagrams/`         | All referenced DNS architecture diagrams (e.g., Split DNS)   |
| `video/`            | Video demo file                                              |

---

## ✅ DNS Configurations Analyzed

1. **ISP-Forwarded DNS** – Simple NAT forwarding to ISP’s resolver  
2. **Internal Resolver + Authoritative DNS** – Local control + DMZ hosting  
3. **Direct Public DNS** – Host-level public resolver usage  
4. **Split DNS (Hybrid)** – Internal + external separation (Diagram 2)  
5. **Split DNS (Flow-Level)** – Hardened DNS with DoT, RPZ, and rate limiting (Diagram 5)  

---

## 🔐 Security Comparison Tables

### 🔹 CIA Evaluation

| Setup               | Confidentiality         | Integrity                | Availability              |
|---------------------|--------------------------|---------------------------|----------------------------|
| ISP DNS             | ❌ No encryption         | ❌ No DNSSEC              | ✅ High but unfiltered     |
| Internal + DMZ      | ✅ Encrypted internally  | ✅ DNSSEC + ACLs          | ✅ Caching + fallback      |
| Public Direct       | ❌ Exposes client IPs    | ❌ Easily spoofed         | ✅ Unfiltered, no logging  |
| Split DNS (Hybrid)  | ✅ Internal/external split | ✅ RPZ + DNSSEC         | ✅ Secure + resilient      |
| Split DNS (Flow)    | ✅ Strongest encryption  | ✅ Full validation + RPZ  | ✅ Best overall resilience |

---

### 🔹 Firewall Policies (Sample)

| Setup             | Key FortiGate Rules |
|-------------------|----------------------|
| ISP-Forwarded     | Allow outbound UDP/53 to ISP |
| Internal + DMZ    | Allow UDP/53 LAN ⇄ 192.168.1.53; Allow TCP/853 outbound; Allow inbound UDP/53 to DMZ |
| Public Direct     | Allow UDP/53 from LAN to any |
| Split DNS Hybrid  | Same as above + restrict outbound |
| Flow-Level Split  | Add DoT/DoH filtering, rate limits, fallback lockdowns |

---

## 🎥 Video Presentation

📺 **[Watch the demo on YouTube](https://your-youtube-link-here)**  
_(Unlisted – available only via direct link)_

---

## 👤 Author

**Dominicus Adjie Wicaksono**  
Student ID: 40352799  
📧 dominicadjiew@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/dominicusadjie)
