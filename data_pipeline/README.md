# Module 1 — Data Pipeline
## Overview
This module implements an end-to-end data pipeline for scraping,
cleaning, transforming, normalizing, storing, and querying book
catalogue data from Books to Scrape.

Pipeline:
Scrape → Clean → Transform → Normalize → SQLite → SQL → Pandas
---

## Data Source
The data source is:
https://books.toscrape.com/
Books to Scrape is a public website designed specifically for
web-scraping practice.
No login, API key, or paid service is required.
---

## Scope
The pipeline scrapes books from at least three categories:
1. Travel
2. Mystery
3. Historical Fiction
The resulting dataset contains at least 60 book records.
---

## Fields Collected
The scraper collects:
- title
- price
- star rating
- availability
- category

After cleaning and transformation, the final dataset contains:

- title
- price_gbp
- price_inr
- star_rating
- rating
- availability
- in_stock
- category
---

## Cleaning Decisions
### Price
The original price contains the GBP currency symbol.
Example:
£51.77
The currency symbol is removed and the value is converted to
a floating-point number.
Example:
51.77
---

### Star Rating
The website represents ratings as text:
- One
- Two
- Three
- Four
- Five

These are converted to integers:
- One → 1
- Two → 2
- Three → 3
- Four → 4
- Five → 5
---

### Availability
Availability is converted to a Boolean value.
Examples:
In stock (22 available) → True
Out of stock → False
SQLite stores this Boolean as:
1 = True
0 = False
---

### Malformed Rows
If a required field cannot be parsed, the affected row is
dropped instead of allowing the pipeline to crash.
This approach was selected because the assignment allows rows
with parsing failures to be dropped, and fabricating values for
a product's price or rating would introduce potentially
incorrect data.
The number of dropped rows is printed during execution.
---

## Currency Conversion
The required project-defined fixed conversion rate is:
**1 GBP = 105.50 INR**
The INR price is calculated using:
price_inr = price_gbp × 105.50
No external currency API is used.
The fixed rate is deterministic and does not depend on a date
or live exchange rate.
---

## Database Design
SQLite is used as the relational database.
### categories

```text
category_id INTEGER PRIMARY KEY
category_name TEXT UNIQUE
