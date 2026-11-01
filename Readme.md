# Soyuz Lookup

> Bluesky profile intelligence, in one file. A **Soyuz** product.
> Built and programmed by **Asad AK**.

Live at **https://lookupsoyuz.vercel.app**

---

## What it is

Soyuz Lookup is a single-file web tool that lets you look up any Bluesky account and instantly surface everything the AT Protocol exposes about it — proxy/PDS, location, DID, followers, following, lists, mentions, links, account age, and timeline metadata.

It runs entirely in your browser. No backend, no server, no database. Just open the HTML file (or visit the Vercel URL) and start searching.

---

## Features

Enter any **@handle**, **DID**, or **bsky.app/profile/...** URL and Soyuz Lookup returns:

| # | Data point | Source |
|---|---|---|
| 1 | **PDS Proxy** | Parsed from the DID document at `plc.directory` |
| 2 | **Location** | Region inferred from the PDS hostname (e.g. `morel.us-east.host.bsky.network` → *United States · East*) |
| 3 | **Followers list** | `app.bsky.graph.getFollowers` — paginated with "Load more" |
| 4 | **DID** | From `app.bsky.actor.getProfile` |
| 5 | **Last active** | Most recent `indexedAt` timestamp from the author feed |
| 6 | **Device / Client** | Honest disclosure — Bluesky's API does not expose this (see note below) |
| 7 | **Links on account** | URLs in bio + `link` facets + external embeds in posts |
| 8 | **Mentioned accounts** | `mention` facets and reply parents across recent posts, with frequency counts |
| 9 | **Account creation date** | `profile.createdAt`, fully formatted |
| 10 | **Account age** | Computed from `createdAt` — years / months / days + total days |
| 11 | **Following count + list** | `app.bsky.graph.getFollows`, paginated |
| 12 | **Lists created by account** | `app.bsky.graph.getLists` |
| 13 | **Lists containing account** | Best-effort — Bluesky exposes no direct endpoint; starter pack membership is shown where available |

### A note on device data

Bluesky's AT Protocol intentionally does **not** attach device or client metadata to posts (unlike Twitter's `source` field). The post record schema has no `device`, `client`, or `via` field. Soyuz Lookup tells you this honestly instead of fabricating data.

---

## UI

- **Dark / light theme toggle** — preference saved in `localStorage`
- **Fully responsive** — works on mobile and desktop, adapts to any screen size
- **Clean card-based layout** — no clutter, every data type in its own section
- **Branding** — Soyuz logo in the title tag and site heading

---

## How authentication works

The tool uses **3 authenticated Bluesky accounts** (provided by the operator) to call Bluesky's XRPC endpoints. It does **not** use the public unauthenticated endpoint, which is heavily rate-limited and returns partial data.

- All 3 sessions are created on page load.
- API calls rotate between sessions automatically.
- On HTTP `429` (rate limit) or `401`/`403` (token expired), the tool transparently retries with the next session.
- This keeps lookups fast and avoids hitting a single account's rate ceiling.

> **Security note:** Credentials are embedded in the HTML for single-file deployment. If you fork this project, replace them with your own Bluesky app passwords (Settings → Privacy and Security → App Passwords on bsky.app).

---

## How to run

### Option 1 — Use the hosted version

Visit **https://lookupsoyuz.vercel.app** — no setup required.

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

- **HTML5** — single file
- **CSS** — embedded, with CSS variables for theming
- **Vanilla JavaScript** — no frameworks, no libraries, no bundler
- **AT Protocol XRPC** — `bsky.social/xrpc` for authenticated calls
- **plc.directory** — for DID document resolution
- **No backend** — runs 100% in the browser

---

## File structure

```
soyuz-lookup.html     ← the entire app (≈ 58 KB, 1381 lines)
README.md             ← this file
```

---

## API endpoints used

| Endpoint | Purpose |
|---|---|
| `com.atproto.server.createSession` | Authenticate with handle + app password |
| `app.bsky.actor.getProfile` | Fetch the target profile |
| `app.bsky.actor.getProfiles` | Batch-resolve actor metadata |
| `app.bsky.graph.getFollowers` | List followers (paginated) |
| `app.bsky.graph.getFollows` | List following (paginated) |
| `app.bsky.graph.getLists` | Lists created by the account |
| `app.bsky.graph.getStarterPacks` | Starter packs the account has made |
| `app.bsky.feed.getAuthorFeed` | Recent posts — used for mentions, links, last active |
| `plc.directory/{did}` | DID document for PDS + location resolution |

---

## Limitations (honest)

- **Device / client data** is not exposed by Bluesky's API. We tell you so.
- **"Lists containing account"** has no direct API. Bluesky would need to add an endpoint like `app.bsky.graph.getListsContaining` for this to be complete. We surface starter pack membership as the closest available signal and explain the gap in the UI.
- **Follower / following lists** are capped at 500 by default to keep the UI snappy, with a "Load more" button for true pagination via cursors.
- **Recent posts scan** is limited to the latest 100 posts (`posts_no_replies` filter) for mentions and links extraction.

---

## Legal & ethics

Soyuz Lookup fetches only data that Bluesky's API already returns to authenticated clients. It does **not**:
- bypass access controls,
- scrape private data,
- store any data (everything is in-memory and discarded on page reload),
- make authenticated actions on behalf of the lookup user (no posting, no following, no muting).

All data shown is publicly visible on the target's Bluesky profile. Use it for research, journalism, due diligence, OSINT, and lawful personal curiosity. Don't use it to harass, dox, or profile people in ways that violate Bluesky's [Community Guidelines](https://bsky.social/about/support/community-guidelines).

---

## Credits

- **Programmer:** Asad AK
- **Organization:** Soyuz
- **Product name:** Soyuz Lookup
- **Powered by:** The AT Protocol and Bluesky's public XRPC API
- **Logos:** [Soyuz title-tag logo](https://iili.io/nrJY9Bs.png) · [Soyuz heading logo](https://iili.io/nrJ7pQn.png)

---

## License

Provided as-is for personal and research use. If you fork it, keep the Soyuz credit and replace the embedded credentials with your own.

---

*Last updated: November 2026*
