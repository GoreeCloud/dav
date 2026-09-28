# GoreeCloud DAV — Project Record

**Document Type:** Repository-Native Project Record
**Status:** Active
**Project:** GoreeCloud DAV
**Repository:** GoreeCloud/dav
**Authority:** Repository-local project record
**Last Updated:** 2026-09-27

## 2026-09-27 — Project specification migration staged

GoreeCloud DAV project-specification authority is being migrated from transitional Google Drive sources into PROJECT-SPECIFICATIONS.md and PROJECT-RECORD.md.

The migration reconciles the historical repository name GoreeCloud/goreecloud-dav to the live GoreeCloud/dav repository.

Current GitHub main controls accepted implementation state. The Drive sources preserve product requirements and historical implementation context; they do not promote unmerged branch source into accepted main.

Both Drive project-specification sources remain protected until this migration is reviewed, accepted, merged, read back from main, and reconciled without outstanding discrepancies.

## 2026-09-27 — Feature-state migration accepted

Pull request #3, "Migrate DAV feature tracking from Drive," was merged to main as 7637cd6cb30ecf8f752c16f1b59109358690161c.

Its migration head was 412999888b77c2896d24e128f756a5ebdbc51155.

The migration established repository-native IMPLEMENTED-FEATURES.md, PLANNED-FEATURES.md, and CHANGELOGS.md.

No lifecycle promotion, production acceptance, or DAV conformance claim was established.

## Current accepted main boundary

At this migration checkpoint, authoritative main contains repository documentation and feature-state records but not the substantive native DAV implementation that exists in pull request #1.

The prior README sentence saying the service "supports WebDAV, CalDAV, CardDAV, sync tokens" is not accepted runtime evidence by itself and must not be used to infer merged protocol implementation.

Current main therefore does not establish production DAV functionality, RFC conformance, external exposure, Stable status, or platform-system acceptance.

## Draft PR #1 — native DAV foundation candidate

Pull request #1, "feat: implement native GoreeCloud DAV foundation," remains open, draft, and unmerged.

Exact head: 993802393d8ad832b10f29f70f57efa8612dee3f.

Exact-head validation passed:

- CI run 33817190439;
- Platform Contract run 33817191022.

Candidate scope includes original GoreeCloud-owned Go software for a bounded DAV foundation, including protocol discovery, baseline DAV resource handling, conditional requests, filesystem persistence, health/readiness/status surfaces, tests, CI, and Platform Contract declaration.

The candidate deliberately emits no DAV compliance token because RFC 4918 class 1, CalDAV calendar-access, and CardDAV addressbook requirements are not yet fully satisfied and qualified.

## 2026-09-08 — Submitted review blockers on PR #1

A submitted COMMENTED review on exact head 993802393d8ad832b10f29f70f57efa8612dee3f records two material blockers.

### Plaintext non-loopback Development exposure

Development configuration can permit a non-loopback listener when Development username/password credentials exist while the server uses plain HTTP.

This can expose Basic credentials and DAV data over plaintext transport.

Required correction: retain loopback-only Development serving or require an explicit, separately validated secure transport boundary before non-loopback binding is permitted.

### Non-atomic conditional PUT

The current candidate reads existing state and evaluates If-Match / If-None-Match before calling the storage write.

Because the storage mutation does not atomically enforce the precondition, concurrent conditional PUTs can validate the same old state and overwrite one another.

Required correction: move conditional-write enforcement into an atomic storage operation or equivalent serialization/compare-and-swap boundary and add a concurrent lost-update regression test.

Passing CI does not resolve these blockers.

## Product decisions preserved from Drive

The migration preserves these governing decisions:

- DAV is the GoreeCloud standards-interoperability boundary for WebDAV, CalDAV, and CardDAV, not the general GoreeCloud synchronization authority;
- GoreeCloud Sync remains responsible for its approved file/folder synchronization, nearby transfer, and temporary-sharing scope;
- DAV must not become a competing source of truth once native application storage contracts exist;
- Go is the intended implementation language for the native infrastructure service;
- production authentication authority belongs to GoreeCloud Identity;
- the local/basic Development provider is transitional and non-production;
- Privacy Shield, Wardveil Security, Everkeep, Manager, and Mesh require substantive implementation and validation before conformance claims;
- the core service is headless, so Glaze UI is justified not-applicable until a user-facing/admin interface exists;
- paths, bodies, resource identifiers, ETags, conditional writes, logs, and external exposure require explicit safety controls;
- source presence does not establish RFC conformance;
- Stable qualification requires applicable platform-system, security, privacy, recovery, compatibility, deployment, and exact-release evidence.

## Authority transition

After this migration is accepted and verified on main:

- PROJECT-SPECIFICATIONS.md becomes the canonical DAV project specification;
- PROJECT-RECORD.md becomes the canonical significant project-history/evidence record;
- IMPLEMENTED-FEATURES.md must reflect only verified accepted implementation;
- PLANNED-FEATURES.md owns planned feature state;
- CHANGELOGS.md owns release/repository change history;
- Google Drive no longer remains a parallel project-specification authority.

The Drive source documents become permanently deletable only after authoritative default-branch readback and final reconciliation succeed.
