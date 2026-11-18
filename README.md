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

The approved Master baseline also defines **Platform-wide Services** as a distinct Architecture Layer, with four Service Scopes (Ecosystem-wide, Service Provider-scoped, Tenant-scoped, and Business-context-scoped), three Category Groups, and fourteen Categories. It distinguishes Category, Service, and Capability as conceptual levels and preserves the existing authority, authorization, security, approval, audit/history, and strict data-isolation boundaries. The Master document is authoritative for the complete taxonomy and responsibility rules.

## Documentation rule

Only decisions explicitly finalized by the project owner belong in the authoritative master document. Discussion notes, rejected approaches, unapproved recommendations, and unresolved matters must not be presented as finalized decisions. Keep discussion history separate from the master source of truth.

## Current project stage

The documented baseline covers the finalized project vision and business-level concepts available as of **18 November 2026**, including the approved Business Entity Lifecycle, Subscription, and Customer Business User Account rules. This repository is documentation-only at this stage. It does not authorize application implementation, database changes, infrastructure changes, or deployment.