# EasyWeb CLI

The EasyWeb CLI is published from the core `EasyWeb 2.0` repository and attached to Hub releases as a `.tgz` package.

**Live documentation:** [EasyWeb 2.0 Hub](https://easysystems-gmbh.github.io/EasyWeb-2.0-Hub/) — install guides, CMS docs, and this CLI reference.

After install, `easyweb help` and `easyweb version` print the hub URL so you can open the docs from the terminal.

## Download

- Tagged releases (recommended):  
  [EasyWeb-2.0-Hub releases](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Hub/releases)
- CI runs (non-tag builds):  
  [Extension + CLI packaging workflow runs](https://github.com/EasySystems-GmbH/EasyWeb-2.0/actions/workflows/vscode-extension-package.yml)

Download the package named like `easyweb-remote-<version>.tgz`.

## Install (macOS, Linux, Windows)

Requirements:

- Node.js 20+
- npm

### Homebrew (macOS, Linux)

```bash
brew tap EasySystems-GmbH/easyweb https://github.com/EasySystems-GmbH/EasyWeb-2.0-Homebrew
brew install easyweb
```

The formula is published to the public Homebrew repo and updated automatically by EasyWeb 2.0 CI on each release.

If you tapped EasyWeb previously from a different source, reset once:

```bash
brew untap easysystems-gmbh/easyweb
brew tap EasySystems-GmbH/easyweb https://github.com/EasySystems-GmbH/EasyWeb-2.0-Homebrew
```

### npm (all platforms)

Install globally from downloaded package:

```bash
npm install -g ./easyweb-remote-<version>.tgz
```

Install latest release directly (macOS/Linux):

```bash
TMP_DIR="$(mktemp -d)" && \
ASSET_URL="$(curl -fsSL https://api.github.com/repos/EasySystems-GmbH/EasyWeb-2.0-Hub/releases/latest | node -e 'let d="";process.stdin.on("data",c=>d+=c);process.stdin.on("end",()=>{const j=JSON.parse(d);const a=(j.assets||[]).find(x=>/^easyweb-remote-.*\.tgz$/i.test(x.name));if(!a){process.exit(1);}process.stdout.write(a.browser_download_url);});')" && \
curl -fsSL "$ASSET_URL" -o "$TMP_DIR/easyweb-remote-latest.tgz" && \
npm install -g "$TMP_DIR/easyweb-remote-latest.tgz" && \
rm -rf "$TMP_DIR" && \
export PATH="$(npm config get prefix)/bin:$PATH"
```

Alternative using `jq` (if installed):

```bash
ASSET_URL=$(curl -fsSL https://api.github.com/repos/EasySystems-GmbH/EasyWeb-2.0-Hub/releases/latest | jq -r '.assets[] | select(.name | test("^easyweb-remote-.*\\.tgz$")) | .browser_download_url')
curl -fsSL "$ASSET_URL" -o /tmp/easyweb-latest.tgz && npm install -g /tmp/easyweb-latest.tgz && rm /tmp/easyweb-latest.tgz
```

Install latest release directly (PowerShell):

```powershell
$release = Invoke-RestMethod https://api.github.com/repos/EasySystems-GmbH/EasyWeb-2.0-Hub/releases/latest
$asset = $release.assets | Where-Object { $_.name -match '^easyweb-remote-.*\.tgz$' } | Select-Object -First 1
if (-not $asset) { throw "No easyweb-remote .tgz asset found in latest release." }
$tmp = Join-Path $env:TEMP "easyweb-remote-latest.tgz"
Invoke-WebRequest $asset.browser_download_url -OutFile $tmp
npm install -g $tmp
Remove-Item $tmp -Force
```

Verify:

```bash
easyweb --help
easyweb version
```

### Troubleshooting: `command not found`

If `easyweb` is not found after install, the npm global bin directory may not be in your PATH. Add it:

```bash
export PATH="$(npm config get prefix)/bin:$PATH"
```

To make this permanent, add that line to `~/.zshrc` (zsh) or `~/.bashrc` (bash), then run `source ~/.zshrc` or open a new terminal.

## Configure Connection

You can pass connection flags on each command:

```bash
easyweb ls / --base-url http://localhost:5055 --username admin --password EasyWebRemote!2026 --theme-path /
```

Or set environment variables.

macOS/Linux:

```bash
export EASYWEB_BASE_URL=http://localhost:5055
export EASYWEB_USERNAME=admin
export EASYWEB_PASSWORD=EasyWebRemote!2026
export EASYWEB_THEME_PATH=/
export EASYWEB_DEFAULT_CULTURE=de
```

PowerShell:

```powershell
$env:EASYWEB_BASE_URL="http://localhost:5055"
$env:EASYWEB_USERNAME="admin"
$env:EASYWEB_PASSWORD="EasyWebRemote!2026"
$env:EASYWEB_THEME_PATH="/"
$env:EASYWEB_DEFAULT_CULTURE="de"
```

### CMS admin (for pull / sync / media / backup)

Workspace pull also downloads navigation and images from the CMS API. Uploads, backups, users, and secrets use the same auth:

macOS/Linux:

```bash
export EASYWEB_ADMIN_EMAIL=admin@easyweb.local
export EASYWEB_ADMIN_PASSWORD=EasyWeb!2026
# or machine token:
# export EASYWEB_API_TOKEN=ew_...
```

On ESYS Hosting stacks, use the deploy form **admin email** and **admin password** (same as the CMS login), not the WebDAV remote-editing password.

Optional:

```bash
export EASYWEB_SITE_ID=4a33a08b-f52f-4e89-bc4b-3ecb8fe49cb5   # default seeded site
```

## Common Commands

```bash
easyweb ls /theme
easyweb push ./theme /theme
easyweb publish . --default-culture de
easyweb pull .
easyweb sync .
easyweb pull-navigation .
easyweb push-navigation .
easyweb snippet dataset products
easyweb push-images .
easyweb push-images-metadata .
easyweb push-documents .
easyweb pull-users . / easyweb push-users .
easyweb pull-secrets . --yes / easyweb push-secrets . --yes
easyweb backup export full ./backup.zip
easyweb backup import full ./backup.zip --yes
easyweb api-token create cli
easyweb cache clear
easyweb privacy-scan
easyweb validate .
easyweb create-theme MyTheme ./Themes
easyweb update --check
easyweb clear /theme --yes
```

## Publish and pull (workspace)

### Sync matrix (canonical)

| Local folder | Server | Transport | Notes |
|--------------|--------|-----------|-------|
| `theme/` | WebDAV `/theme` | WebDAV | Active instance theme |
| `pages/` | WebDAV `/pages` | WebDAV | Page HTML + `.meta.json` |
| `datasets/` | WebDAV `/datasets` | WebDAV | When present |
| `forms/` | WebDAV `/forms` | WebDAV | When present |
| `news/` | WebDAV `/news` | WebDAV | When present |
| `settings/*.json` (allowlist) | WebDAV `/settings` | WebDAV | `site-theme`, `privacy`, `redirects`, `maps`, legal snippets |
| `settings/navigation.json` | CMS DB | CMS API | `EASYWEB_API_TOKEN` or `EASYWEB_ADMIN_*` |
| `images/` | CMS media | CMS API | Pull on `pull .`; push via `push-images` / `publish` |
| `files/` | CMS documents | CMS API | `push-documents` / `publish` |
| `settings/users.json` | CMS users | CMS API | `pull-users` / `push-users` (no hashes on pull) |
| secrets JSON | CMS settings API | CMS API | Explicit `pull-secrets` / `push-secrets --yes` only |

Empty workspace + `easyweb pull .` creates the standard layout and pulls allowlisted settings.

See also [Working with EasyWeb](working-with-easyweb.md) and [WebDAV and CLI routes](webdav-and-cli.md).

### Site workspace vs Core repo

| Context | Pages | Theme |
|---------|-------|-------|
| **Site repo / CLI** (`easyweb publish .`) | `pages/` | `theme/` |
| **Core Docker bind-mount** (`EasyWeb-2.0/`) | `Pages/` | `Themes/site` |
| **WebDAV / server** | `/pages` | `/theme` |

`easyweb publish .` / `pull .` / `validate .` expect a **site workspace** with lowercase `theme/` and `pages/` at the workspace root. The Core repo layout (`Pages/` + `Themes/site`) is for local Docker development (bind mounts — changes are live without publish). To sync a single folder from Core: `easyweb push Themes/site /theme` or `easyweb pull /theme ./theme`.

Standard site layout in git:

```text
my-site/
  theme/                    # layout, assets, inc/  →  /theme
  pages/                    # page HTML and .meta.json  →  /pages
  pages/de/                 # optional culture subfolders
  datasets/                 # optional  →  /datasets
  forms/                    # optional  →  /forms
  news/                     # optional  →  /news
  settings/navigation.json  # main menu (CMS database via API)
  images/                   # CMS uploads (pull)
```

**Publish** (local → server):

```bash
easyweb publish . --default-culture de
```

Uploads `theme/` → `/theme`, `pages/` → `/pages`, and when present `datasets/` → `/datasets`, `forms/` → `/forms`, `news/` → `/news` (with culture mirroring for pages when needed).

When `settings/navigation.json` exists in the workspace, **`publish` also pushes navigation** to the CMS (unless you pass `--skip-navigation`). Requires `EASYWEB_ADMIN_EMAIL` and `EASYWEB_ADMIN_PASSWORD`.
**Pull navigation** (CMS → local):

```bash
easyweb pull-navigation .
```

Writes `settings/navigation.json` with the current menu from the database (includes link `id` values for later updates).

**Push navigation** (local → CMS):

```bash
easyweb push-navigation .
```

Applies `settings/navigation.json` to the CMS:

- Link **order** in the file becomes `sortOrder` (top = first in menu).
- Links with an `id` from a previous pull are **updated**.
- Links without `id` are **created**.
- Server links missing from the file are **deleted**.
- Nested items use **`parentLinkId`** (flat list; parents are pushed before children). Also round-trips `openInNewTab` and `culture`.

After a successful push, `navigation.json` is rewritten with current server ids (use `--no-rewrite` to keep your file unchanged).

Requires CMS admin credentials and **Navigation → Edit** permission (`cms.navigation.edit`).

Legacy `navigation/main.json` is still read if the settings file does not exist.

Example `settings/navigation.json`:

```json
{
  "siteId": "4a33a08b-f52f-4e89-bc4b-3ecb8fe49cb5",
  "links": [
    { "id": "aaaaaaaa-aaaa-4aaa-8aaa-aaaaaaaaaaaa", "title": "Leistungen", "url": "/de/leistungen", "sortOrder": 1 },
    {
      "id": "bbbbbbbb-bbbb-4bbb-8bbb-bbbbbbbbbbbb",
      "title": "Beratung",
      "url": "/de/beratung",
      "sortOrder": 1,
      "parentLinkId": "aaaaaaaa-aaaa-4aaa-8aaa-aaaaaaaaaaaa",
      "openInNewTab": false
    }
  ]
}
```

You can also use a bare array of links. Run `easyweb pull .` first to export ids from the server.

**Pull** (server → local) after CMS or remote edits:

```bash
easyweb pull .
# alias
easyweb sync .
```

Downloads into the workspace (see [sync matrix](#sync-matrix-canonical) above):

| Server source | Local path |
|---------------|------------|
| WebDAV `/theme` | `theme/` |
| WebDAV `/pages` | `pages/` (all cultures, `.meta.json` including SEO and **sliders**) |
| WebDAV `/datasets` | `datasets/` |
| WebDAV `/forms` | `forms/` |
| WebDAV `/news` | `news/` |
| CMS navigation API | `settings/navigation.json` |
| CMS media library | `images/` |

Requires `EASYWEB_ADMIN_EMAIL` and `EASYWEB_ADMIN_PASSWORD` for navigation and images. WebDAV credentials alone sync theme, pages, datasets, forms, and news.

Flags:

- `--skip-cms` — WebDAV only (`theme/`, `pages/`, `datasets/`, `forms/`, `news/`)
- `--skip-images` — pull navigation but not `images/`
- `--skip-navigation` — on `publish`, skip pushing `settings/navigation.json`
- `--no-rewrite` — on `push-navigation`, do not update `navigation.json` with server ids
- `--admin-email`, `--admin-password`, `--site-id` — override env vars

Single-folder pull (WebDAV only):

```bash
easyweb pull /theme ./theme
easyweb pull /pages ./pages
easyweb pull /datasets ./datasets
```

> **Tip:** Edit `settings/navigation.json` in git, then run `easyweb push-navigation .` or `easyweb publish .` to apply changes to the live site.

## Validate theme

Check a workspace or generated theme before publish:

```bash
easyweb validate .
```

Run `validate` against a **theme root** or a site workspace that contains `theme/` (the CLI resolves `theme/` when present). It verifies required files (`theme.json`, `inc/_header.html`, `inc/_footer.html`, `index.html`, `assets/css/main.css`, `assets/js/main.js`), EasyWeb Liquid placeholders, editable regions, and duplicate `theme/` vs `pages/` slug templates. Exit code is `1` when any check fails.

Optional **WARN** lines appear when CMS marker blocks are missing (`ew-site-meta`, `ew-theme-css`, …) — warnings do not fail the command. Instance themes that use `blank.html` as `theme.json` entry may still need `index.html` for a clean validate pass (scaffold from `easyweb create-theme` includes both).

## Create theme and update README docs

Scaffold a new theme from the **Basic Demo Theme** starter ([EasyWeb-2.0-Basic-Demo-Theme](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Basic-Demo-Theme) — same SSOT as Docker first-boot and Theme admin **Load starter theme**):

```bash
easyweb create-theme MyTheme ./Themes
```

The command copies Bootstrap 5 layout with CMS marker blocks (`ew-site-meta`, `ew-theme-css`, `ew-site-footer`, …), `custom.css`, `editor-canvas.css`, slider/gallery base CSS, and empty structural templates (gallery, news, kontakt, legal). Theme name placeholders are applied in `theme.json`, manifest, and nav branding. After scaffolding, treat the result as your instance theme (`Themes/site` or workspace `theme/`) — do not maintain a parallel “Basic” copy.

Refresh the generated **Template docs** section in a theme `README.md`:

```bash
easyweb update docs .
# alias
easyweb update-docs .
```

## Auto Update

The CLI can self-update from the latest Hub release:

```bash
easyweb update --check
easyweb update
```

Notes:

- `--check` only checks and prints latest version.
- `--force` installs even when versions match.
- Update runs `npm install -g <downloaded .tgz>` under the hood.

## Help and Cursor Compatibility

The CLI supports both global and command-level help:

```bash
easyweb --help
easyweb help
easyweb help publish
easyweb help pull
easyweb publish --help
easyweb pull --help
```

This makes command discovery reliable for both humans and AI tooling (including Cursor).

## Further reading

- [CMS admin](cms-admin.md) — hybrid page editor, sliders, image gallery
- [Themes and content](themes-and-content.md) — Liquid, pages, sliders, cultures
- [CMS permissions](cms-permissions.md)
- [EasyWeb remote editing](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/remote-editing.md) — WebDAV layout and credentials (core repo)
- [WebDAV ↔ CMS compatibility](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/webdav-cms-compatibility.md) — markers, conflicts, pull-before-edit (core repo)
- [Page building / CMS blocks](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/cursor-page-building.md) — blocks, zones, snippets (core repo)
