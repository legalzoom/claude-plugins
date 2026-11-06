# LegalZoom for Antigravity

Review contracts with AI, find information from LegalZoom about business formation, and request consultations with independent attorneys through LegalZoom.

This package is a preview prepared for marketplace review. Production OAuth sign-in and authenticated tool access in Antigravity still need an end-to-end check before submission.

## Requirements

- A current Antigravity App, CLI, or IDE version with plugin support.
- Internet access to LegalZoom and its OAuth sign-in service.
- A LegalZoom account for the remote MCP tools. Attorney consultations require an eligible legal plan; the service checks access before scheduling.
- Contract text that Antigravity can read for AI contract review. File extraction depends on the host's available tools; the plugin does not bundle a PDF or Word parser.

## Install from this repository

Clone the repository, then run from its root:

```sh
agy plugin validate --strict ./plugins/legalzoom-antigravity
agy plugin install ./plugins/legalzoom-antigravity
agy plugin list --json
```

The plugin is named `legalzoom`. Its bundled MCP server is namespaced as `legalzoom_legalzoom`. If you already have the separate LegalZoom MCP Store integration installed, avoid enabling both connections at the same time.

In Antigravity, open **Customizations > Installed > MCP Servers**, find the plugin's LegalZoom server, and select **Authenticate** if prompted. Sign in to your LegalZoom account and complete the authorization flow. Never paste your password or an access token into chat or repository files. The configuration relies on OAuth discovery and dynamic client registration; no shared client secret or API key is bundled.

## Included workflows

| Skill | What it does |
| --- | --- |
| `/review-contract` | Analyzes readable contract text, flags risks, and suggests revisions in Markdown. This is AI analysis and does not send the contract to LegalZoom or book an attorney. |
| `/attorney-assist` | Checks plan access and helps request an attorney phone consultation using live LegalZoom tools, after you choose the attorney and time and approve the details being shared. |

The Gemini MCP integration also provides LegalZoom article and product searches and text-based business formation information. Available tools depend on production configuration and your account. The plugin does not add document generation, estate planning widgets, legal filings, purchases, or appointment changes.

Try:

- “Review this contract from the customer's perspective, especially liability and renewal terms.”
- “Help me request an attorney consultation about my business contract.”
- “Find LegalZoom information comparing an LLC and a corporation.”

Contract analysis in Antigravity does not automatically share the conversation or original file with an attorney. Before a consultation request, the skill explains the summary and findings to be shared. When secure document upload is available, use the returned upload link before selecting a consultation time. Do not paste confidential document text into a service request as a substitute for file upload.

## Troubleshooting

- **Missing MCP tools:** Check that the plugin is enabled and the `legalzoom_legalzoom` server is connected. Restart the connection after installing or changing the configuration.
- **Sign-in fails or tools return 401/403:** Reauthenticate in Antigravity's MCP settings. If OAuth registration or the callback is rejected, report it to the plugin maintainer; do not substitute another client's credentials or endpoint.
- **No plan access or availability:** Use the service's actual response. Plan access, supported matters, jurisdictions, and attorney availability can vary. No booking exists until the booking tool confirms success.
- **Service unavailable:** Stop the service workflow and try again later, or visit [LegalZoom](https://www.legalzoom.com). AI contract analysis can still work with readable text already available in the host.
- **Remove this package:** Run `agy plugin uninstall legalzoom`.

## Legal notice

AI-generated contract analysis is general information and may be incomplete or incorrect. Have a licensed attorney review it before relying on it for legal decisions. LegalZoom is not a law firm and does not provide legal advice. Consultations are provided by independent attorneys; eligibility, terms, and availability apply.
