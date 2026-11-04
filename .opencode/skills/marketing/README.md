# marketing (vendored skills)

The skills in this folder are copied from Anthropic's official
[`anthropics/knowledge-work-plugins`](https://github.com/anthropics/knowledge-work-plugins)
repository, `marketing` plugin, version 1.2.0 (Apache-2.0, see `LICENSE`).

Why vendored: that repository ships a Claude Code plugin (`.claude-plugin/plugin.json`),
which OpenCode cannot install. Copying the `SKILL.md` files into `.opencode/skills/` keeps
the same workflows, makes them project-scoped, and commits them so anyone who clones this
repo gets them too.

Changes made on import:

- `../../CONNECTORS.md` links rewritten to `../CONNECTORS.md` to match the new folder depth.
- Nothing else. Skill bodies and frontmatter are unchanged.

Skills: `brand-review`, `campaign-plan`, `competitive-brief`, `content-creation`,
`draft-content`, `email-sequence`, `performance-report`, `seo-audit`.

Slash commands that load them live in `.opencode/commands/marketing/`.

To update, re-copy from the upstream `marketing/skills/` folder and re-apply the link fix.
