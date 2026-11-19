# CBM Domain Lifecycle and Workflow Reference

**Document date:** 19 November 2026  
**Repository branch:** `architecture/domain-architecture-continuation-2026-11-19`  
**Language:** English  
**Document type:** Independent supporting architecture reference  
**Authority relationship:** Supplementary to, and not a replacement for, the CBM Master Business and Architecture Decisions document.

## Document purpose

This document consolidates the project's identified domain lifecycles, independent Business Module workflows, platform-wide operational lifecycles, cross-module business journeys, and the criteria required to determine whether lifecycle/workflow documentation is complete. It is organized as a standalone reference so that each process can be reviewed independently while its interactions with other domains remain explicit.

The document is intentionally broader than a list of happy-path steps. It distinguishes object lifecycles from business workflows, module-owned processes from composed cross-module journeys, and approved rules from sequence maps that still require detailed specification.

## How to use this document

1. Read Sections 1–4 for scope, terminology, status meanings, and ecosystem/platform lifecycles.
2. Read Section 5 to inspect each of the fourteen Business Modules independently. Do not infer shared ownership merely because a workflow crosses modules.
3. Read the cross-module journey section to understand orchestration and completion evidence across domain boundaries.
4. Read the data, migration, portability, offboarding, and audit sections for platform-wide lifecycle obligations.
5. Use the completion standard and status register to identify what remains to be specified before declaring the documentation complete.

## Governing principles

- The Master Business and Architecture Decisions document is authoritative for finalized business concepts, ownership boundaries, invariants, and approved rules.
- This document is a supporting lifecycle/workflow catalogue and gap assessment. It does not approve new business policies, state machines, approval thresholds, statutory interpretations, retention periods, or implementation designs.
- Every domain remains authoritative for its own records and lifecycle. A cross-module journey coordinates independent domain outcomes; it does not merge their records or transfer ownership.
- A state in one domain must not be treated as the state of another domain. Sales Orders, Warehouse Tasks, Inventory Movements, Payments, Tax Determinations, and Accounting Postings have independent states.
- Where an exact rule is not established in the Master document, the sequence is labelled as requiring specification. It must not be treated as an approved operational rule.
- Applicability, configuration, subscription/entitlement, business context, authorization, and tenant boundaries must be evaluated independently where relevant.
- No implementation, database, infrastructure, or deployment authorization is created by this document.

## Contents and organization

- **Part I — Scope and lifecycle model:** definitions, status key, and reading rules.
- **Part II — Ecosystem, identity, authority, and platform lifecycles:** Main Portal, Sub-Portals, Business Entities, identities, ownership, users, access, settings, services, approvals, notifications, documents, APIs, events, and integrations.
- **Part III — Independent Business Module workflows:** all fourteen approved Business Modules, each with separately identified object lifecycles and workflows.
- **Part IV — Cross-module business journeys:** end-to-end orchestration while preserving each module's ownership and completion state.
- **Part V — Data and operational lifecycle:** onboarding, migration, export, offboarding, archival, restoration, history, and audit.
- **Part VI — Completeness assessment:** maturity definitions, known gaps, and minimum completion standard.

---

# CBM Domain Lifecycle and Workflow Reference

**Review date:** 19 November 2026  
**Branch:** `architecture/domain-architecture-continuation-2026-11-19`  
**Document status:** Consolidated lifecycle/workflow reference and completeness assessment; not a finalized implementation specification  
**Authority:** The Master Business and Architecture Decisions document remains the sole authoritative source for approved business concepts and ownership boundaries.

## 1. Purpose and conclusion

This document inventories the independently governed lifecycles and end-to-end workflows identifiable in the current CBM architecture baseline. It separates:
- **Object/domain lifecycle:** the life of one entity, relationship, configuration, record, or operational object.
- **Business workflow:** the sequence of work that creates or changes one or more independently owned domain records.
- **Cross-Module process:** a composed workflow that crosses ownership boundaries without merging them.
- **Platform/operational lifecycle:** the life of shared services, integrations, data, security, and governance capabilities.

The current Master document establishes important lifecycle boundaries and selected detailed state rules. It does **not** yet define every workflow and lifecycle to the level needed for a complete downstream functional specification. This audit therefore does not declare the project documentation complete.

The flows below are architecture-level discovery maps derived from the approved module responsibilities and existing rules. Where the Master document has not approved exact states, approval thresholds, timing, or exception policy, this catalogue deliberately labels the flow as a **sequence to be specified**, not as an already-approved state machine. No new flow in this document transfers ownership or changes the original concepts.

## 2. Reading key

- **Defined:** The Master document contains a substantive lifecycle or sequence with meaningful rules.
- **Partial:** Responsibility and some stages/states are defined, but the complete lifecycle, transition conditions, or exception paths are not.
- **Missing:** The concept is named or its responsibility is assigned, but its own end-to-end lifecycle/workflow is not yet documented.
- **Conditional:** The capability is optional/applicability-controlled; its detailed workflow is required only when enabled.

A state named in one domain must not be reused as if it were the state of another domain. For example, a Sales Order, Warehouse Task, Inventory Movement, Payment, Tax Determination, and Accounting Posting each have separate states.

## 3. Ecosystem, business structure, identity, and authority lifecycles

### 3.1 Main Portal ecosystem governance lifecycle — Partial
**Sequence to specify:** governance policy/rule proposed → impact and compatibility assessment → authorized approval → effective-dated publication → applicable Sub-Portals/business contexts receive the governed baseline → compliance monitoring → amendment/supersession → retirement while preserving historical applicability.

**Unspecified:** governance change request types, mandatory review/approval paths, emergency changes, notice periods, effective-date handling, exception/waiver process, rollback/supersession, and policy-version adoption.

### 3.2 Sub-Portal Service Provider lifecycle — Partial
**Sequence to specify:** provider application/creation → legal/provider identity verification → Main Portal authorization → provider configuration/provisioning → service activation → customer onboarding and service operations → provider change or continuity event → migration/transfer of service-provider relationship → closure/offboarding.

The Main Portal → Sub-Portal relationship is not the Customer Business Organization → Distributor/Shop hierarchy. Provider migration must not transfer Customer Business ownership or tenant data rights.

**Unspecified:** provider application states, approval evidence, activation checks, provider suspension/termination, customer-impact handling, service-level transition, and final provider offboarding.

### 3.3 Customer Business Entity lifecycle — Defined
**Existing flow:** Draft/creation with mandatory structural Parent where applicable → verification where required → Active → authorized Suspended/Deactivated/Closed transitions → controlled reactivation where permitted → separate archival.

Organization suspension/deactivation has defined child Distributor/Shop cascade rules. Child suspension/deactivation does not cascade to its Parent or siblings. Multi-entity operations require an immutable pre-operation snapshot, idempotent execution, concurrency protection, failure recovery, and exact restoration of only objects changed by the original operation. Organization closure requires children to be individually Closed and applicable mappings formally resolved.

**Existing operational states:** Draft, Pending Verification where applicable, Active, Suspended, Deactivated, Closed. Archived is not an operational entity state.

**Remaining detail:** full creation/verification checklist by entity type, exact transition authorization matrix, closure checklist by entity type, exceptional reopening, and operational evidence required for each transition.

### 3.4 Structural Parent–Child relationship lifecycle — Partial
**Sequence to specify:** create child with exactly one eligible direct Parent Organization → validate structural and ownership invariants → activate relationship → maintain governed parent reference → handle Parent-caused suspension/deactivation cascade → close child → preserve structural history.

The approved structure is Organization → Distributor and/or Shop. Distributor–Shop Mapping is not a structural edge.

**Unspecified:** whether and under what exceptional conditions a child can change Parent Organization, transfer preconditions, approval, atomicity, historical parent references, and treatment of dependent subscriptions, registrations, users, mappings, and accounting contexts.

### 3.5 Business Identity lifecycle — Missing
**Sequence to specify:** identity creation → uniqueness and ownership-context validation → activation/use → correction or replacement through governed identity event → suspension/block where justified → retirement while preserving references and historical transaction snapshots.

The Business Identity's exact states, uniqueness scope, correction process, merge/split prohibition or exception, and retirement conditions are not defined as a complete lifecycle.

### 3.6 Legal Identity lifecycle — Missing
**Sequence to specify:** legal-person/entity evidence capture → validation → association with eligible Business/Accounting/GST contexts → effective-dated legal particulars change → legal status change → succession/reorganization event where supported → retirement while preserving statutory history.

Legal Identity must remain distinct from Professional Identity, Business Identity, Owner ID, and GST Registration. Exact evidence, verification, legal-successor and restructuring workflows require specification.

### 3.7 Professional Identity lifecycle — Missing
**Sequence to specify:** professional identity creation → association with a Legal Identity under governed evidence → operational use in an Organization/Distributor/Shop context → name/trade-style/operating particulars change → suspension/retirement → preservation of historical references.

One Legal Identity may support multiple Professional Identities. The exact lifecycle and permitted changes of each Professional Identity are not yet specified.

### 3.8 Ownership Link / Ownership ID lifecycle — Partial
**Sequence to specify:** ownership claim/proposal → evidence and identity validation → authorized approval/verification → effective-dated Ownership Link activation → ownership change/release → correction/dispute handling → historical preservation.

Ownership is separate from authority, structure, mapping, GST registration, and User scope. The lifecycle is named in Section 13 but the full state model, evidence rules, ownership-change workflow, and dispute/succession process remain incomplete.

### 3.9 Business Available Authority Scope lifecycle — Partial
**Sequence to specify:** scope eligibility derived from governed business/ownership relationships → calculate available scope → effective-dated scope change → invalidate/recalculate dependent availability → preserve reason and provenance.

Available authority is not Permission. Exact recalculation triggers, cached-scope invalidation, exceptional scope restrictions, and review workflow need specification.

### 3.10 User Assigned Authority Scope / User–Business Association lifecycle — Partial
**Sequence to specify:** authorized User assignment → scope validation against Business Available Authority Scope → activation → scope extension/restriction → suspension/revocation → review and history.

The User–Business Association lifecycle is explicitly separate from User Account and Credential lifecycles, but its full states, assignment/approval sequence, expiry/revalidation, and emergency revocation are not yet fully defined.

### 3.11 Role, Permission, Delegated Authority, and Approval lifecycle — Partial
**Separate flows:**
1. Role definition/version → approval/publication → assignment → revision/retirement.
2. Permission definition/governance → assignment through eligible Role/User/context → grant/revoke → effective access recalculation.
3. Delegation proposal → authority validation → approval → effective period → use/limit monitoring → expiry/revocation.
4. Approval policy definition → publication → invocation by a business action → decision → action continuation/block → evidence/history.

Approval supports single, multiple, sequential, and parallel patterns. The generic governance is defined, but complete lifecycle/state machines for Role, Permission, Delegation, and each approval instance are not.

## 4. Platform-wide, settings, and shared capability lifecycles

### 4.1 Platform-wide Service and Capability lifecycle — Missing
**Sequence to specify:** service/capability proposal → scope/category/owner classification → governance approval → versioned definition → enablement for eligible scope → configuration/operation → deprecation notice → migration/replacement → retirement.

The four service scopes, three groups, fifteen categories, and Category → Service → Capability classification are established; the lifecycle of each service/capability is not.

### 4.2 Global Business Settings lifecycle — Partial
**Sequence to specify:** governed default/rule definition → Main Portal approval → publication/version → applicability selection → inheritance/resolution → authorized local configuration/override where allowed → effective-date change → supersession/retirement.

India-only, INR-only, IST, language, number/date/address, unit, and Financial Year boundaries are defined. Detailed setting-by-setting approval, effective dating, migration, rollback, and conflict-resolution workflows are not.

### 4.3 Configuration, inheritance, customization, and override lifecycle — Partial
**Sequence to specify:** configuration proposal → applicability/authority validation → protected-invariant validation → conflict/precedence resolution → approval where required → versioned publication → effective use → change/rollback/supersession → audit/history.

Exact precedence and conflict rules for all configuration families are not fully specified. No configuration may weaken tenant isolation, security, ownership, statutory, accounting, tax, or other protected invariants.

### 4.4 Approval instance lifecycle — Partial
**Sequence to specify:** business action requests approval → policy and approver resolution → sequential/parallel/single/multiple steps → approve/reject/return/withdraw/expire as policy permits → revalidation of action preconditions → authorized continuation or final block → immutable decision evidence.

Exact common states, delegation during approval, approver unavailability, quorum, timeout, escalation, resubmission, policy changes during an active approval, and cancellation are not fully specified.

