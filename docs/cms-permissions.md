# CMS permissions

EasyWeb 2.0 restricts CMS features with fine-grained permissions assigned per user.

## Roles

| Preset | Behavior |
|--------|----------|
| **Administrator** | Full access to every CMS area (users, settings, content). Permission toggles are fixed and always on. |
| **Editor** | Full content access (pages, navigation, images, documents, data sets, news, forms). Administration areas (users, settings) are off. Permission toggles are fixed. |
| **Custom** | No built-in role preset. Enable only the permission toggles this user needs. |

Role presets are selected from a dropdown in **Users** (`/admin/users`). The toggles always show the **effective** permissions the user actually has.

## Permissions

| Permission | CMS area |
|------------|----------|
| `cms.pages.view` | View pages (also required to open the Theme editor nav item) |
| `cms.pages.edit` | Create, edit, delete pages (hybrid editor, sliders, popup); also required to save in the Theme editor |
| `cms.navigation.view` | View navigation |
| `cms.navigation.edit` | Edit navigation |
| `cms.files.view` | View image gallery |
| `cms.files.edit` | Upload and manage images |
| `cms.documents.view` | View documents |
| `cms.documents.edit` | Upload and manage documents |
| `cms.datasets.view` | View data sets |
| `cms.datasets.edit` | Create and edit data sets |
| `cms.news.view` | View news and events |
| `cms.news.edit` | Create and edit news and events |
| `cms.forms.view` | View forms and submissions |
| `cms.forms.edit` | Create and edit forms |
| `cms.users.manage` | User manager (assign roles and permissions) |
| `cms.settings.manage` | Settings (including WebDAV / remote editing) |

Permissions are stored as ASP.NET Identity claims (`cms.permission`) and enforced on:

- `/api/cms/*` endpoints
- `/admin/*` Razor pages (sidebar links hidden when not allowed)

Users in the **Admin** role bypass all permission checks. Users in the **Editor** role without stored claims receive the default content set (`EditorDefaults`).

## Assign permissions

1. Sign in as an **Administrator**.
2. Open **Users** in the CMS.
3. Select or create a user.
4. Choose a role preset:
   - **Administrator** or **Editor** — permissions are set automatically (not editable).
   - **Custom** — enable only the toggles the user needs.
5. Save.

### Examples

**Contributor — pages only**

- Preset: **Custom**
- Permissions: **Pages → View**, **Pages → Create, edit, and delete**

**Designer — images only**

- Preset: **Custom**
- Permissions: **Image gallery → View**, **Image gallery → Upload and manage**

**Reviewer — read-only pages**

- Preset: **Custom**
- Permissions: **Pages → View** only (editor is read-only; Save is disabled)

### Seed administrator

The first admin user is created on stack startup from `Seed:AdminEmail` / `Seed:AdminPassword` (Docker: `Seed__AdminEmail`, `Seed__AdminPassword`). This account is protected: it cannot be deleted, demoted, or renamed.

### Passwords

- New users require a password.
- Existing users show a masked, read-only password field.
- Use **Reset password** to set a new password (enter twice for confirmation).
- Permission or email changes do not require a password change.

## API

- `GET /api/cms/me` — current user email, roles, and effective permissions
- `GET /api/cms/permissions` — permission catalog (requires `cms.users.manage`)
- `GET /api/cms/users` — user list with `rolePreset`, `permissions`, `effectivePermissions`, `isSeedAdmin`
- `POST /api/cms/users` / `PUT /api/cms/users/{id}` — `rolePreset`, `roles`, `permissions`, optional `password`

## Related docs

- [CMS admin](cms-admin.md)
