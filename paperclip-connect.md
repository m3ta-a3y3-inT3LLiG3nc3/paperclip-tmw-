# Paperclip Connect — Ingest Plan

**Status: STAGED — plan only. The Paperclip connection is not live yet.**

## Target

- Paperclip company: **Th3 M3ta Way**
- Company ID: `72a661f6-b1fc-4fdb-9167-1c96bd8360b7`
- Connection identity: connects **as M3ta himself**, not as a Paperclip agent.
- Paperclip runs on M3ta's Mac (local server).

This repo is the Paperclip company's natural home: the company is literally
named "Th3 M3ta Way", and this is the Th3 M3ta Way operations hub.

## Connection status

**Pending M3ta's approval.** A "Paperclip Setup" shortcut was placed on the
Mac Desktop (2026-10-15). The connection completes when M3ta:

1. Double-clicks the Desktop shortcut on the Mac.
2. Signs in as himself on the setup page and approves the MCP connection.
3. Confirms back in chat so the connection can be verified from Muse's side.

As of 2026-10-17 the approval has not been confirmed, and the Mac's Muse-app
device channel is the route for verification once he approves.

## What Paperclip should ingest from this repo (once connected)

- `README.md` — the Th3 M3ta Way ops hub overview, tool roster, and the
  domain-separation / no-overlap guardrails.
- `docs/orbit-agency-link.md` — the staged Orbit main-workspace link plan and
  its blockers.
- `docs/rea-integration.md` — the REA reverse-engineering pipeline plan.
- This file — so Paperclip's picture of its own connection state stays honest:
  staged until approved, never assumed live.

## Ingest notes

- Ingest order: README first (company/ops context), then the two plan docs.
- Facts only — staged plans must be ingested *as plans*, with their blocked /
  pending status intact, never as completed work.
- No secrets: this repo contains no tokens, keys, or endpoint URLs, and none
  may be added. Paperclip credentials are handled only through the setup flow
  above.

## After connection

1. Verify the connection landed as M3ta (not as an agent).
2. Feed the ingest set above.
3. Update this file's status to CONNECTED with the verification date.
