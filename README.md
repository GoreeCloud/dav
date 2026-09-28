# GoreeCloud DAV

GoreeCloud DAV is GoreeCloud's first-party standards-interoperability service for WebDAV, CalDAV, and CardDAV.

## Current lifecycle

**Lifecycle:** Development

The authoritative default branch currently contains project documentation and repository-native feature-state records. The substantive native DAV implementation remains unmerged in draft pull request #1.

Accordingly, current main does **not** establish complete WebDAV, CalDAV, CardDAV, sync-token, production, or Stable functionality.

## Project authority

- [PROJECT-SPECIFICATIONS.md](./PROJECT-SPECIFICATIONS.md) — canonical project requirements and acceptance boundaries.
- [PROJECT-RECORD.md](./PROJECT-RECORD.md) — significant project history, evidence, and review blockers.
- [IMPLEMENTED-FEATURES.md](./IMPLEMENTED-FEATURES.md) — verified accepted implementation state.
- [PLANNED-FEATURES.md](./PLANNED-FEATURES.md) — planned feature state.
- [CHANGELOGS.md](./CHANGELOGS.md) — release and repository change history.

## Current implementation candidate

Draft PR #1 contains the native Go DAV foundation. Its exact head 993802393d8ad832b10f29f70f57efa8612dee3f passed CI and Platform Contract validation but has unresolved submitted review blockers involving non-loopback plaintext Development exposure and non-atomic conditional PUT behavior.

Green CI does not clear those blockers or make the candidate accepted main.

## Product boundary

DAV is the external standards-interoperability boundary for calendar/address-book DAV protocols. GoreeCloud Sync remains separately responsible for its approved synchronization and transfer scope.

The GitHub repository becomes the authoritative project-specification/project-record location after this migration is accepted and verified.
