# ai-utils

A growing collection of prompts and patterns that proved useful in my own agentic workflows,
packaged so others can reuse them.

## Seeds

Seeds are **person- or project-independent** prompts. You paste a seed into Claude (Claude Code at
the root of a repo, or a Claude Project with access to your material). It discovers your situation,
proposes a design, pauses for your confirmation, and then bootstraps a utility tailored to you.
The seed is not the thing it builds — it's the prompt that builds it.

| Seed | What it bootstraps |
| --- | --- |
| [Career Wiki](seeds/career-wiki-seed.md) | A personal, LLM-maintained career knowledge base — roles, evidence, case studies, interview stories, applications and decisions — from which CVs, cover letters, a personal website and interview prep are compiled. Privacy levels (`public` / `shareable` / `private`) are part of the schema. |

### How to use the Career Wiki seed

1. Collect your career material where Claude can read it (a folder, a cloud drive, uploads).
2. Decide where the wiki will live — ideally a **private** git repository.
3. Paste everything below the line in [`seeds/career-wiki-seed.md`](seeds/career-wiki-seed.md) into
   a new session and let it run Phase 0. Confirm or adjust its proposal; it builds the rest.

The seed is a public template. **The wiki it builds is your personal data — keep that private.**

## Credits

- The Career Wiki seed adapts the structure and principles of the
  [LLM Wiki seed](https://github.com/kondfox/ai-utils/blob/main/seeds/llm-wiki-seed-prompt.md) by
  [kondfox](https://github.com/kondfox/ai-utils) from codebases to personal careers.
- Both are based on [Andrej Karpathy's LLM Wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

Feedback and ideas are welcome via issues.

## License

[MIT](LICENSE)
