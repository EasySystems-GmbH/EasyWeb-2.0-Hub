# EasyWeb VS Code Extension

The EasyWeb extension packages as a `.vsix` file via GitHub Actions and can be installed in both VS Code and Cursor.

## Download

- Latest tagged releases (recommended):  
  [EasyWeb-2.0-Hub releases](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Hub/releases)
- CI artifacts (for non-tag builds):  
  [Extension packaging workflow runs](https://github.com/EasySystems-GmbH/EasyWeb-2.0/actions/workflows/vscode-extension-package.yml)

Download the `.vsix` file named like `easyweb-remote-<version>.vsix`.

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
3. Run `EasyWeb: Open Remote Theme`.
4. Edit files in the remote workspace.
5. Run `EasyWeb: Publish Theme` (or `EasyWeb: Publish`) to push changes.
6. Run `EasyWeb: Validate` before publishing to check structure/placeholders.

## Helpful Commands

- `EasyWeb: Publish` - sync whole workspace to remote root
- `EasyWeb: Publish Theme` - sync `theme/` to `/theme`
- `EasyWeb: Publish Pages` - sync `pages/` to `/pages`
- `EasyWeb: Refresh Remote Files` - reload remote tree
- `EasyWeb: Clear Remote` - clear remote root after confirmation
