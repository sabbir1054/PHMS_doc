# Police Pharmacy Management System - Complete Documentation

## Table of Contents
1. [Project Overview](#project-overview)
2. [System Architecture](#system-architecture)
3. [Technology Stack](#technology-stack)
4. [Database Schema](#database-schema)
5. [Backend API Documentation](#backend-api-documentation)
6. [Frontend Architecture](#frontend-architecture)
7. [Features & Functionality](#features--functionality)
8. [Authentication & Authorization](#authentication--authorization)
9. [Installation & Setup](#installation--setup)
10. [API Endpoints Reference](#api-endpoints-reference)
11. [Component Documentation](#component-documentation)
12. [State Management](#state-management)
13. [User Roles & Permissions](#user-roles--permissions)
14. [Reporting System](#reporting-system)
15. [File Upload System](#file-upload-system)
16. [Development Guidelines](#development-guidelines)

---
### Application Screenshots

#### Dashboard View
![Dashboard Screenshot](https://i.ibb.co.com/vxJSsdnJ/1PHN.png)

#### Medicine Distribution
![Distribution Screenshot](https://i.ibb.co.com/qMw7CCRk/2PHN.png)

#### Medicine Inventory Management
![Medicine Management Screenshot](https://i.ibb.co.com/bMqj5pPg/3PHN.png)

#### Reports Interface
![Reports Screenshot](https://i.ibb.co.com/dsNL0fMv/4PHN.png)

## Project Overview

### Description
A comprehensive pharmacy management system designed specifically for **Bangladesh Police Hospital Narayanganj**. The system manages medicine inventory, police personnel records, medicine distribution, committee management, quota tracking, leave management, and comprehensive reporting.

### Project Purpose
- Streamline medicine distribution to police personnel and their families
- Track medicine inventory and stock levels
- Manage medicine committees and procurement
- Generate various reports for administrative purposes
- Maintain quota limits for controlled medicine distribution
- Track police personnel and their medical records

### Key Capabilities
- **Medicine Management**: Complete inventory control with stock alerts
- **Distribution System**: Controlled medicine distribution with quota management
- **Personnel Management**: Police officer records with photo uploads
- **Committee Management**: Medicine procurement committee tracking
- **Reporting**: Multiple report types (Daily, Monthly, Custom, DIG)
- **Leave Management**: Track leave records for police personnel
- **User Management**: Role-based access control with three user levels
- **Bulk Operations**: CSV/Excel import for medicine and police data

---

## System Architecture

### Architecture Overview
```
┌─────────────────────────────────────────────────────────────┐
│                      Client Layer                           │
│  React SPA + Redux Toolkit + Material-UI + React Router    │
└─────────────────┬───────────────────────────────────────────┘
                  │ HTTP/REST API
                  │ JWT Authentication
┌─────────────────▼───────────────────────────────────────────┐
│                    API Gateway Layer                        │
│              Express.js + CORS + Middleware                 │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│                  Business Logic Layer                       │
│    Controllers → Services → Validation → Authorization     │
└─────────────────┬───────────────────────────────────────────┘
                  │
┌─────────────────▼───────────────────────────────────────────┐
│                  Data Access Layer                          │
│              Prisma ORM + PostgreSQL                        │
└─────────────────────────────────────────────────────────────┘
```

### Project Structure

#### Backend Structure (`police_pharmacy_backned/`)
```
police_pharmacy_backned/
├── src/
│   ├── app/
│   │   ├── middlewares/         # Authentication, validation, error handling
│   │   │   ├── auth.ts
│   │   │   ├── validateRequest.ts
│   │   │   ├── globalErrorHandler.ts
│   │   │   └── fileUpload.ts
│   │   ├── module/              # Feature modules
│   │   │   ├── auth/            # Authentication module
│   │   │   ├── users/           # User management
│   │   │   ├── polices/         # Police personnel management
│   │   │   ├── committee/       # Committee management
│   │   │   ├── medicine/        # Medicine inventory
│   │   │   ├── medicineSet/     # Predefined medicine sets
│   │   │   ├── order/           # Distribution orders
│   │   │   ├── leave/           # Leave management
│   │   │   └── sitting/         # System settings
│   │   └── routes/              # Route aggregation
│   ├── config/                  # Configuration files
│   ├── helpers/                 # Utility functions
│   ├── shared/                  # Shared utilities (Prisma client)
│   ├── app.ts                   # Express app setup
│   └── server.ts                # Server entry point
├── prisma/
│   ├── schema.prisma            # Database schema
│   ├── seed.ts                  # Database seeding
│   └── verify-seed.ts           # Seed verification
├── uploads/                     # File upload directory
├── package.json
├── tsconfig.json
└── .env
```

#### Frontend Structure (`police_pharmacy_frontend/`)
```
police_pharmacy_frontend/
├── src/
│   ├── axios/                   # API configuration
│   │   ├── axiosinstance.js     # Axios interceptors & config
│   │   └── axiosBaseQuery.js    # RTK Query integration
│   ├── components/              # React components
│   │   ├── common/              # Shared UI components
│   │   ├── dashboard/           # Dashboard components
│   │   ├── Pharmacy/            # Medicine management
│   │   ├── Polices/             # Police management
│   │   ├── Patient/             # Distribution components
│   │   ├── Committee/           # Committee components
│   │   ├── Users/               # User management
│   │   ├── Reports/             # Report components
│   │   ├── Sittings/            # Settings components
│   │   └── forms/               # Form components
│   ├── pages/                   # Page components
│   │   ├── Dashboard/           # Dashboard pages
│   │   ├── login/               # Login page
│   │   └── NotFound/            # 404 page
│   ├── redux/                   # State management
│   │   ├── api/                 # RTK Query API slices
│   │   │   ├── authApi.js
│   │   │   ├── medicineApi.js
│   │   │   ├── policeApi.js
│   │   │   ├── committeeApi.js
│   │   │   ├── orderApi.js
│   │   │   ├── reportApi.js
│   │   │   ├── usersApi.js
│   │   │   ├── medicineSetApi.js
│   │   │   ├── leaveApi.js
│   │   │   └── sittingsApi.js
│   │   ├── store.js             # Redux store
│   │   └── rootReducer.js       # Root reducer
│   ├── routes/                  # Route configuration
│   │   └── Routes.jsx
│   ├── utils/                   # Utility functions
│   ├── constants/               # Constants
│   ├── config/                  # Environment config
│   ├── layout/                  # Layout components
│   ├── App.jsx                  # App root
│   └── main.jsx                 # Entry point
├── public/                      # Static assets
├── package.json
├── vite.config.js
└── .env
```

---

## Technology Stack

### Backend Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **Node.js** | Latest | Runtime environment |
| **TypeScript** | ^5.4.5 | Type-safe development |
| **Express.js** | ^4.19.2 | Web framework |
| **Prisma ORM** | ^6.4.1 | Database ORM |
| **PostgreSQL** | - | Relational database |
| **JWT** | ^9.0.2 | Authentication tokens |
| **bcrypt** | ^5.1.1 | Password hashing |
| **Zod** | ^3.23.8 | Schema validation |
| **Multer** | ^1.4.5 | File uploads |
| **CORS** | ^2.8.5 | Cross-origin requests |
| **cookie-parser** | ^1.4.6 | Cookie handling |
| **http-status** | ^1.7.4 | HTTP status codes |
| **xlsx** | ^0.18.5 | Excel file processing |
| **csv-parser** | ^3.2.0 | CSV file processing |
| **nodemailer** | ^6.10.0 | Email sending |
| **uuid** | ^9.0.1 | Unique ID generation |

### Frontend Technologies
| Technology | Version | Purpose |
|------------|---------|---------|
| **React** | ^18.3.1 | UI framework |
| **Vite** | ^5.4.1 | Build tool |
| **Redux Toolkit** | ^2.2.7 | State management |
| **RTK Query** | Included | API client |
| **React Router** | 6 | Routing |
| **Material-UI** | ^6.1.1 | UI components |
| **Emotion** | ^11.13.3 | CSS-in-JS |
| **Axios** | ^1.7.7 | HTTP client |
| **React Hook Form** | ^7.53.0 | Form handling |
| **jwt-decode** | ^4.0.0 | JWT decoding |
| **react-hot-toast** | ^2.4.1 | Notifications |
| **sweetalert2** | ^11.14.3 | Alert dialogs |
| **html2pdf.js** | ^0.12.0 | PDF generation |
| **recharts** | ^2.12.7 | Data visualization |
| **react-dropzone** | ^14.3.8 | File uploads |

### Development Tools
- **ESLint**: Code linting
- **Prettier**: Code formatting
- **Husky**: Git hooks
- **lint-staged**: Pre-commit linting
- **ts-node-dev**: TypeScript development server

---

## Database Schema

### Entity Relationship Diagram Overview
```
User ──────────────────────────────────────────────────────
(Authentication & Authorization)

Police ─────┬─── Order ─────── OrderItem ─────── Medicine
            ├─── Leave                              │
            ├─── MonthlyQuotaUsage ─────────────────┤
            └─── DynamicQuotaUsage ─────────────────┤
                                                     │
Committee ──┬─── Medicine ────────────────────────────┤
            └─── Medicine_Set ─── MedicineSetItem ───┘

Sitting (System Settings - Singleton)
```

### Database Models

#### 1. User Model
**Purpose**: User authentication and authorization

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| role | ROLE | Required | User role (SUPER_ADMIN, ADMIN, NORMAL_USER) |
| name | String | Required | User full name |
| username | String | Unique | Login username |
| mobile | String | Optional | Mobile number |
| email | String | Optional | Email address |
| password | String | Required | Hashed password |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Enums**:
- `ROLE`: SUPER_ADMIN, ADMIN, NORMAL_USER

---

#### 2. Police Model
**Purpose**: Police personnel records

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| name | String | Required | Police officer name |
| rank | RANK | Required | Police rank |
| rankSerial | Int | Unique, Auto-increment | Rank-based serial number |
| bpCode | String | Unique | BP (Bangladesh Police) code |
| workStation | String | Optional | Work posting location |
| age | Int | Optional | Age |
| bloodGroup | BLOOD_GROUP | Optional | Blood group |
| mobile | String | Unique | Mobile number |
| photo | String | Optional | Photo file path |
| isActive | Boolean | Default: true | Active status |
| deletedAt | DateTime | Optional | Soft delete timestamp |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `Order[]` - Medicine orders
- `Leave[]` - Leave records
- `MonthlyQuotaUsage[]` - Monthly quota tracking
- `DynamicQuotaUsage[]` - Dynamic quota tracking

**Enums**:
- `RANK`: IGP, Addl_IGP, DIG, Addl_DIG, SP, Addl_SP, ASP, Inspector_UB, Inspector_AB, Inspector_TI, SI_UB, SI_AB, Sergeant, TSI, ASI_UB, ASI_AB, ATSI, Naik, Constable, Civil_staff, Others
- `BLOOD_GROUP`: A_POS, A_NEG, B_POS, B_NEG, AB_POS, AB_NEG, O_POS, O_NEG

---

#### 3. Committee Model
**Purpose**: Medicine procurement committee management

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| name | String | Unique | Committee name |
| lotCode | String | Unique | Lot code for tracking |
| isRunning | Boolean | Default: false | Running status |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `Medicine[]` - Medicines under committee
- `Medicine_Set[]` - Medicine sets for committee

---

#### 4. Medicine Model
**Purpose**: Medicine inventory management

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| name | String | Required | Medicine name |
| code | Int | Required | Medicine code |
| quantity | Int | Required | Stock quantity |
| mrp | Float | Optional | Maximum retail price |
| quota | Int | Optional | Monthly quota limit |
| quotaDays | Int | Optional | Dynamic quota period (days) |
| isArchived | Boolean | Required | Archive status |
| committeeId | String | FK | Reference to Committee |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `committee` - Parent committee
- `OrderItem[]` - Order items
- `MonthlyQuotaUsage[]` - Monthly quota usage
- `DynamicQuotaUsage[]` - Dynamic quota usage
- `MedicineSetItem[]` - Medicine set memberships

**Indexes**: `committeeId`, `isArchived`, `code`

---

#### 5. Order Model
**Purpose**: Medicine distribution orders

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| orderCode | String | Unique | Order reference code |
| orderType | ORDER_TYPE | Default: DISTRIBUTION | Order type |
| policeId | String | FK (Optional) | Reference to Police |
| medicineFor | POLICE_RELATIVES | Default: SELF | Medicine recipient |
| orderedBy | String | Default: "" | User who created order |
| createdAt | DateTime | Auto | Order timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `police` - Police officer (optional)
- `orderItems[]` - Order line items

**Enums**:
- `ORDER_TYPE`: DISTRIBUTION, MANUAL_ADJUSTMENT
- `POLICE_RELATIVES`: SELF, FATHER, MOTHER, WIFE, SON, DAUGHTER, HUSBAND

**Indexes**: `policeId`, `createdAt`, `orderType`

---

#### 6. OrderItem Model
**Purpose**: Individual items in an order

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| quantity | Int | Required | Item quantity |
| medicineId | String | FK | Reference to Medicine |
| orderId | String | FK | Reference to Order |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `medicine` - Medicine item
- `Order` - Parent order

**Cascade Rules**:
- On Order delete: CASCADE
- On Medicine delete: RESTRICT

---

#### 7. MonthlyQuotaUsage Model
**Purpose**: Track monthly medicine quota usage per police officer

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| policeId | String | FK | Reference to Police |
| medicineId | String | FK | Reference to Medicine |
| year | Int | Required | Year |
| month | Int | Required | Month (1-12) |
| usedQuota | Int | Default: 0 | Quota used in month |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `police` - Police officer
- `medicine` - Medicine

**Unique Constraint**: `[policeId, medicineId, year, month]`
**Indexes**: `[policeId, year, month]`

---

#### 8. DynamicQuotaUsage Model
**Purpose**: Track quota usage for configurable time periods

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| policeId | String | FK | Reference to Police |
| medicineId | String | FK | Reference to Medicine |
| periodStart | DateTime | Required | Quota period start date |
| periodEnd | DateTime | Required | Quota period end date |
| quotaDays | Int | Required | Number of days in period |
| usedQuota | Int | Default: 0 | Quota used in period |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `police` - Police officer
- `medicine` - Medicine

**Unique Constraint**: `[policeId, medicineId, periodStart]`
**Indexes**: `[policeId, medicineId, periodStart, periodEnd]`

---

#### 9. Medicine_Set Model
**Purpose**: Predefined sets of medicines (e.g., diabetes kit)

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| name | String | Required | Set name |
| committeeId | String | FK | Reference to Committee |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `committee` - Parent committee
- `medicines[]` - Medicine items in set

---

#### 10. MedicineSetItem Model
**Purpose**: Junction table for Medicine_Set and Medicine

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| medicineId | String | FK | Reference to Medicine |
| medicineSetId | String | FK | Reference to Medicine_Set |
| createdAt | DateTime | Auto | Creation timestamp |

**Relations**:
- `medicine` - Medicine item
- `medicineSet` - Medicine set

**Unique Constraint**: `[medicineId, medicineSetId]`
**Cascade**: DELETE on both relations

---

#### 11. Leave Model
**Purpose**: Leave records for police personnel

| Field | Type | Constraints | Description |
|-------|------|-------------|-------------|
| id | String | PK, UUID | Unique identifier |
| description | String | Optional | Leave description |
| address | String | Optional | Leave address |
| posting | String | Optional | Posting location |
| father | String | Optional | Father's name |
| mother | String | Optional | Mother's name |
| startDate | String | Required | Leave start date |
| endDate | String | Required | Leave end date |
| policeId | String | FK | Reference to Police |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Relations**:
- `police` - Police officer

**Indexes**: `policeId`
**Cascade**: DELETE

---

#### 12. Sitting Model
**Purpose**: System settings (singleton - only one record)

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| id | Int | 1 (PK) | Fixed ID |
| pageWidth | Int | 241 | Print page width (mm) |
| pageHeight | Int | 105 | Print page height (mm) |
| leftGap | Int | 15 | Left margin |
| fontSize | Int | 10 | Print font size |
| name | Int | 48 | Name field width |
| serial | Int | 15 | Serial field width |
| quantity | Int | 15 | Quantity field width |
| border | ON_OFF_STATUS | On | Border display |
| monthlyReport | ON_OFF_STATUS | On | Monthly report setting |
| dailyReport | ON_OFF_STATUS | On | Daily report setting |
| digReport | ON_OFF_STATUS | On | DIG report setting |
| lineHeight | Int | 5 | Print line height |
| extraQuota | SHOW_HIDE_STATUS | Show | Extra quota display |
| ageOrDate | DATE_AGE_STATUS | Date | Age/Date display mode |
| totalOnSlip | YES_NO_STATUS | No | Total on slip display |
| createdAt | DateTime | Auto | Creation timestamp |
| updatedAt | DateTime | Auto | Update timestamp |

**Enums**:
- `ON_OFF_STATUS`: On, Off
- `SHOW_HIDE_STATUS`: Show, Hide
- `DATE_AGE_STATUS`: Date, Age
- `YES_NO_STATUS`: Yes, No

---

## Backend API Documentation

### Base URL
```
http://localhost:5000/api/v1
```

### Authentication
Most endpoints require JWT authentication via the `Authorization` header:
```
Authorization: <access_token>
```

### Response Format
All API responses follow this structure:
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

Error responses:
```json
{
  "success": false,
  "message": "Error message",
  "errorMessages": [
    {
      "path": "field_name",
      "message": "Error detail"
    }
  ]
}
```

---

## API Endpoints Reference

### 1. Authentication Module (`/api/v1/auth`)

#### Register User
```http
POST /api/v1/auth/register
Authorization: Required (SUPER_ADMIN, ADMIN)
```
**Request Body**:
```json
{
  "name": "John Doe",
  "username": "johndoe",
  "password": "password123",
  "role": "ADMIN",
  "mobile": "01712345678",
  "email": "john@example.com"
}
```

#### Register Super Admin
```http
POST /api/v1/auth/register-s-admin
Authorization: Required (SUPER_ADMIN only)
```

#### Login
```http
POST /api/v1/auth/login
Authorization: Not Required
```
**Request Body**:
```json
{
  "username": "johndoe",
  "password": "password123"
}
```
**Response**:
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "uuid",
      "name": "John Doe",
      "role": "ADMIN"
    }
  }
}
```

#### Refresh Token
```http
GET /api/v1/auth/refresh-token
Authorization: Not Required (uses cookie)
```

#### Change Password
```http
PATCH /api/v1/auth/change-password
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "oldPassword": "oldpass123",
  "newPassword": "newpass456"
}
```

#### Reset User Password
```http
PATCH /api/v1/auth/resetPassword/:resetId
Authorization: Required (SUPER_ADMIN, ADMIN)
```

---

### 2. User Module (`/api/v1/user`)

#### Get All Users
```http
GET /api/v1/user
Authorization: Required (SUPER_ADMIN, ADMIN)
```

#### Get User Profile
```http
GET /api/v1/user/profile
Authorization: Required (All roles)
```

#### Update Profile
```http
PATCH /api/v1/user/update-profile
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "name": "Updated Name",
  "mobile": "01712345678",
  "email": "updated@example.com"
}
```

#### Delete User
```http
DELETE /api/v1/user/delete-profile/:userId
Authorization: Required (SUPER_ADMIN, ADMIN)
```

---

### 3. Police Module (`/api/v1/police`)

#### Add Police Officer
```http
POST /api/v1/police/addPolice
Authorization: Required (All roles)
Content-Type: multipart/form-data
```
**Form Data**:
```
name: "Officer Name"
rank: "Inspector_UB"
bpCode: "BP12345"
mobile: "01712345678"
workStation: "Dhaka"
age: 35
bloodGroup: "A_POS"
file: [image file]
```

#### Get All Police Officers
```http
GET /api/v1/police?page=1&limit=10&searchTerm=name&rank=Inspector
Authorization: Required (All roles)
```
**Query Parameters**:
- `page`: Page number (default: 1)
- `limit`: Records per page (default: 10)
- `searchTerm`: Search by name, BP code, mobile, workstation
- `rank`: Filter by rank
- `sortBy`: Sort field
- `sortOrder`: asc or desc

#### Get Police by ID
```http
GET /api/v1/police/getPolice/:id
Authorization: Required (All roles)
```

#### Update Police Officer
```http
PATCH /api/v1/police/updatePolice/:id
Authorization: Required (All roles)
Content-Type: multipart/form-data
```

#### Delete Police Officer
```http
DELETE /api/v1/police/deletePolice/:id
Authorization: Required (All roles)
```

#### Get Police Photo
```http
GET /api/v1/police/image/:fileName
Authorization: Not Required
```

#### Remove Police Photo
```http
DELETE /api/v1/police/removePolicePhoto/:id
Authorization: Not Required
```

#### Get Last Order
```http
GET /api/v1/police/lastOrder/:id
Authorization: Required (All roles)
```

#### Check Quota
```http
GET /api/v1/police/checkQuota/:policeId?medicineId=uuid
Authorization: Required (All roles)
```

---

### 4. Committee Module (`/api/v1/committee`)

#### Add Committee
```http
POST /api/v1/committee/add
Authorization: Required (ADMIN, SUPER_ADMIN)
```
**Request Body**:
```json
{
  "name": "Committee 2024-01",
  "lotCode": "LOT001"
}
```

#### Get All Committees
```http
GET /api/v1/committee/getAll
Authorization: Required (All roles)
```

#### Get Committee Details
```http
GET /api/v1/committee/details/:id
Authorization: Required (All roles)
```

#### Update Committee
```http
PATCH /api/v1/committee/update/:id
Authorization: Required (SUPER_ADMIN)
```
**Request Body**:
```json
{
  "name": "Updated Name",
  "lotCode": "LOT002"
}
```

#### Delete Committee
```http
DELETE /api/v1/committee/delete/:id
Authorization: Required (SUPER_ADMIN)
```

#### Update Committee Status
```http
PATCH /api/v1/committee/statusUpdate/:id
Authorization: Required (ADMIN, SUPER_ADMIN)
```
**Request Body**:
```json
{
  "isRunning": true
}
```

#### Merge Medicines
```http
POST /api/v1/committee/mergeMedicines
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "sourceCommitteeId": "uuid",
  "targetCommitteeId": "uuid",
  "medicineIds": ["uuid1", "uuid2"]
}
```

---

### 5. Medicine Module (`/api/v1/medicine`)

#### Add Medicine
```http
POST /api/v1/medicine/add
Authorization: Required (SUPER_ADMIN, ADMIN)
```
**Request Body**:
```json
{
  "name": "Paracetamol 500mg",
  "code": 101,
  "quantity": 1000,
  "mrp": 5.50,
  "quota": 10,
  "quotaDays": 30,
  "committeeId": "uuid"
}
```

#### Get All Medicines
```http
GET /api/v1/medicine?page=1&limit=10&searchTerm=para&committeeId=uuid
Authorization: Required (All roles)
```
**Query Parameters**:
- `page`: Page number
- `limit`: Records per page
- `searchTerm`: Search by name or code
- `committeeId`: Filter by committee
- `isArchived`: Filter archived medicines

#### Get Medicine Details
```http
GET /api/v1/medicine/details/:id
Authorization: Required (All roles)
```

#### Update Medicine
```http
PATCH /api/v1/medicine/update/:id
Authorization: Required (SUPER_ADMIN, ADMIN)
```
**Request Body**:
```json
{
  "name": "Updated Name",
  "quantity": 1500,
  "mrp": 6.00,
  "quota": 15
}
```

#### Delete Medicine
```http
DELETE /api/v1/medicine/delete/:id
Authorization: Required (SUPER_ADMIN, ADMIN)
```

#### Adjust Stock
```http
POST /api/v1/medicine/adjust-stock
Authorization: Required (SUPER_ADMIN)
```
**Request Body**:
```json
{
  "medicineId": "uuid",
  "adjustment": -50,
  "reason": "Damaged stock"
}
```

#### Bulk Upload Medicines
```http
POST /api/v1/medicine/bulk-upload
Authorization: Required (SUPER_ADMIN, ADMIN)
Content-Type: multipart/form-data
```
**Form Data**:
```
file: [CSV/Excel file]
committeeId: "uuid"
```

#### Get Low Stock Medicines (Under 10)
```http
GET /api/v1/medicine/low-stock/under-10
Authorization: Required (All roles)
```

#### Get Low Stock Medicines (Under 100)
```http
GET /api/v1/medicine/low-stock/under-100
Authorization: Required (All roles)
```

#### Get Running Committee Medicines (Alphabetical)
```http
GET /api/v1/medicine/running-committee/alphabetical
Authorization: Required (All roles)
```

#### Generate Monthly Report PDF
```http
POST /api/v1/medicine/monthlyReportPDF
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "year": 2024,
  "month": 10
}
```

#### Generate Custom Date Range Report PDF
```http
POST /api/v1/medicine/customDateRangeReportPDF
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "startDate": "2024-10-01",
  "endDate": "2024-10-31"
}
```

---

### 6. Medicine Set Module (`/api/v1/medicine-set`)

#### Create Medicine Set
```http
POST /api/v1/medicine-set
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "name": "Diabetes Care Kit",
  "committeeId": "uuid"
}
```

#### Get All Medicine Sets
```http
GET /api/v1/medicine-set
Authorization: Required (All roles)
```

#### Get Medicine Set by ID
```http
GET /api/v1/medicine-set/:id
Authorization: Required (All roles)
```

#### Update Medicine Set
```http
PATCH /api/v1/medicine-set/:id
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "name": "Updated Kit Name"
}
```

#### Delete Medicine Set
```http
DELETE /api/v1/medicine-set/:id
Authorization: Required (All roles)
```

#### Add Medicine to Set
```http
POST /api/v1/medicine-set/:id/add-medicine
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "medicineId": "uuid"
}
```

#### Remove Medicine from Set
```http
POST /api/v1/medicine-set/:id/remove-medicine
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "medicineId": "uuid"
}
```

#### Add Multiple Medicines to Set
```http
POST /api/v1/medicine-set/:id/add-multiple-medicines
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "medicineIds": ["uuid1", "uuid2", "uuid3"]
}
```

---

### 7. Order Module (`/api/v1/order`)

#### Create Order
```http
POST /api/v1/order/create
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "policeId": "uuid",
  "medicineFor": "SELF",
  "orderType": "DISTRIBUTION",
  "orderItems": [
    {
      "medicineId": "uuid",
      "quantity": 5
    }
  ]
}
```

#### Get All Orders
```http
GET /api/v1/order/getAll?page=1&limit=10&startDate=2024-10-01&endDate=2024-10-31
Authorization: Required (All roles)
```
**Query Parameters**:
- `page`: Page number
- `limit`: Records per page
- `startDate`: Filter start date
- `endDate`: Filter end date
- `policeId`: Filter by police officer
- `orderType`: Filter by order type

#### Get Order Details
```http
GET /api/v1/order/details/:id
Authorization: Required (All roles)
```

#### Get Dashboard Statistics
```http
GET /api/v1/order/dashboard-statistics
Authorization: Required (All roles)
```
**Response**:
```json
{
  "success": true,
  "data": {
    "totalOrders": 1250,
    "todayOrders": 45,
    "totalMedicines": 350,
    "lowStockMedicines": 12,
    "totalPolice": 1500,
    "activePolice": 1450
  }
}
```

#### Generate Monthly Report
```http
POST /api/v1/order/monthlyReport
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "year": 2024,
  "month": 10
}
```

#### Generate Daily Report
```http
POST /api/v1/order/dailyReport
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "date": "2024-10-07"
}
```

#### Generate Custom Report
```http
POST /api/v1/order/customReport
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "startDate": "2024-10-01",
  "endDate": "2024-10-31"
}
```

#### Generate DIG Report
```http
POST /api/v1/order/digReport
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "year": 2024,
  "month": 10
}
```

---

### 8. Leave Module (`/api/v1/leave`)

#### Add Leave
```http
POST /api/v1/leave/addLeave
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "policeId": "uuid",
  "startDate": "2024-10-15",
  "endDate": "2024-10-20",
  "description": "Medical leave",
  "address": "Dhaka",
  "posting": "Headquarters",
  "father": "Father Name",
  "mother": "Mother Name"
}
```

#### Get All Leaves
```http
GET /api/v1/leave
Authorization: Required (All roles)
```

#### Get Leave by ID
```http
GET /api/v1/leave/getLeave/:id
Authorization: Required (All roles)
```

#### Get Leaves by Police ID
```http
GET /api/v1/leave/police/:policeId
Authorization: Required (All roles)
```

#### Update Leave
```http
PATCH /api/v1/leave/updateLeave/:id
Authorization: Required (SUPER_ADMIN, ADMIN)
```
**Request Body**:
```json
{
  "startDate": "2024-10-16",
  "endDate": "2024-10-21",
  "description": "Updated description"
}
```

#### Delete Leave
```http
DELETE /api/v1/leave/deleteLeave/:id
Authorization: Required (All roles)
```

---

### 9. Settings Module (`/api/v1/sittings`)

#### Get Settings
```http
GET /api/v1/sittings
Authorization: Required (All roles)
```

#### Update Settings
```http
PATCH /api/v1/sittings
Authorization: Required (All roles)
```
**Request Body**:
```json
{
  "pageWidth": 241,
  "pageHeight": 105,
  "fontSize": 10,
  "border": "On",
  "monthlyReport": "On",
  "dailyReport": "On",
  "digReport": "On",
  "extraQuota": "Show",
  "ageOrDate": "Date",
  "totalOnSlip": "No"
}
```

---

## Frontend Architecture

### Routing System

The application uses **React Router v6** with nested routes:

```jsx
<Routes>
  {/* Public Routes */}
  <Route path="/" element={<Navigate to="/login" />} />
  <Route path="/login" element={<Login />} />

  {/* Protected Dashboard Routes */}
  <Route path="/dashboard" element={<ProtectedRoute><DashboardLayout /></ProtectedRoute>}>
    <Route index element={<Dashboard />} />
    <Route path="pharmacy" element={<Pharmacy />} />
    <Route path="polices" element={<Polices />} />
    <Route path="committee" element={<Committee />} />
    <Route path="patient" element={<Patient />} />
    <Route path="reports" element={<Reports />} />
    <Route path="users" element={<Users />} />
    <Route path="medicine-set" element={<MedicineSet />} />
    <Route path="settings" element={<Settings />} />
    {/* ... more routes */}
  </Route>

  {/* 404 */}
  <Route path="*" element={<NotFound />} />
</Routes>
```

### Protected Route Implementation
```jsx
function ProtectedRoute({ children }) {
  const [isAuthenticated, setIsAuthenticated] = useState(null);

  useEffect(() => {
    isLoggedIn().then(authenticated => {
      setIsAuthenticated(authenticated);
    });
  }, []);

  if (isAuthenticated === null) {
    return <CircularProgress />;
  }

  return isAuthenticated ? children : <Navigate to="/login" />;
}
```

---

## Component Documentation

### Dashboard Components

#### Header Component
**Location**: `components/dashboard/Header.jsx`
**Purpose**: Top navigation bar with user info and logout

**Features**:
- User profile display
- Logout functionality
- Navigation links

#### Sidebar Component
**Location**: `components/dashboard/Sidebar.jsx`
**Purpose**: Side navigation menu

**Features**:
- Role-based menu items
- Active route highlighting
- Nested navigation support

**Menu Structure**:
```javascript
const menuItems = {
  SUPER_ADMIN: [
    { label: 'Dashboard', path: '/dashboard', icon: <DashboardIcon /> },
    { label: 'Pharmacy', path: '/dashboard/pharmacy', icon: <LocalPharmacyIcon /> },
    { label: 'Police', path: '/dashboard/polices', icon: <PeopleIcon /> },
    { label: 'Committee', path: '/dashboard/committee', icon: <GroupWorkIcon /> },
    { label: 'Distribution', path: '/dashboard/patient', icon: <AssignmentIcon /> },
    { label: 'Medicine Sets', path: '/dashboard/medicine-set', icon: <CategoryIcon /> },
    { label: 'Reports', path: '/dashboard/reports', icon: <AssessmentIcon /> },
    { label: 'Users', path: '/dashboard/users', icon: <PersonIcon /> },
    { label: 'Settings', path: '/dashboard/settings', icon: <SettingsIcon /> }
  ],
  ADMIN: [/* Similar with fewer items */],
  NORMAL_USER: [/* Limited items */]
};
```

#### DashboardInfo Component
**Location**: `components/dashboard/DasboardInfo.jsx`
**Purpose**: Display dashboard statistics

**Metrics**:
- Total Orders
- Today's Orders
- Total Medicines
- Low Stock Alert
- Total Police Officers
- Active Police

#### InventoryAlert Component
**Location**: `components/dashboard/InventoryAlert.jsx`
**Purpose**: Show low stock medicines

**Features**:
- Under 10 stock alert (critical)
- Under 100 stock alert (warning)
- Real-time updates

---

### Pharmacy Components

#### MedicineListTable Component
**Location**: `components/Pharmacy/MedicineListTable.jsx`
**Purpose**: Paginated medicine table

**Features**:
- Pagination (customizable rows per page)
- Search by name/code
- Committee filter
- Actions: View, Edit, Delete
- Stock level indicators

**Props**:
```javascript
{
  medicines: Array,
  page: Number,
  limit: Number,
  total: Number,
  onPageChange: Function,
  onLimitChange: Function,
  onEdit: Function,
  onDelete: Function,
  onView: Function
}
```

#### AddMedicineModal Component
**Location**: `components/Pharmacy/AddMedicineModal.jsx`
**Purpose**: Add new medicine form

**Form Fields**:
```javascript
{
  name: String (required),
  code: Number (required),
  quantity: Number (required),
  mrp: Number,
  quota: Number,
  quotaDays: Number,
  committeeId: String (required)
}
```

**Validation**:
- Name: min 3 characters
- Code: unique integer
- Quantity: non-negative
- Committee: required selection

#### MedicineSearchFilters Component
**Location**: `components/Pharmacy/MedicineSearchFilters.jsx`
**Purpose**: Search and filter controls

**Filters**:
- Search by name/code (debounced)
- Committee selection
- Archived status
- Stock level filters

---

### Police Components

#### AddPoliceModal Component
**Location**: `components/Polices/AddPoliceModal.jsx`
**Purpose**: Add police officer with photo upload

**Form Fields**:
```javascript
{
  name: String (required),
  rank: Enum (required),
  bpCode: String (required, unique),
  mobile: String (required, unique),
  workStation: String,
  age: Number,
  bloodGroup: Enum,
  photo: File
}
```

**File Upload**:
- Accepts: JPG, PNG, GIF
- Max size: 5MB
- Preview before upload
- Base64 encoding for API

#### UpdatePoliceModal Component
**Location**: `components/Polices/UpdatePoliceModal.jsx`
**Purpose**: Edit police officer details

**Features**:
- Pre-filled form data
- Photo update/remove
- Validation
- Optimistic UI updates

---

### Patient/Distribution Components

#### NewDistributeMedicine Component
**Location**: `components/Patient/NewDistributeMedicine.jsx`
**Purpose**: Medicine distribution form

**Features**:
- Police officer selection
- Medicine selection (autocomplete)
- Quantity input
- Quota validation
- Multiple medicines support
- Medicine set quick-add
- Receipt generation

**Form Structure**:
```javascript
{
  policeId: String,
  medicineFor: Enum (SELF, FATHER, MOTHER, etc.),
  orderItems: [
    { medicineId: String, quantity: Number }
  ]
}
```

**Quota Validation**:
- Check monthly quota
- Check dynamic quota (if configured)
- Show remaining quota
- Warning on quota exceeded

#### MedicineSetCards Component
**Location**: `components/Patient/MedicineSetCards.jsx`
**Purpose**: Quick-add predefined medicine sets

**Features**:
- Grid of medicine sets
- Medicine count display
- One-click add to order
- Committee filtering

---

### Form Components

#### Form Component
**Location**: `components/forms/Form.jsx`
**Purpose**: Base form wrapper with React Hook Form

**Usage**:
```jsx
<Form onSubmit={handleSubmit} defaultValues={defaultValues}>
  <FormInput name="username" label="Username" />
  <FormSelectField name="role" label="Role" options={roles} />
  <Button type="submit">Submit</Button>
</Form>
```

#### FormInput Component
**Location**: `components/forms/FormInput.jsx`
**Purpose**: Text input with validation

**Props**:
```javascript
{
  name: String,
  label: String,
  type: String,
  placeholder: String,
  required: Boolean,
  validation: Object
}
```

#### FormSelectField Component
**Location**: `components/forms/FormSelectField.jsx`
**Purpose**: Dropdown select

**Props**:
```javascript
{
  name: String,
  label: String,
  options: Array<{ value, label }>,
  required: Boolean
}
```

#### FormFileUpload Component
**Location**: `components/forms/FormFileUpload.jsx`
**Purpose**: File upload with preview

**Features**:
- Drag & drop
- File type validation
- Size validation
- Preview image
- Progress indicator

---

## State Management

### Redux Toolkit Setup

#### Store Configuration
**File**: `redux/store.js`
```javascript
import { configureStore } from '@reduxjs/toolkit';
import { baseApi } from './api/baseApi';

export const store = configureStore({
  reducer: {
    [baseApi.reducerPath]: baseApi.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(baseApi.middleware),
});
```

#### Base API Configuration
**File**: `redux/api/baseApi.js`
```javascript
import { createApi } from '@reduxjs/toolkit/query/react';
import { axiosBaseQuery } from '../../axios/axiosBaseQuery';

export const baseApi = createApi({
  reducerPath: 'api',
  baseQuery: axiosBaseQuery(),
  tagTypes: [
    'auth',
    'police',
    'committee',
    'medicine',
    'userProfile',
    'order',
    'users',
    'dashboardStats',
    'medicineSet',
    'leave',
    'sittings'
  ],
  endpoints: () => ({}),
});
```

### RTK Query API Slices

#### Medicine API Slice
**File**: `redux/api/medicineApi.js`
```javascript
export const medicineApi = baseApi.injectEndpoints({
  endpoints: (builder) => ({
    getAllMedicines: builder.query({
      query: (params) => ({
        url: '/medicine',
        method: 'GET',
        params
      }),
      providesTags: ['medicine']
    }),
    addMedicine: builder.mutation({
      query: (data) => ({
        url: '/medicine/add',
        method: 'POST',
        data
      }),
      invalidatesTags: ['medicine']
    }),
    updateMedicine: builder.mutation({
      query: ({ id, ...data }) => ({
        url: `/medicine/update/${id}`,
        method: 'PATCH',
        data
      }),
      invalidatesTags: ['medicine']
    }),
    // ... more endpoints
  })
});

export const {
  useGetAllMedicinesQuery,
  useAddMedicineMutation,
  useUpdateMedicineMutation,
  // ... more hooks
} = medicineApi;
```

### Cache Invalidation Strategy

RTK Query uses **tag-based cache invalidation**:

```javascript
// Adding medicine invalidates 'medicine' tag
addMedicine: builder.mutation({
  invalidatesTags: ['medicine']
})

// Getting medicines provides 'medicine' tag
getAllMedicines: builder.query({
  providesTags: ['medicine']
})

// When addMedicine is called, getAllMedicines automatically refetches
```

**Tag Types**:
- `auth` - Authentication data
- `police` - Police officer data
- `committee` - Committee data
- `medicine` - Medicine inventory
- `userProfile` - User profile
- `order` - Distribution orders
- `users` - User management
- `dashboardStats` - Dashboard statistics
- `medicineSet` - Medicine sets
- `leave` - Leave records
- `sittings` - System settings

---

## Features & Functionality

### 1. Medicine Management

#### Add Medicine
- Manual entry via form
- Bulk upload (CSV/Excel)
- Auto-generate medicine codes
- Committee assignment
- Quota configuration

#### Update Medicine
- Edit all fields except ID
- Stock adjustment
- Quota modification
- Archive/unarchive

#### Search & Filter
- Search by name/code
- Filter by committee
- Filter by stock level
- Filter archived medicines
- Pagination support

#### Stock Alerts
- Low stock (< 10 units) - Critical
- Low stock (< 100 units) - Warning
- Real-time monitoring
- Dashboard notifications

#### Bulk Operations
**CSV/Excel Upload Format**:
```csv
name,code,quantity,mrp,quota,quotaDays
Paracetamol 500mg,101,1000,5.50,10,30
Amoxicillin 250mg,102,500,12.00,5,30
```

---

### 2. Police Personnel Management

#### Add Police Officer
- Manual entry with photo upload
- Rank selection (20+ ranks)
- Blood group tracking
- Work station assignment
- Unique BP code

#### Search & Filter
- Search by name, BP code, mobile, workstation
- Filter by rank
- Sort by rank serial
- Pagination (500-10000 records per page)

#### Photo Management
- Upload profile photo (JPG, PNG, GIF)
- View photo in details
- Remove photo
- Photo served via static endpoint

#### Quota Tracking
- View monthly quota usage
- View dynamic quota usage
- Check remaining quota
- Quota history

---

### 3. Medicine Distribution

#### Distribution Process
1. Select police officer (autocomplete search)
2. Select recipient (SELF, FATHER, MOTHER, etc.)
3. Add medicines manually or via medicine sets
4. Validate quota availability
5. Confirm and create order
6. Generate receipt

#### Quota Validation
**Monthly Quota**:
- Checks usage for current month
- Compares with medicine quota limit
- Shows remaining quota

**Dynamic Quota** (if configured):
- Checks usage for configurable period (e.g., 30 days)
- Sliding window validation
- More flexible than monthly

**Quota Rules**:
- Warning if quota exceeded (doesn't block)
- Super admin can override
- Quota resets based on period

#### Medicine Sets
- Predefined medicine groups
- Quick-add to distribution
- Examples: Diabetes Kit, Hypertension Kit, etc.
- Committee-based

#### Receipt Generation
- Auto-generated receipt
- Print-ready format
- Configurable layout (via Settings)
- PDF export option

---

### 4. Committee Management

#### Committee Lifecycle
1. Create committee with lot code
2. Add medicines to committee
3. Set committee as "Running"
4. Distribute medicines
5. Close committee (set not running)
6. Archive old committees

#### Merge Medicines
**Use Case**: Consolidate medicines from multiple committees

**Process**:
1. Select source committee
2. Select target committee
3. Select medicines to merge
4. Confirm merge
5. Medicines moved to target committee

---

### 5. Reporting System

#### Daily Report
- Auto-print on page load
- Shows all distributions for a specific date
- Grouped by medicine
- Total quantities

#### Monthly Report
- Select year and month
- Navigate previous/next months
- Distribution summary
- PDF export

#### Custom Date Range Report
- Select start and end dates
- Flexible reporting period
- Distribution details
- Medicine-wise summary
- PDF export with custom formatting

#### DIG Report
**Purpose**: Deputy Inspector General monthly report

**Contents**:
- Medicine distribution summary
- Committee-wise breakdown
- Cost analysis (if enabled)
- Stock additions
- Stock balance

**Configuration** (via Settings):
- Enable/disable DIG report
- Include cost
- Show monthly additions
- Show daily stock

---

### 6. Leave Management

#### Add Leave
- Select police officer
- Start and end dates
- Leave description
- Leave address
- Posting location
- Father/mother names (optional)

#### View Leaves
- All leaves list
- Filter by police officer
- Date range filter

#### Update/Delete
- Admin and Super Admin can update
- All roles can delete
- Audit trail

---

### 7. User Management

#### User Roles
- **SUPER_ADMIN**: Full access
- **ADMIN**: Most features except critical operations
- **NORMAL_USER**: Limited to distribution and viewing

#### User Operations
- Register new user (Admin/Super Admin)
- View all users
- Delete user
- Reset user password
- Update profile

#### Password Management
- Change own password
- Reset other user passwords (Admin)
- Hashed with bcrypt
- Minimum 8 characters

---

### 8. System Settings

#### Print Settings
- Page width/height
- Font size
- Left margin
- Line height
- Border display
- Field widths (name, serial, quantity)

#### Report Settings
- Monthly report on/off
- Daily report on/off
- DIG report on/off

#### Display Settings
- Show/hide extra quota
- Age or date display
- Total on slip (Yes/No)

**Settings are stored in Sitting table (singleton)**

---

## Authentication & Authorization

### Authentication Flow

#### 1. Login
```javascript
// Frontend: Login component
const handleLogin = async (credentials) => {
  const response = await userLogin(credentials);
  setToLocalStorage(response.data.accessToken);
  // Server sets refresh token as HTTP-only cookie
  navigate('/dashboard');
};
```

#### 2. Token Storage
- **Access Token**: Stored in localStorage (`"accessToken"`)
- **Refresh Token**: HTTP-only cookie (secure, can't be accessed by JS)

#### 3. Request Authentication
```javascript
// Axios request interceptor
axiosInstance.interceptors.request.use((config) => {
  const token = getFromLocalStorage('accessToken');
  if (token) {
    config.headers.Authorization = token;
  }
  return config;
});
```

#### 4. Token Refresh
```javascript
// Axios response interceptor
axiosInstance.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;

    if (error.response?.status === 401 && !originalRequest._retry) {
      if (originalRequest.url === '/auth/login') {
        return Promise.reject(error);
      }

      originalRequest._retry = true;

      // Refresh token
      const { data } = await axios.get('/auth/refresh-token', {
        withCredentials: true
      });

      setToLocalStorage(data.accessToken);
      originalRequest.headers.Authorization = data.accessToken;

      return axiosInstance(originalRequest);
    }

    return Promise.reject(error);
  }
);
```

#### 5. Protected Routes
```javascript
function ProtectedRoute({ children }) {
  const [isAuthenticated, setIsAuthenticated] = useState(null);

  useEffect(() => {
    isLoggedIn().then(authenticated => {
      setIsAuthenticated(authenticated);
    });
  }, []);

  if (isAuthenticated === null) {
    return <CircularProgress />;
  }

  return isAuthenticated ? children : <Navigate to="/login" />;
}
```

#### 6. Logout
```javascript
const logout = () => {
  removeUserInfo('accessToken');
  navigate('/login');
};
```

---

### Authorization (Role-Based Access Control)

#### Role Hierarchy
```
SUPER_ADMIN (Highest privileges)
    ├── Full CRUD on all entities
    ├── User management
    ├── Stock adjustments
    ├── Delete operations
    └── System settings

