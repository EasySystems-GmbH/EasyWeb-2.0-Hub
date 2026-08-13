# Building pages (CMS-first)

Public guide for site builders (humans, Cursor, and the `easyweb` CLI).  
Workflow overview: [Working with EasyWeb](working-with-easyweb.md).

## Hard rules

- Page files are **body only** (no `<html>` / `<head>` wrapper).
- Build **only with official CMS blocks** so everything stays editable in the page editor.
- **1:1 design parity:** when rebuilding a reference page, do not lose spacing, sizes, or hierarchy (“approximate” is forbidden).
- Wrap editable text in `wf-editable` — **one text node per wrapper** (heading and paragraph = two wrappers). Class is `wf-editable`, **never** `data-wf-editable`.
- Columns block = `row g-3 ew-layout-row` + `col-*` — never bare `row`/`col`.
- Liquid / HTML code blocks: `<!-- ew:zone type=liquid|html id=… -->` … `<!-- /ew:zone -->`.
- Raw HTML only via **Code (HTML)** zone — absolute last resort.
- Never invent `data-gjs-*` attributes.
- Never change theme CMS markers: `ew-site-meta`, `ew-nav-brand`, `ew-asset:*`.
- Never invent custom `<form action="…">` — use CMS forms (`ew-form-container` / `easyweb snippet form`).
- **Feature-Box** (Bootstrap `card`) ≠ **Map** (`ew-map`, only when Settings → Maps is enabled).

## Design priority (strict)

```text
CMS blocks (structure)
  → Settings → Design (site-theme → main.css: button color, typography, global)
  → Block style panel → contentCss (#ew-e-…: margin/padding, size, border, shadow)
  → custom.css (only if panel + Design tab cannot do it — keep block structure)
  → Code (HTML) zone (absolute emergency)
```

### Forbidden

- Extra wrapper `<div>`s only to add padding/margin/max-width
- Inline `style` for spacing (use the style panel → `contentCss`)
- Manual `ew-section-container` in saved page HTML (editor-only)
- Deleting columns/sections and recreating layout in `custom.css`
- “Tons of divs” so the layout can be styled

Example: old `<div style="padding:15px">…</div>` → CMS block + **15px padding** on that block in the style panel.

## Workspace

Site repo after `easyweb pull .`:

```text
theme/  pages/  datasets/  forms/  news/  settings/  images/  files/
```

Core Docker bind-mount uses `Themes/site` + `Pages/` — same content, different folder names. WebDAV paths are always `/theme` and `/pages`.

Navigation supports nested links via `parentLinkId` in `settings/navigation.json` (`easyweb pull-navigation` / `push-navigation`).

## Snippets (editor helpers)

```bash
easyweb snippet dataset <key>
easyweb snippet news <feedKey>
easyweb snippet form <key> [--culture de]
```

Requires `EASYWEB_API_TOKEN` or `EASYWEB_ADMIN_*`.

## Block markup cheat sheet

See [cms-blocks-reference.html](cms-blocks-reference.html) for copy-paste markup.

### Essentials

```html
<section class="wf-editable" data-ew-container="container">
  <div class="container">
    <div class="wf-editable"><h2>Title</h2></div>
    <div class="wf-editable"><p>Body text.</p></div>
    <div class="row g-3 ew-layout-row">
      <div class="col-md-6"><div class="wf-editable"><p>Column</p></div></div>
      <div class="col-md-6"><div class="wf-editable"><p>Column</p></div></div>
    </div>
    <a href="/de/kontakt" class="btn btn-primary ew-btn">Contact</a>
  </div>
</section>

<!-- Feature-Box (not Map) — one text only -->
<div class="card wf-editable">
  <div class="card-body">
    <div class="wf-editable"><p class="card-text mb-0">Feature text</p></div>
  </div>
</div>

<!-- ew:zone type=html id=emergency-only -->
<!-- raw HTML only if no CMS block + CSS can do it -->
<!-- /ew:zone -->
```

## Meta

Each page needs a sibling `.meta.json` (`isPublished`, SEO title/description, optional sliders, `contentCss`). Publish with:

```bash
easyweb publish . --default-culture de
```

## Related Hub docs

- [CLI](cli.md)
- [Remote editing](remote-editing.md)
- [WebDAV and CLI routes](webdav-and-cli.md)
- [Themes and content](themes-and-content.md)
- [CMS admin](cms-admin.md)
