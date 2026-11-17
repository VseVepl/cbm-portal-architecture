# Central Business Management (CBM) Portal — Architecture Reference

This repository is the documentation foundation for the **Central Business Management (CBM) Portal** project.

## Authoritative source

The single authoritative source for finalized business and architectural decisions is:

- [`docs/CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md`](docs/CBM_MASTER_BUSINESS_AND_ARCHITECTURE_DECISIONS.md)

Read that document before drafting, reviewing, or implementing any project-specific architecture, security, data, API, user-experience, operational, or development document.

## Architecture-wide design doctrine

Every architecture and product decision must consider scalability, configurability, extensibility, customization, controlled override capability, and explicit applicability to the correct business contexts. Business-specific variation should be supported within one governed platform wherever appropriate.

Business and Tenant Data Isolation and the Key Hierarchy are strict, non-configurable Stable Core constraints. No customization, configuration, delegation, extension, or override may weaken or bypass them. Flexibility must always remain within Main Portal governance and all other Stable Core rules.

## Documentation rule

Only decisions explicitly finalized by the project owner belong in the authoritative master document. Discussion notes, rejected approaches, unapproved recommendations, and unresolved matters must not be presented as finalized decisions. Keep discussion history separate from the master source of truth.

## Current project stage

The documented baseline covers the finalized project vision and business-level concepts available as of **17 November 2026**. This repository is documentation-only at this stage. It does not authorize application implementation, database changes, infrastructure changes, or deployment.