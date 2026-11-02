# Soyuz Lookup

> Comprehensive Bluesky profile intelligence, in one file. A **Soyuz** product.
> Built and programmed by **Asad AK**.

Live at **https://lookupsoyuz.vercel.app**

---

## What it is

Soyuz Lookup is a single-file web tool that surfaces everything the AT Protocol exposes about any Bluesky account — identity, hosting, network, behavior, engagement, full DID history, posting timeline, trust analysis, and derived analytics like timezone inference and AI/scammer likelihood scoring.

It runs entirely in your browser. No backend, no server, no database. Just open the HTML file (or visit the Vercel URL) and start searching.

The UI features animated gradient backgrounds, glassmorphic cards, count-up stat animations, an animated trust gauge, and a distinctive violet-cyan-pink color palette — designed to feel premium, not generic.

---

## Features

Enter any **@handle**, **DID**, or **bsky.app/profile/...** URL and Soyuz Lookup returns:

### Identity & Account
| # | Data point | Source |
|---|---|---|
| 1 | **DID** | From `app.bsky.actor.getProfile` |
| 2 | **Handle** | From profile |
| 3 | **Account Created** | `profile.createdAt`, fully formatted |
| 4 | **Account Age** | Computed from `createdAt` — years / months / days + total days |
| 5 | **Last Active** | Most recent `indexedAt` from the author feed |
| 6 | **Indexed At** | When Bluesky last indexed the profile |
| 7 | **DID Method** | `did:plc` (Bluesky PLC) or `did:web` (self-hosted) |
| 8 | **Account Type** | Individual / Organization / Bot / Labeler / Feed Generator |
| 9 | **DID Created** | First PLC operation timestamp |
| 10 | **Last DID Update** | Most recent PLC operation |

### Server & Hosting (the honest version)
| # | Data point | Source |
|---|---|---|
| 11 | **PDS Endpoint** | From DID document at `plc.directory` |
| 12 | **PDS Region** | Inferred from PDS hostname (e.g. `morel.us-east.host.bsky.network` → *United States · East*) |
| 13 | **PDS Hostname** | Parsed from PDS endpoint URL |
| 14 | **PDS IP Address** | Resolved live via Cloudflare DNS-over-HTTPS |
| 15 | **PDS Hosting Org / ASN** | From `ipapi.co` IP intelligence |
| 16 | **PDS Datacenter Location** | City, region, country, timezone of the server |

> **Important:** The IP and datacenter location belong to the **PDS server** hosting the account's data — NOT the account owner. Bluesky's API does not expose user IPs. The UI says so explicitly.

### Verification & Labels
| # | Data point | Source |
|---|---|---|
| 17 | **Verification status** | `profile.verification.verifiedStatus` (valid / invalid / none) |
| 18 | **Verified by** | `profile.verification.issuer` |
| 19 | **All labels** | Self-labels, system labels, third-party labeler labels — grouped by source |

### Posting Timeline (new)
| # | Data point | Source |
|---|---|---|
| 20 | **Posting timeline chart** | Bar chart showing exact post counts per day (or per week if span > 180 days) from account creation to now |
| 21 | **Peak posting day/week** | The day/week with the most posts, with exact count |
| 22 | **Total posts in timeline** | Sum of all posts in the timeline |
| 23 | **Timeline span** | Number of days since account creation |

### Trust Analysis · AI / Scammer Radar (new)
| # | Data point | Source |
|---|---|---|
| 24 | **Trust score (0-100)** | Computed from 11+ factors including account age, follower ratio, avatar, bio, verification, labels, posting behavior, engagement, timezone regularity |
| 25 | **Trust category** | Likely Legitimate / Probably OK / Suspicious / High Risk |
| 26 | **AI/Scammer likelihood %** | Inverse of trust score |
| 27 | **Factor breakdown** | Every factor that contributed to the score, with + or - impact |
| 28 | **Animated gauge** | SVG semicircle gauge with colored segments (red → amber → lime → green) and animated needle |

### Posting Activity by Hour
| # | Data point | Source |
|---|---|---|
| 29 | **Posting frequency chart** | Bar chart, 24 hourly buckets (UTC) |
| 30 | **Peak posting hour** | Hour with most posts |
| 31 | **Estimated timezone** | Inferred from sleep/wake pattern (lowest posting hours) |
| 32 | **Original vs replies vs reposts** | Counts + percentages |
| 33 | **Posts with media** | Images / video / external cards |
| 34 | **Posts analyzed** | All posts scanned (up to 2000) |

