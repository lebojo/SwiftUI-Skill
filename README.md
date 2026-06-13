# SwiftUI-Skill

My take on creating projects in SwiftUI (Personal opinion), packaged as a reusable
[Agent Skill](https://docs.claude.com/en/docs/claude-code/skills) for Claude Code and other AI assistants.

It defines architectural standards for native SwiftUI projects: directory layout,
SwiftData persistence, state management with `@Observable`, and `.xcassets` resource conventions.

## Installation

Copy (or symlink) the skill folder into a skills directory:

```bash
# Available in all projects
cp -R swiftui-architecture ~/.claude/skills/

# Or scoped to a single project
cp -R swiftui-architecture /path/to/project/.claude/skills/
```

The skill loads automatically when you ask an AI to create, structure, or review a SwiftUI app.

## Structure

```
swiftui-architecture/
└── SKILL.md    # the skill (YAML frontmatter + guidelines)
```
