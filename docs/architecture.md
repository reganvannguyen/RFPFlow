# RFPFlow System Architecture

## 1. Purpose

This document defines the initial system architecture for RFPFlow.

RFPFlow is an AI-assisted, multi-tenant RFP and proposal-management platform for small software agencies and technology consultancies.

The platform allows organizations to:

1. Create RFP projects.
2. Upload RFP documents.
3. Extract and review requirements.
4. Assign work to team members.
5. Upload trusted company knowledge.
6. Generate grounded proposal responses.
7. Review and approve responses.
8. Check proposal completeness.
9. Export proposal materials.

This architecture supports the complete MVP roadmap while prioritizing Milestone 1:

> A user can create an organization and RFP project, upload a text-based PDF, process it asynchronously, extract candidate requirements with page citations, and review the AI-generated results.

---

## 2. Architectural Goals

The system should be designed around the following goals.

### 2.1 Tenant Isolation

Every organization’s projects, documents, requirements, responses, knowledge base, and exports must remain isolated.

A user must never gain access to another organization’s information by changing a URL, request body, identifier, or storage path.

### 2.2 Human-Supervised AI

AI output must be treated as proposed content rather than automatically trusted content.

Users must review:

* extracted RFP requirements
* generated proposal responses
* company-source citations
* completion and readiness results

AI must not independently:

* approve requirements
* approve responses
* make legal commitments
* invent company capabilities
* determine final submission readiness
* submit proposals

### 2.3 Source Traceability

Extracted requirements should link back to their original RFP source.

Generated proposal claims should link back to trusted company knowledge.

The system must preserve:

* source document
* source page or section
* supporting source text
* extraction or generation metadata

### 2.4 Asynchronous Processing

Long-running tasks must not block normal API requests.

Tasks such as the following should run through background workers:

* PDF text extraction
* document chunking
* requirement extraction
* embedding generation
* AI-assisted response generation
* export generation

### 2.5 Maintainability

The system should separate responsibilities between:

* frontend presentation
* API and authorization
* domain business logic
* background processing
* file storage
* relational data
* AI-provider integration

### 2.6 Incremental Delivery

The architecture must support building the product in vertical slices.

Each feature should include:

* frontend interface
* backend API
* authorization
* database changes
* tests
* error handling
* documentation

### 2.7 Provider Flexibility

The system should avoid tightly coupling business logic to one:

* AI provider
* embedding provider
* object-storage vendor
* background-job framework
* deployment platform

External services should be accessed through application-owned interfaces.

---

## 3. High-Level Architecture

```text
┌─────────────────────────────────────────────────────────────────┐
│                         User Browser                            │
└──────────────────────────────┬──────────────────────────────────┘
                               │ HTTPS
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Next.js Web Application                     │
│                                                                 │
│  - Authentication interface                                    │
│  - Organization and project navigation                         │
│  - Document uploads                                            │
│  - Requirement review                                          │
│  - Assignments and responses                                   │
│  - Compliance and exports                                      │
└──────────────────────────────┬──────────────────────────────────┘
                               │ REST/JSON
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                         FastAPI API                             │
│                                                                 │
│  - Authentication                                              │
│  - Authorization                                               │
│  - Tenant isolation                                            │
│  - Business rules                                              │
│  - API validation                                              │
│  - File-access authorization                                   │
│  - Job creation                                                │
│  - Audit events                                                │
└───────┬──────────────────┬──────────────────┬───────────────────┘
        │                  │                  │
        ▼                  ▼                  ▼
┌───────────────┐  ┌────────────────┐  ┌───────────────────────┐
│  PostgreSQL   │  │ Private Object │  │ Redis / Job Queue     │
│               │  │ Storage        │  │                       │
│ Transactional │  │                │  │ Job messages          │
│ data          │  │ Uploaded files │  │ temporary state       │
│ metadata      │  │ generated      │  │ retry coordination    │
│ audit history │  │ exports        │  │                       │
└───────────────┘  └────────────────┘  └───────────┬───────────┘
                                                   │
                                                   ▼
                                      ┌─────────────────────────┐
                                      │ Background Worker       │
                                      │                         │
                                      │ - PDF extraction        │
                                      │ - Chunking              │
                                      │ - AI extraction         │
                                      │ - Embeddings            │
                                      │ - Draft generation      │
                                      │ - Export generation     │
                                      └───────────┬─────────────┘
                                                  │
                                                  ▼
                                      ┌─────────────────────────┐
                                      │ External AI Provider    │
                                      │                         │
                                      │ - Structured output     │
                                      │ - Embeddings            │
                                      │ - Response generation   │
                                      └─────────────────────────┘
```

---

## 4. Technology Stack

## 4.1 Frontend

* Next.js
* React
* TypeScript
* Next.js App Router
* Tailwind CSS
* shadcn/ui
* TanStack Query
* React Hook Form
* Zod
* Vitest
* React Testing Library
* Playwright

## 4.2 Backend

* Python
* FastAPI
* Pydantic
* SQLAlchemy
* Alembic
* PostgreSQL client library
* pytest
* HTTP client library for external services

