# Architecture

## Decisions made

- The first version is a simple web app in Python. Once the web version is debugged, the app moves to iOS.
- The Russian-English version comes first. Other language pairs are considered after it is debugged.
- The app is free, with no monetization. The source code is hosted at https://github.com/krapukhin/grammar-grind.
- The MIT license covers the whole repository, including generated exercises.
- The app is first debugged on drafts: examples and grammar texts are copied as is with `source = 'draft'`. Generated replacements (`source = 'generated'`) are then created, the project owner reviews them manually (the `reviewed_at` mark), and after that the drafts are deleted.
- Drafts are not published and do not go into git.
- Data is stored in SQLite. The structure is described in `db_schema.md`.

## Open questions

- Which web framework should be used?
- How should user answers be stored, and are accounts needed?
- How should automaticity be measured?
- How should exercises be generated?
- How does the user answer exercises?
