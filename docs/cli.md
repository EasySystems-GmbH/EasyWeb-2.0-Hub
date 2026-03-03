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

Install globally from downloaded package:

```bash
npm install -g ./easyweb-remote-<version>.tgz
```

Verify:

```bash
easyweb --help
easyweb version
```

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
```

PowerShell:

```powershell
$env:EASYWEB_BASE_URL="http://localhost:5055"
$env:EASYWEB_USERNAME="admin"
$env:EASYWEB_PASSWORD="EasyWebRemote!2026"
$env:EASYWEB_THEME_PATH="/"
```

## Common Commands

```bash
easyweb ls /
easyweb push ./theme /theme
easyweb pull /theme ./theme
easyweb publish .
easyweb validate .
easyweb create-theme MyTheme ./Themes
easyweb update --check
easyweb update
easyweb clear / --yes
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
easyweb publish --help
```

This makes command discovery reliable for both humans and AI tooling (including Cursor).
