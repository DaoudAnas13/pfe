# Product Requirements Document (PRD)
## RM – Report Manager
### Web & Mobile Application for IT Equipment Installation & Field Intervention Management

**Document Version:** 1.0  
**Last Updated:** February 2026  
**Status:** Completed Implementation Analysis

---

## Table of Contents
1. [Product Overview & Vision](#product-overview--vision)
2. [Problem Statement & Business Goals](#problem-statement--business-goals)
3. [Target Users & Personas](#target-users--personas)
4. [System Architecture Overview](#system-architecture-overview)
5. [Folder-Based Functional Requirements](#folder-based-functional-requirements)
6. [Business Processes Supported](#business-processes-supported)
7. [Core Features & Entity Model](#core-features--entity-model)
8. [Non-Functional Requirements](#non-functional-requirements)
9. [Roles, Permissions & Access Control](#roles-permissions--access-control)
10. [Technologies & Tools Used](#technologies--tools-used)

---

## Product Overview & Vision

### Executive Summary

**RM – Report Manager** is an integrated web and mobile application platform designed to streamline IT equipment installations and field interventions. It enables technicians to collect site data in real-time using mobile applications, allows supervisors and project coordinators to manage intervention workflows via web interfaces, and facilitates automated Excel report generation compliant with telecom operator templates.

### Vision Statement

To provide a unified, efficient platform that:
- Digitizes field intervention processes for technicians
- Enables real-time data collection and validation at installation sites
- Supports supervisors in workflow management and quality control
- Automates report generation for stakeholders and operators
- Ensures data integrity and traceability throughout the intervention lifecycle

### Key Capabilities

- **Mobile-first data collection** with offline functionality for technicians
- **Web-based workflow management** for supervisors and coordinators
- **Template-driven Excel report generation** conforming to telecom operator standards
- **Role-based access control** with granular permissions
- **Real-time notifications** via Firebase Cloud Messaging
- **Geolocation tracking** of field interventions
- **File attachment management** for photographic evidence and documentation
- **Status tracking and audit trails** for compliance and accountability

---

## Problem Statement & Business Goals

### Problem Statement

Organizations managing IT equipment installations and field interventions faced several challenges:

1. **Manual Data Collection:** Technicians collecting intervention data on paper or using disconnected systems
2. **Data Quality Issues:** Inconsistent, duplicate, or incomplete data submission
3. **Slow Workflow Processing:** Lengthy validation cycles between technicians, supervisors, and coordinators
4. **Manual Report Generation:** Time-consuming, error-prone Excel report creation
5. **Lack of Real-time Visibility:** Management unable to track intervention status in real-time
6. **Compliance & Traceability:** Difficulty ensuring operator template compliance and maintaining audit trails
7. **Communication Delays:** Delayed feedback and notifications between team members

### Business Goals

1. **Digitize Field Operations:** Eliminate paper-based processes and enable mobile data capture
2. **Improve Data Quality:** Enforce validation rules and standardize data collection
3. **Accelerate Workflows:** Reduce intervention processing time and decision cycles
4. **Automate Reporting:** Generate compliant Excel reports with minimal manual intervention
5. **Enable Real-time Oversight:** Provide supervisors with live status updates and metrics
6. **Ensure Compliance:** Maintain audit trails and enforce role-based access controls
7. **Enhance Communication:** Deploy instant notifications for status changes and rejections
8. **Reduce Operational Costs:** Streamline processes and reduce administrative overhead

---

## Target Users & Personas

### 1. Field Technician
**Role:** On-site equipment installation and troubleshooting

**Responsibilities:**
- Arrive at installation site
- Capture equipment details, configurations, and issues
- Take photographs/videos as evidence
- Complete intervention forms based on templates
- Submit data and files (online or offline)
- Receive feedback and rejection notes

**Pain Points:**
- Limited connectivity at remote sites
- Time-consuming manual documentation
- Lack of clear instructions on site

**Needs:**
- Offline data entry capability
- Intuitive mobile forms matching site context
- Easy photo/file attachment
- Real-time guidance and templates

---

### 2. Project Supervisor/Manager
**Role:** Oversee intervention workflows and quality assurance

**Responsibilities:**
- Assign interventions to technicians
- Monitor intervention progress in real-time
- Review submitted data and attachments
- Approve or reject interventions with feedback
- Track technician performance metrics
- Manage team workload

**Pain Points:**
- Limited visibility into field progress
- Manual review and approval processes
- Difficulty tracking rejection reasons

**Needs:**
- Dashboard with real-time intervention status
- Mobile/web approval workflows
- Filtering and search capabilities
- Performance analytics and reports

---

### 3. Project Coordinator/Administrator
**Role:** Manage projects, clients, templates, and system configuration

**Responsibilities:**
- Create and maintain intervention templates
- Define Excel report generation configurations
- Manage client and company information
- Configure access controls and permissions
- Generate compliance reports
- Monitor system health and usage

**Pain Points:**
- Complex template configuration
- Manual export configuration setup
- Limited visibility into system usage

**Needs:**
- Intuitive template builder
- JSON-based mapping for Excel generation
- User and permission management
- System analytics and audit logs

---

### 4. System Administrator
**Role:** Maintain infrastructure, security, and system reliability

**Responsibilities:**
- Manage user accounts and authentication
- Configure security policies and access controls
- Monitor system performance and logs
- Manage database and file storage
- Coordinate deployments and updates
- Ensure data backup and recovery

**Needs:**
- Secure authentication (JWT/OAuth)
- Centralized user management
- Performance monitoring and logs
- Scalable infrastructure

---

## System Architecture Overview

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                                 │
├──────────────────────────┬──────────────────────────────────────┤
│   Web Application        │    Mobile Application               │
│   (Angular Gateway)      │    (Flutter Mobile App)            │
│   - Supervisor Portal    │    - Field Technician              │
│   - Project Management   │    - Offline Capability            │
│   - Report Generation    │    - Geolocation Tracking          │
└──────────────────────────┴──────────────────────────────────────┘
                              │
                    ┌─────────┴──────────┐
                    ▼                    ▼
              ┌──────────────┐      ┌────────────────┐
              │  API Gateway │      │  Microservices │
              │  (JHipster)  │      │   (Spring Boot)│
              └──────────────┘      └────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
    ┌────────────┐        ┌──────────────┐
    │   Core     │        │  Third-party │
    │   APIs     │        │  Services    │
    │            │        │              │
    │ - Reports  │        │ - Firebase   │
    │ - Users    │        │   (Messaging)│
    │ - Entities │        │ - Storage    │
    └────────────┘        │ - Maps       │
        │                 └──────────────┘
        │
        ▼
    ┌────────────────────────┐
    │   Data Access Layer    │
    │   - JPA Repositories   │
    │   - Specifications     │
    └────────────────────────┘
        │
        ▼
    ┌────────────────────────┐
    │    PostgreSQL DB       │
    │  - Schemas & Tables    │
    │  - JSONB Data Types    │
    │  - Liquibase Changelog │
    └────────────────────────┘
        │
        ▼
    ┌────────────────────────┐
    │   File Storage         │
    │   - Excel Templates    │
    │   - Generated Reports  │
    │   - Attachments (BLOB) │
    └────────────────────────┘
```

### Architecture Principles

1. **Microservice Pattern:** Scalable, independently deployable services
2. **API-First Design:** RESTful APIs with standardized request/response
3. **Separation of Concerns:** Clear layers (presentation, business, data)
4. **Data-Driven Forms:** JSON-based dynamic form generation
5. **Event-Driven Notifications:** Firebase for push notifications
6. **Flexible Authorization:** JSONB-based role and permission storage

---

### Technology Stack Layers

#### **Frontend Layer**
- **Web:** Angular 10 with Bootstrap, CoreUI, and DevExtreme components
- **Mobile:** Flutter 2.19+ with BLoC state management
- **Styling:** SCSS, responsive design, Bootstrap 4/5
- **HTTP:** Axios (web), Dio (mobile) for API calls

#### **API/Gateway Layer**
- **API Gateway:** JHipster-generated Spring Boot Gateway
- **Microservices:** Spring Boot 2.2.7 with Spring Cloud integration
- **Request Handling:** RESTful controllers with validation
- **Documentation:** Swagger/OpenAPI compatible

#### **Business Logic Layer**
- **Services:** Spring @Service components with @Transactional support
- **Mappers:** DTOs mapped from/to entities
- **Validators:** Bean validation with custom rules
- **Excel Generation:** Apache POI for dynamic report generation

#### **Data Layer**
- **ORM:** Hibernate 5.4+ with JPA
- **Database:** PostgreSQL with JSONB columns
- **Migrations:** Liquibase for schema versioning
- **Repositories:** Spring Data JPA with custom specifications

#### **Infrastructure**
- **Authentication:** JWT tokens with Spring Security
- **Cloud Messaging:** Firebase Admin SDK
- **Docker:** Containerization and orchestration
- **Build Tools:** Maven, npm/webpack
- **CI/CD:** Git-based workflows

---

### Data Flow

#### **Typical Field Intervention Flow**

```
1. Supervisor Creates/Assigns Intervention
   ├─ Web: POST /api/rapport-requests
   └─ DB: INSERT rapport_request + related entities

2. Technician Receives Notification
   ├─ Firebase: sendNotification()
   └─ Mobile: Update local cache

3. Technician Accesses Intervention (Offline)
   ├─ Mobile: Load cached rapport_request + template
   └─ Form: Generate dynamic form from template

4. Technician Fills Form & Captures Files
   ├─ Mobile: Store data locally (offline)
   ├─ File: Pick images/attachments → cache
   └─ Data: JSON structure matching template

5. Technician Submits Data
   ├─ Mobile: POST /api/rapport-requests/{id}/execution-details
   ├─ DB: INSERT execution_details
   ├─ DB: INSERT files (binary data)
   └─ Firebase: sendNotification(supervisor)

6. Supervisor Reviews & Approves/Rejects
   ├─ Web: GET /api/rapport-requests (filter by status)
   ├─ Web: Review attachments and data
   ├─ Decision: PUT /api/rapport-requests/{id} (status update)
   └─ DB: UPDATE rapport_request status + history

7. Coordinator Generates Report
   ├─ Web: POST /api/config-exports/generate-rapport
   ├─ Service: Query rapport_requests by template/date range
   ├─ Service: Map data to Excel via config_mapping_json
   ├─ Service: Generate XLSX file from template
   └─ File: Return downloadable Excel file

8. System Archives Intervention
   ├─ Scheduled: RapportRequestService.archiveExpiredRequests()
   └─ DB: UPDATE rapport_request.archive_date
```

---

## Folder-Based Functional Requirements

### BACKEND: RM_MS (Spring Boot Microservice)

#### **Core Entities (Domain Layer)**

1. **RapportRequest** - Central intervention entity
   - Reference (unique identifier for intervention)
   - Client final (end customer)
   - Status and status history
   - Dates: creation, completion, validation, planning
   - Template reference
   - Technician and responsible user assignments
   - Report line data (JSONB dynamic structure)
   - File definitions (JSONB)
   - Read/write access controls (JSONB)
   - Archival tracking

2. **RapportTemplate** - Intervention form templates
   - Name and description
   - Mapping import JSON (data schema)
   - Read access controls
   - Delay to archive (SLA configuration)
   - Files definition flexibility
   - Classement formulaire (form organization)
   - Processing delay configuration
   - Email notification recipients
   - Can be created by technician flag
   - Associations: Project, LineTypes

3. **Files** - Attachments and evidence
   - File name, extension, content type
   - Binary data (BLOB)
   - Code piece (reference)
   - Date insertion
   - Association: RapportRequest

4. **FilesDefinition** - Template for file requirements
   - File type definition
   - Required/optional flags
   - Min/max quantities
   - Association: RapportTemplate via LineType

5. **LineType** - Report line/row types
   - Name, description
   - Position/order
   - Associations: RapportTemplate, FilesDefinition

6. **Status** - Intervention statuses
   - Status code (PLANNED, EXECUTED, PENDING_VALIDATION, APPROVED, REJECTED, ARCHIVED)
   - Label for display
   - Color for UI indicators

7. **StatusHistory** - Audit trail
   - Previous status and new status
   - Date and user who changed status
   - Comments

8. **Reject** - Rejection records
   - Comment (reason for rejection)
   - Evidence image (screenshot/photo)
   - Date creation
   - References: Motif, Status, RapportRequest

9. **Motif** - Rejection reasons/codes
   - Code and description
   - Type (technical, administrative, etc.)

10. **CustomNotif** - Custom notification system
    - Title, body, description
    - User and status
    - Date
    - Association: RapportRequest

11. **NotifHistory** - Notification audit trail
    - Title, body, description
    - Status (SENT, NOT SENT)
    - Date sent
    - User login
    - Association: RapportRequest

12. **ConfigExport** - Excel report generation configuration
    - Label (report name)
    - Excel template file (BLOB)
    - Configuration mapping JSON (field mapping)
    - Read access controls
    - Active flag
    - Association: RapportTemplate

13. **OutputDefinition** - Report output definition
    - Name
    - Template file
    - Mapping JSON
    - Association: RapportTemplate

14. **ExecutionDetails** - Execution/completion information
    - Coordinates (latitude/longitude)
    - Timestamp
    - Device details
    - Battery level, connectivity status
    - Association: RapportRequest

15. **MyReport** - Generated report records
    - File name and folder location
    - Creation date and user
    - Association: ConfigExport

16. **Client, Company, Projet** - Organizational entities
    - Client: code, name, email, contact, RNE flag
    - Company: code, name, legal details, contact
    - Projet: code, dates, status, associations

17. **Favori, UserFavori** - User favorites/bookmarks
    - Quick access to frequently used entities

18. **Envoi** - Dispatch/sending records
    - Status tracking for document distribution

#### **API Endpoints (Resource Layer)**

All endpoints follow `/api/resource-name` pattern with CRUD operations:

- **RapportRequest:**
  - `POST /api/rapport-requests` - Create new intervention
  - `POST /api/rapport-requests-custom` - Create with custom line types
  - `GET /api/rapport-requests` - List (paginated, filtered)
  - `GET /api/rapport-requests/{id}` - Get detail
  - `PUT /api/rapport-requests` - Update
  - `DELETE /api/rapport-requests/{id}` - Archive/delete
  - `POST /api/execution-details-from-mobile/{rapportRequestId}/{reference}` - Mobile submission

- **RapportTemplate:**
  - `POST /api/rapport-templates` - Create template
  - `GET /api/rapport-templates` - List
  - `PUT /api/rapport-templates` - Update
  - `GET /api/rapport-templates/{id}` - Get detail

- **ConfigExport:**
  - `POST /api/config-exports` - Create configuration
  - `GET /api/config-exports` - List
  - `PUT /api/config-exports` - Update
  - `POST /api/config-exports/{id}/generate-rapport` - Generate Excel report

- **Status, StatusHistory, Reject, Motif** - Standard CRUD endpoints
- **Files, FilesDefinition** - Upload/download/manage attachments
- **CustomNotif, NotifHistory** - Notification management
- **ExecutionDetails** - Submission handling from mobile
- **Client, Company, Projet** - Master data management

#### **Services (Business Logic)**

1. **RapportRequestService** - Core business logic
   - Create empty/custom interventions
   - Save and update with validation
   - Find interventions by criteria
   - Generate unique reference numbers
   - Archive expired interventions
   - Handle status transitions
   - Apply access control filters

2. **RapportTemplateService** - Template management
   - CRUD operations
   - Template validation

3. **ConfigExportService** - Excel report generation
   - `generateConfigRapport()` - Main report generation method
     - Fetch interventions by date range and statuses
     - Load Excel template
     - Parse mapping configuration (JSON)
     - Map intervention data to Excel cells
     - Handle dynamic row generation
     - Support Column and Row insertion types
     - Support Entity and LineType data sources
     - Generate unique folder and file names
     - Return file path/download link

4. **ExecutionDetailsService** - Field submission handling
   - Process mobile submissions
   - Validate device details
   - Store geolocation data
   - Associate attachments

5. **FileService** - Attachment management
   - Upload files to intervention
   - Download attachments
   - Manage file metadata
   - Clean up orphaned files

6. **NotifHistoryService** - Notification tracking
   - Log sent/failed notifications
   - Audit trail of communications

7. **EmailSenderService** - Email notifications
   - Send notifications to stakeholders
   - Template-based email generation

8. **StatusHistoryService** - Audit trail
   - Record status changes
   - Track responsible user
   - Store transition comments

9. **FirebaseMessagingService** - Push notifications
   - `sendNotification()` - Send notification with logging
   - `sendCustomNotification()` - Custom message dispatch
   - Failure handling and retry logic

10. **ProjetService, ClientService, CompanyService** - Master data
    - CRUD and relationship management

#### **Security & Authorization**

- **AuthoritiesConstants:**
  - `ROLE_ADMIN` - System administrators
  - `ROLE_USER` - Regular authenticated users
  - `ROLE_ANONYMOUS` - Unauthenticated access

- **SecurityUtils:** JWT token validation, user extraction
- **SpringSecurityAuditorAware:** Track created_by, modified_by
- **@PreAuthorize:** Method-level security annotations
- **JSONB Access Controls:** Dynamic role-based read/write access

---

### FRONTEND: ReportManagerANG (Angular Web Application)

#### **Module Structure**

1. **App Module** - Root module
   - Component declarations and imports
   - Global configurations
   - JHipster initialization

2. **Core Module** - Singleton services
   - Authentication service
   - User service
   - JWT token management
   - Interceptors (auth, error handling)

3. **Shared Module** - Reusable components and pipes
   - Models/interfaces
   - Common components
   - Date/number pipes
   - Validators

4. **Admin Module** - System administration
   - User management
   - Permission configuration
   - System monitoring

5. **Entity Module (ReportManage)** - Domain-specific features
   - Sub-modules for each entity:
     - `rapport-request/` - Intervention management
     - `rapport-template/` - Template CRUD
     - `config-export/` - Report configuration
     - `status/` - Status management
     - `client/`, `company/`, `projet/` - Master data
     - `reject/`, `motif/` - Rejection management
     - `files/`, `files-definition/` - Attachment management
     - `custom-notif/`, `notif-history/` - Notifications
     - `my-report/` - Generated reports view

#### **Key Components & Features**

1. **Rapport Request Management**
   - List view with filtering (status, client, date)
   - Detail view with all intervention information
   - Create/edit modal forms
   - Status transition buttons
   - Attachment gallery viewer

2. **Template Builder (config-export)**
   - Excel template upload interface
   - Visual mapping builder
   - JSON editor for advanced configuration
   - Test/preview functionality
   - Supported mapping types:
     - Column-based insertion (repeat rows)
     - Dynamic cell assignment
     - Entity field mapping
     - LineType-based data organization

3. **Report Generator (my-report)**
   - Configuration selection dropdown
   - Date range picker (from/to dates)
   - Status filter multi-select
   - Date type selector (creation, completion, validation, planning)
   - Generate button triggers backend processing
   - Download generated Excel file
   - Report history view

4. **Master Data Management**
   - Client CRUD interface
   - Company CRUD interface
   - Project CRUD interface with client/company associations
   - Status code management with color picker

5. **User Management**
   - Account settings
   - Profile update
   - Password change
   - Language selection (i18n: EN, FR, AR-LY)

#### **Services (HTTP Client)**

1. **RapportRequestService** - REST API communication
   - CRUD operations
   - Custom query methods
   - Pagination support
   - Response/request interceptors

2. **ConfigExportService**
   - CRUD for export configurations
   - `generateRapportActivité()` - Trigger Excel generation

3. **Template/Status/File Services** - Entity-specific APIs

4. **AuthService** - Authentication
   - Login/logout
   - Token storage/retrieval
   - User validation

#### **Styling & UI**
- Bootstrap 4/5 grid system
- CoreUI dashboard components
- DevExtreme data grids
- Font Awesome icons
- Custom SCSS for branding
- Responsive mobile-friendly design
- Accessibility compliance

---

### MOBILE: ReportManagerFLUTTER (Flutter Application)

#### **Project Structure**

```
lib/
├── main.dart                      # App entry point
├── app_router.dart               # Navigation routing
├── firebaseApi.dart              # Firebase setup
├── interceptor.dart              # HTTP interceptor
├── business_logic/               # BLoC state management
│   ├── custom_notif_bloc/       # Notification BLoC
│   ├── login_bloc/              # Authentication BLoC
│   ├── password_bloc/           # Password management BLoC
│   ├── profile_bloc/            # User profile BLoC
│   ├── report_request_bloc/     # Intervention BLoC
│   └── report_request_details_bloc/ # Detail BLoC
├── data/
│   ├── models/                  # Data classes
│   │   ├── report_request.dart  # Intervention model
│   │   ├── files.dart           # Attachment model
│   │   ├── status.dart          # Status model
│   │   ├── user.dart            # User model
│   │   └── ...others
│   ├── services/                # API services
│   │   ├── login_web_service.dart
│   │   ├── report_request_service.dart
│   │   ├── files_service.dart
│   │   └── ...others
│   └── repository/              # Data repositories
├── presentation/
│   ├── screens/                 # Full screens
│   │   ├── login_screens/       # Login/registration
│   │   ├── profile_screen/      # User profile
│   │   ├── report_request_screens/
│   │   │   ├── report_request_screen.dart      # List view
│   │   │   ├── report_request_details_screen.dart
│   │   │   ├── files_definition_form_screen.dart
│   │   │   ├── filter_screen.dart
│   │   │   ├── suivi_screen.dart              # Status tracking
│   │   │   └── welcome_menu.dart
│   │   └── forms/               # Reusable form widgets
│   └── widgets/                 # Custom widgets
└── constants/
    └── urls.dart                # API endpoints
```

#### **Core Features**

1. **Authentication & Offline Access**
   - Login with email/password
   - JWT token storage (flutter_secure_storage)
   - Persistent session management
   - Offline mode support

2. **Intervention List (report_request_screen)**
   - Fetch interventions assigned to technician
   - Filter by status (PLANNED, EXECUTED, PENDING_VALIDATION)
   - Search by reference or client
   - Pull-to-refresh
   - Infinite scroll pagination
   - Offline cache (flutter_cache_manager)

3. **Intervention Details (report_request_details_screen)**
   - View full intervention information
   - Display associated template and status
   - Show file attachments gallery
   - Display geolocation (if captured)
   - Execution history

4. **Dynamic Form Generation (files_definition_form_screen)**
   - Parse rapport template JSON
   - Generate form fields dynamically:
     - Text inputs, text areas
     - Dropdown selections
     - Date/time pickers
     - Image/file pickers
   - Form validation rules
   - Save form progress locally
   - Support for conditional fields (if applicable)

5. **File Management**
   - Image picker (camera/gallery)
   - Multiple file attachment
   - Image cropping/editing
   - File upload with progress indicator
   - Offline file queue
   - Sync when connectivity restored

6. **Geolocation & Device Information**
   - GPS coordinates capture (latitude/longitude)
   - Altitude and accuracy
   - Device details (model, OS version)
   - Battery level snapshot
   - Connectivity status (WiFi/mobile)
   - Package info (app version)

7. **Notifications**
   - Firebase Cloud Messaging integration
   - Push notification handling
   - In-app notification display
   - Notification history/archive

8. **Offline Functionality**
   - Local SQLite storage for interventions
   - Cached form templates
   - Offline form submission queue
   - Background sync when online
   - Conflict resolution

9. **User Profile**
   - View user information
   - Edit profile (name, phone, email)
   - Change password
   - Logout

#### **BLoC State Management**

- **LoginBloc:** Authentication state, credentials validation
- **ProfileBloc:** User data, profile updates
- **ReportRequestBloc:** List state, filtering, pagination
- **ReportRequestDetailsBloc:** Detail view, editing
- **CustomNotifBloc:** Notification state and history
- **PasswordBloc:** Password change workflow

#### **Dependencies**
- `flutter_bloc` - State management
- `dio` - HTTP client
- `flutter_secure_storage` - Secure token storage
- `image_picker` - Camera/gallery access
- `firebase_core`, `firebase_messaging` - Push notifications
- `geolocator`, `device_info_plus` - Device sensors
- `connectivity_plus` - Network status
- `infinite_scroll_pagination` - List pagination
- `json_to_form` - Dynamic form generation (custom package)

#### **UI/UX Design**
- Material Design principles
- Color scheme: Primary #3D5A6C (blue), Accent: #FFF7F6C5 (light green)
- Responsive layout for phones and tablets
- Bottom navigation for main features
- FAB (Floating Action Button) for quick actions
- Bottom sheets for forms
- Toast/snackbar notifications

---

## Business Processes Supported

### 1. Field Intervention Lifecycle

```
PROCESS FLOW:

┌─────────────────────────────────────────────────────────────────┐
│ 1. PLANNING PHASE                                               │
├─────────────────────────────────────────────────────────────────┤
│ - Coordinator/Supervisor creates intervention request
│ - Selects rapport template
│ - Assigns to technician or team
│ - Sets planned date and deadline
│ - Status: PLANNED
│ - Notification sent to technician (Firebase)
│ - Stored: RapportRequest with PLANNED status
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. MOBILE EXECUTION PHASE                                        │
├─────────────────────────────────────────────────────────────────┤
│ - Technician receives assignment (mobile notification)
│ - Opens Flutter app and views intervention details
│ - Accesses intervention (online or offline)
│ - Dynamic form loads from cached template
│ - Fills form fields based on template structure
│ - Captures photos/videos as evidence
│ - Records geolocation (GPS coordinates)
│ - Optionally adds execution notes/comments
│ - Status: EXECUTED (locally marked)
│ - Data cached locally if offline
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. SUBMISSION PHASE                                             │
├─────────────────────────────────────────────────────────────────┤
│ - Technician submits completed form + attachments
│ - Mobile app validates form (client-side)
│ - Files uploaded to backend storage
│ - Execution details recorded (timestamp, device info)
│ - Status: SUBMITTED/PENDING_VALIDATION
│ - Stored: Files, ExecutionDetails, RapportRequest
│ - Notification sent to supervisor for review
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. VALIDATION & APPROVAL PHASE                                  │
├─────────────────────────────────────────────────────────────────┤
│ - Supervisor reviews intervention on web (Angular)
│ - Views form data, attachments, geolocation
│ - Checks data quality and compliance
│ - Decision options:
│   a) APPROVE → Status: APPROVED
│   b) REJECT  → Status: REJECTED
│ - If rejected:
│   ├─ Select rejection motif (reason code)
│   ├─ Add comment and evidence
│   ├─ Notify technician to resubmit
│   └─ Store Reject record
│ - If approved:
│   ├─ Status: APPROVED
│   ├─ Intervention considered complete
│   ├─ Data flagged for archival (if delayToArchive reached)
│   └─ Available for report generation
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. REPORT GENERATION PHASE (Optional)                           │
├─────────────────────────────────────────────────────────────────┤
│ - Coordinator selects ConfigExport (report template)
│ - Specifies date range (from/to)
│ - Filters by status, client, project
│ - System queries approved interventions matching criteria
│ - Excel template loaded with:
│   ├─ Master template from ConfigExport.excelFile
│   ├─ Mapping rules from config_mapping_json
│   └─ Data from RapportRequest + LineType details
│ - For each matched intervention:
│   ├─ Map data to Excel cells/rows (dynamic insertion)
│   ├─ Fill column or row templates
│   ├─ Insert start/end dates
│   └─ Apply formatting rules
│ - Generate unique filename and folder
│ - Store metadata in MyReport
│ - Return file for download
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│ 6. ARCHIVAL PHASE                                               │
├─────────────────────────────────────────────────────────────────┤
│ - Scheduled job checks delayToArchive
│ - Approved interventions past delay marked as ARCHIVED
│ - Data retained in database for compliance
│ - Historical reporting available
│ - Status: ARCHIVED
│ - Can be restored if needed
└─────────────────────────────────────────────────────────────────┘
```

### 2. Data Validation Rules

**RapportRequest Validation:**
- Reference: Unique, auto-generated with format and sequence
- Status: Must be valid status code from Status table
- Client final: Required, max 64 characters
- Dates: creation < fin_travaux < validation
- Template: Must exist and be active
- Technician: Valid user from auth system

**Form Data Validation:**
- LineType fields required per template definition
- File counts per FilesDefinition rules (min/max)
- Date fields: Valid ISO 8601 format
- Numeric fields: Range validation per template
- Geolocation: Valid coordinates (-90 to 90 lat, -180 to 180 lon)

**Excel Report Generation:**
- Template file must be valid XLSX
- Mapping JSON must have valid structure
- Cells referenced must exist in template
- Date values must be parseable
- Data range queries must return results

### 3. Status Transition Rules

```
State Machine Transitions:

PLANNED
  ├─→ EXECUTED
  ├─→ CANCELLED
  └─→ ARCHIVED (if delayToArchive reached)

EXECUTED
  ├─→ PENDING_VALIDATION
  └─→ CANCELLED

PENDING_VALIDATION
  ├─→ APPROVED
  ├─→ REJECTED
  └─→ PENDING_VALIDATION (resubmission after rejection)

APPROVED
  ├─→ ARCHIVED (automatic after delay)
  └─→ CANCELLED

REJECTED
  ├─→ EXECUTED (technician resubmits)
  ├─→ PLANNED (reassign)
  └─→ CANCELLED

ARCHIVED
  └─→ (terminal state, can query for reports)

CANCELLED
  └─→ (terminal state)
```

### 4. Permission & Access Control

**Read Access (readAccess):**
- JSONB structure storing allowed roles
- Format: `{"roles": ["ROLE_ADMIN", "ROLE_SUPERVISOR"], "users": [1, 2, 3]}`
- Evaluated on entity fetch
- Filters results based on user role

**Write Access (writeAccess):**
- JSONB structure storing who can modify entity
- Technician: Can only edit own pending/rejected interventions
- Supervisor: Can approve/reject assigned interventions
- Coordinator: Can create/modify templates and configurations
- Admin: Full access to all entities

---

## Core Features & Entity Model

### Primary Entities & Relationships

```
┌─────────────────────────────────────────────────────────────────┐
│ CORE ENTITY RELATIONSHIPS                                        │
├─────────────────────────────────────────────────────────────────┤

RapportTemplate (1)
  ├─→ (N) LineType
  ├─→ (N) FilesDefinition (via LineType)
  ├─→ (N) OutputDefinition
  ├─→ (N) ConfigExport
  └─→ (1) Projet

RapportRequest (1)
  ├─→ (1) RapportTemplate
  ├─→ (1) Status
  ├─→ (N) StatusHistory
  ├─→ (N) Files
  ├─→ (N) Reject (multiple rejection records possible)
  ├─→ (1) ExecutionDetails
  ├─→ (N) CustomNotif
  └─→ (1) Company (executor)

Files (N)
  └─→ (1) RapportRequest

StatusHistory (N)
  └─→ (1) RapportRequest

CustomNotif (N)
  └─→ (1) RapportRequest

NotifHistory (N)
  └─→ (1) RapportRequest

Projet (N)
  ├─→ (1) Client
  └─→ (1) Company

ConfigExport (1)
  └─→ (1) RapportTemplate

MyReport (N)
  └─→ (1) ConfigExport
```

### Key Attributes Explained

**JSONB Columns (Dynamic JSON Storage)**

1. **RapportRequest.reportLineData**
   - Dynamic structure per template
   - Contains form field values submitted by technician
   - Example:
   ```json
   {
     "equipment_type": "Router",
     "serial_number": "ABC123",
     "configuration": {
       "ip_address": "192.168.1.1",
       "backup_ip": "192.168.1.2"
     },
     "issues_found": ["No power", "Ethernet port down"],
     "notes": "Device requires replacement"
   }
   ```

2. **RapportRequest.filedefinition**
   - Cached template file requirements
   - Tracks which files should be attached
   - Example:
   ```json
   [
     {
       "id": 1,
       "name": "Device Photo",
       "required": true,
       "multiple": false,
       "types": ["jpg", "png"]
     },
     {
       "id": 2,
       "name": "Evidence Images",
       "required": false,
       "multiple": true,
       "types": ["jpg", "png", "pdf"]
     }
   ]
   ```

3. **RapportTemplate.mappingImportJson**
   - Schema definition for form fields
   - Used to generate forms dynamically
   - Specifies field types, validation rules, required flags

4. **ConfigExport.configMappingJson**
   - Excel mapping configuration for report generation
   - Maps form data to Excel cells/rows
   - Example:
   ```json
   [
     {
       "type": "Column",
       "dataOrigin": "Entity",
       "templateBookmarkName": "DEVICE_DATA_START",
       "lineTypeCode": "EQUIPMENT",
       "entityDataName": "reportLineData.equipment_type",
       "excelColumnIndex": 0
     },
     {
       "type": "Row",
       "dataOrigin": "LineType",
       "templateBookmarkName": "LINE_ITEMS_START",
       "lineTypeCode": "SERVICE_ITEMS",
       "excelRowOffset": 1
     }
   ]
   ```

5. **RapportRequest.readAccess / writeAccess**
   - Role-based access control
   - Fine-grained permission management
   - Example:
   ```json
   {
     "roles": ["ROLE_ADMIN", "ROLE_SUPERVISOR"],
     "users": [1, 2, 3],
     "companies": [5, 10]
   }
   ```

---

## Non-Functional Requirements

### Performance Requirements

1. **Response Time**
   - API endpoints: < 2 seconds (95th percentile)
   - Report generation: < 5 minutes for 10,000 records
   - Mobile app launch: < 3 seconds
   - Form rendering: < 1 second

2. **Throughput**
   - Concurrent users: 1,000+
   - API calls per second: 500+ RPS
   - Database queries: < 200ms for 95% of queries
   - File upload: Support 100MB+ Excel files

3. **Scalability**
   - Horizontal scaling via microservices
   - Database connection pooling
   - Caching layer (Redis) for frequently accessed data
   - CDN for static assets

### Reliability & Availability

1. **Uptime SLA:** 99.5% availability
2. **Disaster Recovery:** 4-hour RTO, 1-hour RPO
3. **Backup Strategy:**
   - Daily incremental backups
   - Weekly full backups
   - Off-site replication
   - Retention: 30 days

4. **Error Handling:**
   - Graceful degradation in connectivity loss
   - Retry logic with exponential backoff
   - Transaction rollback on failures
   - Error logging and monitoring

### Security Requirements

1. **Authentication**
   - JWT token-based auth
   - Token expiration: 24 hours
   - Refresh token rotation
   - Multi-factor authentication (optional)

2. **Authorization**
   - Role-based access control (RBAC)
   - Fine-grained permissions via JSONB
   - Endpoint-level security annotations
   - Data-level filtering per user role

3. **Data Protection**
   - AES-256 encryption at rest
   - TLS 1.2+ for data in transit
   - Secure password hashing (bcrypt)
   - GDPR compliance (right to deletion)

4. **Audit & Compliance**
   - Comprehensive audit logs
   - User action tracking
   - Status change history with timestamps
   - Compliance reports generation

5. **API Security**
   - Rate limiting (100 req/min per IP)
   - CORS policy enforcement
   - SQL injection prevention (parameterized queries)
   - XSS protection (output encoding)
   - CSRF tokens for state-changing requests

### Compatibility & Interoperability

1. **Browser Support**
   - Chrome 90+
   - Firefox 88+
   - Safari 14+
   - Edge 90+

2. **Mobile Support**
   - Android 6+ (API level 23)
   - iOS 12+
   - Offline functionality

3. **Third-party Integrations**
   - Firebase Cloud Messaging
   - OAuth/OIDC compatibility
   - Standard REST/JSON APIs
   - Excel/XLSX format support

### Maintenance & Supportability

1. **Code Quality**
   - Code coverage: > 80%
   - Static analysis: SonarQube scanning
   - Linting standards (ESLint, Java checkstyle)
   - Architecture documentation

2. **Monitoring & Logging**
   - Centralized logging (ELK stack compatible)
   - Performance metrics (Prometheus)
   - Error tracking (Sentry or similar)
   - Application health checks

3. **Documentation**
   - API documentation (Swagger)
   - Architecture decision records (ADR)
   - Database schema documentation
   - Deployment runbooks

---

## Roles, Permissions & Access Control

### Role Definitions

#### **1. System Administrator (ROLE_ADMIN)**
- **Purpose:** Manage system infrastructure and security
- **Capabilities:**
  - User account creation/deletion
  - Role assignment and permission management
  - System configuration and settings
  - Access all modules and data
  - View audit logs and system reports
  - Database and backup management
- **Restrictions:** None (unrestricted access)

#### **2. Project Coordinator (ROLE_COORDINATOR)**
- **Purpose:** Configure templates and manage report generation
- **Capabilities:**
  - Create and manage rapport templates
  - Configure Excel export mappings
  - Create and test configurations
  - Generate compliance reports
  - Manage master data (clients, companies, projects)
  - View intervention history
  - Assign interventions to supervisors
- **Restrictions:**
  - Cannot approve/reject interventions
  - Cannot modify intervention statuses directly

#### **3. Project Supervisor (ROLE_SUPERVISOR)**
- **Purpose:** Oversee field operations and quality assurance
- **Capabilities:**
  - View assigned interventions
  - Approve or reject submitted data
  - Add rejection comments and motif codes
  - View technician performance metrics
  - Generate operation reports
  - Assign interventions to technicians
  - Update project status
- **Restrictions:**
  - Cannot modify template definitions
  - Cannot delete archived interventions
  - Limited to assigned projects

#### **4. Field Technician (ROLE_TECHNICIAN)**
- **Purpose:** Collect field data and submit interventions
- **Capabilities:**
  - View assigned interventions
  - Fill intervention forms
  - Capture and upload attachments
  - Record geolocation
  - Submit completed forms
  - View submission history
  - Resubmit rejected interventions
  - Receive notifications
- **Restrictions:**
  - Cannot approve/reject
  - Cannot create new interventions
  - Cannot modify templates
  - Read-only access to own data

#### **5. Guest/Anonymous (ROLE_ANONYMOUS)**
- **Purpose:** Limited public access
- **Capabilities:**
  - Public pages (if any)
  - Authentication/login
- **Restrictions:**
  - No access to core features
  - Requires login for functionality

### Permission Model

#### **Entity-Level Access Control**

```
Entity Type          Admin    Coordinator  Supervisor  Technician
────────────────────────────────────────────────────────────────
RapportRequest
  - Read            ✓        ✓            ✓*          ✓*
  - Create          ✓        ✓            ✓           ✗
  - Update Status   ✓        ✗            ✓*          ✗
  - Approve/Reject  ✓        ✗            ✓*          ✗
  - Delete          ✓        ✗            ✗           ✗

RapportTemplate
  - CRUD            ✓        ✓            ✗           ✗

ConfigExport
  - CRUD            ✓        ✓            ✗           ✗
  - Generate Report ✓        ✓            ✗           ✗

Files
  - Read            ✓        ✓            ✓*          ✓*
  - Upload          ✓        ✗            ✓*          ✓*
  - Delete          ✓        ✗            ✓*          ✗

User Management
  - CRUD            ✓        ✗            ✗           ✗
  - Assign Role     ✓        ✗            ✗           ✗

Legend:
  ✓  = Full access
  ✓* = Limited access (filtered by assignment/project)
  ✗  = No access
```

#### **Field-Level Access Control**

Certain sensitive fields are filtered based on role:

```
Field                                  Visible to
─────────────────────────────────────────────────────
technician_id                          Admin, Supervisor, Technician self
read_access, write_access (JSONB)      Admin, Coordinator
reject.comment, reject.evidenceImage   Admin, Supervisor, Technician self
planArchiveDate, archiveDate           Admin, Supervisor, Coordinator
```

#### **Data Filtering by Read/Write Access**

- **readAccess JSONB:** Dynamically filters entities returned in queries
- **writeAccess JSONB:** Controls which users can modify fields
- Example filter for supervisor viewing interventions:
  ```java
  // Only show interventions where supervisor's role is in readAccess
  WHERE readAccess -> 'roles' ? 'ROLE_SUPERVISOR'
     OR readAccess -> 'users' ? ':userId'
  ```

### API Endpoint Security

```
Endpoint                                            Required Role
─────────────────────────────────────────────────────────────────
POST /api/rapport-requests                         ROLE_COORDINATOR
GET /api/rapport-requests                          ROLE_USER
PUT /api/rapport-requests/{id}                     ROLE_SUPERVISOR
POST /api/rapport-requests/{id}/approve            ROLE_SUPERVISOR
POST /api/rapport-requests/{id}/reject             ROLE_SUPERVISOR

POST /api/rapport-templates                        ROLE_COORDINATOR
GET/PUT /api/rapport-templates                     ROLE_COORDINATOR

POST /api/config-exports                           ROLE_COORDINATOR
POST /api/config-exports/{id}/generate-rapport     ROLE_COORDINATOR

POST /api/files                                    ROLE_TECHNICIAN+
GET /api/files                                     ROLE_USER

GET /api/admin/users                               ROLE_ADMIN
POST/PUT/DELETE /api/admin/users                   ROLE_ADMIN

Protected endpoints use @PreAuthorize annotation:
@PreAuthorize("hasAnyRole('ADMIN','COORDINATOR')")
@PreAuthorize("hasAnyRole('ADMIN','SUPERVISOR')")
@PreAuthorize("isAuthenticated()")
```

---

## Technologies & Tools Used

### Backend Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| **Framework** | Spring Boot | 2.2.7.RELEASE | REST API & Microservices |
| **ORM** | Hibernate | 5.4.15.Final | Object-relational mapping |
| **Database** | PostgreSQL | 12+ | Primary data store |
| **Migration** | Liquibase | 3.9+ | Schema versioning |
| **Build** | Maven | 3.3.9+ | Project building & packaging |
| **Security** | Spring Security | Integrated | Authentication & authorization |
| **Validation** | Bean Validation (JSR-380) | - | Input validation |
| **JSON Processing** | Jackson | 2.11+ | JSON serialization |
| **Excel Generation** | Apache POI | 3.11 | XLSX file generation |
| **Cloud Messaging** | Firebase Admin SDK | Latest | Push notifications |
| **Logging** | Logback | 1.2+ | Centralized logging |
| **API Documentation** | Swagger/OpenAPI | 3.0 | API documentation |
| **Code Quality** | SonarQube | Latest | Static analysis |
| **Container** | Docker | 20.10+ | Containerization |
| **Orchestration** | Docker Compose | - | Multi-container orchestration |

### Frontend Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| **Framework** | Angular | 10.0.0 | Web UI framework |
| **Language** | TypeScript | 4.0+ | Programming language |
| **Build Tool** | Webpack | 4.43+ | Module bundler |
| **Package Manager** | npm | 6.14+ | Dependency management |
| **CSS Framework** | Bootstrap | 4.6.2 | Responsive layout |
| **UI Components** | CoreUI | 5.2.2 | Dashboard components |
| **Data Grid** | DevExtreme | 19.1.12 | Advanced data visualization |
| **Forms** | Reactive Forms | Built-in | Complex form handling |
| **HTTP Client** | HttpClient | Built-in | API communication |
| **Routing** | Angular Router | Built-in | Navigation |
| **State Management** | RxJS | 6.5.5 | Reactive programming |
| **Translation** | @ngx-translate | 12.1.2 | i18n support (EN, FR, AR-LY) |
| **Icons** | Font Awesome | 6.5.2 | Icon library |
| **JSON Editor** | ang-jsoneditor | 1.10.5 | JSON configuration editing |
| **PDF Generation** | jsPDF | 2.5.1 | PDF export |
| **Linting** | ESLint | 6.8.0 | Code quality |
| **Code Style** | Prettier | 6.11.0 | Code formatting |

### Mobile Stack

| Component | Technology | Version | Purpose |
|-----------|-----------|---------|---------|
| **Framework** | Flutter | 2.19.2+ | Mobile UI framework |
| **Language** | Dart | 2.19.2+ | Programming language |
| **State Management** | BLoC (flutter_bloc) | 8.1.2+ | State management pattern |
| **HTTP Client** | Dio | 5.1.0 | API communication |
| **Storage** | flutter_secure_storage | 8.0.0 | Encrypted local storage |
| **Push Notifications** | Firebase Messaging | 14.7.10 | Push notifications |
| **Image Handling** | image_picker | 0.8.4+ | Camera/gallery access |
| **File Management** | path_provider | 2.0.5 | File system access |
| **Caching** | flutter_cache_manager | 3.1.2 | HTTP caching |
| **Forms** | Custom (json_to_form) | Local | Dynamic form generation |
| **Geolocation** | geolocator | 8.0.1 | GPS coordinates |
| **Device Info** | device_info_plus | 8.1.0 | Device details |
| **Battery** | battery_plus | 3.0.4 | Battery info |
| **Permissions** | permission_handler | 10.2.0 | Runtime permissions |
| **Pagination** | infinite_scroll_pagination | 3.2.0 | Lazy loading lists |
| **Localization** | intl | 0.17.0 | i18n support |
| **Network Status** | connectivity_plus | 2.2.2 | Connectivity detection |
| **UI Components** | Material Design | Built-in | UI design system |
| **Icons** | cupertino_icons | 1.0.2 | iOS-style icons |

### Development Tools & Infrastructure

| Tool | Purpose | Version |
|------|---------|---------|
| **JHipster** | Project scaffolding | 6.10.5 |
| **Git** | Version control | Latest |
| **GitHub** | Repository hosting | - |
| **Docker** | Containerization | 20.10+ |
| **PostgreSQL** | Database | 12+ |
| **Redis** | Caching (optional) | 6+ |
| **SonarQube** | Code quality scanning | Latest |
| **Jenkins/GitLab CI** | CI/CD pipeline | - |
| **Figma** | UI/UX Design | Latest |
| **Postman** | API testing | Latest |
| **Android Studio** | Android development | Latest |
| **Xcode** | iOS development | Latest |
| **VS Code** | Code editor | Latest |
| **IntelliJ IDEA** | Java IDE | Latest |

### External Services & Integrations

| Service | Provider | Purpose |
|---------|----------|---------|
| **Cloud Messaging** | Firebase Cloud Messaging | Push notifications to mobile devices |
| **Authentication** | OAuth 2.0 / JWT | Secure user authentication |
| **Cloud Storage** | AWS S3 (optional) | File storage for reports |
| **Monitoring** | Prometheus/Grafana | Performance monitoring |
| **Logging** | ELK Stack | Centralized log aggregation |
| **Maps** | Google Maps API | Geolocation services |

### Deployment Architecture

```
┌──────────────────────────────────────┐
│    Docker Compose Environment        │
├──────────────────────────────────────┤
│                                      │
│  ┌─────────────────────────────┐    │
│  │  Docker Containers          │    │
│  ├─────────────────────────────┤    │
│  │ - Angular Web App           │    │
│  │ - Spring Boot Microservice  │    │
│  │ - JHipster Registry (Eureka)│    │
│  │ - PostgreSQL Database       │    │
│  │ - Hazelcast Cache           │    │
│  │ - Prometheus Monitoring     │    │
│  │ - Grafana Dashboards        │    │
│  │ - SonarQube Analysis        │    │
│  └─────────────────────────────┘    │
│                                      │
└──────────────────────────────────────┘
        │
        ├─→ Environment Variables
        ├─→ Volume Mounts (Database, Files)
        ├─→ Network Configuration
        └─→ Health Checks
```

---

## Summary

**RM – Report Manager** is a comprehensive, production-ready platform that digitizes field intervention workflows from end-to-end. By combining a responsive web application for supervisors, an offline-capable mobile app for technicians, and a powerful backend for data management and Excel report generation, the system addresses critical operational needs in IT equipment installation and maintenance.

The architecture leverages modern microservice patterns, cloud-native technologies, and secure authentication mechanisms to ensure scalability, reliability, and compliance. Dynamic form generation, JSONB-based flexible data storage, and sophisticated Excel mapping capabilities enable rapid adaptation to different operator requirements and field scenarios.

With comprehensive role-based access control, detailed audit trails, and real-time notification systems, RM enables organizations to maintain visibility, quality, and accountability across all field operations while reducing manual overhead and processing time.

---

**Document prepared through comprehensive code analysis**  
**Version 1.0 | February 2026**
