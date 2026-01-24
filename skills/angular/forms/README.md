# Forms Skills

Angular forms skills for building reactive, and signal-based forms.

## Available Skills

| Skill | Description | Versions | URL |
|-------|-------------|----------|-----|
| `reactive-forms` | FormControl, FormGroup, FormBuilder patterns | v15+ | [SKILLS.md](reactive-forms/SKILLS.md) |
| `form-validation` | Sync/async validators, custom validation logic | v15+ | [SKILLS.md](form-validation/SKILLS.md) |
| `signal-forms` | Signal-based form APIs and reactive patterns | v20+ | [SKILLS.md](signal-forms/SKILLS.md) |
| `custom-form-controls` | ControlValueAccessor, custom form components | v15+ | [SKILLS.md](custom-form-controls/SKILLS.md) |
| `form-arrays` | Dynamic forms, nested structures, FormArray | v15+ | [SKILLS.md](form-arrays/SKILLS.md) |

## Auto-invoke Skills

When performing these actions, ALWAYS invoke the corresponding skill FIRST:

| Action | Skill |
|--------|-------|
| Building forms with FormControl/FormGroup | `reactive-forms` |
| Adding form validation rules | `form-validation` |
| Working with signals in forms (v21+) | `signal-forms` |
| Creating reusable form controls | `custom-form-controls` |
| Building dynamic or nested forms | `form-arrays` |
