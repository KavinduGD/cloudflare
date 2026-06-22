# DDOS Protection

Cloudflare's DDoS protection works at multiple layers, and it's actually one of the things that's always on once your domain is proxied through them (orange cloud).

How it works

- Network layer (L3/L4): Cloudflare's network sits in front of your origin server. Since your DNS points to Cloudflare's IPs (not your EC2's IP directly), volumetric attacks like SYN floods or UDP floods hit Cloudflare's massive global network instead of your small EC2 instance. They have far more capacity to absorb traffic than your single server ever could.
- Application layer (L7): This is where HTTP-based floods get filtered — things like a botnet hammering your login page or hitting random URLs to exhaust your server resources. Cloudflare analyzes traffic patterns and can distinguish legitimate visitors from attack traffic.

Key mechanisms

- Anycast routing: Your domain resolves to Cloudflare IPs that are broadcast from hundreds of data centers worldwide. An attack against kavindu.lk gets spread across this whole network rather than concentrated on one point.
- Rate limiting: You can set rules like "block an IP if it makes more than X requests per minute" — useful even for smaller-scale abuse, not just massive attacks.
- Automatic mitigation: Cloudflare's systems detect anomalous traffic spikes and challenge or drop suspicious traffic automatically, without you doing anything.
- "I'm Under Attack" mode: A more aggressive setting you can manually flip on, which puts every visitor through a JS challenge before reaching your origin. Good for emergencies, annoying for normal traffic, so you wouldn't leave it on permanently.
