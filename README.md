# Central Business Management (CBM) Portal — Architecture Reference

This repository is the documentation foundation for the **Central Business Management (CBM) Portal** project.

## Authoritative source

The single authoritative source for finalized business and architectural decisions is:

- [`docs/CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md`](docs/CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md)

Read that document before drafting, reviewing, or implementing any project-specific architecture, security, data, API, user-experience, operational, or development document.

## Architecture-wide design doctrine

Every architecture and product decision must consider scalability, configurability, extensibility, customization, controlled override capability, and explicit applicability to the correct business contexts. Business-specific variation should be supported within one governed platform wherever appropriate.

Business and Tenant Data Isolation and the Business Structure Hierarchy and Structural Integrity are strict, non-configurable Stable Core constraints. The Customer Business Structure is Organization → Distributor / Shop; Distributor and Shop are terminal structural entities. Every Distributor and Shop must have exactly one direct Parent Organization. Within one Customer Business Architecture, the Owner ID set of the Organization must exactly match the Owner ID set of every direct Distributor and Shop child, while each Business Identity retains separate Ownership Links and Ownership IDs. Main Portal and Sub-Portal Service Provider ownership remains independent from customer ownership.

Distributor–Shop Mapping is separate from Structural Membership and Ownership. It requires two-way verification using a Shop-generated Secret Code followed by a separate Distributor-generated confirmation code. Both codes expire, are single-use, rate-limited, and revocable. An Active Mapping permits only Mapping-Type-authorized business transactions and necessary information exchange; it does not change ownership, structure, Parent Organization, or data boundaries.

Business Entity Lifecycle uses parent-child cascade rules and immutable pre-operation snapshots. Parent Organization suspension/deactivation affects its direct Distributor and Shop children according to the approved cascade policy; child state changes do not affect the Parent or siblings. Restoration returns only objects changed by the specific operation to their recorded previous states.

Subscription, Plan, Add-on, Entitlement, Coverage Assignment, Usage Limit, Invoice, and Payment are separate commercial concepts. Plans and Add-ons are configurable and versioned, not hard-coded. Subscription coverage is explicit; Subscription and Entitlement availability do not grant User access or Permissions and do not change Business Entity lifecycle states.

Customer Business User Accounts are Organization-scoped. The same natural person may have separate accounts in different Organizations, each with distinct verified email/mobile details and a separate portal-wide unique User Code. User Code is system-generated, immutable, and follows two uppercase English letters plus four digits. Initial credential establishment is Organization-configurable: Email Temporary Password, OTP Password Setup, or Both Required. URL/link transmission is prohibited for every CBM-generated outbound communication and purpose.

## Platform-wide Services Architecture

The approved Master baseline also defines **Platform-wide Services** as a distinct Architecture Layer, with four Service Scopes (Ecosystem-wide, Service Provider-scoped, Tenant-scoped, and Business-context-scoped), three Category Groups, and fifteen Categories. It distinguishes Category, Service, and Capability as conceptual levels and preserves the existing authority, authorization, security, approval, audit/history, and strict data-isolation boundaries. The Master document is authoritative for the complete taxonomy and responsibility rules.

## Global Business Settings

The approved product scope is India-only. English is the platform default; only Main Portal-enabled Indian languages may be supported. INR is the only currency and IST / Asia/Kolkata is the only time zone. Shared conventions use Indian number grouping, Indian regional/address references, and a Metric baseline with relevant India-use units. Gregorian is the core calendar; the default Financial Year is 1 April–31 March, with alternatives only where permitted by applicable domain rules and explicitly authorized.

Global Business Settings contains six domains: Language & Regional Language; Date, Time & Calendar; Indian Number & Currency; Indian Measurement & Units; Indian Regional & Address; and Indian Business Defaults. Main Portal governs mandatory platform boundaries and approved shared defaults. Sub-Portal settings remain within delegated Service Provider authority and are not a structural parent path to customer Organizations. Organizations govern eligible defaults for their direct Distributor/Shop children; child overrides and User presentation preferences require explicit applicability and authority.

Global Business Settings provides shared defaults and reference conventions; it does not own GST/Tax, Accounting, HR/Payroll, Inventory/Product, Finance/Payment, Business Identity, or other Business Module-specific rules and authoritative records. The Master document defines the complete domains, governance paths, inheritance/override controls, and integrity safeguards.


## Accounting Management

**Accounting Management** is an approved independent Business Module. Its canonical internal model is CBM-owned and uses Double-entry Accounting by default. It provides governed Accounting Contexts and Books of Accounts, Accounting Frameworks and Policies, a hierarchical Chart of Accounts, Account Groups, Ledger Accounts, Voucher Types, Accounting Vouchers, Journal Entries, General Ledger and Sub-ledger records, bill-wise/outstanding allocation, Cost Centres, Cost Categories, Accounting Dimensions, Accounting Periods, Reconciliation, Financial Statements, Fixed Asset Accounting, and Management Accounting capabilities.