## 4.3 Background Processing

Initial recommendation:

* Dramatiq or Celery
* Redis as the message broker

The final framework should be selected during Sprint 0 and recorded in an Architecture Decision Record.

The worker should be a separate process from the API, even if both reuse the same Python application package.

## 4.4 Data and Storage

* PostgreSQL for relational and transactional data
* Redis for background-job coordination and temporary data
* S3-compatible object storage for files
* pgvector or another organization-isolated vector-search solution for embeddings

For local development:

* PostgreSQL through Docker Compose
* Redis through Docker Compose
* MinIO or another local S3-compatible service

## 4.5 Infrastructure

* Docker Compose for local development
* GitHub Actions for continuous integration
* Separate local, staging, and production environments
* Managed PostgreSQL and object storage for production
* Independently deployable web, API, and worker services

---

# 5. Repository Structure

RFPFlow should use a monorepo.

```text
rfpflow/
├── apps/
│   ├── web/
│   ├── api/
│   └── worker/
├── packages/
│   └── shared-types/
├── infrastructure/
├── docs/
│   ├── architecture.md
│   ├── product-requirements.md
│   ├── roadmap.md
│   ├── database-schema.md
│   ├── security.md
│   ├── testing-strategy.md
│   ├── deployment.md
│   ├── adr/
│   └── backlogs/
├── tests/
├── .github/
│   └── workflows/
├── docker-compose.yml
├── .env.example
└── README.md
```

The monorepo allows the frontend, backend, worker, infrastructure, tests, and documentation to evolve together.

It does not mean every application must be deployed as one process.

---

# 6. Frontend Architecture

## 6.1 Frontend Responsibilities

The frontend is responsible for:

* rendering the application interface
* handling navigation
* collecting user input
* performing usability-focused validation
* communicating with the FastAPI backend
* displaying projects, documents, requirements, responses, and status information
* displaying loading, empty, error, and permission states
* showing role-appropriate actions
* managing temporary interface state
* displaying background-job progress
* allowing users to review and correct AI output

The frontend is not responsible for:

* final authorization
* tenant-isolation enforcement
* direct database access
* direct Redis access
* storing object-storage credentials
* calling the AI provider directly
* authoritative file validation
* PDF processing
* approving AI output automatically

FastAPI remains the authoritative security and business-rules boundary.

---

## 6.2 Rendering Strategy

RFPFlow should use the Next.js App Router.

The frontend should use a mixed rendering strategy.

### Server Components

Use server components by default for:

* layouts
* page structure
* static navigation
* low-interactivity content
* server-controlled redirects where practical

### Client Components

Use client components for:

* forms
* document uploads
* upload-progress indicators
* processing-status polling
* requirement tables
* filters and sorting
* dialogs
* review forms
* response editors
* interactive dashboards

A component should become a client component only when it requires:

* browser APIs
* local React state
* event handlers
* TanStack Query hooks
* interactive form behavior

---

## 6.3 Frontend Route Structure

The active organization and project should appear in the URL.

```text
/
├── login
├── register
├── forgot-password
└── app/
    └── organizations/
        ├── new/
        └── [organizationId]/
            ├── dashboard/
            ├── members/
            ├── projects/
            │   ├── new/
            │   └── [projectId]/
            │       ├── overview/
            │       ├── documents/
            │       ├── requirements/
            │       ├── assignments/
            │       ├── responses/
            │       ├── reviews/
            │       ├── compliance/
            │       ├── exports/
            │       └── settings/
            ├── knowledge-base/
            └── settings/
```

Milestone 1 should initially expose only routes that have working functionality:

* authentication
* organization creation
* organization dashboard
* member list
* project list
* project creation
* project overview
* project documents
* project requirements

Routes for later milestones should be added when their features are implemented.

---

## 6.4 Application Layout

The authenticated application should use a consistent application shell.

### Global Header

The global header may include:

* organization switcher
* current project context
* notifications
* user menu
* logout action

### Organization Navigation

The organization-level navigation may include:

* dashboard
* projects
* knowledge base
* members
* organization settings

### Project Navigation

The project-level navigation may include:

* overview
* documents
* requirements
* assignments
* responses
* reviews
* compliance
* exports
* project settings

Navigation items should be added incrementally as features become available.

---

## 6.5 Frontend Folder Structure

The frontend should use feature-based organization.

```text
apps/web/
├── app/
│   ├── login/
│   ├── register/
│   └── app/
├── features/
│   ├── auth/
│   ├── organizations/
│   ├── members/
│   ├── projects/
│   ├── documents/
│   ├── requirements/
│   ├── assignments/
│   ├── knowledge-base/
│   ├── responses/
│   ├── reviews/
│   ├── compliance/
│   └── exports/
├── components/
│   ├── ui/
│   └── layout/
├── lib/
│   ├── api/
│   ├── auth/
│   ├── validation/
│   ├── permissions/
│   └── utils/
├── hooks/
├── types/
└── tests/
```

A feature directory may contain:

