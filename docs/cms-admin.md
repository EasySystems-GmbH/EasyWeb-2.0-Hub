# CMS admin

EasyWeb 2.0 includes a browser-based CMS at `/admin` for managing site content without editing files directly.

Sign in with your **admin email** and **password** (seeded on first run in Docker; on ESYS Hosting, use the credentials from the stack deploy form).

## Admin areas

| Area | Path | Purpose |
|------|------|---------|
| **Pages** | `/admin` | Create and edit pages, WYSIWYG body, SEO fields, page sliders |
| **Navigation** | `/admin/navigation` | Main menu links (`navigations.main.links` in themes) |
| **Image Gallery** | `/admin/files` | Upload and organize images (`/images/...` on the public site) |
| **Documents** | `/admin/documents` | PDFs and other document files |
| **Users** | `/admin/users` | Accounts, roles, and per-feature permissions |
| **Settings** | `/admin/settings` | Remote editing (WebDAV) username and password |

Sidebar links are hidden when the signed-in user lacks the required permission. See [CMS permissions](cms-permissions.md).

## Pages

### List and editor

- Filter by language when multiple cultures are configured.
- Search by title or slug.
- **New page** opens the editor immediately.
- **Preview** opens the public URL for the current slug and culture.

Each page is stored on disk as:

- `pages/{culture}/{slug}.html` — page body HTML
- `pages/{culture}/{slug}.meta.json` — title, SEO fields, and slider data

For a single default culture, files may also live as `pages/{slug}.html` at the root of the pages folder.

### Visual editor (WYSIWYG)

Page body content uses a **TinyMCE** visual editor:

- Formatting: headings, bold, lists, links, tables, images
- **Code** button for raw HTML when needed
- New or empty content is wrapped in `<section class="wf-editable">` for theme compatibility

Users with **view-only** page permission see the editor in read-only mode; Save and Delete are disabled.

### Page sliders

For theme templates that use Liquid image sliders, manage slides on the same page editor:

1. Open **Page sliders** (slider name `main` by default).
2. Click **Add from gallery** and pick images already uploaded in **Image Gallery**.
3. **Drag** slides by the ⠿ handle to reorder.
4. **Save page** — order is stored in `.meta.json` and exposed to Liquid as `current_page.sliders.main.images`.

See [Themes and content](themes-and-content.md#page-sliders-liquid) for the theme markup.

### Templates

Load a starter layout from the theme (`theme/{name}.html` files) into the page body without overwriting an existing title or slug.

## Image Gallery

Upload images into folders under the CMS media library. Files are served publicly at `/images/{path}`.

- Set **title**, **alt text**, and **description** per image (used in sliders and SEO).
- Use folders to organize assets (e.g. `portfolio/`, `team/`).
- Page sliders and theme HTML reference paths like `/images/portfolio/hero.jpg`.

After uploading in the CMS, run `easyweb pull .` locally to sync `images/` into your git workspace. See [CLI](cli.md).

## Navigation

Edit the main navigation tree. Links are rendered in themes via:

```liquid
{% for link in navigations.main.links %}
  <a href="{{ link.Url }}">{{ link.Title }}</a>
{% endfor %}
```

Navigation is stored in the database. Export to `navigation/main.json` with `easyweb pull .` (pull-only; push from CLI is not implemented yet).

## Documents

Manage non-image files (PDF, Office, archives) separately from the image gallery. Document permissions are independent from image gallery permissions.

## Sync with git / CLI

| Direction | Command | What moves |
|-----------|---------|------------|
| Local → server | `easyweb publish .` | `theme/`, `pages/` via WebDAV |
| Server → local | `easyweb pull .` or `easyweb sync .` | `theme/`, `pages/`, `navigation/main.json`, `images/` |

CMS credentials (`EASYWEB_ADMIN_EMAIL`, `EASYWEB_ADMIN_PASSWORD`) are required for pull of navigation and images. WebDAV credentials sync theme and pages only.

## Related docs

- [CMS permissions](cms-permissions.md)
- [Themes and content](themes-and-content.md)
- [CLI](cli.md)
- [VS Code extension](vscode-extension.md)