ADMIN (Administrative privileges)
    ├── Most CRUD operations
    ├── Limited user management
    ├── Cannot delete critical data
    └── Report generation

NORMAL_USER (Basic privileges)
    ├── View operations
    ├── Medicine distribution
    ├── Leave management
    └── Limited reporting
```

#### Backend Authorization Middleware
```typescript
// auth.ts
export const auth = (...requiredRoles: ROLE[]) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      const token = req.headers.authorization;

      if (!token) {
        throw new Error('Unauthorized');
      }

      const decoded = jwt.verify(token, config.jwtSecret);
      req.user = decoded;

      if (requiredRoles.length && !requiredRoles.includes(decoded.role)) {
        throw new Error('Forbidden');
      }

      next();
    } catch (error) {
      next(error);
    }
  };
};
```

#### Usage in Routes
```typescript
router.post(
  '/medicine/add',
  auth(ROLE.SUPER_ADMIN, ROLE.ADMIN),
  validateRequest(MedicineValidation.addMedicineValidation),
  MedicineController.addMedicine
);
```

#### Frontend Role-Based UI
```jsx
// Sidebar component
const Sidebar = () => {
  const { data: userData } = useGetProfileQuery();
  const userRole = userData?.data?.role;

  return (
    <List>
      {/* All users can see Dashboard */}
      <ListItem to="/dashboard">Dashboard</ListItem>

      {/* Admin and Super Admin only */}
      {['ADMIN', 'SUPER_ADMIN'].includes(userRole) && (
        <ListItem to="/dashboard/users">Users</ListItem>
      )}

      {/* Super Admin only */}
      {userRole === 'SUPER_ADMIN' && (
        <ListItem to="/dashboard/manage-stock">Manage Stock</ListItem>
      )}
    </List>
  );
};
```

#### Permission Matrix

| Feature | SUPER_ADMIN | ADMIN | NORMAL_USER |
|---------|-------------|-------|-------------|
| View Dashboard Stats | ✅ | ✅ | ❌ |
| View Medicines | ✅ | ✅ | ✅ |
| Add Medicine | ✅ | ✅ | ❌ |
| Update Medicine | ✅ | ✅ | ❌ |
| Delete Medicine | ✅ | ✅ | ❌ |
| Adjust Stock | ✅ | ❌ | ❌ |
| View Police | ✅ | ✅ | ✅ |
| Add Police | ✅ | ✅ | ✅ |
| Update Police | ✅ | ✅ | ✅ |
| Delete Police | ✅ | ✅ | ✅ |
| View Committee | ✅ | ✅ | ✅ |
| Add Committee | ✅ | ✅ | ❌ |
| Update Committee | ✅ | ❌ | ❌ |
| Delete Committee | ✅ | ❌ | ❌ |
| Merge Medicines | ✅ | ✅ | ✅ |
| Distribute Medicine | ✅ | ✅ | ✅ |
| View Orders | ✅ | ✅ | ✅ |
| View Reports | ✅ | ✅ | ✅ |
| Generate Reports | ✅ | ✅ | ❌ |
| View Users | ✅ | ✅ | ❌ |
| Add User | ✅ | ✅ | ❌ |
| Delete User | ✅ | ✅ | ❌ |
| Reset Password | ✅ | ✅ | ❌ |
| Manage Settings | ✅ | ✅ | ✅ |
| View Medicine Sets | ✅ | ✅ | ✅ |
| Create Medicine Sets | ✅ | ✅ | ❌ |
| Add Leave | ✅ | ✅ | ✅ |
| Update Leave | ✅ | ✅ | ❌ |
| Delete Leave | ✅ | ✅ | ✅ |

---

## Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- PostgreSQL (v12 or higher)
- npm or yarn
- Git

### Backend Setup

#### 1. Clone Repository
```bash
git clone <repository_url>
cd police_pharmacy_backned
```

#### 2. Install Dependencies
```bash
npm install
# or
yarn install
```

#### 3. Environment Configuration
Create `.env` file:
```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/police_pharmacy"