```text
features/requirements/
├── api/
├── components/
├── hooks/
├── schemas/
├── types/
├── utils/
└── tests/
```

Generic components belong in `components/ui`.

Domain-specific components belong in the corresponding feature folder.

---

## 6.6 Component Responsibilities

### UI Components

Generic and reusable:

* buttons
* form inputs
* dialogs
* dropdown menus
* tables
* badges
* cards
* alerts
* pagination controls

### Layout Components

Application structure:

* app shell
* global header
* organization sidebar
* project navigation
* breadcrumbs
* page container

### Feature Components

Domain-specific behavior:

* project creation form
* document upload panel
* document-status badge
* requirement table
* requirement review form
* source-evidence panel
* response editor
* compliance summary

### Page Components

Page components should:

* load route context
* coordinate feature components
* define page layout
* trigger top-level queries

Page components should avoid containing large amounts of business logic.

---

## 6.7 API Communication

All frontend communication with FastAPI should use a centralized API client.

```text
Page or component
→ feature-specific query or mutation
→ centralized API client
→ FastAPI endpoint
```

The centralized client should handle:

* API base URL
* JSON serialization
* authentication credentials
* consistent headers
* response parsing
* request identifiers
* validation errors
* authentication failures
* permission failures
* network failures

Example structure:

```text
lib/api/client.ts
features/projects/api/get-projects.ts
features/projects/api/create-project.ts
features/documents/api/upload-document.ts
features/requirements/api/approve-requirement.ts
```

Components should not repeatedly implement raw request handling.

---

## 6.8 Server State

TanStack Query should manage backend-owned data such as:

* current user
* organizations
* memberships
* projects
* documents
* processing statuses
* requirements
* assignments
* responses
* reviews
* compliance results
* exports

TanStack Query should provide:

* caching
* query invalidation
* retries
* loading states
* refetching
* pagination
* polling
* mutation state

Query keys should include tenant and project context.

Example:

```text
["organizations", organizationId, "projects"]

["organizations", organizationId, "projects", projectId, "documents"]

["organizations", organizationId, "projects", projectId, "requirements", filters]
```

This reduces accidental cache collisions between organizations.

---

## 6.9 Client State

Local React state should manage temporary interface state such as:

* whether a dialog is open
* selected table rows
* unsaved form input
* expanded source evidence
* sidebar display state
* active local tabs

The MVP should not introduce Redux or another large global-state system unless a concrete need appears.

Backend data should not be copied into global client state unnecessarily.

---

## 6.10 Authentication

The preferred authentication design is secure cookie-based authentication.

The backend should issue authentication using:

* HTTP-only cookies
* secure cookies in production
* appropriate SameSite settings
* expiration and refresh rules
* server-side invalidation where applicable

The frontend should not store long-lived access tokens in `localStorage`.

The frontend should load the current user from an endpoint such as:

```text
GET /api/v1/auth/me
```

Expected behavior:

* `401 Unauthorized`: redirect to login
* `403 Forbidden`: show a permission-denied state
* expired session: clear authenticated UI and redirect safely
* successful login: redirect to an organization or organization-selection page

---

## 6.11 Organization Context

The organization ID should be part of the route.

Example:

```text
/app/organizations/{organizationId}/projects
```

The frontend should:

* load the user’s membership
* display the active organization
* include organization context in query keys
* update the route when switching organizations
* hide unavailable actions based on role

The backend must independently verify organization membership.

An organization ID in the URL is context, not authorization.

---

## 6.12 Forms and Validation

Forms should use:

* React Hook Form for form state
* Zod for frontend validation
* Pydantic for backend validation

The frontend should validate for usability.

The backend remains authoritative.

Forms include:

* registration
* login
* organization creation
* project creation
* document upload
* requirement editing
* manual requirement creation
* assignment updates
* response editing
* review actions

Backend field errors should be mapped to the corresponding frontend fields when possible.

---

## 6.13 Tables, Filters, and Pagination

Requirement, project, document, response, and audit lists may become large.

The architecture should support backend pagination and filtering.

Filters should be reflected in URL query parameters where practical.

Example:

```text
/app/organizations/123/projects/456/requirements
    ?review_status=pending_review
    &category=security
    &mandatory=true
    &page=2
```

The backend should own:

* pagination
* major filtering
* search
* sorting

The frontend may handle small presentation-only filters locally.

---

## 6.14 Document Uploads

The frontend upload flow should:

1. Allow the user to select a file.
2. Perform basic client-side type and size checks.
3. Upload the file to the FastAPI backend.
4. Display upload progress.
5. Receive a document record and processing status.
6. Display the document immediately.
7. Begin status polling.
8. Stop polling when processing completes or fails.
9. Display retry options for eligible failures.

The backend must repeat all security validation.

The browser must not upload files directly to permanent public storage.

A later implementation may use short-lived presigned upload URLs, but authorization and finalization must remain controlled by the backend.

---

## 6.15 Background-Job Status

The initial MVP should use polling.

```text
Upload document
→ API returns document ID
→ frontend polls document status
→ status becomes completed or failed
→ query polling stops
```

