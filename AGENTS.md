# AGENTS.md

This document is intended for agents (human or bot) interacting with this Next.js project. It provides a summary of components that are safe to modify, files that should be changed with caution, instructions for running with `bun`, testing recommendations, security guidance, and a directory map for quick reference.

## Purpose
- Project: a Next.js template for digital agencies and creative portfolios.
- Document goal: speed up other agents' understanding without opening every directory.

## Quick start (using bun)
If you prefer to use `bun`, here are the quick setup and development commands:

```bash
# install dependencies
bun install

# run the development server (executes the `dev` script from package.json)
bun run dev

# build and start for production
bun run build
bun run start
```

Note: `bun run <script>` executes scripts from `package.json` (e.g. `dev`, `build`, `start`). If you don't use `bun`, use `npm install` + `npm run dev` instead.

## Components that CAN be modified (safe / commonly changed)
- `src/pages/` — ready-made pages; many subfolders contain page variants (homes, services, blogs, etc.).
- `src/pages/homes/*` — homepage variants (`HomeMain` and themed variants).
- `src/components/` — UI components: `hero-banner`, `service`, `project`, `testimonial`, `video-area`, `text-slider`, etc.
- `src/layouts/` — header, footer, and layout-level components (e.g. `HomeMainHeader`, `HomeMainFooter`).
- `src/data/` — example content (`portfolioData`, `teamData`, `blogData`). Change data here to update demo content without touching components.
- `public/` — static assets (images, fonts, compiled CSS). Safe to replace or add files.
- `src/app/globals.scss` and other SASS files — global styles; modify to adjust theme.
- `src/hooks/` and `src/components/provider/` — providers and hooks (scroll smoothing, custom cursor). Modify to alter front-end behavior.

Guideline: For display/content/section-order changes, start with `src/data/`, `src/pages/*`, or `src/components/*`.

## Files that SHOULD NOT / SHOULD BE CHANGED WITH CAUTION
- `node_modules/`, `.next/` — do not alter (build output / dependencies).
- `package.json` & `resolutions` — allowed to change by maintainers, but updating dependency versions or scripts affects the build environment. Open a PR and run tests when changing.
- `next.config.ts`, `tsconfig.json`, `eslint.config.mjs`, `global.d.ts` — project configuration files; change only if you understand the impact.
- `src/plugins/fragmentShader.ts` or other shader/three.js code — sensitive; modify only if familiar with graphics / three.js.

Note: Changes to config files, dependencies, or the build pipeline must be verified by running `bun run build` and performing local tests.

## Testing (recommendations & quick steps)
The project currently has no built-in tests. Minimum recommendations:

- Unit / component tests: use `vitest` + `@testing-library/react` for React components.
- E2E: use `Playwright` (recommended for UI/flow checks) or `Cypress`.

Example steps (using bun):

```bash
# install testing dev dependencies
bun add -d vitest @testing-library/react @testing-library/jest-dom jsdom

# (optional) install Playwright for e2e
bun add -d @playwright/test

# example scripts (add to package.json "scripts")
# "test:unit": "vitest",
# "test:e2e": "playwright test"

# run unit tests
bun run test:unit

# run e2e tests
bun run test:e2e
```

Add a `vitest.config.ts` and example tests under `__tests__/` to create a baseline. I can generate a test skeleton and scripts in `package.json` if requested.

## Security & quality checks
- Static checks:
  - Type check: `bun run tsc --noEmit` or `npx tsc --noEmit`.
  - Lint: `bun run lint` (the `lint` script already exists in `package.json`).
- Dependency audit: if using `npm`/`node`, run `npm audit`; for `bun`, you can still use `npm audit` or external services (`Snyk`, `Dependabot`) to scan for CVEs.
- Ensure secrets are not committed (.env). Add rules to `.gitignore` if necessary.
- Use a lockfile and Dependabot for automated dependency updates.

Quick example commands:
```bash
# typecheck
bun run tsc --noEmit

# lint
bun run lint

# audit (using npm audit if needed)
npm audit --audit-level=high
```

## Mapping / Project map — quick reference for agents

- `package.json` — metadata + scripts (dev/build/start/lint)
- `next.config.ts` — Next.js configuration
- `tsconfig.json` — TypeScript configuration
- `eslint.config.mjs` — ESLint configuration
- `global.d.ts` — global type augmentations

- `src/app/page.tsx` — entry page (renders `HomeMain`)
- `src/app/globals.scss` — global styles

- `src/pages/` — pages folder (variants / routes)
  - `src/pages/homes/home/HomeMain.tsx` — main homepage composition (assembles all sections)
  - `src/pages/homes/*` — homepage variants (creative-studio, corporate, personal-portfolio, etc.)

- `src/components/` — main UI components, organized by feature:
  - `hero-banner/` — hero and banners
  - `about/` — about section
  - `service/` — services list
  - `project/` — portfolio / project listing
  - `testimonial/` — testimonial slider
  - `video-area/`, `text-slider/`, `banner/`, `work/`, etc.
  - `provider/` — context providers such as `ScrollSmoothProvider`, custom cursor

- `src/layouts/` — headers & footers
  - `layouts/headers/HomeMainHeader.tsx`
  - `layouts/footers/HomeMainFooter.tsx`

- `src/data/` — demo data (`teamData`, `portfolioData`, `blogData`, `priceData`, etc.)

- `src/hooks/` — custom hooks (`useAutoPlayVideo`, `useGsapAnimation`, `useStickyHeader`, etc.)

- `src/plugins/` — utilities/plugins (e.g. `fragmentShader.ts`)

- `src/pages/*` and `src/app/*` — routes and app-level files

- `public/assets/` — images, fonts, compiled scss assets

## Operational notes for agents
- If the task is "change landing text / images" → update `src/data/` and `public/assets/img`.
- If the task is "change layout / section order" → modify `src/pages/homes/home/HomeMain.tsx` or the homepage variant under `src/pages/homes/*`.
- If the task is "change global styling / theme colors" → update `src/app/globals.scss` and related SASS files.
- To add a new page → create a new folder under `src/pages/` and add components and routes following the existing pattern.

---

If you want, I can:
- add a `vitest` test skeleton with an example component test, or
- add test scripts to `package.json`, or
- run `bun install` + `bun run dev` here to visually verify the site.

Choose one option or request adjustments to this AGENTS.md.