# JWT
JWT_SECRET="your-super-secret-jwt-key"
JWT_EXPIRES_IN="7d"
JWT_REFRESH_SECRET="your-refresh-secret"
JWT_REFRESH_EXPIRES_IN="30d"

# Server
PORT=5000
NODE_ENV="development"

# CORS
FRONTEND_URL="http://localhost:5173"
```

#### 4. Database Setup
```bash
# Generate Prisma client
npx prisma generate

# Run migrations
npx prisma migrate dev

# Seed database
npm run db:seed

# Verify seed data
npm run db:verify

# Open Prisma Studio (database GUI)
npm run db:studio
```

#### 5. Start Development Server
```bash
npm run dev
```

Backend will run on `http://localhost:5000`

#### 6. Build for Production
```bash
npm run build
npm start
```

---

### Frontend Setup

#### 1. Navigate to Frontend Directory
```bash
cd police_pharmacy_frontend
```

#### 2. Install Dependencies
```bash
npm install
# or
yarn install
```

#### 3. Environment Configuration
Create `.env` file:
```env
VITE_BACKEND_URL="http://localhost:5000/api/v1"
```

#### 4. Start Development Server
```bash
npm run dev
```

Frontend will run on `http://localhost:5173`

#### 5. Build for Production
```bash
npm run build
npm run preview
```

