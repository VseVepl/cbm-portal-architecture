# CBM Development Phases and Lifecycle Plan

| Document attribute | Value |
|---|---|
| Document status | Development-planning baseline; proposed lifecycle model and technical recommendations, not authorization to implement |
| Baseline date | 19 November 2026 |
| Repository branch | `architecture/domain-architecture-continuation-2026-11-19` |
| Language | English |
| Document role | Supporting development lifecycle and phase architecture |
| Authority | Subordinate to `CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md` |
| Implementation authorization | None; planning and documentation only |
| Related runbook | [CBM Development Foundation Installation and Validation Runbook](CBM_DEVELOPMENT_FOUNDATION_INSTALLATION_RUNBOOK.md) |

## 1. Purpose, authority, and scope

This document defines a proposed, dependency-aware lifecycle for preparing, engineering, verifying, releasing, operating, evolving, migrating, and eventually retiring the CBM Portal. It is intended to turn the approved business and logical architecture into a practical engineering programme without redesigning the approved product.

It is a development plan, not a new business-architecture decision, detailed functional specification, implementation authorization, infrastructure change request, or deployment approval. It does not authorize installation of software on any real machine, application coding, database changes, production configuration changes, or deployment.

### 1.1 Governing sources and precedence

1. `CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md` is the sole authoritative source for finalized business and architecture decisions.
2. `CBM_DOMAIN_LIFECYCLE_AND_WORKFLOW_REFERENCE.md` is the consolidated lifecycle/workflow inventory. It explicitly states that detailed state machines and exception policies remain incomplete.
3. `CBM_LIFECYCLE_AND_WORKFLOW_COMPLETENESS_AUDIT.md` is the discovery/gap assessment and its fifteen-point minimum lifecycle/workflow completion standard remains applicable.
4. `README.md` is a concise overview; it does not override the Master.
5. `CBM_PROJECT_DISCUSSION_HISTORY.md` records chronology and rationale, not normative decisions.
6. This plan governs development-phase sequencing only. It must not silently finalize business policies or domain states absent from the Master.

If a conflict is discovered, stop the affected planning item, identify the exact conflict, and resolve it through the documentation governance process. Do not silently reinterpret a finalized concept.

### 1.2 Non-negotiable architecture constraints

Every phase must preserve the Master baseline, including:

- Strict Business/Tenant Data Isolation and structural integrity as non-overridable Stable Core constraints.
- Main Portal's supreme ecosystem governance authority and the approved Organization → Distributor and/or Shop structure.
- Separation of Legal Identity, Professional Identity, Business Identity, ownership, authority, User scope, Role, Permission, Delegated Authority, Approval, and Subscription Entitlement.
- Independent lifecycles and authoritative ownership of the fourteen approved Business Modules.
- Platform-wide Services as a distinct Architecture Layer; no duplicate Business Modules for shared platform capabilities.
- The No-URL rule for all CBM-generated outbound communications.
- India-only product baseline: INR, IST/Asia/Kolkata, approved Indian language governance, Indian conventions, metric baseline, Gregorian core calendar, and default 1 April–31 March Financial Year.
- CBM-owned canonical Accounting model; external accounting systems such as Tally remain optional exchange adapters.
- Product identity, Inventory, Warehouse, Sales, Purchase, POS, GST/Tax, Finance/Payment, Accounting, Asset, HR, Payroll, Planning, and Counterparty ownership boundaries.
- Typed, versioned, provenance-preserving contracts; immutable accepted transaction snapshots where required.
- Scalability, configurability, extensibility, customization, controlled override, applicability, maintainability, operability, security, and data integrity.

### 1.3 Scope exclusions

This plan does not approve E-commerce, Service Centre Management, Warranty Management, CRM lead/pipeline, Manufacturing, a universal Bill of Materials, unrestricted portal replication, universal advanced WMS, or tax domains beyond the approved GST-focused scope. Conditional capabilities remain conditional and require explicit applicability and scope approval before being scheduled.

## 2. Recommended lifecycle model

### 2.1 Selected model: governed hybrid iterative-incremental lifecycle

Adopt a **hybrid, iterative-incremental, risk-driven lifecycle with explicit phase gates**.

This is a recommended development-process choice, not a finalized business decision. It is selected because CBM combines:
- a broad and interdependent multi-domain SaaS product;
- immutable business and data-isolation invariants that must be established before dependent implementation;
- incomplete detailed lifecycle/workflow specifications that need progressive elaboration;
- independently owned modules that can be developed in bounded increments;
- substantial cross-module integration, statutory, security, migration, and operational risks;
- a long expected product life with ongoing service-provider, customer, and platform evolution.

A purely sequential waterfall model would delay useful validation and make late domain discovery expensive. Uncontrolled continuous delivery without architecture and acceptance gates would risk violating stable domain ownership and security boundaries. A single large-batch release would create excessive integration and migration risk. The hybrid model combines staged governance with short, testable engineering increments.

### 2.2 Lifecycle characteristics

- **Stage gates:** each major phase has documented entry criteria, evidence, acceptance criteria, exit decision, and handover.
- **Iterative elaboration:** requirements and detailed domain specifications are refined in bounded increments, but unresolved business policy is never silently invented.
- **Incremental delivery:** implementation increments are vertical, testable slices that preserve module ownership and contracts.
- **Risk-driven ordering:** tenant isolation, identity/authority, data integrity, financial/tax consequences, concurrency, and migration risks are addressed early.
- **Continuous verification:** automated tests, security analysis, static analysis, dependency review, and documentation traceability run throughout engineering.
- **Controlled promotion:** immutable, versioned artifacts move through compatible environments; production is not rebuilt ad hoc.
- **Independent domain completion:** cross-module journeys are complete only when each required owner domain has confirmed its own outcome.

### 2.3 Standards and guidance tailoring

Use standards as process references, not as a claim of certification or a requirement to reproduce every clause.

| Reference | Current status verified for this plan | How CBM should use it |
|---|---|---|
| ISO/IEC/IEEE 12207:2026 | Active; published 15 April 2026; supersedes 12207:2017 | Primary software life-cycle process reference; tailor processes and outcomes to CBM |
| ISO/IEC/IEEE 24748-1:2024 | Published Edition 2, March 2024 | Life-cycle model, stages, process application, adaptation and management guidance |
| ISO/IEC/IEEE 29148:2018 | Current published edition as of this baseline; reviewed/confirmed 2024; DIS 29148 Edition 3 is still a draft under enquiry | Requirements engineering and information-item guidance; do not cite the draft as a published replacement |
| OWASP SAMM 2.0 | Security maturity model | Organize governance, design, implementation, verification and operations security practices |
| OWASP ASVS 5.0.0 | Stable release, published 30 May 2025 | Testable application-security verification requirements; select applicable requirements and assurance level |
| NIST SP 800-63B-4 | Published July 2025 | Reference for authenticator and session design; not asserted as Indian statutory law |

