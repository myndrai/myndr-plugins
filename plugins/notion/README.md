# Notion

Search, write and organize your Notion workspace through Notion's hosted MCP server
(`https://mcp.notion.com/mcp`).

## What it does

- **Apps:** one remote MCP server, `notion`. Myndr signs in with your Notion account using the MCP
  authorization flow (Notion's own consent screen). Nothing in this package holds a credential.
- **Skills** (by Notion Labs, see below):
  - `notion:knowledge-capture`
  - `notion:meeting-intelligence`
  - `notion:research-documentation`
  - `notion:spec-to-implementation`

In Myndr, tools the Notion server marks read-only run without asking. Every other tool asks for approval by
default, including every tool that changes your workspace: creating or updating pages, databases and comments.
You can set any tool to run without asking for a particular agent.

## Skill provenance

The four skills are vendored from Notion's `makenotion/notion-cookbook`, directory `skills/claude/`, at commit
`3437b6eb85bb8401ce1dad0330c576acbfb3a7a8` (https://github.com/makenotion/notion-cookbook/tree/3437b6eb85bb8401ce1dad0330c576acbfb3a7a8/skills/claude),
under the MIT license, Copyright (c) 2025 Notion. Each skill directory keeps a copy of that `LICENSE`.

Myndr's only edits:
1. Each `SKILL.md` frontmatter `name` drops its `notion-` prefix so it matches its directory (Agent Skills rule):
   `notion-knowledge-capture` → `knowledge-capture`, and likewise for the other three.
2. Tool references change from Claude.ai's connector-qualified form (the `Notion` connector name, a colon, then
   `notion-<tool>`) to Myndr's `notion__notion-<tool>` in every file of the four skills (157 occurrences).

All other text is Notion's, unchanged.

## Logo and trademark

`assets/icon.svg` and `assets/icon-dark.svg` are the official Notion mark from Notion's media kit
(https://www.notion.so/Media-Kit-205535b1d9c4440497a3d7a2ac096286, `NotionLogoFiles.zip`,
`notion-logo-block-main.svg`), unmodified. `icon-dark.svg` is a byte copy of `icon.svg`: the standard mark
carries its own white tile, which reads on dark backgrounds.

Trademark: Notion and the Notion logo are trademarks of Notion Labs, Inc. They are used here only to identify
the service this plugin connects to; this plugin is not made, sponsored or endorsed by Notion Labs. The logo
files are not covered by this repository's MIT license.
