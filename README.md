# Kiliba MCP — ecommerce marketing automation with MARK

Kiliba makes ecommerce marketing automation simple. Connect your shop, activate more than 30 preconfigured scenarios in a few clicks — including abandoned carts, cross-selling, welcome, loyalty, reactivation and back in stock — or create your own workflows for a custom customer journey.

MARK is Kiliba's integrated AI marketing agent. It helps pilot campaign preparation and monitoring, analyzes performance, recommends the next useful action and keeps sensitive decisions under your control. Kiliba can detect the colors, fonts and links of your website to prepare campaigns that look like your brand. MARK can then audit an email and improve its content or supported design from a simple conversational request.

Kiliba MCP connects Claude and compatible AI clients to selected Kiliba reporting, shop diagnostics and explicitly authorized actions. ChatGPT and Mistral integrations are coming soon. The connector does not expose MARK's complete in-product runtime.

- Production endpoint: `https://mcp.kiliba.ai/mcp`
- Transport: MCP Streamable HTTP
- Authentication: Kiliba OAuth 2.0 broker backed by Cognito, using Authorization Code with PKCE `S256`
- Product UI: none; authentication uses the Kiliba sign-in page

## Capabilities

- Discover what Kiliba and its integrated MARK marketing agent can do, with a clear distinction between product capabilities and the subset available through MCP.
- Read the current shop, CMS, module version, synchronization and bounded connection diagnostic.
- Aggregate marketing and ecommerce reporting.
- Customer listing, limited to 100 records per page.
- Transactional template discovery and explicitly confirmed email, SMS, RCS or WhatsApp sends.
- API-triggerable workflow discovery and explicitly confirmed execution.
- Explicitly confirmed customer blacklist and whitelist operations.

See [KILIBA_AND_MARK.md](docs/KILIBA_AND_MARK.md) for the product capability guide, [TOOLS.md](docs/TOOLS.md) for the complete MCP contract and [PRIVACY.md](PRIVACY.md) for data-handling boundaries.

## What can I ask?

- What can Kiliba automate for an ecommerce brand?
- How can MARK help me analyze and prepare a campaign?
- Compare my marketing and ecommerce performance with the previous period.
- Which products or product families contribute most to attributed revenue?
- Which Kiliba capabilities are available through this MCP?
- Is my Kiliba module connected and up to date?
- Is Cloudflare actually blocking Kiliba, or is the module unreachable for another reason?

The capability guide is factual product documentation, not a promise that every Kiliba or MARK feature can be executed from an external AI client. MARK prepares and assists; campaign creation, scheduling and sending remain explicit, distinct steps.

## Connect

- [Claude.ai, Claude Desktop and Claude Code](docs/CLAUDE.md)
- ChatGPT directory availability is coming soon.
- Mistral directory availability is coming soon.

## Claude plugin

This repository contains an Anthropic plugin bundle that combines the remote
Kiliba connector with guidance for using Kiliba and MARK safely and
effectively. Its manifest is in `.claude-plugin/plugin.json`, its connector
reference is in `.mcp.json`, and its workflow guidance is in
`skills/kiliba-marketing/SKILL.md`.

To validate a local checkout with Claude Code:

```sh
claude plugin validate . --strict
```

To test the bundle before directory publication, start Claude Code with
`claude --plugin-dir .` or upload a zip of this repository from **Customize >
Plugins > Add > Upload plugin** in Claude.ai. Connect the Kiliba connector from
the plugin's **Connectors** tab and complete Kiliba OAuth authentication.

The connector uses the permissions of the signed-in Kiliba user. Read-only account access never permits write actions.

## Support and security

Do not post access tokens, customer data or account details in a public issue. Follow [SECURITY.md](SECURITY.md) for vulnerability reports.

Use of the plugin files is governed by [LICENSE](LICENSE). Use of the Kiliba
service remains governed by Kiliba's [terms of service](https://www.kiliba.com/cgu)
and [privacy policy](https://www.kiliba.com/politique-de-confidentialite).
