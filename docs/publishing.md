# skills.sh compliance & publishing

This skill is designed to be installable through
[skills.sh](https://skills.sh), the open agent-skills ecosystem. The
discovery rules, frontmatter requirements, and validation steps are
below.

## Required frontmatter

The `SKILL.md` frontmatter is valid per the
[Agent Skills specification](https://agentskills.io/specification):

| Field | Required | Value |
|-------|----------|-------|
| `name` | yes | `mediawiki-article-creator` — must match the parent directory name |
| `description` | yes | ≤ 1024 chars, no `<` or `>`, describes what the skill does **and when to use it** |
| `license` | no | `MIT` (this project) |
| `compatibility` | no | `Designed for Claude Code (or similar products).` |
| `metadata` | no | `author: eggp`, `version: 1.0.0` |
| `allowed-tools` | no | not set (skill uses default tool set) |

## Name rules

- Lowercase letters, digits, and hyphens only
- 1-64 characters
- Must not start or end with a hyphen
- Must not contain consecutive hyphens
- **Must match the parent directory name** (the skill lives at
  `skills/mediawiki-article-creator/`, so the name is `mediawiki-article-creator`)

## Discovery paths in this repo

The skill is placed under the canonical `skills/<name>/` flat layout,
which skills.sh discovers automatically (one level deep):

```
skills/
└── mediawiki-article-creator/      ← name matches the directory
    ├── SKILL.md
    ├── references/
    ├── workspace/
    └── (no SKILL.md here, so the nested skill is picked up, not this file)
```

You could also discover it via `.claude/skills/`, `skills/.curated/`, or
plugin manifests (`.claude-plugin/marketplace.json`).

## Install command

```bash
# From this GitHub repo
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

You can also list all skills first with `--list`, install globally with
`-g`, or pin to specific agents with `-a`.

## Publishing — no submission required

Per the
[Vercel guide on agent skills](https://vercel.com/kb/guide/agent-skills-creating-installing-and-sharing-reusable-agent-context),
there is **no formal submission process** for skills.sh. To make a skill
discoverable:

1. Put it in a public GitHub repo
2. Make sure `SKILL.md` is valid (per spec above)
3. Add a `README.md` (this one is exactly that)
4. Optionally include a `LICENSE` file
5. Share the install command — installs show up on the skills.sh
   leaderboard organically via install telemetry

A skill is **installable** the moment the CLI can resolve the repo, even
before it appears on the public leaderboard.

## Validation

Validate the frontmatter locally with the
[skills-ref validator](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
npx skills-ref validate skills/mediawiki-article-creator
```

Or use the manual checklist:

- [x] `name` field present, kebab-case, ≤ 64 chars, matches directory name
- [x] `description` field present, ≤ 1024 chars, no `<` or `>`, says both
      what and when
- [x] Optional fields valid (`license`, `compatibility`, `metadata`)
- [x] No unexpected top-level keys in frontmatter
- [x] File is named `SKILL.md` (case-sensitive)

## Building the `.skill` archive

The shipped `skills/mediawiki-article-creator.skill` is a plain ZIP of
the `mediawiki-article-creator/` folder. To rebuild it after edits:

```bash
cd skills
zip -r mediawiki-article-creator.skill mediawiki-article-creator/ -x "*.DS_Store"
```

The skill-creator plugin can also package the skill for you via its
"Skill csomagolása .skill fájlba" workflow.

---

**Next:** [Project info →](project-info.md) · [Back to README →](../README.md)
