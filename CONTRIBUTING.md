# Contributing

Thanks for contributing. Keep changes focused, small, and aligned with the
existing project conventions.

## Before you start

- Read `AGENTS.md` for the full coding, documentation, testing, and workflow
  rules.
- Use Node 24 and pnpm only.
- Create branches with a conventional prefix, such as `fix/`, `feat/`,
  `docs/`, `chore/`, or `refactor/`.
- Keep dependency changes separate unless they are required for the work.

## Development

```bash
nvm use
corepack enable
pnpm install
cp .env.example .env
pnpm dev
```

Use `pnpm compose up -d postgres redis` when you only need supporting local
services. Use `pnpm compose up -d --build` when you want the app and services to
run through Docker Compose.

## Checks

Run the relevant checks before opening a pull request:

```bash
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

## Pull requests

- Use a conventional PR title.
- Explain what changed and how it was tested.
- Include both unit and integration coverage for behaviour changes.
- Do not commit local env overrides or the root `.env`.