Server-Sent Events or WebSockets may be introduced later if polling becomes inefficient.

The frontend should distinguish:

* upload progress
* queued
* extracting text
* extracting requirements
* completed
* failed

---

## 6.16 Error Handling

The frontend should represent errors consistently.

### Error Categories

* input validation
* authentication
* authorization
* not found
* network
* server
* upload failure
* document-processing failure
* AI-provider failure
* export failure

### Presentation

* field errors near form controls
* action errors near the affected interface
* full-page permission or not-found states when appropriate
* retry actions for recoverable failures
* generic fallback page for unexpected errors

Raw stack traces, storage paths, provider secrets, and internal exceptions must not be displayed.

---

## 6.17 Loading and Empty States

Every major page should define:

* initial loading state
* background refresh state
* empty state
* partial-success state
* failure state

Examples:

* no organizations
* no projects
* no documents
* document queued
* no requirements extracted
* all requirements reviewed
* extraction partially failed
* no assignments
* no company knowledge
* no responses awaiting review

---

## 6.18 Role-Aware UI

The frontend should hide or disable actions that a user cannot perform.

Initial permission expectations:

| Action                        | Admin | Proposal Manager | Contributor | Reviewer |
| ----------------------------- | ----: | ---------------: | ----------: | -------: |
| View organization             |   Yes |              Yes |         Yes |      Yes |
| View projects                 |   Yes |              Yes |         Yes |      Yes |
| Create project                |   Yes |              Yes |          No |       No |
| Upload RFP documents          |   Yes |              Yes |          No |       No |
| View requirements             |   Yes |              Yes |         Yes |      Yes |
| Review extracted requirements |   Yes |              Yes |          No |      Yes |
| Assign requirements           |   Yes |              Yes |          No |       No |
| Manage members                |   Yes |               No |          No |       No |
| Draft assigned responses      |   Yes |              Yes |         Yes |       No |
| Approve responses             |   Yes |              Yes |          No |      Yes |
| Generate exports              |   Yes |              Yes |          No |       No |

This matrix may evolve.

Role-aware UI is for usability and is not a security boundary.

---

## 6.19 Accessibility

The frontend should support:

* keyboard navigation
* semantic HTML
* labelled form controls
* visible focus indicators
* accessible dialogs
* accessible tables
* sufficient colour contrast
* screen-reader-compatible status messages
* form errors associated with fields
* status indicators that do not depend only on colour

---

# 7. Backend Architecture

## 7.1 Backend Responsibilities

FastAPI is the main trust and authorization boundary.

The API is responsible for:

* authentication
* session management
* authorization
* tenant isolation
* request validation
* business rules
* database transactions
* document metadata
* file-access authorization
* background-job creation
* status transitions
* audit-event creation
* provider-independent AI coordination
* export authorization

The API should not perform long-running PDF or AI processing inside normal HTTP requests.

---

## 7.2 Backend Structure

The backend should use domain-oriented modules.

```text
apps/api/app/
├── main.py
├── config/
├── database/
├── middleware/
├── common/
├── auth/
├── users/
├── organizations/
├── memberships/
├── projects/
├── documents/
├── processing/
├── requirements/
├── assignments/
├── knowledge_base/
├── retrieval/
├── responses/
├── reviews/
├── compliance/
├── exports/
├── audit/
└── ai/
```

A domain module may contain:

```text
requirements/
├── models.py
├── schemas.py
├── repository.py
├── service.py
├── permissions.py
├── routes.py
└── tests/
```

---

## 7.3 Backend Layers

### Route Layer

Responsible for:

* HTTP request handling
* dependency injection
* request and response schemas
* status codes
* calling application services

Routes should remain thin.

### Service Layer

Responsible for:

* business rules
* authorization-aware operations
* workflow transitions
* transaction coordination
* audit-event creation
* job creation

### Repository Layer

Responsible for:

* database queries
* persistence
* tenant-scoped data access
* pagination
* row locking where required

### Integration Layer

Responsible for external systems:

* object storage
* Redis
* AI provider
* embedding provider
* email provider
* export libraries

---

## 7.4 API Design

The API should use versioned REST endpoints.

Example:

```text
/api/v1/auth
/api/v1/organizations
/api/v1/organizations/{organization_id}/members
/api/v1/organizations/{organization_id}/projects
/api/v1/organizations/{organization_id}/projects/{project_id}/documents
/api/v1/organizations/{organization_id}/projects/{project_id}/requirements
```

Nested routes make tenant and project context explicit.

Resource IDs alone must never bypass authorization.

---

## 7.5 Validation

Pydantic should validate:

* request payloads
* response payloads
* environment configuration
* AI structured output
* status values
* pagination parameters
* file metadata
* provider responses where practical

Database constraints should provide an additional layer of protection.

---

## 7.6 Transactions

Operations that update related records should use database transactions.

Examples:

* creating an organization and initial admin membership
* creating a document record and processing attempt
* approving a requirement and recording approval metadata
* creating a response version and updating the current response
* approving a response and preserving the approved version
* generating an export record and snapshot metadata