---

### Database Migrations

#### Create New Migration
```bash
cd police_pharmacy_backned
npx prisma migrate dev --name <migration_name>
```

#### Apply Migrations
```bash
npx prisma migrate deploy
```

#### Reset Database (WARNING: Deletes all data)
```bash
npm run db:reset
```

#### View Database in Prisma Studio
```bash
npm run db:studio
```

---

### Default Credentials

After running `db:seed`, default accounts are created:

#### Super Admin
- **Username**: `sadmin`
- **Password**: `12345678`

**Note**: Change default password immediately in production!

---

## Development Guidelines

### Code Style

#### Backend (TypeScript)
- Use TypeScript strict mode
- Follow ESLint and Prettier configurations
- Use async/await over promises
- Proper error handling with try-catch
- Type all function parameters and return values

**Example**:
```typescript
// Good
async function getMedicineById(id: string): Promise<Medicine | null> {
  try {
    const medicine = await prisma.medicine.findUnique({
      where: { id },
      include: { committee: true }
    });
    return medicine;
  } catch (error) {
    throw new ApiError(httpStatus.INTERNAL_SERVER_ERROR, 'Failed to fetch medicine');
  }
}

// Bad
function getMedicineById(id) {
  return prisma.medicine.findUnique({ where: { id } });
}
```

