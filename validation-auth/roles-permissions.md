# Roles & Permissions

## 1. Role Definitions
The application uses Role-Based Access Control (RBAC) managed via the `Role` enum in the database schema.

| Role | System Identifier | Description |
| :--- | :--- | :--- |
| **Administrator** | `ADMIN` | Superuser with global access to all resources and user management. |
| **Standard User** | `USER` | The default role for registered members. Can manage their own content but cannot affect system-wide settings. |
| **Viewer** | `VIEWER` | Read-only access. Useful for auditors, public observers, or restricted accounts. |

---

## 2. Permission Matrix
This matrix defines the CRUD (Create, Read, Update, Delete) capabilities for each role.

### Global Permissions

| Feature | Admin | User | Viewer |
| :--- | :---: | :---: | :---: |
| **User Management** | ✅ Full Access | ❌ | ❌ |
| **System Settings** | ✅ Full Access | ❌ | ❌ |
| **View Content** | ✅ All Content | ✅ Own + Public | ✅ Public Only |

### Resource-Specific Permissions (e.g., Posts/Profile)

| Action | Admin | User | Viewer | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Create** | ✅ | ✅ | ❌ | Users can only create resources linked to their own ID. |
| **Read** | ✅ | ✅ | ✅ | *Users* can read their own private data; *Viewers* can only read public data. |
| **Update** | ✅ | ✅ | ❌ | Users can only update their own resources. |
| **Delete** | ✅ | ✅ | ❌ | Users can delete their own resources; Admins can soft-delete any resource for moderation. |

---

## 3. Implementation Logic
* **Authentication:** Verifies identity (JWT/Session).
* **Authorization:** Middleware checks `req.user.role` against required permissions before executing the controller logic.
* **Ownership Check:** For `UPDATE` and `DELETE` operations by `USER`, the system must verify `resource.ownerId === currentUser.id`.