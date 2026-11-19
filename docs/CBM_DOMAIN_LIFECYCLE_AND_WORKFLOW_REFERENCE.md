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
