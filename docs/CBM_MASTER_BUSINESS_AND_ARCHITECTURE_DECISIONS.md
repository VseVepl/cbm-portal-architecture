# Central Business Management (CBM) Portal
# Master Business & Architecture Decisions

| Document attribute | Value |
|---|---|
| Document status | Approved baseline of finalized decisions |
| Baseline date | 17 November 2026 |
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
14. [Distribution closure and Shop continuity](#13-distribution-closure-and-independent-shop-continuity)
15. [Sub-Portal migration](#14-sub-portal-migration-and-service-continuity)
16. [Data archival](#15-data-archival-and-object-storage-principles)
17. [Stable Core, Configurable, and Extensible classification](#16-stable-core-configurable-and-extensible-design-classification)
18. [Cross-concept invariants](#17-required-cross-concept-invariants)
19. [Illustrative scenarios](#18-illustrative-scenarios)
20. [Documentation governance](#19-documentation-governance)
21. [Source basis](#20-source-basis-and-decision-provenance)

---

## 1. Purpose, authority, and use of this document

### 1.1 Purpose

This document consolidates the project vision and all business-level and logical-architecture decisions explicitly finalized for the CBM Portal through 17 November 2026. It is the authoritative baseline from which subsequent project documentation is to be derived.

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
- Strong business-ownership boundaries.
- Strict business and tenant isolation.
- Centralized governance.
- Data integrity.
- Security.
- User-friendliness.
- Long-term maintainability.

---

## 3. Canonical terminology and definitions

This section defines terms that must be used consistently. Similar-sounding terms are not interchangeable.

### 3.1 CBM Ecosystem

The **CBM Ecosystem** is the complete platform environment governed by Main Portal. It includes Main Portal, Sub-Portals, customer Organizations/Tenants, Distributors, Stores, independent Shops, their users, their permitted business relationships, and the services and data managed within CBM.

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

An Organization may operate a Distribution Network, a Retail structure, or both, according to its approved business configuration.

### 3.6 Tenant

A **Tenant** is the logical customer-service and isolation context within CBM for an Organization's use of the platform. Tenant is a service/data-boundary concept; it is not a Service Provider, Owner, User, or Business Relationship.

For the initial business model, an Organization has one primary Tenant context. The Organization's business identity and the Tenant's logical isolation purpose remain conceptually distinct even when associated one-to-one.

### 3.7 Business Identity

A **Business Identity** is the persistent, unique CBM identity of an independently existing business. It distinguishes that business from its owner, its users, its Service Provider, its Tenant context, and its business relationships.

A Business Identity must not be replaced merely because the business changes its Service Provider or enters or leaves a business relationship. Closed identities must not be recycled for a different business.

### 3.8 Owner / Ownership Holder

An **Owner** or **Ownership Holder** is the person or legal entity that owns a Business Identity. An owner is not the same thing as a User account. One owner may own multiple independent businesses. A business may have one or more ownership holders; ownership representation is separate from the assignment of operational Users and Roles.

### 3.9 Distributor

A **Distributor** is an independently identifiable distribution business participating in an Organization's Distribution Network or otherwise operating within the CBM Ecosystem. It has its own Business Identity and ownership. Its network association does not, by itself, transfer ownership to the Organization.

A Distributor's own business authority scope is limited to the Distributor itself. A Distributor does not acquire authority over an independently owned Shop merely because that Shop is mapped to it.

### 3.10 Independent Shop

An **Independent Shop** is a separately owned retail business with its own Business Identity, owner, business data, and operational responsibility. It may establish one or more authorized business relationships with Distributors.

A Distributor–Shop mapping does not make the Shop a child business owned by the Distributor and does not grant unrestricted access to the Shop.

### 3.11 Organization-owned Store / Shop

An **Organization-owned Store** (also described as a directly owned Shop or Retail Location) is an operational unit within an Organization's own business structure. It may have its own Store or Location identifier for operational purposes, but it is not a separate independently owned Business Identity merely because it has a separate location or operational record.

Its ownership remains with the Organization's owner, and its operations remain within the Organization's business structure and authority scope.

### 3.12 Business Structure

**Business Structure** means the internal arrangement of a business and its directly subordinate operational or business components. It is distinct from a relationship between two independently owned businesses.

### 3.13 Business Relationship

A **Business Relationship** is an explicitly established association between distinct business identities, such as a Distributor–Independent Shop mapping. A relationship enables only the interactions and permissions expressly allowed for that relationship. It does not transfer ownership or automatically grant broad administrative authority or data access.

### 3.14 Business Authority Scope

**Business Authority Scope** means the set of business entities or business areas over which a business-level authority may be exercised or delegated. It answers: **“For which businesses or business areas may authority be assigned?”**

Business Authority Scope is a business-level concept. It is not itself a list of actions that an individual User may perform.

### 3.15 User

A **User** is an authenticated individual identity that accesses CBM. A User may be associated with one or more businesses through explicit access relationships. A User is not automatically an Owner and does not receive access to every business owned by the same person or served by the same provider.

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

**Business Data Isolation** means that business records and information are separated by their owning business/Tenant context and are accessible only through valid authority, User access, Permission, and relationship rules. A common owner, Service Provider, network membership, or mapping does not by itself merge separate data boundaries.

---

## 3A. How the CBM relationship model must be read

CBM must not be interpreted as one universal Parent–Child tree. It contains several distinct, overlapping relationship views. Each view answers a different question and must be modelled and evaluated independently.

| Relationship view | Question it answers | Example |
|---|---|---|
| **Platform/service-provider structure** | Who provides the CBM service to this customer business, and under whose platform governance does that provider operate? | Main Portal → Sub-Portal → served Organization/Tenant; or Main Portal → directly served Organization/Tenant. |
| **Business structure** | Which operating units and business components belong within an Organization's own business structure? | Organization → its directly owned Retail Stores; Organization → its Distribution Network. |
| **Ownership relationship** | Who owns each independent Business Identity? | One owner may own several separate businesses; each retains its own identity. |
| **Business relationship** | Which independent businesses are associated for a specific commercial purpose? | Distributor ↔ Independent Shop mapping. |
| **Business Authority Scope** | Over which business entities may a business-level authority be assigned or exercised? | Organization scope includes its Network Distributor but does not automatically pass through it to the Distributor's independent Shops. |
| **User authorization** | Which actions may a particular User perform in a particular business context? | A Store Manager may update records only for Store A. |

These views may refer to the same businesses, but they are not interchangeable. A relationship in one view does not create a relationship in another unless the applicable rule explicitly says so.

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

ORGANIZATION BUSINESS-STRUCTURE / AUTHORITY-SCOPE VIEW

                 ORGANIZATION
                  /        \
      Distribution Network  Organization-owned Stores
               |
          DISTRIBUTOR
               |
       - - mapping relationship - - INDEPENDENT SHOP

Important: the Organization's business authority scope includes the
Distributor, but does not automatically pass through the Distributor
along the mapping relationship to the Independent Shop.
The Sub-Portal's authorized scope may include the associated Shop
within its own service-provider domain. Main Portal's scope is global.

OWNERSHIP VIEW (INDEPENDENT OF THE DIAGRAMS ABOVE)

      Owner A                 Owner B
         |                       |
   Organization A           Independent Shop X
         |
   Organization-owned Store 1

The diagrams are conceptual views, not a database schema or a universal
ownership hierarchy. They illustrate why service provision, ownership,
business structure, mapping, authority scope, and user permission must
remain separate.
```

### 3A.2 Non-inference rule

No one may infer any of the following solely from the existence of another relationship:

- Service Provider status does not imply ownership.
- Ownership does not automatically imply a User login or unrestricted operational permission.
- Business-structure membership does not imply that every User can perform every action.
- Distributor–Shop mapping does not imply ownership, Organization authority scope, or Distributor authority over the Shop.
- A Sub-Portal's authority scope over associated businesses does not imply unrestricted record access for every Sub-Portal User.
- A business's authority scope does not itself define the action types available to a User.

---

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

Business identity, ownership relationships, mapping relationships, authority delegation, permission changes, configuration changes, approvals, migration, archival, and other security-significant operations must preserve integrity and be auditable.

---

## 5. Business entity and ownership model

### 5.1 Independent Business Identities

The following are independent identity-bearing entities in the CBM business model:

- Main Portal as the central platform/SaaS business.
- Each Sub-Portal as an independently owned service-provider business.
- Each Organization as an independent customer business.
- Each Distributor as an independently identifiable distribution business.
- Each Independent Shop as an independently owned retail business.

The fact that these entities have different platform roles does not make their roles interchangeable.

### 5.2 Organization-owned operational units

A Store/Shop owned directly by an Organization is an internal operational unit of that Organization, not a separate independent business identity. It may have a unique operational Store/Location identity to distinguish its transactions, stock, staff, POS activity, and reports.

### 5.3 Ownership rules

1. Each independent business has an explicitly represented owner or ownership holder set.
2. One owner may own multiple independent businesses.
3. A business may have one or more ownership holders.
4. Separate businesses owned by the same person remain separate Business Identities and separate data contexts.
5. User accounts, roles, and operational access are distinct from ownership.
6. A Distributor does not own an Independent Shop merely because the Shop is in its distribution network.
7. An Organization's association with a Distributor does not itself change the Distributor's ownership.
8. Organization-owned Stores remain under the ownership of the Organization's owner.

### 5.4 No in-place business ownership transfer

CBM does not provide an in-place Business Identity ownership-transfer process for a business whose ownership changes. The existing business is closed under its applicable lifecycle rules; the new owner establishes a new business with a new Business Identity. The closed business's identity and history remain distinct and are handled under applicable retention and data rules.

This rule is separate from Sub-Portal or customer Service Provider migration, which does not change business ownership.

### 5.5 Organization configurations

An Organization has one of the following business configurations:

| Organization configuration | Distribution Network | Organization-owned Retail Stores |
|---|---:|---:|
| Hybrid Organization | Yes | Yes |
| Distribution-only Organization | Yes | No |
| Retail-only Organization | No | Yes |

These are three distinct configurations of an Organization's business operation.

- A **Hybrid Organization** operates both a Distribution Network and its own Retail structure. It may also have independent retailers associated with its distribution operations.
- A **Distribution-only Organization** operates a distribution business and may have independently owned retailers associated with its Distribution Network. It does not operate its own Organization-owned Retail Stores under this configuration.
- A **Retail-only Organization** operates its own Retail Stores. Those Stores belong to the Organization and do not have an independent Distributor–Shop relationship with their own Organization.

---

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

### 7.1 Meaning

Business Authority Scope defines the business entities or business areas within which a business-level authority may be assigned or exercised. It establishes the **“where”** boundary for authority.

It does not specify which actions a particular User may perform. That is the function of Permissions.

### 7.2 Main Portal scope

Main Portal's authority scope extends across the entire CBM Ecosystem without subordinate-imposed restriction. It covers all Sub-Portals, Organizations, Distributors, Stores, Independent Shops, and applicable business/service contexts within CBM.

No Sub-Portal, Organization, Distributor, Shop, User, or Role may narrow or constrain Main Portal's supreme authority scope.

### 7.3 Sub-Portal scope

A Sub-Portal's authority scope extends to the businesses and operational entities associated with and served within its authorized Sub-Portal domain. This includes the relevant Organizations, Distribution operations, Distributors, Organization-owned Stores, and Shops associated with that Sub-Portal's domain.

This broader service-provider scope does not transfer ownership to the Sub-Portal and does not automatically grant every Sub-Portal User unrestricted access to every record. User-level access remains subject to Permissions, role/user assignments, applicable purpose, and data-isolation rules.

### 7.4 Organization scope

An Organization's authority scope extends to its own business and the subordinate business/operational entities within its Organization-level structure, including:

- Distributors participating under its Distribution Network.
- Stores/Shops owned by the Organization and operated as part of its Retail structure.
- Other directly subordinate entities that remain within the Organization's own business and ownership structure.

The Organization's authority scope does **not** automatically pass through a Distributor to the Distributor's independently owned Shops. The fact that a Shop is mapped to or associated with a Distributor does not place that Shop within the Organization's authority scope.

Where a Shop/Store has the same ownership as the Organization and forms part of the Organization's own business structure, it may fall within the Organization's scope as an Organization-owned operational unit.

### 7.5 Distributor scope

A Distributor's own Business Authority Scope is limited to the Distributor itself. It does not extend to an Independent Shop mapped to it. A mapping or distribution relationship alone cannot give the Distributor authority over the Shop's business.

### 7.6 Independent Shop scope

An Independent Shop's business scope is its own business. It retains control over its own business context and data, subject to the higher-level Main Portal and applicable Sub-Portal governance that applies to the Shop's service-provider context.

### 7.7 Scope inheritance is not ownership inheritance

A higher business's authority scope may include subordinate businesses as defined above without changing those businesses' owners. Authority-scope relationships must never be used to infer a transfer of legal or beneficial ownership.

### 7.8 Scope check before permission check

An action may be authorized for a User only when:

1. The target business is within the relevant business authority scope; and
2. The User has the required action Permission in the applicable business context; and
3. Any additional required approval and other mandatory controls have been satisfied.

---

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

### 9.1 Separation of business authority and user authorization

Business Authority Scope belongs to the business/governance model. Permissions belong to Users and Roles within a defined business context.

The two questions must be answered separately:

- **Where may authority be assigned?** — Business Authority Scope.
- **What may this User do there?** — User/Role Permission.

### 9.2 User-to-business association

A User must have an explicit valid association with the target business context before business permissions can be applied. A User's access to one business does not automatically grant access to another business, even where the same person owns both businesses.

### 9.3 Role-based and User-specific permissions

Permissions must support assignment through Roles and direct User-specific rules. This allows different Users in the same business, or Users with different Roles, to perform different actions within the same business authority scope.

The model must support granular permission combinations at both Role and individual User level.

### 9.4 Action-oriented permission types

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

### 9.5 Example

An Organization may have authority scope over three Organization-owned Stores. Within that scope:

- An Organization Administrator may have View, Create, Update, and Delete permissions across all three Stores.
- Store Manager A may have View, Create, and Update permissions only for Store A.
- Store Manager B may have View and Create permissions only for Store B.
- An Auditor may have View-only permission across the three Stores.

The fact that the Organization has scope over all three Stores does not mean every User can perform every action in all three Stores.

---

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

Approval requests, approver identity, decision, decision time, approval stage, and final outcome must be recorded for accountability and auditability.

---

## 12. Distributor–Shop business relationships and mapping

### 12.1 Relationship model

Distributor–Shop mapping is a business relationship between separate identities, not an ownership hierarchy. The relationship is independent from both parties' ownership and does not merge their data contexts.

The CBM Ecosystem must support relationships between Distributors and Shops across the ecosystem, regardless of whether the Distributor is directly under Main Portal, under a Sub-Portal, or associated with an Organization served through another Sub-Portal.

### 12.2 Cardinality

- One Distributor may be associated with multiple Independent Shops.
- One Independent Shop may be associated with multiple Distributors.
- Each Distributor–Shop mapping is an independently identifiable relationship with its own lifecycle and auditable history.

### 12.3 Mapping initiation and approval

1. The Shop owner initiates a Distributor Mapping Request.
2. The request uses the Distributor's Unique ID and Secret Code.
3. The system creates a mapping request for the identified Distributor.
4. The Distributor or an appropriately authorized Organization may approve or reject the request, in accordance with the applicable business authority scope and authorization rules.
5. A mapping becomes active only through the required authorization process.

### 12.4 Unique ID and Secret Code

- The **Distributor Unique ID** identifies the Distributor for mapping purposes. It is an identifier, not a grant of access.
- The **Distributor Secret Code** is a protected credential used in initiating/validating the mapping request. It must be protected as a secret and must not be treated as a public identifier.

Knowing a Unique ID or Secret Code does not grant general access to the Distributor or Shop.

### 12.5 Relationship effects and limits

A mapping:

- Establishes only the authorized business relationship.
- Does not change either party's ownership.
- Does not convert an Independent Shop into a Distributor-owned child business.
- Does not automatically expand the Distributor's business authority scope to include the Shop.
- Does not automatically expand the Organization's business authority scope through the Distributor to the Shop.
- Does not grant unrestricted access to Accounting, Inventory, HR, Users, administration, or other business data.
- May enable only the specific transactions and interactions expressly authorized for the relationship.

The Distributor's authorization is necessary and sufficient for enabling the automated transactions covered by this mapping relationship. An additional mandatory Shop approval must not be introduced for those transactions without a separate business decision. This does not remove the Shop owner's role in initiating the mapping request.

### 12.6 Ending a mapping

Ending one Distributor–Shop mapping ends that relationship only. It does not close the Shop, change its ownership, invalidate its Business Identity, or terminate its other Distributor relationships.

---

## 13. Distribution closure and Independent Shop continuity

### 13.1 Continuity principle

Closing a Distribution Network must not automatically destroy or invalidate the Business Identity or historical data of independently owned Shops associated with that network.

### 13.2 Shop options after Distribution Network closure

When a Distribution business/network closes:

1. The Distribution business is closed under its applicable lifecycle process.
2. Each affected Independent Shop remains identifiable and independently owned.
3. Each affected Shop may independently choose whether to subscribe to an Organization Plan.
4. If the Shop accepts the Organization Plan, it continues as an independent Organization/Shop business with its own Business Identity and preserved history.
5. The previous Distributor relationship does not become an ownership relationship.

### 13.3 Shop that does not continue on an Organization Plan

If the Shop owner does not accept an Organization Plan:

- The Shop account becomes inactive.
- Normal operations through that account cease.
- The Shop's data remains preserved pending the archival process.
- The owner receives a direct data-download window of six months from the effective Shop account deactivation date.
- After the six-month download window, the operational copy is removed from the Main Portal operational environment only after successful archival.
- The archived data is retained in external object storage according to the applicable Main Portal retention policy.

The six-month download period is measured from the effective Shop account deactivation date.

---

## 14. Sub-Portal migration and service continuity

### 14.1 Nature of migration

Sub-Portal migration is a complete service-provider migration. It is not a transfer of Tenant ownership, Business Identity, or data rights.

A Sub-Portal and its associated customer/business structure may be migrated to another Sub-Portal or directly under Main Portal, under Main Portal governance.

### 14.2 Migration boundary

Migration must include the complete Sub-Portal scope and all information required to preserve its existing internal structure and service continuity, including as applicable:

- All subordinate business entities.
- The complete business structure and hierarchy.
- Associated business data and records.
- Users and relevant access associations.
- Business records and transactions.
- Documents and attachments.
- Configurations and operational settings.
- Relevant historical and audit records.
- Other associated information necessary for complete continuity and integrity.

### 14.3 Invariants during migration

Migration must preserve:

- Each Business Identity.
- Business ownership and ownership boundaries.
- Tenant ownership and data rights.
- Existing internal business structure.
- Valid business relationships and their history.
- Data integrity and historical continuity.

Only the applicable Service Provider relationship changes. Migration must not silently reassign ownership, merge businesses, or alter Tenant data rights.

### 14.4 Sub-Portal closure or ownership change

If a Sub-Portal closes or its ownership changes, its complete associated business structure and data must be capable of migration to another Sub-Portal or directly to Main Portal while preserving the invariants above.

Migration is a controlled, complete, integrity-verified capability. It is not merely reassignment of database records.

---

## 15. Data archival and object-storage principles

### 15.1 Provider-neutral abstraction

Long-term archival must be designed around an independent Object Storage Abstraction so that the archival system is not locked to a single object-storage provider. The architecture must support changing providers without redesigning the archival domain.

### 15.2 Archival requirements

The archival capability must support:

- Encryption of archived data.
- Complete export of business data and related files.
- Database records and associated documents/attachments.
- An archive manifest.
- Integrity verification and checksums.
- Secure access controls.
- Retention management under Main Portal policy.
- Restore capability.
- Audit history.
- Verification of successful archival before deletion of the operational copy.

### 15.3 Separation from operational data

Archival is preservation, not continuation of normal operations. Removing an operational copy after the required download window and successful archival does not mean that the archived business history is erased or that ownership is transferred.

---

## 16. Stable Core, Configurable, and Extensible design classification

All architectural decisions and capabilities must be classified using these three principles.

### 16.1 Stable Core

Stable Core contains fundamental protections and invariants that must not be made ordinary user-configurable options. It includes:

- Main Portal's permanent supreme authority.
- Authority flowing exclusively downward.
- No peer authority between Sub-Portals.
- Business Identity integrity.
- Ownership boundaries.
- Business and Tenant isolation.
- Separation of Business Authority Scope from User/Role Permissions.
- Authorization integrity.
- Configuration guardrails and protection against unauthorized overrides.
- Relationship integrity.
- Data integrity.
- Security boundaries.
- Approval integrity.
- Audit integrity.
- Preservation of identity, ownership, and data rights during Service Provider migration.

### 16.2 Configurable

Configuration is allowed where a business requirement legitimately varies, provided the change remains within Main Portal governance, explicit permissions, and Stable Core boundaries.

The approved configurable approach includes:

- Default values and permitted business-specific overrides.
- Role-specific and User-specific permissions.
- Business-specific configuration.
- Delegated authority dimensions and limits.
- Approval requirements and patterns.
- Operational/business settings within system-defined bounds.
- Applicable lifecycle and archival policy settings within the governance model.

Configurability must not allow a lower level to weaken or bypass an immutable higher-level rule.

### 16.3 Extensible

The architecture must allow new capabilities to be added without fundamental redesign, using clear business boundaries and contracts. Extensibility includes the ability to introduce:

- New business-management modules.
- New integrations and APIs.
- New reports and analytics capabilities.
- New storage providers behind the Object Storage Abstraction.
- New Business Relationship types.
- New workflow and automation capabilities.
- New operational capabilities that remain consistent with the CBM governance model.

Extensibility must not become an excuse for unnecessary abstraction or overengineering.

---

## 17. Required cross-concept invariants

The following statements summarize rules that must remain true across all future domain and technical designs.

1. Main Portal's authority scope covers the entire CBM Ecosystem and cannot be restricted by a subordinate.
2. Authority flows downward only; no subordinate authority flows upward.
3. A Sub-Portal is an independently owned Service Provider, not a customer Tenant/Organization.
4. A Service Provider does not become the owner of the customer business by providing software services.
5. A customer's Business Identity and ownership remain unchanged when its Service Provider changes.
6. Organization, Distributor, Independent Shop, and Organization-owned Store are not interchangeable entity types.
7. An Organization-owned Store is an operational unit within the Organization; an Independent Shop is a separate business identity.
8. Organization authority scope includes its own subordinate Distributors and its own Stores/Shops, but does not pass through a Distributor to that Distributor's independently owned Shops.
9. Distributor authority scope is limited to the Distributor itself and does not automatically include mapped Independent Shops.
10. A Sub-Portal's authority scope covers the relevant Organizations, Distribution operations, Distributors, Stores, and Shops associated with its authorized Sub-Portal domain.
11. Business Authority Scope says where authority may be assigned; User/Role Permissions say what a User may do.
12. A valid action requires the target business to be in scope and the User to have the relevant Permission.
13. Delegated Authority is constrained by Who, What, Where, Limits, and Duration.
14. Configuration defaults and inheritance flow downward. Overrides require explicit permission and must stay within system-defined bounds.
15. Configuration overrides may be granted to one business and withheld from another.
16. Approval requirements can vary by business, action, Role, and individual User.
17. Approval workflows support single, multiple, sequential, and parallel approval patterns.
18. Approval does not create an underlying action Permission.
19. Distributor–Shop mapping is a relationship, not ownership and not unrestricted access.
20. One Shop may map to multiple Distributors; each mapping is a separate relationship.
21. Ending a mapping does not close the Shop or terminate its other mappings.
22. Distribution closure does not automatically erase the identity or history of independent associated Shops.
23. Sub-Portal migration preserves identity, ownership, data rights, and internal structure; only Service Provider changes.
24. Operational data may be removed after the applicable download period only after archival is verified.
25. No User receives access to a business solely because the User, Owner, Service Provider, or another associated business has a relationship with it.

---

## 18. Illustrative scenarios

These scenarios explain how the finalized rules work together. They do not create additional authority or ownership rights beyond the rules above.

### 18.1 One owner, multiple businesses

Person A owns Organization A, Distributor A, and Independent Shop A. These remain three independent Business Identities, with separate business data and service contexts. Shared ownership does not merge them.

### 18.2 Organization with directly owned Stores

Organization A owns Store 1, Store 2, and Store 3 as operating units. The Organization's business authority scope includes these Stores. The Stores can have separate operational identifiers and different User/Role access, but they are not separate independent business identities solely because they are different locations.

### 18.3 Organization and Distributor, with independent Shops under the Distributor

Organization A has Distributor A in its Distribution Network. Organization A's business authority scope may include Distributor A. Shop X is independently owned and mapped to Distributor A. The Organization's scope does not extend through Distributor A to Shop X, and Distributor A's own scope does not extend to Shop X. The mapping permits only the explicitly authorized relationship interactions.

### 18.4 Sub-Portal serving an Organization and related business network

Sub-Portal A serves Organization A and the relevant associated distribution, store, and shop entities within its authorized domain. Its service-provider authority scope covers those associated businesses under Main Portal governance. This does not make Sub-Portal A their owner or grant every Sub-Portal User unrestricted access to every record.

### 18.5 Different approval rules for the same action

Within one Organization, the same action may require approval for Role A, may not require approval for Role B, and may have a separate user-specific approval rule for User C. The Organization configures these rules within the governing limits. The initiating User must still possess the action Permission.

### 18.6 Configuration override for one business only

Main Portal establishes a default setting. Organization A has explicit permission to override it and sets a permitted value within the system-defined range. Organization B has no override permission and continues to inherit the applicable higher-level value. Organization A's change does not silently change Organization B.

### 18.7 Service Provider migration

Organization A moves from Sub-Portal A to Sub-Portal B under Main Portal governance. Organization A's Business Identity, ownership, internal structure, data rights, and valid historical relationships remain intact. The Service Provider relationship changes.

---

## 19. Documentation governance

### 19.1 Documentation completeness

Project documentation must be sufficiently precise that a reader can understand each included concept without relying on undocumented assumptions. As applicable, definitions, purpose, scope, ownership, authority, constraints, relationships, behaviour, exceptions, and examples must be stated explicitly.

### 19.2 No unresolved matters in the master decision record

The master decision record contains finalized decisions only. An unresolved matter must be discussed and explicitly finalized before it is added. It must not be represented as a decision, and it must not be silently filled by an AI recommendation or an assumed technical convention.

### 19.3 Separate history from decisions

Chronological discussions, reasoning history, rejected approaches, and superseded wording belong in a separate history record if one is maintained. They must not be mixed into the authoritative decision statements.

### 19.4 Downstream documentation consistency

Every later project document must:

- Use the canonical definitions in this master document.
- Preserve the ownership, authority-scope, permission, configuration, approval, and isolation distinctions.
- Identify the master decisions on which it depends.
- Avoid introducing contradictory or broader rights by implication.
- Be updated when a governing master decision changes.

### 19.5 Change control

A proposed change is not final merely because it has been suggested or documented in a discussion. The project owner must explicitly approve it. After approval, the master source of truth is updated first, and dependent documents are reconciled.

---

## 20. Source basis and decision provenance

This baseline consolidates:

1. The user-provided project context document **CBM_Project_Context_and_Decisions_2026-11-17** (provided in the conversation as the CBM project context text), including its confirmed business, service-provider, ownership, mapping, continuity, migration, archival, and Stable Core principles.
2. The subsequent project-owner confirmations and clarifications in the conversation through **17 November 2026**, especially the accepted Business Identity and Ownership model, the Service Provider explanation, the corrected Business Authority Scope model, the separation between scope and User/Role Permissions, hierarchical configuration inheritance and business-specific overrides, and fine-grained approval workflow configurability.

Where a later explicit project-owner clarification refined an earlier general description, the later clarification governs. This includes the precise distinction between:

- Main Portal's ecosystem-wide authority scope;
- Sub-Portal authority scope over associated businesses in its authorized domain;
- Organization authority scope over its own subordinate Distributors and Organization-owned Stores/Shops, but not Independent Shops beneath a Distributor merely because of mapping; and
- Distributor authority scope limited to itself.

---

**End of authoritative finalized baseline.**