# stageor.github.io

Personal site: portfolio, technical blog, and opinion writing. Built with Jekyll, hosted free on GitHub Pages.

## Publish it

1. Create a new GitHub repo named exactly `stageor.github.io` (public).
2. Push this folder to it:
   ```bash
   cd stageor-site
   git init
   git add .
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/stageor/stageor.github.io.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages**, and under "Build and deployment" confirm source is "Deploy from a branch", branch `main`, folder `/root`. GitHub Pages builds Jekyll sites automatically, no extra config needed.
4. Wait a couple of minutes, then visit `https://stageor.github.io`.

## Add a blog post (technical)

Add a file to `_posts/`, named `YYYY-MM-DD-your-title.md`:

```markdown
---
title: "Your title"
tags: [python, ml]
excerpt: "One line summary for the listing page."
---

Your content, in markdown.
```

## Add a thought (opinion)

Same thing, but in `_thoughts/` instead of `_posts/`:

```markdown
---
title: "Your title"
excerpt: "One line summary."
---

Your content.
```

## Add a project to the homepage

Edit `index.html`, find the "Selected work" section, and copy one of the `.project` blocks for your own project.

## Preview locally (optional)

Needs Ruby installed.

```bash
bundle install
bundle exec jekyll serve
```

Then open `http://localhost:4000`.

## Getting indexed by Google

- The `jekyll-sitemap` plugin (already in the `Gemfile`) generates `sitemap.xml` automatically on every build, and `jekyll-seo-tag` fills in page titles and meta tags.
- Once the site is live, submit it at [Google Search Console](https://search.google.com/search-console): add the property `https://stageor.github.io`, verify via the HTML tag or DNS method, then submit `https://stageor.github.io/sitemap.xml`.
- Indexing usually takes a few days to a couple of weeks after submission.

## Custom domain later

GitHub's Student Developer Pack includes a free `.me` domain via Namecheap for students, worth checking since it is free with your UL student email. To attach a custom domain once you have one:

1. Add a file named `CNAME` (no extension) at the root of this repo, containing just your domain, e.g. `stanleygeorge.me`.
2. At your domain registrar, add a `CNAME` record pointing to `stageor.github.io` (or `A` records to GitHub Pages' IPs if pointing the root domain).
3. In repo **Settings → Pages**, enter the custom domain and enable "Enforce HTTPS" once it verifies.