#### Frontend (React/JavaScript)
- Use functional components
- Use hooks for state management
- Use RTK Query for API calls
- Avoid prop drilling (use context or Redux)
- Use Material-UI components consistently

**Example**:
```jsx
// Good
function MedicineList() {
  const { data, isLoading, error } = useGetAllMedicinesQuery({ page: 1, limit: 10 });

  if (isLoading) return <CircularProgress />;
  if (error) return <Alert severity="error">{error.message}</Alert>;

  return (
    <Table>
      {data.data.map(medicine => (
        <TableRow key={medicine.id}>
          <TableCell>{medicine.name}</TableCell>
        </TableRow>
      ))}
    </Table>
  );
}

// Bad
function MedicineList() {
  const [medicines, setMedicines] = useState([]);

  useEffect(() => {
    fetch('/api/medicine').then(res => res.json()).then(setMedicines);
  }, []);

  return <div>{medicines.map(m => <div>{m.name}</div>)}</div>;
}
```

---

### Module Structure (Backend)

Each module follows this structure:

```
module/
├── module.controller.ts    # Request handling
├── module.service.ts       # Business logic
├── module.routes.ts        # Route definitions
├── module.validation.ts    # Zod schemas
├── module.interface.ts     # TypeScript interfaces
└── module.constant.ts      # Constants
```