The exact standards text may be licensed/purchase-controlled. The project must not claim conformance, certification, or clause-level compliance without access to the normative text and a formal assessment.

## 3. Lifecycle overview and phase catalogue

The lifecycle is divided into seventeen governed phases. Some workstreams can overlap after their prerequisites are satisfied; the phase catalogue is not a requirement to serialize every task. Blocking dependencies and phase gates take precedence over calendar convenience.

| Phase | Name | Main result | Gate |
|---|---|---|---|
| P00 | Mobilization and Project Governance | Scope, authority, registers, working agreements, traceability and decision controls | Governance baseline accepted |
| P01 | Development Foundation and Toolchain Setup | Verified developer toolchain and reproducible setup runbooks | Every mandatory tool passes version/configuration/security checks |
| P02 | Requirements and Domain Specification Completion | Approved requirements, domain workflows, states, exceptions and traceability | Required specification set reviewed and gaps explicitly classified |
| P03 | System Architecture and Engineering Baselines | Logical/technical architecture, NFRs, threat model, contracts and architecture records | Architecture baseline reviewed against Master |
| P04 | Environment and Platform Infrastructure Foundation | Segregated, reproducible Dev/CI/Test/QA/Staging/Production/DR foundations | Environment and isolation controls verified |
| P05 | Engineering Workflow, CI and Supply-Chain Controls | Repeatable build/test/review/artifact pipeline | Protected pipeline gates pass |
| P06 | Quality, Security and Compliance Framework Enablement | Shared quality, security, privacy, audit and compliance controls | Control framework and evidence model approved |
| P07 | Core Platform and Stable-Core Capabilities | Tenant/business structure, identity/access foundations, settings, subscription entitlements and shared services foundations | Stable Core and isolation tests pass |
| P08 | Shared Domain Contracts and Integration Foundation | Versioned references, event/API contracts, idempotency and integration infrastructure | Contract governance and compatibility gates pass |
| P09 | Foundational Business Domain Increments | Product/Item, Counterparty and Accounting foundations, plus their governed shared dependencies | Each domain increment independently accepted |
| P10 | Inventory, Warehouse, Purchase and Supply Operations | Stock, physical handling and procurement increments | Stock/warehouse/procurement reconciliation gates pass |
| P11 | Sales, POS, GST/Tax and Finance/Payment Operations | Commercial execution, checkout, tax determination and payment increments | Commercial, tax, payment and reconciliation gates pass |
| P12 | HR, Payroll, Asset and Planning Operations | People, payroll, operational assets and planning increments | Sensitive-data, statutory and accounting boundary gates pass |
| P13 | Cross-Module Journeys, Reporting and Data Migration | End-to-end journeys, reconciliation, analytics and migration capabilities | Every participating domain's completion evidence is reconciled |
| P14 | System Verification, UAT and Operational Readiness | Release candidate and verified operational readiness | All release-blocking defects and readiness criteria closed |
| P15 | Controlled Production Release and Service Transition | Approved, observable, recoverable production release | Release authorization and post-release verification |
| P16 | Operate, Maintain, Evolve, Migrate and Retire | Supported service, upgrades, continuity, portability and controlled retirement | Ongoing service reviews and retirement evidence |

### 3.1 Phase lifecycle common to every phase

Every phase follows this controlled lifecycle:

1. **Proposed:** phase scope, owner, prerequisites, risks and intended outputs are identified.
2. **Ready for entry:** all mandatory entry criteria and upstream dependencies have evidence.
3. **In progress:** activities execute under change control; outputs and decisions are versioned.
4. **Validation:** deliverables are checked against acceptance criteria; tests and evidence are retained.
5. **Blocked / rework required:** failed criteria, unresolved decisions, missing evidence or upstream defects are recorded; dependent work is held where necessary.
6. **Accepted:** accountable reviewer accepts the phase evidence; acceptance does not waive unresolved out-of-scope items.
7. **Closed / handed over:** artifacts, open non-blocking issues, operational ownership and downstream inputs are transferred.
8. **Reopened / superseded:** a material defect, changed requirement, vulnerability, legal change or invalidated assumption triggers controlled rework and impact analysis.

A phase may not be marked complete solely because tasks were attempted or a build succeeded. Acceptance requires evidence. A failed phase returns to the responsible work item or earlier phase; it does not bypass a gate.

### 3.2 Standard phase record

Each phase specification must contain all of the following, even when an item is marked not applicable with a reason:

1. Phase number/name.
2. Purpose/objectives.
3. Scope and explicit exclusions.
4. Business/technical outcomes.
5. Entry criteria/preconditions.
6. Dependencies on earlier phases.
7. Required inputs and decisions.
8. Detailed activities in sequence.
9. Required packages/software/tools/services.
10. Installation/configuration requirements.
11. Architecture/security requirements.
12. Required documentation/records.
13. Deliverables/artifacts.
14. Verification/testing strategy.
15. Validation/acceptance criteria.
16. Failure scenarios/recovery.
17. Rollback/backout strategy.
18. Risks/mitigation.
19. Ownership/responsible roles.
20. Exit criteria/phase gate.
21. Handover to next phase.
22. Change/version management.
23. Operational/maintenance considerations.

For each stateful object/workflow, additionally apply the fifteen-point lifecycle completion standard in Section 12.

## 4. Detailed phase specifications

### P00 — Mobilization and Project Governance

**Purpose and outcomes:** establish a controlled project operating model without reopening approved business concepts. Confirm repository authority, document hierarchy, decision categories, scope boundaries, responsible roles, change control and traceability.

**Scope/exclusions:** project governance, planning, documentation workflow and risk/decision registers. No application implementation or commercial release commitments.

**Entry:** owner-approved project handover and access to the designated repository branch.

**Dependencies/inputs:** current Master, README, Lifecycle Reference, Completeness Audit, Discussion History, current branch and comparison with main.

**Sequence:**
1. Verify repository, exact branch, permissions, branch head and main comparison.
2. Read the complete authoritative and supporting documents.
3. Establish the requirement → architecture → domain specification → implementation → test → release traceability model.
4. Establish separate registers for open owner decisions, technical recommendations, legal/statutory verification, risks, assumptions, dependencies and changes.
5. Assign accountable roles for product/domain ownership, architecture, engineering, security, QA, operations, data migration and release.
6. Define repository protection, review and documentation acceptance practices.
7. Baseline the scope exclusions and explicit implementation authorization boundary.

**Tools:** Git/GitHub, Markdown lint/link checking when adopted, diagram source format selected by the project. No additional package is mandatory solely for governance.

**Security/architecture:** never place secrets, production data or personal data in repository documents; preserve Stable Core and source-of-truth precedence.

**Deliverables:** governance charter, registers, traceability scheme, document map, branch baseline and phase plan.

**Verification/acceptance:** all source documents are current; branch is correct; no contradiction with Master; all roles and registers have owners; scope and authorization boundary are explicit.

