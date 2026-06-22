# Tunnel

- Cloudflare Tunnel lets you expose a server (like your EC2 instance, local server) to the internet through Cloudflare without opening any inbound ports on your firewall or router.

- How it normally works without a tunnel:
  - You open port 80/443 on your EC2 security group, point your domain's DNS to your server's public IP, and the world connects directly to your server. Your origin IP is exposed (unless you're already proxying through Cloudflare).

- How Cloudflare Tunnel changes this:
  - You install cloudflared (a lightweight daemon) on your server.
  - cloudflared makes an outbound connection from your server to Cloudflare's edge network.
  - Cloudflare routes incoming requests for your domain through that same tunnel, back to your server.
  - Your server never exposes any public IP or open ports. All traffic is proxied through Cloudflare's network.

## Benefits of using Cloudflare Tunnel:

- You want zero open inbound ports. Right now your EC2 security group almost certainly still allows inbound 80/443 (and probably 22) from the internet, even with orange-cloud proxying — Cloudflare's proxy hides your IP from casual lookups, but if someone finds your real origin IP, those ports are still live and attackable. Tunnel removes that exposure entirely.
- Origin IP must stay completely hidden. Proxy mode (orange cloud) hides the IP in DNS, but determined recon (historical DNS records, SSL cert transparency logs, other subdomains pointing directly at the IP) can still find it. Tunnel means there's nothing to find — no listening service at all.
- You're behind NAT or don't have a public IP, e.g. running a server at home, in a Docker container with no port forwarding, or on a cloud instance without a public IP. Tunnel solves this without any router config.
- You want to expose SSH, RDP, or internal admin panels safely without ever opening those ports publicly. This is one of the strongest use cases — pairs well with Cloudflare Access for authentication.
- You're running multiple internal services (dashboards, dev environments, internal tools) you don't want indexed or scanned by the internet at all.
