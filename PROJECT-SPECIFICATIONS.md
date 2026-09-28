# GoreeCloud DAV — Project Specifications

**Document Type:** Repository-Native Project Specification
**Status:** Active specification / Development
**Project:** GoreeCloud DAV
**Repository:** GoreeCloud/dav
**Authority:** Repository-local project specification
**Implementation Language:** Go
**License:** AGPL-3.0-only
**Last Updated:** 2026-09-27

## 1. Purpose

GoreeCloud DAV is GoreeCloud's native standards-interoperability service for CalDAV, CardDAV, and the WebDAV capabilities required by those protocols.

Its purpose is to let standards-compatible calendar, task, journal, and contact clients communicate with GoreeCloud-managed data without making a third-party DAV server the permanent GoreeCloud architecture.

Radicale and similar DAV servers are compatibility/usability references only. GoreeCloud DAV is original GoreeCloud software and is not a Radicale fork, wrapper, rebrand, or architectural continuation.

## 2. Product Boundary

DAV is an external standards-interoperability boundary, not GoreeCloud's general synchronization authority.

GoreeCloud Sync remains responsible for approved file/folder synchronization, nearby transfer, and temporary sharing.

The intended long-term flow is:

```text
External DAV clients
  → CalDAV / CardDAV / WebDAV
  → GoreeCloud DAV
  → approved GoreeCloud service APIs / GoreeCloud Mesh
  → Calendar / Contacts / Tasks / other applicable services
```

GoreeCloud DAV must not become an independent competing source of truth once native application storage contracts exist.

A filesystem store may be used as a Development persistence and portability boundary, but it is not a permanent product requirement.

## 3. Current Accepted Repository State

The project is in **Development**.

Authoritative `main` at the start of this migration is commit:

`7637cd6cb30ecf8f752c16f1b59109358690161c`

That revision contains repository-native feature-state records from accepted PR #3 but **does not contain an accepted native DAV service implementation**.

Draft PR #1, **feat: implement native GoreeCloud DAV foundation**, is the current implementation candidate.

Its exact head at migration time is:

`993802393d8ad832b10f29f70f57efa8612dee3f`

Exact-head CI run `33817190439` and Platform Contract run `33817191022` passed.

Those checks validate candidate source at that head; they do not make the draft branch accepted `main`, production-ready, Stable, or standards-conformant.

## 4. Standards and Compatibility Targets

The service must implement applicable portions of:

- WebDAV — RFC 4918;
- CalDAV — RFC 4791;
- CardDAV — RFC 6352;
- iCalendar — RFC 5545;
- vCard — RFC 6350;
- WebDAV Access Control — RFC 3744 where required;
- WebDAV Sync — RFC 6578 where supported;
- RFC 6764 service discovery where applicable;
- HTTP conditional requests, ETags, collection discovery, and interoperable HTTP semantics.

Standards claims require implementation evidence, automated protocol tests, and representative client interoperability evidence.

Source code presence or a partial method set is not complete RFC conformance.

## 5. DAV Compliance-Token Policy

GoreeCloud DAV must not advertise stronger DAV compliance classes/tokens than its accepted implementation supports.

In particular:

- RFC 4918 class `1` must be withheld until all applicable class-1 MUST requirements are satisfied.
- CalDAV `calendar-access` must be withheld until the applicable RFC 4791 requirements and interoperability qualification are complete.
- CardDAV `addressbook` must be withheld until the applicable RFC 6352 requirements and interoperability qualification are complete.

Method discovery through `Allow` or other bounded capability information may describe implemented methods without implying full standards conformance.

## 6. Native Architecture

Go is the preferred implementation language because DAV is an infrastructure/network service and lightweight API.

The architecture should contain clear boundaries for:

- HTTP/DAV transport;
- principals and collection discovery;
- CalDAV;
- CardDAV;
- DAV properties and multistatus responses;
- conditional requests and ETags;
- calendar/address-book resource validation;
- replaceable storage;
- authentication and authorization providers;
- GoreeCloud platform integrations;
- health, readiness, and implementation status.

Protocol helper libraries and storage backends must be replaceable without redefining the product.

## 7. Resource Model

The logical model distinguishes:

- principals;
- calendar homes;
- address-book homes;
- calendar collections;
- address-book collections;
- calendar resources such as `.ics`;
- contact resources such as `.vcf`;
- collection metadata;
- ETags and conditional-write state.