External file or queue operations cannot always be part of the same database transaction.

Those workflows must use explicit failure handling and reconciliation.

---

# 8. Authentication and Authorization

## 8.1 Authentication

Authentication should use secure backend-managed sessions or short-lived tokens delivered through HTTP-only cookies.

Passwords must:

* be hashed using a modern password-hashing algorithm
* never be logged
* never be returned through the API
* never be stored in plain text

Authentication endpoints should be rate-limited in production.

---

## 8.2 Authorization

Authorization must occur in FastAPI for every protected operation.

Authorization checks should confirm:

1. The user is authenticated.
2. The user belongs to the organization.
3. The requested resource belongs to that organization.
4. The user has the required role or permission.
5. The resource is in a state that permits the action.

---

## 8.3 Tenant-Scoped Access

A project query should not be:

```python
get_project(project_id)
```

It should conceptually be:

```python
get_project(
    organization_id=organization_id,
    project_id=project_id,
)
```

The user’s membership must be validated separately.

The same principle applies to:

* documents
* document pages
* chunks
* requirements
* assignments
* company knowledge
* responses
* comments
* reviews
* exports

---

## 8.4 Permission Model

The initial application roles are:

* organization administrator
* proposal manager
* contributor
* reviewer

Permissions should be defined centrally rather than scattered throughout route handlers.

A later version may support custom roles, but the MVP should use fixed roles.

---

# 9. PostgreSQL Architecture

## 9.1 PostgreSQL Responsibilities

PostgreSQL stores:

* users
* organizations
* memberships
* projects
* document metadata
* extracted document pages
* document chunks
* processing attempts
* requirements
* requirement sources
* assignments
* comments
* company knowledge metadata
* embeddings or vector references
* responses
* response versions
* citations
* review actions
* compliance metadata
* export metadata
* audit events
* AI-operation metadata

PostgreSQL should not store large original files directly.

---

## 9.2 Tenant Ownership

Tenant-owned records should contain an `organization_id` where practical.

This provides:

* clearer queries
* easier authorization
* easier indexing
* easier tenant-isolation testing
* simpler operational investigation

Some child records may also be reachable through tenant-owned parents, but explicit organization ownership is preferred for high-risk resources.

---

## 9.3 Identifiers

Primary keys should use UUIDs.

Benefits include:

* reduced predictability
* easier distributed creation
* lower risk of accidental ID collisions across services
* safer public resource identifiers

UUIDs do not replace authorization.

---

## 9.4 Constraints

The database should enforce constraints such as:

* unique normalized user email
* unique membership per user and organization
* valid foreign keys
* valid status values
* valid page numbers
* confidence values between zero and one
* one active assignment where required
* valid source ranges
* unique chunk index per document version
* prevention of duplicate processing artifacts where practical

---

## 9.5 Migrations

Alembic should manage all schema changes.

Rules:

* every schema change requires a migration
* migrations must be reviewed
* CI should validate the migration chain
* application deployment should coordinate migrations safely
* migrations should avoid irreversible destructive changes without a plan

---

# 10. Redis and Background Queue

## 10.1 Redis Responsibilities

Redis should initially support:

* background-job messages
* retry coordination
* temporary locks
* idempotency keys
* short-lived processing state where useful

PostgreSQL remains the authoritative store for durable business state.

Redis data may be lost without losing permanent project information.

---

## 10.2 Queue Responsibilities

The queue should contain small job messages containing identifiers.

Example:

```json
{
  "job_type": "process_rfp_document",
  "document_id": "uuid"
}
```

The queue should not carry complete PDF contents or large extracted text payloads.

The worker should retrieve required data using authorized internal service access.

---

## 10.3 Job Characteristics

Jobs should be:

* idempotent where practical
* retryable
* observable
* associated with processing-attempt records
* safe against duplicate execution
* bounded by timeouts
* explicit about failure states

---

# 11. Background Worker Architecture

## 11.1 Worker Responsibilities

The worker performs long-running and compute-heavy operations.

Milestone 1 responsibilities:

* retrieve uploaded PDF
* extract text
* preserve page boundaries
* normalize text
* create chunks
* invoke AI requirement extraction
* validate AI output
* persist candidate requirements
* update processing status
* record failures

Later responsibilities:

* process company knowledge
* generate embeddings
* create response drafts
* generate exports

---

## 11.2 Worker Code Reuse

The API and worker should reuse shared domain, database, and integration code.

Business logic should not be copied between separate implementations.

The worker may import the application package while running as an independent process.

---

## 11.3 Job State

Durable job state should be stored in PostgreSQL.

Possible processing-attempt fields:

* attempt ID
* organization ID
* document ID
* job type
* status
* started time
* completed time
* error category
* safe error message
* retry count
* triggering user
* worker identifier

---

## 11.4 Failure Handling

Failures should be classified where practical:

* file missing
* unsupported document
* text extraction failed
* likely scanned PDF
* chunking failed
* AI timeout
* AI invalid output
* provider unavailable
* database failure
* storage failure

Users should see understandable messages.

