# Architecture Notes — Web + Streaming (Carbon Radio Example)

This doc is a lightweight map of how I structure my stack:

- web frontends (brands, studio, personal)
- streaming (Carbon Radio)
- edge / infra (Cloudflare)

It’s not meant to be a full spec — just enough to show how I think about pieces fitting together.

---

## High-Level Overview

At a high level, my world looks like this:

```text
User → Cloudflare (DNS + Edge) → Frontend (Cloudflare Pages)
                               ↘ Stream Origin (Radio stack)

