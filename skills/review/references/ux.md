# UX Lens

**Perspective:** "Does this create a good user experience?"

**Output Tag:** `[UX]`

## When to Apply

- Files in `**/components/**`
- Files in `**/app/**/*.tsx`
- Any user-facing changes

## Checks

### Loading States (MUST)

- [ ] Async operations show loading indicator
- [ ] No layout shift during load
- [ ] Skeleton states for content areas

**Flag:** `[UX] Loading: file:line has async operation without loading state`

### Error States (MUST)

- [ ] Errors displayed to user clearly
- [ ] Error messages are actionable
- [ ] Recovery path provided

**Flag:** `[UX] Error state: file:line catches error but shows nothing to user`

### Empty States (SHOULD)

- [ ] Empty data has meaningful display
- [ ] Clear call-to-action when empty

**Flag:** `[UX] Empty state: file:line renders blank when data is empty`

### Accessibility (MUST)

- [ ] Interactive elements keyboard accessible
- [ ] ARIA labels on icons/buttons
- [ ] Focus management on modals/dialogs
- [ ] Color contrast meets WCAG AA

**Flag:** `[UX] A11y: file:line [element] not keyboard accessible`

### Design System (SHOULD)

- [ ] Uses shadcn/ui or Radix primitives
- [ ] Colors from Tailwind theme, not hardcoded
- [ ] Spacing follows design scale

**Flag:** `[UX] Design system: file:line uses custom styling instead of [component]`

## KYA-OS Specific Patterns

### Dashboard Consistency

- [ ] Card-based layouts for data display
- [ ] Consistent header/action bar patterns
- [ ] TanStack Table for data grids

**Flag:** `[UX] Dashboard: file:line creates custom data display instead of using Card pattern`

### Onboarding Flow

- [ ] < 3 clicks to complete action
- [ ] Progress indication for multi-step
- [ ] Clear success/failure feedback

**Flag:** `[UX] Onboarding: flow requires [N] clicks, target is < 3`

### Data Visualization

- [ ] Charts have clear legends
- [ ] Interactive elements have hover states
- [ ] Real-time data shows update indicators

**Flag:** `[UX] Visualization: chart at file:line missing legend/hover states`

## Severity Guide

| Category | Severity |
|----------|----------|
| Missing loading state | MUST |
| Missing error state | MUST |
| Accessibility violation | MUST |
| Missing empty state | SHOULD |
| Design system deviation | SHOULD |
