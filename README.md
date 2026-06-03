# Ecosystem Dynamics Lab Dashboard

A live dashboard for the [Ecosystem Dynamics Lab](https://www.sydney.edu.au/science/about/our-people/academic-staff/aaron-greenville.html) at the University of Sydney, tracking recent ecology journal articles and Australian ecology grant opportunities.

## What it shows

**Journal Watch**
- RSS feeds from 26 ecology journals, refreshed daily
- Latest issues from Landscape Ecology, Plant and Soil, and Ecosystems
- Recent papers from 10 target journals via [OpenAlex](https://openalex.org), grouped by research theme (arid/dryland, fire, predator–prey, climate, biodiversity, and more)

**Grants**
- Status of open and upcoming Australian ecology funding schemes (ARC, Hermon Slade Foundation, Ian Potter Foundation, NSW Environmental Trust, NESP, and others)
- Searchable table of ~71 awarded grants with direct links to funder pages

## Data sources

| Data | Source |
|------|--------|
| Journal RSS feeds | Publisher feeds (Wiley, Springer, Elsevier, BES, ESA) fetched daily |
| Recent papers | [OpenAlex API](https://openalex.org) — free, no key required |
| Grant information | ARC Data Portal, funder websites |

`rss_articles.json` is updated daily and committed alongside `index.html`. The page fetches it at load time via `./rss_articles.json`.

## Keeping the content fresh

**Journal RSS (daily)**
A Cowork scheduled task runs at 7:08 am each day, fetches all 26 journal feeds, and writes a new `rss_articles.json` to the OneDrive folder. After it runs, commit and push that file to update the public page:

```bash
cd github-pages
git add rss_articles.json
git commit -m "Daily RSS update $(date +%Y-%m-%d)"
git push
```

This can be automated (see Step 4 of the migration plan) — but until then it requires a manual push.

**Grants tab (weekly)**
A Cowork scheduled task runs every Monday at 7:05 am. It checks ARC, Hermon Slade, Ian Potter, and NSW Environmental Trust pages and updates the open/upcoming scheme cards. The goal is for this task to also push the updated `index.html` to this repo automatically (see Step 4 of the migration plan). Until that is set up, after the task runs copy the updated `index.html` from the Cowork artifact into this folder and push:

```bash
git add index.html
git commit -m "Weekly grant status update $(date +%Y-%m-%d)"
git push
```

The awarded grants table (~71 rows) changes rarely — only when new funding rounds are announced — and is updated by manually editing `index.html`.

## Local development

Requires a local HTTP server (the JSON fetch won't work over `file://`):

```bash
cd github-pages
python3 -m http.server 8080
# then open http://localhost:8080
```

## Repo structure

```
index.html          # Single-file dashboard (HTML + CSS + JS)
rss_articles.json   # Cached RSS articles, updated daily
README.md
```

## Maintainer

Dr Aaron Greenville — University of Sydney  
[aaron.greenville@gmail.com](mailto:aaron.greenville@gmail.com)