### 4.5 Notification and messaging lifecycle — Missing
**Sequence to specify:** domain event/request → recipient and channel eligibility → template/version resolution → content/security validation → queue → delivery attempt → provider response → retry/backoff or terminal failure → delivery evidence/reconciliation → retention.

The absolute No-URL rule applies to every outbound channel. Delivery status must not be mistaken for completion of the originating business transaction.

### 4.6 Document/file/object lifecycle — Partial
**Sequence to specify:** file/document creation or receipt → type/owner/scope validation → malware/content/security checks where applicable → metadata and version assignment → authorized storage → link to source record → access/download/export → retention/hold → archival → restore or authorized deletion.

Object-storage abstraction, encryption, manifests, export and restore requirements are established. Per-file states, versioning, quarantine, legal hold, retention classes, deletion approval, restore verification, and failed-archive recovery remain incomplete.

### 4.7 Search, reporting, analytics, and background processing lifecycle — Missing
**Separate flows to specify:** report definition/version/publication; report execution/snapshot/refresh; search indexing/update/deletion; background job submission/queue/claim/retry/dead-letter/cancel/complete; scheduled job change/disable; analytics data freshness and reconciliation.

These are Platform Capabilities and must not become alternative authoritative business records.

### 4.8 API, event, and integration contract lifecycle — Partial
**Sequence to specify:** contract proposal/schema → ownership and version compatibility review → authorization/data-scope definition → publication/registration → consumer onboarding → message/API execution → idempotency and correlation → retry/reconciliation/deprecation → consumer migration → retirement.

The ownership and versioned-contract principles are defined. Complete contract versioning, compatibility, event delivery guarantees, dead-letter recovery, replay, consumer migration, and revocation workflows are not.

### 4.9 External connector/provider lifecycle — Missing
**Sequence to specify:** provider/connector selection → due diligence and data/security scope → credential setup → connection validation → mapping/version configuration → activation → health monitoring → credential rotation → suspension/disconnection → data reconciliation/export → retirement.

Provider failure must not corrupt or silently alter CBM source records.

## 5. Business Module workflows — each Module separately

### 5.1 Accounting Management

**A. Accounting Context and Book lifecycle — Partial**  
Legal Identity and accounting purpose validation → Accounting Context creation → Book creation and accountable owner assignment → framework/policy/effective-period setup → Chart of Accounts and Ledger setup → opening balances or migration → operational use → period close/reopen under approval → Book suspension/retirement and archive.

**B. Accounting Voucher-to-Posting workflow — Partial**  
Source/manual/migration evidence received → Book/period/Voucher Type resolution → Draft Voucher and entry lines → account/source/tax/amount validation → debit-credit balance validation → approval where applicable → Posted Voucher → Journal Entry → Ledger Posting → sub-ledger/outstanding allocation → reconciliation → financial reporting.

**C. Correction/reversal workflow — Partial**  
Correction request/evidence → original posted record lock/reference → determine reversal/adjustment/correction method and effective date → approval → new linked Voucher → posting → reconcile original and correcting entries → preserve both histories.

**D. Accounting Period close/reopen — Partial**  
Period readiness checks → source posting completeness → sub-ledger/control account reconciliation → outstanding/valuation/tax/payment checks → close approval → lock period → opening balance carry-forward → controlled reopen request/approval → adjustment/reclose.

**E. Reconciliation workflow — Partial**  
Select two authorized source sets → establish scope/period/matching keys → match → classify timing differences/omissions/duplicates/unmatched/amount variance → investigate → authorized source correction or Accounting adjustment → re-run → approve/close reconciliation.

**F. Accounting migration/onboarding — Defined at stage level**  
Source inspection → mapping → staging → validation → duplicate detection → exceptions → preview → reconciliation → approval → acceptance → post-migration verification. Exact source adapters and detailed mapping are not finalized.

**G. Accounting export/auditor package — Partial**  
Scope and authority validation → Book/period selection → data extraction → provenance/count/exception manifest → consistency check → authorized export → recipient/access record → retention.

### 5.2 Product/Item Management

**A. Product Definition lifecycle — Partial**  
Create Product Definition → classify/group where applicable → create one or more Variants (or governed standard/default Variant) → attach applicable attributes/specifications → assign identifiers → configure packaging/bundles → validate → publish/share → amend through versioned change → block/discontinue/supersede → retain historical references.

**B. Product Variant lifecycle — Partial**  
Create under Product Definition → configure variant dimensions/attributes → assign scoped identifiers → establish applicable packaging/price/tax references → publish → change/version → block/discontinue/supersede → preserve stock/return/historical references.

**C. Product/Item Instance lifecycle — Missing**  
Instance/serial/asset-like occurrence creation or recognition → identity/serial/batch association → custody/stock/use state → transfer/repair/return/retirement where applicable → history. The exact division among serialised Inventory, Asset Management, warranty/service, and instance identity needs a detailed specification.

**D. Identifier lifecycle — Partial**  
Identifier proposal/import → issuer/namespace/type/subject/uniqueness validation → authoritative assignment → use/resolution → correction/remapping → retirement/supersession → provenance/history. GTIN/GS1 assignment must follow the issuing authority; never invent identifiers.

**E. Packaging/Bundle/Kit lifecycle — Missing**  
Configuration draft → component/quantity/UOM validation → packaging hierarchy and trade-use validation → approval/publication → effective use → revision/supersession → preserve historical transaction and stock-unit references. Physical bundle, virtual offer, configurable bundle, kit, and Warehouse Handling Unit remain distinct.

**F. Catalogue publication/sharing lifecycle — Partial**  
Owner creates/maintains catalogue → validate publication scope → authorize recipient/business context → publish version → recipient consumes under permitted rights → local override where authorized → revoke/unpublish/supersede → preserve source and recipient history.

### 5.3 Inventory Management

**A. Stock receipt lifecycle — Partial**  
Purchase/transfer/return/other authorized source → expected receipt reference → Warehouse physical receiving/inspection where used → Purchase commercial discrepancy/acceptance where applicable → Inventory acceptance decision → stock movement/ledger entry → stock status/availability update → valuation/cost update → reconciliation.

A supplier delivery or Purchase receipt confirmation alone must not increase authoritative Inventory stock.

**B. Stock issue/fulfilment lifecycle — Missing**  
Authorized demand/order → availability check → reservation/allocation → Warehouse pick task → pick confirmation → Inventory issue/stock movement → source fulfilment confirmation → reconciliation.

**C. Reservation lifecycle — Missing**  
Reservation request → eligibility/available-to-promise check → reserve quantity → partial consume/release/expiry/cancel → update availability → reconcile against source order and stock.

**D. Stock transfer lifecycle — Missing**  
Transfer request → source/destination eligibility → approval → source reservation/dispatch movement → in-transit custody/status → destination receiving/acceptance → destination movement → discrepancy/loss resolution → close.

**E. Stock adjustment/reconciliation lifecycle — Partial**  
Cycle/physical count or discrepancy → freeze/scope/count evidence → compare book and physical quantities → investigate → approve adjustment → post Inventory movement/valuation effect → reconcile → preserve evidence.

**F. Batch/Lot/Serial/Expiry lifecycle — Missing**  
Batch/serial creation or receipt → identity/attributes/expiry association → available/held/quarantined/reserved/issued/returned/disposed transitions as applicable → recall/expiry/traceability handling. Exact stock states and serial/batch transition matrix are not approved.

**G. Inventory valuation lifecycle — Partial**  
Accepted stock movements and attributable cost facts → costing policy/version → cost component allocation → valuation calculation → reconciliation → period valuation snapshot → authorized revaluation/correction → accounting reference/posting.

### 5.4 Warehouse Management

**A. Warehouse Facility and Location lifecycle — Partial**  
Facility registration → ownership/operator/custody/location validation → layout/zone/section/rack/shelf/bin definition → activation → capacity/status change → closure/decommissioning → historical location references.

**B. Warehouse Work Task lifecycle — Defined at stage/state level**  
Task request from Purchase/Sales/POS/Inventory/return/transfer/manual action → plan → assign → in progress → partial completion/completion OR cancellation/failure/exception → evidence/confirmation → Inventory impact through explicit contract → reconcile. Task states may include requested, planned, assigned, in progress, partially completed, completed, cancelled, failed, exception.

**C. Receiving and putaway workflow — Partial**  
Expected receipt → unload/identify → count/inspect → discrepancy/damage/hold decision → putaway plan → location/Handling Unit assignment → physical putaway confirmation → Inventory acceptance/stock state update.

**D. Pick/pack/dispatch workflow — Partial**  
Authorized fulfilment request → task planning → pick → pick confirmation → pack/Handling Unit formation → staging → load → dispatch confirmation → source order update; Inventory remains authoritative for stock movement.

**E. Cycle count and physical inventory workflow — Partial**  
Count plan → location/item scope → freeze or controlled concurrent movement policy → physical count → variance review → recount/approval → Inventory adjustment → reconcile.

**F. Handling Unit lifecycle — Missing**  
Create/identify → contents/quantity association → close/seal → move/receive/dispatch → split/merge/repack under controlled rules → empty/retire. Handling Units are not Product Variants or trade packaging configurations.

### 5.5 Human Resource Management

**A. Employee identity and employment lifecycle — Missing**  
Workforce need → recruitment/selection capability if enabled → candidate assessment → offer/acceptance → Employee record creation → employment agreement/verification → onboarding → active employment → assignment/department/job/compensation changes → leave/attendance/performance/disciplinary records where applicable → separation decision → notice/exit clearance → employment termination → HR archive/retention.

Recruitment and performance capabilities are not yet approved as mandatory feature scope. Employee remains distinct from User Account.

**B. Employee assignment lifecycle — Missing**  
Assignment request → position/department/location/manager eligibility → approval → effective-dated assignment → transfer/change → end/reassignment → history.

**C. Attendance and leave workflow — Missing**  
Policy/calendar setup → attendance capture/import → exception correction → leave request → eligibility/balance check → approval/rejection → attendance/payroll impact → reconciliation. Applicable policy and statutory rules require later validation.

**D. HR separation/offboarding workflow — Missing**  
Separation initiation → notice/approval → handover and clearance → asset/custody return → revoke User–Business associations/access through separate identity workflow → final HR status → payroll final settlement input → archive and retention. HR does not directly own User Account credentials or payment execution.

### 5.6 Payroll Management

**A. Payroll policy/rule lifecycle — Missing**  
Rule proposal → legal/jurisdiction/applicability validation → approval/version → effective date → payroll-period use → amendment/supersession → preserve historical run basis.

**B. Payroll Period and Run lifecycle — Missing**  
Period open → eligible Employee/workforce snapshot → approved HR attendance/leave/compensation inputs → variable earnings/deductions/benefits inputs → statutory rule version selection → calculation → validation/exception resolution → review/approval → finalization/lock → payslip issuance → payroll liability records → payment instruction to Finance → accounting posting request → reconciliation → close.

**C. Payroll correction/re-run workflow — Missing**  
Correction request → identify affected period/Employee/input/rule → impact calculation → authorization → adjustment or controlled rerun → revised result/payslip/correction record → Finance and Accounting adjustments → preserve original and corrected run history.

**D. Payroll payment workflow — Cross-Module, Missing in detail**  
Finalized Payroll payable → Finance payment batch/instructions → authorization/execution → success/failure/partial payment → settlement/reconciliation → Accounting posting → Payroll payment status update through contract. A payroll calculation or payslip does not prove payment.

### 5.7 Sales Management

**A. Sales quotation/offer lifecycle — Missing**  
Customer/Counterparty selection → product/service reference → price/conditions determination → tax preview/determination as required → quotation draft → validation/approval → issue → customer acceptance/rejection/expiry → convert to agreement/order or close.

**B. Sales agreement/commercial terms lifecycle — Missing**  
Negotiation/draft → credit/authority/price/term validation → approval → effective agreement → amendment/renewal/suspension/termination → preserve version and transaction references.

**C. Sales Order lifecycle — Missing**  
Order creation from accepted offer/agreement or direct authorized order → customer/line/price/tax/credit/availability checks → approval → confirmation → reservation/fulfilment request → partial/full fulfilment → invoice issue → completion/closure OR cancellation/backorder/exception.

**D. Sales fulfilment lifecycle — Partial across Sales/Warehouse/Inventory**  
Fulfilment commitment → Inventory availability/reservation → Warehouse pick/pack/stage/load/dispatch → Inventory issue → delivery/acceptance evidence → Sales fulfilment status update → exception/shortage/return handling.

