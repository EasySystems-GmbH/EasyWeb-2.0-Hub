# Install EasyWeb 2.0 with Docker

This guide runs EasyWeb 2.0 with PostgreSQL using Docker Compose.

Navigation:

- [Getting Started](getting-started.md)
- [CMS admin](cms-admin.md)
- [Themes and content](themes-and-content.md)
- [Repository Map](repositories.md)
- [CLI](cli.md)
- [VS Code Extension](vscode-extension.md)
- [Hub Home](../index.md)

## Docker Package

The EasyWeb Docker package is published via GitHub Packages:

- [easyweb package on GitHub Packages](https://github.com/orgs/EasySystems-GmbH/packages/container/package/easyweb)

Choose the desired tag (e.g. `latest`).

## 1) Create a `docker-compose.yml`

Use `examples/docker-compose.yml` from this repository.

It uses:

- `ghcr.io/easysystems-gmbh/easyweb:latest`
- Named volumes for **Themes, Pages, images, files, datasets, news, forms** (persists CMS content across container recreate; `docker compose down -v` still deletes volumes)

If you want a pinned release, change `latest` to a specific tag.

You can also copy it quickly:

```bash
cp examples/docker-compose.yml docker-compose.yml
```

Direct link:

- [Docker Compose Example](../examples/docker-compose.yml)

For Core **source development** with bind mounts onto `./Pages`, `./Themes/site`, etc., use `EasyWeb-2.0/docker-compose.yml` in the [core repository](https://github.com/EasySystems-GmbH/EasyWeb-2.0) instead of this Hub example.

## 2) Start services

```bash
docker compose up -d
```

## 3) Open EasyWeb

- CMS/API: [http://localhost:8080](http://localhost:8080)
- CMS admin: [http://localhost:8080/admin](http://localhost:8080/admin)
- Remote editing/WebDAV: [http://localhost:5055/webdav/](http://localhost:5055/webdav/)

Default admin login:

- Email: `admin@easyweb.local`
- Password: `EasyWeb!2026`

Use the CMS to edit pages (hybrid visual/code editor), manage [page sliders](themes-and-content.md#page-sliders-liquid), upload images, and edit navigation. See [CMS admin](cms-admin.md).

## 4) Theme and site content

The compose example sets `Themes__DefaultRootPath=/app/Themes/site`. On first start the container entrypoint seeds the bundled demo theme into that folder on the `easyweb_themes` volume.

### Option A — CLI scaffold (recommended)

```bash
# Install CLI first — see docs/cli.md
easyweb create-theme MySite ./workspace
cd ./workspace
easyweb validate .
```

Organize a deployable **site workspace** (CLI expects lowercase `theme/` + `pages/` at the root — see [CLI sync matrix](cli.md#sync-matrix-canonical)):

```text
my-site/
  theme/       # from create-theme output (move files into theme/)
  pages/
  datasets/    # optional
  forms/       # optional
  news/        # optional
  settings/    # navigation.json after pull
```

Publish to the running instance:

```bash
export EASYWEB_BASE_URL=http://localhost:5055
export EASYWEB_USERNAME=admin
export EASYWEB_PASSWORD=EasyWebRemote!2026
export EASYWEB_ADMIN_EMAIL=admin@easyweb.local
export EASYWEB_ADMIN_PASSWORD=EasyWeb!2026

easyweb publish . --default-culture de
```

After editing in the CMS admin, pull changes back to git:

```bash
easyweb pull .
```

### Option B — Basic Demo Theme repository

Clone [EasyWeb 2.0 Basic Demo Theme](https://github.com/EasySystems-GmbH/EasyWeb-2.0-Basic-Demo-Theme) and publish its `theme/` and `pages/` folders with `easyweb publish .`.

## Notes

- For production, change secrets and switch to secure credentials and environment values.
- WebDAV uses `RemoteEditing__*` credentials; CMS pull uses `Seed__AdminEmail` / `Seed__AdminPassword` (see [CLI](cli.md)).