Internal logs may contain more technical detail but must avoid unnecessary document content.

---

# 12. Object-Storage Architecture

## 12.1 Storage Responsibilities

Private object storage should contain:

* uploaded RFP documents
* company knowledge documents
* project attachments
* generated DOCX files
* generated spreadsheet exports
* temporary generated artifacts when required

---

## 12.2 Storage Keys

Storage keys should be generated by the application.

Example:

```text
organizations/{organization_id}/projects/{project_id}/documents/{document_id}/original.pdf
```

Knowledge documents may use:

```text
organizations/{organization_id}/knowledge/{document_id}/original.pdf
```

Storage paths improve organization but do not replace authorization.

---

## 12.3 Access

Files must be private by default.

File access should use one of the following:

* authorized backend streaming
* short-lived presigned URLs created after backend authorization

Permanent public URLs must not be used for private RFP documents.

---

## 12.4 File Validation

The backend should validate:

* file size
* MIME type
* file signature where practical
* extension
* empty files
* supported format
* sanitized filename

A malware-scanning integration point should be planned for production hardening.

---

# 13. AI Integration Architecture

## 13.1 AI Boundary

The AI provider must be accessed only through backend or worker integration code.

The frontend must never receive:

* provider API keys
* provider credentials
* direct unrestricted model access

---

## 13.2 Provider Interface

Business logic should call an application-owned interface.

Conceptual example:

```python
class AIProvider:
    async def extract_requirements(...): ...
    async def generate_embedding(...): ...
    async def generate_response(...): ...
```

This allows the provider implementation to change without rewriting core domain logic.

---

## 13.3 Structured Output

Requirement extraction must use a strict schema.

The worker should:

1. Build the prompt.
2. Send source content and metadata.
3. Receive structured output.
4. Parse the output.
5. Validate it with Pydantic.
6. reject, repair, or retry invalid output.
7. Store only validated candidates.

Raw model output must not be trusted automatically.

---

## 13.4 Prompt Versioning

Prompts should be stored outside route handlers.

Each important AI operation should record:

* operation type
* prompt version
* provider
* model
* configuration
* source references
* start time
* completion time
* usage information where available
* status

---

## 13.5 AI Data Separation

The system must distinguish:

### RFP Context

Describes what the buyer requires.

### Company Knowledge

Describes what the vendor can truthfully claim.

These sources must not be treated as interchangeable.

A requirement from an RFP is not proof that the company satisfies it.

---

# 14. Document-Processing Flow

## 14.1 Upload Flow

```text
User selects PDF
→ frontend performs basic validation
→ frontend sends upload request
→ FastAPI authenticates user
→ FastAPI validates membership and permission
→ FastAPI validates file
→ FastAPI stores file privately
→ FastAPI creates document record
→ FastAPI creates processing record
→ FastAPI queues background job
→ API returns document status
→ frontend begins polling
```

---

## 14.2 Text-Extraction Flow

```text
Worker receives document ID
→ worker loads document metadata
→ worker retrieves file from object storage
→ worker marks document as extracting text
→ worker extracts text by page
→ worker normalizes page text
→ worker stores page records
→ worker detects low-text or scanned PDFs
→ worker creates traceable document chunks
```

---

## 14.3 Requirement-Extraction Flow

```text
Worker loads document chunks
→ worker includes page metadata
→ worker calls AI provider
→ AI returns structured candidate requirements
→ worker validates response schema
→ worker stores valid candidate requirements
→ candidates begin as pending review
→ worker records AI metadata
→ worker marks document completed
→ frontend stops polling
```

---

## 14.4 Requirement-Review Flow

```text
User opens requirement list
→ frontend requests tenant-scoped requirements
→ FastAPI validates organization membership
→ user opens candidate
→ frontend displays requirement and source evidence
→ user edits, approves, rejects, or requests clarification
→ FastAPI validates permission and state
→ FastAPI updates requirement
→ FastAPI records audit event
→ frontend refreshes review progress
```

---

# 15. Knowledge-Base and RAG Flow

This flow is implemented in later milestones.

## 15.1 Knowledge Processing

```text
Administrator uploads company document
→ API validates access and file
→ worker extracts and normalizes text
→ worker creates chunks
→ worker generates embeddings
→ chunks and vectors are stored with organization ownership
```

## 15.2 Retrieval

```text
User opens RFP requirement
→ backend builds retrieval query
→ vector search is restricted to organization
→ relevant company chunks are returned
→ sources remain linked to original documents
```

## 15.3 Response Generation

```text
Contributor requests draft
→ backend loads requirement and RFP context
→ backend retrieves company evidence
→ backend separates RFP context from company evidence
→ worker calls AI provider
→ model generates grounded draft
→ citations are validated and stored
→ response version is created
→ contributor reviews and edits
```

---

# 16. Response Review Flow

```text
Contributor saves response
→ contributor submits response for review
→ response status becomes ready for review
→ reviewer opens response and citations
→ reviewer approves or requests changes
→ action creates review record and audit event
→ approved version is preserved
```

Editing an approved response should create a new revision and invalidate the current approval until the new version is reviewed.

