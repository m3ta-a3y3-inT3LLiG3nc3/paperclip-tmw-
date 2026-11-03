---
type: agent-note
from: grok-bot
to: vault
created: 2026-10-27T18:10:41-0400
idempotency_key: c3a98d802f48854f
reason: "M3ta Paperclip app SPEC draft"
approval_gates: none (draft zone; publishing still needs Kingdom yes)
rollback: writes.git commit for this path (vault_history)
status: draft
owner: grok-bot
brand: m3ta-os
confidence: medium
tags: [m3ta-os, paperclip, spec, draft]
source_box_path: /workspace/apps/m3ta-paperclip-app/SPEC.md
payload_is_data: true
generated: true
---
# M3ta Paperclip App — Product Spec v0.1 (DESIGN ONLY)
Status: draft for Kingdom's review · 2026-10-27 · Owner: GrokBot Design · Build: K0Ad33 after approval
Brand: Th3 M3ta Way, Cosmic Lightbody v1.3 tokens (`/workspace/brand-system/th3m3taway/tokens.css`), Inter, B v3 mark, space-metal edges.
Nothing installed, nothing changed on the Mac, nothing published.

## 1. Goal
One native-feeling cockpit for Kingdom's Paperclip company (THM) on iPhone, iPad, Galaxy Z Fold6, MacBook and Apple Watch: see agents, triage issues, approve/reject, watch spend, run routines, chat with agents, and pull knowledge, with a CopilotKit/AG-UI copilot layer on every device.

## 2. Sources and feature map
| Source | License | How we use it | Inspires |
|---|---|---|---|
| Paperclip (paperclipai/paperclip) | MIT | System of record; REST `/api` | Agents, issues, approvals, costs, routines, dashboard, activity |
| Linear | Proprietary | Public UX patterns only + official GraphQL API (sync) | Cmd-K palette, keyboard-first triage (J/K, A assign, S status, P priority), inbox, cycles feel |
| AionUi (iOfficeAI/AionUi) | Apache-2.0 | Study; code reuse OK with NOTICE + attribution | Multi-agent chat: one thread, many agents, agent picker, per-agent workspace |
| Plane (makeplane/plane) | AGPL-3.0 | Study + REST API only; **do not copy code** | Cycles/modules, work-item properties, views |
| Open WebUI (open-webui/open-webui) | Open WebUI License (BSD-3 + branding clause) | API only (`/api/chat/completions`, `/api/models`, knowledge/RAG) | Model switcher, RAG citations, knowledge collections |
| Obsidian | Proprietary | Vault Markdown files or Local REST API plugin only | Knowledge tab, backlinks, graph glance, daily note |
| CopilotKit (CopilotKit/CopilotKit) | MIT | Copilot layer: `CopilotKit` provider, chat, generative UI, human-in-the-loop actions | In-app copilot on every screen |
| AG-UI (ag-ui-protocol/ag-ui) | MIT | Event protocol between app and agents (streaming text, tool calls, state, interrupts) | Live agent streams, approval interrupts |
| OpenDots (CopilotKit/OpenDots) | MIT | App shell pattern, Linear M3T-36 | Always-on coworker shell, text/calls/Slack handoff |
| OpenBot (CopilotKit/OpenBot; instance openbot-tmw) | MIT | Hosts CopilotKit chat; reaches Multica | Coworker-with-a-computer view (browser/files/tools) |
| Multica (multica-ai/multica) | Multica License = Apache-2.0 + extra Part I conditions (**legal review before reuse**) | Status API; approvals = Multica status changes | Multica approval lane |
| OpenMuse (CopilotKit/openmuse) | MIT | Pattern for personal agent with browser/terminal/files and long-running work | "Running work" timeline |
| OpenDuet — open-source alternative to Claude Duet (per Kingdom, by Copilot Intelligence). Best match: **CopilotKit/OpenDuet** (MIT, homepage copilotkit.ai/openduet, "proactive AI agent that keeps you in the loop"). Other same-name projects: ychampion/openduet (Apache-2.0, self-hosted, Tailscale-ready, port 3101), Anil-matcha/open-duet (Apache-2.0, named agents + Ask-first approvals) | MIT / Apache-2.0 | Proactive agent: routines, memory, "Ask first" approvals. Its approval requests surface in our Approvals queue (lane: Duet) | Proactive nudges card in Inbox; Duet lane in Approvals |
| Copilot Intelligence | **UNCONFIRMED as a separate company/product.** No public product, API or license found under that name; the OpenDuet published under copilotkit.ai suggests it refers to **CopilotKit** (MIT). Treat as CopilotKit until Kingdom gives a link | n/a | Copilot layer = CopilotKit/AG-UI |
| CopilotSora | **UNCONFIRMED** — no public repo/product found under that name (GitHub, CopilotKit org) | Placeholder only | TBD by Kingdom |

