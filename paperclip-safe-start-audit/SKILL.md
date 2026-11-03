---
name: paperclip-safe-start-audit
description: Use when auditing local Paperclip startup safety.
version: 0.1.0
author: th3m3taway, Hermes Agent
license: MIT
platforms: [macos, linux]
metadata:
  hermes:
    tags: [paperclip, startup, audit, safety]
    related_skills: [systematic-debugging]
---

# Paperclip Safe Startup Audit

Audit an existing local Paperclip instance without confusing setup, a live API, or installed adapters with verified company/agent identity. Do not change databases, credentials, profiles, or business work merely to make inventory possible.

## When to Use

- Restore or verify an existing local Paperclip service with preservation constraints.
- Enumerate companies and agents through documented read-only REST endpoints.
- Determine whether a restart can dispatch pending work despite timer controls.

## Procedure

1. Resolve the exact existing instance, configuration, runtime-info and installed CLI paths from user context or filesystem discovery with `search_files`. Never use package-manager commands that could install or update a package. Reduce configuration to an allowlist before output; do not dump mixed secret-bearing JSON.
2. Inspect installed package metadata, CLI help, local API docs, and the actual installed startup code with `read_file` and `search_files`. With `terminal`, invoke the discovered CLI using its existing Node entrypoint and `--help` before using its flags. Avoid `env` commands that print secrets.
3. Check actual processes and TCP listeners with `terminal`. Compare configured origin with runtime-info origin and check both recorded server PID and embedded PostgreSQL PID. Stale files are evidence, not running services. Do not remove lock files or start duplicates.
4. Run bounded loopback GET probes to both candidate origins with no cookies or authorization when local-trusted mode is configured. Fetch only documented endpoints. Distinguish connection refusal, HTTP authentication failures, and successful JSON responses.
5. Trace startup side effects before authorization to start is exercised. Inspect migrations, secret/auth backfills, managed-home changes, agent reconciliation, plugin job/schedule startup, chat/email recovery, and any queue delivery. Check whether each is actually gated by the proposed control. If exclusions cannot be enforced through a supported startup path, stop before launching; record a precise safety/approval blocker rather than patching installed code or wrapping a custom substitute server.
6. Only when the inspected startup path fits the user's scope, start the existing instance with `terminal(background=true)`, retain its returned session/log handle, and keep the listener loopback/private. Do not use detached unmanaged shell jobs. Never initialize a cluster, replace a database, reset authentication, or approve migrations as a startup workaround without separate authorization.
7. Read back the exact live origin through HTTP. Use documented `GET /api/companies`, then `GET /api/companies/{companyId}/agents` for every returned company. Save sanitized batches incrementally, preserve native Paperclip IDs and adapter types, and deduplicate/count in code. A Paperclip agent ID is distinct from an external runtime/profile identity.
8. Write the requested audit through `write_file`. Record exact configured/recorded/verified origins separately, live PID or null, auth mode and whether access was actually proven, managed background handle or null, source references, HTTP evidence, company IDs, agent IDs/types, and blockers. Parse and count the artifact in `terminal`; unknown database totals must remain null, not zero.

## Maintenance-only startup preparation

When normal startup can process work, keep the original instance offline and prepare a source-anchored maintenance plan instead of treating a heartbeat-disable flag as a universal safety switch. The operator selected this approach for this workstation. Do not patch the live npm cache or silently duplicate a secret-bearing database. Require a reviewed startup boundary before backfills/queues/plugins, explicitly allowlisted GET endpoints, read-only database enforcement, and independent egress/process containment. Test against an authorized isolated copy before proposing use on the original. Database read-only mode alone does not stop external sends. A preparation document is not an implemented maintenance service.

### Isolated implementation and synthetic verification

