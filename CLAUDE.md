# CLAUDE.md — snake-eater

This file provides guidance for AI assistants (Claude Code and similar tools) working in this repository.

## Repository Overview

**Repository:** `glutenmagic/snake-eater`

This is an early-stage project. Currently the repo contains:

- `.mcp.json` — MCP server configuration for Figma integration

No application source code has been committed yet. This CLAUDE.md will be updated as the codebase grows.

## Repository Structure

```
snake-eater/
├── .mcp.json       # MCP server configuration (Figma)
└── CLAUDE.md       # This file
```

## MCP Server Configuration

The `.mcp.json` file configures the **Figma MCP server**, enabling Claude Code to access Figma design files directly.

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "figma-developer-mcp", "--stdio"],
      "env": {
        "FIGMA_API_KEY": "${FIGMA_API_KEY}"
      }
    }
  }
}
```

**Setup required:** Export `FIGMA_API_KEY` in your shell before starting Claude Code:

```sh
export FIGMA_API_KEY=your_figma_personal_access_token
```

You can generate a Figma personal access token at **Figma → Settings → Security → Personal access tokens**.

## Git Workflow

### Branches

- **Feature branches** follow the pattern `claude/<feature-description>-<id>` for Claude-driven work.
- Always develop on a dedicated feature branch; do not commit directly to `main`.

### Commit conventions

- Use concise, imperative commit messages (e.g. `Add snake movement logic`, `Fix collision detection`).
- Include a blank line between the subject and body when extra context is needed.
- Claude Code sessions append a session URL to the commit body automatically — leave these in place.

### Push workflow

```sh
git push -u origin <branch-name>
```

Retry up to 4 times with exponential backoff (2 s, 4 s, 8 s, 16 s) on network failures.

## Development Conventions

These conventions should be adopted once source code is added:

- **Do not add speculative abstractions.** Write only the code the current task requires.
- **Do not add error handling for impossible cases.** Trust internal invariants; validate only at system boundaries (user input, external APIs).
- **No superfluous comments.** Only comment logic that isn't self-evident.
- **No backwards-compat shims** for removed code — delete unused things cleanly.
- **Security:** Avoid OWASP top-10 vulnerabilities (XSS, injection, etc.) from the start.

## Working with Figma Designs

With the Figma MCP server configured, Claude Code can:

- Read component definitions, styles, and layouts from Figma files.
- Translate designs into code without manual copy-paste.

Pass Figma file URLs or node IDs when asking Claude to implement designs.

## Updating This File

Update `CLAUDE.md` whenever:

- New source directories or significant files are added.
- A build system, test framework, or linter is introduced.
- Environment variables or external service dependencies change.
- Coding conventions are established or revised.