**Failure/recovery:** inaccessible repository or conflicting source baseline blocks entry to documentation changes; report the exact failure and do not write to another branch.

**Rollback/change:** governance changes are version-controlled and reviewed; restore prior approved governance revision if a change is invalid.

**Ownership/handover:** project owner accountable; architecture/documentation lead responsible. Handover to P01 with an approved governance baseline.

### P01 — Development Foundation and Toolchain Setup

**Purpose and outcomes:** produce a documented, reproducible and verified developer workstation/toolchain foundation. This is the first software-installation planning stage and is detailed in the companion Installation Runbook.

**Scope/exclusions:** host prerequisites, OS, Git, runtimes, backend/frontend toolchains, PostgreSQL 18+, local supporting services where justified, build/test/security/documentation utilities and editor integration. This plan does not execute installations.

**Entry:** P00 accepted; target workstation/OS/architecture and resource profile identified; no unresolved platform incompatibility.

**Dependencies/inputs:** approved Laravel/React direction; PostgreSQL 18+ mandatory baseline; official support matrices; licensing/security review.

**Sequence:**
1. Inventory hardware, OS, architecture, virtualization, network, disk, existing runtimes and conflicts.
2. Select a supported LTS host baseline and package-management source.
3. Establish trusted repositories, signing-key verification, TLS CA trust and system update policy.
4. Install Git and basic diagnostic/build utilities.
5. Install supported PHP runtime and required extensions; verify CLI and web/runtime parity.
6. Install Composer and verify signatures/checksums where offered.
7. Install Node.js LTS and the selected package manager; verify lockfile reproducibility.
8. Install PostgreSQL 18 server/client and migration/backup/restore utilities; validate encoding, locale, timezone, role and connection policy.
9. Install or provision optional supporting services only where a confirmed requirement exists (e.g. Redis, S3-compatible object storage, local mail sink).
10. Install container tooling only if selected for reproducibility; verify daemon, Compose, image provenance and network exposure.
11. Install browser automation and browser dependencies for supported developer/CI OS.
12. Enable static analysis, formatting, test, dependency/license/security and secret-scanning tools.
13. Configure TLS for local development where required; do not weaken production TLS.
14. Configure editor/IDE integration without making one vendor editor mandatory.
15. Capture exact versions, checksums/image digests, licenses, configuration, validation output and rollback instructions.
16. Re-run clean-room setup on a second machine or disposable environment to test reproducibility.

**Deliverables:** approved software inventory, compatibility matrix, setup scripts/runbooks, environment manifest, validation report, known limitations and recovery procedure.

**Acceptance:** all mandatory tools install from official/trusted sources; exact versions are recorded; runtime/framework/database compatibility passes; a clean checkout can install dependencies from lockfiles and run prescribed checks; no secrets are committed; PostgreSQL backup and restore are demonstrated in a disposable database.

**Failure/recovery:** stop on unsupported OS/architecture, untrusted repository/signature, package conflict, incompatible extension, failed database restore or irreproducible dependency graph. Preserve diagnostic logs; roll back only the affected package/configuration and retry after compatibility review.

**Security:** least-privilege developer accounts; no routine root use; secrets in approved local secret store/environment injection; database bound to intended interfaces; firewall/container port review; no production data copied into local development.

**Ownership:** platform/toolchain engineer responsible; security/architecture review; developer validation.

**Handover:** verified environment manifest and installation runbook to P02–P05. P01 completion does not authorize implementation.

### P02 — Requirements and Domain Specification Completion

**Purpose/outcomes:** turn the approved product baseline and lifecycle catalogue into testable, traceable, owner-reviewed specifications before dependent implementation.

**Scope/exclusions:** requirements, applicability, domain boundaries, object lifecycles, workflows, state machines, actor/authority matrices, business rules, exceptions, data contracts and acceptance examples. No silent business-policy invention.

**Entry/dependencies:** P00 accepted; relevant P01 environment available for later validation; Master and lifecycle documents read.

**Sequence:**
1. Build an inventory of requirements, domain objects, workflows, 14 Modules, Platform-wide Services and cross-module journeys.
2. Map each item to authoritative owner, consumer, applicability, data classification and History set.
3. Apply the fifteen-point completion standard to each lifecycle/workflow.
4. Define states, allowed/forbidden transitions, guards, triggering events, actor/authority, evidence and terminal conditions.
5. Specify failure, timeout, retry, idempotency, concurrency, partial completion, compensation, reversal and manual recovery.
6. Specify effective/recorded time, versioning, retention/legal hold, archive/restore and deletion conditions.
7. Define cross-domain request/event/response contracts and per-domain completion evidence.
8. Identify legal/statutory claims and verify them against current authoritative sources; record jurisdiction, applicability and effective date.
9. Separate technical recommendations from owner-level business-policy decisions.
10. Produce acceptance criteria and traceability to future tests.
11. Review conflicts with the Master and reconcile before marking a specification accepted.

**Deliverables:** requirements catalogue, domain specifications, workflow/state diagrams, actor/authority matrices, contract requirements, decision/legal register, traceability and acceptance examples.

**Verification:** domain-owner review; architecture review; no unowned source record; no inferred permission; no missing exception path without explicit classification; all cross-module states remain independent.

**Failure/recovery:** return incomplete specifications to their owning domain; unresolved owner decisions remain blockers only where needed, otherwise explicitly bounded. Legal uncertainty is not replaced with guessed rules.

**Exit:** implementation-ready specifications for the intended increment, not necessarily all 14 Modules at once. P02 is iterative and can run per bounded domain increment under P09–P13 gates.

### P03 — System Architecture and Engineering Baselines

**Purpose/outcomes:** define the technical architecture that implements approved business concepts while satisfying quality attributes and operational constraints.

**Scope:** system context, deployment topology, trust boundaries, data architecture, tenancy, API/event architecture, frontend/backend boundaries, integration, observability, resilience, scalability, security, accessibility and architecture decision records.

**Entry/dependencies:** relevant P02 specifications; P01 toolchain compatibility evidence.

**Sequence:**
1. Derive measurable non-functional requirements (availability, latency, throughput, recovery, capacity, accessibility, security and data portability) from owner-approved priorities.
2. Model threats and trust boundaries, including cross-tenant access, privileged roles, provider access, credential/session abuse, integrations and file processing.
3. Define tenant isolation strategy and prove it with testable architecture requirements.
4. Define domain ownership and data write boundaries; avoid shared write ownership and universal master models.
5. Define API, event, idempotency, versioning, correlation, outbox/inbox and reconciliation strategy.
6. Define database schema/migration policy, transaction boundaries, locking/concurrency and data integrity.
7. Define frontend architecture, state/data fetching, component accessibility and design-system governance.
8. Define file/object storage, encryption, backup, restore and portability.
9. Define logging/metrics/tracing, privacy-safe observability and audit/history separation.
10. Define deployment, configuration, secret and environment promotion patterns.
11. Evaluate alternatives and record decisions with rationale, constraints, consequences and review triggers.

