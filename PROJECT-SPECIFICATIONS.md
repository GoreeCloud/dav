# GoreeCloud DAV — Project Specifications
Native CalDAV and CardDAV Interoperability Service
Document Metadata
Status: Active Development — repository specification; accepted main remains documentation/feature records while the native service foundation is unmerged in PR #1
Classification: Internal
Document Type: Software Project Specification and Implementation Blueprint
Official Full Name: GoreeCloud DAV
Approved Short Name: DAV
Brand Relationship: Prefixed GoreeCloud family service
Suite Membership: No — infrastructure/interoperability service
Repository: GoreeCloud/dav
Development Model: Original GoreeCloud-owned native software
Implementation Language: Go
License: AGPL-3.0-only
Authority: Repository-local project specification
1. Purpose
GoreeCloud DAV is the native GoreeCloud standards-interoperability service for CalDAV, CardDAV, and the WebDAV capabilities required by those protocols. Its purpose is to let standards-compatible calendar, task, journal, and contact clients communicate with GoreeCloud-managed data without making a third-party DAV application the permanent GoreeCloud architecture.
GoreeCloud DAV is inspired by the useful role served by lightweight DAV servers such as Radicale, but it is not a Radicale fork, rebrand, wrapper, or architectural continuation. The implementation is original GoreeCloud software built against public standards and independently selected foundational libraries.
2. Product Boundary
DAV is an external interoperability boundary, not GoreeCloud's general synchronization authority. GoreeCloud Sync remains responsible for approved file/folder synchronization, nearby transfer, and temporary sharing. DAV is responsible for standards-based calendar and address-book interoperability.
The intended long-term flow is:
External DAV clients → CalDAV/CardDAV/WebDAV → GoreeCloud DAV → approved GoreeCloud service APIs and GoreeCloud Mesh → GoreeCloud Calendar, GoreeCloud Contacts, GoreeCloud Tasks, and other applicable services.
GoreeCloud DAV must not become an independent competing source of truth for application data once native application storage contracts exist. The initial filesystem store is a development-stage persistence implementation and portability boundary, not a permanent requirement.
3. Standards and Compatibility Targets
The service will implement the applicable portions of:
• WebDAV — RFC 4918.
• CalDAV — RFC 4791.
• CardDAV — RFC 6352.
• iCalendar — RFC 5545.
• vCard — RFC 6350.
• WebDAV Access Control — RFC 3744 where required by the authorization model.
• WebDAV Sync — RFC 6578 where supported by the synchronization-token implementation.
• HTTP conditional requests, entity tags, collection discovery, and related interoperable HTTP semantics.
Compatibility claims require automated protocol tests and client interoperability evidence. Source code presence alone does not establish complete RFC conformance.
4. Native Architecture
The service is implemented in Go because GoreeCloud's language strategy identifies Go as the preferred language for infrastructure services, network services, synchronization services, daemons, and lightweight APIs.
Core architecture:
• HTTP/DAV transport layer.
• Principal and collection discovery.
• CalDAV engine.
• CardDAV engine.
• DAV property and multistatus engine.
• Conditional request and ETag handling.
• Calendar/address-book resource validation.
• Storage interface with filesystem implementation for the foundation.
• Authentication and authorization provider boundary.
• Platform integration boundaries for GoreeCloud Identity, Privacy Shield, Wardveil Security, Everkeep, GoreeCloud Manager, and GoreeCloud Mesh.
• Health, readiness, and implementation-status interfaces.
The service must remain capable of replacing the storage engine and protocol helper libraries without redefining the product.
5. Resource Model
The development resource model separates:
• Principals.
• Calendar homes.
• Address-book homes.
• Calendar collections.
• Address-book collections.
• Calendar resources such as .ics objects.
• Contact resources such as .vcf objects.
• Collection metadata.
• Entity tags and conditional-write state.
Resource paths and storage paths must be normalized, validated, and constrained so protocol input cannot escape the configured data root.
6. Authentication and Authorization
GoreeCloud Identity is the intended authority for production authentication, account identity, service identity, roles, permissions, and authorization.
The development foundation may expose a deliberately bounded local/basic development provider to make protocol behavior testable before GoreeCloud Identity integration is available. That provider is transitional, must be clearly identified as non-production, and must not be represented as GoreeCloud Identity integration.
The service must default to loopback-only operation during development and must fail safely rather than silently exposing unauthenticated DAV service on external interfaces.
7. Privacy Shield
Privacy Shield is applicable because DAV processes contacts, calendars, tasks, journals, account identifiers, sharing relationships, and potentially sensitive metadata.
Required integration will govern permitted collection, access, sharing, transfer, retention, export, and deletion. Logging must avoid capturing DAV payload bodies or unnecessary contact/calendar content. Current foundation work does not claim Privacy Shield conformance until the applicable contracts are implemented and validated.
8. Wardveil Security
Wardveil Security is applicable to request trust, authentication context, credentials, sessions, administrative operations, abuse resistance, integrity, and security events.
The native service foundation must provide secure defaults including bounded request bodies, safe path handling, conditional-write protections, minimal exposure, privacy-conscious diagnostics, and explicit authentication boundaries. Wardveil conformance remains incomplete until its current contracts are substantively integrated and validated.
9. Everkeep
Everkeep is applicable to backup, restore, migration, portability, and service continuity.
DAV synchronization is not backup. Calendar and contact data require independent recoverability. The storage interface and exportable standards formats must support future backup and restore workflows, but Everkeep conformance requires tested recovery integration rather than a documentation declaration.
10. GoreeCloud Manager
GoreeCloud Manager is applicable to service inventory, lifecycle state, version reporting, health, readiness, configuration visibility, maintenance state, and authorized administrative operations.
The foundation exposes health/readiness and implementation-status endpoints. These are Manager-oriented service interfaces but do not by themselves constitute completed GoreeCloud Manager integration.
11. GoreeCloud Mesh
GoreeCloud Mesh is applicable to future capability discovery, service discovery, platform events, and versioned communication with Calendar, Contacts, Tasks, and related GoreeCloud services.
DAV-specific integration paths must not replace approved Mesh contracts where Mesh applies. The initial filesystem implementation does not claim Mesh participation.
12. Glaze UI
The core goreecloud-dav repository is a headless network service. Glaze UI is therefore Not Applicable — Justified for the initial headless service surface. If a user-facing or administrative web interface is added, the current stable applicable Glaze UI contract becomes mandatory for that interface.
13. Initial Implemented Foundation Scope
The first source milestone is intended to provide real, testable implementation for:
• Loopback-only default HTTP service with graceful shutdown.
• OPTIONS capability discovery.
• DAV principal, calendar-home, and address-book-home discovery.
• PROPFIND Depth 0/1 multistatus responses with empty-body/allprop, propname, explicit property selection, separate 404 propstats for unavailable properties, bounded request bodies, and conservative rejection of omitted or infinite depth through DAV:propfind-finite-depth because infinite depth is not implemented.
• MKCALENDAR and collection creation.
• GET and HEAD of stored resources.
• PUT of validated iCalendar and vCard resources.
• ETag generation and If-Match / If-None-Match protections.
• DELETE of resources and development collections.
• Initial calendar-query, calendar-multiget, addressbook-query, and addressbook-multiget REPORT handling with bounded bodies, namespace validation, collection-scoped href validation, per-href multiget statuses, limited safe query filtering, query-depth handling, and requested-property/data projection that avoids returning calendar or vCard payloads unless explicitly requested.
• Filesystem persistence with atomic writes.
• Request-size limits and path-traversal protections.
• Development authentication provider boundary.
• Health, readiness, and status endpoints.
• Automated unit/integration tests and CI.
This milestone is not complete CalDAV/CardDAV conformance and is not production acceptance.
14. Deferred Capabilities
Deferred work includes:
• Full WebDAV property mutation and arbitrary dead-property persistence.
• Complete WebDAV ACL behavior.
• RFC 6578 synchronization tokens and incremental change journals.
• Scheduling extensions and server-side invitation workflows.
• Recurrence-aware calendar filtering beyond baseline resource filtering.
• Full address-book filtering semantics.
• Native application datastore adapters.
• GoreeCloud Identity production adapter.
• Privacy Shield, Wardveil Security, Everkeep, Manager, and Mesh conformance.
• Performance/load qualification.
• Broad client compatibility matrix.
• Production TLS/reverse-proxy deployment guidance and acceptance.
• Stable lifecycle qualification.
15. Security and Data Safety Requirements
The service must:
• Reject path traversal and unsafe collection/resource identifiers.
• Avoid following client-controlled paths outside the configured storage root.
• Bound request bodies before allocation.
• Use atomic file publication for resource changes.
• Generate deterministic entity tags from stored bytes.
• Enforce applicable conditional request headers.
• Keep credentials and secrets out of repository history.
• Avoid recording sensitive DAV payloads in routine logs.
• Default to a local-only development listener.
• Treat external exposure as a separately reviewed deployment decision.
16. Repository Requirements
The repository must maintain at minimum:
• README.md
• PROJECT-SPECIFICATIONS.md
• PROJECT-RECORD.md
• FEATURES.md
• BENEFITS.md
• COMPETITIVE-OBJECTIVES.md
• BRANDING.md
Because DAV is a network-facing and data-sensitive service, the repository should also maintain SECURITY.md, architecture documentation, CI validation, configuration examples without secrets, tests, and an explicit license.
17. Competitive Objective
Radicale is a compatibility and usability benchmark, not the source architecture. GoreeCloud DAV should preserve the lightweight deployment and standards interoperability users value in DAV servers while improving GoreeCloud-native identity, privacy governance, security integration, recovery awareness, administration, and first-party interoperability.
Competitive analysis must not copy upstream branding, interface design, product architecture, protected assets, or incompatible source code.
18. Licensing and Provenance
GoreeCloud DAV is original GoreeCloud-owned software licensed AGPL-3.0-only. Third-party dependencies retain their own licenses.
No Radicale source code is required for the native implementation. Public DAV standards, interoperability behavior, and independently selected foundational libraries may be used as engineering references within their applicable legal and licensing boundaries.
19. Development and Release Status
Current status is Active Development. The repository and implementation are not Stable, production-approved, or evidence of complete CalDAV/CardDAV compliance.
Stable qualification requires applicable Integral Platform System conformance, security/privacy/recovery validation, compatibility testing, packaging/deployment validation, and exact-release evidence consistent with GoreeCloud release requirements.
20. Completion Direction
The long-term target is a compact, self-hostable, standards-compatible GoreeCloud DAV service that provides a durable external compatibility boundary while GoreeCloud applications continue to use richer native APIs and Mesh contracts internally.
The product succeeds when external DAV clients can interoperate reliably without forcing GoreeCloud's internal architecture, authorization model, recovery model, or application data model to be defined by DAV or by a third-party server.
21. Implementation Record — September 3, 2026
A first native implementation milestone has been developed in GoreeCloud/dav on feature/native-dav-foundation and proposed through pull request #1. The implementation is not merged or Stable at this record.
Current branch source for this milestone includes loopback-safe configuration, development authentication with filesystem-compatible principal validation, atomic filesystem storage, DAV principal isolation, RFC 6764 well-known redirects, conservative OPTIONS method discovery, PROPFIND empty-body/allprop, propname and explicit property selection with finite-depth enforcement, MKCALENDAR and MKCOL, resource GET/HEAD/PUT/DELETE, SHA-256 ETags, conditional PUTs, calendar/address-book query and multiget REPORT handling with limited safe filters, same-authority and collection-scoped href validation, duplicate-href collapse, per-requested-resource status responses, requested-property/data projection, filesystem symlink-containment hardening, health/readiness/status interfaces, tests, CI, and a GoreeCloud Platform Contract v0.2 declaration. These are branch implementation claims; the newest exact-head CI revision remains subject to validation.
Protocol claim correction — superseding the earlier DAV: 1 statement: the foundation now emits no DAV compliance class/token. RFC 4918 class 1 is withheld because the source does not yet satisfy all applicable class-1 MUST requirements, including PROPPATCH, COPY, MOVE, and complete WebDAV property behavior. The CalDAV calendar-access and CardDAV addressbook tokens are likewise withheld until their applicable requirements and interoperability qualification are complete. RFC 6764 service discovery remains an explicit standards target.
Earlier source revisions passed repository-document validation, gofmt, go test ./..., go vet ./..., and go build ./cmd/goreecloud-dav. The branch has since received additional PROPFIND, REPORT, data-minimization, and Platform Contract changes; exact-head GitHub CI is required again for the newest revision before those changes are treated as validated source evidence.
The following remain incomplete and must not be represented as implemented conformance: RFC 4918 class-1 WebDAV behavior, including PROPPATCH, COPY, MOVE, and complete live/dead property semantics; full CalDAV/CardDAV compliance; WebDAV ACL; RFC 6578 synchronization tokens/change journal; complete report filtering; production GoreeCloud Identity; Privacy Shield; Wardveil Security; Everkeep; GoreeCloud Manager; GoreeCloud Mesh; external production exposure; broad client qualification; and Stable release qualification.
The earlier machine-readable schema-location gap is resolved: the repository now contains goreecloud.platform.yaml using GoreeCloud Platform Contract schema version 0.2 and an immutable-pinned reusable validation workflow. The manifest identifies GoreeCloud DAV as an unversioned development service, records current health/readiness/status interfaces, marks applicable Manager, Privacy Shield, Wardveil Security, Everkeep, Mesh, and Identity integrations as migration-required, justifies Glaze UI as not applicable for the current headless service, records required continuity obligations, and remains explicitly nonconformant with blockers. Manifest adoption and validation do not establish Stable qualification, production approval, or completed platform-system conformance.

