# opencode Workspace Configuration

This repository contains a local **opencode** workspace setup and a curated set of custom agent definitions.

If you use opencode for AI-assisted development, this repo gives you a reproducible starting point for:
- managing providers and permissions, and
- using specialized subagents for writing, review, architecture, research, and product work.

## Features

- 18 custom agent prompt files in `agents/`
- Permission defaults configured for common tools (`read`, `glob`, `bash`, etc.)

## Project structure

```text
.
├── agents/             # Custom subagent definitions (Markdown + frontmatter)
├── opencode.json       # Workspace config (permissions/providers/models)
```

## License
MIT
