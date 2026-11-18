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
14. [GST Taxpayer Profile and GST Registration Architecture](#12a-gst-taxpayer-profile-and-gst-registration-architecture)
15. [Sub-Portal migration and service continuity](#13-sub-portal-migration-and-service-continuity)
16. [Data archival and object-storage principles](#14-data-archival-and-object-storage-principles)
17. [Domain History and Audit Trail Management](#14a-domain-history-and-audit-trail-management)
18. [Stable Core, Configurable, Customizable, and Extensible principles](#15-stable-core-configurable-customizable-and-extensible-design-principles)
19. [Cross-concept invariants](#16-required-cross-concept-invariants)
20. [Illustrative scenarios](#17-illustrative-scenarios)
21. [Documentation governance](#18-documentation-governance)
22. [Source basis and decision provenance](#19-source-basis-and-decision-provenance)
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

A Distributor must have exactly one direct Parent Organization as part of its Business Structure. That Parent Organization is mandatory when the Distributor is created; a Distributor must not be created as unassigned, standalone, or with multiple structural parents. The Parent Organization relationship establishes structural membership only and does not determine ownership, Legal Identity, Professional Identity, or the Distributor's Business Relationships. A Distributor must have a Professional Identity. Its own Legal Identity is optional and must be explicitly established and validated where applicable; it is not automatically inherited from its Organization or another entity. Structural placement does not determine ownership. The Owner or Owners of a Distributor are determined only through explicit Ownership Links.

### 3.10 Shop

A **Shop** is the unified CBM retail business entity. The previous two-category classification of Shops has been discontinued. CBM must not model Shops as separate entity types or separate Business Structure classifications based on ownership.

A Shop must have exactly one direct Parent Organization as part of its Business Structure. That Parent Organization is mandatory when the Shop is created; a Shop must not be created as unassigned, standalone, or with multiple structural parents. The Parent Organization relationship establishes structural membership only and does not determine ownership, Legal Identity, Professional Identity, or the Shop's Business Relationships. A Shop is a terminal structural entity under the currently approved Business Structure model: it cannot have another Shop, a Distributor, or any other business entity as a structural child. An Organization may have multiple Shops and may have both Shops and Distributors at the same time.

Every Shop must have its own Professional Identity. Its own Legal Identity is optional and must be explicitly established and validated where applicable. A Shop may have its own Legal Identity or may be associated with another eligible Legal Identity, subject to verified legal facts and applicable law. A Shop's structural placement does not determine who owns it. Ownership is determined independently through explicit Ownership Links; therefore, a Shop structurally belonging to an Organization is not necessarily owned by that Organization or by the same Owner as the Organization.

The Shop entity remains the same regardless of its ownership arrangement, Legal Identity, Professional Identity, or Business Relationships. Those are separate dimensions and must not create separate Shop structural classifications.

### 3.11 Business Structure

**Business Structure** means the permitted structural placement and direct Parent–Child relationships among CBM's structural entities. The currently approved structure is Main Portal → Sub-Portal for Platform Structure, and Organization → Distributor / Shop for Customer Business Structure. Distributor and Shop are terminal structural entities. Service Provider relationships, Ownership Links, Business Identity associations, Business Relationships/Mapping, Authority Scope, User Scope Mapping, Roles, and Permissions are separate models and must not be inferred from structural placement.

### 3.12 Business Structure

**Business Structure** means the internal arrangement of a business and its directly subordinate operational or business components. It is distinct from a relationship between two independently owned businesses.

### 3.13 Business Relationship

A **Business Relationship** is an explicitly established association between distinct business identities, such as a Distributor–Shop Mapping. A relationship enables only the interactions and permissions expressly allowed for that relationship. It does not transfer ownership or automatically grant broad administrative authority or data access.

### 3.14 Business Authority Scope

**Business Authority Scope** is the business-level authority boundary that identifies the business entities or business areas for which authority may be exercised or assigned. It answers: **“Where may this business-level authority be applied?”** It does not itself grant access to an individual User or define which actions that User may perform.

#### 3.14.1 Business Available Authority Scope

**Business Available Authority Scope** is the set of entities or business areas that a business-level authority is eligible to govern or make available for assignment, after applying Main Portal governance, valid Business Structure, applicable Ownership Links, permitted Business Relationships, and all relevant Stable Core constraints. It is the maximum eligible scope from which User-level scope may be assigned. Availability is not an automatic User grant.

#### 3.14.2 User Assigned Authority Scope

**User Assigned Authority Scope** is the specific set of entities, operational units, locations, or business areas explicitly assigned to a User through one or more User Scope Mappings, within a valid User–Business association and subject to the assigning authority's permitted scope. It may cover one entity, selected multiple entities, or the full scope the assigning authority is permitted to assign. Full-scope assignment must be explicit. It must never be inferred from a User's level, title, Role, ownership, business association, parent–child position, Business Available Authority Scope, or any other Mapping. Explicit exclusions may be applied where the governing assignment model permits them.

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

The Parent Organization relationship identifies the entity's place in the Customer Business Structure. It does not establish ownership, determine Legal Identity or Professional Identity, create a Distributor–Shop Business Relationship, or by itself grant every User access or every possible management action. Authority availability and individual User access continue to be determined by Sections 7, 8, and 9.

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
10. Shops are direct structural children of an Organization, but their ownership is determined independently through explicit Ownership Links. A Shop's structural placement does not establish that it is owned by the Organization or by the Organization's Owner(s).
11. Ownership Links may be used as an eligibility input when determining Business Available Authority Scope, but only under the explicit rules in Section 7. They do not by themselves grant User access, User Assigned Authority Scope, or action Permissions.

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
- Shop entities that are direct structural children of the Organization, where and to the extent expressly included by applicable Main Portal-governed authority rules. Their structural placement does not itself grant authority or determine ownership.
- An independently identified business only where a valid Ownership Link and an explicit Main Portal-governed rule make that business eligible for the relevant owner-linked authority scope.

An Organization's Business Available Authority Scope must be determined under explicit Main Portal-governed authority rules. A Shop's structural placement under an Organization does not itself establish ownership or unrestricted authority. A Distributor–Shop Mapping does not, by itself, expand the Organization's authority scope through the Distributor to another business context. Any additional authority must be expressly authorized and must preserve independent ownership and Business Data Isolation.

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

## 13. Sub-Portal migration and service continuity

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
- Relevant domain-specific History records and Audit Trail records, preserved separately and according to their respective retention rules.
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

## 14. Data archival and object-storage principles

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
- Domain-specific History records and Audit Trail records, preserved separately according to their respective retention rules.
- Verification of successful archival before deletion of the operational copy.

### 15.3 Separation from operational data

Archival is preservation, not continuation of normal operations. Removing an operational copy after the required download window and successful archival does not mean that the archived business history is erased or that ownership is transferred.

---

## 14A. Domain History and Audit Trail Management

### 15A.1 Mandatory separation

**Domain History** and **Audit Trail** are separate and independent record categories. They have different purposes, contents, structures, retention rules, and access permissions. They must not be merged into one table, one undifferentiated log, one combined domain History, or one permission domain. An event may create both records, but each is maintained independently for its own purpose.

### 15A.2 Domain-specific History and approved History domains

A History record set belongs to one clearly defined domain object, entity, or relationship. Each approved History domain must have its own dedicated domain-specific table/record set; a shared mixed-domain History table is not permitted. It records meaningful business changes and events that authorized business Users need to understand. It must contain only events and information belonging to that domain.

The History domains established in this baseline are:
1. **GST Registration History:** GST Registration and GST Registration Assignment events only.
2. **Distributor–Shop Mapping History:** events concerning the specific Distributor–Shop relationship only.
3. **Ownership History:** events concerning the relevant Owner ID, Ownership ID, and Ownership Link only.
4. **User Scope Mapping History:** events concerning the relevant User-to-entity/location scope assignment only.

A User ID or User profile change must never be inserted into GST Registration History, Ownership History, or Mapping History. A separate User account/profile History may be introduced only if a distinct business need is established and approved. Other History domains must not be assumed merely because Audit Trail records exist; each requires an explicit business requirement and a definition of scope, contents, permissions, and retention.

### 15A.3 History contents, integrity, and retention

Each History record set records only fields relevant to its own domain. As applicable, it may include affected domain object/relationship, event type, previous and new domain values/states, effective date/time, recorded date/time, reason, responsible actor, approver, and supporting reference. A significant domain change must not silently overwrite the only record of its previous state. Corrections must preserve the original entry and identify the correction or superseding event.

History must be preserved throughout the existence of the relevant Business Identity/business context. It must not be routinely purged or deleted while that business remains in existence. It may be removed only as part of authorized permanent deletion of the relevant business.

History may be available to multiple appropriately authorized Users. Its visibility is controlled by separate History permissions and applicable business scope. Permission to view History does not grant permission to perform the underlying action or to view the Audit Trail.

### 15A.4 Comprehensive system-wide Audit Trail

Audit Trail must capture all relevant system events and activities across CBM in chronological order. Its coverage is not limited to the History domains above or to security events alone.

As applicable, Audit Trail includes successful and failed actions, creation and modification attempts, access and authentication events, User and Role changes, User Scope Mapping changes, Ownership Link changes, Business Relationship/Mapping changes, GST verification and assignment events, configuration changes, approvals and rejections, administrative actions, data exports, archival and deletion operations, and security events.

Audit Trail must capture sufficient context to reconstruct what happened, when, in which business/entity context, by whom or by what system process, and with what outcome. It may contain events from multiple domains in one chronological sequence. It does not replace any domain-specific History.

### 15A.5 Separate access permissions

1. History and Audit Trail must have separate access permissions.
2. A User may view relevant domain History without being allowed to view Audit Trail.
3. Audit Trail access is restricted to specifically authorized roles/Users under Main Portal governance and applicable security policy.
4. Access to one domain's History does not grant access to another domain's History.
5. Permission to view History does not grant permission to create, change, or revoke the underlying business object.
6. Audit Trail permission does not automatically grant access to all business History.
7. Viewing or exporting either record category is itself an activity captured in the Audit Trail, subject to applicable policy.

### 15A.6 Retention, archival, and deletion

Domain History is retained for the full lifetime of the relevant business context and may be removed only through authorized permanent deletion of that business. It must not be purged according to an Audit Trail retention schedule.

Audit Trail has a separate retention and archival policy. It is not required to be permanent in every case. Retention periods, archival, and eligible deletion are governed by Main Portal policy and applicable legal, regulatory, contractual, and security requirements.

Business archival must preserve Domain History and Audit Trail separately where each remains within its applicable retention period. Their integrity, provenance, and access controls remain independently enforceable after archival or migration.

### 15A.7 Paired-record examples

When a GSTIN is changed, GST Registration History records only the previous and new GST registration/assignment, effective date, recorded date, reason, actor, and approver where applicable. The Audit Trail separately records relevant system events, including actor/session, request and outcome, verification, authorization, and approval events.

When a User ID is changed, that event is recorded in the comprehensive Audit Trail. It is not inserted into GST Registration History. A separate User History is permitted only if a distinct business need is approved.

When a Distributor–Shop Mapping is created, changed, suspended, or ended, the specific Mapping History records the business chronology of that relationship. The Audit Trail separately captures system events and actions associated with those operations.

---

## 15. Stable Core, Configurable, Customizable, and Extensible design principles

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

## 16. Required cross-concept invariants

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
27. Every Distributor and Shop must have exactly one direct Parent Organization when created; neither may be created unassigned, standalone, or with multiple structural parents.
28. A Distributor–Shop Mapping does not require either party to select, know, or disclose the other party's Parent Organization.
29. Distributor–Shop Mapping activation requires successful two-way verification: the Shop's code must first be verified by the Distributor, and a separate Distributor confirmation code must then be verified by the Shop.
30. Both mapping codes must expire, be single-use, be protected by rate limits, and support revocation. An expired, consumed, or revoked code cannot complete its verification stage.
31. An Active Mapping permits only Mapping-Type-authorized business transactions and necessary information exchange. It does not change ownership, Structural Membership, Parent Organization, or either party's Business Data Boundary, and it does not grant broad or unrestricted data access.
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

## 17. Illustrative scenarios

These scenarios explain how the finalized rules work together. They do not create additional authority or ownership rights beyond the rules above.

### 17.1 One owner, multiple businesses

Person A may hold separate Ownership Links to Organization A, Distributor A, and Shop A where the ownership facts support each relationship. Each Owner-to-Business relationship has its own Ownership ID. Their Professional Identities remain separately maintainable. Where two or more of these business contexts share one Legal Identity, that shared Legal Identity does not merge their Professional Identities, business data, or service contexts. Shared ownership does not merge independent businesses or automatically give every User access to all of them.

### 17.2 Organization with multiple Shops

Organization A has Shop S1, Shop S2, and Shop S3 as direct structural children. Each Shop has its own Professional Identity and operational context. Each Shop's ownership is determined independently through its Ownership Links; a Shop may be owned by the same Owner(s) as the Organization or by another Owner. Its own Legal Identity is optional and must be explicitly associated and validated where applicable. A shared Legal Identity does not merge the Shops' Professional Identities or operational records. The Organization's Business Available Authority Scope and each User's assigned scope are determined independently under the applicable authority rules.

### 17.3 Organization, Distributor, and mapped Shops

Organization A has Distributor D1 and Shop S1 as direct structural children. Distributor D1 is linked to Owner A through an Ownership Link, while Shop S1 is linked to Owner B through a separate Ownership Link. Shop S1 also has a Business Mapping with D1. The Mapping does not make Shop S1 a structural child of D1, change its Owner, or merge their data boundaries. Neither D1 nor S1 needs to select, know, or disclose the other party's Parent Organization to establish the Mapping. Any authority eligibility beyond the structural scope must be established independently under explicit Main Portal-governed rules.

The Organization's direct structural relationship with Shop S1 is independent of the Distributor–Shop Mapping. The Mapping itself does not enlarge either party's authority scope.

### 17.4 Selective User scope within one business

Organization A's Business Available Authority Scope includes Distributor D1 and Shops S1, S2, and S3. User U1 is assigned only S1; User U2 is assigned D1 and S2; User U3 is explicitly assigned the full available scope. Their Organization-level association does not automatically give them identical access. Each User's effective access is further limited by that User's Permissions and applicable approvals.

### 17.5 Owner identity is not a User account

Person A is linked as an Owner to Organization A and Shop A through separate Ownership Links. This does not automatically create a login account or grant operational access. If Person A uses CBM, a valid User account, User–Business association, User Assigned Authority Scope, and Permissions are still required.

### 17.6 Sub-Portal serving an Organization and related business network

Sub-Portal A serves Organization A and the relevant associated Distributor and Shop entities within its authorized domain. Its service-provider authority scope covers those associated businesses under Main Portal governance. This does not make Sub-Portal A their owner or grant every Sub-Portal User unrestricted access to every record.

### 17.7 Different approval rules for the same action

Within one Organization, the same action may require approval for Role A, may not require approval for Role B, and may have a separate user-specific approval rule for User C. The Organization configures these rules within the governing limits. The initiating User must still possess the action Permission and the assigned scope for the target entity.

### 17.8 Configuration override for one business only

Main Portal establishes a default setting. Organization A has explicit permission to override it and sets a permitted value within the system-defined range. Organization B has no override permission and continues to use the applicable higher-level value. Organization A's change does not silently change Organization B.

### 17.9 Service Provider migration

Organization A moves from Sub-Portal A to Sub-Portal B under Main Portal governance. Organization A's Business Identity, ownership, internal structure, data rights, and valid historical relationships remain intact. The Service Provider relationship changes.

### 17.10 GST Registration Assignment at different CBM levels

The GST Registration Assignment capability is not limited to Distributor–Shop relationships. Subject to legal eligibility and Main Portal policy, a GST Registration may be associated with the legal person/context operating Main Portal, a Sub-Portal service provider, an Organization, a Distributor, a Shop, or another enabled CBM entity/context. The GST Registration always belongs to the actual registered taxpayer. The CBM association identifies an eligible operational or transaction context; it does not change the taxpayer or permit an unrelated entity to use that GSTIN. No Owner ID, Business Structure, service-provider relationship, or other Mapping automatically establishes GSTIN eligibility.

### 17.11 Domain History and Audit Trail are separate

When a GSTIN is changed, GST Registration History records only the GST Registration-domain change, including previous and new registration/assignment, effective date, recorded date, reason, actor, and approver where applicable. A User ID change is not inserted into GST Registration History. The User ID change and GSTIN change may both appear as separate events in the comprehensive Audit Trail, in chronological order. Authorized business Users may view GST Registration History without being permitted to view Audit Trail.

### 17.12 Two-way Distributor–Shop Mapping verification

Shop S1 already exists in CBM under its own Parent Organization and has its own Shop Unique ID. Distributor D1 also already exists in CBM under its own Parent Organization and has its own Distributor Unique ID. S1 generates a Shop Secret Code and shares its Shop Unique ID and code with D1. An authorized D1 User enters them in D1's CBM context, and CBM verifies the first stage. D1 then generates a separate Distributor Confirmation Code for that pending mapping and shares it with S1. An authorized S1 User enters that confirmation code in S1's CBM context. Only after this second verification succeeds does the mapping become Active.

Neither party needs to select, know, or disclose the other party's Parent Organization. Both codes are subject to expiry, one-time use, rate limiting, and revocation. Once Active, the mapping enables only the business transactions and necessary information exchange authorized for its Mapping Type; it does not change either party's ownership, structural membership, Parent Organization, or independent data boundary.

## 18. Documentation governance

### 19.1 Documentation completeness

Project documentation must be sufficiently precise that a reader can understand each included concept without relying on undocumented assumptions. As applicable, definitions, purpose, scope, ownership, authority, constraints, relationships, behaviour, exceptions, and examples must be stated explicitly.

### 19.2 No unresolved matters in the master decision record

The master decision record contains finalized decisions only. An unresolved matter must be discussed and explicitly finalized before it is added. It must not be represented as a decision, and it must not be silently filled by an AI recommendation or an assumed technical convention.

### 19.3 Separate Project Discussion History from domain records and decisions

Chronological project discussions, reasoning, rejected approaches, and superseded wording belong in a separate **Project Discussion History** record if one is maintained. Project Discussion History is not Domain History and is not the Audit Trail. Neither Domain History nor Audit Trail belongs in the authoritative decision text as a substitute for project discussion history.

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

## 19. Source basis and decision provenance

This baseline consolidates:

1. The user-provided project context document **CBM_Project_Context_and_Decisions_2026-11-17** (provided in the conversation as the CBM project context text), including its confirmed business, service-provider, ownership, mapping, continuity, migration, archival, and Stable Core principles.
2. The subsequent project-owner confirmations and clarifications in the conversation through **18 November 2026**, including the finalized unified Shop entity and simplified Business Structure (Organization → Distributor / Shop), exactly one mandatory Parent Organization for every Distributor and Shop, the Legal Identity and Professional Identity applicability matrix, Organization Network Configurations, GSTIN eligibility independence, generic GST Registration Assignment, typed and purpose-specific Mapping, two-way Distributor–Shop Secret Code verification with expiry, one-time use, rate limiting and revocation, mapping-type-limited business transactions and information exchange, explicit User Scope Mapping, and the strict separation, contents, access, and retention distinctions between Domain History and Audit Trail.
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

---

**End of authoritative finalized baseline.**