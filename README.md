Prisma Schema → Database & Backend Understanding

📌 Objective
This assignment is designed to help college interns understand how real-world software systems are built using a database schema. You are given a Prisma schema from a production-grade project.
Your goal is to:
Understand the database design
Create clear documentation
Design ER diagrams and APIs
Learn backend and system design fundamentals
⚠️ This is not a coding assignment. This is a thinking + documentation assignment.

👥 Team Structure
There are two interns, each with a clearly defined role. Do not overlap work.
👤 Intern 1 — Database & Prisma Intern
Focus: Data structure, relations, constraints, performance
👤 Intern 2 — Backend & Business Logic Intern
Focus: System behavior, APIs, validation, authorization

📁 Mandatory Folder Structure
Create the following folder structure exactly:
intern-assignment/
│
├── schema-understanding/
│   └── models-explanation.md
│
├── er-diagram/
│   ├── er-diagram.png
│   └── er-diagram-source.drawio
│
├── api-design/
│   ├── api-list.md
│   └── request-response-examples.md
│
├── validation-auth/
│   ├── validation-matrix.md
│   └── roles-permissions.md
│
├── performance-notes/
│   └── scaling.md
│
└── review/
    ├── intern-a-review.md
    └── intern-b-review.md

Missing or extra files will be considered incomplete submission.

### Purpose
Explain what this model represents in the system.

### Fields
| Field | Type | Required | Explanation |
|------|------|----------|-------------|

### Primary Key
- <field name>

### Relationships
- Explain how this model connects to others

Rules:
❌ No copy-paste from Prisma docs
❌ No "self-explanatory" answers
✅ A non-technical person should understand it


👤  TASKS (Backend / API / Business Logic)
✅ Task B1: System Understanding
Answer in plain English:
What kind of system is this?
Who are the users?
What problem does it solve?
Step-by-step user flow

✅ Task B2: API Design
Design REST APIs for each major model.
Required APIs:
Create
Read
Update
Delete
API Template:
POST /entities

Request:
{
  "field": "value"
}

Response:
{
  "id": "123",
  "field": "value"
}

Status Codes:
- 201 Created
- 400 Bad Request


✅ Task B3: Validation Matrix
Fill the table for each field:
Field
Frontend
Backend
Database
Reason


✅ Task B4: Roles & Permissions
Step 1: Define Roles
Admin
User
Viewer (if applicable)
Step 2: Permission Matrix
Role
Create
Read
Update
Delete



🏁 Final Deliverables
Complete schema documentation
ER diagram (PNG + source)
API documentation
Validation & authorization docs
Performance & scaling notes

🧠 Industry Practices & Concepts You Are Expected to Learn
This assignment is intentionally designed to mirror how real software companies work. Below are the industry-standard concepts that your tasks map to. You are expected to understand and apply them while completing your work.

1️⃣ Database Design & Data Modeling (Core Industry Skill)
Used by companies to ensure data is:
Consistent
Scalable
Easy to query
You are learning:
Entity–Relationship modeling (ERD)
Normalization (avoiding duplicate data)
Primary keys vs foreign keys
Nullable vs non-nullable fields
Enums vs free-text fields
Why companies care:
Bad schema design is extremely expensive to fix later.

2️⃣ ORM Usage (Prisma) — How Modern Backends Work
Modern companies rarely write raw SQL everywhere.
You are learning:
How ORMs abstract databases
How Prisma models map to SQL tables
Relations, indexes, and constraints in ORMs
Migration-based schema evolution
Why companies care:
ORMs reduce bugs, improve productivity, and enforce consistency.

3️⃣ API Design (Backend Contract Design)
APIs are contracts, not random functions.
You are learning:
RESTful API conventions
Resource-based URL design
Proper HTTP status codes
Request/response shaping
Why companies care:
APIs are consumed by frontend, mobile apps, and third parties.

4️⃣ Validation Strategy (Defense in Depth)
Real systems validate data at multiple layers.
You are learning:
Frontend validation (UX)
Backend validation (business rules)
Database validation (hard guarantees)
Why companies care:
Never trust client input.

5️⃣ Authorization & Security Basics
Every serious system controls who can do what.
You are learning:
Role-Based Access Control (RBAC)
Permission matrices
Protected vs public APIs
Why companies care:
Security bugs are business-ending bugs.

6️⃣ Performance & Scalability Thinking
Systems must work today and at scale.
You are learning:
Identifying high-growth tables
Indexing strategy
Read vs write-heavy operations
Recognizing slow queries
Why companies care:
Performance issues directly affect revenue and user trust.

7️⃣ Deletion Strategies (Very Important in Industry)
Companies rarely hard-delete data blindly.
You are learning:
Cascade delete
Restrict delete
Soft delete (isDeleted flag)
Why companies care:
Data loss is often irreversible.

8️⃣ Documentation as a First-Class Skill
Good engineers document systems clearly.
You are learning:
Writing clear technical documentation
Explaining complex systems simply
Creating diagrams that others can understand
Why companies care:
Code changes, documentation stays.

9️⃣ Cross-Team Review & Collaboration
Engineering is a team sport.
You are learning:
Reviewing others’ work
Giving constructive feedback
Aligning database and API decisions
Why companies care:
Most bugs are caused by miscommunication, not bad code.

🔟 Thinking Like a Software Engineer (Not a Student)
This assignment forces you to:
Ask "why" before "how"
Think about future scale
Consider real users
Make trade-offs
This is exactly how engineers are evaluated in real companies.

✅ Evaluation Criteria
Can you explain why a table exists?
Do ER diagrams match the schema exactly?
Are APIs realistic and complete?
Are validations correctly placed?
Can you think about scaling?
Passing this assignment means you understand real backend systems, not just college projects.
