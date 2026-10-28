# NCAA Recruitment: Mapped

ISA 401 Final Project, Fall 2026, Team 6, Section B

<!--
How to use this template (delete this comment block once your team is set up):

3. Every team member clones the new repository in GitHub Desktop (File > Clone repository)
   to M:\isa401\ and opens project.Rproj in RStudio.
4. Replace every [bracketed] placeholder below. Keep the headings: they follow the
   Project page on Canvas, and this README is the index of your final submission.

Never commit API keys. Keep them in a .Renviron file in this folder (already in .gitignore).
-->

## Submission

| Item | Link |
|---|---|
| Dashboard (Tableau Public or Power BI) | [link] |
| Recorded technical presentation (YouTube, unlisted) | [link] |
| This repository | [link] |

## Team

| Member | What I built | Portfolio project entry |
|---|---|---|
| [First Last] | [for example: the API pulls and the merge] | [https://USERNAME.github.io/portfolio/#...] |
| Jacob Farmer | [ ] | https://jacobbfarmer2005.github.io/portfolio/ |
| Peter Franz | [ ] | https://franzp227.github.io/portfolio/ |
| Dalton Kane | [ ] | https://djthork.github.io/portfolio/ |

Each member adds this project to their own portfolio site, in their own words, with a link
back to this repository and the dashboard, and updates the Skills section of their portfolio
and of their `business_intelligence` README with skills they used in this project.

## The Question

[The research question or business problem, in one or two sentences.]

[Why it matters, and what makes it interesting or new compared with analyzing an existing dataset.]

## Data Sources

At least three real-world sources, collected with at least two methods (web scraping, API,
LLM extraction from text).

| Source | Link | Method | One row is | Code |
|---|---|---|---|---|
| [name] | [url] | [web scraping / API / LLM extraction] | [one listing on one date] | `markdowns/01_acquire.Rmd` |
| [name] | [url] | [ ] | [ ] | [ ] |
| [name] | [url] | [ ] | [ ] | [ ] |

## Pipeline

| Step | File | What it does |
|---|---|---|
| 1. Acquire | `markdowns/01_acquire.Rmd` | [ ] |
| 2. Clean and transform | `markdowns/02_clean.Rmd` | [ ] |
| 3. Merge | `markdowns/03_merge.Rmd` | [ ] |
| 4. Validate and summarize | `markdowns/04_validate.Rmd` | [ ] |

Raw files go in `data/raw/`; the cleaned and merged files go in `data/processed/`.

## How to Reproduce

1. R version: [4.x.x]. Packages: [list them, or point to the setup chunk that installs them].
2. API keys: create a `.Renviron` file in this folder with these names (values are your own):
   - `[KEY_NAME]=`
3. Knit the four files in `markdowns/` in order, 01 to 04.

## The Final Dataset

- File: `data/processed/[name].csv`
- One row is: [ ]
- Rows and columns: [ ]

| Column | Type | Description | Source |
|---|---|---|---|
| [ ] | [ ] | [ ] | [ ] |

## Data Issues and How We Handled Them

[Missing values, inconsistent formats, duplicate records, mismatched keys: what you found and what you did.]

## Validation Table

The observations that break an expected rule (impossible values, duplicates, invalid labels or
codes, failed joins). Produced in `markdowns/04_validate.Rmd`.

| Rule | Rows that break it | What we did |
|---|---|---|
| [ ] | [ ] | [ ] |

## Limitations

[Assumptions, gaps in the data, and what the dataset cannot answer.]
