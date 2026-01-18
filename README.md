# Public Sample — Infra & Streaming Notes

This repo is intentionally minimal.

Most of my real work (brands, radio, studios, product infra) sits in **private repositories** for IP reasons. This public repo exists to show how I think about:

- choosing infrastructure (why Cloudflare)
- designing and operating a streaming stack (Carbon Radio)
- keeping things simple, resilient, and maintainable

There is no "app" here on purpose — this is about architecture and judgment, not copy-pasteable code.

---

## Why Cloudflare for My Stack

I moved my projects (brands, studio sites, and radio frontend) onto **Cloudflare + Cloudflare Pages** for a few reasons:

### 1. DNS + SSL + CDN in one place

I want:

- one control plane for **DNS**
- automatic **HTTPS** (no cert drama)
- global **CDN caching** out of the box

Cloudflare gives me:

- DNS at the edge
- universal SSL
- smart caching
- built-in firewall / DDoS handling

That means I don’t have to duct-tape:
registrar DNS → random hosting → separate CDN → manual cert renewals.

### 2. Static frontends, private repos

Most of my sites are:

- static or very light front-ends
- built locally, deployed via Git
- served from Cloudflare Pages

Cloudflare Pages gives me:

- Git-based deploys
- preview URLs for each change
- zero-maintenance infra for static sites

Behind that, **repos are private**. The live sites are public; the implementation details are not. That’s intentional: branding, layout, routing logic, and some of the infra patterns are part of my IP.

### 3. One edge for everything

I’d rather have:

- one edge layer (Cloudflare)
- one place for redirects, cache rules, and headers
- one mental model for “what happens when someone hits my domain”

Cloudflare lets me:

- enforce canonical domains (e.g. `https://domain.com` only)
- handle HTTP → HTTPS redirects cleanly
- protect everything behind the same WAF and caching layer

As I add more projects, the complexity doesn’t explode — they all live behind the same system.

---

## How I Think About Streaming (Carbon Radio)

Carbon Radio is my streaming project: a pirate-radio-meets-curated-station vibe. I keep the full stack private, but the way I think about it is simple:

### 1. Separate *stream* from *site*

There are two distinct pieces:

1. **The audio stream**  
   - source (playlists, shows, live input)
   - streaming engine (e.g. Liquidsoap/Icecast/Shoutcast-style stack)
   - distribution (stream URL, relays, directories like TuneIn, etc.)

2. **The frontend / website**  
   - brand, schedule, show notes
   - “play” entry point (embedded player or external link)
   - info for listeners and partners

The **stream stack** is infrastructure.  
The **site** is presentation and brand.

I keep those decoupled on purpose:

- stream can move providers or ports without breaking the brand
- site can redeploy, replatform, or redesign without touching the audio core

### 2. Design for resilience, not just “it works”

Streaming is fragile if you’re sloppy.

My mental checklist:

- **Fallbacks:** What happens if a playlist is empty? If a file is bad? If a source dies mid-track?
- **Silence:** How do I avoid dead air? Is there a safety playlist or default bed?
- **Monitoring:** How do I know if the stream is up without sitting there listening 24/7?
- **Latency:** Directory services (like TuneIn) and some players cache state — don’t assume instant updates.

Carbon’s stack is built so that:

- the stream has a stable, boring core
- experiments happen at the edges (playlists, rotations, imaging)
- listeners never see the hacky parts

### 3. Use the web stack as a “control room display”

The frontend is not just a billboard; it’s also:

- the “public face” of the station
- a place to surface:
  - what’s playing / what kind of vibe
  - when certain shows/blocks run
  - how to reach or support the project

Even if the backend is radios + scripts + streaming daemons, the **web layer** is where you turn that into a coherent experience.

So I care about:

- fast load
- simple, clear layout
- predictable URLs
- zero downtime (hence Cloudflare)

### 4. Keep the IP, share the concepts

The exact stack behind Carbon Radio stays private:

- specific configs
- playlists
- routing logic
- integration quirks
- scripting details

What I *am* comfortable sharing are:

- how I decompose the system
- how I keep it resilient
- why I avoid coupling the stream and the site
- why I run everything behind a single edge (Cloudflare)

If someone wants to evaluate my technical thinking, they don’t need my radio station’s source code — they need to see how I structure tradeoffs, not copy-paste my infrastructure.

---

## How This Maps to Work / Evaluation

If you’re reading this as part of a hiring or evaluation process:

- Most of my **real projects** (brands, products, media) are in **private repos**.
- I’m happy to:
  - walk through architectures
  - explain design choices
  - discuss tradeoffs
  - design new systems from scratch

But for live, operating projects like Carbon Radio, I treat the code and detailed configs as IP.

This repo exists to share **how I think**, not to open-source my studio.

---

## Contact

- Personal site: https://jfbaker.com  
- Radio project: https://carbonradio.live  
- Email: jfbeach@pm.me