---

# 17. Compliance and Export Flow

## 17.1 Compliance

The compliance service evaluates:

* mandatory requirements
* assignments
* due dates
* blockers
* response status
* approval status
* attachment requirements
* extraction-review status

The service returns calculated readiness information.

The system must not present readiness as legal or contractual approval.

## 17.2 Export

```text
Proposal manager requests export
→ API validates permission
→ API creates export job
→ worker loads a consistent project snapshot
→ worker generates DOCX or spreadsheet
→ worker stores file privately
→ export record is marked completed
→ user receives authorized download
```

---

# 18. Trust Boundaries

## 18.1 Browser Boundary

The browser is untrusted.

The backend must not trust:

* organization IDs
* project IDs
* role claims
* file metadata
* status values
* hidden fields
* frontend validation results

## 18.2 API Boundary

FastAPI is the main application trust boundary.

It validates:

* authentication
* authorization
* tenancy
* request data
* workflow transitions
* file access
* job creation

## 18.3 Worker Boundary

The worker is trusted application infrastructure but must still:

* verify record state
* load tenant-owned resources carefully
* avoid trusting queue payloads beyond identifiers
* validate AI output
* use least-privilege credentials

## 18.4 External Provider Boundary

AI and storage providers are external systems.

The application must:

* minimize transmitted data
* protect credentials
* handle outages
* validate responses
* avoid exposing one organization’s data to another
* document provider data-handling assumptions

---

# 19. Security Architecture

The system should implement:

* secure password hashing
* HTTP-only authentication cookies
* CSRF protection where required
* restricted CORS
* backend role enforcement
* tenant-scoped queries
* private object storage
* short-lived signed URLs
* file size and format validation
* upload limits
* rate limiting for sensitive endpoints
* environment-based secret management
* structured logging without secrets
* audit events
* database constraints
* dependency scanning
* automated tenant-isolation tests

A separate `docs/security.md` file should expand the threat model and specific controls.

---

# 20. Observability

## 20.1 Logging

Logs should include:

* timestamp
* severity
* environment
* service name
* request ID
* job ID
* organization ID where safe
* project ID where safe
* document ID where safe
* event type
* error category

Logs should not include:

* passwords
* access tokens
* provider secrets
* complete confidential documents
* unnecessary prompt contents
* signed storage URLs

## 20.2 Metrics

Useful metrics include:

* API request duration
* API failure rate
* queued job count
* processing duration
* processing failure rate
* PDF extraction duration
* AI operation duration
* AI invalid-output rate
* token usage
* export duration

## 20.3 Audit Events

Audit events should record business-relevant actions such as:

* organization creation
* membership changes
* project creation
* document upload
* processing retry
* requirement edits
* requirement approval
* requirement rejection
* assignment changes
* response generation
* response approval
* export generation

---

# 21. Testing Architecture

## 21.1 Backend Unit Tests

Test:

* permission rules
* status transitions
* validation
* document normalization
* chunking
* compliance calculations
* provider-response parsing

## 21.2 API Integration Tests

Test:

* authentication
* organization creation
* tenant isolation
* project operations
* file upload
* background-job creation
* requirement review
* response workflow
* export authorization

## 21.3 Worker Tests

Test:

* job idempotency
* PDF extraction
* page preservation
* chunking
* AI validation
* retry behavior
* failure-state updates

## 21.4 Frontend Tests

Test:

* forms
* validation messages
* upload states
* processing-status display
* requirement-review interactions
* role-aware actions
* error and empty states

## 21.5 End-to-End Tests

The primary Milestone 1 test should cover:

```text
Register
→ log in
→ create organization
→ create project
→ upload PDF
→ wait for processing
→ view candidate requirements
→ inspect source evidence
→ edit requirement
→ approve requirement
→ reject requirement
→ add requirement manually
```

The full MVP test should continue through:

```text
Assign requirement
→ upload company knowledge
→ generate grounded response
→ request review
→ approve response
→ check compliance
→ export proposal
```

## 21.6 AI Evaluations

AI evaluation should measure:

* requirement recall
* requirement precision
* source-page accuracy
* duplicate rate
* category accuracy
* retrieval relevance
* citation validity
* unsupported claims
* insufficient-evidence behavior

---

# 22. Deployment Architecture

## 22.1 Environments

RFPFlow should use:

* local development
* staging
* production

Each environment should have separate:

* database
* Redis instance
* object-storage bucket
* secrets
* AI-provider configuration
* application URLs

---

## 22.2 Deployment Units

Deployable units:

* Next.js web application
* FastAPI API
* background worker
* PostgreSQL
* Redis
* object storage

The web, API, and worker should be independently restartable.

---

## 22.3 Initial Deployment Option

A practical portfolio deployment may use:

* Vercel for Next.js
* Render, Railway, Fly.io, or a cloud container platform for FastAPI and worker
* managed PostgreSQL
* managed Redis
* S3-compatible object storage

The exact deployment providers should be recorded in a later ADR.

---

## 22.4 CI/CD

