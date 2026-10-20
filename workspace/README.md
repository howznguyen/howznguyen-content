# workspace/

One file per thing on the desk, in the machine, or running as a service. The
site's **Workspace** page (`/workspace`) reads this folder and nothing else.

## File shape

`<slug>.mdx`, frontmatter only. The body is ignored, so an empty file after the
frontmatter is correct. Files whose name starts with `_` are skipped.

```mdx
---
name: "Apple Studio Display"
category: hardware
description: "One screen for code, design, and calls."
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
| `description` | yes | One sentence: what it is and what it is doing here. |
| `category` | yes | `hardware`, `software`, or `service`. Anything else falls back to `software`. |
| `specs` | no | The configuration that identifies this exact one: colour, chip, memory, size. Rendered in the Specs column. |
| `url` | no | Product or docs page. When present the name becomes a link (opens in a new tab). |
| `buy` | no | Where to buy it. Rendered as a `Buy` link in the last column. |
| `since` | no | Year it arrived. Rendered as `since 2022`. |
| `order` | no | Integer, lower first. Ties break alphabetically by `name`. Default is 100. |

## How it renders

The page groups rows in this order: **Hardware → Software → Services**, and a
category with no entries is left out. Each row is the name, the one-line
description, and `since <year>`. The **Specs** and **Buy** columns appear only
when at least one entry sets them.

## Publishing

The site reads this repo from `main` through the GitHub API, so a new file shows
up after it is pushed (or after the content webhook fires). Nothing needs to be
added to the site repo.
