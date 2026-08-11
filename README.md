"SQL analysis of flight/airport data to uncover passenger traffic, route, and population insights."
# Airport Flight Data SQL Analysis

## Problem Statement
Air travel is a key component of global connectivity and economic development, but understanding patterns of airport usage — passenger flow, route popularity, and how city population affects operations — remains complex. This project analyzes airport and flight data to uncover trends in passenger volume, flight frequency, and the relationship between city population and air traffic, with the goal of supporting airport planning, route optimization, and policy-making.

**Data source:** BTS T-100 Domestic Market (All Carriers) dataset — ~19,900 flight records with passenger, freight, mail, carrier, origin/destination, and route-level details.

## Schema
Data was normalized into four relational tables:

![ER Diagram](./er_diagram.png)

| Table | Purpose |
|---|---|
| `AIRPORT` | Airport ID, code, city, state — one row per unique airport |
| `AIRLINE` | Airline ID, carrier code, carrier name |
| `FLIGHT` | One row per flight record — passengers, freight, mail, distance, origin/destination airport IDs (foreign keys), year/quarter/month |
| `CITY_POP` | City name and population, used to study population's effect on traffic |

`FLIGHT` intentionally does not store city names or airport codes directly — those are retrieved via JOIN to `AIRPORT`, avoiding redundant storage (3NF normalization).

## Key Findings
1. **Total passenger traffic per route** — Identified the top 10 busiest and least busy routes by passenger volume.
2. **Average passengers per flight** — Calculated per route and per airport, combining inbound and outbound legs.
3. **Flight frequency** — Identified the highest-traffic city-pair corridors by flight count.
4. **Top-performing origin airports** — Atlanta, Denver, and Dallas-Fort Worth handle the highest outbound passenger volumes.
5. **Seat utilization** — Not directly measurable; the raw dataset has no seat capacity column (see Limitations).
6. **Top destination airports** — Same leaders (ATL, DEN, DFW) dominate inbound traffic as well.
7. **Population vs. passenger traffic** — Examined the ratio of total air traffic to city population, city by city.
8. **Population vs. flight frequency** — Assessed whether larger origin/destination populations correlate with higher route frequency.

Full queries: [`queries.sql`](./queries.sql)

## Data Cleaning & Challenges
Real-world data required a couple of non-obvious fixes during analysis:

- **Multi-airport cities (New York, Houston, etc.):** Aggregating traffic by airport before joining to city-level population data caused the same city to appear multiple times with inflated totals. Fixed by summing traffic to one row per city *before* joining population.
- **City name string-splitting bug (Dallas/Fort Worth):** An overly broad string-cleaning step meant to strip country suffixes accidentally merged "Dallas/Fort Worth" into "Dallas," colliding two distinct metro areas. Fixed by narrowing the string-split logic.

## Limitations
- No seat capacity/seats column exists in the source data, so seat utilization (Objective 5) could not be directly measured.

## Tools Used
"SQL — built as a self-directed data analysis project."


