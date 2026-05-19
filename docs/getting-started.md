# EasyWeb 2.0 - Getting Started

Welcome to EasyWeb 2.0.

Use this repository as your public entry point for:

- installation instructions
- CMS and theme documentation
- links to related EasyWeb repositories and assets

Live documentation site:

- [https://easysystems-gmbh.github.io/EasyWeb-2.0-Hub/](https://easysystems-gmbh.github.io/EasyWeb-2.0-Hub/)

## Recommended Repositories

- `EasyWeb-2.0 Hub` (this repo): public resources and docs
- `EasyWeb 2.0` (core): platform, CMS, CLI, extension
- `EasyWeb 2.0 Basic Demo Theme` (public): starter theme for new installations

## Documentation Navigation

- [Install with Docker](install-with-docker.md)
- [CMS admin](cms-admin.md) — manage pages, sliders, images, navigation
- [CMS permissions](cms-permissions.md)
- [Themes and content](themes-and-content.md) — Liquid, `pages/`, `theme/`
- [CLI](cli.md) — publish and pull workspaces
- [VS Code extension](vscode-extension.md)
- [Repository Map](repositories.md)
- [Hub Home](../index.md)

## Next Steps

1. Follow [Install with Docker](install-with-docker.md) to run EasyWeb locally.
2. Sign in to the CMS at `/admin` (`admin@easyweb.local` / `EasyWeb!2026` by default in Docker).
3. Explore [CMS admin](cms-admin.md): edit pages in the visual editor, upload images, configure sliders.
4. Install the [CLI](cli.md) or [VS Code extension](vscode-extension.md) for git-based workflows.
5. Create or clone a site workspace (`theme/`, `pages/`, `navigation/`) and use `easyweb publish .` to deploy, or `easyweb pull .` after CMS edits. Push menu changes with `easyweb push-navigation .`.
6. Customize the [Basic Demo Theme](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Basic-Demo-Theme) as your starting point.
7. Restrict editor access with [CMS permissions](cms-permissions.md) when working in a team.
