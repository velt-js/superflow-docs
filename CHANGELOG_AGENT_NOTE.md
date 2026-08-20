# Note for agent: adding changelog entries to this repo

This is the Superflow docs site, built with Mintlify (config: `docs.json`). A **Changelog** tab already exists in the navigation and points to a single page: `changelog/changelog.mdx`. Your job is to add real changelog entries to that page.

## How to add an entry

1. Open `changelog/changelog.mdx`. Keep the frontmatter (`title`, `description`) untouched.
2. Add one `<Update>` block per release, **at the top of the file** (newest first). There is a template in an MDX comment inside the file:

   ```mdx
   <Update label="Month DD, YYYY" description="Month DD-DD, YYYY">
     ## Release Title

     ### New Features

     - **Feature Name**: One-line description of what it does.

     ### Improvements

     - Short description of the improvement.

     ### Bug Fixes

     - Fixed an issue where...
   </Update>
   ```

   `label` is the date shown in the left rail of the changelog page; `description` renders as a small badge and holds the **date range** the release covers (e.g. `Aug 1-5, 2026`).

   **Superflow does not use version numbers.** There is no `vX.Y.Z` to find, so do not look for one, do not ask for one, and do not block on one. Releases are identified by the period they cover, which is always in the payload.

   Omit any section (New Features / Improvements / Bug Fixes) that has no items. Do not leave empty headings.
3. Images/screenshots go under `images/changelog/<release>/` and are embedded with the same pattern used elsewhere in this repo:

   ```mdx
   <Frame>
   ![](/images/changelog/v2-4-0/1.png)
   </Frame>
   ```

## Conventions

- Write in the same voice as the rest of the docs: short, direct, user-facing. Describe what the user can now do, not internal implementation details.
- Bold the feature name at the start of each "New Features" bullet.
- No entry should reference internal ticket numbers or code identifiers.
- There is a legacy `product-updates/` folder with 2024 entries that is NOT in the navigation. Do not add to it. If asked to migrate it, convert each old file into an `<Update>` block in `changelog/changelog.mdx` (or a per-year page, see below) and then delete the folder.

## Validate before committing

This repo has no `package.json` or linter, so do NOT look for `npm run lint`. Instead run:

```bash
PUPPETEER_SKIP_DOWNLOAD=1 npx mintlify broken-links
```

**The env var is required in a sandboxed environment and is the whole reason this command is written out in full.** `npx mintlify` installs the CLI fresh, that install pulls puppeteer, and puppeteer tries to download a Chromium binary from a host outside the default network allowlist. The download fails, the install fails, and the failure surfaces as an opaque npx error with no mention of Chromium, so an agent burns several turns discovering that broken-links was never the problem. `PUPPETEER_SKIP_DOWNLOAD=1` skips the download; broken-links does not need a browser.

It parses every MDX page (catching unclosed `<Update>` tags) and checks internal links.

**Which errors to act on:** only those naming the file you edited. Everything else in this repo is pre-existing and not yours to fix in a changelog PR. As of this writing that pre-existing set is ~21 broken links, and it is NOT limited to the `broken.mdx` / `broken2.mdx` junk files. The bulk are `app.usesuperflow.com` links spread across roughly a dozen unrelated pages (`dashboard/`, `security/`, `product-features/`). Do not try to fix them, do not count them as a regression, and do not let their number talk you out of committing. A clean way to be sure: run the command once BEFORE your edit and once after, and compare.

Avoid `mintlify validate`: it is strict mode and always fails on those junk files.

## When the file gets large

Keep everything in `changelog/changelog.mdx` until it becomes unwieldy (roughly: more than a year of entries or the page is slow to scan). Then split **by year**:

1. Create `changelog/2026.mdx`, `changelog/2025.mdx`, etc., each with the same frontmatter pattern and that year's `<Update>` blocks. The current year stays in (or becomes) the first page listed.
2. Update the Changelog tab in `docs.json`. It currently looks like:

   ```json
   {
     "tab": "Changelog",
     "groups": [
       { "group": "Changelog", "pages": ["changelog/changelog"] }
     ]
   }
   ```

   Replace the `pages` array with the per-year pages, newest year first.

## Previewing locally

- Run `mintlify dev --port 3001 --no-open` from the repo root. Do NOT use the `mint` CLI on this machine: it crashes on startup. Port 3000 is often occupied by an unrelated app.
- The page renders at `http://localhost:3001/changelog/changelog`.
- Ignore the pre-existing parse error about `broken2.mdx`: it is an intentionally broken test file that is not in the navigation.
