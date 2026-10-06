# Leggett Capital Partners Website, Claude Code Onboarding

This guide gets Claude Code on a new computer up to speed on the LCP marketing website so you can make edits and deploy from anywhere.

## What this project is

The public Leggett Capital Partners marketing site. It is a **static website**: plain HTML, CSS, and JavaScript with **no build step**. It is hosted on **GitHub Pages**, which redeploys automatically on every push to `main`.

- **Repo:** https://github.com/Leggett-Capital-Partners/leggett-capital-partners  (branch: `main`)
- **Live site:** https://leggett-capital-partners.github.io/leggett-capital-partners/
- **GitHub account with access:** `jd182-jpg` (owner of the Leggett-Capital-Partners org)

## First-time setup on a new PC

1. Install **Node.js** (needed for Claude Code).
2. Install Claude Code: `npm install -g @anthropic-ai/claude-code`
3. Install **Git** and the **GitHub CLI**, then sign in as `jd182-jpg`: `gh auth login`
4. Clone the repo: `gh repo clone Leggett-Capital-Partners/leggett-capital-partners`
5. Open that folder and run `claude`. Sign in with your Anthropic account.

That is all Claude Code needs. GitHub is the single source of truth, so no files have to be copied between computers.

## GitHub is the source of truth (read before editing)

Other people edit this site too: Ashley (and others) can edit content through a CMS (see below) or directly on GitHub. **GitHub `main` is frequently ahead of any local copy.**

- Before editing, always `git pull` (or clone fresh) so you start from the current live content.
- Before pushing, confirm you are not about to overwrite newer work. A safe guard is to fetch and check that local `HEAD` still equals `origin/main`; if origin moved, pull/rebase first, then push.
- Never bulk-copy an old local folder over the repo. That can wipe edits other people made through the CMS.

## Deploy workflow

1. Edit the files in the repo.
2. Commit and push to `main`.
3. GitHub Pages rebuilds automatically (about one minute).
4. Verify: reload the live URL, or check the build with `gh api repos/Leggett-Capital-Partners/leggett-capital-partners/pages/builds/latest`. When `status` is `built` and the `commit` matches what you pushed, it is live.

## How the site is structured

- **`index.html`** — page structure and the baked-in content (works even if JavaScript is off).
- **`styles.css`** — all styling. Brand navy is `#1C4070`. Cinzel / Barlow Condensed / Inter fonts.
- **`script.js`** — the `MEMBERS` object (team member data for the bio popups), the stat count-up animation, and `applySiteTeam` which merges CMS content into the team cards.
- **`cms-render.js`** — fetches `content/site.json` at load, fills the page by CSS selector, and dispatches a `site:ready` event. The HTML stays as a fallback if the JSON fails to load.
- **`content/site.json`** — the editable content (hero, stats, approach, portfolio, story, contact, footer, and per-person team info).
- **`.pages.yml`** — configures the Pages CMS form fields.

### Editing team members

A person's title, bio, and photo live in **two places** that must stay in sync:
- `content/site.json` under `team.<person>` (this is what the CMS edits), and
- `script.js` in the `MEMBERS` object (the baked-in fallback).

When you change a bio or title, update **both**. Photos are `assets/team/<person>.jpg`, portrait roughly 2:3, optimized to about 800x1200.

### Team roster and tiers (on the site)

- **Firm Leadership:** John Leggett, Ashley Tucker Zatcoff, Brian Weinberg, Earl Correll
- **Operating Partners:** Brad Elmore, Eric Kline
- **Firm Professionals:** Hiba Alkhuzaie, Seslee Skrabanek, Andrew Herrick, Jackson Darr

## The CMS (how Ashley edits without code)

Non-coders edit the site through **Pages CMS** at https://app.pagescms.org (sign in with GitHub, open the `leggett-capital-partners` project). Their edits commit to the repo and redeploy like any other change. This is why you must always pull before editing.

## Conventions and gotchas

- **Cache-busting:** `index.html` links `styles.css` and `script.js` with a `?v=YYYYMMDD` version. Bump it whenever you change CSS or JS so browsers load the new version. Swapping a team photo keeps the same filename, so mention that returning visitors may need a hard refresh.
- **Writing style:** no em dashes in any site copy. Use commas, colons, or periods.
- **Compliance and claims language (important):** this is a public page for an SEC-registered investment adviser, so marketing copy is compliance-sensitive. A compliance review in October 2026 softened the site's language, and new copy should follow the same rules:
  - Avoid unsubstantiable superlatives and absolutes: "the best of," "ensure," "every deal," "low-risk, high-return," "mature investments / attractive return." Prefer hedged phrasing: "what we believe is," "aims to / striving to," "targets," "potential," "growth potential."
  - Avoid promissory or guarantee-sounding language about returns, success, or outcomes ("build long-term trust and success" became "strive to build…").
  - The **AUM figure uses a rolling "as of most recent quarter-end" label** (not a hard calendar date), backed by a general fair-value / as-of-quarter-end basis note in the footer disclaimer, so it does not need a quarterly date edit. Keep the number consistent with the most recently filed ADV, and update the number itself only if it changes materially (expected roughly quarterly).
  - **Awards and accolades** (e.g., 40 Under 40, CoStar Power Broker) require a disclosure of how each was earned and whether it was paid for. These were removed from the John, Earl, and Brad bios pending that disclosure; do not re-add an award without it.
  - When in doubt, soften the claim or leave it out, and flag it for compliance review. The same rules apply to the investor deck ("Leggett Capital Partners Introduction …pptx") kept in the Marketing library.
- **OneDrive mirror (optional backup):** a synced git clone lives in the Bakers Creek "Marketing" library under `Leggett Capital Partners/Brand Assets/Leggett Capital Partners Website Files`. It is just a mirror; GitHub is authoritative. OneDrive rewrites files to CRLF on folder moves, which makes `git pull` abort on every file; if that happens, confirm it is only line-ending noise (`git diff --ignore-all-space --stat` shows 0 real changes) and `git reset --hard origin/main`.
- **Not in the CMS (code only):** adding or removing a whole person or portfolio company, and any layout, color, logo, or font change.
