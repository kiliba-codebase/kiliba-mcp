# ChatGPT

The [Kiliba plugin for ChatGPT](https://chatgpt.com/plugins/plugin_asdk_app_6abd8942e1f88191b730634070f113b7)
connects to the production MCP endpoint with scoped OAuth permissions. It uses
the authenticated Kiliba user's shop access and requires explicit confirmation
for write actions.

Every tool already advertises its exact OAuth scope, title and safety
annotations. The authenticated `get_profile` tool returns a stable
pseudonymous profile reference so supported clients can distinguish connected
Kiliba identities without receiving the Cognito subject.

Never paste an access token or customer data into a repository or conversation.

## Example prompts

- `What can Kiliba and MARK do for an ecommerce marketing team?`
- `Which of those capabilities can you execute through this connector today?`
- `Compare my marketing and ecommerce performance over the last 30 days.`
- `Explain which Kiliba automation capabilities could help repeat purchases, without changing anything.`
- `Check whether my shop module is connected, which version is installed and whether an update is available.`
- `Is Cloudflare blocking Kiliba? Only say so if the live diagnostic proves it.`
- `List my Smartletter drafts and saved audience segments for this Kiliba shop.`
- `Prepare an HTML Smartletter draft for my review, without scheduling or sending it.`
- `Use the HTML and images in this ZIP to prepare a Smartletter draft for my review. Identify each locally referenced image, give me its Kiliba direct-upload link so I can choose the file in my browser, then retrieve its CDN URL and replace the relative path. Show me the resulting draft before creating it. Do not schedule or send.`

The first two prompts should call `discover_kiliba_capabilities` and clearly
separate the full Kiliba/MARK product from the public MCP contract. A new
conversation helps ChatGPT pick up a newly published tool definition.

Image transfer is one file per `upload_campaign_image` link. The Kiliba MCP
server does not receive ZIP archives. Extract the ZIP on your device so you can
choose each image in the browser upload page. After upload, the assistant calls
`get_campaign_image_upload` and uses its returned CDN URL. No image bytes or
base64 need to pass through a ChatGPT tool argument.
