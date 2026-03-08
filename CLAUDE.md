# Wooly Lully Proto — Claude Development Notes

## What This Is

Clickable prototype for the Wooly Lully e-commerce store — a Swiss cashmere & merino knitwear brand based in Zurich. This is a **static HTML/CSS/JS** site deployed via GitHub Pages. No build step, no framework, no bundler.

**Live prototype:** https://mbevilac.github.io/wooly-lully-proto/
**Repo:** mbevilac/wooly-lully-proto (private)
**Primary branch:** main
**Deploys:** Pushing to `main` triggers GitHub Pages deploy via `.github/workflows/deploy.yml`

## Relationship to wooly-lully

This repo is the **design prototype**. The main `mbevilac/wooly-lully` repo holds the production project: requirements docs, product data (JSON + WooCommerce CSV), legal content, deployment scripts, and GitHub issue tracking (72 issues across 7 milestones). The prototype was created as part of Phase 0 (Issue #82 in wooly-lully).

## File Structure

```
wooly-lully-proto/
├── index.html          # Prototype landing page (flow overview + UX audit notes)
├── homepage.html       # Homepage: hero, featured products (4), mission block, newsletter
├── shop.html           # Shop: 12-product grid, 6 filter dimensions, sort, mobile filter drawer
├── pdp.html            # Product Detail Page: specs, size selector, fit labels, gallery, cart, cross-sell
├── checkout.html       # One-page checkout: express pay, contact, delivery, payment, confirmation
├── about.html          # About page: mission, vision, materials education
├── app.js              # Shared JS — product data, cart (localStorage), filters, drawers, all interactions
├── style.css           # Shared CSS — design tokens, full responsive styles (~1800 lines)
├── imgs/
│   ├── products/       # 16 product images (AI-generated PNGs)
│   ├── lifestyle/      # 3 lifestyle images
│   └── size-guide/     # 6 mannequin images (XS–XXL)
└── .github/workflows/
    └── deploy.yml      # GitHub Pages deployment (on push to main)
```

## Key Architecture Decisions

- **No build step** — plain HTML/CSS/JS, directly served by GitHub Pages
- **Single JS file** (`app.js`) — all logic in one ~800-line file, initialised via `DOMContentLoaded`
- **Cart state** — `localStorage` key `wl_proto_cart`, JSON array of items
- **Product switching on PDP** — `pdp.html?product=slug` reads URL params, swaps content from `PRODUCTS` object in `app.js`
- **Only 4 products fully wired** in `app.js` PRODUCTS object: `navy-crewneck`, `oatmeal-merino`, `cream-scarf`, `black-merino`. Other 8 products link to PDP but fall back to navy-crewneck data.
- **Fonts** — Google Fonts: Playfair Display (serif headings) + Inter (sans body)

## Design Tokens (CSS custom properties in `:root`)

| Token | Value | Usage |
|-------|-------|-------|
| `--bg` | `#FAFAF8` | Page background |
| `--bg-beige` | `#F5F2EC` | Card/section backgrounds |
| `--gold` | `#D4A574` | Accent colour, CTAs highlights |
| `--text` | `#1A1A1A` | Primary text |
| `--text-muted` | `#6B6B6B` | Secondary text |
| `--font-serif` | Playfair Display | Headings |
| `--font-sans` | Inter | Body text |

## Product Catalog (12 products)

| # | Product | Price | Material | Fit | Style |
|---|---------|-------|----------|-----|-------|
| 1 | Navy Cashmere Crewneck | CHF 285 | 100% cashmere | Classic | Crewneck |
| 2 | Cream Cashmere Crewneck | CHF 285 | 100% cashmere | Classic | Crewneck |
| 3 | Burgundy Cashmere V-Neck | CHF 295 | 100% cashmere | Relaxed | V-Neck |
| 4 | Camel Cashmere V-Neck | CHF 295 | 100% cashmere | Relaxed | V-Neck |
| 5 | Charcoal Cashmere Turtleneck | CHF 310 | 100% cashmere | Slim | Turtleneck |
| 6 | Ivory Cashmere Turtleneck | CHF 310 | 100% cashmere | Slim | Turtleneck |
| 7 | Grey Cashmere Cardigan | CHF 350 | 100% cashmere | Classic | Cardigan |
| 8 | Navy Cashmere Cardigan | CHF 350 | 100% cashmere | Classic | Cardigan |
| 9 | Oatmeal Merino Crewneck | CHF 195 | 80/20 merino-cashmere | Classic | Crewneck |
| 10 | Black Merino V-Neck | CHF 195 | 80/20 merino-cashmere | Classic | V-Neck |
| 11 | Cream Cashmere Scarf | CHF 180 | 100% cashmere | One Size | Scarf |
| 12 | Grey Cashmere Beanie | CHF 185 | 100% cashmere | One Size | Beanie |

## UX Differentiators (from competitive audit)

These are the 10 UX recommendations implemented in this prototype, based on analysis of Lunu.ch, GOBI EU, and Laal.ch:

- **R1** Material specs block visible on every PDP (composition, micron, gsm, origin)
- **R2** Fit labels per size (Classic / Relaxed / Slim) replacing traditional size charts
- **R3** Return policy prominent on PDP and in cart drawer
- **R4** Slide-in cart drawer (no page navigation)
- **R5** CHF-native pricing + Swiss identity (Swiss Post, Zurich address)
- **R6** Live AJAX-style filter system (6 dimensions, no page reload)
- **R7** Composition visible on product grid cards
- **R8** Sticky ATC bar on mobile PDP
- **R9** Curated cross-sell ("Also pairs well with")
- **R10** One-page checkout with express pay at top

## Business Constants

- **Free shipping threshold:** CHF 300
- **Flat shipping rate:** CHF 9 (Swiss Post Priority, 2-3 business days)
- **Return window:** 14 days
- **VAT rate:** 8.1% (Swiss standard rate from Jan 2024, included in price)
- **Currency:** CHF only

## Authentication

Same as wooly-lully — see that repo's CLAUDE.md for full details.

### Quick reference:
- **`gh` CLI:** Requires `HTTPS_PROXY="$GLOBAL_AGENT_HTTP_PROXY"` for GitHub API access
- **`git push/pull`:** Uses session-managed proxy on origin remote (auto-configured)

## Workflow

- **Branch pattern:** `claude/<description>-<session-suffix>`
- **PRs:** Create via `gh pr create --repo mbevilac/wooly-lully-proto`
- **No tests or linters** — this is a static prototype, no CI checks beyond deploy
