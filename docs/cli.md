# EasyWeb CLI

The EasyWeb CLI is published from the core `EasyWeb 2.0` repository and attached to Hub releases as a `.tgz` package.

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

### CMS admin (for pull / sync)

Workspace pull also downloads navigation and uploaded images from the CMS API. That uses **CMS admin** credentials (not WebDAV):

macOS/Linux:

```bash
export EASYWEB_ADMIN_EMAIL=admin@easyweb.local
export EASYWEB_ADMIN_PASSWORD=EasyWeb!2026
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
easyweb push-navigation .
easyweb pull .
easyweb sync .
easyweb pull /theme ./theme
easyweb validate .
easyweb create-theme MyTheme ./Themes
easyweb update --check
easyweb update
easyweb clear /theme --yes
```

## Publish and pull (workspace)

Standard site layout in git:

```text
my-site/
  theme/          # layout, assets, inc/
  pages/          # page HTML and .meta.json
  pages/de/       # optional culture subfolders
```

**Publish** (local → server):

```bash
easyweb publish . --default-culture de
```

Uploads `theme/` → WebDAV `/theme` and `pages/` → `/pages` (with culture mirroring when needed).

When `navigation/main.json` exists in the workspace, **`publish` also pushes navigation** to the CMS (unless you pass `--skip-navigation`). Requires `EASYWEB_ADMIN_EMAIL` and `EASYWEB_ADMIN_PASSWORD`.

**Push navigation** (local → CMS only):

```bash
easyweb push-navigation .
```

Applies `navigation/main.json` to the CMS:

- Link **order** in the file becomes `sortOrder` (top = first in menu).
- Links with an `id` from a previous pull are **updated**.
- Links without `id` are **created**.
- Server links missing from the file are **deleted**.

After a successful push, `main.json` is rewritten with current server ids (use `--no-rewrite` to keep your file unchanged).

Requires CMS admin credentials and **Navigation → Edit** permission (`cms.navigation.edit`).

Example `navigation/main.json`:

```json
{
  "siteId": "4a33a08b-f52f-4e89-bc4b-3ecb8fe49cb5",
  "links": [
    { "title": "Home", "url": "/home" },
    { "title": "About", "url": "/about" }
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

Downloads into the workspace:

| Server source | Local path |
|---------------|------------|
| WebDAV `/theme` | `theme/` |
| WebDAV `/pages` | `pages/` (all cultures, `.meta.json` including SEO and **sliders**) |
| CMS navigation API | `navigation/main.json` |
| CMS media library | `images/` |

Requires `EASYWEB_ADMIN_EMAIL` and `EASYWEB_ADMIN_PASSWORD` for navigation and images. WebDAV credentials alone sync only theme and pages.

Flags:

- `--skip-cms` — WebDAV only (`theme/`, `pages/`)
- `--skip-images` — pull navigation but not `images/`
- `--skip-navigation` — on `publish`, skip pushing `navigation/main.json`
- `--no-rewrite` — on `push-navigation`, do not update `main.json` with server ids
- `--admin-email`, `--admin-password`, `--site-id` — override env vars

Single-folder pull (WebDAV only):

```bash
easyweb pull /theme ./theme
easyweb pull /pages ./pages
```

> **Tip:** Edit `navigation/main.json` in git, then run `easyweb push-navigation .` or `easyweb publish .` to apply changes to the live site.

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

- [CMS admin](cms-admin.md) — WYSIWYG pages, sliders, image gallery
- [Themes and content](themes-and-content.md) — Liquid, pages, sliders, cultures
- [CMS permissions](cms-permissions.md)
- [EasyWeb remote editing](https://github.com/EasySystems-GmbH/EasyWeb-2.0/blob/main/docs/remote-editing.md) — WebDAV layout and credentials (core repo)