**E. Sales Invoice lifecycle — Missing**  
Invoice preparation from eligible sale/fulfilment → counterparty/legal/GST registration validation → price and immutable tax determination snapshot → document validation/numbering → authorization → issue → receivable reference → payment status consumed from Finance → correction/credit/debit/void only through legally permitted process → preserve issued invoice.

**F. Sales Return and commercial adjustment lifecycle — Missing**  
Return request → original invoice/line validation → eligibility/condition/quantity review → approval → physical return to Warehouse → Inventory receipt/inspection/stock disposition → GST treatment → Sales credit/debit/refund obligation → Finance refund/settlement → Accounting posting → close.

### 5.8 Purchase & Procurement Management

**A. Purchase Requisition lifecycle — Missing**  
Need identified → requisition draft → item/quantity/required date/budget reference → policy/authority validation → approval → RFQ/quotation or direct sourcing → convert to Purchase Order or reject/cancel.

**B. RFQ and supplier selection workflow — Missing**  
Sourcing strategy → eligible suppliers → RFQ issue → quotation receipt → completeness/commercial/technical evaluation → comparison → conflict/exception handling → selection approval → purchase agreement/order.

**C. Supplier quotation/agreement lifecycle — Partial**  
Quotation received → supplier/price/UOM/tax/validity validation → evaluation/approval → accept/reject/expire → agreement creation/version → amendment/expiry/termination → preserve quote and accepted order price snapshot.

**D. Purchase Order lifecycle — Missing**  
PO draft → supplier/terms/price/tax/delivery/budget checks → approval → issue/acceptance → open/partial fulfilment → delivery/receipt confirmation → invoice matching → close/cancel/short-close.

**E. Purchase receipt and discrepancy workflow — Partial across Purchase/Warehouse/Inventory**  
Supplier delivery → Warehouse physical receiving/inspection → quantity/quality discrepancy evidence → Purchase commercial receipt confirmation/acceptance decision → Inventory acceptance and stock ledger update → supplier invoice matching → resolve mismatch.

**F. Purchase Invoice lifecycle — Missing**  
Invoice receipt → supplier/PO/receipt/price/quantity/tax validation → three-way or applicable matching → discrepancy resolution → GST determination and immutable snapshot → approval/acceptance → payable obligation → Finance payment → Accounting posting → correction/return/credit note.

**G. Purchase Return workflow — Missing**  
Return request → original purchase/receipt/invoice validation → approval → Warehouse pick/dispatch/hand-over → Inventory issue/return movement → supplier credit/debit adjustment → GST treatment → Finance settlement/refund → Accounting posting.

### 5.9 Point of Sale (POS) Operations

**A. POS Terminal lifecycle — Missing**  
Terminal registration → device/site/business-context validation → security/configuration → activation → assignment/maintenance → suspend/replace → retire.

**B. POS Operator Context lifecycle — Partial**  
Operator identity and scope verification → terminal permission validation → assignment to terminal/session → context switch/end → revoke access. Operator context is not a User Account or Role.

**C. POS Session/Shift lifecycle — Partial**  
Open session → operator and terminal validation → opening till/float count → active checkout operations → cash-in/cash-out and exception recording → checkout close → cash count → variance review → session/till reconciliation → close/approval.

**D. POS Checkout workflow — Partial**  
Scan/search item → resolve Product/Variant/package → price determination → quantity/discount/override checks → GST/tax determination → customer/party capture where needed → tender selection → Finance payment initiation/result → Sales-owned original commercial sale/invoice creation → POS checkout completion → Inventory/Warehouse fulfilment/stock effects → Accounting posting separately.

**E. Offline POS synchronization/recovery — Conditional, Missing in detail**  
Offline eligibility check → local operation with unique IDs/sequence → secure local persistence → reconnect → idempotent upload → server validation → duplicate/conflict/statutory checks → accept/reject/exception queue → reconcile POS, Sales, Finance, Inventory and Accounting statuses → recover failed items. Offline capability must not be assumed enabled.

### 5.10 GST/Tax Management

**A. GST Taxpayer Profile lifecycle — Partial**  
Taxpayer identity/evidence capture → legal identity linkage → verification → active administration → legal particulars change → inactive/retired status with statutory history.

**B. GST Registration lifecycle — Partial**  
Registration application/evidence capture → GSTIN/official status verification → registered place/jurisdiction capture → active effective period → amendment/suspension/cancellation/revocation status updates → historical preservation. CBM records verified facts; it does not grant registration.

**C. GST Registration Assignment lifecycle — Partial**  
Proposed target/purpose → legal taxpayer and place-of-business eligibility check → authoritative verification → approval → effective-dated assignment → transaction GSTIN selection → amendment/end/reassignment → preserve historical transaction references.

**D. HSN/SAC Registry lifecycle — Partial**  
Official code/source ingestion → full-code and six-digit CBM Stable Core validation → hierarchy/description/version/effective-date validation → publish for classification → supersede/retire/correct → preserve previous official version.

**E. Product/Service Tax Classification Assignment lifecycle — Partial**  
Goods Variant or authoritative Service Offering/Supply Subject → legally relevant facts/evidence → candidate code resolution → validation/review/approval → effective assignment → change/reclassification → historical preservation.

**F. GST Rate Rule lifecycle — Partial**  
Official notification/source ingestion → scope/description/conditions/components validation → legal effective date and publication date → review/approval → schedule/publish → active determination → supersession/withdrawal/correction → preserve historic rules.

**G. GST Tax Determination workflow — Partial**  
Transaction/supply facts → subject and classification assignment → applicable registration/context → place-of-supply and transaction conditions → rate-rule resolution → taxable value and price-basis calculation → components/rounding → ambiguity/exception block → determination snapshot → originating transaction acceptance → immutable historical retention.

**H. GST Return/Compliance workflow — Missing**  
Period/registration scope → source transaction collection → completeness/eligibility validation → reconciliation with Sales/Purchase/Accounting/Finance data → return preparation → review/approval → filing/submission via authorized process → acknowledgement/status capture → mismatch/rejection/amendment/correction → payment/liability reference → compliance close and history. Filing procedures and statutory deadlines require current official validation at implementation.

### 5.11 Finance & Payment Management

**A. Operational Cash/Bank Account lifecycle — Missing**  
Account proposal → ownership/legal entity/context verification → bank/account details and authorization → activation → statement/feed connection → mandate/credential rotation → freeze/close → archive and reconciliation. This is operational account administration, not Accounting Ledger ownership.

**B. Payment Method/Provider Configuration lifecycle — Missing**  
Provider/method evaluation → contract/security/compliance checks → credential/configuration setup → test/validation → approval → activate → monitor/change/rotate → suspend/disconnect → reconcile outstanding transactions.

**C. Payment Transaction lifecycle — Partial**  
Payment obligation/request → amount/currency/party/instrument validation → authorization → initiation → processing → successful/failed/rejected/timeout → retry or alternate route where permitted → refund/reversal/chargeback where applicable → settlement → reconciliation.

**D. Collection/Disbursement workflow — Missing**  
Receivable/payable or approved business obligation → payment instruction → approval → provider/bank execution → confirmation/evidence → partial/full allocation reference → settlement → exception resolution → Accounting allocation/posting separately.

**E. Settlement lifecycle — Partial**  
Provider/bank settlement batch → transaction-to-batch matching → fees/withholding/adjustment validation → settlement confirmation → unmatched/short/over items → exception resolution → operational reconciliation → Accounting Bank-to-Ledger reconciliation separately.

**F. Refund/Reversal/Chargeback workflow — Partial**  
Original payment lookup → eligibility and amount validation → approval → provider/bank request → execution result → update Finance payment records → notify originating Sales/POS/Purchase obligation → GST/Accounting correction where required → reconcile and preserve links to original.

### 5.12 Asset Management

**A. Operational Asset lifecycle — Partial**  
Acquisition or authorized Inventory-to-Asset recognition event → duplicate/source validation → Asset registration/identity → classification → custody/responsible person/location → use/status → inspection/maintenance/repair/insurance → transfer/reassignment → impairment/retirement decision → physical disposal → close and archive.

**B. Inventory-to-Asset conversion workflow — Partial**  
Eligible Inventory item/quantity → capitalization/asset eligibility check → authorization → controlled conversion event with source references → duplicate prevention → Asset identity registration → Inventory disposition/quantity update → Accounting fixed-asset recognition request → reconcile.

**C. Asset maintenance/repair workflow — Missing**  
Maintenance due/issue reported → work request → triage/approval → assign provider/technician → inspection/repair → parts/material issue from Inventory → completion/test → cost/evidence capture → Asset status update → Accounting cost treatment reference → close. Advanced enterprise maintenance/service-centre scope is not automatically approved.

**D. Asset transfer/disposal workflow — Missing**  
Transfer/disposal request → custody/ownership/authority check → approval → physical handover/disposal evidence → Asset location/custody/status update → Inventory impact if any → Accounting transfer/disposal accounting → reconciliation and closure.

### 5.13 Business Planning & Budget Management

**A. Plan/Scenario lifecycle — Missing**  
Planning cycle creation → scope/owner/period → assumptions and source references → draft scenarios → evaluate/compare → review/approval → published/approved baseline → monitor → revise/version → supersede/close.

**B. Budget lifecycle — Partial**  
Budget request → business/department/period allocation → assumptions and account/dimension references → draft → review → approval → approved baseline → controlled revision/reallocation → actual-vs-budget monitoring → close/archive.

**C. Forecast/Target lifecycle — Missing**  
Forecast/target definition → assumptions and version → authorized actual/operational data input → calculation → review → publish → rolling update/reforecast → compare with prior baseline → close.

**D. Budget consumption/control workflow — Missing**  
Proposed commitment (e.g., Purchase Requisition/Order) → applicable budget lookup → availability/threshold check → warning/block/approval per policy → record commitment/reservation only if separately approved → actual source transaction → consume/release commitment → variance analysis. An approved budget does not itself authorize a Purchase Order or create a GL transaction.

### 5.14 Counterparty Management

**A. Party/Counterparty lifecycle — Partial**  
Scoped party creation → identity and source validation → duplicate detection → review/approval where required → Customer and/or Supplier role profile activation → contacts/addresses → maintain/update → suspend/deactivate → archive under retention.

**B. Customer/Supplier role-profile lifecycle — Missing**  
Add role to existing Party → role-specific eligibility/terms reference → activation → role status/credit/procurement changes by owning domain → suspend/end role → preserve role history. One Party may hold both roles without creating duplicate identities by default.

**C. Duplicate resolution and merge/split workflow — Partial**  
Candidate detection → scope/permission check → evidence comparison → impact analysis across transactions/references → authorized decision → merge with surviving identity and redirects OR split erroneous combined record → preserve source IDs/provenance → reconcile dependent references → retain immutable history. Exact conflict handling and reversibility rules are not yet defined.

## 6. Cross-Module end-to-end workflows — each remains separately owned

### 6.1 Lead-to-Cash / Order-to-Cash — Missing as an end-to-end specification
Customer/Counterparty → Sales quotation/agreement → Sales Order → price determination → GST determination → Inventory reservation → Warehouse fulfilment → Inventory issue → Sales Invoice → Finance collection/payment → settlement → Accounting posting/reconciliation → Sales closure. Each stage has its own state and failure/reversal path. CRM lead/pipeline is not assumed in scope.

### 6.2 Procure-to-Pay — Missing as an end-to-end specification
Need/Requisition → budget/authority check → sourcing/RFQ → Counterparty/Supplier → quotation/agreement → Purchase Order → supplier delivery → Warehouse receiving/inspection → Purchase commercial acceptance → Inventory stock acceptance → Purchase Invoice matching → GST determination → Finance payment/settlement → Accounting posting/reconciliation → close.

### 6.3 Record-to-Report — Partial
Source transactions from Sales/Purchase/POS/Inventory/Payroll/Finance/GST → source-to-Accounting mapping → Voucher preparation/validation/approval/posting → sub-ledger and GL reconciliation → period close → financial statements → audit/export package. Source-domain completion does not imply posting completion.

### 6.4 Hire-to-Retire — Missing
Workforce need → recruitment where enabled → hire/Employee creation → onboarding → assignment and employment changes → attendance/leave → payroll inputs → performance/disciplinary processes where enabled → separation → access/custody offboarding → final payroll → records retention. HR, Payroll, User Account, Asset and Finance records remain separate.

### 6.5 Payroll-to-Pay — Missing
Payroll period/run → approved inputs → calculation → validation/approval/finalization → payslip/liability → Finance disbursement → provider/bank settlement → Accounting posting → Payroll/Finance/Accounting reconciliation.

