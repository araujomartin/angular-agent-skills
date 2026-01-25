---
name: signal-based-inputs-outputs
description: >
  Modern function-based inputs, outputs, and models using Angular Signals API.
  Trigger: When defining component inputs/outputs, when migrating from decorators, when working with reactive component APIs, when implementing two-way binding.
metadata:
  version: "1.0.0"
  author: araujomartin
  angular:
    minVersion: "17.3.0"
    maxVersion: "21.0.0"
    supportedVersions: ["17", "18", "19", "20", "21"]
---
## Version Support

**Minimum Angular Version:** 17.3.0  
**Maximum Angular Version:** 21.0.0  
**Supported Versions:** 17, 18, 19, 20, 21

> **Note:** Signal inputs (`input()`, `output()`, `model()`) were introduced as **developer preview** in v17.3. They became **stable/production-ready** in v19.0.
## When to Use

Load this skill when:
- Defining component or directive inputs and outputs
- Migrating from `@Input()` and `@Output()` decorators to function-based APIs
- Implementing reactive component interfaces
- Working with two-way binding using `model()`
- Building components with signal-based reactivity

## Critical Patterns

### Pattern 1: Function-Based Inputs

**Introduced in:** v17.3 (developer preview) | **Stable since:** v19.0

Use the `input()` and `input.required()` functions instead of the `@Input()` decorator. These create `InputSignal` objects that integrate seamlessly with Angular's reactivity system.

```typescript
// ✅ GOOD - Function-based inputs
import { Component, input, ChangeDetectionStrategy } from '@angular/core';

interface User {
  id: string;
  name: string;
}

@Component({
  selector: 'app-user-card',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <div class="user-card">
      <h3>{{ user().name }}</h3>
      <p>ID: {{ user().id }}</p>
      @if (disabled()) {
        <span class="badge">Disabled</span>
      }
    </div>
  `
})
export class UserCardComponent {
  // Required input - will error if not provided
  readonly user = input.required<User>();
  
  // Optional input with default value
  readonly disabled = input(false);
  
  // Optional input without default (undefined initially)
  readonly userId = input<string>();
}
```

```typescript
// ❌ BAD - Old decorator-based approach
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user-card',
  standalone: true,
  template: `...`
})
export class UserCardComponent {
  @Input({ required: true }) user!: User;
  @Input() disabled = false;
  @Output() selected = new EventEmitter<User>();
}
```

### Pattern 2: Function-Based Outputs

**Introduced in:** v17.3 (developer preview) | **Stable since:** v19.0

Use the `output()` function instead of `@Output()` decorator with `EventEmitter`. This creates an `OutputEmitterRef` that provides a cleaner, more type-safe API.

```typescript
// ✅ GOOD - Function-based outputs
import { Component, input, output, ChangeDetectionStrategy } from '@angular/core';

interface User {
  id: string;
  name: string;
}

@Component({
  selector: 'app-user-list',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    @for (user of users(); track user.id) {
      <div (click)="handleClick(user)">
        {{ user.name }}
      </div>
    }
  `
})
export class UserListComponent {
  readonly users = input.required<User[]>();
  
  // Output with typed event
  readonly userSelected = output<User>();
  
  // Output for void events
  readonly listCleared = output<void>();
  
  handleClick(user: User): void {
    this.userSelected.emit(user);
  }
  
  clearList(): void {
    this.listCleared.emit();
  }
}
```

```typescript
// ❌ BAD - Old decorator-based approach
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-user-list',
  standalone: true,
  template: `...`
})
export class UserListComponent {
  @Input({ required: true }) users!: User[];
  @Output() userSelected = new EventEmitter<User>();
  @Output() listCleared = new EventEmitter<void>();
}
```

### Pattern 3: Two-Way Binding with model()

**Introduced in:** v17.3 (developer preview) | **Stable since:** v19.0

Use the `model()` function for two-way binding. It creates both an input and an output (with "Change" suffix) automatically, replacing the `[(value)]` two-way binding pattern with decorators.

```typescript
// ✅ GOOD - Model for two-way binding
import { Component, model, input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-custom-checkbox',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <label>
      <input 
        type="checkbox"
        [checked]="checked()"
        (change)="toggle()"
      />
      {{ label() }}
    </label>
  `
})
export class CustomCheckboxComponent {
  readonly label = input.required<string>();
  
  // Creates both "checked" input and "checkedChange" output
  readonly checked = model(false);
  
  toggle(): void {
    this.checked.set(!this.checked());
  }
}
```

**Usage in parent:**
```typescript
@Component({
  selector: 'app-parent',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  imports: [CustomCheckboxComponent],
  template: `
    <!-- Two-way binding -->
    <app-custom-checkbox 
      label="Accept terms"
      [(checked)]="termsAccepted"
    />
    
    <!-- Or one-way binding -->
    <app-custom-checkbox 
      label="Subscribe"
      [checked]="subscribed"
      (checkedChange)="onSubscribeChange($event)"
    />
  `
})
export class ParentComponent {
  termsAccepted = signal(false);
  subscribed = signal(true);
  
