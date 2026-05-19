# EasyWeb Repository Map

EasyWeb 2.0 is organized into separate repositories with clear responsibilities.

## Repositories

### `EasyWeb 2.0` (core)

Core platform repository:

- ASP.NET Core API and CMS admin (`/admin`)
- WYSIWYG page editor, page sliders, image gallery, navigation, documents
- Per-user CMS permissions
- Fluid (Liquid) theme runtime
- CLI (`easyweb`) and VS Code extension (`easyweb-remote`)
- tests and infrastructure

### `EasyWeb-2.0 Hub` (public)

Public documentation hub (this repo):

- onboarding and install guides
- [CMS admin](cms-admin.md), [permissions](cms-permissions.md), [themes and content](themes-and-content.md)
- CLI and extension guides
- central index to EasyWeb resources

### `EasyWeb 2.0 Basic Demo Theme` (public)

Starter theme repository:

- ready-to-use theme for new installations
- baseline templates and assets (including `gallery.html` slider example)
- safe starting point for custom themes

## Documentation Navigation

- [Getting Started](getting-started.md)
- [Install with Docker](install-with-docker.md)
- [CMS admin](cms-admin.md)
- [CMS permissions](cms-permissions.md)
- [Themes and content](themes-and-content.md)
- [CLI](cli.md) — `publish`, `pull`, `sync`
- [VS Code Extension](vscode-extension.md)
- [Docker Compose Example](../examples/docker-compose.yml)
- [Hub Home](../index.md)
