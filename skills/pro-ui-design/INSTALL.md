# pro-ui-design — Installation

## Claude Code CLI

### Option 1: Project-level skill (recommended)
```bash
# In your project root
mkdir -p .claude/skills
tar -xzf pro-ui-design.tar.gz -C .claude/skills/
```

The skill folder ends up at `.claude/skills/pro-ui-design/SKILL.md`
and Claude Code will automatically discover and use it.

### Option 2: Global skill (available across all projects)
```bash
mkdir -p ~/.claude/skills
tar -xzf pro-ui-design.tar.gz -C ~/.claude/skills/
```

### Option 3: Manual copy
Just copy the `pro-ui-design/` folder (with SKILL.md + references/) 
into your project's `.claude/skills/` directory.

## Claude.ai (web)
Use the `.skill` file — it's a zip that can be uploaded directly.

## What's inside

```
pro-ui-design/
├── SKILL.md                          # Main orchestrator
└── references/
    ├── typography.md                 # Font selection, pairing, scale
    ├── anti-patterns.md              # Defaults to avoid (intention-based)
    ├── boldness.md                   # When and how to be daring
    ├── mobile.md                     # Real mobile design, not stacked desktop
    ├── layout-systems.md             # Grid systems, section rhythm
    ├── color-and-surface.md          # Palettes, texture, depth
    ├── motion-and-interaction.md     # Animation, hover, easing
    ├── design-dimensions.md          # The axis system for unique designs
    └── design-critic.md              # Self-review rubric
```
