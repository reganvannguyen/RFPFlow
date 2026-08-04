# RFPFlow MVP Roadmap

## 1. Purpose

This roadmap defines the planned development sequence for the complete RFPFlow Minimum Viable Product.

RFPFlow is an AI-assisted RFP and proposal-management platform for small software agencies and technology consultancies. It converts uploaded RFP documents into a structured proposal workspace where teams can identify requirements, assign work, draft grounded responses, review answers, check completeness, and export proposal materials.

The roadmap divides the MVP into six major milestones.

Each milestone delivers a usable part of the overall workflow and builds on the milestone before it.

---

## 2. Full MVP Workflow

The complete MVP should support the following user journey:

```text
Create an account and organization
→ create an RFP project
→ upload RFP documents
→ extract and review requirements
→ assign requirements to team members
→ upload company knowledge
→ retrieve relevant company evidence
→ create grounded proposal responses
→ review and approve responses
→ check proposal completeness
→ export proposal materials
```

The MVP is complete only when all six milestones are finished.

---

## 3. Roadmap Summary

| Milestone   | Name                                 | Primary Outcome                                   |
| ----------- | ------------------------------------ | ------------------------------------------------- |
| Milestone 1 | RFP Ingestion and Requirement Review | Understand what the RFP requires                  |
| Milestone 2 | Assignments and Workflow Tracking    | Organize who is responsible for each requirement  |
| Milestone 3 | Company Knowledge Base and Retrieval | Find trusted company information for responses    |
| Milestone 4 | Grounded Response Drafting           | Create evidence-supported proposal answers        |
| Milestone 5 | Response Review and Approval         | Review, revise, and approve proposal responses    |
| Milestone 6 | Compliance Checks and Export         | Determine readiness and export proposal materials |

---

# 4. Milestone 1: RFP Ingestion and Requirement Review

## 4.1 Goal

Allow a user to create an organization and RFP project, upload a text-based PDF, process it asynchronously, extract candidate requirements, and review the results against the original document.

This milestone answers:

> What does this RFP require the company to complete?

## 4.2 Included Features

### Project Foundation

* monorepo structure
* Next.js frontend
* FastAPI backend
* PostgreSQL database
* Redis
* background worker
* Docker Compose
* environment-variable management
* linting and formatting
* automated testing foundations
* continuous integration

### Authentication

* user registration
* login
* logout
* protected application routes
* current-user retrieval
* secure password storage
* password reset as a lower-priority feature

### Organizations and Roles

* organization creation
* organization memberships
* active organization selection
* role-based permissions
* tenant isolation
* organization member listing
* invitations as a lower-priority feature

### RFP Projects

* project creation
* project listing
* project detail view
* project metadata
* project status
* project archive support as a lower-priority feature

### Document Upload

* PDF uploads
* private file storage
* file validation
* project document listing
* authorized document viewing
* document-processing status

### Background Processing

* queued document processing
* worker-based extraction
* processing-state transitions
* failure handling
* retry support
* idempotency controls

### PDF Text Extraction

* text extraction from text-based PDFs
* page-number preservation
* page-level storage
* text normalization
* document chunking
* scanned-PDF detection
* extracted-text viewing

### AI Requirement Extraction

* structured extraction schema
* versioned extraction prompt
* AI extraction from document chunks
* source-page references
* source-text preservation
* confidence scores
* model metadata
* structured-output validation
* duplicate detection as a lower-priority feature

### Human Review

* candidate requirement list
* source evidence display
* requirement editing
* requirement approval
* requirement rejection
* manual requirement creation
* clarification status
* low-confidence filtering
* review-progress display
* duplicate merging as a lower-priority feature

### Security and Quality

* backend authorization
* organization-scoped queries
* cross-tenant access tests
* private document access
* audit events
* structured logging
* CI checks
* architecture documentation
* database documentation
* security documentation
* AI-pipeline documentation

## 4.3 Excluded Features

This milestone does not include:

* assigning requirements to team members
* internal deadlines
* team workload tracking
* company knowledge documents
* embeddings
* vector retrieval
* proposal response generation
* response editing
* response approval
* proposal completeness checking
* DOCX or XLSX export

## 4.4 Dependencies

This is the first milestone and has no product-feature dependencies.

The engineering foundation must be completed before the other milestone features can be implemented reliably.

## 4.5 Completion Criteria

Milestone 1 is complete when:

* a user can register and log in
* a user can create an organization
* an authorized user can create an RFP project
* an authorized user can upload a valid text-based PDF
* the upload creates a background-processing job
* PDF text is extracted and preserved by page
* the document is divided into traceable chunks
* AI-generated candidate requirements are validated
* each candidate includes source text and page references
* a proposal manager can edit, approve, reject, and manually add requirements
* users cannot access another organization’s data
* the primary workflow is covered by automated tests
* the milestone can be demonstrated end to end

## 4.6 Suggested Sprint Range

* Sprint 0: Engineering foundation
* Sprint 1: Authentication and organizations
* Sprint 2: RFP project management
* Sprint 3: Document upload and storage
* Sprint 4: PDF processing
* Sprint 5: AI requirement extraction
* Sprint 6: Human requirement review
* Sprint 7: Milestone hardening

## 4.7 Detailed Backlog

The detailed backlog for this milestone should be stored at:

```text
docs/backlogs/milestone-1-rfp-ingestion.md
```

---

# 5. Milestone 2: Assignments and Workflow Tracking

## 5.1 Goal

Allow proposal managers to assign approved requirements to organization members and track progress toward completion.

This milestone answers:

> Who is responsible for each requirement, and what is its current status?

## 5.2 Included Features

### Requirement Assignments

* assign a requirement to an organization member
* reassign a requirement
* remove an assignment
* identify unassigned requirements
* record who created or changed an assignment

### Internal Deadlines

* add an internal due date
* update an internal due date
* identify overdue requirements
* distinguish the internal deadline from the RFP submission deadline

### Requirement Workflow Status

Initial statuses should include:

* not started
* in progress
* ready for review
* changes requested
* approved
* blocked
* not applicable

The system should validate permitted status transitions.

### Contributor Workspace

* view requirements assigned to the current user
* filter by status
* filter by due date
* identify overdue work
* open the related RFP source
* update work status

### Project Requirement Board

* view all approved requirements
* filter by assignee
* filter by status
* filter by category
* filter by mandatory status
* sort by internal deadline
* search by title and description

### Comments

* add internal comments to requirements
* view comment history
* identify comment author and timestamp
* restrict comments to the correct organization
* preserve comments during assignment changes

### Project Progress

* count approved requirements
* count unassigned requirements
* count completed requirements
* count blocked requirements
* display progress by status
* display progress by assignee
* show overdue assignments

### Team Workload

* view the number of assigned requirements per member
* view incomplete work per member
* identify members with no assignments
* avoid treating item count as a complete measure of effort

### Notifications

For the MVP, notifications may remain inside the application.

Possible events include:

* requirement assigned
* requirement reassigned
* internal deadline changed
* comment added
* status changed
* requirement becomes overdue

Email or Slack notifications are not required in this milestone.

### Audit History

Record:

* assignment creation
* reassignment
* due-date changes
* status changes
* comments
* blocked-status changes
* not-applicable decisions

## 5.3 Excluded Features

This milestone does not include:

* company knowledge documents
* semantic retrieval
* AI response generation
* response version history
* response approval workflow
* compliance dashboard
* proposal export
* advanced notification integrations
* automatic workload balancing

## 5.4 Dependencies

Milestone 2 depends on Milestone 1 because:

* requirements must exist before they can be assigned
* only reviewed and approved requirements should enter the main workflow
* organization members and roles must already exist
* tenant isolation must already be enforced

## 5.5 Completion Criteria

Milestone 2 is complete when:

* a proposal manager can assign a requirement to a team member
* contributors can view their assigned requirements
* internal due dates can be added
* requirement statuses can be updated
* blocked and overdue items are visible
* comments can be added
* project progress is displayed
* workload can be viewed by team member
* all assignment data is tenant-isolated
* status and assignment actions are audited
* core workflow actions are covered by automated tests

## 5.6 Suggested Sprint Range

* Sprint 8: Assignment data model and APIs
* Sprint 9: Contributor workspace and status tracking
* Sprint 10: Comments, progress views, and milestone hardening

## 5.7 Planned Backlog File

```text
docs/backlogs/milestone-2-assignments.md
```

---

# 6. Milestone 3: Company Knowledge Base and Retrieval

## 6.1 Goal

Allow organizations to upload trusted company documents and retrieve relevant evidence for proposal responses.

This milestone answers:

> What company information can support a truthful answer to this RFP requirement?

