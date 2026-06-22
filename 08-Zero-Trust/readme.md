# Zero Trust

- That's Cloudflare Zero Trust (Zero Trust Network Access platform) — the broader product that Cloudflare Tunnel is actually part of.
- The core idea behind Zero Trust: Instead of "anyone inside the network/VPN is trusted," every single request — even from your own devices — has to prove who it is before getting access. No implicit trust based on network location.

## VPN vs Zero Trust: A Quick Comparison

### Traditional VPN

- You connect once, and then you're "inside" the network — like being handed a key to the whole building.
- Trust is location-based: once authenticated, your device can usually reach anything on that network unless other firewalls stop it.
- Needs a VPN server/gateway exposed to the internet (a target for attackers), and client software on every device.
- Scaling and managing access for many users/services gets clunky — typically all-or-nothing network-level access.

### Cloudflare Zero Trust

- Built on the principle "never trust, always verify" — no implicit trust just because you're connected.
- Every request to every app/resource is checked individually: who you are, what device you're on, is MFA satisfied, does your identity provider say you're allowed, etc.
- Access is application-specific, not network-wide. You might be allowed to reach your internal dashboard but not SSH into the EC2 box, even though both sit behind the same Tunnel.
- No exposed VPN server: this is where Cloudflare Tunnel comes in. Your EC2 instance makes an outbound connection to Cloudflare, so there's no open inbound port for attackers to find or hit. Combined with Access policies, this is "Zero Trust Network Access" (ZTNA).
- Identity-aware: integrates with Google/GitHub/Okta/etc. so access decisions are tied to who someone is, not just whether they have a VPN config file.
