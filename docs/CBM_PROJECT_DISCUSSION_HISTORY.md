# CBM Project Discussion and Architecture Decision History

**Record status:** Non-authoritative chronological context
**Current through:** 19 November 2026
**Authoritative source:** [CBM Master Business and Architecture Decisions](CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md)

## 1. Purpose and authority

This document preserves a concise chronological record of the major project discussions, decision directions, architectural choices, and documentation reconciliations that led to the current CBM business-concept and logical-architecture baseline.

It is not a second Master Decision Document, product roadmap, implementation specification, Domain History, or system Audit Trail. It does not independently approve requirements or change an authoritative decision. If any summary in this record differs from the Master document, the current finalized Master document governs.

The history is selective and decision-focused. It is not a verbatim transcript of every conversation. It records why major boundaries were selected and how the documentation evolved, while leaving the complete normative wording and current rules in the Master document.

## 2. Chronological record

### 17–18 November 2026 — Consolidation of the business and governance foundation

The initial project context and subsequent project-owner clarifications were consolidated into a single business and architecture baseline.

The foundational direction established that CBM is a unified business-management ecosystem, but not one undifferentiated business data model. Main Portal remains the permanent ecosystem governance authority. Sub-Portals are independent Service Provider Businesses and do not become owners of customer businesses. The Customer Business structure remains Organization → Distributor and/or Shop, with a mandatory direct Parent Organization for each Distributor and Shop.

Ownership, legal/professional identity, service-provider relationships, structural placement, business authority, user scope, role, permission, delegation, approval, and subscription entitlement were retained as separate concepts. The baseline also preserved the independent lifecycles for Business Entities, Subscriptions, User Accounts, User–Business Associations, Credentials/Authenticators, and business Mappings.

The project-owner clarifications established a strong Stable Core: strict Business/Tenant Data Isolation, structural integrity, Main Portal supremacy, downward-only authority, ownership integrity, and the prohibition on weakening these controls through configuration, customization, extension, delegation, or override.

User and communication rules were consolidated, including Organization-specific Customer Business User Accounts, separate accounts for the same natural person across Organizations, immutable portal-wide User Codes, the approved initial credential-establishment modes, and the absolute prohibition on outbound URLs and URL-resolving links/QR codes.

### 18 November 2026 — Platform-wide Services and Global Business Settings

The project owner approved the Platform-wide Services Architecture as a distinct architecture layer, with four service scopes, three service category groups, fifteen categories, and the conceptual Category → Service → Capability hierarchy.

The project owner also approved India-only Global Business Settings and their separate governance paths. India-only product scope, INR, IST/Asia/Kolkata, English default, Indian language governance, Indian number grouping/address conventions, metric baseline, Gregorian core calendar, and the default 1 April–31 March Financial Year were captured without transferring statutory or domain-specific ownership to the shared settings layer.

These approvals kept shared platform capabilities and shared defaults distinct from Business Module domain responsibility.

### 19 November 2026 — Accounting and external-system independence

Accounting Management was finalized as an independent Business Module with CBM-owned canonical accounting concepts, Books of Accounts, double-entry by default, Accounting Contexts, accounting controls, posting, reconciliation, financial statements, Fixed Asset Accounting, and Management Accounting.

The default accrual recognition basis was selected for general-purpose Books, subject to the applicable law and Accounting Framework. Tally-specific exchange was kept optional and adapter-based; Tally or another external product was not allowed to define CBM's canonical accounting or business model.

The external-system boundary was clarified: external systems are sources or destinations for selected data exchange. CBM must accept only sufficiently reliable data, identify missing or ambiguous information, and require authorized completion rather than inventing business facts. External customizations, workflows, data structures, or modules do not become CBM architecture requirements merely because an external system supports them.

### 19 November 2026 — Product/Item, Pricing, GST/Tax, Inventory, and Warehouse boundaries

Product/Item Management was finalized as an independent Business Module. Its approved canonical structures remained Product Definition, Product Variant, Product/Item Instance, Product Family/Group/Classification, Trade Packaging Configuration, and Product Bundle/Kit. Product/Item identity remained distinct from stock, warehouse placement, price, tax determination, transactions, and individual physical occurrences.