### 6.6 Acquire-to-Retire Asset — Missing
Purchase/Inventory acquisition → Asset recognition → operational custody/use/maintenance → transfers/inspection → impairment/retirement decision → physical disposal → Accounting fixed-asset treatment → reconciliation and archival.

### 6.7 Return-to-Refund/Credit — Missing
Sales/Purchase original transaction → return request/authorization → physical Warehouse receipt/inspection → Inventory disposition → originating Sales/Purchase commercial credit/debit record → GST treatment → Finance refund/settlement → Accounting posting → reconciliation.

### 6.8 POS-to-Financial Close — Missing
POS sessions/checkouts/till movements → Sales commercial documents → Finance payment and settlement → Inventory stock effects → Warehouse tasks if needed → GST snapshots/compliance → Accounting postings → POS session/till and cross-domain reconciliation.

### 6.9 Plan-to-Performance — Missing
Plan/budget/target baseline → approved allocations → operational commitments → actual source transactions → Accounting and domain actuals → variance analysis → forecast/revision → approval of new baseline → close.

### 6.10 GST Compliance-to-Filing — Missing
GST registrations/assignments → HSN/SAC and rate rules → accepted transaction tax snapshots → period data aggregation → Sales/Purchase/Accounting reconciliation → return preparation → review/approval → filing/acknowledgement → amendment/rejection resolution → compliance close.

### 6.11 Counterparty-to-Commercial Relationship — Partial
Party create/verify → Customer/Supplier role → Sales/Purchase commercial terms (owned separately) → transactions → ongoing contact/address/relationship maintenance → role suspension/termination → preserve transaction history.

### 6.12 Cross-Module correction and event recovery — Partial
Source correction request → originating domain validates and records correction → publish versioned correction event → consuming domains identify affected references → each consumer performs its own authorized adjustment/reversal/reconciliation → idempotent retry and exception closure → preserve original and corrected records. An event acknowledgement is not proof of downstream business completion.

## 7. Data, migration, retention, and service continuity lifecycles

### 7.1 Data import/migration lifecycle — Partial
Source discovery → source authority/reliability assessment → extraction → mapping/version → staging → validation → duplicate detection → exception report → preview → reconciliation → approval → acceptance → post-import verification → source/batch provenance retention. Each target domain accepts only its own records.

### 7.2 Customer export/data portability lifecycle — Partial
Request and authority validation → scope/tenant/legal hold/retention check → domain-specific extraction → file/document inclusion → manifest and provenance → validation/reconciliation → authorized package creation → secure delivery/access record → expiry/retention.

### 7.3 Customer offboarding lifecycle — Partial
Offboarding request/trigger → authority and contractual checks → service/Subscription consequences → user/access revocation → open transactions and obligations resolution → export/data portability → domain-specific archive/retention/legal hold → resource decommissioning when eligible → deletion only under approved retention/legal rules → final evidence.

### 7.4 Sub-Portal migration lifecycle — Partial
Migration request and target eligibility → scope inventory → destination readiness → migration plan → data/configuration/file/identity/relationship transfer → integrity and tenant-boundary validation → service continuity cutover → reconcile → approve completion → old provider relationship closure. Existing document states invariants, but not a complete operational runbook/state machine.

### 7.5 Archival and restoration lifecycle — Partial
Archive eligibility → legal/retention/hold checks → freeze/snapshot as required → complete export → encryption → manifest/checksum/provenance → archive storage → verify → authorized operational copy removal → restore request/authorization → restore to controlled target → integrity verification → record restoration outcome. Exact lifecycle states, deletion windows, retention classes and restore testing requirements remain open.

### 7.6 Domain History and Audit Trail lifecycle — Partial
Domain event/action → owner-specific History record where applicable → separate system-wide Audit event → integrity/access controls → retention/archival → authorized export → deletion only under approved policy. The separation and 25 History domains are defined; per-domain retention schedules, immutable storage controls, legal holds, redaction, and deletion mechanics are not.

## 8. Lifecycle/workflow completeness matrix

| Area | Current documentation maturity | Conclusion |
|---|---|---|
| Business Entity + parent cascades | Detailed state model and restoration controls | Defined, some operational details remain |
| Subscription/Plan/Entitlement/Billing | State categories and independence rules | Partial |
| User Account and credential bootstrap | Detailed core states and credential paths | Defined for initial provisioning; broader login/session security separate |
| User–Business association and User scope | Separate lifecycle recognized | Partial |
| Distributor–Shop Mapping | Two-stage mutual confirmation and end relationship boundary | Partial; full state/exception matrix remains |
| GST Taxpayer/Registration/Assignment | Eligibility, verification, effective assignment | Partial |
| Approval workflow | Patterns and governance | Partial |
| Accounting | Voucher/posting, migration stages, period/reconciliation principles | Partial |
| Product/Item | Versioning, identifiers, lifecycle preservation | Partial |
| Pricing | Explicit conceptual states and snapshots | Partial |
| HSN/SAC and GST rate/tax determination | Registry/assignment/rule/snapshot principles | Partial |
| Inventory | Ownership and movement concepts | Missing/partial workflow detail |
| Warehouse | Task stages and states, physical operations | Partial |
| HR | Responsibility and History domain only | Missing detailed lifecycle |
| Payroll | Responsibility and History domain only | Missing detailed lifecycle |
| Sales | Ownership and document concepts only | Missing detailed lifecycle |
| Purchase & Procurement | Ownership and receipt boundaries only | Missing detailed lifecycle |
| POS | Session/till/checkout ownership and History scope | Partial |
| Finance & Payment | Payment/settlement statuses and ownership | Partial |
| Asset | Operational ownership and recognition boundary | Missing detailed lifecycle |
| Planning & Budget | Planning ownership and baseline concept | Missing detailed lifecycle |
| Counterparty | Party/role ownership and merge/split guardrails | Partial |
| Platform-wide Services | Taxonomy and ownership | Missing service lifecycle |
| Global Settings/configuration | Governance and inheritance principles | Partial |
| Notifications/files/integrations/jobs/reporting | Platform capability classification | Missing detailed lifecycles |
| Migration/offboarding/archive | Requirements and some stages | Partial |
| Cross-Module business journeys | Ownership matrix only | Missing end-to-end specifications |

## 9. Minimum completion standard for each lifecycle/workflow

A lifecycle or workflow is not considered fully documented merely because its owner or a list of stages is named. Each applicable flow must define:

1. **Purpose and boundary:** what starts it, what it accomplishes, and what it explicitly does not do.
2. **Authoritative owner:** owning Module/domain and each participating Module's responsibility.
3. **Actors and authority:** initiator, responsible actor, approver, system actor, scope, Role and Permission.
4. **Trigger and preconditions:** eligibility, required source records/evidence, prerequisites, effective dates and applicability.
5. **States and transitions:** complete state list, allowed transitions, forbidden transitions, terminal states, transition guards and who/what may cause them.
6. **Normal path:** numbered, independently understandable end-to-end steps with inputs, outputs and resulting records.
7. **Decision branches:** approvals, rejection, partial acceptance, expiry, conflict, duplicate, statutory conditions and configurable choices.
8. **Failure/recovery:** timeout, retry, idempotency, concurrency, partial completion, compensation/reversal, manual intervention and reconciliation.
9. **Cross-domain contracts:** request/event/response, identifiers, correlation, versioning, ownership, asynchronous status and downstream completion evidence.
10. **Financial/tax/stock consequences:** separate source transaction, payment, tax, inventory, warehouse and accounting outcomes.
11. **History/audit:** domain History, system Audit Trail, immutable snapshots, provenance and correction links.
12. **Security and data boundaries:** tenant isolation, least privilege, sensitive data, separation of duties and no unauthorized scope expansion.
13. **Timing and retention:** effective/recorded times, deadlines, expiry, retention, archival, restoration and deletion.
14. **Applicability and configuration:** eligible business types/modules, optional features, defaults, inheritance and non-overridable controls.
15. **Completion criteria:** what constitutes success, what remains pending, what can be reopened, and how final reconciliation proves completion.

A workflow that spans Modules must define each Module's own completion state. The overall journey is complete only when its required independent outcomes have been confirmed or explicitly accepted as exceptions.

## 10. Final audit conclusion

**The current CBM documentation is not yet complete at lifecycle/workflow level.** It is a strong business-concept and domain-ownership baseline, and it contains several important lifecycle decisions, but it does not yet provide a separate, complete workflow/state-transition specification for every business process and every independently governed object.

The highest-priority gaps are:
1. Sales, Purchase, POS, Finance/Payment, Inventory, Warehouse end-to-end state and exception flows.
2. HR Employee/Employment and Payroll period/run/correction/payment lifecycles.
3. GST compliance/return preparation/filing and tax exception workflows.
4. Asset acquisition/recognition/maintenance/transfer/disposal workflows.
5. Planning/budget/forecast lifecycle and budget-consumption control.
6. Counterparty onboarding/role/duplicate merge-split workflows.
7. Business Identity, Legal Identity, Professional Identity, Ownership Link, User scope, Role/Permission and Delegation lifecycles.
8. Platform service, configuration, approval, notification, file, API/event, connector and background-job lifecycles.
9. Complete customer onboarding/offboarding, Sub-Portal migration, data export, archival, restoration and deletion runbooks.
10. Cross-Module journeys, reconciliation, correction propagation, retries and recovery.

No exact statutory filing procedure, tax rate, payroll rule, retention duration, security threshold, approval limit, or commercial policy should be invented to make a diagram look complete. Such details must be validated against current authoritative sources and the applicable business policy when the relevant detailed specification is prepared.

**Completion decision:** Do not label the CBM project documentation “fully complete” until every applicable flow in this catalogue has a complete, reviewed lifecycle/workflow specification under the 15-point standard above, cross-referenced to its owner Module and reconciled with the Master decision baseline.


---

## Document control and relationship to other project records

- **Authoritative decisions:** [CBM Master Business and Architecture Decisions](CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md)
- **Chronological rationale and discussion:** [CBM Project Discussion History](CBM_PROJECT_DISCUSSION_HISTORY.md)
- **Earlier lifecycle discovery audit:** [CBM Lifecycle and Workflow Completeness Audit](CBM_LIFECYCLE_AND_WORKFLOW_COMPLETENESS_AUDIT.md)

This reference consolidates the lifecycle inventory and completeness assessment for convenient independent review. If any wording here appears to conflict with the Master document, the Master document governs approved concepts and boundaries. The conflict must be recorded and resolved; this reference must not silently override the Master.

**Completion declaration:** As of 19 November 2026, the project's approved business-concept and module-ownership baseline is substantial, but detailed lifecycle and workflow documentation is not complete. The status labels and gaps in this reference are an assessment, not a claim that all listed processes have been approved or fully specified.


---

# 11. Supplementary lifecycle inventory from the systematic coverage review

## 11.1 Customer Business onboarding, provisioning, and operational activation — Partial

This is a composed onboarding workflow, not an additional Business Entity state and not a new Business Module.

**Sequence to specify:** service-provider/channel entry or authorized direct creation → identify the intended Customer Business Architecture → validate Organization eligibility and accountable identity/ownership evidence → create the Organization record → establish its Business Identity and applicable Legal/Professional Identity references → establish the eligible primary Service Provider relationship → configure permitted Global Business Settings and business defaults → establish Subscription/Plan/Coverage/Entitlement where applicable → enable only applicable Business Modules and capabilities → create eligible Distributor/Shop children under the mandatory Parent Organization and validate Owner ID Set consistency → capture/verify applicable GST taxpayer, registration, place-of-business and assignment references → provision Organization-scoped User Accounts and explicitly assign User Scope Mappings → configure authorized Roles, Permissions and Approval Workflows → selectively import/opening data through the owning Modules → validate operational readiness, unresolved exceptions and reconciliation → authorized activation/operational handover.

**Required independent completion evidence:** Business Entity activation; identity and ownership validation; service-provider relationship readiness; subscription/entitlement availability where applicable; applicable registration evidence; User Account and access readiness; module-specific master-data readiness; import reconciliation; and explicit acceptance of outstanding exceptions. Completion of one item must not implicitly complete another.

**Boundary:** Creating an Organization does not automatically create its Distributor/Shop children, GST registrations, User Accounts, Subscription coverage, accounting Books, or operational opening balances. Each is a separately owned record/workflow. A service-provider relationship is not ownership.

## 11.2 Owner, Owner ID, Ownership Link, and Ownership ID lifecycle — Partial

**Separate object/workflows:** Owner identity reference; Owner ID; Business Identity; Ownership Link; and Ownership ID must not be collapsed into one record.