**Deliverables:** architecture overview, context/container/deployment diagrams, threat model, NFR catalogue, ADRs, data and integration principles, environment architecture, observability and resilience baseline.

**Acceptance:** traceability to requirements; no violation of Master ownership; measurable NFRs have validation methods; security threats have mitigations/tests; all unresolved choices are labelled.

**Recovery/change:** architecture changes require impact analysis across domains, contracts, migrations, security, tests, environments and runbooks. Reopen impacted ADRs, not unrelated decisions.

### P04 — Environment and Platform Infrastructure Foundation

**Purpose/outcomes:** establish controlled, isolated and reproducible environments for development, CI, test, integration, QA, staging, production and disaster recovery.

**Entry/dependencies:** P01 and P03 accepted; environment-specific owners and access policies identified.

**Sequence:**
1. Define environment purpose, trust level, data class, network boundaries and allowed integrations.
2. Provision infrastructure through reviewed, repeatable configuration/IaC where justified.
3. Separate accounts, credentials, databases, buckets, queues, caches, certificates and network policies.
4. Establish secrets/certificate lifecycle, rotation and revocation.
5. Configure database roles, TLS, backups, point-in-time recovery where supported, restore validation and retention.
6. Configure cache/queue/object storage only when required by accepted architecture.
7. Configure DNS/reverse proxy/TLS and ingress restrictions.
8. Seed synthetic test data; prohibit casual production-data copying.
9. Establish environment manifests and drift detection.
10. Validate network isolation, tenant test isolation, backup restore and environment teardown/recreation.

**Deliverables:** environment topology, provisioning code/runbooks, access matrix, secrets/certificate runbooks, backup/restore evidence, synthetic data policy and drift report.

**Acceptance:** environments are reproducible, access is least-privilege, production is isolated, restore tests pass, secrets are not in source control, and differences are documented.

**Failure/recovery:** failed provisioning returns to known infrastructure state; no manual undocumented drift. Restore from approved backups/snapshots only after integrity validation.

### P05 — Engineering Workflow, CI and Supply-Chain Controls

**Purpose/outcomes:** establish consistent pull-request, build, test, artifact and provenance workflows.

**Entry/dependencies:** P01, P03 and P04 foundations.

**Sequence:** define branch protection and review policy; create deterministic dependency install; run formatting/static analysis/unit tests/integration checks; build frontend/backend; scan dependencies/licenses/secrets; create immutable artifacts; attach provenance and SBOM; publish only to approved registries; promote the same artifact across environments; retain logs and evidence.

**Tools:** Git/GitHub Actions or another approved CI service; Composer/npm lockfiles; PHPUnit; Playwright; PHPStan/Larastan if approved; Laravel Pint; ESLint/TypeScript; dependency/license scanners; container scanner only if images are built.

**Acceptance:** clean build from clean checkout; lockfile enforcement; no unreviewed dependency drift; protected branches and required checks; artifact identity and promotion evidence; CI secrets are scoped and masked.

**Failure/recovery:** failed checks block merge/release; retry only transient infrastructure failures; do not retry deterministic failing tests until root cause is addressed. Revoke compromised runners/tokens and rebuild artifacts from trusted sources.

### P06 — Quality, Security and Compliance Framework Enablement

**Purpose/outcomes:** operationalize common quality and security controls before high-risk domain implementation.

**Entry/dependencies:** P02 requirements and P03 threat/architecture baselines; P05 pipeline.

**Sequence:** establish test pyramid and coverage policy; define code review; threat modelling; security requirements; ASVS control selection; tenant isolation and authorization test suites; privacy/data classification; dependency/license governance; vulnerability SLAs as owner-approved operational policy; secure coding; migration testing; accessibility; performance; backup/restore and incident exercises; evidence retention.

**Deliverables:** quality plan, security plan, control-to-test matrix, vulnerability handling workflow, accessibility strategy, test data policy, compliance applicability register.

**Acceptance:** controls map to requirements and pipelines; critical tenant/security regressions block; legal applicability is evidence-based; incident and vulnerability ownership is assigned.

### P07 — Core Platform and Stable-Core Capabilities

**Purpose/outcomes:** implement, in approved increments, the platform foundations required by all customer-facing domains.

**Scope:** business/tenant boundary, Main Portal and Sub-Portal governance foundations, Organization/Distributor/Shop structural integrity, identity references, Organization-scoped User Accounts, authentication/authorization foundations, User–Business association and scope, Role/Permission/Delegation/Approval support, Subscription/Entitlement separation, Global Business Settings, Platform-wide Services foundations, History/Audit, notification/file/event/job infrastructure as prioritized.

**Entry/dependencies:** P02–P06 accepted for each increment; exact state machines and security requirements ready.

**Sequence:** build invariant enforcement first; add tenant isolation and structural constraint tests; implement identity/account separation; add authorization evaluation without inferring access from ownership or Role; implement subscription/entitlement separately from User permissions and Business Entity lifecycle; add settings governance; add platform service registry/capability controls; add History/Audit and shared platform operations. Each capability has its own state machine and owner.

**Security:** deny-by-default; least privilege; tenant context established and checked server-side; no client-supplied tenant identifier trusted without authorization; immutable audit evidence; outbound No-URL rule tested across all channels.

**Acceptance:** cross-tenant negative tests pass; invalid structures cannot be created; authority and permission remain separate; account boundaries hold; entitlement does not grant permission; stable-core tests are non-configurable and run in CI.

**Failure/recovery:** security invariant failure blocks all downstream domain work. Rollback must not orphan identity/history or weaken tenant isolation; use forward-fix/data repair plans where destructive rollback is unsafe.

### P08 — Shared Domain Contracts and Integration Foundation

**Purpose/outcomes:** create typed, versioned, ownership-preserving contracts and integration mechanisms.

**Entry/dependencies:** P03, P05–P07; contract requirements from P02.

**Sequence:** define canonical identifiers and references; define schema registry/version policy; define synchronous APIs and asynchronous events; establish outbox/inbox/idempotency/correlation; define retries/backoff/dead-letter/manual recovery; define contract compatibility and deprecation; define connector credentials and scopes; test duplicate, reordered, delayed and lost messages; define reconciliation and replay boundaries.

**Acceptance:** contract owner/consumer/permissions/version/provenance explicit; replay is safe; consumer acknowledgement is not treated as business completion; cross-tenant contract tests pass.

**Conditional services:** broker, Redis, schema registry and API gateway are selected only when architecture and load/security requirements justify them.

### P09 — Foundational Business Domain Increments

**Purpose/outcomes:** implement initial independently owned domain foundations in dependency-aware increments.