The Price Reference and Price Determination model was retained as a shared domain contract rather than a separate Pricing Module or universal Price Master. Product/Item declared prices, Sales selling prices, Purchase supplier prices, Inventory cost/valuation, GST/Tax determination, Finance payment facts, and Accounting values retained their distinct owners.

HSN/SAC Registry, classification assignment, GST Rate Applicability Rules, and GST Tax Determination were separated. The six-digit minimum HSN/SAC rule was retained as a CBM Stable Core invariant, not represented as a universal statutory minimum. Full official codes must be preserved; codes must not be truncated, padded, or fabricated. Accepted transactions preserve immutable Tax Determination Snapshots.

Inventory and Warehouse were kept as separate independent Modules: Inventory owns stock quantities, states, movements, reservations, reconciliation, and valuation; Warehouse owns facilities, storage layout, Handling Units, warehouse tasks, and physical execution.

### 19 November 2026 — Domain decomposition and the fourteen-Module Catalogue

The project owner clarified that related information must not be collapsed into one model or module merely because it concerns the same real-world subject. Domain separation must follow genuine differences in responsibility, authoritative owner, lifecycle, applicability, permissions, validation, history, or change requirements. It must not become indiscriminate fragmentation: cohesive facts with the same owner and lifecycle may remain together.

The project owner then delegated completion of the remaining Business Module classification and subordinate architecture decisions wherever no indispensable project-owner business-policy choice remained. The resulting catalogue was finalized as fourteen independent Business Modules:

1. Accounting Management.
2. Product/Item Management.
3. Inventory Management.
4. Warehouse Management.
5. Human Resource Management.
6. Payroll Management.
7. Sales Management.
8. Purchase & Procurement Management.
9. Point of Sale (POS) Operations.
10. GST/Tax Management.
11. Finance & Payment Management.
12. Asset Management.
13. Business Planning & Budget Management.
14. Counterparty Management.

The classification was based on independent business responsibilities and source-record ownership, not on the number of screens, workflows, data tables, or application clients. Module availability remains applicability-controlled; the Catalogue does not require every business to enable all fourteen Modules.

The following structural choices were made to keep related responsibilities together without merging unrelated ownership:

- Purchase and Procurement remain one independent Module; sourcing, supplier selection, orders, invoices, and commercial receipt confirmation remain together, while physical receiving belongs to Warehouse and accepted stock belongs to Inventory.
- POS Operations is an independent business responsibility, not merely a POS application or screen. POS owns terminal/session/till/checkout operational facts; Sales owns the original commercial sale.
- Finance & Payment remains one independent Module with Cash/Bank Operations and Payment Operations subdomains. Operational payment execution/settlement remains separate from Accounting balances and postings.
- Asset Management owns operational Asset identity and lifecycle. Accounting retains Fixed Asset Accounting.
- Business Planning & Budget Management owns plans and budgets, not actual source transactions or posted accounting facts.
- Counterparty Management owns scoped Party identity and Customer/Supplier role profiles. Sales and Purchase retain their respective commercial terms and transactions.
- GST/Tax is an independent Business Module. Its tax-domain records do not merge with Product, Sales, Purchase, Payroll, Inventory, Finance, or Accounting source records.

The service-classification boundary was also clarified: a Service Offering or Service/Supply Subject used for SAC classification is supplied by its authoritative service domain. It must not be forced into a physical-goods Product Variant merely to satisfy tax classification. Product/Item may support governed catalogue representations where appropriate, but that does not transfer ownership of service performance or the SAC subject to Product/Item Management.

## 3. Documentation review and reconciliation — 19 November 2026

A consistency review was performed after the initial Module Catalogue was recorded.

The review identified and corrected the following documentation defects without changing the approved business concepts:

1. The earlier Business Module summary in Section 6D.3 still described eight now-finalized domain responsibilities as awaiting future classification. It was replaced with a fourteen-Module summary aligned to Section 6I.
2. Section 6H.1 still described the GST/Tax Module classification as not yet finalized. It was updated to reflect the final independent Module classification.
3. Section 6I had initially been appended after the final-document marker. It was moved to its logical location after Section 6H and before Section 7. The Table of Contents was updated, and invalid heading anchors were corrected.
4. Section 17A's Domain History catalogue did not yet include all the History domains required by the newly finalized Modules and already specified source domains. It was expanded to twenty-five separate owner-specific History record sets. The comprehensive Audit Trail examples and paired-record examples were updated accordingly.
5. Canonical definitions 3.131–3.166 were added for Sales, Purchase, POS, payment, asset, planning, counterparty, GST compliance, and Service Offering/Supply Subject concepts so that related terms are not treated as interchangeable.
6. Cross-concept invariants 200–217 were added to summarize the fourteen-Module Catalogue and its most important ownership, lifecycle, applicability, and composition boundaries.
7. Product/Item wording was refined to distinguish governed service-related catalogue representation from the authoritative Service Offering/Supply Subject and service-delivery lifecycle.
8. README was reconciled to the Master Catalogue, owner boundaries, current documentation stage, and documentation-only authorization boundary.

The review also checked the official GS1 publication status. The official archive identified Version 26.0.0 as the latest published consolidated General Specifications edition as of the baseline date. Ratified changes intended for Version 27.0 were recorded separately from the currently published edition; future change notices are not to be treated as already incorporated in the published standard.

## 4. Current authoritative references

- [Master Business and Architecture Decisions](CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md) — the complete authoritative finalized baseline.
- [README](../README.md) — the repository overview and concise architecture summary.

The Master document's Section 22 contains the source basis and decision provenance for the current baseline. Its Section 21 governs documentation completeness, separation of Project Discussion History from Domain History/Audit Trail, downstream consistency, and change control.

## 5. Change-history rule

When a new project-owner decision is finalized, update the Master document first and reconcile dependent documentation. Then append a dated entry here describing the decision context, the selected architectural direction, the rationale/boundary, and the Master sections affected.

Do not record an unapproved proposal as a decision. Do not use this file to override the Master document. Do not place Domain History or operational Audit Trail events in this project-level history record.

This history records architectural discussion and documentation evolution only. It does not authorize application implementation, code changes, database changes, infrastructure changes, or deployment.

## 6. Lifecycle and workflow completeness audit — 19 November 2026

A repository-wide review of the current branch was performed specifically to test whether the project had documented the lifecycle of every major object and the independent workflow of every business activity. The review covered the Master document's ecosystem/identity/authority sections, all fourteen Business Modules, Platform-wide Services, settings/configuration, approvals, integrations, migration, offboarding, archival, Domain History and Audit Trail.

The review found that the Master baseline contains substantive lifecycle rules for Business Entities and parent-caused cascades, Subscription states and billing separation, User Account and initial credential establishment, Distributor–Shop two-way mapping, selected Product/Variant and Price Record versioning, Warehouse task states, Accounting Voucher/posting controls, GST registration/classification/rate snapshots, and data migration/archival safeguards. However, many of these remain partial rather than complete operational specifications.

The review also found that detailed end-to-end workflows are still missing or incomplete for several domains, notably Sales, Purchase & Procurement, Inventory, HR, Payroll, POS offline recovery, GST return preparation/filing, Finance payment execution/settlement, Asset Management, Planning/Budget, Counterparty duplicate merge/split, Platform-wide service operations, notification delivery, integration/event recovery, and full customer onboarding/offboarding. Cross-Module journeys such as Order-to-Cash, Procure-to-Pay, Hire-to-Retire, Payroll-to-Pay, Acquire-to-Retire, Return-to-Refund, and Record-to-Report are not yet fully specified as complete stateful workflows.

A separate [CBM Lifecycle and Workflow Completeness Audit](CBM_LIFECYCLE_AND_WORKFLOW_COMPLETENESS_AUDIT.md) was added to the branch. It records 52 independently scoped lifecycle/workflow entries, maps the fourteen Modules separately, distinguishes Defined/Partial/Missing/Conditional maturity, provides conceptual sequence maps derived from existing ownership decisions, and establishes a fifteen-point minimum completion standard. It explicitly states that the project documentation is **not yet complete at detailed lifecycle/workflow specification level**. The Master document was not changed by this audit, because this catalogue is a discovery/gap-assessment artefact and does not approve new business policies or state machines.