## 6.2 Included Features

### Company Knowledge Documents

Organization administrators should be able to upload documents such as:

* company overviews
* employee resumes
* project case studies
* security policies
* privacy policies
* certifications
* previous proposal responses
* service descriptions
* support procedures
* technical architecture documents
* insurance documents
* standard company answers

### Knowledge-Document Management

* upload supported company documents
* list company documents
* view processing status
* view document metadata
* archive or remove outdated documents
* identify who uploaded each document
* prevent project documents from being confused with company documents

### Document Processing

* background processing
* text extraction
* page or section preservation
* normalization
* chunking
* failure handling
* retry support

The first version may support text-based PDF and DOCX documents.

### Embeddings

* generate embeddings for knowledge chunks
* store embeddings using an organization-isolated approach
* track the embedding model
* regenerate embeddings when content changes
* avoid embedding unchanged content repeatedly

### Vector Retrieval

* search company knowledge semantically
* restrict search to the active organization
* return relevant chunks
* preserve source-document references
* support configurable result limits
* return similarity or relevance information

### Metadata Filtering

Allow retrieval to filter by information such as:

* document type
* employee
* service
* project
* certification
* date
* active or archived status

The initial metadata model may remain limited.

### Retrieval Testing Interface

Provide a development or administrator interface where a user can:

* enter a test question
* inspect retrieved chunks
* view relevance ordering
* open the source document
* verify that another organization’s content is not returned

### Knowledge Quality Controls

* mark documents as active or archived
* prevent archived documents from being used by default
* display outdated-document warnings where possible
* allow administrators to remove incorrect content
* record processing and retrieval metadata

### Security

* strict organization isolation
* private file storage
* authorized viewing
* no cross-tenant vector search
* safe logging
* deletion behavior for document chunks and embeddings

## 6.3 Excluded Features

This milestone does not include:

* generating final response drafts
* automatic acceptance of retrieved facts
* response editing
* response approval
* compliance checking
* proposal export
* external document connectors
* advanced document lifecycle policies
* customer-managed embedding models

## 6.4 Dependencies

Milestone 3 depends on:

* organization and tenant infrastructure from Milestone 1
* document-processing patterns from Milestone 1
* approved RFP requirements from Milestone 1
* project workflow from Milestone 2

Retrieval should be tested independently before it is connected to AI response generation.

## 6.5 Completion Criteria

Milestone 3 is complete when:

* an organization administrator can upload a company document
* the document is processed asynchronously
* the document is divided into source-linked chunks
* embeddings are generated and stored
* a user can search the organization’s knowledge semantically
* retrieved chunks include source references
* archived documents are excluded appropriately
* users cannot retrieve content from another organization
* retrieval quality can be inspected through a testing interface
* document deletion removes or disables related searchable content
* retrieval behavior is covered by automated tests

## 6.6 Suggested Sprint Range

* Sprint 11: Knowledge-document management
* Sprint 12: Knowledge processing and embeddings
* Sprint 13: Retrieval, filtering, and evaluation
* Sprint 14: Security and milestone hardening

## 6.7 Planned Backlog File

```text
docs/backlogs/milestone-3-knowledge-base.md
```

---

# 7. Milestone 4: Grounded Response Drafting

## 7.1 Goal

Allow contributors to create proposal responses manually or generate AI-assisted drafts grounded in RFP context and trusted company evidence.

This milestone answers:

> How should the company respond based on information it can support?

## 7.2 Included Features

### Response Data Model

Each response should support:

* project
* requirement
* response status
* current content
* creator
* last editor
* timestamps
* AI-generated indicator
* citation relationships
* version relationships

### Manual Response Creation

Contributors should be able to:

* create a response manually
* edit response content
* save drafts
* view the associated requirement
* view the original RFP source
* add internal notes
* continue working across sessions

### Response Editor

The editor should support basic formatting:

* paragraphs
* headings
* bold text
* bullet lists
* numbered lists
* links

Advanced page-layout editing is outside the MVP.

### Retrieval for Drafting

The system should retrieve two separate forms of context:

#### RFP Context

Used to understand:

* the exact question
* related mandatory requirements
* evaluation criteria
* submission constraints
* related sections of the RFP

#### Company Evidence

Used to support:

* company capabilities
* previous experience
* security controls
* team qualifications
* service processes
* certifications
* project examples

