bitchat for Africa (Android) 
bitchat is an Android messaging app that operates without internet or mobile networks. It uses Bluetooth and long-range technologies to connect phones across distances, enabling communication where internet is unavailable or costly. It avoids network control and protects user privacy.

# Key Features:

1. No mobile network needed; messages travel phone-to-phone via Bluetooth.
2. Functions in areas with slow, expensive, or no internet.
3. No phone number or mobile money required; no automatic charges.
4. Battery-efficient for outages and load shedding.
5. Messages hop through nearby phones to reach distant recipients.
6. Offline message storage until recipients reconnect.
7. Secure, password-protected group chats without central servers.
8. Small, efficient packets reduce data and power usage.
9. Fake messages sent occasionally to enhance privacy.
10. Plans to support WiFi Direct, LoRa, and optional secure internet.

# Technical Overview:
1. Bluetooth Low Energy Mesh Networking
1.1 Phones act as both broadcasters (peripherals) and listeners (centrals).
1.2 Broadcast a unique “service UUID” identifying bitchat nodes.
1.3 Scan for peers within ~30m and form local clusters.
1.4 Some phones bridge clusters for multi-hop reach.
1.5 Messages carry a TTL (starting at 7), decrementing each hop to prevent loops.

2. Message Relay and Store-and-Forward
2.1 Messages hop phone-to-phone, respecting TTL limits.
2.2 Offline recipients’ messages cached locally (12-hour expiry or indefinite for favorites).
2.3 Cached messages deliver when recipients reconnect.
2.4 Duplicate messages filtered with bloom filters to avoid resend.

3. Encryption and Security
3.1 Shared keys established via X25519 key exchange.
3.2 Messages encrypted with AES-256-GCM for confidentiality and integrity.
3.3 Ed25519 signatures verify message authenticity.
3.4 Group chats use password-derived keys with Argon2id hashing.
3.5 Random ephemeral peer IDs per session prevent tracking.

4. Binary Protocol and Fragmentation
4.1 Messages sent as compact binary packets with headers, timestamps, TTL, payload, and signatures.
4.2 Large messages split into ≤500-byte fragments labeled START, CONTINUE, END.
4.3 Fragments sent with 20ms spacing to maintain BLE stability.
4.4 Recipients reassemble full messages upon receiving all fragments.

5. Privacy Enhancements
5.1 Random “cover traffic” fake messages sent every 30–120 seconds.
5.2 Random 50–500 ms delays on send/receive hide timing patterns.
5.3 No user registration, phone numbers, or permanent IDs ensure anonymity.