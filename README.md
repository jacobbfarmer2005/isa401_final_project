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
| Jacob Farmer | Team Roster Scraping, Data Cleaning, Data Merge | https://jacobbfarmer2005.github.io/portfolio/ |
| Peter Franz | [ ] | https://franzp227.github.io/portfolio/ |
| Dalton Kane | [ ] | https://djthork.github.io/portfolio/ |

Each member adds this project to their own portfolio site, in their own words, with a link
back to this repository and the dashboard, and updates the Skills section of their portfolio
and of their `business_intelligence` README with skills they used in this project.

## The Question

Which U.S. communities produce the most FBS players per capita, and which community characteristics (income, population density, education, poverty rate, etc.) best explain the differences?

We are interested in how player development is concentrated across the country with NIL contributions impacting university athletics and recruitment. Unlike existing recruiting rankings our analysis combines scraped rosters for every FBS team with Census demographics to measure talent production at the community level, gaining a more holistic view of player development and it's potential causes.

## Data Sources

At least three real-world sources, collected with at least two methods (web scraping, API,
LLM extraction from text).

| Source | Link | Method | One row is | Code |
|---|---|---|---|---|
| [name] | [url] | [web scraping / API / LLM extraction] | [one listing on one date] | `markdowns/01_acquire.Rmd` |
| [name] | [url] | [ ] | [ ] | [ ] |
| [name] | [url] | [ ] | [ ] | [ ] |

Add a link to the data dictionary for the final merged dataset here.

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

**Inconsistent formats.** The 139 roster sites use more than a dozen page layouts and record the same information in many ways. Heights appear as `6' 2''`, `6′2″` and `6-2`. Class years appear as `R-Fr.`, `5th` and `Graduate`. Positions appear as abbreviations (`OL`, `DB`), full names and combinations (`WR/CB`). Hometown states appear as postal codes, AP-style abbreviations (`Fla.`, `Ill.`), full names, provinces and countries. In `02_clean.Rmd` we converted heights to inches, mapped class years to eligibility years 1–8, spelled out every position, and split hometowns into city, state (USPS code) and country. We also aligned the school names in the CFBD stats with the roster names (e.g., `Pittsburgh` → `Pitt`) and converted CFBD proportions to percentages. On the Census side, names were split into name, geography type and state. The ACS codes for incomes beyond its reporting limits (2,499 and 250,001) were reset to the limits and flagged in `hometown_median_income_censored`. Census statistical areas that are not real towns (e.g., census county divisions) were flagged and excluded from matching. Census values are kept at full precision.

**Mismatched keys.** Rosters and CFBD stats share no ID, so players were joined on school + a standardized name key in three passes: exact name, then first initial + last name, then similar names within the same school (near-identical spellings, extra surnames and nicknames). This left 87 of 8,684 CFBD players unmatched. Hometowns were matched to ACS 2024 5-year data in order: first Census places (cities and Census-designated places), then county subdivisions (towns and townships, the main local governments in states such as New Jersey, Pennsylvania and Michigan), then counties for rosters that list a county as the hometown. City names were standardized first: "St./Ft./Mt." and "N./S./E./W." were expanded, "Village of" was dropped, and spaces were ignored. When two places or townships in a state share a name, the more populous one is used. New York City players are matched to their borough (Bronx, Brooklyn, Manhattan, Queens or Staten Island), which the Census treats as a county subdivision, rather than to the whole city. Players listing "Harlem" or "New York" are treated as Manhattan. These steps cut unmatched U.S. hometowns from 780 to 468, so 97.0% of U.S. hometowns now match. `hometown_match_level` records which geography each hometown matched: 14,622 places, 308 county subdivisions and 23 counties.

**Missing values, outliers and duplicates.** Missing values were kept as `NA`, never imputed. Stat columns are `NA` when a player has no record in that category, and Census columns are `NA` when a hometown did not match. Colorado State and Delaware do not publish height or weight, and Georgia Tech does not publish class year. Hometown population had one extreme value: every New York City player shared the whole city's 8.5 million residents. The borough matching above replaced it with borough populations (0.5–2.6 million) without removing any players, so the largest town-level hometown is now Los Angeles (3.9 million). Two hometowns matched Census places with zero residents, so their county's data was used instead. The 23 county-level matches describe a whole county rather than a town, so they are excluded from all hometown and per-capita summaries. The largest of these is Tonopah, AZ, which falls back to Maricopa County (4.6 million residents). The validation checks found no duplicate players, no duplicate CFBD IDs and no non-players: coaches, staff, Team IMPACT members, memorial entries and honorary captains were removed during scraping.

