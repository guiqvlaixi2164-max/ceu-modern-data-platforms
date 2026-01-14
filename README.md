# ceu-modern-data-platforms
Data Engineering 2 - Modern Data Platforms. dbt, Snowflake, Databricks, Apache Spark

## Installation

Databricks Setup:
1) Sign up for Databricks Free Edition: https://www.databricks.com/learn/free-edition

Snowflake Setup:

2) Register to Snowflake: https://signup.snowflake.com/?trial=student&cloud=aws&region=us-west-2
3) Set up Snowflake tables: https://dbtsetup.nordquant.com/

dbt Setup

4) Fork this repo and clone it to your PC: https://github.com/nordquant/dbt-student-repo
5) Ensure you have a compatible Python Version: https://docs.getdbt.com/faqs/Core/install-python-compatibility (if you don't, install Python 3.13) 
6) Install uv: https://docs.astral.sh/uv/getting-started/installation/
7) Install packages: `uv sync`
8) Activate the virtualenv
- Windows (PowerShell): `.\.venv\Scripts\Activate.ps1`
- Windows (CMD): `.venv\Scripts\activate.bat`
- macOS / Linux: `. .venv/bin/activate`

## Starting a dbt Project
Create a dbt project (all platforms):
```sh
dbt init --skip-profile-setup airbnb
```
Once done, drag and drop the `profiles.yml` file you downloaded to the `airbnb` folder.

Try if dbt works
`dbt debug`

# Models
## Code used in the lesson

### SRC Listings
`models/src/src_listings.sql`:

```sql
WITH raw_listings AS (
    SELECT
        *
    FROM
        AIRBNB.RAW.RAW_LISTINGS
)
SELECT
    id AS listing_id,
    name AS listing_name,
    listing_url,
    room_type,
    minimum_nights,
    host_id,
    price AS price_str,
    created_at,
    updated_at
FROM
    raw_listings

```

### SRC Reviews
`models/src/src_reviews.sql`:

```sql
WITH raw_reviews AS (
    SELECT
        *
    FROM
        AIRBNB.RAW.RAW_REVIEWS
)
SELECT
    listing_id,
    date AS review_date,
    reviewer_name,
    comments AS review_text,
    sentiment AS review_sentiment
FROM
    raw_reviews
```


## Exercise

Create a model which builds on top of our `raw_hosts` table.

1) Call the model `models/src/src_hosts.sql`
2) Use a CTE (common table expression) to define an alias called `raw_hosts`. This CTE selects every column from the raw hosts table `AIRBNB.RAW.RAW_HOSTS`
3) In your final `SELECT`, select every column and record from `raw_hosts` and rename the following columns:
   * `id` to `host_id`
   * `name` to `host_name`

### Solution

```sql
WITH raw_hosts AS (
    SELECT
        *
    FROM
       AIRBNB.RAW.RAW_HOSTS
)
SELECT
    id AS host_id,
    NAME AS host_name,
    is_superhost,
    created_at,
    updated_at
FROM
    raw_hosts
```

