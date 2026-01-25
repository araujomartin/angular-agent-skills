# Angular Agent Skills - Skills Registry

## How to Use This Guide

- This file serves as a central registry of all available skills in this repository
- Each skill provides specialized instructions for AI agents working with Angular
- Load skills on-demand based on the task at hand
- Skills are organized by category and Angular version support

## Available Skills

### Meta Skills (Repository Management)

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| `skill-creator` | Create new Angular AI agent skills | [SKILL.md](skills/skill-creator/SKILL.md) | All |

### Angular Core

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| `angular-component-inputs` | Signal-based component inputs and models | [SKILL.md](skills/angular-component-inputs/SKILL.md) | v17.3+ |

### Angular Forms

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| `reactive-forms` | Reactive Forms patterns and best practices | [README.md](skills/angular/forms/README.md) | v15+ |
| `signal-forms` | Signal-based Forms (future) | [Coming Soon](skills/angular/forms/signal-forms/) | v18+ |

### Angular Migrations

#### Pattern Migrations

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| `ng-module-to-standalone` | Migrate from NgModule to Standalone | [Coming Soon](skills/angular/migrations/pattern-migrations/ng-module-to-standalone/) | v15+ |
| `reactive-forms-to-signal-forms` | Migrate from Reactive to Signal Forms | [Coming Soon](skills/angular/migrations/pattern-migrations/reactive-forms-to-signal-forms/) | v18+ |

#### Version Upgrades

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| `v15-to-v16` | Upgrade from Angular v15 to v16 | [Coming Soon](skills/angular/migrations/version-upgrades/v15-to-v16/) | v15→v16 |

### NgRx

| Skill | Description | Path | Versions |
|-------|-------------|------|----------|
| *Coming soon* | NgRx state management patterns | - | - |

---

## Auto-invoke Skills

When performing these actions, AI agents should ALWAYS load the corresponding skill FIRST:

| Action | Skill to Load |
|--------|---------------|
| Creating a new skill or updating skill documentation | `skill-creator` |
| Working with signal-based inputs or models | `angular-component-inputs` |
| Implementing reactive forms | `reactive-forms` |
| Migrating from NgModule to Standalone | `ng-module-to-standalone` |
| Migrating between Angular versions | Appropriate `v{x}-to-v{y}` skill |
| Converting Reactive Forms to Signal Forms | `reactive-forms-to-signal-forms` |

---

## Skill Categories Explained

### Core Skills
Angular framework fundamentals: components, directives, pipes, dependency injection, signals, etc.

### Forms Skills
All forms-related patterns: Reactive Forms, Template-Driven Forms, Signal Forms, validation, etc.

### Router Skills
Angular Router patterns: guards, resolvers, lazy loading, route configuration, etc.

### HTTP Skills
HttpClient patterns: interceptors, error handling, caching, API calls, etc.

### Performance Skills
Optimization techniques: change detection, lazy loading, OnPush strategy, defer blocks, etc.

### Testing Skills
Testing strategies: unit tests, integration tests, E2E tests, mocking, fixtures, etc.

### Migration Skills
Two types:
1. **Pattern Migrations**: Migrating from one Angular pattern to another (e.g., NgModule → Standalone)
2. **Version Upgrades**: Upgrading between Angular major versions (e.g., v18 → v19)

### NgRx Skills
State management patterns using NgRx: store, effects, selectors, entities, etc.

### RxJS Skills
Reactive programming patterns: operators, observables, subjects, best practices, etc.

---

## Version Support Matrix

| Angular Version | Status | Key Features | Notes |
|----------------|--------|--------------|-------|
| v15 | Supported | Standalone APIs stable | Minimum supported version |
| v16 | Supported | Signals introduced | New reactivity model |
| v17 | Supported | Built-in control flow, defer | Modern template syntax |
| v18 | Supported | Zoneless preview, Signal Forms | Performance & DX improvements |
| v19 | Supported | Signal inputs stable | Enhanced component authoring |
| v20 | Supported | Latest stable | Current stable release |
| v21 | Development | Future features | Upcoming version |

---

## Contributing

To add a new skill:

1. Read the [skill-creator](skills/skill-creator/SKILL.md) skill first
2. Follow the guidelines in [CONTRIBUTING.md](CONTRIBUTING.md)
3. Use the [template](template/SKILL_TEMPLATE.md)
4. Submit a PR following conventional commits
5. Add your skill to this registry

---

## Skill Loading Strategy

### For AI Agents

When receiving a user request:

1. **Identify the Angular feature** mentioned (forms, router, etc.)
2. **Check Angular version** in project (if applicable)
3. **Load relevant skill** from this registry
4. **Apply patterns** from the skill to your response
5. **Reference skill** if user needs more context

### For Humans

When working with an AI agent:

1. **Check this registry** for available skills
2. **Mention the skill name** in your request (e.g., "Use the reactive-forms skill")
3. **Specify Angular version** if relevant
4. **Provide feedback** to improve skills

---

## Skill Quality Standards

All skills in this registry must:

- ✅ Follow the [Agent Skills](https://agentskills.io/) format
- ✅ Include clear Angular version support
- ✅ Have complete, runnable code examples
- ✅ Show both correct and incorrect patterns
- ✅ Follow Angular Style Guide conventions
- ✅ Use TypeScript strict mode
- ✅ Be tested with real Angular projects
- ✅ Pass community review and voting

---

## Resources

- **Official Angular Docs**: [angular.dev](https://angular.dev)
- **Angular Update Guide**: [update.angular.io](https://update.angular.io)
- **Angular Style Guide**: [angular.dev/style-guide](https://angular.dev/style-guide)
- **Agent Skills Format**: [agentskills.io](https://agentskills.io/)
- **Contributing Guide**: [CONTRIBUTING.md](CONTRIBUTING.md)

---

Last Updated: January 22, 2026
