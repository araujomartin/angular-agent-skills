# Angular Skill Template

Use this template when creating a new Angular skill. Copy this file to your skill folder and rename it to `SKILLS.md`.

---

```markdown
---
name: your-skill-name
description: >
  Brief description of what this Angular skill covers.
  Trigger: When working with [Angular feature], when building [type of component/service], when using [Angular API].
metadata:
  version: "1.0.0"
  author:
    - name: Your Name
      url: https://your-website-or-github
  angular:
    minVersion: "15.0.0"
    maxVersion: "21.0.0"
    supportedVersions: ["15", "16", "17", "18", "19", "20", "21"]
---

## Version Support

**Minimum Angular Version:** 15.0.0  
**Maximum Angular Version:** 21.0.0  
**Supported Versions:** 15, 16, 17, 18, 19, 20, 21

## When to Use

Load this skill when:
- [Condition 1, e.g., "Working with Reactive Forms"]
- [Condition 2, e.g., "Implementing form validation"]
- [Condition 3, e.g., "Using FormBuilder or FormControl"]

## Critical Patterns

### Pattern 1: [Name]

**Supported in:** v15+

[Explanation of the pattern]

```typescript
// Good example
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  template: `...`
})
export class ExampleComponent {
  // Your code here
}
```

### Pattern 2: [Name]

**Supported in:** v17+

[Explanation of the pattern and what changed from previous versions]

```typescript
// Good example (v17+)
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `...`
})
export class ExampleComponent {
  // Your code here
}
```

### Pattern 3: [Name]

**Supported in:** v20+

[Explanation of the pattern for newer versions]

```typescript
// Good example (v20+)
import { Component, signal } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `...`
})
export class ExampleComponent {
  value = signal(0);
}
```

For detailed examples, common mistakes, and advanced patterns, see [references/patterns.md](references/patterns.md).

## Quick Reference

| Task | Pattern |
|------|---------|
| [Task 1] | `code snippet` |
| [Task 2] | `code snippet` |
| [Task 3] | `code snippet` |

## Resources

- [Official Angular Documentation](https://angular.dev)
- [Angular Update Guide](https://update.angular.io)
- [Relevant API Reference](https://angular.dev/api)
```

---

## References Folder Structure

Extensive examples and patterns should be moved to a `references/` folder within your skill directory:

```
your-skill-name/
├── SKILL.md              # Main skill file (concise)
└── references/
    └── patterns.md       # Extended examples, common mistakes, advanced patterns
```

The `references/patterns.md` file should contain:
- **Real-World Examples**: Complete, production-ready code examples
- **Common Mistakes**: Anti-patterns with corrections
- **Advanced Patterns**: Complex use cases and edge cases

Keep the main SKILL.md focused on critical patterns and quick reference, with a link to references for deeper exploration.

---

## Validation Checklist

Before submitting, ensure your skill meets these requirements:

### Frontmatter (REQUIRED)
- [ ] Has `name:` field (lowercase, hyphens only)
- [ ] Has `description:` field with "Trigger:" included
- [ ] Has `metadata.version:` in semver format
- [ ] Has `metadata.angular:` with version support info

### Required Sections
- [ ] `## Version Support` - Shows supported Angular versions
- [ ] `## When to Use` - Clear triggers for when to apply this skill
- [ ] `## Critical Patterns` - Core patterns with version labels
- [ ] Link to `references/` folder for extended examples (if applicable)

### Code Quality
- [ ] Examples are complete and can run standalone
- [ ] Each pattern clearly labeled with supported versions
- [ ] Shows both what to do (✅) and what not to do (❌)
- [ ] Follows Angular style guide conventions