## 3. License findings
- MIT (Paperclip, CopilotKit, AG-UI, OpenDots, OpenBot, OpenMuse, CopilotKit/OpenDuet); Apache-2.0 alt OpenDuets (ychampion, Anil-matcha): reuse freely; keep copyright notices.
- Apache-2.0 (AionUi): reuse OK; keep LICENSE/NOTICE, mark changes.
- AGPL-3.0 (Plane): copying code into our app would make the app AGPL and, when served over a network, oblige source release. Rule: API calls only, clean-room UI.
- Open WebUI License: BSD-3 style but forbids removing "Open WebUI" branding above 50 users/30 days without permission. Rule: talk to OWUI by API, never embed or rebrand its UI.
- Multica License: Apache-2.0 plus Part I conditions; read Part I before reusing any code. API use is fine.
- Linear, Obsidian: proprietary; no decompiling, no assets. Official Linear GraphQL; Obsidian via vault files / Local REST API plugin.
- Brand: our own palette, Inter (OFL), B v3 mark. No third-party logos inside the UI except as plain-text integration names.

## 4. Information architecture
Tabs (phone bottom bar; tablet/fold/Mac sidebar):
1. **Inbox** (home): approvals waiting, mentions, OpenDuet proactive nudges, failed runs. Linear-style triage.
2. **Agents**: board by status (running / idle / blocked / paused), heartbeat, current issue, cost today, Wake.
3. **Issues**: list + board, filters, cycles (Plane-inspired), issue detail with comments, documents, linked approvals.
4. **Approvals**: queue with tap-to-approve, two-step for gated items; lanes: Paperclip, Multica, JEV gate.
5. **Chat**: multi-agent thread (AionUi), model switcher + RAG citations (OWUI), CopilotKit generative UI cards.
6. **Knowledge**: Obsidian vault notes, backlinks, Paperclip issue documents.
7. **Spend**: summary, by agent / project / provider, quota windows, budget alerts.
Global: Cmd-K palette, copilot sheet (CopilotKit) reachable from every screen, Routines in More.

## 5. Data sources and auth
- **Paperclip** `https://c0achs-macbook-pro.tailb9ca6c.ts.net:8443/api` — pending Kingdom's "Confirm Paperclip tailnet serve on 8443" in Operator Node. Tailnet only, no Funnel. Auth: Better Auth session (authenticated mode) via in-app login; token in Keychain/Keystore. Board-claim / CLI-auth challenge flow considered for device pairing (QR on Mac → phone), to confirm with Paperclip docs.
- Key endpoints (verified in repo source): `GET /companies/:companyId/dashboard`, `/agents`, `/issues`, `/issues/summaries`, `/approvals`, `/activity`, `/routines`, `/costs/summary|by-agent|by-project|by-provider|quota-windows|window-spend`; `GET /agents/:id`, `/agents/:id/runtime-state`; `POST /agents/:id/wakeup`; `GET /approvals/:id`, `/approvals/:id/issues`, `POST /approvals/:id/approve|reject|comments|resubmit`; `POST /routines/:id/run`; `GET /agents/me/inbox/mine`.
- **Multica** via OpenBot (openbot-tmw): status reads; approve = Multica status change by Kingdom or chief-of-staff agents only.
- **OWUI**: OpenAI-compatible `/api/chat/completions`, `/api/models`, knowledge endpoints; API key per user, tailnet only.
- **Linear**: GraphQL `https://api.linear.app/graphql`, OAuth app, read + issue comments.
- **Plane**: REST `/api/v1/workspaces/:slug/projects/:id/issues|cycles`, X-API-Key (current saved key is being rejected; QU3BII getting a valid one).
- **Obsidian**: Local REST API plugin over tailnet (HTTPS + API key) or read-only vault sync folder.
- **CopilotKit runtime**: hosted by openbot-tmw; AG-UI event stream over HTTPS/SSE.

