# Intro to Communication Protocols

## HTTP (HyperText Transfer Protocol)
HTTP is an application-layer protocol that runs on top of TCP. It uses request and response messages where a client sends an HTTP request and a server replies with an HTTP response. HTTP is the foundation of web browsing and is used for loading web pages, APIs, and other web resources.

- Full form: HyperText Transfer Protocol
- Transport layer: TCP
- Main concept: request/response model
- Use cases: web browsing, REST APIs, loading HTML/CSS/JavaScript, accessing web resources

## HTTP/3 (QUIC)
HTTP/3 is the latest version of HTTP that runs over QUIC instead of TCP. QUIC uses UDP as its transport and provides faster connection establishment, multiplexing without head-of-line blocking, and lower latency. It is designed for modern web use and improves performance for users and content delivery.

- Full form: HyperText Transfer Protocol version 3
- Transport layer: UDP (via QUIC)
- Main concept: request/response model with lightweight, fast connections
- Use cases: faster web browsing, streaming, real-time application delivery, mobile and IoT optimized traffic

## HTTPS (HyperText Transfer Protocol Secure)
HTTPS is HTTP with encryption and authentication. It still uses TCP, but it secures traffic using TLS/SSL. During connection setup, the client and server perform a TLS handshake, exchange certificates, verify identities, and agree on encryption keys before sending encrypted HTTP packets.

- Full form: HyperText Transfer Protocol Secure
- Transport layer: TCP
- Main concept: encrypted HTTP over TLS/SSL with secure handshake
- Use cases: secure web browsing, online banking, e-commerce, login pages, any sensitive data transfer

## WebSocket
WebSocket is a protocol that upgrades an initial HTTP connection into a persistent, full-duplex communication channel. After the HTTP handshake and upgrade request, the client and server maintain an open socket for bidirectional data exchange, making it ideal for real-time apps.

- Full form: WebSocket (no expanded acronym)
- Transport layer: initially HTTP, then upgraded to TCP socket
- Main concept: HTTP upgrade to full-duplex socket communication
- Use cases: live chat, notifications, multiplayer games, real-time dashboards, collaborative editing

## SMTP (Simple Mail Transfer Protocol)
SMTP is the standard protocol for sending email across the internet. It defines how mail clients and servers transfer email messages, usually from a sender client to a mail server and then between mail servers until delivery.

- Full form: Simple Mail Transfer Protocol
- Transport layer: TCP
- Main concept: email transfer from sender to mail server and between servers
- Use cases: sending email, mail server delivery, outbound mail relays, transactional notifications

## FTP (File Transfer Protocol)
FTP is a protocol for transferring files between a client and a server. It uses separate channels for control commands and data transfer, allowing file listing, upload, and download operations in a structured session.

- Full form: File Transfer Protocol
- Transport layer: TCP
- Main concept: control channel plus data channel for file transfers
- Use cases: uploading or downloading files, website publishing, file sharing, remote file management