**Example - Medicine Module**:

```typescript
// medicine.routes.ts
router.post('/add', auth(ROLE.ADMIN), validateRequest(MedicineValidation.addMedicineValidation), MedicineController.addMedicine);

// medicine.controller.ts
const addMedicine = catchAsync(async (req, res) => {
  const result = await MedicineService.addMedicine(req.body);
  sendResponse(res, {
    statusCode: httpStatus.CREATED,
    success: true,
    message: 'Medicine added successfully',
    data: result
  });
});

// medicine.service.ts
const addMedicine = async (data: IMedicine): Promise<Medicine> => {
  const medicine = await prisma.medicine.create({
    data,
    include: { committee: true }
  });
  return medicine;
};

// medicine.validation.ts
const addMedicineValidation = z.object({
  body: z.object({
    name: z.string().min(3),
    code: z.number().int().positive(),
    quantity: z.number().int().nonnegative(),
    committeeId: z.string().uuid()
  })
});
```

---

### API Response Standards

#### Success Response
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... },
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 100
  }
}
```

#### Error Response
```json
{
  "success": false,
  "message": "Error message",
  "errorMessages": [
    {
      "path": "field_name",
      "message": "Validation error"
    }
  ],
  "stack": "Error stack (development only)"
}
```

---

### Git Workflow

#### Branch Naming
- `feature/feature-name` - New features
- `bugfix/bug-name` - Bug fixes
- `hotfix/critical-fix` - Critical production fixes
- `refactor/refactor-name` - Code refactoring

#### Commit Messages
Follow conventional commits:
```
feat: add medicine bulk upload
fix: resolve quota calculation bug
docs: update API documentation
refactor: improve police search performance
test: add medicine service tests
```

#### Pre-commit Hooks
Husky runs automatically:
- ESLint
- Prettier
- Type checking

---

### Testing Guidelines

#### Backend Testing (TODO)
```bash
npm run test
```

#### Frontend Testing (TODO)
```bash
npm run test
```

---

### Security Best Practices

#### Backend
1. **Password Security**
   - Hash with bcrypt (salt rounds: 10)
   - Minimum 8 characters
   - Never log passwords

2. **JWT Security**
   - Use strong secrets (min 32 characters)
   - Short access token expiry (7 days)
   - HTTP-only cookies for refresh tokens
   - Verify token on every request

3. **Input Validation**
   - Validate all inputs with Zod
   - Sanitize user inputs
   - Use parameterized queries (Prisma does this)

4. **CORS Configuration**
   - Whitelist specific origins
   - Enable credentials
   - No wildcard in production

5. **Error Handling**
   - Don't expose stack traces in production
   - Generic error messages to clients
   - Log detailed errors server-side

#### Frontend
1. **XSS Prevention**
   - React escapes by default
   - Avoid `dangerouslySetInnerHTML`
   - Sanitize user-generated content

2. **Token Storage**
   - Access token in localStorage (acceptable risk)
   - Refresh token in HTTP-only cookie (secure)
   - Clear tokens on logout

3. **HTTPS Only**
   - Force HTTPS in production
   - Secure cookie flag

---

### Performance Optimization

#### Backend
1. **Database**
   - Add indexes on frequently queried fields
   - Use pagination for large datasets
   - Optimize N+1 queries with `include`
   - Use database connection pooling

2. **Caching**
   - Cache static data (committees, settings)
   - Use Redis for session storage (future enhancement)

3. **File Uploads**
   - Limit file size
   - Compress images
   - Store in CDN (future enhancement)

#### Frontend
1. **Code Splitting**
   - Lazy load routes
   - Dynamic imports for heavy components

2. **API Optimization**
   - Use RTK Query caching
   - Debounce search inputs
   - Pagination for large lists

3. **Asset Optimization**
   - Optimize images
   - Minify JavaScript/CSS
   - Use production build

---

## Deployment

### Backend Deployment (Example: Railway/Render)

#### 1. Environment Variables
Set in deployment platform:
```env
DATABASE_URL=<production_postgres_url>
JWT_SECRET=<production_secret>
JWT_REFRESH_SECRET=<production_refresh_secret>
NODE_ENV=production
FRONTEND_URL=<production_frontend_url>
```

#### 2. Build Command
```bash
npm run build
```

#### 3. Start Command
```bash
npm start
```

#### 4. Database Migration
```bash
npx prisma migrate deploy
npx prisma generate
```

---

### Frontend Deployment (Example: Vercel/Netlify)

#### 1. Build Command
```bash
npm run build
```

#### 2. Output Directory
```
dist/
```

#### 3. Environment Variables
```env
VITE_BACKEND_URL=<production_backend_url>
```

#### 4. Redirects (for SPA)
**Vercel** (`vercel.json`):
```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/" }
  ]
}
```

**Netlify** (`_redirects`):
```
/*    /index.html   200
```

---

## Future Enhancements

### Planned Features
1. **SMS Notifications**: Send medicine distribution alerts
2. **Email Notifications**: Order confirmations and reports
3. **Advanced Analytics**: Charts and graphs for trends
4. **Mobile App**: React Native mobile application
5. **Barcode Scanning**: Quick medicine/police lookup
6. **Prescription Upload**: Attach prescriptions to orders
7. **Multi-language Support**: Bengali and English
8. **Dark Mode**: Theme switching
9. **Export Data**: CSV/Excel export for all modules
10. **Audit Logs**: Track all user actions
11. **Real-time Updates**: WebSocket for live notifications
12. **Advanced Search**: Elasticsearch integration
13. **Backup & Restore**: Automated database backups
14. **Two-Factor Authentication**: Enhanced security
15. **API Rate Limiting**: Prevent abuse

---

## Troubleshooting

### Common Issues

#### Database Connection Error
**Error**: `Can't reach database server`

**Solution**:
1. Check PostgreSQL is running
2. Verify `DATABASE_URL` in `.env`
3. Check database credentials
4. Ensure database exists

```bash
# Create database
psql -U postgres
CREATE DATABASE police_pharmacy;
```

---

#### Prisma Client Not Generated
**Error**: `Cannot find module '@prisma/client'`

**Solution**:
```bash
npx prisma generate
```

---

#### CORS Error
**Error**: `Access to XMLHttpRequest has been blocked by CORS policy`

**Solution**:
1. Check frontend URL in backend `.env` (`FRONTEND_URL`)
2. Verify CORS configuration in `app.ts`
3. Ensure `withCredentials: true` in axios

---

#### Port Already in Use
**Error**: `Port 5000 is already in use`

**Solution**:
```bash
# Linux/Mac
lsof -ti:5000 | xargs kill -9

# Windows
netstat -ano | findstr :5000
taskkill /PID <PID> /F
```

---

#### Token Refresh Loop
**Error**: Infinite refresh token requests

**Solution**:
1. Check refresh token endpoint doesn't require auth
2. Verify `originalRequest._retry` flag
3. Clear localStorage and cookies
4. Check refresh token expiry

---

## Support & Contact

### Documentation
- Backend README: `police_pharmacy_backned/readme.md`
- Frontend README: `police_pharmacy_frontend/README.md`
- Database Setup: `police_pharmacy_backned/DATABASE_SETUP.md`
- Reports Documentation: `police_pharmacy_backned/REPORTS_DOCUMENTATION.md`
- User Manual: `police_pharmacy_frontend/UserManual.md`

### Additional Resources
- Prisma Documentation: https://www.prisma.io/docs
- Redux Toolkit: https://redux-toolkit.js.org
- Material-UI: https://mui.com
- Express.js: https://expressjs.com

---

## Changelog

### Version 1.0.0 (Current)
- Initial release
- Complete CRUD for all entities
- Role-based access control
- Reporting system
- Medicine distribution
- Quota management
- Leave management
- Settings configuration

---

## License

This project is proprietary software developed for Bangladesh Police Hospital Narayanganj.

---
