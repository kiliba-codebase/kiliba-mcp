# Kiliba and MARK capabilities

Kiliba makes ecommerce marketing automation accessible in a few clicks. Once the shop is connected, a merchant can activate more than 30 preconfigured scenarios — abandoned carts, cross-selling, welcome, loyalty, reactivation, birthdays, back in stock and seasonal moments — or create custom workflows for a specific customer journey.

MARK is Kiliba's integrated AI marketing agent. It helps pilot campaign preparation and monitoring: understanding the objective, examining evidence, recommending audiences and products, preparing guarded drafts and analyzing observed results. MARK never silently turns preparation into scheduling or sending.

Kiliba can detect colors, fonts, navigation links and visual cues from the merchant's website to prepare campaigns that resemble the brand. MARK can audit an email and improve supported wording, subjects, preheaders, links, images, colors, spacing, mobile layout, product cards and calls to action from a simple conversational request.

This page summarizes current product capabilities. It deliberately distinguishes them from the smaller set exposed through the public MCP server.

## Understand marketing and ecommerce performance

Kiliba and MARK can analyze aggregate marketing and commerce evidence such as:

- total store revenue and revenue attributed under Kiliba's rules;
- orders, buyers and average order value;
- sends, delivery, opens, clicks, conversions, bounces and unsubscribes;
- campaigns, Automation scenarios, workflows, Smartletters and channels;
- product and product-family performance;
- trends and comparisons across explicit periods.

Kiliba does not present observed attribution as incremental revenue, profit, margin, ROI or causal proof.

**MCP availability:** aggregate account, campaign, trend, ecommerce and attributed-product reporting is available today.

## Automate customer lifecycle marketing

Kiliba Automation provides more than 30 ready-to-use ecommerce scenarios. They cover recurring moments including welcome, abandoned carts, cross-selling, reactivation, loyalty, repeated purchase, birthdays, seasonal events and back in stock. Merchants can activate the relevant journey in a few clicks or create a custom workflow when their business needs a different automation.

MARK can explain scenarios, inspect their state and statistics, diagnose their execution path and prepare supported parameter changes with confirmation.

**MCP availability:** aggregate scenario and workflow reporting is available. Enabled API-triggerable workflows can be discovered and triggered after explicit confirmation. Full scenario configuration and diagnosis remain inside Kiliba/MARK.

## Create and refine Smartletters

MARK can help prepare a Smartletter's objective, products, audience, languages, wording, theme, composition and preflight. Draft creation, scheduling and sending remain separate decisions.

Kiliba can start from the shop itself: website colors, fonts and links can be detected and proposed before validation. Within Kiliba's editor, MARK can audit the email and conversationally refine supported subjects, preheaders, wording, links, images, colors, spacing, mobile layout, product cards, blocks and calls to action. Changes use guarded native editor operations rather than arbitrary MJML or CSS.

**MCP availability:** Smartletter aggregate performance can be analyzed. Creation and conversational editing are not exposed through the current public MCP.

## Build audiences and segments

MARK can understand explicit audience criteria in natural language, resolve account products, categories, lists and segments, preview raw and eligible volumes, explain exclusions and prepare supported segment creation after confirmation.

**MCP availability:** bounded customer listing and explicitly confirmed blacklist or whitelist operations are available. Dynamic-segment compilation and creation remain inside Kiliba/MARK.

## Coordinate channels and transactional messaging

Kiliba supports email, SMS, RCS, WhatsApp and transactional templates. MARK can prepare channel-specific marketing drafts while keeping content preparation, preview, draft creation, scheduling and sending as distinct steps.

**MCP availability:** transactional templates can be discovered. A transactional email, SMS, RCS or WhatsApp send can be prepared, validated and executed after explicit confirmation. Marketing campaign creation is not exposed.

## Use MARK as an integrated marketing agent

MARK can:

- explain Kiliba and navigate to relevant product surfaces;
- read authorized account state with freshness and limitations;
- analyze contacts, catalog, campaigns and ecommerce evidence;
- recommend priorities, audiences, scenarios and product opportunities;
- prepare guarded changes and require confirmation for sensitive writes;
- retain task context and asynchronous operation state;
- diagnose common marketing, deliverability, integration and rendering problems before support escalation.

MARK is broader than this external connector. Connecting Kiliba MCP to ChatGPT or Claude does not expose MARK's complete internal skill catalog or in-product navigation.

## Safety model

- Account access and rights are checked server-side.
- Reads, preparations and writes are separate.
- Preparation creates, schedules and sends nothing.
- Sensitive actions require explicit confirmation.
- Confirmation replay does not duplicate a completed action.
- Business payloads, tokens and recipients are excluded from technical audit logs.
- Current MCP activity distinguishes the OAuth client, Cognito subject, account and action; successful writes also appear in Kiliba activity history with a pseudonymous actor.

## Capability discovery contract

Call `discover_kiliba_capabilities` with one of these topics:

- `overview`
- `mark_agent`
- `analytics_reporting`
- `ecommerce_growth`
- `automation_workflows`
- `smartletters_content`
- `audiences_segments`
- `multichannel_messaging`
- `diagnostics_support`
- `safety_governance`

Every result explicitly says whether the capability is available through MCP now, partially exposed, or available only inside Kiliba/MARK.
