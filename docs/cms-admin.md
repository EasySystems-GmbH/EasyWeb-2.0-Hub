# CMS admin

EasyWeb 2.0 includes a browser-based CMS at `/admin` for managing site content without editing files directly.

Sign in with your **admin email** and **password** (seeded on first run in Docker; on ESYS Hosting, use the credentials from the stack deploy form).

Local Docker: open `http://localhost:8080/admin` (public site also on **8080**; WebDAV / `easyweb` CLI default to **5055**).

## Admin areas

| Area | Path | Purpose |
|------|------|---------|
| **Pages** | `/admin` | Create and edit pages (hybrid editor), SEO fields, templates, page sliders, page popup |
| **DataSets** | `/admin/datasets` | Structured data (schemas + culture rows) for Liquid blocks |
| **Image Gallery** | `/admin/files` | Upload and organize images (`/images/...` on the public site) |
| **Documents** | `/admin/documents` | PDFs and other document files (`/files/...`) |
| **News** | `/admin/news` | News feeds and posts |
| **Forms** | `/admin/forms` | Form definitions and submissions |
| **Navigation** | `/admin/navigation` | Main menu links (`navigations.main.links` in themes) |
| **Theme** | `/admin/theme` | Layout, header/footer, custom CSS/JS, live preview |
| **Users** | `/admin/users` | Accounts, role presets, and per-feature permissions |
| **Settings** | `/admin/settings` | Design, Meta, E-Mail, Languages, Assets, Maps, Cookies, Redirects, Backup, WebDAV |
| **Help** | `/admin/help` | In-product customer guide (DE/EN/FR/IT) |

Sidebar links are hidden when the signed-in user lacks the required permission. See [CMS permissions](cms-permissions.md).

## Pages

### List and editor

- Filter by language when multiple cultures are configured.
- Search by title or slug.
- **New page** opens the editor immediately.
- **Preview** opens the public URL for the current slug and culture.

Each page is stored on disk as:

- `pages/{culture}/{slug}.html` — page body HTML
- `pages/{culture}/{slug}.meta.json` — title, SEO fields, slider data, `contentCss` (camelCase keys)

For a single default culture, files may also live as `pages/{slug}.html` at the root of the pages folder.

### Hybrid page editor (GrapesJS + Monaco)

Page body content uses a **hybrid editor**:

| Tab | Role |
|-----|------|
| **Visual** | GrapesJS canvas — drag blocks, edit text, styles, traits |
| **Code** | Monaco — raw HTML / Liquid zones |
| **Preview** | Public rendering, including responsive breakpoints |

Common blocks: Section, Columns, Text, List, Image, Button, Slider, Gallery, FAQ, Downloads, PDF, YouTube, Divider, Map (when Maps is enabled), Form, DataSet, News, Page folder, Code (Liquid/HTML).

- New or empty content is wrapped in `<section class="wf-editable">` for theme compatibility.
- Toolbar **Popup** configures a page-level modal (`<!-- ew:page-popup -->`), not a canvas block.
- Users with **view-only** page permission see the editor in read-only mode; Save and Delete are disabled.

### Page sliders

For pages that use the Slider block (or theme Liquid sliders), manage slides on the same page editor:

1. Open **Page sliders** (slider name `main` by default), or configure images on the Slider block.
2. Click **Add from gallery** and pick images already uploaded in **Image Gallery**.
3. **Drag** slides by the ⠿ handle to reorder.
4. **Save page** — order is stored in `.meta.json` and exposed to Liquid as `current_page.sliders.main.images`.

See [Themes and content](themes-and-content.md#page-sliders-liquid) for the theme markup.

### Templates

Load a starter layout from the theme (`theme/{name}.html` files) into the page body without overwriting an existing title or slug.

## Theme

Edit shared layout under **Theme** (`/admin/theme`): header/footer, custom CSS/JS, and live preview. The active instance theme is the site theme root (Docker: `Themes/site`). Prefer restoring the standard theme from the UI rather than hand-editing CMS markers (`ew-site-meta`, `ew-nav-brand`, `ew-asset:*`).

## DataSets, News, Forms

| Area | Use |
|------|-----|
| **DataSets** | Define fields and culture-specific rows; insert via Liquid blocks on pages |
| **News** | Feeds and posts; list/slider snippets on pages |
| **Forms** | Build forms in the admin; drop a Form block on a page (no custom POST handlers) |

## Image Gallery

Upload images into folders under the CMS media library. Files are served publicly at `/images/{path}`.

- Set **title**, **alt text**, and **description** per image (used in sliders and SEO).
- Use folders to organize assets (e.g. `portfolio/`, `team/`).
- Page sliders and theme HTML reference paths like `/images/portfolio/hero.jpg`.

After uploading in the CMS, run `easyweb pull .` locally to sync `images/` into your git workspace. See [CLI](cli.md).

## Navigation

Edit the main navigation tree in the CMS, or manage it in git via `settings/navigation.json`.

Links are rendered in themes via:

```liquid
{% for link in navigations.main.links %}
  <a href="{{ link.Url }}">{{ link.Title }}</a>
{% endfor %}
```

### Git / CLI workflow

| Step | Command |
|------|---------|
| Download current menu | `easyweb pull-navigation .` → writes `settings/navigation.json` |
| Edit in your repo | Change `title`, `url`, or reorder the `links` array |
| Upload to CMS | `easyweb push-navigation .` or `easyweb publish .` |

`easyweb pull .` also downloads navigation into `settings/` (with theme, pages, and images).

Array order is the menu order. After push, the CLI refreshes `id` fields in `navigation.json` so the next push can update the same links.

Requires **Navigation → Edit** permission. See [CLI — push navigation](cli.md#publish-and-pull-workspace).

## Documents

Manage non-image files (PDF, Office, archives) separately from the image gallery. Document permissions are independent from image gallery permissions. Public URLs use `/files/...` (PDF flipbook, downloads).

## Settings

| Tab | Purpose |
|-----|---------|
| **Design** | Logo, fonts, brand and navigation colors, button styles |
| **Meta** | Site name, SEO templates, favicon, Open Graph, structured data, optional `llms.txt` |
| **E-Mail** | Outgoing mail for forms and system messages |
| **Languages** | Supported cultures |
| **Assets** | Extra site assets |
| **Maps** | Enable Google Maps; unlocks the Map block in the page editor |
| **Cookies** | Cookie banner / consent categories |
| **Redirects** | URL redirects |
| **Backup** | Full site export/import (and legacy theme import where available) |
| **WebDAV** | Remote-editing username/password; live-cache clear |

## Sync with git / CLI

| Direction | Command | What moves |
|-----------|---------|------------|
| Local → server | `easyweb publish .` | `theme/`, `pages/`, and when present `datasets/`, `forms/`, `news/` via WebDAV; `settings/navigation.json` via CMS API |
| Server → local | `easyweb pull .` or `easyweb sync .` | same WebDAV roots + `settings/navigation.json` + `images/` |

CMS credentials (`EASYWEB_ADMIN_EMAIL`, `EASYWEB_ADMIN_PASSWORD`) are required for navigation and images. WebDAV credentials sync theme, pages, datasets, forms, and news. Full matrix: [CLI — sync matrix](cli.md#sync-matrix-canonical).

## Related docs

- [CMS permissions](cms-permissions.md)
- [Themes and content](themes-and-content.md)
- [CLI](cli.md)
- [VS Code extension](vscode-extension.md)
- [Page building / CMS blocks](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/cursor-page-building.md) (core repo)
- [WebDAV ↔ CMS](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/webdav-cms-compatibility.md) (core repo)
