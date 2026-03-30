# Pro Skills Plugin

Professional-grade skills for Claude Code. Each skill applies a senior specialist's pipeline with anti-pattern detection, step-by-step guidance, and deep reference materials.

## Skills

| Skill | What it does |
|-------|-------------|
| **pro-developer** | Production-grade code with defensive coding, error handling, and code structure review |
| **pro-ui-design** | Frontend design with typography, color systems, layout grids, motion, and design critique |
| **pro-copywriting** | Human-sounding copy with AI-tell detection, voice briefs, rhythm analysis, and Swedish support |
| **pro-seo** | SEO strategy with search intent analysis, topic clusters, E-E-A-T content, and technical audits |
| **pro-system-design** | System architecture with trade-off analysis, simplicity-first design, and failure mode review |
| **business-ideation** | Business idea generation, 5-filter screening, competitive analysis, and honest stress testing |
| **meta-prompting** | Prompt and skill design with technique reference, failure mode analysis, and testing |
| **sdlc-architect** | Full SDLC lifecycle: UX, architecture, implementation, testing, and deployment phases |

## Install

```bash
claude install github:elis132/pro-skills-plugin
```

## How it works

Each skill has a `SKILL.md` with the core pipeline and a `references/` directory with deep-dive materials. Claude loads references on demand to keep context lean.

```
skills/pro-developer/
├── SKILL.md                    # Core pipeline (~2k words)
└── references/
    ├── defensive-coding.md     # Error handling, validation, race conditions
    ├── code-structure.md       # File organization, naming, decomposition
    ├── api-patterns.md         # API design patterns
    ├── database-patterns.md    # Query patterns, migrations
    ├── react-patterns.md       # React-specific patterns
    └── code-critic.md          # Self-review checklist
```

Skills trigger automatically based on what you ask. Every coding request routes through pro-developer. Every writing request routes through pro-copywriting. And so on.

## License

MIT
