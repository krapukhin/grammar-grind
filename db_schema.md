# Database structure

DBMS: SQLite.

SQLite does not check foreign keys by default, so `PRAGMA foreign_keys = ON` must be executed on every connection.

## `source` values

The `source` column exists in `grammar_texts` and `prompts`:

- `draft`: a draft copied as is. It is used for debugging and deleted after the replacements have been manually reviewed.
- `generated`: a generated replacement.

## Reviewing replacements

The `reviewed_at` column exists in `grammar_texts` and `prompts`. It stores the date and time of the manual review in UTC, for example `2026-11-04 20:15:00`. An empty value means the row has not been reviewed. Reviewing a prompt also covers its answers.

Drafts may be deleted only when neither table has rows left with `source = 'generated'` and an empty `reviewed_at`.

## Tables

### `topics`

A tree of grammar topics. A top-level topic has an empty `parent_id`. The full name of a topic is made up of the titles along the path from the root, for example `Tenses / Present / Present Simple`.

The tree currently has three levels, but the schema does not limit its depth. Prompts are attached to bottom-level topics. The schema does not check this, so the code must.

The `topics_root_title` index is needed because in SQLite `UNIQUE (parent_id, title)` does not prevent duplicate titles when `parent_id` is empty.

```sql
CREATE TABLE topics (
    id         INTEGER PRIMARY KEY,
    parent_id  INTEGER REFERENCES topics(id),
    title      TEXT NOT NULL,
    UNIQUE (parent_id, title)
) STRICT;

CREATE UNIQUE INDEX topics_root_title ON topics(title) WHERE parent_id IS NULL;
```

### `grammar_texts`

Texts explaining the grammar. A topic can have one text of each kind, so a draft and its replacement are stored side by side while the replacement is being reviewed. Intermediate levels of the tree may have no text.

```sql
CREATE TABLE grammar_texts (
    id           INTEGER PRIMARY KEY,
    topic_id     INTEGER NOT NULL REFERENCES topics(id),
    text         TEXT NOT NULL,
    source       TEXT NOT NULL CHECK (source IN ('draft', 'generated')),
    reviewed_at  TEXT,
    UNIQUE (topic_id, source)
) STRICT;
```

### `prompts`

Prompts shown to the user. Each prompt belongs to one topic.

`source_number`: purpose to be specified. May be empty.

```sql
CREATE TABLE prompts (
    id             INTEGER PRIMARY KEY,
    topic_id       INTEGER NOT NULL REFERENCES topics(id),
    text           TEXT NOT NULL,
    source         TEXT NOT NULL CHECK (source IN ('draft', 'generated')),
    source_number  INTEGER,
    reviewed_at    TEXT,
    UNIQUE (topic_id, text)
) STRICT;
```

### `answers`

Accepted answers that the user's answer is compared with. A prompt can have several answers. The topic and origin of an answer are determined through its prompt. Deleting a prompt automatically deletes its answers.

```sql
CREATE TABLE answers (
    id         INTEGER PRIMARY KEY,
    prompt_id  INTEGER NOT NULL REFERENCES prompts(id) ON DELETE CASCADE,
    text       TEXT NOT NULL,
    UNIQUE (prompt_id, text)
) STRICT;
```
