# Privacy and data boundaries

Kiliba MCP uses the permissions of the authenticated Kiliba user and rechecks current account access for every request.

Aggregate reporting is the default data surface. It does not expose contact lists, raw events or individual orders. Personal data is returned only when the user explicitly invokes the dedicated customer tool, or supplies a recipient for an explicitly confirmed action.

The MCP service does not log OAuth tokens, email addresses, phone numbers or tool payloads. AI clients remain responsible for their own processing and retention of tool inputs and results; consult the privacy terms of the client you choose before enabling customer-data tools.

When a user prepares a sensitive action, Kiliba temporarily stores the bounded
action payload in encrypted AWS infrastructure so it can be executed only
after explicit confirmation. The confirmation becomes unusable after five
minutes and the record is scheduled for automatic deletion. Physical deletion
can occur later according to the cloud provider's TTL processing window. The
payload is not used for advertising or model training by Kiliba.

Disconnect the connector in the AI client to remove its stored authorization. Kiliba account administrators can also remove the user's underlying account access.

Kiliba's public privacy policy is available at
https://www.kiliba.com/politique-de-confidentialite. Privacy questions can be
sent through https://www.kiliba.com/contact.
