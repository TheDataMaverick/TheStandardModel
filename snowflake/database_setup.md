# The Standard Model Database Setup

Remember to star 🌟 and watch  this repository. 

If anything, **and I do mean anything**, is unclear, please reach out either via an [issue](https://github.com/TheDataMaverick/TheStandardModel/issues), by [tagging](https://getdbt.slack.com/archives/CETJLH1V3/p1684844381972109) me on the [dbt Slack community](https://www.getdbt.com/community/join-the-community/), or by sending me an email at martin@imus.dk.

If you think something could be improved, let’s discuss it on the [dbt Slack community](https://www.getdbt.com/community/join-the-community/), just write your suggestion and [tag me](https://getdbt.slack.com/archives/CETJLH1V3/p1684844381972109). 🙏

*If 10,000 developers use The Standard Model as a basis for their data platform, the smallest improvement can really make a difference in people's lives!* 🦸‍♂️

**The Standard Model has two database types:** 
 - analytical
 - data loader


## Analytical database
The database for your data warehouse. 

You will have one analytical database per dbt project. Therefore, most companies will only have one analytical database.

The name of your analytical database could be your company’s name, your team name, or something that tells the user that this is where all your dbt models are located.

Don't create separate schemas for staging, different layers, or marts. *A large number of schemas will become unmanageable.*

The database has three types of schemas: data warehouse, development, and CI.

The analytical database is 100% managed by dbt and can be created via `dbt run` or `dbt build`. 

Create one Snowflake role to use for all dbt processes, (CD, CI, scheduled runs, and development). This significantly reduces errors. (See Snowflake setup).

Since Snowflake presents schemas in alphabetical order, you will have the production data warehouse schema on top, and all CI schemas will be under your development schemas.

Exposures, also called Marts and Subject areas, can be created with Snowflake roles that have select on specific tables, and via naming convention. (See Snowflake Setup) (See Exposures)

### Data warehouse schema
Simply called datawarehouse.

Where all production models are located.

Should always be a representation of your `main` branch.

The schema can be populated by a CD process, a scheduled run, by a run started by a developer, or by a run started by an orchestration system (like [Dagster](https://dagster.io/)).

### Development schemas

Each developer will have their own schema. 

The name should be: `dev__<user_name>`

Will be created when a developer does a `dbt run` or `dbt build`.

### CI schemas

A schema should be created on each PR.

The schema should be deleted when the PR is closed.

The name should be: `pr__<pr_number_000#> __<pr_name>`

## Data loader database

Each data loader (Fivetran, Airbyte, Meltano), should have its own database.

Name should be the data loader name: `fivetran`, `airbyte`, `meltano`

Each data source should have its own schema.

No one, except the data loader, should make changes to the database.
