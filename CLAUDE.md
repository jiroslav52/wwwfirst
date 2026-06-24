# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project type

Static HTML/CSS site — a Russian-language plant shop ("PLANT"). No build step, no package manager, no tests. Pages are opened directly in a browser; the entry point is [code/index.html](code/index.html).

Only [code/search.html](code/search.html) contains a small inline `<script>` for client-side filtering of products by name/category. Everything else is HTML + CSS.

## Folder layout

Three flat folders at the root — never mix asset types:

- `code/` — all HTML files and `main.css`
- `images/` — all raster assets (`.jpg`, `.png`, `.ico`)
- `icons/` — all SVG icons

## Asset path conventions

HTML lives in `code/`, so every reference reaches across folders:

- Photos / favicon: `src="../images/<filename>"`
- Icons: `src="../icons/<filename>"`
- Stylesheet: `href="main.css"` (same folder — no prefix)
- Cross-page links: `href="product.html"` (same folder — no prefix)

If you create a new asset and forget the `../images/` or `../icons/` prefix, the image will 404 silently.

## Page inventory

All HTML pages share the same `<header>` and `<footer>` markup — when you edit nav/footer, propagate the change to every file. Pages:

- `index.html` — home (hero + categories + "Предложения месяца" offers grid)
- `catalog.html` — full catalog grid (4 cards per row via `display: grid`)
- `care.html` — "Советы по уходу" (care tips grouped by plant type)
- `payment.html` — "Оплата и доставка" (payment methods, delivery, returns)
- `search.html` — client-side product search; cards carry `data-name` and `data-category` for filtering by the inline `<script>` at the bottom of the file
- `product.html`, `product-fikus-1.html`, `product-fikus-2.html`, `product-aloe.html` — product detail pages, all built from the `product.html` template
- `trash.html` — empty cart state

The header `<a class="header-button" aria-label="Контакты">Контакты</a>` link points to `#footer` — the footer carries `id="footer"` so the in-page anchor lands at the bottom contact block. Profile icon currently goes nowhere (`href="#"`) — there is no profile page yet.

To add a new product page: copy `code/product.html`, swap image src, category, title, description, features, price, and `<title>`. Then add it as a clickable card to the relevant grid (catalog.html, index.html offers, and search.html — remember to add a `data-name` / `data-category` on the search.html card or it will never appear in results).

## CSS conventions

Single stylesheet [code/main.css](code/main.css) — no preprocessors, no modules. Class naming is BEM-ish with hyphens (`block-element-modifier`). The stylesheet has roughly three regions concatenated end-to-end: original component styles → animations/transitions section → info-page and search-page styles. New CSS should usually be appended at the end.

Quirks worth knowing before editing:

- **Numbered variant classes** like `offer-card-title1`, `offer-card-category1`, `current-price-catalog1` are intentional — they're layout variants for cards that have different content (e.g., with/without an old-price strikethrough). Don't dedupe them into one rule without checking every usage.
- **Typo-tolerant selectors** — some HTML class names have typos (`catalog-card-mage`, `catalog-card-contnet`) and the CSS targets both the correct and typo names with comma selectors. If you fix a typo in HTML, also remove the matching fallback selector in CSS.
- The catalog grid is `display: grid; grid-template-columns: repeat(4, 1fr); gap: 30px` on `.list-catalog` — that's how cards lay out in 4 columns.
- `.current-price-catalog` / `.current-price-catalog1` use `padding-left: 220px / 270px` as a sliding-shift hack to push the price right. This breaks at narrow viewports — prefer `justify-content: space-between` on the parent `.catalog-card-price` when reworking pricing.
- Hover animations are concentrated in the `===== Animations & hover transitions =====` section near the end of main.css. Cards lift (`translateY(-6px)`) and their inner images zoom (`scale(1.04)`) on hover via `parent:hover .child` selectors — keep that parent-child pairing intact when restructuring markup.

## Language

All UI copy is in Russian. Keep new strings, alt text, and labels in Russian unless asked otherwise.