### Engagement Stats
| # | Data point | Source |
|---|---|---|
| 35 | **Avg likes / post** | Aggregated from ALL posts scanned |
| 36 | **Avg reposts / post** | Aggregated from ALL posts scanned |
| 37 | **Avg replies / post** | Aggregated from ALL posts scanned |
| 38 | **Avg quotes / post** | Aggregated from ALL posts scanned |
| 39 | **Top post by engagement** | Post with highest combined engagement, with preview + link |

### Content Analysis
| # | Data point | Source |
|---|---|---|
| 40 | **Top hashtags** | `tag` facets from ALL posts, with counts |
| 41 | **Top domains linked** | Aggregated from all links, with counts |
| 42 | **ALL accounts mentioned** | `mention` facets + reply parents from ALL posts (up to 2000), with frequency + first-seen date — captures every client mentioned in art commission posts |
| 43 | **Links on account** | URLs in bio + `link` facets + external embeds from ALL posts |

### Network
| # | Data point | Source |
|---|---|---|
| 44 | **Followers count + list** | `app.bsky.graph.getFollowers` — paginated |
| 45 | **Following count + list** | `app.bsky.graph.getFollows` — paginated |
| 46 | **Lists created by account** | `app.bsky.graph.getLists` |
| 47 | **Starter packs created** | `app.bsky.graph.getStarterPacks` |
| 48 | **Lists containing account** | Best-effort (no direct API; UI explains the limitation) |

### DID Document & History
| # | Data point | Source |
|---|---|---|
| 49 | **DID** | DID document `id` |
| 50 | **Also Known As** | All handles / AT URIs the DID has been associated with |
| 51 | **Signing keys** | All `verificationMethod` entries with key IDs |
| 52 | **Rotation keys** | All `rotationKeys` entries |
| 53 | **Operation history** | Full PLC audit log — every operation with timestamp, type, and CID |

### What's NOT exposed (and the UI says so honestly)
- **Account owner's IP address** — Bluesky's API never returns this. The PDS IP shown is the server's IP, not the user's. The UI labels this clearly.
- **Device / Client used to post** — The AT Protocol post record schema has no device or client field. The UI explains this rather than fabricating data.
- **Lists containing an account** — No API endpoint exists for this. The UI notes the limitation.

---

## UI

- **Dark / light theme toggle** — preference saved in `localStorage`
- **Fully responsive** — verified on mobile (390 px) and desktop (1400 px)
- **Animated gradient background** — floating gradient blobs that slowly drift
- **Glassmorphic cards** — backdrop-blur with semi-transparent surfaces
- **Count-up stat animations** — follower/post counts animate from 0 to target
- **Card entrance animations** — cards fade and slide up as they render
- **Animated bar charts** — bars rise from baseline with staggered delays
- **Animated trust gauge** — SVG needle animates to the score position
- **Hover effects** — cards lift, buttons glow, avatars scale
- **Distinctive color palette** — violet → cyan → pink gradient system (not generic blue)
- **Soyuz branding throughout**:
  - **Circular Soyuz favicon** in the browser tab (SVG with circle clip-path, gradient ring)
  - **Animated spinning logo** in the header (slow rotation)
  - Prominent **Soyuz logo + wordmark** in the header
  - **"A Soyuz Product"** badge above the hero title
  - Soyuz logo watermark in the footer

---

## How authentication works

The tool uses **3 authenticated Bluesky accounts** (provided by the operator) to call Bluesky's XRPC endpoints. It does **not** use the public unauthenticated endpoint, which is heavily rate-limited and returns partial data.

- All 3 sessions are created on page load.
- API calls rotate between sessions automatically.
- On HTTP `429` (rate limit) or `401`/`403` (token expired), the tool transparently retries with the next session.
- This keeps lookups fast and avoids hitting a single account's rate ceiling.

> **Security note:** Credentials are embedded in the HTML for single-file deployment. If you fork this project, replace them with your own Bluesky app passwords (Settings → Privacy and Security → App Passwords on bsky.app).

---

## External services used (all CORS-enabled, all called from the browser)

| Service | Purpose | Auth required |
|---|---|---|
| `bsky.social/xrpc` | All Bluesky API calls | Yes (3 rotating sessions) |
| `plc.directory` | DID document resolution + PLC audit log | No (public) |
| `cloudflare-dns.com/dns-query` | DNS-over-HTTPS to resolve PDS hostname → IP | No (public) |
| `ipapi.co` | IP intelligence (ASN, hosting org, datacenter city) for PDS server | No (rate-limited) |

