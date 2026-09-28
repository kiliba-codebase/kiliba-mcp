# Shop configuration and module diagnosis

`get_shop_configuration` reads bounded public configuration for one authorized Kiliba account and runs the same module diagnostic used by MARK.

## Returned information

- shop name, public URL, country, currency and CMS;
- installed Kiliba module version;
- latest compatible version when the release catalog is available;
- whether an update is available or required;
- current connection and synchronization status;
- last observed synchronization date when available;
- a bounded diagnostic code, recovery guidance and optional official help link.

The tool never returns the module endpoint, CMS credentials, authentication token, API key or raw diagnostic response.

## Connection interpretation

- `connected`: the live endpoint returned valid Kiliba module metadata.
- `connected_from_account_state`: a hosted connector and completed synchronization are recorded by Kiliba.
- `synchronization_in_progress`: connection exists and initial synchronization is running.
- `not_synchronized`: connection exists but no completed synchronization has been observed.
- `not_configured`: the account has no complete store connection.
- `module_disabled`: the endpoint explicitly reports that the module is disabled.
- `authentication_failed`: the endpoint rejected Kiliba authentication.
- `account_mismatch`: the reachable PrestaShop module is attached to another Kiliba account.
- `connection_refused`, `timeout` or `unreachable`: Kiliba did not obtain valid module metadata.
- `unsupported`: this CMS connection type does not support the live module diagnostic.

## Cloudflare rule

Only report a confirmed Cloudflare obstruction when:

```text
diagnosis.cloudflareChallengeDetected = true
module.diagnosticCode = cloudflare_blocked
```

This state is produced only when the live module response contains a recognized Cloudflare challenge signature, such as its challenge platform or error 1020 page.

Do not infer Cloudflare from a generic HTTP error:

- `access_blocked` can represent another firewall or browser-security protection;
- `unreachable` does not identify a cause;
- `timeout` only means no valid response arrived in time;
- a plain 403 or 429 is classified as access blocking unless a Cloudflare signature is actually present.

When Cloudflare is detected, use the official Kiliba help link returned by the tool, apply the allowlisting instructions, and then rerun the diagnostic before claiming recovery.
