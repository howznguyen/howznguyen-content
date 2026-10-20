# journey/

One file per milestone. The site's **Journey** page (`/journey`) reads this
folder and nothing else: there is no separate list to keep in sync.

## File shape

`<slug>.mdx`. The frontmatter places the entry on the timeline; the body is the
entry, written like a post with standard Markdown and the usual MDX components.
A file with no body falls back to its `description`, which is all a short
milestone needs. Files whose name starts with `_` are skipped.

```mdx
---
title: "Knowns"
date: "2025-12-25"
description: "A local-first memory layer that keeps project tasks, docs, decisions, and reusable knowledge connected for people and AI coding agents."
href: "/projects/knowns-sh"
linkLabel: "View project"
---
```

## Translations

`<slug>.<locale>.mdx` is a translation of `<slug>.mdx`, for example
`knowns.vi.mdx`. The site groups the pair into one milestone: `/journey` shows
every milestone, and `/journey/<locale>` shows only the ones translated into
that language. A language shows up on the site as soon as one file carries it,
so translating a single milestone is enough to turn `/journey/vi` on.

The locale has to be one the site knows. It is read from the filename, and the
last dot-separated segment is only treated as a language when it matches a
supported code (`vi` today).

A translation stands alone rather than inheriting anything, so reproduce the
fields the original had:

- Keep `date` **identical** to the original. It is what places both versions on
  the timeline, and a mismatched one puts the same milestone in two years.
- Translate `title`, `description`, `linkLabel`, and `images[].alt`.
- Leave `href` and `x` as they are. They point at the same thing in every
  language.
- The body is the body. Translate it, and keep the same MDX components.

An entry with no translation is simply absent from `/journey/<locale>` rather
than shown in another language.

## Fields

| Field | Required | Notes |
| --- | --- | --- |
| `title` | yes | The milestone's heading. A product or release name reads better than a sentence. |
| `date` | yes | ISO day, `YYYY-MM-DD`. The year in it groups the entry, and the full date orders the list. |
| `description` | no | One sentence: what shipped, and why it mattered. Avoid marketing copy. It is the fallback for a file with no body, so a short milestone can be frontmatter only. |
| `images` | no | Photos for the milestone: a list of `src` + `alt`. They stack under the description. |
| `x` | no | An X post to embed: the status URL, or just the numeric id. |
| `href` | no | A route on the site (`/projects/…`) or an external URL. A route renders as an internal link; anything else opens in a new tab with an ↗. |
| `linkLabel` | no | Link text. Defaults to `View`. Use `View project` when it points at a project page. |
| `order` | no | Integer, lower first. Only breaks ties: the date is what orders the page. |

## Media

Both fields are optional and independent. The entry reads title, description,
photos, the embedded post, then the link.

```mdx
---
title: "Got my very first 3D printer"
date: "2025-11-02"
description: "A Bambu Lab A1 Mini, and a desk drawer organizer as its first job."
images:
  - src: "https://res.cloudinary.com/…/organizer-1.jpg"
    alt: "The drawer organizer in place, holding pliers and screwdrivers."
  - src: "https://res.cloudinary.com/…/organizer-2.jpg"
    alt: "The same organizer from above, showing the printed dividers."
x: "https://x.com/howznguyen/status/1234567890123456789"
---
```

- `alt` is not decoration: it is what a screen reader announces and what shows
  when the image fails. Say what the photo shows.
- Images are rendered at their own aspect ratio. The page reserves a 3:2 box
  until the file lands, so a very tall or very wide photo will nudge the entry
  slightly as it loads. Keep photos roughly landscape and it won't.
- The embedded post is fetched from X's public syndication API and cached with
  the page. If the post is deleted or protected, the entry falls back to a plain
  link, so an entry never breaks because of someone else's post.
- `x` is frontmatter on purpose. The body renders in the browser, and the embed
  is fetched while the page is built, so an `<XPost>` written into the body
  appears in the first paint and then disappears once the page hydrates. Photos
  have no such constraint: `<Image>` in the body works, and `images:` does too.

## How it renders

Entries are grouped under the year of their `date`, newest year first and newest
entry first inside it: the year sits in a column beside the timeline, each entry
hangs off a 20px ring marker with an 8px dot, and a dashed line connects one
entry to the next, stopping at the last entry of the year. Each entry is its title,
then the body when the file has one (otherwise the one-line `description`),
photos, the embedded post, and the link when `href` is set. The body renders
through the same MDX pipeline as a post, so components like `<Callout>` and
`<Image>` work and glossary terms resolve.

## Publishing

The site reads this repo from `main` through the GitHub API, so a new file shows
up after it is pushed (or after the content webhook fires). Nothing needs to be
added to the site repo. When running the site locally with
`CONTENT_LOCAL_DIR=<this checkout>`, an edit shows on the next reload without a
push.