  onSubscribeChange(value: boolean): void {
    this.subscribed.set(value);
  }
}
```

### Pattern 4: Input Aliases

**Introduced in:** v17.3 (developer preview) | **Stable since:** v19.0

Use the `alias` option to provide a different name for the input property in templates while keeping a different name in the component class.

```typescript
// ✅ GOOD - Input with alias
import { Component, input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-user-profile',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <h2>{{ userName() }}</h2>
  `
})
export class UserProfileComponent {
  // Template uses "name", component uses "userName"
  readonly userName = input.required<string>({ alias: 'name' });
}
```

**Usage:**
```html
<!-- Use the alias in templates -->
<app-user-profile name="John Doe" />
```

## Real-World Examples

### Example 1: Product Card Component

**Context:** E-commerce product display with selection capability

```typescript
import { Component, input, output, computed, ChangeDetectionStrategy } from '@angular/core';
import { CurrencyPipe } from '@angular/common';

interface Product {
  id: string;
  name: string;
  price: number;
  imageUrl: string;
  inStock: boolean;
}

@Component({
  selector: 'app-product-card',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  imports: [CurrencyPipe],
  template: `
    <article class="product-card" [class.out-of-stock]="!product().inStock">
      <img [src]="product().imageUrl" [alt]="product().name" />
      <div class="content">
        <h3>{{ product().name }}</h3>
        <p class="price">{{ product().price | currency }}</p>
        
        @if (showDiscount() && discountedPrice()) {
          <p class="discount">
            Was: <s>{{ product().price | currency }}</s>
            Now: {{ discountedPrice() | currency }}
          </p>
        }
        
        <button 
          (click)="handleSelect()"
          [disabled]="!product().inStock || disabled()"
        >
          {{ product().inStock ? 'Add to Cart' : 'Out of Stock' }}
        </button>
      </div>
    </article>
  `,
  styles: [`
    .product-card { border: 1px solid #ccc; padding: 1rem; }
    .product-card.out-of-stock { opacity: 0.6; }
    .discount { color: #e74c3c; font-weight: bold; }
  `]
})
export class ProductCardComponent {
  // Required input
  readonly product = input.required<Product>();
  
  // Optional inputs with defaults
  readonly disabled = input(false);
  readonly showDiscount = input(false);
  readonly discountPercent = input(0);
  
  // Output for product selection
  readonly productSelected = output<Product>();
  
  // Computed property for discounted price
  readonly discountedPrice = computed(() => {
    if (this.discountPercent() > 0) {
      const discount = this.product().price * (this.discountPercent() / 100);
      return this.product().price - discount;
    }
    return null;
  });
  
  handleSelect(): void {
    if (this.product().inStock && !this.disabled()) {
      this.productSelected.emit(this.product());
    }
  }
}
```

### Example 2: Custom Toggle Component with Two-Way Binding

**Context:** Reusable toggle switch with model binding

```typescript
import { Component, model, input, computed, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-toggle',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <div class="toggle-wrapper">
      @if (label()) {
        <label [for]="toggleId()">{{ label() }}</label>
      }
      
      <button
        [id]="toggleId()"
        type="button"
        role="switch"
        [attr.aria-checked]="value()"
        [attr.aria-label]="ariaLabel() || label()"
        [disabled]="disabled()"
        [class.toggle-on]="value()"
        [class.toggle-off]="!value()"
        (click)="toggle()"
      >
        <span class="toggle-thumb"></span>
      </button>
      
      @if (description()) {
        <p class="description">{{ description() }}</p>
      }
    </div>
  `,
  styles: [`
    .toggle-wrapper {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
    }
    
    button[role="switch"] {
      width: 3rem;
      height: 1.5rem;
      border-radius: 1rem;
      border: none;
      cursor: pointer;
      position: relative;
      transition: background-color 0.2s;
    }
    
    .toggle-off { background-color: #ccc; }
    .toggle-on { background-color: #4caf50; }
    
    .toggle-thumb {
      position: absolute;
      top: 0.125rem;
      left: 0.125rem;
      width: 1.25rem;
      height: 1.25rem;
      border-radius: 50%;
      background: white;
      transition: transform 0.2s;
    }
    
    .toggle-on .toggle-thumb {
      transform: translateX(1.5rem);
    }
    
    button:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }
    
    .description {
      font-size: 0.875rem;
      color: #666;
      margin: 0;
    }
  `]
})
export class ToggleComponent {
  // Model for two-way binding (creates value input and valueChange output)
  readonly value = model(false);
  
  // Optional inputs
  readonly label = input<string>();
  readonly description = input<string>();
  readonly disabled = input(false);
  readonly ariaLabel = input<string>();
  
  // Computed ID for accessibility
  readonly toggleId = computed(() => 
    `toggle-${Math.random().toString(36).substring(2, 9)}`
  );
  
  toggle(): void {
    if (!this.disabled()) {
      this.value.update(v => !v);
    }
  }
}
```

**Usage in parent:**
```typescript
import { Component, signal, ChangeDetectionStrategy } from '@angular/core';
import { ToggleComponent } from './toggle.component';

@Component({
  selector: 'app-settings',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  imports: [ToggleComponent],
  template: `
    <div class="settings">
      <h2>Settings</h2>
      
      <app-toggle
        label="Email Notifications"
        description="Receive email updates about your account"
        [(value)]="emailNotifications"
      />
      
      <app-toggle
        label="Dark Mode"
        description="Switch to dark theme"
        [(value)]="darkMode"
      />
      
      <app-toggle
        label="Beta Features"
        description="Try out experimental features"
        [(value)]="betaFeatures"
        [disabled]="!isPremiumUser()"
      />
      
      <button (click)="saveSettings()">Save Settings</button>
    </div>
  `
})
export class SettingsComponent {
  emailNotifications = signal(true);
  darkMode = signal(false);
  betaFeatures = signal(false);
  isPremiumUser = signal(true);
  
  saveSettings(): void {
    console.log({
      emailNotifications: this.emailNotifications(),
      darkMode: this.darkMode(),
      betaFeatures: this.betaFeatures()
    });
  }
}
```

### Example 3: Data Table Row Component

**Context:** Table row with selection and action outputs

```typescript
import { Component, input, output, computed, ChangeDetectionStrategy } from '@angular/core';
import { DatePipe } from '@angular/common';

interface TableRow {
  id: string;
  name: string;
  email: string;
  status: 'active' | 'inactive' | 'pending';
  createdAt: Date;
}

@Component({
  selector: 'app-table-row',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  imports: [DatePipe],
  template: `
    <tr 
      [class.selected]="selected()"
      [class.status-active]="row().status === 'active'"
      [class.status-inactive]="row().status === 'inactive'"
      [class.status-pending]="row().status === 'pending'"
    >
      @if (selectable()) {
        <td>
          <input 
            type="checkbox"
            [checked]="selected()"
            (change)="toggleSelection()"
          />
        </td>
      }
      
      <td>{{ row().name }}</td>
      <td>{{ row().email }}</td>
      <td>
        <span [class]="'badge badge-' + row().status">
          {{ statusLabel() }}
        </span>
      </td>
      <td>{{ row().createdAt | date:'short' }}</td>
      
      @if (showActions()) {
        <td class="actions">
          <button (click)="handleEdit()">Edit</button>
          <button (click)="handleDelete()">Delete</button>
        </td>
      }
    </tr>
  `,
  styles: [`
    tr { cursor: pointer; }
    tr.selected { background-color: #e3f2fd; }
    .badge {
      padding: 0.25rem 0.5rem;
      border-radius: 0.25rem;
      font-size: 0.75rem;
      font-weight: 600;
    }
    .badge-active { background-color: #4caf50; color: white; }
    .badge-inactive { background-color: #9e9e9e; color: white; }
    .badge-pending { background-color: #ff9800; color: white; }
    .actions button { margin: 0 0.25rem; }
  `]
})
export class TableRowComponent {
  // Required input
  readonly row = input.required<TableRow>();
  
  // Optional inputs
  readonly selectable = input(true);
  readonly selected = input(false);
  readonly showActions = input(true);
  
  // Outputs for different actions
  readonly selectionToggled = output<TableRow>();
  readonly editClicked = output<TableRow>();
  readonly deleteClicked = output<TableRow>();
  
  // Computed status label
  readonly statusLabel = computed(() => {
    const status = this.row().status;
    return status.charAt(0).toUpperCase() + status.slice(1);
  });
  
  toggleSelection(): void {
    this.selectionToggled.emit(this.row());
  }
  
  handleEdit(): void {
    this.editClicked.emit(this.row());
  }
  
  handleDelete(): void {
    this.deleteClicked.emit(this.row());
  }
}
```

## Common Mistakes

### ❌ Don't: Mix decorators with function-based APIs

**Why this is problematic:** Mixing old decorator-based and new function-based APIs creates inconsistency and confusion. Choose one approach and stick with it.

```typescript
// ❌ BAD - Mixed approaches
@Component({
  selector: 'app-mixed',
  standalone: true,
  template: `...`
})
export class MixedComponent {
  @Input() oldStyleInput!: string;  // Old style
  readonly newStyleInput = input<string>();  // New style
  
  @Output() oldStyleOutput = new EventEmitter<void>();  // Old style
  readonly newStyleOutput = output<void>();  // New style
}
```

**✅ Instead, do this:**

```typescript
// ✅ GOOD - Consistent function-based approach
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-consistent',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `...`
})
export class ConsistentComponent {
  readonly newStyleInput = input<string>();
  readonly anotherInput = input.required<string>();
  