These two context groups must remain clearly separated.

### AI Draft Generation

A contributor should be able to:

* request a draft for a requirement
* provide optional drafting instructions
* select a desired level of detail
* review retrieved evidence
* generate a structured response
* edit the generated output

### Grounding Rules

Generated drafts must:

* address the selected requirement
* rely on retrieved company evidence
* include citations
* avoid unsupported company claims
* identify insufficient evidence
* avoid inventing customers, certifications, metrics, or experience
* remain marked as AI-generated until edited or approved
* require human review

### Citations

Each citation should include:

* company source document
* page or section
* supporting text
* connection to the generated response
* authorized access to the original source

RFP citations and company citations should be visually distinguishable.

### Insufficient Evidence Handling

When evidence is missing, the system should:

* state that available company information is insufficient
* identify which information is missing
* allow the user to write the answer manually
* allow the user to upload additional company documents
* avoid generating a complete but unsupported answer

### Response Versions

* save a new version after meaningful edits
* identify the version author
* identify whether a version began as AI-generated
* compare versions
* restore or reference a previous version
* preserve citation history where practical

### AI Operation Tracking

Record:

* initiating user
* requirement
* prompt version
* model
* retrieved RFP context
* retrieved company evidence
* generation time
* usage metadata where available
* success or failure status

## 7.3 Excluded Features

This milestone does not include:

* final response approval
* reviewer sign-off
* compliance dashboard
* DOCX export
* real-time collaborative editing
* automatic pricing generation
* automatic legal commitments
* autonomous proposal completion

## 7.4 Dependencies

Milestone 4 depends on:

* reviewed RFP requirements from Milestone 1
* assignment workflow from Milestone 2
* company retrieval from Milestone 3

Response generation should not begin until retrieval returns useful and tenant-safe evidence.

## 7.5 Completion Criteria

Milestone 4 is complete when:

* a contributor can create a manual response
* a contributor can request an AI-assisted response draft
* the system retrieves relevant RFP context
* the system retrieves relevant company evidence
* the generated draft contains source citations
* unsupported claims are avoided or clearly flagged
* insufficient evidence produces an appropriate warning
* the contributor can edit and save the response
* response versions are preserved
* users cannot generate drafts from another organization’s knowledge
* core drafting behavior is covered by tests and AI evaluations

## 7.6 Suggested Sprint Range

* Sprint 15: Response model and manual editor
* Sprint 16: Drafting retrieval pipeline
* Sprint 17: Grounded AI response generation
* Sprint 18: Citations, versions, and milestone hardening

## 7.7 Planned Backlog File

```text
docs/backlogs/milestone-4-response-drafting.md
```

---

# 8. Milestone 5: Response Review and Approval

## 8.1 Goal

Allow contributors to submit responses for review and allow reviewers to request changes or approve final answers.

This milestone answers:

> Has an authorized person reviewed and accepted this proposal response?

## 8.2 Included Features

### Response Workflow

Initial response statuses should include:

* not started
* drafting
* ready for review
* changes requested
* approved
* blocked
* not applicable

The system should validate allowed transitions.

### Submit for Review

Contributors should be able to:

* submit a response for review
* include an optional review note
* select or identify a reviewer where appropriate
* prevent incomplete submissions where required
* receive confirmation that the response was submitted

### Review Queue

Reviewers should be able to:

* view responses awaiting review
* filter by project
* filter by requirement category
* filter by contributor
* filter by age or due date
* open the related requirement and source evidence

### Review Workspace

A reviewer should be able to inspect:

* the RFP requirement
* RFP source text
* current response
* previous response versions
* company citations
* AI-generated indicators
* contributor notes
* earlier review comments

### Request Changes

A reviewer should be able to:

* request changes
* provide a required explanation
* identify specific concerns
* return the response to the contributor
* preserve the reviewed version
* record the reviewer and timestamp

### Approve Response

A reviewer should be able to:

* approve a response
* record the approving user
* record approval time
* preserve the approved version
* prevent unauthorized approval
* reopen the response through a controlled revision process

### Review Comments

* add comments
* reply to comments where practical
* resolve comments
* preserve comment history
* identify authors and timestamps
* restrict comments to the organization

### Controlled Revisions

When an approved response requires changes:

* approval should not silently remain valid
* the system should create a new draft version
* the previous approved version should remain available
* the response should return to a review-required state

### Audit History

Record:

* submission for review
* reviewer assignment
* comments
* changes requested
* approval
* reopened responses
* version changes

### Review Notifications

In-application notifications may include:

* response submitted
* review assigned
* changes requested
* response approved
* reviewer comment added

External notifications remain optional.

## 8.3 Excluded Features

This milestone does not include:

* complete proposal-readiness checks
* proposal exports
* electronic signatures
* legal approval automation
* configurable multi-stage enterprise workflows
* approval delegation
* real-time collaborative editing

## 8.4 Dependencies

Milestone 5 depends on:

* assignment and workflow infrastructure from Milestone 2
* editable and versioned responses from Milestone 4
* user roles and permissions from Milestone 1

## 8.5 Completion Criteria

Milestone 5 is complete when:

* a contributor can submit a response for review
* a reviewer can view the response and supporting sources
* a reviewer can leave comments
* a reviewer can request changes
* the contributor can revise and resubmit
* an authorized reviewer can approve the response
* approved versions remain traceable
* editing an approved response invalidates or reopens approval appropriately
* unauthorized users cannot approve responses
* review actions are audited
* core review workflows are covered by automated tests

## 8.6 Suggested Sprint Range

* Sprint 19: Response workflow and submission
* Sprint 20: Reviewer workspace and comments
* Sprint 21: Approval, controlled revisions, and milestone hardening

## 8.7 Planned Backlog File

```text
docs/backlogs/milestone-5-review-approval.md
```

---

# 9. Milestone 6: Compliance Checks and Export

## 9.1 Goal

Allow proposal managers to determine whether a proposal is complete and export the project’s responses and requirement information.

This milestone answers:

> Is the proposal ready to prepare for submission?

## 9.2 Included Features

### Compliance Rules

The system should identify:

* unanswered requirements
* unassigned requirements
* overdue assignments
* blocked requirements
* responses still in drafting
* responses awaiting review
* responses with changes requested
* unapproved mandatory responses
* missing required attachments
* unresolved clarification items
* failed document-processing jobs
* low-confidence extraction items still pending review
* submission deadlines approaching or passed

### Mandatory and Optional Items

Compliance calculations should distinguish between:

* mandatory requirements
* optional requirements
* informational sections
* not-applicable items
* rejected AI extractions

Rejected extraction candidates must not count as missing proposal work.

### Compliance Dashboard

The dashboard should show:

* overall readiness status
* completion percentage
* mandatory-item completion
* optional-item completion
* requirement counts by status
* response counts by status
* assignment issues
* attachment issues
* review issues
* extraction-review issues
* deadline warnings

### Readiness Status

Possible readiness states:

* not ready
* at risk
* nearly ready
* ready for export

The exact logic must be documented and should not claim that the proposal is legally or commercially safe to submit.

### Required Attachments

Users should be able to:

* mark a requirement as requiring an attachment
* upload or link a project attachment
* associate an attachment with a requirement
* identify missing required attachments
* view attachment status

Advanced form-filling is outside the MVP.

### DOCX Export

Generate a DOCX containing:

* project title
* issuing organization
* submission deadline
* approved requirement titles
* approved response text
* requirement source references
* company citations where appropriate
* approval information
* generation date

The MVP does not need to reproduce the buyer’s exact formatting.

### Requirements Matrix Export

Generate an XLSX or CSV containing fields such as:

* requirement ID
* title
* description
* category
* type
* mandatory status
* assignee
* internal due date
* workflow status
* review status
* response status
* source document
* source pages
* attachment requirement
* approval information

### Compliance Summary Export

Generate a summary containing:

* total mandatory requirements
* completed mandatory requirements
* outstanding mandatory requirements
* missing attachments
* overdue assignments
* unapproved responses
* unresolved blockers
* readiness status

### Export Authorization

* only authorized users can generate exports
* exports belong to one organization and project
* exports are stored privately
* download links are authorized or short-lived
* exports do not include another organization’s data

### Export History

Record:

* export type
* project
* generating user
* generation time
* included version or snapshot
* export status
* storage reference

### Snapshot Consistency

An export should represent a consistent project state.

Where practical:

* response versions should be identified
* approval state should be captured
* export generation should not mix unrelated updates
* the export should display its generation timestamp

## 9.3 Excluded Features

This milestone does not include:

* automatic submission to procurement portals
* exact reproduction of every RFP template
* electronic signatures
* advanced pricing calculations
* legal review certification
* customer-facing proposal websites
* Microsoft Word add-ins
* complex branded proposal-template builders

## 9.4 Dependencies

Milestone 6 depends on all earlier milestones because:

* requirements must be extracted and reviewed
* assignments and statuses must exist
* responses must be drafted
* responses must be reviewed and approved
* citations and attachments must be available

## 9.5 Completion Criteria

Milestone 6 is complete when:

* the compliance dashboard identifies incomplete work
* mandatory and optional items are calculated separately
* missing attachments are visible
* unapproved mandatory responses block ready status
* a proposal manager can generate a DOCX response package
* a proposal manager can generate an XLSX or CSV requirements matrix
* a proposal manager can generate a compliance summary
* export access is tenant-isolated
* export generation is audited
* the complete workflow from account creation through export is covered by an end-to-end test

## 9.6 Suggested Sprint Range

* Sprint 22: Compliance rules and dashboard
* Sprint 23: Attachments and readiness calculations
* Sprint 24: DOCX and requirements-matrix exports
* Sprint 25: Full MVP hardening and end-to-end testing

## 9.7 Planned Backlog File

```text
docs/backlogs/milestone-6-compliance-export.md
```

---

# 10. Full MVP Completion Criteria

The RFPFlow MVP is complete when a user can perform the entire workflow below.

## 10.1 Account and Organization

* register an account
* log in securely
* create or join an organization
* access only authorized organization data
* use role-based permissions

## 10.2 RFP Project Creation

* create an RFP project
* enter project information
* upload a text-based PDF RFP
* view document-processing status

## 10.3 RFP Analysis

* extract document text by page
* extract candidate requirements
* view source text and page citations
* edit extracted requirements
* approve or reject extracted requirements
* manually add missed requirements

## 10.4 Project Coordination

* assign requirements to team members
* set internal deadlines
* track workflow statuses
* add comments
* identify unassigned, blocked, and overdue work

## 10.5 Company Knowledge

* upload company documents
* process and chunk company documents
* create embeddings
* retrieve relevant organization-specific evidence
* inspect source references
* remove or archive outdated information

## 10.6 Proposal Responses

* create responses manually
* generate AI-assisted drafts
* retrieve relevant RFP context
* retrieve relevant company evidence
* include citations
* identify insufficient evidence
* edit and version responses

## 10.7 Review and Approval

* submit responses for review
* leave review comments
* request changes
* revise responses
* approve final responses
* preserve approval and version history

## 10.8 Compliance and Export

* identify incomplete requirements
* identify unassigned or overdue work
* identify unapproved responses
* identify missing attachments
* calculate proposal readiness
* export a DOCX proposal package
* export a requirements matrix
* export a compliance summary

## 10.9 Engineering Quality

* backend authorization is enforced
* tenant-isolation tests pass
* unit tests pass
* integration tests pass
* end-to-end tests pass
* AI extraction evaluations are documented
* retrieval evaluations are documented
* generated-response grounding is evaluated
* CI runs on pull requests
* database migrations are reliable
* background processing is reliable
* private files remain protected
* architecture and security documentation are complete
* the application is deployed to a usable environment

---

# 11. Milestone Dependencies

```text
Milestone 1
RFP ingestion and requirement review
        |
        v
Milestone 2
Assignments and workflow tracking
        |
        v
Milestone 3
Company knowledge base and retrieval
        |
        v
Milestone 4
Grounded response drafting
        |
        v
Milestone 5
Response review and approval
        |
        v
Milestone 6
Compliance checks and export
```

Some engineering work may overlap, but major product dependencies should remain in this order.

For example:

* document-storage components from Milestone 1 may be reused in Milestone 3
* comments from Milestone 2 may influence review comments in Milestone 5
* compliance-rule planning may begin earlier, but it cannot be completed until response approval exists
* export prototypes may be tested earlier, but final exports depend on approved responses

---

# 12. Suggested Release Strategy

## Release 0.1: Requirement Extraction Prototype

Includes:

* PDF upload
* text extraction
* AI requirement extraction
* basic requirement list

This may be an internal prototype and does not need full multi-user functionality.

## Release 0.2: Milestone 1 Complete

Includes:

* authentication
* organizations
* RFP projects
* secure uploads
* background processing
* requirement extraction
* human review
* tenant isolation

This is the first credible product demonstration.

