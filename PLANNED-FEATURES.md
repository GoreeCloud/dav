# GoreeCloud DAV — Planned Features

> **Authority:** Repository-native planned-feature record. Plans and draft branches are not accepted implementation evidence.

**Status:** Active roadmap control
**As of:** 2026-09-27
**Authoritative project specification:** PROJECT-SPECIFICATIONS.md
**Canonical repository:** GoreeCloud/dav

## Roadmap

| ID | Feature / obligation | Priority | Current state |
| --- | --- | --- | --- |
| DAV-001 | Complete repository-local project-specification/project-record migration and permanently retire Drive sources after main readback. | High | Migration candidate |
| DAV-002 | Reconcile draft PR #1 with PROJECT-SPECIFICATIONS.md / PROJECT-RECORD.md authority before any merge. | Critical | Required |
| DAV-003 | Review and accept the native Go DAV foundation only after required repository review/approval gates. | High | Draft PR #1 |
| DAV-004 | Complete RFC 4918 class-1 requirements before advertising class-1 DAV compliance. | Critical | Planned |
| DAV-005 | Complete applicable CalDAV RFC 4791 requirements and interoperability evidence before advertising calendar-access. | High | Planned |
| DAV-006 | Complete applicable CardDAV RFC 6352 requirements and interoperability evidence before advertising addressbook. | High | Planned |
| DAV-007 | Implement complete property mutation/live-dead property behavior, COPY/MOVE, and applicable ACL semantics. | High | Planned |
| DAV-008 | Implement RFC 6578 sync tokens/change journal where required. | High | Planned |
| DAV-009 | Add production GoreeCloud Identity authentication/authorization. | Critical | Planned / blocked |
| DAV-010 | Integrate applicable Privacy Shield and Wardveil Security controls with evidence. | Critical | Planned / blocked |
| DAV-011 | Establish Everkeep backup/restore/recovery evidence independent of synchronization. | Critical | Planned |
| DAV-012 | Integrate Manager and Mesh where applicable without bypassing owning-service authority. | High | Planned |
| DAV-013 | Add native application datastore/service adapters so DAV does not become a competing application source of truth. | High | Planned |
| DAV-014 | Validate broad standards-client interoperability and performance/load behavior. | High | Planned |
| DAV-015 | Establish production TLS/reverse-proxy/exposure configuration and rollback acceptance. | Critical | Planned |
| DAV-016 | Keep graphical UI out of scope unless introduced; any future GoreeCloud-controlled UI must use current Stable Glaze UI authority. | Medium | Governing boundary |
| DAV-017 | Complete exact-release security/privacy/recovery/deployment evidence before Stable qualification. | Critical | Planned |

## Maintenance

Google Drive roadmap synchronization is retired.

Update this file from accepted repository implementation, PROJECT-SPECIFICATIONS.md, current platform governance, and GoreeCloud Tasks Management. Missing obligations, stale status, duplicated work, or undocumented disposition changes are defects.
