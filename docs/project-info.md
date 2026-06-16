# Project info

Versioning, contributing, license, and acknowledgements.

## 🔢 Versioning

This project uses [Semantic Versioning](https://semver.org/) —
`MAJOR.MINOR.PATCH`.

- **MAJOR** — breaking changes to the skill structure or workflow
- **MINOR** — new features (new reference, new workflow step, new example)
- **PATCH** — bug fixes, typo fixes, small clarifications

Current version: **1.0.0** (initial release)

## 🤝 Contributing

Contributions are welcome! Please open an issue or a pull request.

When adding a new reference file:

- Use the same Markdown style as the existing files
- Include a table of contents at the top if the file is longer than 300
  lines
- Link the new file from `SKILL.md` in the "Referenced files" section
- Run the skill-creator validation before submitting

When fixing a bug:

- Describe the bug and the fix in the PR description
- Add an example to `references/03-conversion-playbooks.md` if it's a
  conversion edge case

## 📄 License

This project is licensed under the [MIT License](../LICENSE)
(Copyright © 2026 EggProject).

The MIT License grants **every person** who obtains a copy of this
software the right to **use, copy, modify, merge, publish, distribute,
sublicense, and/or sell** copies of it, free of charge. The only
condition is that the copyright notice and permission notice must be
preserved in all copies or substantial portions of the software, and
the software is provided **"as is"** without warranty of any kind — see
the full text in [`LICENSE`](../LICENSE) for details.

## 🙏 Acknowledgements

This skill was built with the official
[Claude skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) —
a meta-skill for designing, testing, and packaging Claude Code skills.

- The [MediaWiki community](https://www.mediawiki.org/wiki/Community) —
  for the excellent documentation that this skill distills
- The [Claude Code team](https://docs.claude.com/en/docs/claude-code) —
  for the skills system that makes this possible
- The
  [official Claude skill-creator plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) —
  for the meta-skill workflow that designed and packaged this one
- [Pandoc](https://pandoc.org/),
  [OpenOffice](https://www.openoffice.org/) — for the conversion tools
  the skill recommends

---

**Back to:** [README →](../README.md)