**Initial order:** Counterparty identity and role profiles; Product/Item catalogue identity; Accounting Contexts/Books/Chart of Accounts foundation. These are not merged into a common master. The exact sequencing among these increments may be parallelized only after their shared identity, tenant and contract dependencies pass.

**Entry:** P07/P08 accepted; domain-specific P02 specs complete; test and migration strategy defined.

**Sequence per Module:** implement its own schema/services/API/UI; validate tenant scope; add domain History; implement states/guards; create contract consumers/producers; unit/integration/negative tests; migration/seed strategy; reconcile with owning domain; publish versioned increment.

**Boundaries:** Counterparty owns scoped Party identity and Customer/Supplier role profiles, not Sales/Purchase terms. Product/Item owns catalogue identity and approved structures, not stock, warehouse, sales, purchase, GST records or accounting. Accounting owns Books, vouchers, postings and balances, not source-domain commercial records.

**Acceptance:** no shared-write master; each Module has independent lifecycle, authorization, History, tests and owner approval.

### P10 — Inventory, Warehouse, Purchase and Supply Operations

**Purpose/outcomes:** deliver supply-side operational capabilities without merging commercial procurement, physical execution and stock ownership.

**Dependencies:** P07/P08; Product/Item and Counterparty references; accounting/tax contracts where required; P02 state machines.

**Recommended increments:**
1. Inventory core: stock identity/context, stock status, stock ledger/movement, reservation/availability, reconciliation and valuation policy hooks.
2. Warehouse core: facilities/locations, Handling Units and physical receiving/inspection/putaway/picking/packing/dispatch tasks.
3. Purchase & Procurement: requisition/sourcing/supplier selection/quotation/agreement/PO/original Purchase Invoice/return/commercial receipt confirmation.
4. Integrate Purchase → Warehouse physical receipt → Inventory accepted stock as independent records with explicit matching/reconciliation.

**Acceptance:** physical receipt does not automatically mean accepted stock; Purchase commercial confirmation does not write Inventory quantities; warehouse task completion does not substitute for Inventory posting; all quantity/UOM and valuation rules are explicit and tested.

**Conditional:** advanced WMS and offline/handheld capabilities are only delivered when approved and applicable.

### P11 — Sales, POS, GST/Tax and Finance/Payment Operations

**Purpose/outcomes:** deliver customer-facing commercial, checkout, tax and payment execution capabilities while preserving independent transaction owners.

**Dependencies:** P07/P08/P09; Product/Item, Counterparty, Inventory/Warehouse as required; applicable GST/Tax and Accounting specifications.

**Recommended increments:**
1. GST/Tax foundations: GST taxpayer/registration records, HSN/SAC registry and assignments, rate applicability rules, tax determination and immutable snapshots; statutory workflows verified at design time.
2. Sales: quotation/offer, agreement, order, fulfilment commitment, original Sales Invoice, returns/credit notes and selling conditions.
3. Finance & Payment: payment initiation/execution, collection/disbursement, provider evidence, settlement, refund/reversal/chargeback and reconciliation.
4. POS Operations: terminals, sessions, operators, tills, checkout traces and session reconciliation; POS must not become owner of original commercial Sales Invoice.
5. Compose Order-to-Cash and POS-to-Financial-Close journeys; reconcile tax, payment, inventory and accounting outcomes independently.

**Acceptance:** Sales owns commercial source records; GST/Tax owns tax determination; Finance/Payment owns payment execution/settlement; Accounting owns posting; Inventory owns stock; POS owns checkout/till/session facts. No single workflow status substitutes for all participating domain states.

**Compliance:** no tax rate, filing deadline, payroll/tax policy or legal interpretation is hard-coded from stale research. Revalidate official GST/RBI/other applicable sources and scope at specification time.

### P12 — HR, Payroll, Asset and Planning Operations

**Purpose/outcomes:** implement people, payroll, operational assets and planning as distinct domains.

**Dependencies:** P07/P08; Accounting, Counterparty, GST/Tax, Finance/Payment contracts as applicable; complete sensitive-data and statutory specifications.

**Recommended increments:**
1. HR: Employee/Employment identity and lifecycle, onboarding, attendance/leave and workforce administration within approved scope.
2. Payroll: independent payroll rules, periods, runs, calculations, payslips, statutory obligations and authoritative payroll results.
3. Asset Management: operational asset identity, custody, assignment, location, transfer, maintenance, inspection, retirement and disposal.
4. Business Planning & Budget: plans, budgets, targets, forecasts, scenarios and baseline revisions; actuals remain owned by source domains.
5. Integrate Payroll → Finance payment → Accounting posting; Asset operational lifecycle → Fixed Asset Accounting; Planning → actuals/variance analysis without ownership transfer.

**Acceptance:** HR owns Employment records; Payroll owns payroll calculations/results; Finance executes payment; Accounting posts; Asset owns operational lifecycle; Accounting owns fixed asset accounting; Planning does not write actual source transactions.

**Risks:** personal/sensitive data, statutory rules, segregation of duties, historical payroll corrections and asset valuation. Legal review and owner approval are mandatory where applicable.

### P13 — Cross-Module Journeys, Reporting and Data Migration

**Purpose/outcomes:** implement and verify end-to-end orchestration and data movement after source-domain contracts are stable.

**Journeys to specify and deliver by applicability:** Order-to-Cash; Procure-to-Pay; Hire-to-Retire; Payroll-to-Pay; Acquire-to-Retire; Return-to-Refund/Credit; POS-to-Financial-Close; Plan-to-Performance; GST Compliance-to-Filing; Counterparty-to-Commercial Relationship; Record-to-Report; customer/provider onboarding and closure; cross-portal exchange where authorized.

**Sequence:** specify orchestrator ownership; define independent domain commands/events; define correlation and expected outcomes; execute happy path and every exception; reconcile financial/tax/stock/accounting records; build reports from owner-approved facts; implement migration extract/map/stage/validate/dedupe/preview/approve/import/reconcile/export; verify counts, totals, provenance and access scope.

**Acceptance:** every participant has a separate completion state; partial success is visible; retries are idempotent; corrections propagate as linked adjustments/reversals; migration is reversible or has a tested compensation/restore plan; reports do not become alternative sources of truth.

### P14 — System Verification, UAT and Operational Readiness

**Purpose/outcomes:** prove that the release candidate meets requirements, architecture, security, performance, compliance applicability and operational support requirements.

**Entry:** integrated release candidate; P02–P13 evidence available for release scope.

**Sequence:** run unit/component/integration/system/E2E/regression/negative tenant tests; verify migration and rollback; threat-model and security test; dependency/license/SBOM checks; load/stress/soak/failure testing; accessibility/usability review; backup restore and DR exercise; business UAT; accounting/tax/stock reconciliation; support runbooks and incident drills; known-defect and risk acceptance.

**Acceptance:** all blocking criteria pass; UAT is signed by accountable business owner; no unresolved critical/high security or data-integrity issue without explicit risk acceptance; observability, support and rollback are operational; legal/statutory verification is current.

