# SOP — Setting Up Microsoft's `powerbi-authoring` Plugin (semantic-model-authoring skill + Power BI Modeling MCP)

Human-facing setup guide for connecting Claude Code to Microsoft's real, live `semantic-model-authoring` skill and the `powerbi-modeling-mcp` server — distinct from `rdlc-tmdl-build-agent`, which is our own file-based authoring skill. This setup enables **Verify Mode**: live, best-practice semantic review of an already-built model, using Microsoft's actual current tooling rather than our own frozen/distilled reference material.

Do this once per machine (plugin installs at user/local scope). Not required for normal `rdlc-tmdl-build-agent` Authoring Mode builds — only needed when you want live MCP-based verification.

---

## Prerequisites

Confirm in a **real, standalone terminal** (Windows Terminal or PowerShell launched normally from the Start Menu — **not** typed into the Claude Code chat panel, which runs commands in its own sandboxed tool environment with a different PATH):

```powershell
node --version
npm --version
npx --version
```

All three must return a version number. If any fail, install Node.js first — the MCP server is fetched and run via `npx`, and none of the following steps work without it.

**Optional sanity check** — this runs the exact command the plugin will later run automatically:
```powershell
npx -y @microsoft/powerbi-modeling-mcp@latest --start
```
If it launches without error (even if it just sits waiting, since it's a server process), `Ctrl+C` to stop it and proceed. If it fails here, fix that before continuing — every later step depends on this working.

---

## Step 1 — Install the plugin

In the Claude Code chat panel (VS Code extension or terminal), open the plugin manager:

```
/plugin
```

(If `/plugin` doesn't respond, try `/plugins` — exact command name has varied slightly across versions. Both should open a graphical "Manage Plugins" dialog.)

1. Go to the **Marketplaces** tab
2. In the "GitHub repo, URL, or path..." field, enter:
   ```
   microsoft/skills-for-fabric
   ```
3. Click **Add**
4. Switch to the **Plugins** tab
5. Find and install **`powerbi-authoring`**

This single install registers **all 5 bundled skills** (`semantic-model-authoring`, `powerbi-report-planning`, `powerbi-report-design`, `powerbi-report-authoring`, `powerbi-report-management`) *and* the `powerbi-modeling-mcp` MCP server declaration together — they're bundled in one plugin manifest, not separate installs.

---

## Step 2 — Full restart (not optional)

**MCP servers are only registered and spawned at session startup.** Installing the plugin mid-session does not retroactively connect anything — this was a real point of confusion during initial setup, worth taking seriously.

Do a full reload, not just closing the chat panel:
- Command Palette (`Ctrl+Shift+P`) → **Developer: Reload Window**, or
- Fully close and reopen VS Code

---

## Step 3 — Verify the full setup, end to end

Do not assume it worked from a green status light alone — "spawned" and "genuinely connected and usable" are different states, confirmed by real experience during setup (a server can start successfully, log all its tool registrations, and still fail to connect due to a stalled authentication handshake).

Use this verification prompt:

```
Verify my Power BI / Fabric MCP setup end to end before we do
anything else. Specifically:

1. Confirm the powerbi-authoring plugin is installed and enabled.
2. Confirm all 5 bundled skills are visible to you, especially
   semantic-model-authoring.
3. Confirm the powerbi-modeling-mcp MCP server shows as CONNECTED,
   not just "spawned" — check for mcp__*powerbi-modeling-mcp*__
   tools actually being present and callable, not just that the
   process started.
4. If it's not connected, tell me specifically what's blocking it —
   e.g., stuck on interactive browser authentication, timed out, or
   something else — rather than just "not available."
5. If it IS connected, run one simple read-only tool call (e.g.,
   list tables) against an actual local semantic model to prove the
   connection works, not just that it reports "connected."

Report all five findings clearly, one by one. Don't attempt any
write/edit action yet — this is verification only.
```

**If step 3/4 reports a stalled or timed-out connection**: check the MCP servers panel (`/mcp` or the MCP status icon) for the specific server, click **Reconnect**, and watch closely for a browser window/tab opening for interactive Microsoft/Fabric sign-in — the server's `InteractiveBrowser` auth mode requires this, and Claude Code's own connection attempt can time out waiting for it if you're not watching for the popup.

---

## Step 4 — Confirm exact invocable skill names (namespacing)

Skills bundled inside a plugin are **namespaced** — `<plugin-name>:<skill-name>`, not the bare skill name. Confirm the exact form before relying on it:

```
List every skill currently available to you in this session, exactly
as they'd appear if I asked you to use one by name. I want to confirm
semantic-model-authoring and the other powerbi-authoring skills are
genuinely loaded and invokable — not just declared in the plugin
manifest. Show me the exact namespaced name I'd need to reference
(e.g., powerbi-authoring:semantic-model-authoring).
```

Expected result: `powerbi-authoring:semantic-model-authoring`, `powerbi-authoring:powerbi-report-planning`, etc.

---

## Step 5 — Prove it with a real invocation

A listing that says a skill is "available" is still one step short of certainty. Actually invoke it:

```
Invoke powerbi-authoring:semantic-model-authoring directly. Confirm
it loads successfully, then use it to run a best-practice review
against [your live semantic model path] via the powerbi-modeling-mcp
connection.

This is read-only analysis — do not make any changes to the model.
```

A successful load and real tool response (not an error) is the only fully reliable confirmation the whole chain — plugin → skill → MCP → live model — actually works.

---

## Safety rules while using this setup

- **Never edit TMDL files by hand (or via `rdlc-tmdl-build-agent`) while an MCP session is live against that same model.** Mixing hand-authored file edits with a simultaneously-open MCP connection risks desync/corruption. Disconnect first (or confirm the agent disconnects after use), then resume file-based work.
- **Bounded retries**: if an MCP tool call fails, allow one corrected retry. If it fails again, stop and report — do not let an agent loop indefinitely against a live MCP connection; this both burns tokens and can leave a model in a partially-modified state.
- **This setup is for verification, not primary authoring** — `rdlc-tmdl-build-agent`'s Authoring Mode (file-based, no MCP) remains the default for building new models. This MCP connection is specifically for the Verify Mode use case: checking an already-built model against Microsoft's live, current best-practice logic.

---

## Troubleshooting log — real issues hit during initial setup

| Symptom | Cause | Fix |
|---|---|---|
| `claude --version` "not found" when typed in chat | Chat panel runs commands in its own sandboxed tool environment, different PATH than your real shell | Check versions in a real, separate terminal window instead |
| `/plugin marketplace add ...` answered as plain chat text, not a command | Slash command not intercepted — likely a stale session state, not necessarily an old version | Try `/plugin` alone (no args) first to open the graphical manager; use the UI fields instead of typing the full command |
| MCP server shows "Failed" / "Request timed out" despite terminal showing successful tool registration | `InteractiveBrowser` auth handshake stalls waiting for browser sign-in that was missed or timed out | Click Reconnect, watch immediately for a browser sign-in window, complete it promptly |
| Skill/MCP install seems to do nothing | MCP servers only register at session startup, not retroactively mid-session | Full VS Code window reload (or full restart) after any plugin install |
