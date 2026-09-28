# ChatGPT

Kiliba's ChatGPT directory integration is coming soon. It will use the same
production MCP endpoint, scoped OAuth permissions and explicit confirmation
for sensitive actions as the Claude connector.

Every tool already advertises its exact OAuth scope, title and safety
annotations. The authenticated `get_profile` tool returns a stable
pseudonymous profile reference so supported clients can distinguish connected
Kiliba identities without receiving the Cognito subject.

Never paste an access token or customer data into a repository or conversation.

## Planned validation prompts

- `What can Kiliba and MARK do for an ecommerce marketing team?`
- `Which of those capabilities can you execute through this connector today?`
- `Compare my marketing and ecommerce performance over the last 30 days.`
- `Explain which Kiliba automation capabilities could help repeat purchases, without changing anything.`
- `Check whether my shop module is connected, which version is installed and whether an update is available.`
- `Is Cloudflare blocking Kiliba? Only say so if the live diagnostic proves it.`

The first two prompts should call `discover_kiliba_capabilities` and clearly separate the full Kiliba/MARK product from the narrower public MCP contract.
