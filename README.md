# vue3-a11y-modal

Vue 3 + TypeScript demo of an **accessible modal** with keyboard support.

## What it demonstrates

- `role="dialog"` and `aria-modal="true"`
- Title linked via `aria-labelledby`
- **Escape** closes the dialog
- **Tab / Shift+Tab** focus trap while open
- Initial focus on the close control when opened
- Focus restored to the trigger button when closed
- Backdrop click closes; clicks inside the dialog do not (`@click.stop`)
- `v-model:open` via `defineModel`

## Run locally

```bash
npm install
npm run dev
```

## Scripts

| Command | Purpose |
|---------|---------|
| `npm run dev` | Vite dev server |
| `npm run typecheck` | TypeScript check (`vue-tsc`) |
| `npm run build` | Typecheck + production build |

## Layout

- `src/components/A11yModal.vue` — accessible dialog
- `src/components/ModalDemo.vue` — open trigger + focus restore

## CI

GitHub Actions runs install → typecheck → build on push and pull requests to `main`.