Accrual basis is the default recognition basis for general-purpose Books; another basis may be used only where the applicable law and Accounting Framework permit it and an authorized policy enables it. Each statutory Book has exactly one accountable Legal Identity. Separate Legal Identities retain separate authoritative Books; Professional Identities and operating locations under one Legal Identity may be tracked through approved accounting dimensions where appropriate. Accounting records do not take ownership of Sales, Purchase, POS, Inventory, GST/Tax, Finance/Payment, HR/Payroll, Subscription/Billing, or other source-domain records.

The **External Accounting Data Exchange Model** is a vendor-neutral import/export boundary separate from CBM's canonical Accounting Model. Optional vendor-specific profiles/adapters, including a Tally exchange adapter where a customer has a supported need, translate only selected and documented data. They do not define CBM's Accounting concepts or require reproduction of external structures, workflows, customizations, or accounting methods. Accounting supports governed, selective migration; opening-balance and historical-data handling where appropriate; scoped exports; and permission-controlled CA/Auditor data packages. Import acceptance, posting, provenance, reconciliation, and source-domain ownership remain explicit and auditable.

The complete approved scope, ownership boundaries, Accounting Context/Books rules, migration and exchange safeguards, and applicable framework boundaries are defined in Section 6C of the Master document.


## Business Module Boundaries, Shared Models, and Connectivity

CBM distinguishes independent **Business Modules**, **Shared Domain Models/Contracts**, **Internal Platform Capabilities**, **Application Client Channels**, **External Connectivity Capabilities**, and **Cross-cutting Functions**. These are separate architecture classes. A shared model, platform service, report, client application, or connector does not automatically become a Business Module or acquire authority over a domain's records.

The currently established independent module responsibilities include **Accounting Management**, **Product/Item Management**, **Inventory Management**, **Warehouse Management**, **Human Resource Management**, and **Payroll Management**, each with its own authoritative business scope. **Product/Item Management** owns Product Definitions, Product Variants, catalogue attributes/classifications, typed product identifiers, Trade Packaging Configurations, Bundle/Kit definitions, and catalogue lifecycle under Section 6F. It does not own sales/purchase/POS transactions, transaction prices, stock ledgers/valuation, batch/serial inventory lifecycle, Warehouse execution, Accounting entries, or GST/Tax determinations. Sales, Purchase/Procurement, POS, GST/Tax, Finance/Payment, Asset Management, enterprise planning/budgeting, and counterparty-management responsibilities remain distinct domain areas for their respective module-boundary workstreams; they must not be absorbed by established modules merely because they share workflows or data.

### Internal Platform Capabilities

User/Identity lifecycle support, Authentication, User–Business association and scope support, Role Management, Permission Management, Delegated Authority evaluation, Approval execution, Configuration, Notification/Communication, Event & Messaging, API/Integration infrastructure, Audit/History infrastructure, Search/Reporting infrastructure, File/Document infrastructure, Background Processing, and Platform/Service Provider administration are provided through the existing approved Platform-wide Services Architecture in Section 6A and its governing sections. They are not duplicate Business Modules. User Account is not Employee; job title is not CBM Role; Role is not Permission; Authentication is not Authorization; Approval is not Permission.

### Shared Domain Models

Shared contracts include Business Identity references, User/Actor references, Product/Item references, units and packaging, Business Location, Inventory Position, Warehouse Location and Handling, Source Transaction/Document references, Accounting Posting, Workforce/Employment, External Identifier/Provenance, Event/Change, and File/Supporting Document references. Each identifies authoritative owner, permitted consumers and operations, versioning, provenance, and applicable context. Shared use does not create universal ownership, shared write access, or a competing source of truth.

### Product/Item Management

Product/Item Management is an approved independent Business Module. The shared Product/Item Reference Model distinguishes Product Definition, Product Variant, optional Product Family/Group and classification, Product/Item Instance, typed Product/Item Identifiers, machine-readable Data Carriers (including barcodes, QR, RFID and NFC), Trade Packaging Configurations, and Product Bundles/Kits. SKU, Product Code, GTIN, manufacturer/supplier/customer codes, PLU and other identifiers are separate typed values with explicit issuer, subject level, namespace, uniqueness scope and lifecycle. A Barcode/Data Carrier is not itself assumed to be the canonical identifier. Product Variants represent configured item forms, not individual physical units; unit-level records are created only where traceability or another valid business requirement needs them. Product attributes, variant dimensions, combinations, identifiers, packaging, and catalogue sharing are configurable under the Master document's authority, applicability, history, and strict tenant-isolation rules. Section 6F defines the full model and ownership boundaries.

