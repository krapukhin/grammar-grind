# History

Log of completed work in the project. New entries are added at the top.

## 2026-11-05

- Added a commit convention to `AGENTS.md`.
- Decided that commit messages and all repository text are in English, except learner-facing Russian content. Translated the existing documents.
- Created the GitHub repository `krapukhin/grammar-grind`, initialized git and added `.gitignore`. Draft source files go into the ignored `drafts/` folder.
- Adopted GitHub Flow, with `dev` as the main development branch and more or less stable versions rolled out to `main`.

## 2026-11-04

- Created `AGENTS.md` and `CLAUDE.md`.
- Created `history.md`, `architecture.md` and `db_schema.md`.
- Decided to build the first version as a simple web app in Python with a database.
- Decided to start with the Russian-English version and move the app to iOS after the web version is debugged.
- Decided to make the app free, open source on GitHub and without monetization.
- Added `LICENSE` (MIT). The license covers code and exercises.
- Chose SQLite as the DBMS. Publishing on GitHub is postponed; development is local for now.
- Described the `topics`, `prompts` and `answers` tables in `db_schema.md`. Tested the schema on in-memory SQLite.
- Organized topics in `topics` into a tree through `parent_id` and added `source` and `source_number` to `prompts`. Tested the schema on in-memory SQLite.
- Decided to debug the app on textbook examples and then replace them with generated ones.
- Decided to write grammar texts in our own words instead of copying them from the textbook.
- Reversed the previous decision: examples and grammar texts are first copied as is with `source = 'draft'`, then replaced with generated ones, and the drafts are deleted after a manual review.
- Moved grammar texts from `topics` to a `grammar_texts` table and replaced the `textbook` value with `draft`. Tested the replacement workflow on in-memory SQLite.
- Added a `reviewed_at` column to `grammar_texts` and `prompts` to mark the manual review of replacements.
