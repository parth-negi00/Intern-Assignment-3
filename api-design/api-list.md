# API Endpoint Index

This document provides a high-level overview of the available REST API endpoints for the ERP System.

## 1. Authentication & User Management
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/auth/register` | Register a new user account | Public |
| POST | `/auth/login` | Login using Email/Password or Provider | Public |
| POST | `/auth/refresh` | Refresh access token using session | Public |
| POST | `/auth/verify-otp` | Verify email or 2FA OTP code | Public |
| GET | `/users/me` | Get profile details of the current logged-in user | Authenticated User |
| PUT | `/users/me` | Update personal profile (avatar, phone) | Authenticated User |
| GET | `/users/:id/sessions` | View active login sessions | User / Admin |

## 2. Organization & Governance
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/organizations` | Create a new tenant organization | Authenticated User |
| GET | `/organizations/:id` | Get organization details and settings | Org Member |
| PUT | `/organizations/:id` | Update organization name or settings | Owner / Admin |
| POST | `/organizations/:id/units` | Create a department or business unit | Admin |
| GET | `/organizations/:id/roles` | List defined roles and hierarchy | Org Member |
| POST | `/organizations/:id/invite` | Invite a user to the organization | Admin |
| PUT | `/organizations/:id/users/:userId/role` | Assign a role (RBAC) to a user | Admin |

## 3. Human Resources (HR)
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/employees` | Create a new employee profile linked to a User | HR / Admin |
| GET | `/employees` | List all employees (supports filtering by Dept) | HR / Admin / Manager |
| GET | `/employees/:id` | Get sensitive details (Salary, Bank Info) | HR / Admin |
| PUT | `/employees/:id` | Update employment status or salary | HR / Admin |
| GET | `/attendance/my-history` | Get current user's attendance logs | Employee |
| POST | `/attendance/check-in` | Mark daily check-in (creates Attendance record) | Employee |
| POST | `/attendance/check-out` | Mark daily check-out | Employee |

## 4. Payroll Engine
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| GET | `/payroll/configurations` | Get global settings for salary calculation | HR Admin |
| POST | `/payroll/run` | Trigger monthly payroll calculation for a set of employees | HR Admin |
| GET | `/payroll-records` | List generated payroll records (payslips) | HR Admin |
| GET | `/payroll-records/:id` | View specific payslip details (allowances/deductions) | Owner / HR |
| PATCH | `/payroll-records/:id/status` | Mark record as 'Processed' or 'Paid' | Finance / Owner |

## 5. Form Builder (App Configuration)
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/modules` | Create a new application module (folder) | Admin |
| POST | `/forms` | Create a new Form definition | Admin |
| GET | `/forms/:id` | Get Form schema (fields, sections, logic) | Authenticated User |
| PUT | `/forms/:id` | Update form structure (add fields/sections) | Admin |
| POST | `/forms/:id/publish` | Publish form (activates Table Mapping) | Admin |
| GET | `/forms/:id/fields` | List all fields and their validation rules | Developer / Admin |

## 6. Form Data (Dynamic Records)
*Note: These endpoints route data to tables `FormRecord1` through `FormRecord15` based on internal mapping.*

| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/forms/:formId/records` | Submit a new record to a dynamic form | User (with Write Perms) |
| GET | `/forms/:formId/records` | List records for a specific form (supports filters) | User (with Read Perms) |
| GET | `/forms/:formId/records/:recordId` | Get a specific record's JSON data | User (with Read Perms) |
| PUT | `/forms/:formId/records/:recordId` | Update an existing record | User (with Edit Perms) |
| DELETE | `/forms/:formId/records/:recordId` | Delete a record | Admin / Manager |

## 7. Data Operations & Compliance
| Method | Endpoint | Description | Access Level |
| :--- | :--- | :--- | :--- |
| POST | `/jobs/import` | Upload CSV/JSON for bulk import | Admin |
| GET | `/jobs/import/:id` | Check status of import job (Rows processed/failed) | Admin |
| POST | `/jobs/export` | Trigger bulk export of Form Data to CSV | Admin |
| GET | `/audit-logs` | View system-wide action history | Security Admin |
| GET | `/activities` | View recent user activity feed | Manager / Admin |