# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the official Claude Code plugin marketplace repository. It contains:
- `/plugins` - Internal plugins developed and maintained by Anthropic
- `/external_plugins` - Third-party plugins from partners and the community

The repository's primary task is maintaining **Chinese documentation** (README.zh-CN.md) for all plugins.

## Repository Structure

```
.claude-plugin/
└── marketplace.json    # Plugin registry with metadata

plugins/               # ~30 internal plugins
├── <plugin-name>/
│   ├── .claude-plugin/plugin.json
│   ├── README.md
│   ├── README.zh-CN.md   # Chinese translation
│   ├── commands/         # Slash commands
│   ├── agents/           # Agent definitions
│   └── skills/           # Skill definitions
└── ...

external_plugins/      # Third-party plugins
├── greptile/
├── stripe/
└── ...
```

## Common Tasks

### Adding Chinese Documentation for a New Plugin

When a new plugin is added to `marketplace.json`:
1. Create `README.zh-CN.md` in the plugin directory
2. Translate the English README to Chinese
3. Follow the translation patterns used in existing README.zh-CN.md files

### Updating Existing Chinese Documentation

When the English README is updated, corresponding updates to the Chinese version should be made:
- Keep terminology consistent with official Chinese documentation
- Match the structure and section headings of the English version

### Plugin Structure Validation

The repository has a GitHub workflow (`.github/workflows/validate-frontmatter.yml`) that validates frontmatter in plugin files.

## Key Files

- `.claude-plugin/marketplace.json` - Central plugin registry, defines all available plugins
- `README.md` / `README.zh-CN.md` - Main documentation
- Each plugin's `README.md` - Plugin-specific documentation

## Installation

To install a plugin from this marketplace:
```
/plugin install {plugin-name}@claude-plugin-directory
```

Or browse via `/plugin > Discover`
