# Angular Component Input Patterns

Extended examples and common patterns for signal-based inputs and models.

## Table of Contents
- [Real-World Examples](#real-world-examples)
- [Common Mistakes](#common-mistakes)
- [Advanced Patterns](#advanced-patterns)

## Real-World Examples

### Product Card Component

**Context:** E-commerce product display with computed discount pricing

```typescript
import { Component, input, computed, ChangeDetectionStrategy } from '@angular/core';
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
        
        <p class="status">
          {{ product().inStock ? 'Available' : 'Out of Stock' }}
        </p>
      </div>
    </article>
  `,
  styles: [`
    .product-card { border: 1px solid #ccc; padding: 1rem; }
    .product-card.out-of-stock { opacity: 0.6; }
    .discount { color: #e74c3c; font-weight: bold; }
    .status { margin-top: 0.5rem; font-weight: 600; }
  `]
})
export class ProductCardComponent {
  // Required input
  readonly product = input.required<Product>();
  
  // Optional inputs with defaults
  readonly disabled = input(false);
  readonly showDiscount = input(false);
  readonly discountPercent = input(0);
  
  // Computed property for discounted price
  readonly discountedPrice = computed(() => {
    if (this.discountPercent() > 0) {
      const discount = this.product().price * (this.discountPercent() / 100);
      return this.product().price - discount;
    }
    return null;
  });
}
```

**Usage:**
```html
<app-product-card 
  [product]="currentProduct"
  [showDiscount]="true"
  [discountPercent]="15"
/>
```

### Custom Toggle Component with Two-Way Binding

**Context:** Reusable toggle switch with model binding and accessibility

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
  // Model for two-way binding (creates value input and valueChange event)
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

**Why this is problematic:** `model()` creates both the input and a paired change event, adding unnecessary complexity when you only need to receive data.

```typescript
// ❌ BAD - Using model when input would suffice
@Component({
  selector: 'app-display',
  standalone: true,
  template: `<p>{{ message() }}</p>`
})
export class DisplayComponent {
  // Creates both "message" input and its "messageChange" event
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
  // Only creates input, no change event needed
  readonly message = input('');  // ✅ Simpler and clearer
}
```

## Advanced Patterns

### Input Transforms

Apply transformations when inputs are set:

```typescript
import { Component, input, booleanAttribute, numberAttribute } from '@angular/core';

@Component({
  selector: 'app-form-field',
  template: `...`
})
export class FormFieldComponent {
  // Transform string to boolean
  required = input(false, { transform: booleanAttribute });
  
  // Transform string to number
  maxLength = input(0, { transform: numberAttribute });
  
  // Custom transform
  tags = input<string[]>([], {
    transform: (value: string | string[]) => 
      typeof value === 'string' ? value.split(',').map(t => t.trim()) : value
  });
}
```

**Usage:**
```html
<!-- These all work -->
<app-form-field required />
<app-form-field [required]="true" />
<app-form-field required="true" />

<app-form-field maxLength="100" />
<app-form-field [maxLength]="100" />

<app-form-field tags="angular,signals,inputs" />
<app-form-field [tags]="['angular', 'signals']" />
```

### Computed Values from Multiple Inputs

Derive state from multiple inputs using `computed()`:

```typescript
import { Component, input, computed } from '@angular/core';

@Component({
  selector: 'app-user-badge',
  template: `
    <div [class]="badgeClass()">
      <img [src]="avatarUrl()" />
      <span>{{ displayName() }}</span>
    </div>
  `
})
export class UserBadgeComponent {
  firstName = input.required<string>();
  lastName = input.required<string>();
  isPremium = input(false);
  avatarSize = input<'small' | 'medium' | 'large'>('medium');
  
  // Computed from multiple inputs
  displayName = computed(() => 
    `${this.firstName()} ${this.lastName()}`
  );
  
  avatarUrl = computed(() => {
    const size = this.avatarSize();
    const name = this.displayName();
    return `https://api.example.com/avatar/${name}?size=${size}`;
  });
  
  badgeClass = computed(() => 
    this.isPremium() ? 'badge premium' : 'badge standard'
  );
}
```

### Model with Validation

Use `model()` with validation logic:

```typescript
import { Component, model, computed, effect } from '@angular/core';

@Component({
  selector: 'app-range-slider',
  template: `
    <input 
      type="range" 
      [value]="value()"
      [min]="min()"
      [max]="max()"
      (input)="onInput($event)"
    />
    <span>{{ value() }}</span>
    @if (error()) {
      <p class="error">{{ error() }}</p>
    }
  `
})
export class RangeSliderComponent {
  min = input(0);
  max = input(100);
  value = model(50);
  
  error = computed(() => {
    const val = this.value();
    const minVal = this.min();
    const maxVal = this.max();
    
    if (val < minVal) return `Value must be at least ${minVal}`;
    if (val > maxVal) return `Value must be at most ${maxVal}`;
    return null;
  });
  
  onInput(event: Event) {
    const target = event.target as HTMLInputElement;
    const newValue = Number(target.value);
    
    // Clamp value within bounds
    const clamped = Math.max(this.min(), Math.min(this.max(), newValue));
    this.value.set(clamped);
  }
}
```
