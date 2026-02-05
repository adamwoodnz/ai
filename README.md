# ai
AI tooling and configurations for Cursor IDE

## Overview
This repository contains Cursor IDE skills, MCP (Model Context Protocol) configurations, and related AI development tooling.

## Contents

### Skills (`skills/`)
Custom Cursor skills for common development workflows:
- **git-commit**: Craft clear, informative git commit messages following best practices
- **git-pull**: Pull the latest changes from remote repositories
- **pnpm-link-local**: Link local packages using pnpm for development testing
- **pr-feedback**: Fetch and assess review comments on pull requests

### MCP Configuration
Model Context Protocol configuration for integrating external tools and services with Cursor. The `mcp.json` file is user-specific and not tracked in this repository.

## Usage
These files are typically stored in `~/.cursor/` and are automatically loaded by Cursor IDE. The skills provide specialized capabilities that the AI assistant can use when helping with development tasks.
