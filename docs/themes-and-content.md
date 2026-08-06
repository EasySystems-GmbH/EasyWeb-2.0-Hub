# Themes and content

EasyWeb 2.0 serves public HTML from **theme templates** and **page files** on disk (not from a legacy database `Pages` table for public render).

## Site workspace layout

Typical git / CLI workspace (what `easyweb publish` / `pull` expect):

```text
my-site/
  theme/              # layout, assets, inc/, optional {slug}.html templates  →  /theme
  pages/              # page HTML + .meta.json  →  /pages
  pages/de/           # optional culture subfolders
  datasets/           # optional  →  /datasets
  forms/              # optional  →  /forms
  news/               # optional  →  /news
  settings/           # allowlisted site JSON + navigation.json; secrets via pull-/push-secrets
  images/             # CMS gallery (pull . / push-images)
  files/              # documents (push-documents)
```

On the **server**, WebDAV `/theme` is the active instance theme (filesystem `Themes/site`). In the Core Docker repo the same content is bind-mounted as `Themes/site` — not as a folder named `theme/`. Full path table: [CLI — site workspace vs Core](cli.md#site-workspace-vs-core-repo).

Publish and pull: [CLI](cli.md). Push navigation only: `easyweb push-navigation .`.

**Do not edit `Themes/LegacyStarter` as the live theme** — it is only the system seed for `easyweb create-theme` and theme restore. Edit the instance theme (`theme/` in a site workspace, or `Themes/site` under Core Docker).

## Theme directory (`theme/`)

| Path | Role |
|------|------|
| `inc/_header.html`, `inc/_footer.html` | Shared chrome (keep CMS markers intact) |
| `blank.html`, `index.html` | Layout wrappers for page body from `pages/` |
| `{slug}.html` | Full-page template when no page body exists for that slug |
| `assets/` | CSS, JS, images at `/theme/assets/...` |

WebDAV path: `/theme`. Public static URL: `/theme/...`.

## Pages directory (`pages/`)

| File | Content |
|------|---------|
| `{culture}/{slug}.html` | Page body HTML |
| `{culture}/{slug}.meta.json` | Title, SEO title, SEO description, **sliders** |

Example `.meta.json` (camelCase keys):

```json
{
  "title": "Gallery",
  "seoTitle": "Gallery",
  "seoDescription": "Our work",
  "listingDescription": "",
  "previewImage": "",
  "listingButtonText": "",
  "contentCss": null,
  "templateName": null,
  "isNewsDetailPage": false,
  "isPublished": true,
  "sliders": {
    "main": {
      "images": [
        { "path": "portfolio/slide-1.jpg", "title": "Slide 1", "altText": "Description" }
      ]
    }
  }
}
```

## How a URL is rendered

Public routes: `/{culture}/{slug}` or `/{slug}` (single culture).

1. If **`pages/.../{slug}.html`** has content → body is wrapped in `blank.html` or `index.html`.
2. Else if **`theme/{slug}.html`** exists → full theme template, empty page body (sliders still load from `.meta.json` if the CMS page exists).
3. Else **404**.

Avoid duplicating the same slug as both `theme/{slug}.html` and `pages/{slug}.html`; `easyweb validate .` warns about this.

## Liquid / Fluid templates

EasyWeb uses **Fluid** (Liquid-compatible syntax) in theme HTML.

### Common variables

| Variable | Description |
|----------|-------------|
| `{{ current_theme.path }}` | Theme asset root (e.g. `/theme`) |
| `{{ current_page.seo.title }}` | SEO title |
| `{{ current_page.seo.description }}` | Meta description |
| `navigations.main.links` | Main nav (`Title`, `Url`) |
| `current_page.sliders.main.images` | Slider images (see below) |

### Navigation loop

```liquid
{% for link in navigations.main.links %}
  <a href="{{ link.Url }}">{{ link.Title }}</a>
{% endfor %}
```

### Page sliders (Liquid)

Define a slider region in your theme template:

```liquid
<section class="ew-slider">
  {% for image in current_page.sliders.main.images %}
  <figure>
    <img src="{{ image.url }}" alt="{{ image.title }}">
  </figure>
  {% endfor %}
</section>
```

Each `image` provides:

| Field | Example |
|-------|---------|
| `url` | `/images/portfolio/hero.jpg` |
| `title` | Display title |
| `alt` | Alt text (falls back to title) |

**CMS workflow**

1. Upload images in **Image Gallery**.
2. Add `theme/gallery.html` (or any template) with the loop above.
3. Create a CMS page with slug `gallery` (body can be empty).
4. In **Page sliders**, add images from the gallery and drag to reorder.
5. Save — public page at `/gallery` (or `/de/gallery` with cultures).

Example in the core repo: `Themes/BasicDemoTheme/gallery.html`.

Slider name `main` matches `current_page.sliders.**main**.images`. Additional slider names can be added in metadata; the admin UI defaults to `main`.

## Editable regions (`wf-editable`)

Mark blocks that authors edit in the CMS page editor (GrapesJS visual canvas):

```html
<section class="wf-editable">
  <h1>Welcome</h1>
</section>
```

At render time, `wf-*` classes are mapped to `data-wf-*` for editor tooling.

## Cultures

When multiple cultures are configured (`Localization:SupportedCultures`), use `pages/de/home.html`, etc. Publish with culture mirroring:

```bash
easyweb publish . --default-culture de
```

## Caching

Theme static files use short browser cache headers so updates appear after publish. Sites behind **Cloudflare** (e.g. ESYS Hosting) may need edge purge or cache-busting query strings on CSS/JS (`main.css?v=2`).

## Related docs

- [CMS admin](cms-admin.md) — hybrid page editor (GrapesJS + Monaco) and slider UI
- [CMS permissions](cms-permissions.md)
- [CLI](cli.md) — publish, pull, validate
- [Core: legacy theme compatibility](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/legacy-theme-compatibility.md) — full `wf-*` and fixture notes
