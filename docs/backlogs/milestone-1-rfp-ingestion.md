# RFPFlow MVP Backlog

## 1. Purpose

This document defines the implementation backlog for the first major RFPFlow milestone.

The backlog converts the Product Requirements Document into prioritized epics, user stories, acceptance criteria, dependencies, and technical tasks.

The first milestone is:

> A user can create an organization and an RFP project, upload a text-based PDF, process it asynchronously, and review AI-extracted candidate requirements with source-page citations.

This milestone does not yet include:

* company knowledge-base retrieval
* AI-assisted response generation
* requirement assignments
* response approval workflows
* compliance dashboards
* proposal exports
* procurement-portal submission

---

## 2. Backlog Conventions

### 2.1 Priority Levels

| Priority | Meaning                                            |
| -------- | -------------------------------------------------- |
| P0       | Required for the first milestone                   |
| P1       | Important but not required for the first milestone |
| P2       | Useful improvement that can be completed later     |

### 2.2 Story Statuses

Stories may use the following statuses:

* Backlog
* Ready
* In Progress
* In Review
* Blocked
* Done

### 2.3 Story Point Scale

Story points estimate relative complexity rather than exact development time.

| Points | General Meaning                          |
| -----: | ---------------------------------------- |
|      1 | Very small change                        |
|      2 | Small change                             |
|      3 | Moderate change                          |
|      5 | Significant change                       |
|      8 | Large or uncertain change                |
|     13 | Too large and should probably be divided |

### 2.4 Definition of Ready

A story is ready for implementation when:

* the business goal is clear
* acceptance criteria are defined
* dependencies are identified
* required designs or API contracts are available
* unresolved questions do not prevent implementation
* the story is small enough to complete within one sprint

### 2.5 Definition of Done

A story is complete when:

* all acceptance criteria are satisfied
* backend authorization is enforced where applicable
* automated tests are added or updated
* linting and type checking pass
* database migrations are included when required
* error states are handled
* relevant documentation is updated
* the implementation is reviewed through a pull request
* the feature works in the development environment
* no critical known defect remains

---

# 3. Milestone 1 Scope

## Included

The first milestone includes:

1. Project foundation
2. Authentication
3. Organization creation
4. Organization membership and roles
5. RFP project creation and viewing
6. PDF document upload
7. Secure file storage
8. Background processing jobs
9. PDF text extraction
10. Page-level source preservation
11. AI-assisted requirement extraction
12. Structured-output validation
13. Requirement review
14. Requirement editing and approval
15. Tenant isolation
16. Audit events for important actions
17. Automated testing
18. Continuous integration
19. Development documentation

## Excluded

The following are outside the first milestone:

* assignments to contributors
* internal requirement deadlines
* company knowledge-base uploads
* embeddings and vector search
* response drafting
* response citations
* response version history
* reviewer approval workflow
* compliance dashboard
* proposal export
* payment and subscription features
* mobile applications
* OCR for scanned PDFs
* DOCX and XLSX ingestion
* advanced notifications
* real-time collaboration

---

# 4. Epic Summary

| Epic ID  | Epic                                   | Priority | Milestone   |
| -------- | -------------------------------------- | -------: | ----------- |
| EPIC-001 | Project Foundation                     |       P0 | Milestone 1 |
| EPIC-002 | Authentication                         |       P0 | Milestone 1 |
| EPIC-003 | Organizations and Membership           |       P0 | Milestone 1 |
| EPIC-004 | RFP Project Management                 |       P0 | Milestone 1 |
| EPIC-005 | Document Upload and Storage            |       P0 | Milestone 1 |
| EPIC-006 | Background Document Processing         |       P0 | Milestone 1 |
| EPIC-007 | PDF Text Extraction                    |       P0 | Milestone 1 |
| EPIC-008 | AI Requirement Extraction              |       P0 | Milestone 1 |
| EPIC-009 | Requirement Review                     |       P0 | Milestone 1 |
| EPIC-010 | Tenant Isolation and Security          |       P0 | Milestone 1 |
| EPIC-011 | Auditability and Observability         |       P1 | Milestone 1 |
| EPIC-012 | Testing and Continuous Integration     |       P0 | Milestone 1 |
| EPIC-013 | Documentation and Developer Experience |       P0 | Milestone 1 |

---

# 5. EPIC-001: Project Foundation

## Epic Goal

Create a reliable local-development foundation for the RFPFlow frontend, backend, worker, database, and supporting services.

---

## STORY-001: Create the monorepo structure

**Priority:** P0
**Story Points:** 3
**Dependencies:** None

### User Story

As a developer,
I want a clearly organized monorepo,
so that the frontend, backend, worker, infrastructure, and documentation can be developed consistently.

### Acceptance Criteria

* A Git repository is initialized.
* The repository contains directories for the web application, API, worker, infrastructure, and documentation.
* Each application has its own dependency configuration.
* Shared project documentation is stored in a top-level `docs` directory.
* Root-level commands are documented.
* Generated files and secrets are excluded through `.gitignore`.
* The repository structure is explained in the README.

