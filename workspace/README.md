# workspace/

One file per thing on the desk. The site's **Workspace** page (`/workspace`)
reads this folder and nothing else.

## File shape

`<slug>.mdx`, frontmatter only. The body is ignored, so an empty file after the
frontmatter is correct. Files whose name starts with `_` are skipped.

```mdx
---
name: "Apple Studio Display"
category: hardware
specs: "Tilt- and height-adjustable stand, nano-texture glass"
url: "https://www.apple.com/studio-display/"
buy: "https://www.apple.com/shop/buy-mac/studio-display"
since: 2023
order: 10
---
```

## Fields

| Field | Required | Notes |
| --- | --- | --- |
| `name` | yes | Shown as the row's heading. |
| `category` | yes | `hardware`. Anything else falls back to `hardware`. |
| `description` | no | A line about what it is doing here. The table is usually enough without one, so most entries leave it out. |
| `specs` | no | The configuration that identifies this exact one: colour, chip, memory, size. Rendered in the Specs column. |
| `url` | no | Product or docs page. When present the name becomes a link (opens in a new tab). |
| `buy` | no | Where to buy it. Rendered as a `Buy` link in the last column. |
| `since` | no | Year it arrived. Rendered as `since 2022`. |
| `order` | no | Integer, lower first. Ties break alphabetically by `name`. Default is 100. |

## How it renders

The page is one **Hardware** table. Each row is the name (linked when `url` is
set), the optional description, `since <year>`, and the **Specs** and **Buy**
columns, which appear only when at least one entry sets them.

## Publishing

The site reads this repo from `main` through the GitHub API, so a new file shows
up after it is pushed (or after the content webhook fires). Nothing needs to be
added to the site repo.