The continuous-integration pipeline should run:

1. dependency installation
2. formatting checks
3. linting
4. type checking
5. backend unit tests
6. backend integration tests
7. frontend tests
8. production builds
9. migration validation
10. security or dependency checks where practical

Deployment should occur only after required checks pass.

Production database migrations should use a controlled deployment step.

---

# 23. Scalability Considerations

The MVP is optimized for correctness and maintainability rather than very large scale.

Initial scaling methods include:

* multiple stateless API instances
* multiple worker instances
* queue-based workload distribution
* managed PostgreSQL
* database indexes
* paginated endpoints
* object storage for files
* cached or precomputed status summaries where needed

Potential future improvements:

* dedicated document-processing queues
* provider-specific rate limiting
* batch embeddings
* read replicas
* separate vector database
* event-driven notifications
* Server-Sent Events
* project-level processing quotas

These should not be implemented until justified by actual usage.

---

# 24. Initial Architectural Decisions

The following decisions should be recorded as separate ADRs.

## ADR-001: Use a Monorepo

Store frontend, backend, worker, infrastructure, and documentation in one repository.

## ADR-002: Use Next.js and TypeScript

Use Next.js App Router for the web application.

## ADR-003: Use FastAPI and Python

Use FastAPI for the application API and Python-based AI and document-processing workflows.

## ADR-004: Use PostgreSQL

Use PostgreSQL as the authoritative relational data store.

## ADR-005: Use Background Workers

Run long document, AI, and export operations outside HTTP requests.

## ADR-006: Use Private Object Storage

Store uploaded documents and generated exports outside PostgreSQL.

## ADR-007: Use HTTP-Only Cookie Authentication

Avoid storing long-lived access tokens in browser local storage.

## ADR-008: Preserve Page-Level Sources

Store page-level extracted text and page ranges for traceability.

## ADR-009: Require Human Review

Treat AI-extracted requirements and generated responses as proposed content.

## ADR-010: Enforce Tenant Isolation in the Backend

Do not rely on frontend routing or hidden controls for organization security.

## ADR-011: Use Feature-Based Frontend Organization

Keep domain-specific UI, hooks, schemas, and API functions together.

## ADR-012: Start with Polling

Use polling for document-processing progress during the MVP.

## ADR-013: Validate AI Output

Use strict Pydantic schemas before storing AI-generated structured data.

## ADR-014: Separate RFP and Company Context

Do not treat buyer requirements as evidence of vendor capabilities.

---

# 25. Known Tradeoffs

## 25.1 Application-Enforced Multi-Tenancy

The MVP uses shared tables with organization ownership and backend authorization.

Advantages:

* simpler infrastructure
* easier development
* suitable for a portfolio-scale application

Risks:

* every tenant-owned query must be correctly scoped
* authorization mistakes can expose data

Mitigation:

* reusable authorization dependencies
* repository query conventions
* explicit `organization_id`
* cross-tenant integration tests

---

## 25.2 Polling Instead of Real-Time Events

Polling is easier to implement and test.

Disadvantages:

* repeated requests
* delayed updates
* less efficient at large scale

It is acceptable for Milestone 1 and can later be replaced with Server-Sent Events.

---

## 25.3 Shared API and Worker Codebase

The API and worker reuse application code but run as separate processes.

Advantages:

* reduced duplication
* shared domain models
* consistent validation

Risk:

* poor module boundaries could create tight coupling

Mitigation:

* domain modules
* clear services
* provider interfaces
* independent process startup

---

## 25.4 PostgreSQL-Based Vector Search

Using pgvector initially reduces infrastructure complexity.

Potential disadvantages:

* less specialized than a dedicated vector database
* may require migration later for very large workloads

This is acceptable for the MVP.

---

# 26. Milestone 1 Architecture Scope

The first implemented architecture should include:

* Next.js web application
* FastAPI API
* PostgreSQL
* Redis
* background worker
* private object storage
* authentication
* organizations and memberships
* RFP projects
* PDF uploads
* page-level PDF extraction
* document chunks
* AI requirement extraction
* requirement review
* audit events
* automated testing
* continuous integration

Later milestone modules may appear in the architecture but should not be implemented prematurely.

---

# 27. Architecture Validation Checklist

Before Sprint 0 is considered complete, verify that:

* the repository structure is established
* the frontend starts locally
* the API starts locally
* PostgreSQL is reachable
* Redis is reachable
* object storage is reachable
* the worker starts
* the API can enqueue a test job
* the worker can complete a test job
* migrations can be applied
* backend tests run
* frontend tests run
* CI runs on pull requests
* environment variables are documented
* secrets are excluded from source control
* health-check endpoints are available
* service responsibilities are documented

---

# 28. Next Architecture Deliverable

After this architecture document is accepted, create:

```text
docs/database-schema.md
```

That document should define:

* entities
* fields
* primary keys
* foreign keys
* tenant ownership
* relationships
* constraints
* indexes
* deletion behavior
* status enumerations
* an entity-relationship diagram

The database design should begin with Milestone 1 entities while accounting for later MVP relationships.
