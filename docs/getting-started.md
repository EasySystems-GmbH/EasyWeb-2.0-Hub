# EasyWeb 2.0 - Getting Started

Welcome to EasyWeb 2.0.

Use this repository as your public entry point for:

- installation instructions
- public user/developer documentation
- links to related EasyWeb repositories and assets

Live documentation site:

- [https://easysystems-gmbh.github.io/EasyWeb-2.0-Hub/](https://easysystems-gmbh.github.io/EasyWeb-2.0-Hub/)

## Recommended Repositories

- `EasyWeb-2.0 Hub` (this repo): public resources and docs
- `EasyWeb 2.0 Basic Demo Theme` (public): starter theme for users

## Documentation Navigation

- [Install with Docker](install-with-docker.md)
- [Repository Map](repositories.md)
- [Docker Compose Example](../examples/docker-compose.yml)
- [VS Code Extension](vscode-extension.md)
- [CLI](cli.md)
- [Hub Home](../index.md)

## Create a Base Theme with the CLI

After installing the CLI, create a new base theme scaffold:

```bash
easyweb create-theme MyTheme ./Themes
```

This creates a starter theme folder at `./Themes/MyTheme`.

Next steps:

1. Open the created theme folder and update templates/assets.
2. Validate before publish:

   ```bash
   easyweb validate ./Themes/MyTheme
   ```

3. Publish your theme when ready:

   ```bash
   easyweb publish ./Themes/MyTheme
   ```

See [CLI](cli.md) for additional commands and connection setup options.

## Next Steps

1. Follow [Install with Docker](install-with-docker.md) to run EasyWeb locally.
2. Pull and customize the Basic Demo Theme for your site.
3. Use Hub docs as your source of truth for public instructions.
