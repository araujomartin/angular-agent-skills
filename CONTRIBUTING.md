# Contributing to Angular Agent Skills

Thank you for wanting to contribute! This document explains how to submit your own Angular skills to the community collection.

## Review Process

The contribution process is simple and straightforward:

```
1. Submit PR ──> 2. Maintainer Review ──> 3. Merge or Request Changes
```

### How It Works

1. **Submit your PR** following the guidelines below
2. **Maintainer reviews** your contribution
3. **If approved** → Your skill is merged
4. **If changes needed** → Feedback is provided
5. **If uncertain** → Community input may be requested

### Community Input (Optional)

In cases where the maintainer needs more perspectives:
- Community feedback will be requested via comments
- PR reactions can be used to gauge interest
- The maintainer makes the final decision considering the input received

## Automated Validations

All PRs are automatically validated for:

### 1. Conventional Commits

All commit messages must follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <description>

Types: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert
Scopes: core, forms, router, http, migrations, etc.
```

**Examples:**
- `feat(forms): add signal forms skill`
- `fix(core): correct standalone component example`
- `docs: update README installation steps`
- `feat(migrations): add v18 to v19 upgrade guide`

### 2. SKILL.md Format

Angular skills are validated for proper structure:

**Required Frontmatter:**
```yaml
---
name: skill-name          # lowercase, hyphens only
description: >
  What this Angular skill does.
  Trigger: When AI should use this.
metadata:
  author:
    - name: Your Name
      url: https://your-website-or-github
  version: "1.0.0"
  angular:
    minVersion: "15.0.0"
    maxVersion: "21.0.0"
    supportedVersions: ["15", "16", "17", "18", "19", "20", "21"]
---
```

**Required Sections:**
- `## Version Support` - Clearly state supported Angular versions
- `## When to Use` or `## Trigger` - When to load this skill
- `## Critical Patterns` or `## Core Patterns` - Key Angular patterns
- `## Real-World Examples` - At least 3 complete, runnable code examples
- `## Common Mistakes` - Anti-patterns with corrections

---

## How to Submit a Skill

### Step 1: Fork and Clone

```bash
# Fork the repo on GitHub, then:
git clone https://github.com/YOUR_USERNAME/angular-agent-skills.git
cd angular-agent-skills
```

### Step 2: Choose the Right Location

Organize your skill in the appropriate folder:

```
skills/
├── angular/
│   ├── core/              # Core Angular features (components, directives, pipes)
│   ├── forms/             # Forms (reactive, template-driven, signal forms)
│   ├── router/            # Angular Router patterns
│   ├── http/              # HttpClient patterns
│   ├── performance/       # Performance optimization
│   ├── testing/           # Testing strategies
│   └── migrations/        # Migration guides
│       ├── pattern-migrations/    # Feature-to-feature migrations
│       └── version-upgrades/      # Angular version upgrades
├── ngrx/                  # NgRx state management
├── rxjs/                  # RxJS patterns
└── skill-creator/         # How to create skills
```

### Step 3: Use the Template

Copy the template and create your skill:

```bash
# For a new Angular core skill
cp template/SKILL_TEMPLATE.md skills/angular/core/your-skill-name.md

# For a migration guide
mkdir -p skills/angular/migrations/version-upgrades/v19-to-v20
cp template/SKILL_TEMPLATE.md skills/angular/migrations/version-upgrades/v19-to-v20/SKILL.md
```

### Step 4: Skill Requirements

Your Angular skill must:

- [ ] Have a clear, descriptive name (lowercase, hyphens: `signal-inputs`, `reactive-forms`, `standalone-components`)
- [ ] Include proper YAML frontmatter with Angular version support
- [ ] State minimum and maximum Angular versions
- [ ] Label each pattern with the Angular version it was introduced
- [ ] Include "Version Support" section at the top
- [ ] Have "When to Use" or "Trigger" section
- [ ] Have "Critical Patterns" or "Core Patterns" section
- [ ] Include at least 3 complete, runnable code examples
- [ ] Show both what to do (✅) and what not to do (❌)
- [ ] List anti-patterns specific to Angular
- [ ] Follow Angular Style Guide conventions
- [ ] Use TypeScript strict mode
- [ ] Be tested with real Angular projects
- [ ] Use conventional commits

