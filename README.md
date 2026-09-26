# Velocity

⚡ **Velocity** is a polished, open-source internet speed test web app built with **HTML + CSS + vanilla JavaScript**.

It measures:
- Ping (median RTT)
- Jitter (mean absolute delta)
- Download speed (peak sustained throughput)
- Upload speed

It is designed for GitHub Pages deployment with zero config and works from repository sub-paths using only relative paths.

## Features

- Single-page app in `index.html` (embedded CSS + JS)
- Distinctive animated gauge + live throughput graph
- EN ⇄ TA language toggle for UI strings
- Theme modes: dark / light / auto
- Quick and Standard modes
- Cancel support across in-flight requests
- Route transparency: global edge vs site-host fallback
- Friendly blocked-network/offline messaging
- Local history in `localStorage` with mini chart
- Result sharing: copy text + PNG export
- Mascot "Volt" with state-based animations/messages
- PWA shell support (`manifest.json`, `sw.js`)

## Measurement flow

Each run executes sequentially:

1. **Latency & jitter**
   - Tries 10 cache-busted requests to:
     - `https://speed.cloudflare.com/__down?bytes=0`
   - Calculates:
     - Ping = median RTT (ms)
     - Jitter = mean absolute delta between consecutive RTT samples
   - Fallback: `assets/ping.txt` (shown as **Site host fallback**)

2. **Download**
   - Parallel requests (Quick: 4, Standard: 6)
   - Cloud route:
     - `https://speed.cloudflare.com/__down?bytes=N`
   - Fallback route:
     - `assets/speed-payload.bin`
   - Reports sustained throughput in Mbps with warmup discarded and live updates

3. **Upload**
   - Uploads generated binary data via POST (no user data)
   - Endpoint:
     - `https://speed.cloudflare.com/__up`
   - 3 parallel connections

## Resilience and privacy

- 10s timeout applied to fetches
- Handles online/offline events
- Gives clear likely-cause message for blocked networks (ad-blockers, Brave Shields, firewalls, VPNs, offline)
- No backend, no API keys, no cookies, no analytics, no trackers
- No personal data stored/transmitted
- ISP/city/country (if available) is retrieved from `https://ipapi.co/json/`
- Honest limitation shown in UI:
  - Browsers cannot read SIM operator or phone number

## Run locally

No build tools required.

- Open `index.html` directly, or
- Serve repository root with any static server

## GitHub Pages deployment

1. Push repository to GitHub
2. Open **Settings → Pages**
3. Under **Build and deployment**, choose **Deploy from a branch**
4. Select **main / (root)**
5. Save

Your app will be available at:
`https://<user>.github.io/<repo>/`

## Troubleshooting

- **Ad-blocker / Brave Shields**: allow the test endpoints and retry
- **Corporate firewall / VPN**: restrictive routing may block Cloudflare test endpoints
- **Offline**: reconnect and run again
- **Fallback route in use**: site-host fallback can measure locally but may differ from global edge behavior

## Regenerate `assets/speed-payload.bin`

From repository root:

```bash
dd if=/dev/urandom of=assets/speed-payload.bin bs=1024 count=1024
```

Expected file size: **1,048,576 bytes (1 MiB)**.

## License

MIT (see [`LICENSE`](./LICENSE)).
