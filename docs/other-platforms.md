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
> - Without a subscription → the platform *does not store* the skill, so
>   you paste the content into each prompt. The skill content is plain
>   Markdown, so it pastes cleanly.
> - The `.skill` file is just a ZIP — only Claude Code and skills.sh-aware
>   tools read it natively. For other platforms, you only need the
>   `SKILL.md` content (or any of the `references/*.md` files you want
>   to expose to the model).

## Claude (web interface — claude.ai) — Claude Pro, Team, or Enterprise

Claude web has **Projects**, which let you store a long system prompt and
a bag of uploaded files that apply to every conversation in the project.

**With subscription (recommended):**

1. Open [claude.ai](https://claude.ai) → **Projects** → **New project**.
2. Name it e.g. *MediaWiki Article Creator*.
3. In the **Project instructions** field, paste the entire content of
   `SKILL.md` (everything after the `---` frontmatter closing fence).
4. Optionally upload one or more `references/*.md` files in the **Project
   knowledge** panel.
5. Open a new chat inside the project — Claude now follows the skill
   rules automatically.

**Without subscription (ad-hoc):**

1. Download `skills/mediawiki-article-creator.skill` from this repo (or
   open `SKILL.md` directly on GitHub).
2. Copy the Markdown body of `SKILL.md` into your clipboard.
3. In a new chat, paste it as the first message prefixed with *"Follow
   these instructions for the rest of this conversation:"*.
4. The skill is active for that chat only. Re-paste per session.

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
Custom GPT, but the skill content can still be pasted into any chat.

**With subscription (Plus / Team / Enterprise):**

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

**Without subscription (Free tier):**

1. Open `SKILL.md` on GitHub and copy its Markdown body.
2. Start a new chat and paste it as the first message:
   > *You are an expert MediaWiki article author. Follow these rules for
   > the rest of this conversation:*
   >
   > *<paste SKILL.md body here>*
3. Re-paste at the start of every new conversation — ChatGPT free does
   not persist instructions across chats.

## Google Gemini (gemini.google.com) — Gemini Advanced / Workspace

Gemini has **Gems** (premium) and ad-hoc chats. Free users can paste
content into a chat but cannot save a Gem.

**With subscription (Gemini Advanced / Workspace):**

1. Open [gemini.google.com/gems/create](https://gemini.google.com/gems/create).
2. In **Instructions**, paste the body of `SKILL.md`. Gems accept long
   instructions — a full SKILL.md fits.
3. (Optional) Under **Knowledge**, upload up to 10 files (PDF, DOCX, TXT,
   MD) — the limit per the
   [Nov 2024 Workspace update](https://workspaceupdates.googleblog.com/2024/11/upload-google-docs-and-other-file-types-to-gems.html).
4. Save the Gem. It is now available in the Gems sidebar for any chat.

**Without subscription (Free tier):**

1. Download or copy `SKILL.md` from this repo.
2. Start a new chat, paste the SKILL.md body as the first user message
   with a brief preamble.
3. Re-paste per chat. Gems (saved instructions) are not available on the
   free tier.

## File format: `.skill` vs `.zip` vs raw `.md`

| Format | What it is | When to use |
|--------|-----------|-------------|
| `SKILL.md` (raw) | The skill's instruction file, plain Markdown | **Best for pasting into a prompt** on any platform. |
| `.skill` | A ZIP archive with `SKILL.md` and bundled `references/` | **Best for Claude Code / skills.sh** — `npx skills add` understands it. |
| `.zip` | Plain ZIP with the same contents | **Best for ChatGPT knowledge upload** — re-zip if you unzipped the `.skill`. |

In short: for the platforms above, you almost always want the **raw
`SKILL.md` content** in the prompt, and optionally a few `references/*.md`
files uploaded to the platform's knowledge panel. The `.skill` / `.zip`
format is only meaningful for Claude Code and tools that speak the Agent
Skills protocol.

## Recommended prompt preamble

When pasting `SKILL.md` into any platform without a dedicated skill slot,
use this template to keep behaviour stable:

```text
You are an expert MediaWiki author and editor. From now on, follow the
rules and workflows in the document below. Load the references if you need
detailed syntax, design patterns, or conversion playbooks.

<PASTE THE FULL CONTENT OF SKILL.md HERE>

Acknowledge by listing the two workflows you will follow, and wait for my
first task.
```

This preamble primes the model to treat the pasted content as a persistent
system-prompt override rather than a one-off question.

---

**Next:** [Skill format →](skill-format.md) · [Back to README →](../README.md)
