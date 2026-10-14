# Northstar — Dynamic Form & Design System

A responsive lead-capture form built with React, TypeScript, and plain CSS. The fields are rendered from a typed configuration array, and all validation lives in a pure validation module.

## Requirements

- Node.js 20 or newer (Node 18.18+ should also work with compatible npm versions)
- npm

## Install and run

```bash
npm install
npm run dev
```

Open the local URL printed by Vite (usually `http://localhost:5173`).

To run a production type-check and build:

```bash
npm run build
npm run preview
```

## Folder layout

```text
src/
├── App.tsx
├── main.tsx
├── styles/
│   ├── tokens.css                 # Color, spacing, typography, breakpoint tokens
│   └── global.css                 # Reset and global defaults
├── design-system/
│   ├── types.ts                   # Shared field, config, values, and error types
│   ├── FormRenderer.tsx           # Maps config field types to atoms
│   ├── atoms/
│   │   ├── TextInput.tsx
│   │   ├── SelectInput.tsx
│   │   ├── TextArea.tsx
│   │   ├── CheckboxInput.tsx
│   │   └── Button.tsx
│   └── molecules/
│       └── Field.tsx              # Label, control, hint, and error composition
└── features/
    └── lead/
        ├── leadConfig.ts          # Lead field configuration and initial values
        ├── validation.ts          # Pure config + values => errors functions
        ├── LeadFormPage.tsx       # Feature state, submission, and page composition
        └── lead-form.css          # Desktop/mobile page and form layout
```

## Architecture notes

- **Design tokens:** `src/styles/tokens.css` defines color, spacing, typography, radii, shadows, and breakpoint tokens.
- **Atoms:** `src/design-system/atoms/` contains controlled inputs and the button. Atoms accept values, labels, and callbacks; they do not contain lead-specific business rules.
- **Field molecule:** `src/design-system/molecules/Field.tsx` composes a label, control slot, hint, and error.
- **Dynamic form:** `src/design-system/FormRenderer.tsx` maps each `FieldConfig.type` to an atom. Add fields by extending `leadFormConfig` and the shared types as needed; there is no per-field hardcoded JSX in the page.
- **Config:** `src/features/lead/leadConfig.ts` defines field names, types, labels, options, conditions, and validation metadata.
- **Validation:** `src/features/lead/validation.ts` exports pure `validateForm(config, values)` and `validateField(...)` functions. It handles required values, email format, exact phone digit count, max length, and conditional visibility.
- **Desktop layout:** `src/features/lead/lead-form.css` uses a two-column field grid above 1024px. Full name and email share a row; notes and consent span both columns.
- **Mobile/tablet layout:** the same CSS switches to a single column at 1024px and below. Below 768px, the submit action is sticky at the bottom of the form card, with safe-area padding for phones.

## Behavior included

- Required full name, email, lead type, phone, and consent.
- Email format validation; phone must contain exactly 10 digits.
- Company name appears only when lead type is `Company` and is required only while visible. Switching back to `Individual` clears the company value.
- Notes are optional and limited to 200 characters.
- Validation runs on blur and submit. Submit validates every visible field, shows all current errors, and does not accept invalid values.
- Successful submissions are displayed on the page; no backend is required.
- Labels, error announcements, invalid states, focus indicators, and keyboard-operable controls are included.
