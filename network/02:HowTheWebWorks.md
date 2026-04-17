# 🕸️ How the Web Works — Beginner to Advanced

> A structured, three-layer breakdown of what actually happens from the moment you type a URL to the moment pixels appear on your screen. Written for frontend engineers who want to think like systems engineers.

---

# 🟢 LEVEL 1 — BEGINNER
## The Big Picture: What Happens When You Open a URL?

You type `https://google.com` and hit Enter. Here's the simple version:

```
Your Browser
     ↓ asks "where is google.com?"
DNS Server
     ↓ returns "it's at IP 142.250.x.x"
Your Browser
     ↓ connects to that server
Google's Server
     ↓ sends back the HTML page
Your Browser
     ↓ renders it on screen
```

That's the whole story at 10,000 feet. Now let's go deeper.

---

## The Physical Journey (Mobile Example)

When you use a mobile browser, the data physically travels like this:

```
Your Phone
  → (4G/5G radio signal)
Cell Tower
  → (fiber cable)
Phone Company's Network
  → (internet backbone)
DNS Server (finds the IP)
  → (routes to correct data center)
Google's Server
  → (same path, reversed)
Back to Your Phone
```

Every one of those hops adds **latency** — the delay before your data arrives.

---

## Key Terms You Need to Know

| Term | Plain English |
|------|--------------|
| **URL** | The address you type — `https://google.com/search?q=cats` |
| **IP Address** | The actual numeric address of the server |
| **DNS** | The service that converts `google.com` → `142.250.x.x` |
| **ISP** | Your internet provider (Jio, Airtel, Comcast...) |
| **Server** | A computer in a data center waiting for your requests |
| **Network Tab** | The browser DevTools panel showing all HTTP requests |
| **HTTP** | The language browsers and servers use to talk |
| **HTTPS** | HTTP with encryption (the `S` = Secure) |

---

## What is the Network Tab?

Open DevTools → Network Tab. It shows every request your browser makes:

```
GET /api/users        200 OK    45ms    application/json
GET /style.css        304       12ms    text/css
GET /hero-image.jpg   200 OK    180ms   image/jpeg
GET /analytics.js     200 OK    90ms    application/javascript
```

Each row is one request. You can see:
- **What** was requested (the URL)
- **Whether** it succeeded (status code)
- **How long** it took (timing)
- **What type** of data came back

This is your window into the web's communication layer.

---

## Wired vs Wireless — Does It Matter?

**Yes.** Wired (Ethernet) is more stable and faster than wireless because:
- No interference from walls, other devices, or neighbors
- Lower latency (typically 1–5ms vs 10–50ms for Wi-Fi)
- No signal drops

For most users browsing normally, Wi-Fi is fine. But for real-time apps (video calls, live trading, gaming), wired connections matter.

---

## Domain Names: What's in a URL?

```
https://docs.github.com/en/get-started
  ↑         ↑      ↑    ↑
Protocol  Subdomain SLD  TLD
                 └─ Second Level Domain
```

- **Protocol**: `https://` — how to communicate
- **TLD** (Top Level Domain): `.com`, `.org`, `.in`, `.dev`, `.io`
- **SLD** (Second Level Domain): `github` — the brand/name you register
- **Subdomain**: `docs` — a section of the same domain
- **Path**: `/en/get-started` — which specific page

**Root domain** = `github.com` (SLD + TLD together)

**ICANN** manages who can register TLDs. **WHOIS** lets you look up the owner of any domain.

---

## Servers and Data Centers (Simple View)

A **server** is just a powerful computer that:
1. Listens on a port (usually 80 for HTTP, 443 for HTTPS)
2. Receives your request
3. Sends back a response

**Data centers** are buildings full of these servers, with:
- 24/7 power and cooling
- Multiple internet connections
- Physical security
- Located in major cities around the world

---

# 🟡 LEVEL 2 — INTERMEDIATE
## A Deeper Look at the Full Request Lifecycle

### The ISP Hierarchy

Your data doesn't go directly to Google. It travels through a chain:

```
Your Device
  → Local ISP (your city's provider)
    → Regional ISP (state/national level)
      → Tier-1 / Global ISP (backbone owner)
        → Peering Point / IXP
          → Google's Network
            → Google's Server
```

**Local ISP**: The last mile — connects your home. Usually a cable company or telecom.

