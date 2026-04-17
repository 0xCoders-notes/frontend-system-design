# 🌐 Networking Fundamentals — for Frontend Engineers

> A plain-English guide to how computers talk to each other, how the internet works, and why any of it matters when you're building for the web.

---

## 1. What is a Network?

A **network** is just two or more devices connected so they can share information.

Your phone talking to your router? That's a network. Your laptop downloading a file from GitHub? That's a network. The entire internet? That's a *massive* network of networks.

Think of it like a postal system — devices are houses, and data is mail. Networking is the infrastructure that gets the mail from one house to another.

---

## 2. What is the Internet?

The internet is a **global network of interconnected computers and servers**, linked by physical cables (fiber optic, coaxial) and wireless signals.

No single company owns the internet. It's a cooperative system maintained by:

- **ISPs** (Internet Service Providers) — the companies that give you access (Jio, Airtel, Comcast, etc.)
- **IXPs** (Internet Exchange Points) — physical hubs where different networks connect and exchange traffic
- **Backbone providers** — companies that own the massive undersea and underground fiber cables spanning continents

> 🧠 Mental model: The internet is like a highway system. ISPs build roads to your house. IXPs are the major intersections. Backbone providers own the interstate highways.

---

## 3. How Connectivity Works

### Wired Connections
| Type | Speed | Use |
|------|-------|-----|
| Ethernet (Cat5e/Cat6) | Up to 10 Gbps | Home/office LAN |
| Fiber Optic | Up to 100+ Gbps | ISP backbone, data centers |
| Coaxial Cable | Up to 1 Gbps | Cable internet to homes |

### Wireless Connections
| Type | Range | Use |
|------|-------|-----|
| Wi-Fi (802.11ax / Wi-Fi 6) | ~30–100m | Home/office |
| 4G LTE | Several km | Mobile internet |
| 5G | ~500m (high freq) | Next-gen mobile |

### The Hardware Chain (Your Home)

```
[Your Device]
     ↓  (Wi-Fi or Ethernet)
[Router]  ← assigns local IP addresses, routes traffic between devices
     ↓  (Ethernet)
[Modem]  ← converts digital signals ↔ analog signals for the ISP line
     ↓  (Phone line / Fiber / Cable)
[ISP Network]
     ↓
[The Internet]
```

- **Modem** = the translator between your home network and your ISP
- **Router** = the traffic director inside your home network

---

## 4. IP Addresses

Every device on the internet has an **IP address** — a unique identifier, like a mailing address.

### Two versions:
- **IPv4**: `192.168.1.1` — 4 billion addresses (running out)
- **IPv6**: `2001:0db8:85a3::8a2e:0370:7334` — virtually unlimited

### Public vs Private IPs
- **Private IP**: Assigned by your router to devices in your home (e.g., `192.168.1.x`) — only visible inside your network
- **Public IP**: Assigned by your ISP to your router — visible to the internet

> Your router uses **NAT (Network Address Translation)** to map private IPs to the single public IP your ISP gave you.

---

## 5. DNS — The Internet's Phone Book

You type `google.com`. The browser doesn't know where that is. It needs an IP address.

**DNS (Domain Name System)** translates human-readable domain names into IP addresses.

```
google.com  →  DNS Lookup  →  142.250.182.46
```

### Domain Anatomy

```
https://mail.google.com
         ↑     ↑    ↑
    subdomain  SLD  TLD

- TLD (Top Level Domain): .com, .org, .in, .dev
- SLD (Second Level Domain): google, github, amazon
- Subdomain: mail, docs, api, www
- Root Domain: The invisible dot at the end (google.com.)
```

**ICANN** (Internet Corporation for Assigned Names and Numbers) is the non-profit that oversees domain name registration globally. **WHOIS** is a public database where you can look up who owns a domain.

---

## 6. Servers and Data Centers

A **server** is just a computer that listens for requests and sends back responses. It runs 24/7, has high uptime guarantees, and is usually located in a **data center**.

A **data center** is a large facility with:
- Thousands of servers racked in rows
- Redundant power supplies (backup generators)
- Cooling systems (servers generate huge heat)
- Physical security
- Multiple internet connections (redundancy)

Big players like Google, AWS, and Cloudflare have data centers on every continent, which is how they serve users with low latency globally.

---

## 7. ISP Hierarchy

```
Your Device
     ↓
Local ISP (e.g., your city's cable company)
     ↓
Regional ISP (e.g., state-level or national carrier)
     ↓
Global/Tier-1 ISP (e.g., AT&T, NTT — own the backbone)
     ↓
Peering / IXP
     ↓
Other Global ISPs
     ↓
Destination Server
```

- **Local ISP**: The last-mile provider — connects your home to the network
- **Regional ISP**: Aggregates many local ISPs; buys transit from Tier-1
- **Global/Tier-1 ISP**: The internet's backbone; peers directly with other Tier-1s without paying

---

## 8. What Happens When Data Gets Lost?

The internet uses **TCP (Transmission Control Protocol)** to ensure reliable delivery.

TCP breaks data into **packets**, sends them, and waits for acknowledgment (ACK). If no ACK arrives in time:

1. The sender **retransmits** the lost packet
2. This continues until confirmed received or connection times out

For cases where speed matters more than reliability (like video streaming), **UDP** is used instead — it sends packets and doesn't wait for confirmation.

---

## 9. The Protocol Stack (Quick Mental Model)

| Layer | What it does | Example |
|-------|-------------|---------|
| Application | What the data means | HTTP, DNS, SMTP |
| Transport | Reliable delivery | TCP, UDP |
| Internet | Routing between networks | IP |
| Network Access | Physical transmission | Ethernet, Wi-Fi |

As a frontend engineer, you mostly live at the **Application layer** — but understanding what's beneath helps you debug, optimize, and reason about latency.

---

## 10. Why This Matters for You as a Frontend Engineer

| Concept | Where it shows up in your work |
|---------|-------------------------------|
| DNS | Slow DNS resolution = slow TTFB |
| TCP/TLS | Connection setup adds latency; HTTP/2 helps |
| IP & Routing | CDN placement affects load time |
| ISP hierarchy | Users in rural areas have more hops |
| Data loss/retry | Network tab shows failed/retried requests |
| Servers | Choosing region affects latency for your users |

---

*Next: See `doc2-how-the-web-works.md` for the full deep-dive into what happens from URL to rendered page.*