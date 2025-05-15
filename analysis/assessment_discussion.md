{\rtf1\ansi\ansicpg1252\cocoartf2822
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica;}
{\colortbl;\red255\green255\blue255;}
{\*\expandedcolortbl;;}
\paperw11900\paperh16840\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\pard\tx566\tx1133\tx1700\tx2267\tx2834\tx3401\tx3968\tx4535\tx5102\tx5669\tx6236\tx6803\pardirnatural\partightenfactor0

\f0\fs24 \cf0 # \uc0\u55357 \u56536  Assessment Discussion \'96 DNS Architecture & Security Recommendations (CatGPT)\
\
**Dominicus Adjie Wicaksono**  \
Student ID: 40352799  \
Module: CSC3064 \'96 Network Security  \
Queen\'92s University Belfast\
\
---\
\
## \uc0\u55358 \u56800  Overview\
\
This document provides a technical discussion and solution analysis for the DNS architecture assessment based on the fictional CatGPT scenario. It covers five DNS configuration models, evaluates them against security principles (CIA), and recommends the most secure design, aligned with enterprise networking best practices.\
\
---\
\
## \uc0\u9881 \u65039  DNS Architecture Comparisons\
\
### 1. ISP-Forwarded DNS\
CatGPT relies on the ISP\'92s recursive DNS servers. Internal hosts send UDP/53 queries to FortiGate 60B, which Source NATs and forwards them upstream.  \
**Issues**: No encryption, DNSSEC, logging, or filtering.  \
**Assessment**: Simplistic but unsuitable for a secure organization.\
\
---\
\
### 2. Self-Hosted Internal Resolver + DMZ Authoritative DNS\
Internal resolver at `192.168.1.53` (e.g., Unbound or BIND) handles local queries, enforces ACLs, and forwards external queries using DoT (TCP/853).  \
A public DNS server at `165.120.2.6` in the DMZ responds to public domain queries.  \
**Benefits**: Full control, encryption, DNSSEC, logging, and filtering.  \
**Assessment**: A strong improvement in visibility and security.\
\
---\
\
### 3. Direct Public Resolvers\
Clients query public resolvers (e.g., `8.8.8.8`) directly. FortiGate NATs these as `165.120.2.5`.  \
**Issues**: Lacks DNSSEC, logging, filtering, and internal control.  \
**Assessment**: High risk; exposes internal systems and prevents monitoring.\
\
---\
\
### 4. Split DNS (Hybrid)\
Internal resolver answers local queries and securely forwards external queries via DoT.  \
Public-facing DNS is handled by the authoritative server in the DMZ.  \
**Benefits**: Secure split between internal/external, encrypted traffic, DNSSEC, ACLs, and RPZ filtering.  \
**Assessment**: Secure, scalable, and manageable architecture.\
\
---\
\
### 5. Split DNS \'96 Flow-Level Implementation\
Enhances the hybrid model with rate-limiting, anomaly detection, and stricter egress policies.  \
Outbound DNS uses DoT/DoH; internal resolution remains local; `.internal` domains are isolated.  \
**Assessment**: Offers the highest level of security and operational visibility.\
\
---\
\
## \uc0\u55357 \u56592  Firewall Policy Comparison\
\
| Setup         | Key FortiGate Rules                                                                 |\
|---------------|--------------------------------------------------------------------------------------|\
| ISP Forwarded | Allow outbound UDP/53 from LAN to ISP resolver                                       |\
| Internal + DMZ| Allow UDP/53 to internal resolver; TCP/853 to public; inbound UDP/53 to DMZ server   |\
| Public Direct | Allow outbound UDP/53 from LAN to any; block DNS from DMZ                           |\
| Hybrid Split  | Same as Internal + DMZ, plus deny unauthorized DNS traffic                          |\
| Flow-Level    | As above + rate-limiting, DoT/DoH enforcement, and restricted fallback resolvers     |\
\
---\
\
## \uc0\u55357 \u56590  CIA Evaluation Table\
\
| Setup       | Confidentiality       | Integrity              | Availability             |\
|-------------|------------------------|-------------------------|---------------------------|\
| ISP DNS     | \uc0\u10060  Unencrypted         | \u10060  No validation        | \u9989  High but unmonitored   |\
| Internal    | \uc0\u9989  Internal protection | \u9989  DNSSEC, ACLs         | \u9989  Caching, fallback      |\
| Public      | \uc0\u10060  Exposes clients     | \u10060  Spoofing risk        | \u9989  Always available       |\
| Hybrid      | \uc0\u9989  Split zones         | \u9989  RPZ, DNSSEC          | \u9989  Hardened resolution     |\
| Flow-Level  | \uc0\u9989  Strongest           | \u9989  Full validation & RPZ| \u9989  Best fault-tolerance    |\
\
---\
\
## \uc0\u55357 \u57057 \u65039  DNS-Based Command & Control Risk Analysis\
\
| Setup       | C2 Visibility & Prevention Summary                                                 |\
|-------------|-------------------------------------------------------------------------------------|\
| ISP DNS     | \uc0\u10060  No visibility or logging                                                         |\
| Internal    | \uc0\u9989  Internal logging and anomaly detection possible                                  |\
| Public      | \uc0\u10060  No monitoring; DNS tunneling undetectable                                        |\
| Hybrid      | \uc0\u9989  Logs and filtering; better but not exhaustive                                   |\
| Flow-Level  | \uc0\u9989  Best control: logs + RPZ + DoT + anomaly detection + rate limiting               |\
\
---\
\
## \uc0\u9989  Final Recommendation\
\
**Recommended DNS Setup**: **Split DNS (Flow-Level)** \'96 as outlined in Diagram 5.\
\
### Justification:\
- Enforces **DoT/DoH encryption** for outbound queries\
- Provides **internal query logging** and **RPZ filtering**\
- Supports **recursion ACLs** and **query rate-limiting**\
- Offers **full visibility** and **compartmentalization** of internal/external DNS\
- Prevents **DNS tunneling**, **data leakage**, and **spoofing attacks**\
- Scales effectively with CatGPT\'92s expected growth\
\
---\
\
\uc0\u55357 \u56524  This architecture achieves **maximum DNS security** while balancing **performance**, **internal visibility**, and **future scalability** \'97 ideal for an AI-based enterprise like CatGPT.}