22. Current Accepted-State and Review Boundary — September 27, 2026

Authoritative main at the start of this migration is commit 7637cd6cb30ecf8f752c16f1b59109358690161c.

Main contains repository documentation and repository-native feature-state records, including the accepted Drive feature-roadmap migration from pull request #3. The substantive native DAV source foundation remains unmerged in draft pull request #1 and therefore must not be represented as accepted implementation on main.

Pull request #1 current exact head is 993802393d8ad832b10f29f70f57efa8612dee3f. Exact-head CI run 33817190439 and Platform Contract run 33817191022 passed.

A submitted review on that exact head identifies two unresolved safety blockers:

1. Non-loopback Development serving can transmit Basic credentials and DAV data over plaintext HTTP because non-loopback binding can be configured while the executable serves plain HTTP. Development serving must remain loopback-only or require a separately validated secure transport boundary before non-loopback exposure.
2. Conditional PUT checking is not atomic with resource publication. If-Match / If-None-Match validation must be bound to the actual storage mutation through serialization, compare-and-swap, or equivalent atomic enforcement, with concurrent lost-update regression coverage.

Green CI does not clear these review blockers.

23. Repository Authority and Maintenance

PROJECT-SPECIFICATIONS.md is the canonical project specification after migration acceptance.

PROJECT-RECORD.md preserves significant history, evidence, review blockers, and authority transitions.

IMPLEMENTED-FEATURES.md records only verified accepted implementation state and must not infer PR #1 source as merged functionality.

PLANNED-FEATURES.md records planned feature state.

CHANGELOGS.md records repository/release change history.

Google Drive project-specification copies are transitional migration sources only. After this migration is accepted, verified on the authoritative default branch, and reconciled, those Drive copies must be permanently deleted.
