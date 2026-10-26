# FDE-102 — Claude Architecture Certificate (learning workspace)

Practice workspace for the Panaversity course **FDE-102**: https://panaversity.org/courses/FDE-102
Owner: Irfan Ahmed (GitHub: `gurupak`). GitHub CLI (`gh`) is authenticated and git uses it for HTTPS credentials.

## Folder convention — every new task gets its own folder

- Create each new practice task/exercise in its own top-level folder, never at the root.
- Name it `NN-short-kebab-name/` where `NN` is the next two-digit number (e.g. `01-first-api-call/`, `02-tool-use-basics/`).
- Each task folder has a `README.md` covering: goal, course module/lesson it maps to, how to run it, and what was learned.
- Keep dependencies, virtualenvs, `.env` files and build output inside the task folder (they are gitignored).
- After creating a task, add a row for it in the root `README.md` task index.

## Defaults

- Use the latest Claude models (e.g. `claude-opus-5-5`, `claude-sonnet-5-5`, `claude-haiku-5-5`) unless a lesson specifies otherwise.
- Never commit API keys; read them from environment variables or a local `.env`.