**Sequence to specify:** ownership claim or authorized capture → validate the Owner and relevant Business Identity references → collect and assess permitted evidence → enforce Customer Business Ownership Consistency across the Organization and each direct Distributor/Shop child → create the separately scoped Ownership Link and Ownership ID for the relevant Business Identity → record effective and recorded times and provenance → maintain → submit an ownership change, correction, release, succession or dispute → validate all affected entities and dependent references → authorize and apply the change without silently altering unrelated authority, User Scope, structural Parent, Service Provider or Mapping relationships → retain the prior and new ownership chronology.

**Unspecified:** accepted evidence classes, verification strength, disputed/contested ownership procedure, legal succession/reorganization, effective-date policy, correction versus replacement, and approval authority. These require explicit project policy and, where law is involved, current jurisdiction-specific legal review.

## 11.3 Distributor–Shop Mapping lifecycle — Partial

**Approved initiation and mutual-verification sequence:** an eligible Shop initiates a Mapping request and generates its own Secret Code → the eligible Distributor verifies that Shop-generated code → after that verification, the Distributor generates a separate confirmation code → the Shop verifies the Distributor-generated code → only after both distinct verification stages succeed may the Mapping become Active.

**Subsequent lifecycle to specify:** Active Mapping operation within its approved Mapping Type → authorized change to Mapping Type or permitted exchange scope → revalidation/reapproval where required by the still-to-be-defined policy → suspension or restriction where permitted → revocation/termination by an authorized actor → preserve the Mapping-specific History → future re-mapping through a new valid initiation/verification process.

Both codes remain separate, expire, are single-use, rate-limited and revocable. Failed, expired, replayed, revoked or rate-limited attempts must not create an Active Mapping. A Mapping does not change ownership, structural Parent, data isolation, or confer unrestricted permissions. Exact retry limits, code lifetime, cooldown, request expiry, suspension grounds, remapping restrictions and unilateral/bilateral termination conditions remain policy items and are not invented here.

## 11.4 User Account, contact verification, authentication, credential, authenticator, and session lifecycles — Partial

These are separate but connected lifecycles. The account, the person, the User–Business association, User Scope Mapping, credential, authenticator, and authenticated session are not interchangeable.

### A. Customer Business User Account lifecycle

**Sequence to specify:** authorized provisioning request → validate exactly one owning Organization's Business Architecture → allocate an unused portal-wide immutable User Code → capture distinct account contact identifiers and verify applicable channels → establish the initial credential using the Organization's configured method (Email Temporary Password, OTP Password Setup, or Both Required) → activate ordinary account access only after the required credential process succeeds → maintain authorized profile/contact data → suspend/restrict/reactivate where permitted → terminate account → revoke bound authenticators and sessions → preserve account and action history under the applicable retention rules.

The same natural person may have separate accounts for different Organizations; these remain independent. User Code is never a credential or permission. The detailed global identity model for Main Portal and Sub-Portal personnel remains separate where specified.

### B. Contact-channel verification and change lifecycle

**Sequence to specify:** add/change email or mobile number → validate account ownership and permitted uniqueness/context rules → initiate in-application verification through a permitted channel → issue a purpose-bound OTP → verify within approved validity/retry limits → replace the current verified contact reference only after success → record prior/new values under protected history/audit controls → revoke any pending verification attempt. A contact update must not silently alter a permanent password, User Code, User–Business association, or User Scope.

All outbound communication remains subject to the absolute No-URL Rule. Verification must be completed inside the CBM application; no URL, hyperlink, QR-to-URL, invitation link, magic link or password-reset link may be sent.

### C. Login/authentication lifecycle

**Sequence to specify:** receive login attempt → resolve the User Code and the accompanying account identifier against the same account → verify the credential and applicable authentication requirements → apply the approved abuse/rate-limit and account-state checks → record success/failure and security-relevant outcome → issue or deny an authenticated session → apply the separately defined redirect/return-destination rules → on failure, preserve a safe recovery path without weakening account boundaries.

The Master document records only the login-identification baseline and expressly leaves the complete Login Architecture to separate discussion. Exact login sequence, lockout/anti-abuse thresholds, MFA requirements, error disclosure, redirect policy, and recovery evidence are therefore not approved by this Reference.

### D. Credential and authenticator lifecycle

**Separate sequence:** credential/OTP/temporary-password issuance or replacement → secure delivery or in-application presentation under the selected approved method → restricted-purpose use → successful binding/establishment → ordinary use → change/replacement or recovery → suspected loss/theft/compromise/unauthorized duplication → prompt protective suspension or invalidation as applicable → re-proofing/recovery → rebind replacement authenticator → revoke retired/compromised authenticator → preserve lifecycle evidence.

Temporary Password and OTP rules in Master Sections 3.15 and 15 remain controlling. Permanent password change, recovery, credential compromise, authenticator binding and revocation need their own detailed policies. No new authentication factor or channel is approved merely by this sequence map.

### E. Authenticated Session lifecycle

**Sequence to specify:** successful authentication → session establishment bound to the authentication event and endpoint → active use → reauthentication/renewal where allowed → explicit logout, inactivity timeout, overall lifetime expiry, account suspension/termination, credential/authenticator compromise, or authorized revocation → server-side invalidation → client-side cleanup → audit/security evidence → reauthentication before new access.

The design must explicitly consider browser tabs/windows, multiple devices, concurrent sessions, session refresh, access-token lifetime, remote logout, expiry races, and session invalidation after scope/permission changes. Session timeout values and concurrency policy remain unapproved. A session ending at one relying application does not automatically prove that another independent relying-party session ended.

**Research alignment:** NIST SP 800-63B-4 separately addresses authenticator binding, maintenance, loss/theft/compromise, expiration, invalidation and session management. It discusses explicit logout, inactivity and overall timeouts, reauthentication, and warns against insecure session-secret storage such as HTML5 Local Storage. These are research inputs for later CBM security specification, not automatic CBM policy adoption.

## 11.5 Role, Permission, User Scope, Delegation, and Approval instance lifecycles — Partial

The four control types remain distinct.

- **Role definition/version:** propose → define responsibility set → validate authority and applicability → approve/publish → assign in a business context → version/change → retire; existing assignments must resolve against the governed versioning policy.
- **Permission grant:** define action and target resource → validate Role/User-specific assignment authority → grant within assigned User Scope → evaluate on each action → change/restrict/revoke → preserve access-decision evidence. Permission cannot enlarge User Assigned Authority Scope.
- **User Scope Mapping:** authorized assignment request → validate User–Business association and Business Available Authority Scope → explicitly select entity/location/business-area scope and exclusions where supported → approve if required → activate → modify/expire/suspend/revoke → recalculate dependent access → preserve User Scope Mapping History.
- **Delegation:** define delegator, recipient, actions, scope, limits and duration → validate delegator's authority → approve → activate for the bounded period → monitor use/limits → expiry/revocation → preserve evidence.
- **Approval instance:** action requests approval → freeze or version the relevant submitted proposal → resolve policy and eligible approvers → execute configured sequential/parallel/single/multiple steps → approve/reject/return/withdraw/expire as permitted → revalidate action preconditions and authority → continue, block or require resubmission → preserve decision and actor evidence.

Policy version changes while a request is pending, approver unavailability, escalation, quorum, delegation during approval, emergency intervention and resubmission rules remain to be specified per approval family. Approval never substitutes for Permission.

## 11.6 Platform Subscription, Plan, Add-on, Entitlement, Usage Limit, Invoice, and Payment lifecycles — Partial

This is the CBM ecosystem's Platform Subscription/Billing domain, not the customer's Finance & Payment Management Module.

- **Commercial Account:** establish provider/customer commercial relationship → validate accountable party and billing context → activate → maintain billing contacts and terms → suspend/close → preserve commercial history.
- **Plan and Plan Version:** define plan/capability/limit/coverage terms → validate applicability and authority → approve/publish a version with effective time → make available for new or permitted changed subscriptions → supersede/retire without rewriting existing accepted subscription history.
- **Add-on:** define optional capability/coverage and compatibility → version/publish → attach or detach under subscription change rules → apply its effective period and entitlement impact → retire for future selection while preserving prior subscriptions.
- **Subscription:** request/accept → select Plan Version and compatible Add-ons → establish commercial period and lifecycle terms → create explicit Coverage Assignments → resolve Entitlements and Usage Limits → activate → monitor usage/payment state → renewal/upgrade/downgrade/change → grace/suspension/reinstatement where the Master permits → cancellation/termination → retention/archive.
- **Subscription Coverage Assignment:** propose covered entity/module/capability/scope → validate eligibility and commercial authority → approve/activate → change/expand/reduce/end with effective dating → preserve assignment history.
- **Entitlement:** derive from applicable Subscription, Plan Version, Add-on, Coverage Assignment, applicability and effective dates → publish/evaluate entitlement result → update on approved commercial change → expire/revoke → retain resolution provenance. Entitlement does not grant User Permission.
- **Usage Limit:** resolve the active limit and scope → record usage against its governed measurement/window → evaluate threshold → apply only the approved warning/restriction/overage response → reconcile corrections/resets/period changes → retain usage evidence. Exact counters, reset windows and overage policy are not approved here.
- **Platform Invoice:** derive invoiceable subscription charges/adjustments → validate customer/account/tax/billing context → issue/version the invoice under applicable rules → record receivable and due date → correction/credit/debit or cancellation only through the defined billing process → retain.
- **Platform Subscription Payment:** initiate payment against the platform billing obligation → provider/bank response → pending/success/failure/timeout/partial outcome → retry/reconcile according to payment contract → allocate to invoice → apply any approved grace/suspension/reinstatement consequence → refund/reversal/chargeback handling → close and preserve provider evidence.

Platform Subscription invoice/payment facts must not be moved into a customer's business Finance records merely because the same platform may use payment infrastructure. CBM's platform billing lifecycle and a Tenant's business payment lifecycle are distinct.

## 11.7 GST taxpayer, registration, place-of-business, assignment, and compliance record lifecycles — Partial

The Reference's Section 5.10 already covers taxpayer profile, GST Registration, Registration Assignment, HSN/SAC registry, classification assignment, rate rules, transaction Tax Determination and filing. The following granularity is also required:

- **Place of Business record:** proposed principal/additional place → validate Legal Identity, relevant GST Registration and evidence → create/verify within the applicable registration context → update particulars or add/remove a place under the permitted legal process → preserve effective period and registration linkage. A CBM Place of Business reference is not itself a new GST registration.
- **GST authorized signatory/representative reference:** capture the role and evidence under the relevant GST Registration → verify eligibility/authority → maintain/change/revoke → preserve filing-signatory evidence. It is a tax-domain actor reference, not a CBM User Role or a general business Permission.
- **Registration application/update lifecycle:** assemble official identity and place evidence → authorized submission by the taxpayer/signatory → record acknowledgement/reference → monitor verification/queries → approved/rejected/pending outcome → record effective status and conditions → update amendments/suspension/cancellation/revocation facts from authoritative evidence. CBM must not represent an application as granted merely because it was submitted.
- **Return/filing record lifecycle:** create period/registration-specific return work → compile source-domain facts → validate completeness and eligibility → reconcile data and identify missing/duplicate/mismatched records → prepare statement/return → generate and review summary/preview → authorized signatory confirmation → submit/file via the permitted channel → retain ARN/acknowledgement and filed status → track liability/payment separately → process rejection, correction, amendment or subsequent return only through the legally permitted route → close the period with evidence.

Official GST Portal guidance distinguishes preparation, summary/preview, filing and acknowledgement for GSTR-1, and separately documents creation, saving, payment of tax and filing for GSTR-3B. It also states that GSTR-3B amendment is not allowed. These guides are process evidence for current portal workflows, not permission to hard-code those form names, controls, deadlines or correction paths permanently. At detailed design time, confirm current return types, taxpayer category, applicability, statutory deadlines, amendment routes and electronic signing requirements from the then-current official GST sources.

## 11.8 Accounting master-data and Fixed Asset Accounting sub-lifecycles — Partial

Section 5.1 covers Book, Voucher, Posting, Period, Reconciliation and data exchange. Each foundational accounting object also needs an explicit lifecycle:

