# CMS permissions

EasyWeb 2.0 restricts CMS features with **fine-grained permissions** assigned per user (in addition to Admin and Editor roles).

## Roles

| Role | Behavior |
|------|----------|
| **Admin** | Full access to every CMS area (users, settings, all content). |
| **Editor** | Access is controlled by permissions (see below). With **no** permissions selected, Editors receive the default content set (pages, navigation, images, documents). |

## Permissions

| Permission | CMS area |
|------------|----------|
| `cms.pages.view` | View pages |
| `cms.pages.edit` | Create, edit, delete pages (including WYSIWYG and sliders) |
| `cms.navigation.view` | View navigation |
| `cms.navigation.edit` | Edit navigation |
| `cms.files.view` | View image gallery |
| `cms.files.edit` | Upload and manage images |
| `cms.documents.view` | View documents |
| `cms.documents.edit` | Upload and manage documents |
| `cms.users.manage` | User manager (assign roles and permissions) |
| `cms.settings.manage` | Remote editing / WebDAV settings |

Permissions are enforced on CMS API endpoints and admin UI (sidebar links are hidden when not allowed).

## Assign permissions

1. Sign in as an **Admin**.
2. Open **Users** in the CMS (`/admin/users`).
3. Select or create a user.
4. Set roles (e.g. `Editor`) and tick the permissions they need.
5. Save.

### Examples

**Contributor — pages only**

- Role: `Editor`
- Permissions: **Pages → View**, **Pages → Create, edit, and delete**

**Designer — images only**

- Role: `Editor`
- Permissions: **Image gallery → View**, **Image gallery → Upload and manage**

**Reviewer — read-only pages**

- Role: `Editor`
- Permissions: **Pages → View** only (WYSIWYG and sliders are read-only; Save is disabled)

## API (integrations)

- `GET /api/cms/me` — current user email, roles, and effective permissions
- `GET /api/cms/permissions` — permission catalog (Admin only)

## Related docs

- [CMS admin](cms-admin.md)
