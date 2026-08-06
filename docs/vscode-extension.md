# EasyWeb VS Code Extension

The EasyWeb extension packages as a `.vsix` file via GitHub Actions and can be installed in both VS Code and Cursor.
The same workflow also publishes the EasyWeb CLI package (`.tgz`) for terminal-based workflows.

## Download

- Latest tagged releases (recommended):  
  [EasyWeb-2.0-Hub releases](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Hub/releases)
- CI artifacts (for non-tag builds):  
  [Extension packaging workflow runs](https://github.com/EasySystems-GmbH/EasyWeb-2.0/actions/workflows/vscode-extension-package.yml)

Download the `.vsix` file named like `easyweb-remote-<version>.vsix`.
For CLI usage, see [CLI Guide](cli.md).

## Install from VSIX

### VS Code

1. Open Extensions panel.
2. Open the `...` menu.
3. Choose `Install from VSIX...`.
4. Select the downloaded `.vsix`.

### Cursor

1. Open Extensions panel.
2. Open the `...` menu.
3. Choose `Install from VSIX...`.
4. Select the downloaded `.vsix`.

## Basic Usage

1. Run `EasyWeb: Configure Connection`.
2. Use defaults for local docker setup:
   - Base URL: [http://localhost:5055](http://localhost:5055)
   - Username: `admin`
   - Password: `EasyWebRemote!2026`
   - Remote root path: `/`
3. Open your local **site workspace** (folders `theme/` and `pages/`; optionally `datasets/`, `forms/`, `news/`). This is not the Core repo layout `Themes/site` + `Pages/` — see [CLI — site workspace vs Core](cli.md#site-workspace-vs-core-repo).
4. Run `EasyWeb: Publish` to push local changes to the server (`theme/`, `pages/`, and when present datasets/forms/news; plus `settings/navigation.json` when present), or use **Pull Navigation** / **Pull from Server** to download CMS data into `settings/` and `images/`.
5. Run `EasyWeb: Validate` before publishing to check structure/placeholders.

For CMS features (hybrid page editor, page sliders, permissions), use the browser admin at `/admin` — see [CMS admin](cms-admin.md).

Optional settings (for pull: navigation and images):

- `easywebRemote.adminEmail` — CMS admin email (default `admin@easyweb.local`)
- `easywebRemote.adminPassword` — CMS admin password
- `easywebRemote.siteId` — site GUID for navigation (default seeded site)

You can also set `EASYWEB_ADMIN_EMAIL` and `EASYWEB_ADMIN_PASSWORD` in the environment.

## Helpful Commands

- `EasyWeb: Publish` — sync local `theme/`, `pages/`, and when present `datasets/` / `forms/` / `news/` to WebDAV; pushes `settings/navigation.json` when configured
- `EasyWeb: Pull Navigation` — download menu to `settings/navigation.json`
- `EasyWeb: Push Navigation` — apply `settings/navigation.json` to the CMS
- `EasyWeb: Pull from Server` — sync server → local (`theme/`, `pages/`, `datasets/`, `forms/`, `news/`, `settings/navigation.json`, `images/`)
- `EasyWeb: Publish Theme` — sync `theme/` to `/theme` only
- `EasyWeb: Publish Pages` — sync `pages/` to `/pages` only
- `EasyWeb: Open Remote Theme` — browse/edit remote files via `easyweb:/` URI
- `EasyWeb: Refresh Remote Files` — reload remote tree
- `EasyWeb: Clear Remote` — clear a remote path after confirmation

See [CLI Guide](cli.md) for terminal equivalents (`easyweb publish`, `easyweb push-navigation`, `easyweb pull`, `easyweb sync`).

## Related docs

- [CMS admin](cms-admin.md)
- [Themes and content](themes-and-content.md)
- [CMS permissions](cms-permissions.md)
