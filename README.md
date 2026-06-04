# Ecosystem Dynamics Lab Dashboard

A live dashboard for the [Ecosystem Dynamics Lab](https://www.sydney.edu.au/science/about/our-people/academic-staff/aaron-greenville.html) at the University of Sydney, tracking recent ecology journal articles and Australian ecology grant opportunities.

## What it shows

**Journal Watch**
- RSS feeds from 26 ecology journals, refreshed daily
- Recent papers from 10 target journals via [OpenAlex](https://openalex.org), grouped by research theme (fire, climate, conservation, soil, animal ecology, and more)
- Sidebar "Jump to" links for quick navigation between sections

**Grants**
- Status of open and upcoming Australian ecology funding schemes (ARC, Hermon Slade Foundation, Ian Potter Foundation, NSW Environmental Trust, NESP, and others)
- Searchable table of ~71 awarded grants with direct links to funder pages
- Student & ECR grants tab covering 14 society and early-career funding sources

## Data sources

| Data | Source |
|------|--------|
| Journal RSS feeds | Publisher feeds (Wiley, Springer, Elsevier, BES, ESA) fetched daily |
| Recent papers | [OpenAlex API](https://openalex.org) — free, no key required |
| Grant information | ARC Data Portal, funder websites |

## How content is kept fresh

Both automated updates are handled by Cowork scheduled tasks — no manual steps required.

**Journal RSS — daily (automated)**
A Cowork task runs at 7:08 am each day. It fetches all 26 journal feeds, writes `rss_articles.json` to the workspace, and automatically commits and pushes it to this repo. The page loads the file at `./rss_articles.json` on each visit.

**Grant status — weekly (automated)**
A Cowork task runs every Monday at 7:05 am. It checks ARC, Hermon Slade, Ian Potter, NSW Environmental Trust, and other funder pages, updates the open/upcoming scheme cards in the dashboard, and pushes the updated `index.html` to this repo.

A second task runs at 7:11 am Monday to update the Student & ECR Grants tab.

**Awarded grants table — manual**
The ~71-row awarded grants table changes only when new funding rounds are announced. To add or update entries, edit `index.html` directly and push:

```bash
git add index.html
git commit -m "Update awarded grants table"
git push
```

## Local development

Requires a local HTTP server (`file://` blocks the JSON fetch):

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Repo structure

```
index.html          # Single-file dashboard (HTML + CSS + JS)
rss_articles.json   # Cached RSS articles, committed daily by scheduled task
README.md
```

## Maintainer

Dr Aaron Greenville — University of Sydney  
[Ecosystem Dynamics Lab](https://www.sydney.edu.au/science/about/our-people/academic-staff/aaron-greenville.html)
