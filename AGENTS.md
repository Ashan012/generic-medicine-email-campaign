# AGENTS.md — generic-medicine-email-campaign

Yeh file is project ke rules hain. OpenCode ise har session mein khud parhta hai, is liye
har rule yahan likha hai — chat mein dobara samjhane ki zaroorat nahi.

---

## 1. Zubaan (sab se pehla rule)

- **Baat-cheet Roman Urdu mein.** Technical terms English mein rakho (skill, hook, MCP,
  commit, session), kyunki unka tarjuma confusion banata hai.
- Files, code, commit messages, aur prompts **English** mein — taake tooling aur Claude
  Code exam ke terms match rahein.
- Chat chhoti rakho. Lambi explanation sirf jab main poochhun.

## 2. Hum Claude Code par nahi, OpenCode par chal rahe hain

`assignment.md` Claude Code ke liye likha gaya hai. Hum wahi course **OpenCode V2** par
kar rahe hain. Is liye har step par yeh tariqa:

1. **Pehle scan karo.** Prompt mein jo cheez Claude Code specific hai (file names, slash
   commands, CLI flags, folder names) usay pehchano.
2. **Equivalent dhoondo.** Neeche mapping table se OpenCode ka possible option nikalo.
   Agar docs mein clear answer na ho to guess mat karo — mujhse poocho.
3. **Batao kya badla.** Build shuru karne se pehle 2–4 lines mein likho:
   - kaunsa Claude Code cheez thi
   - OpenCode mein kya use kar rahe hain
   - kya koi feature missing hai
4. **Invent mat karo.** Aisa flag, file ya feature mat banao jo OpenCode mein mojood nahi.
   Missing cheez ko "nearest equivalent + limitation" ke saath handle karo.
5. **Exam ke liye alag rakho.** Agar main CCAR-F exam ke baare mein poochhun to jawab
   Claude Code ki terminology mein do. Implementation OpenCode mein hogi, lekin exam
   answer Claude Code wala hi rahega. Dono ko mix mat karo.

### Mapping table: Claude Code → OpenCode V2

| Claude Code | OpenCode V2 mein | Notes |
| --- | --- | --- |
| `CLAUDE.md` | `AGENTS.md` | V2 `CLAUDE.md` ko fallback ke tor par bhi nahi parhta |
| Subfolder ka `CLAUDE.md` | Us folder ka `AGENTS.md` | Folder explore/read hone par load hota hai |
| `@brand/voice.md` import | `opencode.json` ka `instructions` array | Koi `@import` directive nahi: `"instructions": ["brand/voice.md"]` |
| `.claude/rules/*.md` + `paths:` | Koi direct equivalent nahi | Nearest: nested `AGENTS.md`, ya `instructions` mein glob. Missing feature clearly batao |
| `/memory`, `/context` | `opencode debug config` + `AGENTS.md` chain | Loaded sources inspect karne ka tareeqa |
| Plugin marketplace + install | `plugins` config, `opencode plugin add` | Anthropic ka marketing plugin yahan nahi chalta; uske workflows ko local commands/skills banao |
| `/marketing:campaign-plan` | `.opencode/commands/campaign-plan.md` | `$ARGUMENTS` + frontmatter `agent:` |
| `.claude/skills/<id>/SKILL.md` | `.opencode/skills/<id>/SKILL.md` | OpenCode `.claude/skills` bhi parhta hai (compat), magar hum native `.opencode` use karenge |
| Skill frontmatter: `argument-hint` | Command frontmatter ya `$ARGUMENTS` | Skill mein direct equivalent nahi |
| Skill frontmatter: `context: fork` | Command mein `subagent: true` | Isolated child session; sirf summary wapas aati hai |
| Skill frontmatter: `allowed-tools` | Agent ke `permissions` | Skill-level tool allowlist nahi hai |
| `.claude/settings.json` permissions | `opencode.json` ka `permissions` | `action` / `resource` / `effect` |
| `.claude/settings.local.json` | Global `~/.config/opencode/opencode.json` | Project config sab ke saath share hota hai |
| `PreToolUse` hook (`.mjs`, exit 2) | Plugin `.opencode/plugins/<name>/index.ts` → `ctx.tool.hook("execute.before")` | Throw karke block karo |
| Hard, non-bypassable block | `policies` → `{ "action": "tool.use", "resource": "<tool>:*", "effect": "deny" }` | "Blocked by configuration policy" |
| `.mcp.json` | `opencode.json` → `mcp.servers.<name>` | `type: "local"`, `command` array |
| `${VAR:-default}` | Sirf `{env:VAR}` | Default value config mein literal likho; asli secret env se |
| `/mcp` | `/mcps` | Server status / OAuth |
| `claude -p ... --json-schema` | `opencode run --format json` + script mein schema validate | `--json-schema` mojood nahi |
| `--allowedTools` | Agent `permissions` | Headless run ke liye dedicated agent banao |
| `claude -n <name>` | `--title`, ya `/sessions` mein rename | Named-session flag nahi |
| `claude --resume <name>` | `opencode run --session <id>` / `--continue` / `/sessions` | Resume ID se hota hai |
| `--fork-session -n <name>` | `opencode run --session <id> --fork --title <name>` | API: `POST /api/session/{sessionID}/fork` |
| `/compact` | Auto compaction; manual `POST /api/session/{id}/compact` | `/compact` slash command nahi |
| Plan mode (`Shift+Tab`) | `plan` agent: `Shift+Tab`, `/agents`, ya `--agent plan` | Same concept |
| Subagent `.claude/agents/*.md` | `.opencode/agents/*.md` with `mode: subagent` | |
| `/exit` phir `claude` | `/new` | Fresh session = honest check |
| Glob / Grep / Read / Write | `glob` / `grep` / `read` / `write` | Tool names lowercase |

