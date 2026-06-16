# Using this skill on other AI platforms

This skill is built around the open
[Agent Skills specification](https://agentskills.io/specification), so it
can be used — at least in part — on any AI assistant that lets you inject
a system prompt or a long instruction block. Below is how to use it on the
most common platforms.

> **Quick orientation:**
>
> - With a subscription → the platform *stores* the skill and applies it
>   automatically to every conversation.
> - Without a subscription → see the dedicated [Free-tier usage](#free-tier-usage-no-subscription-no-storage)
>   section below — the same approach works on any platform.

## Claude (web interface — claude.ai) — Claude Pro, Team, or Enterprise

Claude web has **Projects**, which let you store a long system prompt and
a bag of uploaded files that apply to every conversation in the project.

1. Open [claude.ai](https://claude.ai) → **Projects** → **New project**.
2. Name it e.g. *MediaWiki Article Creator*.
3. In the **Project instructions** field, paste the entire content of
   `SKILL.md` (everything after the `---` frontmatter closing fence).
4. Optionally upload one or more `references/*.md` files in the **Project
   knowledge** panel.
5. Open a new chat inside the project — Claude now follows the skill
   rules automatically.

## Claude Cowork

Claude Cowork is Anthropic's agentic desktop product. Skills install the
same way as in Claude Code:

```bash
# Install for Cowork
npx skills add eggp/easter-skill-media-wiki-converter \
  --skill mediawiki-article-creator -a claude-code
```

In Cowork, you can also drop the unzipped skill folder directly into the
skills directory the app watches (see Cowork's docs for the exact path
per OS). After a restart, the skill is available as an invokable
workflow.

## ChatGPT (OpenAI) — Plus, Team, or Enterprise

ChatGPT has **Custom GPTs** for subscribers. Free users cannot create a
Custom GPT — see the [free-tier section](#free-tier-usage-no-subscription-no-storage)
below for the no-storage workaround.

1. Open [chatgpt.com](https://chatgpt.com) → **Explore GPTs** → **Create**.
2. In **Configure** → **Instructions**, paste the body of `SKILL.md`.
3. (Optional) In **Knowledge**, upload `SKILL.md` and any
   `references/*.md` files you want the GPT to consult. ChatGPT accepts
   PDF, TXT, and Markdown; uploading a `.zip` works for grouping many
   files at once but counts as a single knowledge entry.
4. Save as *Only me* (or share).

> ⚠️ **File-format note:** ChatGPT does *not* read Claude's `.skill` zip
> format. Upload either the individual `.md` files or a plain `.zip`
> containing them. The frontend unzips the archive on upload and indexes
> the text content.

## Google Gemini (gemini.google.com) — Gemini Advanced / Workspace

Gemini has **Gems** (premium) and ad-hoc chats. Free users can paste
content into a chat but cannot save a Gem — see the
[free-tier section](#free-tier-usage-no-subscription-no-storage) below.

1. Open [gemini.google.com/gems/create](https://gemini.google.com/gems/create).
2. In **Instructions**, paste the body of `SKILL.md`. Gems accept long
   instructions — a full SKILL.md fits.
3. (Optional) Under **Knowledge**, upload up to 10 files (PDF, DOCX, TXT,
   MD) — the limit per the
   [Nov 2024 Workspace update](https://workspaceupdates.googleblog.com/2024/11/upload-google-docs-and-other-file-types-to-gems.html).
4. Save the Gem. It is now available in the Gems sidebar for any chat.

## Free-tier usage (no subscription, no storage)

All of the above platforms offer a usable path even on the free tier —
they just don't store the skill for you, so you point the model at the
skill on every new chat. The recipe is the same everywhere: **let the
LLM fetch the skill itself from GitHub.** You don't need to download
anything.

1. **Open a new chat** on the platform of your choice (ChatGPT, Claude
   web, Gemini, or any other AI chat UI that supports tool use or URL
   fetching).
2. **Paste this prompt** as your first message — the model does the
   rest:

   ```text
   Load the MediaWiki Article Creator skill from this public GitHub
   repository and behave as if it were your active system-prompt
   override for the rest of this conversation:

     https://github.com/EggProject/easter-skill-media-wiki-converter

   Concretely:
   1. Fetch the raw SKILL.md from
      https://raw.githubusercontent.com/EggProject/easter-skill-media-wiki-converter/main/skills/mediawiki-article-creator/SKILL.md
      and read it in full.
   2. From now on, follow every rule and workflow defined in that
      SKILL.md. If you need detailed syntax, design patterns, or
      conversion playbooks, fetch the relevant file from the
      references/ folder on demand (e.g. .../references/01-syntax-cheatsheet.md,
      .../references/02-design-patterns.md, etc.).
   3. Acknowledge by listing the two workflows you will follow
      (parallel iterative authoring; format conversion with independent
      verification), and wait for my first task.
   ```

3. **Re-paste per chat.** Free-tier chats don't persist instructions
   across conversations, so you repeat step 2 at the start of every
   new session. (The model re-fetches the same URL each time — the
   file is small, ~5 KB.)

This approach works on ChatGPT free, Claude.ai free, Gemini free, and
any other AI chat UI that gives the model URL-fetching or web-browsing
tools — no platform account upgrade required, and no manual download
needed.

> 💡 **If the platform blocks outbound URL fetches** (rare on modern
> chat UIs, but possible on some offline or sandboxed setups), the
> fallback is to download
> [`SKILL.md`](../skills/mediawiki-article-creator/SKILL.md) manually
> from GitHub and paste its content directly into the first message
> instead. The Markdown is plain text, so it pastes cleanly.

## File format: `.skill` vs `.zip` vs raw `.md`

| Format | What it is | When to use |
|--------|-----------|-------------|
| `SKILL.md` (raw) | The skill's instruction file, plain Markdown | **Best for free-tier** — fetched by the model from GitHub, or pasted as a fallback. |
| `.skill` | A ZIP archive with `SKILL.md` and bundled `references/` | **Best for Claude Code / skills.sh** — `npx skills add` understands it. |
| `.zip` | Plain ZIP with the same contents | **Best for ChatGPT knowledge upload** — re-zip if you unzipped the `.skill`. |

In short: for the platforms above, you almost always want the **raw
`SKILL.md` content** — fetched from GitHub by the model (free tier) or
in the project/Gem instructions (premium). The `.skill` / `.zip`
format is only meaningful for Claude Code and tools that speak the
Agent Skills protocol.

---

**Next:** [Skill format →](skill-format.md) · [Back to README →](../README.md)