**Failure/recovery:** release candidate rejected; defects routed to owning phase/module; rerun impacted regression and dependent journeys; no waiver by schedule pressure.

### P15 — Controlled Production Release and Service Transition

**Purpose/outcomes:** release an approved, immutable artifact to production with monitored and recoverable change.

**Entry:** P14 accepted; release owner authorization; change window, backup, rollback and communications approved.

**Sequence:** verify artifact digest/signature/SBOM; verify environment/configuration; backup and restore checkpoint; apply backward-compatible expand/contract migrations where possible; deploy controlled canary/rolling strategy where supported; run health and synthetic tests; verify tenant isolation; reconcile business-critical records; monitor error/performance/security indicators; decide continue/rollback/forward-fix; record release evidence.

**Acceptance:** release is authorized; deployed artifact matches tested artifact; migrations and smoke tests pass; monitoring and support handover confirmed; no unresolved release blocker.

**Rollback:** application rollback is not automatically database rollback. Use backward-compatible schema strategy and preapproved restore/forward-fix plan. Never delete accepted financial/statutory/history data to force rollback.

### P16 — Operate, Maintain, Evolve, Migrate and Retire

**Purpose/outcomes:** sustain secure, reliable service and provide controlled change, customer/provider migration, portability, offboarding and component/platform retirement.

**Lifecycle domains:**
- Operations: monitoring, alerting, support, incident/problem, capacity, backup, restore, DR and service continuity.
- Maintenance: security patches, dependency upgrades, runtime/database upgrades, compatibility tests, deprecation and configuration drift.
- Evolution: change request, impact analysis, requirements, ADR/spec update, migration, implementation, validation, release and post-release review.
- Data lifecycle: retention, legal hold, archival, restore, export, portability and authorized deletion.
- Customer offboarding: authority/contract checks, access revocation, obligations, export, archive/retention, resource decommissioning and final evidence.
- Sub-Portal migration: target eligibility, scope inventory, transfer plan, identity/data/configuration/file/relationship migration, tenant isolation, cutover, reconciliation and old-provider closure.
- Component retirement: dependency inventory, consumer notice, migration window, data export, credential revocation, decommission and evidence retention.
- Platform retirement: owner-approved business/contract/legal plan, customer notice, portability, continuity, statutory retention and verified final deletion only when lawful.

**Acceptance:** service objectives are measured; patches and upgrades are tested; restore/DR exercises pass; customer export is verifiable; retirement does not destroy records subject to retention/legal hold; all credentials and integrations are revoked when eligible.

## 5. Development environment model

Environments must be logically and operationally separated. Identical infrastructure is not required; compatible major versions, documented configuration differences, reproducible builds and controlled promotion are required.

| Environment | Purpose | Data rule | Promotion/controls |
|---|---|---|---|
| Developer local | Fast isolated development | Synthetic or specifically approved anonymized data only | Individual credentials; disposable services |
| Shared development (optional) | Team integration | Synthetic/non-production data | Protected access; frequent reset |
| CI | Deterministic build and automated checks | Ephemeral fixtures | No production secrets; short-lived tokens |
| Automated test | Repeatable unit/integration/E2E | Generated fixtures | Isolated DB/schema per run; resettable |
| Integration | Real contract/provider integration | Synthetic or masked data | Dedicated credentials and rate limits |
| QA | Functional/regression verification | Approved test data | Controlled release candidate |
| Staging/pre-production | Production-like final verification | Synthetic/masked data | Same artifact/config model; no casual production copy |
| Production | Live service | Production data only | Least privilege, approvals, monitoring, backups |
| Backup/DR | Recovery and continuity | Encrypted protected copies | Separate access/control plane; restore exercises |

### 5.1 Configuration and secrets

- Application configuration is externalized and validated at startup.
- Secrets are not committed to Git, issue trackers, build logs or documentation.
- Use separate secrets per environment and service; rotate/revoke with evidence.
- Certificates have owners, expiry monitoring, renewal and revocation procedures.
- No production secret is reused in development, CI, QA or staging.
- Environment parity means compatible contracts and version baselines, not identical credentials or data.
- Feature enablement must be applicability- and authority-controlled, not hidden in ad hoc environment flags.

### 5.2 Data protection

- Production data is not copied to developer/test environments by default.
- Any approved derived dataset requires legal/privacy/security approval, minimization, masking/tokenization and a documented deletion date.
- Synthetic fixtures must represent edge cases without reproducing real personal or business data.
- Backup access is separate from normal application access.
- Restore is not accepted until integrity, counts, referential constraints, tenant boundaries and application compatibility are verified.

## 6. Dependency-driven Module implementation order

The following is a planning dependency model, not a new ownership decision or a requirement to implement every optional capability.

| Prerequisite | Dependent increment | Relationship | Blocking evidence |
|---|---|---|---|
| P00–P06 governance/toolchain/architecture/quality | All implementation phases | Global prerequisite | Phase-gate evidence |
| Tenant/business structure, identity, authority and access foundations (P07) | All customer-facing Modules | Mandatory Stable Core dependency | Isolation/authorization negative tests |
| Subscription/Entitlement and applicability (P07) | Module/service enablement | Controls availability only; does not grant permission | Entitlement and permission separation tests |
| Platform-wide Services (P07) | Notifications, files, audit, jobs, reporting, API/integration | Shared platform capabilities, not Business Modules | Service scope/owner contracts |
| Shared references/contracts (P08) | All dependent domains | Typed, versioned, owner-preserving | Compatibility and idempotency tests |
| Counterparty | Sales, Purchase, Finance and other Party consumers | Party identity/profile dependency | Identity and role-profile contract |
| Product/Item | Inventory, Warehouse, Purchase, Sales, POS, GST classification consumers | Product identity dependency; does not own consumer facts | Product/Variant reference contract |
| Accounting Context/Books/Chart foundation | Financial postings and reconciliation consumers | Accounting-owned foundation | Posting and Book controls |
| GST/Tax foundations | Taxable Sales/Purchase/Payroll/payment workflows where applicable | Tax determination dependency | Current legal applicability and snapshot tests |
| Inventory | Warehouse, Sales fulfilment, Purchase receiving, POS stock effect | Stock authority dependency | Stock ledger and reservation integrity |
| Warehouse | Physical receipt/putaway/pick/pack/dispatch | Physical execution dependency | Handling/task evidence |
| Purchase | Procure-to-Pay and inbound supply | Commercial procurement dependency | Purchase document lifecycle |
| Sales | Order-to-Cash and customer returns | Commercial sales dependency | Sales document lifecycle |
| Finance & Payment | Payment, refund, settlement, reconciliation journeys | Payment execution dependency | Provider and settlement reconciliation |
| POS | POS-to-Financial-Close | Checkout/session facts | Till/session and Sales/Finance contract |
| HR | Payroll | Employment source dependency | HR employment contract |
| Payroll | Payroll-to-Pay and Accounting | Payroll result dependency | Payroll calculation/review and payment contract |
| Asset Management | Fixed Asset Accounting | Operational asset reference | Asset/Accounting contract |
| Planning & Budget | Plan-to-Performance | Consumes approved plans and source actuals | Versioned baseline and source references |
| All participating domains | Cross-module journeys and analytics | Orchestration/reconciliation | Per-domain completion evidence |

