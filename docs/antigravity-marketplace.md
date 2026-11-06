# Antigravity marketplace preparation

Package root: `plugins/legalzoom-antigravity`. Marketplace/plugin name: `legalzoom`.

This is a separate package from `plugins/legalzoom`: Antigravity uses `mcp_config.json`,
the Gemini endpoint, and discoverable skills; Claude keeps its existing `.mcp.json`,
endpoint, and command. The package directory name is not the installed plugin ID.
The Antigravity package is not registered in the Claude marketplace file.

## Submission materials

- Manifest: `plugins/legalzoom-antigravity/plugin.json`
- Pre-install documentation: `plugins/legalzoom-antigravity/README.md`
- Square bundled logo: `plugins/legalzoom-antigravity/assets/logo.svg` (256 × 256,
  transparent outer padding, existing LegalZoom logomark on a white tile for contrast)
- MCP configuration: `plugins/legalzoom-antigravity/mcp_config.json`
- Skills: `review-contract` and `attorney-assist`
- Source repository: <https://github.com/legalzoom/claude-plugins>
- Package path to supply with the repository: `plugins/legalzoom-antigravity`

The attorney skill adapts the existing workflow to current tool schemas and
Antigravity authentication settings. It does not hardcode plan pricing, fabricate
service results, claim full transcript delivery, or promise post-booking file
attachment. The contract skill preserves AI-only analysis with no service calls.

## Production endpoint and OAuth evidence

Unauthenticated checks on 2026-11-06:

- `POST https://www.legalzoom.com/mcp/gemini/v1` with an MCP `initialize` request
  returned HTTP 401 and a `WWW-Authenticate` challenge pointing to
  `https://www.legalzoom.com/.well-known/oauth-protected-resource/mcp/gemini/v1`.
- That metadata returned `resource: https://www.legalzoom.com/mcp/gemini/v1`
  and `authorization_servers: [https://auth.apigw.legalzoom.com]`.
- `https://auth.apigw.legalzoom.com/.well-known/oauth-authorization-server`
  advertised authorization code grants, PKCE `S256`, and
  `registration_endpoint: https://auth.apigw.legalzoom.com/oauth2/register`.
- No CIMD support was advertised. Discovery therefore points Antigravity toward
  dynamic client registration, with no manually supplied client ID or secret.
  Advertised token endpoint authentication methods include confidential-client
  methods and do not include `none`; successful client registration and token
  exchange must be confirmed with the actual Antigravity client.

These checks verify a reachable protected route and discovery metadata. A 401
before authentication does not establish that the integration's production
feature gate is enabled, tools can be listed, or a given account is entitled.
No OAuth client was registered, credentials changed, or user authentication
performed during preparation.

## Validation and release checks

Preparation checks passed on 2026-11-06:

- Official Antigravity CLI 1.5.2, downloaded from Google's release manifest and
  verified against its SHA-512 checksum: strict validation returned no findings,
  discovering 2 skills and 1 MCP server.
- Local `plugin install` succeeded; `plugin list --json` reported `legalzoom`
  version `0.1.0`, both skills, and the MCP server. The temporary installation was
  then uninstalled. No sign-in or authenticated service calls were performed.
- Manifest validated against the JSON Schema embedded in the official build
  documentation. The standalone schema URL returned 404 during preparation.
- Both skills passed the skill-authoring frontmatter validator. Logo dimensions
  and local references passed checks; a rendered light/dark preview was inspected.

Run from the repository root with the current official Antigravity CLI:

```sh
agy plugin validate --strict ./plugins/legalzoom-antigravity
agy plugin install ./plugins/legalzoom-antigravity
agy plugin list --json
```

Before marketplace submission:

- [ ] Complete Antigravity OAuth sign-in with an approved test account. Verify
  client registration and the `https://antigravity.google/oauth-callback` redirect,
  PKCE authorization, code exchange, reconnection, and token refresh. Do not reuse
  another client's credentials if registration fails.
- [ ] Verify `initialize` and `tools/list` against production after authentication;
  confirm the Gemini feature gate and the knowledge, formation, and attorney tools
  actually available. Adjust README claims if the tool list differs.
- [ ] Run a read-only article/formation query and entitlement check. Confirm
  behavior for an account without an eligible plan and for authentication failure.
- [ ] In Antigravity, invoke `/review-contract` on sample non-confidential text;
  verify grounded findings, readable output, no MCP service calls, and no claims
  that an attorney has reviewed the document.
- [ ] With a designated test account and authorization for test side effects,
  exercise the attorney workflow through the approved test process: real topics,
  locations, availability, optional upload before scheduling, explicit confirmation,
  and an actual booking response. Check failures and uncertain outcomes. Do not
  create a production customer booking merely to finish this checklist.
- [ ] Inspect the Marketplace detail preview and logo on Light and Dark themes;
  confirm copy, branding, license, and publisher attribution with the owner.
- [ ] Merge an approved release and confirm the package path is publicly fetchable
  at the revision provided to Google. Remove the preview notice only after the
  end-to-end checks pass.
- [ ] Submit through Google's partner intake form, identifying the package root
  within this repository. This preparation does not submit or publish the plugin.

## References

- [Official plugin authoring and submission requirements](https://antigravity.google/docs/build-plugins/)
- [Official MCP transport and OAuth requirements](https://antigravity.google/docs/mcp/)
- [Official CLI download instructions](https://antigravity.google/download)