## Release 0.3: Milestone 2 Complete

Includes:

* assignments
* internal deadlines
* workflow statuses
* comments
* project progress

The application now provides useful project coordination even without response generation.

## Release 0.4: Milestone 3 Complete

Includes:

* company knowledge documents
* embeddings
* semantic retrieval
* source-linked evidence

The system can now find relevant company information but does not yet generate final responses.

## Release 0.5: Milestone 4 Complete

Includes:

* manual response editor
* grounded AI drafts
* citations
* insufficient-evidence handling
* response versions

The application now demonstrates the core RAG feature.

## Release 0.6: Milestone 5 Complete

Includes:

* review submission
* comments
* change requests
* approvals
* controlled revisions

The application now supports a complete internal proposal-writing workflow.

## Release 1.0: Full MVP

Includes:

* compliance dashboard
* required attachments
* DOCX export
* requirements-matrix export
* compliance summary
* full workflow hardening
* deployment
* complete demonstration

---

# 13. Roadmap Principles

## 13.1 Build Vertical Slices

Each sprint should deliver a working path through the frontend, backend, database, authorization, and tests.

Avoid building the entire frontend or backend in isolation.

## 13.2 Keep AI Human-Supervised

AI-generated requirements and responses must remain reviewable and editable.

The AI must not independently approve requirements, responses, pricing, legal commitments, or proposal readiness.

## 13.3 Preserve Sources

Requirements should link back to the RFP.

Generated proposal claims should link back to company evidence.

Source traceability is a core product requirement, not an optional enhancement.

## 13.4 Protect Tenant Data

Every milestone must include tenant-isolation checks.

Organization identifiers supplied by the frontend must never be treated as sufficient authorization.

## 13.5 Test AI Components Separately

The project should evaluate:

* requirement extraction
* requirement-source accuracy
* retrieval relevance
* citation validity
* unsupported claims
* insufficient-evidence behavior

AI quality should not be evaluated only by whether the wording sounds good.

## 13.6 Delay Advanced Features

The team should not prioritize:

* procurement-portal submission
* exact government-form reproduction
* advanced pricing tools
* real-time collaborative editing
* bid-win prediction
* CRM integrations
* enterprise single sign-on
* custom workflow builders

until the full MVP workflow works reliably.

## 13.7 Update Planning as the Product Evolves

Later milestone backlogs should remain high-level until the team approaches them.

Before beginning each milestone:

1. review what was learned from the previous milestone
2. update assumptions
3. create or refine the detailed milestone backlog
4. confirm architecture changes
5. define milestone-specific acceptance criteria
6. select the first sprint stories

---

# 14. Documentation Structure

The project documentation should eventually use the following structure:

```text
docs/
├── product-requirements.md
├── roadmap.md
├── architecture.md
├── database-schema.md
├── security.md
├── testing-strategy.md
├── deployment.md
├── ai-extraction-pipeline.md
├── rag-pipeline.md
├── export-design.md
├── adr/
│   ├── ADR-001-monorepo.md
│   ├── ADR-002-frontend-framework.md
│   ├── ADR-003-backend-framework.md
│   ├── ADR-004-background-workers.md
│   ├── ADR-005-object-storage.md
│   ├── ADR-006-tenant-isolation.md
│   └── ADR-007-vector-search.md
└── backlogs/
    ├── milestone-1-rfp-ingestion.md
    ├── milestone-2-assignments.md
    ├── milestone-3-knowledge-base.md
    ├── milestone-4-response-drafting.md
    ├── milestone-5-review-approval.md
    └── milestone-6-compliance-export.md
```

Only the Milestone 1 backlog needs to be highly detailed before development begins.

The later backlog files should be created and expanded closer to their implementation dates.

---

# 15. Immediate Next Step

The planning sequence is now:

```text
Product Requirements Document
→ MVP Roadmap
→ Milestone 1 Detailed Backlog
→ Initial Architecture Design
→ Database Design
→ Sprint 0 Repository Setup
→ Milestone 1 Implementation
```

The immediate next deliverable should be:

```text
docs/architecture.md
```

That document should define:

* frontend architecture
* backend architecture
* worker architecture
* database responsibilities
* file-storage approach
* Redis and queue responsibilities
* AI-provider boundaries
* authentication approach
* tenant-isolation approach
* request flows
* background-processing flows
* deployment boundaries
* trust boundaries
* major technology decisions
