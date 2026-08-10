# EasyWeb Repository Map

EasyWeb 2.0 is organized into separate repositories with clear responsibilities.

## Repositories

### `EasyWeb 2.0` (core)

Core platform repository:

- ASP.NET Core API and CMS admin (`/admin`)
- Hybrid page editor (GrapesJS + Monaco), page sliders, image gallery, navigation, documents, data sets, news, forms, theme
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

**Single source of truth** for the CMS-ready Bootstrap starter:

- Edit only this repo’s `theme/` folder
- Docker first-boot and Theme admin **Load starter theme** copy into instance `Themes/site`
- `easyweb create-theme` scaffolds from the same starter
- No demo news/dataset/gallery **entries** — structural templates only
- CLI / WebDAV / code-first compatible (same file contract as any site `theme/`)

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
