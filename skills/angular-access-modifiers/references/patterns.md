
# Angular Component Access Modifier Patterns

Extended examples and patterns for the correct use of access modifiers in Angular components, especially in combination with the modern input/output/model APIs.

## Table of Contents
- [Real-World Examples](#real-world-examples)
- [Common Mistakes](#common-mistakes)
- [Advanced Patterns](#advanced-patterns)

## Real-World Examples

### Input Signal (v17+)

**Context:** Receiving data from a parent using the modern `input()` API

```typescript
import { Component, input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-user-badge',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<span>{{ userName() }}</span>`
})
export class UserBadgeComponent {
  // Public API: input
  public readonly userName = input.required<string>();
}
```


### Output Signal (v17+)

**Context:** Exposing an event using the modern `output()` API

```typescript
import { Component, output, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-save-button',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<button (click)="save()">Save</button>`
})
export class SaveButtonComponent {
  // Public API: output
  public readonly saved = output<void>();

  save() {
    this.saved.emit();
  }
}
```


### Template-only state

**Context:** State only used in the template, not exposed as public API

```typescript
import { Component, signal, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-counter',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<div>{{ count() }}</div>`
})
export class CounterComponent {
  // Template-only
  protected readonly count = signal(0);
}
```


### Internal logic

**Context:** State or logic only used inside the class

```typescript
import { Component, signal, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-secret',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<span>Secret logic</span>`
})
export class SecretComponent {
  // Class-only
  private readonly secretValue = signal('hidden');
}
```

## Common Mistakes

### ❌ Don't use public for everything

```typescript
export class BadComponent {
  public count = 0; // ❌ Should be protected or private
}
```

**✅ Instead:**

```typescript
export class GoodComponent {
  protected readonly count = 0;
}
```

### ❌ Don't use private for template state

```typescript
export class BadComponent {
  private readonly count = 0; // ❌ Not accessible in the template
}
```

**✅ Instead:**

```typescript
export class GoodComponent {
  protected readonly count = 0; // ✅ Accessible in the template
}
```


## Advanced Patterns

### Combining input/output with access modifiers

```typescript
import { Component, input, output, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-advanced',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <button (click)="onClick()">Emit</button>
    <span>{{ value() }}</span>
  `
})
export class AdvancedComponent {

  // Public API
  public readonly value = input.required<number>();
  public readonly valueChange = output<number>();

  // Template-only state
  protected readonly internalState = signal('visible');

  // Internal logic
  private readonly secret = signal('hidden');

  onClick() {
    this.valueChange.emit(this.value() + 1);
  }
}
```

For more examples of signal inputs and outputs, see the [angular-component-inputs](../angular-component-inputs/references/input-patterns.md) skill.
