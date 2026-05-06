# howznguyen-content

Public content repository for [howznguyen.dev](https://howznguyen.dev). Blog posts and projects live here as MDX files and are fetched by the site at runtime.

**Contributions welcome.** See [Contributing](#contributing) below.

## Structure

```
howznguyen-content/
├── README.md              ← you are here
├── CONTRIBUTING.md        ← contribution guide
├── AGENTS.md              ← AI coding assistant instructions
├── llms.txt               ← AI agent reference
├── posts/                 ← blog posts
│   ├── _template.mdx      ← copy this to start a new post
│   ├── _demo.mdx          ← showcase of all components
│   ├── caddy-server.mdx              ← neutral (language-agnostic)
│   ├── my-post.en.mdx                ← English translation
│   └── my-post.vi.mdx                ← Vietnamese translation
└── projects/              ← project showcases
    ├── _demo.mdx
    └── howznguyen-dev.mdx
```

### Rules

- Filename is the slug. `posts/caddy-server.mdx` → `/blog/caddy-server`
- Only files ending in `.mdx` are fetched
- Files starting with `_` (underscore) are treated as templates and ignored
- No subdirectories inside `posts/` or `projects/`

### Translations

Posts and projects can have translations by adding a locale suffix to the filename:

```
posts/
├── my-post.mdx           # neutral — no language attached
├── my-post.en.mdx        # English
├── my-post.vi.mdx        # Vietnamese
└── my-post.ja.mdx        # Japanese
```

**How it renders:**

- `my-post.mdx` (neutral, no suffix) → `/blog/my-post`
- `my-post.en.mdx` → `/blog/my-post/en`
- `my-post.vi.mdx` → `/blog/my-post/vi`

**Rules:**

- A file **without a suffix** is "neutral" — not tagged with any language. Use this for code-heavy posts, reference docs, or posts where the language isn't relevant.
- A post can have **only the neutral file**, **only localized files**, or **both**.
- If there is no neutral file, `/blog/my-post` serves the first available translation (prefers `en`).
- The language switcher appears automatically when more than one variant exists.
- Supported locale codes: `en`, `vi`, `ja`, `zh`, `ko`, `fr`, `es`, `de`, `pt`, `ru`.

## Frontmatter

### Posts (`posts/*.mdx`)

```mdx
---
title: "Your post title"
description: "Shown in listings, meta tags, and social previews"
date: 2025-01-25
updatedAt: 2025-02-01        # optional
tags:
  - Tag One
  - Tag Two
author: howznguyen             # single GitHub username (no @)
# or for multiple authors:
# authors:
#   - howznguyen
#   - another-github-user
featured: true                 # optional, promotes to hero section on /blog
image: "https://cdn.example.com/cover.png"   # optional but recommended
---
```

| Field         | Type               | Required | Notes                                             |
|---------------|--------------------|----------|---------------------------------------------------|
| `title`       | string             | yes      | Post title                                        |
| `description` | string             | yes      | Short summary, shown on cards and as meta         |
| `date`        | YYYY-MM-DD         | yes      | Publish date                                      |
| `updatedAt`   | YYYY-MM-DD         | no       | Last updated date, shown in post header           |
| `tags`        | string[]           | yes      | First tag becomes the primary category label      |
| `author`      | string             | no       | Single GitHub username (without `@`)              |
| `authors`     | string[]           | no       | Multiple GitHub usernames. Merged with `author`   |
| `featured`    | boolean            | no       | `true` promotes the post to hero position         |
| `image`       | url                | no       | Cover image URL. Shown on card, hero, and OG      |

**About authors:** For each GitHub username, the site fetches the profile (cached 24h) and shows the real name, avatar, and a link to github.com. Multiple authors render as stacked avatars. Usernames must be alphanumeric + hyphens only, max 39 chars.

### Projects (`projects/*.mdx`)

```mdx
---
title: "Project name"
description: "One-liner about the project"
date: 2025-01-01
tags:
  - Next.js
  - TypeScript

# Project classification — renders with a type-specific badge, gradient, and icon
type: open-source          # open-source | side-project | work | experiment | product | writing
status: shipped            # active | wip | shipped | archived | sunset

# Work-specific (shown only for type: work)
role: "Founding Engineer"
company: "Knowns Dev"
year: 2025

featured: true
# single cover (shown on the listing card)
image: "https://cdn.example.com/cover.png"
# optional gallery shown on the project detail page
images:
  - "https://cdn.example.com/screenshot-1.png"
  - "https://cdn.example.com/screenshot-2.png"
  - "https://cdn.example.com/screenshot-3.png"
github: "https://github.com/you/project"    # optional
demo: "https://project.example.com"          # optional
---
```

**Project `type` values:**

| Value          | Icon | Use for                                            |
|----------------|------|----------------------------------------------------|
| `open-source`  | ⭐   | OSS libraries, tools, CLIs you publish/maintain    |
| `side-project` | 🎨   | Personal hobby projects                            |
| `work`         | 💼   | Client or company work (use with role/company/year)|
| `experiment`   | 🧪   | Prototypes, proofs of concept                      |
| `product`      | 🚀   | Launched commercial products, SaaS                 |
| `writing`      | 📝   | Docs, guides, manuscripts                          |

**Project `status` values:**

| Value     | Dot    | Meaning                                  |
|-----------|--------|------------------------------------------|
| `active`  | pulsing green  | Currently being worked on         |
| `wip`     | pulsing blue   | In progress, not yet ready        |
| `shipped` | solid green    | Launched / released               |
| `archived`| solid gray     | No longer maintained              |
| `sunset`  | solid orange   | Officially wound down             |

**Gallery behavior:**

- `image` and `images` are merged + deduplicated.
- The first resolved image is used as the **listing thumbnail**.
- The project detail page shows a gallery with arrows, thumbnails, and a `+N` badge.
- Projects without images fall back to the type gradient + icon.

**Recommended image size:** `3824 × 2474` (aspect ratio ~1.55:1). Keep consistent dimensions across all screenshots in `images` for a clean gallery.

## Writing content

### Standard markdown

All standard markdown works: headings (`##`, `###`), **bold**, *italic*, lists, blockquotes, `inline code`, [links](https://example.com), tables, images.

### Code blocks

Use triple backticks with a language tag for syntax highlighting:

````md
```typescript
const greeting = "Hello"
```
````

Supported languages: `typescript`, `javascript`, `tsx`, `jsx`, `python`, `go`, `rust`, `bash`, `shell`, `json`, `yaml`, `sql`, `graphql`, `dockerfile`, `nginx`, `caddyfile`, `toml`, `md`, `mdx`.

### Custom components

See [`posts/_demo.mdx`](posts/_demo.mdx) for a live example of every component. Quick reference:

- `<Callout type="info|warning|error|success|note|tip" title="...">` — highlighted box
- `<Image src="..." alt="..." caption="..." />` — image with caption + zoom
- `<Bookmark url="..." title="..." description="..." />` — link preview card
- `<Video src="..." />` — YouTube URL auto-detected
- `<TodoList>` + `<Todo checked>` — checklist
- `<Quote color="blue|red|green|...">` — styled blockquote
- `<Code language="..." title="...">` — code block with custom title

## Publishing workflow

1. Fork this repo
2. Copy `posts/_template.mdx` to `posts/your-post-slug.mdx` (or `.en.mdx` / `.vi.mdx` if translating)
3. Write your post
4. Preview at [howznguyen.dev/preview](https://howznguyen.dev/preview)
5. Open a pull request

## Related

- Main site: https://howznguyen.dev
- Preview tool: https://howznguyen.dev/preview
- Content repo: https://github.com/howznguyen/howznguyen-content

## License

You retain copyright to your contributions. By opening a PR, you grant permission for it to be published on howznguyen.dev under the same terms as the rest of the content on the site.