**Regional ISP**: Aggregates multiple local ISPs. Buys bandwidth upstream.

**Global ISP (Tier-1)**: Owns the physical internet backbone (undersea fiber, transcontinental cables). Examples: AT&T, NTT, Lumen. They peer directly with each other without money changing hands.

---

### What if Data Gets Lost or Fails?

The internet uses **TCP (Transmission Control Protocol)** which handles this automatically:

**How TCP ensures delivery:**
1. Data is split into numbered **packets** (e.g., packet 1, 2, 3, 4...)
2. Receiver sends back **ACK** (acknowledgment) for each packet
3. If ACK doesn't arrive within a timeout → sender **retransmits**
4. Packets may arrive out of order → TCP **reorders** them

**What this looks like in the Network Tab:**
- A request that stalls and then succeeds = TCP retransmit happened
- A request that fails = TCP gave up after max retries (connection timeout)
- `ERR_CONNECTION_RESET` = connection was forcefully closed
- `ERR_CONNECTION_TIMED_OUT` = no response after many retries

**UDP** skips this handshaking for speed — used in video calls, DNS, gaming. "Best effort" delivery.

---

### DNS Deep Dive

DNS lookup is a multi-step process:

```
Browser asks: "Where is google.com?"

1. Check browser's own DNS cache  → Not found
2. Ask the OS DNS cache           → Not found
3. Ask your Router's DNS cache    → Not found
4. Ask your ISP's DNS Resolver    → Not found
5. ISP Resolver asks Root Server  → "Ask .com nameserver"
6. ISP Resolver asks .com Server  → "Ask Google's nameserver"
7. ISP Resolver asks Google's NS  → "142.250.182.46"
8. Response cached at each level
9. Browser gets the IP
```

This is called **recursive DNS resolution**. In practice, most of the time the answer is cached at step 1–4 so it's nearly instant.

**TTL (Time to Live)**: Every DNS record has a TTL — how long it can be cached. This is why DNS changes can take time to propagate globally (from minutes to 48 hours).

**ICANN** manages the root zone (step 5). **WHOIS** is a public protocol/database to look up domain registration records.

---

### The Full Request Pipeline

Once DNS resolves the IP, this is the exact sequence your browser follows:

```
1. DNS Lookup               → Get IP address
2. TCP Handshake (3-way)    → Establish connection
   SYN → SYN-ACK → ACK
3. TLS/SSL Handshake        → Encrypt the connection
   - Exchange certificates
   - Verify server identity
   - Agree on encryption keys
4. HTTP Request             → Send GET / POST / etc.
5. Server processes request → Runs your code, hits database, etc.
6. Response in chunks       → HTML/JSON arrives in TCP packets
7. Browser parses response  → Rendering begins (see Level 3)
```

**Each step adds latency.** This is why:
- **HTTP/2** multiplexes requests (multiple over one connection)
- **HTTP/3 + QUIC** uses UDP to eliminate handshake overhead
- **Keep-Alive** reuses TCP connections across requests

---

### Browser Parallel Request Limits

**Browsers can only make 6–8 parallel TCP connections per domain** (HTTP/1.1 limit).

```
Requests 1–6   → Start immediately (parallel)
Request 7, 8   → WAIT in queue until a slot opens
Request 9, 10  → Wait even longer...
```

**Implications for frontend engineers:**
- Too many resources on one domain = waterfall queuing in the Network Tab
- **Domain sharding** (old trick): split resources across `cdn1.`, `cdn2.` etc. to get more parallel slots — mostly obsolete with HTTP/2
- **HTTP/2** solves this: multiplexing means many streams over one connection, no 6-limit
- **Bundle your JS/CSS** to reduce total request count on HTTP/1.1

---

# 🔴 LEVEL 3 — ADVANCED
## Under the Hood: Deep Web Mechanics

### The First Moments After Typing a URL

Before any network request fires, your browser checks multiple caches in this order:

```
1. Service Worker Cache      ← installed by your app's SW, fully custom
2. HTTP Cache (Browser)      ← Cache-Control, ETag, Last-Modified
3. DNS Cache (Browser)       ← avoids re-resolving known domains
4. Operating System DNS Cache
5. Router DNS Cache
6. ISP DNS Resolver Cache
7. ... then full resolution happens
```

**Service Worker** is the most powerful: it can intercept ANY network request and return cached content, serve offline pages, or modify responses before they reach the browser.