- **Accounting Framework/Policy/Basis:** select eligible legal/accounting context → validate permitted framework and recognition basis → approve/effectively publish version → assign to Book → change only under governed prospective/retrospective transition rules → preserve the policy basis used by historical entries and statements.
- **Chart of Accounts / Account Group / Ledger Account:** design proposal → validate hierarchy, account nature, control/sub-ledger use and uniqueness → approve/publish → activate for permitted Books/periods → restrict/deactivate/merge only with controlled balance/reference migration → preserve historical account mappings and report comparatives.
- **Voucher Type:** define purpose, numbering, permitted source/action, required evidence, posting rules and approval requirements → approve/version/publish → use in eligible Books → change under effective-dated rules → retire without invalidating posted Vouchers.
- **Accounting Entry / Journal Entry / Posting:** draft from accepted source or authorized manual entry → validate account, period, dimension, tax/source references and debit-credit balance → approve where applicable → post to Ledger → prevent silent edits → correct using linked reversal/adjustment → reconcile.
- **Sub-ledger and Outstanding Allocation:** establish control-account mapping → create source receivable/payable/open-item facts → allocate/part-allocate/settle/write off only under authorized policy → reverse/reallocate with traceability → reconcile control account to detail.
- **Cost Centre / Cost Category / Accounting Dimension:** define/version → determine applicability and permitted value sets → assign to transactions/Books → change or retire with reporting-history preservation. They do not become separate Business Modules.
- **Financial Statement/reporting run:** select Book/framework/period/version → verify posting and reconciliation readiness → generate reports from authoritative posted facts → review/approve where required → publish/export snapshot → correct by a new run/version, not by mutating the source ledger.
- **Fixed Asset Accounting:** eligible acquisition/recognition evidence → capitalize in the appropriate Book → establish accounting cost, useful-life/method/residual assumptions under the applicable framework → periodic depreciation → impairment/revaluation where permitted → component replacement/derecognition → disposal accounting → reconcile to the separate Operational Asset record. CBM Asset Management owns physical/operational asset facts; Accounting owns capitalization and accounting treatment.

The IFRS Foundation's IAS 2 and IAS 16 pages are useful reference examples for the distinct accounting topics of inventory cost/expense recognition and PPE recognition, measurement, depreciation, impairment and derecognition. They are not a declaration that IFRS is CBM's universal or mandatory accounting framework. Each Book must follow its applicable approved Accounting Framework and law.

## 11.9 Product/Item catalogue, identifiers, attributes, price and tax-reference sub-lifecycles — Partial

Section 5.2 covers the principal Product Definition, Variant, Instance, Identifier, packaging/bundle/kit and catalogue-sharing paths. The following subordinate objects must also be mapped independently:

- **Product Family/Group/Classification:** define taxonomy node → validate parent/relationship rules and applicability → publish/version → assign/reassign Product Definitions or Variants → retire/merge/supersede without rewriting historical classification snapshots.
- **Attribute definition / Variant dimension / specification schema:** propose typed attribute and subject level → define datatype, units, precision, validation, applicability, required/optional status, inheritance and permitted override → approve/publish version → assign values to the correct Product Definition, Variant, Packaging, Instance or Component → evolve schema with migration/compatibility controls → retire while retaining historical values.
- **Identifier and external identifier mapping:** identify issuer/namespace/identifier type/subject level → validate uniqueness scope, format and authority → assign/import → resolve during transaction/scan/import → detect collision/duplicate → correct/remap/supersede/retire with provenance. Preserve significant characters and leading zeroes; do not invent regulated or issuer-controlled values.
- **Data Carrier (barcode/QR/RFID/NFC):** encode an authorized identifier/data reference → associate with the correct Product/Variant/Package/Instance/Handling Unit subject → print/commission → scan/read/resolve → replace/damage/decommission → preserve traceability. A carrier is not itself the subject's identity. QR codes that resolve to URLs may not be used in CBM outbound communications under the Absolute No-URL Rule.
- **Trade Packaging Configuration:** define package/contained unit and conversion quantity/UOM → validate hierarchy, commercial/stock applicability and identifiers → publish/effectively version → use in price, sales, purchase and inventory transactions → supersede while preserving historical transaction UOM/basis.
- **Bundle/Kit:** define bundle type and components/quantities → validate fixed/virtual/configurable/kit rules and availability/price/tax handling → approve/publish → sell/assemble/consume only through its applicable domain contract → revise/retire while preserving prior transaction composition. This does not imply a manufacturing BOM.
- **Declared Product/Packaging Price:** create declaration and subject/basis → validate required source/legal declaration and tax basis → approve/publish with effective date → supersede/correct → preserve price declaration snapshot. It remains separate from Sales price lists, Purchase prices and Inventory valuation.
- **Service Offering/Supply Subject:** the authoritative service domain creates and maintains the actual service/supply identity and offering lifecycle → define applicability, scope and version/effective period → publish/withdraw/change → expose a typed reference to Product/Item where applicable → GST/Tax maintains SAC classification and Tax Determination. This reference does not create a generic Service Operations, Service Centre or service-delivery Business Module.

## 11.10 Inventory and Warehouse subordinate operational lifecycles — Partial

- **Inventory Position:** derive from accepted Inventory transactions and eligible reservations/statuses → expose a scoped as-of position → reconcile to the authoritative Stock Ledger → correct through source movements, not by direct editing of a derived display.
- **Inventory Status:** create/use only governed status values applicable to the stock context → transition through authorized receipt, inspection, quality/hold, reservation, availability, issue, return or disposal events → preserve status history. The exact status catalogue and transition matrix remain to be approved; examples in Section 5.3 are not a finalized universal enumeration.
- **Batch/Lot:** establish batch identity and supplier/manufacturer/source attributes at authorized receipt/creation → link quantities and expiry/quality facts → allocate/consume/transfer/return/hold/recall/dispose → preserve traceability and lineage. Batch identity does not automatically imply unit serial identity.
- **Serialised item:** establish or validate each serial identity at receipt/creation → link to one eligible Product/Variant/Instance and stock movement → track location/custody/status → issue/return/repair/transfer/dispose → prevent duplicate active identity and preserve complete history.
- **Stock reservation/allocation:** validate demand, eligible stock, ownership/location/status and time window → reserve atomically → partially consume/release/expire/cancel → re-evaluate availability → reconcile reservation against the originating Sales/POS/other demand.
- **Inventory movement:** authorized source event → validate item/UOM/quantity/location/status/source reference → reserve/confirm movement → record dispatch/in-transit/receipt where applicable → accept/reject discrepancy → post immutable Stock Movement → update Inventory Position and valuation inputs → reconcile. Physical Warehouse execution is a separate record.
- **Inventory cost/valuation:** receive cost facts from Purchase/other authorized sources → apply the Book/Inventory's approved costing and valuation policy → allocate eligible purchase/conversion/other costs → calculate issue/closing values → recognize write-down/loss/revaluation where permitted → close valuation period → reconcile to Accounting via explicit posting contract. IAS 2 is a reference example, not a CBM-wide selected accounting rule.
- **Warehouse Storage Location/Bin/Zone/Section:** define location hierarchy and type → validate facility, capacity, location code and permitted handling → activate → block/close/reconfigure with controlled relocation of resident Handling Units/stock references → retain historical location references.
- **Handling Unit:** create/identify → associate contents and quantities → close/seal → receive/move/pick/stage/load/dispatch → split/merge/repack only under authorized traceable operations → empty/retire. A Handling Unit is not a Product Variant, Trade Packaging Configuration or Inventory ownership record.
- **Warehouse Work Task exception/recovery:** request and plan → assign and execute → record partial/failed/blocked/damaged/short or unsafe outcome → hold or escalate → re-plan/reassign/cancel → confirm completed physical work → publish explicit Inventory/Purchase/Sales contract outcome → reconcile. A completed task does not by itself prove Inventory acceptance.

## 11.11 Counterparty contact, address and role-reference lifecycles — Partial

**Contact/address record:** add with Party and tenant/business scope → validate type, purpose, source and permitted usage → verify where required → mark preferred/primary only under explicit rules → amend/version → deactivate/retire → preserve provenance and relevant historical transaction snapshots. Contact details for a Party must not be treated as User Account login identifiers unless separately registered and verified for that account under the User lifecycle.

**Customer/Supplier role profile:** add a role to an existing scoped Party → validate role eligibility and duplicate conditions → activate role → maintain role-specific relationship status → suspend/end role → retain the role history and commercial references. Sales owns customer commercial terms; Purchase owns supplier terms. One Party may hold both roles. The Customer/Supplier role profile is not itself a User, Employee, Legal Identity, GST Registration or Business Identity.

**Merge/split:** before merge, identify the surviving Party, record all source identities, analyze dependent references and prevent cross-tenant merges; update only permitted reference links via governed migration, never rewrite accepted historical transaction snapshots. A split must reconstruct the separately evidenced Parties and reassign only references demonstrably attributable to each, with an exception report and audit evidence. Exact reversibility, conflict priority and approval rules remain unresolved.

## 11.12 Application Client, endpoint/device, terminal, and provider credential lifecycles — Partial / Conditional

**Application Client registration:** client purpose/owner and supported portal context → authorization and permitted scopes → client registration/version → credential or device binding where applicable → activation → usage monitoring and version compatibility → credential rotation/revocation → deprecation/migration → retirement. An Application Client is a channel, not a Business Module, User Account, or independent owner of business records.

**Managed endpoint/device:** register device/terminal and its business/location context → validate authorized owner/operator and security posture as applicable → bind permitted client credentials → activate → monitor/maintain/update → lost/stolen/compromised quarantine or revocation → replacement/transfer → decommission and erase/revoke remaining credentials. POS terminal lifecycle remains POS-owned; warehouse scanners/handhelds remain execution clients and do not own Inventory/Warehouse facts.

**External provider credentials:** issue/store restricted provider credentials → test binding and least-scope access → activate → rotate/renew → suspend on suspected compromise → revoke on disconnect/termination → verify provider-side revocation where supported → preserve security evidence. Exact vault, cryptographic and provider-specific procedures belong in technical security specifications.

## 11.13 Platform Service operations, release/change, incident and continuity lifecycles — Partial / Conditional

The Master classifies Platform-wide Services, categories and capabilities. For each enabled service, operational specification must also cover:

- **Service/capability provisioning:** approve scope and owner → configure dependencies → provision for eligible ecosystem/Service Provider/Tenant/business scope → validate health and access → activate → monitor capacity and usage → change/version → deprecate/migrate → retire.
- **Service configuration/version lifecycle:** draft version → compatibility/security/data-boundary review → authorize/publish → stage/apply to the exact applicable scopes → monitor → rollback or supersede using a controlled change record → preserve version applicability.
- **Incident lifecycle:** detect/report → classify severity and affected service/tenant/data scope → contain and protect data → communicate through permitted channels → restore service → reconcile incomplete/duplicate work → validate recovery → close with root cause/actions. No severity thresholds, service-level targets or notification timing are invented here.
- **Problem/change lifecycle:** identify recurring cause or proposed change → assess impact/risk/dependencies → approve under authority → schedule and execute → verify → roll back or close → retain change evidence.
- **Backup/restore and disaster-recovery lifecycle:** define eligible data/service scope and recovery objectives → create protected backup/replica → monitor and test recoverability → authorize recovery → restore in controlled order → verify integrity, tenant separation, event/transaction consistency and access → reconcile post-recovery work → record recovery outcome.
- **Capacity/performance lifecycle:** measure service usage and capacity → forecast → provision/scale within authorized constraints → monitor saturation → remediate → record cost/availability effects.
- **Software/service release lifecycle:** build/package → security and compatibility review → test → approve → release to scoped environment → monitor → rollback/hotfix if required → deprecate prior version. This is Platform operational lifecycle, not a customer business transaction or authority to implement software in this documentation phase.

These operational workflows are conditional on the relevant platform capability and require later service-specific runbooks and targets.

## 11.14 Shared Domain Model, contract, external mapping, and synchronization lifecycles — Partial

- **Shared Domain Model/Contract:** identify owner and use case → define typed subject/reference and permitted operations → define schema/version, provenance, tenant/business context and compatibility → approve/publish → consumer onboarding → version transition/deprecation → consumer migration → revoke/retire. Shared contracts do not create shared write authority.
- **Source Transaction Reference:** establish reference to an owner-issued source record → validate source owner, scope, immutable identifier, version and relationship → consume under permitted context → retain reference through correction/reversal → handle source retirement without dangling or silently substituted references.
- **External Identifier Mapping:** identify external issuer/namespace/source system → map to the CBM subject and field with confidence/provenance → validate uniqueness and ambiguity → activate mapping version → resolve imports/exports → correct/supersede/retire while preserving prior mappings.
- **Synchronization Contract:** identify source-of-truth per field/object → define direction, permitted operations, ordering/version, cursor/sequence, idempotency, conflict rule and deletion semantics → authorize endpoints → synchronize → detect duplicates/gaps/conflicts → reconcile → replay or compensate as permitted → revoke/retire. No bidirectional unrestricted synchronization is implied.
- **Portal-to-Portal interoperability:** authorize participating portals and exact dataset/purpose → validate user and portal scope → establish contract/version → exchange only allowed records → reconcile acknowledgements/conflicts and provenance → revoke relationship/credentials and terminate exchange → preserve each portal's ownership and tenant boundaries.

