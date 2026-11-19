# Central Business Management (CBM) Portal
# Master Business & Architecture Decisions

| Document attribute | Value |
|---|---|
| Document status | Approved baseline of finalized decisions |
| Baseline date | 19 November 2026 |
| Document role | Single authoritative source for finalized business and architectural decisions captured to date |
| Project stage | Business concept and logical architecture definition |
| Implementation authorization | This document does not authorize software implementation or deployment |
| Language | English; CBM domain terms are retained consistently |

## Contents

1. [Purpose, authority, and use](#1-purpose-authority-and-use-of-this-document)
2. [Project vision and product scope](#2-project-vision-and-product-scope)
3. [Canonical terminology and definitions](#3-canonical-terminology-and-definitions)
4. [How the relationship model must be read](#3a-how-the-cbm-relationship-model-must-be-read)
5. [Stable Core principles](#4-stable-core-principles)
6. [Business entity and ownership model](#5-business-entity-and-ownership-model)
7. [Service-provider architecture](#6-service-provider-architecture)
8. [Platform-wide Services Architecture](#6a-platform-wide-services-architecture)
9. [Global Business Settings](#6b-global-business-settings)
10. [Accounting Management](#6c-accounting-management)
11. [Business Module Boundaries, Shared Models, and Connectivity Architecture](#6d-business-module-boundaries-shared-models-and-connectivity-architecture)
12. [Inventory Management and Warehouse Management Boundaries](#6e-inventory-management-and-warehouse-management-boundaries)
13. [Product/Item Management, Product Identity, Variants, Identifiers, and Trade Packaging](#6f-productitem-management-product-identity-variants-identifiers-and-trade-packaging)
14. [Pricing, Price References, Price Determination, and Price History](#6g-pricing-price-references-price-determination-and-price-history)
15. [HSN/SAC Classification, GST Rate Applicability, and Tax Determination](#6h-hsnsac-classification-gst-rate-applicability-and-tax-determination)
16. [Business Module Catalogue Completion and Domain Ownership Boundaries](#6i-business-module-catalogue-completion-and-domain-ownership-boundaries)
17. [Business Authority Scope](#7-business-authority-scope)
18. [Delegated Authority](#8-delegated-authority)
19. [Users, Roles, and Permissions](#9-users-roles-permissions-and-action-authorization)
20. [Configuration governance](#10-configuration-governance-defaults-inheritance-and-overrides)
21. [Approval Workflow governance](#11-approval-workflow-governance)
22. [Distributor–Shop relationships and mapping](#12-distributorshop-business-relationships-and-mapping)
23. [Business Entity Lifecycle and State Management](#13-business-entity-lifecycle-and-state-management)
24. [Subscription, Plans, Entitlements, and Billing](#14-subscription-plans-entitlements-and-billing)
25. [User Account Lifecycle and Initial Credential Establishment](#15-user-account-lifecycle-and-initial-credential-establishment)
26. [Sub-Portal migration and service continuity](#16-sub-portal-migration-and-service-continuity)
27. [Data archival and object-storage principles](#17-data-archival-and-object-storage-principles)
28. [Domain History and Audit Trail Management](#17a-domain-history-and-audit-trail-management)
29. [Stable Core, Configurable, Customizable, and Extensible principles](#18-stable-core-configurable-customizable-and-extensible-design-principles)
30. [Cross-concept invariants](#19-required-cross-concept-invariants)
31. [Illustrative scenarios](#20-illustrative-scenarios)
32. [Documentation governance](#21-documentation-governance)
33. [Source basis and decision provenance](#22-source-basis-and-decision-provenance)
---

## 1. Purpose, authority, and use of this document

### 1.1 Purpose

This document consolidates the project vision and all business-level and logical-architecture decisions explicitly finalized for the CBM Portal through 19 November 2026. It is the authoritative baseline from which subsequent project documentation is to be derived.

The document exists to prevent finalized decisions from being lost, contradicted, interpreted inconsistently, or omitted when later architecture and development documents are prepared.

### 1.2 Single source of truth

This document is the single authoritative source for the finalized concepts and rules recorded in it. A later document may explain or specialize a rule for a particular domain, but it must not contradict or silently redefine this baseline.

When a later discussion explicitly changes an approved decision:

1. The change must be clearly approved by the project owner.
2. This master document must be updated to reflect the new decision.
3. Any dependent documentation must then be reviewed and updated consistently.
4. The change history must be kept separately from the authoritative decision text.

### 1.3 Final-decision-only rule

Only explicitly finalized and approved decisions belong in this document. Unresolved questions, pending choices, unapproved proposals, rejected approaches, and discussion history are not part of the authoritative decision record.

The absence of a topic from this document must not be interpreted as approval of an implementation or technical design for that topic.

### 1.4 Interpretation rules

- Definitions in Section 3 govern the meaning of capitalized CBM domain terms used throughout this document.
- “Must” and “must not” express mandatory rules.
- “May” expresses an allowed capability, subject to all applicable higher-level rules and permissions.
- “For example” introduces an illustration, not an exhaustive list.
- Business ownership, business authority scope, user permission, service-provider responsibility, and business relationships are separate concepts. One must never be inferred automatically from another.
- No implementation detail, database structure, framework, programming language, or infrastructure choice is implied unless expressly stated.

---

## 2. Project vision and product scope

### 2.1 Project identity

The project is named **Central Business Management (CBM) Portal**.

CBM is a comprehensive SaaS-based business-management platform combined with a white-label service-provider ecosystem.

### 2.2 Product objective

CBM is intended to provide practical, user-friendly business-management services to different types of businesses while supporting independent service providers, strong business boundaries, centralized platform governance, and long-term expansion.

CBM's product objective is to provide a unified portal ecosystem through which an eligible business can access the broad set of business capabilities it needs in one coherent environment. This unified experience must be achieved by composing independently bounded Business Modules, Shared Domain Models, and Internal Platform Capabilities—not by merging domain responsibilities, creating duplicate sources of truth, or weakening ownership and isolation. The capabilities presented to each business must be governed by explicit applicability, Service/Subscription entitlements where relevant, authority, User Scope, Permissions, and configuration. The product must be extensible to new business needs without assuming that every capability is mandatory or applicable to every business.

### 2.3 Initial business-management capabilities

The product vision includes:

- Accounting Management.
- Product/Item Management.
- Inventory Management.
- Warehouse Management.
- Human Resource Management.
- Reports and Analytics.
- Point of Sale (POS).
- Additional practical business-management capabilities introduced in the future under Main Portal governance.

These are initial product capability areas, not a declaration that every broad capability, feature, screen, or service is automatically an independent Business Module. The definitive current independent Business Module classification is the fourteen-Module Catalogue in Section 6I. The distinction between Business Modules, Shared Domain Models, Internal Platform Capabilities, Application Client Channels, and External Connectivity Capabilities is governed by Sections 6D and 6I. Reports and Analytics infrastructure is shared, while the business meaning and authoritative calculations of each domain's reports remain with that domain.

### 2.4 Explicit current-scope exclusions

The current project vision excludes:

- E-commerce.
- Service Centre Management.
- Warranty Management.

These exclusions describe the current approved product vision. They are not an implementation decision about how any future scope change must be delivered.

### 2.5 Design qualities

The CBM architecture must support:

- Scalability.
- Configurability.
- Extensibility.
- Customization.
- Explicit applicability across different business types, business contexts, and operating conditions.
- Controlled override capability, subject to explicit authority and immutable system boundaries.
- Strong business-ownership boundaries.
- Strict business and tenant isolation.
- Centralized governance.
- Data integrity.
- Security.
- User-friendliness.
- Long-term maintainability.

### 2.6 Architecture-wide design doctrine

Every business concept, capability, module, workflow, integration, configuration model, and technical design must be evaluated deliberately against the following dimensions:

1. **Scalability:** Can the capability support growth in businesses, users, records, transactions, workload, and service-provider participation without fundamental redesign?
2. **Configurability:** Can legitimate differences in business policy and operation be expressed through governed settings rather than unnecessary code forks?
3. **Extensibility:** Can new capabilities, business types, workflows, integrations, and rules be added through clear boundaries and contracts?
4. **Customization:** Can a business adapt permitted behaviour, presentation, workflows, and operational choices to its own legitimate needs without changing another business's context?
5. **Override capability:** Can an authorized level override an inherited default or policy where explicitly permitted, while remaining within higher-level authority and immutable system limits?
6. **Applicability:** Can a feature, rule, configuration, workflow, or policy be applied to the correct business types, entities, users, roles, actions, locations, and circumstances—without unintentionally affecting contexts for which it is not intended?
7. **Business-specific variation:** Can different businesses use different permitted settings and workflows within the same CBM platform, without requiring separate product forks?
8. **Maintainability and operability:** Can these variations be understood, audited, tested, supported, and evolved without uncontrolled complexity?

These dimensions are design obligations, not a requirement to make every feature configurable. For each capability, the architecture must deliberately decide what is invariant, what is configurable, what is customizable, what may be overridden, and where it applies. A capability may remain fixed when variation would create security, integrity, consistency, or disproportionate complexity risks.

#### 2.6.1 Immutable boundary takes precedence

Business Data Isolation and Business Structure Hierarchy and Structural Integrity are strict Stable Core constraints. They must not be weakened, bypassed, or made subject to business-level customization, configuration, delegation, or override. All flexibility elsewhere must operate within these constraints, as well as Main Portal's supreme governance and other Stable Core rules.

An explicitly authorized business interaction does not merge the parties' data boundaries. It permits only the specific access or transaction allowed by the applicable relationship, authority, and permission rules.

#### 2.6.2 One platform, many legitimate business needs

CBM must be capable of supporting materially different business requirements through governed applicability, configuration, customization, and extension wherever appropriate. Differences between businesses must not automatically require separate product versions or code forks.

This flexibility must be designed intentionally and independently at the relevant business scope. A setting or customization for one business must not silently affect another business. Higher-level governance may establish mandatory defaults, limits, and policies; lower-level variation is permitted only where expressly authorized.

#### 2.6.3 No flexibility by implication

The existence of a configuration facility, extension point, customization mechanism, or override capability does not grant permission to use it. Every variation remains subject to applicable authority scope, explicit permission, approval requirements where configured, system-defined bounds, auditability, and Stable Core constraints.

#### 2.6.4 Separate domain responsibilities; compose through explicit relationships

A real-world business subject may have many different aspects, attributes, records, rules, and lifecycle events. CBM must not place all information concerning that subject into one oversized record, one universal master, or one Business Module merely because the information refers to the same Product, Service, Employee, Customer, Supplier, Asset, Transaction, or other subject.

Where aspects have different business purposes, authoritative owners, lifecycles, applicability, permissions, sensitivity, validation, history, or change frequency, they must be represented and managed by the appropriate separate domain model or responsible capability. Their relationship to the common subject must be established through explicit, typed, governed references and contracts. The common subject identity may serve as the stable reference point; it does not absorb ownership of every related fact.

For example, a Product Variant may be the common subject referenced by Product/Item attributes and specifications, Product-declared price records, Sales-owned selling-price records, Purchase-owned supplier-price records, Inventory-owned cost/valuation records, HSN/SAC classification assignments, and GST Rate Applicability Rules. Those records are related to the same Variant but remain independently owned and governed according to their business meaning. A consumer workflow may assemble the relevant views and calculations without merging their authoritative models.

This rule is not an instruction to create a separate Module or model for every individual field. Cohesive facts that share the same business responsibility, lifecycle, authority, and validation may remain together. Separation is required when it preserves meaningful domain ownership, independent configuration, customization, applicability, security, history, or evolution. Excessive fragmentation, duplicated sources of truth, circular dependencies, and untyped references are also prohibited.

A composed screen, report, workflow, calculation, or transaction may consume several domain models through explicit contracts. It must preserve each source owner's authority, version, provenance, permissions, correction lifecycle, and data-isolation boundary. A reference, association, or calculated result does not transfer ownership or create shared-write authority.

---

## 3. Canonical terminology and definitions

This section defines terms that must be used consistently. Similar-sounding terms are not interchangeable.

### 3.1 CBM Ecosystem

The **CBM Ecosystem** is the complete platform environment governed by Main Portal. It includes Main Portal, Sub-Portals, customer Organizations/Tenants, Distributors, Shops, their users, their permitted business relationships, and the services and data managed within CBM.

### 3.2 Main Portal

**Main Portal** is the central and permanently supreme platform authority and the primary SaaS service provider of the CBM Ecosystem. It governs the platform, establishes mandatory ecosystem rules, determines and controls subordinate authority, and may provide software services directly to customer businesses.

Main Portal is not merely another Sub-Portal. Its supreme authority is a permanent Stable Core principle.

### 3.3 Sub-Portal

A **Sub-Portal** is an independently owned and operated SaaS service-provider business operating within the CBM Ecosystem under authority delegated by Main Portal. It may have its own owner, service-provider identity, branding, and permitted customer operations.

A Sub-Portal is a service provider. It is not a Tenant and is not a customer Organization. It has no authority over another Sub-Portal and cannot establish or enlarge its own platform authority.

### 3.4 Service Provider

A **Service Provider** is the platform business that makes CBM software services available to a customer business. For a given customer service context, the provider is Main Portal directly or an authorized Sub-Portal.

Being a Service Provider does not make that provider the owner of the customer business, its data, or its assets. Service provision, business ownership, and platform authority are separate relationships.

### 3.5 Organization

An **Organization** is an independent customer business that uses CBM software services. It has its own Business Identity, ownership, business data, internal business structure, and associated users. It may receive services directly from Main Portal or through an authorized Sub-Portal.

An Organization may have multiple direct structural children of either or both permitted types: Distributor and Shop. Multiple Distributors may be grouped as a **Distribution Network**; multiple Shops may be grouped as a **Retail Network**. An Organization with both distribution and retail operations has a **Hybrid Network** configuration. These are operating configurations/groupings, not separate Business Identities, Owners, Service Providers, or customer businesses. Each Distributor and Shop retains its own Business Identity and separately recorded Ownership Links and Ownership IDs. Within that Organization's Customer Business Architecture, however, each child entity's Owner ID set must exactly match the Organization's Owner ID set under Sections 3.12 and 5.3.1; separate Ownership Links do not permit different Owner ID sets.

### 3.6 Tenant

A **Tenant** is the logical customer-service and isolation context within CBM for an Organization's use of the platform. Tenant is a service/data-boundary concept; it is not a Service Provider, Owner, User, or Business Relationship.

For the initial business model, an Organization has one primary Tenant context. The Organization's business identity and the Tenant's logical isolation purpose remain conceptually distinct even when associated one-to-one.

### 3.7 Business Identity

A **Business Identity** is the complete CBM identity representation of a business or identifiable business-operating context. It comprises two distinct identity dimensions: **Legal Identity** and **Professional Identity**. These dimensions serve different purposes and must not be treated as interchangeable. A Business Identity is distinct from its Owner, Users, Service Provider, Tenant context, Business Structure, and Business Relationships.

A **Legal Identity** represents the person or legal entity to which the relevant business activity legally belongs. It is represented using applicable legal particulars and identifiers, which may include legal name, legal form, incorporation/registration identifiers, PAN or other taxpayer identifier, applicable GST Taxpayer Profile and GST Registrations, registered address, and relevant statutory particulars. Legal Identity is not synonymous with a GSTIN: GSTIN identifies a particular GST Registration, and one legal taxpayer may have multiple registrations where applicable law permits or requires.

A **Professional Identity** represents how a business, service, location, branch, Shop, or other identifiable operating context presents and is recognized in actual commercial/professional activity. It may include its trading/business name, brand, branch/Shop name, business category, operating address/location, contact details, logo/branding, and other context-specific particulars. It may be independently maintained even when the relevant Legal Identity is shared with other Professional Identities.

Legal Identity and Professional Identity are separate but related. One Legal Identity may be associated with multiple distinct Professional Identities. For example, five Shops may each have their own Shop name, location, contact details, and Professional Identity while operating under one legal taxpayer and one applicable GST Registration. The shared legal/tax identity does not merge the Shops' distinct Professional Identities or operational records. Conversely, a Professional Identity must not be assumed to have a particular Legal Identity or GST Registration without an explicit, verified association.

This two-dimensional identity model applies to all eligible CBM identity-bearing contexts, not only customer businesses. Main Portal and each Sub-Portal may each maintain their own Legal Identity and Professional Identity for their respective platform/service-provider business. Organizations, Distributors, and Shops may each maintain the applicable Legal Identity and Professional Identity dimensions. A Shop's own Legal Identity is optional; its Professional Identity is required.

A Business Identity must not be replaced merely because the business changes its Service Provider or enters or leaves a Business Relationship. Changes to Legal Identity particulars or Professional Identity particulars must be handled as changes to the relevant identity dimension, with appropriate domain history and audit records; they must not silently merge distinct identities. Closed identity records must not be recycled for a different business or operating context.

#### 3.7.1 Identity dimensions and cardinality

1. One Legal Identity may be associated with multiple distinct Professional Identities, including multiple branches, Shops, or operating contexts.
2. A distinct Professional Identity does not automatically imply a distinct Legal Identity, legal person, GST Taxpayer Profile, GST Registration, Owner ID, or Ownership Link.
3. Shared Legal Identity does not merge distinct Professional Identities, operational records, locations, transactions, inventory, staff scopes, or business relationships.
4. The association between a Legal Identity and a Professional Identity must be explicit, validated, and effective-dated where appropriate. It must not be inferred solely from a shared Owner ID, similar name, common address, Business Structure, or Mapping.
5. The same two-dimensional model applies to Main Portal and Sub-Portals as service-provider businesses, and to customer/business entities and eligible operational contexts. A portal's Service Provider role does not remove the need to represent its own legal and professional identity.
6. Identity particulars must be maintained in the correct dimension. Legal names and statutory identifiers belong to Legal Identity; trading names, brand/branch/Shop presentation and operating contact/location particulars belong to Professional Identity, while shared or context-specific fields must be modelled without duplicating or conflating the two dimensions.

### 3.8 Owner / Ownership Holder

An **Owner** or **Ownership Holder** is the person or legal entity that owns a Business Identity. An owner is not the same thing as a User account. One owner may own multiple independent businesses. A business may have one or more ownership holders; ownership representation is separate from the assignment of operational Users and Roles.

#### 3.8.1 Owner ID

**Owner ID** is the unique CBM identifier for a person or legal entity acting as an Owner. It identifies the Owner independently of any Business Identity, Ownership Link, or User account. One Owner ID may be linked to multiple independent Business Identities. Owner ID is not a login credential or User ID.

#### 3.8.2 Ownership ID

**Ownership ID** is the unique identifier of one specific Ownership Link between an Owner ID and a Business Identity. Each distinct Owner-to-Business ownership relationship has its own Ownership ID. A Business Identity may have multiple Ownership IDs, and one Owner ID may participate in multiple Ownership Links across different businesses.

Ownership ID identifies the relationship only. It does not represent an equity percentage, financial share, investment amount, or economic interest. CBM's ownership model in this context records who owns or holds ownership rights in a business for business-authority purposes; it does not calculate financial ownership.

#### 3.8.3 Ownership Link

An **Ownership Link** is the explicit relationship connecting an Owner ID to a Business Identity, identified by an Ownership ID. Ownership Links are independent of Business Structure and Business Relationships. A Shop's mapping to a Distributor does not create or change an Ownership Link. An Ownership Link may establish an eligible basis for ownership-related authority scope under Section 7, but it does not by itself grant a User operational access or action Permissions.

### 3.9 Distributor

A **Distributor** is an identifiable distribution business entity whose direct structural parent is an Organization. An Organization may have multiple Distributor entities. A Distributor is a terminal structural entity under the currently approved Business Structure model: it cannot have a Shop, another Distributor, or any other business entity as a structural child. A Distributor must not exist as a standalone structural entity outside an Organization's Business Structure.

A Distributor must have exactly one direct Parent Organization as part of its Business Structure. That Parent Organization is mandatory when the Distributor is created; a Distributor must not be created as unassigned, standalone, or with multiple structural parents. Within the Customer Business Architecture, the Distributor's set of Owner IDs must be exactly the same as the set of Owner IDs linked to its Parent Organization. The ownership relationships are still represented separately for each Business Identity through distinct Ownership Links and Ownership IDs; the Organization entity itself is not the legal Owner. A Distributor must not have a different, additional, or partial Owner ID set from its Parent Organization. The Parent Organization relationship establishes structural membership and the mandatory ownership-consistency constraint; it does not determine Legal Identity, Professional Identity, or the Distributor's Business Relationships. A Distributor must have a Professional Identity. Its own Legal Identity is optional and must be explicitly established and validated where applicable; it is not automatically inherited from its Organization or another entity.

### 3.10 Shop

A **Shop** is the unified CBM retail business entity. The previous two-category classification of Shops has been discontinued. CBM must not model Shops as separate entity types or separate Business Structure classifications based on ownership.

A Shop must have exactly one direct Parent Organization as part of its Business Structure. That Parent Organization is mandatory when the Shop is created; a Shop must not be created as unassigned, standalone, or with multiple structural parents. Within the Customer Business Architecture, the Shop's set of Owner IDs must be exactly the same as the set of Owner IDs linked to its Parent Organization. The ownership relationships are still represented separately for each Business Identity through distinct Ownership Links and Ownership IDs; the Organization entity itself is not the legal Owner. A Shop must not have a different, additional, or partial Owner ID set from its Parent Organization. The Parent Organization relationship establishes structural membership and the mandatory ownership-consistency constraint; it does not determine Legal Identity, Professional Identity, or the Shop's Business Relationships. A Shop is a terminal structural entity under the currently approved Business Structure model: it cannot have another Shop, a Distributor, or any other business entity as a structural child. An Organization may have multiple Shops and may have both Shops and Distributors at the same time.

Every Shop must have its own Professional Identity. Its own Legal Identity is optional and must be explicitly established and validated where applicable. A Shop may have its own Legal Identity or may be associated with another eligible Legal Identity, subject to verified legal facts and applicable law. A Shop's Legal Identity may differ where legally appropriate, but its Owner ID set must remain consistent with its Parent Organization's Owner ID set.

The Shop entity remains the same regardless of its ownership arrangement, Legal Identity, Professional Identity, or Business Relationships. Those are separate dimensions and must not create separate Shop structural classifications.

### 3.11 Business Structure

**Business Structure** means the permitted structural placement and direct Parent–Child relationships among CBM's structural entities. The currently approved structure is Main Portal → Sub-Portal for Platform Structure, and Organization → Distributor / Shop for Customer Business Structure. Distributor and Shop are terminal structural entities. Service Provider relationships, Ownership Links, Business Identity associations, Business Relationships/Mapping, Authority Scope, User Scope Mapping, Roles, and Permissions are separate models and must not be inferred from structural placement.

### 3.12 Customer Business Ownership Consistency

**Customer Business Ownership Consistency** is the mandatory rule that an Organization and every Distributor and Shop directly within that Organization's Customer Business Structure must have exactly the same set of Owner IDs. Each Business Identity retains its own explicit Ownership Links and distinct Ownership IDs. The same Owner ID set is therefore recorded separately against the Organization, each Distributor, and each Shop. Different, additional, or partial Owner ID sets among these entities are not permitted within one Organization's Business Structure.

This rule applies only to the Customer Business Architecture. Main Portal and Sub-Portal remain separate Service Provider businesses and may have different ownership from one another and from customer Organizations. Service Provider ownership is not inherited from, and does not impose ownership on, a customer business.

### 3.13 Business Relationship

A **Business Relationship** is an explicitly established association between distinct business identities, such as a Distributor–Shop Mapping. A relationship enables only the interactions and permissions expressly allowed for that relationship. It does not transfer ownership or automatically grant broad administrative authority or data access.

### 3.14 Business Authority Scope

**Business Authority Scope** is the business-level authority boundary that identifies the business entities or business areas for which authority may be exercised or assigned. It answers: **“Where may this business-level authority be applied?”** It does not itself grant access to an individual User or define which actions that User may perform.

#### 3.14.1 Business Available Authority Scope

**Business Available Authority Scope** is the set of entities or business areas that a business-level authority is eligible to govern or make available for assignment, after applying Main Portal governance, valid Business Structure, applicable Ownership Links, permitted Business Relationships, and all relevant Stable Core constraints. It is the maximum eligible scope from which User-level scope may be assigned. Availability is not an automatic User grant.

#### 3.14.2 User Assigned Authority Scope

**User Assigned Authority Scope** is the specific set of entities, operational units, locations, or business areas explicitly assigned to a User through one or more User Scope Mappings, within a valid User–Business association and subject to the assigning authority's permitted scope. It may cover one entity, selected multiple entities, or the full scope the assigning authority is permitted to assign. Full-scope assignment must be explicit. It must never be inferred from a User's level, title, Role, ownership, business association, parent–child position, Business Available Authority Scope, or any other Mapping. Explicit exclusions may be applied where the governing assignment model permits them.

### 3.15 User

A **User** is an individual account identity that can authenticate to CBM. A User Account is a system account record, not a global identity that automatically spans multiple Customer Business Architectures. An Owner is not automatically a User, and ownership does not itself create a User Account or grant operational access.

#### 3.15.1 Customer Business User Account boundary

Within the Customer Business Architecture, each User Account must belong to exactly one Organization's Business Architecture. A single User Account must not be associated with two or more Organizations. The account may be authorized for relevant entities and contexts within its own Organization's Business Architecture, subject to the separately governed User Scope Mapping and Permissions rules.

The same natural person may work for more than one Organization. In that case, each Organization must have a separate User Account for that person. Those accounts are separate CBM account identities, with separate account records, credentials, User Codes, and contact identifiers. An account created within one Organization's Business Architecture must not be used to access or act within another Organization's Business Architecture. The accounts must not be merged into a shared cross-Organization Customer Business User identity.

For the same natural person, the email address and mobile number registered for one Organization-specific User Account must be different from the email address and mobile number registered for that person's User Account in another Organization. The distinct accounts must maintain their own verified contact details. This Customer Business account-boundary rule does not redefine the separate account and authority arrangements of Main Portal or Sub-Portal Service Provider personnel.

#### 3.15.2 User Code

A **User Code** is a system-generated, immutable, portal-wide unique identifier assigned to each CBM User Account. It is exactly six characters long and follows this fixed pattern:

- The first two characters must be uppercase English letters A–Z.
- The following four characters must be decimal digits 0–9.
- No spaces, punctuation, lowercase letters, or additional characters are permitted.
- Example format: AB1234.

The User Code must be generated automatically by CBM. A User, Organization, Service Provider, or administrator must not create, choose, edit, customize, or replace it. The User Code is read-only and may only be viewed by appropriately authorized parties in the relevant account context. It never changes during that account's lifetime and must not be reassigned to another account after termination.

Uniqueness applies across the entire CBM Portal, not merely within one Organization or Service Provider. Each separate User Account belonging to the same natural person therefore receives a different User Code. The User Code is an identifier, not a password, secret, authenticator, or authorization grant.

The fixed format provides a finite namespace of 6,760,000 possible values (26 × 26 × 10,000). The system must allocate codes with collision-safe uniqueness enforcement, monitor remaining capacity, and never reuse an assigned code. If the available namespace is exhausted, allocation must fail safely; the format must not be silently altered or bypassed.

#### 3.15.3 Initial credential establishment methods

A newly provisioned Customer Business User Account must establish its permanent password through the Organization's configured account-provisioning method. The Organization may configure one of the following modes:

1. **Email Temporary Password:** CBM generates a temporary bootstrap password and sends it to the account's registered email address. It may be used only to enter the restricted initial password-establishment process. It must not grant normal business access.
2. **OTP Password Setup:** CBM sends a one-time verification code through an enabled and verified channel—Email, SMS to the registered mobile number, or WhatsApp to the registered mobile number. The User enters the OTP within the CBM application and, after successful verification, establishes a permanent password. No URL or link is used.
3. **Both Required:** Both the Email Temporary Password method and the OTP Password Setup method must be successfully completed for initial credential establishment. They are cumulative requirements, not alternative choices in this mode.

The selected Organization configuration governs new account provisioning. A configuration change must not silently reset or invalidate the established credentials of existing accounts. Any later re-verification or credential replacement must use a separately authorized credential lifecycle action.

#### 3.15.4 Temporary bootstrap password

A Temporary Password is a single-purpose bootstrap credential. It is not a normal reusable password and must not grant access to business modules, dashboards, business data, or ordinary account functions. It may only establish or resume the restricted password-change process.

If password validation or password storage fails, the Temporary Password must not be revoked solely because that attempt failed. It remains usable only for the restricted password-establishment process until the permanent password is successfully stored, or until the Temporary Password expires or is explicitly revoked. Once the permanent password is successfully stored, the Temporary Password must immediately become invalid and must never be usable again.

Temporary Passwords must be generated securely, transmitted only through the configured email mechanism, never written to application logs, and never stored as plaintext in the database. Passwords must be stored using a suitable adaptive password-hashing mechanism.

#### 3.15.5 OTP and restricted password-setup session

An OTP is a short-lived verification secret issued for a specific User Account and a specific verification purpose. OTPs must be cryptographically generated, protected against guessing and replay, single-use, time-limited, rate-limited, and invalidated after successful verification. OTP values must not be written to application logs or retained as long-term plaintext.

Successful OTP verification may establish a restricted password-setup session that permits only the required permanent-password creation and confirmation actions. If saving the permanent password fails, the User may retry within that restricted session without reusing the already-consumed OTP. If the restricted session expires or is revoked before password establishment succeeds, a new valid OTP verification is required. The restricted session must never grant ordinary business access.

OTP delivery through Email, SMS, or WhatsApp is an approved CBM capability, but OTP is not assumed to be compromise-proof. The Organization chooses the initial-credential method mode; applicable security controls must mitigate interception, guessing, replay, and account-recovery abuse.

#### 3.15.6 No URL or link transmission

CBM must never send a URL, hyperlink, clickable link, URL-bearing button, QR code that resolves to a URL, or equivalent link-based action to any person for any reason or through any outbound communication channel. This prohibition applies to all CBM-generated email, SMS, WhatsApp, notifications, account provisioning, verification, recovery, invitation-like communication, and future communication purposes.

CBM must not implement invitation links, registration links, activation links, password-reset links, magic-login links, file-download links, or link-based verification. Where a User action or verification is required, the User must open the CBM application independently and complete the action there using the applicable credentials, OTP, or other approved in-application process. This is a platform-wide, non-overridable White-label security and product rule.

#### 3.15.7 Login identification baseline

The approved login-identification concept is:

User Code + (Email Address OR Mobile Number OR Username) + Password

The User Code identifies the specific User Account across the CBM Portal, while the accompanying account identifier must belong to that same account. This statement records the required account-identification inputs only. Detailed login channels, authentication sequences, session behaviour, and the complete Login Architecture are governed by a separate discussion and must not be inferred from this baseline.

### 3.16 Role

A **Role** is a named set of responsibilities or access functions assigned to a User within a defined business context. A Role is not a Business Identity and does not itself expand the business authority scope.

### 3.17 Permission

A **Permission** is an authorization for a User, through an applicable Role or a User-specific assignment, to perform a defined action within an allowed business scope. It answers: **“What may this User do?”**

Examples include View, Create, Update, Delete, Approve, Reject, Export, and Configure. The supported permission catalogue may be extended as the product's capabilities grow.

### 3.18 Delegated Authority

**Delegated Authority** is a defined grant of authority from a higher-authority business/platform level to an eligible recipient, constrained by the delegator's own authority and by explicit scope, action, limits, and duration.

Every delegation is defined using five dimensions: Who, What, Where, Limits, and Duration.

### 3.19 Configuration

A **Configuration** is a business or operational setting that controls system behaviour within the platform's immutable boundaries and the authority of the business configuring it.

### 3.20 Default, Inheritance, and Override

- **Default Value:** The value established at a governing level when no lower-level permitted override applies.
- **Inheritance:** The top-down application of an applicable higher-level configuration to subordinate businesses.
- **Override:** A permitted, business-specific change to an inherited/default value, within all system-defined non-overridable limits.

### 3.21 Approval Workflow

An **Approval Workflow** is a configurable control that determines whether an action must receive approval, who may approve it, how many approvals are needed, and whether approvals are obtained sequentially or in parallel.

Approval is a control layered on top of permission. Approval does not create a permission that the initiating User does not possess.

### 3.22 Business Data Isolation

**Business Data Isolation** means that business records and information are separated by their owning business/Tenant context and are accessible only through valid authority, User access, Permission, and relationship rules. A common owner, Service Provider, network membership, or mapping does not by itself merge separate data boundaries. Explicitly authorized access is limited to its approved purpose and scope; it does not merge the underlying businesses' data boundaries.

### 3.23 Applicability

**Applicability** defines the business contexts, entity types, scopes, conditions, actions, Users, Roles, or operating circumstances to which a feature, configuration, policy, permission, or workflow applies. Applicability must be explicit enough to prevent a rule intended for one business context from unintentionally affecting another.

### 3.24 Customization

**Customization** is an authorized adaptation of permitted product behaviour, presentation, workflow, or operational choices to meet a business's legitimate requirements. Customization must remain within Main Portal governance, the applicable authority and permission model, and all Stable Core constraints. It does not mean changing immutable platform rules or another business's data or settings.

### 3.25 Business Structure Hierarchy and Structural Integrity

**Business Structure Hierarchy and Structural Integrity** is the strict, non-configurable rule governing the permitted structural levels, entity placement, and parent–child relationships among Main Portal, Sub-Portal, Organization, Distributor, and Shop. Only explicitly valid structures defined by the CBM business model are permitted. A structurally invalid arrangement cannot be created, configured, customized, delegated, or overridden into validity.

The following arrangements are expressly invalid: a Distributor placed under a Shop; an Organization placed under a Distributor; a Distributor or Shop existing independently without belonging to an Organization's business structure; and a Distributor or Shop assigned to more than one Parent Organization. Every Distributor and Shop must have exactly one direct Parent Organization. The hierarchy must not be inverted, bypassed, or treated as an arbitrary user-configurable tree.

This is a business-structure rule only. It has no meaning or application in cryptography, encryption, encryption-key management, or key derivation. The service-provider relationship, Organization business structure, ownership relationship, and Distributor–Shop business mapping remain distinct relationship views; one must not be inferred from another.

### 3.26 Mapping

**Mapping** is a typed, purpose-specific, explicitly established association between defined source and target entity types or contexts. The unqualified term does not imply a particular pair of entity types, direction, purpose, eligibility, authority, or effect. Every Mapping type must have its own definition of purpose, endpoints, direction, cardinality, eligibility, authorization, lifecycle, effective dates, applicable History, and access controls. Rules for one Mapping type must not be generalized to another.

**Distributor–Shop Mapping** is one specific Business Relationship Mapping type defined in Section 12. It is not the universal Mapping model and does not limit Mapping or Assignment capabilities elsewhere in CBM. Other separately defined types may associate other eligible CBM entity types or contexts, including Main Portal, Sub-Portals, Organizations, Distributors, Shops, Users, GST Registrations, and future approved entity types. This illustrative list does not itself authorize any association. Where ambiguity is possible, use a complete qualified term such as Distributor–Shop Mapping, User Scope Mapping, or GST Registration Assignment.

### 3.27 User Scope Mapping

**User Scope Mapping** is the explicit assignment relationship identifying the CBM entities, operational units, locations, or business areas within which a specific User may act, within a valid User–Business association and the assigning authority's permitted scope. Business Available Authority Scope defines the business-level authority boundary; it does not automatically assign every eligible entity or location to every User. User Scope Mapping determines the specific scope assigned to that User. Full-scope assignment must be deliberate and explicit. Roles and Permissions independently determine permitted actions within the assigned scope.

### 3.28 Domain History

**Domain History**, also called Business History or History in a specific domain, is the structured, chronological record of meaningful business events and changes for one defined domain object or relationship, maintained for business reference. Each History record set must contain only events and information belonging to that domain. For example, GST Registration History records GST Registration changes and must not contain unrelated User ID or User profile changes.

History is distinct from the Audit Trail. It is maintained throughout the existence of the relevant Business Identity/business context and must not be routinely purged while that business remains in existence. It may be removed only as part of authorized permanent deletion of the relevant business. History visibility is controlled by its own business permissions and scope; it is not automatically restricted to Audit Trail administrators.

### 3.29 Audit Trail

**Audit Trail** is the comprehensive, chronological record of system events and activities across CBM, including successful and failed actions, changes, access and authentication events, administrative operations, approvals, security events, and other system or User activities required to reconstruct what happened, when, in which context, by whom or by what process, and with what outcome. It may contain events from multiple domains in chronological order; it is not divided into business-facing domain Histories.

Audit Trail access is separately permissioned and more restricted than ordinary business History access. Its retention is governed by a separate retention and archival policy. Audit Trail records are not required to be retained permanently in every case, subject to applicable legal, regulatory, contractual, and security requirements.

### 3.30 GST Taxpayer Profile

A **GST Taxpayer Profile** represents the actual person or legal entity registered, or intended to be registered, as a taxpayer under applicable GST law. It is distinct from Owner ID, Business Identity, User ID, and every CBM portal or business entity record.

### 3.31 GST Registration

A **GST Registration** is a specific registration granted to a taxpayer under applicable GST law and identified by its GSTIN or other applicable registration identifier. Each registration is an independently identifiable record with its own jurisdiction, registration particulars, official status, effective dates, and statutory consequences.

### 3.32 GST Registration Assignment

A **GST Registration Assignment** is the explicit, typed association by which a specific GST Registration is associated with an eligible CBM entity, operational unit, location, or transaction context for a defined GST purpose, subject to verified taxpayer identity, lawful registration coverage, applicable tax rules, and Main Portal governance. Its target is not restricted to Distributor or Shop. It may be any CBM entity type or context enabled by Main Portal, including Main Portal, Sub-Portal, Organization, Distributor, Shop, or another approved current or future context. This list is illustrative, not an automatic eligibility grant.

### 3.33 Place of Business

A **Place of Business** for GST purposes is a location recognized and recorded in relation to a GST Registration under applicable GST law and official registration records. A CBM operational location is not automatically a GST-registered Place of Business merely because it exists in the Business Structure or participates in a Mapping.

### 3.34 Business Entity Lifecycle

**Business Entity Lifecycle** is the governed progression of a Business Entity through its permitted operational states, including creation, verification where required, activation, suspension, deactivation, closure, and subsequent archival. Entity Lifecycle is distinct from Subscription Lifecycle, User Account Lifecycle, Ownership Link Lifecycle, Business Relationship Lifecycle, Service Provider Relationship Lifecycle, and other domain-specific lifecycles.

### 3.35 Lifecycle Operation

A **Lifecycle Operation** is one authorized, identifiable action that changes the lifecycle state of an entity and, where an approved cascade policy applies, the state of its affected structural children or relationships. A Lifecycle Operation has its own unique operation identifier, initiating context, reason, actor or system process, approval where required, effective time, outcome, and related snapshot reference.

### 3.36 Lifecycle Operation Snapshot

A **Lifecycle Operation Snapshot** is an immutable record of the relevant pre-operation state of the entities and relationships within the scope of a Lifecycle Operation. It records enough information to identify which objects were changed by that operation, which objects were already in a different state, and what state must be considered for a safe restoration. It is not a database backup and does not replace Domain History or Audit Trail.

### 3.37 Parent-Caused Cascade

A **Parent-Caused Cascade** is an explicitly defined lifecycle effect in which a state change to a Parent Organization causes specified state changes to its direct structural Distributor and Shop children and to specified relationships involving those children. It applies only where the relevant lifecycle policy expressly defines the cascade. It must not be inferred for Service Provider relationships or unrelated Business Relationships.

### 3.38 Lifecycle Restoration

**Lifecycle Restoration** is the controlled reversal of state changes caused by a specific earlier Lifecycle Operation. Restoration uses that operation's immutable snapshot and operation identity. It restores only objects changed by that operation and must not overwrite a pre-existing state or a later independent lifecycle change.

### 3.39 Commercial Account

A **Commercial Account** is the commercial contract and billing reference through which a customer receives CBM services from one Service Provider. It identifies the applicable contracting/billing context and may have one or more Subscriptions. It is not an Owner ID, User Account, Business Identity, or Business Structure entity.

### 3.40 Plan and Plan Version

A **Plan** is a configurable commercial package of CBM service entitlements and commercial terms. A **Plan Version** is an identifiable, published version of that Plan with its own defined features, Entitlements, Usage Limits, compatibility rules, and commercial terms. Published Plan Versions are preserved for historical Subscription interpretation; changing a published Plan requires a new version rather than silently changing existing contractual meaning.

### 3.41 Subscription

A **Subscription** is the time-bounded or continuing service agreement applied to a Commercial Account under a particular Plan Version and its approved Add-ons, coverage, billing terms, and effective dates. Subscription status controls service entitlement availability; it does not itself create User access, action Permissions, ownership, or Business Entity lifecycle changes.

### 3.42 Subscription Coverage Assignment

A **Subscription Coverage Assignment** explicitly identifies the CBM business entity or entities covered by a Subscription and the effective period and coverage rule. Coverage is not inferred from Ownership, Parent–Child structure, Service Provider association, or Business Mapping.

### 3.43 Entitlement

An **Entitlement** is a defined feature, service capability, or permitted product capacity made available through an eligible Subscription and its Coverage Assignment. An Entitlement does not replace User Assigned Authority Scope or action Permissions.

### 3.44 Usage Limit

A **Usage Limit** is a measurable upper bound or threshold for an Entitlement, such as a quantity, capacity, transaction count, storage amount, or other governed usage metric. Its measurement scope, period, aggregation, and enforcement behaviour must be explicitly defined.

### 3.45 Add-on

An **Add-on** is a separately defined and versioned commercial extension that may add or extend specified Entitlements or Usage Limits beyond the base Plan. It is not a hard-coded package and may be applied only where compatible with the relevant Plan and Subscription.

### 3.46 Invoice

An **Invoice** is a financial document recording a charge or amount due under a Commercial Account or Subscription arrangement. Invoice status and settlement are separate from Subscription service status.

### 3.47 Payment

A **Payment** is a financial settlement transaction made against an Invoice or another valid commercial obligation. Payment processing status, Invoice status, and Subscription status are separate records and must not be conflated.

---

### 3.48 Platform-wide Services

**Platform-wide Services** is the distinct Architecture Layer and Capability Group that provides shared, governed capabilities for use across multiple CBM business-management modules and authorized service-provider, Tenant, and business contexts. It supports common responsibilities without taking ownership of the relevant Business Module's domain rules or authoritative business records.

### 3.49 Platform-wide Service Scope

A **Platform-wide Service Scope** classifies the ecosystem or business context in which a Platform-wide Service is governed, made available, or applied. The approved scopes are Ecosystem-wide, Service Provider-scoped, Tenant-scoped, and Business-context-scoped. A Service Scope is not Business Authority Scope, User Assigned Authority Scope, ownership, or a Permission and does not itself grant access.

### 3.50 Service Category Group

A **Service Category Group** is a high-level classification grouping related Platform-wide Service Categories by their broad architectural responsibility. The approved groups are Platform Foundation Services, Shared Functional Services, and Platform Operations & Governance Services.

### 3.51 Service Category

A **Service Category** is a governed functional grouping of related Platform-wide Service responsibilities within one approved Service Category Group. The approved Category taxonomy and responsibility boundaries are defined in Section 6A.

### 3.52 Platform-wide Service

A **Platform-wide Service** is a defined shared responsibility or cohesive service offering within an approved Service Category. It has an explicit purpose, scope, consumers, inputs/outputs, ownership, policy boundaries, and service contract. It is a conceptual architecture responsibility and does not imply a separate deployment unit.

### 3.53 Service Capability

A **Service Capability** is a specific function or operation made available by a Platform-wide Service to its authorized consumers under the applicable service contract, Service Scope, and authorization controls.

### 3.54 Global Business Settings

**Global Business Settings** is the governed set of shared India-specific business and regional defaults, preferences, and reference settings made available across eligible CBM contexts. It provides common defaults and presentation/reference conventions; it does not take ownership of domain-specific business rules, statutory determinations, or authoritative records belonging to Business Modules.



### 3.55 Accounting Management

**Accounting Management** is an independent Business Module responsible for the governed recognition, classification, recording, posting, control, reconciliation, and financial reporting of the financial effects of business activities within its defined Accounting Contexts and Books of Accounts. It owns its Accounting Policies, Chart of Accounts, Account Groups, Ledger Accounts, Voucher Types, Accounting Vouchers, Journal Entries, Ledger Postings, Accounting Period controls, and authoritative accounting records. It does not take ownership of the originating business transactions or domain rules of other Business Modules.

### 3.56 Accounting Context

An **Accounting Context** is the governed business and legal scope within which one or more Books of Accounts are maintained. It identifies the accountable Business Identity and Legal Identity, applicable Professional Identity or operating scopes, accounting purpose, governing Accounting Framework and Policies, authorized Books, and applicable authority and access boundaries. An Accounting Context is not automatically identical to an Organization, Tenant, Distributor, Shop, Service Provider, Owner, or User.

### 3.57 Book of Accounts / Accounting Book

A **Book of Accounts** or **Accounting Book** is a governed, internally coherent set of accounting records for a defined accounting purpose and accountable legal/business context. It includes its Chart of Accounts, Accounting Vouchers, Journal Entries, Ledger Postings, Accounting Periods, balances, and required supporting references. A statutory Book must have exactly one accountable Legal Identity. Separate Legal Identities must not share or merge their authoritative statutory Books. Additional management or purpose-specific Books may be maintained only with an explicit purpose, policy, ownership, and reconciliation relationship that prevents competing or conflicting authoritative records.

### 3.58 Accounting Framework

An **Accounting Framework** is the set of accounting standards, presentation requirements, recognition and measurement principles, and related authoritative guidance applicable to a particular Accounting Context or Book. For Indian businesses, the applicable framework may include Indian Accounting Standards (Ind AS), Accounting Standards (AS), or another framework permitted or required for the relevant entity and purpose under applicable Indian law and authoritative regulation. Framework applicability is determined by the actual legal and regulatory status of the entity; it is not an unrestricted User preference.

### 3.59 Accounting Policy

An **Accounting Policy** is a governed selection or application of permitted accounting principles, recognition methods, measurement methods, classifications, and presentation practices within the applicable Accounting Framework. Policies must be effective-dated, versioned, authorized, and consistent with mandatory legal and framework requirements. A configurable policy cannot override an applicable statutory or regulatory requirement.

### 3.60 Accounting Basis

An **Accounting Basis** determines when financial effects are recognized in the Books of Accounts. The default recognition basis for CBM's general-purpose Accounting Books is accrual basis. A cash basis or other permitted treatment may be used only for an entity, report, or purpose for which the applicable law and Accounting Framework allow it and an authorized, effective-dated Accounting Policy expressly enables it. A cash-oriented report or view must not silently change the authoritative recognition basis of statutory Books.

### 3.61 Chart of Accounts

A **Chart of Accounts** is the governed, hierarchical classification structure used by an Accounting Book to organize its Account Groups and Ledger Accounts. It supports standardized baseline classifications and authorized business-specific extensions without weakening the minimum classification, reporting, and integrity requirements of the applicable Accounting Framework.

### 3.62 Account Group

An **Account Group** is a classification node within a Chart of Accounts under which one or more subordinate Groups or Ledger Accounts are organized according to their accounting nature and reporting treatment. Groups may be hierarchical. System-defined foundational classifications remain protected; authorized business-specific Groups may be added within the applicable framework and reporting rules.

### 3.63 Ledger Account

A **Ledger Account** is an individual account within a Chart of Accounts to which one or more Accounting Entries are posted and from which balances and transaction histories are derived. A Ledger Account may be associated with a relevant Business Identity, Customer, Supplier, Bank Account, Tax reference, or other domain record, but such association does not transfer ownership of that source record to Accounting.

### 3.64 Voucher Type

A **Voucher Type** classifies an Accounting Voucher by its accounting purpose and governs its permitted structure, numbering, validation, approval, and posting behaviour. CBM may provide protected system-defined Voucher Types and allow authorized, governed custom Voucher Types. A custom Voucher Type must inherit or explicitly satisfy mandatory Accounting Controls and cannot bypass Double-entry, authorization, or source-domain rules.

### 3.65 Accounting Voucher

An **Accounting Voucher** is an authoritative Accounting record representing a recognized financial transaction, adjustment, opening entry, or other permitted accounting event within a Book of Accounts. It has a Voucher Type, date and period references, one or more Accounting Entry Lines, supporting references, provenance, and a controlled lifecycle. A Voucher may originate from an authorized manual accounting action or from a source transaction owned by another Business Module. It is not automatically the same record as a Sales Invoice, Purchase Invoice, Payment, POS receipt, or other originating business document.

### 3.66 Accounting Entry / Journal Entry

An **Accounting Entry** or **Journal Entry** is the balanced set of debit and credit lines representing the accounting effect of an Accounting Voucher. Each line identifies its Ledger Account, debit or credit amount, applicable Accounting Book, and permitted analytical or source references. One Voucher may contain multiple debit and credit lines. The total debit and total credit of every posted entry set must balance.

### 3.67 Posting

**Posting** is the controlled act by which a validated and, where required, approved Accounting Voucher becomes part of the authoritative balances and transaction history of its Book of Accounts. Draft, validated, pending-approval, posted, and reversal/correction conditions are distinct. Posted records must not be silently overwritten or destructively edited; corrections must preserve the original record and use authorized reversal, adjustment, or correction records.

### 3.68 Sub-ledger and Outstanding Allocation

A **Sub-ledger** is a detailed accounting record set that supports and reconciles to a corresponding control Ledger Account, such as customer receivables or supplier payables. **Outstanding Allocation** links receipts, payments, credit notes, advances, and other permitted settlement amounts to the relevant open references or obligations. It may include bill-wise references such as new reference, against reference, advance, and on-account allocation. The original commercial invoice or obligation remains owned by its originating Business Module.

### 3.69 Cost Centre

A **Cost Centre** is an Accounting analytical unit used to attribute, accumulate, and report income or expenditure for a department, branch, project, activity, person, operating unit, or other approved management dimension. A Cost Centre is not a Business Entity, Legal Identity, Structural Parent/Child relationship, Tenant, Ownership Link, or User Scope.

### 3.70 Cost Category

A **Cost Category** is a governed analytical classification that enables parallel or alternative allocation of eligible income and expenditure across defined Cost Centres or equivalent analytical units. It does not create a new Business Entity or change the authoritative ownership of the underlying transaction.

### 3.71 Accounting Dimension

An **Accounting Dimension** is an approved analytical attribute used to classify, allocate, filter, or report Accounting Entries beyond the Ledger Account itself. Cost Centres and Cost Categories are supported forms. Additional dimensions may be introduced through governed extensions, with explicit applicability, validation, security, and reporting rules.

### 3.72 Accounting Period

An **Accounting Period** is a defined time interval within an Accounting Book for recognizing, organizing, reviewing, closing, and reporting accounting activity. It is governed by the applicable Financial Year, Accounting Framework, statutory requirements, and authorized Accounting Policies. The shared default Financial Year remains 1 April through 31 March, subject to the existing rules in Section 6B.

### 3.73 Reconciliation

**Reconciliation** is a controlled comparison of two or more relevant records, balances, statements, or transaction sets to identify agreement, timing differences, omissions, duplicates, and exceptions. Accounting owns the reconciliation of its Ledger and accounting records; source records such as bank statements, payment settlements, tax determinations, or inventory valuation remain owned by their respective domains.

### 3.74 Accounting Data Migration

**Accounting Data Migration** is the governed transfer of existing accounting masters, opening balances, transactions, and supporting references from a prior Accounting System or source into CBM. It includes source identification, mapping, staging, validation, exception handling, reconciliation, authorized acceptance, provenance, and duplicate protection. Migration does not imply that every source record is accepted as valid or posted automatically.

### 3.75 Accounting Data Export and Audit Data Package

**Accounting Data Export** is the controlled provision of Accounting records, reports, and related permitted references in a selected structured or human-readable format. An **Audit Data Package** is a scoped, traceable collection of Accounting records and authorized cross-domain supporting information prepared for an authorized auditor or other permitted recipient. It is not, by itself, an audit opinion, certification, or statutory filing.

### 3.76 External Accounting Data Exchange Model

The **External Accounting Data Exchange Model** is the separate compatibility and transformation boundary through which CBM exchanges data with external Accounting Systems. It defines external representations, format/version compatibility, mappings, transformations, source identifiers, import/export validation, and exchange provenance. It is not the authoritative Accounting Domain Model and does not own accounting rules or Books of Accounts.

### 3.77 Tally Data Exchange Model

The **Tally Data Exchange Model** is an optional, vendor-specific exchange profile used only when a CBM customer requires supported data transfer to or from Tally. It describes the relevant Tally-side representations, identifiers, fields, formats, and version-specific exchange behaviour needed for that selected exchange scope. It is not a mandatory CBM platform capability, a template for CBM's canonical Accounting Model, or a requirement to reproduce Tally concepts, workflows, customizations, or internal structures. Tally-specific details remain confined to the optional exchange boundary.

### 3.78 Tally Compatibility Adapter

A **Tally Compatibility Adapter** is an optional, governed connector that may be provided when a customer needs a supported Tally data-import or data-export path. It translates only the selected, documented, supported exchange data to or from the relevant canonical CBM domain contracts. It is subject to Platform-wide Integration & API, Security, Authorization, and Audit safeguards and the receiving Module's ownership and validation rules. Its existence does not require every CBM deployment or customer to use Tally, and does not imply full Tally feature, customization, workflow, or native-backup compatibility.

### 3.79 Fixed Asset Accounting

**Fixed Asset Accounting** is the Accounting responsibility for the financial recognition and accounting records of eligible fixed assets, including acquisition cost, capitalization, depreciation, impairment, revaluation where applicable, disposal, and related ledger treatment. Physical asset custody, maintenance, assignment, and operational tracking are separate responsibilities and are not transferred to Accounting merely because an asset has an Accounting record.

### 3.80 Management Accounting

**Management Accounting** is the use of authoritative accounting and relevant operational information for internal cost, margin, profitability, and financial-performance analysis and decision support. Accounting owns the integrity and meaning of the accounting actuals it supplies. Enterprise planning, budget creation, forecasts, targets, and their approval workflows remain with the appropriate planning responsibility/module; they do not become Accounting records merely because they are compared with actuals.



### 3.81 Business Module Catalogue

The **Business Module Catalogue** is the governed, authoritative classification of independent Business Modules in CBM. Each entry identifies its independent business responsibility, domain rules, authoritative data ownership, functional boundaries, operational responsibility, contracts, dependencies, applicability, and extensibility. A capability, feature, menu, report, screen, integration, application client, or shared data model does not qualify as a Business Module merely because it is sizeable or separately named.

### 3.82 Shared Domain Model

A **Shared Domain Model** is a canonical conceptual model, reference structure, or data contract reused by two or more Business Modules or platform capabilities. It defines common meaning and interoperability without automatically becoming a separate Business Module, shared database, universal master-data owner, or unrestricted data-access layer. Every Shared Domain Model must identify the authoritative owner of each source entity or field, permitted consumers, allowed operations, provenance, and applicable context/security boundaries.

### 3.83 Shared Model Contract

A **Shared Model Contract** is the governed agreement defining the meaning, identity, references, permitted state changes, validation, versioning, compatibility, and ownership rules for a Shared Domain Model or cross-Module interaction. A contract must prevent consumers from silently redefining or taking ownership of source-domain data.

### 3.84 Internal Platform Capability

An **Internal Platform Capability** is a reusable capability provided within the CBM Platform-wide Services Architecture for use by multiple Business Modules, portals, or authorized contexts. Examples include User/Identity lifecycle support, Authentication, Role and Permission evaluation, Configuration, Approval execution, Notification, Audit/History infrastructure, Event & Messaging, and shared API execution. An Internal Platform Capability is not automatically a Business Module and does not take ownership of the consuming Business Module's domain rules or records.

### 3.85 External Connectivity Capability

An **External Connectivity Capability** enables authorized interaction between CBM and an application, device, independent system, external service, or separately governed portal. It includes adapters, application-client interfaces, data exchange, synchronization, federation, and integration contracts. It is provided through the existing Platform-wide Integration & API and Event & Messaging categories, while each Business Module retains its domain-specific transformation, validation, and data ownership.

### 3.86 Application Client

An **Application Client** is a user-facing or system-facing application that accesses CBM capabilities through authorized interfaces. Examples include the CBM Web Portal, a first-party desktop application, a first-party mobile application, a warehouse handheld application, or an authorized third-party client. An Application Client is an access/presentation channel, not a Business Module or independent source of truth for CBM business data.

### 3.87 External System

An **External System** is an application, service, platform, device-management system, Accounting System, partner portal, or other system that is outside the authoritative CBM application/domain boundary for the interaction in question. A first-party CBM desktop/mobile client is an Application Client; it is not automatically an independent External System merely because it runs on a separate device.

### 3.88 Portal Interoperability

**Portal Interoperability** is the governed exchange, federation, or synchronization of selected data and events between separately operated portal instances or portal ecosystems. It does not merge their identity, authority, Tenant boundaries, source records, or ownership. Each exchanged data set must have a designated source of truth, permitted purpose, direction, recipient, mapping, synchronization policy, provenance, and conflict-resolution rule.

### 3.89 Product/Item Reference Model

The **Product/Item Reference Model** is the shared canonical model and contract through which eligible Business Modules identify and reference products, services, product variants, trade packaging configurations, and other governed commercial or operational items. Its core identity structure distinguishes a Product Definition from its Product Variants and from individual physical instances. It also supports optional product-family/group and classification relationships, typed identifiers, machine-readable data carriers, packaging and unit-of-measure references, product attributes, lifecycle/version references, and authorized source/provenance mappings.

Product/Item Management is the authoritative owner of Product Definitions, Product Variants, product/variant attributes and classifications, product-level and variant-level identifiers, and Product/Item catalogue lifecycle where that Business Module is enabled. The Product/Item Reference Model is nevertheless a Shared Domain Model: Sales, Purchase, POS, Inventory, Warehouse Management, Asset Management, and other eligible domains consume the references through explicit contracts and retain ownership of their own transactions and domain facts. Inventory owns stock quantities, stock states, stock valuation, and operational batch/serial traceability; Warehouse owns physical placement and warehouse execution; neither may redefine the core Product/Variant identity. A Product/Item reference does not imply ownership, stocking, saleability, purchaseability, availability, or physical existence.

### 3.90 Inventory Position Model

The **Inventory Position Model** is the governed representation of stock held or controlled for an accountable Business Context, including the relevant product/item reference, quantity, unit, stock status, ownership/custody distinction where applicable, and permitted batch, serial, expiry, condition, reservation, and location references. Inventory Management owns the authoritative stock ledger, quantities, stock states, and valuation rules. Warehouse Management contributes authoritative physical placement and execution references through an explicit contract.

### 3.91 Warehouse Management

**Warehouse Management** is an independent Business Module responsible for the governed physical and operational management of warehouse facilities and warehouse work. It owns the warehouse layout and location hierarchy, storage areas, racks, shelves, bins, handling-unit placement, warehouse work queues/tasks, receiving, putaway, replenishment, picking, packing, staging, dispatch, returns handling, and related execution records within its authorized scope. It does not own Product Master definitions, enterprise-wide stock valuation, or the authoritative Inventory Ledger.

### 3.92 Warehouse Facility

A **Warehouse Facility** is a defined physical or authorized third-party facility or operating site used for receiving, storing, handling, or dispatching goods. Its location, operational controller/operator, property/occupancy relationship, and stock ownership/custody arrangements are separate attributes and must not be collapsed into one internal/external flag.

### 3.93 Storage Location and Storage Bin

A **Storage Location** is a defined physical or logical place at which inventory may be held or handled, such as a warehouse, storage area, zone, rack, shelf, cabinet, or bin. A **Storage Bin** is the smallest independently addressable storage position where the model requires bin-level control. The physical hierarchy is configurable and may omit levels not relevant to a particular facility.

### 3.94 Handling Unit

A **Handling Unit** is a uniquely identifiable physical grouping or carrier of goods, such as a pallet, carton, tote, container, or other permitted handling package, whose contents and movements may be tracked as a unit. A Handling Unit is not itself a product, stock owner, warehouse location, or accounting ledger.

### 3.95 Warehouse Workstation / Counter

A **Warehouse Workstation** or **Warehouse Counter** is a defined operational work point used for activities such as receiving, inspection, picking, packing, returns, issuance, or dispatch. A counter is not automatically a storage bin or inventory location. Where stock is physically held at a counter, the relevant temporary or fixed stock-holding position must be separately identified and linked.

### 3.96 Warehouse Custody

**Warehouse Custody** identifies which party physically controls, stores, or handles goods at a particular time and facility. Custody is distinct from Legal Ownership of the goods, Inventory valuation ownership, facility property ownership, and the right to access or manage the warehouse. External/third-party storage does not by itself transfer stock ownership or grant access to another business's data.

### 3.97 Source Transaction Reference

A **Source Transaction Reference** is a stable, typed reference from one Business Module's record to an originating record owned by another Module or External System. It preserves source identity, provenance, context, and traceability without copying or transferring the source record's ownership.

### 3.98 External Identifier Mapping

An **External Identifier Mapping** links a CBM identifier to an identifier used by a specified External System, portal, data source, or exchange version. It must identify the source/target system and context, object type, mapping validity, and lifecycle. An external identifier must not be treated as a CBM primary identity or as an authorization grant.

### 3.99 Synchronization Contract

A **Synchronization Contract** defines how selected records or events are shared between systems, including the source of truth, permitted data scope, direction, initial/full and incremental exchange, frequency or trigger, versioning, duplicate handling, idempotency, ordering where required, conflict resolution, deletion/retention propagation, reconciliation, and failure recovery. Synchronization does not imply unrestricted two-way write access.

### 3.100 Business Location Reference

### 3.101 Inventory Management

**Inventory Management** is an independent Business Module responsible for authoritative stock quantities, stock status, reservations/availability, inventory movements, reconciliation, and valuation within its authorized Business Contexts. It consumes Product/Item references and, where advanced Warehouse Management is enabled, warehouse-location and physical-execution references. It does not own core Product/Item definitions or advanced warehouse layout and execution.

### 3.102 Inventory Transaction / Stock Movement

An **Inventory Transaction** or **Stock Movement** is an authorized record of a change to inventory quantity, status, ownership/custody attribution, or accountable stock location. It is owned by Inventory Management and must preserve the source transaction or physical-execution reference that justifies the change. A Warehouse task or physical handling event is not automatically an accepted Inventory Stock Movement.

### 3.103 Inventory Status

An **Inventory Status** classifies stock availability or condition, such as available, reserved, blocked, damaged, quarantined, under inspection, or otherwise restricted. Status definitions, transition rules, and availability effects are owned by Inventory Management. A Warehouse location or task status does not by itself determine the authoritative Inventory Status.

### 3.104 Warehouse Work Task

A **Warehouse Work Task** is an authorized operational instruction or execution record for a physical warehouse activity such as receiving, inspection, putaway, replenishment, picking, packing, staging, loading, dispatch, returns handling, or internal relocation. Warehouse Management owns its task lifecycle and physical completion evidence. A task request or completion does not by itself post an Inventory Stock Movement.

### 3.105 Warehouse Storage Type / Section

A **Warehouse Storage Type** classifies a storage area by its physical or operational characteristics, constraints, or use. A **Warehouse Storage Section** groups storage positions within a relevant type or area by shared attributes or putaway/picking purpose. These are optional hierarchy levels, used only where the facility's operational design requires them.

### 3.106 Internal Warehouse Facility

An **Internal Warehouse Facility** is a facility or storage operation managed or directly operated by the accountable business or an authorized part of its operating organization. "Internal" describes an operational/control relationship for the specified scope; it does not automatically establish property ownership, inventory ownership, Legal Identity, or User access.

### 3.107 External / Third-party Warehouse Facility

An **External / Third-party Warehouse Facility** is a facility operated, controlled, or provided by an independent party, such as a third-party logistics provider, partner, supplier, customer, or public warehouse operator. Its relationship to CBM and the business must be explicitly identified. External status does not transfer Inventory ownership, Book ownership, Tenant data rights, or authority to the operator.


A **Business Location Reference** is a reusable reference to a business operating location, address, site, branch, or facility. It does not automatically represent a Warehouse Storage Location, Storage Bin, POS Counter, User Scope, Legal Identity, or Business Entity. Domain-specific location models may specialize the reference while retaining their own authoritative location semantics.


### 3.108 Product Definition

A **Product Definition** (or **Product**) is the canonical catalogue-level definition of a product or service that describes the common identity and characteristics shared by its Product Variants. It may represent a tangible good, service, non-stock item, or another eligible commercial/operational offering. It is not, by itself, an individual physical unit, inventory balance, price, transaction, or proof that the product is offered or stocked by a particular business.

### 3.109 Product Variant

A **Product Variant** is a distinct, configured form of a Product Definition whose selected characteristics make it meaningful to identify separately for one or more applicable business purposes, such as purchasing, selling, stocking, fulfilment, or service delivery. A Product Variant is the normal operational item reference when a transaction or stock record must identify the specific form involved. A Product Variant is a defined type/configuration, not an individual physical occurrence. A Product with no meaningful variation must still be usable through one standard/default variant representation; the interface may present that as a simple Product without requiring the User to manage variant complexity.

### 3.110 Product Family, Group, and Classification

A **Product Family** or **Product Group** is an optional grouping relationship used to associate related Product Definitions for catalogue, commercial, technical, reporting, or navigation purposes. A **Product Classification** assigns a Product or Product Variant to a governed taxonomy, category, class, collection, or other classification scheme. These groupings and classifications are not Product identity levels, are not required parent records, and do not imply a single universal product hierarchy. A Product may participate in multiple independently governed groupings or taxonomies where permitted.

### 3.111 Product/Item Identifier

A **Product/Item Identifier** is a typed identifier value associated with a specified Product Definition, Product Variant, Trade Packaging Configuration, physical instance, component, batch/lot, or other explicitly supported subject. Each identifier type defines its issuing authority or namespace, applicable subject level, format and validation, uniqueness scope, permitted purpose, provenance, and lifecycle. Identifiers may include CBM internal record IDs, business Product Codes, Item Codes, SKUs, PLUs, manufacturer model/part numbers, supplier/customer item codes, GTINs, and other approved internal, industry, regulatory, or external-system identifiers. These types are not interchangeable and do not all have the same uniqueness or allocation rules.

### 3.112 Data Carrier / Barcode

A **Data Carrier** is a machine-readable representation or physical carrier used to capture one or more identifiers and/or structured data attributes. It may include a one-dimensional barcode, two-dimensional symbol such as a DataMatrix or QR Code, RFID tag, NFC tag, or another supported carrier. A **Barcode** is one type of optical machine-readable Data Carrier. The carrier/symbology (for example EAN-13, UPC-A, Code 128, GS1-128, ITF-14, GS1 DataMatrix, or QR Code) is distinct from the identifier scheme and data values it encodes. A carrier is not automatically a unique Product Identifier.

### 3.113 Product/Item Instance

A **Product/Item Instance** is one individual physical or otherwise separately identifiable occurrence of a Product Variant or component. It may be represented as an individual CBM record when unit-level identity or traceability is required, or remain implicit within quantity-based Inventory where no individual instance record is required. A Product/Item Instance may carry multiple identifiers issued by different parties or identifying different aspects of the same unit. Identifiers that belong to a separately identifiable component or interface must be associated with that component/interface subject rather than incorrectly treated as the identity of the complete unit.

### 3.114 Batch / Lot

A **Batch** or **Lot** is a group of Product Variant units identified by a common production, procurement, quality, traceability, or other governed grouping reference. A batch/lot may carry attributes such as manufacture date, expiry/best-before date, source, quality status, or supplier batch reference. A lot identifies a group and does not, by itself, identify each individual unit within that group.

### 3.115 Trade Packaging Configuration

A **Trade Packaging Configuration** is a defined packaging or trade-unit arrangement for a Product Variant, such as each/unit, inner pack, box, case, carton, or pallet-level trade configuration. It may define contained quantity, unit-of-measure conversion, packaging materials, dimensions, weight, permitted handling, and applicable product/trade identifiers. It is distinct from the underlying Product Variant and from a Warehouse Handling Unit. A packaging configuration may be separately identified, ordered, priced, sold, purchased, or stocked where the business and applicable standards require it.

### 3.116 Product Bundle / Kit

A **Product Bundle** or **Kit** is a defined commercial or operational composition of one or more Product Variants and/or Trade Packaging Configurations. Its composition may be fixed or governed by permitted alternatives, and may be physically pre-packed/assembled or virtually grouped for an offer or order. A bundle/kit is not automatically a Product Variant, a Trade Packaging Configuration, a manufacturing bill of materials, or a Warehouse Handling Unit; its identity, composition, stock treatment, and fulfilment behaviour must be explicitly defined for its use case.

### 3.117 Product Attribute and Variant Dimension

A **Product Attribute** is a typed property describing a Product Definition, Product Variant, packaging configuration, or another explicitly applicable product subject. A **Variant Dimension** is an attribute or governed combination of attributes whose values distinguish Product Variants for an applicable business purpose. Not every attribute is a Variant Dimension: descriptive, compliance, measurement, or informational differences must not create separate variants unless the applicable product policy says that the difference changes the separately managed item identity.



### 3.118 Price Record

A **Price Record** is an owner-domain record stating a monetary amount for a declared price purpose, an identified Product/Variant/Trade Packaging Configuration or other permitted subject, and an explicitly defined business/applicability scope and validity period. It must identify its owning domain, Price Type, amount, INR currency, applicable quantity basis and unit/packaging reference, tax basis where relevant, source/provenance, approval/status, and effective/recorded times. A Price Record is not automatically an accepted transaction price, tax determination, inventory valuation, or accounting value.

### 3.119 Price Type

A **Price Type** identifies the business meaning and intended use of a price, such as declared MRP, Manufacturer Suggested Retail Price / RRP, selling/list/retail/wholesale/dealer price, customer/contract/promotion price, supplier quotation/agreement/purchase reference price, or a governed cost/valuation reference. Each Price Type must define its owner domain, eligible subject levels, permitted scope, tax-basis behaviour, lifecycle, visibility, and applicable validation. Different Price Types must not be treated as interchangeable merely because they contain monetary amounts.

### 3.120 Price List

A **Price List** is a governed, named collection of Price Records and associated applicability conditions used for a declared commercial purpose, such as a sales channel, customer group, supplier relationship, contract, or business context. A Price List has an owning domain and business context, effective lifecycle, eligible consumers, and explicit rules for publication, sharing, inheritance, priority, and override. A Price List is not itself a Product catalogue, transaction document, or tax schedule.

### 3.121 Price Condition

A **Price Condition** is a governed qualification or adjustment associated with a Price Record or Price List, such as customer/supplier applicability, quantity break, channel, location, contract, date/time, promotion, discount, surcharge, or other permitted commercial condition. Conditions must state their scope, qualification logic, combination/exclusivity behaviour, priority, effective period, and authority. A Price Condition does not itself establish the legal GST treatment of a transaction.

### 3.122 Price Tax Basis

A **Price Tax Basis** states how a Price Record's amount is expressed in relation to applicable indirect taxes: tax-inclusive, tax-exclusive, or not stated/not applicable where legitimately permitted for that Price Type. It describes the representation of the amount; it does not declare tax applicability, tax classification, HSN/SAC, GST rate, tax liability, or entitlement to input tax credit. Those determinations remain with the authoritative Tax domain and applicable law.

### 3.123 Price Determination

**Price Determination** is the governed process by which the responsible domain resolves the applicable Price Record and Price Conditions for a specific subject, business context, counterparty, channel, quantity, unit/packaging, and effective time. It must produce a reproducible result, identify the selected records and rules, and report conflicts or missing/ambiguous data rather than silently guessing.

### 3.124 Price Snapshot

A **Price Snapshot** is the immutable price-related information captured by an accepted transaction or other authoritative domain event at the time it is accepted. It preserves the selected price and applicable conditions, relevant amounts, unit/quantity basis, discount/surcharge, tax basis, tax breakdown or tax reference as determined by the owning domains, and source/version references needed to explain the accepted result later. A later Price Record change must not rewrite a historical Price Snapshot.

### 3.125 Declared Retail Price

A **Declared Retail Price** is a price declaration associated with a Product or Trade Packaging Configuration, such as a legally applicable Maximum Retail Price (MRP), Manufacturer Suggested Retail Price, Recommended Retail Price, or other sourced declared/list reference. These declarations have different legal and commercial meanings. In particular, MRP must not be treated as an ordinary transaction Selling Price or as synonymous with RRP/MSRP.

### 3.126 HSN/SAC Code

An **HSN/SAC Code** is an official tax-classification code used to classify goods under the Harmonized System of Nomenclature (HSN) or services under the applicable Service Accounting Code (SAC) structure for GST/tax purposes. The Code Type (HSN or SAC), exact full code value, official description, source/authority, applicable tariff or classification version, and effective status must be explicit. The code is a tax-classification reference, not a Product Code, SKU, GTIN, barcode, Product Category, or GST Rate. CBM stores the code as a string so significant characters and leading zeroes are preserved.

### 3.127 HSN/SAC Classification Assignment

An **HSN/SAC Classification Assignment** is the governed, effective-dated association of an HSN/SAC Code with the eligible supply subject to which that classification applies, ordinarily a Product Variant for goods or an eligible Service Offering/supply subject for services. It records the subject, code reference and version, applicable business/supply context where relevant, evidence/source, effective and recorded times, status, reason, and approval where required. The assignment is distinct from the Code Registry entry and from a GST Rate Applicability Rule.

### 3.128 GST Rate Applicability Rule

A **GST Rate Applicability Rule** is a versioned, legally sourced rule that defines the GST rate treatment applicable to a specified tariff/classification scope, goods or service description, and any associated statutory conditions, exclusions, exceptions, or effective period. It may define the relevant CGST, SGST/UTGST, IGST, Compensation Cess, or other legally applicable components and must preserve its statutory source and effective dates. It is not a permanent rate field on an HSN/SAC Code or Product Variant.

### 3.129 GST Tax Determination

A **GST Tax Determination** is the authorized domain decision for a specific supply/transaction context that resolves the applicable tax classification, rate rule, statutory conditions, taxable value, tax components, and resulting tax treatment using the facts and legal timing relevant to that supply. It is distinct from the HSN/SAC Code, the Code Assignment, the Rate Rule, the Price Record, and the Price Tax Basis.

### 3.130 Tax Determination Snapshot

A **Tax Determination Snapshot** is the immutable classification- and tax-related evidence captured by an accepted transaction. It preserves the full HSN/SAC code and assignment version where applicable, the selected GST Rate Applicability Rule and version, relevant statutory source/notification and effective dates, the determination context, taxable value, applicable component rates and tax amounts, and provenance. Later changes to codes, assignments, rate rules, or tax policies must not rewrite the accepted snapshot.


## 3A. How the CBM relationship model must be read

CBM must not be interpreted as one universal Parent–Child tree. It contains several distinct, overlapping relationship views. Each view answers a different question and must be modelled and evaluated independently.

| Relationship view | Question it answers | Example |
|---|---|---|
| **Platform/service-provider structure** | Who provides the CBM service to this customer business, and under whose platform governance does that provider operate? | Main Portal → Sub-Portal → served Organization/Tenant; or Main Portal → directly served Organization/Tenant. |
| **Business structure** | Which entities are direct structural children of an Organization? | Organization → Distributor entities and/or Shop entities. Both are terminal structural entities. |
| **Identity relationship** | What legal person/entity is responsible, and how is each business or operating context professionally identified? | One Legal Identity may be associated with multiple distinct Professional Identities, such as five separately named Shops operating under one legal taxpayer. | 
| **Ownership relationship** | Who owns each independently owned business? | Owner ID → Ownership ID → ownership-bearing Business Identity; one Owner may be linked to several separate businesses. |
| **Business relationship** | Which independent businesses are associated for a specific commercial purpose? | Distributor ↔ Shop through the specific Distributor–Shop Mapping type. |
| **Business Available Authority Scope** | Which entities are eligible for a business-level authority to govern or make available for User assignment? | Eligibility is determined under explicit authority rules; neither structural placement nor Mapping automatically grants unrestricted scope. |
| **User Assigned Authority Scope** | Which of the business-available entities has been explicitly assigned to this User? | An Organization-level User may be assigned only Shop A and Shop C, not every Shop. |
| **Permission / Role** | Which actions may this User perform within the assigned entity scope? | A Shop Manager may update records only for Shop A. |

These views may refer to the same businesses or operational contexts, but they are not interchangeable. A relationship in one view does not create a relationship in another unless the applicable rule explicitly says so. Distributor–Shop Mapping is one specific Mapping type, not a universal definition or restriction on Mapping across CBM. Other Mapping and Assignment types must be separately defined with their own endpoints, purpose, eligibility, authority, and lifecycle. Ownership Links may inform eligible Business Available Authority Scope under Section 7; they do not change Business Structure or automatically grant a User access. User Scope Mapping determines the specific entities or areas assigned to a User; business-level authority availability alone does not assign a User's scope. GST Registration Assignment is separately governed and is never inferred from Owner ID, Business Structure, service-provider association, or another Mapping. The Business Structure Hierarchy has its own strict valid-parent and valid-child constraints; it is not a freely rearrangeable universal tree.

### 3A.1 Conceptual illustration

```text
PLATFORM / SERVICE-PROVIDER VIEW

                         MAIN PORTAL
                    (global supreme authority)
                       /             \
             direct service       delegates service
                  /                   \
       ORGANIZATION / TENANT       SUB-PORTAL
                                      |
                          associated customer/business
                                  domain

BUSINESS STRUCTURE VIEW

                    ORGANIZATION A
                      /        \
              DISTRIBUTOR D1   SHOP S1
                   (terminal)  (terminal)

              DISTRIBUTOR D2   SHOP S2
                   (terminal)  (terminal)

Organization A may have multiple Distributors, multiple Shops, or both.
A Distributor and a Shop are direct structural children of the Organization.
Neither may have structural children under the currently approved model.

BUSINESS RELATIONSHIP / MAPPING VIEW

          DISTRIBUTOR D1  - - - - -  SHOP S1
          DISTRIBUTOR D2  - - - - -  SHOP S1

These are separately configured Distributor–Shop Business Mappings.
They do not change either entity's structural parent or ownership.

OWNERSHIP LINK VIEW (INDEPENDENT OF BUSINESS STRUCTURE AND MAPPING)

                     OWNER ID SET {A, B}
                              |
             +----------------+----------------+
             |                |                |
       separate Ownership  separate Ownership  separate Ownership
          Links/IDs           Links/IDs           Links/IDs
             |                |                |
       ORGANIZATION A    DISTRIBUTOR D1       SHOP S1

Within this Customer Business Architecture, Organization A and each of
its direct Distributor and Shop children have exactly the same Owner ID
set {A, B}. Each Business Identity retains its own separate Ownership
Links and Ownership IDs. The matching Owner ID set does not merge the
Business Identities, Legal Identities, Professional Identities, or data.

A Distributor or Shop in a different Organization's Customer Business
Architecture is governed by that other Organization's own internally
consistent Owner ID set. Structural placement does not itself create an
Ownership Link.

USER AUTHORIZATION VIEW

       BUSINESS AVAILABLE AUTHORITY SCOPE
                     |
          explicit User scope assignment
                     |
          USER ASSIGNED AUTHORITY SCOPE
                     |
             Role / Permissions
                     |
              Effective access

These are conceptual views, not a database schema or a universal
ownership hierarchy. They illustrate why service provision, ownership,
business structure, mapping, available authority scope, user scope
assignment, and action permission must remain separate.
```

### 3A.2 Non-inference rule

No one may infer any of the following solely from the existence of another relationship:

- Service Provider status does not imply ownership.
- Ownership does not automatically imply a User login, User Assigned Authority Scope, or unrestricted operational Permission.
- Business-structure membership does not imply that every User can perform every action or access every subordinate entity.
- A Distributor–Shop Mapping alone does not imply ownership or ownership-based authority over the Shop. Any eligible owner-linked scope must arise separately under Section 7. This specific Mapping type does not define or restrict other Mapping or Assignment types across CBM.
- An Ownership Link may inform Business Available Authority Scope only under the explicit rules in Section 7; it does not automatically grant User access.
- A Sub-Portal's authority scope over associated businesses does not imply unrestricted record access for every Sub-Portal User.
- A business's Business Available Authority Scope does not itself define the User Assigned Authority Scope or action types available to a User.

## 4. Stable Core principles

The following principles are fundamental and must not be exposed as ordinary subordinate-business configuration options.

### 4.1 Absolute Main Portal supremacy

Main Portal is permanently supreme and is the ultimate authority over the entire CBM Ecosystem. Its authority cannot be restricted, limited, overridden, revoked, or compromised by any subordinate entity for any reason.

### 4.2 Authority flows only downward

All platform authority originates from Main Portal and flows downward. No subordinate entity may establish authority flowing upward. There is no peer-level authority between Sub-Portals, and no Sub-Portal may exercise authority over another Sub-Portal.

### 4.3 Ownership is independent of authority

Ownership and platform governance are separate. A business may be independently owned while remaining within the applicable authority scope and governance of Main Portal and its authorized service-provider chain.

### 4.4 Identity is independent of Service Provider

A business's persistent Business Identity does not change merely because its Service Provider changes. Service Provider migration is not an ownership transfer.

### 4.5 A relationship is not ownership

A Distribution Network association or Distributor–Shop mapping does not transfer ownership, merge identities, or grant unrestricted administrative or data access.

### 4.6 Scope is not permission

Business Authority Scope determines where authority may be assigned. User/Role Permissions determine which actions an individual User may perform. A User must satisfy both the applicable business scope and the required permission for an action to be authorized.

### 4.7 Business and Tenant isolation

Distinct independently owned businesses retain their own identities and data boundaries. Access between them must be explicitly authorized and must not be inferred from shared ownership, service provision, organizational association, or mapping.

### 4.8 Integrity and auditability

Business identity, ownership relationships, Mapping relationships, authority delegation, User Scope Mapping changes, permission changes, configuration changes, approvals, migration, archival, and all other relevant system activities must be captured in the comprehensive Audit Trail. Where an approved domain History exists, its domain-specific record must be maintained separately.

### 4.9 Strict Business Data Isolation

Business and Tenant data boundaries are immutable security constraints. No configuration, customization, extension, delegated authority, or override may merge independent business data contexts or weaken their isolation. Any permitted cross-business interaction must be explicit, purpose-limited, authorized, and auditable without changing the underlying ownership or isolation boundary.

### 4.10 Strict Business Structure Hierarchy and Structural Integrity

The permitted Business Structure Hierarchy is an immutable Stable Core business-structure constraint. The valid entity levels and parent–child placements must be preserved in every domain, logical, data, and technical design. The invalid arrangements defined in Section 3.25 must never be supported. No subordinate business, Service Provider, User, Role, workflow, configuration, customization, delegation, or override may invert, bypass, or weaken these structural rules.

### 4.11 Absolute prohibition on outbound URLs and links

CBM must not send any URL, hyperlink, clickable link, URL-bearing button, or QR code that resolves to a URL to any person, through any channel, for any purpose. This includes account creation, invitation-like communication, verification, authentication, password establishment, password recovery, notifications, documents, and all future CBM-generated communications. The prohibition is a non-overridable Stable Core and White-label rule. Required actions must be completed by the User inside the CBM application after independently opening it; CBM must use approved in-application verification methods rather than sending links.

---

## 5. Business entity and ownership model

### 5.1 Business and operating contexts

The following CBM contexts must be capable of maintaining their applicable Legal Identity and Professional Identity dimensions independently of their platform or business role:

- Main Portal as the central platform/SaaS service-provider business.
- Each Sub-Portal as an independently operated service-provider business.
- Each Organization as an independent customer business.
- Each Distributor as a distribution business entity within an Organization's Business Structure.
- Each Shop as the unified retail business entity and terminal structural child of an Organization.

These contexts have different platform, service-provider, business, or operational roles. Their roles are not interchangeable. Identity dimensions must not be used to infer ownership, structural placement, service-provider relationships, or User authority.

### 5.2 Unified Shop entity and identity applicability

The Shop is one unified entity type, regardless of who owns it or which Legal Identity is applicable. A Shop is a direct structural child of exactly one Organization and is terminal in the currently approved Business Structure.

The following Legal Identity and Professional Identity applicability matrix is finalized:

| Entity | Legal Identity | Professional Identity |
|---|---|---|
| Main Portal | Required | Required |
| Sub-Portal | Required | Required |
| Organization | Required | Required |
| Distributor | Optional | Required |
| Shop | Optional | Required |

For Distributor and Shop, an optional Legal Identity must be explicitly associated and validated where applicable. It must not be inherited automatically from an Organization, Owner, Service Provider, or another entity. Every Distributor and Shop must have a Professional Identity. A Shop may maintain a distinct Professional Identity while being associated with a Legal Identity shared with another eligible business context, subject to verified facts and applicable law.

Ownership determines whose Distributor or Shop it is. Business Structure determines its permitted structural position. Legal Identity determines the relevant legal person/entity. Professional Identity determines how the business or operating context presents itself. These dimensions must not be conflated.

### 5.2.1 Mandatory Parent Organization

Every Distributor and every Shop must be assigned exactly one direct Parent Organization when it is created. Parent Organization is a mandatory structural relationship, not an optional mapping attribute. A Distributor or Shop must not be created without a Parent Organization or assigned to multiple Organizations.

The Parent Organization relationship identifies the entity's place in the Customer Business Structure. The Distributor's or Shop's Owner ID set must also match the Parent Organization's Owner ID set under Section 5.3.1, with separate Ownership Links and Ownership IDs recorded for each Business Identity. This ownership-consistency rule does not determine Legal Identity or Professional Identity, create a Distributor–Shop Business Relationship, or by itself grant every User access or every possible management action. Authority availability and individual User access continue to be determined by Sections 7, 8, and 9.

A Distributor–Shop Mapping does not require either party to select, disclose, or know the other party's Parent Organization. Each party's Parent Organization remains part of its own structural and authorized administration context; it is not a mapping credential, mapping prerequisite, or information that must be exposed to the other mapping party.

### 5.3 Ownership Links and ownership identity

1. Each independent Business Identity must have one or more explicit Ownership Links to its Owner or Owners.
2. Each Owner is identified by an Owner ID. Each individual Owner-to-Business relationship is identified by a distinct Ownership ID.
3. One Owner ID may be linked to multiple independent Business Identities. A Business Identity may be linked to multiple Owner IDs.
4. Owner ID identifies the Owner; Ownership ID identifies the particular Ownership Link; Business Identity identifies the independent business. These identifiers and concepts are not interchangeable.
5. In this CBM model, ownership records identify who owns or holds ownership rights in a business for authority-scope purposes. They do not represent or calculate equity, financial shares, percentages, investment, profit entitlement, or economic interest.
6. Owner identity and ownership are separate from User accounts, authentication credentials, Roles, Permissions, and operational assignments. An Owner does not automatically become a User, and an Owner's User account does not automatically receive access to every business linked to that Owner.
7. Separate businesses linked to the same Owner ID remain separate Business Identities and separate data contexts.
8. A Distributor does not own a Shop merely because the Shop is in its Distribution Network or mapped to it.
9. An Organization's association with a Distributor does not itself change the Distributor's ownership or the ownership of any Shop associated with that Distributor.
10. Shops are direct structural children of an Organization. Within that Customer Business Architecture, each Shop's Owner ID set must exactly match its Parent Organization's Owner ID set, while each Shop retains its own explicit Ownership Links and Ownership ID records. The same ownership-consistency rule applies to every Distributor under that Organization.
11. Ownership Links may be used as an eligibility input when determining Business Available Authority Scope, but only under the explicit rules in Section 7. They do not by themselves grant User access, User Assigned Authority Scope, or action Permissions.

### 5.3.1 Mandatory ownership consistency within a Customer Business Architecture

For a given Organization, the set of Owner IDs linked to the Organization must exactly match the set of Owner IDs linked to every Distributor and every Shop that is a direct structural child of that Organization. This equality is mandatory throughout the Customer Business Architecture. A child Distributor or Shop must not have a different, additional, or partial Owner ID set.

Each Organization, Distributor, and Shop remains a separate Business Identity and must have its own explicit Ownership Links, with a distinct Ownership ID for each Owner-to-Business relationship. The rule does not merge Business Identities, Legal Identities, Professional Identities, transaction records, or data boundaries. It ensures that ownership does not vary between an Organization and the Distributors/Shops within that Organization's one Customer Business Architecture.

This Customer Business ownership-consistency rule does not apply to the Service Provider Architecture. Main Portal and each Sub-Portal are separate Service Provider businesses and may have different Owner IDs from one another and from customer Organizations. A Sub-Portal's ownership does not become the ownership of its customer Organization.

A change to an Organization's Owner ID set cannot be applied while leaving its direct structural Distributor or Shop children with a different Owner ID set. This baseline does not define or authorize an Ownership Transfer workflow. No such workflow may be inferred, implemented, or treated as approved from the ownership-consistency rule alone; any ownership-change process requires its own explicit project-owner approval and finalized rules.

### 5.4 Organization Network Configurations

An Organization may operate in one of three configurations:

| Organization Network Configuration | Distributor entities | Shop entities |
|---|---:|---:|
| **Distribution Network** | One or more | None required by this configuration |
| **Retail Network** | None required by this configuration | One or more |
| **Hybrid Network** | One or more | One or more |

These configurations describe the Organization's operating arrangement. They do not create different Organization, Distributor, or Shop entity types, and they do not determine Ownership, Legal Identity, Professional Identity, Service Provider, or User Authority.

- **Distribution Network** describes the Organization's distribution arrangement through its Distributor entities.
- **Retail Network** describes the Organization's retail arrangement through its Shop entities.
- **Hybrid Network** describes an Organization operating through both Distributor and Shop entities.

A Distributor remains an individual entity; Distribution Network describes the collective distribution arrangement. A Shop remains an individual entity; Retail Network describes the collective retail arrangement. Distributor and Shop are both direct structural children of the Organization and are both terminal structural entities.

## 6. Service-provider architecture

### 6.1 Main Portal as primary service provider and ecosystem authority

Main Portal is the central platform authority and primary SaaS service provider. It may provide CBM services directly to Organizations/Tenants and governs all Sub-Portals and subordinate businesses in the ecosystem.

### 6.2 Sub-Portal as an independent service-provider business

A Sub-Portal is independently owned and operated. It may provide CBM services to its permitted customer Organizations/Tenants under its own service-provider identity and branding, within the scope delegated by Main Portal.

A Sub-Portal:

- Is a separate business and may have its own owner.
- May manage its permitted customer-service operations.
- May use its approved white-label identity and branding.
- Must comply with Main Portal's mandatory platform policies.
- Cannot enlarge, restrict, override, or transfer its own platform authority beyond what Main Portal permits.
- Has no authority over another Sub-Portal.

### 6.3 Customer's Primary Service Provider

An Organization/Tenant receives its CBM service through Main Portal directly or through an authorized Sub-Portal. At a given time, the customer service context has one Primary Service Provider responsible for making the service available through the CBM service-provider structure.

### 6.4 Service provision does not confer ownership

A Service Provider does not become the owner of the Organization, Distributor, Shop, or their business data merely by providing CBM software services. Service-provider status is not an ownership relationship.

### 6.5 Service-provider migration

A customer business may migrate from one Service Provider to another under Main Portal governance. Such a migration changes the service-provider relationship only. It does not change the customer's Business Identity, ownership, data rights, or internal business structure.

---

## 6A. Platform-wide Services Architecture

### 6A.1 Purpose and architectural position

**Platform-wide Services** is a distinct Architecture Layer and Capability Group within the CBM architecture. It provides shared, governed capabilities that may be used by multiple business-management modules, service-provider contexts, tenants, and business contexts without transferring ownership of their domain responsibilities to the shared service layer.

The layer exists to prevent every Business Module or portal context from independently rebuilding common capabilities, while preserving clear domain ownership, authority, data boundaries, and service contracts. It is a logical architecture classification. It does not prescribe a deployment topology, programming language, framework, database, vendor, process boundary, microservice count, or separate application.

Platform-wide Services must follow the architecture-wide design doctrine in Section 2.6 and the Stable Core constraints in Sections 4 and 18. Their shared nature must never be used to create a universal data-access privilege or bypass a business domain's authorization rules.

### 6A.2 Approved Service Scopes

Every Platform-wide Service must declare the scope or scopes in which it is available and applicable. The four approved Service Scopes are:

1. **Ecosystem-wide:** A service capability governed centrally for use across the CBM Ecosystem. Main Portal retains supreme governance. Ecosystem-wide availability does not mean every Sub-Portal, Tenant, Business, User, or process has unrestricted access to every service operation or data set.
2. **Service Provider-scoped:** A service capability operating within the authorized context of Main Portal or a particular Sub-Portal as a Service Provider. One Sub-Portal's service-provider context must not expose another Sub-Portal's private operational data or authority.
3. **Tenant-scoped:** A service capability operating within a particular Organization's Tenant context. Tenant-scoped operation must preserve strict Tenant and Business Data Isolation.
4. **Business-context-scoped:** A service capability operating for a specifically identified Business Entity, business unit, location, relationship, or other authorized business-operating context within the applicable Tenant or Service Provider authority. Its exact eligible context must be explicit.

A Service Scope classifies where a service is governed, made available, or applied. It is not a Business Authority Scope, User Assigned Authority Scope, Role, Permission, Ownership Link, Service Provider relationship, Tenant identity, or Business Relationship. A Service Scope never grants access by itself.

A service may support more than one of the four scopes where its responsibility genuinely requires it. Each supported scope and its boundary must be stated explicitly; a broad scope must not be inferred merely because a service is shared. Where one service supports multiple scopes, data, configuration, policy, credentials, events, and operational actions must remain separated according to their respective contexts.

### 6A.3 Approved Service Category Groups and Categories

Platform-wide Services are organized into three approved groups and fifteen approved categories. The groups classify the broad architectural responsibility; the categories classify related service responsibilities. The category taxonomy is:

#### Group A — Platform Foundation Services

**A1. Identity & Authentication Services**

Shared capabilities for identity lifecycle coordination and authentication, including credential and password management, OTP, MFA, authenticator lifecycle, session and device management, account recovery, and authentication-provider integration.

Identity represents the relevant person, account, or system principal. Authentication establishes or verifies that principal's claimed identity. This category does not determine what the authenticated principal may access or do; that is governed by Authorization & Policy Evaluation and the applicable CBM authorization model. It must preserve the distinct User Account, User–Business Association, Credential/Authenticator Lifecycle, and Service Provider/Customer identity boundaries already established in this Master.

**A2. Authorization & Policy Evaluation Services**

Shared capabilities for evaluating whether a requested action is permitted under the applicable CBM authority, scope, permission, policy, relationship, and control conditions. Responsibilities may include permission-decision evaluation, role-based and User-specific permission evaluation, Business Available Authority Scope and User Assigned Authority Scope checks, Delegated Authority constraints, policy-decision evaluation, and recording authorization decision context for the comprehensive Audit Trail.

This category is an evaluation capability, not a new owner or source of truth for the underlying business rules and assignments. It must consume and respect the authoritative CBM concepts and records defined in Sections 4, 7, 8, 9, 10, and 11. It must not redefine, merge, replace, or silently administer Business Authority Scope, User Scope Mapping, Roles, Permissions, Delegated Authority, Approval Workflow, Ownership, or Business Relationships. It must not infer authority from ownership, hierarchy, service-provider association, Mapping, authentication success, or service availability.

Authorization evaluation must enforce all applicable conditions together. A successful decision for one action, entity, purpose, or context must not be reused as a broader grant for another. A denial or missing required condition must fail closed. Main Portal's supreme authority, downward-only delegation, Stable Core constraints, and strict Business/Tenant Data Isolation take precedence over subordinate policies.

**A3. Security & Trust Services**

Shared security and trust capabilities, including cryptographic key management, secrets management, encryption support, certificate and trust-material lifecycle, secure token validation, security policy enforcement, abuse and attack protection, rate-limiting infrastructure, security risk signals, and security-incident support.

This category provides cross-cutting security controls and trust infrastructure. It does not replace Identity & Authentication, does not decide business action Permissions, and does not become an alternative authorization authority. Identity proof, authentication, authorization, security enforcement, and audit recording remain distinct responsibilities that cooperate through explicit contracts.

Security controls must protect every supported Service Scope and must not weaken tenant boundaries or allow a lower authority to override Main Portal's mandatory security policies. Security telemetry and operational signals do not replace the comprehensive Audit Trail.

**A4. Configuration & Policy Management Services**

Shared capabilities for configuration and policy definition and resolution, including configuration registry/schema, defaults, inheritance resolution, authorized overrides, policy versioning, effective dates, validation, and configuration-change history.

This category must implement, not replace or weaken, the configuration governance already approved in Section 10. Defaults and inheritance flow downward; overrides require explicit authority and permission; system-defined bounds and Stable Core constraints remain non-overridable. Configuration availability does not itself grant permission to change a setting. Configuration and policy applicability must be explicit for the relevant Service Scope and business context.

#### Group B — Shared Functional Services

**B1. Communication & Notification Services**

Shared capabilities for human-facing communication and notification. Communication responsibilities include message delivery through approved channels such as email, SMS, and WhatsApp; delivery-provider management and failover; delivery status; retries and failure handling. Notification responsibilities include determining what notification is required, routing it to the intended recipient/channel, applying notification preferences, and managing notification templates and language variants.

Notification determines and coordinates the intended communication; Communication handles delivery. These are related but distinguishable responsibilities and must not be conflated. All CBM-generated outbound communication remains subject to the absolute, non-overridable prohibition on sending URLs, hyperlinks, URL-bearing buttons, or URL-resolving QR codes for any purpose or through any channel (Section 4.11 and Section 19). Provider capability or customer configuration cannot override this rule.

**B2. File, Document & Storage Services**

Shared capabilities for file and document handling, including upload/download coordination, attachments, metadata, versioning, file validation, malware scanning, access-controlled retrieval, document lifecycle support, provider-neutral object-storage abstraction, archival integration, and retention/preservation support.

Operational file handling, business document management, object-storage infrastructure, and long-term archival are related but distinct responsibilities. This category must respect the provider-neutral Object Storage Abstraction and archival requirements in Section 17, as well as the separate retention and access requirements for Domain History and Audit Trail in Section 17A. File or storage availability does not itself authorize access to the stored content.

**B3. Workflow & Approval Services**

Shared capabilities for defining and executing workflows, including approval routing, single/multiple/sequential/parallel/conditional approval patterns, escalation, delegated approval handling, workflow versioning, and execution history.

The distinction between approval governance and workflow execution is mandatory. Section 11 defines whether approval is required, eligible approvers, and the approval pattern permitted for a business action. This category provides the shared execution capability for those rules; it must not independently invent approval requirements or eligible approvers. Approval never grants the initiating User the action Permission that the User otherwise lacks. Workflow execution must preserve the applicable User Scope, Permission, Delegated Authority, Main Portal governance, and Audit Trail requirements.

**B4. Integration & API Services**

Shared capabilities for controlled interaction with external systems and consumers, including API lifecycle and versioning, API access controls, external-provider connectors, integration registry, webhook configuration and delivery, interface validation, rate limits, and integration monitoring.

This category governs the shared integration boundary and interface capabilities. Business rules, domain validation, and authoritative business records remain owned by the relevant Business Module or domain. An API or integration credential does not itself grant business authority; each operation must be authorized in its actual business, Tenant, Service Provider, and User/system-principal context.

The absolute no-URL rule in Sections 4.11 and 19 applies to CBM-generated outbound communications to people. This section does not create a new decision that prohibits URLs as technical endpoint addresses or payload fields in system-to-system API/webhook contracts; such interfaces remain subject to security, authorization, privacy, and integration governance. Human-facing communication must never use this distinction as a route to send a prohibited URL or link.

**B5. Event & Messaging Services**

Shared capabilities for internal and platform event distribution and asynchronous messaging, including event publication and subscription, routing, queues, delivery coordination, retries, dead-letter handling, event schema definition and versioning, and support for idempotent processing.

This category enables decoupled communication between authorized capabilities and Business Modules through explicit event/message contracts. It is distinct from B1 Communication & Notification Services, which handles human-facing communications, and B4 Integration & API Services, which handles external/system interface boundaries. An event may trigger a notification or an external integration, but the event-distribution responsibility remains distinct from those downstream responsibilities.

An event or message is not, by itself, an authorization grant, approval, command acceptance, or proof that a business transaction has completed. Producers and consumers must enforce the applicable authorization, Service Scope, Tenant/Business Data Isolation, purpose limitation, and data-minimization rules. Events must not expose one Tenant's or Business's data to another context merely because a consumer is subscribed to a shared channel. Subscription eligibility and access to event payloads must be explicitly authorized. Event contracts must identify their meaning, producer, permitted consumers, versioning expectations, and relevant context sufficiently to prevent ambiguous interpretation.

Event delivery and processing must tolerate retries and duplicate delivery where applicable; consumers must not assume that a message is delivered exactly once. Idempotent handling and explicit failure/recovery behaviour are required where repeated processing could cause duplicate or inconsistent business effects. A successful publication or delivery does not replace the authoritative transaction record or its domain lifecycle.

The event/messaging infrastructure is not the Audit Trail. It must not merge with, replace, or become the retention source of truth for Domain History or Audit Trail. Relevant events and actions must still be recorded in the appropriate Domain History and/or comprehensive Audit Trail under Section 17A, independently of message delivery, queue retention, or dead-letter handling.

**B6. Localization & Reference Data Services**

Shared capabilities for language and locale support, translation, IST-based date/time handling, date/number formatting, INR currency metadata, units of measure, Indian State/UT/District and other regional references, Indian address formats, and other approved common reference data. These capabilities must operate only within the India-only product scope and the fixed INR-only and IST-only boundaries in Section 6B.

Generic Reference Data must remain distinct from a Business's own Master Data. For example, a shared country or unit-of-measure reference does not become a customer's Organization-specific Product Category, Price List, Customer, Supplier, or other Business Master Data. Reference data access and permitted overrides must follow Main Portal governance and the applicable business context.

**B7. Search, Reporting & Analytics Infrastructure**

Shared infrastructure capabilities for search, indexing, query processing, report generation, export processing, analytics, aggregation, and scheduled report execution.

This category provides common infrastructure and shared execution capabilities; it does not take ownership of the business meaning, definitions, calculations, or authoritative records of a domain report. A report whose business meaning belongs to Accounting remains governed by the Accounting domain; Inventory, HR, and other domain reports remain governed by their respective domains. Search, reporting, analytics, and export results must enforce the same Data Isolation, Service Scope, User Assigned Authority Scope, Permissions, purpose limitation, and Audit Trail requirements as direct access to the underlying data.

#### Group C — Platform Operations & Governance Services

**C1. Audit & History Infrastructure**

Shared infrastructure capabilities supporting Domain History and the comprehensive Audit Trail, including event capture pipelines, integrity protection, access-control enforcement, retrieval, export, retention, archival, and integrity verification.

This category must preserve the mandatory separation in Section 17A. Domain-specific History and the comprehensive Audit Trail are separate record categories with different purposes, contents, permissions, and retention rules. They must never be merged into one record set, one undifferentiated log, or one permission domain. Shared infrastructure does not mean shared records or shared authorization. Audit & History Infrastructure does not redefine which business events belong in a particular Domain History or which system events must be recorded in the Audit Trail.

**C2. Observability & Monitoring Services**

Shared operational capabilities for logs, metrics, distributed traces, service health, performance/error/availability monitoring, operational alerting, and diagnostic context.

Observability is for operating, diagnosing, and improving service reliability. It is not Domain History and is not the comprehensive Audit Trail. An operational signal such as an API error, latency spike, or service outage may be relevant to operations without being a business-history event; a security-sensitive or business-significant action must still be recorded in the Audit Trail where required. Operational logs and traces must not be treated as a substitute for the governed audit record, and access to them must respect sensitive-data and Tenant boundaries.

**C3. Background Processing & Scheduling Services**

Shared capabilities for asynchronous/background jobs and scheduled or recurring work, including job dispatch and execution coordination, retry and failure handling, priorities, workload control, idempotent job support, and long-running task coordination.

Examples include scheduled report generation, bulk notification delivery, exports, archival tasks, and reconciliation. This category provides execution and scheduling infrastructure; the business meaning, authorization, and final state changes remain owned by the relevant domain. Background execution must retain the initiating or system principal, applicable business context, authorization basis, and required Audit Trail context. A scheduled job does not acquire broader authority merely because it runs without an interactive User.

**C4. Platform Administration & Operational Management Services**

Shared capabilities supporting administration and operation of the CBM platform and its service-provider ecosystem, including platform service registry, service lifecycle administration, Service Provider administration support, operational configuration, capacity/resource management, backup and disaster-recovery coordination, maintenance, and operational readiness.

Main Portal retains supreme ecosystem governance and control. A Sub-Portal may administer only the service-provider and operational matters delegated to it by Main Portal. Tenant or Business administrators may manage only matters explicitly delegated to their scope. This category does not grant any administrative actor broader authority, cross-Sub-Portal access, or customer-business ownership. Backup and disaster-recovery coordination must preserve Data Isolation, identity and ownership integrity, and the separate retention obligations for business data, Domain History, and Audit Trail.

### 6A.4 Category, Service, and Capability levels

The approved conceptual hierarchy has three distinct levels:

- **Category:** A governed functional grouping of related Platform-wide Service responsibilities.
- **Service:** A defined responsibility or cohesive service offering within a Category, with an explicit purpose, scope, consumers, inputs/outputs, ownership, policy boundaries, and service contract.
- **Capability:** A specific function or operation that a Service makes available to its authorized consumers.

For example, within B1 Communication & Notification Services, Communication Services may include Email Delivery, SMS Delivery, and WhatsApp Delivery; Notification Services may include Notification Routing, Notification Preference Management, and Notification Template Management. These examples illustrate the hierarchy and do not require a separate deployment component for each item.

The three levels are conceptual architecture and governance distinctions. They do not mean that every Category, Service, or Capability must be implemented as a separate microservice, application, process, database, vendor product, or independently deployed unit. Technical decomposition must be decided separately when implementation architecture is authorized.

Each Service and Capability must remain within its Category's approved responsibility and must identify any dependency on other services. A capability must not be duplicated under another Category in a way that creates conflicting ownership or competing sources of truth. Where responsibilities cooperate, their contracts and decision ownership must remain explicit.

### 6A.5 Cross-category responsibility and boundary rules

1. **Identity, authentication, authorization, and security are distinct.** Identity identifies the principal; authentication verifies the principal; authorization evaluates whether the principal may perform a specific action in a specific context; Security & Trust provides cross-cutting protective controls. One responsibility must not silently absorb or replace another.
2. **Authorization evaluates approved CBM rules; it does not create them.** The existing Business Authority Scope, User Assigned Authority Scope, User Scope Mapping, Roles, Permissions, Delegated Authority, approval requirements, and Main Portal governance remain authoritative in their respective domains.
3. **Approval is not authorization.** Workflow & Approval executes an approved approval requirement. Approval does not supply missing action Permission, User scope, business authority, or Stable Core eligibility.
4. **Notification, delivery, events, and integrations are distinct.** Notification determines what should be communicated to a person; Communication delivers it; Event & Messaging distributes internal/platform events; Integration & API provides external/system interfaces.
5. **Domain ownership remains with the relevant Business Module.** Shared services provide common capabilities and contracts. They do not take ownership of Accounting, Inventory, HR, POS, or other module-specific business rules or authoritative records merely because they process or transport related data.
6. **Audit, History, and Observability remain distinct.** Audit & History Infrastructure supports the separately governed Domain History and Audit Trail records. Observability & Monitoring supports operational diagnosis. Neither operational logs nor event queues replace the Audit Trail.
7. **Shared infrastructure never means shared business access.** Every service operation, event, message, search result, report, export, file retrieval, and background job must preserve the relevant Service Scope, valid business context, authorization decision, purpose limitation, and strict Data Isolation.
8. **No authority by technical connectivity.** A service registration, API key, integration connection, event subscription, queue membership, file identifier, or successful authentication does not itself create Business Authority Scope, User Assigned Authority Scope, a Role, Permission, Ownership Link, or Business Relationship.
9. **Main Portal governance remains supreme.** Sub-Portals and customer businesses may configure or operate only the services and settings within their expressly delegated authority. No Platform-wide Service may enable upward authority, peer Sub-Portal control, or a lower-level override of Stable Core rules.
10. **No assumed category expansion.** A capability that appears useful does not automatically create a new Category or alter the approved taxonomy. A new Category or a material change to a Category's responsibility requires explicit project-owner approval and an update to this Master.

### 6A.6 Applicability and design obligations

Every proposed Platform-wide Service, Service, and Capability must define, as applicable:

- Its business purpose and responsibility boundary.
- Its Category and any dependent Categories/Services.
- Its supported Service Scope(s) and eligible contexts.
- Its provider/operational ownership and Main Portal governance boundary.
- Its authorized producers, consumers, Users, and system principals.
- The business data and metadata it may process, and the data it must not expose.
- Its authorization, approval, and security requirements.
- Its configuration, inheritance, customization, and permitted override rules.
- Its applicability to business types, entities, modules, locations, and operating conditions.
- Its failure, retry, recovery, continuity, retention, and audit requirements where relevant.
- Its service contract, versioning, and compatibility expectations where relevant.

These are architecture-definition obligations, not an approval of a specific technical implementation. The detailed design of any individual service remains subject to the relevant domain decisions and explicit project authorization.

## 6B. Global Business Settings

### 6B.1 Purpose and architectural position

**Global Business Settings** provides common, governed Indian business-context defaults, preferences, and reference settings across CBM. It is a shared configuration responsibility governed by Section 6A (Platform-wide Services), Section 10 (Configuration governance), the Stable Core principles, and Business Module responsibility boundaries.

It provides shared defaults and reference conventions. It does not own Accounting, Tax/GST, Inventory, Human Resource Management and Payroll Management, Finance/Payment, POS, or other domain-specific rules, calculations, statutory determinations, or authoritative business records.

### 6B.2 India-only product and regional scope

CBM is designed exclusively for businesses operating in the Indian business and regional context. The approved product scope is India-only; CBM is not a multi-country or international business platform.

The fixed platform-wide boundaries are:

1. **Country and business market:** India.
2. **Platform default language:** English.
3. **Supported languages:** Indian languages only. Main Portal governs which Indian languages are enabled and available; the principle of Indian-language support does not require every language to be enabled at launch.
4. **Currency:** Indian Rupee (INR) only, represented by ₹. No selectable foreign currencies or multi-currency business operation.
5. **Time zone:** Indian Standard Time (IST), using Asia/Kolkata, is the sole platform time zone. No multi-time-zone operation or user-selectable time zones.
6. **Number presentation:** Indian number grouping, including thousand, lakh, and crore conventions where applicable.
7. **Measurement baseline:** Metric conventions, with additional India-relevant business units supported where required by legitimate business contexts.
8. **Regional references and addresses:** Indian address conventions and Indian administrative references, including States, Union Territories, Districts, PIN Codes, and other relevant India-specific reference data.

No multi-country, multi-currency, multi-time-zone, or international-localization framework is part of the approved product scope. A future change requires explicit project-owner approval and a corresponding Master update; extensibility does not imply such approval.

### 6B.3 Approved Global Business Settings domains

Global Business Settings contains the following six approved setting domains. These are responsibility groupings, not a requirement for six separate applications, services, databases, or deployment units.

#### A. Language and Regional Language Settings

This domain governs English as the platform default; Main Portal-governed availability of supported Indian languages; applicable business-scope default language; individual User language preference where supported; language applicability; and translation resources and language-specific presentation.

A User's display-language preference does not change Business Identity, Legal Identity, Professional Identity, statutory names, source-language business records, or authoritative business content. It does not grant access or change transaction meaning.

#### B. Date, Time, and Calendar Settings

This domain governs IST / Asia/Kolkata as the sole time-zone reference; permitted date display formats; 12-hour or 24-hour time display; week-start convention; Gregorian as the core System Calendar; general business-calendar and working-day defaults; and Indian public, festival, and State/UT-specific holiday references where maintained and enabled.

There is no configurable time zone at platform, Service Provider, Tenant, Business, or User level. Display preferences do not alter authoritative timestamps, transaction dates, effective dates, or recorded dates. Holiday and working-day references do not automatically determine statutory, payroll, banking, accounting, or module-specific obligations.

#### C. Indian Number and Currency Settings

This domain governs INR and ₹ presentation; Indian digit grouping and lakh/crore presentation; permitted amount-display conventions; decimal-place presentation within the applicable domain's monetary precision rules; negative-amount presentation; and common number formatting.

Foreign-currency selection, foreign-currency transactions, exchange-rate management, and multi-currency accounting are outside the approved scope. Monetary precision, rounding, tax treatment, accounting calculations, and authoritative transaction values remain governed by the relevant Business Module and applicable law. Display preferences must not change stored amounts or financial results.

#### D. Indian Measurement and Units Settings

This domain governs shared Metric references such as kilogram, gram, metric tonne, metre, centimetre, millimetre, litre, and millilitre; India-relevant business units such as square metre, square foot, and acre where applicable; shared unit references and explicitly defined conversions; and permitted default unit preferences.

A shared unit reference does not make that unit applicable to every module or transaction. Product-, inventory-, accounting-, logistics-, and other domain-specific permitted units, conversion precision, and authoritative quantity calculations remain owned by the relevant Business Module. Presentation conversion must not silently alter a transaction's source quantity or source unit.

#### E. Indian Regional and Address Settings

This domain governs India as the fixed country context; Indian States and Union Territories; Districts and relevant administrative references; PIN Code references and validation support where available; Indian address structure and presentation; and regionally applicable shared reference data.

Reference data does not itself prove residence, a Business Identity's legal address, a GST Place of Business, or legal eligibility. Address ownership and verification remain with the relevant identity or business domain.

#### F. Indian Business Defaults

This domain governs common Indian business-context defaults, including Financial Year defaults, general business-calendar defaults, general working-day defaults, common business-operating preferences not owned by a specific Business Module, and other expressly governed shared India-specific business defaults.

The default Financial Year is **1 April through 31 March**. An alternative may be used only where permitted for the relevant business and purpose, supported by applicable domain rules, and explicitly authorized at the appropriate governance level. This default is not a universal legal requirement for every business, entity, report, or statutory purpose.

General defaults do not replace Accounting, Tax/GST, Human Resource Management and Payroll Management, Inventory, POS, Finance/Payment, or other domain rules. Statutory accounting treatment, tax periods and returns, payroll periods, inventory valuation periods, and domain-specific calendars remain governed by their respective Business Modules and applicable law.

### 6B.4 Governance paths and configuration authority

Global Business Settings respects distinct platform/service-provider and customer-business governance paths. These are not one universal structural hierarchy.

**Path A — Platform and Service Provider governance**

1. **Main Portal** establishes mandatory India-only product boundaries, platform defaults, approved language availability, shared Indian reference-data governance, and mandatory platform policies.
2. **Sub-Portal** may configure or administer only Service Provider-scoped settings and operational preferences expressly delegated by Main Portal. A Sub-Portal is not a structural parent of an Organization and does not automatically control or override that customer's business settings.
3. A Sub-Portal's settings remain separate from customer-business settings and another Sub-Portal's private settings.

**Path B — Customer Business governance**

1. **Organization** governs its eligible business settings and establishes applicable defaults for its direct structural Distributor and Shop children.
2. **Distributor and Shop** may maintain independently applicable settings only where the setting permits it and the entity has explicit authority to override or customize the inherited value.
3. Organization → Distributor / Shop may govern inheritance for customer-business settings. It does not create a relationship to a Sub-Portal's service-provider configuration.
4. Settings must not propagate to sibling Organizations, Distributors, Shops, or unrelated contexts unless an explicit approved inheritance rule applies.

**Path C — User preferences**

1. A User may maintain individual presentation preferences only where the setting supports User-level preference.
2. User preferences are subordinate to mandatory platform policy and applicable business settings.
3. User preferences cannot change authoritative business settings, business records, legal particulars, financial calculations, transaction values, time-zone rules, or domain-owned policy.
4. There is no User-selectable time zone or foreign currency.

These paths do not grant authority by role name, structural position, service-provider association, or access to a settings interface. The actor requires explicit Permission and any required Delegated Authority or approval.

### 6B.5 Configuration, inheritance, applicability, and override

All Global Business Settings are subject to Section 10. Each setting must identify its purpose, owning domain, eligible contexts, governing authority, default, inheritance path, permitted override levels, allowed values, effective date, version/change history, and audit requirements as applicable.

Main Portal's mandatory India-only boundaries are non-overridable through Sub-Portal, Tenant, Organization, Distributor, Shop, or User configuration. This includes India-only country scope, INR-only currency, IST-only time zone, and Indian-languages-only language scope.

Within those fixed boundaries, Main Portal may establish platform defaults and mandatory policies; Sub-Portals may manage only delegated Service Provider-scoped settings; Organizations may configure eligible customer-business settings and defaults for their direct children; Distributors and Shops may override inherited settings only where explicitly permitted; and Users may set only supported personal presentation preferences. No lower-level setting may change a higher-level value or affect a sibling/unrelated context. Applicability must be explicit.

### 6B.6 Separation from Business Module ownership

Global Business Settings must not take ownership of:

- GSTIN, GST Taxpayer Profile, GST Registration, GST Registration Assignment, GST applicability, taxpayer eligibility, or statutory GST determinations.
- GST rates, tax classification, tax calculation, tax liability, returns, or tax-period rules.
- Invoice numbering, invoice lifecycle, or domain-specific document numbering.
- Accounting policies, chart of accounts, ledger rules, accounting treatment, financial statements, or authoritative accounting calculations.
- Payroll, salary, employee statutory benefits, attendance, leave, or HR-specific calendar rules.
- Product-specific units, product master data, inventory quantity rules, stock valuation, or inventory conversion calculations.
- Business Identity, Legal Identity, Professional Identity, statutory particulars, or verified business addresses.
- Bank accounts, payment instruments, payment-provider configuration, settlement, or Finance/Payment rules.

Global Business Settings may provide a shared default, display convention, or reference dataset only where the relevant Business Module explicitly consumes it and retains ownership of its rules and authoritative records. A shared default never establishes statutory eligibility or legal compliance by itself.

### 6B.7 Data integrity, audit, and cross-service safeguards

1. Stored timestamps and authoritative transaction dates remain independent of date/time display preferences.
2. Language preferences do not translate, replace, or alter authoritative legal names, source records, or transaction meaning.
3. Number and currency display preferences do not change stored amounts, calculations, or accounting results.
4. Unit presentation or conversion does not silently change source quantities or source units.
5. Shared regional reference data is not proof of legal identity, address, registration, or eligibility.
6. Every read or change preserves applicable Service Scope, business context, User Assigned Authority Scope, Permission, purpose limitation, and strict Business/Tenant Data Isolation.
7. Configuration changes, overrides, effective dates, and their authorizing authority are auditable under the applicable Audit Trail rules.
8. Localization & Reference Data Services under Section 6A may provide infrastructure and reference datasets; Global Business Settings governs approved CBM defaults and applicability. Neither takes ownership of domain-specific business rules.

### 6B.8 Relationship to Platform-wide Services

Global Business Settings is a governed configuration/business-settings responsibility that may use Platform-wide Services, particularly Configuration & Policy Management Services and Localization & Reference Data Services. Those services provide infrastructure and common capabilities; they do not determine a setting's business meaning, authority, or domain ownership.

The India-only scope is a product/business boundary, not a technical localization implementation specification. Generic technical capabilities in B6 do not authorize international language, country, currency, or time-zone support: CBM's enabled language set remains Indian languages only, country and regional references remain India-specific, currency remains INR, and time-zone handling remains IST / Asia/Kolkata. Language-resource implementation, formatting libraries, reference-data storage, and service decomposition remain outside this business and logical-architecture decision.

---

## 6C. Accounting Management

### 6C.1 Approved architectural classification and purpose

**Accounting Management is an independent Business Module.** Its classification and the business/logical architecture scope in this Section were explicitly approved by the project owner on 19 November 2026.

Accounting Management is responsible for the governed recognition, classification, recording, posting, control, reconciliation, and financial reporting of the financial effects of business activities within its authorized Accounting Contexts and Books of Accounts. It owns its Accounting Policies, Chart of Accounts, Account Groups, Ledger Accounts, Voucher Types, Accounting Vouchers, Journal Entries, Ledger Postings, Accounting Period controls, and authoritative Accounting records.

The Module is not merely a data-entry facility or a collection of financial reports. It is the authoritative accounting domain for the Books of Accounts it governs. It must support the full accounting lifecycle while preserving the source-domain ownership of Sales, Purchase, Inventory, POS, GST/Tax, Finance/Payment, Human Resource Management and Payroll Management, Business Identity, and other Business Modules.

This Section establishes Business and Logical Architecture responsibilities. It does not prescribe database schemas, API endpoints, application screens, software packages, deployment units, or implementation technologies.

### 6C.2 Foundational accounting principles

1. **Double-entry by default:** CBM's default Accounting model is Double-entry Accounting. Every posted Accounting Entry set must have equal total debits and credits. A single transaction may contain multiple debit and credit lines.
2. **Accrual by default:** The default recognition basis for general-purpose Accounting Books is accrual basis. A cash basis or other alternative may be used only where the applicable law and Accounting Framework permit it and an authorized, effective-dated Accounting Policy expressly enables it. This is a product default, not a claim that every Indian business is legally required to use the same basis.
3. **Authoritative records:** Each Book of Accounts has a clearly identified accountable owner and one authoritative set of posted records. Reports, exports, exchange packages, caches, and external copies do not replace those records.
4. **Traceability:** Every posted record must be traceable to its originating source, authorized manual action, migration batch, or other valid accounting basis.
5. **No silent mutation:** Posted records must not be silently overwritten or destructively altered. Corrections must preserve the original record and use governed reversal, adjustment, or correction records.
6. **Domain separation:** Accounting owns the accounting treatment and accounting records, not the original commercial, operational, tax, payment, inventory, payroll, or identity records owned by other domains.
7. **India-only boundaries:** Accounting operates within the approved India-only, INR-only, IST-only, Indian-language and regional boundaries in Sections 2 and 6B. Foreign-currency accounting and multi-currency operation are outside the approved product scope. A source import containing foreign-currency transactions must be identified as an exception; it must not be silently converted or posted as INR.
8. **Framework compliance:** Accounting Frameworks and Policies must reflect the legal and professional requirements applicable to the actual entity and purpose. Configurability cannot be used to select an inapplicable standard or override mandatory requirements.

### 6C.3 Accounting Contexts and Books of Accounts

1. Accounting Context and Book of Accounts are separate concepts. An Accounting Context defines the accountable business/legal scope and governance; a Book of Accounts is the coherent set of accounting records maintained for a defined purpose within that context.
2. Every statutory Book must have exactly one accountable Legal Identity. Separate Legal Identities must maintain separate authoritative statutory Books. Common ownership, common branding, common service provider, common Organization-level ownership rules, or a Distributor–Shop Mapping does not merge their Books.
3. One Legal Identity may maintain more than one Book where a distinct statutory, regulatory, management, operational, or other expressly governed accounting purpose justifies it. Each additional Book must have a defined purpose, owner, policy, scope, effective period, and reconciliation relationship. It must not create a competing or ambiguous authoritative statutory record.
4. Multiple Professional Identities, branches, Shops, departments, or operating locations under the same Legal Identity may ordinarily be represented in the same statutory Books using authorized Accounting Dimensions, Cost Centres, or other attribution rules. A separate Professional Identity alone does not require a separate statutory Book.
5. Where a Distributor or Shop has a distinct Legal Identity, its statutory Accounting Books must be separate from the Books of another Legal Identity. Where it operates under a Legal Identity shared with other Professional Identities, the relationship to that Legal Identity must be explicitly verified and its transactions attributed through approved dimensions or other governed accounting arrangements.
6. A Shop's optional Legal Identity must not be invented or inferred from its Professional Identity, Owner ID, Parent Organization, address, or Mapping. The accountable Legal Identity for its accounting must be explicitly established.
7. An Accounting Context may contain multiple Books for the same accountable Legal Identity and approved purposes. A consolidated reporting group may refer to Books of multiple legal entities only under explicit authority and applicable accounting/reporting rules. Consolidation does not merge the underlying Books, Legal Identities, Ownership Links, Tenant boundaries, or source records.
8. Accounting applicability, Book creation, access, consolidation, and reporting must not be inferred from structural placement, ownership, Service Provider association, Subscription Coverage, or User Role. Explicit authority, Permission, and applicable scope are required.

### 6C.4 Accounting Frameworks, Policies, and recognition basis

1. Accounting must support the Indian Accounting Framework applicable to each entity and reporting purpose, including Ind AS, Accounting Standards (AS), or another framework permitted or required under applicable Indian law and authoritative regulation.
2. Framework applicability must be determined from the actual legal/entity classification, regulatory status, sector, reporting purpose, and effective law. The platform may provide governed applicability profiles and guidance, but a User must not freely select an inapplicable framework.
3. Applicable Accounting Frameworks, statutory presentation requirements, and accounting rules must be maintained as versioned and effective-dated rule profiles. Changes in authoritative law or standards must be reviewed and introduced through governed updates; existing posted records and historical reports must retain their applicable historical basis.
4. Accrual basis is the default recognition basis for general-purpose Books. Cash basis or another recognition approach may be enabled only where legally and professionally permitted for the specific entity and purpose. Cash-oriented management views must not alter accrual-based statutory Books.
5. Double-entry is the default and mandatory basis for all ordinary posted CBM Accounting Entries. Any exceptional statutory or legacy source representation must be normalized, reconciled, and accepted into the CBM Double-entry model before it becomes authoritative.
6. Accounting owns accounting policies, recognition within the applicable framework, account classification, measurement and presentation of its financial records. GST/Tax owns tax determinations and statutory tax rules; Inventory owns product/stock quantity and inventory valuation determinations; Finance/Payment owns payment execution and settlement source records.
7. The default Financial Year remains 1 April through 31 March. Any alternative is permitted only under the existing Section 6B rules and the applicable Accounting Framework and domain requirements.

### 6C.5 Chart of Accounts, Account Groups, and Ledger Accounts

1. Each Book must have a governed Chart of Accounts containing hierarchical Account Groups and Ledger Accounts.
2. The Chart of Accounts must provide a coherent baseline classification for assets, liabilities, equity/capital, income, and expenses, together with any further classifications required by the applicable Accounting Framework and reporting needs.
3. Main Portal may govern protected foundational classifications, standard templates, identifiers, and minimum reporting requirements. Authorized businesses may create additional Groups and Ledgers, subject to classification integrity, applicability, and reporting rules.
4. Account Groups classify Ledger Accounts and may form parent-child hierarchies. Ledger Accounts hold the accounting balances and transaction postings.
5. Ledger Accounts may reference Customer, Supplier, Bank Account, Tax, Business Identity, Product/Inventory, Employee/Payroll, or other domain records where required. Such a reference does not transfer ownership of the referenced record to Accounting.
6. A shared party or business identity may be referenced by multiple Accounting Books only where each Book has its own authorized accounting relationship. The reference must not merge the Books or grant cross-business access.
7. System templates and custom Charts of Accounts must be versioned or otherwise controlled so that changes do not silently rewrite historical classifications or financial statements.

### 6C.6 Voucher Types, Accounting Vouchers, Journals, and Posting

1. Accounting must support distinct Voucher Types and Accounting Vouchers. A Voucher Type classifies the accounting purpose and defines permitted structure, numbering, validation, approval, and posting behaviour. A Voucher records a particular accounting transaction or adjustment.
2. The foundational Voucher Type set must include the common accounting purposes of Contra, Payment, Receipt, Journal, Sales, Purchase, Debit Note, and Credit Note. Further types may be added when justified by business and accounting requirements. These names are conceptual accounting types and do not transfer Sales, Purchase, Tax, POS, or Payment domain ownership to Accounting.
3. Authorized businesses may configure permitted Voucher Types, numbering series, prefixes/suffixes, effective-date rules, narration requirements, approval conditions, and validated posting templates. Protected system controls and mandatory framework requirements cannot be overridden.
4. A Voucher may contain multiple Accounting Entry Lines. Each line must identify its Ledger Account, debit or credit amount, applicable Book and period, and permitted source and analytical references. Each posted Voucher's total debit must equal its total credit.
5. Journal Entries and Ledger Postings are derived from the approved Accounting Voucher and are maintained as authoritative accounting records. A Voucher, its Journal Entry, and its resulting Ledger postings are related but distinct conceptual records.
6. Voucher states must support controlled preparation and posting, including Draft, Validated, Pending Approval where applicable, Posted, and reversal/correction conditions. Exact state applicability may vary by Voucher Type and approved policy.
7. Posting must require valid Book, period, accounts, balanced entries, required source evidence, applicable approvals, and Permissions. A successful source transaction or an Event Message does not by itself prove that accounting posting has completed.
8. A posted Voucher must not be silently edited or deleted. Corrections must use traceable reversal, adjustment, or correction records with explicit links to the original Voucher and appropriate effective dates.
9. Voucher numbering must be unique within its defined numbering scope and governed series. Source-system Voucher Numbers must also be retained as external references where imported; they must not replace CBM's own identity and uniqueness rules.

### 6C.7 General Ledger, Sub-ledgers, and Outstanding Allocation

1. Accounting owns the General Ledger and the balances derived from posted entries in each Book.
2. Sub-ledgers may provide party-wise or obligation-wise detail for control accounts such as Trade Receivables and Trade Payables. Sub-ledger totals must reconcile to their corresponding control Ledger Accounts.
3. Outstanding Allocation must support relevant bill-wise or obligation-wise references, including advances, new references, settlement against existing references, partial settlement, and on-account amounts where applicable.
4. Customer and Supplier identities and original Sales/Purchase documents remain owned by their source domains. Accounting maintains its ledger references, balances, allocation history, and accounting treatment.
5. Cash, Bank, Loan, Tax, Inventory, Fixed Asset, Income, Expense, Receivable, Payable, Equity, and other appropriate Ledger classifications must be supported where applicable to the Book and Accounting Framework.

### 6C.8 Cost Centres, Cost Categories, and Accounting Dimensions

1. Accounting must support Cost Centres and Cost Categories for permitted income, expense, cost, and profit analysis.
2. Cost Centres may be hierarchical and may represent departments, branches, projects, activities, people, operating units, or other management dimensions.
3. Cost Categories may enable parallel allocation of the same eligible transaction across different analytical sets. Governed allocation templates or classes may automate permitted percentage-based or rule-based allocations.
4. Additional Accounting Dimensions may be introduced through explicit, versioned extension rules. Each dimension must define its meaning, eligible accounts/transactions, applicability, validation, aggregation, reporting, and access rules.
5. A Cost Centre or Accounting Dimension is not a Business Entity, Legal Identity, Ownership Link, structural parent-child relationship, Tenant, User Scope, or Permission. Mapping an Accounting Dimension to a Shop, Distributor, branch, or department does not create or modify those separate relationships.
6. Allocation rules must be traceable, effective-dated where applicable, and reconcilable to the original posted amount. They must not duplicate the underlying financial transaction.

### 6C.9 Management Accounting Information and Planning Boundary

1. Accounting may provide management-accounting information derived from its authoritative posted records, including cost analysis, profitability analysis, margin analysis, financial performance analysis, and actual-versus-plan comparisons where an authorized planning source exists.
2. Accounting owns the accounting classifications, actual financial amounts, accounting calculations, and accounting-originated analytical results. It does not thereby own enterprise-wide business planning, operational plans, workforce plans, sales plans, procurement plans, or the creation and approval of all budgets and forecasts.
3. Budgets, forecasts, targets, and plans are planning records. Their creation, revision, or approval does not itself create a posted financial transaction or alter the General Ledger.
4. A business-planning or budget-management capability/module, where provided, owns its planning records, planning workflow, versions, targets, and approved budget baselines. It consumes Accounting actuals through explicit contracts; Accounting consumes approved budget/plan references needed for financial comparison without becoming their source of truth.
5. A budget or forecast may be financially structured and may use approved Accounts, Accounting Periods, and Accounting Dimensions, but those references do not transfer planning ownership to Accounting.

### 6C.10 Fixed Asset Accounting Boundary

1. Accounting owns the financial accounting treatment and records for fixed assets, including acquisition cost, capitalization, depreciation, impairment, revaluation where applicable, disposal, and related disclosures and ledger treatment, under the Accounting Framework applicable to the Book.
2. Accounting may maintain the financial fixed-asset register and accounting attributes needed to support capitalization, depreciation, impairment, disposal, ledger balances, and financial reporting. This register is not a substitute for a full operational Asset Management record.
3. Physical asset master administration, custody, location, assignment, maintenance, repair, inspection, insurance administration, utilization, and operational asset lifecycle belong to a separate Asset Management responsibility/module where that capability is provided. Accounting receives approved asset events and references and records their financial effects.
4. Asset Management and Accounting must use explicit shared Asset Identity and Asset Event references, with Accounting retaining ownership of financial values and postings and Asset Management retaining ownership of operational asset facts.
5. Fixed Asset Accounting must follow the Accounting Framework applicable to the Book and must not hard-code one depreciation, useful-life, or valuation rule for every asset and business.

### 6C.11 Accounting Periods and Closing

1. Each Book must have explicitly defined Accounting Periods within its applicable Financial Year and Accounting Framework.
2. The default Financial Year is 1 April through 31 March, subject to Section 6B. An alternative requires applicable domain support and explicit authorization.
3. Accounting must support opening balances, period transactions, adjustments, closing balances, period review, closing, and controlled reopening.
4. A closed period must be protected against unauthorized postings and changes. Reopening requires explicit Permission, applicable approval, a recorded reason, and Domain History/Audit Trail evidence.
5. Period closing must not delete or rewrite source transactions, posted entries, or previous closing evidence. Opening balances for a subsequent period must be traceable to the prior authorized closing state and permitted adjustments.

### 6C.12 Reconciliation

1. Accounting must provide reconciliation capabilities for its own records and for authorized comparisons with external or other-domain source records.
2. Reconciliation may include Bank-to-Ledger, Customer/Supplier sub-ledger-to-control-ledger, source-transaction-to-accounting-posting, Inventory valuation-to-accounting, GST determination-to-accounting, Payment/Settlement-to-accounting, and migration-source-to-imported-record comparisons.
3. The source domain remains the owner of its source records. Accounting owns the reconciliation result and accounting adjustments within its own Books; it cannot alter a source-domain record merely to force a match.
4. Reconciliation must identify matches, timing differences, omissions, duplicates, unmatched items, and exceptions. Resolution must be authorized, traceable, and must not silently change posted records.

### 6C.13 Financial Statements and Reporting

1. Accounting owns the authoritative accounting calculations and financial content for Trial Balance, General Ledger, Profit and Loss Statement, Balance Sheet, Cash Flow Statement where applicable, and other financial statements required by the relevant Accounting Framework.
2. It must support ledger, voucher, day-book, period, receivables, payables, cost-centre, cost-category, and other relevant accounting reports.
3. Financial statements must use the applicable Accounting Framework, Accounting Policies, approved Book scope, reporting period, and effective rules. Presentation and calculations must not be determined solely by a User display preference.
4. Consolidated reporting across Books or Legal Identities may be produced only where the relationship, authority, reporting purpose, and applicable Accounting Framework permit it. Consolidated reporting does not merge underlying Books, Legal Identities, ownership, or data boundaries.
5. Shared Reporting & Analytics infrastructure may distribute, visualize, or analyse accounting reports under authorization. It does not own Accounting calculations, accounting classifications, or authoritative financial statements.
6. GST returns, tax determinations, payroll reports, payment statements, and inventory valuation reports remain owned by their respective Business Modules. Accounting may provide its own accounting views and authorized references to those domains.

### 6C.14 Integration with other Business Modules

1. Accounting receives authorized source transactions or domain facts from Sales, Purchase, POS, Inventory, Warehouse Management, GST/Tax, Finance/Payment, Human Resource Management, Payroll Management, Subscription/Billing, and other eligible Business Modules through explicit contracts.
2. The originating Business Module retains ownership of the original transaction, source document, operational lifecycle, and domain rules. Accounting owns only its accounting interpretation, Voucher, Journal Entries, Ledger Postings, balances, and accounting history.
3. Each source-to-accounting mapping must identify the source event/record, eligible Book, Voucher Type, Ledger mapping, tax/valuation/payment references where applicable, effective Accounting Policy, posting status, and correction behaviour.
4. Mapping and posting rules must be versioned and effective-dated. A change in a source module or a mapping must not silently rewrite already posted accounting records.
5. Source transaction status and Accounting Posting status are separate. A source transaction may exist before its accounting posting; a failed or pending posting must be visible as an exception and must not be represented as completed accounting.
6. Event publication, API success, payment success, invoice issuance, stock movement, or a POS checkout does not independently establish that the corresponding Accounting Voucher has been validated and posted.
7. Duplicate source events or retries must not create duplicate Accounting Vouchers or postings. Corrections to source transactions must trigger governed accounting adjustment, reversal, or reprocessing rules without deleting the prior accounting history.
8. Accounting must not directly take over or independently recreate the source domain's validation logic. It validates the accounting requirements and the integrity of the data needed for its own posting.

### 6C.15 Domain Ownership Boundaries and Exclusions

Accounting Management owns accounting treatment and accounting records, not the following originating responsibilities:

- **Business Identity:** Legal Identity, Professional Identity, Owners, statutory identity particulars, and verified business identity records.
- **GST/Tax:** GSTIN, GST Taxpayer Profile, GST Registration, GST applicability, tax classification, tax rates, statutory tax calculation, tax liability determination, returns, and tax compliance records.
- **Finance/Payment:** Bank Account ownership/administration, payment instruments, payment initiation/execution, collection, settlement, payment-provider configuration, and authoritative payment records.
- **Sales:** Customer-facing sales lifecycle, commercial sales terms, sales order, original Sales Invoice, delivery, and Sales Returns.
- **Purchase:** Supplier-facing purchase lifecycle, purchase terms, purchase order, original Purchase Invoice, goods receipt ownership where assigned, and Purchase Returns.
- **Product/Item Management:** Product and item master definitions, variants, product attributes, product classifications, and commercial product identity.
- **Inventory Management:** Authoritative stock quantities, stock ledger, reservations/availability, stock states, batch/serial/expiry-controlled stock facts, and inventory valuation determination.
- **Warehouse Management:** Warehouse facilities, zones, aisles, racks, shelves, bins, handling-unit placement, warehouse work, and physical warehouse execution.
- **POS:** Checkout, sales capture, POS operational transaction, and POS-specific lifecycle.
- **Human Resource Management:** Employee identity and employment records, recruitment, employment relationships, attendance, leave, workforce administration, and HR policies.
- **Payroll Management:** Payroll calculations, payroll runs, payslips, statutory payroll deductions/obligations, and authoritative payroll records.
- **Subscription/Billing:** Commercial Account, Subscription, Plan, Entitlement, subscription invoice, and commercial billing lifecycle as defined by Section 14.
- **Platform-wide Services:** Shared authorization, security, approval execution, messaging, integration infrastructure, file/document infrastructure, and reporting infrastructure.

Accounting may maintain corresponding Ledger Accounts, accounting references, posting records, or reports where necessary. Such records do not transfer source-domain ownership.

### 6C.16 External Accounting-System Data Exchange and Optional Tally Adapter

1. The External Accounting Data Exchange Model is a vendor-neutral import/export boundary. It translates selected external Accounting data into or from CBM's canonical Accounting contracts; it does not define CBM's internal Accounting concepts, policies, Books, or authoritative records.
2. A Tally Data Exchange profile and Tally Compatibility Adapter are optional. They are relevant only when a customer needs a supported Tally data-transfer path. They are not mandatory platform features, and Tally's internal structures, labels, workflows, defaults, or version-specific behaviour must not determine CBM's Accounting model or any other Business Module's architecture.
3. Every external Accounting exchange must use the shared Integration & API, Security, Authorization, and Audit capabilities. Accounting validates and accepts only Accounting-owned records. Data belonging to Product/Item Management, Inventory, Warehouse, GST/Tax, HR, Payroll, Finance/Payment, Sales, Purchase, POS, or another domain must be routed to that domain and accepted under its own rules.
4. Import scope must be selected according to the business-continuity need, source availability, data quality, legal/authorization constraints, and supported exchange capability. Importing all source records, historical customizations, or every source-system feature is not mandatory. Source masters, opening balances, transactions, and supporting references are imported only to the extent they are relevant, available, valid, sufficiently detailed, and accepted by the receiving domain.
5. CBM's approved Accounting model and policies remain authoritative after onboarding. For example, source data maintained under a single-entry approach must not cause CBM to adopt that approach where CBM's approved Accounting model requires double-entry by default. Conversion to CBM records is permitted only where reliable source facts and approved, traceable mappings are sufficient. Missing or ambiguous accounting facts must be reported for authorized resolution; the adapter must not invent entries, balances, dates, counterparties, or accounting explanations.
6. Export must provide the selected CBM Accounting data needed for business continuity in a format and structure that the intended receiving system is documented to accept. An export is not a promise to recreate the customer's former system configuration, customizations, workflows, or proprietary company backup. Any claimed import compatibility at the destination must be supported by documented and validated capabilities.
7. Tally-specific format/version support, where required, is verified against the applicable official Tally exchange documentation and remains confined to that optional adapter. The same vendor-neutral exchange boundary must permit other supported Accounting System adapters without changing CBM's canonical model.
8. External exchange success does not itself create or post an Accounting record. Every imported record must pass the receiving domain's mapping, validation, authorization, reconciliation, and acceptance controls.

### 6C.17 Accounting Data Import, Migration, and Onboarding

1. Accounting must support complete historical migration, opening-balance migration, and selective migration for eligible source systems and approved scopes.
2. Migration may include Chart of Accounts, Account Groups, Ledgers, opening balances, Accounting Vouchers, Journal Entries, Ledger balances, bill-wise/sub-ledger references, Cost Centres, Cost Categories, Accounting Period references, narrations, and supporting references to the extent available and valid.
3. Data owned by Product/Item Management, Inventory, Warehouse Management, GST/Tax, Human Resource Management, Payroll Management, Finance/Payment, Sales, Purchase, POS, or another domain must be routed to and accepted by that domain under its own rules. An accounting migration must not silently absorb source records owned by another Module.
4. Every migration must identify the source system/company, source period, extraction method, source version where known, selected scope, accountable Business Identity/Legal Identity, mapping version, and import batch.
5. Migration must use controlled stages: source inspection, mapping, staging, validation, duplicate detection, exception handling, preview, reconciliation, authorized approval, acceptance, and post-migration verification.
6. Imported records must preserve available source identifiers, source voucher numbers, original dates, narrations, references, and provenance. Missing or transformed information must be identified; the system must not invent source facts.
7. A migration must not silently omit invalid records, coerce foreign-currency values into INR, overwrite existing Books, or create duplicate transactions. Unsupported or ambiguous records must be reported as exceptions for authorized resolution.
8. Imported historical records may become authoritative posted records only after the required validation, mapping, reconciliation, and approval. Records that cannot be verified must remain staged, rejected, or otherwise clearly unaccepted; they must not be presented as valid posted accounting.
9. Re-running a migration or retrying an import must be idempotent within the defined source and batch identity. Duplicate protection and correction handling must preserve traceability.
10. The migration record must retain the source, batch, results, record counts, exceptions, reconciliation outcomes, acceptance decision, and responsible authorization.
11. Migration options must be configurable by source, Book, historical period, record type, and permitted Business Context. The selected mode must not weaken Accounting Integrity or legal retention requirements.

### 6C.18 Accounting Data Export, CA/Auditor Support, and Records Access

1. Accounting must support scoped exports of Chart of Accounts, Groups, Ledgers, Vouchers, Journal Entries, Ledger Transactions, Trial Balance, Financial Statements, sub-ledger/outstanding detail, Cost Centre/Category reports, Reconciliation results, and other authorized accounting records.
2. Exports may be provided in structured machine-readable formats and human-readable reports suitable for the recipient and target system. Format availability and import compatibility must be stated accurately; a report format is not automatically a round-trip data-exchange format.
3. An Audit Data Package may contain the selected Accounting records and necessary supporting references from other Business Modules, but cross-domain information must be obtained through the source Module's authorized contract and Permissions.
4. CA/Auditor access must be explicitly granted, purpose-limited, scoped to the relevant Business Identity, Legal Identity, Books, periods, records, and functions, and read-only by default. A CA/Auditor designation does not independently grant access to any business or its data.
5. An Audit Data Package must carry enough provenance and scope information to identify the Book, accountable identity, period, extraction time, selected record categories, record counts, exceptions, and reconciliation status. Its creation must be auditable.
6. Exporting data does not transfer ownership of the original records or authorize the recipient to alter CBM Books. An export is a copy or representation for its authorized purpose.
7. An Audit Data Package is not an audit opinion, certification, statutory return, or assurance that all external compliance requirements have been met.
8. Accounting record retention, retrieval, archival, and legal hold must follow the applicable law for the relevant entity and record type, together with authorized platform policy. No single retention duration is presumed to apply to every Business Entity or record category.

### 6C.19 Fixed Asset Accounting and Management Accounting

1. Accounting includes the financial recognition and accounting records for fixed assets: acquisition cost, capitalization, depreciation, impairment, revaluation where applicable, disposal, and related disclosures and ledger treatment.
2. Physical asset custody, location, maintenance, repair, assignment, and operational lifecycle remain separate from Fixed Asset Accounting and belong to the appropriate operational domain/capability.
3. Accounting may include Management Accounting capabilities for internal budgets, forecasts, cost/profit analysis, and actual-versus-budget reporting.
4. Budgets and forecasts are planning records and do not alter the General Ledger merely by being created or approved. Actual financial transactions are posted through the normal Accounting Voucher and Posting controls.
5. Asset and Budget capabilities must be configurable and extensible, but their rules must remain consistent with applicable Accounting Frameworks and other domain ownership boundaries.

### 6C.20 Configurability, Extensibility, Customization, and Applicability

1. Chart of Accounts, authorized Account Groups, Ledgers, Voucher Types, numbering series, Narration rules, approval conditions, Cost Centres, Cost Categories, Accounting Dimensions, allocation templates, reporting layouts, and migration mappings must be configurable within governed limits.
2. Standard templates may be provided for common Indian business contexts and applicable Accounting Frameworks. A template is a governed starting point, not a substitute for evaluating the actual entity and purpose.
3. Authorized businesses may customize permitted accounting workflows, analytical structures, and reports without creating uncontrolled product forks or changing another business's configuration.
4. Extensions may add new Accounting Dimensions, Voucher Types, source mappings, reports, and external-system adapters through explicit contracts and versioning.
5. Applicability must identify the eligible Legal Identities, Books, business contexts, transactions, periods, users, and reporting purposes. A capability must not be enabled universally merely because it exists.
6. Main Portal may establish protected accounting baselines, minimum data integrity rules, approved templates, supported frameworks, and mandatory controls. Lower-level settings may vary only where explicitly permitted and authorized.
7. Business Data Isolation, Tenant Data Isolation, legal ownership, Double-entry integrity of posted records, auditability, and protected system controls are non-overridable.

### 6C.21 Relationship with Platform-wide Services

Accounting consumes shared Platform-wide capabilities without transferring its domain ownership. Relevant services include Authorization; Security & Trust; Approval Workflow; Audit & History Infrastructure; Event & Messaging; Integration & API; File & Document Services; and Reporting Infrastructure.

Platform-wide Services may provide common execution, transport, storage, security, workflow, and presentation capabilities. Accounting remains the authoritative owner of its accounting meaning, policies, posting rules, ledgers, and financial calculations. Each service operation must preserve the relevant Service Scope, Business Context, authorization, purpose limitation, and Business/Tenant Data Isolation.

### 6C.22 Applicability across the Customer Business Architecture

1. Accounting Management may be made applicable to eligible Organizations, Distributors, and Shops, subject to the applicable Legal Identity, Accounting Context, Book, and accounting requirements.
2. Accounting is not automatically mandatory or independently instantiated at every structural level. A Book must be created only where a valid accounting purpose and accountable legal/business context exist.
3. Where multiple Shops, branches, or operating contexts belong to the same Legal Identity, they may be represented within the same statutory Books using appropriate Accounting Dimensions. A separate Professional Identity alone does not require a separate statutory Book.
4. Where an Organization, Distributor, or Shop has a distinct Legal Identity, its statutory Books must remain separate. Shared Owner IDs, Parent Organization, common service provider, Distributor–Shop Mapping, common branding, or common Users do not merge Accounting Books.
5. Accounting access and User assignment must follow explicit User–Business Association, User Scope Mapping, Permissions, Delegated Authority, and applicable approval controls. Accounting Book visibility must not be inferred from hierarchy, ownership, Mapping, or a person's CA/Accountant title.
6. Authorized consolidated reports may be produced across eligible Books where applicable rules permit, but consolidation does not merge source Books or relax their data boundaries.

### 6C.23 Accounting Domain History, Audit Trail, and Retention

Accounting must maintain its Domain History for material accounting-domain changes and lifecycle events, including configuration, Voucher, posting, correction, reversal, period, reconciliation, migration, and export events as applicable. The comprehensive Audit Trail remains a separate Platform-wide record with its own access and retention rules under Section 17A.

Accounting History must not be treated as a substitute for the Audit Trail, operational logs, event messages, or external export records. Retention and archival must follow the applicable statutory requirements for the accountable entity and record category. Where a legal hold or investigation applies, normal archival or deletion must not remove protected records.

## 6D. Business Module Boundaries, Shared Models, and Connectivity Architecture

### 6D.1 Purpose and architectural classification

CBM must distinguish explicitly between the following architectural classes:

1. **Business Modules:** Independent domain owners responsible for their own business purpose, rules, operations, and authoritative records.
2. **Shared Domain Models and Shared Model Contracts:** Reusable canonical concepts and contracts that let Modules exchange and reference data without merging ownership.
3. **Internal Platform Capabilities:** Reusable services available within CBM under Section 6A, including Identity, User/Role/Permission support, workflow, configuration, security, messaging, audit, and reporting infrastructure.
4. **Application Client Channels:** Web, desktop, mobile, handheld, and other authorized applications through which people or systems interact with CBM.
5. **External Connectivity Capabilities:** Governed adapters and contracts connecting CBM to external applications, systems, devices, service providers, and independently operated portals.
6. **Cross-cutting Functions:** Requirements such as security, auditability, privacy, data isolation, configurability, extensibility, and observability that affect multiple architectural classes but do not take ownership of domain data merely by being cross-cutting.

These classes are not interchangeable. A Shared Model is not a Business Module; an Internal Platform Capability is not a Business Module; an Application Client is not a Business Module; and an External Connector is not a domain owner.

### 6D.2 Business Module independence and separation rules

1. Each Business Module must own one clearly defined business responsibility/domain, its domain rules and policies, its authoritative records, its operational lifecycle, and the contracts through which it interacts with other Modules.
2. A Business Module must not absorb another Module's responsibility merely because the same screen, transaction, workflow, report, operator, or customer action touches both domains.
3. A user-facing workflow may coordinate multiple Modules, but each Module remains authoritative for its own state transition and record.
4. A Module may consume a source record, reference, event, calculation, or result from another Module only through an explicit contract. It must not silently copy the source record and treat the copy as a competing source of truth.
5. Functional size, menu placement, product packaging, Subscription Plan, application deployment, or database grouping does not determine whether something is a Business Module.
6. Shared functionality must be implemented once at the correct shared layer where appropriate; domain-specific validation and business meaning must remain with the relevant Module.
7. An optional capability may be enabled for one eligible business context and disabled for another, provided that this does not weaken Stable Core, legal requirements, data integrity, or cross-domain contracts.

### 6D.2.1 Domain decomposition and composition of related concepts

A common real-world subject must not be treated as one indivisible data model merely because many kinds of information refer to it. CBM must separate records and capabilities according to their distinct business responsibility, meaning, authoritative owner, lifecycle, applicability, permission/sensitivity, validation, history, and independent configuration needs. The subject's stable identity is used to connect those independently governed aspects through explicit, typed, versioned references and contracts.

For example, Product/Item Management owns Product Definitions, Variants, product attributes and specifications, and catalogue lifecycle. A Product Variant may be referenced by separate Product-declared Price Records, Sales-owned Selling Price Records and Conditions, Purchase-owned supplier-price records, Inventory-owned cost/valuation records, and GST/Tax-owned HSN/SAC Classification Assignments and GST Rate Applicability Rules. The references compose a complete commercial view of the same Variant; they do not convert those responsibilities into Product fields or transfer their authoritative ownership to Product/Item Management.

This principle applies beyond Product/Item Management wherever one business subject has separable aspects—for example, an Employee and its employment, access-account, payroll, attendance, and payment facts; an Asset and its identity, custody, maintenance, depreciation, and accounting facts; or a Customer/Supplier and its identity, relationship, credit, transaction, tax, and settlement facts. These are illustrations of the decomposition rule, not new approvals of the detailed architecture of those domains.

This separation must not become indiscriminate fragmentation. Cohesive facts with the same business meaning, owner, lifecycle, and policy may remain within one model. A separate model or Module is justified by a real responsibility boundary, not merely by the existence of another field. A Shared Domain Model or contract may provide common references and reusable behaviour without creating a universal master, duplicate source of truth, shared-write authority, or new Business Module by implication.

A composed workflow, screen, report, or calculation may assemble data from several owners only through explicit contracts. It must preserve each owner's validation, permissions, effective/version history, provenance, correction lifecycle, and Business/Tenant Data Isolation. The detailed responsibility split for Product, Price, and GST/Tax is governed by Sections 6F, 6G, and 6H.

### 6D.3 Confirmed module boundaries and distinct ownership areas

The following fourteen independent Business Modules and boundaries are established in the current architecture baseline. This is the definitive classification; the detailed responsibilities and transaction boundaries are governed by Section 6I and the referenced domain sections.

| Business Module | Authoritative responsibility | Explicit boundary |
|---|---|---|
| **Accounting Management** | Accounting Contexts/Books, Accounting Policies, Chart of Accounts, Ledgers, Vouchers, Journals, Posting, accounting balances, Reconciliation, and financial statements | Does not own source business transactions, operational payment execution, stock records, tax classifications, or HR/Payroll source records. See Section 6C. |
| **Product/Item Management** | Product Definitions, Product Variants, catalogue attributes/classifications, typed product identifiers, Trade Packaging Configurations, Bundle/Kit definitions, and applicable declared product/packaging prices | Does not own Sales/Purchase transactions, Sales/Purchase pricing records, Inventory Ledger/valuation, batch/serial stock lifecycle, or Warehouse physical execution. See Sections 6F–6G. |
| **Inventory Management** | Stock ledger, quantities, stock states, reservations/availability, stock movements, stock reconciliation, and inventory valuation | Does not own core Product/Item definitions or physical Warehouse execution/layout. See Section 6E. |
| **Warehouse Management** | Warehouse facilities/layout, storage hierarchy, physical stock placement, Handling Units, and warehouse work execution | Does not own Product/Item definitions, stock valuation, or the authoritative Inventory Ledger. See Section 6E. |
| **Human Resource Management** | Employee/personnel records in the employment context, recruitment, onboarding, employment relationships, attendance, leave, workforce administration, and HR policies | Does not own User Account credentials/roles/permissions or Payroll calculations and authoritative Payroll results. |
| **Payroll Management** | Payroll rules, payroll periods/runs, earnings/deductions, payslips, payroll calculations, statutory payroll obligations, and authoritative payroll records | Consumes authorized employee/employment/attendance facts from HR; it does not own employment records or payment execution. |
| **Sales Management** | Customer-facing commercial sales lifecycle, quotations/offers, sales agreements/terms, sales orders, original Sales Invoices, returns, and sales-side pricing conditions | Does not own Product identity, tax determination, stock/warehouse execution, payment settlement, or accounting postings. See Section 6I.2. |
| **Purchase & Procurement Management** | Requisitions, sourcing, RFQs, supplier selection, quotations/agreements, Purchase Orders, original Purchase Invoices, returns, and commercial purchase matching | Does not own authoritative stock, physical warehouse execution, tax determination, payment settlement, or accounting postings. See Section 6I.3. |
| **Point of Sale (POS) Operations** | POS terminals/workstations, operator context, sessions/shifts, tills/cash drawers, checkout execution traces, and POS operational reconciliation | Does not become the authoritative Sales transaction/Invoice, payment-settlement, or Accounting master. See Section 6I.4. |
| **GST/Tax Management** | GST taxpayer/registration tax-domain records, HSN/SAC Registry and assignments, GST Rate Applicability Rules, tax determination, and GST statutory/compliance records | Does not own original Sales/Purchase transactions, Product identity, payment execution, Payroll calculation, or Accounting Books/postings. See Sections 6H and 6I.5. |
| **Finance & Payment Management** | Operational cash/bank functions, payment initiation/execution, collections/disbursements, settlement, refunds/reversals, provider evidence, and payment reconciliation | Does not own the General Ledger, authoritative accounting balances, or accounting postings. See Section 6I.6. |
| **Asset Management** | Operational asset identity, custody, location, assignment, usage/status, inspection, maintenance, repair, retirement, and physical disposal | Does not own capitalization, depreciation, impairment, revaluation, or disposal accounting. See Section 6I.7. |
| **Business Planning & Budget Management** | Business plans, budgets, targets, forecasts, scenarios, allocations, approved planning baselines, revisions, and planning analysis | Does not own posted financial actuals or operational source transactions and does not automatically authorize commitments. See Section 6I.8. |
| **Counterparty Management** | Business/Tenant-scoped Party identity, Customer/Supplier role profiles, contact/address records, relationship lifecycle, duplicate detection, and controlled merge/split | Is not a User Account, Employee, Business Identity, Legal Identity, or universal cross-tenant master. See Section 6I.9. |

The fourteen-Module Catalogue is a responsibility classification, not a universal enablement list. Each Module retains its own authoritative records, lifecycle, History, applicability, and authorization boundary. A Module may consume another Module's records only through authorized contracts; a shared screen, workflow, report, or event does not transfer ownership or grant unrestricted write access. The catalogue does not create additional Modules by implication.

### 6D.4 Internal Platform Capabilities

Internal Platform Capabilities are provided through the approved Platform-wide Services Architecture in Section 6A. They are shared execution and governance capabilities, not duplicate business modules. Their authoritative business concepts remain in the sections and domains that own them.

| Internal capability | Shared capability responsibility | Authoritative source / boundary |
|---|---|---|
| User and identity lifecycle support | Provisioning coordination, account status operations, authentication identity support | User Account and User–Business Association concepts remain governed by Section 15 and applicable identity records. |
| Authentication | Password, OTP, MFA, authenticator and session verification | Identity & Authentication Services (A1); authentication does not grant business permissions. |
| User–Business Association and User Scope support | Association and assigned-context checks | Authoritative association and User Scope rules remain in Sections 7–9 and 15. |
| Role Management | Role definitions, assignment and role lifecycle support | Roles and their scope/meaning remain governed by Section 9; a job title or HR position is not automatically a Role. |
| Permission Management | Permission definitions, assignment and evaluation support | Permissions and action authorization remain governed by Section 9 and A2; a Role does not automatically grant every action. |
| Delegated Authority | Delegation constraints and evaluation | Section 8 remains authoritative. |
| Approval execution | Workflow routing, approval steps, escalation and execution history | Section 11 defines approval requirements and eligible approvers; B3 executes them. |
| Configuration Management | Schema, defaults, inheritance, versions and authorized overrides | Section 10 and A4 remain authoritative. |
| Notification and Communication | Notification decision/routing and approved-channel delivery | B1; it must preserve the absolute no-URL outbound communication rule. |
| Internal Event & Messaging | Event publication, subscriptions, routing, retries and delivery | B5; event delivery is not proof of domain transaction completion. |
| API and Integration infrastructure | Shared interface registry, authentication/authorization enforcement, versioning, throttling and monitoring | B4; domain validation remains with the owning Module. |
| Audit and History infrastructure | Separate Domain History and Audit Trail storage, retrieval and protection | C1 and Section 17A; records remain separate. |
| Search, Reporting and Analytics infrastructure | Shared search, indexing, report rendering, query and export execution | B7; domain calculations and report meaning remain with the relevant Module. |
| File and Document infrastructure | Shared file storage, attachment, retrieval, version and archive capability | B2; business document meaning and ownership remain with the relevant Module. |
| Background processing and scheduling | Shared execution, retry, recurring jobs and workload coordination | C3; the job retains its original authority and business context. |
| Platform and Service Provider administration | Shared platform operation and delegated service-provider administration | C4; Main Portal retains supreme governance. |

User Management, Role Management, Permission Management, and related administration must not be implemented as separate duplicate Business Modules merely because they have substantial interfaces or workflows. Their internal platform responsibilities must be reused consistently by Accounting, Inventory, Warehouse, HR, Payroll, POS, and all other eligible Modules.

A User Account is not an Employee record. An Employee is not automatically a User. A Role is not a job title. A Permission is not a Subscription Entitlement. Authentication is not Authorization. Approval is not Permission. These distinctions are mandatory.

### 6D.5 Shared Domain Models and shared contracts

A Shared Domain Model must define common meaning and references without becoming a universal source of truth. The following Shared Models are established as the cross-domain model families CBM must support:

| Shared Model / Contract | Canonical meaning | Authoritative owner and consumer boundary |
|---|---|---|
| **Business Identity Reference Model** | Legal Identity, Professional Identity and verified Business Identity references | Business Identity domain remains authoritative under Section 3.7. A reference does not transfer identity ownership. |
| **User / Actor Reference Model** | User Account, System Principal, authenticated actor and permitted business-context references | User Account/identity records remain governed by Sections 9 and 15; employee/personnel records remain HR-owned. |
| **Product/Item Reference Model** | Product Definitions, Product Variants, their attributes and specifications, product tax-classification references/assignment references, optional Family/Group relationships, typed identifiers, Data Carrier references, Trade Packaging Configurations, Product Bundle/Kit compositions, declared MRP/RRP and legally required product/package price declarations, and applicable version/provenance references | Product/Item Management owns catalogue definitions, product/variant/trade-unit identity, product attributes/specifications, and its declared product/package price records. It may retain a reference to the applicable HSN/SAC Classification Assignment but does not own the HSN/SAC Code Registry, GST Rate Applicability Rules, or GST Tax Determination. Inventory, Warehouse, Sales, Purchase, POS, Asset Management, and other consumers retain their own stock, handling, transaction, commercial-price, cost, asset, and operational facts under Sections 6E–6H. |
| **Price Reference and Price Determination Contract** | Typed Price Records, Price Lists, Price Conditions, tax-basis declarations, effective periods, selected-price references, and Price Snapshot references | Price Records remain authoritative in their owning domains: Product/Item Management for declared product/package prices; Sales for selling-price lists and sales conditions; Purchase for supplier quotation/agreement/purchase reference prices; Inventory for its cost/valuation references. A shared contract or reusable determination capability does not create a universal price owner or a separate Pricing Business Module. GST/Tax owns tax determination under Section 6H. |
| **HSN/SAC Classification and GST Rate Applicability Contract** | HSN/SAC Code references, effective-dated classification assignments, GST Rate Applicability Rule references and versions, statutory-source references, and Tax Determination/Snapshot references | GST/Tax owns the HSN/SAC Code Registry, classification-assignment rules, GST Rate Applicability Rules, and GST Tax Determination. Product/Item Management supplies the eligible Product Variant reference for goods; the eligible Service Offering/supply domain supplies the service subject for SAC. Other domains consume tax results through this contract without taking ownership of tax codes or rules. |
| **Unit of Measure and Packaging Reference Model** | Shared units, permitted conversions, product-specific Trade Packaging Configuration references, and quantity presentation | Shared Indian reference units are governed by Section 6B/B6; Product/Item Management owns canonical product-specific packaging definitions, while Inventory, Sales, Purchase, and Warehouse own their operational applicability, precision, quantity, and transaction/stock rules. |
| **Business Location Reference Model** | Business address, site, branch, operating location and facility references | Business Identity/Professional Identity and relevant domain owners retain their source facts. Warehouse-specific locations are governed by Warehouse Management. |
| **Inventory Position Model** | Product, stock quantity, stock state, batch/serial/expiry, ownership/custody references and authorized location references | Inventory owns authoritative stock quantities, states and valuation; Warehouse owns physical placement and execution facts through a shared contract. |
| **Warehouse Location and Handling Contract** | Facility, zone, rack, shelf, bin, handling unit, workstation and warehouse-task references | Warehouse Management owns warehouse topology and task records; Inventory consumes the location/placement references it needs for stock integrity. |
| **Source Transaction and Document Reference Model** | Source Module identity, record identifier, document reference, version/status and provenance | The originating Module owns the original record; consuming Modules retain references and their own consequences. |
| **Accounting Posting Contract** | Source accounting facts, posting references, Ledger mappings, posting status and correction linkage | Accounting owns the Voucher, Journal, Ledger Posting and resulting financial records; the source Module owns the original transaction. |
| **Workforce / Employment Reference Model** | Employee and employment-context references required by other domains | HR owns the Employee and employment records; Payroll and authorized operational Modules consume permitted facts. |
| **External Identifier and Provenance Model** | Source/target system identifiers, mapping, import batch, source version and lineage | The relevant integration/receiving Module governs mappings within its contract; external identifiers do not become CBM identity or authority. |
| **Event and Change Reference Model** | Event identity, producer, type, context, version, timestamp and correlation/provenance references | B5 provides shared event infrastructure; the producer Module owns the event's business meaning and authoritative state. |
| **File and Supporting Document Reference Model** | File identity, metadata, version, business-document relationship and controlled access | B2 provides file infrastructure; the originating Business Module owns the document's business meaning and access purpose. |

Shared Models are not automatically shared databases, universal master-data screens, or common write permissions. Each model must specify its authoritative owner, allowed consumers, permitted read/write operations, effective dates, versioning, provenance, data classification, and authorization requirements.

A shared reference must not merge separate Legal Identities, Owners, User Accounts, Customer and Supplier roles, Employee and User records, Business Entities, Accounting Books, Inventory owners, Warehouses, or Tenant boundaries.

### 6D.6 Important non-equivalences that must remain explicit

| Concept A | Concept B | Required distinction |
|---|---|---|
| Inventory Management | Warehouse Management | Stock records/quantity/valuation versus physical storage topology and warehouse execution. |
| Product/Item Master | Inventory Position | Product definition versus the stock held for a product at a context/location/status. |
| Warehouse Facility | Business Location | A facility used for warehouse operations versus a general business address/operating location. |
| Warehouse Bin | Inventory Storage Summary | Exact physical slot versus the stock ledger/position represented at a given level. |
| Warehouse Counter | POS Counter | Warehouse work point versus retail checkout/POS operating point. |
| Employee | User Account | Employment identity/relationship versus system login/account identity. |
| HR Role/Job Title | CBM Role | Employment position versus permission-bearing system role. |
| Role | Permission | Grouping/assignment of capabilities versus authorization to perform a specific action. |
| Business Module | Shared Capability | Domain owner versus reusable platform function. |
| Shared Model | Shared Data Ownership | Common contract/reference versus common authoritative ownership. |
| Application Client | Business Module | Access channel/presentation app versus business responsibility owner. |
| External Connector | External Business Module | Integration adapter versus the domain system or Module that owns business meaning. |
| Accounting Voucher | Source Sales/Purchase/Payment Document | Accounting record versus originating commercial/payment document. |
| External Storage-Location Record | CBM Warehouse Facility | Source location description versus a governed CBM facility and physical-execution model; mapping requires sufficient source meaning and must not be presumed. |
| Portal Federation | Business Hierarchy | Cross-system data interaction versus Organization/Distributor/Shop structural placement. |
| Product Definition | Product Variant | Common product identity and shared characteristics versus a specific configured/operational form. |
| Product Variant | Product/Item Instance | A repeatable type/configuration that may have many occurrences versus one individual occurrence. |
| Product Family / Product Group | Product Identity | Optional grouping/classification versus the identity of a Product Definition or Variant. |
| Product Tax Classification | Product Taxonomy | A regulated HSN/SAC assignment used for tax purposes versus a business/product grouping or reporting taxonomy. |
| HSN/SAC Code | GST Rate Applicability Rule | The classification key/reference versus a time- and condition-dependent statutory rate rule; neither is a permanent substitute for the other. |
| Product Variant | GST Tax Determination | The goods subject reference versus the tax-domain decision for a particular supply/transaction context. |
| Product Code / SKU | CBM Internal Record ID | A business/catalogue identifier with an explicit scope and lifecycle versus an immutable CBM record identity. |
| Identifier | Barcode / Data Carrier | The typed value that identifies a subject versus the machine-readable representation that carries one or more values. |
| GTIN | CBM Product Code / SKU | A GS1 trade-item identification key allocated under GS1 rules versus a CBM/business-defined identifier with its own namespace and scope. |
| Batch / Lot | Serial Number | A group of units sharing a traceability reference versus an identifier for an individual unit or component. |
| Product Packaging Configuration | Warehouse Handling Unit | A defined trade/packaging unit versus a movable logistics carrier/grouping used in physical operations. |
| Product Variant | Product Price | The identity of the priced item versus a price determined and owned by an applicable commercial/pricing domain and context. |
| Product Attribute | Variant Dimension | A descriptive property versus a property explicitly selected to distinguish separately managed variants. |

### 6D.7 External Connectivity and Application Client Architecture

External Connectivity must be treated as a distinct architecture model under the existing Integration & API (B4) and Event & Messaging (B5) Categories. It must not be mixed into the internal Accounting, Inventory, HR, Warehouse, or other Business Module models.

The supported connectivity classes are:

**A. External Business-System Integration**

Examples include optional adapters for external Accounting Systems (including Tally where specifically required), ERP/CRM applications, payment and banking providers, tax/e-invoice services where legally and technically supported, partner systems, and other approved external business applications. These examples do not make any named vendor or connector mandatory.

Each integration must have a registered system identity, purpose, eligible business context, data scope, authentication/trust method, authorization policy, contract/version, mapping, auditability, failure/retry handling, and revocation lifecycle.

**B. Application Client Connectivity**

CBM must be capable of serving authorized clients through the Web Portal and, where developed, first-party Desktop, Mobile, Warehouse Handheld, and other client applications. Authorized third-party clients may also connect through governed interfaces.

An Application Client is not permitted to connect directly to authoritative storage or bypass CBM domain contracts. It must use approved interfaces and receive only the data/actions permitted to its authenticated User or System Principal and assigned business context.

First-party Desktop and Mobile applications remain CBM clients even though they execute outside the central web application. Third-party applications are separately registered External Clients and must receive only explicit, revocable, least-privilege access. Native application authentication must follow current applicable security best practices; external system connectivity or a successful login does not itself grant business access.

**C. Device and Peripheral Connectivity**

Where required, CBM may support controlled connectivity to scanners, barcode/RFID readers, printers, weighing devices, label systems, payment terminals, and other operational devices. Device identity, registration, trust, authorization, data origin, and action scope must be explicit. A device connection does not become a Business Module or authority grant.

**D. Portal-to-Portal Interoperability**

CBM must preserve an extensible model for data exchange and synchronization with separately operated portals, including a future portal developed by the project owner. Such interoperability must not be confused with Main Portal/Sub-Portal governance or Customer Business Structure.

The Portal Interoperability Model must support, where authorized:

- Direct API-based exchange or federated query.
- Event-based change notification and selected event sharing.
- Scheduled or incremental synchronization.
- Full-snapshot import/export where justified.
- A controlled central aggregation or reporting layer for selected, explicitly permitted data.
- External identity mapping, source provenance, lineage, reconciliation, versioning, conflict handling, and revocation.

A central aggregation or data-centralization facility must not silently replace the source portal's authoritative records. Every exchanged field or dataset must identify its source of truth, permitted purpose, allowed recipient, scope, retention, and update authority. A receiving portal must not overwrite a source portal's authoritative record unless an explicit bilateral contract grants that exact operation.

Two-way synchronization must not be assumed. Where it is authorized, each field/entity must have a defined write authority, conflict-resolution method, idempotency rule, version/effective-time handling, deletion/retention treatment, and recovery process. Synchronization must not merge Tenants, Business Identities, Owners, Users, Accounting Books, stock ownership, or Portal authority.

### 6D.8 Integration, API, and event contracts

1. Synchronous APIs, asynchronous events, file exchanges, and portal synchronization are different interaction patterns and must have explicit contracts.
2. API contracts must define operations, data schemas, versions, authentication, authorization, error behaviour, pagination/limits where relevant, and compatibility expectations.
3. Event contracts must define event identity, producer, business meaning, permitted consumers, context, version, delivery semantics, duplicate/retry handling, and recovery behaviour.
4. A common event envelope may be used to improve portability, but it must not replace the source Module's domain-specific event schema or validation.
5. External adapters must translate external representations into canonical CBM domain contracts. They must not make an external vendor's object model the internal authoritative model.
6. Source identifiers, correlation identifiers, external identifiers, mapping versions, and migration/synchronization batch references must be preserved where applicable.
7. Integration credentials, tokens, subscriptions, webhooks, device identifiers, and API keys do not grant unrestricted data or action rights.
8. Each operation must enforce the relevant Business Context, Tenant boundary, User/System Principal, Purpose, Permission, Delegated Authority, approval, data classification, and Main Portal governance rules.
9. Shared interface standards may be adopted where appropriate, including OpenAPI for HTTP API descriptions, AsyncAPI for message/event interface descriptions, CloudEvents for interoperable event metadata, and relevant GS1 visibility standards for supply-chain use cases. Their adoption does not itself approve a particular vendor, deployment topology, protocol, or implementation.
10. External Connectivity must be extensible by adapters and contracts rather than requiring every Business Module to implement each vendor-specific protocol independently.

### 6D.9 Internal versus external capability boundary

1. **Internal capabilities** are those provided within the CBM Platform-wide Services Architecture and reused by its Business Modules and portals. Examples include User/Identity lifecycle support, Authentication, Role/Permission evaluation, Configuration, Approval, Notification, Audit/History, Event & Messaging, Search/Reporting infrastructure, and Background Processing.
2. **External capabilities** are those enabling CBM to interact with systems, devices, client applications, services, or separately operated portals outside the relevant internal service boundary.
3. A capability can have both internal and external aspects. For example, Integration & API is an internal shared capability that enables external connectivity; a system-specific adapter, when required and supported, is a connector that uses that shared infrastructure.
4. An Application Client is a consumer of internal capabilities and domain contracts; it is not itself an external business integration merely because it runs on a desktop or mobile device.
5. Each internal or external capability must identify its provider, consumers, source-of-truth owner, authorized context, data scope, contract, lifecycle, security requirements, and failure/recovery rules.
6. No internal or external capability may create a second authoritative User, Role, Permission, Product, Stock, Ledger, Employee, Warehouse, or Business Identity record in conflict with its domain owner.

### 6D.10 Governance and design obligations

Every Business Module, Shared Domain Model, Internal Platform Capability, Application Client, and External Connectivity Capability must be reviewed for:

- One clear responsibility and authoritative owner.
- Explicit inclusions and exclusions.
- Source-of-truth and field-level ownership.
- Relationships and contracts with other Modules.
- Shared-model reuse without duplicate ownership.
- Applicable Business Entity, Tenant, Service Provider, Legal Identity, User, and operational scope.
- Authorization, approval, audit, retention, and data-protection requirements.
- Configurability, customization, controlled overrides, and versioning.
- Extensibility and backward compatibility.
- Failure, retry, reconciliation, migration, and recovery behaviour where relevant.
- Security and strict Business/Tenant Data Isolation.

A capability must remain fixed where integrity, legal compliance, or Stable Core requires it; configurable where legitimate business variation is needed; and extensible where new integrations, clients, models, or domain capabilities may be added. Flexibility never grants permission to violate ownership, authorization, or isolation rules.


### 6D.11 External-System Onboarding, Data Portability, and Customer Offboarding

1. **Independent CBM architecture:** CBM's Business Modules, Shared Domain Models, Internal Platform Capabilities, business rules, and authoritative records must be designed from CBM's approved business requirements. An external system is a data source or destination, not an architectural template. CBM must not copy another product's overall structure, optional modules, terminology, workflows, customizations, accounting conventions, inventory granularity, or warehouse model merely to simplify an adapter.
2. **Purpose of data exchange:** External-system onboarding/import exists to obtain the selected, relevant data needed to establish or continue the customer's business operations in CBM. Customer offboarding/export exists to provide the selected CBM data needed to continue those operations in another system or environment. Data transfer is not a requirement to reproduce the source or destination product.
3. **Domain ownership:** Every imported or exported record must be classified by its business meaning and routed through the relevant CBM Business Module and its canonical contract. The receiving Module owns validation, acceptance, lifecycle, and authoritative CBM records. An adapter must not create a competing source of truth or bypass domain rules.
4. **Selective and evidence-based scope:** The exchange scope must be agreed according to business-continuity needs, the customer's authorized purpose, the source/destination's documented capabilities, data availability, quality, granularity, and applicable retention/legal constraints. Neither complete source-system replication nor transfer of every historical or technical record is presumed necessary.
5. **No assumed source capability:** CBM must not assume that a source customer enabled or used Accounting, Inventory, Warehouse Management, serial/batch tracking, or any other module. Absence of a source record or feature must not be interpreted as proof that the corresponding business activity never occurred, that stock is zero, or that a CBM capability is unnecessary.
6. **Missing or insufficient data:** Import assessment must distinguish available and mappable data from missing, unsupported, ambiguous, inconsistent, invalid, or insufficiently detailed data. These gaps must be reported clearly. Where required, an authorized customer User must be able to establish the missing master data, opening information, operational details, or supporting references directly in CBM using the receiving Module's normal validation and authorization controls. CBM must not fabricate source facts or silently invent defaults that change business meaning.
7. **Granularity and traceability:** The imported level of detail must not exceed what the source actually contains. For example, an aggregate quantity of ten mobile phones does not provide ten individual device identities or IMEI values. CBM may retain the accepted aggregate quantity where its Inventory rules permit it; where serialized tracking or IMEI capture is required for a subsequent process, the missing unit-level records must be entered and validated before that process can rely on them. No serial number, IMEI, batch, expiry, location, or other source fact may be fabricated.
8. **Different domain rules and accounting methods:** CBM's approved domain rules remain authoritative after import. Source records maintained under a different accounting method, classification, workflow, or level of detail must be mapped only where reliable facts and an approved, traceable transformation are sufficient. For example, source single-entry records do not require CBM to abandon its approved double-entry-by-default Accounting model. If the source facts are insufficient for a reliable conversion, the gap must be staged and resolved through an authorized process rather than disguised as a completed conversion.
9. **No migration of source customizations by default:** Customer-specific source-system fields, scripts, screens, workflows, reports, numbering schemes, configuration, and custom business rules are not automatically imported or recreated. Their business-relevant data may be mapped to CBM concepts or captured through CBM's governed configuration/customization mechanisms only where independently justified, supported, and authorized. The existence of a source customization does not create a CBM requirement.
10. **Inventory and Warehouse applicability:** If the source system did not maintain Inventory, the customer may establish Products/Items and opening stock in CBM through the applicable Product/Item and Inventory processes. If the source contains only aggregate stock or basic storage locations, CBM must not invent serial-level data or advanced warehouse topology. Advanced Warehouse Management remains a separate CBM applicability and adoption decision based on actual business needs, not on the structure of an external system.
11. **Outbound business continuity:** Offboarding export must provide the selected data held in CBM that the customer/business is authorized to receive and needs to continue business, subject to authorization, legal retention, and data-protection rules. Depending on the agreed scope, this may include relevant master data, accepted opening/current balances, transactions, stock and tracking details, counterparty references, document references, and permitted supporting files. The exported content, mappings, record counts, exceptions, and provenance must be identified. No export format or destination compatibility may be promised unless the destination's documented and validated capabilities support it.
12. **Exchange safeguards:** Import and export must preserve source/CBM identifiers, record ownership, effective dates, versions, provenance, and relationships where available. The process must include scope review, mapping, validation, duplicate handling, preview, exception reporting, authorization, acceptance or export confirmation, and reconciliation appropriate to the data. Retries must be duplicate-safe. Ongoing synchronization, bidirectional writes, and conflict resolution require separate explicit contracts; they are never implied by a one-time migration.
13. **Security and lifecycle:** Data exchange must follow Business/Tenant Data Isolation, Purpose limitation, explicit authority, Permission, approval where required, auditability, retention, and revocation rules. Export does not itself authorize deletion of CBM records; archival and deletion remain separately governed.
14. **Separation from Service Provider migration:** Customer Business onboarding/offboarding to or from an external business system is distinct from Sub-Portal/Service Provider migration under Section 16. It must not change Business Identity, ownership, Tenant boundaries, Business Structure, or authority unless a separate approved process expressly governs that change.

## 6E. Inventory Management and Warehouse Management Boundaries

### 6E.1 Independent Module classification

**Inventory Management and Warehouse Management are separate independent Business Modules.** They cooperate through explicit Shared Domain Models and contracts but own different business responsibilities and authoritative records.

Inventory Management owns the authoritative Inventory Ledger, stock quantities, stock states, reservations/availability, inventory movement records, reconciliation, and inventory valuation. Warehouse Management owns warehouse facilities/layout, storage-location hierarchy, physical stock placement, Handling Unit placement, warehouse work/tasks, and physical warehouse execution.

Inventory Management may operate with simple storage-location/godown tracking without advanced Warehouse Management. Advanced WMS is optional where business complexity justifies detailed facility layout, rack/bin control, Handling Units, directed work, and operational execution. It is not mandatory for every Organization, Distributor, or Shop.

### 6E.2 Inventory Management responsibility

Inventory Management owns the authoritative business record of stock within its authorized Inventory Contexts. Its responsibilities include, where applicable:

- Stock ledger, quantities, balances, and valuation.
- Receipts, issues, transfers, returns, and adjustments.
- On-hand, available, reserved, blocked, damaged, quarantine, inspection, consignment, and other governed stock states.
- Batch/lot, serial-number, expiry, condition, and other tracked stock identities.
- Inventory ownership references and custody distinctions.
- Inventory valuation methods and calculations.
- Reservations and availability responses for authorized Sales, Purchase, POS, Warehouse, and other consumers.
- Reconciliation and acceptance of physical-count adjustments.
- Inventory history, traceability, and reporting.

Inventory consumes Product/Item references. It may own inventory-specific attributes, classifications, stock rules, the operational applicability of units and Product/Item-defined packaging configurations, batch/serial/expiry tracking, and stock conditions; it does not thereby own the canonical Product/Item or Trade Packaging definitions.

The Inventory Ledger is authoritative for accepted stock quantities and stock states. A warehouse task, physical count, Sales/Purchase order, POS transaction, or external message is evidence or a source instruction; it is not an authoritative inventory balance change until accepted under Inventory's validation and authorization rules.

### 6E.3 Warehouse Management responsibility

Warehouse Management is responsible for the physical and operational execution of goods within authorized facilities. Its responsibilities include, where applicable:

- Warehouse facilities and operating-model records.
- Facility layout and configurable location hierarchy.
- Storage areas/zones, storage types/sections, aisles, racks, shelves, levels, cabinets, bins, slots, docks, and staging locations.
- Handling Units such as pallets, cartons, totes, and containers.
- Receiving, unloading, inspection, putaway, replenishment, picking, packing, staging, loading, dispatch, returns handling, and physical internal transfers.
- Warehouse task templates, queues, priorities, work allocation, and execution policies.
- Physical stock placement and location-level handling records.
- Warehouse operator/task assignment and device-assisted execution.
- Cycle-count tasks and physical-count evidence.
- Capacity, utilization, location accuracy, task performance, and operational reports.

Warehouse Management owns the physical layout, warehouse location identity/hierarchy, Handling Unit position, Warehouse Work Task records, and physical execution evidence. It does not own core Product/Item definitions, the enterprise Inventory Ledger, stock valuation, Sales/Purchase Orders, Customer/Supplier relationships, payment execution, or Accounting records.

### 6E.4 Inventory versus Warehouse: authoritative boundary

1. **Inventory owns what stock exists and its accountable quantity/status.** It is authoritative for stock balances, stock movements, status, reservations/availability, inventory ownership references, and valuation.
2. **Warehouse owns where and how goods are physically handled.** It is authoritative for facility topology, storage positions, physical placement, Handling Unit position, work tasks, and execution evidence.
3. A location-level stock view may be derived from or reconciled against the Inventory Position Model, but Warehouse physical-placement records must not become a competing Inventory Ledger.
4. Where advanced Warehouse Management is enabled, Warehouse owns the Storage Location hierarchy and physical placement. Inventory owns quantity/status attributed to the relevant Warehouse Location reference.
5. Where advanced Warehouse Management is not enabled, Inventory may maintain simple Storage Location/Godown references and stock-by-location balances. This does not imply full WMS, bins, racks, Handling Units, or directed warehouse tasks.
6. A physical movement between bins/locations and an Inventory Stock Movement are related but distinct records. Warehouse records requested/actual physical work; Inventory records the authorized stock quantity/status/location transition.
7. A Warehouse task request is not proof of physical completion. Warehouse completion is execution evidence; Inventory applies the stock-ledger transition only after relevant Inventory validation, reconciliation, and authorization.
8. A physical count or discrepancy recorded by Warehouse is evidence/input for Inventory reconciliation. It does not independently authorize a stock adjustment.
9. Shared Inventory Position and Warehouse Location/Handling contracts must reconcile physical placement views with authoritative stock balances without duplicate stock ownership, quantities, or ledgers.
10. Both Modules must preserve source references, timestamps, actors/devices, quantities/units, stock owner/custodian distinctions, batch/serial/expiry data, source/destination locations, task/confirmation status, and exception history where applicable.

### 6E.5 Product/Item relationship

1. Product/Item Management, Inventory Management, and Warehouse Management are separate independent Business Modules with distinct authoritative responsibilities.
2. Product/Item Management owns Product Definitions, Product Variants, applicable product/variant attributes and classifications, product-level and variant-level catalogue identifiers, and Trade Packaging/Bundle definitions as specified in Section 6F.
3. Inventory owns stock-specific facts for a Product Variant or applicable trade unit, including quantities, status, valuation, reservations, batch/lot records, serial/instance traceability, expiry, and inventory-specific attributes and tracking operations. Product/Item Management may define product-side tracking characteristics or minimum requirements, but it does not own the Inventory Ledger or operational stock-traceability lifecycle.
4. Warehouse owns physical handling/storage constraints, warehouse placement, Handling Units, and operational handling rules associated with referenced products, variants, trade packages, and stock.
5. A Product/Item reference does not imply that the item is stocked, available, owned by a particular business, saleable, purchasable, or physically located in a particular Warehouse.
6. Product/Item responsibility must not be folded into Inventory or Warehouse merely because each needs product information. The independent module boundary and its consumer contracts are defined in Sections 6D and 6F.

### 6E.6 Warehouse facility: internal/external, operator, property, ownership, and custody

"Internal Warehouse" and "External Warehouse" must not be represented by one ambiguous flag. A Warehouse Facility relationship must separately identify, where applicable:

1. **Operational control/operator:** The business's own operation, an authorized third-party logistics/warehouse operator, a partner, supplier/customer, public warehouse provider, or another identified party.
2. **Physical/geographic relationship:** On the business's operating premises, on another site, off-site, at supplier/customer premises, or at a third-party logistics facility.
3. **Property/occupancy relationship:** Owned, leased, licensed, shared, public, partner-provided, or another authorized arrangement.
4. **Inventory ownership:** The Legal Identity or other eligible party that owns the goods.
5. **Physical custody:** The party currently storing, controlling, or handling the goods.
6. **Warehouse operational authority:** Which business and authorized Users may configure, view, assign, or execute work at the facility and for which stock scope.
7. **Service/contract relationship:** The agreement or permitted business relationship under which another party operates, stores, or handles goods, where relevant.

An Internal Warehouse does not necessarily mean the business owns the land/building. An External Warehouse does not mean the external operator owns the goods. A business may own inventory held at a third-party 3PL facility; supplier/customer-owned goods may also be held at a business-operated facility under an authorized custody or consignment arrangement.

Facility ownership, property/occupancy, operational control, Inventory ownership, physical custody, Service Provider association, and access permissions must never be inferred from one another. A third-party operator receives only explicitly authorized facility/task/stock data, not unrestricted access to the business's Inventory, Customer, Supplier, Accounting, HR, or Tenant data.

### 6E.7 Warehouse physical and logical location hierarchy

Warehouse layout must be configurable to the actual structure, scale, and operating complexity of each facility. CBM must not force every facility to use every hierarchy level.

A supported conceptual hierarchy may include:

**Business Site → Warehouse Facility → Warehouse → Storage Area / Zone → Storage Type / Section → Aisle → Rack → Shelf / Level → Bin / Slot**

Additional optional locations may include receiving docks, dispatch docks, inspection areas, staging areas, bulk-floor areas, open yards, cold/controlled areas, picking areas, packing areas, and returns areas.

- **Business Site:** The wider operating site in which a facility may exist.
- **Warehouse Facility:** The physical or authorized third-party operating facility.
- **Warehouse:** The warehouse operating unit or logical warehouse within a facility, where needed.
- **Storage Area / Zone:** A physical or logical subdivision used for operational, environmental, security, turnover, or handling purposes.
- **Storage Type / Section:** An optional classification of storage areas/positions by characteristics, restrictions, or putaway/picking purpose.
- **Aisle:** A physical or logical access path between storage positions.
- **Rack:** A storage structure within which shelves or levels may be arranged.
- **Shelf / Level:** A defined horizontal or vertical storage subdivision.
- **Bin / Slot:** The smallest independently addressable storage position where bin-level control is enabled.
- **Dock / Staging Area:** A receiving, dispatch, inspection, or temporary holding area used for movement coordination.
- **Handling Unit:** A pallet, tote, carton, container, or similar carrier whose contents and movements may be tracked together.

A simple facility may use only Warehouse → Location. A small Shop may use only a stockroom or a few named locations. An advanced warehouse may use zones, aisles, racks, shelves, bins, Handling Units, capacity rules, and directed work. Optional levels and naming conventions are configurable subject to uniqueness, addressability, traceability, and stock integrity.

A Bin is an addressable storage position; it is not necessarily a physical container. The actual container/carrier may be represented as a Handling Unit.

### 6E.8 Racks, shelves, bins, Handling Units, and counters

1. Racks, shelves/levels, and bins/slots are distinct location elements. A rack may contain shelves/levels; each level may contain independently addressable bins where required.
2. The model must support storage positions that are not rack-based, such as bulk floor, open yard, tank, cage, cabinet, cold room, or other physical/logical storage areas.
3. A Handling Unit identifies a movable grouping/carrier of goods, such as a pallet, carton, tote, cage, or container. Its identity, contents, and movement history must not be confused with Product/Item, Inventory owner, or Storage Bin.
4. A **Warehouse Counter / Workstation** is an operational work point, such as receiving, inspection, picking/issue, packing, returns, dispatch, or material issue. It is not automatically a Storage Location or Bin.
5. A **POS Counter** or checkout terminal belongs to POS, not Warehouse Management. A Warehouse may reference a Shop/POS hand-off point where authorized without taking ownership of POS operations.
6. If goods are physically held or staged at a counter/workstation, the stock-holding position must be separately represented as a temporary or fixed Storage Location/Bin and linked to that workstation.
7. Counter/workstation configuration, task types, stock-holding behaviour, and device use must be applicable to the actual operation.

### 6E.9 Warehouse Work and execution lifecycle

Warehouse Management must support configurable warehouse work patterns appropriate to the business, including:

- Expected receipt, receiving, and unloading.
- Inspection and putaway.
- Replenishment.
- Picking and picking confirmation.
- Packing and Handling Unit formation.
- Staging and loading.
- Dispatch confirmation.
- Returns receiving and inspection.
- Internal location transfer.
- Cycle counting and physical inventory evidence.
- Exception handling for shortage, overage, wrong location, damage, mis-pick, failed task, or external-custody discrepancy.

Warehouse work may be triggered by Purchase, Sales, POS, Inventory, return, transfer, or authorized manual operation. The source Module owns the originating order or business transaction. Warehouse owns the execution Task; Inventory owns the accepted stock impact.

Warehouse task states may include requested, planned, assigned, in progress, partially completed, completed, cancelled, failed, or exception, as appropriate to task type. These are Warehouse operational states; they do not replace source order states or Inventory stock states.

Partial quantities, split Handling Units, authorized substitutions, retries, duplicate messages, delayed confirmations, and failed execution must be handled explicitly and traceably.

### 6E.10 Inventory–Warehouse Shared Models and execution contracts

Inventory and Warehouse must cooperate through explicit contracts:

**A. Inventory Position Model:** Inventory owns Product reference, stock owner where applicable, quantity, unit, status, batch/serial/expiry/condition, reservation, valuation, and accepted stock movement. A Warehouse Location reference may be attached where physical storage is tracked.

**B. Warehouse Location and Handling Contract:** Warehouse owns facility, warehouse, zone/area, Storage Type/Section, aisle, rack, shelf/level, bin/slot, dock/staging, Handling Unit placement, and operational task references. Inventory consumes permitted location references needed for stock-by-location integrity.

**C. Warehouse Execution Contract:** Communicates source request, stock/context references, requested physical operation, task identity, actor/device, expected and actual quantities/units, source/destination location, Handling Unit, completion evidence, exceptions, and acknowledgement/reconciliation state.

**D. Inventory Movement Contract:** Inventory records the accepted stock quantity/status/location effect linked to source transaction and warehouse execution evidence where required. It must preserve idempotency and prevent duplicate stock movements.

Contracts must define producer/consumer, ownership, data fields, permitted operations, status transitions, correlation/source IDs, versioning, retries, partial completion, failure, correction, and authorization.

### 6E.11 Relationships with other Business Modules

| Related Module | Its authoritative responsibility | Inventory/Warehouse relationship |
|---|---|---|
| Product/Item Management | Product identity, core descriptions, variants, attributes and classification | Inventory references products; Warehouse uses handling/storage attributes. |
| Purchase Management | Supplier order, commercial purchase lifecycle and original documents | Purchase provides expected-receipt facts; Warehouse receives/inspects goods; Inventory accepts stock and valuation facts. |
| Sales Management | Sales order, commercial sale and original documents | Sales requests availability/reservation; Inventory supplies stock status; Warehouse picks/packs/dispatches. |
| POS | Checkout and POS transaction capture | POS requests stock availability and provides sale facts; Inventory records stock effects; Warehouse may replenish or fulfil pickup. |
| Finance/Payment | Payment, collection, banking and settlement | Payment execution remains separate; custody or warehouse fees may be referenced through approved contracts. |
| Accounting Management | Financial ledgers, accounting treatment and financial statements | Inventory supplies approved valuation/accounting facts; Warehouse task completion alone does not post Accounting entries. |
| GST/Tax | Tax classification, applicability, determination, liability and statutory records | Tax facts come from GST/Tax; warehouse location does not itself determine GST registration or tax liability. |
| Human Resource Management | Employee and employment records | Warehouse tasks may be assigned to authorized Users/Employees; Employee and User Account remain distinct. |
| Payroll Management | Payroll calculations and authoritative payroll records | Permitted work/time facts may be supplied through authorized Human Resource Management and Payroll Management contracts; Warehouse does not own payroll calculations. |
| Asset Management | Physical asset custody, maintenance, assignment and operational lifecycle | Warehouse may provide location/handling references; warehouse bins and equipment are not automatically fixed-asset accounting records. |

### 6E.12 Simple storage versus advanced Warehouse Management

1. Inventory Management must support simple Storage Location/Godown tracking for businesses that do not need advanced WMS.
2. Advanced Warehouse Management adds facility layout, optional hierarchy levels, bin/rack/Handling Unit control, directed work, work queues, task execution, capacity, and detailed physical-placement capabilities.
3. A simple Inventory Storage Location does not automatically become a fully modelled Warehouse Management facility. Enabling advanced WMS requires governed activation/migration, explicit location hierarchy, stock reconciliation, and mapping of existing locations.
4. Where advanced WMS is not enabled, Inventory may maintain simple location identifiers and stock-by-location balances. These do not imply WMS task execution, bin-level placement, or Handling Unit tracking.
5. Where WMS is enabled, Warehouse owns location topology/physical placement; Inventory owns quantity and status attributed to those locations.
6. A business may use simple storage tracking in one context and advanced WMS in another where configuration and entitlements permit. The authoritative Inventory Ledger must remain coherent and must not fragment into duplicated balances.

### 6E.13 External Storage-Location Data Mapping

1. Storage-location information imported from an external system must be interpreted according to its verified source meaning, available detail, and the customer's business-continuity requirements.
2. A basic source location may be mapped to a CBM Inventory Storage Location where that is the correct semantic equivalent. It must not automatically be represented as an advanced Warehouse Management facility.
3. A source hierarchy may be mapped to CBM Warehouse Facility/location concepts only where the source details genuinely identify those physical or operational levels, the mapping is valid, and the customer has an applicable need for the corresponding CBM Warehouse capability.
4. CBM must not invent facilities, zones, racks, shelves, bins, capacities, Handling Units, warehouse tasks, physical movements, operator identities, or custody facts that are absent from the source or have not been independently verified.
5. The source system's storage model must not determine CBM's Inventory or Warehouse architecture. Imported records must be routed to their authoritative CBM domain and pass its validation, authorization, and reconciliation rules.

### 6E.14 Applicability, configuration, and governance across the Customer Business Architecture

1. Inventory Management and Warehouse Management may apply to eligible Organizations, Distributors, and Shops according to actual operations. Neither Module is automatically required at every structural level.
2. An Organization may operate central warehouses serving eligible operating contexts. A Distributor may operate warehouses, godowns, depots, or external 3PL facilities. A Shop may use a stockroom, simple locations, racks, shelves, bins, or advanced store-room operations where justified.
3. Applicability must be established through explicit Business Context, accountable Inventory owner, facility relationship, stock ownership/custody, and authorized inter-business contract. It must not be inferred from the Organization → Distributor/Shop hierarchy or Distributor–Shop Mapping.
4. Warehouse layout, optional hierarchy, storage types, rack/bin structures, Handling Unit types, counters/workstations, capacity rules, putaway/picking strategies, operating calendars, external-facility arrangements, inventory tracking dimensions, stock statuses, reservations, and applicable valuation methods must be configurable within governed limits.
5. Optional capabilities require explicit applicability, entitlement where relevant, authority, User Scope, and Permissions. Third-party operators receive only facility/task/stock data required for authorized work.
6. Configuration must be versioned/effective-dated where necessary and auditable. It must not permit one business to alter another business's facilities, inventory, stock ownership, or data boundary.

### 6E.15 History, reconciliation, and physical integrity

Inventory and Warehouse maintain distinct Domain Histories for their own records. Inventory History includes stock receipts/issues/transfers/adjustments, valuation-relevant changes, reservation/status transitions, and reconciliation. Warehouse History includes facility/layout changes, location changes, Handling Unit placement, task assignment/execution, physical counts, and operational exceptions. The comprehensive Audit Trail remains separate under Section 17A.

Physical counts, damaged/blocked/quarantined stock, missing goods, overages, short picks, wrong-bin placements, external-custody discrepancies, partial task completion, and uncompleted tasks must be explicit records or exceptions. They must not be silently corrected by overwriting previous states. Inventory adjustments require Inventory authority and validation; Warehouse preserves physical evidence and execution history.


## 6F. Product/Item Management, Product Identity, Variants, Identifiers, and Trade Packaging

### 6F.1 Independent Business Module and purpose

**Product/Item Management is an independent Business Module.** It owns the business responsibility for defining, classifying, identifying, governing, and maintaining the product/item catalogue and its lifecycle. The **Product/Item Reference Model** remains a Shared Domain Model and contract; it is not a second Business Module or a universal shared-write database.

Product/Item Management provides reliable canonical references to eligible consumers, including Sales, Purchase, POS, Inventory, Warehouse Management, and Asset Management. Each consumer owns its own source transactions and operational facts. A screen or workflow may coordinate several Modules, but it must not merge their ownership. Product/Item Management does not own sales orders/invoices, purchase orders/bills, POS transactions, customer/supplier relationships, Inventory quantities/valuation, Warehouse physical execution, Accounting entries, GST/Tax determinations, payment execution, or asset lifecycle merely because those domains use Product/Item references.

The model must accommodate physical goods, services, non-stock items, consumables, raw materials, components, configurable offerings, and future eligible item classes without forcing every business to use every capability. Applicability is explicit and governed.

### 6F.2 Canonical identity structure

CBM must distinguish the following concepts:

1. **Product Definition (Product):** the common catalogue identity and shared characteristics.
2. **Product Variant:** a specific configured form of the Product that must be distinguished for an applicable commercial or operational purpose.
3. **Product/Item Instance:** an individual physical occurrence, represented separately only when the applicable tracking, operational, contractual, or legal requirement needs unit-level identity.
4. **Product Family / Group / Classification:** optional relationships used to organize or classify Products; they are not required identity parents.
5. **Trade Packaging Configuration:** an optional packaging/trade-unit definition associated with a Product Variant.
6. **Product Bundle / Kit:** a separately governed composition of Product Variants and/or trade units where the relevant business use requires it.

The conceptual relationship is:

- Optional Product Family, Group, Category, Collection, Brand Line, or other taxonomies may classify or group one or more Products through typed relationships. They do not create a compulsory Product parent-child tree.
- A Product Definition has one or more Product Variants. Where the Product has no meaningful variation, CBM must support one standard/default Variant. The user experience may hide variant administration for such a simple Product, but any transaction or stock record that requires a concrete operational item must resolve to an explicit Variant-level reference.
- A Product Variant may have zero or more Trade Packaging Configurations and may be referenced by zero or more Product/Item Instances.
- Batch/Lot and serial/instance tracking are Inventory-owned operational traceability structures linked to the Product Variant. They are not extra Product hierarchy levels.
- A Product Bundle/Kit may compose several Product Variants or trade units according to a versioned composition rule.

A Product Definition, Product Variant, Product/Item Instance, package, lot, serial, component, and Handling Unit are different subjects. They must never be collapsed into one generic “Product” record or one untyped code field.

### 6F.3 Product Family, groups, taxonomies, and classifications

Product Family is optional. CBM must not require every Product to belong to exactly one Family, nor require a Family to be the sole parent of a Product. A business may use one or more independent classification schemes for different purposes, such as product category, merchandise hierarchy, technical family, brand line, collection, season, department, regulatory class, or reporting group.

Each taxonomy must define its own scope, hierarchy rules, allowed membership cardinality, effective lifecycle, governance, and applicability. A Product may belong to multiple taxonomies where the relevant scheme permits it; a particular taxonomy may impose one primary classification where that business purpose requires it. Classification membership does not change Product identity, Product Variant identity, data ownership, pricing, stock, or Business Structure.

Product Family, Product Category, Brand, Manufacturer, Supplier, Customer, and Business Entity are distinct concepts. Manufacturer and brand-owner information must not be inferred from the supplier currently selling an item. HSN/SAC is a regulated tax-classification system, not a Product taxonomy or Product Identifier. A Product taxonomy answers a product-organization/reporting question; an HSN/SAC assignment answers a statutory goods/services classification question. Product/Item Management may reference the applicable HSN/SAC assignment for an eligible Variant but does not own or redefine the tax-classification registry or GST rate rules.

### 6F.4 Product Variant model and variant dimensions

A Product Variant represents a meaningfully distinguishable configured form. Variant dimensions may include, for example, colour, size, capacity, memory, storage, material, grade, style, configuration, voltage, power rating, model/version, finish, flavour, pack content, or other business-relevant characteristics. This list is illustrative and must not be hard-coded as a universal fixed set.

Each Product type/category or Product Definition may use an appropriate governed attribute/dimension schema. The schema may support required, optional, conditional, dependent, mutually exclusive, multi-value, and context-specific values. It must allow valid combinations to be defined explicitly; not every mathematical combination of values must be created as a valid Variant. A business may create only the combinations it actually offers, purchases, stocks, configures, or otherwise manages.

Only attributes that change the identity or separate handling of an operational offering should normally define a Variant. Descriptive facts that do not distinguish the item for the relevant business purpose should remain attributes rather than generating unnecessary Variants. Physical dimensions common to all Variants may remain Product-level attributes; they must not automatically create Variants.

CBM must support at least these patterns where applicable:

- A simple Product with one standard/default Variant.
- A Product with pre-defined, separately selectable Variants.
- A Product with a constrained set of valid combinations, including combinations that are unavailable or prohibited.
- A configurable Product where some values are selected for an order or service instance and need not all be pre-created as permanent catalogue Variants.
- A Product or service with variants even where no Inventory record is required.
- A Product with variants that differ in purchaseability, saleability, stockability, traceability, or operational applicability.

The architecture must not create a permanent Variant for every theoretical combination. For configurable-to-order or made-to-order cases, the chosen configuration must be preserved in the relevant transaction/configuration snapshot; it becomes a separately stocked or permanently identified Variant only where the applicable lifecycle requires that identity.

### 6F.5 Product attributes, inheritance, and controlled customization

Product/Item attributes must be typed, governed, reusable, and applicable at the correct subject level. Attribute definitions may specify data type, unit of measure, allowed values, validation rules, precision, cardinality, language/localized label, sensitivity, effective dates, source/provenance, and applicability to Product, Variant, packaging, instance, or another supported subject.

For each attribute, the governing schema must expressly define one of the applicable behaviours, including:

- Common Product-level value inherited by Variants and locked against lower-level override.
- Product-level default inherited by Variants with an authorized Variant-level override.
- Attribute required or maintained only at Variant level.
- Attribute applicable only to an individual package, lot, instance, component, or other subject.
- Derived/calculated value whose source and recalculation policy are explicit.
- Not applicable to the selected Product type or business context.

Inheritance and override behaviour must be explicit per attribute; it must not be inferred merely because two records are related. A Variant override must not silently change the Product Definition or sibling Variants. Changes to a Product-level default must not silently rewrite historical transactions, existing instance records, or previously accepted stock facts.

Authorized businesses may define permitted custom attributes, product templates, variant dimensions, validation rules, categories, labels, and workflows through the Configuration Management governance of Section 10. Main Portal may establish mandatory attribute schemas, controlled vocabularies, reserved identifier types, or validation boundaries where interoperability, legal compliance, security, or data integrity requires them. Lower levels may extend or override only where the applicable authority and Permission expressly allow it. Custom fields must not silently redefine an established core field or the semantics of an external standard.

### 6F.6 Product codes, identifiers, namespaces, and uniqueness

A Product or Variant may have multiple identifiers for different purposes and issued by different parties. CBM must therefore use a typed identifier model rather than assuming one universal Product Code, one SKU, or one barcode.

Supported identifier types may include, where applicable:

- Immutable CBM internal record identifiers.
- Business-defined Product Codes and Item Codes.
- Business/catalogue-scoped SKUs (Stock Keeping Units).
- POS/retail PLUs and other operational lookup codes.
- Manufacturer model numbers and Manufacturer Part Numbers (MPN).
- Supplier/vendor item codes and customer part numbers.
- GS1 GTINs and other applicable GS1 identification keys.
- ISBNs, industry-specific identifiers, regulatory/product-registration identifiers, and other governed external identifiers where relevant.
- External marketplace, distributor, procurement, or legacy-system item references.

This list is extensible and is not an instruction to implement every identifier type. An identifier's name does not determine its subject level or uniqueness scope; its registered type definition does.

The following rules apply:

1. The CBM internal record identifier is distinct from every business or external identifier. It is not a substitute for a Product Code, SKU, GTIN, serial number, or barcode.
2. A Product Code may identify a Product Definition, while an Item Code or SKU may identify a Product Variant or another configured trade unit. The actual subject level must be declared by its identifier type and catalogue policy.
3. SKU is not a universal global Product identity. It is normally meaningful within the issuing business/catalogue or other declared namespace. Two businesses may legitimately use the same SKU for different items; the same item may have different SKUs in different authorized catalogues.
4. Product Code and SKU are distinct identifier types even where a business chooses to use the same character value for both. CBM must not infer equivalence from matching text.
5. Manufacturer, supplier, customer, and external-system codes must retain the identity of the issuer and source context. A supplier's item code must not be treated as a manufacturer code or CBM SKU without an explicit, verified mapping.
6. A GTIN identifies a GS1 trade item under GS1 rules; it is not an arbitrary CBM-generated Product Code. CBM must preserve its complete value and leading zeroes, validate it under the applicable standard, and record its allocator/provenance where available. CBM must not invent, fabricate, or allocate a GS1 identifier without the required GS1 licensing and allocation authority.
7. A GTIN is not automatically required for every Product, Variant, service, inventory item, or business. Its applicability depends on the trade item and the relevant supply-chain/sector requirements.
8. The same Product Variant may have multiple valid identifiers, such as an internal SKU, a manufacturer code, a supplier code, and a GTIN. A Product may also have a distinct Product Code in addition to Variant-level identifiers.
9. The same identifier value may appear under different identifier types or namespaces where their rules permit it. Uniqueness must be enforced within the identifier type's declared scope: for example, global, issuer-wide, manufacturer-wide, business/catalogue, external-system/account, or a defined combination of subject and namespace.
10. Each identifier type must define whether its value is case-sensitive, whether whitespace/separators are significant, allowed character set and length, checksum or validation algorithm, normalization and comparison rules, reuse prohibition or permitted reuse interval, and whether an identifier may be historical, temporary, or reassigned.
11. Identifier values must be stored and handled as strings where their semantics are code-like. Leading zeroes, significant letters, punctuation, and case must not be lost by numeric conversion or unsafe normalization.
12. Where a type permits multiple identifiers for one subject, CBM may distinguish a primary identifier for a particular business purpose from secondary identifiers and aliases. “Primary” is purpose/context-specific and does not make other identifiers invalid.
13. A retired or replaced identifier must retain its historical association and status. It must not be silently reassigned to an unrelated Product or Variant contrary to the identifier type's reuse rules.
14. Identifier validation, collision handling, duplicate resolution, and manual override must be governed and auditable. A matching code alone is not sufficient proof that two records represent the same Product.
15. HSN/SAC, product category, tax rate, descriptive model name, and free-text search label must not be treated as a Product identifier unless a separately defined, authoritative scheme explicitly gives that value an identification role.

An identifier record should be able to preserve, as applicable, identifier type, value, namespace, issuer/allocator, subject type and subject reference, scope, purpose, source, verification status, validity period, lifecycle status, provenance, and any applicable check/validation outcome. This is a logical contract, not a prescribed database schema.

### 6F.7 Barcode, QR, RFID, NFC, and scan resolution

A Barcode/Data Carrier is not synonymous with the identifier that it carries. CBM must distinguish:

- The identifier scheme/type and identifier value.
- The Data Carrier or symbology used to represent the value.
- The encoded payload, which may contain one identifier or several structured identifiers/attributes.
- The label/marking profile and the physical object or package on which the carrier is placed.

CBM must not assume that every barcode contains a SKU, that every Product has a barcode, that every barcode value is globally unique, or that the symbology determines the business meaning of the scanned data. Examples of carriers/symbologies include EAN-13, EAN-8, UPC-A, UPC-E, Code 128, GS1-128, ITF-14, GS1 DataBar, GS1 DataMatrix, GS1 QR Code, QR Code, RFID, and NFC. This is an extensible set; the applicability and supported scanners/devices are governed separately.

A single Product/Variant/package/instance may have multiple carriers or labels for different uses. A carrier may encode more than one value, such as a trade-item identifier together with batch/lot, serial, manufacture date, expiry date, quantity, or other allowed attributes. Scan processing must identify the symbology and applicable encoding/profile, parse the payload, resolve each typed value to the correct subject and context, validate the values, and reject or route ambiguous/unknown payloads for authorized resolution. It must not treat an arbitrary scanned string as a valid Product automatically.

Where GS1 Application Identifiers are used, their defined data semantics and format rules must be preserved; the same AI value must not be repurposed for a CBM-specific meaning. GS1 Data Carriers, GS1 Application Identifiers, and non-GS1 carrier formats may coexist through registered profiles. Non-GS1 product codes and barcodes must remain supported where permitted by the business use case and device capability.

Each standards-based profile must identify the standards body, standard and release/version, applicable ratified change notifications or amendments, relevant market/sector/application scope, effective date, supported identifier keys/AIs, data carriers, and parsing/validation rules. A newly ratified change notification does not silently rewrite the interpretation of historical labels, identifiers, or accepted transactions. Standards changes must be adopted through a versioned profile and governed compatibility/migration process. CBM must distinguish the latest published standard release from ratified changes announced for a future release; it must not treat an unpublished future consolidated edition as already published.

Printing, scanning, barcode generation, RFID/NFC encoding, and device interaction are shared technical capabilities or client/device integrations. They do not take ownership of Product/Item identity or stock/warehouse transactions. A scanned identifier does not itself authorize a stock movement, sale, purchase, adjustment, or other transaction. Product labels, printed materials, and outbound communications must also comply with the existing absolute no-URL rule in Section 15 and invariant 64: a QR/Data Carrier must not be used to send or expose a URL-resolving action where that rule applies.

### 6F.8 Physical instances, component identifiers, batch/lot, and serial tracking

A Product Variant may represent many physically identical units. The Variant is the repeatable configuration; the individual unit is an Instance. CBM must not force an individual instance record for every quantity-based item. A business may track stock by aggregate quantity where the use case and applicable requirements permit it.

Where individual traceability is required, an individual Product/Item Instance must be represented and associated with the relevant Product Variant, and its inventory/asset lifecycle must be owned by the appropriate Business Module. Examples include serial-tracked electronics, individually controlled equipment, regulated medical devices, or other items where a unique-unit history is necessary.

A Product/Variant may have an applicable tracking profile that states the tracking characteristics or minimum requirements for downstream operations. Inventory Management owns the authoritative operational tracking policy and records for stock, including batch/lot, serial, expiry, condition, and their relationship to accepted stock movements. The Product/Item profile is a product-side constraint/reference, not an Inventory Ledger or a substitute for Inventory's policy. A business-context-specific Inventory policy may impose stricter tracking where authorized, but must not reduce an applicable legal, regulatory, contractual, manufacturer, or already-accepted traceability requirement.

Supported tracking patterns may include:

- Quantity-only tracking, where no individual unit or batch identity is required.
- Batch/Lot tracking, where a group of units shares a traceability identity.
- Individual serial tracking, where each controlled unit requires a distinct serial/instance reference.
- Batch/Lot plus serial tracking, where each instance is associated with a batch/lot.
- Expiry, best-before, manufacture date, production source, or other traceability attributes at the level required by the product and applicable domain policy.
- Other industry-specific or future tracking profiles introduced through governed extension.

A serial number identifies an individual unit or component within its declared issuer/type/namespace and uniqueness scope. It is not a Product Variant attribute and must not be stored as if every unit had the same serial. A serial value may not be globally unique across all manufacturers or identifier types; the identifier definition must determine its scope and validation. The association must retain Product Variant, issuing party, identifier type, value, and instance/component subject so that the same alphanumeric serial issued by different manufacturers does not create a false collision.

One physical unit may carry multiple identifiers, for example a manufacturer serial number, one or more IMEI values, an internal asset tag, a supplier reference, or another unit-level code. These values must be represented as distinct typed identifiers linked to the same Unit Instance or to the correct subcomponent/interface. An IMEI, MAC address, component serial, or other interface/component identifier must not automatically be treated as the identifier of the complete device if it identifies a narrower subject. The model must support parent-unit/component relationships and multiple identifiers per unit without collapsing the identifiers into one serial-number field.

Batch/Lot records may contain many units and may be linked to individual serial-tracked instances. A serial-tracked instance may also belong to a batch. Lot and serial are complementary, not mutually exclusive alternatives. Batch/Lot identity and stock lifecycle remain Inventory-owned. Where an item is also an operational fixed asset, Asset Management owns the asset's assignment, maintenance, depreciation/asset lifecycle, and related facts; it may reference the Product/Item Instance and identifiers without taking ownership of Inventory stock balances. Warranty Management is outside the currently approved product scope and must not be implied by serial capture alone.

### 6F.9 Units of measure, packaging, and trade-unit hierarchy

Product identity, Unit of Measure, Trade Packaging Configuration, and Warehouse Handling Unit must remain distinct.

The shared Unit of Measure and Packaging Reference Model provides governed units and conversions. Inventory, Sales, Purchase, and Warehouse apply them under their own domain rules and precision. The Product/Item catalogue may provide default/recommended units and packaging references, but it does not override a consuming Module's authoritative quantity, valuation, or transaction rules.

A Product Variant may be represented in more than one permitted unit of measure for purchasing, stocking, selling, or handling. Conversions must be explicit, dimensionally valid, and sufficiently precise. A conversion may be Variant-specific or context-specific where the physical/commercial facts require it. Variable-measure items such as goods traded by actual weight, length, area, volume, or another measured quantity must preserve the relevant actual measure; CBM must not assume that every package contains an invariant integer number of pieces.

A pack of 10 pieces, box of 50, or carton of 500 may be represented as a Trade Packaging Configuration associated with the underlying Product Variant, with its own pack code/identifier and GTIN where applicable. A package level may be separately saleable, purchasable, orderable, or stockable where the business requires it. Each independently traded packaging level must be identified and validated according to the relevant identification standard and allocator's rules; CBM must not assume that the individual unit's identifier can be reused for every outer pack. Stock quantities and conversions must avoid double-counting the same physical contents across pieces, boxes, cartons, and other levels.

Packaging is not automatically a Variant. If a packaging change materially changes the Product's defining configuration or trade identity, it may require a new Variant or another distinct Product/trade-unit identity under the applicable policy. If it only changes the permitted pack quantity or trade packaging level, a Trade Packaging Configuration is normally the more accurate representation. The decision must be explicit and preserve separate identifiers where required.

A Warehouse Handling Unit (such as a movable pallet, tote, container, or transport carton) is an operational logistics carrier whose contents and physical movements are managed by Warehouse Management. It is not automatically a predefined retail Trade Packaging Configuration. One Handling Unit may contain multiple Products/Variants or packages, and its contents may change through authorized warehouse work.

### 6F.10 Bundles, kits, components, and product relationships

CBM must distinguish at least the following use patterns where applicable:

- A **physical fixed bundle/kit** has a defined, fixed composition and is physically combined or assembled. Where it is independently traded or stocked as one unit, it must have an appropriate Product/Variant or separately identified Trade Unit representation and an explicit component composition.
- A **virtual bundle/offer** groups separately identified Products/Variants for a commercial offer but is not itself a physical stocked item. Its fulfilment and stock effects are recorded against the component items by the owning transaction and Inventory contracts.
- A **configurable/dynamic bundle** permits specified component choices or substitutions under a governed composition rule. The selected composition must be preserved for the relevant transaction/fulfilment instance.
- A **kit** may be a set of parts supplied together, a group prepared for one task, or an assembled product. Its meaning and inventory/warehouse treatment must be declared; the term alone does not determine stock or manufacturing behaviour.

Product/Item Management owns the catalogue-level definition and approved component relationships where applicable. Purchase, Sales, POS, Inventory, Warehouse, and any future Manufacturing/Production Module own their respective transaction, stock, physical execution, and production records. A product composition reference is not automatically a manufacturing Bill of Materials or a production order.

The Product/Item relationship model should support typed, optionally effective-dated relationships where useful, such as component-of, contains, accessory-for, compatible-with, substitute-for, replacement-for, successor-of, supersedes, or used-with. Each relationship type must define direction, subject levels, cardinality, applicability, validation, effective period, and whether it is informational or operationally enforceable. A compatibility or replacement link must not silently authorize substitution, change historical transactions, or alter the original Product/Variant identity.

### 6F.11 Commercial applicability, prices, and tax boundary

A Product Definition or Variant may be marked as applicable for sale, purchase, stocking, consumption, service delivery, or another supported business purpose. These are independently governed applicability capabilities; the existence of a Product record does not automatically enable every purpose. Applicability may vary by business context, channel, location, effective date, and authorized business policy.

Product/Item Management defines the item that a commercial or operational record refers to. Product Variant is the normal reference subject for product-specific commercial prices and goods tax classification. Where the applicable rules permit a Product-level default to cover several Variants, that inheritance must be explicit, validated, and versioned; it must not be assumed merely because Variants share a Product Definition.

Product/Item Management owns catalogue-level declared price records that are intrinsic to, or declared for, the Product/Variant/Trade Packaging Configuration, including legally applicable MRP, Manufacturer Suggested Retail Price / RRP, and required unit-sale-price declarations where the relevant packaged-commodity rules apply. Such declarations must preserve issuer/source, applicable package and declaration version, validity, and history. MRP, RRP/MSRP, and actual Selling Price are distinct.

Product/Item Management does not own every price or commercial amount associated with the item. Sales owns selling Price Lists, sales conditions, and sales transaction-price facts; Purchase owns supplier quotation/agreement and purchase transaction-price facts; Inventory owns inventory cost and valuation records; Accounting owns accounting records and postings; GST/Tax owns the HSN/SAC Code Registry, tax-classification assignments, GST rate applicability rules, tax determination, liability, and statutory records. Product/Variant records retain explicit references to applicable classification assignments; they must not embed or independently redefine HSN/SAC master codes or GST rates as ordinary Product attributes. A Product/Variant reference is not itself a price, discount, landed cost, inventory valuation, or accounting value.

The complete cross-domain Price Reference, Price Determination, tax-basis, price-history, and transaction-snapshot rules are defined in Sections 6G and 6H. The Product Variant to HSN/SAC assignment relationship and GST rate determination boundary are defined in Section 6H.

### 6F.12 Catalogue ownership, publication, sharing, and tenant isolation

Every Product Definition, Variant, classification, identifier set, and catalogue customization must have an explicit owning Business Context or an explicitly governed platform reference-data owner. Product/Item Management is responsible for catalogue ownership metadata and sharing contracts; being a Service Provider or having a Distributor–Shop Mapping does not automatically confer ownership of, or access to, another business's Product catalogue.

A business may maintain its own catalogue, use an authorized published catalogue, or combine its own items with externally sourced catalogue references. Catalogue publication and consumption must be explicit, permission-controlled, purpose-limited, revocable, and governed by source-of-truth, version, update, and conflict-resolution rules. A Main Portal or Sub-Portal reference catalogue may be offered only under an authorized publication/service model; a Sub-Portal does not become the owner of a customer's catalogue merely because it provides the service.

Where an Organization makes catalogue defaults available to its Distributor/Shop children, the relationship, inherited data, permitted local additions, override rights, and update behaviour must be explicitly configured under Sections 7 and 10. Structural parentage alone does not create unrestricted catalogue access or edit permission. Child-specific customizations must not mutate the parent's Product definitions or another child's values.

A local business code, identifier alias, descriptive override, or permitted attribute customization must remain scoped to its owning context. It must not rewrite the source Product or another business's catalogue. Cross-Tenant catalogue data sharing must not bypass strict Business/Tenant Data Isolation. A separately authorized published reference may be consumed through a governed read/replication contract; it does not create shared write access or merge ownership.

If an external Product record is imported as a new locally owned catalogue record, CBM must preserve its source system, source identifier, mapping, import provenance, and any source version available. It must not silently assume that similarly named Products from different systems or businesses are identical. A deliberate merge or mapping must be validated and auditable.

### 6F.13 Lifecycle, versioning, change, and history

Product Definition, Product Variant, identifiers, packaging configurations, bundles/compositions, and classification memberships must have separately governed lifecycle states and effective dates as required by their purpose. A Product may remain active while one Variant is discontinued; a Variant may stop being offered while existing inventory, returns, service, audit, or historical transaction references remain valid.

A discontinued, blocked, superseded, or inactive catalogue item must not be deleted or silently repurposed in a way that breaks historical references. The applicable consuming Modules define whether new transactions are blocked, existing stock may still be sold/issued, returns are allowed, or a replacement is required. Replacement/supersession creates an explicit relationship; it does not rewrite old transactions or automatically transfer stock.

Changes to Product/Variant attributes must preserve the definition that applied at the time of an accepted transaction, stock movement, or individually tracked event. Where the consuming Module needs an immutable description/configuration snapshot, it must capture it through its own source-document or history contract. Product/Item Management maintains its own Domain History for catalogue changes; the comprehensive Audit Trail remains separate under Section 17A. History must distinguish who changed a value, which context and source owned it, when it was effective, when it was recorded, and whether the change was a correction, replacement, or new version.

Identifier changes, retirements, external remappings, merges, and reassignments must preserve provenance and historical association. A new GTIN or other standards-governed identifier must be allocated only under the applicable issuing authority's rules. Product similarity, a new image/name, a packaging change, or a supplier import must not trigger automatic identifier reuse or merge.

### 6F.14 External data exchange and interoperability

The Product/Item Reference Model must be vendor-neutral and independent of any one accounting, ERP, POS, marketplace, warehouse, or inventory product. External system structures and customizations do not define CBM's canonical Product, Variant, package, identifier, tracking, or bundle model.

External imports may include Product/Variant records, identifiers, barcodes, attributes, dimensions, units, packaging, manufacturer/supplier references, classification, and available provenance. Each receiving domain must validate and accept only the data within its ownership boundary. Missing or ambiguous variant distinctions, barcodes, serials, packaging conversions, or ownership must be identified and resolved by authorized data entry or a documented mapping; CBM must never invent identifiers, serials, unit-level identities, conversion factors, or product facts.

Exports/offboarding must preserve identifiers with their types/namespaces, Product/Variant relationships, packaging and unit conversions, relevant classifications, effective versions, source mappings, and traceability data within the authorized data scope. Export does not promise that an external application will support or reproduce CBM's complete data model or configuration. Interoperability contracts must follow Section 6D.8 and the approved External-System Onboarding and Customer Offboarding rules.

### 6F.15 Configurability, extensibility, and mandatory integrity controls

Product/Item Management must be designed against the full architecture-wide doctrine in Sections 2.6, 10, and 18. Subject to Main Portal governance and Stable Core, it must support controlled variation in:

- Product types, catalogue templates, attribute definitions, value dictionaries, labels, and applicable taxonomies.
- Variant dimensions, allowed values, dependencies, valid combinations, and configurable-to-order patterns.
- Product Codes, SKU policies, identifier types, namespaces, validation, uniqueness scope, and purpose-specific primary identifier choices.
- Barcode/Data Carrier symbologies, parser/encoding profiles, versioned standards profiles/change notifications, supported device capabilities, and label profiles.
- Sale, purchase, stock, service, and other applicability rules.
- Product/Variant tracking characteristics and Inventory tracking-policy references.
- Packaging levels, units, conversions, trade-unit identification, and bundle composition.
- Lifecycle rules, approvals, publication, authorized local additions, permitted overrides, and change workflows.

Configuration must be versioned, scoped, auditable, and applied only to eligible contexts. It must not make every possible feature mandatory or add needless complexity to simple businesses. A Shop managing only a small number of quantity-based items must not be forced to configure product-family hierarchies, serial tracking, advanced packaging, or Warehouse functionality. A regulated or traceability-intensive business must be able to enable the necessary stronger controls without changing the core model.

The following are mandatory Stable Core/integrity boundaries and are not ordinary business-level override options:

1. Strict Business/Tenant Data Isolation and authorized access.
2. Immutable CBM record identity and preservation of historical references.
3. Explicit identifier type, issuer/namespace, subject level, and uniqueness scope; no assumption that one code or barcode identifies every level.
4. Preservation of significant identifier characters and leading zeroes.
5. No fabricated or unauthorized GS1 or other externally governed identifiers.
6. No cross-context catalogue access, edits, or overrides without explicit authorization and publication/sharing contract.
7. No silent mutation of another Product, Variant, business catalogue, accepted stock fact, or historical transaction.
8. Inventory and Warehouse ownership boundaries in Sections 6D and 6E.
9. Legal, regulatory, contractual, and other higher-authority tracking/identification requirements must not be weakened by lower-level configuration.
10. All changes, imports, identifier resolution, merges, overrides, and lifecycle transitions must preserve required Domain History and Audit Trail separation.

Flexibility must come from a clear stable identity model plus governed extensions, not from untyped fields, ambiguous code reuse, uncontrolled variant proliferation, unrestricted tenant sharing, or domain-boundary violations.




## 6G. Pricing, Price References, Price Determination, and Price History

### 6G.1 Purpose and architectural classification

CBM must support multiple kinds of prices and commercial amounts without reducing them to one generic Product price field or merging the authoritative responsibilities of Product/Item Management, Sales, Purchase, Inventory, GST/Tax, and Accounting.

The **Price Reference and Price Determination Contract** is a Shared Domain Model/contract. Reusable price lookup, condition evaluation, calculation, and resolution capabilities may be provided as shared capabilities where appropriate. Their existence does not establish a separate Pricing Business Module or create one universal authoritative Price Master. Each Price Record remains owned by the Business Module responsible for the business meaning and lifecycle of that price.

Price applicability, effective dates, price conditions, approval, visibility, and override rules must follow the architecture-wide principles in Sections 2.6, 6A, 10, 17A, and 18. A common price interface or calculation service must never take ownership of another Module's source record.

### 6G.2 Price categories and authoritative ownership

The following Price Types and ownership rules are established:

| Price Type / Amount | Authoritative owner | Required distinction |
|---|---|---|
| Legally applicable Maximum Retail Price (MRP) | Product/Item Management | Declared maximum retail price for covered packaged commodities; subject to applicable Legal Metrology law and declaration source. |
| Manufacturer Suggested Retail Price / Recommended Retail Price (MSRP/RRP) | Product/Item Management | Manufacturer/brand-owner commercial recommendation; not automatically a statutory MRP ceiling. |
| Other manufacturer/importer/brand-owner declared or list price | Product/Item Management | Sourced declaration/reference, not automatically the actual price charged by a seller. |
| Legally required unit-sale-price declaration | Product/Item Management catalogue/package declaration | Maintained where the applicable packaged-commodity rules require it; it is distinct from the actual transaction price per unit. |
| Base/List/Retail/Wholesale/Dealer/Distributor Selling Price | Sales | Governed selling Price Lists and sales applicability conditions. |
| Customer-specific, customer-group, contract, channel, location, quantity-break, or promotional Selling Price | Sales | Sales-owned Price Records and Price Conditions; accepted transaction values remain on the Sales/POS source transaction. |
| Supplier quotation, supplier Price List, negotiated purchase reference, or Purchase Agreement Price | Purchase | Supplier and agreement-specific commercial terms; accepted Purchase transactions retain their own actual values. |
| Actual Purchase Price | Purchase | The accepted price recorded in the relevant purchase transaction/document; it is not a reusable universal cost field. |
| Landed/Acquisition Cost and Inventory valuation cost | Inventory | Inventory-owned cost formation, allocation, costing method, and valuation records. |
| Standard/Planned Cost or other Inventory cost reference | Inventory | Governed cost reference within the Inventory domain; not the actual supplier transaction price. |
| Last/average purchase-price references | Derived from authorized Purchase data, under the relevant consuming/reporting domain contract | Calculated values with defined context and methodology; not manually maintained as an unrelated authoritative price master. |
| Tax amount, tax rate, tax classification, and statutory tax determination | GST/Tax | Tax remains independently determined by the authoritative Tax domain and applicable law; it is not owned by the Price List or Price Record. |
| Accounting value and accounting posting | Accounting | Resulting accounting interpretation and records; not the source of Sales/Purchase price facts or Inventory cost methodology. |

The phrase **Original Price** is ambiguous and must not be used as a universal Price Type or a single field for purchase cost, list price, MRP, or historical price. The actual business meaning must be represented by an explicit Price Type.

POS consumes the applicable Sales pricing contract and owns its accepted POS transaction facts; it must not create a competing authoritative sales Price List. A future Pricing Business Module is not established by this Section. Any future proposal to change module classification requires separate project-owner approval.

### 6G.3 Price subject and relationship to Product, Variant, and packaging

A Price Record must explicitly identify the subject to which it applies. Permitted subjects may include Product Definition, Product Variant, Trade Packaging Configuration, or another eligible subject expressly supported by the owner domain.

Product Variant is the normal subject for a price that applies to the configured item itself. Trade Packaging Configuration is the preferred subject when the amount is specific to a pack, box, case, carton, or other separately identified trade unit. Product-level prices may be used only where the owner domain's explicit rule makes the price valid across all eligible Variants or where it is a declared default with controlled inheritance.

A Price Record must identify the quantity basis and unit/packaging basis to which its amount refers. For example, price per piece, kilogram, litre, metre, pack, box, or carton must not be ambiguous. Variable-measure goods must preserve the applicable measured quantity and unit. A package price must not be interpreted as a per-piece price without an explicit conversion rule.

Where one declared price applies to a specific package revision, label/declaration version, batch/lot, or effective period, that association must be recorded where necessary to establish which declaration applies to the physical goods. CBM must not force per-unit price records when a package-level or batch-level reference provides sufficient traceability. MRP applicability and subject selection must follow the applicable Legal Metrology requirements; CBM must not assume that every Product or Variant requires an MRP.

### 6G.4 Price Lists, Price Conditions, applicability, and selection

Sales and Purchase must be able to maintain multiple independently governed Price Lists appropriate to their purposes. Examples include retail, wholesale, dealer, distributor, customer-specific, customer-group, contract, channel, supplier-specific, negotiated-agreement, and quantity-break prices. These are examples, not mandatory Price Lists for every business.

A Price List or Price Record may have explicit conditions for:
- Owning Organization, Distributor, Shop, or other eligible Business Context.
- Customer, customer group, supplier, supplier group, agreement, or contract.
- Sales/Purchase channel, POS context, location, or fulfilment context.
- Product, Product Variant, Trade Packaging Configuration, unit of measure, and quantity range.
- Effective date/time, expiry, season, promotion, or other time-bound commercial rule.
- Currency, which remains INR only under the approved Global Business Settings.
- Tax basis where relevant.
- Any additional governed business condition or applicability dimension.

Not every condition must be mandatory or supported for every Price Type. The owner domain must explicitly define which dimensions are valid, optional, required, mutually exclusive, or inapplicable. An absent condition must not accidentally broaden access or applicability beyond the owning business context.

Several Price Lists or Conditions may qualify for the same transaction. Their priority, combination/exclusivity, replacement, stacking, fallback, and conflict behaviour must be deterministic and governed by the owner domain. The system must not silently choose an arbitrary price when applicable records conflict or required information is missing. It must return a clear conflict, exception, or authorized resolution path.

Discounts, promotional reductions, surcharges, rebates, and other adjustments must remain distinct Price Conditions or transaction adjustments, as appropriate. They must not destructively rewrite the underlying list/base Price Record. Whether and how an adjustment affects taxable value is determined by GST/Tax under applicable law, not inferred solely from its label in a Price List.

### 6G.5 Time validity, versioning, and price changes

Price Records and Price Conditions must support effective-dated changes. A new price may be prepared in advance, approved, and scheduled to become effective at a future date/time. The end of one price's validity and the start of its successor must be explicit. Where overlapping validity is deliberately permitted, its priority and selection rules must be explicit; accidental conflicting overlaps must be detected.

The price model must distinguish:
1. **Effective Time:** when the price or condition is valid in the business world.
2. **Recorded Time:** when CBM recorded the price or change.
3. **Approval/Publication Time:** when the relevant authorization or publication made it eligible for use, where that lifecycle applies.

A Price Record change must create a new version or a traceable superseding record. The previous approved value and its scope must remain retrievable. A correction must preserve the original record and identify the correction reason, actor, approver where applicable, and relationship to the corrected/superseding record. Backdated changes must not silently alter completed transactions or conceal when CBM learned/recorded the change.

Price Records must have an explicit lifecycle appropriate to their owner domain, such as draft, submitted, approved, scheduled, active, suspended, expired, withdrawn, or superseded. These are conceptual states; an owner domain may use a governed subset or additional states. An unapproved/draft/suspended Price Record must not be used as an active price merely because its date range includes the current date.

### 6G.6 Tax-inclusive and tax-exclusive price basis

Where relevant, every Price Record must be able to state whether its amount is:
- **Tax Inclusive:** the stated amount includes the applicable indirect tax components represented by that price basis.
- **Tax Exclusive:** the stated amount is before the applicable indirect tax components, which are determined and added where legally applicable.
- **Not Stated / Not Applicable:** permitted only where the Price Type and business use legitimately allow the basis not to be specified or no tax-inclusive/exclusive representation applies.

Tax basis is part of the Price Record's meaning; it is not a User display preference. A User interface must not silently switch an authoritative inclusive amount to an exclusive amount or vice versa without a defined, authorized calculation and clear presentation.

This Section does not define, own, or store the applicable GST rate, HSN/SAC classification, tax category, tax applicability, Input Tax Credit entitlement, place-of-supply decision, or statutory tax liability. Those remain with the GST/Tax domain and applicable law, under Section 6H. A Price Record's tax basis must be consumed by GST/Tax through an explicit contract when a transaction is evaluated. A shared price calculation capability must not calculate or override statutory tax.

Purchase Price Records should normally default to Tax Exclusive for ordinary supplier price references because the commercial purchase price is generally maintained before GST. This is a default, not an assumption about every source document: an imported quotation, accepted purchase transaction, supplier invoice, or other source value may explicitly be Tax Inclusive or Tax Exclusive and must preserve the actual source basis. Sales Price Records may be Tax Inclusive or Tax Exclusive according to their declared commercial purpose and business policy. The basis must be explicit on each applicable Price Record and accepted transaction; a module-level default must never silently replace a source-declared basis.

Where a supply value is stated inclusive of GST, the relevant tax component must be extracted using the applicable statutory valuation rule, including CGST Rules Rule 35 where it applies. Tax-exclusive values must have tax determined separately by GST/Tax. The applicable transaction may also require additional statutory valuation adjustments or rules; inclusive/exclusive arithmetic alone does not replace GST valuation law.

### 6G.7 MRP, RRP/MSRP, and packaged-commodity declarations

MRP, RRP/MSRP, Selling Price, and unit-sale-price declarations are distinct.

For pre-packaged commodities covered by the Legal Metrology (Packaged Commodities) Rules, the declared Retail Sale Price/MRP is the maximum price at which the covered packaged commodity may be sold to the consumer and is declared inclusive of all applicable taxes. A retailer must not charge a covered packaged commodity above its applicable MRP. A lower actual Selling Price, discount, or promotion may be used where otherwise lawful; the MRP is not itself the transaction Selling Price. MRP must not be treated as a universal requirement or statutory ceiling for every service, loose good, non-covered package, or exempt category; applicability and exclusions must follow the current law and authoritative guidance.

Product/Item Management must preserve, where applicable:
- The declared MRP amount and INR currency.
- The identified Product Variant and relevant Trade Packaging Configuration/package level.
- The manufacturer, packer, importer, brand owner, or other responsible declaring party and source evidence, as applicable.
- The declaration/label version, effective/recorded dates, and applicable batch/lot or package revision references where required for traceability.
- The applicable Legal Metrology applicability/verification status and any permitted revised declaration or sticker history.
- Unit-sale-price declarations where legally required, including their declared measurement basis and applicable rounding/format requirements.

A manufacturer's suggested retail price or RRP/MSRP is a different commercial declaration. It must not be labelled MRP or enforced as the statutory MRP ceiling merely because it is a suggested/list price. Similarly, an MRP update must not overwrite the old package declaration; the original and revised declarations and their validity/traceability must remain available. Any permitted sticker, revised declaration, or correction must be handled only as allowed by applicable law.

CBM must use a versioned, maintainable Legal Metrology compliance profile for relevant declaration requirements. A legal applicability status must not be fabricated from a product name or category alone. The relevant business data, package facts, and legal rule version must be sufficiently established before CBM treats a declaration as verified.

### 6G.8 Price Determination, override, approval, and security

The domain owning a Price Record owns the authoritative Price Determination rules for its purpose. A reusable shared capability may execute those rules through a versioned contract, but it must receive explicit business context, subject, counterparty/channel, quantity/unit, effective time, and any other required inputs. It must return the selected Price Record/Condition references, version, amount, tax basis, calculation explanation, and any exception/conflict.

The owner domain must configure a deterministic priority and fallback policy. Examples include a contract-specific price taking precedence over a customer-group list, or a time-bound promotional condition applying only within its approved scope. Such examples do not establish one universal priority order for all domains.

An authorized User may be permitted to override a determined price only within explicit authority and policy. An override must record the original determined price, final entered/accepted price, reason, actor, timestamp, approval outcome/reference where required, and the affected transaction/context. Permission to override one Price Type or business context does not grant permission to modify another. Approval does not create missing action Permission.

Purchase cost, supplier agreement, margin, and Inventory valuation information may be commercially sensitive. Access must be independently permission-controlled by Price Type, owning domain, business context, purpose, and User scope. Sales/POS Users must not automatically see Purchase Cost or margin details merely because they can view or sell a Product.

Price List sharing, publication, inheritance, and local overrides across Organization, Distributor, and Shop contexts must be explicit and follow Sections 7 and 10. Structural parentage, Service Provider association, or Distributor–Shop Mapping alone does not grant access to another business's prices. Cross-Tenant price sharing must preserve strict Business/Tenant Data Isolation and must never create unrestricted shared-write authority.

### 6G.9 Purchase prices, cost formation, and historical transaction snapshots

Supplier quotation price, purchase agreement price, accepted purchase order price, supplier invoice price, acquisition/landed cost, and Inventory valuation are distinct values and events.

Purchase Management owns supplier quotations, purchase agreements, purchase orders, invoices, and accepted purchase transaction-price facts. A quotation or agreement reference does not prove that the goods were purchased at that amount. The accepted purchase transaction records its actual values and their source references.

Inventory Management owns its cost formation, allocation, costing policy, and stock valuation. Landed/Acquisition Cost may include relevant purchase and directly attributable cost components under the applicable Inventory policy; it must not be substituted automatically for the supplier invoice unit price. Standard or planned cost is a reference/policy value, not proof of actual acquisition price.

Last Purchase Price and Average Purchase Price are derived references. Their calculation must define the accountable Business Context, Product Variant/packaging and unit basis, supplier inclusion/exclusion, quantity/returns/charges treatment, time range, and source data. These derived values must be reproducible and must not be manually overwritten as though they were original transaction facts.

Whenever Sales, POS, or Purchase accepts a transaction, the source transaction must preserve an immutable Price Snapshot for its accepted line/value facts. It must capture as applicable the subject, selected Price Record and Price Condition versions, quantity and unit/packaging basis, base/selected price, discounts/surcharges, override details, tax basis, linked Tax Determination Snapshot/reference, final accepted line amount, currency, effective time, and provenance. The associated tax snapshot must preserve the HSN/SAC code and classification-assignment version where applicable, the selected GST Rate Applicability Rule and version, statutory notification/source reference, applicable component rates and tax amounts, and relevant determination context as defined in Section 6H. A later change to any Product, Price List, Price Condition, MRP, HSN/SAC assignment, GST rate rule, or Price Record must not silently rewrite that completed transaction's historical snapshot.

Corrections, credit/debit notes, returns, post-supply discounts, and other adjustments must be represented through the owning transaction domain's authorized correction/adjustment lifecycle. They must not be implemented by editing an old Price Snapshot or overwriting a historical transaction price.

### 6G.10 Configurability, domain independence, and integrity

Pricing must be designed against the complete architecture-wide doctrine: scalability, configurability, extensibility, customization, controlled override, applicability, business-specific variation, maintainability, and operability.

The responsible domains must be able, where applicable, to configure:
- Price Types and their permitted subject levels.
- Multiple Price Lists and independent business-purpose lists.
- Business, customer, supplier, contract, channel, location, quantity, UOM, and time applicability.
- Price Conditions, quantity breaks, discount/surcharge behaviour, priority, exclusivity, and combination policies.
- Tax basis for applicable Price Types, without making the Price Record authoritative for tax rates/classification.
- Effective-date scheduling, version lifecycle, approval requirements, and controlled overrides.
- Local price lists, publication, inheritance, sharing, and revocation within explicit authority.
- Price visibility and sensitive-data permissions.

Not every business must use every Price Type, list, condition, package level, or approval workflow. A simple Shop may maintain one uncomplicated selling list and a small number of Product declared prices. A distributor or complex business may use multiple customer/supplier lists, quantity breaks, contracts, and scheduled prices. Both use the same governed concepts without separate product forks.

The following rules are mandatory:
1. Price Records remain owned by their authoritative domain; the Shared Price Reference/Determination Contract does not create a universal price owner.
2. A single generic Original Price field must not replace distinct Price Types.
3. Product, Variant, and Trade Packaging references must not be confused with Price Records.
4. MRP is distinct from RRP/MSRP, Selling Price, and transaction value.
5. Applicable MRP declarations must comply with the relevant Legal Metrology rules; they are inclusive of all taxes where the law requires and cannot be exceeded for covered packaged retail commodities.
6. Tax Inclusive/Exclusive is a Price Tax Basis, not a tax-rate or HSN/SAC master and not a tax determination.
7. Sales, Purchase, Inventory, GST/Tax, and Accounting retain their independent responsibilities and source records.
8. Price changes must preserve prior versions, Effective Time, Recorded Time, source, reason, actor, and approval evidence where applicable.
9. Accepted transaction Price Snapshots are immutable; later price changes cannot rewrite completed commercial transactions.
10. Cost and margin data must be protected by explicit least-privilege permissions.
11. Price List inheritance or sharing must be explicitly authorized and must not bypass Tenant/Data Isolation.
12. Price selection, conflict handling, rounding, and adjustment behaviour must be deterministic and auditable.
13. All monetary records operate in INR under the approved India-only/INR-only Global Business Settings; no lower-level setting may introduce another currency.



## 6H. HSN/SAC Classification, GST Rate Applicability, and Tax Determination

### 6H.1 Purpose and responsibility boundaries

CBM must manage tax classification, GST rate rules, and transaction tax determination as related but distinct responsibilities and records. The following must not be reduced to one HSN/SAC text field or one GST percentage stored on a Product/Variant:

1. The official HSN/SAC Code Registry.
2. The effective-dated HSN/SAC Classification Assignment connecting an eligible goods/service subject to a code.
3. GST Rate Applicability Rules and their statutory conditions, sources, and effective versions.
4. GST Tax Determination for a particular supply/transaction context.
5. The immutable Tax Determination Snapshot preserved by an accepted transaction.

The GST/Tax domain is the authoritative owner of the HSN/SAC Code Registry, classification-assignment rules, GST Rate Applicability Rules, tax determination, and its statutory tax records. GST/Tax is also a finalized independent Business Module, as expressly classified in Section 6I.5. Its Module classification does not merge tax records with the originating Sales, Purchase, POS, Payroll, Product/Item, Inventory, or Accounting records; those ownership boundaries remain separate.

Product/Item Management owns Product/Variant identity, specifications, and applicable product attributes. It must reference the relevant HSN/SAC Classification Assignment for a goods Variant without owning or redefining the tax code or rate. A Price Record owns only its declared Price Type and Price Tax Basis. Sales, Purchase, POS, Inventory, and Accounting consume tax references or results through explicit contracts and retain their own authoritative transaction, stock, cost, or accounting records.

### 6H.2 HSN/SAC Code Registry and mandatory minimum length

The GST/Tax domain must maintain a governed HSN/SAC Code Registry. Each code record must identify, as applicable:

- Code Type: HSN for goods or SAC for services.
- The exact full official code value and its digit length.
- The official description and applicable classification/tariff hierarchy.
- The issuing/statutory authority and authoritative source/reference.
- The classification/tariff edition or source version and applicable effective period.
- Current status, supersession or retirement reference, and provenance.
- Any official explanatory notes, exclusions, or source metadata required to interpret the code.

HSN and SAC are distinct code systems for their respective supply subjects. A service classification must not be treated as a goods HSN merely because the service is sold through a Product catalogue. The code value must be stored as a string, not as a numeric amount, so significant characters and leading zeroes are preserved.

**CBM's mandatory minimum code length is six digits.** Every HSN/SAC code assigned to a CBM goods Variant or service supply subject, selected for CBM GST Tax Determination, or maintained as an assignable CBM code must contain at least six digits. A four-digit, two-digit, or other shorter code must not be accepted as an assignable HSN/SAC code in CBM. This six-digit minimum is a non-configurable, non-customizable, non-overridable Stable Core integrity rule. No business, User, Service Provider, configuration, import, integration, or lower-level policy may reduce it.

The minimum-length rule does not authorize CBM to fabricate a longer code. A valid eight-digit or other longer official code must be preserved in its exact full form. CBM must not truncate the last digits of a valid longer code to create a shorter code, append zeroes, pad a code, or invent a code/description that is not an officially valid classification. Where no valid official code of at least six digits can be established for the relevant subject, the assignment must remain unresolved and be routed for authorized tax-classification review; the system must not guess or silently substitute a shorter code.

The six-digit minimum is an internal CBM Stable Core requirement and must not be represented as a universal statement of the minimum digit count required by every GST reporting circumstance. The applicable statutory reporting, invoice, e-invoice, e-way bill, and filing requirements must be independently checked against the current law and portal rules. Such legal verification must not weaken the CBM internal assignment invariant or justify truncating an official code.

### 6H.3 Variant and service classification assignments

For goods, Product Variant is the normal and primary subject to which the applicable HSN/SAC Classification Assignment is linked. The assignment must reference a specific Registry code/version; it must not copy the code as an uncontrolled free-text value. It must preserve the owning Business Context, source/evidence, classification rationale or relevant product facts where required, effective time, recorded time, status, actor, and approval/review reference where applicable.

A Product Definition-level default may be used only where the GST/Tax domain's governed rule explicitly establishes that the classification applies to all eligible Variants under that Product. A Variant-level classification must take precedence where its composition, material, function, processing, use, specification, packaging condition, or another legally relevant characteristic changes the classification or applicable treatment. Inheritance must be explicit, validated, effective-dated, and auditable; similarity of Product names or category alone is not sufficient evidence.

Where classification can legitimately vary by a legally relevant supply context or condition, the model may preserve multiple scoped/effective assignments or a governed conditional classification rule. There must be one unambiguous applicable result for a given determination context. Conflicting or incomplete assignments must produce an explicit exception, not an arbitrary selection.

For services, SAC must be assigned to the eligible Service Offering or other authoritative service/supply subject defined by the responsible service domain. CBM must not force a SAC code onto an unrelated physical-goods Variant, nor force every service into a goods Product hierarchy. The relevant source domain supplies the service reference; GST/Tax owns the code assignment and tax interpretation.

HSN/SAC Classification Assignment is not a Product Attribute, Product Family, Product Category, SKU, GTIN, PLU, Product Code, or barcode. A Product taxonomy may coexist with a tax classification, but the two answer different questions and have separate owners, rules, and histories.

### 6H.4 GST Rate Applicability Rules and effective-dated rate management

GST Rate management must be separate from HSN/SAC Code Registry management. An HSN/SAC code is a classification reference; it is not itself a permanent GST rate. CBM must not assume a one-to-one, one-rate-for-ever relationship between one code and one GST rate.

A GST Rate Applicability Rule must be able to represent the legally relevant tariff/code scope, goods/service description, required product or supply characteristics, conditions, exclusions, exemptions or other statutory treatments, rate components, effective dates, statutory source, publication/notification references, and the rule version. As applicable, rate components may include CGST, SGST/UTGST, IGST, Compensation Cess, or another legally applicable component. The model must preserve the statutory distinction between nil-rated, exempt, zero-rated, non-taxable, and other treatments; it must not collapse legally different treatments into an unexplained numeric zero.

A rate rule may apply to a specific full tariff item, sub-heading, heading, chapter, service heading, or other legally defined scope. Where an official schedule associates a code/heading with a description, condition, package status, or exception, those qualifications are part of the rule and must be retained. The same code or tariff scope may therefore participate in more than one rate rule where the official schedule distinguishes goods descriptions or conditions. Code-only matching is insufficient where the law requires additional facts.

Rate Rules must be effective-dated and versioned. CBM must preserve the date from which a rule legally applies, its publication/notification date and source, the date CBM recorded/imported it, and its approval/validation status for use. A future notified rate change may be loaded and scheduled without prematurely changing the active determination. A superseded, withdrawn, or corrected rule remains historically retrievable. A late-recorded or backdated notification must not silently rewrite accepted transactions.

The applicability and priority of rate rules must be deterministic. Where multiple rules appear to match, the Tax domain must resolve them through their legally defined scope and specificity. If the available facts do not establish one valid rule, CBM must report an exception and must not choose a convenient or lowest/highest rate automatically.

### 6H.5 GST Tax Determination and price interaction

GST Tax Determination must consume the relevant HSN/SAC Classification Assignment and GST Rate Applicability Rule through explicit versioned references. It must also evaluate the transaction/supply facts required by applicable law, which may include the nature and description of the supply, relevant product/service characteristics, statutory conditions or exemptions, time-of-supply/effective legal timing, transaction type, place of supply, intra-State/inter-State treatment, applicable GST Registration context, taxable-value rules, discounts or other statutory adjustments, and Price Tax Basis. This list is illustrative and does not replace the applicable law.

The same Product Variant and its classification reference may be used in different transactions, but the resulting tax determination may differ where the lawful transaction context or effective rule differs. The HSN/SAC code alone must never be treated as the complete transaction tax calculation.

Tax Determination must return or preserve the resolved classification and assignment reference, selected rate rule/version, applicable component rates, taxable value, tax components and amounts, statutory treatment, relevant legal source, effective time, and an explanation/provenance sufficient to reproduce and audit the result. Missing classification, incomplete conditions, conflicting rules, invalid code length, or insufficient transaction facts must be surfaced as an exception; CBM must not fabricate or infer a tax rate from a Product name, prior transaction, supplier record, or display value.

The GST/Tax domain applies the Price Tax Basis established by the relevant Price Record or transaction source. Purchase price references should normally be tax-exclusive, but the actual source quotation, purchase order, invoice, or accepted transaction must state and preserve its real basis. Sales Price Records may be tax-inclusive or tax-exclusive. Inclusive-price extraction and exclusive-price tax addition must follow the applicable statutory valuation provisions and the Price Architecture in Section 6G; no generic arithmetic formula replaces statutory valuation rules. Purchase input-tax credit eligibility and Inventory cost treatment remain separate legal/domain determinations and must not be inferred solely from the Purchase Price Tax Basis.

### 6H.6 Classification and rate history

The following History record sets must be separately maintained within the GST/Tax domain:

1. **HSN/SAC Registry History:** creation, official-source updates, description changes, hierarchy/version changes, correction, supersession, retirement, and validity changes for a code record.
2. **Product/Service Tax Classification Assignment History:** assignment, reassignment, effective-date change, scope change, review, correction, approval, and supersession linking a goods Variant or service/supply subject to an HSN/SAC code.
3. **GST Rate Rule History:** creation/import, statutory notification, rate/component/condition changes, future scheduling, activation, correction, supersession, withdrawal, and expiry of a GST Rate Applicability Rule.

Each History must preserve the prior version and relevant code/rule references, source notification or official evidence, effective time, recorded time, publication/approval time where applicable, reason, actor, and approver. A code or rule correction must not delete the historical record that was previously treated as valid. The Domain History record sets remain distinct from the comprehensive Audit Trail under Section 17A.

### 6H.7 Immutable transaction Tax Determination Snapshot

An accepted Sales, POS, or Purchase transaction that requires GST treatment must preserve the Tax Determination Snapshot applicable to its accepted supply/line. As applicable, it must contain:

- The Product Variant or service/supply subject reference.
- The complete HSN/SAC code, Code Type, official description/source version, and Classification Assignment reference/version.
- The selected GST Rate Applicability Rule, rule version, statutory notification/source reference, and effective date.
- The relevant classification/rate conditions and determination context.
- The applicable Price Record/Price Snapshot reference and Tax Inclusive/Exclusive basis.
- Taxable value, applicable tax treatment, component rates, component amounts, rounding outcome, and accepted line/transaction totals.
- Source/provenance, determination timestamp, and any authorized override/correction reference.

The originating transaction domain owns the accepted transaction and its immutable snapshot. GST/Tax owns the source classification/rate rules and the tax determination record. A later change to a Product Variant, HSN/SAC Registry, classification assignment, GST rate rule, tax configuration, or Price Record must not silently recalculate or rewrite an already accepted transaction. Any correction, debit/credit note, return, or other adjustment must follow the owning transaction domain's authorized lifecycle and preserve its link to the original snapshot.

### 6H.8 Governance, security, configurability, and integrity

The HSN/SAC Registry and GST Rate Applicability Rules must be maintained from authoritative statutory sources, including relevant CBIC notifications/schedules and official GSTN/CBIC code directories or guidance, as applicable. Imported reference data must preserve source, source version, import time, effective time, validation result, and approval/publication status. External feeds or vendor-supplied tax tables are not authoritative merely because they are available or current-looking. A source update must not automatically reclassify existing Product Variants or silently replace a Tax Determination Rule without governed validation and effective-date handling.

The GST/Tax domain may configure legally permitted rate conditions, applicability dimensions, review/approval workflows, publication timing, jurisdictional/business context, and authorized maintenance operations. Those options must remain bounded by the statutory source and Main Portal governance. The six-digit minimum code-length rule is not configurable or overridable. Tenant-specific tax contexts, registrations, access, and permissions must remain isolated and purpose-limited.

The architecture must support different businesses and supply contexts without forcing every business to use every code, rate rule, tax condition, service classification, or workflow. It must also support future statutory changes through versioned reference data and explicit contracts without changing the stable meaning of Product/Variant identity, price records, or historical transactions.

## 6I. Business Module Catalogue Completion and Domain Ownership Boundaries

**Decision status:** The following Business Module classifications and responsibility boundaries were finalized on 19 November 2026 under the project owner's explicit instruction to independently finalize all architecture decisions that do not require an owner-level business or product-policy choice. These decisions extend the existing Sections 6A–6H and do not supersede any previously finalized rule unless an explicit amendment is recorded.

### 6I.1 Complete Independent Business Module Catalogue

CBM's independent Business Module Catalogue contains the following fourteen Modules:

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

A Business Module is an independently governed business responsibility with its own authoritative records, domain rules, lifecycle, history, applicability, and authorization boundary. This catalogue does not make every Module mandatory for every Business Entity. Module availability and use remain subject to applicable Business Context, Main Portal governance, Subscription/Entitlement where applicable, and explicit authorization.

The catalogue is not an exhaustive list of every future capability. A future capability must be classified by its genuine business responsibility and must not be added as a separate Module merely because it has a screen, report, workflow, integration, or data table.

### 6I.2 Sales Management

Sales Management owns the customer-facing commercial sales lifecycle and its authoritative sales records, including quotations/offers, sales agreements and terms, sales orders, original Sales Invoices, Sales Returns and related commercial credit/debit documents, sales-side price lists and conditions, discounts/promotions, fulfilment commitments and sales-domain reporting.

Sales does not own Product/Item identity or declared product/packaging prices, GST classification or tax determination, stock quantities or stock movements, physical warehouse execution, payment execution/settlement, or accounting postings. It consumes the relevant Product/Item, Pricing, GST/Tax, Inventory, Warehouse, Finance/Payment, Counterparty and Accounting contracts.

The original commercial Sales Invoice and its sales lifecycle belong to Sales Management. GST/Tax owns the applicable tax determination and statutory tax records; Accounting owns the accounting interpretation, vouchers and postings; Finance & Payment owns payment execution and settlement. A shared workflow or composed screen does not merge these ownership boundaries.

### 6I.3 Purchase & Procurement Management

Purchase & Procurement Management is one independent Business Module. Sourcing, procurement evaluation, supplier selection, purchase requests/requisitions, RFQs, supplier quotations, purchase agreements, Purchase Orders, purchase-side terms, original Purchase Invoices, Purchase Returns and related commercial credit/debit documents belong to this Module. It owns supplier quotation/agreement and purchase transaction price records and commercial purchase matching.

Purchase owns the commercial facts of what was requested, ordered, invoiced, returned, and commercially received. It does not own the authoritative stock ledger, physical warehouse execution, tax determination, payment execution/settlement, or accounting postings.

The receiving boundary is explicitly divided:
- Purchase owns the purchase-order and supplier-document relationship, commercial receipt confirmation, and purchase-side discrepancy/acceptance facts.
- Warehouse owns physical receiving execution, inspection work where assigned, putaway, handling, and physical placement.
- Inventory owns accepted stock quantities, stock states, stock ledger entries, reservations, and inventory movements.
- GST/Tax owns tax determination.
- Finance & Payment owns payment execution and settlement.
- Accounting owns accounting interpretation and postings.

A Purchase receipt or supplier delivery notice does not itself increase authoritative Inventory stock. Stock changes only through the Inventory domain's validated process and explicit source references.

### 6I.4 Point of Sale (POS) Operations

POS Operations is an independent Business Module because terminal/session, cashier, till, checkout execution and POS operational lifecycle are distinct business responsibilities. It owns POS terminals/workstations, POS operator context, POS sessions and shifts, cash-drawer/till operations, opening float, cash-in/cash-out, drawer counting, checkout execution traces, POS operational exceptions, and POS session/till reconciliation.

The POS Application (web, desktop, mobile, or handheld) is an Application Client Channel and is not itself the POS Business Module.

POS is not a competing Sales Transaction or Sales Invoice master. Sales Management owns the authoritative commercial sale and original Sales Document. POS owns its terminal/session/checkout/till operational records and maintains typed references to the corresponding Sales records. POS consumes Sales pricing/commercial determination, GST/Tax determination, Inventory availability/reservation, and Finance/Payment execution through explicit contracts.

Payment execution and settlement belong to Finance & Payment. Accounting postings belong to Accounting. A completed POS checkout, provider response, or payment event does not by itself prove that the required Accounting posting has been validated and posted.

Offline POS is an optional, applicability-controlled capability, not a universal assumption. If enabled, it must provide governed transaction sequencing, duplicate prevention/idempotency, synchronization, conflict handling, reconciliation, statutory compliance checks, and traceable failure recovery before offline-originated transactions are treated as accepted.

### 6I.5 GST/Tax Management

GST/Tax Management is an independent Business Module and the authoritative owner of the GST/Tax domain, including GST Taxpayer Profile and GST Registration tax-domain records, HSN/SAC Code Registry, HSN/SAC Classification Assignments, GST Rate Applicability Rules, GST Tax Determination, tax components and taxable-value calculations, statutory tax records, GST return preparation/validation/filing records where applicable, and tax-domain history and compliance exceptions.

All finalized rules in Section 6H remain applicable, including the CBM Stable Core six-digit minimum, preservation of the full official code, separate code/assignment/rate histories, effective-dated legal rules, deterministic applicability, provenance, and immutable transaction Tax Determination Snapshots. The six-digit minimum is a CBM internal invariant and is not represented as a universal statutory minimum.

GST/Tax does not own original Sales or Purchase transactions, Product/Item identity, Inventory or Warehouse records, Payment execution, Payroll calculation, or Accounting Books and postings. GST/Tax's final classification as an independent Business Module is confirmed by this Section.

The initial statutory tax scope is GST-focused. Other tax regimes, such as TDS or Income Tax, must be added only through an explicit domain-scope decision. Payroll-related statutory deductions and payroll obligations remain owned by Payroll Management where they arise from payroll rules and employment.

### 6I.6 Finance & Payment Management

Finance & Payment Management is one independent Business Module with distinct Cash/Bank Operations and Payment Operations subdomains. It owns operational bank/cash account administration, payment methods and instruments as applicable, payment instructions and initiation, collections, disbursements, payment execution states, provider/gateway operational references and configuration, settlements, refunds, reversals, chargebacks, payment failures, provider/bank settlement evidence, payment-to-obligation application instructions, and payment/settlement reconciliation records.

Finance & Payment does not own a General Ledger or a competing authoritative Accounting balance. Accounting owns Books, receivable/payable accounting balances, authoritative accounting allocation and postings, Bank-to-Ledger reconciliation, and financial statements. Finance & Payment owns payment execution and settlement facts; Accounting records their authorized accounting effects.

Subscription/Billing for the CBM platform's own commercial relationship remains within the existing Platform Subscription/Billing domain under Section 14. A Customer Business's own collections and disbursements belong to that Customer Business's Finance & Payment Module. The two contexts must not share records or authority merely because both involve payments.

### 6I.7 Asset Management

Asset Management is an independent Business Module owning operational asset identity and lifecycle, including asset registration, classification, custody, responsible assignment, physical location, transfers, usage/status, inspection, maintenance, repair, insurance administration, retirement and physical disposal records.

Accounting Management retains ownership of Fixed Asset Accounting, including capitalization, acquisition cost accounting, depreciation, impairment, revaluation where applicable, disposal accounting, ledger treatment and financial disclosures. Purchase owns the original acquisition transaction. Inventory/Warehouse own stock and physical handling records where an asset passes through those processes. Asset Management and Accounting use explicit shared Asset Identity and Asset Event references; neither takes over the other's source records.

An inventory item does not become an Asset merely because of price or value. A controlled conversion/recognition event is required where an item is taken into use as an asset. That event must preserve the relationship between the source Inventory records and the new Asset operational record and must prevent duplicate stock/asset quantity representation.

Asset Management does not automatically include Warranty Management, Service Centre Management, Fleet Management, or a full enterprise maintenance execution suite. Those capabilities require separate applicability and scope decisions; existing explicit exclusions remain unchanged.

### 6I.8 Business Planning & Budget Management

Business Planning & Budget Management is one independent Business Module owning business plans, financial and operational budgets, departmental budgets, targets, forecasts, scenarios, allocations, approved baselines, revisions, versions, planning approvals, and planned-versus-actual planning analysis.

Budgets, forecasts, targets and plans are planning records. Their creation, approval or revision does not create a posted financial transaction, alter a General Ledger, reserve funds automatically, approve a Purchase Order, or authorize a Sales commitment unless a separate explicitly governed contract and authorization rule provides that effect.

Accounting remains the authoritative source of posted financial actuals and accounting classifications. Sales, Purchase, Inventory, HR, Payroll, Finance/Payment and other source domains remain authoritative for their operational facts. Planning consumes authorized actuals and source references through explicit contracts and owns only its planning records, versions, approval state and analytical results.

### 6I.9 Counterparty Management

Counterparty Management is an independent Business Module owning scoped Party/Counterparty identity records, party classification, Customer and Supplier role profiles, contact persons and contact details, addresses, relationship status/lifecycle, duplicate detection, controlled merge/split, provenance and counterparty-domain history.

Customer and Supplier are business roles/profiles that may coexist for the same Party. They are not inherently separate natural persons or legal entities. Role-specific commercial terms and conditions remain owned by Sales or Purchase respectively; Counterparty Management owns the underlying party identity and role/profile references.

Counterparty is not a CBM User Account, Employee, Business Identity, Legal Identity, or GST Registration. Those identities and records remain within their respective authoritative domains. A Counterparty record does not grant system access or business authority.

Counterparty records are scoped to the applicable Business/Tenant context and are not a universal cross-tenant shared master. Name, address, phone, email, tax identifier or other matching values do not grant access to another tenant's Counterparty data. Duplicate detection and merge/split require explicit scope, authorization, evidence, provenance and history. Cross-tenant sharing, if ever required, needs a separate governed consent and data-sharing architecture; it is not implied by this Module.

Counterparty Management is not automatically a full CRM, Marketing Automation, Sales Pipeline, Customer Support, or Customer Success module. Such capabilities require separately justified domain boundaries and applicability.

### 6I.10 Cross-Module Ownership and Transaction Rules

The following ownership rules apply across the Catalogue:

| Business fact / responsibility | Authoritative owner |
|---|---|
| Product and goods identity, variants, packaging and catalogue definitions | Product/Item Management |
| Product/packaging declared prices, including applicable MRP/RRP | Product/Item Management |
| Sales quotations, orders, original Sales Invoices, sales terms and selling price conditions | Sales Management |
| Supplier quotations, purchase agreements, purchase orders, original Purchase Invoices and purchase terms | Purchase & Procurement Management |
| POS terminal/session/checkout/till operational facts | POS Operations |
| Original commercial sale linked to a POS checkout | Sales Management |
| HSN/SAC Registry, assignments, GST rates and tax determination | GST/Tax Management |
| Payment initiation, execution, settlement and operational payment records | Finance & Payment Management |
| Accounting Books, vouchers, postings, balances and financial statements | Accounting Management |
| Stock quantities, stock ledger, reservations, stock movements and inventory valuation | Inventory Management |
| Warehouse facility/layout, storage locations and physical warehouse execution | Warehouse Management |
| Employee and employment records | Human Resource Management |
| Payroll rules, runs, calculations and payroll results | Payroll Management |
| Operational asset identity, custody, maintenance and lifecycle | Asset Management |
| Fixed Asset Accounting | Accounting Management |
| Plans, budgets, targets, forecasts and approved planning baselines | Business Planning & Budget Management |
| Party identity and Customer/Supplier role profiles | Counterparty Management |

A Module may request, validate, consume, reference, or receive events from another Module only through an authorized, versioned and purpose-scoped contract. Cross-Module workflows, dashboards, reports, screens, shared storage infrastructure, or event transport do not transfer source-record ownership or grant shared write access.

Source transaction state, payment state, tax determination state, inventory state, warehouse execution state, and accounting posting state are distinct. Success in one domain does not imply success in another. Each domain must expose its own status and exceptions, preserve idempotency and provenance, and support authorized reconciliation and correction without silently rewriting accepted historical facts.

### 6I.11 Shared Domain and Platform Classification

The following are not independent Business Modules merely by virtue of being shared or visible across Modules:

- Pricing Reference and Price Determination remain a Shared Domain Model/Contract under Section 6G; there is no universal Pricing Master or separate Pricing Module.
- Product/Item Reference Model remains governed by Product/Item Management and its shared contract; it is not a separate Module.
- HSN/SAC and GST Rate references remain GST/Tax-owned contracts.
- UOM, currency, address, location, party/actor references, document/source references and event/change references are shared typed models or governed reference capabilities with explicit owners and scopes, not universal shared-write masters.
- Approval execution, authorization, security, messaging, API/event transport, file/document infrastructure, audit/history infrastructure, search/reporting infrastructure and background processing remain Internal Platform Capabilities or Platform-wide Services under Section 6A and related sections.
- Web, desktop, mobile and handheld applications are Application Client Channels.
- External accounting, tax, payment, commerce, device and other connectors are External Connectivity Capabilities. External systems do not define CBM's canonical domain models.

### 6I.12 Service Offering / SAC Subject Boundary

A Service Offering or other eligible service/supply subject used for SAC classification must be supplied by its authoritative service domain. It must not be forced into a physical-goods Product Variant or the Product/Item goods hierarchy merely to satisfy GST classification. Product/Item Management's six approved canonical structures remain unchanged by this Section. No generic Service Operations or Service Centre Module is created by implication. The responsible service domain must be identified when a concrete service business capability is defined; GST/Tax continues to own SAC classification assignment and tax determination.

### 6I.13 Applicability, Configuration and Non-Override Rules

The fourteen-Module Catalogue is a responsibility classification, not a universal enablement list. Applicability must be evaluated against Business Entity type/context, legal and operational purpose, subscription/entitlement where relevant, supported capability, and authorized configuration.

Each Module may configure eligible workflows, document types, numbering, conditions, approval rules, reports, templates and extensions within protected domain invariants. Configuration or customization must not:
- transfer an authoritative record to another Module;
- create a competing source of truth;
- bypass Business/Tenant Data Isolation, Permissions, Delegated Authority or approval controls;
- alter accepted historical transactions or their snapshots;
- weaken statutory, accounting, tax, identity, security or audit integrity requirements;
- create uncontrolled circular dependencies or unrestricted cross-domain writes.

Main Portal may define protected baselines and non-overridable platform invariants. Lower-level configuration and overrides are permitted only where explicitly authorized by the relevant governance rules.

### 6I.14 Decisions Deliberately Outside This Classification

The following are product/commercial or later domain-detail decisions and are not determined by this Catalogue:
- MVP, release sequencing, rollout priorities and commercial packaging;
- Subscription Plan prices, add-on prices and entitlement bundles;
- detailed feature scope for advanced/offline POS, CRM, marketing, customer support, fleet, warranty, service centre or enterprise maintenance;
- detailed service-domain ownership for any future Service Offering that has not yet been assigned to a concrete service business domain;
- statutory implementation profiles, tax rates, retention durations and legal filing procedures, which must be validated against current authoritative requirements at the authorized implementation/compliance stage.

These matters do not prevent the fourteen-Module classification or its ownership boundaries from being authoritative.

## 7. Business Authority Scope

### 7.1 Meaning and separation

Business Authority Scope defines where a business-level authority may be exercised or made available for assignment. It does not, by itself, grant any User access or specify which actions a User may perform.

The architecture must distinguish:

- **Business Available Authority Scope:** the entities or business areas eligible to be governed by the relevant business-level authority and from which User scope may be assigned.
- **User Assigned Authority Scope:** the specific entities or business areas explicitly assigned to an individual User within a valid User–Business association.
- **Permissions:** the actions that the User may perform within the assigned scope.

These are separate controls and must be evaluated independently.

### 7.2 Main Portal scope

Main Portal's authority scope extends across the entire CBM Ecosystem without subordinate-imposed restriction. It covers all Sub-Portals, Organizations, Distributors, Shops, and applicable business/service contexts within CBM.

No Sub-Portal, Organization, Distributor, Shop, User, or Role may narrow or constrain Main Portal's supreme authority scope.

### 7.3 Sub-Portal scope

A Sub-Portal's business/service-provider authority scope extends to the businesses and operational entities associated with and served within its authorized Sub-Portal domain. This includes the relevant Organizations, their Distributor and Shop entities, and other business contexts expressly associated with that domain, subject to Main Portal governance.

This provider-level scope does not transfer ownership to the Sub-Portal and does not automatically grant every Sub-Portal User access to every associated entity or record. Each Sub-Portal User must have an explicit valid User–Business association, User Assigned Authority Scope, and required Permissions, subject to purpose limitation and Data Isolation.

### 7.4 Organization scope

An Organization's Business Available Authority Scope may include:

- The Organization's own business context.
- Distributor entities belonging to its Business Structure, grouped as its Distribution Network when multiple, where and to the extent permitted by applicable Main Portal-governed authority rules.
- Shop entities that are direct structural children of the Organization, where and to the extent expressly included by applicable Main Portal-governed authority rules. Their Owner ID sets must match the Organization's Owner ID set under Section 5.3.1; this ownership-consistency constraint does not itself assign User scope or grant every action Permission.
- An independently identified business only where a valid Ownership Link and an explicit Main Portal-governed rule make that business eligible for the relevant owner-linked authority scope.

An Organization's Business Available Authority Scope must be determined under explicit Main Portal-governed authority rules. The mandatory ownership-consistency rule in Section 5.3.1 does not itself grant every User access or every action Permission. A Distributor–Shop Mapping does not, by itself, expand the Organization's authority scope through the Distributor to a Shop belonging to another Organization. Any additional owner-linked authority must be expressly authorized under the applicable Section 7 rules and must preserve the separate Business Identities and Business Data Isolation.

### 7.5 Distributor scope

A Distributor's Business Available Authority Scope always includes the Distributor's own business context.

In addition, it may include a Shop associated with the Distributor's distribution context **only when** an Owner ID linked to that Shop through an Ownership Link is also linked through an Ownership Link to the Distributor, and the applicable Main Portal-governed rules explicitly permit that owner-linked scope.

A Distributor's scope does not include other mapped Shops merely because they participate in its Distribution Network. A mapped Shop that has no Owner ID in common with the Distributor remains outside the Distributor's ownership-based scope, even when mapped to the Distributor. Mapping enables only the expressly authorized relationship interactions; it does not create an Ownership Link or grant unrestricted administrative or data access.

Where a Distributor has multiple Owners, owner-linked eligibility must be evaluated against the explicit Ownership Links and applicable governance rules. It must not be inferred from a shared name, contact detail, User account, or informal association.

### 7.6 Shop scope

A Shop's own business scope includes its own Business Identity and business context. Its ownership and data boundary remain independent.

If the Shop shares an Owner ID with another business, that Ownership Link may be considered for an eligible owner-linked scope under the rules applicable to that other business. This does not make the Shop a structural child of that business, transfer its ownership, or merge its data boundary. Its association with a Distributor still permits only the explicitly authorized relationship interactions.

### 7.7 Scope relationships do not transfer ownership

A Business Available Authority Scope may include an entity because of a valid structural relationship or an expressly permitted owner-linked eligibility rule. Neither basis changes the entity's Owner, Ownership Links, Business Identity, or data boundary. Authority scope must never be used to infer ownership transfer or ownership inheritance.

### 7.8 Business availability does not grant User access

A business having an entity within its Business Available Authority Scope does not mean every User associated with that business may access the entity. A User receives access only to the scope explicitly assigned to that User, and only within a valid User–Business association and the applicable Permissions.

### 7.9 Explicit User scope assignment

User Assigned Authority Scope must be explicit. It may be assigned to one entity, selected multiple entities, or the full Business Available Authority Scope. A partial assignment may deliberately omit otherwise available entities. Where supported by the governing assignment rules, explicit exclusions may further narrow a selected scope.

A User's organizational level, title, Role, Owner identity, User–Business association, or access to one entity does not automatically grant access to all entities available to that business. Full available scope must be an intentional, authorized assignment.

A newly eligible or newly associated entity must not silently become accessible to existing Users. Future-entity inclusion is permitted only when an explicit, authorized assignment rule clearly defines that behaviour; otherwise a new or changed scope requires an explicit assignment.

### 7.10 Authorization checks

An action may be authorized only when all applicable conditions are satisfied:

1. Main Portal governance and Stable Core constraints permit the access.
2. The target entity is within the relevant Business Available Authority Scope.
3. The User has a valid User–Business association for the target context.
4. The target entity is within that User's explicitly assigned User Assigned Authority Scope.
5. The User has the required action Permission in the applicable business context.
6. Any required approval and other mandatory controls have been satisfied.

Failure of any required condition means the action must be denied. A Business Relationship may permit a narrowly defined interaction, but it cannot bypass these checks or expand access beyond its explicit purpose.

## 8. Delegated Authority

### 8.1 General rule

Authority may be delegated only downward, only within the delegator's own authority, and only to a permitted recipient. A delegate cannot receive more authority than the delegator is entitled to assign.

### 8.2 Required dimensions

Every Delegated Authority grant is defined by all five dimensions:

| Dimension | Required meaning |
|---|---|
| **Who** | The recipient of the delegated authority: an eligible business, user, role, or other explicitly authorized subject. |
| **What** | The category or type of authority/actions being granted. |
| **Where** | The business entities, locations, or scope in which the grant may be used. |
| **Limits** | Quantitative, qualitative, operational, or policy constraints placed on the grant. |
| **Duration** | The period for which the grant is valid, including its effective validity boundary. |

### 8.3 Delegation constraints

- A grant must be explicit and sufficiently specific to identify its recipient, actions, scope, limits, and duration.
- No implied or assumed authority exists beyond the grant.
- A delegate cannot expand the grant or create higher-level platform authority for itself.
- Main Portal may modify, restrict, suspend, withdraw, or revoke subordinate delegated authority.
- Delegation and its changes must be auditable.
- Delegated authority does not transfer business ownership or merge data boundaries.

---

## 9. Users, Roles, Permissions, and action authorization

### 9.1 Separate authorization dimensions

The CBM authorization model must answer three distinct questions:

- **Where may the business-level authority be made available?** — Business Available Authority Scope.
- **Where may this individual User act?** — User Assigned Authority Scope.
- **What may this User do there?** — Role-based or User-specific Permissions.

A Role or Permission must not expand the User's assigned business scope. Business availability must not be treated as an individual User grant.

### 9.2 User-to-business association

A User must have an explicit, valid association with the target business context before business permissions can be applied. A User's access to one business does not automatically grant access to another business, even where the same person owns both businesses.

A User's level or association with an Organization, Distributor, or other business does not automatically grant access to every subordinate or related entity. User Assigned Authority Scope must separately identify the entities or business areas that the User may access.

### 9.3 Explicit scope assignment

The system must support explicit assignment of a User Assigned Authority Scope from the relevant Business Available Authority Scope. The assignment may cover:

- One specific Business Entity or operational unit.
- A selected set of multiple entities.
- The full available scope, only when explicitly and validly assigned.
- A partial scope that intentionally excludes other available entities.
- Explicit exclusions where permitted by the governing assignment model.

The assignment must not exceed the assigner's own authority or the Business Available Authority Scope available to the assigning business. A User, Role, or lower-level administrator cannot assign a scope that they are not authorized to grant. Scope assignments, changes, and revocations must be auditable.

### 9.4 Role-based and User-specific permissions

Permissions must support assignment through Roles and direct User-specific rules. This allows different Users in the same business, or Users with different Roles, to perform different actions within their respective User Assigned Authority Scopes.

Role assignment does not itself grant access to every entity available to the business. A Role's action permissions operate only within the User's valid assigned scope and business context.

### 9.5 Action-oriented permission types

Permissions represent actions, not business areas. Common action types include:

- View/read.
- Create.
- Update/edit.
- Delete.
- Approve.
- Reject.
- Export.
- Configure/override, where explicitly permitted.
- Other distinct operations needed by CBM modules.

A permission to perform one action does not imply permission to perform another action.

### 9.6 Example: different User scopes within one Organization

Assume Organization A's Business Available Authority Scope may include Distributor D1 and Shops S1, S2, and S3 where permitted by the applicable authority rules.

- User U1 is associated with Organization A and is assigned only Organization A and Shop S1. U1 cannot access D1, S2, or S3 merely because U1 is an Organization-level User.
- User U2 is associated with Organization A and is assigned Distributor D1 and Shop S2. U2 cannot access S1 or S3 unless those entities are also assigned.
- User U3 is explicitly assigned the full available scope and may access D1, S1, S2, and S3, subject to Permissions and all other controls.
- Shops mapped to D1 that have no Owner ID in common with D1 are not included in Organization A's scope merely because of the mapping, and cannot be added to these Users' assignments on that basis.

The same Role may therefore operate over different entity sets for different Users. The assigned scope and action Permissions must both be satisfied.

### 9.7 Scope assignment governance

The authority to create, modify, or revoke a User Assigned Authority Scope is itself subject to explicit Permission and Delegated Authority. An assigning User may grant only scope that is available to the assigning business and within the assigner's own authority. Scope assignment must not permit privilege escalation, upward authority, cross-business data access, or bypass of Main Portal governance.

Scope assignments and their effective changes must be auditable. Revoking or narrowing a scope must prevent subsequent access outside the remaining assignment, subject to the platform's applicable security and audit requirements.

## 10. Configuration governance: defaults, inheritance, and overrides

### 10.1 Governing principle

Configuration is hierarchical and must remain subordinate to Main Portal governance and Stable Core boundaries. Each business may maintain independent effective configuration within the authority and override permissions granted to it.

### 10.2 System-defined non-overridable bounds

Where a configuration has system-defined minimum and maximum limits or another immutable allowed-value boundary, those limits are non-overridable. No business-level configuration or delegated permission may bypass them.

These limits protect the system's integrity and remain above all subordinate configuration choices.

### 10.3 Default values

A higher-level portal/business may establish a default value within the system-defined bounds. That default applies downward to subordinate businesses that do not have a valid permitted override.

### 10.4 Inheritance

A subordinate business inherits the applicable effective configuration from its governing higher level unless a valid override applies within that subordinate business's own scope.

The inheritance model is top-down. A lower level cannot force a configuration value upward onto a higher level.

### 10.5 Business-specific override permission

A business may change an inherited/default value only when it has explicit permission to override that specific configuration. Override permission may be granted to one business and withheld from another business at the same or a different level.

### 10.6 Override constraints

- An override must remain within all system-defined minimum/maximum and allowed-value boundaries.
- Override authority must be explicit and independently enforceable.
- An override applies only within the business context for which it is authorized; it must not silently change a sibling business's configuration.
- A business without override permission must use the applicable inherited value.
- Configuration changes and their authorizing permissions must be auditable.

### 10.7 Example

Assume a setting has a system-defined range of 1 through 100 and Main Portal sets the default to 50. A Sub-Portal or Organization may use 50 by inheritance. If a particular Organization receives explicit override permission, it may set its own value to 70. Another Organization without that permission continues to use its inherited value. No override may set the value below 1 or above 100.

---

## 11. Approval Workflow governance

### 11.1 Approval is a separate control

For an action, the system must be able to determine whether approval is required, the required approval pattern, and the eligible approver(s). Approval requirements are configurable separately from the underlying action Permission.

A User who lacks permission to initiate an action does not gain that permission merely because an approver could approve it. Where an action requires approval, the action is not finalized until the required approval condition is satisfied.

### 11.2 Approval requirement conditions

Approval configuration must support rules that vary by:

- Business.
- Action/operation.
- Role.
- Individual User.
- Eligible approver User(s) or Role(s).

The same action may require approval for one Role but not another. It may require approval for one specific User but not another. Each business must be able to configure its own approval requirements within the authority and governance rules applicable to it.

### 11.3 Approval patterns

The approval model must support, in combination as configured:

1. **Single approval** — one authorized approval is required.
2. **Multiple approvals** — more than one authorized approval is required.
3. **Sequential approval** — approvals occur in a defined order, with a later stage dependent on the earlier stage as configured.
4. **Parallel approval** — multiple approvers may act at the same time.

The model must be configurable to express the required approver count and approval combination for the business action.

### 11.4 Fine-grained combinations

Approval rules must support highly granular combinations so that different Users and Roles can have different approval requirements for the same action. Approval settings must be independently configurable for each business, subject to higher-level mandatory controls.

### 11.5 Approval and audit

Approval requests, approver identity, decision, decision time, approval stage, and final outcome must be recorded in the comprehensive Audit Trail. A separate Approval History may be introduced only if a distinct business need is explicitly approved; it must remain separate from the Audit Trail.

---

## 12. Distributor–Shop business relationships and mapping

### 12.1 Relationship model

Distributor–Shop Mapping is a Business Relationship between a Distributor and a Shop, not a structural Parent–Child relationship or ownership hierarchy. The Shop remains a direct structural child of its Organization. The Mapping is independent of both parties' ownership and does not merge their data contexts.

The CBM Ecosystem must support authorized Distributor–Shop business mappings across the ecosystem. A Distributor must belong to an Organization's business structure and cannot exist as a standalone entity without an Organization. The Organization may receive CBM services directly from Main Portal or through a Sub-Portal; that service-provider relationship does not change the Distributor's structural placement. A Shop's Mapping to a Distributor is a separate Business Relationship, not a change to either entity's structural parent.

### 12.2 Cardinality

- One Distributor may be associated with multiple Shops.
- One Shop may be associated with multiple Distributors.
- Each Distributor–Shop Mapping is an independently identifiable relationship with its own lifecycle and its own Distributor–Shop Mapping History. Related system events are separately recorded in the Audit Trail; Mapping History and Audit Trail are not combined.

### 12.3 Two-way mapping initiation and confirmation

A Distributor–Shop Mapping may be established only between two existing, eligible CBM business entities. Both the Distributor and the Shop must already have their own CBM Business Identity and unique CBM identifier. An external business or an external identifier alone cannot be used to create this CBM mapping.

The mapping requires independent confirmation by both parties through two separate Secret Codes:

1. **Shop initiation:** An authorized Shop User generates a Shop Secret Code in the Shop's CBM business context. The Shop shares its Shop Unique ID and this Shop-generated Secret Code with the intended Distributor.
2. **Distributor's first verification:** An authorized Distributor User enters the Shop Unique ID and Shop Secret Code in the Distributor's CBM business context. CBM verifies that the identified Shop exists, is eligible for the requested relationship, and that the submitted code is valid and usable for this pending mapping.
3. **Distributor confirmation:** Only after the first verification succeeds, an authorized Distributor User generates a separate Distributor Confirmation Code for that specific pending mapping. The Distributor shares this second code with the Shop.
4. **Shop's final verification:** An authorized Shop User enters the Distributor Confirmation Code in the Shop's CBM business context. CBM verifies that the code belongs to the intended Distributor and the corresponding pending mapping, and that it remains valid and usable.
5. **Activation:** The mapping becomes Active only after both the Shop-originated code and the Distributor-originated confirmation code have been successfully verified. Completion of only the first verification must never activate the mapping.

The two codes serve different confirmation stages and must not be treated as interchangeable. The verification and confirmation actions remain subject to the applicable User–Business association, User Assigned Authority Scope, Permissions, and other mandatory authorization controls. Parent Organization selection or disclosure for the other party is not part of this process.

### 12.4 Unique IDs and Secret Codes

- The **Shop Unique ID** identifies the Shop involved in the mapping request. It is an identifier, not an authentication credential or grant of access.
- The **Shop Secret Code** is generated by the Shop and is used by the Distributor for the first verification stage.
- The **Distributor Unique ID** identifies the Distributor involved in the mapping. It is an identifier, not an authentication credential or grant of access.
- The **Distributor Confirmation Code** is independently generated by the Distributor after successful first-stage verification and is used by the Shop for final confirmation.
- Each Secret Code is associated with its intended business, verification stage, and pending mapping. A code for one stage or mapping must not be accepted as confirmation for another stage or mapping.

Knowing a Unique ID or Secret Code does not grant general access to either business or its data.

### 12.5 Mandatory Secret Code security controls

Both the Shop Secret Code and the Distributor Confirmation Code must be subject to all of the following controls:

- **Code Expiry:** Each code must become invalid after its defined validity period.
- **One-Time Use:** A code may be successfully used only once. A successfully consumed code must not be reusable.
- **Rate Limiting:** Code generation and code-verification attempts must be rate-limited to prevent abuse, guessing, and repeated unauthorized attempts. Applicable limits must be governed by the platform's security controls.
- **Revocation:** A code must be capable of being revoked before successful use. A revoked code must immediately become invalid and must not complete its verification stage.

An expired, consumed, or revoked code cannot be used to complete the pending mapping. If a required code is no longer valid, the affected verification stage must not succeed; the mapping must remain non-Active until a valid, authorized confirmation process is completed. The exact validity periods and rate-limit thresholds are security-policy values, not fixed numeric values established by this business decision.

### 12.6 Relationship effects and limits

An Active Distributor–Shop Mapping:

- Establishes only the authorized Business Relationship of the relevant Mapping Type.
- Permits only the business transactions and necessary information exchange expressly authorized for that Mapping Type and applicable business rules.
- Does not change either party's ownership or create an Ownership Link.
- Does not change either party's Structural Membership or Parent Organization.
- Does not require either party to select, view, or disclose the other party's Parent Organization. Parent Organization information is not made available to the mapping counterparty merely because the Mapping is Active.
- Does not convert a Shop into a Distributor-owned child business.
- Does not, by itself or solely because of the mapping, expand the Distributor's Business Available Authority Scope to include the Shop. Any separate owner-linked eligibility must be determined under Section 7.
- Does not, by itself or solely because of the mapping, expand the Organization's Business Available Authority Scope through the Distributor to the Shop. Any separate owner-linked eligibility must be determined under Section 7 and must not be inferred from the mapping.
- Does not grant unrestricted access to Accounting, Inventory, HR, Users, administration, or any other business data. Only information necessary for the expressly authorized relationship interactions may be exchanged, subject to the applicable authority, scope, permission, and data-isolation rules.
- Does not merge the parties' Business Identities, Ownership Contexts, or Business Data Boundaries.

The two-way code verification confirms the participating businesses' intent to establish the mapping; it does not itself grant broad administrative authority. The exact transactions and information permitted remain governed by the applicable Mapping Type, Main Portal-governed business rules, and authorization controls.

### 12.7 Ending a mapping

Ending one Distributor–Shop Mapping ends that relationship only. It does not close the Shop, change either party's ownership, invalidate either party's Business Identity, change either party's Parent Organization, or terminate the Shop's other Distributor relationships.
---

## 12A. GST Taxpayer Profile and GST Registration Architecture

### 12A.1 Purpose and separation

CBM must support configurable management of GST Taxpayer Profiles, GST Registrations, GSTINs, registered Places of Business, and explicit associations of registrations with eligible CBM entities and transaction contexts.

Subject to applicable law and verified taxpayer facts, the architecture must support one taxpayer profile with one or multiple registrations; one registration associated with multiple eligible CBM entities or contexts of the same registered taxpayer; and one CBM entity/context associated with multiple registrations where legally required or permitted. These are legally constrained capabilities, not unrestricted many-to-many links.

GST Taxpayer Profile, GST Registration, GST Registration Assignment, Business Identity, Owner ID, Business Structure, Business Relationship Mapping, Business Available Authority Scope, User Scope Mapping, and User Permissions are separate concepts. A relationship in one model does not automatically establish a relationship or permission in another.

### 12A.2 GST Taxpayer Profile and GST Registration

A GST Taxpayer Profile represents the actual person or legal entity registered, or intended to be registered, under GST. It is a tax-domain representation of the taxpayer's Legal Identity, not the Professional Identity of a Shop, branch, brand, or portal presentation. It is not an Owner ID, User account, Business Identity, or CBM entity merely because that entity has a name or owner. The legal person operating Main Portal or a Sub-Portal may have its own profile for its own taxable activities. A customer Organization or other business may have a separate profile. Multiple distinct Professional Identities, such as several Shops, may be associated with the same taxpayer profile and applicable GST Registration(s) where verified legal and GST facts permit. A service-provider relationship does not merge taxpayer identities.

Each GST Registration is an independently identifiable record supporting, as applicable, GSTIN, linked taxpayer profile, State/Union Territory and jurisdiction, legal/trade name, taxpayer type, official status, effective dates, cancellation/suspension dates, Principal and Additional Places of Business, registration certificate, verification source/result/date, and other statutory particulars. GSTIN must be unique within CBM's GST Registration records. Official status is stored separately from CBM's internal verification or assignment workflow status.

A taxpayer profile may group multiple registrations for administration, but that grouping must not merge registrations or suppress statutory consequences. Sections 25(4) and 25(5) of the CGST Act treat registrations/establishments covered by those provisions as distinct persons/establishments for GST purposes. CBM must preserve registration-level separation.

### 12A.3 GST Registration Assignment across CBM levels

A **GST Registration Assignment** is the explicit, typed association of a GST Registration with an eligible CBM entity, operational unit, location, or transaction context for a stated GST purpose.

The target is not restricted to Distributor or Shop. Subject to Main Portal applicability rules and legal validation, this capability may be enabled for any supported CBM level/context, including:
- Main Portal / the legal person operating the Main Portal service-provider business.
- Sub-Portal / the legal person operating that Sub-Portal service-provider business.
- Organization.
- Distributor.
- Shop.
- Shop or another operational location.
- Any future CBM entity/context expressly enabled by Main Portal.

This list is illustrative, not restrictive, and does not mean every registration may be assigned to every target. Each assignment must be supported by the registered taxpayer's legal identity, lawful business/place-of-business coverage, intended purpose, applicable GST law, and verified registration particulars.

A Main Portal or Sub-Portal GST Registration belongs to the legal person operating that service-provider business for its own taxable activities. It does not become the GSTIN of customer Organizations or their Users. A customer's GST Registration does not become the GSTIN of its Service Provider.

GST Registration Assignment must not be inferred from Owner ID/Ownership Link, Business Structure, parent–child placement, any Business Relationship or Mapping (including Distributor–Shop Mapping), service-provider association, shared User accounts, Business Available Authority Scope, User Scope Mapping, or any other association that does not itself establish GST eligibility. No Mapping type is a shortcut for GSTIN eligibility. Distributor–Shop Mapping has no authority to authorize GSTIN use.

A GST Registration Assignment does not transfer the GST registration, change the registered taxpayer, change Business Identity or ownership, modify Business Structure, create another Business Relationship, widen authority scope, or grant Users permission to view or use the GSTIN.

### 12A.4 Places of Business, cardinality, and effective dates

For each GST Registration, CBM must represent the Principal Place of Business and Additional Places of Business as recorded by the GST authority. CBM operational locations may be associated with a GST-registered Place of Business only through an explicit, verified location association. A CBM location or Mapping does not itself make a location a GST-registered Place of Business.

Subject to legal eligibility, one taxpayer profile may have multiple registrations; one registration may be assigned to multiple eligible entities/locations/contexts of the same registered taxpayer; and one CBM entity/context may be associated with multiple registrations where State, location, or separate-registration requirements make this valid. A parent-level assignment does not automatically flow to subordinate entities or locations. Any permitted inheritance must be explicit, legally valid, and separately governed.

The official registration effective dates/status period and the CBM assignment effective period are separate. Each assignment must support effective from/to, status, target context and purpose, linked registration, verification result/source, reason, supporting reference, recorded date/actor, and approval. Conflicts must be detected; a new assignment must not overwrite or erase a previous assignment.

### 12A.5 GSTIN eligibility, verification, and selection

GSTIN eligibility must be determined independently for each proposed GST Registration Assignment. Evaluation must consider the actual registered taxpayer, authoritative registration particulars, relevant State/Union Territory, registered Places of Business, target CBM entity/context, intended GST purpose, effective period, and applicable law.

Owner ID, shared ownership, Distributor–Shop Mapping, any other Mapping, hierarchy, or service-provider association must not independently establish eligibility. Verification must use authoritative GST sources, including GST Portal taxpayer search and official registration evidence, and record source, result, time, and responsible process/User. A user-entered GSTIN is not automatically verified.

GSTIN selection must consider the CBM entity and transaction context, validated assignment, registered Places of Business, State/Union Territory, effective period, official status, and applicable GST rules. A configured default must not override statutory requirements. A GSTIN assigned at one CBM level must not automatically be used by another. If the correct registration cannot be determined, CBM must require authorized resolution or block the GST-sensitive transaction. The GST Registration used on an issued GST document must be preserved; later assignment changes must not rewrite historical transactions.

### 12A.6 User access and configurability

A GST Registration Assignment does not grant every User of that entity access to the GSTIN. User access requires a valid User–Business association, explicit User Scope Mapping covering the relevant entity/context, the specific GST Permission, and applicable approval.

Viewing taxpayer profiles, viewing registrations, amending records, verifying GSTIN, creating/changing assignments, selecting GSTIN for transactions, and viewing GST Registration History must be independently permissionable. Main Portal governs eligible entity types/contexts, mandatory eligibility and verification controls, permitted assignment patterns, effective-date rules, and approvals. Configuration cannot waive legal eligibility or make one taxpayer's GSTIN available to an unrelated taxpayer.

### 12A.7 GST Registration History

GST Registration History is a dedicated Domain History record set containing only GST Registration and GST Registration Assignment events. It records, as applicable, affected entity/context and registration, previous and new GSTIN/assignment, effective date/time, recorded date/time, reason, actor, approver, and supporting verification/document reference.

It must not contain unrelated events such as User ID or User profile changes. Such events belong in the comprehensive Audit Trail and in a separate User History only if a distinct business need is approved. GST Registration History is preserved throughout the lifetime of the relevant business context and may be removed only as part of authorized permanent deletion of that business. Multiple appropriately authorized Users may view it under separate History permissions. It is not the Audit Trail.

### 12A.8 GST legal and policy boundary

CBM's GST capability supports information, configuration, validation, and transactions. It does not itself grant GST registration, conclusively decide legal eligibility, or replace the GST Portal, statutory filings, or professional tax advice. The design baseline must account for CGST Act Sections 25(1), 25(2), 25(4), and 25(5), and applicable CGST Rules governing separate registration for multiple places of business within a State/Union Territory. Current law and official GST Portal procedures must be rechecked before implementation.

Official references:
- [CGST Act, Section 25 — CBIC Tax Information Portal](https://taxinformation.cbic.gov.in/content-page/explore-act/1000294/1000001).
- [CBIC consolidated CGST Rules, including Rule 11 amendment for multiple places of business](https://cbic-gst.gov.in/pdf/amended-01012022-CGST-Rules-2017-Part-A.pdf).
- [GST Portal Manual — Search Taxpayer using GSTIN/UIN](https://tutorial.gst.gov.in/userguide/taxpayersdashboard/Search_Taxpayer_manual.htm).
- [GST Portal Manual — Apply for Registration](https://tutorial.gst.gov.in/userguide/registration/Apply_for_Registration_Normal_Taxpayer.htm).

---

## 13. Business Entity Lifecycle and State Management

### 13.1 Scope and separation

Business Entity Lifecycle governs the operational existence and state transitions of Organizations, Distributors, and Shops. The Parent–Child cascade rules in this section apply only to the Customer Business Structure relationship Organization → Distributor / Shop. They do not apply to the Main Portal → Sub-Portal Service Provider relationship or to Business Mappings.

The following lifecycles are separate but explicitly related where a rule defines an effect:

- Business Entity Lifecycle.
- Structural Parent–Child Cascade Lifecycle.
- Business Relationship / Mapping Lifecycle.
- Ownership Link Lifecycle.
- Subscription Lifecycle.
- User Account Lifecycle.
- User–Business Association Lifecycle.
- Authentication Credential / Authenticator Lifecycle.
- Archival Lifecycle.

A change in one lifecycle must not silently change another. A defined cascade is permitted only where this section expressly states it.

### 13.2 Business Entity operational states

The common operational states are:

| State | Meaning |
|---|---|
| Draft | Entity creation has begun, but required creation or validation conditions are not complete. |
| Pending Verification | A required verification is outstanding. |
| Active | Entity is operational and may participate in applicable activities, subject to Subscription, authority, authorization, and other rules. |
| Suspended | Entity is temporarily restricted under an authorized suspension operation. |
| Deactivated | Entity's regular operation has been disabled. Reactivation may be permitted through an authorized operation. |
| Closed | The entity has been formally closed. It is terminal for normal operations and cannot be ordinarily reactivated. |

Not every entity must pass through every state. Pending Verification applies only where a verification is required. A Distributor or Shop must have exactly one Parent Organization from creation as already established in Sections 3 and 5; it must not remain unassigned while in Draft or any later state.

Archived is not a Business Entity operational state. Archival is a separate preservation lifecycle governed by Section 17. A Closed entity may have its records and files archived without replacing its Closed operational state.

### 13.3 State transition controls

Every state transition must be explicitly authorized and satisfy the applicable Main Portal governance, policy, validation, approval, and operational preconditions.

The common transition model is:

- Draft → Pending Verification, when verification is required.
- Draft → Active, when no separate verification is required and all creation conditions are satisfied.
- Pending Verification → Active, after successful verification and all other activation conditions are met.
- Active → Suspended, Deactivated, or Closed, through the applicable authorized operation.
- Suspended → Active, Deactivated, or Closed, through an authorized operation.
- Deactivated → Active or Closed, through an authorized operation.
- Closed → no ordinary reactivation. Any exceptional correction or reopening requires an explicit Main Portal-authorized exceptional process, a recorded reason, and preserved history; it must not be treated as routine reactivation.

A state change must record its reason, actor or system process, relevant authorization and approval, effective time, recorded time, and Lifecycle Operation reference.

### 13.4 Parent Organization suspension and deactivation cascades

Within the Customer Business Structure, an Organization is the direct Parent of its Distributors and Shops. The following cascade behaviour is mandatory:

**Organization Suspended:**
- Every direct child Distributor and Shop that is Active must become Suspended by the same Parent-Caused Lifecycle Operation.
- A child that was already Suspended remains Suspended; its pre-existing suspension is not treated as newly caused by the Parent operation.
- A child that was already Deactivated or Closed remains in that state.
- Every Active Distributor–Shop Mapping involving a child that is suspended by this operation becomes Suspended.
- The other endpoint of a cross-Organization Mapping is not itself suspended merely because its counterparty's Parent Organization is suspended.

**Organization Deactivated:**
- Every direct child Distributor and Shop that is not already Deactivated or Closed must become Deactivated by the same Parent-Caused Lifecycle Operation. This includes a child that was Active or Suspended before the Parent operation.
- A child already Deactivated or Closed remains in its pre-existing state and must not be treated as changed by the Parent operation.
- Every Distributor–Shop Mapping involving a child that is deactivated by this operation and that is not already Deactivated or Terminated must become Deactivated.
- If a Mapping connects to an endpoint under another Organization, only the Mapping relationship changes. The remote Distributor or Shop and its Parent Organization remain unaffected.

Parent suspension or deactivation does not transfer ownership, change Legal Identity or Professional Identity, terminate unrelated business entities, or merge data boundaries.

### 13.5 Child suspension or deactivation

A Distributor or Shop being Suspended or Deactivated must not change the State of its Parent Organization or any sibling Distributor or Shop.

When a Distributor or Shop is Suspended, its Active Distributor–Shop Mappings become Suspended. When a Distributor or Shop is Deactivated, its Mappings that are not already Deactivated or Terminated become Deactivated. These relationship changes affect only the mappings involving that particular endpoint. Other mappings of the remote endpoint remain unchanged.

Examples:

- Deactivating Distributor D1 does not deactivate its Parent Organization or any Shop. Only D1 and the Mappings involving D1 are affected.
- Deactivating Shop S1 does not deactivate its Parent Organization, any Distributor, or another Shop. Only S1 and the Mappings involving S1 are affected.
- Deactivating one Shop does not deactivate another Shop merely because they share the same Parent Organization.

### 13.6 Lifecycle Operation Snapshots

Before a Parent-Caused Cascade or another multi-entity Lifecycle Operation changes any state, CBM must capture an immutable Lifecycle Operation Snapshot.

The snapshot must record, as applicable:

- Unique Lifecycle Operation ID and Snapshot ID.
- Parent Organization and every affected direct child entity.
- Each entity's exact pre-operation state.
- Each affected Distributor–Shop Mapping and its exact pre-operation state.
- Parent identifiers and relevant structural relationships.
- Owner ID sets required to verify ownership consistency.
- Which entities and relationships were actually changed by the operation and which were already in their prior state.
- The operation type, reason, initiating actor/system process, approval reference, effective time, recorded time, and state/version references required to prevent an obsolete restoration.

The snapshot is a state record for a particular operation. It is not a substitute for a complete database backup, Domain History, or Audit Trail. It must be immutable and retained according to the approved Business Entity Lifecycle History and archival rules.

### 13.7 Reactivation and exact restoration

When an Organization is reactivated after a Parent-Caused Suspension or Deactivation, CBM must use the snapshot associated with that exact Parent operation.

- Restore only the child entities and relationships that were actually changed by that operation.
- Restore each changed entity to its recorded pre-operation state, not indiscriminately to Active.
- An entity that was already Deactivated or Closed before the Parent operation must remain Deactivated or Closed.
- A child that was already Suspended before a Parent Suspension must remain Suspended after the Parent is reactivated.
- Restore a Mapping only if that Mapping was changed by the same operation, its recorded pre-operation state was eligible for restoration, and both endpoints are currently eligible to participate.
- A Mapping that was already Suspended, Deactivated, or Terminated before the Parent operation must not become Active merely because the Parent is reactivated.
- A later independent state change must never be overwritten by an older snapshot. State/version and operation-causation checks must be performed before restoration.

The restoration must reference the original Lifecycle Operation and Snapshot. A new restoration event is recorded; the original operation and its snapshot are never rewritten.

### 13.8 Lifecycle operation consistency and failure handling

A Parent-Caused Cascade is one controlled logical operation. CBM must not report the operation as successfully completed while only a subset of its required child and relationship changes have been completed.

The implementation must provide transactionally consistent or equivalently reliable operation orchestration, concurrency protection, idempotent retries, and failure recovery. Conflicting lifecycle changes must be controlled while a cascade or restoration is in progress. A retry must not duplicate History/Audit events or incorrectly apply the cascade twice. The exact technical mechanism is an implementation matter, but the resulting business state must not be partially or ambiguously completed.

### 13.9 Closure preconditions

Deactivation and Closure are different operations.

An Organization must not be moved to Closed while any direct Distributor or Shop child remains in a non-Closed operational state. Each child must first be individually resolved and Closed through its own authorized lifecycle operation. Applicable Distributor–Shop Mappings must be ended or otherwise formally resolved before the related business context is finally closed.

Closing a Parent Organization does not silently create ownership transfers, change the structural Parent of a child, or erase child records. The closure operation must preserve the structural and relationship history required to understand the closed business.

### 13.10 Lifecycle History, Mapping History, and Audit Trail

Business Entity Lifecycle History is a domain-specific History record set for the lifecycle of a particular Organization, Distributor, or Shop. It records that entity's own state changes and relevant Parent-Caused effects, including the Lifecycle Operation reference and snapshot reference. It must not contain unrelated User, Subscription, GST, or other domain events.

Distributor–Shop Mapping state changes remain in the specific Mapping's Domain History. The comprehensive Audit Trail separately records the initiating operation, cascade scope, affected objects, authorization, approvals, snapshot creation, state changes, restoration, failures, retries, and outcomes. Snapshot, Domain History, and Audit Trail remain separate records with separate purposes and access controls.

## 14. Subscription, Plans, Entitlements, and Billing

### 14.1 Commercial Account and Service Provider boundary

A Commercial Account belongs to one Service Provider commercial context: Main Portal or one authorized Sub-Portal. The Commercial Account identifies the customer's contracting and billing relationship with that Service Provider. It may have one or more Subscriptions.

A Commercial Account is not an Owner ID, User Account, Business Identity, or Business Structure entity. The contracting/billing party must be explicitly identified; it must not be inferred from User identity, Parent Organization, ownership alone, or Distributor–Shop Mapping.

A Service Provider migration does not silently transfer a Commercial Account or Subscription to another provider. Any contractual transition must be expressly authorized under Main Portal governance. Existing invoices, payments, subscription terms, entitlement history, and other financial records must remain traceable through the transition.

### 14.2 Plan Catalog and versioning

Plans and Add-ons must be data/configuration driven, not hard-coded product packages.

Main Portal governs the allowed Entitlement catalogue, measurement types, composition rules, and immutable platform boundaries. An authorized Service Provider may define and publish Plans within the permitted catalogue and its delegated commercial authority.

Every published Plan must have an identifiable Plan Version. A published Plan Version must preserve the Entitlements, Usage Limits, compatibility rules, duration/billing terms, and commercial conditions under which it was offered. A change that alters published contractual meaning must create a new Plan Version; it must not silently rewrite the meaning of an existing Subscription.

Add-ons are independently defined, versioned commercial extensions. Their compatibility with Plans, other Add-ons, and Subscription terms must be explicit. They must not be hard-coded or treated as unrestricted overrides.

### 14.3 Subscription and explicit coverage

A Subscription applies a specific Plan Version to a Commercial Account for an explicit term or renewal arrangement. It must identify its effective start, applicable end/renewal terms, billing arrangement, covered entities/contexts, attached Add-ons, and relevant commercial conditions.

Subscription Coverage must be established through an explicit Subscription Coverage Assignment. Supported coverage modes are:

1. **Organization Only:** covers the Organization itself.
2. **Selected Business Entities:** covers only the explicitly selected eligible Distributors and/or Shops.
3. **Full Organization Network:** covers the Organization and the eligible Distributor and Shop entities within its current Business Structure, as defined by the Subscription terms.
4. **Future-inclusive Organization Network:** covers the Organization's current eligible network and also includes subsequently created eligible child entities, but only when this mode is expressly selected in the Subscription terms.

Coverage must never be inferred from Ownership, Parent–Child placement alone, Service Provider association, User association, or Business Mapping. A Subscription must not cover another Organization's Shop merely because that Shop is mapped to a covered Distributor.

### 14.4 Entitlements and entitlement resolution

The effective Entitlements available to a covered entity are derived from the applicable Active/Trial Subscription, its Plan Version, compatible Add-ons, Coverage Assignment, effective dates, Usage Limits, and applicable Main Portal/Service Provider rules.

Every Entitlement identifies the feature or capacity it makes available. Each Entitlement type must have an explicit composition rule defining how multiple eligible sources combine—for example, exclusive selection, additive capacity, maximum capacity, or an expressly defined replacement rule. No implicit merge, precedence, or privilege expansion is permitted.

An Entitlement makes a product capability commercially available. It does not grant a User access, User Assigned Authority Scope, Role, Permission, or action authorization. Existing User authorization requirements continue to apply independently.

### 14.5 Usage Limits and enforcement

Each Usage Limit must define:

- The measured resource or activity.
- The entity/business scope to which the measurement applies.
- The unit and aggregation method.
- The measurement or billing period.
- The limit and any warning thresholds.
- The enforcement mode and any permitted overage treatment.
- The reset, carry-forward, or renewal behaviour where applicable.

A limit may use an expressly defined hard-block, soft-warning, or authorized overage model. The behaviour must be stated in the Plan/Entitlement terms and must not be guessed by an implementation. Exceeding a limit must not silently delete, corrupt, or transfer existing business data.

### 14.6 Subscription lifecycle states

The common Subscription service states are:

| State | Meaning |
|---|---|
| Pending Activation | Subscription has been created but activation conditions are not yet complete or effective. |
| Trial | An approved trial period is active. |
| Active | Subscription is active for its effective term and coverage. |
| Grace Period | A governed grace period is in effect after an applicable billing/renewal issue; permitted service behaviour is controlled by policy. |
| Suspended | Subscription service entitlement is suspended under an authorized policy or action. |
| Cancelled | Subscription has been cancelled before or at its defined cancellation effective point. |
| Expired | Subscription term has ended without a continuing effective renewal. |

A scheduled cancellation is represented by its cancellation instruction and effective date; it does not need to be a separate operational Subscription state.

Past Due is a billing/Invoice condition, not a Subscription state. Payment Collection pause or failure must not be conflated with the Subscription's service state.

### 14.7 Invoice and Payment lifecycles

Invoice and Payment are independently identifiable financial records.

Common Invoice statuses are Draft, Issued/Open, Partially Paid, Paid, Past Due, Void, and Uncollectible, subject to the applicable accounting and legal requirements.

Common Payment statuses are Initiated, Processing, Successful, Failed, and Reversed/Refunded. Refunds and reversals must remain traceable to the original Payment and related Invoice or commercial obligation.

One Invoice may be settled through one or more Payments, including partial payments where permitted. A successful Payment does not by itself prove that every other Invoice or Subscription obligation is settled. Invoice status, Payment status, and Subscription status must be resolved independently through defined business rules.

### 14.8 Payment default, Grace Period, suspension, and reinstatement

A failed, late, or missing Payment must not automatically deactivate, close, or delete the Organization, Distributor, Shop, or User Accounts.

The applicable Commercial Account/Subscription policy must define due dates, reminders, retries, Grace Period, permitted service restrictions, suspension conditions, and reinstatement conditions within Main Portal governance.

During Grace Period, the Subscription's permitted service behaviour follows its defined policy. When the applicable Grace Period and required conditions end, the Subscription may be Suspended under an authorized rule. On successful payment/default resolution, the Subscription may be reinstated where its contractual and policy conditions permit.

Subscription suspension or expiry changes entitlement availability only. It does not change the operational lifecycle state of the covered Business Entity, the User Account state, Ownership, Business Structure, or Domain History.

### 14.9 Upgrade, downgrade, renewal, and cancellation changes

Every Plan change, Add-on change, renewal, upgrade, downgrade, cancellation, or other Subscription amendment must have an explicit effective date/time and preserve the before-and-after contractual terms.

Where supported, the applicable terms must determine whether a change takes effect immediately or at the next billing period, and whether proration, credit, additional charge, or limit adjustment applies. The system must preserve the old and new Plan Versions, coverage, Entitlements, Usage Limits, billing calculation references, and approval/authorization records.

Automatic renewal must be expressly defined in the Subscription terms. It must not be inferred merely because a Subscription has previously renewed.

### 14.10 Subscription termination, retention, and entity independence

Cancellation, expiry, or suspension of a Subscription does not itself deactivate or close a covered Organization, Distributor, or Shop. Nor does it terminate a User Account or rewrite business ownership.

Business data, invoices, payments, domain history, and audit records remain subject to their own retention, export, archival, and deletion policies. Subscription termination must not be used as an implicit business deletion instruction.

## 15. User Account Lifecycle and Initial Credential Establishment

### 15.1 Account boundary in the Customer Business Architecture

Each Customer Business User Account belongs to exactly one Organization's Business Architecture. A User Account cannot be reused across multiple Organizations.

The same natural person may work in two or more Organizations, but each Organization must provision a separate User Account for that person. Each account has a separate User Code, email address, mobile number, credentials, account state, and account history. The email address and mobile number used for one of that person's Organization-specific accounts must not be reused for another such account.

A User Account created within Organization A cannot be used to access Organization B, even where the same person works in both Organizations or holds ownership rights in both. This restriction does not prevent a User from being explicitly assigned to multiple eligible Distributors/Shops within the single Organization's own Business Architecture under the separately governed User Scope and Permissions rules.

### 15.2 User Account states

The Customer Business User Account states are:

| State | Meaning |
|---|---|
| Pending Verification | Required account/contact or initial credential verification has not been completed. |
| Active | Account is established and may authenticate, subject to applicable authentication, association, scope, and permission controls. |
| Locked | Account authentication is temporarily blocked by security controls, such as repeated failed attempts or risk detection. |
| Suspended | Account use is restricted by an authorized administrative or security action. |
| Deactivated | Account is disabled for normal use but remains identifiable and historically preserved. |
| Terminated | The account relationship has been formally ended; normal authentication is permanently prohibited. |

Invited is not a User Account state and is not part of the CBM account model. CBM does not use invitation URLs or invitation links.

Password Change Required is a restricted credential-establishment condition, not a separate User Account state. An account in this condition must not receive normal business access.

### 15.3 Three independent User-related lifecycles

The following three lifecycles are separate and must not be conflated:

1. **User Account Lifecycle:** creation, verification, activation, locking, suspension, deactivation, and termination of the account record.
2. **User–Business Association Lifecycle:** establishment, activation, suspension, and revocation of the association within that account's permitted Customer Business Architecture.
3. **Authentication Credential / Authenticator Lifecycle:** creation, verification, use, replacement, expiry, compromise handling, and revocation of passwords, OTP methods, and other approved authenticators.

A transition in one lifecycle affects another only through an explicitly defined rule. In particular, terminating one Organization-specific User Account does not terminate a different account held by the same natural person in another Organization.

### 15.4 Authorized User provisioning; no invitation model

A User Account is provisioned by an Authorized User completing the required account details and business-account setup within CBM. The system creates the account record, allocates its immutable User Code, records its Organization boundary, and applies the Organization's configured initial credential-establishment method.

CBM does not send an invitation URL, activation URL, registration link, or any other link to complete account provisioning. The process is completed through the CBM application using the approved credential or OTP method.

### 15.5 Organization-configurable initial credential method

Each Organization must be able to configure one of these initial credential-establishment modes:

| Configuration mode | Required process |
|---|---|
| Email Temporary Password | CBM sends the generated Temporary Password to the User's registered email. The User establishes a permanent password through the restricted bootstrap process. |
| OTP Password Setup | CBM sends an OTP using a supported verified channel. The User enters the OTP inside CBM and establishes a permanent password. |
| Both Required | The User must successfully complete both the Email Temporary Password proof and the OTP Password Setup proof before normal account use is enabled. |

The three modes are selectable Organization configuration values. Both Required means both proof methods are mandatory; it does not mean the Organization may select either one at runtime.

Supported OTP delivery channels are Email, SMS to the registered mobile number, and WhatsApp to the registered mobile number. The selected channel must be available and verified for the account. OTP delivery and Temporary Password delivery must not include any URL or link.

The Organization's configured mode governs the account establishment process. A later change to the Organization's configuration does not silently reset credentials or force existing Users through a new setup process. A future re-verification or credential replacement must be a separately authorized action.

### 15.6 Temporary Password bootstrap behaviour

The Email Temporary Password is a single-purpose bootstrap credential. It is not a general password and cannot be used to access normal business functions.

The Temporary Password may authenticate the User only into the restricted initial password-establishment process. If password validation or password storage fails, it must remain valid for retry within that same restricted purpose until it is successfully replaced, expires, or is explicitly revoked. A failed password-change attempt alone must not invalidate it.

Until the permanent password has been successfully stored, all normal business operations, dashboards, modules, data access, and account functions other than the required password-establishment process remain blocked.

After successful permanent-password storage, the Temporary Password is immediately and permanently invalidated. It cannot be used again. It must have a defined expiry and revocation capability and must be protected as a secret.

### 15.7 OTP password setup and retry behaviour

OTP is a one-time, short-lived verification secret bound to a specific account and password-establishment purpose. It must be cryptographically generated, rate-limited, protected from replay, invalidated after successful verification, and excluded from application logs and long-term plaintext storage.

Successful OTP verification establishes only a restricted password-setup session. That session permits password creation and confirmation only. It does not grant normal CBM account access.

If password storage fails after successful OTP verification, the User may retry within the still-valid restricted setup session without reusing the consumed OTP. If the restricted session expires or is revoked before the permanent password is stored, a new OTP verification is required. The same restrictions apply when the Organization selects Both Required; normal account use remains blocked until both required proofs and permanent-password establishment succeed.

OTP is an approved supported method, but it is not treated as compromise-proof. The Organization may select OTP alone, Temporary Password alone, or both methods as mandatory.

### 15.8 Password storage and credential security

Permanent and Temporary Password values must never be stored as plaintext in the CBM database or written to logs. Password storage must use an appropriate adaptive password-hashing algorithm and properly managed salts. Temporary Passwords must be transmitted only through the configured email channel and protected as bootstrap secrets.

Credential changes, resets, authenticator replacement, recovery, compromise handling, and revocation must be authenticated or verified through their applicable approved processes. Any successful password change or security-sensitive credential change must apply the relevant session renewal/revocation controls defined by the Login and Authentication Architecture.

### 15.9 Account recovery without URLs

Password recovery and account recovery must not use URL tokens, reset links, magic links, or any other URL-based action.

Recovery must be performed within the CBM application using the applicable verified OTP channel or an authorized support-assisted identity-verification procedure. A recovery process must not disclose whether an unrelated account exists, must limit repeated attempts, and must preserve an audit record of the recovery outcome. If the User no longer controls the registered channels, recovery must use an independently authorized manual/support verification path; it must still not send a URL or link.

### 15.10 Termination and historical integrity

When a User Account is Terminated:

- Normal authentication is permanently denied.
- Active sessions and account-bound authentication tokens are revoked.
- Active authenticators/credentials are revoked or rendered unusable.
- The account's active associations are ended according to the account termination process.
- The User Code is retained as a historical identifier and is never reassigned.
- Existing business records and Audit Trail events retain their original actor/account reference.
- Termination of this account does not terminate another separate account belonging to the same natural person in another Organization.
- Terminating a User Account does not deactivate, suspend, close, or alter the Business Entity.

Detailed Login Architecture, login channels, session policy, and the full User Scope/Permissions design remain governed by their separate discussions and must not be inferred from this account-lifecycle baseline.

## 16. Sub-Portal migration and service continuity

### 16.1 Nature of migration

Sub-Portal migration is a complete service-provider migration. It is not a transfer of Tenant ownership, Business Identity, or data rights.

A Sub-Portal and its associated customer/business structure may be migrated to another Sub-Portal or directly under Main Portal, under Main Portal governance.

### 16.2 Migration boundary

Migration must include the complete Sub-Portal scope and all information required to preserve its existing internal structure and service continuity, including as applicable:

- All subordinate business entities.
- The complete business structure and hierarchy.
- Associated business data and records.
- Users and relevant access associations.
- Business records and transactions.
- Documents and attachments.
- Configurations and operational settings.
- Relevant domain-specific History records and Audit Trail records, preserved separately and according to their respective retention rules.
- Other associated information necessary for complete continuity and integrity.

### 16.3 Invariants during migration

Migration must preserve:

- Each Business Identity.
- Business ownership and ownership boundaries.
- Tenant ownership and data rights.
- Existing internal business structure.
- Valid business relationships and their history.
- Data integrity and historical continuity.

Only the applicable Service Provider relationship changes. Migration must not silently reassign ownership, merge businesses, or alter Tenant data rights.

### 16.4 Sub-Portal closure or ownership change

If a Sub-Portal closes or its ownership changes, its complete associated business structure and data must be capable of migration to another Sub-Portal or directly to Main Portal while preserving the invariants above.

Migration is a controlled, complete, integrity-verified capability. It is not merely reassignment of database records.

---

## 17. Data archival and object-storage principles

### 17.1 Provider-neutral abstraction

Long-term archival must be designed around an independent Object Storage Abstraction so that the archival system is not locked to a single object-storage provider. The architecture must support changing providers without redesigning the archival domain.

### 17.2 Archival requirements

The archival capability must support:

- Encryption of archived data.
- Complete export of business data and related files.
- Database records and associated documents/attachments.
- An archive manifest.
- Integrity verification and checksums.
- Secure access controls.
- Retention management under Main Portal policy.
- Restore capability.
- Domain-specific History records and Audit Trail records, preserved separately according to their respective retention rules.
- Verification of successful archival before deletion of the operational copy.

### 17.3 Separation from operational data

Archival is preservation, not continuation of normal operations. Removing an operational copy after the required download window and successful archival does not mean that the archived business history is erased or that ownership is transferred.

---

## 17A. Domain History and Audit Trail Management

### 17A.1 Mandatory separation

**Domain History** and **Audit Trail** are separate and independent record categories. They have different purposes, contents, structures, retention rules, and access permissions. They must not be merged into one table, one undifferentiated log, one combined domain History, or one permission domain. An event may create both records, but each is maintained independently for its own purpose.

### 17A.2 Domain-specific History and approved History domains

A History record set belongs to one clearly defined domain object, entity, or relationship. Each approved History domain must have its own dedicated domain-specific table/record set; a shared mixed-domain History table is not permitted. It records meaningful business changes and events that authorized business Users need to understand. It must contain only events and information belonging to that domain.

The following Domain History record sets are established by the approved business and logical architecture. Each record set is owned by the named domain and must have its own dedicated domain-specific record structure; unrelated domains must not be combined into a mixed History record set.

1. **GST Registration History:** GST Taxpayer Profile/GST Registration particulars and GST Registration Assignment events only. It does not contain unrelated GST returns, HSN/SAC classification, or User profile changes.
2. **Distributor–Shop Mapping History:** events concerning one specific Distributor–Shop relationship, its verification stages, authorized scope/type, status changes, suspension, revocation, and termination only.
3. **Ownership History:** events concerning the relevant Owner ID, Ownership ID, and Ownership Link, including establishment, change, correction, and termination only.
4. **User Scope Mapping History:** events concerning the relevant User-to-entity/location scope assignment, including assignment, change, suspension, and revocation only.
5. **Business Entity Lifecycle History:** lifecycle state changes for one Organization, Distributor, or Shop, including the causal Lifecycle Operation and associated Snapshot reference. It contains only that entity's lifecycle events.
6. **Accounting Domain History:** material accounting-domain changes and lifecycle events, including Accounting Context/Book configuration, Accounting Policies, Voucher preparation/validation/approval/posting, correction, reversal, Accounting Period control, reconciliation, migration, and export events as applicable. It does not replace the authoritative Vouchers, Journal Entries, Ledger Postings, or Audit Trail.
7. **Human Resource / Employment History:** Employee and employment-record changes, recruitment/onboarding milestones where retained as employment-domain facts, employment relationship/status, assignment, attendance/leave corrections, and other material workforce-record events. It does not contain User Account credentials, Role/Permission changes, or Payroll calculation records. Sensitive personal and employment data must be minimized and protected by HR-specific scope and permissions.
8. **Payroll Domain History:** Payroll policy/rule versions, payroll-period and run lifecycle, calculation inputs/results corrections, earnings/deductions, payslip issuance/correction, approval/finalization, statutory payroll calculation records, and payroll-domain exceptions. It does not contain Finance payment execution or Accounting postings, which remain in their own domains.
9. **Product/Item Catalogue History:** Product Definition/Variant changes, catalogue attributes/specifications/classifications, identifiers and external mappings, packaging configurations, Bundles/Kits, publication/sharing, lifecycle, merge/split, correction, and supersession events. It does not contain Product declared-price history, Sales/Purchase transaction facts, Inventory stock movements, or GST tax-classification/rate histories.
10. **Inventory Domain History:** Inventory-owned stock receipt/issue/transfer/adjustment, reservation/status transition, reconciliation, and valuation-relevant changes. It must reference the authoritative Inventory Transactions/Stock Movements and must not duplicate or replace the Inventory Ledger or accepted stock-movement records.
11. **Warehouse Domain History:** Warehouse facility/layout and location-hierarchy changes, Handling Unit placement/movement, Warehouse task assignment/execution, physical counts, and warehouse operational exceptions. It must reference the authoritative Warehouse Work/physical-execution records and must not replace Inventory stock records.
12. **Product/Item Declared Price History:** declared MRP, Manufacturer Suggested Retail Price / RRP, legally required unit-sale-price declarations, their product/package scope, source, declaration version, effective/recorded times, and authorized corrections or superseding declarations. It does not contain Sales transaction prices or Purchase transaction prices.
13. **Sales Pricing History:** Sales-owned Price Lists, Selling Price Records, Price Conditions, promotions/discount rules, effective-date changes, approvals, and authorized overrides. It does not replace the accepted Sales/POS transaction records or their Price Snapshots.
14. **Sales Commercial Document History:** Sales quotations/offers, agreements/terms, orders, original Sales Invoices, Sales Returns, commercial credit/debit documents, fulfilment commitments, and related approval, issue, cancellation, correction, and supersession events. It does not contain Sales Pricing History, POS till/session events, Finance payment execution, GST source-rule history, or Accounting postings.
15. **Purchase Pricing History:** Purchase-owned supplier quotation/agreement/reference-price records, their terms, effective-date changes, approvals, and authorized overrides. It does not replace accepted Purchase transaction records or supplier invoice history.
16. **Purchase Commercial Document History:** purchase requisitions/requests, sourcing and supplier-selection decisions, RFQs, supplier quotations, purchase agreements, Purchase Orders, original Purchase Invoices, Purchase Returns, commercial receipt confirmations, discrepancy/acceptance decisions, and commercial matching/correction events. It does not replace Purchase Pricing History, Warehouse physical-receiving records, Inventory stock movements, Finance payment execution, or Accounting postings.
17. **POS Operations History:** POS terminal/workstation configuration, operator/session/shift lifecycle, till/cash-drawer opening and closing, opening float, cash-in/cash-out, counts, checkout execution traces, operational exceptions, offline synchronization/recovery where enabled, and session/till reconciliation. It does not duplicate the original Sales commercial document, Finance payment transaction, or Accounting posting.
18. **HSN/SAC Registry History:** GST/Tax-owned official code records, descriptions, classification/tariff versions, sources, effective dates, corrections, supersessions, and retirements.
19. **Product/Service Tax Classification Assignment History:** GST/Tax-owned effective-dated assignments connecting Product Variants or eligible service/supply subjects to HSN/SAC codes, including source evidence, scope, review, correction, and approval events.
20. **GST Rate Rule History:** GST/Tax-owned GST Rate Applicability Rule versions, statutory notification references, rate components, conditions, effective dates, scheduled changes, corrections, supersessions, withdrawals, and expiry. It does not replace accepted transaction Tax Determination Snapshots.
21. **GST Compliance and Filing History:** GST return preparation, validation, approval where applicable, submission/filing, acknowledgements, amendments, corrections, rejected/failed submissions, reconciliation, and compliance exceptions. It does not replace the originating Sales/Purchase transaction, GST Registration History, HSN/SAC Registry History, Classification Assignment History, GST Rate Rule History, or accepted transaction Tax Determination Snapshots.
22. **Finance & Payment Operations History:** operational Cash/Bank Account administration, payment instructions and attempts, execution outcomes, settlement, refund, reversal, chargeback, provider/bank evidence, operational reconciliation, and related correction/exception events. It does not contain Accounting Books, receivable/payable balances, accounting allocations/postings, or Bank-to-Ledger reconciliation.
23. **Asset Management History:** asset registration/classification, custody and responsible-person assignment, location and transfer, usage/status, inspection, maintenance/repair, insurance administration, retirement, and physical-disposal events. It does not replace Inventory/Warehouse physical records or Accounting Fixed Asset Accounting history.
24. **Business Planning & Budget History:** business plan, budget, target, forecast, scenario, assumption, allocation, approved-baseline, revision/version, planning-approval, and planned-versus-actual analysis changes. It does not copy or replace actual operational records or posted Accounting facts.
25. **Counterparty Domain History:** Party classification, Customer/Supplier role-profile creation/change/status, contact/address changes, relationship lifecycle, duplicate-resolution, controlled merge/split, provenance, and correction/supersession events. It does not contain User Account, Employee, Business Identity, Legal Identity, or GST Registration history.

The History record sets listed above cover the domains explicitly established in this baseline. A new History domain must not be inferred merely because Audit Trail or operational logs exist; it requires a defined business need, owner, scope, contents, permissions, and retention rule. A domain History may reference its owner's authoritative transactions/events but must not become a competing source of truth or a duplicate transaction ledger.

### 17A.3 History contents, integrity, and retention

Each History record set records only fields relevant to its own domain. As applicable, it may include affected domain object/relationship, event type, previous and new domain values/states, effective date/time, recorded date/time, reason, responsible actor, approver, and supporting reference. A significant domain change must not silently overwrite the only record of its previous state. Corrections must preserve the original entry and identify the correction or superseding event.

History must be preserved throughout the existence of the relevant Business Identity/business context. It must not be routinely purged or deleted while that business remains in existence. It may be removed only as part of authorized permanent deletion of the relevant business.

History may be available to multiple appropriately authorized Users. Its visibility is controlled by separate History permissions and applicable business scope. Permission to view History does not grant permission to perform the underlying action or to view the Audit Trail.

### 17A.4 Comprehensive system-wide Audit Trail

Audit Trail must capture all relevant system events and activities across CBM in chronological order. Its coverage is not limited to the History domains above or to security events alone.

As applicable, Audit Trail includes successful and failed actions, creation and modification attempts, access and authentication events, User and Role changes, User Scope Mapping changes, Ownership Link changes, Business Relationship/Mapping changes, GST Registration verification and assignment events, HR employment-record and Payroll policy/run/approval changes, Product/Item catalogue and identifier changes, Inventory stock/valuation and Warehouse location/task changes, Sales quotations/orders/invoices/returns and Purchase sourcing/orders/invoices/receipts/returns, POS terminal/session/shift/till/checkout/cash-movement and offline-sync events, Finance/Cash/Bank account administration and payment instruction/execution/settlement/refund/reversal/chargeback events, GST return preparation/filing/acknowledgement/amendment events, Asset custody/transfer/maintenance/disposal events, planning/budget/forecast version and approval changes, Counterparty role/contact/address/merge/split changes, Price Record/Price List/Price Condition changes and overrides, price/tax approvals and rejections, History access/export, configuration changes, administrative actions, data exports, archival and deletion operations, and security events. Sensitive HR, Payroll, payment, tax, and Counterparty data must be handled with the applicable field-level/contextual protections; comprehensive Audit Trail coverage does not grant ordinary Users access to the underlying sensitive content.

Audit Trail must capture sufficient context to reconstruct what happened, when, in which business/entity context, by whom or by what system process, and with what outcome. It may contain events from multiple domains in one chronological sequence. It does not replace any domain-specific History.

### 17A.5 Separate access permissions

1. History and Audit Trail must have separate access permissions.
2. A User may view relevant domain History without being allowed to view Audit Trail.
3. Audit Trail access is restricted to specifically authorized roles/Users under Main Portal governance and applicable security policy.
4. Access to one domain's History does not grant access to another domain's History.
5. Permission to view History does not grant permission to create, change, or revoke the underlying business object.
6. Audit Trail permission does not automatically grant access to all business History.
7. Viewing or exporting either record category is itself an activity captured in the Audit Trail, subject to applicable policy.

### 17A.6 Retention, archival, and deletion

Domain History is retained for the full lifetime of the relevant business context and may be removed only through authorized permanent deletion of that business. It must not be purged according to an Audit Trail retention schedule.

Audit Trail has a separate retention and archival policy. It is not required to be permanent in every case. Retention periods, archival, and eligible deletion are governed by Main Portal policy and applicable legal, regulatory, contractual, and security requirements.

Business archival must preserve Domain History and Audit Trail separately where each remains within its applicable retention period. Their integrity, provenance, and access controls remain independently enforceable after archival or migration.

### 17A.7 Paired-record examples

When a GSTIN is changed, GST Registration History records only the previous and new GST registration/assignment, effective date, recorded date, reason, actor, and approver where applicable. The Audit Trail separately records relevant system events, including actor/session, request and outcome, verification, authorization, and approval events.

When a User ID is changed, that event is recorded in the comprehensive Audit Trail. It is not inserted into GST Registration History. A separate User History is permitted only if a distinct business need is approved.

When a Distributor–Shop Mapping is created, changed, suspended, or ended, the specific Mapping History records the business chronology of that relationship. The Audit Trail separately captures system events and actions associated with those operations.

When a Product Variant's HSN assignment changes or a GST rate notification changes an applicable rule, the relevant HSN/SAC Registry History, Classification Assignment History, or GST Rate Rule History records the corresponding domain chronology. The Audit Trail separately captures the actors, permissions, source/import process, approvals, failures, access, and resulting system events.

When a Sales Invoice is issued or corrected, Sales Commercial Document History records the Sales-owned commercial document change. The accepted invoice retains the applicable Price and Tax Determination Snapshots; GST/Tax retains its classification/rate/compliance history; Accounting retains its authorized posting history; Finance & Payment records any payment execution or settlement. The Audit Trail separately records the system actions, permissions, approvals, and outcomes across those domains.

When a payment is executed, settled, refunded, or reversed, Finance & Payment Operations History records the payment-domain chronology and provider/bank evidence. Accounting separately records the authorized accounting allocation, voucher/posting, and Bank-to-Ledger reconciliation. A successful provider response does not by itself establish a completed Accounting posting.

When an operational Asset is transferred to another custodian or location, Asset Management History records the custody/location change. If the event has an accounting effect, Accounting records it within Fixed Asset Accounting History; any related physical inventory movement remains in Inventory/Warehouse records. The Audit Trail separately captures authorization, approval, system actor, and outcome.

---

## 18. Stable Core, Configurable, Customizable, and Extensible design principles

All architectural decisions and capabilities must be evaluated using the Stable Core, Configurable, and Extensible principles, together with the additional design dimensions of customization, override capability, and applicability defined in Section 2.6. These are complementary design lenses, not mutually exclusive labels.

### 18.1 Stable Core

Stable Core contains fundamental protections and invariants that must not be made ordinary user-configurable options. It includes:

- Main Portal's permanent supreme authority.
- Authority flowing exclusively downward.
- No peer authority between Sub-Portals.
- Business Identity integrity.
- Ownership boundaries.
- Strict Business and Tenant Data Isolation.
- Strict Business Structure Hierarchy and Structural Integrity.
- Separation of Business Authority Scope from User/Role Permissions.
- Authorization integrity.
- Configuration guardrails and protection against unauthorized overrides.
- Relationship integrity.
- Data integrity.
- Security boundaries.
- Approval integrity.
- Audit integrity.
- Preservation of identity, ownership, and data rights during Service Provider migration.

### 18.2 Configurable

Configuration is allowed where a business requirement legitimately varies, provided the change remains within Main Portal governance, explicit permissions, and Stable Core boundaries.

The approved configurable approach includes:

- Default values and permitted business-specific overrides.
- Role-specific and User-specific permissions.
- Business-specific configuration.
- Delegated authority dimensions and limits.
- Approval requirements and patterns.
- Operational/business settings within system-defined bounds.
- Applicable lifecycle and archival policy settings within the governance model.

Configurability must not allow a lower level to weaken or bypass an immutable higher-level rule. Data Isolation and Business Structure Hierarchy and Structural Integrity are never configurable or overridable. Customization and configuration must be limited to the applicable business context and must not silently affect another business.

### 18.3 Extensible

The architecture must allow new capabilities to be added without fundamental redesign, using clear business boundaries and contracts. Extensibility includes the ability to introduce:

- New business-management modules.
- New integrations and APIs.
- New reports and analytics capabilities.
- New storage providers behind the Object Storage Abstraction.
- New Business Relationship types.
- New workflow and automation capabilities.
- New operational capabilities that remain consistent with the CBM governance model.

Extensibility must not become an excuse for unnecessary abstraction or overengineering.

### 18.4 Customization, Override, and Applicability

- Customization may adapt only those product behaviours and choices that the governing Stable Core and Main Portal policies permit.
- Override is a controlled authority, not an inherent consequence of having access to a configuration screen. It requires explicit permission for the specific setting and business context.
- Applicability must be defined for every policy, configuration, workflow, permission, and extension where different business contexts may require different treatment.
- Business-specific customization and overrides must be isolated to their authorized context and must not propagate to sibling or unrelated businesses unless an explicit governing rule requires inheritance.
- None of these capabilities may weaken Data Isolation, Business Structure Hierarchy and Structural Integrity, ownership integrity, Main Portal supremacy, or other Stable Core constraints.

---

## 19. Required cross-concept invariants

The following statements summarize rules that must remain true across all future domain and technical designs.

1. Main Portal's authority scope covers the entire CBM Ecosystem and cannot be restricted by a subordinate.
2. Authority flows downward only; no subordinate authority flows upward.
3. A Sub-Portal is an independently owned Service Provider, not a customer Tenant/Organization.
4. A Service Provider does not become the owner of the customer business by providing software services.
5. A customer's Business Identity and ownership remain unchanged when its Service Provider changes.
6. Organization, Distributor, and Shop are distinct entity types.
7. A Shop is the unified terminal structural entity directly under an Organization. Ownership and Legal Identity do not create separate Shop classifications.
8. Structural placement, Business Available Authority Scope, User Assigned Authority Scope, and Permissions are independent dimensions. A Distributor–Shop Mapping does not change structural placement or automatically grant additional authority.
9. A Distributor's Business Available Authority Scope includes itself; any additional Shop scope requires an explicit, separately authorized rule and is never created by Mapping alone.
10. A Shop retains its own Business Identity, ownership, and data boundary even when it has an authority relationship or Business Mapping with another business.
11. Business Available Authority Scope determines the business-level authority boundary; it does not itself assign any specific entity or location to a User or grant that User access. The User's specific entity/location scope is determined through explicit User Scope Mapping.
12. Every User must have an explicit valid User–Business association and explicit User Scope Mapping for the target entity. User level, title, Role, Owner identity, structural position, Business Available Authority Scope, or another Mapping does not automatically grant the full available scope.
13. Role/Permission determines what actions a User may perform; it cannot expand the User's assigned entity scope.
14. A valid action requires the target to be within the relevant Business Available Authority Scope and the User Assigned Authority Scope, together with the required Permission and any applicable approval/control.
15. Delegated Authority is constrained by Who, What, Where, Limits, and Duration.
16. Configuration defaults and inheritance flow downward. Overrides require explicit permission and must stay within system-defined bounds.
17. Configuration overrides may be granted to one business and withheld from another.
18. Approval requirements can vary by business, action, Role, and individual User.
19. Approval workflows support single, multiple, sequential, and parallel approval patterns.
20. Approval does not create an underlying action Permission.
21. Distributor–Shop Mapping is one specific Business Relationship Mapping type, not a universal definition or boundary for Mapping across CBM; it is not ownership and does not grant unrestricted access.
22. Every Distributor and Shop must have exactly one direct Parent Organization when created; neither may be created unassigned, standalone, or with multiple structural parents.
23. A Distributor–Shop Mapping does not require either party to select, know, or disclose the other party's Parent Organization.
24. Distributor–Shop Mapping activation requires successful two-way verification: the Shop's code must first be verified by the Distributor, and a separate Distributor confirmation code must then be verified by the Shop.
25. Both mapping codes must expire, be single-use, be protected by rate limits, and support revocation. An expired, consumed, or revoked code cannot complete its verification stage.
26. An Active Mapping permits only Mapping-Type-authorized business transactions and necessary information exchange. It does not change ownership, Structural Membership, Parent Organization, or either party's Business Data Boundary, and it does not grant broad or unrestricted data access.
27. One Shop may map to multiple Distributors; each mapping is a separate relationship.
28. Ending a mapping does not close the Shop or terminate its other mappings.
29. Sub-Portal migration preserves identity, ownership, data rights, and internal structure; only Service Provider changes.
30. Operational data may be removed after the applicable download period only after archival is verified.
31. No User receives access to a business solely because the User, Owner, Service Provider, or another associated business has a relationship with it. Access requires explicit valid association, assigned scope, and Permissions.
32. Business and Tenant Data Isolation are strict Stable Core constraints and cannot be weakened through configuration, customization, extension, delegation, or override.
33. Business Structure Hierarchy and Structural Integrity are strict Stable Core constraints: invalid entity placement and parent–child arrangements cannot be created, configured, customized, delegated, or overridden.
34. Owner ID identifies an Owner; Ownership ID identifies one specific Ownership Link; neither is a User ID or Business ID.
35. Ownership Links identify ownership for business-authority purposes, not equity percentages, financial shares, or economic interest.
36. Ownership Links may inform Business Available Authority Scope only under explicit Main Portal-governed rules. They do not automatically grant User access or action Permissions.
37. Every feature, policy, configuration, workflow, permission, and extension must have deliberate applicability to the business contexts for which it is intended.
38. Scalability, configurability, extensibility, customization, override capability, and applicability must be considered throughout architecture design; none grants permission to violate Stable Core constraints.
39. A business-specific variation must remain within its authorized business context and must not silently affect another business.
40. Mapping is typed and purpose-specific. Distributor–Shop Mapping does not restrict or define other Mapping or Assignment types.
41. GST Registration Assignment may target any CBM entity type/context enabled by Main Portal, subject to verified taxpayer identity, lawful registration coverage, and applicable GST rules; it is not limited to Distributor or Shop.
42. GSTIN eligibility is determined from the actual registered taxpayer, verified registration particulars, lawful Place of Business coverage, intended GST purpose, and applicable law. Owner ID, any Mapping, hierarchy, or service-provider association alone never establishes eligibility.
43. GST Registration Assignment does not transfer a GST registration or permit an unrelated legal person/distinct person to use that GSTIN.
44. User Scope Mapping, not merely a User's business level or Business Available Authority Scope, determines the specific entities/locations assigned to that User.
45. Domain History is maintained separately for each approved domain and contains only that domain's events. It is preserved for the lifetime of the relevant business context and removed only through authorized permanent business deletion.
46. Audit Trail is the comprehensive chronological record of relevant system events across all domains. It is separate from Domain History and follows its own retention policy; permanent retention is not universally required.
47. History and Audit Trail have separate record sets and access permissions. A business History may be visible to multiple authorized Users without granting those Users Audit Trail access.
48. Project Discussion History, Domain History, and Audit Trail are three distinct concepts.
49. Within one Customer Business Architecture, the Owner ID set of the Organization must exactly match the Owner ID set of every direct structural Distributor and Shop. Each entity has its own explicit Ownership Links and Ownership IDs. Main Portal and Sub-Portal Service Provider ownership remains independent.
50. An Organization's suspension and deactivation apply the expressly defined Parent-Caused Cascade to its direct Distributor and Shop children. Child suspension or deactivation never changes the Parent Organization or sibling entity states.
51. Suspending or deactivating a Distributor or Shop affects only that entity and the applicable Mappings involving that endpoint. A remote mapping counterparty and its other relationships are not automatically suspended or deactivated.
52. Every multi-entity Parent-Caused Lifecycle Operation requires an immutable pre-operation Snapshot. Restoration is tied to the exact operation and restores only objects changed by that operation to their recorded previous states. Previously Deactivated/Closed objects and later independent state changes must not be overwritten.
53. Business Entity Lifecycle, Parent–Child Cascade, Mapping Lifecycle, Subscription Lifecycle, User Account Lifecycle, User–Business Association Lifecycle, and Credential/Authenticator Lifecycle remain separate, with only explicitly defined interactions between them.
54. Archived is an archival lifecycle status, not a normal Business Entity operational state. Business Entity Closure and Archival are separate.
55. A Subscription is attached to a Commercial Account in one Service Provider commercial context. Subscription Coverage must be explicitly assigned; ownership, structural membership, Service Provider association, or Mapping alone does not create coverage.
56. Plan and Add-on definitions are configurable and versioned, not hard-coded. Published Plan Versions preserve their contractual meaning; changes that alter that meaning require a new version.
57. Entitlements and Usage Limits govern commercial feature availability and capacity. They do not grant User Scope, Role, Permission, or action authorization.
58. Invoice, Payment, and Subscription statuses are independent. Past Due is a billing/Invoice condition, not a Subscription operational state. Subscription suspension, cancellation, or expiry does not itself deactivate a Business Entity or terminate a User Account.
59. Each Customer Business User Account belongs to exactly one Organization's Business Architecture. The same natural person may have separate accounts in multiple Organizations, each with different verified email and mobile details, credentials, and User Code; one account cannot cross Organization boundaries.
60. Every CBM User Account has one portal-wide unique, system-generated, immutable six-character User Code formed by two uppercase English letters followed by four digits. It is read-only, never reused, and is not a secret or authorization grant.
61. The Organization chooses one of three initial credential-establishment modes for new Customer Business User Accounts: Email Temporary Password, OTP Password Setup, or Both Required. In Both Required mode, both methods must succeed.
62. A Temporary Password is a restricted, single-purpose bootstrap credential. Failed password storage does not by itself revoke it; it remains usable only for the restricted password-establishment process until success, expiry, or revocation.
63. OTP is a one-time, short-lived verification secret. Email, SMS, and WhatsApp are supported delivery channels under the approved account-establishment process; OTP is not treated as compromise-proof.
64. CBM must never send any URL, hyperlink, URL-bearing button, or URL-resolving QR code to any person for any purpose or through any outbound communication channel. Account establishment and recovery must be completed without URL-based actions.
65. Invited is not a User Account state and the CBM account model does not use invitation links. Authorized User provisioning and the approved initial credential-establishment process are used instead.
66. Platform-wide Services is a distinct Architecture Layer and Capability Group that provides shared governed capabilities without taking ownership of Business Module domain rules or authoritative business records.
67. The four approved Platform-wide Service Scopes are Ecosystem-wide, Service Provider-scoped, Tenant-scoped, and Business-context-scoped. Service Scope classifies service applicability and availability; it is not Business Authority Scope, User Assigned Authority Scope, ownership, or a Permission.
68. Platform-wide Services is organized into three approved groups—Platform Foundation Services, Shared Functional Services, and Platform Operations & Governance Services—and fifteen approved Categories as defined in Section 6A. A new Category or material change to the approved taxonomy requires explicit project-owner approval.
69. Category, Service, and Capability are distinct conceptual architecture levels. Their distinction does not prescribe separate applications, processes, databases, vendors, microservices, or deployment units.
70. Identity, Authentication, Authorization, and Security & Trust are separate responsibilities. Authorization evaluates the existing approved CBM authority, scope, permission, delegation, and approval rules; it does not replace or redefine their authoritative sources.
71. Approval execution does not grant missing action Permission, User Assigned Authority Scope, Business Available Authority Scope, or other authority.
72. Communication & Notification, Event & Messaging, and Integration & API are separate responsibilities for human-facing communication, internal/platform event distribution, and external/system interfaces respectively. Technical connectivity, authentication, event subscription, or API credentials do not grant business authority.
73. Event publication, delivery, or consumption does not itself prove that a business transaction has completed or grant permission to access or change business data. Event processing must preserve context, authorization, idempotency, and Data Isolation.
74. Event & Messaging infrastructure, operational logs/traces/metrics, Domain History, and the comprehensive Audit Trail are distinct. Event queues and observability data never replace the separately governed Domain History or Audit Trail.
75. Audit & History Infrastructure must preserve the mandatory separation of Domain History and Audit Trail in their records, purposes, access permissions, and retention rules.
76. Every Platform-wide Service operation, event, message, search result, report, export, file retrieval, and background job must preserve the applicable Service Scope, business context, authorization, purpose limitation, and strict Business/Tenant Data Isolation.
77. A service's shared or ecosystem-wide availability does not grant unrestricted access to its operations or data. Service Scope never substitutes for explicit authorization.
78. Platform-wide Services do not alter Main Portal's permanent supreme authority, downward-only delegation, ownership boundaries, business structure, or any other Stable Core constraint.

79. CBM's approved product and business market scope is India-only. Multi-country operation and international localization are not part of the approved scope.
80. English is the platform default language. Only Indian languages may be supported, and Main Portal governs which Indian languages are enabled and available.
81. INR is the only supported currency; IST / Asia/Kolkata is the only supported time zone. No lower-level configuration or User preference may introduce foreign currency or another time zone.
82. Indian number grouping, Indian regional/address references, and the approved Metric baseline with relevant India-use units govern shared presentation and reference conventions.
83. Global Business Settings provides shared defaults, preferences, and reference conventions; it does not take ownership of statutory determinations, authoritative business records, or domain-specific rules belonging to Business Modules.
84. Main Portal's mandatory India-only boundaries are non-overridable. Other eligible settings inherit through their expressly defined governance path and may be overridden only with explicit authority, applicability, and Permission.
85. Main Portal and Sub-Portal Service Provider settings form a governance path separate from the customer-business path. A Sub-Portal is not a structural parent of an Organization and has no automatic authority to override customer-business settings.
86. Organization may establish eligible defaults for its direct Distributor and Shop children. Child-level overrides and User presentation preferences are allowed only where explicitly applicable and authorized; neither may change authoritative business settings or affect unrelated contexts.
87. The default Financial Year is 1 April through 31 March. An alternative is permitted only where supported by applicable domain rules and explicitly authorized; the default is not a universal legal assertion.
88. Gregorian is the core System Calendar. Indian public, festival, and State/UT holiday references are separate shared calendar data and do not automatically determine statutory or module-specific obligations.
89. Date/time, language, number/currency, and unit display preferences must not alter authoritative timestamps, legal identity particulars, stored amounts, calculations, source quantities, or source units.
90. GST/Tax, Accounting, Human Resource Management, Payroll Management, Product/Item Management, Inventory Management, Finance/Payment, Business Identity, and other domain-specific responsibilities remain owned by their respective Business Modules. Global Business Settings may provide defaults/reference data without assuming domain ownership.
91. Accounting Management is an independent Business Module with authoritative ownership of its Accounting Policies, Chart of Accounts, Account Groups, Ledger Accounts, Voucher Types, Accounting Vouchers, Journal Entries, Ledger Postings, and Book-specific accounting records. It does not own the originating transactions or domain rules of other Business Modules.
92. CBM's default Accounting model is Double-entry. Every posted Accounting Entry set must have equal total debit and credit. Accrual basis is the default for general-purpose Books; alternative recognition bases may be used only where applicable law and the relevant Accounting Framework permit them and an authorized policy enables them.
93. Every statutory Book of Accounts has exactly one accountable Legal Identity. Separate Legal Identities maintain separate authoritative statutory Books. Common Owner IDs, structural hierarchy, Service Provider association, shared Professional Identity branding, or Business Mapping never merges Books.
94. Multiple Professional Identities, branches, Shops, or operating locations under the same Legal Identity may be attributed within shared statutory Books through authorized Accounting Dimensions; a separate Professional Identity alone does not require a separate statutory Book.
95. Multiple Books under one Legal Identity require explicit accounting purposes, policies, ownership, scope, and reconciliation relationships. They must not create competing or ambiguous authoritative statutory records.
96. Consolidated reporting across eligible Books or Legal Identities is allowed only under applicable accounting/reporting rules and explicit authority. It does not merge underlying Books, Legal Identities, ownership, or Business/Tenant Data Boundaries.
97. Accounting Context, Book applicability, access, and posting authority are not inferred from Organization, Distributor, Shop, Tenant, Owner, User Role, Subscription Coverage, Service Provider association, or Distributor–Shop Mapping.
98. The applicable Accounting Framework is selected according to actual legal/entity classification, regulatory status, sector, reporting purpose, and effective authoritative requirements. Accounting framework or policy configuration cannot override mandatory requirements.
99. The default Financial Year remains 1 April through 31 March, subject to Section 6B and applicable domain requirements. Accounting Periods and closing controls are owned by Accounting.
100. The Chart of Accounts is hierarchical and includes Account Groups and Ledger Accounts. Protected foundational classifications and authorized business-specific extensions must coexist without undermining reporting or framework integrity.
101. Voucher Types classify Accounting Vouchers and govern numbering, structure, validation, approval, and posting. Authorized custom Voucher Types cannot bypass system controls, applicable Accounting Framework requirements, or source-domain ownership.
102. Every posted Accounting Voucher has a balanced set of debit and credit entries and a clear Book, period, source/provenance, and authorized posting reference. Voucher, Journal Entry, and Ledger Posting are related but distinct concepts.
103. Posted Accounting records must not be silently overwritten or destructively edited. Corrections require traceable, authorized reversal, adjustment, or correction records linked to the original record.
104. General Ledger balances are authoritative within a Book. Sub-ledger and bill-wise/outstanding detail must reconcile to the corresponding control Ledger Accounts. Original Customer/Supplier identities and commercial invoices remain owned by their source domains.
105. Cost Centres, Cost Categories, and other Accounting Dimensions are analytical accounting constructs. They do not create or alter Business Entities, Legal Identities, Business Structure, Ownership Links, User Scope, or Permissions.
106. Budgets and forecasts are planning records. Their creation or approval does not itself post a financial transaction or change the General Ledger; Accounting supplies actuals and accounting analysis but does not become the owner of enterprise-wide planning.
107. Accounting owns fixed-asset financial recognition and treatment, including capitalization, depreciation, impairment, revaluation where applicable, and disposal records. Physical asset custody and operational lifecycle remain separate Asset Management responsibilities.
108. Accounting owns its financial statement calculations and accounting reports. Shared Reporting Infrastructure may distribute or analyse them but does not assume accounting data or calculation ownership.
109. Sales Management, Purchase/Procurement Management, Point of Sale, Product/Item Management, Inventory Management, Warehouse Management, GST/Tax, Finance/Payment, Human Resource Management, Payroll Management, Subscription/Billing, and other domains retain ownership of their originating transactions, documents, operational states, statutory determinations, and source records. Accounting owns only the resulting accounting interpretation and accounting records.
110. A source transaction, Event Message, API response, invoice issuance, payment success, stock movement, warehouse task completion, payroll calculation, or POS checkout does not by itself prove that the corresponding Accounting Voucher has been validated and posted.
111. Source-to-accounting mappings and posting rules must be authorized, versioned, effective-dated, idempotent, and traceable. Corrections to source transactions must not silently rewrite previously posted accounting records.
112. External-system exchange profiles and adapters are optional, governed translation boundaries. No external vendor's data model, terminology, workflow, customization, or product behaviour defines CBM's canonical Business Module or Shared Model architecture.
113. Customer onboarding data import must be limited to selected business-continuity data that is authorized, available, sufficiently reliable, and accepted by the relevant CBM domain. Missing, unsupported, ambiguous, or insufficiently detailed data must be exposed for authorized completion; CBM must not fabricate source facts.
114. Customer offboarding export must make the selected business-continuity data held in CBM and authorized for export by the customer/business available in documented, validated formats supported by the intended destination where such support exists. Export does not promise recreation of a destination vendor's configuration/customizations or proprietary backup unless that exact capability is officially supported and validated.
115. Accounting Data Migration must identify its source, accountable identity, Book, period, mapping, and batch; stage, validate, detect duplicates, handle exceptions, reconcile, obtain authorization, preserve provenance, and verify accepted records.
116. Imported historical data is not authoritative posted accounting merely because it was uploaded. It becomes accepted/posted only after required validation, reconciliation, and approval. Unsupported or ambiguous records remain explicit exceptions.
117. Migration retries must be duplicate-safe and idempotent within their defined source and batch identity. Source identifiers, voucher references, dates, narrations, and available provenance must be preserved.
118. CA/Auditor access and Audit Data Package creation require explicit, purpose-limited, Business/Legal Identity, Book, period, and record scope, with read-only access by default. A CA/Auditor title alone grants no access.
119. An Audit Data Package is a scoped data/report export, not an audit opinion, certification, statutory return, or assurance of compliance. Cross-domain supporting records are included only through their owning Module's authorized contract and Permissions.
120. Accounting record retention follows the applicable law for the accountable entity and record category plus authorized platform policy. No single retention period is presumed to apply to every CBM business or record.
121. Accounting operates within the fixed India-only and INR-only product boundaries. Foreign-currency accounting and multi-currency operation are outside the approved scope; source data containing foreign-currency transactions must not be silently converted to INR.
122. Business/Tenant Data Isolation, Legal Identity ownership, Book ownership, Double-entry integrity, posting traceability, and protected Accounting controls are non-overridable.
123. Business Modules, Shared Domain Models, Internal Platform Capabilities, Application Client Channels, External Connectivity Capabilities, and Cross-cutting Functions are distinct architectural classes. A Shared Model, Platform Service, client application, connector, or report is not automatically a Business Module.
124. Accounting Management, Product/Item Management, Inventory Management, Warehouse Management, Human Resource Management, and Payroll Management are separate domain owners. Product/Item Management owns Product/Variant catalogue identity and definitions; Inventory owns operational stock facts and traceability; Warehouse owns physical handling and execution. HR owns employment/personnel facts, Payroll owns payroll calculations/results, Finance/Payment owns payment execution/settlement, and Accounting owns financial postings.
125. Inventory Management owns authoritative stock quantities, stock states, reservations/availability, inventory movements, and valuation. Warehouse Management owns warehouse facilities/layout, storage locations, physical placement, handling units, and warehouse execution. Neither Module may replace the other's authoritative records.
126. Product/Item definitions, Inventory Position, and Warehouse physical placement are distinct concepts. Inventory and Warehouse consume the Product/Item reference but retain their own domain-specific stock and handling facts.
127. A simple Inventory Storage Location/godown is not automatically a fully configured Warehouse Management facility. Advanced WMS capabilities are optional by business applicability and must not be forced on every Organization, Distributor, or Shop.
128. The Warehouse Location hierarchy may include optional facility, warehouse, zone, storage type/section, aisle, rack, shelf/level, bin/slot, dock, and staging area levels. A Bin is the smallest independently addressable storage position where bin-level control is enabled; facilities need not use every level.
129. A Warehouse Counter/Workstation is an operational work point, not automatically a Storage Bin or stock location. A POS Counter is a separate POS responsibility. Stock physically held at a counter must have an explicit stock-holding location reference.
130. Internal/external warehouse classification must distinguish facility operator/control, physical/geographic relationship, property/occupancy relationship, inventory ownership, physical custody, and operational access. No single internal/external flag may be used to infer all these relationships.
131. External/third-party warehouse custody does not transfer stock ownership or confer access to the business's full Inventory or other Tenant data. Stock owner, custodian, facility operator, and authorized Users are separately identified.
132. Inventory quantity and valuation remain authoritative in Inventory Management. Warehouse physical placement and task execution remain authoritative in Warehouse Management. Shared Inventory Position and Warehouse Location contracts reconcile the respective views without creating duplicate stock ledgers.
133. Warehouse physical count or task completion is not itself an Inventory adjustment or proof of stock-ledger posting. Inventory changes require Inventory validation and authorization; warehouse execution evidence must be traceable.
134. User Account is distinct from Employee; HR job title is distinct from CBM Role; Role is distinct from Permission; Authentication is distinct from Authorization; Approval is distinct from Permission; Subscription Entitlement is distinct from User Permission.
135. User/Identity, Role, Permission, Approval, Configuration, Notification, Event, API, Audit, Search/Reporting, File/Document, and Background Processing functions are Internal Platform Capabilities under Section 6A and relevant governance sections. They must not be duplicated as independent Business Modules without a separate domain-owner justification.
136. Reports and Analytics infrastructure is shared Platform-wide capability. The domain meaning, rules, authoritative calculations, and source data for each report remain with its Business Module.
137. Web, desktop, mobile, handheld, and other Application Clients are access/presentation channels. They must use authorized CBM interfaces and may not bypass Business Module contracts or access authoritative storage directly.
138. External-system connectors, device integrations, and Portal-to-Portal interoperability use shared Integration & API/Event & Messaging capabilities while retaining domain-specific adapters, mappings, validation, authorization, and revocation. A vendor-specific adapter is implemented only where a supported business need justifies it.
139. External data must be translated into the appropriate CBM domain contracts. Source-specific Inventory, storage, Accounting, Payroll, and other structures must not redefine CBM's canonical models or be mapped beyond the semantics and detail actually supported by the source.
140. Portal-to-Portal exchange or central aggregation must preserve the source portal's authoritative ownership. Every dataset/field requires a source of truth, purpose, recipient, scope, mapping, provenance, synchronization policy, conflict rule, retention treatment, and revocation control. Portal federation does not merge Tenant, Business Identity, User, ownership, or authority boundaries.
141. Shared Domain Models are contracts and common concepts, not universal data stores or permission grants. Every shared model identifies field/entity ownership, authorized consumers, permitted operations, versioning, provenance, and applicable context.
142. A Source Transaction Reference, External Identifier, API credential, event subscription, Application Client registration, device identity, or synchronization relationship does not itself grant Business Authority, User Scope, Role, Permission, ownership, or unrestricted data access.
143. Inventory Management, Warehouse Management, Human Resource Management, Payroll Management, Product/Item Management, Sales Management, Purchase/Procurement Management, Point of Sale, GST/Tax, Finance/Payment, Asset Management, Business Planning, and other distinct domain responsibilities must not be merged merely because they share a workflow, data reference, report, application client, or integration.
144. A cross-Module process must preserve each Module's separate state, source record, validation, authorization, and correction lifecycle. Event/API delivery does not prove a domain transaction or downstream posting has completed.
145. Shared Product/Item, Unit/Packaging, Location/Facility, Inventory Position, Source Transaction, Accounting Posting, Workforce, External Identifier, Event, and Document references must have explicit authoritative owners and contracts; shared use never implies shared write authority.
146. Business Data Isolation, Tenant Data Isolation, Main Portal governance, explicit authority, domain ownership, and Stable Core rules apply to all internal and external integrations, client applications, warehouse operators, third-party providers, and portal synchronization.
147. CBM must provide a coherent unified portal experience by composing independently governed Business Modules, Shared Domain Models, Internal Platform Capabilities, and External Connectivity. Unified access or workflow orchestration does not merge Module ownership, source records, business authority, Tenant boundaries, or Permissions; each capability remains subject to explicit applicability and authorization.

148. Product/Item Management is an independent Business Module and owns Product Definitions, Product Variants, catalogue attributes/classifications, product/variant identifiers, Trade Packaging Configurations, and catalogue lifecycle. The Product/Item Reference Model is a Shared Domain Model, not a second shared-write Business Module.
149. Product Definition, Product Variant, Product/Item Instance, Product Family/Group/Classification, Trade Packaging Configuration, Product Bundle/Kit, Batch/Lot, Serial Identifier, and Warehouse Handling Unit are distinct concepts and must not be treated as interchangeable hierarchy levels.
150. A Product Definition must support a standard/default Variant where no meaningful variation exists. A transaction or stock record requiring a concrete operational item must resolve to an appropriate Variant or separately governed trade-unit reference; a Variant is not an individual physical occurrence.
151. Product Family/Group and taxonomy membership are optional, purpose-specific classifications. They do not establish a mandatory single-parent Product hierarchy and do not alter CBM's Business Structure Hierarchy.
152. Product/Item Identifiers are typed and scoped. Product Code, Item Code, SKU, PLU, MPN, supplier/customer codes, GTIN, and other external or industry identifiers are not interchangeable merely because their values look alike. Their issuer, subject level, namespace, purpose, validation, uniqueness scope, provenance, and lifecycle must be explicit.
153. Barcode, QR, RFID, NFC, and other Data Carriers are distinct from the identifier values and data attributes they carry. A scanned value or carrier does not automatically identify a Product or authorize a business transaction.
154. GTIN is a GS1 trade-item identification key and must follow applicable GS1 allocation and validation rules. CBM must not fabricate or allocate a GS1 identifier without the applicable licence and authority; a GTIN is not mandatory for every CBM Product or Variant.
155. An identifier subject may have multiple valid identifiers and multiple Data Carriers. Uniqueness is enforced within each identifier type's declared namespace/scope; significant characters and leading zeroes are preserved.
156. Product/Item Management owns catalogue-level Product/Variant identity and product-side tracking characteristics. Inventory Management owns operational stock quantities, stock states, valuation, batch/lot records, and accepted serial/instance traceability; Warehouse Management owns physical placement, Handling Units, and warehouse execution.
157. Batch/Lot identifies a group of units; a serial identifier identifies an individual unit or component within its declared issuer/type/scope. Batch and serial tracking may be used together. One unit may have multiple identifiers, and component/interface identifiers must remain associated with the correct component/interface subject.
158. Quantity-based items do not require a separately persisted Product/Item Instance for every physical unit unless traceability, operational, contractual, or legal requirements require unit-level records. CBM must not fabricate missing unit identifiers or serial values during import.
159. Trade Packaging Configurations, Units of Measure, Product Variants, and Warehouse Handling Units are distinct. Separately traded packaging levels may require their own identifiers; conversions and stock effects must prevent double-counting physical contents.
160. Product Bundles/Kits must distinguish physical fixed compositions, virtual offers, and configurable/dynamic compositions. Their stock and fulfilment effects remain with the relevant transaction and Inventory contracts; a composition record is not automatically a manufacturing Bill of Materials.
161. Product/Variant identity is not a transaction price, purchase cost, inventory valuation, discount, or tax determination. Each remains owned by the applicable Sales, Purchase, Inventory, Accounting, or GST/Tax domain.
162. Product attributes, variant dimensions, valid combinations, identifier schemes, packaging, and catalogue customizations must be configurable and extensible under explicit authority, applicability, versioning, and permissions. Flexibility must not weaken Stable Core, identifier integrity, historical references, module boundaries, or Tenant Data Isolation.
163. Catalogue publication, sharing, inheritance, local additions, and overrides require explicit governed relationships and authorization. Organization/Distributor/Shop structure and Service Provider association alone do not grant catalogue access or edit authority; no cross-Tenant sharing is implied.
164. Product/Variant changes, identifier retirement, catalogue merges, packaging changes, and replacement/supersession must preserve effective history and must not silently rewrite accepted transactions, stock facts, physical instances, or another business's catalogue.

165. The Price Reference and Price Determination Contract is a Shared Domain Model/contract and does not itself establish a separate Pricing Business Module or a universal authoritative Price Master.
166. Every Price Record has an explicit Price Type, authoritative owner domain, subject, business/applicability scope, INR amount, quantity/UOM/packaging basis where relevant, tax basis where applicable, source/provenance, lifecycle, and effective/recorded times.
167. Price Types with different business meanings—including MRP, RRP/MSRP, Selling Price, Supplier Quotation/Agreement Price, Actual Purchase Price, Landed/Acquisition Cost, and Inventory Valuation—must not be conflated or stored as one ambiguous Original Price field.
168. Product/Item Management owns declared MRP/RRP and other applicable Product/Packaging price declarations; Sales owns selling Price Lists and sales conditions; Purchase owns supplier quotations/agreements and purchase transaction prices; Inventory owns cost formation and valuation; GST/Tax owns tax determination; Accounting owns accounting records and postings.
169. A Price Record must identify whether it applies to a Product, Product Variant, Trade Packaging Configuration, or another expressly permitted subject, and must preserve its quantity and unit/packaging basis.
170. MRP, RRP/MSRP, and actual transaction Selling Price are distinct. MRP applies only where required by applicable law; for covered pre-packaged retail commodities it is declared inclusive of all applicable taxes and the commodity must not be sold above its applicable MRP. Legal applicability/exemptions must not be guessed or generalized to every item.
171. Legally required unit-sale-price declarations, where applicable, are distinct from the actual transaction price and must preserve the declared measurement basis and applicable statutory format/rounding.
172. A Price Tax Basis of Inclusive or Exclusive describes how a price amount is represented. It does not establish GST applicability, HSN/SAC, tax classification, tax rate, Input Tax Credit entitlement, or tax liability; those are GST/Tax responsibilities.
173. Tax-inclusive supply values must be treated under the applicable statutory valuation rules, including CGST Rules Rule 35 where it applies. Price arithmetic does not replace GST valuation law.
174. Sales and Purchase may maintain multiple purpose-specific Price Lists and Price Conditions for businesses, customers, suppliers, contracts, channels, locations, quantities, units/packages, and validity periods where applicable. Price priority, combination, exclusivity, fallback, and conflict resolution must be deterministic and governed.
175. Discounts, promotions, surcharges, rebates, and other adjustments must remain separately represented as governed conditions or transaction adjustments; they must not destructively rewrite the underlying base/list Price Record. GST treatment remains governed by GST/Tax and applicable law.
176. Price Records support Effective Time, Recorded Time, and approval/publication lifecycle where applicable. Scheduled future changes and authorized corrections must preserve old versions and must not silently backdate or rewrite completed transactions.
177. Last Purchase Price and Average Purchase Price are derived references with explicit calculation scope and methodology, not manually maintained authoritative replacements for Purchase transaction facts.
178. Landed/Acquisition Cost and Inventory Valuation are Inventory-owned results/policies and are not automatically equal to supplier invoice price, list price, or MRP.
179. Accepted Sales, POS, and Purchase transactions preserve immutable Price Snapshots with the applicable price/condition versions, unit/quantity basis, discounts/surcharges, tax basis and Tax-domain result references, accepted values, and provenance. Later price changes do not rewrite accepted transactions.
180. Product/Item Declared Price History, Sales Pricing History, Purchase Pricing History, and Inventory Cost and Valuation History are separate domain histories owned by their respective Modules. They are not merged into one mixed Price History record set and do not replace transaction records or the comprehensive Audit Trail.
181. Price changes, approvals, overrides, failures, and access to sensitive price history are captured in the comprehensive Audit Trail independently of each domain's Price History.
182. Cost, margin, supplier terms, and other commercially sensitive price information require explicit purpose-limited permissions and must not be visible to every User who can view or sell a Product.
183. Price List publication, inheritance, sharing, and local override require explicit authority and applicability. Organization/Distributor/Shop structure, Service Provider association, or Distributor–Shop Mapping alone does not grant price access or editing authority; strict Business/Tenant Data Isolation remains mandatory.
184. All authoritative CBM prices operate in INR only. No Price List, Price Record, override, User preference, or lower-level configuration may introduce a foreign currency or multi-currency business operation.

185. A real-world subject may have multiple distinct aspects, records, and lifecycles. CBM must separate them into the appropriate authoritative domain models when their business meaning, responsibility, owner, applicability, permissions, validation, history, or configuration needs differ; they must be connected through explicit, typed, versioned references and contracts rather than collapsed into one oversized master or Module.
186. Domain decomposition must be purposeful, not indiscriminate. Cohesive facts with the same business responsibility, owner, lifecycle, and validation may remain together; a separate Module or model must be justified by a real responsibility boundary, not merely by another field or screen.
187. A composed workflow, screen, report, or calculation may combine independently owned domain facts through authorized contracts. It must not transfer ownership, create competing sources of truth, grant shared-write authority, bypass permissions, or weaken Business/Tenant Data Isolation.
188. Product Variant is the normal reference subject for goods-specific price records and HSN/SAC classification assignment. Price records and tax-classification assignments remain separate domain-owned records linked to the Variant; they are not untyped Product fields or evidence that Product/Item Management owns pricing or tax determination.
189. HSN and SAC are distinct classification types for goods and services. An HSN/SAC code is a regulated tax-classification reference, not a Product taxonomy, SKU, GTIN, Product Code, barcode, or GST rate.
190. Every HSN/SAC code assignable to a CBM goods Variant or service/supply subject, or selectable for CBM GST Tax Determination, must contain at least six digits. This minimum is non-configurable and non-overridable Stable Core. Codes shorter than six digits are not valid CBM assignments.
191. The six-digit minimum does not authorize synthetic code creation. Exact full official codes must be preserved; valid longer codes must not be truncated, shortened, padded, or fabricated. A classification that cannot be verified as an official code of at least six digits remains unresolved and must not be guessed or silently substituted.
192. HSN/SAC Registry entries, Product/Service Tax Classification Assignments, GST Rate Applicability Rules, and transaction GST Tax Determinations are distinct records with distinct purposes and owner boundaries. Product/Item Management references the applicable assignment for goods Variants; GST/Tax owns the registry, assignment rules, rate rules, and tax determinations.
193. A GST Rate Applicability Rule is not a permanent property of an HSN/SAC Code or Product Variant. A code or tariff scope may relate to multiple rate rules across effective periods or statutory descriptions/conditions; code-only matching must not replace required description, supply, package, or legal-condition checks.
194. GST Rate Applicability Rules must preserve statutory sources/notifications, rate components, conditions, exclusions, relevant tax treatment, effective dates, recorded dates, versioning, and correction/supersession history. Conflicts or missing facts must produce an explicit exception, not an arbitrary rate selection.
195. Tax Inclusive/Exclusive remains an explicit Price Tax Basis. Purchase price references should normally default to Tax Exclusive, while source documents and accepted purchase transactions must preserve their actual declared basis. Sales prices may be Tax Inclusive or Tax Exclusive. No generic module default may silently change a source-declared basis.
196. GST Tax Determination must evaluate the applicable HSN/SAC assignment, rate-rule version, legally relevant supply/transaction facts, tax timing, and Price Tax Basis. A Product Variant's code reference alone is not a complete tax determination.
197. HSN/SAC Registry History, Product/Service Tax Classification Assignment History, and GST Rate Rule History are separate domain histories. They remain separate from Product, Sales, Purchase, and Inventory price histories, from transaction snapshots, and from the comprehensive Audit Trail.
198. Accepted transactions requiring GST treatment preserve an immutable Tax Determination Snapshot containing the complete code and assignment version, selected rate rule/version and source, applicable conditions, determination context, taxable value, tax components/amounts, and provenance. Subsequent product, classification, rate, or price changes must not rewrite the accepted snapshot.
199. External data, vendor tax tables, Product names, prior transactions, or imported text must not automatically establish or replace an HSN/SAC assignment or GST rate. Authoritative statutory source validation, explicit effective dating, approval where required, and traceable provenance are mandatory.





## 20. Illustrative scenarios

These scenarios explain how the finalized rules work together. They do not create additional authority or ownership rights beyond the rules above.

### 20.1 One owner, multiple independent business architectures

Person A and Person B own Organization A. The same Owner ID set—Person A and Person B—must be linked separately to every Distributor and Shop directly under Organization A, with separate Ownership IDs for each Business Identity. Person A may also own a different Organization B as a separate Customer Business Architecture, with its own internally consistent Owner ID set. Separate Business Identities and Data Boundaries remain independent even when some Owner IDs are shared.

### 20.2 Organization with multiple Shops

Organization A has Shop S1, Shop S2, and Shop S3 as direct structural children. Each Shop has its own Professional Identity and operational context. The Owner ID set linked to each Shop must exactly match the Owner ID set linked to Organization A, while each Shop retains its own Ownership Links and distinct Ownership IDs. Its own Legal Identity is optional and must be explicitly associated and validated where applicable. Matching ownership does not merge the Shops' Professional Identities or operational records. The Organization's Business Available Authority Scope and each User's assigned scope are determined independently under the applicable authority rules.

### 20.3 Organization, Distributor, and mapped Shop in another Organization

Organization A has Distributor D1 and Shop S1 as direct structural children. Organization A, D1, and S1 have the same Owner ID set, with separate Ownership Links and Ownership IDs for each Business Identity. Organization B has Shop S2 under its own Customer Business Architecture and its own internally consistent Owner ID set. D1 and S2 may establish a Distributor–Shop Mapping through the approved two-way verification process. The Mapping does not make S2 a structural child of D1 or Organization A, does not change either Organization's ownership-consistency rule, and does not merge the businesses' data boundaries. Neither D1 nor S2 needs to select, know, or disclose the other party's Parent Organization to establish the Mapping. Any authority eligibility beyond the expressly permitted Mapping interaction must be established under the existing Main Portal-governed Authority Scope rules.

The Organization's direct structural relationship with Shop S1 is independent of the Distributor–Shop Mapping. The Mapping itself does not enlarge either party's authority scope.

### 20.4 Selective User scope within one business

Organization A's Business Available Authority Scope includes Distributor D1 and Shops S1, S2, and S3. User U1 is assigned only S1; User U2 is assigned D1 and S2; User U3 is explicitly assigned the full available scope. Their Organization-level association does not automatically give them identical access. Each User's effective access is further limited by that User's Permissions and applicable approvals.

### 20.5 Owner identity is not a User account

Person A is linked as an Owner to Organization A and Shop A through separate Ownership Links, and the Shop's Owner ID set matches Organization A's Owner ID set. This does not automatically create a login account or grant operational access. If Person A uses CBM, a valid Organization-scoped User Account, User–Business association, User Assigned Authority Scope, and Permissions are still required.

### 20.6 Sub-Portal serving an Organization and related business network

Sub-Portal A serves Organization A and the relevant associated Distributor and Shop entities within its authorized domain. Its service-provider authority scope covers those associated businesses under Main Portal governance. This does not make Sub-Portal A their owner or grant every Sub-Portal User unrestricted access to every record.

### 20.7 Different approval rules for the same action

Within one Organization, the same action may require approval for Role A, may not require approval for Role B, and may have a separate user-specific approval rule for User C. The Organization configures these rules within the governing limits. The initiating User must still possess the action Permission and the assigned scope for the target entity.

### 20.8 Configuration override for one business only

Main Portal establishes a default setting. Organization A has explicit permission to override it and sets a permitted value within the system-defined range. Organization B has no override permission and continues to use the applicable higher-level value. Organization A's change does not silently change Organization B.

### 20.9 Service Provider migration

Organization A moves from Sub-Portal A to Sub-Portal B under Main Portal governance. Organization A's Business Identity, ownership, internal structure, data rights, and valid historical relationships remain intact. The Service Provider relationship changes.

### 20.10 GST Registration Assignment at different CBM levels

The GST Registration Assignment capability is not limited to Distributor–Shop relationships. Subject to legal eligibility and Main Portal policy, a GST Registration may be associated with the legal person/context operating Main Portal, a Sub-Portal service provider, an Organization, a Distributor, a Shop, or another enabled CBM entity/context. The GST Registration always belongs to the actual registered taxpayer. The CBM association identifies an eligible operational or transaction context; it does not change the taxpayer or permit an unrelated entity to use that GSTIN. No Owner ID, Business Structure, service-provider relationship, or other Mapping automatically establishes GSTIN eligibility.

### 20.11 Domain History and Audit Trail are separate

When a GSTIN is changed, GST Registration History records only the GST Registration-domain change, including previous and new registration/assignment, effective date, recorded date, reason, actor, and approver where applicable. A User ID change is not inserted into GST Registration History. The User ID change and GSTIN change may both appear as separate events in the comprehensive Audit Trail, in chronological order. Authorized business Users may view GST Registration History without being permitted to view Audit Trail.

### 20.12 Two-way Distributor–Shop Mapping verification

Shop S1 already exists in CBM under its own Parent Organization and has its own Shop Unique ID. Distributor D1 also already exists in CBM under its own Parent Organization and has its own Distributor Unique ID. S1 generates a Shop Secret Code and shares its Shop Unique ID and code with D1. An authorized D1 User enters them in D1's CBM context, and CBM verifies the first stage. D1 then generates a separate Distributor Confirmation Code for that pending mapping and shares it with S1. An authorized S1 User enters that confirmation code in S1's CBM context. Only after this second verification succeeds does the mapping become Active.

Neither party needs to select, know, or disclose the other party's Parent Organization. Both codes are subject to expiry, one-time use, rate limiting, and revocation. Once Active, the mapping enables only the business transactions and necessary information exchange authorized for its Mapping Type; it does not change either party's ownership, structural membership, Parent Organization, or independent data boundary.

### 20.13 Parent Organization deactivation and exact child restoration

Organization A has Shop S1, Shop S2, and Distributor D1. Before Organization A is deactivated, S1 is already Deactivated, while S2 and D1 are Active. Organization A's deactivation operation captures a Lifecycle Operation Snapshot. S1 remains Deactivated and is recorded as unchanged by that operation. S2 and D1 are changed to Deactivated by the Parent-Caused Cascade. Any Active Distributor–Shop Mappings involving the affected children are also deactivated, with their prior states captured.

When Organization A is later reactivated, CBM restores only the child entities and Mappings changed by that exact deactivation operation. S2 and D1 return to their recorded pre-operation states. S1 remains Deactivated because it was already Deactivated before the Parent operation. A Mapping that was already Deactivated before the Parent operation is not reactivated. A cross-Organization counterparty remains operational; only the Mapping involving the deactivated endpoint changes.

### 20.14 Child Deactivation does not affect its Parent or siblings

Organization A has Distributor D1, Shop S1, and Shop S2. Deactivating Shop S1 changes only S1 and the Distributor–Shop Mappings involving S1. Organization A, D1, and S2 remain in their existing states. A different Mapping between D1 and S2 is unaffected. Reactivating S1 can restore only the Mappings changed by S1's deactivation operation, subject to the current state of the other endpoints.

### 20.15 Subscription suspension does not deactivate a Business Entity

Organization A has an Active Subscription that covers Organization A and Shop S1. The Subscription enters Grace Period and is later Suspended under its billing policy. Organization A and S1 retain their Business Entity lifecycle states; they are not automatically Suspended, Deactivated, or Closed. The applicable Entitlements become unavailable or restricted as defined by the Subscription policy. User Accounts, ownership, Business Structure, and historical business data remain separate and intact.

### 20.16 One natural person with two Organization-specific User Accounts

A natural person works for Organization A and Organization B. Organization A provisions User Account UA with its own verified email, mobile number, credentials, and portal-wide unique User Code. Organization B independently provisions User Account UB with different email and mobile details and a different portal-wide unique User Code. UA belongs only to Organization A's Customer Business Architecture; UB belongs only to Organization B's Customer Business Architecture. Neither account can be used to access the other Organization. Terminating UA does not terminate UB.

### 20.17 Organization-configured initial credential methods

Organization A configures OTP Password Setup. A new User Account under Organization A completes OTP verification using an enabled verified channel and sets a permanent password inside the CBM application. Organization B configures Both Required. Its new User must successfully complete both the Email Temporary Password bootstrap process and the OTP verification process before normal account use is enabled. Neither process sends a URL or link. If a Temporary Password password-change save attempt fails, the Temporary Password remains valid only for the restricted password-establishment process until success, expiry, or revocation.

### 20.18 Immutable portal-wide User Code

Two separate User Accounts are provisioned for the same natural person in two different Organizations. The system allocates two different six-character User Codes, each having two uppercase English letters followed by four digits. Both codes are globally unique across CBM, are generated by the system, are read-only and immutable, and are never reused. The User Code identifies an account; it is not a secret or a Permission.

### 20.19 India-only Global Business Settings and separate governance paths

Main Portal establishes English as the platform default, enables selected Indian languages, and enforces INR-only and IST-only platform boundaries. Organization A receives service through Sub-Portal SP1, but SP1's delegated service-provider settings do not make SP1 the structural parent of Organization A or permit SP1 to override Organization A's business settings. Organization A establishes its permitted business-calendar and working-day defaults; its Distributor D1 and Shop S1 inherit those defaults unless an explicit Permission allows a context-specific override. A User may select an available Indian-language display preference but cannot select another time zone or currency. Accounting and Human Resource Management and Payroll Management continue to own their respective authoritative financial-year, statutory, payroll, and domain-calendar rules.


## 21. Documentation governance

### 21.1 Documentation completeness

Project documentation must be sufficiently precise that a reader can understand each included concept without relying on undocumented assumptions. As applicable, definitions, purpose, scope, ownership, authority, constraints, relationships, behaviour, exceptions, and examples must be stated explicitly.

### 21.2 No unresolved matters in the master decision record

The master decision record contains finalized decisions only. An unresolved matter must be discussed and explicitly finalized before it is added. It must not be represented as a decision, and it must not be silently filled by an AI recommendation or an assumed technical convention.

### 21.3 Separate Project Discussion History from domain records and decisions

Chronological project discussions, reasoning, rejected approaches, and superseded wording belong in a separate **Project Discussion History** record if one is maintained. Project Discussion History is not Domain History and is not the Audit Trail. Neither Domain History nor Audit Trail belongs in the authoritative decision text as a substitute for project discussion history.

### 21.4 Downstream documentation consistency

Every later project document must:

- Use the canonical definitions in this master document.
- Preserve the ownership, authority-scope, permission, configuration, approval, and isolation distinctions.
- Apply the architecture-wide design doctrine: evaluate scalability, configurability, extensibility, customization, override capability, and applicability.
- Treat Data Isolation and Business Structure Hierarchy and Structural Integrity as strict, non-overridable Stable Core constraints.
- Identify the master decisions on which it depends.
- Avoid introducing contradictory or broader rights by implication.
- Be updated when a governing master decision changes.

### 21.5 Change control

A proposed change is not final merely because it has been suggested or documented in a discussion. The project owner must explicitly approve it. After approval, the master source of truth is updated first, and dependent documents are reconciled.

---

## 22. Source basis and decision provenance

This baseline consolidates:

1. The user-provided project context document **CBM_Project_Context_and_Decisions_2026-11-17** (provided in the conversation as the CBM project context text), including its confirmed business, service-provider, ownership, mapping, continuity, migration, archival, and Stable Core principles.
2. The subsequent project-owner confirmations and clarifications in the conversation through **18 November 2026**, including the unified Shop entity and simplified Business Structure (Organization → Distributor / Shop); exactly one mandatory Parent Organization for every Distributor and Shop; identical Owner ID sets for an Organization and all of its direct Distributor/Shop children within the Customer Business Architecture, while Service Provider ownership remains independent; the Legal Identity and Professional Identity applicability matrix; Organization Network Configurations; GSTIN eligibility independence; generic GST Registration Assignment; two-way Distributor–Shop Secret Code verification with expiry, one-time use, rate limiting, and revocation; Mapping-Type-limited transactions and information exchange; Business Entity Lifecycle parent-child cascades and immutable restoration snapshots; separate Subscription, Plan, Add-on, Entitlement, Coverage, Usage Limit, Invoice, and Payment concepts; separate User Account, User–Business Association, and Credential/Authenticator lifecycles; Organization-configurable initial credential establishment; one Organization-scoped Customer Business User Account per account; immutable portal-wide User Code; the absolute no-URL/no-link outbound communication rule; explicit User Scope Mapping; and the strict separation, contents, access, and retention distinctions between Domain History and Audit Trail.
3. Official GST reference material used for the logical architecture boundary: Section 25 of the CGST Act, applicable CGST Rules for separate registration at multiple places of business, and GST Portal taxpayer-search and registration guidance.
4. The project owner's explicit approval on 18 November 2026 of the Platform-wide Services Architecture Layer, its four Service Scopes, three Category Groups, fifteen Categories, and the Category → Service → Capability conceptual hierarchy, including the responsibility boundaries and cross-category safeguards recorded in Section 6A.
5. The project owner's explicit approval on 18 November 2026 of the India-only Global Business Settings scope, its six approved setting domains, three distinct governance paths, non-overridable India-specific platform boundaries, and the separation between shared defaults/reference settings and Business Module-owned rules recorded in Section 6B.
6. The project owner's explicit approval on 19 November 2026 of Accounting Management as an independent Business Module and its complete business/logical architecture scope recorded in Section 6C, together with the owner's delegation to make and document the necessary subordinate accounting-architecture decisions. Under that delegation, accrual was selected as the default recognition basis for general-purpose Books, subject to legal and Accounting Framework applicability. The approved scope includes Double-entry by default, CBM-defined Accounting Contexts and Books of Accounts, Accounting Controls, Reconciliation, Financial Reporting, Management Accounting, Fixed Asset Accounting, source-domain ownership boundaries, and governed migration/export and auditor support. These canonical concepts are defined by CBM business requirements, not by reproducing another Accounting System.
7. The project owner's explicit approval on 19 November 2026 that a Tally-specific data-exchange profile and adapter, if required, remain optional and separate from the canonical CBM Accounting Domain Model. Any such exchange is limited to supported, documented data transfer and does not imply native backup compatibility or reproduction of Tally functionality or customization.
8. The project owner's explicit direction on 19 November 2026 to separate Business Modules such as Accounting, Inventory, Human Resource, and Warehouse Management; distinguish Shared Domain Models from Internal Platform Capabilities and External Connectivity; preserve independently governed external-system and Application Client interoperability; and define warehouse facilities, storage hierarchy, counters, and internal/external custody boundaries. The project owner delegated the necessary subordinate architecture analysis and decisions, subject to previously approved Stable Core, governance, and documentation rules.
9. The project owner's explicit clarification on 19 November 2026 that CBM must not be designed as a copy of Tally or any other external system. External-system relationships are for importing the relevant available data needed to establish business continuity and exporting the relevant CBM data needed to continue business elsewhere. The source system's optional modules, data structures, customizations, accounting method, inventory granularity, warehouse concepts, and workflows do not define CBM's architecture. Missing or insufficient source data must be identified and completed through authorized CBM setup or data entry; data must never be invented. Vendor-specific compatibility is limited to the documented and validated data-transfer capability actually required.

10. The project owner's explicit direction and approval on 19 November 2026 to finalize the Product/Item concept and directly update the authoritative documentation. The approved baseline establishes Product/Item Management as an independent Business Module; distinguishes Product Definition, Product Variant, Product/Item Instance, optional Family/Group classification, typed identifiers, Data Carriers, Trade Packaging Configurations, and Bundles/Kits; and requires flexible, governed product, variant, identifier, packaging, and tracking policies within the existing Stable Core, tenant-isolation, authority, module-ownership, and documentation rules.
11. The project owner's explicit approval on 19 November 2026 of the HSN/SAC Classification, GST Rate Applicability, and Tax Determination architecture in Section 6H, including a separate HSN/SAC Code Registry, effective-dated Product Variant/service classification assignments, independently versioned statutory GST Rate Applicability Rules, a non-configurable minimum six-digit CBM HSN/SAC code requirement, preservation of exact full official codes without truncation or fabrication, separate classification/rate histories, and immutable transaction Tax Determination Snapshots.
12. The project owner's explicit approval on 19 November 2026 of the general domain-decomposition and composition rule in Sections 2.6.4 and 6D.2.1: one real-world subject may be referenced by many separately owned aspects and records; those aspects must be managed in the appropriate models/domains when their responsibilities, meanings, lifecycles, permissions, applicability, or histories differ, and then composed through explicit governed contracts. This is not an instruction to create a separate model for every field or to fragment cohesive domain responsibilities.
13. The project owner's delegation on 19 November 2026 to complete the remaining Business Module classification and subordinate architecture decisions wherever no indispensable project-owner business-policy choice remained, using research and reasoned selection where needed. Under that delegation, the complete fourteen-Module Catalogue and the ownership boundaries for Sales, Purchase & Procurement, POS Operations, GST/Tax, Finance & Payment, Asset Management, Business Planning & Budget Management, and Counterparty Management were finalized and recorded in Section 6I. This classification completes the previously identified distinct domain responsibilities without transferring their ownership to other Modules. It does not make every Module universally applicable and does not authorize software implementation or deployment.

Where a later explicit project-owner clarification refined an earlier general description, the later clarification governs. This includes the precise distinction between:

- Main Portal's ecosystem-wide authority scope;
- Sub-Portal authority scope over associated businesses in its authorized domain;
- Organization authority scope over its direct structural children, governed independently from Ownership and User Scope; a Distributor–Shop Mapping does not alter structural placement or itself confer authority; and
- Distributor Business Available Authority Scope includes the Distributor itself and may include associated Shops linked to the same Owner ID only under the explicit owner-linked eligibility rules in Section 7; Distributor–Shop Mapping alone does not confer that scope.
- GST Registration Assignment is a separate typed association that may target any CBM entity/context enabled by Main Portal; it is not limited to Distributor–Shop Mapping and is never inferred from Owner ID or another Mapping.
- User Scope Mapping determines the specific entities/locations assigned to an individual User; business-level authority availability alone does not assign that User's scope.
- Domain History and Audit Trail are separate records with separate contents, purposes, permissions, and retention rules.

Official GST references consulted for the legal architecture boundary:

- [CGST Act, Section 25 — CBIC Tax Information Portal](https://taxinformation.cbic.gov.in/content-page/explore-act/1000294/1000001).
- [CBIC consolidated CGST Rules, including Rule 11 amendment for multiple places of business](https://cbic-gst.gov.in/pdf/amended-01012022-CGST-Rules-2017-Part-A.pdf).
- [GST Portal Manual — Search Taxpayer using GSTIN/UIN](https://tutorial.gst.gov.in/userguide/taxpayersdashboard/Search_Taxpayer_manual.htm).
- [GST Portal Manual — Apply for Registration](https://tutorial.gst.gov.in/userguide/registration/Apply_for_Registration_Normal_Taxpayer.htm).

These references support the logical architecture boundaries. Current law and official GST Portal procedures must be rechecked before implementation.


Official accounting and interoperability references consulted for Section 6C:

- [India Code — Companies Act, 2013, Section 128 (Books of Account)](https://www.indiacode.nic.in/bitstream/123456789/2114/5/A2013-18.pdf?v=20260611082938): Section 128 states that companies must keep books on accrual basis and according to the double-entry system; this company-specific statutory requirement must not be generalized to every business type.
- [India Code — Companies Act, 2013, Section 129 (Financial Statements)](https://www.indiacode.nic.in/bitstream/123456789/2114/5/A2013-18.pdf?v=20260611082938): financial statement requirements and applicable notified standards for companies.
- [Ministry of Corporate Affairs — Companies (Accounting Standards) Rules, 2021](https://www.mca.gov.in/bin/dms/getdocument?mds=RKk43Bmg99ksfV0bUGr6XA%253D%253D&type=open): official notified Accounting Standards rules for the classes of companies to which they apply.
- [ICAI — Accounting Standards as on 1 April 2025](https://www.icai.org/post/accounting-standards-as): professional reference for Accounting Standards; applicability must be checked against the relevant entity and current law.
- [ICAI — Compendium of Indian Accounting Standards 2025–2026](https://www.icai.org/post/compendium-of-indian-accounting-standards): reference to Ind AS and amendments included in the cited compendium.
- [TallyHelp — Import Data from JSON or XML into TallyPrime](https://help.tallysolutions.com/import-data-from-xml-or-json/): current release-specific import formats and the distinction between Masters and Transactions.
- [TallyHelp — Export Data in TallyPrime](https://help.tallysolutions.com/export-data-in-tally/): structured and human-readable export capabilities.
- [TallyHelp — Import Data FAQ](https://help.tallysolutions.com/import-data-faq/): full-data exchange workflows and Masters-before-Transactions guidance.

The Tally references above are retained only as optional connector references for verifying documented import/export capabilities and format/version behaviour when a Tally exchange is actually required. They are not sources for CBM's canonical Accounting concepts, business rules, inventory model, warehouse model, or product architecture. Accounting standards, legal applicability, statutory retention, and external exchange-format compatibility must be revalidated against current authoritative sources before implementation or compliance use.

Official references consulted for Business Module separation, warehouse structure, and interoperability in Sections 6D–6E:

- [Microsoft Learn — Warehouse management overview](https://learn.microsoft.com/en-us/dynamics365/supply-chain/warehousing/warehouse-management-overview): distinguishes Warehouse Management processes from lighter Inventory Management capabilities and describes facility movement, location-level tracking, and warehouse work.
- [Microsoft Learn — Warehouse configuration overview](https://learn.microsoft.com/en-gb/dynamics365/supply-chain/warehousing/warehouse-configuration): describes configurable warehouse layout, zones, and location levels including aisle, rack, shelf, and bin.
- [SAP Help — Warehouse Structure in the Warehouse Management System](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/b2dee5e83e2446149294f9860a7c08f0/008bbd534f22b44ce10000000a174cb4.html?locale=en-US): describes warehouse hierarchy and storage types/sections/bins.
- [SAP Help — Storage Type](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/b2dee5e83e2446149294f9860a7c08f0/f78abd534f22b44ce10000000a174cb4.html): explains physical/logical storage areas and their relationship with storage bins.
- [SAP Help — Storage Bin](https://help.sap.com/docs/SAP_S4HANA_ON-PREMISE/9905622a5c1f49ba84e9076fc83a9c2c/c5dd5c540fe1033de10000000a441470.html): defines storage bins as independently identifiable storage positions.
- [OpenAPI Specification 3.2.1](https://spec.openapis.org/oas/v3.2.1.html): standard description of HTTP API interfaces; referenced as a possible interoperability contract standard, not a mandated implementation.
- [AsyncAPI Documentation](https://www.asyncapi.com/docs): standard description of message-driven/event-driven API interfaces; referenced as a possible contract standard, not a mandated event platform.
- [CloudEvents Specification](https://github.com/cloudevents/spec): vendor-neutral event data format and metadata for interoperability across services, platforms, and systems.
- [GS1 EPCIS / CBV Standards Repository](https://ref.gs1.org/standards/): EPCIS enables applications to share visibility event data within and across enterprises; relevant where supply-chain traceability use cases justify it, not a universal mandatory standard.
- [IETF RFC 8252 — OAuth 2.0 for Native Apps](https://www.rfc-editor.org/rfc/rfc8252.html): best current practice for native application authorization using an external user-agent.
- [IETF RFC 9700 — Best Current Practice for OAuth 2.0 Security](https://www.rfc-editor.org/rfc/rfc9700.html): current OAuth security recommendations, including PKCE and redirect protection.
- [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html): identity layer on OAuth 2.0 for interoperable user authentication.
- [Microsoft Learn — Principle of least privilege](https://learn.microsoft.com/en-us/entra/id-governance/scenarios/least-privileged): supports minimum necessary access for users, applications, and agents; referenced as a security design principle rather than a required vendor product.

These references inform logical boundaries and interoperable contracts. They do not mandate Microsoft Dynamics, SAP, Tally, GS1, OAuth/OIDC, or a particular deployment design. Exact protocol profiles, product versions, legal requirements, and device integration methods must be validated during the authorized technical design stage.

Official references consulted for Product/Item Management, product identity, variants, identifiers, and barcodes in Section 6F:

- [GS1 General Specifications, Release 26.0 (ratified January 2026)](https://ref.gs1.org/standards/genspecs/): current GS1 reference for trade-item identification, GTIN allocation, identification levels, packaging/trade-item groupings, serialisation, identifier attributes, Application Identifiers, and supported GS1 data carriers.
- [GS1 General Specifications archive](https://ref.gs1.org/standards/genspecs/archive): official release history; the January 2026 Release 26.0.0 is the latest published consolidated edition consulted for this baseline.
- [GS1 General Specifications Change Notifications](https://ref.gs1.org/standards/genspecs/gscn/): official registry of ratified changes; as of the 19 November 2026 baseline, it lists ratified changes for the planned Version 27.0, which must be distinguished from the currently published Version 26.0.0.
- [GS1 Global Product Classification (GPC)](https://ref.gs1.org/standards/gpc/): an optional external product-classification vocabulary that may be mapped through a governed taxonomy profile; it does not mandate one CBM product hierarchy or replace business-specific classifications.
- [GS1 — Who is responsible for numbering trade items?](https://support.gs1.org/support/solutions/articles/43000734414-who-is-responsible-for-numbering-trade-items-): GS1 guidance on the usual GTIN allocator (the brand owner) and the role of an assigned GS1 Company Prefix and applicable exceptions.
- [GS1 Application Identifiers](https://ref.gs1.org/ai/): official searchable reference for identifiers that define the meaning and format of data attributes encoded in GS1 barcodes and EPC/RFID tags.
- [GS1 — Why should I use GS1 Application Identifiers in a barcode?](https://support.gs1.org/support/solutions/articles/43000734173-why-should-i-use-the-gs1-application-identifiers-in-a-barcode/): explains how GS1 AIs distinguish identification keys from attributes such as serial, batch/lot, and expiry data, and how these data may be represented through supported GS1 symbols.
- [GS1 — Are all barcodes the same?](https://support.gs1.org/support/solutions/articles/43000734155-are-all-barcodes-the-same-): explains differences between barcode symbologies, data capacities, and GS1-specific data conventions.
- [Microsoft Learn — Product dimensions](https://learn.microsoft.com/en-us/dynamics365/supply-chain/pim/product-dimensions): a product-management implementation example of configurable variant dimensions, dimension groups, valid combinations, and product variants. It is a comparative reference, not a CBM implementation mandate.

These sources support the distinction between an identified trade item, its variants and attributes, identifier values, and the data carrier that transports those values. They do not require CBM to adopt GS1 identifiers for every item, reproduce Microsoft Dynamics concepts, or use a vendor-specific Product Master. CBM's canonical model and applicability rules remain governed by this document; standards are applied where the relevant business, trade, regulatory, or interoperability use case justifies them.


Official references consulted for Pricing, MRP, and tax-basis boundaries in Section 6G:

- [Department of Consumer Affairs — Legal Metrology (Packaged Commodities) Rules and amendments](https://consumeraffairs.nic.in/acts-and-rules/legal-metrology): official legal-metrology rules and amendments; current applicability, package classes, declarations, and exemptions must be validated against the effective legal text before implementation.
- [Department of Consumer Affairs — Legal Metrology FAQs](https://consumeraffairs.nic.in/sites/default/files/file-uploads/latestnews/LM_FAQs.pdf): official guidance explaining MRP inclusive of all taxes, prohibition on retail sale above applicable MRP, unit-sale-price declarations, and related packaged-commodity questions. Guidance must be read with the effective Rules and amendments.
- [Government of India — Legal Metrology (Packaged Commodities) Amendment Rules, 2021](https://upload.indiacode.nic.in/showfile?actid=AC_PB_82_942_00004_00004_1550217305600&filename=the_legal_metrology_%28packaged_commodities%29_rules%2C2011_with_amendments.pdf&type=rule): official amendment text relevant to packaged-commodity declarations and rule changes.
- [Department of Consumer Affairs — Legal Metrology (Packaged Commodities) Amendment Rules, 2022, G.S.R. 226(E)](https://consumeraffairs.nic.in/sites/default/files/uploads/legal-metrology-acts-rules/GSR226.pdf): official amendment text for unit-sale-price declaration requirements and related rule changes.
- [CBIC — Central Goods and Services Tax Act, 2017, Section 15](https://cbic-gst.gov.in/hindi/CGST-bill-e.html): official statutory basis for transaction value and treatment of specified inclusions and discounts. Current amendments and applicable rules must be checked when used.
- [CBIC — Central Goods and Services Tax Rules, 2017, Rule 35](https://cbic-gst.gov.in/pdf/01012022-CGST-Rules-2017-amended_Part-A.pdf): official method for determining the tax component where a supply value is inclusive of the applicable integrated, central, State, or Union Territory tax.

These sources support the separation of declared MRP, transaction selling price, unit-sale-price declaration, GST-inclusive/exclusive price basis, and statutory tax valuation. They do not establish CBM's future HSN/SAC or GST-rate model. GST/Tax remains the authoritative owner of tax determination; legal rates, classifications, package applicability, and statutory amendments must be revalidated against official current sources before implementation or compliance use.



Official references consulted for HSN/SAC classification, GST rate schedules, and tax-rule versioning in Section 6H:

- [CBIC — GST Goods and Services Rates](https://cbic-gst.gov.in/hindi/gst-goods-services-rates.html): official schedules list tariff/classification scopes together with goods/service descriptions, conditions, CGST, SGST/UTGST, IGST, and cess components. The schedule demonstrates why a bare HSN/SAC-to-one-rate lookup is insufficient; the relevant description and statutory conditions must also be considered.
- [CBIC — Central Tax (Rate) Notifications](https://cbic-gst.gov.in/hindi/central-tax-rate.html) and [CBIC Tax Information Portal — Notifications](https://taxinformation.cbic.gov.in/): official notification repositories used to source rate changes and their effective dates. For example, Notification No. 01/2026-Central Tax (Rate), dated 30 April 2026, amended tariff-code entries and took effect from 1 May 2026; this illustrates the need for effective-dated, source-linked rate-rule versions.
- [GSTN — Advisory on HSN and GSTR-1 Filing, 24 August 2021](https://tutorial.gst.gov.in/downloads/news/advisoryonhsnandgstr1.pdf): the advisory described then-applicable reporting digit requirements by aggregate turnover and explicitly warned against truncating a valid eight-digit HSN to six digits when the truncated value is not an independently valid tariff code. Its reporting thresholds are historical guidance and must not be represented as a current universal legal rule; current filing/e-invoice/e-way bill requirements must be verified before implementation.
- [GST Portal — Search HSN/SAC](https://services.gst.gov.in/services/searchhsnsac): official GSTN lookup facility for validating available HSN/SAC code descriptions and references.

These sources support the architecture's separation of code classification, product/service assignment, rate applicability, transaction-specific tax determination, effective-date versioning, and source provenance. They do not establish that every HSN/SAC code has one permanent rate, do not replace the effective statutory notification or applicable GST law, and do not make the CBM six-digit minimum a universal statutory minimum. Current law, notifications, code directories, portal validation, and reporting obligations must be rechecked from official sources before implementation or compliance use.

Official technical references consulted for security and SaaS architecture boundaries:

- [NIST SP 800-63B-4 — Authentication and Authenticator Management](https://csrc.nist.gov/pubs/sp/800/63/B/4/final): authenticator lifecycle, OTP handling, replay resistance, and session considerations.
- [NIST SP 800-63B-4 — Authenticator technical requirements](https://pages.nist.gov/800-63-4/sp800-63b/authenticators/): OTP single-use, validity windows, rate limiting, and secure handling.
- [OWASP Forgot Password Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html): secure recovery identifiers, restricted recovery process, and password-reset security.
- [OWASP Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html): OTP lifetime, single-use, attempt limits, and storage.
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html): adaptive password hashing and secure password storage.
- [OWASP Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html): session renewal and invalidation after security-sensitive changes.
- [AWS SaaS Lens — Tenant Onboarding](https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/tenant-onboarding.html), [Tenant Isolation](https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/tenant-isolation.html), and [Identity and Access Management](https://docs.aws.amazon.com/wellarchitected/latest/saas-lens/identity-and-access-management.html): tenant lifecycle orchestration, isolation, and tenant-bound identity.
- [Stripe Billing — Subscription API and lifecycle](https://docs.stripe.com/api/subscriptions): a vendor example of subscription states, billing behaviour, and effective subscription changes. Stripe is a research reference, not a required CBM provider.

These references inform security and implementation considerations but do not override CBM's explicitly approved business rules. In particular, CBM's no-URL rule and its Organization-configurable initial credential methods are project-specific decisions. Technical implementations must follow the approved CBM rules while applying appropriate compensating security controls.
---



**End of authoritative finalized baseline.**