## Validation Table

The observations that break an expected rule (impossible values, duplicates, invalid labels or codes, failed joins). Produced in `markdowns/04_validate.Rmd`. All 64 rules are listed in that file. 54 rules now pass and 10 fail. The 52 rules not shown below passed in both runs. Counts show the original validation run → the run after the fixes.

| Rule | Rows that break it | What we did |
|---|---|---|
| 56. Per-capita rates only for hometowns with population above 0 | 2 → 0 | **Fixed.** Two hometowns matched Census places with no residents, so they now use their county's data and are excluded from per-capita summaries. |
| 58. Failed join: US hometown did not match a Census geography | 780 → 468 | **Reduced.** Added city-name standardization, county subdivision (township) matching, NYC borough matching and county matching. The remainder are unincorporated communities and neighborhoods too small for the ACS, Census naming differences, and U.S. territories the ACS does not cover. |
| 59. Failed join: CFBD player did not match a roster player | 120 → 87 | **Reduced.** Added a similar-name matching pass (37 matches, each reviewed by hand). The remaining players most likely left the roster during the season. |
| 60. Weight more than 3.5 robust SDs from the position median | 88 → 88 | **Verified.** The most extreme cases match the schools' published rosters, so these are real players and were kept. |
| 61. Height more than 3.5 robust SDs from the position median | 14 → 14 | **Verified.** Same check as rule 60. |
| 62. School roster size outside the IQR fences | 3 schools → 3 | **Verified.** Army (188), Navy (188) and Rutgers (134) match their published rosters. Service academies carry unusually large rosters. |
| 63. School missing rate far above the overall rate | 11 school/column pairs → 11 | **Documented.** Colorado State and Delaware do not publish height or weight, and Georgia Tech does not publish class year. |
| 64. Census-matched and unmatched players have similar height/weight | Fails → fails | **Documented.** Unmatched hometowns are not a random sample (see Limitations). |
| 52. Each cell holds a single value | 88 → 88 | **Accepted.** Multi-position players (e.g., Kicker/Punter) are kept as listed. |
| 32. Rushing touchdowns ≤ carries | 2 → 2 | **Accepted.** Touchdowns on laterals or fumble recoveries do not require a carry. |
| 51. Each variable forms one column | Table-level | **Accepted.** `previous_school_1`–`4` are kept as separate columns on purpose. |
| 55. Dataset holds every variable the research question needs | Table-level → passes | **Resolved.** University proximity was removed from the scope of the research question, so the dataset now holds every variable the question needs. |
## Limitations

**Geographic coverage.** 468 U.S. hometowns (3.0%) could not be matched to ACS data, so those players have no community characteristics. Most are unincorporated communities and neighborhoods inside larger cities (e.g., Cypress, TX; Ellenwood, GA; Watts in Los Angeles). The Census Bureau does not publish estimates for these as named places, and matching them would require an outside geocoding source beyond our three data sources. These players cluster in the suburbs of Houston, Atlanta and Nashville, so the matched and unmatched groups differ (rule 64), and per-capita results may slightly under-count those metro areas. The 23 county-level matches are excluded from hometown and per-capita summaries, along with the unmatched players. Both still count toward their state's total.

**Assumptions in the matching.** Hometowns are matched by name, so when two places or townships in a state share a name, we assume the player is from the more populous one. Players listing "New York, NY" are assumed to be from Manhattan, following Postal Service usage. A roster's listed hometown is assumed to be where the player grew up, though some list a high school town or a nearby larger city. Player matching uses names only. The 87 CFBD players without a roster match are assumed to have left the roster but were not individually verified, and only the most extreme height and weight outliers were checked by hand. Census figures are 2024 ACS 5-year estimates (survey period 2020–2024) and carry sampling error, especially for small towns.

**What the dataset cannot answer.** The data covers one season (2026) of FBS rosters only. It excludes FCS and lower divisions, players who did not reach college football, and changes over time. It can show where FBS players come from and how that relates to community characteristics, but it cannot establish that those characteristics cause player production. Coaching, high school programs, recruiting networks and NIL opportunities are not measured. Rosters change during the season, so the data reflects the rosters at the time they were scraped.