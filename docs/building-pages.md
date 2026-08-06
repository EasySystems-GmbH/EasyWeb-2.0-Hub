# Building pages (CMS-first)

Public guide for site builders (humans, Cursor, and the `easyweb` CLI).  
Workflow overview: [Working with EasyWeb](working-with-easyweb.md).

## Hard rules

- Page files are **body only** (no `<html>` / `<head>` wrapper).
- Prefer official CMS blocks and Bootstrap layout over custom HTML/JS.
- Wrap editable text in `wf-editable` — **one text block per wrapper** (heading and paragraph = two wrappers).
- Liquid / HTML code blocks: `<!-- ew:zone type=liquid|html id=… -->` … `<!-- /ew:zone -->`.
- Never invent `data-gjs-*` attributes.
- Never change theme CMS markers: `ew-site-meta`, `ew-nav-brand`, `ew-asset:*`.
- Never invent custom `<form action="…">` — use CMS forms (`ew-form-container` / `easyweb snippet form`).
- **Karte** (Bootstrap `card`) ≠ **Map** (`ew-map`, only when Settings → Maps is enabled).

## Workspace

Site repo after `easyweb pull .`:

```text
theme/  pages/  datasets/  forms/  news/  settings/  images/  files/
```

Core Docker bind-mount uses `Themes/site` + `Pages/` — same content, different folder names. WebDAV paths are always `/theme` and `/pages`.

## Snippets (editor helpers)

```bash
easyweb snippet dataset <key>
easyweb snippet news <feedKey>
easyweb snippet form <key> [--culture de]
```

Requires `EASYWEB_API_TOKEN` or `EASYWEB_ADMIN_*`.

## Block markup cheat sheet

See [cms-blocks-reference.html](cms-blocks-reference.html) for copy-paste markup (section, text, list, image, button, card, alert, columns, slider, gallery, form, map, FAQ, popup, Liquid zone).

### Essentials

```html
<div class="wf-editable"><h2>Title</h2></div>
<div class="wf-editable"><p>Body text.</p></div>

<a href="/de/kontakt" class="btn btn-primary ew-btn">Contact</a>

<section class="wf-editable" data-ew-container="container">
  <div class="container">…</div>
</section>

<!-- ew:zone type=liquid id=my-block -->
{% comment %} Liquid here {% endcomment %}
<!-- /ew:zone -->
```

## Meta

Each page needs a sibling `.meta.json` (`isPublished`, SEO title/description, optional sliders). Publish with:

```bash
easyweb publish . --default-culture de
```

## Related Hub docs

- [CLI](cli.md)
- [Remote editing](remote-editing.md)
- [WebDAV and CLI routes](webdav-and-cli.md)
- [Themes and content](themes-and-content.md)
- [CMS admin](cms-admin.md)
