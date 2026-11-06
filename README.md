# LegalZoom Plugins

Internal Claude Code plugin marketplace for LegalZoom.

An Antigravity package is also available for local testing in
[`plugins/legalzoom-antigravity`](plugins/legalzoom-antigravity/README.md).
It includes its own `plugin.json`, Gemini MCP configuration, logo, and adapted
contract review and attorney consultation skills. The existing Claude package
and marketplace registration remain separate.

From this repository's root:

```sh
agy plugin validate --strict ./plugins/legalzoom-antigravity
agy plugin install ./plugins/legalzoom-antigravity
```

See the [Antigravity marketplace preparation checklist](docs/antigravity-marketplace.md)
for validation evidence and the remaining release checks.

## Installation

### Manual Installation (Recommended)

For step-by-step instructions with screenshots, see the [Installation Guide](docs/install.md).

Quick steps:
1. Clone this repository
2. Run `./create_plugin_zip.py` to generate `legalzoom.zip`
3. Upload the zip file via Claude's Plugins menu

### Claude Code CLI

Add this marketplace to Claude Code:

```bash
/plugin marketplace add legalzoom/claude-plugins
```

Then install individual plugins:

```bash
/plugin install legalzoom@legalzoom-marketplace
```

## Available Plugins

### legalzoom

AI-powered contract review with seamless attorney consultation:

- `/review-contract` — In-depth contract analysis with risk-scored findings
- `/attorney-assist` — Connect with a LegalZoom attorney with full conversation context

## Contributing

Add new plugins under the `plugins/` directory following the standard structure:

```
plugins/your-plugin/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── your-skill/
        └── SKILL.md
```

Then register it in `.claude-plugin/marketplace.json`.