### Suggested Structure

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
├── .github/
│   └── workflows/
├── docker-compose.yml
├── .env.example
└── README.md
```

### Technical Tasks

* Initialize Git.
* Create root directories.
* Add root README.
* Add `.gitignore`.
* Add `.editorconfig`.
* Add license if appropriate.
* Add contribution guidelines if desired.

---

## STORY-002: Initialize the Next.js frontend

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-001

### User Story

As a developer,
I want a typed frontend application,
so that user-facing RFPFlow features can be built consistently.

### Acceptance Criteria

* The frontend uses Next.js.
* The frontend uses React and TypeScript.
* Tailwind CSS is configured.
* A component-library strategy is selected and documented.
* A basic application shell is displayed.
* Environment variables are loaded safely.
* Linting and formatting commands are available.
* The production build succeeds.

### Technical Tasks

* Create the Next.js project.
* Configure TypeScript strict mode.
* Configure Tailwind CSS.
* Add a basic layout.
* Add an application navigation placeholder.
* Add environment-variable validation.
* Add lint and format scripts.

---

## STORY-003: Initialize the FastAPI backend

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-001

### User Story

As a developer,
I want a structured backend application,
so that business logic and APIs can be implemented reliably.

### Acceptance Criteria

* The backend uses FastAPI and Python.
* The backend exposes a health-check endpoint.
* Application settings are loaded from environment variables.
* API routes use versioned prefixes.
* Dependency injection is used where appropriate.
* Structured error responses are supported.
* Logging is configured.
* Automated tests can start the application.

### Technical Tasks

* Create the FastAPI application.
* Add `/api/v1/health`.
* Configure application settings.
* Configure CORS for local development.
* Add route modules.
* Add error-handling middleware.
* Add logging configuration.

---

## STORY-004: Configure PostgreSQL and migrations

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-003

### User Story

As a developer,
I want a version-controlled relational database,
so that application data can be stored and changed safely.

### Acceptance Criteria

* PostgreSQL is available locally through Docker.
* SQLAlchemy is configured.
* Alembic is configured.
* An initial migration can be created and applied.
* Database connections are managed safely.
* Tests can use a separate test database.
* Migration instructions are documented.

### Technical Tasks

* Add PostgreSQL to Docker Compose.
* Configure SQLAlchemy.
* Configure Alembic.
* Create the initial migration.
* Add database session dependencies.
* Add migration scripts to project commands.

---

## STORY-005: Configure Redis and the worker foundation

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-003

### User Story

As a developer,
I want background-job infrastructure,
so that document processing does not block HTTP requests.

### Acceptance Criteria

* Redis is available locally.
* A background worker starts successfully.
* The API can enqueue a test job.
* The worker can execute the job.
* Failed jobs are logged.
* Job configuration is loaded from environment variables.
* Worker startup instructions are documented.

### Technical Tasks

* Select Celery, Dramatiq, ARQ, or another Python job framework.
* Add Redis to Docker Compose.
* Create a worker application.
* Add a test task.
* Configure retries and timeouts.
* Add worker health logging.

---

## STORY-006: Create a unified local-development environment

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-002, STORY-003, STORY-004, STORY-005

### User Story

As a developer,
I want to start the complete application locally,
so that all services can be tested together.

### Acceptance Criteria

* Docker Compose starts required infrastructure.
* The frontend can communicate with the backend.
* The backend can communicate with PostgreSQL and Redis.
* The worker can consume queued jobs.
* Required environment variables are documented.
* A new developer can follow the README to start the project.
* Health checks are included where practical.

### Technical Tasks

* Finalize Docker Compose.
* Add `.env.example`.
* Add startup scripts.
* Verify networking between services.
* Document local URLs.
* Add troubleshooting notes.

---

# 6. EPIC-002: Authentication

## Epic Goal

Allow users to create accounts, authenticate securely, and access protected application pages.

---

## STORY-007: Create the user data model

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-004

### User Story

As a developer,
I want a user data model,
so that registered users can be stored securely.

### Acceptance Criteria

* Users have unique identifiers.
* Email addresses are unique and normalized.
* Passwords are never stored in plain text.
* User records include creation and update timestamps.
* Disabled users can be prevented from authenticating.
* A database migration creates the required table.

### Suggested Fields

* `id`
* `email`
* `password_hash`
* `first_name`
* `last_name`
* `is_active`
* `created_at`
* `updated_at`

---

## STORY-008: Register a user account

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-007

### User Story

As a new user,
I want to register an account,
so that I can use RFPFlow.

### Acceptance Criteria

* The user can enter a name, email address, and password.
* Invalid email addresses are rejected.
* Weak or invalid passwords are rejected according to documented rules.
* Duplicate email addresses are rejected.
* Passwords are securely hashed.
* Successful registration returns a safe user representation.
* The password hash is never returned.
* Registration errors are understandable.
* Automated tests cover successful and failed registration.

---

## STORY-009: Log in to the application

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-008

### User Story

As a registered user,
I want to log in securely,
so that I can access my organizations and projects.

### Acceptance Criteria

* A user can log in with email and password.
* Invalid credentials return a generic authentication error.
* Disabled accounts cannot log in.
* A secure session or token is issued after authentication.
* Authentication secrets are not exposed to the frontend.
* Protected API endpoints reject unauthenticated requests.
* Automated tests cover valid and invalid login attempts.

---

## STORY-010: Log out of the application

**Priority:** P0
**Story Points:** 2
**Dependencies:** STORY-009

### User Story

As an authenticated user,
I want to log out,
so that other people cannot continue using my session.

### Acceptance Criteria

* A logged-in user can log out.
* The active session or token is invalidated or removed appropriately.
* The frontend returns the user to the login page.
* Protected pages cannot be accessed after logout.

---

## STORY-011: Retrieve the current user

**Priority:** P0
**Story Points:** 2
**Dependencies:** STORY-009

### User Story

As an authenticated user,
I want the application to know who I am,
so that my identity and permissions can be displayed correctly.

### Acceptance Criteria

* The backend exposes a current-user endpoint.
* The endpoint returns only safe user information.
* Unauthenticated requests are rejected.
* The frontend loads the current user when the application starts.
* Loading and error states are handled.

---

## STORY-012: Protect frontend routes

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-009, STORY-011

### User Story

As a user,
I want private pages to require authentication,
so that project information is not exposed publicly.

### Acceptance Criteria

* Unauthenticated users are redirected to the login page.
* Authenticated users can access protected pages.
* Authentication state persists appropriately after page refresh.
* The frontend does not rely on route protection as the only security control.
* Backend authorization remains authoritative.

---

## STORY-013: Reset a forgotten password

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-008

### User Story

As a user who forgot my password,
I want to reset it securely,
so that I can regain access to my account.

### Acceptance Criteria

* A user can request a password-reset link.
* The response does not reveal whether an email exists.
* Reset tokens expire.
* Reset tokens can be used only once.
* A valid token allows the user to set a new password.
* Existing sessions may be invalidated after reset.
* Password-reset events are recorded.

---

# 7. EPIC-003: Organizations and Membership

## Epic Goal

Allow users to create organizations and ensure that organization-owned data is isolated.

---

## STORY-014: Create the organization data model

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-004, STORY-007

### User Story

As a developer,
I want an organization data model,
so that RFPFlow can support multiple separate companies.

### Acceptance Criteria

* Organizations have unique identifiers.
* Organizations have names.
* Organization records include creation and update timestamps.
* Organization ownership is represented through membership rather than only a user field.
* A database migration creates the organization table.

### Suggested Fields

* `id`
* `name`
* `slug`
* `created_at`
* `updated_at`

---

## STORY-015: Create the organization-membership model

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-014

### User Story

As a developer,
I want organization memberships and roles,
so that users can belong to organizations with specific permissions.

### Acceptance Criteria

* A user can belong to one or more organizations.
* Memberships have roles.
* Duplicate membership records are prevented.
* Memberships include creation timestamps.
* The initial supported roles are documented.
* A database migration creates the required table.

### Initial Roles

* `organization_admin`
* `proposal_manager`
* `contributor`
* `reviewer`

For the first milestone, some roles may share permissions, but the data model should support future separation.

---

## STORY-016: Create an organization

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-014, STORY-015

### User Story

As an authenticated user,
I want to create an organization,
so that my company can manage its RFP projects.

### Acceptance Criteria

* An authenticated user can create an organization.
* An organization name is required.
* The creator becomes an organization administrator.
* The organization appears in the creator’s organization list.
* Unauthenticated users cannot create organizations.
* Organization creation is recorded in the audit history.
* Automated tests verify the creator’s membership and role.

---

## STORY-017: List the current user’s organizations

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-016

### User Story

As a user,
I want to view organizations I belong to,
so that I can select the correct workspace.

### Acceptance Criteria

* The user sees only organizations they belong to.
* Each result includes the user’s membership role.
* Users cannot retrieve organizations through another user’s membership.
* Empty-state messaging is displayed when the user has no organizations.

---

## STORY-018: Select an active organization

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-017

### User Story

As a user who belongs to multiple organizations,
I want to select an active organization,
so that I can work within the correct company workspace.

### Acceptance Criteria

* The user can select an organization they belong to.
* The active organization is reflected in application routing or state.
* An organization switch updates displayed data.
* A user cannot select an organization they do not belong to.
* Refreshing the page preserves or safely restores the active organization.

---

## STORY-019: View organization members

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-015, STORY-018

### User Story

As an organization member,
I want to view other organization members,
so that I know who is part of the proposal team.

### Acceptance Criteria

* Members can view a list of users in their organization.
* The list displays names, emails, and roles.
* Users cannot view members of organizations they do not belong to.
* Sensitive authentication information is never returned.
* Pagination is supported or planned for larger organizations.

---

## STORY-020: Invite an organization member

**Priority:** P1
**Story Points:** 8
**Dependencies:** STORY-019

### User Story

As an organization administrator,
I want to invite another user,
so that team members can collaborate in RFPFlow.

### Acceptance Criteria

* Only authorized roles can create invitations.
* An invitation includes an email address and role.
* Invitation tokens expire.
* Duplicate active invitations are handled.
* Existing organization members are not invited twice.
* An invited user can accept the invitation.
* The accepted user becomes an organization member.
* Invitation actions are audited.

### Milestone Note

For an initial demonstration, direct member creation may temporarily replace email delivery, but the limitation must be documented.

---

## STORY-021: Change a member’s role

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-019

### User Story

As an organization administrator,
I want to change a member’s role,
so that their permissions match their responsibilities.

### Acceptance Criteria

* Only authorized users can change roles.
* A role must be one of the supported values.
* Users cannot change roles in another organization.
* The organization cannot accidentally lose all administrators.
* Role changes are audited.
* Permission changes take effect immediately.

---

## STORY-022: Remove an organization member

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-019

### User Story

As an organization administrator,
I want to remove a member,
so that former team members cannot access company data.

### Acceptance Criteria

* Only authorized users can remove members.
* A user cannot remove members from another organization.
* The organization cannot accidentally lose all administrators.
* Removed users lose access immediately.
* Historical records retain appropriate user references.
* Removal is recorded in the audit history.

---

# 8. EPIC-004: RFP Project Management

## Epic Goal

Allow authorized organization members to create and view RFP projects.

---

## STORY-023: Create the RFP project data model

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-014

### User Story

As a developer,
I want an RFP project model,
so that uploaded documents and extracted requirements can be grouped together.

### Acceptance Criteria

* Every project belongs to one organization.
* Projects have unique identifiers.
* Projects include a title.
* Projects may include an issuing organization.
* Projects may include a description.
* Projects may include a submission deadline.
* Projects have a status.
* Projects include creation and update timestamps.
* A database migration creates the table.

### Suggested Fields

* `id`
* `organization_id`
* `title`
* `issuing_organization`
* `description`
* `submission_deadline`
* `status`
* `created_by_user_id`
* `created_at`
* `updated_at`

### Initial Statuses

* `draft`
* `active`
* `archived`

---

## STORY-024: Create an RFP project

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-023, STORY-018

### User Story

As a proposal manager,
I want to create an RFP project,
so that I can organize an upcoming proposal.

### Acceptance Criteria

* The user must be authenticated.
* The user must belong to the active organization.
* The user must have permission to create projects.
* A project title is required.
* The issuing organization is optional.
* The description is optional.
* The submission deadline is optional.
* The project belongs to the active organization.
* The project creator is recorded.
* The created project appears in the organization project list.
* Project creation is audited.

---

## STORY-025: List organization projects

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-024

### User Story

As an organization member,
I want to view my organization’s RFP projects,
so that I can access current proposal work.

### Acceptance Criteria

* Users see only projects belonging to the active organization.
* Archived projects can be filtered separately.
* Each project displays its title, status, issuing organization, and deadline.
* Empty-state messaging is displayed when no projects exist.
* The list supports pagination or has a documented pagination plan.
* Users outside the organization cannot retrieve the list.

---

## STORY-026: View an RFP project

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-024

### User Story

As an organization member,
I want to open an RFP project,
so that I can view its documents and extracted requirements.

### Acceptance Criteria

* The project page displays project metadata.
* The page contains navigation for documents and requirements.
* Users outside the project organization receive an authorization error.
* A nonexistent project returns a not-found response.
* The frontend handles loading, authorization, and not-found states.

---

## STORY-027: Edit project details

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-026

### User Story

As a proposal manager,
I want to update project details,
so that changes to the RFP can be reflected.

### Acceptance Criteria

* Authorized users can edit project fields.
* Unauthorized members cannot edit projects.
* The project organization cannot be changed through this operation.
* Updated timestamps are recorded.
* Changes are audited.
* Validation errors are displayed clearly.

---

## STORY-028: Archive an RFP project

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-026

### User Story

As a proposal manager,
I want to archive an inactive project,
so that completed or abandoned proposals do not clutter active work.

### Acceptance Criteria

* Authorized users can archive a project.
* Archived projects remain stored.
* Archived projects are excluded from the default active-project list.
* Archived projects remain inaccessible to users outside the organization.
* The action is audited.

---

# 9. EPIC-005: Document Upload and Storage

## Epic Goal

Allow authorized users to upload RFP PDFs and store them securely.

---

## STORY-029: Create the document data model

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-023

### User Story

As a developer,
I want a document model,
so that uploaded RFP files and their processing state can be tracked.

### Acceptance Criteria

* Every RFP document belongs to one project.
* Every document is traceable to an organization through its project.
* Document metadata includes the original filename.
* Document metadata includes a storage key.
* Document metadata includes MIME type and size.
* Document metadata includes processing status.
* Document metadata includes who uploaded it.
* Document metadata includes timestamps.
* A database migration creates the table.

### Suggested Fields

* `id`
* `organization_id`
* `project_id`
* `original_filename`
* `storage_key`
* `mime_type`
* `file_size_bytes`
* `checksum`
* `processing_status`
* `processing_error`
* `uploaded_by_user_id`
* `created_at`
* `updated_at`

---

## STORY-030: Configure private object storage

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-006

### User Story

As a developer,
I want private file storage,
so that uploaded RFP documents are not publicly exposed.

### Acceptance Criteria

* Development storage uses a documented local or S3-compatible solution.
* Production-compatible storage keys are generated.
* Files are private by default.
* Direct permanent public URLs are not used.
* Storage credentials are loaded from environment variables.
* File retrieval requires authorization through the backend or short-lived signed URLs.
* Storage behavior is documented.

### Technical Tasks

* Configure MinIO, local storage, or another development solution.
* Create a storage-service abstraction.
* Implement upload, download, and delete operations.
* Add integration tests where practical.

---

## STORY-031: Upload a PDF to an RFP project

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-029, STORY-030

### User Story

As a proposal manager,
I want to upload an RFP PDF,
so that RFPFlow can process and extract its requirements.

### Acceptance Criteria

* The user must be authenticated.
* The user must belong to the project organization.
* The user must have document-upload permission.
* Only supported PDF files are accepted.
* The file size must be within the configured limit.
* The original filename is preserved safely.
* A unique storage key is generated.
* The file is stored privately.
* A document database record is created.
* The initial processing status is stored.
* A background job is queued after upload.
* The HTTP request does not wait for complete document processing.
* Upload failures do not create inconsistent database records.
* Upload actions are audited.

---

## STORY-032: List project documents

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-031

### User Story

As a project member,
I want to view uploaded RFP documents,
so that I can understand which files are being processed.

### Acceptance Criteria

* Users see documents belonging only to the selected project.
* Each document displays its filename, size, upload date, and processing status.
* Failed documents display an understandable failure state.
* Users outside the organization cannot list project documents.
* The frontend updates processing statuses when they change.

---

## STORY-033: Download or view an uploaded document

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-031

### User Story

As a project member,
I want to open the original RFP document,
so that I can verify extracted information against the source.

### Acceptance Criteria

* The user must be authorized for the project.
* The file remains private.
* The backend returns the file or a short-lived authorized URL.
* Users outside the organization cannot retrieve the file.
* Missing files return a controlled error.
* The original PDF can be displayed in the browser where supported.

---

## STORY-034: Delete a project document

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-031

### User Story

As a proposal manager,
I want to remove an incorrectly uploaded document,
so that it is not used for extraction.

### Acceptance Criteria

* Only authorized users can delete documents.
* The file is removed or scheduled for removal from storage.
* Related processing jobs are cancelled or ignored where possible.
* Related extracted records are handled according to a documented policy.
* Users outside the organization cannot delete documents.
* The action is audited.

---

## STORY-035: Validate uploaded files

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-031

### User Story

As an organization,
I want uploaded files to be validated,
so that unsupported or dangerous files are rejected.

### Acceptance Criteria

* File extensions are not trusted by themselves.
* MIME type is checked.
* File signatures are checked where practical.
* Maximum file size is enforced.
* Empty files are rejected.
* Filenames are sanitized.
* Executable content is rejected.
* Validation errors are understandable.
* A future malware-scanning integration point is documented.

---

# 10. EPIC-006: Background Document Processing

## Epic Goal

Process uploaded documents asynchronously and expose reliable processing states.

---

## STORY-036: Define document-processing states

**Priority:** P0
**Story Points:** 2
**Dependencies:** STORY-029

### User Story

As a project member,
I want clear processing states,
so that I know whether a document is ready, still processing, or failed.

### Acceptance Criteria

* Supported statuses are defined in one shared location.
* Status transitions are documented.
* Invalid transitions are prevented where practical.
* The frontend displays each supported status.
* Status names are consistent across the database, API, and frontend.

### Initial Statuses

* `uploaded`
* `queued`
* `extracting_text`
* `extracting_requirements`
* `completed`
* `failed`

---

## STORY-037: Queue document processing after upload

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-005, STORY-031, STORY-036

### User Story

As a proposal manager,
I want uploaded documents processed automatically,
so that I do not have to start extraction manually.

### Acceptance Criteria

* A background job is queued after a successful upload.
* The job receives a document identifier rather than raw file contents.
* The document status changes to `queued`.
* Duplicate job creation is reduced through idempotency controls.
* Queue failures are detected and recorded.
* The upload endpoint still returns a clear result if queueing fails.

---

## STORY-038: Process a document in the background

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-037

### User Story

As a project member,
I want processing to happen outside the API request,
so that the application remains responsive for large documents.

### Acceptance Criteria

* The worker loads the document securely.
* The worker verifies the document still exists and remains eligible for processing.
* The worker updates processing states.
* The worker invokes text extraction.
* The worker invokes requirement extraction after text extraction succeeds.
* A successful job marks the document as completed.
* An unsuccessful job marks the document as failed.
* Failure information is stored without leaking sensitive data.
* Repeated execution does not create uncontrolled duplicate records.

---

## STORY-039: Retry failed document processing

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-038

### User Story

As a proposal manager,
I want to retry a failed document,
so that temporary errors do not require another upload.

### Acceptance Criteria

* Authorized users can retry a failed document.
* Only documents in valid states can be retried.
* The retry creates or schedules a new processing attempt.
* Previous failure details remain available for debugging.
* Duplicate active processing attempts are prevented.
* Retry actions are audited.

---

## STORY-040: Track processing attempts

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-038

### User Story

As a developer,
I want processing attempts recorded,
so that failures and retries can be diagnosed.

### Acceptance Criteria

* Each attempt has a unique identifier.
* Start and finish times are recorded.
* Attempt status is recorded.
* Failure categories are recorded.
* Retry relationships are recorded where practical.
* Sensitive document content is not copied into logs unnecessarily.

---

# 11. EPIC-007: PDF Text Extraction

## Epic Goal

Extract text from text-based PDF documents while preserving page-level source references.

---

## STORY-041: Create the document-page data model

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-029

### User Story

As a developer,
I want page-level document records,
so that extracted requirements can reference their original pages.

### Acceptance Criteria

* Each page belongs to one document.
* Page numbers are preserved.
* Extracted page text is stored.
* Page records are ordered.
* Duplicate pages for the same extraction version are prevented.
* A database migration creates the table.

### Suggested Fields

* `id`
* `organization_id`
* `document_id`
* `page_number`
* `text`
* `character_count`
* `created_at`

---

## STORY-042: Extract text from a text-based PDF

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-041

### User Story

As a proposal manager,
I want text extracted from an uploaded PDF,
so that RFPFlow can analyze its requirements.

### Acceptance Criteria

* Text is extracted from supported text-based PDFs.
* Text is separated by page.
* Original page numbering is preserved.
* Extraction handles common PDF encoding cases.
* Empty or unreadable pages are recorded appropriately.
* Extraction failures produce a controlled error.
* Extracted text is associated with the correct organization and document.
* Automated tests include a representative test PDF.

---

## STORY-043: Detect PDFs that likely require OCR

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-042

### User Story

As a proposal manager,
I want to know when a PDF cannot be read as text,
so that I understand why extraction is incomplete.

### Acceptance Criteria

* The system detects documents with little or no extractable text.
* The document is flagged as likely scanned or unsupported.
* The user receives an understandable message.
* The document is not falsely presented as successfully extracted.
* Manual requirement creation remains possible in a later milestone.
* OCR is not required for the first milestone.

---

## STORY-044: Normalize extracted PDF text

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-042

### User Story

As a developer,
I want extracted text cleaned consistently,
so that AI processing receives usable content.

### Acceptance Criteria

* Repeated whitespace is normalized.
* Page boundaries are retained.
* Common line-break artifacts are reduced.
* Content is not reordered in a way that changes meaning.
* Original raw extraction can be retained if needed for debugging.
* Normalization behavior is tested.

---

## STORY-045: Divide document text into extraction chunks

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-044

### User Story

As a developer,
I want long documents divided into traceable chunks,
so that AI extraction can process them within model limits.

### Acceptance Criteria

* Chunks remain associated with the source document.
* Chunks retain page-range information.
* Chunk sizes respect configured limits.
* Reasonable overlap is supported where needed.
* Headings and paragraph boundaries are preserved where practical.
* No page text is silently omitted.
* Chunk creation is deterministic for unchanged input.
* Chunking is covered by automated tests.

### Suggested Fields

* `id`
* `organization_id`
* `document_id`
* `start_page`
* `end_page`
* `text`
* `chunk_index`
* `token_estimate`
* `created_at`

---

## STORY-046: View extracted document text

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-042

### User Story

As a project member,
I want to inspect extracted text,
so that I can understand what RFPFlow processed.

### Acceptance Criteria

* Authorized users can view extracted pages.
* Page numbers are displayed.
* Empty pages are indicated.
* Users outside the organization cannot view extracted text.
* The interface handles large documents through pagination or virtualization.

---

# 12. EPIC-008: AI Requirement Extraction

## Epic Goal

Use AI to identify candidate RFP requirements while preserving sources and validating structured output.

---

## STORY-047: Define the requirement data model

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-023, STORY-029

### User Story

As a developer,
I want a structured requirement model,
so that AI-extracted RFP items can be reviewed consistently.

### Acceptance Criteria

* Every requirement belongs to one project and organization.
* A requirement may reference one source document.
* A requirement contains a title and description.
* A requirement has a type and category.
* A requirement records whether it appears mandatory.
* A requirement records a review status.
* A requirement may include an AI confidence value.
* A requirement includes source-location data.
* A requirement records whether it was created by AI or manually.
* A database migration creates the table.

### Suggested Fields

* `id`
* `organization_id`
* `project_id`
* `source_document_id`
* `title`
* `description`
* `requirement_type`
* `category`
* `is_mandatory`
* `source_text`
* `source_start_page`
* `source_end_page`
* `confidence_score`
* `review_status`
* `origin`
* `created_by_user_id`
* `created_at`
* `updated_at`

### Initial Requirement Types

* `question`
* `mandatory_requirement`
* `eligibility_requirement`
* `required_attachment`
* `deadline`
* `submission_instruction`
* `evaluation_criterion`
* `contractual_requirement`
* `informational`

### Initial Categories

* `company_information`
* `technical`
* `security`
* `privacy`
* `legal`
* `financial`
* `experience`
* `team`
* `project_management`
* `support`
* `submission`
* `other`

### Initial Review Statuses

* `pending_review`
* `approved`
* `rejected`
* `needs_clarification`

---

## STORY-048: Define the AI extraction schema

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-047

### User Story

As a developer,
I want a strict AI response schema,
so that model output can be validated before entering the database.

### Acceptance Criteria

* The extraction schema is represented using Pydantic.
* Required fields are explicitly defined.
* Enumerated fields reject unknown values or map them safely.
* Confidence values are bounded.
* Source-page values are validated.
* Empty titles and descriptions are rejected.
* Invalid output is not directly stored.
* Schema examples are documented.

### Example Structured Output

```json
{
  "requirements": [
    {
      "title": "Describe privacy safeguards",
      "description": "The proponent must explain how personal information will be protected.",
      "requirement_type": "question",
      "category": "privacy",
      "is_mandatory": true,
      "source_text": "The Proponent must describe the safeguards...",
      "source_start_page": 24,
      "source_end_page": 24,
      "confidence_score": 0.94
    }
  ]
}
```

---

## STORY-049: Create the requirement-extraction prompt

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-048

### User Story

As a developer,
I want a versioned extraction prompt,
so that requirement extraction is consistent and testable.

### Acceptance Criteria

* The prompt defines which item types should be extracted.
* The prompt requires source text and page references.
* The prompt distinguishes requirements from general background information.
* The prompt instructs the model not to invent requirements.
* The prompt requests structured output.
* Prompt versions are identifiable.
* The prompt is stored outside route-handler code.
* Prompt behavior is documented.

---

## STORY-050: Extract candidate requirements from a document chunk

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-045, STORY-048, STORY-049

### User Story

As a proposal manager,
I want candidate requirements extracted from RFP text,
so that I do not have to locate every requirement manually.

### Acceptance Criteria

* The worker sends a document chunk to the configured AI provider.
* The request includes source-page metadata.
* The AI response is parsed into the extraction schema.
* Valid candidate requirements are produced.
* Invalid output causes a controlled failure or repair attempt.
* Model errors are handled.
* Provider timeouts are handled.
* AI requests do not expose data from other organizations.
* The model configuration is recorded for evaluation.

---

## STORY-051: Store validated candidate requirements

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-050

### User Story

As a proposal manager,
I want extracted requirements saved to the project,
so that I can review them after processing completes.

### Acceptance Criteria

* Only validated requirements are stored.
* Stored requirements belong to the correct organization and project.
* Source document and page ranges are recorded.
* Requirements begin in `pending_review` status.
* AI-created requirements are marked with an AI origin.
* Duplicate worker execution does not create uncontrolled duplicate records.
* Requirement creation is transactional where practical.

---

## STORY-052: Extract requirements from all document chunks

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-050, STORY-051

### User Story

As a proposal manager,
I want the complete RFP processed,
so that candidate requirements are extracted from every relevant section.

### Acceptance Criteria

* All eligible chunks are processed.
* Progress can be determined from processed chunk counts.
* Failed chunk extraction is recorded.
* Temporary failures may be retried.
* The document is not marked complete until required chunk processing finishes.
* Partial completion is distinguished from full success.
* Reprocessing does not blindly duplicate previous results.

---

## STORY-053: Detect likely duplicate requirements

**Priority:** P1
**Story Points:** 8
**Dependencies:** STORY-052

### User Story

As a proposal manager,
I want likely duplicates identified,
so that repeated requirements can be reviewed efficiently.

### Acceptance Criteria

* The system identifies exact or highly similar candidate requirements.
* Potential duplicates are flagged rather than automatically deleted.
* Source references from each occurrence are preserved.
* Users can still review each candidate.
* The duplicate-detection method is documented.
* False-positive risks are acknowledged.

---

## STORY-054: Record AI extraction metadata

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-050

### User Story

As a developer,
I want AI operation metadata recorded,
so that extraction quality and failures can be evaluated.

### Acceptance Criteria

* The AI provider is recorded.
* The model identifier is recorded.
* The prompt version is recorded.
* The operation type is recorded.
* Start and finish times are recorded.
* Token or usage information is recorded when available.
* The related document and project are recorded.
* Sensitive content is stored only according to a documented policy.

---

## STORY-055: Create an extraction evaluation dataset

**Priority:** P1
**Story Points:** 8
**Dependencies:** STORY-048, STORY-050

### User Story

As a developer,
I want a representative evaluation dataset,
so that extraction changes can be measured rather than judged only manually.

### Acceptance Criteria

* The dataset includes multiple sample RFP sections.
* Expected requirements are documented.
* Expected source pages are documented.
* Mandatory and optional items are represented.
* Multiple requirement categories are represented.
* The dataset excludes confidential material.
* An evaluation script can compare extracted and expected results.
* Known limitations are documented.

---

# 13. EPIC-009: Requirement Review

## Epic Goal

Allow proposal managers to verify, correct, approve, and reject AI-extracted requirements.

---

## STORY-056: List candidate requirements

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-051

### User Story

As a proposal manager,
I want to view extracted candidate requirements,
so that I can verify the AI output.

### Acceptance Criteria

* The project displays requirements belonging only to that project.
* Each item displays title, type, category, mandatory status, confidence, and review status.
* Each item displays its source document and page.
* Requirements can be filtered by review status.
* Requirements can be filtered by category.
* Requirements can be sorted.
* Large result sets support pagination.
* Users outside the organization cannot view requirements.

---

## STORY-057: View requirement details and source evidence

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-056, STORY-033

### User Story

As a proposal manager,
I want to inspect a candidate requirement and its source,
so that I can determine whether the extraction is accurate.

### Acceptance Criteria

* The requirement detail view displays all structured fields.
* The original source text is displayed.
* The source document name is displayed.
* The source page range is displayed.
* The user can open the related PDF.
* The relevant page can be opened or identified.
* Users outside the organization cannot view requirement details.

---

## STORY-058: Approve a candidate requirement

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-057

### User Story

As a proposal manager,
I want to approve a correct requirement,
so that it becomes part of the project’s confirmed requirement list.

### Acceptance Criteria

* Only authorized users can approve requirements.
* The requirement must belong to the active organization.
* Approval changes the review status to `approved`.
* The approving user is recorded.
* The approval timestamp is recorded.
* Approval is audited.
* Repeated approval requests are handled safely.

---

## STORY-059: Reject a candidate requirement

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-057

### User Story

As a proposal manager,
I want to reject an incorrect extraction,
so that it does not appear as a confirmed project requirement.

### Acceptance Criteria

* Only authorized users can reject requirements.
* Rejection changes the review status to `rejected`.
* An optional rejection reason can be recorded.
* The rejecting user and time are recorded.
* Rejected requirements remain available in review history.
* Rejection is audited.

---

## STORY-060: Edit a candidate requirement

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-057

### User Story

As a proposal manager,
I want to correct an extracted requirement,
so that inaccurate AI output can be fixed before approval.

### Acceptance Criteria

* Authorized users can edit supported requirement fields.
* Source evidence remains visible.
* The original AI-generated values remain traceable through history or audit data.
* Validation rules are enforced.
* Editing does not allow the requirement to move to another organization.
* The editor and timestamp are recorded.
* Changes are audited.

---

## STORY-061: Mark a requirement as needing clarification

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-057

### User Story

As a proposal manager,
I want to mark an unclear requirement,
so that it can be revisited before final approval.

### Acceptance Criteria

* Authorized users can set `needs_clarification`.
* A clarification note can be stored.
* The requirement appears in the corresponding filter.
* The action is audited.

---

## STORY-062: Manually add a requirement

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-047

### User Story

As a proposal manager,
I want to add a requirement that the AI missed,
so that the project does not depend on perfect extraction.

### Acceptance Criteria

* Authorized users can create a requirement manually.
* Title and description are required.
* Type, category, and mandatory status can be selected.
* Source document and page may be provided.
* The requirement origin is marked as manual.
* The new requirement can begin as approved or pending review according to permission rules.
* Creation is audited.

---

## STORY-063: Bulk approve selected requirements

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-058

### User Story

As a proposal manager,
I want to approve several clearly correct requirements at once,
so that document review is more efficient.

### Acceptance Criteria

* Users can select multiple pending requirements.
* Only authorized users can perform bulk approval.
* Every selected requirement is checked for tenant access.
* Partial failures are reported clearly.
* Approval metadata is recorded for each requirement.
* The operation is audited.

---

## STORY-064: Merge duplicate requirements

**Priority:** P1
**Story Points:** 8
**Dependencies:** STORY-053, STORY-057

### User Story

As a proposal manager,
I want to merge duplicate requirements,
so that repeated RFP language does not create unnecessary work.

### Acceptance Criteria

* Users can select requirements from the same project.
* One requirement becomes the retained record.
* Source references from merged requirements are preserved.
* Merged records remain traceable.
* Requirements from different organizations cannot be merged.
* The merge operation is audited.
* The interface clearly warns that merging changes project structure.

---

## STORY-065: Display low-confidence extractions

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-056

### User Story

As a proposal manager,
I want low-confidence requirements highlighted,
so that I can prioritize uncertain AI output.

### Acceptance Criteria

* A configurable low-confidence threshold exists.
* Low-confidence items can be filtered.
* Confidence is not presented as proof of correctness.
* Status is not communicated through colour alone.
* Items without confidence values are handled.

---

## STORY-066: Show extraction-review progress

**Priority:** P1
**Story Points:** 3
**Dependencies:** STORY-056, STORY-058, STORY-059

### User Story

As a proposal manager,
I want to see review progress,
so that I know how much AI output remains unverified.

### Acceptance Criteria

* The project displays counts for pending, approved, rejected, and clarification items.
* A review-completion percentage is displayed.
* The calculation is documented.
* Counts are scoped to the current organization and project.
* The frontend updates after review actions.

---

# 14. EPIC-010: Tenant Isolation and Security

## Epic Goal

Ensure that users can access only data belonging to organizations of which they are authorized members.

---

## STORY-067: Create reusable organization-membership authorization

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-015, STORY-009

### User Story

As a developer,
I want reusable authorization dependencies,
so that organization access is enforced consistently across endpoints.

### Acceptance Criteria

* Backend authorization verifies authentication.
* Backend authorization verifies organization membership.
* Role requirements can be specified.
* Authorization failures return consistent errors.
* Frontend state is never treated as proof of authorization.
* Authorization logic is tested independently.

---

## STORY-068: Scope all project queries by organization

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-067, STORY-023

### User Story

As an organization,
I want project queries tenant-scoped,
so that another company cannot access my RFP projects.

### Acceptance Criteria

* Project-list queries require organization membership.
* Project-detail queries validate organization ownership.
* Project updates validate organization ownership.
* Project identifiers alone are insufficient for access.
* Cross-tenant access attempts are covered by integration tests.

---

## STORY-069: Scope all document queries by organization

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-067, STORY-029

### User Story

As an organization,
I want document operations tenant-scoped,
so that private RFP files cannot be exposed to another company.

### Acceptance Criteria

* Upload operations validate project membership.
* Document-list operations validate membership.
* File downloads validate membership.
* Delete and retry operations validate permissions.
* Storage keys are not accepted as proof of authorization.
* Cross-tenant document tests are included.

---

## STORY-070: Scope all requirement queries by organization

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-067, STORY-047

### User Story

As an organization,
I want requirement operations tenant-scoped,
so that extracted RFP information remains private.

### Acceptance Criteria

* Requirement listing validates project membership.
* Requirement detail access validates organization ownership.
* Review actions validate role and organization.
* Manual creation validates organization and project.
* Cross-tenant requirement tests are included.

---

## STORY-071: Add role-based permission checks

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-067

### User Story

As an organization administrator,
I want actions limited by role,
so that contributors cannot perform administrative operations.

### Acceptance Criteria

* Role permissions are defined centrally.
* Project creation requires an allowed role.
* Document upload requires an allowed role.
* Requirement approval requires an allowed role.
* Membership administration requires an organization administrator.
* Permission-denied responses are consistent.
* Role combinations are covered by automated tests.

### Initial Permission Matrix

| Action               | Admin | Proposal Manager | Contributor | Reviewer |
| -------------------- | ----: | ---------------: | ----------: | -------: |
| View organization    |   Yes |              Yes |         Yes |      Yes |
| View projects        |   Yes |              Yes |         Yes |      Yes |
| Create project       |   Yes |              Yes |          No |       No |
| Upload RFP documents |   Yes |              Yes |          No |       No |
| View requirements    |   Yes |              Yes |         Yes |      Yes |
| Review extraction    |   Yes |              Yes |          No |      Yes |
| Manage members       |   Yes |               No |          No |       No |

The exact matrix may evolve, but changes must be documented.

---

## STORY-072: Protect sensitive file and document metadata

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-069

### User Story

As an organization,
I want storage details hidden,
so that internal file paths and credentials are not exposed.

### Acceptance Criteria

* Raw storage credentials are never returned.
* Internal storage keys are excluded unless needed.
* Signed URLs expire quickly.
* Error messages do not expose internal paths.
* Logs avoid unnecessary sensitive filenames or contents.

---

## STORY-073: Add security headers and basic API protections

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-002, STORY-003

### User Story

As a user,
I want the application to use basic web-security controls,
so that common attacks are reduced.

### Acceptance Criteria

* Security headers are configured.
* CORS is restricted appropriately.
* Cookie settings are secure when cookies are used.
* CSRF protection is implemented when required by the authentication design.
* Request-body size limits are configured.
* Rate limiting is added to sensitive authentication endpoints or documented for production.
* Security decisions are documented.

---

# 15. EPIC-011: Auditability and Observability

## Epic Goal

Record important user and AI actions and provide enough operational information to diagnose failures.

---

## STORY-074: Create the audit-event data model

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-004

### User Story

As an organization,
I want important actions recorded,
so that changes can be traced to users and AI operations.

### Acceptance Criteria

* Audit events have unique identifiers.
* Events may include an organization, project, user, and target entity.
* Event type is recorded.
* Event time is recorded.
* Safe metadata can be recorded.
* Audit events are append-only through normal application behavior.
* A database migration creates the table.

### Example Event Types

* `organization.created`
* `organization.member_invited`
* `organization.member_removed`
* `project.created`
* `project.updated`
* `document.uploaded`
* `document.processing_retried`
* `requirement.created_by_ai`
* `requirement.created_manually`
* `requirement.updated`
* `requirement.approved`
* `requirement.rejected`
* `ai.extraction_started`
* `ai.extraction_failed`
* `ai.extraction_completed`

---

## STORY-075: Record milestone audit events

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-074

### User Story

As a proposal manager,
I want important project actions recorded,
so that I can understand how project data changed.

### Acceptance Criteria

* Organization creation is audited.
* Project creation and modification are audited.
* Document uploads and deletions are audited.
* Requirement review actions are audited.
* AI extraction operations are auditable.
* Failed audit recording does not silently create inconsistent business actions.
* Sensitive document contents are not copied into audit metadata.

---

## STORY-076: Add structured application logging

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-003, STORY-005

### User Story

As a developer,
I want structured logs,
so that API and worker failures can be diagnosed.

### Acceptance Criteria

* Logs include timestamps and severity levels.
* API requests include request or correlation identifiers.
* Worker jobs include job and document identifiers.
* Exceptions include stack traces in development.
* Sensitive credentials and tokens are not logged.
* Logging configuration differs appropriately between development and production.

---

## STORY-077: Add processing metrics

**Priority:** P2
**Story Points:** 5
**Dependencies:** STORY-038, STORY-050

### User Story

As a developer,
I want basic processing metrics,
so that slow or unreliable extraction can be identified.

### Acceptance Criteria

* Document-processing duration is measurable.
* Text-extraction duration is measurable.
* AI-extraction duration is measurable.
* Failure counts can be determined.
* Metrics do not expose document contents.
* A future monitoring integration is documented.

---

# 16. EPIC-012: Testing and Continuous Integration

## Epic Goal

Create automated checks that protect core behavior, tenant isolation, and the first milestone workflow.

---

## STORY-078: Configure backend unit testing

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-003

### User Story

As a developer,
I want backend unit testing configured,
so that business logic can be verified automatically.

### Acceptance Criteria

* A Python test framework is configured.
* Tests run through one documented command.
* Test settings are separate from development settings.
* Example unit tests are included.
* Test coverage can be generated.
* The test command returns a nonzero exit code on failure.

---

## STORY-079: Configure API integration testing

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-004, STORY-078

### User Story

As a developer,
I want API integration tests,
so that endpoints, authentication, and database behavior can be tested together.

### Acceptance Criteria

* Tests use an isolated test database.
* Database state is cleaned between tests.
* Authentication helpers are available.
* Organization and project fixtures are available.
* API responses and database results can be asserted.
* Integration tests run in CI.

---

## STORY-080: Configure frontend testing

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-002

### User Story

As a developer,
I want frontend tests configured,
so that important components and user interactions can be verified.

### Acceptance Criteria

* A frontend test framework is configured.
* Components can be rendered in tests.
* User interactions can be simulated.
* API requests can be mocked.
* Example tests are included.
* Frontend tests run in CI.

---

## STORY-081: Configure end-to-end testing

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-006, STORY-009

### User Story

As a developer,
I want end-to-end tests,
so that the complete user workflow can be verified in a browser.

### Acceptance Criteria

* Playwright or an equivalent framework is configured.
* Tests can start or connect to the complete application.
* Authentication helpers are available.
* Screenshots or traces are retained on failure.
* End-to-end tests can run locally.
* A CI strategy is documented.

---

## STORY-082: Add authentication tests

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-008, STORY-009, STORY-079

### User Story

As a developer,
I want authentication tests,
so that account access remains secure.

### Acceptance Criteria

Tests cover:

* successful registration
* duplicate registration
* successful login
* invalid login
* disabled-user login
* current-user retrieval
* protected-route rejection
* logout behavior

---

## STORY-083: Add tenant-isolation integration tests

**Priority:** P0
**Story Points:** 8
**Dependencies:** STORY-068, STORY-069, STORY-070, STORY-079

### User Story

As an organization,
I want tenant isolation automatically tested,
so that future changes do not expose company data.

### Acceptance Criteria

Tests verify that a user from Organization A cannot:

* view Organization B
* view Organization B’s members
* list Organization B’s projects
* open Organization B’s project
* upload to Organization B’s project
* list Organization B’s documents
* download Organization B’s files
* view Organization B’s extracted text
* view Organization B’s requirements
* approve or edit Organization B’s requirements

---

## STORY-084: Add document-upload tests

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-031, STORY-035, STORY-079

### User Story

As a developer,
I want upload behavior tested,
so that invalid and unauthorized files are handled safely.

### Acceptance Criteria

Tests cover:

* successful PDF upload
* invalid file type
* oversized file
* empty file
* unauthorized upload
* cross-tenant upload
* storage failure
* database failure
* queueing failure

---

## STORY-085: Add PDF extraction tests

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-042, STORY-044

### User Story

As a developer,
I want PDF extraction tested,
so that page references and text remain reliable.

### Acceptance Criteria

Tests verify:

* page count preservation
* page-number preservation
* extracted text presence
* empty-page handling
* invalid PDF handling
* normalization behavior
* likely scanned-PDF detection

---

## STORY-086: Add AI schema-validation tests

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-048

### User Story

As a developer,
I want invalid AI output tested,
so that malformed data cannot enter the database.

### Acceptance Criteria

Tests cover:

* valid structured output
* missing required fields
* invalid categories
* out-of-range confidence values
* invalid page ranges
* empty titles
* empty descriptions
* malformed JSON
* unexpected additional content

---

## STORY-087: Add requirement-review tests

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-058, STORY-059, STORY-060, STORY-062

### User Story

As a developer,
I want requirement-review behavior tested,
so that human corrections and approvals remain reliable.

### Acceptance Criteria

Tests cover:

* approving a pending requirement
* rejecting a pending requirement
* editing a requirement
* manually creating a requirement
* invalid status transitions
* unauthorized review actions
* cross-tenant review actions
* audit-event creation

---

## STORY-088: Create the first-milestone end-to-end test

**Priority:** P1
**Story Points:** 8
**Dependencies:** STORY-081 and all P0 milestone features

### User Story

As a developer,
I want the primary milestone workflow tested end to end,
so that the complete product path can be demonstrated reliably.

### Acceptance Criteria

The test completes the following workflow:

1. Register a user.
2. Log in.
3. Create an organization.
4. Create an RFP project.
5. Upload a test PDF.
6. Observe document processing.
7. Open extracted requirements.
8. View source text and page references.
9. Edit one requirement.
10. Approve one requirement.
11. Reject one requirement.
12. Manually add one requirement.

---

## STORY-089: Configure continuous integration

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-078, STORY-080

### User Story

As a developer,
I want automated CI checks,
so that broken changes are detected before merging.

### Acceptance Criteria

The CI pipeline runs:

* backend formatting checks
* backend linting
* backend type checking where configured
* backend unit and integration tests
* frontend formatting checks
* frontend linting
* frontend type checking
* frontend tests
* production builds
* migration validation

Additional criteria:

* CI runs on pull requests.
* CI fails when required checks fail.
* Secrets are not committed.
* CI configuration is documented.

---

# 17. EPIC-013: Documentation and Developer Experience

## Epic Goal

Document the product, architecture, setup, decisions, and limitations so that the project resembles professional software development.

---

## STORY-090: Create the main project README

**Priority:** P0
**Story Points:** 3
**Dependencies:** STORY-006

### User Story

As a developer or reviewer,
I want a clear README,
so that I can understand and run RFPFlow.

### Acceptance Criteria

The README includes:

* project summary
* problem statement
* milestone scope
* architecture overview
* technology stack
* repository structure
* local setup instructions
* required environment variables
* database migration commands
* worker commands
* test commands
* known limitations

---

## STORY-091: Document the system architecture

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-006

### User Story

As a developer,
I want the architecture documented,
so that component responsibilities and data flow remain clear.

### Acceptance Criteria

The document includes:

* frontend responsibilities
* backend responsibilities
* worker responsibilities
* database responsibilities
* object-storage responsibilities
* Redis and queue responsibilities
* AI-provider integration
* request and processing flows
* trust boundaries
* tenant-isolation approach
* an architecture diagram

### Suggested File

```text
docs/architecture.md
```

---

## STORY-092: Document the initial database design

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-047

### User Story

As a developer,
I want the database model documented,
so that entity relationships and tenant ownership are understandable.

### Acceptance Criteria

The document includes:

* users
* organizations
* organization memberships
* projects
* documents
* document pages
* document chunks
* requirements
* processing attempts
* AI operations
* audit events
* foreign-key relationships
* tenant-ownership relationships
* key indexes and constraints
* an entity-relationship diagram

### Suggested File

```text
docs/database-schema.md
```

---

## STORY-093: Document the AI extraction pipeline

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-049, STORY-052

### User Story

As a developer,
I want the AI pipeline documented,
so that extraction decisions, validation, and risks are transparent.

### Acceptance Criteria

The document includes:

* input document flow
* text extraction
* page preservation
* chunking
* model invocation
* prompt versioning
* structured-output validation
* requirement storage
* duplicate handling
* human review
* failure handling
* privacy considerations
* known limitations
* evaluation strategy

### Suggested File

```text
docs/ai-extraction-pipeline.md
```

---

## STORY-094: Create architecture decision records

**Priority:** P1
**Story Points:** 5
**Dependencies:** STORY-006

### User Story

As a developer,
I want important decisions recorded,
so that future changes can be understood in context.

### Initial ADRs

* ADR-001: Use a monorepo.
* ADR-002: Use Next.js for the frontend.
* ADR-003: Use FastAPI for the backend.
* ADR-004: Use PostgreSQL for transactional data.
* ADR-005: Use background workers for document processing.
* ADR-006: Store files in private object storage.
* ADR-007: Preserve page-level document sources.
* ADR-008: Require human review for AI-extracted requirements.
* ADR-009: Use application-enforced tenant isolation.
* ADR-010: Validate AI output using Pydantic schemas.

### Acceptance Criteria

* ADRs use a consistent format.
* Each ADR includes context, decision, consequences, and status.
* ADRs are stored under `docs/adr`.
* New major architectural choices require new ADRs.

---

## STORY-095: Document the security model

**Priority:** P0
**Story Points:** 5
**Dependencies:** STORY-071, STORY-072

### User Story

As a developer or reviewer,
I want the security model documented,
so that authentication, authorization, and tenant risks are clear.

### Acceptance Criteria

The document includes:

* authentication approach
* session or token handling
* password handling
* role-based permissions
* tenant-isolation strategy
* file-access controls
* upload validation
* environment-secret management
* logging restrictions
* common threats
* security-test coverage
* known limitations

### Suggested File

```text
docs/security.md
```

---

# 18. Suggested Sprint Plan

The exact sprint size may change, but the following order keeps development incremental.

---

## Sprint 0: Engineering Foundation

### Goal

Create a working development environment with frontend, backend, PostgreSQL, Redis, worker, tests, and CI foundations.

### Suggested Stories

* STORY-001: Create the monorepo structure
* STORY-002: Initialize the Next.js frontend
* STORY-003: Initialize the FastAPI backend
* STORY-004: Configure PostgreSQL and migrations
* STORY-005: Configure Redis and the worker foundation
* STORY-006: Create a unified local-development environment
* STORY-078: Configure backend unit testing
* STORY-080: Configure frontend testing
* STORY-089: Configure continuous integration
* STORY-090: Create the main project README

### Sprint Outcome

A developer can clone the repository, start the services, open the frontend, call the backend health endpoint, connect to PostgreSQL and Redis, and run automated checks.

---

## Sprint 1: Authentication and Organizations

### Goal

Allow a user to register, log in, create an organization, and enter its workspace.

### Suggested Stories

* STORY-007: Create the user data model
* STORY-008: Register a user account
* STORY-009: Log in to the application
* STORY-010: Log out of the application
* STORY-011: Retrieve the current user
* STORY-012: Protect frontend routes
* STORY-014: Create the organization data model
* STORY-015: Create the organization-membership model
* STORY-016: Create an organization
* STORY-017: List the current user’s organizations
* STORY-018: Select an active organization
* STORY-067: Create reusable organization-membership authorization
* STORY-082: Add authentication tests

### Sprint Outcome

A user can create an account, log in, create an organization, and access a protected organization workspace.

---

## Sprint 2: RFP Project Management

### Goal

Allow authorized users to create and view tenant-isolated RFP projects.

### Suggested Stories

* STORY-019: View organization members
* STORY-023: Create the RFP project data model
* STORY-024: Create an RFP project
* STORY-025: List organization projects
* STORY-026: View an RFP project
* STORY-068: Scope all project queries by organization
* STORY-071: Add role-based permission checks
* STORY-083: Begin tenant-isolation integration tests
* STORY-091: Document the system architecture

### Sprint Outcome

A proposal manager can create and open an RFP project, while unauthorized users cannot access it.

---

## Sprint 3: Document Upload and Storage

### Goal

Allow a proposal manager to upload and securely access a PDF.

### Suggested Stories

* STORY-029: Create the document data model
* STORY-030: Configure private object storage
* STORY-031: Upload a PDF to an RFP project
* STORY-032: List project documents
* STORY-033: Download or view an uploaded document
* STORY-035: Validate uploaded files
* STORY-036: Define document-processing states
* STORY-037: Queue document processing after upload
* STORY-069: Scope all document queries by organization
* STORY-072: Protect sensitive file and document metadata
* STORY-084: Add document-upload tests

### Sprint Outcome

An authorized user can upload a valid PDF, see its status, and securely reopen the original document.

---

## Sprint 4: PDF Processing

### Goal

Extract page-level text from uploaded PDFs through a background worker.

### Suggested Stories

* STORY-038: Process a document in the background
* STORY-041: Create the document-page data model
* STORY-042: Extract text from a text-based PDF
* STORY-043: Detect PDFs that likely require OCR
* STORY-044: Normalize extracted PDF text
* STORY-045: Divide document text into extraction chunks
* STORY-046: View extracted document text
* STORY-076: Add structured application logging
* STORY-085: Add PDF extraction tests

### Sprint Outcome

An uploaded text-based PDF is processed asynchronously, and its extracted text can be viewed by page.

---

## Sprint 5: AI Requirement Extraction

### Goal

Generate and store validated candidate requirements with page citations.

### Suggested Stories

* STORY-047: Define the requirement data model
* STORY-048: Define the AI extraction schema
* STORY-049: Create the requirement-extraction prompt
* STORY-050: Extract candidate requirements from a document chunk
* STORY-051: Store validated candidate requirements
* STORY-052: Extract requirements from all document chunks
* STORY-054: Record AI extraction metadata
* STORY-070: Scope all requirement queries by organization
* STORY-086: Add AI schema-validation tests
* STORY-093: Document the AI extraction pipeline

### Sprint Outcome

The worker can process a PDF and store AI-extracted candidate requirements with source document and page references.

---

## Sprint 6: Human Requirement Review

### Goal

Allow proposal managers to inspect, correct, approve, reject, and manually add requirements.

### Suggested Stories

* STORY-056: List candidate requirements
* STORY-057: View requirement details and source evidence
* STORY-058: Approve a candidate requirement
* STORY-059: Reject a candidate requirement
* STORY-060: Edit a candidate requirement
* STORY-062: Manually add a requirement
* STORY-066: Show extraction-review progress
* STORY-074: Create the audit-event data model
* STORY-075: Record milestone audit events
* STORY-087: Add requirement-review tests
* STORY-092: Document the initial database design
* STORY-095: Document the security model

### Sprint Outcome

A proposal manager can review the AI output against the original source, correct mistakes, approve valid requirements, reject invalid ones, and add missed requirements.

---

## Sprint 7: Milestone Hardening

### Goal

Improve reliability, complete tenant tests, finish the end-to-end workflow, and prepare a milestone demonstration.

### Suggested Stories

* STORY-039: Retry failed document processing
* STORY-040: Track processing attempts
* STORY-053: Detect likely duplicate requirements
* STORY-055: Create an extraction evaluation dataset
* STORY-061: Mark a requirement as needing clarification
* STORY-063: Bulk approve selected requirements
* STORY-064: Merge duplicate requirements
* STORY-065: Display low-confidence extractions
* STORY-073: Add security headers and basic API protections
* STORY-081: Configure end-to-end testing
* STORY-083: Complete tenant-isolation integration tests
* STORY-088: Create the first-milestone end-to-end test
* STORY-094: Create architecture decision records

### Sprint Outcome

The first milestone is demonstrable, tested, documented, and protected against core authorization failures.

---

# 19. Milestone 1 Acceptance Criteria

Milestone 1 is complete when all of the following are true:

## Authentication

* A user can register.
* A user can log in.
* A user can log out.
* Protected pages require authentication.

## Organizations

* A user can create an organization.
* The creator becomes an administrator.
* Organization memberships and roles are stored.
* Users can view only organizations they belong to.

## Projects

* An authorized user can create an RFP project.
* Projects belong to exactly one organization.
* Users cannot access projects from other organizations.

## Documents

* An authorized user can upload a text-based PDF.
* Invalid files are rejected.
* Files are stored privately.
* Uploaded documents display processing status.
* Authorized users can reopen the original PDF.
* Unauthorized users cannot access stored files.

## Background Processing

* Uploading a file queues a background job.
* Document processing does not block the upload request.
* Processing states are visible.
* Failures are recorded.

## PDF Extraction

* Text is extracted by page.
* Page numbers are preserved.
* Long text is divided into traceable chunks.
* PDFs with little extractable text are identified as likely requiring OCR.

## AI Extraction

* The AI receives document text with page metadata.
* The AI returns structured candidate requirements.
* AI output is validated before storage.
* Each candidate requirement includes source text and page references.
* Invalid model output does not enter the database as valid data.
* Candidate requirements begin in a pending-review state.

## Human Review

* A proposal manager can list extracted requirements.
* A proposal manager can inspect source evidence.
* A proposal manager can edit a requirement.
* A proposal manager can approve a requirement.
* A proposal manager can reject a requirement.
* A proposal manager can manually add a missed requirement.
* Review actions are recorded.

## Security

* Backend authorization is enforced.
* Tenant-isolation integration tests pass.
* Storage identifiers do not bypass authorization.
* Organization data is not exposed through predictable resource identifiers.
* Secrets are stored outside source control.

## Engineering Quality

* Automated tests pass.
* Linting passes.
* Type checking passes.
* Production builds pass.
* Database migrations apply successfully.
* CI runs on pull requests.
* Core architecture and security decisions are documented.
* The primary milestone workflow can be demonstrated end to end.

---

# 20. First Milestone Demonstration Script

The milestone demonstration should show the following workflow:

1. Open RFPFlow.
2. Register a new account.
3. Log in.
4. Create an organization named `Northstar Software Consulting`.
5. Create an RFP project named `City Services Portal RFP`.
6. Enter an issuing organization and submission deadline.
7. Upload a sample text-based RFP PDF.
8. Show that the upload request returns before processing completes.
9. Show the document status changing from queued to processing.
10. Open the original PDF.
11. Show extracted text separated by page.
12. Show AI-extracted candidate requirements.
13. Open one requirement.
14. Show its source text and page number.
15. Edit the requirement title or category.
16. Approve the corrected requirement.
17. Reject an incorrect requirement.
18. Manually add a requirement that was not extracted.
19. Show the extraction-review progress.
20. Log in as a user from another organization.
21. Demonstrate that the second user cannot access the first organization’s project, document, or requirements.

---

# 21. Initial Technical Risks

## Risk 1: The milestone is too large

The first milestone includes authentication, multi-tenancy, file storage, workers, PDF processing, AI extraction, and review workflows.

### Mitigation

* Follow the sprint order.
* Complete one vertical slice at a time.
* Keep nonessential features at P1 or P2.
* Do not begin response generation before requirement review works.

## Risk 2: AI extraction quality is inconsistent

The model may miss requirements or create duplicates.

### Mitigation

* Preserve source text and page references.
* Require human review.
* Allow manual requirement creation.
* Add an evaluation dataset.
* Avoid treating confidence scores as guaranteed accuracy.

## Risk 3: Cross-tenant data leakage

Incorrect queries may expose another organization’s files or requirements.

### Mitigation

* Enforce authorization in the backend.
* Scope all resource queries by organization.
* Add explicit cross-tenant integration tests.
* Never trust frontend organization identifiers alone.

## Risk 4: PDF extraction is unreliable

PDFs can contain unusual layouts, tables, images, and broken text encoding.

### Mitigation

* Limit the initial milestone to text-based PDFs.
* Preserve the original PDF.
* Flag documents with insufficient extractable text.
* Document OCR as a future feature.
* Allow manual correction.

## Risk 5: Background processing creates duplicate records

Retries or duplicate jobs may repeat extraction.

### Mitigation

* Design jobs to be idempotent.
* Track processing attempts.
* Use database constraints.
* Clear or version previous extraction results deliberately.
* Avoid relying only on the queue to prevent duplicates.

## Risk 6: AI-provider costs become difficult to control

Large PDFs may require many model calls.

### Mitigation

* Limit upload size and page count initially.
* Track model usage.
* Use deterministic chunks.
* Avoid reprocessing unchanged documents.
* Use smaller models where suitable.
* Add project-level or organization-level limits later.

---

# 22. Recommended First GitHub Issues

After completing the planning documents, create these issues first:

1. `Set up RFPFlow monorepo`
2. `Initialize Next.js web application`
3. `Initialize FastAPI API application`
4. `Configure PostgreSQL and Alembic`
5. `Configure Redis and background worker`
6. `Create local Docker Compose environment`
7. `Configure backend and frontend test frameworks`
8. `Create initial CI workflow`
9. `Write project README`
10. `Create system architecture document`

Do not create every backlog story as a GitHub issue immediately.

Create issues one sprint at a time so that:

* the active backlog remains manageable
* acceptance criteria can be refined before development
* architectural decisions can influence later stories
* unnecessary implementation details are not locked in too early

---

# 23. Recommended Starting Point

The first implementation story should be:

> STORY-001: Create the monorepo structure.

The first development sequence should be:

```text
Create GitHub repository
→ create repository structure
→ initialize Next.js
→ initialize FastAPI
→ configure PostgreSQL
→ configure Redis and worker
→ create Docker Compose
→ add test foundations
→ add CI
→ document local setup
```

Only after Sprint 0 is stable should authentication and organization features begin.