**HTTP Cache** is controlled by response headers:
```
Cache-Control: max-age=86400, public   ← cache for 1 day
ETag: "abc123"                          ← fingerprint for validation
Last-Modified: Mon, 17 Apr 2024...     ← timestamp for validation
```

On repeat visits: browser sends `If-None-Match: "abc123"` → server returns `304 Not Modified` (no body, just header) → browser uses cached version.

---

### What is Peering? How Google Gets Close to You

**Problem:** The more ISP hops between you and Google's server, the higher the latency.

**Peering** = two networks agree to exchange traffic directly without paying a third party. This bypasses ISP hierarchies.

**Google's solution — Google Global Cache (GGC):**
- Google places servers directly **inside or next to local ISPs**
- YouTube videos, Google fonts, Maps tiles — served from these local caches
- Latency drops from 100ms+ to <10ms for many users

**How it works in practice:**
```
Without peering:  You → Local ISP → Regional ISP → Tier-1 → Google (4+ hops)
With GGC:         You → Local ISP → [Google server inside ISP] (1 hop)
```

Google essentially **rents rack space** at regional and local ISPs, placing caches there. This is called **CDN at the ISP level** and it's why YouTube buffers less than smaller video platforms.

Other CDNs (Cloudflare, Akamai, Fastly) do the same — they have **Points of Presence (PoP)** in hundreds of cities globally, so your request hits a nearby node instead of crossing continents.

---

### SSL/TLS Handshake — What Actually Happens

```
CLIENT                          SERVER
  |                               |
  |──── ClientHello ─────────────>|   "I support TLS 1.3, here are cipher suites"
  |<─── ServerHello ──────────────|   "Let's use AES-256-GCM"
  |<─── Certificate ──────────────|   "Here's my SSL cert (signed by CA)"
  |<─── ServerHelloDone ──────────|
  |                               |
  |  [Browser verifies cert       |
  |   against trusted CA list]    |
  |                               |
  |──── ClientKeyExchange ───────>|   "Here's the session key (encrypted)"
  |──── ChangeCipherSpec ────────>|   "Let's switch to encrypted now"
  |──── Finished ────────────────>|
  |<─── ChangeCipherSpec ─────────|
  |<─── Finished ─────────────────|
  |                               |
  |  [Encrypted HTTP begins]      |
```

**TLS 1.3** (modern) reduces this to **1 round trip** (1-RTT), down from 2 RTT in TLS 1.2. Even 0-RTT is possible for repeat connections (session resumption).

The SSL cert is signed by a **CA (Certificate Authority)** like DigiCert, Let's Encrypt. Browsers ship with a built-in list of trusted CAs.

---

### How the Web Page Renders (Browser Rendering Pipeline)

After the HTML arrives, here's the precise sequence:

#### Phase 1 — Loading & Parsing

```
GET html → Response
  ↓
Parse HTML (Build DOM)  ← parser reads tokens, creates nodes
  |
  ├── Hits <link rel="stylesheet"> → GET CSS (render-blocking)
  ├── Hits <script src="...">     → GET JS (parser-blocking!)
  │     [Unless defer or async]
  └── FCP happens here (First Contentful Paint)
```

**Parser-blocking**: When the parser hits a `<script>` tag without `defer` or `async`, it **stops** and waits for the JS to download and execute. This is why `<script>` tags at the bottom of `<body>` was the old best practice.

**defer**: Script downloads in parallel but executes only after HTML is fully parsed. Preserves order. Fires before `DOMContentLoaded`.

**async**: Script downloads in parallel and executes **as soon as it loads** — interrupts parser if needed. No order guarantee. Good for analytics.

```
sync:   [=====BLOCK=====][execute][...rest of parsing...]
defer:  [...parsing...][       ][execute after parse]
async:  [...parsing...][execute when ready (unpredictable)]
```

#### Phase 2 — Style Calculation

```
Parse CSS → Build CSSOM
```

The **CSSOM** (CSS Object Model) cannot be built until ALL stylesheets are loaded — because any stylesheet can override any other. This is render-blocking by design.

CSS cascade resolution:
```
span.hidden gets:
  display: none     ← from .hidden rule
  display: block    ← from span rule (overridden)
  font-weight: bold ← inherited from p
  font-size: 14px   ← inherited from div
  font-size: 16px   ← inherited from body (overridden by div)
```

