# Working with EasyWeb

Two ways to build and manage a site — **same desired-state files and same rules**.

| Path | When | How |
|------|------|-----|
| **Local Core Docker** | Platform development, bind-mount preview | Edit `Pages/` + `Themes/site` → live on `http://localhost:8080` (no `publish` needed) |
| **Site workspace + CLI** | Customer server / git site repo | `easyweb pull .` → edit `theme/` + `pages/` (+ datasets/forms/news/settings/images) → `validate` → `publish .` |

WebDAV containers and CLI sync use the **site workspace** layout (`theme/`, `pages/`, …). Core Docker uses the same content under `Themes/site` and `Pages/` via bind mounts.

## Desired-state layout

| Folder | Transport | Role |
|--------|-----------|------|
| `theme/` | WebDAV `/theme` | Liquid theme, CSS/JS, header/footer |
| `pages/` | WebDAV `/pages` | Page body HTML + `.meta.json` |
| `datasets/` | WebDAV `/datasets` | DataSet schemas + rows |
| `forms/` | WebDAV `/forms` | Form definitions |
| `news/` | WebDAV `/news` | Feeds + posts |
| `settings/` | WebDAV `/settings` (allowlist) + CMS API | Design/SEO/privacy/maps/redirects; `navigation.json` via API |
| `images/` | CMS API | Gallery binaries + `.meta.json` |
| `files/` | CMS API | Documents / PDFs |

Secrets (`smtp.json`, `form-security.json`, `remote-editing.json`) stay off default pull/publish — use `easyweb pull-secrets` / `push-secrets --yes`.

## Typical cycles

**Local Core:**

```text
edit Pages/ + Themes/site → refresh http://localhost:8080
```

**Remote / site repo:**

```bash
easyweb pull .
# edit theme/ pages/ datasets/ forms/ news/ settings/ images/
easyweb validate .
easyweb publish . --default-culture de
```

Full instance migrate: `easyweb backup export|import` (same modules as Admin → Sicherung).

## Shared building rules

- Page files are **body only** (no full HTML document).
- Official CMS blocks / Bootstrap; wrap editable text in `wf-editable` (one text node per wrapper).
- Liquid/HTML zones: `<!-- ew:zone type=liquid|html id=… -->` … `<!-- /ew:zone -->`.
- Never invent `data-gjs-*` attributes or custom form POST handlers.
- Never change theme markers: `ew-site-meta`, `ew-nav-brand`, `ew-asset:*`.

See [Building pages](building-pages.md), [Remote editing / WebDAV](remote-editing.md), and [CLI](cli.md).