### 6.1 Parallelizable work

Parallel work is permitted only when shared contracts, prerequisites and ownership are stable. Examples:
- Product/Item and Counterparty increments may proceed in parallel after P07/P08 references and tenant boundaries are stable.
- Accounting foundation may proceed in parallel with catalogue work if its own requirements and dependencies are accepted.
- HR and Asset work may proceed in parallel after platform identity, security, and data classification are stable.
- Documentation, test automation, accessibility, threat modelling and operational runbooks continue throughout.
- Domain increments must synchronize at contract version and integration gates; no team may write another Module's authoritative records.

## 7. Cross-phase quality, security and release governance

The following controls are continuous and are not deferred to a final security phase.

| Control | Minimum expectation | Evidence |
|---|---|---|
| Requirements traceability | Requirement → owner → design → code → test → release | Traceability records |
| Threat modelling | Updated for new trust boundary, data flow or privilege | Threat model and mitigation tests |
| Code review | Independent review for protected changes | Review approval and linked checks |
| Automated testing | Unit, integration, system, E2E, regression and negative tests as applicable | Reproducible reports |
| Tenant isolation | Positive and cross-tenant negative tests | Isolation test suite |
| Authorization | Scope, Role, Permission, Delegation and Approval evaluated separately | Authorization matrix/tests |
| Data integrity | Constraints, immutable references, snapshots, reconciliation | Database and domain tests |
| Migration | Forward, rollback/restore/compensation strategy; production-like rehearsal | Migration report |
| Dependencies | Vulnerability, license, provenance and update review | SBOM/scanner report |
| Secrets | No source-control secrets; rotation and incident response | Secret scan and rotation evidence |
| Build provenance | Immutable artifact, digest, source commit and build identity | Artifact manifest/attestation |
| Configuration | Versioned, reviewed, environment-specific, drift-detected | Configuration review |
| Performance | Load, stress, soak and capacity validation against approved NFRs | Benchmark report |
| Accessibility | Automated and manual checks against selected WCAG criteria | Accessibility report |
| Backup/DR | Restore and continuity exercises | Restore/DR evidence |
| Release | Approval, health checks, rollback/forward-fix decision | Release record |
| Operations | Monitoring, incident response, audit and support readiness | Runbooks and exercise records |
| Documentation | Ownership, decisions, cross-references and change history | Documentation review |

### 7.1 Cross-module completion rule

A technically successful workflow or API call is not proof of business completion. A composed journey must record each participating Module's own source outcome, correlation reference, acknowledgement, exception and reconciliation status. A downstream consumer acknowledgement is not a substitute for the source owner's completion evidence.

### 7.2 Change and version management

Every material change must identify affected requirements, architecture decisions, contracts, schemas, data migrations, tests, security controls, documentation, environment configuration, support procedures and customer impact. Breaking changes require a version/deprecation/migration strategy. Accepted records and historical references must not be silently rewritten.

## 8. Phase governance, gates and RACI-style accountability

Roles are functional responsibilities; one person may hold multiple roles in a small team, but approval and execution should be separated for high-risk changes where practical.

| Role | Accountability |
|---|---|
| Project Owner / Product Authority | Business priorities, product policy, risk acceptance, release authorization |
| Domain Owner | Domain requirements, source-of-truth rules, workflow acceptance and reconciliation |
| Business/System Analyst | Requirements, workflows, actors, traceability and acceptance scenarios |
| Solution/Domain Architect | Architecture, contracts, dependencies, ADRs and boundary compliance |
| Engineering Lead | Implementation quality, technical execution and code review |
| Platform/DevOps Engineer | Toolchain, environments, CI, infrastructure, backup and deployment automation |
| Security Lead | Threat model, security requirements, vulnerability triage and security acceptance |
| QA/Test Lead | Test strategy, test evidence, defect status and verification |
| Data/Migration Lead | Data mapping, migration integrity, export, restore and reconciliation |
| Operations/SRE Lead | Monitoring, incidents, capacity, continuity and operational readiness |
| Legal/Compliance Adviser | Applicable statutory/legal interpretation and evidence |
| Release Manager | Release checklist, artifact identity, approvals, cutover and rollback coordination |

No role may approve a business-policy change outside its authority. Legal/Compliance review does not replace the project owner's product decision; project-owner preference does not replace legal advice.

## 9. Risk and decision registers

Maintain separate, dated registers with owner, status, impact, mitigation, evidence and review date.

### 9.1 Key risks

| Risk | Impact | Required mitigation/gate |
|---|---|---|
| Incomplete domain state machines | Rework and inconsistent business behaviour | P02 specification gate per increment |
| Tenant isolation defect | Cross-business data exposure | P07 invariant tests; block release |
| Domain ownership leakage | Duplicate sources of truth and inconsistent records | P03/P08 contract review; owner-specific writes |
| Unverified statutory logic | Filing, payroll or compliance harm | Current official/legal verification before implementation |
| Unsupported package/runtime combination | Build failure/security exposure | P01 compatibility matrix and lockfiles |
| Uncontrolled dependencies | Supply-chain/license risk | Dependency allowlist/review, SBOM and scanning |
| Migration corruption | Loss/duplication of financial or operational records | Rehearsal, checksums, reconciliation and restore |
| Async duplicate/out-of-order events | Double posting or stale state | Idempotency, ordering rules and reconciliation |
| Secret leakage | Account/system compromise | Secret scanning, least privilege, rotation |
| Environment drift | Non-reproducible releases | IaC/manifests and drift detection |
| Performance assumptions | Cost/availability failure | Measured NFR and load tests |
| Overly broad scope | Schedule and complexity risk | Explicit exclusions and owner-controlled scope |
| Unverified backup | False recovery confidence | Periodic restore and DR exercises |
| Unclear operational ownership | Slow incident recovery | Runbooks, on-call and escalation ownership |

### 9.2 Decision classification

Every unresolved item must be labelled as one of:
- Finalized decision (only if present in the Master or explicitly approved and recorded there).
- Recommended technical choice.
- Proposed phase design.
- Pending owner decision.
- Requires official/legal verification.
- Conditional capability.
- Explicitly out of scope.

Do not add a pending decision to the Master. When the owner explicitly finalizes a new business/architecture decision, update the Master first and reconcile dependent documents.

## 10. Maintenance, upgrades and lifecycle continuity

