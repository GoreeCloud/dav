# GoreeCloud DAV — Project Record

**Document Type:** Repository-Native Project Record
**Status:** Active
**Project:** GoreeCloud DAV
**Repository:** GoreeCloud/dav
**Authority:** Repository-local project record
**Last Updated:** 2026-09-27

## 2026-09-27 — Repository-local project migration staged

GoreeCloud DAV project requirements and significant project history are being migrated from transitional Google Drive sources into PROJECT-SPECIFICATIONS.md and PROJECT-RECORD.md.

Historical repository naming `GoreeCloud/goreecloud-dav` is reconciled to the live repository `GoreeCloud/dav`.

Current GitHub `main` controls accepted implementation state.

At migration start, `main` is commit `7637cd6cb30ecf8f752c16f1b59109358690161c` and contains repository-native feature-state records but no accepted native DAV service implementation.

The current README statement implying DAV protocol support is therefore corrected in the migration candidate to distinguish product purpose/planned capability from accepted implementation.

Both Drive project-specification sources remain protected until review, merge, authoritative main readback, and final reconciliation succeed.

## 2026-09-27 — Feature-state migration accepted

Pull request #3, **Migrate DAV feature tracking from Drive**, was merged.

- PR head: `412999888b77c2896d24e128f756a5ebdbc51155`
- Merge commit: `7637cd6cb30ecf8f752c16f1b59109358690161c`

The repository established:

- IMPLEMENTED-FEATURES.md;
- PLANNED-FEATURES.md;
- CHANGELOGS.md.

This did not establish native service acceptance, release readiness, or production status.

## Draft PR #1 — Native DAV implementation candidate

Pull request #1, **feat: implement native GoreeCloud DAV foundation**, remains open and draft.

Exact candidate head at migration time:

`993802393d8ad832b10f29f70f57efa8612dee3f`

Exact-head validation:

- CI run `33817190439` — passed;
- Platform Contract run `33817191022` — passed.

The draft candidate includes a substantial native Go DAV foundation, including loopback-safe configuration, Development authentication, atomic filesystem storage, DAV principal isolation, RFC 6764 redirects, OPTIONS discovery, baseline PROPFIND, collection creation, GET/HEAD/PUT/DELETE, ETags, conditional writes, calendar/address-book query and multiget handling, storage containment hardening, health/readiness/status interfaces, tests, CI, and a Platform Contract v0.2 declaration.

These are **candidate branch** capabilities, not accepted `main` implementation.

No submitted review/approval was present in the recorded candidate state.

## 2026-09-08 — Draft PR #1 safety review blockers

A submitted review on exact head `993802393d8ad832b10f29f70f57efa8612dee3f` identified two blockers that are not covered by the green CI results:

1. **Plaintext non-loopback Development exposure.** The candidate permits a non-loopback listener when Development username/password credentials are configured while the executable serves ordinary HTTP. This can expose Basic credentials and DAV content in plaintext. Development must remain loopback-only unless an explicitly accepted secure transport boundary is required and validated.

2. **Non-atomic conditional PUT.** The candidate evaluates `If-Match` / `If-None-Match` before calling a storage write that does not bind the expected ETag/existence state to publication. Concurrent conditional PUTs can therefore validate the same old state and overwrite one another. Conditional-write enforcement must move into an atomic storage operation, serialization boundary, or equivalent compare-and-swap mechanism with a lost-update regression test.

The review explicitly states that green CI on the candidate head does not cover these safety conditions and that a corrected head must be revalidated before Ready-for-Review status.

These blockers remain part of the project record until authoritative PR #1 evidence demonstrates they are resolved.

## DAV compliance correction

The draft implementation intentionally emits no DAV compliance class/token.

Earlier development wording implying `DAV: 1` was superseded because applicable RFC 4918 class-1 MUST requirements remain incomplete, including PROPPATCH, COPY, MOVE, and complete WebDAV property behavior.

CalDAV `calendar-access` and CardDAV `addressbook` tokens are likewise withheld pending applicable standards implementation and interoperability qualification.

This correction preserves a fail-closed standards-claim boundary.

## Product direction preserved from Drive

The Drive specification established these enduring project decisions:

- DAV is the CalDAV/CardDAV/WebDAV interoperability boundary, not general GoreeCloud synchronization authority.
- GoreeCloud Sync remains separate.
- Native application/service APIs and Mesh contracts remain internal authorities rather than being redefined by DAV.
- Go is the service implementation language.
- Production authentication is intended to use GoreeCloud Identity.
- Privacy Shield, Wardveil Security, Everkeep, Manager, Mesh, and applicable platform governance remain separate acceptance requirements.
- The service is headless; Glaze UI becomes applicable only to later graphical surfaces.
- Filesystem persistence is a Development storage boundary, not a permanent source-of-truth mandate.
- Standards/conformance claims require protocol and client evidence.
- Radicale is a benchmark, not an architectural upstream.
- The project is AGPL-3.0-only.

## Repository-authority transition

Historical Drive documentation and draft PR #1 still identify Google Drive as the canonical project specification.

After this migration is accepted and verified on `main`:

- PROJECT-SPECIFICATIONS.md becomes the canonical project specification;
- PROJECT-RECORD.md becomes the significant project-history/evidence record;
- IMPLEMENTED-FEATURES.md controls accepted implementation;
- PLANNED-FEATURES.md controls planned feature state;
- CHANGELOGS.md controls repository-native change history;
- Google Drive is no longer a parallel project-specification authority.

Draft PR #1 must be reconciled against this authority model before it can be accepted. Its Drive-authority statements and any competing `SPECIFICATIONS.md` must not reintroduce a second canonical specification.

## Current acceptance boundary

GoreeCloud DAV remains Development.

No accepted `main` evidence currently establishes:

- native DAV runtime availability;
- RFC 4918 class-1 conformance;
- complete CalDAV/CardDAV conformance;
- sync-token support;
- WebDAV ACL;
- production GoreeCloud Identity;
- accepted Privacy Shield / Wardveil Security / Everkeep / Manager / Mesh integration;
- external production exposure;
- broad client interoperability;
- Stable or production approval.

Those claims require future accepted evidence.
