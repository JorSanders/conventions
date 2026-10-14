# CLAUDE.md

<!-- Shared block, keep identical across projects. Source: conventions/CLAUDE.template.md -->

## Shared rules

- Focus on writing readable, maintainable and simple code. Prefer the boring solution.
- All variables should have self explanatory names. Don't use abbreviations unless a name would become way too long.
- Don't add comments unless absolutely needed to explain unexpected behaviour or code.
- Always handle every error case.
- Use conventional commits, lowercase `type: imperative summary`. Each commit adds exactly one feature or fixes exactly one bug.
- The information in README.md must always be up to date and follow the shared README structure: title, why, features, project specific sections, how to get or run it, then "Any issues?" and "Buy me a coffee".
- Write all text casually in first person, as me sharing something I made or found. Not a manual, not marketing copy. Be honest about what is untested or unknown.
- Never use em dashes. Rewrite the sentence, or use a comma, colon or parentheses instead.
- Titles and headings are sentence case: one capital at the start, never all caps, never title case. No `text-transform: uppercase` anywhere.
- Any images should be cleaned of their metadata before committing.
- Formatting follows `.editorconfig`: 2 spaces, LF, final newline. JavaScript uses double quotes and semicolons, like Prettier's defaults.
- Everything is Unlicense. Vendored libraries and fonts keep their own license file next to them.

<!-- End of shared block -->

## This project

- The base rules every jor.dev project follows, plus the templates every project's `CLAUDE.md` and `README.md` are made from. Nothing here runs, it is documentation.
- When a shared rule changes, change `CLAUDE.template.md` here first, then every project's `CLAUDE.md` copy of the shared block.
- The style rules live in the brand repository (https://github.com/JorSanders/brand), this one links to it instead of repeating it.