### Step 5: Validation Checklist

Before submitting, verify:

**Code Quality:**
- [ ] All TypeScript examples compile without errors
- [ ] Examples use latest Angular APIs for the target version
- [ ] Standalone components are preferred (v16+)
- [ ] Signal APIs are used where appropriate (v17+)
- [ ] Built-in control flow (@if, @for) is used (v17+)
- [ ] No deprecated APIs unless showing migration path

**Documentation:**
- [ ] Clear version labels on every pattern
- [ ] Migration paths for older versions (when applicable)
- [ ] Links to official Angular documentation
- [ ] Real-world context for examples

**Formatting:**
- [ ] Consistent code style (Prettier recommended)
- [ ] Proper markdown formatting
- [ ] Working code fence syntax highlighting

### Step 6: Submit Pull Request

```bash
git checkout -b feat/add-your-skill-name
git add skills/angular/
git commit -m "feat(category): add your-skill-name skill"
git push origin feat/add-your-skill-name
```

Then open a PR on GitHub with:

- **Title**: `feat(category): add your-skill-name skill`
- **Description**: Fill out the PR template
  - What Angular feature does this cover?
  - Which Angular versions are supported?
  - Why is this skill needed?
  - Have you tested it with an AI agent?
- **Labels**: `community-skill`, `voting`

### Step 7: Respond to Feedback

During the review:
- Respond to maintainer comments and questions
- Make requested improvements
- Be open to suggestions and constructive feedback
- Update your PR based on review feedback

### What to Avoid

- Copying Angular documentation verbatim (reference it instead)
- Mixing patterns from different Angular versions without labels
- Using deprecated APIs without migration guidance
- Outdated patterns (e.g., NgModule when standalone is available)
- Incomplete examples that can't compile
- Skills without TypeScript types
- Examples that don't follow Angular Style Guide

## Special Considerations for Angular Skills

### Version Support Matrix

When creating skills, consider these Angular milestones:

| Version | Key Features | Notes |
|---------|-------------|-------|
| v15 | Standalone APIs stable | Minimum supported version |
| v16 | Signals introduced | Reactivity model |
| v17 | Built-in control flow, defer | New template syntax |
| v18 | Zoneless support preview | Performance optimization |
| v19 | Signal inputs stable | Component authoring |
| v20 | Upcoming features | Latest stable |
| v21 | Signal Forms | Development version |

### Migration Patterns

When creating migration skills:

1. **Before/After Examples**: Show old pattern vs new pattern
2. **Step-by-Step**: Break down the migration into clear steps
3. **Breaking Changes**: Highlight any breaking changes
4. **Compatibility**: Note if old and new can coexist
5. **CLI Support**: Mention if Angular CLI has automated migration

Example:
```typescript
// ❌ OLD - NgModule-based
@NgModule({
  declarations: [MyComponent],
  imports: [CommonModule]
})
export class MyModule {}

// ✅ NEW (v15+) - Standalone
@Component({
  selector: 'my-component',
  imports: [NgIf, NgFor],
  standalone: true,
  template: `...`
})
export class MyComponent {}
```

## After Acceptance

Once your skill is approved and merged:

1. It will be added to the appropriate `skills/` folder
2. Registered in AGENTS.md
3. Listed in README.md with version support
4. You'll be credited as the author
5. Available for the community to use and improve

## Updating Existing Skills

To update an existing skill:

1. **Angular Version Updates**: When new Angular versions are released
   - Update the supported versions in frontmatter
   - Add new patterns or API changes
   - Test examples with new version

2. **Bug Fix**: If you find errors or outdated examples
   - Open a PR with your fix
   - Clearly explain what was wrong
   - Include the correction
   - Minor fixes can be fast-tracked

3. **Enhancement**: For additional examples or patterns
   - Open a PR with your additions
   - Explain the value added
   - Provide context for the enhancement

## Questions?

- Open an issue with the `question` label
- Tag your issue with relevant labels: `angular-core`, `angular-forms`, `migration`, etc.
- Check existing issues first to avoid duplicates