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
| [signal-based-inputs-outputs](skills/angular/core/signal-based-inputs-outputs.md) | Signal-based component inputs and outputs | v17.3+ | When trying to communicate between components |

### Angular Forms

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|
| [reactive-forms](skills/angular/forms/README.md) | Reactive Forms patterns | v15+ | When implementing reactive forms |
| signal-forms | Signal-based Forms | v21+ | When using signal forms *(Coming Soon)* |

### Angular Migrations

#### Pattern Migrations

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|
| ng-module-to-standalone | Migrate to Standalone APIs | v15+ | When converting NgModules *(Coming Soon)* |
| reactive-forms-to-signal-forms | Migrate to Signal Forms | v21+ | When converting forms *(Coming Soon)* |

#### Version Upgrades

| Skill | Description | Versions | Trigger |
|-------|-------------|----------|---------|
| v15-to-v16 | Angular v15 → v16 upgrade | v15→v16 | When upgrading to v16 *(Coming Soon)* |

### NgRx

*Coming soon* - Be the first to contribute NgRx state management skills!

---

## How Skills Work

Each skill contains:

1. **Version Support** - Clear indication of which Angular versions are supported
2. **Trigger Conditions** - When the AI should load this skill
3. **Critical Patterns** - Specific Angular conventions to follow (labeled by version)
4. **Code Examples** - Complete, runnable TypeScript/Angular code
5. **Common Mistakes** - Anti-patterns to avoid
6. **Migration Paths** - How to upgrade from older patterns

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

```
skills/
├── angular/
│   ├── core/              # Core Angular features
│   ├── forms/             # Forms patterns
│   ├── router/            # Router patterns
│   ├── http/              # HTTP patterns
│   ├── performance/       # Optimization
│   ├── testing/           # Testing strategies
│   └── migrations/        # Migration guides
│       ├── pattern-migrations/    # Feature-to-feature
│       └── version-upgrades/      # Version-to-version
├── ngrx/                  # NgRx state management
├── rxjs/                  # RxJS patterns
└── skill-creator/         # How to create skills
```

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
