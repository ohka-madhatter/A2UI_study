# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A2UI (Agent-to-User Interface) is a declarative JSON format and renderer set that allows AI agents to generate rich, interactive UIs rendered natively across multiple platforms. Agents send JSON describing UI intent; clients render using native components.

**Status:** v0.8 Public Preview
**License:** Apache 2.0

## Build Commands

### Web Core Library
```bash
cd renderers/web_core
npm install && npm run build
```

### Lit Renderer
```bash
cd renderers/lit
npm install && npm run build
npm run dev    # Dev server with hot reload
npm test       # Run tests
```

### Angular Renderer
```bash
cd renderers/angular
npm install && npm run build
```

### Python Agent SDK
```bash
cd a2a_agents/python/a2ui_agent
uv run pyink --check .           # Check formatting
uv run pyink .                    # Format code
uv run --with pytest pytest tests/  # Run tests
```

### Agent Samples
```bash
cd samples/agent/adk/<sample_name>
export GEMINI_API_KEY="your_key"
uv run .
```

### Client Samples
```bash
# Lit shell
cd samples/client/lit/shell
npm install && npm run dev

# Angular samples
cd samples/client/angular
npm run build <sample_name>
```

### Documentation
```bash
pip install -r requirements-docs.txt
mkdocs serve   # Local preview
mkdocs build   # Generate site/
```

## Architecture

### Core Concept
A2UI separates UI **generation** (agent-side) from UI **execution** (client-side):
1. Agent generates JSON payload describing UI components
2. Transport via A2A Protocol or other
3. Client's A2UI Renderer parses JSON
4. Abstract components mapped to concrete framework implementations

### Directory Structure
- `renderers/web_core/` - Shared TypeScript types, data models, styles (no UI components)
- `renderers/lit/` - Lit web components implementing the spec
- `renderers/angular/` - Angular renderer library
- `renderers/flutter/` - Flutter renderer
- `a2a_agents/python/a2ui_agent/` - Python agent SDK
- `samples/agent/adk/` - Agent sample implementations
- `samples/client/` - Client sample implementations
- `specification/` - JSON schemas and protocol docs (v0_8, v0_9, v0_10)
- `tools/` - Editor and inspector tools

### Multi-Version Support
The codebase supports multiple protocol versions simultaneously:
- `src/v0_8/` and `src/v0_9/` directories in web_core
- Version-specific exports in package.json
- JSON schema definitions per version in `specification/`

### Build Orchestration
- **wireit** orchestrates npm/TypeScript builds with dependency graphs
- TypeScript projects use composite project references
- Python uses **uv** for workspace-based dependency management

## Code Style

### TypeScript
- Target: ES2022, Module: ESNext
- Strict mode enabled
- Google TypeScript Style (gts)

### Python
- **pyink** formatter (unstable mode, 2-space indent)
- Google Python Style Guide
- Format check: `uv run pyink --check .`

## Testing

### TypeScript/Web
- Node.js built-in test runner
- Test files: `*.test.ts`
- Command: `npm test`

### Python
- pytest framework
- Command: `uv run --with pytest pytest tests/`

## CI/CD

GitHub Actions workflows in `.github/workflows/`:
- `web_build_and_test.yml` - Lit renderer
- `ng_build_and_test.yml` - Angular renderer
- `python_a2ui_agent_build_and_test.yml` - Python SDK
- `lit_samples_build.yml` - Lit samples
- `docs.yml` - Documentation

## Key Dependencies

### Web Renderers
- Lit, markdown-it for Lit renderer
- Angular 21+ for Angular renderer
- All web renderers depend on `web_core`

### Python SDK
- `a2a-sdk>=0.3.0` - Agent-to-Agent protocol
- `google-adk>=1.8.0` - Google Agent Development Kit
- `google-genai>=1.27.0` - Gemini API
- `jsonschema>=4.0.0` - Schema validation