Protocol paths and storage paths must be normalized, validated, and constrained so client input cannot escape the configured data root.

## 8. Authentication and Authorization

GoreeCloud Identity is the intended production authority for:

- account identity;
- service identity;
- roles and permissions;
- authorization;
- applicable delegated access.

Development may use a bounded local/basic provider solely to make protocol behavior testable before production Identity integration exists.

Such a provider must be explicitly non-production and must not be described as GoreeCloud Identity integration.

Development service exposure must fail safely. Loopback-only defaults are required unless a separately accepted design establishes authenticated external exposure.

## 9. Privacy Requirements

DAV processes potentially sensitive calendars, contacts, tasks, journals, account identifiers, sharing relationships, and metadata.

Privacy requirements include:

- purpose limitation;
- data minimization;
- bounded retention;
- export/deletion behavior;
- controlled sharing and transfer;
- no routine logging of DAV payload bodies;
- no unnecessary contact/calendar content in diagnostics;
- separation of authentication/audit metadata from protected content;
- explicit network/exposure behavior.

Privacy Shield conformance requires substantive integration and validation, not documentation alone.

## 10. Security Requirements

Applicable Wardveil Security and service-level security requirements include:

- bounded request bodies;
- path traversal protection;
- symlink/storage-root containment;
- explicit authentication boundaries;
- conditional-write protections;
- secure credential/session handling;
- minimal external exposure;
- abuse resistance;
- integrity validation;
- privacy-conscious diagnostics;
- fail-closed behavior when required security evidence or authorization is missing.

Credentials and reusable secrets must not be committed to repository history.

External exposure is a separately reviewed deployment decision.

## 11. Recovery and Everkeep

DAV synchronization is not backup.

Calendar/contact/task data must remain independently recoverable.

Requirements include:

- replaceable storage boundary;
- standards-based export where applicable;
- backup of durable service state;
- clean restore procedures;
- migration and portability support;
- restoration testing;
- separation of reconstructible cache from irreplaceable state.

Everkeep conformance requires tested recovery evidence.

## 12. GoreeCloud Manager

Manager integration may include:

- service inventory;
- lifecycle state;
- version/status;
- health and readiness;
- configuration visibility;
- maintenance state;
- authorized administrative operations.

Health/readiness/status endpoints are useful interfaces but do not, by themselves, constitute accepted Manager integration.

## 13. GoreeCloud Mesh

Mesh may provide:

- capability discovery;
- service discovery;
- versioned service relationships;
- platform events;
- communication with Calendar, Contacts, Tasks, and other applicable GoreeCloud services.

DAV-specific integration paths must not bypass approved Mesh contracts where Mesh applies.

## 14. Glaze UI

The core DAV product is a headless network service.

Glaze UI is therefore not applicable to the initial service runtime itself.

If a GoreeCloud-controlled user-facing or administrative graphical/web interface is introduced, that surface must comply with the current applicable Stable Glaze UI authority before acceptance.

## 15. Planned Native Foundation Capability

The initial service milestone is intended to provide, once accepted:

- loopback-only HTTP service with graceful shutdown;
- RFC 6764 well-known redirects;
- conservative OPTIONS method discovery;
- DAV principal/calendar-home/address-book-home discovery;
- PROPFIND Depth 0/1 with allprop, propname, and explicit property selection;
- separate unavailable-property status handling;
- bounded request bodies;
- finite-depth enforcement;
- MKCALENDAR and collection creation;
- GET/HEAD of stored resources;
- PUT of validated iCalendar/vCard resources;
- SHA-256 ETags;
- If-Match / If-None-Match protections;
- DELETE of supported resources/Development collections;
- baseline calendar-query, calendar-multiget, addressbook-query, and addressbook-multiget REPORT handling;
- namespace validation;
- collection-scoped href validation;
- per-href multiget statuses;
- bounded/safe filtering;
- data projection that avoids returning calendar/vCard payloads unless requested;
- filesystem persistence with atomic writes;
- request-size/path-traversal protections;
- Development authentication provider boundary;
- health, readiness, and status endpoints;
- automated tests and CI.

Until PR #1 or a successor is accepted on `main`, these are candidate/planned capabilities rather than accepted repository implementation.