## 6. Approval gates (non-negotiable)
- Every approve/reject calls Paperclip `/approvals/:id/approve|reject` or the Multica status change. The app never performs the gated action itself.
- JEV-gated items (deploys via Hostinger/Cloudflare/Vercel, publishing, spend, network changes) show a **JEV** badge, require hold-to-confirm + Face ID/biometric, and only record Kingdom's decision; the owning agent executes.
- Items whose rule says "confirm in agent X's own chat" (Operator Node network changes, K0Ad33 BAMA) show a deep link instead of an Approve button.
- Copilot (CopilotKit) actions that write anything use human-in-the-loop `renderAndWaitForResponse`; no auto-approve, no auto-spend.
- Watch: approve only for non-JEV items; JEV items show "Open on iPhone".

## 7. Sync / integration plan
| System | Direction | Mechanism | Notes |
|---|---|---|---|
| Paperclip | read/write (comments, approvals, wake, run routine) | REST + poll 15s foreground / push via relay later | Source of truth |
| Linear | read; optional comment mirror | GraphQL + webhook into n8n | Map by issue link (e.g. M3T-52 ↔ THM-217) |
| Plane | read | REST poll 5 min | Blocked on valid API key |
| OWUI | chat + RAG | REST/stream | Models list drives model switcher |
| Obsidian | read; append to daily note (opt-in) | Local REST API | Backlinks computed from `[[links]]` |
| Multica | read + status change (Kingdom) | via OpenBot | Approval lane |
| CopilotKit/AG-UI | bidirectional events | SSE | Shared state = current screen context |
Offline: cached last snapshot (MMKV/SQLite), queued comments; approvals never queue offline.

## 8. Stack
- **Expo (React Native, TypeScript)** for iOS, iPadOS, Android. Expo Router; layouts by width class: compact (<600dp phone / Fold cover), medium (600–840dp Fold inner, iPad portrait → 2-pane), expanded (>840dp iPad landscape → 3-pane). Fold posture via `react-native-window-size`/Jetpack WindowManager module.
- **CopilotKit**: React Native support per CopilotKit docs (verify current RN package at build time); AG-UI client for streams.
- **Mac**: Tauri 2 shell around the web build (consistent with K0Ad33's M3ta-OS Mac app) — recommended; Catalyst as fallback.
- **watchOS**: SwiftUI target via Expo config plugin (`@bacons/apple-targets`), WatchConnectivity to iPhone; complications for pending-approval count.
- Notifications: APNs/FCM via relay (open question: who hosts the relay on the tailnet).
- Design tokens exported from `tokens.v1.3.json` → RN theme.

## 9. Screens (mockups in `mockups/`, contact sheet `mockups/contact-sheet.png`)
01 Phone Inbox · 02 Agents board · 03 Issue detail · 04 Approvals tap-to-approve · 05 Spend · 06 AI Chat (AionUi/OWUI + CopilotKit) · 07 Knowledge (Obsidian) · 08 iPad split view · 09 Fold inner split · 10 Mac window · 11 Watch approval glance.

## 10. Open questions for Kingdom
1. CopilotSora: what is it (link or repo)? Marked UNCONFIRMED.
2. OpenDuet: confirm CopilotKit/OpenDuet vs ychampion/openduet vs Anil-matcha/open-duet, and send a link for "Copilot Intelligence" (no product/API/license found; assumed = CopilotKit).
3. Device pairing: login on each device, or QR pairing from the Mac?
4. Push notifications relay host (n8n on tailnet vs OpenBot)?
5. Obsidian: Local REST API plugin on the Mac, or read-only vault sync?
6. Chat default: OWUI models, Paperclip agents, or OpenBot/CopilotKit coworkers first?
7. Mac shell: Tauri (match M3ta-OS app) confirmed?
8. Should Linear↔Paperclip issues be mirrored or only linked?
9. Plane API key (pending with QU3BII).

## 11. Companion doc
`BAMA-WIRING.md` duplicates the integration wiring as an implementation spec for K0Ad33's M3ta-0S BAMA build.