- Verify the candidate's Git top-level and tracked manifests with `terminal` before cloning or making a worktree; a directory can inherit an unrelated parent vault's repository while containing no tracked application source. Pin a clean public source checkout to the installed build's recorded commit when local tracked source is absent, and disclose any necessary source fetch separately from dependency installation.
- Use a separate executable entry before normal startup imports; a flag checked inside the full server cannot undo module-load instrumentation or service imports. Reuse direct schema/query subpaths rather than broad DB/service barrels that also expose migration or execution code.
- Execute strict TDD against a newly initialized, explicitly synthetic PostgreSQL cluster. Apply the tracked schema only during fixture preparation, use a non-owner SELECT-only role plus session/transaction read-only checks, and compare complete logical table snapshots containing queued runs, plugin jobs, deliveries, and fake legacy auth records.
- Treat migration-journal order literally. Upstream indices and timestamps can have gaps, duplicates, or non-monotonic values; do not normalize them. Compare exact SQL hashes and recorded journal entries, and fail closed instead of reconciling history. Validate the inventory relation kinds/column types and reject views, foreign tables, and RLS policies before reads.
- Reuse existing dependencies without installing packages. When native PostgreSQL is already available, fixture-only direct binary invocation avoids wrapper repair/chmod behavior in an installed cache. Disable Unix sockets for TCP-only fixtures if a long profile path exceeds the socket-path limit.
- Verify a closed built import graph and instrument the actual maintenance child for process, worker, scheduler, filesystem-write, and non-fixture network calls; self-test the traps. JavaScript traps and loopback binding are not OS egress isolation. Keep future real-data/production trials separately authorized.
- Repeat process/listener checks after testing. Parse listener stdout even when `lsof` exits nonzero: querying several ports can produce partial success. If an unexpected production startup appears, record ownership read-only, do not silently query/stop it, and do not claim the original remained offline. Normalize timestamp representations before comparing metadata.

## Synthetic macOS containment verification

Use a generated `sandbox-exec` profile to independently enforce and test the synthetic maintenance process: `(deny network*)`, `(deny process-fork)`, plus exact loopback TCP exceptions for the fresh fixture DB and HTTP listener. Validate integer ports and reject production ports before policy interpolation. The macOS policy parser accepts `localhost:<port>`, not `127.0.0.1:<port>`; keep application binds literal IPv4 loopback. Verify the allowed DB connection, a reachable disallowed loopback TCP listener, reserved-address TCP, UDP denial, and child-process EPERM. Treat timeouts as failures, not proof of sandbox denial. Handle UDP socket error events as well as send callbacks. Exercise the actual compiled maintenance entry under the policy, compare synthetic logical data, and confirm cleanup. Do not claim filesystem, DNS-broker, universal native-code, or production isolation from these bounded tests. Keep synthetic fixtures and code-only tests separate from real-data permission grants.

## Pitfalls

- `paperclipai run` bootstraps and runs doctor repairs by default. The observed `--no-repair` flag limits doctor repair, not all server startup side effects; inspect the installed build rather than extrapolating.
- The inspected build treats non-interactive pending migrations as approved unless `PAPERCLIP_MIGRATION_PROMPT=never` is set. `PAPERCLIP_MIGRATION_AUTO_APPLY=true` overrides that protection; migration-history reconciliation may run before the approval branch. Do not assume an unknown schema is safe.
- `HEARTBEAT_SCHEDULER_ENABLED=false` is not a universal no-dispatch switch. Independent plugin jobs, chat/email delivery, startup queues, GitHub polling, and cleanup can remain active. Verify these paths in the actual installed build.
- Existing built-in-agent reconciliation is not proof that new agents must be created. Inspect the provisioning set and distinguish possible modifications from confirmed executed effects.
- Local-trusted board requests need no authentication. `GET /api/agents/me` still requires an authenticated agent; never treat board access as proof of an agent identity.
- Adapter packages are installed capabilities, not configured agents or verified routes. Do not map runtime identities by display-name equality.
- If the API is unavailable, empty collected arrays mean no records observed. They do not establish that the existing database is empty.

## Verification

- Require an HTTP JSON API read before claiming the service live.
- Preserve all runtime/process/log handles exactly and distinguish stale PIDs from live ones.
- Verify the artifact parses, its computed counts match collected arrays, all requested companies were traversed when possible, and completeness/blockers are explicit.
- Compare hashes of preflight instance metadata after the audit when preservation matters; never expose credential values as evidence.