  readonly newStyleOutput = output<void>();
  readonly anotherOutput = output<string>();
}
```

### ❌ Don't: Forget to call signal inputs as functions

**Why this is problematic:** Signal inputs are functions that must be called with `()` to access their value. Forgetting this will result in errors.

```typescript
// ❌ BAD - Not calling signal as function
@Component({
  selector: 'app-bad',
  standalone: true,
  template: `
    <!-- This won't work! -->
    <h1>{{ userName }}</h1>
    <p>Length: {{ userName.length }}</p>
  `
})
export class BadComponent {
  readonly userName = input.required<string>();
}
```

**✅ Instead, do this:**

```typescript
// ✅ GOOD - Calling signal inputs correctly
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-good',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `
    <!-- Correct: call the signal function -->
    <h1>{{ userName() }}</h1>
    <p>Length: {{ userName().length }}</p>
  `
})
export class GoodComponent {
  readonly userName = input.required<string>();
  
  // Also call in component methods
  logUserName(): void {
    console.log(this.userName());  // ✅ Correct
    // console.log(this.userName);  // ❌ Wrong
  }
}
```

### ❌ Don't: Try to modify input signals directly

**Why this is problematic:** Input signals are read-only. You cannot use `.set()` or `.update()` on them. If you need mutable state, create a separate writable signal.

