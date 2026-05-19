# AGENTS.md

Instructions for AI coding assistants (Cursor, Claude, Copilot, etc.) working in this repository.

## Repository purpose

This is a **content-only repository** for [howznguyen.dev](https://howznguyen.dev). It contains MDX files for blog posts and project showcases. There is no application code here.

If asked to write code (components, utilities, config), **stop and confirm** — this repo only accepts content.

## Acceptable changes

- Creating `.mdx` files in `posts/` or `projects/`
- Creating series folders in `series/` with `_meta.mdx` + chapter files
- Editing frontmatter or body of existing `.mdx` files
- Adding images to `assets/`
- Updating `README.md` / `AGENTS.md` / `llms.txt`

Not acceptable here:
- Code files (`.ts`, `.js`, `.tsx`, etc.)
- Build config, `package.json`
- Subdirectories inside `posts/` or `projects/`

## Filename rules

```
posts/my-new-post.mdx          → /blog/my-new-post         (neutral)
posts/my-new-post.en.mdx       → /blog/my-new-post/en      (English)
posts/my-new-post.vi.mdx       → /blog/my-new-post/vi      (Vietnamese)
projects/my-project.mdx        → /projects/my-project
series/my-series/_meta.mdx     → /series/my-series          (series metadata)
series/my-series/01-chapter.mdx → rendered on /series/my-series
_template.mdx                  → ignored
```

- Base filename (minus any locale suffix, minus `.mdx`) = URL slug. Keep kebab-case, ASCII only.
- Files starting with `_` are skipped by the publisher (except `_meta.mdx` inside series folders). Use for drafts and templates.
- Folders starting with `_` inside `series/` are ignored (use for demos/drafts).
- No nested folders — everything lives flat in `posts/` or `projects/`.
- `series/` uses subdirectories: one folder per series.
- Locale suffix comes directly before `.mdx`: `.en.mdx`, `.vi.mdx`, `.ja.mdx`, etc.
- A file **without** a locale suffix is "neutral" — no language attached.
- Supported locale codes: `en`, `vi`, `ja`, `zh`, `ko`, `fr`, `es`, `de`, `pt`, `ru`.

## Frontmatter

### Posts

```yaml
---
title: "Your title here"
description: "One-sentence summary, ≤ 160 chars"
date: 2026-05-06
updatedAt: 2026-05-10        # optional
tags:
  - Tag One
  - Tag Two
author: howznguyen             # single GitHub username (no @)
# or for multiple authors:
# authors:
#   - howznguyen
#   - co-author-username
featured: false                # optional
image: "https://cdn.example.com/cover.png"   # optional
---
```

### Projects

```yaml
---
title: "Project name"
description: "What it does, one sentence"
date: 2026-01-01
tags:
  - Next.js
  - TypeScript

# Classification — controls badge, gradient, and icon on the site
type: open-source            # open-source | side-project | work | experiment | product | writing
status: shipped              # active | wip | shipped | archived | sunset

# Work-specific fields (use with type: work)
role: "Founding Engineer"
company: "Knowns Dev"
year: 2025

featured: false
image: "https://cdn.example.com/cover.png"
images:                                      # optional gallery (3824×2474 recommended)
  - "https://cdn.example.com/shot-1.png"
  - "https://cdn.example.com/shot-2.png"
github: "https://github.com/user/repo"      # optional
demo: "https://project.example.com"         # optional
authors:                                    # optional GitHub usernames
  - howznguyen
---
```

### Authors

The site supports one or multiple GitHub users as authors:

- `author: "<github-username>"` — single author
- `authors: ["user1", "user2"]` — multiple authors
- Leading `@` is stripped. Usernames must be alphanumeric + hyphens, max 39 chars.
- For each username the site fetches the GitHub profile (cached 24h) for name, avatar, and profile link.
- Display names with spaces in `author` are invalid and fall back to the site owner.

### Project type and status

`type` controls the visual treatment. Default is `side-project`.

| type           | icon | use for                                               |
|----------------|------|-------------------------------------------------------|
| `open-source`  | ⭐   | OSS libraries, tools, CLIs                            |
| `side-project` | 🎨   | Personal hobby projects                               |
| `work`         | 💼   | Client or company work — add role/company/year        |
| `experiment`   | 🧪   | Prototypes, proofs of concept                         |
| `product`      | 🚀   | Launched commercial products, SaaS                    |
| `writing`      | 📝   | Docs, guides                                          |

`status` adds a colored indicator:

- `active` — pulsing green
- `wip` — pulsing blue
- `shipped` — solid green
- `archived` — gray
- `sunset` — orange

### Validation rules

- `title`, `description`, `date`, `tags` are required.
- `date` must be ISO-8601 `YYYY-MM-DD` (no time).
- `tags` must be an array, not comma-separated.
- URLs in `image`, `github`, `demo` must be absolute HTTPS.

### Series (`series/<slug>/_meta.mdx`)

```yaml
---
title: "Series Title"
description: "What this series covers"
date: 2026-05-19
updatedAt: 2026-05-19           # optional
tags:
  - nextjs
  - react
featured: false                  # optional
image: "https://cdn.example.com/cover.png"  # optional
status: active                   # active | complete | paused | archived
---
```

### Series chapters (`series/<slug>/NN-chapter-slug.mdx`)

```yaml
---
title: "Chapter Title"
description: "Brief chapter summary"  # optional
---
```

Chapter rules:
- Filename must start with a numeric prefix for ordering: `01-`, `02-`, etc.
- Body headings start at `##` (chapter title is rendered from frontmatter, not from body).
- All custom components (Callout, Image, Video, etc.) are available.
- Each series folder must have exactly one `_meta.mdx`.
- Folders starting with `_` inside `series/` are ignored.

## Available MDX components

Globally available inside any MDX file. **Do not add import statements.**

### `<Callout>`
- `type`: `info` (default) | `warning` | `error` | `success` | `note` | `tip`
- `title`: optional per-type default
- `emoji`: optional override

### `<Image>`
- `src` (required), `alt` (recommended), `caption` (optional)
- Click-to-zoom enabled automatically

### `<Bookmark>`
- `url` (required), `title`, `description`, `icon`, `cover`

### `<Video>`
- `src` (required). YouTube URLs auto-detected.

### `<TodoList>` + `<Todo>`
- `<Todo checked>` for completed items. Read-only.

### `<Quote>`
- `color`: `gray` (default) | `red` | `blue` | `green` | `yellow` | `orange` | `purple`

### `<Code>`
- Props: `language`, `title`, `showLineNumbers`. Children as template literal.
- Prefer fenced code blocks; use `<Code>` only when you need a custom title.

### `<TutorialSteps>` + `<Step>` + `<Highlight>`
- Interactive image-based tutorial walkthrough with pixel-precise animated highlights
- `TutorialSteps`: `autoAdvance` (optional, ms between steps, pauses on hover)
- `Step`: `image` (URL), `width` (original px), `height` (original px)
- `Highlight`: `x`, `y` (pixel in original image), optional `w`, `h` (rectangle), `label` (tooltip on hover)
- Without `w`/`h` → pulsing circle. With `w`/`h` → highlighted rectangle region.
- Supports keyboard navigation (arrow keys) and clickable step dots

## Code blocks

Always specify a language:

````md
```typescript
const greeting = "Hello";
```
````

Supported: `typescript`, `javascript`, `tsx`, `jsx`, `python`, `go`, `rust`, `bash`, `shell`, `json`, `yaml`, `sql`, `graphql`, `dockerfile`, `nginx`, `caddyfile`, `toml`, `md`, `mdx`, `text`.

## Task workflow for the AI agent

### Creating a new post

1. Confirm the target slug and language.
2. Compute file path:
   - Neutral: `posts/<slug>.mdx`
   - Translation: `posts/<slug>.<locale>.mdx`
3. Check whether a file at that path already exists. If yes, ask before overwriting.
4. Fill frontmatter. Leave `featured: false` unless asked. Use `author: howznguyen` as default.
5. For a co-authored post, use the `authors` array.
6. Write body with standard markdown + available components.
7. Prefer fenced code blocks over `<Code>`.
8. End the file with a newline.
9. Do not commit or push — leave that to the user.

### Adding a translation of an existing post

1. Read the existing neutral or default-locale file for context.
2. Create `posts/<base-slug>.<locale>.mdx`.
3. Mirror frontmatter from the original (same `slug`, `date`, `tags`, etc.).
4. Translate `title`, `description`, body content. Leave code blocks unchanged.
5. Do NOT add a `locale` field to frontmatter — the locale comes from the filename.

### Updating an existing post

1. Read the full file first.
2. Preserve frontmatter fields the user did not ask to change.
3. For substantial changes, update `updatedAt`.
4. Do not reformat untouched sections.

### Adding a project

1. File goes under `projects/`.
2. Ask for `type`: `open-source`, `side-project`, `work`, `experiment`, `product`, `writing`. If unclear, use `side-project`.
3. For `type: work`, also ask for `role`, `company`, `year`.
4. Ask for `status` if known.
5. Include `github` and `demo` URLs if provided.
6. If multiple screenshots, use the `images` array. First image is listing cover.

### Creating a series

1. Confirm the series slug (folder name, kebab-case).
2. Create `series/<slug>/` directory.
3. Create `series/<slug>/_meta.mdx` with frontmatter (title, description, date, tags, status).
4. Create chapter files with numeric prefix: `01-chapter-slug.mdx`, `02-chapter-slug.mdx`, etc.
5. Each chapter has minimal frontmatter: `title` (required), `description` (optional).
6. Chapter body starts at `##` — do not use `#` (the title comes from frontmatter).
7. All chapters are rendered on a single page at `/series/<slug>`.
8. Do not commit or push — leave that to the user.

### Adding a chapter to an existing series

1. Read existing chapters to determine the next numeric prefix.
2. Create `series/<slug>/NN-chapter-slug.mdx`.
3. Follow the same frontmatter and heading rules as above.

## Preview

Authors can preview without pushing at https://howznguyen.dev/preview:
- Upload an `.mdx` file
- Paste raw MDX
- Enter a raw GitHub URL (e.g. from a fork branch)

Recommend this step to the user after creating or modifying a file.

## Things to avoid

- Do not add JavaScript/TypeScript files
- Do not add `import` statements inside MDX
- Do not use HTML tags that aren't listed above
- Do not commit large binaries — use a CDN
- Do not guess slugs from titles without confirming the computed slug
- Do not use display names in the `author` field (GitHub usernames only)
- Do not put locale info in frontmatter — it comes from the filename suffix

## Questions to ask before acting

If information is missing:
- Target slug (filename)?
- Neutral (no suffix) or a specific locale (`.en`, `.vi`, etc.)?
- Tags?
- Author: GitHub username? Multiple co-authors?
- Featured?
- Cover image URL?
- `updatedAt` on edits?
- For projects: type, status, and work-specific fields if applicable?

<!-- KNOWNS GUIDELINES START -->

**CRITICAL: You MUST read and follow `KNOWNS.md` in the repository root before doing any work. It is the canonical source of truth for all agent behavior in this project.**

## Canonical Guidance

- Knowns is the repository memory layer for humans and the AI-friendly working layer for agents.
- The source of truth for repo-level agent guidance is `KNOWNS.md`.
- Read `KNOWNS.md` first whenever the runtime supports reading repository files.
- Load behavior, memory policy, and workflow rules from `KNOWNS.md`; treat this file only as a compatibility entrypoint.
- If this file and `KNOWNS.md` differ, follow `KNOWNS.md`.

## Minimum Rules

- Use Knowns as the canonical system for tasks, docs, templates, and workflow state.
- Never manually edit Knowns-managed task or doc markdown.
- Search first, then read only relevant docs and code.
- Use `search` for discovery; use MCP `retrieve` tool when a workflow needs structured context with citations. Fall back to CLI `knowns retrieve` if MCP is unavailable.
- For code context retrieval, prefer MCP tools over CLI: use `code({ action: "search" })` first, then `code({ action: "symbols" })`, then `code({ action: "deps" })`. Treat CLI `knowns code ...` as fallback for manual inspection or debugging.
- Plan before implementation unless the user explicitly overrides that workflow.
- Validate before considering work complete.
- Use memory tools: `memory({ action: "list" })` at session start, `memory({ action: "add" })` after tasks for reusable knowledge.
- Proactively capture durable memory based on `KNOWNS.md` memory rules; do not wait for an explicit user instruction to save memory when scope and durability are clear.

## Quick Reference

```bash
knowns doc list --plain               # List docs
knowns task list --plain              # List tasks
knowns task <id> --plain              # View task
knowns doc "<path>" --plain --smart  # View doc
knowns search "query" --plain        # Search docs/tasks
knowns retrieve "query" --json      # Retrieve structured context pack (CLI fallback)
```

<!-- KNOWNS GUIDELINES END -->
