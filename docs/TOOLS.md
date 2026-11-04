# Tool contract

## Connected profile

- `get_profile`

Returns a stable pseudonymous profile reference and optional display name or email for the authenticated Kiliba identity. ChatGPT can use it to distinguish several connected identities. The profile reference does not expose the Cognito subject and the email must never be used as an authorization decision.

## Product discovery

- `discover_kiliba_capabilities`

Use this read-only tool when a user asks what Kiliba or MARK can do, which product area fits a marketing or ecommerce goal, or which functions are currently available through the MCP. Its result marks every capability as:

- `available_now`: directly supported by the current public MCP;
- `partial`: only the described subset is exposed;
- `kiliba_or_mark_only`: available in Kiliba or MARK, but not through the current public MCP.

The optional `kiliba://product/capabilities` resource lists the available capability topics. Some clients, including versions of the Claude API MCP connector, support tools but not MCP resources; the discovery tool is therefore the portable contract.

## Shop configuration and module diagnosis

- `get_shop_configuration`

Use this read-only tool when the user asks:

- which CMS and Kiliba module version are connected;
- whether the installed module is up to date;
- whether the shop has synchronized;
- whether the module is disabled, unreachable, misconfigured or bound to another account;
- whether a Cloudflare challenge is currently blocking Kiliba.

The tool runs Kiliba's bounded live module diagnostic and never returns an endpoint, credential, token or raw CMS response. Report Cloudflare only when `diagnosis.cloudflareChallengeDetected` is `true`. An `access_blocked`, `timeout` or `unreachable` result is not proof of Cloudflare.

See [SHOP_CONFIGURATION.md](SHOP_CONFIGURATION.md) for interpretation rules.

## Aggregate reporting

- `list_accounts`
- `get_account_overview`
- `get_campaign_performance`
- `get_performance_trend`
- `get_ecommerce_performance`
- `get_attributed_product_performance`
- `compare_shops`: compares two to five authorized shops over the same period and combines only additive totals when currencies match.

The `kiliba://reporting/metrics` resource documents metric definitions, units, attribution, freshness and limitations. Reporting defaults to 30 days, accepts at most 24 months and returns at most 50 rows. Results distinguish measured zero from unavailable data through `ready`, `partial` and `empty` statuses.

Aggregate reporting never returns individual orders, raw events, margin, profit or invented ROI. Customer and segment tools below return scoped personal or audience data only when the user's grant and OAuth scope permit them.

## Customer access

- `list_customers`: up to 100 customers per opaque cursor page; filter by exact email or part of a name with `search_text`.
- `get_customer_profile`: one exact email in one shop; estimated email contactability, RFM segment, last order, blacklist reason and recent received scenarios.
- `get_contact_base_health`: current contact totals and recent updates, blacklist reasons for a selected period, last known synchronization state. The returned limitations explain estimated contactability and bounce counts.
- `prepare_customer_status_change`
- `execute_customer_status_change`

## Smartletters and audience segments

- `list_campaigns`: browse drafts, schedules and sent Smartletters. Sent status uses the same recorded delivery evidence as the Kiliba app; a past schedule alone is not proof of delivery.
- `get_campaign_content`: read localized subject, rendered HTML preview and links. Set `include_source=true` when editing a custom HTML draft; use `sourceHtml`, `wordings` and `variables` to preserve its placeholders. The preview is an estimate, not an exact archive of each delivered email.
- `list_sender_profiles`: inspect configured sender identities.
- `upload_campaign_image`: create a one-time browser upload link for one PNG, JPEG, GIF or WebP image (maximum 2 MiB) in the selected shop. Give the link to the user; the image file is selected and transmitted directly from their browser, never through a text tool argument.
- `get_campaign_image_upload`: after the user imports the file, retrieve its Kiliba CDN URL using the `uploadRef` from the first tool. Replace the matching local image path in the HTML before preparing a draft. Existing public HTTPS image URLs need no upload.
- `list_segments` and `get_segment`: inspect saved audience definitions and counts.
- `preview_segment_audience`: count an unsaved audience using existing Kiliba conditions.
- `prepare_segment_creation` and `execute_segment_creation`: save a reviewed segment. Supported conditions include email contactability, RFM, last order, email engagement over a period, customer group and custom attributes.
- `prepare_campaign_draft` and `execute_campaign_draft`: create an unscheduled HTML Smartletter after review.
- `prepare_campaign_draft_update` and `execute_campaign_draft_update`: modify an unscheduled HTML draft with a revision check.
- `prepare_campaign_schedule` and `execute_campaign_schedule`: review and schedule a Smartletter at a Europe/Paris date and time.
- `prepare_campaign_cancellation` and `execute_campaign_cancellation`: review and cancel an active schedule. Already delivered messages cannot be recalled.

Draft creation and editing never schedule or send. Use the returned opaque campaign, segment and sender references only with the same authorized shop. Campaign-specific non-opener segments, per-recipient optimized send times and A/B subject tests are not exposed by these tools.
Image upload stores an asset in the selected shop's Kiliba image library. The upload link is valid until the returned `expiresAt` (at most 10 minutes) and accepts one file; the resulting CDN URL remains retrievable for one hour. Uploading does not create a campaign. The user chooses the file in the browser; the assistant must not serialize image bytes or ask the user to paste base64.
Audience exclusions require at least one included segment; an automatic audience cannot silently ignore an exclusion.

## Transactional messaging

- `list_transactional_templates`
- `prepare_transactional_send`
- `execute_transactional_send`

Preparation validates the template, variables, recipient country, channel configuration and current credits without sending or debiting. Execution repeats authoritative checks and atomically consumes a single-use confirmation.

## Workflows

- `list_triggerable_workflows`
- `prepare_workflow_trigger`
- `execute_workflow_trigger`
- `get_automation_configuration`: read all configured scenarios and workflows, including inactive entries and their product, category and audience exclusions. Sensitive workflow node values are omitted.

Only enabled workflows with an API trigger are exposed. Preparation validates the recipient and active-run constraint without starting a workflow.

## Confirmation model

Write actions always require a prepare call followed by explicit user confirmation and an execute call. Confirmation tokens are opaque, expire after five minutes and are consumed atomically. Retrying an already completed confirmation returns its stored result and does not repeat the action.
