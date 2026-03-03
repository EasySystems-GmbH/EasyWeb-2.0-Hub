# Install EasyWeb 2.0 with Docker

This guide runs EasyWeb 2.0 with PostgreSQL using Docker Compose.

Navigation:

- [Getting Started](getting-started.md)
- [Repository Map](repositories.md)
- [Hub Home](../index.md)

## Docker Package

The EasyWeb Docker package is published via GitHub Packages:

- `https://github.com/orgs/EasySystems-GmbH/packages/container/package/easyweb`

Choose the desired tag (e.g. `latest`).

## 1) Create a `docker-compose.yml`

Use `examples/docker-compose.yml` from this repository.

It uses:

- `ghcr.io/easysystems-gmbh/easyweb:latest`

If you want a pinned release, change `latest` to a specific tag.

You can also copy it quickly:

```bash
cp examples/docker-compose.yml docker-compose.yml
```

Direct link:

- [Docker Compose Example](../examples/docker-compose.yml)

## 2) Start services

```bash
docker compose up -d
```

## 3) Open EasyWeb

- CMS/API: `http://localhost:8080`
- Remote editing/WebDAV: `http://localhost:5055/webdav/`

Default admin login:

- Email: `admin@easyweb.local`
- Password: `EasyWeb!2026`

## 4) Add the Basic Demo Theme

Clone or copy the `EasyWeb 2.0 Basic Demo Theme` repo into your mounted `Themes` folder, then set:

- `Themes__DefaultRootPath=/app/Themes/BasicDemoTheme`

## Notes

- For production, change secrets and switch to secure credentials and environment values.