- Maintain a supported-version inventory for OS, language runtimes, framework, database, browser automation, container base images and all direct/transitive dependencies.
- Re-evaluate support status at least at each planned upgrade/release and whenever a security advisory affects the stack.
- Upgrade through a repeatable branch/CI path; run compatibility, migration, performance, security and regression tests.
- Pin dependencies through lockfiles and container digests; update pins through reviewed automation, not floating production tags.
- Keep an SBOM for released artifacts and record licenses and provenance.
- Maintain backup/restore, certificate renewal, secret rotation, incident, DR, export and offboarding runbooks.
- Deprecate APIs/events with consumer inventory, notice, migration guidance and sunset evidence.
- Retire data/components only after authority, retention, legal hold, portability and dependency checks.

## 11. Definition of completion

The Development Phase and Lifecycle Plan is complete only when:
- all phases and their dependencies are documented;
- every phase has the 23-field specification in Section 3.2;
- the first-stage installation runbook has a complete compatibility-checked inventory and validation/recovery procedures;
- environment and promotion rules are defined;
- the module dependency order and parallelization gates are justified;
- quality/security/compliance controls are continuous and testable;
- operational, maintenance, migration, offboarding, archival and retirement lifecycles are addressed;
- open owner/legal decisions are separately classified;
- cross-references and source citations are verified;
- no conflict with the Master exists.

This plan provides a phase architecture and phase-level lifecycle. It does **not** claim that all detailed CBM domain lifecycles are now complete. The separate Lifecycle Reference's Section 16.3 remains authoritative for outstanding domain-level specification gaps.

## 12. Fifteen-point lifecycle/workflow completion standard

Every applicable object lifecycle and workflow must define:

1. Purpose and boundary.
2. Authoritative owner.
3. Actors and authority.
4. Trigger and preconditions.
5. States and transitions, including guards and forbidden transitions.
6. Normal path.
7. Decision branches.
8. Failure and recovery.
9. Cross-domain contracts.
10. Financial/tax/stock consequences.
11. History/audit and immutable evidence.
12. Security and data boundaries.
13. Timing and retention.
14. Applicability and configuration.
15. Completion and reconciliation criteria.

A workflow spanning Modules must preserve independent completion states. No state machine, statutory value, retention duration, approval threshold, security timeout or commercial rule is approved by this development plan unless separately approved in the Master.

## 13. Official research sources

All version-sensitive facts below were checked against official sources on 19 November 2026. Versions are **planning baselines**, not a substitute for a fresh compatibility and security check immediately before installation. Exact patch versions, package hashes, image digests and lockfiles must be captured at execution time.

### 13.1 Lifecycle and engineering standards
- ISO/IEC/IEEE 12207:2026 — [IEEE Standards Association publication record](https://standards.ieee.org/ieee/12207/11416/).
- ISO/IEC/IEEE 24748-1:2024 — [ISO publication record](https://www.iso.org/standard/84709.html).
- ISO/IEC/IEEE 29148:2018 — [ISO publication record](https://www.iso.org/standard/72089.html).
- ISO/IEC/IEEE DIS 29148 Edition 3 — [ISO draft record](https://www.iso.org/standard/94091.html); draft only at this baseline.
- OWASP SAMM — [Model](https://owaspsamm.org/model/).
- OWASP ASVS 5.0.0 — [Official project](https://asvs.owasp.org/).
- NIST SP 800-63B-4 — [Final publication](https://csrc.nist.gov/pubs/sp/800/63/B/4/final).

### 13.2 Platform and toolchain sources
- Ubuntu releases/lifecycle — [Ubuntu release cycle](https://ubuntu.com/about/release-cycle).
- Docker Engine Ubuntu support/install — [Official Docker documentation](https://docs.docker.com/engine/install/ubuntu/).
- PHP support policy — [PHP supported versions](https://www.php.net/supported-versions.php).
- Laravel 13 release/support policy — [Laravel release notes](https://laravel.com/framework/docs/13.x/releases).
- Laravel database support — [Laravel database documentation](https://laravel.com/framework/docs/database).
- Composer — [Official download and verification instructions](https://getcomposer.org/download/).
- Node.js release schedule — [Node.js Release Working Group](https://github.com/nodejs/Release).
- React — [React versions](https://react.dev/versions).
- Vite — [Getting Started and Node requirements](https://vite.dev/guide/).
- Tailwind CSS — [Official documentation](https://tailwindcss.com/docs/installation).
- PostgreSQL — [Versioning policy](https://www.postgresql.org/support/versioning/) and [PostgreSQL 18 documentation](https://www.postgresql.org/docs/18/).
- Playwright — [Installation and system requirements](https://playwright.dev/docs/intro).
- Git — [Official documentation](https://git-scm.com/doc).

### 13.3 Security and operational patterns
- OWASP SAMM and ASVS as above.
- GitHub Actions security hardening — [Official documentation](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions).
- PostgreSQL backup — [Official documentation](https://www.postgresql.org/docs/18/backup.html).
- PostgreSQL client utilities — [Official documentation](https://www.postgresql.org/docs/18/reference-client.html).
- AWS Prescriptive Guidance, transactional outbox — [Pattern reference](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html).
- AWS Prescriptive Guidance, retry with backoff — [Pattern reference](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/retry-backoff.html).

## 14. Research limitations and items requiring revalidation

- The normative ISO standards may require purchase/access. This plan uses their official scope/status records and does not claim clause-level conformance.
- Laravel 13 supports PHP 8.3–8.5 under its published support table. The selected PHP patch and all required extensions must be rechecked against the exact dependency graph.
- PostgreSQL 18 is the approved project baseline. The latest supported 18.x minor must be selected at provisioning; the project must not pin an obsolete minor.
- Node.js release status changes over time. Select an LTS line only; do not use Alpha/Current for production toolchains.
- Vite, React, Tailwind, Playwright and package-manager versions must be pinned by lockfiles and revalidated together.
- Licensing must be checked for the exact package, transitive dependencies, fonts, browser binaries, container images and commercial use context. Do not assume all packages share the same license.
- Exact hardware sizing, availability targets, backup retention, RPO/RTO, vulnerability remediation SLAs, data retention periods, statutory filing rules and legal interpretations require workload evidence or owner/legal approval.
- Optional infrastructure (Redis, message broker, object storage, local mail sink, container orchestration, API gateway) must not be installed merely because it is popular. It requires an accepted architecture need and operational owner.

## 15. Document maintenance and change control

Review this plan whenever a phase gate changes, a major architecture baseline changes, a runtime reaches end of support, a material security advisory affects the stack, a module dependency changes, or an operational/retention/legal obligation changes.

Changes to this plan must:
1. Re-verify the working branch before editing.
2. Preserve the Master authority and approved module ownership.
3. Identify impacted phase specifications, runbooks, README links and history.
4. Record technical rationale and source evidence.
5. Keep pending decisions out of the Master.
6. Revalidate all cross-references and official source URLs.

**End of document.**
