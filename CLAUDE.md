# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Superpowers is a plugin system for coding agents that provides a workflow of composable "skills" for software development. It installs into Claude Code, Cursor, Codex, OpenCode, and Gemini CLI.

## Key Commands

### Running Integration Tests

```bash
cd tests/claude-code
./test-subagent-driven-development-integration.sh
```

Integration tests execute real Claude Code sessions and verify behavior through session transcripts. Tests can take 10-30 minutes.

### Analyzing Token Usage

```bash
python3 tests/claude-code/analyze-token-usage.py ~/.claude/projects/<project-dir>/<session-id>.jsonl
```

### Brainstorming Visual Server

The visual companion server is a zero-dependency Node.js WebSocket server:

```bash
# Start server
node skills/brainstorming/scripts/server.js

# Environment variables
BRAINSTORM_PORT=49152-65535  # Random if not set
BRAINSTORM_HOST=127.0.0.1    # Default
BRAINSTORM_DIR=/tmp/brainstorm  # Screen directory
```

## Architecture

### Skills System

Skills are the core abstraction. Each skill lives in `skills/<name>/SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name
description: Use when [triggering conditions] - NOT workflow summary
---
```

**Key principle**: Description must contain only triggering conditions, not workflow summaries. Claude may follow the description instead of reading the skill body if workflow is included.

### Plugin Configuration

- `.claude-plugin/plugin.json` - Claude Code plugin manifest
- `.cursor-plugin/plugin.json` - Cursor plugin manifest
- `gemini-extension.json` - Gemini CLI extension manifest
- `hooks/hooks.json` - Session start hooks

### Core Workflow

1. **brainstorming** → Refines ideas through questions, produces design spec
2. **writing-plans** → Creates detailed implementation plan from spec
3. **subagent-driven-development** OR **executing-plans** → Dispatches subagents per task
4. **test-driven-development** → RED-GREEN-REFACTOR during implementation
5. **requesting-code-review** → Reviews between tasks
6. **finishing-a-development-branch** → Merge/PR/cleanup

### Skill Writing (TDD for Documentation)

Creating skills follows RED-GREEN-REFACTOR:

1. **RED**: Run pressure scenario WITHOUT skill, document baseline failures
2. **GREEN**: Write skill addressing those specific failures
3. **REFACTOR**: Close loopholes from new rationalizations

**Iron Law**: No skill without a failing test first. Untested skills = untested code.

### Directory Structure

```
skills/
  <skill-name>/
    SKILL.md              # Required - main reference
    supporting-file.*     # Only if needed (heavy reference or tools)

agents/
  code-reviewer.md        # Agent definition for review tasks

commands/
  brainstorm.md           # Slash command definitions

hooks/
  hooks.json              # Session lifecycle hooks
  session-start           # Hook scripts

docs/
  testing.md              # Integration test documentation
  superpowers/
    specs/                # Design specs from brainstorming
    plans/                # Implementation plans
```

## Platform Adaptation

Skills use Claude Code tool names. Non-CC platforms should reference:
- `skills/using-superpowers/references/codex-tools.md` for Codex tool equivalents
- `skills/using-superpowers/references/gemini-tools.md` for Gemini CLI

## Testing Requirements

When running tests, ensure:
1. Running FROM the superpowers directory (skills only load from plugin dir)
2. Local dev marketplace enabled: `"superpowers@superpowers-dev": true` in `~/.claude/settings.json`
3. Use `--permission-mode bypassPermissions` and `--add-dir` for test directories

## Version Management

Version is defined in three places (keep synchronized):
- `.claude-plugin/plugin.json`
- `.cursor-plugin/plugin.json`
- `gemini-extension.json`