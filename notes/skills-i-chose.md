# Skills I chose

Date checked: 2026-11-04
Searches run: `email marketing`, `email sequence`, `campaign plan`, `resend`
Nothing was installed.

## Use these

**1. Anthropic's official marketing plugin — `anthropics/knowledge-work-plugins`**

For planning and writing campaigns. It is published by Anthropic itself and the repository
has about **34.7K GitHub stars**. The `marketing` plugin already contains the exact
workflows this studio needs: `campaign-plan`, `email-sequence`, `brand-review`,
`content-creation` and `performance-report`. Because it is first-party, the instructions and
the install command are the ones the tool maker supports, and the plugin is built to be
installed per project.

- Skills inside: `brand-review`, `campaign-plan`, `competitive-brief`, `content-creation`,
  `draft-content`, `email-sequence`, `performance-report`, `seo-audit`
- Why: official publisher, large public repo, covers both the plan and the writing steps.

**2. Resend's own skills — `resend/resend-skills`**

For sending email through Resend. It is published by Resend, the email service this studio
connects to, and has about **24.2K installs**. The star count is small (about **210**), but
that is expected for a young vendor repository, and provenance matters more than popularity
here: this is the company that owns the API and the MCP server we use. The repository also
ships `agent-email-inbox`, `resend-cli` and the related React Email skills.

- Why: first-party vendor, matches our MCP server, documents the test addresses
  (`@resend.dev`) we are allowed to use.

**Worth knowing:** `coreyhaines31/marketingskills` also looked strong — about **58.3K stars**,
over 80K installs on its `emails` skill, and a clean Snyk/Socket audit. The popular
`email-sequence` skill from that repository has been **merged into a single skill called
`emails`**, so searching for "email sequence" now returns `emails`. It is a good backup for
copywriting help, but it is a third-party repo, not the vendor or the tool maker.

## Avoid these

**1. `mbfinotti/advertising-skills` — mainly `ad-campaign-consolidation`**

About **25.1K installs** but only **2 GitHub stars**, from a single personal account. The
skill itself is about paid advertising accounts (merging ad campaigns), not email marketing,
so even if the text is fine it is the wrong tool for this job. High install numbers with a
nearly empty repository is the pattern to be suspicious of: the download counter can be
inflated, copied, or boosted, and there is no community review behind it.

**2. `open.feishu.cn@lark-mail`**

It shows about **849.8K installs** — one of the biggest numbers in the email searches — but
the publisher is a website domain, not a GitHub account, so there is no repository and no
star count I can check. When I tried to open its skills.sh page it returned **404**, so the
listing could not be verified at all. It is also a mail-client integration for Lark, not a
campaign-writing or sending skill. Big number, no verifiable owner, wrong category: skip it.

## How I decided

Three checks before installing any skill:

1. **Who publishes it.** Official first (`anthropics`, `resend`), or a known author with a
   real public repository. Prefer a company or person whose name I can trace.
2. **Does the repository back up the install count.** Compare installs with GitHub stars,
   repository age, and the security audits shown on the skills.sh page (Socket, Snyk, Gen
   Agent Trust Hub). Thousands of installs with a handful of stars is a warning, not a
   trophy.
3. **Does it fit, and what does it do.** Read the `SKILL.md` before installing: does it
   actually cover email campaigns, and does it ask for network access, credentials, or file
   writes it does not need. A popular skill can still be the wrong skill.