```typescript
// ❌ BAD - Trying to modify input signal
@Component({
  selector: 'app-bad',
  standalone: true,
  template: `...`
})
export class BadComponent {
  readonly count = input(0);
  
  increment(): void {
    this.count.set(this.count() + 1);  // ❌ ERROR: Property 'set' does not exist
  }
}
```

**✅ Instead, do this:**

```typescript
// ✅ GOOD - Use model() for mutable input or separate signal
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-good',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `...`
})
export class GoodComponent {
  // Option 1: Use model() if parent needs to track changes
  readonly count = model(0);
  
  increment(): void {
    this.count.set(this.count() + 1);  // ✅ Works with model
  }
}

// OR

import { signal, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-good-alternative',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `...`
})
export class GoodAlternativeComponent {
  // Option 2: Keep input read-only and use internal signal
  readonly initialCount = input(0);
  readonly count = signal(0);
  
  ngOnInit(): void {
    this.count.set(this.initialCount());
  }
  
  increment(): void {
    this.count.update(v => v + 1);  // ✅ Works with writable signal
  }
}
```

### ❌ Don't: Use model() when you only need one-way input

**Why this is problematic:** `model()` creates both an input and an output, adding unnecessary complexity when you only need to receive data.

```typescript
// ❌ BAD - Using model when input would suffice
@Component({
  selector: 'app-display',
  standalone: true,
  template: `<p>{{ message() }}</p>`
})
export class DisplayComponent {
  // Creates both "message" input and "messageChange" output
  // but we never emit changes
  readonly message = model('');  // ❌ Overkill
}
```

**✅ Instead, do this:**

```typescript
// ✅ GOOD - Use input() for read-only data
import { ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-display',
  standalone: true,
  changeDetection: ChangeDetectionStrategy.OnPush,
  template: `<p>{{ message() }}</p>`
})
export class DisplayComponent {
  // Only creates input, no output needed
  readonly message = input('');  // ✅ Simpler and clearer
}
```

## Quick Reference

| Task | Pattern |
|------|---------|
| Required input | `readonly user = input.required<User>()` |
| Optional input with default | `readonly disabled = input(false)` |
| Optional input without default | `readonly userId = input<string>()` |
| Input with alias | `readonly userName = input.required<string>({ alias: 'name' })` |
| Simple output | `readonly clicked = output<void>()` |
| Typed output | `readonly userSelected = output<User>()` |
| Two-way binding | `readonly checked = model(false)` |
| Required model | `readonly value = model.required<string>()` |
| Access input value | `this.userName()` |
| Emit output | `this.clicked.emit()` |
| Update model | `this.checked.set(true)` |

## Resources

- [Official Angular Signals Guide](https://angular.dev/guide/signals)
- [Signal Inputs Documentation](https://angular.dev/guide/signals/inputs)
- [Signal Outputs Documentation](https://angular.dev/guide/signals/outputs)
- [Model Inputs Documentation](https://angular.dev/guide/signals/model)
- [Angular API Reference - input()](https://angular.dev/api/core/input)
- [Angular API Reference - output()](https://angular.dev/api/core/output)
- [Angular API Reference - model()](https://angular.dev/api/core/model)
