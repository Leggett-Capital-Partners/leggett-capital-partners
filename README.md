# Leggett Capital Partners — Website

Marketing site for Leggett Capital Partners. Pure static HTML/CSS/JS, no build step, no dependencies.

## Structure
```
index.html      # all page content / sections
styles.css      # brand system (navy #13294B, teal accent), Barlow Condensed / Inter / Cinzel
script.js       # sticky nav, mobile menu, count-up stats, scroll reveal, team bio modal
cms-render.js   # fetches content/site.json and fills the page from it (HTML stays as fallback)
content/site.json  # editable content: hero, stats, approach, portfolio, story, contact, footer, team
.pages.yml      # Pages CMS field config
assets/         # logos, hero image, team headshots, portfolio-company logos
robots.txt / sitemap.xml  # basic SEO plumbing
```

## Hosting

Hosted on **GitHub Pages**, which redeploys automatically on every push to `main`.
Live site: https://leggett-capital-partners.github.io/leggett-capital-partners/

## Preview locally
```
python3 -m http.server 4321
# open http://localhost:4321
```

## Editing content

Two ways to edit:
- **Non-coders**: Pages CMS at https://app.pagescms.org (sign in with GitHub, open this project). Edits commit straight to `main` and redeploy like any other change.
- **Code changes**: edit the files directly and push. Team member titles/bios/photos live in *two* places that must stay in sync — `content/site.json` under `team.<person>` (what the CMS edits) and the `MEMBERS` object in `script.js` (the baked-in fallback used if the JSON fails to load).

Because the CMS commits directly to `main`, always `git pull` before editing locally so you don't clobber someone else's CMS edit.

## Conventions
- Cache-busting: `index.html` links `styles.css`/`script.js`/`cms-render.js` with a `?v=YYYYMMDD` version. Bump it whenever you change CSS or JS so browsers pick up the new version.
- No em dashes in site copy — use commas, colons, or periods.
- Team photos: `assets/team/<person>.jpg`, portrait roughly 2:3, optimized to about 800x1200.