## 11.15 Subscription billing, customer business Finance and provider payment regulatory boundary — mandatory separation

Three separate contexts must be retained:

1. **CBM Platform Subscription Billing:** Commercial Account, Plan, Subscription, Coverage Assignment, Entitlement, Usage Limit, platform Invoice and platform Payment (Section 11.6).
2. **Customer Business Finance & Payment:** operational cash/bank accounts, customer/supplier obligations, payment instructions, collections/disbursements, provider execution, settlement, refunds, chargebacks and payment reconciliation (Section 5.11).
3. **Accounting:** books, receivable/payable balances, allocations, vouchers, ledger postings and bank-to-ledger reconciliation (Sections 5.1 and 11.8).

Payment service provider/Payment Aggregator onboarding and regulated operation may require separate regulatory due diligence. RBI's Master Direction on Regulation of Payment Aggregators is shown as updated on 11 November 2026. Applicability depends on the actual role and activity of CBM or the provider; the existence of a payment integration does not by itself make every CBM customer or the portal a Payment Aggregator. Legal/compliance review must determine the regulated party, authorization, scope, escrow/settlement, KYC, security, dispute and reporting duties before an implementation-specific workflow is finalized.

---

# 12. Additional end-to-end journeys required for complete lifecycle coverage

The journeys below complement Section 6. They do not combine domain ownership. Each participating Module must expose its own completion state, immutable source record and exception result.

### 12.1 Customer Business onboarding-to-operational-readiness — Partial
Main Portal/eligible Service Provider → Customer Business Organization → identities/ownership → direct child entities where required → Service Provider relationship → Subscription/coverage/entitlements → Global Settings/configuration → GST taxpayer/registrations/places/assignments as applicable → Counterparty/Product/Accounting opening data → User Accounts/scopes/permissions → applicable Module setup → migration/reconciliation → readiness approval → operations. Each activity may be omitted only when explicitly not applicable; one successful step does not imply another.

### 12.2 Service Provider onboarding, customer servicing, migration and exit — Partial
Main Portal eligibility/governance → Sub-Portal provider identity and authority → provisioning/configuration → customer service relationship → service provisioning and monitoring → provider change or migration request → destination readiness and contract/data-scope validation → controlled transfer and continuity cutover → reconcile service, access, billing and data → terminate old provider relationship → preserve customer ownership, tenant data rights and history.

### 12.3 Subscription-to-Cash / Platform Billing-to-Reconciliation — Partial
Commercial Account → Plan Version/Add-on selection → Subscription → Coverage Assignments → Entitlements/Usage Limits → platform invoice → platform payment → provider/bank confirmation → invoice allocation → grace/suspension/reinstatement/cancellation outcome under approved policy → platform billing reconciliation and history. This is not customer-business Sales or Finance workflow.

### 12.4 User-to-Work / Joiner-Mover-Leaver access journey — Partial
Organization-scoped User Account creation → initial credential establishment → contact/authenticator verification → User–Business association → explicit User Scope Mapping → Role/Permission and delegated authority → login/session use → role/scope changes during movement → suspension/termination → revoke sessions/authenticators and User–Business association → preserve HR, User, scope and Audit histories separately. Employee employment status does not automatically create or terminate a User Account unless an explicit cross-domain policy triggers that separate action.

### 12.5 Purchase-to-Asset recognition — Partial
Purchase need/requisition → procurement and Purchase Order → physical receipt → Warehouse execution → Inventory acceptance → supplier invoice/matching → Asset eligibility and recognition decision → controlled Inventory-to-Asset conversion where applicable → Asset registration/custody → Accounting fixed-asset recognition/depreciation schedule under the applicable Book → reconcile Inventory, Asset and Accounting independently. Purchase receipt alone does not create an Asset, and operational Asset registration alone does not prove accounting capitalization.

### 12.6 Inventory recall / quality or expiry exception — Missing
Source/quality/expiry signal → identify affected Product/Variant, Batch/Lot/Serial and quantities → trace locations, movements, counterparties and affected transactions → place authorized stock/locations on hold or otherwise restrict use where policy permits → notify relevant authorized parties through approved communication channels → identify already issued/sold quantities → coordinate return/withdrawal/replacement/disposal through Sales/Purchase/Inventory/Warehouse → record financial/tax/accounting consequences in the owning domains → verify containment and reconciliation → close with evidence. Exact recall authority, statutory notification and product-specific obligations require separate policy/legal validation.

### 12.7 GST return liability-to-payment and accounting close — Partial
Period and GST Registration → compile/validate outward and inward supplies and other applicable tax facts → prepare applicable return/statement → reconcile portal auto-population and source records → authorize and file → retain acknowledgement/status → establish tax liability/payment reference → Finance execution/payment evidence where managed by CBM → GST compliance reconciliation → Accounting posting/control-account reconciliation → close period or retain exception. Filing, payment and accounting posting are separate outcomes.

### 12.8 Cash/Bank statement-to-payment-to-ledger reconciliation — Partial
Operational account/feed or statement → import and validate source statement → normalize transactions → match payment instructions/provider settlements/refunds/fees → identify unmatched, duplicate, reversed or timing items → resolve in Finance & Payment → separately prepare authorized accounting allocations/vouchers → reconcile Bank Ledger → approve close and preserve source statement and matching evidence.

### 12.9 Cross-Portal data exchange and source-system migration — Partial
Source discovery and authority → scope and purpose approval → mapping/schema/version approval → extract/stage/validate → tenant isolation and identifier/provenance checks → exception resolution → target-domain acceptance → reconciliation → controlled cutover/synchronization → revoke source credentials when appropriate → export/retain migration manifest and evidence. Portal-to-Portal exchange, external-system migration and Sub-Portal provider migration are distinct workflows.

### 12.10 Business closure and permanent deletion — Partial
Closure request/trigger → validate authority, ownership, Parent/children and relationship dependencies → close/resolve child entities and mappings as required → resolve open commercial, stock, payment, payroll, tax and accounting obligations through their owning domains → terminate subscription/access/service-provider links where applicable → export customer data → archive domain History and Audit Trail independently → check retention/legal holds → authorize permanent deletion only when eligible → verify deletion scope and preserve permitted deletion evidence. Business Entity Closed, Archived and permanently deleted are distinct concepts.

### 12.11 Configuration and policy change-to-effective-operation — Partial
Policy/configuration proposal → ownership, authority, applicability and Stable Core review → impact analysis against inherited/overridden settings and dependent workflows → approval/version → effective-dated publication → apply only to eligible contexts → handle in-flight workflows under a defined version policy → monitor → rollback/supersede/retire → preserve decision, version and affected-scope history.

---

# 13. Master glossary and approved History-domain coverage crosswalk

This crosswalk is intended to prevent a named concept in the Master Glossary from disappearing between the business baseline and the lifecycle catalogue. It maps concept families to the relevant lifecycle sections. Being mapped means that the concept has a lifecycle/workflow entry or an explicit owner-boundary reference; it does **not** mean every detailed state machine is complete.

| Master glossary / concept family | Lifecycle Reference coverage |
|---|---|
| CBM Ecosystem; Main Portal; Sub-Portal; Service Provider | Sections 3.1–3.2, 4.1, 11.1, 11.2, 12.2 |
| Organization; Tenant; Distributor; Shop; Business Structure; Business Structure Hierarchy and Structural Integrity; Customer Business Ownership Consistency | Sections 3.3–3.4, 11.1, 12.1 |
| Business Identity; Legal Identity; Professional Identity; Owner / Ownership Holder; Owner ID; Ownership ID; Ownership Link | Sections 3.5–3.8, 11.1–11.2 |
| Business Relationship; Mapping; Distributor–Shop Mapping | Sections 3.4, 11.3, 12.2 |
| Business Authority Scope; Business Available Authority Scope; User Assigned Authority Scope; User Scope Mapping | Sections 3.9–3.10, 11.5 |
| User; Customer Business User Account; User Code; Login Identification; Temporary Password; OTP; restricted password-setup session; No URL rule | Sections 3.10–3.11, 11.4 |
| Role; Permission; Delegated Authority; Approval Workflow | Sections 3.11, 4.4, 11.5 |
| Configuration; Default; Inheritance; Override; Applicability; Customization | Sections 4.2–4.4, 11.13, 12.11 |
| Business Data Isolation; Lifecycle Operation; Lifecycle Operation Snapshot; Parent-Caused Cascade; Lifecycle Restoration | Sections 3.3–3.4 and 7.3–7.5 |
| Commercial Account; Plan; Plan Version; Subscription; Subscription Coverage Assignment; Entitlement; Usage Limit; Add-on; Invoice; Payment | Sections 11.6, 11.15, 12.3 |
| Platform-wide Services; Platform-wide Service Scope; Service Category Group; Service Category; Platform-wide Service; Service Capability; Internal Platform Capability | Sections 4.1, 4.7–4.9, 11.13 |
| Global Business Settings | Sections 4.2–4.3, 12.11 |
| Accounting Management; Accounting Context; Book of Accounts; Accounting Framework; Accounting Policy; Accounting Basis; Chart of Accounts; Account Group; Ledger Account; Voucher Type; Accounting Voucher; Accounting Entry/Journal Entry; Posting; Sub-ledger and Outstanding Allocation; Cost Centre; Cost Category; Accounting Dimension; Accounting Period; Reconciliation | Sections 5.1 and 11.8 |
| Accounting Data Migration; Accounting Data Export and Audit Data Package; External Accounting Data Exchange Model; Tally Data Exchange Model; Tally Compatibility Adapter | Sections 5.1, 7.1–7.2, 11.14 |
| Fixed Asset Accounting; Management Accounting | Sections 5.1, 11.8, 12.5 |
| Business Module Catalogue; all fourteen Business Modules | Section 5.1–5.14; cross-module journeys in Sections 6 and 12 |
| Shared Domain Model; Shared Model Contract; Source Transaction Reference; External Identifier Mapping; Synchronization Contract | Sections 4.8–4.9, 11.14, 12.9 |
| External Connectivity Capability; External System; Application Client; Portal Interoperability | Sections 4.8–4.9, 11.12, 11.14, 12.9 |
| Product/Item Reference Model; Product Definition; Product Variant; Product Family/Group/Classification; Product/Item Instance; Product/Item Identifier; Data Carrier/Barcode; Product Attribute/Variant Dimension; Trade Packaging Configuration; Product Bundle/Kit; Batch/Lot | Sections 5.2 and 11.9–11.10 |
| Inventory Management; Inventory Position Model; Inventory Transaction/Stock Movement; Inventory Status | Sections 5.3 and 11.10 |
| Warehouse Management; Warehouse Facility; Storage Location/Storage Bin; Handling Unit; Warehouse Workstation/Counter; Warehouse Custody; Warehouse Storage Type/Section; Internal Warehouse Facility; External/Third-party Warehouse Facility; Business Location Reference | Sections 5.4 and 11.10 |
| Price Record; Price Type; Price List; Price Condition; Price Tax Basis; Price Determination; Price Snapshot; Declared Retail Price | Sections 5.2, 5.7, 5.8 and 11.9 |
| GST Taxpayer Profile; GST Registration; GST Registration Assignment; Place of Business; HSN/SAC Code; HSN/SAC Classification Assignment; GST Rate Applicability Rule; GST Tax Determination; Tax Determination Snapshot; GST Compliance/Filing Record | Sections 5.10 and 11.7 |
| Sales Quotation/Offer; Sales Agreement/Commercial Terms; Sales Order; Sales Invoice; Sales Return; Sales Commercial Credit/Debit Document | Sections 5.7, 6.1, 6.7, 12.7 |
| Purchase Requisition/Request; RFQ; Supplier Quotation; Purchase Agreement; Purchase Order; Purchase Invoice; Purchase Return; Purchase Commercial Receipt Confirmation | Sections 5.8, 6.2, 6.7, 12.5 |
| POS Terminal/Workstation; POS Operator Context; POS Session/Shift; Till/Cash Drawer; POS Checkout; Payment Instruction; Payment Transaction | Sections 5.9, 5.11, 6.8, 11.12 |
| Payment Settlement; Payment Reconciliation; Operational Cash/Bank Account | Sections 5.11, 11.15, 12.8 |
| Operational Asset; Asset Recognition/Inventory-to-Asset Conversion Event | Sections 5.12, 6.6, 12.5 |
| Business Plan; Budget; Forecast; Approved Planning Baseline | Sections 5.13, 6.9 |
| Party/Counterparty; Customer Role Profile; Supplier Role Profile; Counterparty Contact Record | Sections 5.14 and 11.11 |
| Service Offering/Supply Subject | Sections 11.9 and 12.1; originating service domain remains authoritative |

