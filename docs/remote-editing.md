# Remote editing (WebDAV + CLI)

Connect Cursor/VS Code or the `easyweb` CLI to a running EasyWeb instance.

Canonical overview of both local Docker and remote CLI: [Working with EasyWeb](working-with-easyweb.md).

## Connection

- WebDAV: `https://<host>/webdav/` (local: `http://localhost:5055/webdav/`)
- Username / password: `RemoteEditing__Username` / `RemoteEditing__Password`

CMS API (same host; local also on `:8080` or `:5055`):

- Cookie login: `EASYWEB_ADMIN_EMAIL` / `EASYWEB_ADMIN_PASSWORD`
- Or machine token: `EASYWEB_API_TOKEN` (`Authorization: Bearer …`)

## Six WebDAV containers

| Path | Purpose |
|------|---------|
| `/theme` | Active theme (`Themes/site` on server) |
| `/pages` | Page HTML + `.meta.json` |
| `/datasets` | DataSets |
| `/forms` | Forms |
| `/news` | News |
| `/settings` | **Allowlisted** site JSON only (`site-theme`, `privacy`, `redirects`, `maps`, `privacy-legal-snippets.*`) |

Secrets (`smtp.json`, `form-security.json`, `remote-editing.json`, `api-tokens.json`) are **not** exposed over WebDAV.

## CLI sync

```bash
easyweb pull .          # containers + settings allowlist + nav + images
easyweb publish .       # containers + settings allowlist; nav/images/docs when present
easyweb push Themes/site /theme   # Core repo → remote theme
```

See [CLI](cli.md) for the full command matrix (snippets, backup, users, secrets, tokens).

## Compatibility notes

- Last-writer-wins between Admin UI and WebDAV/CLI.
- Prefer one authoring path at a time for a given page.
- Managed theme markers must stay intact for CMS settings sync.
