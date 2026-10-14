# jor.dev conventions

The base rules every jor.dev project follows. The look is in the [brand](https://github.com/JorSanders/brand), this is everything else: how a project is built, laid out, documented and committed. Each project carries a `CLAUDE.md` made from `CLAUDE.template.md` and a `README.md` shaped like `README.template.md`, so the rules travel with the repository.

Three kinds of project live here:

| Kind | Projects | Ships as |
| --- | --- | --- |
| Static site | Resume, game-sens, qr-maker, chess-sweeper, Nightfarers, Sorceries | GitHub Pages at `<name>.jor.dev` |
| App | ps-battery | GitHub release and Microsoft Store |
| Tooling | Scripts, conventions | Used locally |
| Static site | brand | GitHub Pages at brand.jor.dev, also the source every other site copies from |

## Static sites

- **Frameworkless.** Plain HTML, CSS and JavaScript. No React, no Vue, no Next.js, no Tailwind, no TypeScript. Browser features are enough, target the current versions of Chrome, Firefox and Safari.
- **Buildless.** The repository is the site. Serving the folder with any static server is the whole development setup, and GitHub Pages serves the same files. No bundler, no transpiler, no CSS preprocessor, no `dist/` or `docs/` output folder.
- **Dev tooling is allowed, build steps are not.** A `package.json` with devDependencies is fine for scripts that generate data or assets, like chess-sweeper's position validator or the brand's logo generator. The output of those scripts is committed, and the site must work without ever running them.
- **Dependencies are vendored.** A runtime library goes in `vendor/` as-is with its license file next to it, like `vendor/jsQR.js`. No npm runtime dependencies, no CDN script tags. Fonts go in `fonts/` with their license file.
- **Relative paths.** Links, scripts, styles and images use `./` paths so the site works at a custom domain and under a `/repo/` path. Only canonical, Open Graph and sitemap URLs are absolute.
- **ES modules.** JavaScript is loaded with `<script type="module" src="./src/main.js">`. Split by concern into small files under `src/`. A single `index.html` with inline style and script is fine while the page is small, split it once the inline CSS or JS passes about 200 lines.
- **Run locally** with `python3 -m http.server 8000` from the repository root, ES modules do not load from `file://`.

### Layout

```
index.html            the page, with the standard head below
style.css             styles, built on brand/tokens.css and base.css
src/main.js           entry module, more modules next to it
brand/                copied by brand/scripts/install.sh: tokens.css, base.css, theme.js, fonts/, logo/
fonts/                other self hosted fonts with their license files, if any
vendor/               runtime libraries as-is with their license files
scripts/              dev only node scripts, if any, with package.json at the root
favicon.svg           from brand/logo/icon.svg
favicon.ico           32x32 from brand/logo/png/
apple-touch-icon.png  180x180 from brand/logo/png/
og-image.png          1200x630 social preview
CNAME                 <name>.jor.dev
.nojekyll             disables Jekyll on GitHub Pages
robots.txt            crawler policy with the sitemap URL
sitemap.xml           sitemap
README.md             see README.template.md
CLAUDE.md             see CLAUDE.template.md
LICENSE               the Unlicense
.editorconfig         copy of the one here
```

### Standard head

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Name</title>
  <meta name="description" content="One sentence." />
  <link rel="canonical" href="https://name.jor.dev/" />
  <meta name="color-scheme" content="light dark" />
  <meta name="theme-color" content="#f3f3f3" media="(prefers-color-scheme: light)" />
  <meta name="theme-color" content="#121417" media="(prefers-color-scheme: dark)" />
  <meta property="og:type" content="website" />
  <meta property="og:url" content="https://name.jor.dev/" />
  <meta property="og:title" content="Name" />
  <meta property="og:description" content="One sentence." />
  <meta property="og:image" content="https://name.jor.dev/og-image.png" />
  <meta name="twitter:card" content="summary_large_image" />
  <link rel="icon" type="image/svg+xml" href="./favicon.svg" />
  <link rel="icon" type="image/x-icon" sizes="32x32" href="./favicon.ico" />
  <link rel="apple-touch-icon" href="./apple-touch-icon.png" />
  <link rel="preload" href="./brand/fonts/Montserrat-Regular.woff2" as="font" type="font/woff2" crossorigin />
  <link rel="stylesheet" href="./brand/tokens.css" />
  <link rel="stylesheet" href="./brand/base.css" />
  <link rel="stylesheet" href="./style.css" />
  <script src="./brand/theme.js"></script>
  <script type="module" src="./src/main.js"></script>
</head>
```

The body starts with the shared site header, the wordmark and the color scheme picker. The markup is in `../brand/README.md` under "Site header". The page's own header with its title comes after it.

Add JSON-LD structured data when the page is an article or a tool. The resume keeps `color-scheme: light` and drops the dark theme-color, it is an A4 sheet.

### Accessibility and SEO

- Semantic HTML, one `h1`, headings in order, landmarks (`main`, `nav`, `header`, `footer`).
- Every form control has a label. Live regions for status text. Visible focus states on everything interactive, the brand focus ring does this.
- WCAG AA contrast in light and dark mode. The brand tokens are checked, project colors must be too.
- Never rely on color alone to convey meaning. Add a shape, icon or text.
- Keyboard: everything a mouse can do, the keyboard can do.
- Title, description, canonical, Open Graph and Twitter tags, sitemap and robots stay in sync with the content. When the page changes, they change.

### Deployment

GitHub Pages from the `main` branch, root folder, no workflow. The repository settings point Pages at `main` and `/`, `CNAME` holds `<name>.jor.dev`, and a DNS CNAME record for `<name>` points at `jorsanders.github.io`. Pushing to `main` deploys. No versions, no changelog, no releases for sites.

## Apps

ps-battery is the model: `README.md` as described below, `CLAUDE.md` with the shared block and the language rules, `CHANGELOG.md` and tags from release-please with conventional commits, a `check` workflow that formats, lints and tests on every push and pull request, a `release` workflow that builds the artifacts and signs them with Sigstore, Dependabot for dependencies and actions, and actions pinned to commit SHAs. A `PRIVACY.md` when the app runs on someone else's machine.

## Code style

- Readable, maintainable and simple. Prefer the boring solution.
- Self explanatory names, no abbreviations unless a name would get way too long. `controller` not `ctrl`, `index` not `i` outside a tight loop.
- No comments unless they explain unexpected behaviour or code. Names and structure do the explaining.
- Handle every error case. No empty catch blocks, no ignored results.
- Formatting follows `.editorconfig`: 2 spaces, LF, final newline, 4 spaces for shell and Rust. JavaScript uses double quotes, semicolons and trailing commas in multiline lists, which is what Prettier's defaults produce, so `npx prettier` can format without a config file.
- Rust passes Clippy with no warnings and `rustfmt`. Shell scripts are POSIX and have a `--help` flag.
- Images are stripped of metadata before committing.

## Git

- Branch `main`.
- Conventional commits: `type: imperative summary` in lowercase. Types: `feat`, `fix`, `docs`, `style`, `refactor`, `chore`, `ci`. One feature or one fix per commit, so the history reads as a changelog and release-please can build one.
- Commit the generated output of dev scripts together with the change that caused it.

## Writing

- Casual first person, as if I am sharing something I made or found. Not a manual, not marketing copy.
- Honest about limits: what is untested, what is community data, what I do not know. The ps-battery disclaimer is the tone.
- Never use em dashes. Use a comma, colon, parentheses or a new sentence.
- Titles and headings are sentence case: one capital at the start, never all caps, never title case, in text and in CSS.
- Dutch names and places are fine, the text itself is in English.

## README

Every README follows `README.template.md`, which is the ps-battery README with the project specific parts marked. The fixed order is: title, optional hero image, one paragraph of why, the live URL for sites, a numbered Features list with screenshots, project specific explanation sections, how to get or run it, Files for sites, how to regenerate generated things, Privacy and Disclaimer when they apply, then the two fixed closing sections. Sections that do not apply are left out, the order never changes.

## CLAUDE.md

Every project has a `CLAUDE.md` with the shared block from `CLAUDE.template.md` verbatim, then a `This project` section with what is specific. When a shared rule changes, change the template and every project.

## License

Everything is released under the Unlicense, the `LICENSE` file here goes in every repository. Vendored libraries and fonts keep their own licenses next to the files.

## Where the projects stand

| Project | Fits | To do |
| --- | --- | --- |
| game-sens | Buildless, vanilla, Pages from root | Vendor Chart.js instead of the cdnjs tag, add LICENSE, conventional commits from now on, add `.editorconfig` |
| chess-sweeper | Buildless, ES modules, node only for data scripts | Add LICENSE and `.editorconfig`, brand tokens for the chrome |
| qr-maker | Buildless, jsQR vendored with license | Drop the Actions deploy workflow and serve `main` root directly, add LICENSE, favicon set from brand |
| Resume | Montserrat, minimal | Drop Vite: move `public/*` to the root, delete `docs/` and `package.json`, make the font paths relative, point Pages at root. Stays light only |
| Nightfarers, Sorceries | Single file, Pages from root | Add README, CLAUDE.md, LICENSE, favicon and og-image, relative paths check. Split CSS and JS out once they grow |
| ps-battery | The model for apps and READMEs | Nothing |
| Scripts | POSIX, `--help`, Super-Linter | Rename `claude.md` to `CLAUDE.md`, rename branch `master` to `main` |
