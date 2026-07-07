# python-playbook

My personal repository of best practices, guides, style rules and templates for Python development. It serves as a personal knowledge base and as a reference context.

![Python](https://img.shields.io/badge/python-3.14-blue)
![License](https://img.shields.io/github/license/JoelBuenrostro/python-playbook)
![Last Commit](https://img.shields.io/github/last-commit/JoelBuenrostro/python-playbook)
![Markdown](https://img.shields.io/badge/docs-markdown-informational)
![Repo size](https://img.shields.io/github/repo-size/JoelBuenrostro/python-playbook)
![Maintained](https://img.shields.io/badge/maintained-yes-brightgreen)
![PRs Welcome](https://img.shields.io/badge/PRs-not%20accepted-red)

## Structure

```
python-playbook/
├── index.md              # General map of all rules 
├── rules/                # Atomic rules, one idea per file
├── guides/               # Long documents, design decisions and criteria
├── snippets/             # Reusable and tested code
├── templates/            # Complete project boilerplates
└── context/              # Condensed context
```

## How to navigate

- Start with 'index.md' to see the full map.
- Each rule in 'rules/' has a unique ID (### format) and front matter with tags and references to other related rules (Zettelkasten style).
- The 'snippets/' are actual, executable code, not just illustrative examples.

## Conventions

- Every file in 'rules/' contains front matter YAML with 'id', 'title', 'tags', 'relates_to', and 'updated'.
- IDs are sequential and are never reused, even if a rule becomes deprecated (it is marked as 'status:deprecated' instead of deleted).
- The code in 'snippets/' must be executable as is, without any undocumented hidden dependencies. 