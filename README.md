# GoreeCloud DAV

GoreeCloud DAV is GoreeCloud's native CalDAV, CardDAV, and supporting WebDAV interoperability project.

## Current lifecycle

**Development.**

The authoritative `main` branch currently contains project/feature documentation only. The native Go DAV service implementation remains in draft pull request #1 and is **not yet accepted implementation**.

Draft PR #1 has passed its recorded exact-head CI and Platform Contract checks, but those checks do not make the draft branch production-ready, Stable, standards-conformant, or part of authoritative `main`.

## Project authority

- [PROJECT-SPECIFICATIONS.md](./PROJECT-SPECIFICATIONS.md) — normative protocol, architecture, security/privacy, integration, deployment, and acceptance requirements.
- [PROJECT-RECORD.md](./PROJECT-RECORD.md) — significant project history and accepted/candidate evidence boundaries.
- [IMPLEMENTED-FEATURES.md](./IMPLEMENTED-FEATURES.md) — accepted implementation state on the authoritative branch.
- [PLANNED-FEATURES.md](./PLANNED-FEATURES.md) — planned capabilities and obligations.
- [CHANGELOGS.md](./CHANGELOGS.md) — repository-native change history.

After project-record migration acceptance and authoritative main readback, GitHub is the project-specification and project-record source of truth.

## Product boundary

DAV is the external standards-interoperability boundary for calendars, contacts, tasks/journals where applicable.

GoreeCloud Sync remains the separate file/folder/transfer synchronization authority.

Native GoreeCloud applications should use their approved service APIs and Mesh contracts internally rather than making DAV the universal internal data model.

## Standards targets

The project targets applicable portions of:

- WebDAV (RFC 4918);
- CalDAV (RFC 4791);
- CardDAV (RFC 6352);
- iCalendar (RFC 5545);
- vCard (RFC 6350);
- WebDAV ACL (RFC 3744) where required;
- WebDAV Sync (RFC 6578) where supported;
- RFC 6764 discovery.

No DAV compliance class/token is accepted on `main` today.

## Draft native implementation

Draft PR #1 contains a substantial Go Development candidate, including bounded DAV method handling, filesystem-backed storage, path and body protections, Development authentication boundaries, health/readiness/status interfaces, tests, CI, and a Platform Contract declaration.

That candidate remains subject to review/acceptance and must be reconciled to the repository-native project-record authority before merge.

## License

GoreeCloud DAV is original GoreeCloud-owned software licensed **AGPL-3.0-only**.
