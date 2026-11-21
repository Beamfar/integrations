# Beamfar — AI Assistant Add-ons (Claude Code, GitHub Copilot / VS Code, Google Antigravity, Gemini CLI, OpenAI Codex & MCP)

Connect your AI assistant to [Beamfar](https://beamfar.com/mcp-server)'s hosted, read-only MCP server (`https://api.beamfar.com/mcp`) to check monitored sites, inspect two-region incident evidence, correlate pre-incident DNS/TLS/deploy timelines, review external security findings, and summarise monthly client reports.

- **Read-only by design:** All 12 MCP tools only read stored monitoring observations — nothing can modify sites, monitors, alerts, billing, or status pages.
- **OAuth 2.1 or API key:** Sign in directly via OAuth 2.1 (PKCE S256) in Claude.ai, Claude Code, GitHub Copilot / VS Code, Google Antigravity, ChatGPT, Codex, Cursor, and Gemini CLI, or pass a read-only `BEAMFAR_API_KEY` (`bfk_...`).
- **Included on every plan (Free included):** Works on Free (1 site checked every 5 minutes, 3,000 API/MCP requests per month, 10 requests per minute, no card, no time limit), Solo, Team, Agency, and Agency Plus.
- **EU-hosted Streamable HTTP:** Runs in Falkenstein, Germany (Hetzner) with zero local database or runtime required.

---

## Quick Install

### 1. GitHub Copilot & VS Code (MCP Server)

Add Beamfar in `.vscode/mcp.json` (or install directly from [beamfar.com/mcp-server](https://beamfar.com/mcp-server) / GitHub MCP Registry `com.beamfar/website-monitoring`):

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

### 2. Google Antigravity (Plugin + MCP + Incident Triage Skill)

Includes `.antigravity-plugin/plugin.json`, `mcp_config.json` (`https://api.beamfar.com/mcp`), bundled logo (`assets/logo.png`), and `skills/incident-triage/SKILL.md`.

### 3. Claude Code (Official Plugin)

```bash
claude plugin marketplace add beamfar/integrations
claude plugin install beamfar@beamfar
```

Run `/beamfar:check-sites` or connect the endpoint directly:

```bash
claude mcp add --transport http beamfar https://api.beamfar.com/mcp
```

### 4. Google Gemini CLI (Official Extension)

```bash
gemini extensions install https://github.com/Beamfar/integrations
```

Installs the MCP server, `GEMINI.md` triage rules, `incident-triage` skill, and `/beamfar:check-sites`.

### 5. OpenAI Codex CLI & App (Plugin, `config.toml` & Skill)

```bash
codex mcp add beamfar --url https://api.beamfar.com/mcp
codex mcp login beamfar
```

Or copy [`codex/config.toml`](codex/config.toml) into `~/.codex/config.toml` and [`skills/incident-triage/SKILL.md`](skills/incident-triage/SKILL.md) into `~/.codex/skills/beamfar-incident-triage/SKILL.md`.

### 6. Claude.ai, ChatGPT & Cursor

- **Claude.ai & Claude Desktop:** Settings -> Connectors -> Add custom connector -> `https://api.beamfar.com/mcp` (OAuth 2.1).
- **ChatGPT:** Settings -> Connectors -> Create custom connector -> `https://api.beamfar.com/mcp` (OAuth 2.1).
- **Cursor:** Visit [beamfar.com/mcp-server](https://beamfar.com/mcp-server) or [Cursor Directory](https://cursor.directory/plugins/beamfar).
- **Official MCP Registry:** `com.beamfar/website-monitoring`.

---

## Repository Layout

| Assistant | Manifest / Entry Point | Included Components |
| --- | --- | --- |
| **Google Antigravity** | `.antigravity-plugin/plugin.json`, `mcp_config.json`, `assets/logo.png` | Hosted MCP server, `incident-triage` skill, starter prompts |
| **GitHub Copilot / VS Code** | `.vscode/mcp.json` (or `com.beamfar/website-monitoring` via GitHub MCP Registry) | Hosted Streamable HTTP OAuth 2.1 MCP server |
| **Claude Code** | `.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`, `.mcp.json` | Hosted MCP server, `/beamfar:check-sites` command, `incident-triage` skill |
| **Gemini CLI** | `gemini-extension.json`, `GEMINI.md` | Hosted MCP server, context rules, `/beamfar:check-sites` command, `incident-triage` skill |
| **OpenAI Codex** | `plugin.json`, `mcp.json`, `AGENTS.md`, `codex/config.toml` | Hosted MCP server, Codex plugin descriptor, `config.toml`, `incident-triage` skill |

## The 12 Read-Only MCP Tools

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

## Safety & Privacy

- **Untrusted content fencing:** Text read from monitored websites or DNS records arrives inside `<<<UNTRUSTED ...>>>` blocks and is treated strictly as data, never as instructions.
- **No fabricated measurements:** Missing checks always return `"data unavailable"`.
- **Instant revocation:** Revoke any OAuth grant or API key anytime in Beamfar under **Settings -> Connected assistants** or **Integrations & API**.

## Trademarks & License

Claude and Claude Code are trademarks of Anthropic, PBC. Gemini and Antigravity are trademarks of Google LLC. GitHub Copilot and Visual Studio Code are trademarks of Microsoft Corporation. Codex and ChatGPT are trademarks of OpenAI. Beamfar is an independent service and is not affiliated with or endorsed by any assistant vendor.

MIT License — see [LICENSE](LICENSE).
