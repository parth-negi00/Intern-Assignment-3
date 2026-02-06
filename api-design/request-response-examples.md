# Request & Response Examples

This document provides detailed JSON examples for the primary API endpoints defined in `api-list.md`.

---

## 1. Authentication & Users

### Register User
**Endpoint:** `POST /auth/register`

**Request Body:**
```json
{
  "email": "alex.dev@papercup-factory.com",
  "password": "SecurePassword123!",
  "first_name": "Alex",
  "last_name": "Dev",
  "username": "alexdev",
  "mobile": "+15550199"
}
Response (201 Created):

JSON
{
  "id": "user_cuid12345678",
  "email": "alex.dev@papercup-factory.com",
  "username": "alexdev",
  "status": "PENDING",
  "provider": "EMAIL",
  "createdAt": "2026-02-06T10:00:00Z"
}
Get My Profile
Endpoint: GET /users/me

Response (200 OK):

JSON
{
  "id": "user_cuid12345678",
  "email": "alex.dev@papercup-factory.com",
  "first_name": "Alex",
  "last_name": "Dev",
  "department": "Engineering",
  "organizationId": "org_cuid98765432",
  "status": "ACTIVE",
  "avatar": "[https://cdn.example.com/avatars/alex.jpg](https://cdn.example.com/avatars/alex.jpg)"
}
2. Organization Management
Create Organization
Endpoint: POST /organizations

Request Body:

JSON
{
  "name": "Global Papercup Dynamics",
  "ownerId": "user_cuid12345678"
}
Response (201 Created):

JSON
{
  "id": "org_cuid98765432",
  "name": "Global Papercup Dynamics",
  "ownerId": "user_cuid12345678",
  "createdAt": "2026-02-06T10:05:00Z"
}
Create Organization Unit (Department)
Endpoint: POST /organizations/:id/units

Request Body:

JSON
{
  "name": "Production Line A",
  "description": "Main assembly line for 8oz cups",
  "parentId": "unit_cuid_manufacturing",
  "sortOrder": 1,
  "isActive": true
}
Response (201 Created):

JSON
{
  "id": "unit_cuid55555",
  "name": "Production Line A",
  "organizationId": "org_cuid98765432",
  "level": 1,
  "parentId": "unit_cuid_manufacturing"
}
3. Human Resources (HR)
Create Employee Profile
Endpoint: POST /employees

Request Body:

JSON
{
  "userId": "user_cuid12345678",
  "employeeName": "Alex Dev",
  "gender": "MALE",
  "designation": "Senior Technician",
  "department": "Production",
  "dateOfJoining": "2024-01-15T00:00:00Z",
  "shiftType": "Morning",
  "bankName": "HDFC Bank",
  "bankAccountNo": "1234567890",
  "totalSalary": 75000.00,
  "status": "ACTIVE"
}
Response (201 Created):

JSON
{
  "id": "emp_cuid444444",
  "userId": "user_cuid12345678",
  "employeeName": "Alex Dev",
  "designation": "Senior Technician",
  "status": "ACTIVE",
  "createdAt": "2026-02-06T10:10:00Z"
}
Mark Attendance (Check-In)
Endpoint: POST /attendance/check-in

Request Body:

JSON
{
  "date": "2026-02-06",
  "checkInTime": "09:00:00",
  "ipAddress": "192.168.1.55",
  "notes": "Arrived on time"
}
Response (200 OK):

JSON
{
  "id": "att_cuid333333",
  "userId": "user_cuid12345678",
  "date": "2026-02-06",
  "checkedIn": true,
  "checkInTime": "09:00:00",
  "checkedOut": false
}
4. Payroll Engine
Trigger Payroll Run
Endpoint: POST /payroll/run

Request Body:

JSON
{
  "month": 1,
  "year": 2026,
  "organizationId": "org_cuid98765432",
  "processAsync": true
}
Response (202 Accepted):

JSON
{
  "message": "Payroll calculation started for Jan 2026.",
  "jobId": "job_payroll_001"
}
Get Payroll Record (Payslip)
Endpoint: GET /payroll-records/:id

Response (200 OK):

JSON
{
  "id": "pay_cuid222222",
  "employeeId": "emp_cuid444444",
  "month": 1,
  "year": 2026,
  "baseSalary": 75000.00,
  "presentDays": 22,
  "leaveDays": 1,
  "overtimeHours": 5.5,
  "allowances": {
    "HRA": 15000,
    "Transport": 2000
  },
  "deductions": 4000.00,
  "netSalary": 88000.00,
  "status": "pending"
}
5. Form Builder
Create Form
Endpoint: POST /forms

Request Body:

JSON
{
  "moduleId": "mod_inventory_01",
  "name": "Daily Machine Inspection",
  "description": "Checklist for start-of-shift machine status",
  "isEmployeeForm": true,
  "allowAnonymous": false
}
Response (201 Created):

JSON
{
  "id": "form_cuid_machine_check",
  "name": "Daily Machine Inspection",
  "isPublished": false,
  "tableMapping": null
}
Add Field to Form
Endpoint: POST /forms/:id/fields

Request Body:

JSON
{
  "sectionId": "sec_general_info",
  "type": "NUMBER",
  "label": "Oil Pressure Level",
  "placeholder": "Enter PSI",
  "validation": {
    "min": 100,
    "max": 500,
    "required": true
  },
  "order": 1
}
Response (201 Created):

JSON
{
  "id": "field_cuid_oil_pressure",
  "label": "Oil Pressure Level",
  "type": "NUMBER",
  "visible": true
}
6. Form Data (Dynamic)
Submit Form Record
Endpoint: POST /forms/:formId/records

Request Body: Note: The recordData object is dynamic and matches the fields created in the Form Builder.

JSON
{
  "recordData": {
    "machine_id": "MAC-5501",
    "oil_pressure": 320,
    "vibration_check": "PASS",
    "inspected_by": "Alex Dev",
    "photo_proof": "[https://s3.aws.com/uploads/img_55.jpg](https://s3.aws.com/uploads/img_55.jpg)"
  },
  "status": "submitted",
  "ipAddress": "10.0.0.1"
}
Response (201 Created):

JSON
{
  "id": "rec_cuid_submission_999",
  "formId": "form_cuid_machine_check",
  "submittedAt": "2026-02-06T10:30:00Z",
  "submittedBy": "user_cuid12345678",
  "recordData": {
    "machine_id": "MAC-5501",
    "oil_pressure": 320,
    "vibration_check": "PASS"
  }
}
Update Form Record
Endpoint: PUT /forms/:formId/records/:recordId

Request Body:

JSON
{
  "recordData": {
    "oil_pressure": 325,
    "comments": "Retested pressure, sensor was loose."
  }
}
Response (200 OK):

JSON
{
  "id": "rec_cuid_submission_999",
  "updatedAt": "2026-02-06T11:15:00Z",
  "recordData": {
    "machine_id": "MAC-5501",
    "oil_pressure": 325,
    "vibration_check": "PASS",
    "comments": "Retested pressure, sensor was loose."
  }
}
7. Data Operations
Create Import Job
Endpoint: POST /jobs/import

Request Body:

JSON
{
  "moduleId": "mod_inventory_01",
  "formId": "form_cuid_machine_check",
  "fileUrl": "[https://s3.aws.com/uploads/legacy_data.csv](https://s3.aws.com/uploads/legacy_data.csv)",
  "duplicateHandling": "UPSERT"
}
Response (201 Created):

JSON
{
  "id": "job_import_777",
  "status": "PENDING",
  "totalRows": 0,
  "createdAt": "2026-02-06T12:00:00Z"
}