## 16. Deferred Capabilities

Deferred or incomplete work includes:

- PROPPATCH;
- COPY and MOVE;
- complete live/dead property semantics;
- full WebDAV ACL behavior;
- RFC 6578 synchronization tokens and change journals;
- scheduling extensions;
- server-side invitation workflows;
- recurrence-aware calendar filtering;
- full address-book filtering semantics;
- native application datastore adapters;
- production GoreeCloud Identity;
- accepted Privacy Shield / Wardveil Security / Everkeep / Manager / Mesh integration;
- broad interoperability qualification;
- load/performance qualification;
- production TLS/reverse-proxy guidance and acceptance;
- Stable lifecycle qualification.

No deferred item may be represented as accepted merely because it appears in a specification or draft branch.

## 17. Storage and Data Safety

The service must:

- reject traversal and unsafe identifiers;
- avoid following client-controlled paths outside the storage root;
- bound bodies before unbounded allocation;
- use atomic publication for resource changes;
- generate deterministic entity tags from stored bytes;
- enforce applicable conditional headers;
- preserve principal/collection isolation;
- protect against symlink escape;
- avoid sensitive payload logging;
- separate secrets from ordinary configuration.

Storage implementation may evolve, but these safety properties remain required.

## 18. Repository Requirements

The repository must maintain:

- README.md;
- PROJECT-SPECIFICATIONS.md;
- PROJECT-RECORD.md;
- IMPLEMENTED-FEATURES.md;
- PLANNED-FEATURES.md;
- CHANGELOGS.md;
- FEATURES.md where useful;
- BENEFITS.md;
- COMPETITIVE-OBJECTIVES.md;
- BRANDING.md;
- SECURITY.md or equivalent security guidance;
- architecture documentation;
- CI validation;
- configuration examples without secrets;
- license information.

A deprecated `SPECIFICATIONS.md` must not become a competing canonical specification.

## 19. Competitive Objective and Provenance

Radicale is a benchmark for lightweight DAV deployment and interoperability, not the product architecture.

GoreeCloud DAV should preserve the useful qualities of lightweight DAV services while improving:

- GoreeCloud-native identity;
- privacy governance;
- security integration;
- recovery awareness;
- administration;
- first-party service interoperability.

Competitive work must not copy upstream branding, protected assets, incompatible code, interface design, or architecture.

GoreeCloud DAV is original GoreeCloud-owned AGPL-3.0-only software. Third-party dependencies retain their own licenses.

## 20. Deployment and Exposure

The service must default to a bounded Development exposure model.

Production exposure requires separately accepted evidence for:

- authentication/authorization;
- TLS and ingress configuration;
- reverse-proxy behavior;
- request/body limits;
- abuse controls;
- health/readiness;
- logging/privacy;
- backup/recovery;
- monitoring;
- rollback/disable path;
- supported clients and protocols.

A working loopback Development server is not production acceptance.

## 21. Stable Qualification

Stable qualification requires evidence appropriate to claimed support, including:

- applicable WebDAV/CalDAV/CardDAV conformance;
- protocol test suites;
- representative client compatibility;
- production Identity authorization;
- Privacy Shield and Wardveil Security acceptance;
- Everkeep recovery evidence;
- Manager/Mesh integration where applicable;
- packaging/deployment;
- external exposure;
- monitoring;
- recovery/rollback;
- exact release evidence.

CI success alone does not establish Stable or production approval.

## 22. Current Platform Contract Boundary

Draft PR #1 contains a GoreeCloud Platform Contract v0.2 declaration and validated workflow.

That declaration belongs to the unmerged candidate branch and therefore is not accepted `main` state.

Even if later accepted, its schema/version and platform-system declarations must be reconciled with current GoreeCloud platform governance at the time of production/Stable qualification.

## 23. Maintenance

PROJECT-SPECIFICATIONS.md must evolve when standards targets, architecture, storage, authentication, privacy/security, platform integrations, deployment, or acceptance requirements materially change.

PROJECT-RECORD.md preserves significant history and evidence.

IMPLEMENTED-FEATURES.md records accepted implementation only.

PLANNED-FEATURES.md records planned work.

CHANGELOGS.md records repository/release-oriented changes.

After migration acceptance and default-branch readback, Google Drive must no longer remain a parallel project-specification or project-record authority.
