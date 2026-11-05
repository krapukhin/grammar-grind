# AGENTS.md

Instructions for AI agents working in this repository.

## Project

Grammar Grind is a free, open-source (MIT license) app for learning English. Its core concept is repeating exercises many times until they become automatic.

## Language

All text in this repository is written in English: documentation, code, comments, identifiers and commit messages. The only exception is learner-facing Russian content, such as prompts, grammar texts and UI strings.

## Current state

There is no code yet, so there are no build, lint or test commands either. Add the commands to this file once they exist.

The first version is a simple Russian-English web app in Python with an SQLite database. Once it is debugged, the app moves to iOS. Do not make architectural decisions that are still open on your own: propose them to the project owner.

Drafts (rows with `source = 'draft'` in `prompts` and `grammar_texts`) exist only for local debugging and are not published, because they may be subject to third-party copyright. Do not add them to git in any form: not in code, seed data or a database file. Keep draft source files in the `drafts/` folder, which git ignores. Drafts may be deleted only after the project owner has manually reviewed the generated replacements. The `reviewed_at` mark means a human reviewed the row, so do not set it yourself.

## Documentation

- `architecture.md`: architecture details, decisions made and open questions.
- `db_schema.md`: database structure. Update it with every schema change.
- `history.md`: log of completed work. Add an entry after finishing a piece of work.

## Branches

The project uses GitHub Flow, with `dev` as the base branch instead of `main`.

- `dev` is the main development branch and the base for all new branches.
- `main` holds more or less stable versions. Merge `dev` into `main` only when the project owner asks.
- Each change gets one short-lived branch created from `dev` and named `<type>/<short-description>`, where the type is a commit type, for example `feat/answer-check`.
- Do not commit directly to `main` or `dev`. Changes reach them through pull requests.
- Delete a working branch after it is merged.

## Commits

Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/):

```text
<type>(<scope>): <description>

<body>

<footer>
```

- **Type:** `feat`, `fix`, `docs`, `refactor`, `test` or `chore`.
- **Scope:** optional, for example `db`, `web` or `content`.
- **Description:** imperative mood, lowercase first letter, no trailing period, up to 72 characters.
- **Body:** optional, explains why the change is needed.
- **Footer:** optional service lines, for example `Co-Authored-By`.
- **Breaking changes:** mark breaking database schema or API changes with `!` after the type or scope, for example `feat(db)!: ...`.

Examples:

```text
feat(db): add grammar_texts table
fix(web): ignore case when comparing answers
docs: describe commit convention
```

Rules for every commit:

- A commit contains one logical change.
- Entries in `history.md` and edits to `db_schema.md` and `architecture.md` related to the change go into the same commit.
- Before committing, check that no drafts are included.
