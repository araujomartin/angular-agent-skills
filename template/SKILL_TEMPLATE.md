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

## Real-World Examples

### Example 1: [Use Case]

**Context:** [When/where to use this]

```typescript
// Complete working example
import { Component } from '@angular/core';

@Component({
  selector: 'app-example',
  standalone: true,
  template: `
    <div>
      <!-- Your template -->
    </div>
  `
})
export class ExampleComponent {
  // Implementation
}
```

### Example 2: [Use Case]

**Context:** [When/where to use this]

```typescript
// Another complete example
```

## Common Mistakes

### ❌ Don't: [Anti-pattern name]

**Why this is problematic:** [Explanation]

```typescript
// ❌ BAD - Don't do this
export class BadComponent {
  // What NOT to do
}
```

**✅ Instead, do this:**

```typescript
// ✅ GOOD - Correct approach
export class GoodComponent {
  // Correct implementation
}
```

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
- [ ] `## Real-World Examples` - Complete, runnable code examples
- [ ] `## Common Mistakes` - Anti-patterns with corrections

### Code Quality
- [ ] Examples are complete and can run standalone
- [ ] Each pattern clearly labeled with supported versions
- [ ] Shows both what to do (✅) and what not to do (❌)
- [ ] Follows Angular style guide conventions