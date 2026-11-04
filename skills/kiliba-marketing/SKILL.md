---
name: kiliba-marketing
description: Analyze and operate an authenticated Kiliba ecommerce marketing account. Use for performance, shops, customers, segments, Smartletter drafts and schedules, automation configuration, transactional messages, or API-triggered workflows.
---

# Use Kiliba safely and effectively

Use the Kiliba connector for facts about the authenticated user's Kiliba accounts. Never infer access from an email address or from conversation context: the connector enforces the user's current Kiliba permissions.

## Select the account

Call `list_accounts` before any account-specific tool when the user has not identified a shop. If several shops match, ask the user to choose. Reuse the returned opaque `accountRef`; never invent or expose an internal account identifier. Report an inactive or plan-ineligible shop as such instead of treating it as an empty result. Write access also depends on the authenticated user's permissions.

## Answer product questions

Call `discover_kiliba_capabilities` when the user asks what Kiliba or MARK can do. Distinguish:

- capabilities available through this MCP connector;
- capabilities available only in the Kiliba application or through MARK, Kiliba's integrated marketing agent.

Do not claim that the connector exposes MARK's complete in-product runtime.

## Analyze performance

- Use `get_account_overview` for a broad marketing and ecommerce review.
- Use `get_campaign_performance` to compare campaigns, Automation scenarios, workflows, or Smartletters.
- Use `get_performance_trend` for observed daily, weekly, or monthly changes; do not present it as a forecast.
- Use `get_ecommerce_performance` to compare shop revenue and Kiliba-attributed revenue.
- Use `compare_shops` for two to five authorized shops. Combine only additive totals when currencies match; never sum rates or averages.
- Use `get_attributed_product_performance` for products or product families contributing to attributed results.

Always preserve the period, data freshness, attribution rules, and limitations returned by the connector. Do not describe attributed revenue as causal lift, profit, margin, or ROI.

By default, omit `attribution_window` so reporting tools use the attribution window configured on the Kiliba account. When the user explicitly asks to compare or recalculate attribution over another supported window, pass `4h`, `24h`, `5days`, or `30days`. Make clear that this is a temporary analysis override and does not change the account configuration.

For aggregate engagement rates, use `rate_calculation: kiliba_average` by default to preserve the calculation displayed in Kiliba. Use `weighted_by_sends` only when the user explicitly asks for rates weighted by message volume, and label the calculation mode in the answer.

## Diagnose the shop connection

Use `get_shop_configuration` for the CMS, installed module version, synchronization state, connection status, and bounded module diagnostic. Report Cloudflare only when `cloudflareChallengeDetected` is explicitly true. A timeout, unreachable module, or generic access failure is not evidence of Cloudflare.

## Handle customer data

Use `list_customers` only when the user explicitly asks for customer-level data. Search by exact email or a name fragment when possible. Use `get_customer_profile` for one exact email and `get_contact_base_health` for aggregate contactability, blacklist and synchronization questions. Results can contain personal data: return only the fields needed for the request and avoid repeating unnecessary personal information. Label estimated counts and respect freshness or limitation fields.

## Inspect campaigns, audiences and automations

- Use `list_campaigns` for Smartletter drafts, scheduled campaigns and recorded sends. A past schedule is not proof that a send completed.
- Use `get_campaign_content` for localized subject, audience, exclusions, links and an HTML preview. When editing a custom HTML draft, request `include_source=true` and preserve its source HTML, wordings and variables; a sample preview is not proof of recipient-specific delivered content.
- Use `list_sender_profiles` to find a configured sender before preparing a draft. An empty list with a limitation means no sender is configured for that shop.
- When the user asks to create a Smartletter from attached HTML and images, inspect the HTML or archive in the conversation. Do not pass an archive to Kiliba. For each locally referenced image, call `upload_campaign_image` with the selected shop's `account_ref`, present its `uploadUrl` to the user, and ask them to choose that image in the browser. Never copy image bytes or base64 into tool arguments. After the user confirms the upload, call `get_campaign_image_upload` with `uploadRef`. Only when its status is `COMPLETED`, replace the matching local path (`src`, `srcset`, CSS `url()` and background attributes) with the returned `imageUrl`. If status is `PENDING`, ask the user to finish the upload; if the link expired or failed, create a new one. Check that no local image reference remains in the final HTML. Keep existing public HTTPS image URLs as they are. The upload accepts PNG, JPEG, GIF or WebP up to 2 MiB each. Never invent a URL or claim an image was hosted without a successful result. Upload only images needed for the requested draft.
- Use `list_segments` and `get_segment` for saved audiences. Use `preview_segment_audience` to count proposed conditions before `prepare_segment_creation`.
- Use `get_automation_configuration` to inspect active and inactive scenarios, workflows and their exclusions. Use `list_triggerable_workflows` only to find enabled workflows with an API trigger.

The connector does not currently offer campaign-specific non-opener segments, per-recipient optimized send times or A/B subject tests. Do not imply that these options are available through the MCP.

## Require confirmation for actions

Consequential actions follow two distinct steps:

1. Call the relevant `prepare_*` tool and show the exact proposed action, shop, content, recipient, audience or affected-customer count as applicable.
2. Ask for explicit confirmation. Only after a new affirmative user message call the matching `execute_*` tool with the returned confirmation token.

Never treat the initial request as confirmation, never skip preparation, and never substitute one action for another. This applies to segment creation, HTML Smartletter draft creation and editing, scheduling and cancellation, customer blacklist or whitelist changes, transactional messages, and workflow triggers.

`upload_campaign_image` creates a temporary one-time browser upload link; `get_campaign_image_upload` retrieves its Kiliba CDN URL. Neither creates nor sends a campaign. The user must still review and explicitly confirm the draft through `prepare_campaign_draft` and `execute_campaign_draft`.

Creating or editing a draft never schedules or sends it. Scheduling is a separate confirmed action. Use `get_campaign_content` and its `updatedAt` before preparing a draft update so a newer edit is not overwritten. A scheduled send can be cancelled only while its schedule is still active; already delivered messages cannot be recalled. Keep campaign, segment and sender references within their original authorized shop.

If the preparation expires or the action changes, prepare it again and request a fresh confirmation.
