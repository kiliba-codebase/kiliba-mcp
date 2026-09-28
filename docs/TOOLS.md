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

The `kiliba://reporting/metrics` resource documents metric definitions, units, attribution, freshness and limitations. Reporting defaults to 30 days, accepts at most 24 months and returns at most 50 rows. Results distinguish measured zero from unavailable data through `ready`, `partial` and `empty` statuses.

Reporting never returns contacts, segments, individual orders, raw events, margin, profit or invented ROI.

## Customer access

- `list_customers`: up to 100 customers per opaque cursor page.
- `prepare_customer_status_change`
- `execute_customer_status_change`

## Transactional messaging

- `list_transactional_templates`
- `prepare_transactional_send`
- `execute_transactional_send`

Preparation validates the template, variables, recipient country, channel configuration and current credits without sending or debiting. Execution repeats authoritative checks and atomically consumes a single-use confirmation.

## Workflows

- `list_triggerable_workflows`
- `prepare_workflow_trigger`
- `execute_workflow_trigger`

Only enabled workflows with an API trigger are exposed. Preparation validates the recipient and active-run constraint without starting a workflow.

## Confirmation model

Write actions always require a prepare call followed by explicit user confirmation and an execute call. Confirmation tokens are opaque, expire after five minutes and are consumed atomically. Retrying an already completed confirmation returns its stored result and does not repeat the action.
