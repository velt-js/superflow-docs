# Superflow Docs

The source for [docs.usesuperflow.com](https://docs.usesuperflow.com), built with [Mintlify](https://mintlify.com).

## Structure

Navigation lives in `docs.json`. Five tabs, in order:

| Tab | What's in it | Folders |
| --- | --- | --- |
| **Start Here** | Home, quickstart, video library | `index.mdx`, `quickstart.mdx`, `watch.mdx` |
| **Agents & AI** | Agents overview, building, running, AI Co-Pilot | `agents/` |
| **Install** | One page per platform or framework | `no-code-platforms/`, `web-frameworks/` |
| **Guides** | Toolbar, comments, dashboard, files, integrations | `how-to-guides/`, `dashboard/`, `files/`, `Integrations/` |
| **Reference** | Features, billing, security, REST API, updates | `product-features/`, `billing/`, `security/`, `rest-apis/`, `product-updates/` |

Assets: screenshots in `images/<section>/<page-slug>/`, screen recordings in `videos/`.

## Writing conventions

These are what make the docs work for people who would rather not read:

- **Media first.** If a page has a video, it goes directly under the frontmatter, above the written steps. Install pages open with the YouTube walkthrough; agent pages open with a screen recording.
- **Steps, not numbered paragraphs.** Procedures use `<Steps>` / `<Step title="…">`. The step title carries the instruction, so it is scannable without reading the body.
- **A screenshot per step.** Wrap it in `<Frame>` and always give the image alt text: `![What the screenshot shows](/images/…)`.
- **Responsive embeds.** Videos and iframes use `className="w-full aspect-video rounded-xl"` — never fixed pixel width and height.
- **Frontmatter on every page.** `title`, `description` and `icon`. The description is what shows in search results and on hover cards.
- **Never move a page without a redirect.** Add an entry to `redirects` in `docs.json`.

## Development

```bash
npm i -g mint
mint dev
```

## Publishing

Changes deploy automatically after merging to the default branch. Pull requests generate a preview link.
