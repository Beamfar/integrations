# Beamfar — AI Assistant Add-ons & Read-Only MCP Server

[![Beamfar MCP connector](https://glama.ai/mcp/connectors/com.beamfar/website-monitoring/badges/score.svg)](https://glama.ai/mcp/connectors/com.beamfar/website-monitoring)
[![Official MCP Registry](https://img.shields.io/badge/MCP_Registry-com.beamfar%2Fwebsite--monitoring-0ea5e9)](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.beamfar/website-monitoring)
[![OAuth 2.1](https://img.shields.io/badge/Auth-OAuth_2.1_%2B_PKCE-6366f1)](https://beamfar.com/mcp-server)
[![License: MIT](https://img.shields.io/badge/License-MIT-slate)](LICENSE)

**Stop finding out from clients (or 3 AM single-region false alarms).**

Connect **Claude, Claude Code, GitHub Copilot / VS Code, Google Antigravity, Gemini CLI, OpenAI Codex, ChatGPT, or Cursor** to [Beamfar](https://beamfar.com/mcp-server)'s hosted, strictly read-only MCP server (`https://api.beamfar.com/mcp`) to verify deploys, triage confirmed two-region outages, catch accidental `noindex` / `robots.txt` regressions, track expiring SSL certificates and domains, and generate 30-second client risk briefs — straight from your editor or terminal.

- **Two-region EU confirmation (Germany + Finland):** Outages open only when both regions agree; a single-region blip is recorded as a regional anomaly with no outage spam.
- **Strictly read-only by design:** All 16 MCP tools only read stored monitoring evidence — no assistant can delete monitors, pause alerts, or alter billing.
- **Zero-secret OAuth 2.1 (PKCE S256):** Connect in two clicks without copying static API keys (scoped read-only `BEAMFAR_API_KEY` also supported).
- **Listed on Glama & Official MCP Registry:** [Glama MCP Directory](https://glama.ai/mcp/connectors/com.beamfar/website-monitoring) · Official MCP Registry `com.beamfar/website-monitoring`.
- **Free plan included (€0 forever, no card):** 1 site checked every 5 minutes from both EU regions, 3,000 API/MCP requests per month, 10 requests per minute, email + push alerts, and 1 public status page.
- **Test any public site with zero login:** Run a free live check right now at [beamfar.com/scan](https://beamfar.com/scan?utm_source=github&utm_medium=readme&utm_campaign=integrations_repo).

---

## Copy-Paste Prompts Once Connected

Ask your AI assistant any of these straight after connecting:

| Workflow | Prompt to try |
| --- | --- |
| **30-second morning agency brief** | *"Run my morning brief: which monitored sites are down, have expiring SSL certs or domains, SEO blocks, or open incidents?"* |
| **Post-deploy verification** | *"Run a post-deploy check on `example.com`: verify two-region health, recent DNS/TLS/SEO/deploy changes, and external security."* |
| **Incident root-cause triage** | *"Triage our latest open incident — show confirming two-region evidence, likely cause with confidence, and what changed right beforehand."* |
| **Accidental SEO regression check** | *"Did any deploy or CMS update in the last 7 days flip `noindex`, block `robots.txt`, or shift canonicals on my monitored sites?"* |
| **Monthly client summary** | *"Summarise the latest monthly report for my client sites: measured uptime, confirmed incidents, and expiring certificates."* |

---

## Quick Install

### 1. Claude Code (Official Plugin — 4 Skills + 3 Slash Commands)

```bash
claude plugin marketplace add Beamfar/integrations
claude plugin install beamfar@beamfar
```

Includes `/beamfar:check-sites`, `/beamfar:morning-brief`, `/beamfar:deploy-check`, and all 4 skills. Or connect the MCP server directly:

```bash
claude mcp add --transport http beamfar https://api.beamfar.com/mcp
```

### 2. GitHub Copilot & VS Code (MCP Server)

Add Beamfar in `.vscode/mcp.json` (or install from [beamfar.com/mcp-server](https://beamfar.com/mcp-server) / GitHub MCP Registry `com.beamfar/website-monitoring`):

```json
{
  "servers": {
    "beamfar": {
      "type": "http",
      "url": "https://api.beamfar.com/mcp"
    }
  }
}
```

### 3. Google Antigravity (Plugin + MCP + 4 Skills)

Includes `.antigravity-plugin/plugin.json`, `mcp_config.json` (`https://api.beamfar.com/mcp`), bundled logo (`assets/logo.png`), starter prompts, and all 4 universal skills under `skills/`.

### 4. Google Gemini CLI (Official Extension)

```bash
gemini extensions install https://github.com/Beamfar/integrations
```

Installs the MCP server, `GEMINI.md` triage rules, all 4 skills, and `/beamfar:check-sites`, `/beamfar:morning-brief`, and `/beamfar:deploy-check`.

### 5. OpenAI Codex CLI & App (Plugin, `config.toml` & 4 Skills)

```bash
codex mcp add beamfar --url https://api.beamfar.com/mcp
codex mcp login beamfar
```

Or copy [`codex/config.toml`](codex/config.toml) into `~/.codex/config.toml` and copy any skill folder under [`skills/`](skills/) into `~/.codex/skills/`.

### 6. Claude.ai, ChatGPT & Cursor

- **Claude.ai & Claude Desktop:** Settings -> Connectors -> Add custom connector -> `https://api.beamfar.com/mcp` (OAuth 2.1).
- **ChatGPT:** Settings -> Connectors -> Create custom connector -> `https://api.beamfar.com/mcp` (OAuth 2.1).
- **Cursor:** Install via [beamfar.com/mcp-server](https://beamfar.com/mcp-server) or [Cursor Directory](https://cursor.directory/plugins/beamfar).
- **Official MCP Registry:** `com.beamfar/website-monitoring`.

---

## Included Open-Source Skills & Slash Commands (`v1.3.0`)

| Component | Path | What it automates |
| --- | --- | --- |
| **`incident-triage`** skill | `skills/incident-triage/SKILL.md` | Two-region outage confirmation, ranked likely cause, pre-incident DNS/TLS/deploy timeline, and client recap. |
| **`pre-deploy-check`** skill | `skills/pre-deploy-check/SKILL.md` | Pre/post-release go/hold verification across regional health, active maintenance, recent diffs, and security posture. |
| **`agency-morning-brief`** skill | `skills/agency-morning-brief/SKILL.md` | 30-second client portfolio risk briefing (open incidents, expiring SSL/domains, SEO blocks, DNS changes, reports). |
| **`seo-regression-guard`** skill | `skills/seo-regression-guard/SKILL.md` | Detects accidental `noindex`, blocked `robots.txt`, shifted canonicals, and redirect regressions after deploys or CMS updates. |
| **`/beamfar:check-sites`** | `commands/check-sites.md` · `commands/beamfar/check-sites.toml` | Instant status check across all monitored sites and open incidents. |
| **`/beamfar:morning-brief`** | `commands/morning-brief.md` · `commands/beamfar/morning-brief.toml` | Agency portfolio risk digest organized into Action needed today / Watchlist / Totals. |
| **`/beamfar:deploy-check`** | `commands/deploy-check.md` · `commands/beamfar/deploy-check.toml` | Fast Go / Hold check right before or after shipping to production. |

---

## The 16 Read-Only MCP Tools

| Tool | What it reads |
| --- | --- |
| `list_sites` | Organisation status counts and every site's status, open incidents, and last check time (`site_id` source). |
| `site_status` | One site's overall state, per-monitor uptime and valid-check coverage over 1–2160 hours, and latest regional results. |
| `list_incidents` | Incidents newest first, optionally filtered by `site_id` and `state` (`open` or `resolved`). |
| `incident_evidence` | Stored confirming and resolving probe observations (region, HTTP/TLS/DNS facts, error class, timings). |
| `explain_incident` | Ranked likely cause category, confidence, supporting evidence, ruled-out causes, and what could not be checked. |
| `what_changed` | Pre-incident and weekly site timeline: DNS, TLS certificate, redirect, SEO signals, domain expiry, and deploy markers. |
| `first_day_findings` | Portfolio-wide ranked findings (expiring certificates/domains, redirects, SEO blocks, DNS issues, sites down). |
| `monthly_report_summary` | Stored monthly client report summary (uptime, coverage, confirmed incidents, changes, expiring certificates). |
| `security_overview` | Portfolio security and SEO integrity scan status, coverage, warnings, notices, and known software flaws (`vulnerable`). |
| `site_security` | Latest scan details for one site: coverage, gaps, software components, and external findings with evidence. |
| `list_maintenance` | Planned maintenance schedules (`upcoming`, `active`, `ended`) and active/next windows. |
| `dns_changes` | Confirmed changes in watched NS, MX, SPF, DMARC, CAA, and TXT records. |
| `exploit_watch` | Ranked Exploit Watch open risk chains, cross-site CISA KEV/OSV advisory threads, and stack-specific fix cards. |
| `site_security_layers` | Connected read-only security layers (seen of applicable), auto-detected stack, and visibility gaps for one site. |
| `am_i_under_attack` | Two-row compromise vs attack-attempt check, ordered response steps, and explicit blind spots for one site. |
| `security_audit_surface` | Disprove-verified live attack-surface and coverage ledger (confirmed vs needs-verification findings, exposed APIs/secrets, blind spots) for one site. |

---

## Companion Open-Source Tools

- **GitHub Action (Deploy Markers):** [`Beamfar/github-action`](https://github.com/Beamfar/github-action) ([GitHub Marketplace](https://github.com/marketplace/actions/beamfar-deploy-marker)) — pin release markers onto your Beamfar incident timeline.
- **Official WordPress.org Plugin:** [`beamfar-monitoring`](https://wordpress.org/plugins/beamfar-monitoring/) — wp-admin dashboard status widget, WP-Cron heartbeat, and plugin/theme/core update markers.
- **Interactive MCP Hub:** [beamfar.com/mcp-server](https://beamfar.com/mcp-server)

## Safety & Privacy

- **Untrusted content fencing:** Text read from monitored websites or DNS records arrives inside `<<<UNTRUSTED ...>>>` blocks and is treated strictly as data, never as instructions.
- **No fabricated measurements:** Missing checks always return `"data unavailable"`.
- **Instant revocation:** Revoke any OAuth grant or API key anytime in Beamfar under **Settings -> Connected assistants** or **Integrations & API**.

## Trademarks & License

Claude and Claude Code are trademarks of Anthropic, PBC. Gemini and Antigravity are trademarks of Google LLC. GitHub Copilot and Visual Studio Code are trademarks of Microsoft Corporation. Codex and ChatGPT are trademarks of OpenAI. Beamfar is an independent service and is not affiliated with or endorsed by any assistant vendor.

MIT License — see [LICENSE](LICENSE).