### Inventory and Warehouse separation

Inventory Management owns authoritative stock quantities, stock states, reservations/availability, stock movements, reconciliation, and inventory valuation. Warehouse Management owns warehouse facilities/layout, storage locations, racks, shelves, bins, Handling Unit placement, warehouse tasks, and physical warehouse execution. A simple Inventory Godown/Storage Location is distinct from advanced Warehouse Management. Advanced WMS is optional by business applicability. Product/Item Management owns the product-side identity and catalogue definitions; Inventory owns operational stock/lot/serial tracking and Warehouse owns physical handling and placement.

Warehouse facility relationships do not use one ambiguous internal/external flag. Operational control/operator, geographic relationship, property/occupancy, inventory ownership, physical custody, and operational access are separate dimensions. Warehouse Counters/Workstations are operational work points, not automatically Storage Bins or POS Counters. Inventory and Warehouse cooperate through explicit Shared Models and contracts without duplicating the Inventory Ledger or transferring stock ownership.

### External Connectivity and Application Clients

External connectivity uses the existing Integration & API and Event & Messaging categories. It supports governed exchange with external business systems, optional vendor-specific adapters only where a documented and supported business need exists, approved devices/peripherals, first-party desktop/mobile/handheld Application Clients, authorized third-party clients, and future Portal-to-Portal interoperability. No named vendor or connector is mandatory, and external product structures do not define CBM's internal architecture. A first-party desktop/mobile app is an Application Client, not a Business Module. Portal-to-Portal exchange or central aggregation must preserve each source portal's authoritative records, identity, Tenant boundaries, permissions, purpose limitation, synchronization and conflict rules, provenance, retention, and revocation controls. Two-way unrestricted synchronization is never implied.

CBM's unified-portal objective is delivered by composing independent Business Modules and shared capabilities under explicit applicability, authorization, and data-ownership boundaries. The full ownership boundaries, Shared Model contracts, Inventory/Warehouse rules, internal capabilities, and connectivity safeguards are defined in Sections 6D and 6E of the Master document.

## External-System Onboarding, Data Portability, and Customer Offboarding

CBM must be designed from its own approved business requirements, not as a copy of Tally or another external product. External systems are data sources or destinations. Their optional modules, internal structures, workflows, customizations, accounting methods, inventory granularity, and warehouse concepts do not determine CBM's canonical Business Modules or Shared Models.

External-system onboarding imports only the selected, authorized, available, sufficiently reliable data needed to establish or continue business operations in CBM. The receiving CBM Module validates and owns accepted records. Import does not require copying every source record or reproducing source-specific customizations.

Missing or insufficient information must be identified rather than guessed. The customer must be able to enter or complete required information directly in CBM using the receiving Module's normal controls. For example, a customer whose previous system did not maintain Inventory can establish Products/Items and opening stock in CBM. An aggregate quantity of ten phones does not provide ten individual device identities or IMEI values; where serial/IMEI tracking is required, the customer must supply and validate the missing unit-level details before dependent processes rely on them. CBM must never fabricate such data.

CBM's approved domain rules remain authoritative after import. For example, source single-entry accounting does not require CBM to abandon its approved double-entry-by-default model. Conversion is performed only where reliable source facts and approved mappings are sufficient; otherwise gaps require explicit resolution. External-system customizations are not automatically recreated.

Customer offboarding must provide the selected CBM-owned data needed to continue the business elsewhere, in formats the intended destination is documented and validated to accept. This does not promise recreation of that destination's configuration, customizations, workflows, or proprietary backup. Data exchange must preserve scope, ownership, identifiers, provenance, validation, reconciliation, authorization, and auditability. Customer onboarding/offboarding with external systems is distinct from Sub-Portal/Service Provider migration under Section 16.

## Documentation rule

Only decisions explicitly finalized by the project owner belong in the authoritative master document. Discussion notes, rejected approaches, unapproved recommendations, and unresolved matters must not be presented as finalized decisions. Keep discussion history separate from the master source of truth.

## Current project stage

The documented baseline covers the finalized project vision and business-level concepts available as of **19 November 2026**, including the approved Business Entity Lifecycle, Subscription, Customer Business User Account, and Accounting Management rules. The wider Business Module Boundaries workstream remains in progress; approval of Accounting Management and Product/Item Management does not imply that every other potential Business Module has been classified or approved. This repository is documentation-only at this stage. It does not authorize application implementation, database changes, infrastructure changes, or deployment.