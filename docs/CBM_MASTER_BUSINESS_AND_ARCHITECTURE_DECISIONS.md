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
14. [Distribution closure and Shop continuity](#13-distribution-closure-and-independent-shop-continuity)
15. [Sub-Portal migration](#14-sub-portal-migration-and-service-continuity)
16. [Data archival](#15-data-archival-and-object-storage-principles)
17. [Stable Core, Configurable, Customizable, and Extensible design principles](#16-stable-core-configurable-customizable-and-extensible-design-principles)
18. [Cross-concept invariants](#17-required-cross-concept-invariants)
19. [Illustrative scenarios](#18-illustrative-scenarios)
20. [Documentation governance](#19-documentation-governance)
21. [Source basis](#20-source-basis-and-decision-provenance)

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

#### 3.8.1 Owner ID

**Owner ID** is the unique CBM identifier for a person or legal entity acting as an Owner. It identifies the Owner independently of any Business Identity, Ownership Link, or User account. One Owner ID may be linked to multiple independent Business Identities. Owner ID is not a login credential or User ID.

#### 3.8.2 Ownership ID

**Ownership ID** is the unique identifier of one specific Ownership Link between an Owner ID and a Business Identity. Each distinct Owner-to-Business ownership relationship has its own Ownership ID. A Business Identity may have multiple Ownership IDs, and one Owner ID may participate in multiple Ownership Links across different businesses.

Ownership ID identifies the relationship only. It does not represent an equity percentage, financial share, investment amount, or economic interest. CBM's ownership model in this context records who owns or holds ownership rights in a business for business-authority purposes; it does not calculate financial ownership.

#### 3.8.3 Ownership Link

An **Ownership Link** is the explicit relationship connecting an Owner ID to a Business Identity, identified by an Ownership ID. Ownership Links are independent of Business Structure and Business Relationships. A Shop's mapping to a Distributor does not create or change an Ownership Link. An Ownership Link may establish an eligible basis for ownership-related authority scope under Section 7, but it does not by itself grant a User operational access or action Permissions.

### 3.9 Distributor

A **Distributor** is an independently identifiable distribution business participating in an Organization's Distribution Network or otherwise operating within the CBM Ecosystem. It has its own Business Identity and one or more Ownership Links. Its network association does not, by itself, transfer ownership to the Organization.

A Distributor's Business Available Authority Scope includes the Distributor itself and may include an Independent Shop linked to the same Owner ID only under the explicit owner-linked eligibility rules in Section 7. A Distributor does not acquire authority over another independently owned Shop merely because that Shop is mapped to it.

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

**Business Authority Scope** is the business-level authority boundary that identifies the business entities or business areas for which authority may be exercised or assigned. It answers: **“Where may this business-level authority be applied?”** It does not itself grant access to an individual User or define which actions that User may perform.

#### 3.14.1 Business Available Authority Scope

**Business Available Authority Scope** is the set of entities or business areas that a business-level authority is eligible to govern or make available for assignment, after applying Main Portal governance, valid Business Structure, applicable Ownership Links, permitted Business Relationships, and all relevant Stable Core constraints. It is the maximum eligible scope from which User-level scope may be assigned. Availability is not an automatic User grant.

#### 3.14.2 User Assigned Authority Scope

**User Assigned Authority Scope** is the explicit subset of a Business Available Authority Scope assigned to a specific User within a valid User–Business association. It may cover one entity, selected multiple entities, or the full available scope. Full-scope access must be explicitly assigned; it must never be inferred from a User's level, title, Role, ownership, business association, parent–child position, or a Distributor–Shop mapping. Explicit exclusions may be applied where the governing assignment model permits them.

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

**Business Data Isolation** means that business records and information are separated by their owning business/Tenant context and are accessible only through valid authority, User access, Permission, and relationship rules. A common owner, Service Provider, network membership, or mapping does not by itself merge separate data boundaries. Explicitly authorized access is limited to its approved purpose and scope; it does not merge the underlying businesses' data boundaries.

### 3.23 Applicability

**Applicability** defines the business contexts, entity types, scopes, conditions, actions, Users, Roles, or operating circumstances to which a feature, configuration, policy, permission, or workflow applies. Applicability must be explicit enough to prevent a rule intended for one business context from unintentionally affecting another.

### 3.24 Customization

**Customization** is an authorized adaptation of permitted product behaviour, presentation, workflow, or operational choices to meet a business's legitimate requirements. Customization must remain within Main Portal governance, the applicable authority and permission model, and all Stable Core constraints. It does not mean changing immutable platform rules or another business's data or settings.

### 3.25 Business Structure Hierarchy and Structural Integrity

**Business Structure Hierarchy and Structural Integrity** is the strict, non-configurable rule governing the permitted structural levels, entity placement, and parent–child relationships among Main Portal, Sub-Portal, Organization, Distributor, and Shop. Only explicitly valid structures defined by the CBM business model are permitted. A structurally invalid arrangement cannot be created, configured, customized, delegated, or overridden into validity.

The following arrangements are expressly invalid: a Distributor placed under a Shop; an Organization placed under a Distributor; and a Distributor existing independently without belonging to an Organization's business structure. The hierarchy must not be inverted, bypassed, or treated as an arbitrary user-configurable tree.

This is a business-structure rule only. It has no meaning or application in cryptography, encryption, encryption-key management, or key derivation. The service-provider relationship, Organization business structure, ownership relationship, and Distributor–Shop business mapping remain distinct relationship views; one must not be inferred from another.

---

## 3A. How the CBM relationship model must be read

CBM must not be interpreted as one universal Parent–Child tree. It contains several distinct, overlapping relationship views. Each view answers a different question and must be modelled and evaluated independently.

| Relationship view | Question it answers | Example |
|---|---|---|
| **Platform/service-provider structure** | Who provides the CBM service to this customer business, and under whose platform governance does that provider operate? | Main Portal → Sub-Portal → served Organization/Tenant; or Main Portal → directly served Organization/Tenant. |
| **Business structure** | Which operating units and business components belong within an Organization's own business structure? | Organization → its directly owned Retail Stores; Organization → its Distribution Network. |
| **Ownership relationship** | Who owns each independent Business Identity? | Owner ID → Ownership ID → Business Identity; one Owner may be linked to several separate businesses. |
| **Business relationship** | Which independent businesses are associated for a specific commercial purpose? | Distributor ↔ Independent Shop mapping. |
| **Business Available Authority Scope** | Which entities are eligible for a business-level authority to govern or make available for User assignment? | A Distributor may include an associated Shop linked to the same Owner ID, only under Section 7; mapping alone is insufficient. |
| **User Assigned Authority Scope** | Which of the business-available entities has been explicitly assigned to this User? | An Organization-level User may be assigned only Store A and Store C, not every Store. |
| **Permission / Role** | Which actions may this User perform within the assigned entity scope? | A Store Manager may update records only for Store A. |

These views may refer to the same businesses, but they are not interchangeable. A relationship in one view does not create a relationship in another unless the applicable rule explicitly says so. Ownership Links may inform eligible Business Available Authority Scope under Section 7; they do not change Business Structure, create a Distributor–Shop mapping, or automatically grant a User access. The Business Structure Hierarchy has its own strict valid-parent and valid-child constraints; it is not a freely rearrangeable universal tree.

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


BUSINESS STRUCTURE / BUSINESS RELATIONSHIP VIEW

                 ORGANIZATION A
                  /          \
      Distribution Network   Organization-owned Stores
               |
          DISTRIBUTOR D1
               |
       - - mapping relationship - - INDEPENDENT SHOP X
       - - mapping relationship - - INDEPENDENT SHOP Y

Mapping connects the Distributor and each Shop for expressly
authorized business interactions. Mapping does not establish ownership.


OWNERSHIP LINK VIEW (INDEPENDENT OF BUSINESS STRUCTURE AND MAPPING)

       OWNER ID B                         OWNER ID C
           |                                  |
      OWNERSHIP ID                       OWNERSHIP ID
           |                                  |
      DISTRIBUTOR D1                   INDEPENDENT SHOP Y
           |
      OWNERSHIP ID
           |
      INDEPENDENT SHOP X

If Distributor D1 and Shop X are linked to the same Owner ID, Shop X
may be eligible for D1's owner-linked Business Available Authority
Scope under Section 7. Shop Y, linked to a different Owner ID, is not
included in D1's scope merely because it is mapped to D1.


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
- A Distributor–Shop mapping alone does not imply ownership or ownership-based authority over the Shop. Any eligible owner-linked scope must arise separately under Section 7.
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

Business identity, ownership relationships, mapping relationships, authority delegation, permission changes, configuration changes, approvals, migration, archival, and other security-significant operations must preserve integrity and be auditable.

### 4.9 Strict Business Data Isolation

Business and Tenant data boundaries are immutable security constraints. No configuration, customization, extension, delegated authority, or override may merge independent business data contexts or weaken their isolation. Any permitted cross-business interaction must be explicit, purpose-limited, authorized, and auditable without changing the underlying ownership or isolation boundary.

### 4.10 Strict Business Structure Hierarchy and Structural Integrity

The permitted Business Structure Hierarchy is an immutable Stable Core business-structure constraint. The valid entity levels and parent–child placements must be preserved in every domain, logical, data, and technical design. The invalid arrangements defined in Section 3.25 must never be supported. No subordinate business, Service Provider, User, Role, workflow, configuration, customization, delegation, or override may invert, bypass, or weaken these structural rules.

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

### 5.3 Ownership Links and ownership identity

1. Each independent Business Identity must have one or more explicit Ownership Links to its Owner or Owners.
2. Each Owner is identified by an Owner ID. Each individual Owner-to-Business relationship is identified by a distinct Ownership ID.
3. One Owner ID may be linked to multiple independent Business Identities. A Business Identity may be linked to multiple Owner IDs.
4. Owner ID identifies the Owner; Ownership ID identifies the particular Ownership Link; Business Identity identifies the independent business. These identifiers and concepts are not interchangeable.
5. In this CBM model, ownership records identify who owns or holds ownership rights in a business for authority-scope purposes. They do not represent or calculate equity, financial shares, percentages, investment, profit entitlement, or economic interest.
6. Owner identity and ownership are separate from User accounts, authentication credentials, Roles, Permissions, and operational assignments. An Owner does not automatically become a User, and an Owner's User account does not automatically receive access to every business linked to that Owner.
7. Separate businesses linked to the same Owner ID remain separate Business Identities and separate data contexts.
8. A Distributor does not own an Independent Shop merely because the Shop is in its Distribution Network or mapped to it.
9. An Organization's association with a Distributor does not itself change the Distributor's ownership or the ownership of any Shop associated with that Distributor.
10. Organization-owned Stores remain operational units of the Organization; their ownership remains with the Owner or Owners linked to the Organization.
11. Ownership Links may be used as an eligibility input when determining Business Available Authority Scope, but only under the explicit rules in Section 7. They do not by themselves grant User access, User Assigned Authority Scope, or action Permissions.

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

Main Portal's authority scope extends across the entire CBM Ecosystem without subordinate-imposed restriction. It covers all Sub-Portals, Organizations, Distributors, Stores, Independent Shops, and applicable business/service contexts within CBM.

No Sub-Portal, Organization, Distributor, Shop, User, or Role may narrow or constrain Main Portal's supreme authority scope.

### 7.3 Sub-Portal scope

A Sub-Portal's business/service-provider authority scope extends to the businesses and operational entities associated with and served within its authorized Sub-Portal domain. This includes the relevant Organizations, Distribution operations, Distributors, Organization-owned Stores, and Independent Shops associated with that domain, subject to Main Portal governance.

This provider-level scope does not transfer ownership to the Sub-Portal and does not automatically grant every Sub-Portal User access to every associated entity or record. Each Sub-Portal User must have an explicit valid User–Business association, User Assigned Authority Scope, and required Permissions, subject to purpose limitation and Data Isolation.

### 7.4 Organization scope

An Organization's Business Available Authority Scope includes:

- The Organization's own business context.
- Distributors participating under its Distribution Network, within the Organization's permitted governance and operational authority.
- Stores/Shops that are operational units owned and operated as part of the Organization's own Retail structure.
- Other directly subordinate operational entities that remain within the Organization's own business and ownership structure.
- An independently identified business only where a valid Ownership Link and an explicit Main Portal-governed rule make that business eligible for the relevant owner-linked authority scope.

An Organization's scope does **not** automatically pass through a Distributor to every Independent Shop mapped to or associated with that Distributor. A mapping, network association, or structural proximity alone does not place an independently owned Shop within the Organization's scope. Where an owner-linked scope is permitted, eligibility must arise from the relevant Owner ID/Ownership Links and governing rules—not from the Distributor–Shop mapping—and must not be treated as Organization ownership or a merger of data boundaries.

### 7.5 Distributor scope

A Distributor's Business Available Authority Scope always includes the Distributor's own business context.

In addition, it may include an independently identified Shop that is associated with the Distributor's distribution context **only when** an Owner ID linked to that Shop through an Ownership Link is also linked through an Ownership Link to the Distributor, and the applicable Main Portal-governed rules permit that owner-linked scope.

A Distributor's scope does not include other mapped Shops merely because they participate in its Distribution Network. A Shop owned by a different Owner ID remains outside the Distributor's ownership-based scope, even when mapped to the Distributor. Mapping enables only the expressly authorized relationship interactions; it does not create an Ownership Link or grant unrestricted administrative or data access.

Where a Distributor has multiple Owners, owner-linked eligibility must be evaluated against the explicit Ownership Links and applicable governance rules. It must not be inferred from a shared name, contact detail, User account, or informal association.

### 7.6 Independent Shop scope

An Independent Shop's own business scope includes its own Business Identity and business context. Its ownership and data boundary remain independent.

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

Assume Organization A's Business Available Authority Scope includes Distributor D1 and Organization-owned Stores S1, S2, and S3.

- User U1 is associated with Organization A and is assigned only Organization A and Store S1. U1 cannot access D1, S2, or S3 merely because U1 is an Organization-level User.
- User U2 is associated with Organization A and is assigned Distributor D1 and Store S2. U2 cannot access S1 or S3 unless those entities are also assigned.
- User U3 is explicitly assigned the full available scope and may access D1, S1, S2, and S3, subject to Permissions and all other controls.
- Independent Shops mapped to D1 but owned by unrelated Owners are not included in Organization A's scope merely because of the mapping, and cannot be added to these Users' assignments on that basis.

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

Approval requests, approver identity, decision, decision time, approval stage, and final outcome must be recorded for accountability and auditability.

---

## 12. Distributor–Shop business relationships and mapping

### 12.1 Relationship model

Distributor–Shop mapping is a business relationship between separate identities, not an ownership hierarchy. The relationship is independent from both parties' ownership and does not merge their data contexts.

The CBM Ecosystem must support authorized Distributor–Shop business mappings across the ecosystem. A Distributor must belong to an Organization's business structure and cannot exist as a standalone entity without an Organization. The Organization may receive CBM services directly from Main Portal or through a Sub-Portal; that service-provider relationship does not change the Distributor's structural placement. A Shop's mapping to a Distributor is a separate business relationship, not a change to either entity's structural parent.

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
- Does not change either party's ownership or create an Ownership Link.
- Does not convert an Independent Shop into a Distributor-owned child business.
- Does not, by itself or solely because of the mapping, expand the Distributor's Business Available Authority Scope to include the Shop. Any separate owner-linked eligibility must be determined under Section 7.
- Does not, by itself or solely because of the mapping, expand the Organization's Business Available Authority Scope through the Distributor to the Shop. Any separate owner-linked eligibility must be determined under Section 7 and must not be inferred from the mapping.
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

## 16. Stable Core, Configurable, Customizable, and Extensible design principles

All architectural decisions and capabilities must be evaluated using the Stable Core, Configurable, and Extensible principles, together with the additional design dimensions of customization, override capability, and applicability defined in Section 2.6. These are complementary design lenses, not mutually exclusive labels.

### 16.1 Stable Core

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

Configurability must not allow a lower level to weaken or bypass an immutable higher-level rule. Data Isolation and Business Structure Hierarchy and Structural Integrity are never configurable or overridable. Customization and configuration must be limited to the applicable business context and must not silently affect another business.

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

### 16.4 Customization, Override, and Applicability

- Customization may adapt only those product behaviours and choices that the governing Stable Core and Main Portal policies permit.
- Override is a controlled authority, not an inherent consequence of having access to a configuration screen. It requires explicit permission for the specific setting and business context.
- Applicability must be defined for every policy, configuration, workflow, permission, and extension where different business contexts may require different treatment.
- Business-specific customization and overrides must be isolated to their authorized context and must not propagate to sibling or unrelated businesses unless an explicit governing rule requires inheritance.
- None of these capabilities may weaken Data Isolation, Business Structure Hierarchy and Structural Integrity, ownership integrity, Main Portal supremacy, or other Stable Core constraints.

---

## 17. Required cross-concept invariants

The following statements summarize rules that must remain true across all future domain and technical designs.

1. Main Portal's authority scope covers the entire CBM Ecosystem and cannot be restricted by a subordinate.
2. Authority flows downward only; no subordinate authority flows upward.
3. A Sub-Portal is an independently owned Service Provider, not a customer Tenant/Organization.
4. A Service Provider does not become the owner of the customer business by providing software services.
5. A customer's Business Identity and ownership remain unchanged when its Service Provider changes.
6. Organization, Distributor, Independent Shop, and Organization-owned Store are not interchangeable entity types.
7. An Organization-owned Store is an operational unit within the Organization; an Independent Shop is a separate Business Identity.
8. Organization authority scope includes its own subordinate Distributors and Organization-owned Stores. It does not automatically pass through a Distributor to all Independent Shops mapped to that Distributor. Any owner-linked eligibility must be separately established under Section 7 and must not be inferred from mapping.
9. A Distributor's Business Available Authority Scope includes itself and may include only those associated Independent Shops whose Ownership Links establish an eligible shared Owner ID with the Distributor, subject to Main Portal-governed rules. Other mapped Shops remain outside that owner-linked scope.
10. An Independent Shop retains its own Business Identity, ownership, and data boundary even when it has an owner-linked authority relationship with another business.
11. Business Available Authority Scope determines which entities are eligible for business-level authority and User-scope assignment; it does not itself grant access to any User.
12. Every User must have an explicit valid User–Business association and an explicit User Assigned Authority Scope for the target entity. User level, title, Role, Owner identity, structural position, or mapping does not automatically grant the full available scope.
13. Role/Permission determines what actions a User may perform; it cannot expand the User's assigned entity scope.
14. A valid action requires the target to be within the relevant Business Available Authority Scope and the User Assigned Authority Scope, together with the required Permission and any applicable approval/control.
15. Delegated Authority is constrained by Who, What, Where, Limits, and Duration.
16. Configuration defaults and inheritance flow downward. Overrides require explicit permission and must stay within system-defined bounds.
17. Configuration overrides may be granted to one business and withheld from another.
18. Approval requirements can vary by business, action, Role, and individual User.
19. Approval workflows support single, multiple, sequential, and parallel approval patterns.
20. Approval does not create an underlying action Permission.
21. Distributor–Shop mapping is a Business Relationship, not ownership and not unrestricted access.
22. One Shop may map to multiple Distributors; each mapping is a separate relationship.
23. Ending a mapping does not close the Shop or terminate its other mappings.
24. Distribution closure does not automatically erase the identity or history of independent associated Shops.
25. Sub-Portal migration preserves identity, ownership, data rights, and internal structure; only Service Provider changes.
26. Operational data may be removed after the applicable download period only after archival is verified.
27. No User receives access to a business solely because the User, Owner, Service Provider, or another associated business has a relationship with it. Access requires explicit valid association, assigned scope, and Permissions.
28. Business and Tenant Data Isolation are strict Stable Core constraints and cannot be weakened through configuration, customization, extension, delegation, or override.
29. Business Structure Hierarchy and Structural Integrity are strict Stable Core constraints: invalid entity placement and parent–child arrangements cannot be created, configured, customized, delegated, or overridden.
30. Owner ID identifies an Owner; Ownership ID identifies one specific Ownership Link; neither is a User ID or Business ID.
31. Ownership Links identify ownership for business-authority purposes, not equity percentages, financial shares, or economic interest.
32. Ownership Links may inform Business Available Authority Scope only under explicit Main Portal-governed rules. They do not automatically grant User access or action Permissions.
33. Every feature, policy, configuration, workflow, permission, and extension must have deliberate applicability to the business contexts for which it is intended.
34. Scalability, configurability, extensibility, customization, override capability, and applicability must be considered throughout architecture design; none grants permission to violate Stable Core constraints.
35. A business-specific variation must remain within its authorized business context and must not silently affect another business.

## 18. Illustrative scenarios

These scenarios explain how the finalized rules work together. They do not create additional authority or ownership rights beyond the rules above.

### 18.1 One owner, multiple businesses

Person A owns Organization A, Distributor A, and Independent Shop A. The same Owner ID is linked to each independent Business Identity through a separate Ownership ID for each Owner-to-Business relationship. These remain three independent Business Identities, with separate business data and service contexts. Shared ownership does not merge them or automatically give every User access to all three.

### 18.2 Organization with directly owned Stores

Organization A owns Store 1, Store 2, and Store 3 as operating units. The Organization's Business Available Authority Scope includes these Stores. The Stores can have separate operational identifiers and different User Assigned Authority Scopes and Role/Permission access, but they are not separate independent Business Identities solely because they are different locations.

### 18.3 Organization and Distributor, with independently owned Shops under the Distributor

Organization A has Distributor D1 in its Distribution Network. Distributor D1 is linked to Owner B through an Ownership Link. Shop X is independently owned and mapped to D1. If Shop X is also linked to Owner B through its own Ownership Link, and Main Portal-governed rules permit the owner-linked scope, Shop X may be included in D1's Business Available Authority Scope. Shop Y is mapped to D1 but is linked to a different Owner ID; Shop Y is not included in D1's owner-linked scope merely because of the mapping. Neither mapping changes either Shop's Business Identity, Owner, or data boundary.

Organization A's scope does not automatically pass through D1 to Shop X or Shop Y. Any eligibility for Shop X must arise from the explicit Owner ID/Ownership Links and applicable governance rules, not from the Distributor–Shop mapping.

### 18.4 Selective User scope within one business

Organization A's Business Available Authority Scope includes Distributor D1 and Organization-owned Stores S1, S2, and S3. User U1 is assigned only S1; User U2 is assigned D1 and S2; User U3 is explicitly assigned the full available scope. Their Organization-level association does not automatically give them identical access. Each User's effective access is further limited by that User's Permissions and applicable approvals.

### 18.5 Owner identity is not a User account

Person A is linked as an Owner to Organization A and Independent Shop A. This does not automatically create a login account or grant operational access. If Person A uses CBM, a valid User account, User–Business association, User Assigned Authority Scope, and Permissions are still required.

### 18.6 Sub-Portal serving an Organization and related business network

Sub-Portal A serves Organization A and the relevant associated distribution, store, and shop entities within its authorized domain. Its service-provider authority scope covers those associated businesses under Main Portal governance. This does not make Sub-Portal A their owner or grant every Sub-Portal User unrestricted access to every record.

### 18.7 Different approval rules for the same action

Within one Organization, the same action may require approval for Role A, may not require approval for Role B, and may have a separate user-specific approval rule for User C. The Organization configures these rules within the governing limits. The initiating User must still possess the action Permission and the assigned scope for the target entity.

### 18.8 Configuration override for one business only

Main Portal establishes a default setting. Organization A has explicit permission to override it and sets a permitted value within the system-defined range. Organization B has no override permission and continues to use the applicable higher-level value. Organization A's change does not silently change Organization B.

### 18.9 Service Provider migration

Organization A moves from Sub-Portal A to Sub-Portal B under Main Portal governance. Organization A's Business Identity, ownership, internal structure, data rights, and valid historical relationships remain intact. The Service Provider relationship changes.

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
- Apply the architecture-wide design doctrine: evaluate scalability, configurability, extensibility, customization, override capability, and applicability.
- Treat Data Isolation and Business Structure Hierarchy and Structural Integrity as strict, non-overridable Stable Core constraints.
- Identify the master decisions on which it depends.
- Avoid introducing contradictory or broader rights by implication.
- Be updated when a governing master decision changes.

### 19.5 Change control

A proposed change is not final merely because it has been suggested or documented in a discussion. The project owner must explicitly approve it. After approval, the master source of truth is updated first, and dependent documents are reconciled.

---

## 20. Source basis and decision provenance

This baseline consolidates:

1. The user-provided project context document **CBM_Project_Context_and_Decisions_2026-11-17** (provided in the conversation as the CBM project context text), including its confirmed business, service-provider, ownership, mapping, continuity, migration, archival, and Stable Core principles.
2. The subsequent project-owner confirmations and clarifications in the conversation through **18 November 2026**, especially the accepted Business Identity and Ownership model, the Service Provider explanation, the corrected Business Authority Scope model, the separation between scope and User/Role Permissions, hierarchical configuration inheritance and business-specific overrides, and fine-grained approval workflow configurability.

Where a later explicit project-owner clarification refined an earlier general description, the later clarification governs. This includes the precise distinction between:

- Main Portal's ecosystem-wide authority scope;
- Sub-Portal authority scope over associated businesses in its authorized domain;
- Organization authority scope over its own subordinate Distributors and Organization-owned Stores/Shops, but not Independent Shops beneath a Distributor merely because of mapping; and
- Distributor Business Available Authority Scope includes the Distributor itself and may include associated Independent Shops linked to the same Owner ID only under the explicit owner-linked eligibility rules in Section 7; mapping alone does not confer that scope.

---

**End of authoritative finalized baseline.**