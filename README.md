# NCAA Recruitment: Mapped

ISA 401 Final Project, Fall 2026, Team 6, Section B

## Submission

| Item | Link |
|---|---|
| Dashboard (Tableau Public or Power BI) | [link] |
| Recorded technical presentation (YouTube, unlisted) | [link] |
| This repository | https://github.com/jacobbfarmer2005/isa401_final_project |

## Team

| Member | What I built | Portfolio project entry |
|---|---|---|
| [First Last] | [for example: the API pulls and the merge] | [https://USERNAME.github.io/portfolio/#...] |
| Jacob Farmer | Team Roster Scraping, Data Cleaning, Data Merge, Data Validation | https://jacobbfarmer2005.github.io/portfolio/ |
| Peter Franz | [ ] | https://franzp227.github.io/portfolio/ |
| Dalton Kane | [ ] | https://djthork.github.io/portfolio/ |

Each member adds this project to their own portfolio site, in their own words, with a link
back to this repository and the dashboard, and updates the Skills section of their portfolio
and of their `business_intelligence` README with skills they used in this project.

## The Question

Which U.S. communities produce the most FBS players per capita, and which community characteristics (income, population density, education, poverty rate, etc.) best explain the differences?

We are interested in how player development is concentrated across the country with NIL contributions impacting university athletics and recruitment. Unlike existing recruiting rankings our analysis combines scraped rosters for every FBS team with Census demographics to measure talent production at the community level, gaining a more holistic view of player development and it's potential causes.

## Data Sources

Three real-world sources, collected with web scraping and APIs. 

| Source | Link | Method | One row is | Code |
|---|---|---|---|---|
| FBS team rosters | Official athletics websites of the 139 FBS teams (e.g., https://ohiostatebuckeyes.com/sports/football/roster) | Web scraping | One player on one team's 2026 roster | `markdowns/01_acquire.Rmd` |
| College Football Data (CFBD) API | https://api.collegefootballdata.com (endpoint `/stats/player/season`) | API | One statistic for one player in the 2026 season (02 pivots this to one row per player) | `markdowns/01_acquire.Rmd` |
| U.S. Census Bureau, ACS 5-year estimates (2024) and TIGER/Line boundary files | https://www.census.gov/data/developers/data-sets/acs-5year.html | API (tidycensus, plus tigris for boundaries) | One geography (place, county subdivision or county) with its ACS estimates and land area | `markdowns/01_acquire.Rmd` |

## Pipeline

| Step | File | What it does |
|---|---|---|
| 1. Acquire | `markdowns/01_acquire.Rmd` | Scrapes the rosters of all 139 FBS teams. Pulls 2024 ACS 5-year estimates for every U.S. place, county subdivision and county from the Census API, along with tigris boundary files for land area and coordinates. Pulls 2026 player season stats from the CFBD API. Saves three raw CSVs to `data/raw/`. |
| 2. Clean and transform | `markdowns/02_clean.Rmd` | Standardizes names, positions, heights, weights, class years and hometowns (city, state, country). Splits the previous-school lists into separate columns. Cleans the ACS data: flags income values that hit the reporting cap, removes Census-defined statistical areas from the county subdivisions, and computes population density from boundary land area. Pivots the CFBD stats to one row per player. Saves cleaned files to `data/processed/`. |
| 3. Merge | `markdowns/03_merge.Rmd` | Joins CFBD stats to rosters in three passes (exact name, first initial + last name, then similar name) and records the method in `match_method`. Matches each U.S. hometown to Census data, trying the place first, then the county subdivision (town, township or NYC borough), then the county, and records the level in `hometown_match_level`. Writes the final dataset, `fbs_players_merged.csv` (15,744 rows × 97 columns). |
| 4. Validate and summarize | `markdowns/04_validate.Rmd` | Checks 64 validation rules covering ranges, missing values, duplicates, valid categories and join quality, with 54 passing and 10 failing. Produces an observation-level table of the rows that break each rule and a before-and-after comparison against the original validation run. Writes descriptive summaries (Tables 5–9, Charts 1–5) of players and of per-capita FBS production by community. |

Raw files are in `data/raw/`; the cleaned and merged files are in `data/processed/`.

## How to Reproduce

1. R version: [4.6.1]. Packages: rvest, xml2, chromote, httr2, tidycensus, tigris, sf, dplyr, tidyr, purrr, stringr, stringi, readr, tibble, tidyselect, countrycode, here, ggplot2, scales, knitr, rmarkdown

Run this setup chunk in your console to install this project's packages:

```r
   install.packages(c(
     "rvest", "xml2", "chromote", "httr2",               # web scraping and API requests
     "tidycensus", "tigris", "sf",                       # Census data, boundary files and geography
     "dplyr", "tidyr", "purrr", "stringr", "stringi",    # data cleaning and merging
     "readr", "tibble", "tidyselect",
     "countrycode", "here",                              # validation checks
     "ggplot2", "scales", "knitr", "rmarkdown"           # summaries, charts and knitting
   ))
   ```

2. API keys: create a `.Renviron` file in the project root folder with these names (values are your own):
   - `CFBD_API_KEY=` (get free key from https://collegefootballdata.com/key)
   - `CENSUS_API_KEY=` (get free key from https://api.census.gov/data/key_signup.html)

3. The four data pipline files are knitted in `markdowns/` in order, 01 to 04, next to their associated `.rmd` files.
   - `01_acquire.html`
   - `02_clean.html`
   - `03_merge.html`
   - `04_validate_before_fixes.html`
   - `04_validate_after_fixes.html` Table 4 (before vs. after the fixes) compares against `data/processed/validation/baseline/`, which holds the original validation run.

Rosters change during the season, so a fresh scrape may give slightly different counts than those reported in this README.

## The Final Dataset

- File: `data/processed/fbs_players_merged.csv`
- One row is: one player and his associated conference, school, player statistics, and hometown demographics.
- Rows and columns: 15,744 rows × 97 columns

| Column | Type | Description | Source |
|---|---|---|---|
| `school` | factor | FBS school whose 2026 roster lists the player (139 schools) | Rosters (web scraping) |
| `conference` | factor | FBS conference of the school, 2026 alignment (11 conferences, including FBS Independent) | Rosters (web scraping) |
| `name` | character | Player's name as listed on the school roster | Rosters (web scraping) |
| `year` | factor | Eligibility year, 1–8; redshirt status ignored (R-Fr. = 1); NA = not listed | Rosters (web scraping) |
| `position` | factor | Position, spelled out (e.g., Wide Receiver); multiple positions separated by "/" | Rosters (web scraping) |
| `player_id` | character | collegefootballdata.com (CFBD) player ID; NA = no matching 2026 stats | CFBD API |
| `match_method` | factor | How the roster player was matched to CFBD stats: "Exact name", "First initial + last name" or "Similar name" (near-identical spelling, extra surname, or nickname within the same school); NA = not matched | Derived (03_merge) |
| `season` | integer | Season of the CFBD statistics (2026); NA = not matched | CFBD API |
| `height_in` | numeric | Player height in inches | Rosters (web scraping) |
| `weight_lbs` | integer | Player weight in pounds | Rosters (web scraping) |
| `hometown_city` | character | Player's hometown city as listed on the roster | Rosters (web scraping) |
| `hometown_state` | factor | Hometown state as a USPS code (e.g., OH); full province name for non-US hometowns | Rosters (web scraping) |
| `hometown_country` | factor | Hometown country (e.g., United States, Canada, Australia) | Rosters (web scraping) |
| `previous_school_1` | character | Most recently listed previous college (transfers); NA = none | Rosters (web scraping) |
| `previous_school_2` | character | Second listed previous college; NA = none | Rosters (web scraping) |
| `previous_school_3` | character | Third listed previous college; NA = none | Rosters (web scraping) |
| `previous_school_4` | character | Fourth listed previous college; NA = none (14 players in 2026). Previous-school columns are kept only when at least one player has a value, so the number of these columns can change between seasons | Rosters (web scraping) |
| `receiving_longest_yards` | integer | Longest reception of the season, in yards | CFBD API |
| `receptions` | integer | Total receptions (count) | CFBD API |
| `receiving_touchdowns` | integer | Receiving touchdowns (count) | CFBD API |
| `receiving_yards` | integer | Total receiving yards | CFBD API |
| `receiving_yards_per_reception` | numeric | Receiving yards per reception (yards) | CFBD API |
| `rushing_carries` | integer | Total rushing attempts (count) | CFBD API |
| `rushing_longest_yards` | integer | Longest rush of the season, in yards | CFBD API |
| `rushing_touchdowns` | integer | Rushing touchdowns (count) | CFBD API |
| `rushing_yards` | integer | Total rushing yards (can be negative) | CFBD API |
| `rushing_yards_per_carry` | numeric | Rushing yards per carry (yards) | CFBD API |
| `passes_defended` | integer | Passes defended/broken up (count) | CFBD API |
| `qb_hurries` | integer | Quarterback hurries (count) | CFBD API |
| `sacks` | numeric | Sacks (count; half credit allowed for shared sacks) | CFBD API |
| `tackles_solo` | integer | Solo tackles (count) | CFBD API |
| `defensive_touchdowns` | integer | Defensive touchdowns (count) | CFBD API |
| `tackles_for_loss` | numeric | Tackles for loss (count; half credit allowed) | CFBD API |
| `tackles_total` | integer | Total tackles, solo plus assisted (count) | CFBD API |
| `interceptions_yards_per_return` | numeric | Interception return yards per interception (yards) | CFBD API |
| `interceptions` | integer | Interceptions made (count) | CFBD API |
| `interceptions_touchdowns` | integer | Interceptions returned for a touchdown (count) | CFBD API |
| `interceptions_yards` | integer | Total interception return yards | CFBD API |
| `punt_returns_yards_per_return` | numeric | Punt return yards per return (yards) | CFBD API |
| `punt_returns_longest_yards` | integer | Longest punt return of the season, in yards | CFBD API |
| `punt_returns` | integer | Punt returns (count) | CFBD API |
| `punt_returns_touchdowns` | integer | Punt returns for a touchdown (count) | CFBD API |
| `punt_returns_yards` | integer | Total punt return yards | CFBD API |
| `passing_attempts` | integer | Pass attempts (count) | CFBD API |
| `passing_completions` | integer | Pass completions (count) | CFBD API |
| `passing_interceptions` | integer | Interceptions thrown (count) | CFBD API |
| `completion_percent` | numeric | Completions ÷ attempts × 100 (percent, 0–100) | CFBD API |
| `passing_touchdowns` | integer | Passing touchdowns (count) | CFBD API |
| `passing_yards` | integer | Total passing yards | CFBD API |
| `passing_yards_per_attempt` | numeric | Passing yards per attempt (yards) | CFBD API |
| `field_goals_attempted` | integer | Field goals attempted (count) | CFBD API |
| `field_goals_made` | integer | Field goals made (count) | CFBD API |
| `field_goal_longest_yards` | integer | Longest made field goal, in yards | CFBD API |
| `field_goal_percent` | numeric | Field goals made ÷ attempted × 100 (percent, 0–100) | CFBD API |
| `kicking_points` | integer | Points scored by kicking: 3 × field goals + extra points (points) | CFBD API |
| `extra_points_attempted` | integer | Extra points attempted (count) | CFBD API |
| `extra_points_made` | integer | Extra points made (count) | CFBD API |
| `punts_inside_20` | integer | Punts downed inside the opponent's 20-yard line (count) | CFBD API |
| `punting_longest_yards` | integer | Longest punt of the season, in yards | CFBD API |
| `punts` | integer | Punts (count) | CFBD API |
| `punting_touchbacks` | integer | Punts resulting in a touchback (count) | CFBD API |
| `punting_yards` | integer | Total punting yards | CFBD API |
| `punting_yards_per_punt` | numeric | Average yards per punt (yards) | CFBD API |
| `kick_returns_yards_per_return` | numeric | Kickoff return yards per return (yards) | CFBD API |
| `kick_returns_longest_yards` | integer | Longest kickoff return of the season, in yards | CFBD API |
| `kick_returns` | integer | Kickoff returns (count) | CFBD API |
| `kick_returns_touchdowns` | integer | Kickoff returns for a touchdown (count) | CFBD API |
| `kick_returns_yards` | integer | Total kickoff return yards | CFBD API |
| `fumbles` | integer | Fumbles (count) | CFBD API |
| `fumbles_lost` | integer | Fumbles lost to the opponent (count) | CFBD API |
| `fumbles_recovered` | integer | Fumbles recovered (count) | CFBD API |
| `hometown_geoid` | character | Census FIPS code of the hometown geography: 7 digits for a place (state + place), 10 for a county subdivision (state + county + subdivision), 5 for a county (state + county); NA = no Census match or non-US | Census ACS API |
| `hometown_match_level` | factor | Census geography the hometown was matched to: "Place" (city or Census-designated place), "County subdivision" (town, township or NYC borough) or "County" (roster listed a county, or the matched place had no residents). All hometown_ columns describe this geography; NA = no Census match or non-US | Derived (03_merge) |
| `hometown_place_type` | factor | Census place or county subdivision type (city, town, village, borough, CDP, township, charter township, etc.); NA for county-level matches | Census ACS API |
| `hometown_population` | integer | Total population of the hometown geography, ACS 2024 5-year (persons); the whole county for county-level matches | Census ACS API |
| `hometown_median_age` | numeric | Median age of hometown residents, ACS 2024 5-year (years) | Census ACS API |
| `hometown_pct_male` | numeric | Percent of hometown residents who are male (percent, 0–100) | Census ACS API |
| `hometown_pct_white` | numeric | Percent of hometown residents who are non-Hispanic white (percent, 0–100) | Census ACS API |
| `hometown_pct_black` | numeric | Percent of hometown residents who are non-Hispanic Black (percent, 0–100) | Census ACS API |
| `hometown_pct_asian` | numeric | Percent of hometown residents who are non-Hispanic Asian (percent, 0–100) | Census ACS API |
| `hometown_pct_hispanic` | numeric | Percent of hometown residents who are Hispanic or Latino, any race (percent, 0–100) | Census ACS API |
| `hometown_pct_other_race` | numeric | Percent of hometown residents in all other non-Hispanic groups (percent, 0–100) | Census ACS API |
| `hometown_median_income` | numeric | Median household income of the hometown, ACS 2024 5-year (US dollars; censored at $2,500 and $250,000) | Census ACS API |
| `hometown_median_income_censored` | factor | Flags incomes at the ACS limit: "Bottom-coded (under $2,500)" or "Top-coded (over $250,000)"; NA = not censored | Derived from Census ACS (02_clean) |
| `hometown_poverty_rate` | numeric | Percent of hometown residents below the poverty line (percent, 0–100) | Census ACS API |
| `hometown_bachelors_or_higher` | numeric | Percent of hometown residents 25+ with a bachelor's degree or higher (percent, 0–100) | Census ACS API |
| `hometown_longitude` | numeric | Longitude of a point inside the hometown geography (decimal degrees) | Census boundary files (tigris) |
| `hometown_latitude` | numeric | Latitude of a point inside the hometown geography (decimal degrees) | Census boundary files (tigris) |
| `hometown_land_area_sqmi` | numeric | Land area of the hometown geography (square miles); the whole county for county-level matches | Census boundary files (tigris) |
| `hometown_population_density` | numeric | Hometown population ÷ land area (persons per square mile) | Derived: Census ACS ÷ Census boundary files |
| `hometown_county_geoid` | character | 5-digit FIPS code of the county containing the hometown (state + county) | Census ACS API |
| `hometown_county_name` | character | Name of the county containing the hometown (e.g., Baldwin County) | Census ACS API |
| `hometown_county_population` | integer | Total population of the hometown county, ACS 2024 5-year (persons) | Census ACS API |
| `hometown_county_median_income` | numeric | Median household income of the hometown county, ACS 2024 5-year (US dollars) | Census ACS API |
| `hometown_county_poverty_rate` | numeric | Percent of hometown county residents below the poverty line (percent, 0–100) | Census ACS API |
| `hometown_county_bachelors_or_higher` | numeric | Percent of hometown county residents 25+ with a bachelor's degree or higher (percent, 0–100) | Census ACS API |
| `hometown_county_population_density` | numeric | Hometown county population ÷ land area (persons per square mile) | Derived: Census ACS ÷ Census boundary files |

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