---

## How to run

### Option 1 — Use the hosted version

Visit **https://soyuzlookup.vercel.app/** — no setup required.

### Option 2 — Run locally

1. Download `soyuz-lookup.html`.
2. Double-click it. It opens in your default browser.
3. Start typing handles.

That's it. No dependencies, no build step, no Node, no npm install.

### Option 3 — Deploy to Vercel yourself

1. Create a new Vercel project.
2. Drop `soyuz-lookup.html` in the root.
3. Deploy. Vercel serves it as a static file.

---

## Tech stack

- **HTML5** — single file (≈ 102 KB, 1704 lines)
- **CSS** — embedded, with CSS variables for theming, animations, glassmorphism
- **Vanilla JavaScript** — no frameworks, no libraries, no bundler
- **AT Protocol XRPC** — `bsky.social/xrpc` for authenticated calls
- **PLC directory** — for DID document + audit log
- **Cloudflare DNS-over-HTTPS** — for live PDS hostname resolution
- **ipapi.co** — for PDS IP intelligence
- **No backend** — runs 100% in the browser

---

## File structure

```
soyuz-lookup.html     ← the entire app (≈ 94 KB, 2063 lines)
README.md             ← this file
```

---

## API endpoints used

| Endpoint | Purpose |
|---|---|
| `com.atproto.server.createSession` | Authenticate with handle + app password |
| `app.bsky.actor.getProfile` | Fetch the target profile |
| `app.bsky.graph.getFollowers` | List followers (paginated) |
| `app.bsky.graph.getFollows` | List following (paginated) |
| `app.bsky.graph.getLists` | Lists created by the account |
| `app.bsky.graph.getStarterPacks` | Starter packs the account has made |
| `app.bsky.feed.getAuthorFeed` | Recent posts — used for mentions, links, hashtags, engagement, posting pattern, timezone inference |
| `plc.directory/{did}` | DID document for PDS + location resolution |
| `plc.directory/{did}/log/audit` | Full PLC operation history |
| `cloudflare-dns.com/dns-query` | Resolve PDS hostname to IP |
| `ipapi.co/{ip}/json/` | IP intelligence for PDS server |

---

## Limitations (honest)

- **Account owner's IP** is not exposed by Bluesky's API, ever. The PDS IP shown is the server's.
- **Device / client data** is not exposed by Bluesky's API. The post record schema has no `device`, `client`, or `via` field.
- **"Lists containing account"** has no direct API. The UI surfaces the limitation.
- **Follower / following lists** are capped at 500 by default with "Load more" cursor pagination.
- **Recent posts scan** is limited to the latest 100 posts (`posts_no_replies` filter) for analytics.
- **Timezone inference** is a heuristic based on posting patterns — not exact.
- **DNS resolution** depends on Cloudflare's DoH service being reachable.
- **IP intelligence** depends on `ipapi.co` being reachable and not rate-limited.

---

## Legal & ethics

Soyuz Lookup fetches only data that Bluesky's API already returns to authenticated clients, plus public DNS and IP intelligence for the **PDS server** (not the user). It does **not**:

- bypass access controls,
- scrape private data,
- expose user IP addresses (because Bluesky doesn't expose them),
- store any data (everything is in-memory and discarded on page reload),
- make authenticated actions on behalf of the lookup user (no posting, no following, no muting),
- use link traps, tracking pixels, or any social-engineering technique.

All data shown is publicly visible on the target's Bluesky profile or derivable from public post metadata. Use it for research, journalism, due diligence, OSINT, and lawful personal curiosity. Don't use it to harass, dox, or profile people in ways that violate Bluesky's [Community Guidelines](https://bsky.social/about/support/community-guidelines).

---

## Credits

- **Programmer:** Asad AK
- **Organization:** Soyuz
- **Product name:** Soyuz Lookup
- **Powered by:** The AT Protocol, Bluesky's XRPC API, the PLC directory, Cloudflare DNS-over-HTTPS, and ipapi.co
- **Logos:** [Soyuz title-tag logo](https://iili.io/nrJY9Bs.png) (rendered as a circle via SVG clip-path) · [Soyuz heading logo](https://iili.io/nrJ7pQn.png)

---

## License

Provided as-is for personal and research use. If you fork it, keep the Soyuz credit and replace the embedded credentials with your own.

---

*Last updated: November 2026*