> Version note: yeh mapping OpenCode **v2.0.28** ke docs se verify ki gayi hai. Naya
> version aaye to pehle docs check karo, phir table update karo.

## 3. Har step ka workflow

`assignment.md` ke 15 steps, 3 parts. Har step par:

1. **Step parho** — assignment.md se, poora (Build + Check + "You'll know it worked when").
2. **Mapping lagao** — section 2 wala process. Chhota "kya convert hua" note do.
3. **BUILD** — OpenCode mein karo. Files `opencode.json`, `AGENTS.md`, `.opencode/` ke
   andar banengi; `.claude/`, `.mcp.json`, `CLAUDE.md` **nahi**.
4. **Commit** — har step ke end par chhota, clear English commit.
5. **CHECK** — naye session (`/new`) mein, taake checker ne khud nahi banaya.
   Check prompt ko bhi OpenCode ke hisaab se adapt karo (e.g. "Use Glob" waise hi rehta
   hai, magar `claude plugin list` jaisi cheezein badal do).
6. **Tick** — `assignment.md` ke Progress tracker mein checkbox tick karo aur neeche wala
   count update karo (`N of 15 done`).

Ek waqt mein **ek step**. Agla step tab jab current step ka check pass ho jaye.

## 4. Project ka final shape (OpenCode flavour)

```
generic-medicine-email-campaign/
├── AGENTS.md                  is project ke rules (yeh file)
├── assignment.md              course / assignment
├── opencode.json              shared project config (MCP, permissions, commands)
├── brand/voice.md
├── leads/
│   ├── leads.csv
│   └── AGENTS.md              sirf is folder ke rules
├── campaigns/steep-club-launch/
│   ├── plan.md
│   ├── emails/
│   └── groups/
├── .opencode/
│   ├── agents/email-reviewer.md
│   ├── commands/              campaign-plan, email-sequence, brand-review
│   ├── skills/personalize/SKILL.md
│   └── plugins/send-guard/index.ts
├── review/findings.schema.json
└── scripts/review.mjs
```

Original course ka `.claude/`, `.mcp.json`, `CLAUDE.md` structure **copy nahi** karna.
Concept same hai, naam OpenCode ke.

## 5. Safety rules (non-negotiable)

- **Kabhi real email nahi.** Sirf `@resend.dev` test addresses ya mera apna email.
- Leads sirf `@example.com` par. Koi real naam/email add nahi.
- Resend practice server: `http://localhost:4000` (`npx emulate@0.12.3 start --service resend`).
- **Koi secret repo mein nahi.** Real key sirf environment variable se, config mein
  `{env:RESEND_API_KEY}`. Commit se pehle `git diff` check karo.
- `opencode.json` committed hai (team ke liye). Personal cheezein global config mein.
- Delete se pehle **poocho**. Bulk delete ya `rm -rf` kabhi khud se nahi.
- Guard ko bypass karne ki koshish mat karo — na main karunga, na tum.

## 6. Git

- Har step ke baad commit. Message English, ek line, kya hua.
- Commit se pehle: `git status` aur `git diff` dekhna.
- `.gitignore` mein system files (`.DS_Store`, `Thumbs.db`) aur local personal config.
- `git init` pehle hi ho chuka hai (ya kar lenge step 2 par).

## 7. Verify karo, maano mat

- "Measure, don't eyeball": character counts, link counts, file counts shell/Node se lo.
- Check prompts ke liye naya session lazmi — same session apne kaam ko defend karta hai.
- Koi cheez kaam nahi kar rahi to error quote karo, guess se fix mat karo.
- Docs: <https://opencode.ai/v2/docs/>

## 8. Jab confusion ho

Do options ho aur farq matter kare — poocho, khud decide mat karo. Ek line ka sawal kaafi
hai: "Yahan X ya Y, kya use karun?"

## 9. The studio (studio brief)

This folder is my email studio. I use it to plan and write email campaigns for a small
business: the plan for a campaign, the emails themselves, and personalised versions for
groups of customers.

| Folder | What goes in it |
| --- | --- |
| `brand/` | The brand voice and style: words we use, words we avoid, promises we never make. |
| `leads/` | Practice lead lists and customer groups. Test data only. |
| `campaigns/` | One folder per campaign: the plan, the emails, and the groups we send to. |
| `notes/` | Research, decisions and scratch notes that are not part of a campaign. |

Rules for this studio:

- **Write in plain English.** Short sentences, no jargon, no hype.
- **Never send email to real people.** Practice sends only, to test addresses such as
  `@resend.dev` or my own inbox.
- **Ask me before deleting anything.**