#### Phase 3 — Script Execution (Inside V8)

When JS executes, this is what happens in the V8 engine (Chrome):

```
Script Streamer Thread:   [Parse → AST]
                                ↓ AST Internalization
Main Thread:  [Other] ──────────────── [Compile → Bytecode] ── [Execute] ── [Other]
```

- **Parsing** happens on a background thread (script streaming)
- **Compile** (bytecode finalization) and **Execute** block the main thread
- This is why large JS bundles make pages feel slow — they block interaction (TTI)

**TTI (Time to Interactive)** = when the main thread is free enough for user interaction.

#### Phase 4 — Render Tree Construction

```
DOM + CSSOM → Render Tree
```

Non-visible nodes are **excluded** from the render tree:
- `<head>`, `<script>`, `<link>` — non-visual by nature
- Any element with `display: none` — excluded entirely
- `visibility: hidden` — still in render tree (takes up space), just invisible

```
DOM:           html → head(link, script) + body → div → p("some") + p → span("text")
                                                                          ↑ display:none

Render Tree:   html → body → div → p("some") + p
                                             (span excluded — display:none)
```

#### Phase 5 — Layout (Reflow)

The browser calculates **exact pixel positions and sizes** for every render tree node.

```
html    { height: 100% }
body    { width: 50% }
div     { height: 200px }
p       { width: 50% }
span    { position: absolute; right: 25px }
```

This produces a **box model** for every element.

**Reflow (Layout) is triggered by:**
- Modifying width, height, position, float
- User interaction (resize, scroll)
- JS reading layout properties (`offsetWidth`, `getBoundingClientRect`)
- Adding/removing DOM elements

⚠️ Reflow is expensive — it recalculates the entire affected subtree.

#### Phase 6 — Paint & Compositing

```
Layout Tree → Paint Tree → Layers → Composite → Pixels on Screen
```

**Paint**: The browser figures out what to draw (colors, borders, text, images) — not where (that's Layout).

**Repaint is triggered by:**
- Changing any visible CSS property (color, background, border-radius, box-shadow)
- Cheaper than reflow (no geometry change)

**Compositing**: The browser splits the page into **layers** (like Photoshop layers), paints them separately, and combines them on the GPU.

Properties that trigger GPU compositing (cheap to animate!):
- `transform` (translate, scale, rotate)
- `opacity`

**The golden rule for animations:**
```
❌ Animating: width, height, top, left  →  causes Layout + Paint + Composite (slowest)
⚠️  Animating: color, background         →  causes Paint + Composite (medium)
✅ Animating: transform, opacity        →  Composite only (fastest, 60fps)
```

---

### Full Request → Render Summary

```
Type URL
  → Check SW Cache / HTTP Cache / DNS Cache
  → DNS Resolution (recursive, multiple cache layers)
  → TCP Handshake (SYN → SYN-ACK → ACK)
  → TLS Handshake (cert exchange, session key, 1-RTT in TLS 1.3)
  → HTTP GET Request
  → Server processes (app logic, DB query, etc.)
  → Response in TCP packets (chunked transfer)
  → Parse HTML → Build DOM
  → Discover CSS → GET CSS → Build CSSOM  [render-blocking]
  → Discover JS → GET JS → Execute JS     [parser-blocking unless defer/async]
  → Merge DOM + CSSOM → Render Tree       [excludes display:none nodes]
  → Layout (Reflow) → calculate positions
  → Paint → draw pixels per layer
  → Composite → combine layers on GPU
  → Display on screen ✓
```

---

## Quick Reference: Performance Implications

| What You Do | What the Browser Does | Cost |
|-------------|----------------------|------|
| Add a sync `<script>` in `<head>` | Blocks parser until downloaded + executed | High |
| Add render-blocking CSS | Delays CSSOM → delays render tree | High |
| `element.style.width = '...'` in JS loop | Forces reflow on each read | Very High |
| Animate `transform` | GPU composite only | Low |
| Animate `top/left` | Triggers layout every frame | Very High |
| `display: none` on element | Removes from render tree | Medium (reflow) |
| `visibility: hidden` | Stays in render tree | Low (repaint) |
| Too many resources on one domain | Queues beyond 6 connections | High (HTTP/1.1) |
| Service Worker caches responses | Skips all network (instant) | None (best) |

---

*Companion doc: `doc1-networking-fundamentals.md`*