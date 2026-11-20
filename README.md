# Beamfar — Claude Code Plugin

Connect Claude Code to [Beamfar](https://beamfar.com/mcp-server)'s hosted, read-only MCP server (`https://api.beamfar.com/mcp`) to check monitored sites, inspect two-region incident evidence, review pre-incident DNS/TLS/deploy timelines, check external security findings, and summarise monthly client reports.

- **Read-only by design:** All 12 MCP tools only read stored monitoring observations — nothing can modify sites, monitors, alerts, billing, or status pages.
- **Included on every plan (Free included):** Works on Free (3,000 API/MCP requests per month, 10 requests per minute), Solo, Team, Agency, and Agency Plus. Connecting (`initialize` and `tools/list`) is free and spends zero quota units.
- **EU-hosted Streamable HTTP:** Connects directly to `https://api.beamfar.com/mcp` with no local Python runtime, Node.js bridge, or database access required.

## 2-minute setup

### 1. Create a read-only API key in Beamfar

1. Sign in to [Beamfar](https://app.beamfar.com/app) as an organisation owner or admin.
2. Open **Integrations & API -> AI assistants** (or **API keys**), create a **Read-only** key (`scope: read`), and copy the `bfk_...` token (shown once).
3. Export it in your shell environment (never commit a key to git):

```bash
export BEAMFAR_API_KEY="bfk_your_read_only_key_here"
```

### 2. Install the plugin in Claude Code

From a public GitHub repository or local checkout:

```bash
claude plugin marketplace add beamfar/integrations
claude plugin install beamfar@beamfar
```

Or connect the MCP endpoint directly with a single command:

```bash
claude mcp add --transport http beamfar https://api.beamfar.com/mcp \
  --header "Authorization: Bearer ${BEAMFAR_API_KEY}"
```

### 3. Try it

Run the slash command in Claude Code:

```text
/beamfar:check-sites
```

Or ask in plain English:

- *"Which of our sites are down or having regional issues right now?"*
- *"Show the two-region evidence and what changed before the latest incident on shop.example.com."*
- *"Are any TLS certificates or domains expiring soon across our portfolio?"*
- *"Which sites have warnings or incomplete coverage in the latest security scan?"*

## What is included

| Component | Path | Purpose |
| --- | --- | --- |
| MCP server config | `.mcp.json` | Connects to `https://api.beamfar.com/mcp` (`http` / Streamable HTTP) with `Authorization: Bearer ${BEAMFAR_API_KEY}`. |
| Triage skill | `skills/incident-triage/SKILL.md` | Teaches Claude to verify maintenance windows, read two-region confirming evidence, correlate pre-incident timelines, and present `explain_incident` results as ranked evidence rather than diagnoses. |
| Slash command | `commands/check-sites.md` | `/beamfar:check-sites` for a fast portfolio health and open-incident briefing. |
| Plugin manifests | `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Metadata for Claude Code plugin installation and marketplace discovery. |

## The 12 read-only MCP tools

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

## Safety & privacy

- **Untrusted content fencing:** Text read from monitored websites or DNS records arrives inside `<<<UNTRUSTED ...>>>` blocks and is treated strictly as data, never as instructions. Hidden prompt-injection text targeting AI assistants is redacted by Beamfar before it reaches the MCP response.
- **Key hygiene:** Keep `BEAMFAR_API_KEY` in your environment or OS secret store. Owners and admins can rotate or revoke keys anytime in Beamfar under **Integrations & API**.

## Trademarks & license

Claude and Claude Code are trademarks of Anthropic, PBC. Beamfar is an independent service and is not affiliated with or endorsed by Anthropic.

MIT License — see [LICENSE](LICENSE).
