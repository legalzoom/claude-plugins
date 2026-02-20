# LegalZoom Claude Plugins

Internal Claude Code plugin marketplace for LegalZoom.

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
