# Database Schema Models

This document provides a detailed breakdown of the data models defined in the `schema.prisma` file, including their fields, data types, and relationships.

## 1. Overview
The database uses a relational structure (SQL) designed to handle:
* **Authentication & Authorization:** Managing users and their specific roles.
* **Content Management:** Handling posts and their association with authors.

---

## 2. Models

### A. User Model
**Purpose:** Represents a registered entity in the system. This is the core model for authentication and permission handling.

| Field | Type | Attributes | Description |
| :--- | :--- | :--- | :--- |
| `id` | String | `@id`, `@default(uuid())` | The unique identifier for the user. Uses UUIDs for security and scalability. |
| `email` | String | `@unique` | The user's email address. Used for login and must be unique across the system. |
| `name` | String? | `Nullable` | The user's display name. Optional. |
| `role` | Role | `@default(USER)` | The permission level assigned to the user (See Enums below). |
| `posts` | Post[] | Relation | A "virtual" field representing the list of posts created by this user. |

### B. Post Model
**Purpose:** Represents a content entry (e.g., a blog post or article) created by a User.

| Field | Type | Attributes | Description |
| :--- | :--- | :--- | :--- |
| `id` | Int | `@id`, `@default(autoincrement())` | The unique identifier for the post. |
| `title` | String | -- | The headline/title of the post. |
| `content` | String? | `Nullable` | The main body text of the post. |
| `published`| Boolean| `@default(false)` | Flag indicating if the post is visible to the public. |
| `author` | User | Relation | The relationship field connecting the post to its creator. |
| `authorId` | String | Foreign Key | The actual database column storing the User's UUID. |

---

## 3. Enums

### Role
**Purpose:** Defines the strict set of permission levels available in the system. This prevents invalid roles from being assigned.

* **`ADMIN`**: Full system access (User management, content moderation).
* **`USER`**: Standard access (Create/Read/Update/Delete own content).
* **`VIEWER`**: Read-only access (Cannot create or edit content).

---

## 4. Entity Relationships (ERD)

The schema implements a **One-to-Many** relationship between Users and Posts.

* **1 User ↔ Many Posts**: A single User can be the author of multiple Posts.
* **1 Post ↔ 1 User**: A Post must belong to exactly one User (the author).

**Technical Implementation:**
* The `Post` table holds a foreign key column `authorId` which references the `id` column in the `User` table.
* If a `User` is deleted, the behavior depends on the `onDelete` constraint (typically cascading delete or set null, depending on business requirements).