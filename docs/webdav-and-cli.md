# WebDAV limits and CLI routes

WebDAV is the file channel for desired-state content. Anything else goes through the CMS API via the `easyweb` CLI (or Admin UI / backup ZIP).

| Area | WebDAV | CLI / CMS API |
|------|--------|---------------|
| Theme, pages, datasets, forms, news | Yes | `pull` / `publish` / `push` |
| Allowlisted settings JSON | Yes (`/settings`) | Included in `pull` / `publish` |
| Navigation links | No | `pull-navigation` / `push-navigation` |
| Images | No | `pull` (download), `push-images`, `push-images-metadata` |
| Documents | No | `push-documents` (`files/` or `documents/`) |
| Users / roles | No | `pull-users` / `push-users` |
| Secrets (SMTP, captcha, WebDAV password) | No | `pull-secrets` / `push-secrets --yes` |
| Full instance | No | `backup export|import` |
| Live cache / privacy scan | No | `cache clear`, `privacy-scan` |
| Machine tokens | No | `api-token create|list|revoke` |

Form submission inbox, 404 hit logs, and similar runtime data stay Admin/API read paths — not declarative site files.

Details: [Working with EasyWeb](working-with-easyweb.md), [CLI](cli.md), [Remote editing](remote-editing.md).
