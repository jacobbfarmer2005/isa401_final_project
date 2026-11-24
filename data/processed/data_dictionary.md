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