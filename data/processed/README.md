Cleaned and merged files written by markdowns/02 to 04.

***Data Dictonary for fbs_players_merged.csv***

| Variable name | Data type | Description |
|---|---|---|
| school | factor | FBS school whose 2026 roster lists the player (139 schools) |
| conference | factor | FBS conference of the school, 2026 alignment (11 conferences, including FBS Independent) |
| name | character | Player's name as listed on the school roster |
| year | factor | Eligibility year, 1–8; redshirt status ignored (R-Fr. = 1); NA = not listed |
| position | factor | Position, spelled out (e.g., Wide Receiver); multiple positions separated by "/" |
| player_id | character | collegefootballdata.com (CFBD) player ID; NA = no matching 2026 stats |
| match_method | factor | How the roster player was matched to CFBD stats: "Exact name" or "First initial + last name"; NA = not matched |
| season | integer | Season of the CFBD statistics (2026); NA = not matched |
| height_in | numeric | Player height in inches |
| weight_lbs | integer | Player weight in pounds |
| hometown_city | character | Player's hometown city as listed on the roster |
| hometown_state | factor | Hometown state as a USPS code (e.g., OH); full province name for non-US hometowns |
| hometown_country | factor | Hometown country (e.g., United States, Canada, Australia) |
| previous_school_1 | character | Most recently listed previous college (transfers); NA = none |
| previous_school_2 | character | Second listed previous college; NA = none |
| previous_school_3 | character | Third listed previous college; NA = none |
| receiving_longest_yards | integer | Longest reception of the season, in yards |
| receptions | integer | Total receptions (count) |
| receiving_touchdowns | integer | Receiving touchdowns (count) |
| receiving_yards | integer | Total receiving yards |
| receiving_yards_per_reception | numeric | Receiving yards per reception (yards) |
| rushing_carries | integer | Total rushing attempts (count) |
| rushing_longest_yards | integer | Longest rush of the season, in yards |
| rushing_touchdowns | integer | Rushing touchdowns (count) |
| rushing_yards | integer | Total rushing yards (can be negative) |
| rushing_yards_per_carry | numeric | Rushing yards per carry (yards) |
| passes_defended | integer | Passes defended/broken up (count) |
| qb_hurries | integer | Quarterback hurries (count) |
| sacks | numeric | Sacks (count; half credit allowed for shared sacks) |
| tackles_solo | integer | Solo tackles (count) |
| defensive_touchdowns | integer | Defensive touchdowns (count) |
| tackles_for_loss | numeric | Tackles for loss (count; half credit allowed) |
| tackles_total | integer | Total tackles, solo plus assisted (count) |
| interceptions_yards_per_return | numeric | Interception return yards per interception (yards) |
| interceptions | integer | Interceptions made (count) |
| interceptions_touchdowns | integer | Interceptions returned for a touchdown (count) |
| interceptions_yards | integer | Total interception return yards |
| punt_returns_yards_per_return | numeric | Punt return yards per return (yards) |
| punt_returns_longest_yards | integer | Longest punt return of the season, in yards |
| punt_returns | integer | Punt returns (count) |
| punt_returns_touchdowns | integer | Punt returns for a touchdown (count) |
| punt_returns_yards | integer | Total punt return yards |
| passing_attempts | integer | Pass attempts (count) |
| passing_completions | integer | Pass completions (count) |
| passing_interceptions | integer | Interceptions thrown (count) |
| completion_percent | numeric | Completions ÷ attempts × 100 (percent, 0–100) |
| passing_touchdowns | integer | Passing touchdowns (count) |
| passing_yards | integer | Total passing yards |
| passing_yards_per_attempt | numeric | Passing yards per attempt (yards) |
| field_goals_attempted | integer | Field goals attempted (count) |
| field_goals_made | integer | Field goals made (count) |
| field_goal_longest_yards | integer | Longest made field goal, in yards |
| field_goal_percent | numeric | Field goals made ÷ attempted × 100 (percent, 0–100) |
| kicking_points | integer | Points scored by kicking: 3 × field goals + extra points (points) |
| extra_points_attempted | integer | Extra points attempted (count) |
| extra_points_made | integer | Extra points made (count) |
| punts_inside_20 | integer | Punts downed inside the opponent's 20-yard line (count) |
| punting_longest_yards | integer | Longest punt of the season, in yards |
| punts | integer | Punts (count) |
| punting_touchbacks | integer | Punts resulting in a touchback (count) |
| punting_yards | integer | Total punting yards |
| punting_yards_per_punt | numeric | Average yards per punt (yards) |
| kick_returns_yards_per_return | numeric | Kickoff return yards per return (yards) |
| kick_returns_longest_yards | integer | Longest kickoff return of the season, in yards |
| kick_returns | integer | Kickoff returns (count) |
| kick_returns_touchdowns | integer | Kickoff returns for a touchdown (count) |
| kick_returns_yards | integer | Total kickoff return yards |
| fumbles | integer | Fumbles (count) |
| fumbles_lost | integer | Fumbles lost to the opponent (count) |
| fumbles_recovered | integer | Fumbles recovered (count) |
| hometown_geoid | character | 7-digit Census place FIPS code of the hometown (state + place); NA = no Census match or non-US |
| hometown_place_type | factor | Census place type (city, town, village, borough, CDP, etc.) |
| hometown_population | integer | Total population of the hometown, ACS 2024 5-year (persons) |
| hometown_median_age | numeric | Median age of hometown residents, ACS 2024 5-year (years) |
| hometown_pct_male | numeric | Percent of hometown residents who are male (percent, 0–100) |
| hometown_pct_white | numeric | Percent of hometown residents who are non-Hispanic white (percent, 0–100) |
| hometown_pct_black | numeric | Percent of hometown residents who are non-Hispanic Black (percent, 0–100) |
| hometown_pct_asian | numeric | Percent of hometown residents who are non-Hispanic Asian (percent, 0–100) |
| hometown_pct_hispanic | numeric | Percent of hometown residents who are Hispanic or Latino, any race (percent, 0–100) |
| hometown_pct_other_race | numeric | Percent of hometown residents in all other non-Hispanic groups (percent, 0–100) |
| hometown_median_income | numeric | Median household income of the hometown, ACS 2024 5-year (US dollars; censored at $2,500 and $250,000) |
| hometown_median_income_censored | factor | Flags incomes at the ACS limit: "Bottom-coded (under $2,500)" or "Top-coded (over $250,000)"; NA = not censored |
| hometown_poverty_rate | numeric | Percent of hometown residents below the poverty line (percent, 0–100) |
| hometown_bachelors_or_higher | numeric | Percent of hometown residents 25+ with a bachelor's degree or higher (percent, 0–100) |
| hometown_longitude | numeric | Longitude of a point inside the hometown place (decimal degrees) |
| hometown_latitude | numeric | Latitude of a point inside the hometown place (decimal degrees) |
| hometown_land_area_sqmi | numeric | Land area of the hometown place (square miles) |
| hometown_population_density | numeric | Hometown population ÷ land area (persons per square mile) |
| hometown_county_geoid | character | 5-digit FIPS code of the county containing the hometown (state + county) |
| hometown_county_name | character | Name of the county containing the hometown (e.g., Baldwin County) |
| hometown_county_population | integer | Total population of the hometown county, ACS 2024 5-year (persons) |
| hometown_county_median_income | numeric | Median household income of the hometown county, ACS 2024 5-year (US dollars) |
| hometown_county_poverty_rate | numeric | Percent of hometown county residents below the poverty line (percent, 0–100) |
| hometown_county_bachelors_or_higher | numeric | Percent of hometown county residents 25+ with a bachelor's degree or higher (percent, 0–100) |
| hometown_county_population_density | numeric | Hometown county population ÷ land area (persons per square mile) |