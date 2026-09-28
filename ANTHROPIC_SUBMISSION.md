# Anthropic Directory submission pack

This document contains public, non-secret copy prepared for Anthropic's
developer portal. Never add reviewer credentials or the OAuth client secret to
this repository.

## Submission 1 — MCP connector

- Submission type: `MCP connector`
- Connection: universal URL
- Server URL: `https://mcp.kiliba.ai/mcp`
- Authentication: OAuth 2.0 with Anthropic-held client credentials
- OAuth Client ID: provide the dedicated directory Client ID directly to Anthropic
- OAuth Client Secret: transfer only through the secure channel Anthropic provides in reply

### Listing

- Name: `Kiliba`
- Permanent slug proposal: `kiliba`
- One-line description: `Analyze ecommerce marketing performance, diagnose your connected shop, and run confirmed Kiliba actions with Claude.`
- Detail description:

  `Connect Claude to your Kiliba account using secure OAuth. Review aggregate marketing and ecommerce performance, compare campaigns and automation scenarios, identify attributed product contribution, and inspect your shop, synchronization, CMS and module version. Kiliba can also expose selected customer, transactional messaging and API-triggered workflow tools according to the authenticated user's existing permissions. Sensitive actions use a prepare-and-confirm flow and are never executed from the initial request alone. The connector also explains Kiliba and MARK capabilities while distinguishing features available through MCP from the complete MARK experience inside Kiliba.`

- Category candidates: Marketing, Ecommerce, Data & Analytics, Productivity
- Documentation: `https://github.com/kiliba-codebase/kiliba-mcp/tree/main`
- Privacy policy: `https://www.kiliba.com/politique-de-confidentialite`
- Support: `https://www.kiliba.com/contact`
- Company: `Kiliba`
- Website: `https://www.kiliba.com`
- Logo: `assets/kiliba-icon-512.png`

Choose only categories that exist verbatim in the portal. The slug is permanent
after publication, so confirm it before submitting.

### Use cases

1. Compare aggregate Kiliba marketing and ecommerce performance over a selected period.
2. Rank campaigns, Automation scenarios, workflows or Smartletters using observed performance.
3. Diagnose shop connectivity, synchronization, CMS and installed Kiliba module version.
4. Identify products or product families contributing to Kiliba-attributed revenue.
5. Prepare and explicitly confirm supported customer, messaging and workflow actions.

Prerequisites: an active Kiliba account, access to at least one connected shop,
and the permissions required for the tools the user enables.

The connector reads aggregate business data and can return personal customer
data only through the dedicated customer tool. It can write data or trigger an
action only through explicitly confirmed execution tools.

### Data handling answers

- Underlying API: Kiliba's own first-party API.
- Personal data: yes, only when a user invokes the dedicated customer tool or provides a recipient for an action.
- Health data: no.
- Sponsored content: no.
- Advertising use: no.
- Model-training use by Kiliba: no.
- Server logs: OAuth tokens, tool payloads, emails and phone numbers are excluded.
- Temporary storage: prepared action payloads are unusable after five minutes and scheduled for automatic TTL deletion from encrypted AWS infrastructure.
- Third parties: AWS hosts the service; the selected AI client receives tool inputs and results at the user's request under that client's terms.
- Intended for people under 18: no.

### Reviewer access

Create a dedicated, populated Kiliba demo account with:

- no MFA, SMS challenge or email-confirmation blocker;
- one active connected demo shop;
- representative reporting, campaign and product data;
- safe demo transactional templates and API-triggered workflow;
- no real customer or recipient data.

Enter its credentials only in Anthropic's reviewer-credentials fields.

### Required functional prompts

- `Which Kiliba shops can I access?`
- `Compare my marketing and ecommerce performance over the last 30 days.`
- `Which campaigns performed best this month?`
- `Check whether my Kiliba module is connected and up to date.`
- `Which product families contributed most to attributed revenue?`
- `What can Kiliba and MARK do, and which capabilities are available through this connector?`
- Prepare a safe demo action, verify that Claude asks for confirmation, then confirm it and verify the activity history.

## Submission 2 — Plugin bundle

- Submission type: `Plugin bundle`
- Repository: `https://github.com/kiliba-codebase/kiliba-mcp`
- Plugin path: repository root
- Tracked branch: `main`
- Manifest: `.claude-plugin/plugin.json`
- Connector configuration: `.mcp.json`
- Primary skill: `skills/kiliba-marketing/SKILL.md`
- Version: `1.0.0`

Submit the connector first. Submit the plugin from the same Claude organization,
then pair it with the Kiliba connector so users see one set of tools.

## OAuth credential email

Use this template without committing the Client ID or client secret. Never
include the client secret in an email.

**To:** `mcp-review@anthropic.com`  
**Subject:** `Kiliba MCP connector — Anthropic-held OAuth credentials`

```text
Hello Anthropic MCP review team,

Kiliba is preparing the following remote MCP connector for submission to the
Claude directory:

- Name: Kiliba
- MCP server: https://mcp.kiliba.ai/mcp
- OAuth Client ID: <CLIENT_ID>
- OAuth callback: https://claude.ai/api/mcp/auth_callback

This is a dedicated confidential OAuth client for Anthropic's hosted connector.
Please send us the secure transfer instructions for its client secret. We will
not send the secret by email or commit it to the public repository.

Documentation: https://github.com/kiliba-codebase/kiliba-mcp/tree/main

Best regards,
Kiliba
```

## Release commands

```sh
npx -y @anthropic-ai/claude-code@latest plugin validate . --strict
```

After validation, push `main`, open `https://claude.ai/directory/manage`, select
**Submit new**, and create the two submissions above.