## 13.1 Approved Domain History register — all 25 domains

The Master document establishes twenty-five separate Domain History record sets. Every one is mapped below. These must remain separate record sets; this list must not be interpreted as permission to merge them.

| # | Approved Domain History record set | Lifecycle Reference coverage |
|---:|---|---|
| 1 | GST Registration History | Sections 5.10, 11.7 |
| 2 | Distributor–Shop Mapping History | Sections 3.4, 11.3 |
| 3 | Ownership History | Sections 3.8, 11.2 |
| 4 | User Scope Mapping History | Sections 3.10, 11.5 |
| 5 | Business Entity Lifecycle History | Sections 3.3–3.4, 12.10 |
| 6 | Accounting Domain History | Sections 5.1, 11.8 |
| 7 | Human Resource / Employment History | Section 5.5 |
| 8 | Payroll Domain History | Section 5.6 |
| 9 | Product/Item Catalogue History | Sections 5.2, 11.9 |
| 10 | Inventory Domain History | Sections 5.3, 11.10 |
| 11 | Warehouse Domain History | Sections 5.4, 11.10 |
| 12 | Product/Item Declared Price History | Sections 5.2, 11.9 |
| 13 | Sales Pricing History | Sections 5.7, 11.9 |
| 14 | Sales Commercial Document History | Section 5.7 |
| 15 | Purchase Pricing History | Sections 5.8, 11.9 |
| 16 | Purchase Commercial Document History | Section 5.8 |
| 17 | POS Operations History | Section 5.9 |
| 18 | HSN/SAC Registry History | Section 5.10 |
| 19 | Product/Service Tax Classification Assignment History | Section 5.10 |
| 20 | GST Rate Rule History | Section 5.10 |
| 21 | GST Compliance and Filing History | Sections 5.10, 11.7 |
| 22 | Finance & Payment Operations History | Sections 5.11, 11.15 |
| 23 | Asset Management History | Section 5.12 |
| 24 | Business Planning & Budget History | Section 5.13 |
| 25 | Counterparty Domain History | Section 5.14, 11.11 |

---

# 14. Research basis and application boundaries

The following primary/official materials were reviewed to validate lifecycle dimensions and workflow completeness. They inform the architecture-level sequence catalogue; they do not replace the CBM Master baseline or approve a CBM-specific policy.

1. **NIST SP 800-63B-4 — Digital Identity Guidelines: Authentication and Authenticator Management (published July 2025; supersedes SP 800-63B-3).** Relevant to authenticator binding, maintenance, loss/theft/compromise, expiration, invalidation and session management. The session guidance discusses explicit logout, inactivity/overall timeouts, reauthentication, session secrets and endpoint handling. Reference: [NIST publication](https://csrc.nist.gov/pubs/sp/800/63/B/4/final), [Authenticator Event Management](https://pages.nist.gov/800-63-4/sp800-63b/events/), and [Session Management](https://pages.nist.gov/800-63-4/sp800-63b/session/). NIST is used as a design reference, not claimed as an Indian statutory requirement for CBM.

2. **GST Portal — Official user guides.** The GSTR-1 guide documents period selection, preparation, record creation/amendment, summary generation, preview and filing with DSC/EVC, including acknowledgement/ARN. The GSTR-3B guide documents creation, saving, payment of tax and filing, and states that GSTR-3B amendment is not allowed. The registration guide documents application stages and verification; the non-core amendment guide documents a separate amendment process. References: [GSTR-1 creation and filing](https://tutorial.gst.gov.in/userguide/returns/Creation_of_Outward_Supplies_Return_in_GSTR-1.htm), [GSTR-3B creation, payment and filing](https://tutorial.gst.gov.in/userguide/returns/Create_and_Submit_GSTR3B.htm), [GST Registration application](https://tutorial.gst.gov.in/userguide/registration/Apply_for_Registration_Normal_Taxpayer.htm), and [Non-core registration amendments](https://tutorial.gst.gov.in/userguide/registration/Non-Core_Fields.htm). These are current process examples; forms, taxpayer applicability, due dates, correction routes and portal steps must be re-verified from official material at the time of detailed specification.

3. **Reserve Bank of India — Master Direction on Regulation of Payment Aggregators.** The RBI page identifies the direction and shows it updated as on 11 November 2026. It is relevant only to regulatory applicability assessment for entities actually carrying on regulated PA activity or providers in that role; a payment integration alone does not make CBM or every Tenant a Payment Aggregator. Reference: [RBI Master Directions](https://www.rbi.org.in/Scripts/BS_ViewMasDirections.aspx?id=12896).

4. **IFRS Foundation — IAS 2 Inventories.** Provides a reference taxonomy for inventory cost, cost formulas, write-downs and expense recognition. Reference: [IAS 2](https://www.ifrs.org/issued-standards/list-of-standards/ias-2-inventories/).

5. **IFRS Foundation — IAS 16 Property, Plant and Equipment.** Provides a reference taxonomy for asset recognition, initial cost measurement, depreciation, impairment and derecognition. Reference: [IAS 16](https://www.ifrs.org/issued-standards/list-of-standards/ias-16-property-plant-and-equipment/). IFRS is not made CBM's default accounting framework by this reference; the applicable Book framework and law govern.

6. **AWS Prescriptive Guidance — Transactional Outbox and Retry with Backoff patterns.** The outbox material highlights dual-write risk and duplicate event delivery/idempotent consumers; retry guidance distinguishes transient from non-transient failures and emphasizes idempotency. These patterns are technology-neutral research inputs for event recovery, not a mandate to use AWS. References: [Transactional Outbox](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html) and [Retry with Backoff](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/retry-backoff.html).

**Research limitation:** Official materials above do not resolve CBM-specific approval limits, business evidence policies, data-retention periods, module applicability, payroll/HR rules, GST rates, exact statutory due dates, pricing policy, session timeout values, or all edge-case transitions. Those remain to be researched and specified when the relevant domain specification is prepared. No value has been invented to fill those policy gaps.

---

# 15. Scope exclusions and conditional workflow register

A completeness audit must distinguish an omitted in-scope lifecycle from a capability that is not approved or is expressly optional. The following must not be added to the CBM baseline by implication:

- **CRM lead/pipeline and full CRM lifecycle:** not implied by Counterparty Management or Sales quotation workflows.
- **Manufacturing execution and universal Bill of Materials:** not implied by Product Bundles/Kits or Inventory.
- **Service Centre, repair-shop network or generic Service Operations Module:** not created by the Service Offering/SAC reference. Any service-delivery lifecycle belongs to an explicitly approved originating service domain.
- **Advanced Warehouse Management:** optional by applicability; if enabled, its detailed receiving, putaway, picking, packing, labour/task, wave, yard and exception workflows must be specified. Simple storage/godown functions must not be silently expanded into full WMS.
- **Recruitment, performance, disciplinary and advanced workforce capabilities:** only where the HR feature is approved and enabled; the Employee/Employment core lifecycle remains necessary.
- **Offline POS:** conditional; if enabled, offline sequencing, uniqueness, secure persistence, sync, idempotency, conflict handling and recovery are mandatory design topics.
- **Tax domains beyond the approved initial GST-focused scope:** TDS, Income Tax and other tax regimes require a separate scope decision.
- **Insurance underwriting/claims administration:** not implied by Asset insurance administration.
- **External system-specific workflows/customizations:** Tally/SAP and other systems remain adapters/data sources/destinations; they do not define CBM's canonical lifecycle or module ownership.
- **Unrestricted Portal-to-Portal replication:** never implied; only explicitly authorized and scoped interoperability contracts are allowed.

Where one of these capabilities is later approved, it becomes an applicability-controlled lifecycle/workflow requirement and must be added to the owner and cross-module registers before implementation.

---

# 16. Updated systematic completeness determination — 19 November 2026

## 16.1 What this review verified

The Reference was checked against:
- the Master Glossary and the complete 14-Module Catalogue;
- the Master lifecycle, subscription/billing, User/credential, mapping, migration, archival, and History/Audit sections;
- the Master Shared Model, Product, Pricing, HSN/SAC/GST, Inventory/Warehouse and transaction ownership boundaries;
- all twenty-five separately approved Domain History record sets;
- official source material for authentication/session, GST registration/returns, payment-aggregator regulatory scope, inventory and asset accounting topics, and distributed event retry/recovery.

## 16.2 Coverage status after this update

| Coverage layer | Result after review | Qualification |
|---|---|---|
| All fourteen approved Business Modules | Included | Their detailed state machines and exception policies remain at mixed maturity |
| Ecosystem, customer onboarding and business structure | Included | Eligibility, evidence, approvals and complete operational checklists remain partial |
| Identity, ownership and typed relationships | Included | Legal evidence, succession/dispute rules and some relationship policies remain open |
| User Account, credential, authenticator and session | Included as separate lifecycle families | Full Login Architecture and security values remain deliberately unapproved |
| Authority, scope, Roles, Permissions, Delegation and Approval | Included separately | Exact policy states, approver resolution and exception rules remain partial |
| Subscription, Plan, Add-on, Coverage, Entitlement, Usage Limit, invoice and payment | Included separately | Commercial pricing, limits, grace and billing policy details remain to be specified |
| Platform Services and operational capabilities | Included | Service-specific runbooks, targets and operational states remain partial |
| Accounting foundational objects and fixed-asset accounting | Included | Framework-specific recognition, close and reporting details require applicable policy |
| Product, identifiers, packaging, bundle/kit, attributes and prices | Included | Detailed attribute schema, packaging conversion and publication policies remain partial |
| Inventory and Warehouse operational subobjects | Included | Exact stock-state matrix, costing policy and warehouse exception states remain partial |
| Sales, Purchase, POS, GST/Tax, Finance, Asset, Planning, Counterparty | Included | Exact per-document states, legal/statutory and business-policy choices remain partial |
| Cross-module journeys | Expanded | Some reconciliation, migration, onboarding and closure journeys still need detailed specifications |
| Import, export, provider migration, offboarding, archive, restore and deletion | Included | Exact retention schedules, legal holds and deletion conditions are not invented |
| All 25 approved Domain History sets | Explicitly mapped | Their record schemas, access policies and retention controls still need detailed design |
| Research basis | Added | Only relevant authoritative/high-quality references reviewed; not every domain's implementation law researched in depth |

## 16.3 Remaining work that cannot responsibly be declared complete

The systematic review has closed material **catalogue-coverage gaps** by adding the lifecycle families and crosswalks above. It has not produced a complete field/state/transition specification for every object. The remaining work is detailed domain specification, with particular focus on:

1. Exact state lists, transitions, guards, terminal outcomes and actor/authority matrices for each object.
2. Complete exception paths for failure, partial completion, concurrency, timeout, retry, idempotency, compensation, manual recovery and reconciliation.
3. Exact cross-domain contract schemas, identifiers, event ordering/versioning, acknowledgement semantics and downstream completion evidence.
4. Applicable Indian legal/statutory, GST, payroll, employment, payment, records and retention requirements at the time of detailed design.
5. Final business decisions for unresolved commercial, approval, evidence, limits, deadlines, configuration precedence and security values.
6. Explicit data retention, legal hold, archival, restoration, export and permanent deletion specifications.
7. Service-specific platform operations, disaster recovery, incident/change runbooks and measurable service targets.

An unresolved item must remain labelled as such and must not be silently filled with an assumed rule.

## 16.4 Final conclusion

**Lifecycle and workflow catalogue coverage is now materially more complete and has been cross-referenced against the Master Glossary, fourteen Business Modules and all twenty-five approved Domain History sets.** This improves confidence that the principal in-scope lifecycle families have not been omitted from the Reference.

**The CBM lifecycle/workflow documentation as a whole is still not fully complete.** The Reference now provides a broader, traceable inventory and research basis, but the outstanding specifications listed in Section 16.3 must still be completed and reviewed before the project can be declared fully documented at lifecycle/workflow level. This is a deliberate accuracy boundary, not a claim that every missing detail can be settled without further owner policy or authoritative legal evidence.

