# Angular Agent Skills

> Community-maintained collection of Angular skills for AI agents like Claude Code, OpenCode, and other AI assistants.

Skills are specialized instruction sets that teach AI assistants how to work with Angular across different versions, following best practices and modern patterns.

## Philosophy

- **Version-Aware** - Every skill clearly states which Angular versions it supports (e.g., v16+, v17+)
- **Migration-Focused** - Dedicated skills for upgrading between Angular versions and migrating patterns
- **Community-Driven** - Open for contributions with a democratic voting process
- **Agent-Ready** - Structured following the [Agent Skills](https://agentskills.io/) format

---

## Available Skills

> **Start here!** Use the [skill-creator](skills/skill-creator/SKILL.md) skill to create proper Angular skills.

### Meta Skills

| Skill | Description | Trigger |
|-------|-------------|---------|
| [skill-creator](skills/skill-creator/SKILL.md) | Create new Angular AI agent skills | When creating new skills or documenting Angular patterns |

### Angular Core

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|
| [angular-component-inputs](skills/angular-component-inputs/SKILL.md) | Signal-based component inputs and models | v17.3+ | When defining component inputs or two-way binding |

### Angular Forms

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|
| [angular-forms](skills/angular-forms/SKILL.md) | Reactive Forms patterns | v15+ | When implementing reactive forms *(Coming Soon)* |
| angular-signal-forms | Signal-based Forms | v21+ | When using signal forms *(Coming Soon)* |

### Angular Migrations

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|  
| angular-standalone-migration | Migrate to Standalone APIs | v15+ | When converting NgModules *(Coming Soon)* |
| angular-signal-forms-migration | Migrate to Signal Forms | v21+ | When converting forms *(Coming Soon)* |
| angular-v15-to-v16 | Angular v15 → v16 upgrade | v15→v16 | When upgrading to v16 *(Coming Soon)* |

### State Management

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|  
| ngrx-store | NgRx state management | All | When implementing NgRx *(Coming Soon)* |
| ngrx-effects | NgRx side effects | All | When handling side effects *(Coming Soon)* |

### RxJS

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|  
| rxjs-operators | RxJS operators and patterns | All | When working with observables *(Coming Soon)* |

---

## How Skills Work

Each skill contains:

1. **Version Support** - Clear indication of which Angular versions are supported
2. **Trigger Conditions** - When the AI should load this skill
3. **Critical Patterns** - Specific Angular conventions to follow (labeled by version)
4. **Quick Reference** - Concise cheat sheet for common tasks
5. **References Folder** (optional) - Extended examples, common mistakes, and advanced patterns

The main skill file stays concise and focused, with detailed patterns in a separate `references/` folder when needed.

When the AI detects a matching context (e.g., working with Angular forms), it reads the skill file and applies those patterns to its responses.

---

## Contributing

We welcome community contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for the full process.

> **Pro tip**: Load the [skill-creator](skills/skill-creator/SKILL.md) skill to create Angular skills that follow our conventions!

### Quick Overview

1. **Fork** this repository
2. **Create** your skill following the [skill template](template/SKILL_TEMPLATE.md)
3. **Submit** a Pull Request using [conventional commits](https://www.conventionalcommits.org/)
4. **Maintainer reviews** and provides feedback
5. **Merge** once approved (community input may be requested for complex cases)

---

## Skill Structure

Skills are organized in a flat structure with descriptive names:

```
skills/
├── angular-component-inputs/     # Signal-based inputs and models
├── angular-forms/                # Reactive Forms patterns
├── angular-signal-forms/         # Signal-based Forms (v21+)
├── angular-router/               # Router patterns and guards
├── angular-http/                 # HttpClient patterns
├── angular-standalone/           # Standalone components migration
├── ngrx-store/                   # NgRx state management
├── rxjs-operators/               # RxJS patterns
└── skill-creator/                # How to create skills
```

### Individual Skill Structure

Each skill follows this pattern:

```
skill-name/
├── SKILL.md              # Main skill file (concise, critical patterns)
└── references/           # Optional: Extended patterns and examples
    └── patterns.md       # Real-world examples, common mistakes, advanced patterns
```

The main `SKILL.md` should be concise and focus on critical patterns. Extended examples, common mistakes, and advanced patterns should be moved to the `references/` folder to keep the skill focused and easy to scan.

**Naming Convention:** Use `angular-feature` format for core Angular skills (e.g., `angular-forms`, `angular-router`). For libraries, use `library-feature` format (e.g., `ngrx-store`, `rxjs-operators`).

---

## Angular Version Support

| Version | Status | Key Features | Notes |
|---------|--------|--------------|-------|
| v15 | ✅ Supported | Standalone APIs stable | Minimum supported version |
| v16 | ✅ Supported | Signals introduced | New reactivity model |
| v17 | ✅ Supported | Built-in control flow, @defer | Modern template syntax |
| v18 | ✅ Supported | Zoneless preview, Signal Forms | Performance improvements |
| v19 | ✅ Supported | Signal inputs stable | Enhanced component authoring |
| v20 | ✅ Supported | Latest stable | Current stable release |
| v21 | 🚧 Development | Future features | Upcoming version |

---

## Other Resources

- **Angular Documentation**: [angular.dev](https://angular.dev)
- **Angular Update Guide**: [update.angular.io](https://update.angular.io)
- **Agent Skills Format**: [agentskills.io](https://agentskills.io/)
- **Skills Registry**: See [AGENTS.md](AGENTS.md) for the complete list

---

## Acknowledgments

This project is based on the [Gentleman-Skills](https://github.com/Gentleman-Programming/Gentleman-Skills) repository created by [Alan Buscaglia (Gentleman Programming)](https://github.com/Gentleman-Programming). The skill-creator and repository structure follow the patterns established in that project.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
