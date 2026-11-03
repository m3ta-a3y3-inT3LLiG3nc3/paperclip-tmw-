# Paperclip diagnostic recipes

## Discovery and access

Discover the live process and installed package path rather than assuming a version. Use lsof on the selected process to identify its listener and stdout/stderr destinations. Check the relevant tunnel rule to confirm the public hostname reaches that instance.

Useful probes:

    GET /api/health
    GET /api/heartbeat-runs/<run-id>

A 200 application shell verifies web delivery, not run existence. An unauthenticated run endpoint can return 404 even when the run exists. Do not bypass access controls; use authorized local evidence within the user's troubleshooting scope.

## Signed-in desktop inspection

When asked to use the computer/browser, inspect the live signed-in application instead of assuming a mainstream browser is running. Load the installed computer-use guidance, discover the installed driver and current schemas, then target only the relevant browser application:

    cua-driver describe list_windows
    cua-driver call list_windows '{"app":"<observed browser app>","session":"platform-review"}'
    cua-driver describe get_window_state
    cua-driver call get_window_state '{"pid":<returned pid>,"window_id":<returned window_id>,"session":"platform-review","query":"Runtime","include_screenshot":false}'

This recipe has been exercised for native accessibility inspection of a signed-in Paperclip run page; it does not establish that a repair control was activated or a generation succeeded. Select the exact returned window by its target title, and use a semantic query plus a bounded tree before widening the capture. Repeat the same non-default session label throughout the workflow.

If page-aware binding does not recognize the browser, try the native accessibility read on that same window before concluding GUI access is blocked. Keep the fallback scope narrow; do not scan unrelated page contents or copy login cookies.

If a wrapper's health check and direct driver calls disagree, inspect the resolved executable and existing daemon owner before changing anything. Test a harmless advertised read through the installed CLI; wrapper failure alone does not establish that the existing driver cannot serve native reads. Preserve the shared daemon and permission settings; do not restart or upgrade them merely to make a diagnostic pass. Treat each successful read as evidence only for the route actually exercised.

Inspect only schemas needed for the next action and filter large discovery outputs before returning them. Repeated full help dumps and unbounded application/log listings consume the interaction budget without advancing the repair. When acting is permitted, use fresh returned element tokens and checkpoint readbacks according to the computer-use guidance; never infer a successful mutation from a successful snapshot.

## Read-only database inspection

A working local approach uses the postgres JavaScript package bundled with Paperclip. Resolve its installed path first. Load the instance connection string internally without printing it:

    const sql = postgres(connectionString, {
      max: 1,
      connection: { default_transaction_read_only: 'on' }
    });
    try {
      // Parameterized SELECT queries only.
    } finally {
      await sql.end();
    }

Discover columns with information_schema.columns before version-dependent queries. Useful observed tables:

- agents: id, company_id, name, adapter_type, adapter_config, status.
- heartbeat_runs: id, company_id, agent_id, created_at, status, error, error_code, result_json, context_snapshot.
- summary_slots: company_id, scope_kind, scope_id, slot_key, status, failure_reason, generating_issue_id, document_id.

Filter by company_id and bound recent runs with ORDER BY created_at DESC LIMIT. Project adapter_config fields individually. Inspect key names for structural discovery, not values. A null direct aiConnectionId field does not prove managed authentication is absent; binding may be resolved elsewhere.

## Failure branches

Managed-auth incompatibility:
- Check configurationIncomplete.reason and executionRecovery.providerWorkStarted.
- Read installed assertManagedAiProjectAuth, observed in services/ai-connection-runtime.js.
- The observed implementation checks command-line auth/config overrides and scans provider configuration from effective cwd through ancestor directories to root.
- Anthropic files include .claude/settings.json and .claude/settings.local.json; OpenAI files include .codex/config.toml. Inspect the current validator for exact match rules.
- Home-level configuration can block managed execution in nested workspaces even without an explicit agent cwd or auth override.
- Report matching key names only. A null cwd or missing snapshot field does not establish the effective directory.
- A supported isolated execution/configuration arrangement is a candidate repair, not a verified fix until exercised. Do not present untested isolation settings as a proven recipe.

Workspace validation:
- isolated_workspace using git_worktree requires a valid Git base workspace.
- Verify the effective project/execution workspace with git before changing strategy. Do not initialize an arbitrary directory merely to silence validation.

Responsible-user membership:
- Verify the resolved responsible user is an active member of the target company.
- Treat this independently from provider authentication; repairing one will not repair the other.

Stuck summaries:
- Compare failed producers with summary_slots status, failure_reason, generating_issue_id, and document_id.
- A generating slot with a failed producer establishes stale derived state, not continued execution or success.
- Preserve evidence and investigate supported reconciliation before resetting state. Never mark a slot complete without its verified document.
