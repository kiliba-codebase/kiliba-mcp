# Connect Claude

## Claude.ai and Claude Desktop

The recommended route is to install Kiliba from Anthropic's directory once
the listing is published. Until then, the production connector remains
available as a custom connector.

In **Settings > Connectors**, add a custom connector with:

```text
https://mcp.kiliba.ai/mcp
```

In **Advanced settings**, enter the public Claude OAuth client ID published below and leave the client secret empty. Select **Connect**, complete authentication on the Kiliba sign-in page and review the permissions before enabling tools.

```text
OAuth Client ID: 7vpgvs802m7msu4manjusdu0ak
OAuth Client Secret: leave empty
```

The client ID is public by design. Never request or share a client secret for this connection.

## Claude Code

Claude Code uses the public PKCE client created for CLI use. It has no client secret.

```sh
claude mcp add --transport http \
  --client-id 3nv4hfrsqtk6qot09087a70e4p --callback-port 8080 \
  Kiliba https://mcp.kiliba.ai/mcp
```

Then run `/mcp` inside Claude Code and authenticate Kiliba in the browser. The callback registered by Kiliba is `http://localhost:8080/callback`.

To remove stored credentials later:

```sh
claude mcp logout Kiliba
```

## Suggested tests

- `What can Kiliba and its MARK agent do?`
- `Which Kiliba capabilities are available directly through this MCP?`
- `How could Kiliba help analyze ecommerce growth and automate customer lifecycle marketing?`
- `Check my Kiliba module connection, synchronization and installed version.`
- `Is the connection failure a detected Cloudflare challenge or only a generic access failure?`

Claude should use `discover_kiliba_capabilities` for these questions and must preserve the distinction between capabilities available inside Kiliba/MARK and those exposed through MCP.

## Plugin bundle

The Anthropic plugin bundle in this repository adds product-specific guidance
on top of the same remote connector. It does not contain credentials, customer
data, or server implementation code.

From the repository's parent directory, validate it with:

```sh
claude plugin validate ./kiliba-mcp --strict
```

For a local Claude Code test:

```sh
claude --plugin-dir ./kiliba-mcp
```

Inside Claude Code, run `/mcp` to connect the bundled Kiliba server and verify
that the `kiliba-marketing` skill is available.
