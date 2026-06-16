# Installation

Three install paths are supported, in order of recommended use.

## Option 1 — Install from skills.sh (recommended)

The skill is published on the open [skills.sh](https://skills.sh) registry.
Install with `npx skills add`:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator
```

The CLI copies or symlinks the skill into the agent's local skills directory
(`./<agent>/skills/` by default).

### Global install

Add `-g` to install once for your user, available across all your projects:

```bash
npx skills add eggp/easter-skill-media-wiki-converter --skill mediawiki-article-creator -g
```

The skill is then placed under `~/<agent>/skills/`.

### Pin to a specific agent

Use `-a` to install only for a specific agent (useful if you have multiple
installed, e.g. `claude-code`, `codex`, `opencode`, `cursor`):

```bash
npx skills add eggp/easter-skill-media-wiki-converter \
  --skill mediawiki-article-creator \
  -a claude-code
```

After installation, restart Claude Code so it picks up the new skill.

## Option 2 — Drop the `.skill` file into your project's `skills/` directory

The shipped `mediawiki-article-creator.skill` archive is a standard ZIP.

```bash
# From the root of your project
mkdir -p skills
cp /path/to/mediawiki-article-creator.skill skills/
cd skills
unzip mediawiki-article-creator.skill
```

Claude Code automatically picks up skills placed under `skills/`.

## Option 3 — Drop the unzipped folder into `.claude/skills/`

If you have already extracted the archive (or cloned the repo), you can copy
the unzipped folder directly:

```bash
mkdir -p .claude/skills
cp -r /path/to/mediawiki-article-creator .claude/skills/
```

The skill is loaded from `.claude/skills/mediawiki-article-creator/`.

## Verifying the install

After any of the three install methods, start a fresh Claude Code session
and ask:

> "Are you aware of the mediawiki-article-creator skill?"

Claude should confirm it has the skill available, and you can proceed to
the [workflows](workflows.md).

---

**Next:** [Workflows →](workflows.md) · [Other platforms →](other-platforms.md)
