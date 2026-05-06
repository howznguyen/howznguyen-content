# AGENTS.md

Instructions for AI coding assistants (Cursor, Claude, Copilot, etc.) working in this repository.

## Repository purpose

This is a **content-only repository** for [howznguyen.dev](https://howznguyen.dev). It contains MDX files for blog posts and project showcases. There is no application code here.

If asked to write code (components, utilities, config), **stop and confirm** — this repo only accepts content.

## Acceptable changes

- Creating `.mdx` files in `posts/` or `projects/`
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
_template.mdx                  → ignored
```

- Base filename (minus any locale suffix, minus `.mdx`) = URL slug. Keep kebab-case, ASCII only.
- Files starting with `_` are skipped by the publisher. Use for drafts and templates.
- No nested folders — everything lives flat in `posts/` or `projects/`.
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
