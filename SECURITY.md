# Security policy

## Scope

prismarine-chunk reads and writes serialized Java and Bedrock chunk data, including palettes, light arrays, NBT block entities, blobs, and cache responses. Malformed network data can trigger excessive allocation, out-of-bounds access, parser exceptions, or denial of service.

## Supported versions

There is no formal long-term security-support matrix for this fork. Triage starts with current master and the latest published package. The compatibility target is the version dispatch in src/index.js, including Java 26.2 and 26.3 and the Bedrock families listed in CONTEXT.md; older versions are not promised separate security maintenance.

## Reporting a vulnerability

As verified on 2026-09-19, this fork has no enabled GitHub private vulnerability-reporting endpoint. Do not disclose exploit bytes or private world data in a public issue, discussion, pull request, or chat message.

1. Check the repository GitHub **Security** tab for **Report a vulnerability** and use the private form if available.
2. If it is unavailable, use a private contact method listed on the [Misty4119 GitHub profile](https://github.com/Misty4119). If no private route is visible, request one without publishing the vulnerability details and then send them privately.
3. Remove world coordinates, account identifiers, server addresses, proprietary fixtures, and credentials unless needed to reproduce the issue.

Include the affected commit/version and edition, input type and size, minimal reproduction or fixture, expected and observed behavior, impact, and environment. Do not send live server data if a reduced fixture is sufficient. No response or remediation time is promised.

For ordinary decoding or API bugs, use the [public issue tracker](https://github.com/Misty4119/prismarine-chunk/issues) after redacting sensitive data.
