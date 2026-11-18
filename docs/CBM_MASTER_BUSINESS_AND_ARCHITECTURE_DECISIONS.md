# Central Business Management (CBM) Portal
# Master Business & Architecture Decisions

| Document attribute | Value |
|---|---|
| Document status | Approved baseline of finalized decisions |
| Baseline date | 18 November 2026 |
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
8. [Business Authority Scope](#7-business-authority-scope)
9. [Delegated Authority](#8-delegated-authority)
10. [Users, Roles, and Permissions](#9-users-roles-permissions-and-action-authorization)
11. [Configuration governance](#10-configuration-governance-defaults-inheritance-and-overrides)
12. [Approval Workflow governance](#11-approval-workflow-governance)
13. [Distributor–Shop relationships and mapping](#12-distributor-shop-business-relationships-and-mapping)
14. [Business Entity Lifecycle and State Management](#13-business-entity-lifecycle-and-state-management)
15. [Subscription, Plans, Entitlements, and Billing](#14-subscription-plans-entitlements-and-billing)
16. [User Account Lifecycle and Initial Credential Establishment](#15-user-account-lifecycle-and-initial-credential-establishment)
17. [Sub-Portal migration and service continuity](#16-sub-portal-migration-and-service-continuity)
18. [Data archival and object-storage principles](#17-data-archival-and-object-storage-principles)
19. [Domain History and Audit Trail Management](#17a-domain-history-and-audit-trail-management)
20. [Stable Core, Configurable, Customizable, and Extensible principles](#18-stable-core-configurable-customizable-and-extensible-design-principles)
21. [Cross-concept invariants](#19-required-cross-concept-invariants)
22. [Illustrative scenarios](#20-illustrative-scenarios)
23. [Documentation governance](#21-documentation-governance)
24. [Source basis and decision provenance](#22-source-basis-and-decision-provenance)
---

## 1. Purpose, authority, and use of this document

### 1.1 Purpose

This document consolidates the project vision and all business-level and logical-architecture decisions explicitly finalized for the CBM Portal through 18 November 2026. It is the authoritative baseline from which subsequent project documentation is to be derived.

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

### 2.3 Initial business-management capabilities

The product vision includes:

- Accounting Management.
- Inventory Management.
- Human Resource Management.
- Reports and Analytics.
- Point of Sale (POS).
- Additional practical business-management capabilities introduced in the future under Main Portal governance.

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

An Organization may have multiple direct structural children of either or both permitted types: Distributor and Shop. Multiple Distributors may be grouped as a **Distribution Network**; multiple Shops may be grouped as a **Retail Network**. An Organization with both distribution and retail operations has a **Hybrid Network** configuration. These are operating configurations/groupings, not separate Business Identities, Owners, Service Providers, or customer businesses. The ownership of each Distributor and Shop is determined independently through Ownership Links.

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

       OWNER ID A                 OWNER ID B
           |                          |
      OWNERSHIP ID               OWNERSHIP ID
           |                          |
      ORGANIZATION A                 SHOP S1

A Shop may have an Owner different from the Owner(s) of its structural
Organization. Structural placement does not establish ownership.

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

A change to an Organization's Owner ID set cannot be applied while leaving its direct structural Distributor or Shop children with a different Owner ID set. Ownership Transfer workflows remain subject to the existing deferral and must not be inferred or implemented from this consistency rule alone.

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

A Distributor's scope does not include other mapped Shops merely because they participate in its Distribution Network. A Shop owned by a different Owner ID remains outside the Distributor's ownership-based scope, even when mapped to the Distributor. Mapping enables only the expressly authorized relationship interactions; it does not create an Ownership Link or grant unrestricted administrative or data access.

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
- Shops mapped to D1 but owned by unrelated Owners are not included in Organization A's scope merely because of the mapping, and cannot be added to these Users' assignments on that basis.

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

The History domains established in this baseline are:
1. **GST Registration History:** GST Registration and GST Registration Assignment events only.
2. **Distributor–Shop Mapping History:** events concerning the specific Distributor–Shop relationship only.
3. **Ownership History:** events concerning the relevant Owner ID, Ownership ID, and Ownership Link only.
4. **User Scope Mapping History:** events concerning the relevant User-to-entity/location scope assignment only.
5. **Business Entity Lifecycle History:** lifecycle state changes for one Organization, Distributor, or Shop, including the causal Lifecycle Operation and associated Snapshot reference. It contains only that entity's lifecycle events.

A User ID or User profile change must never be inserted into GST Registration History, Ownership History, or Mapping History. A separate User account/profile History may be introduced only if a distinct business need is established and approved. Other History domains must not be assumed merely because Audit Trail records exist; each requires an explicit business requirement and a definition of scope, contents, permissions, and retention.

### 17A.3 History contents, integrity, and retention

Each History record set records only fields relevant to its own domain. As applicable, it may include affected domain object/relationship, event type, previous and new domain values/states, effective date/time, recorded date/time, reason, responsible actor, approver, and supporting reference. A significant domain change must not silently overwrite the only record of its previous state. Corrections must preserve the original entry and identify the correction or superseding event.

History must be preserved throughout the existence of the relevant Business Identity/business context. It must not be routinely purged or deleted while that business remains in existence. It may be removed only as part of authorized permanent deletion of the relevant business.

History may be available to multiple appropriately authorized Users. Its visibility is controlled by separate History permissions and applicable business scope. Permission to view History does not grant permission to perform the underlying action or to view the Audit Trail.

### 17A.4 Comprehensive system-wide Audit Trail

Audit Trail must capture all relevant system events and activities across CBM in chronological order. Its coverage is not limited to the History domains above or to security events alone.

As applicable, Audit Trail includes successful and failed actions, creation and modification attempts, access and authentication events, User and Role changes, User Scope Mapping changes, Ownership Link changes, Business Relationship/Mapping changes, GST verification and assignment events, configuration changes, approvals and rejections, administrative actions, data exports, archival and deletion operations, and security events.

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