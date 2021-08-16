# Sparkify

ETL (Extract, Transform & Load) Pipline to extract user activity and song data from json files and ingestion into a Postgres Database.
This Project is part of the [Udacity Data Engineering nanodegree](https://www.udacity.com/course/data-engineer-nanodegree--nd027).
For convenience i have created a microservice infrastructure (powered by docker) to improve reproducibility.

- [About](#about)
- [ETL Pipline and Database Design](#etl-pipline-and-database-design)
- [Tech/Framework used](#tech/framework-used)
- [Deployment](#deployment)
- [Getting Started](#getting_started)
- [Files](#files)

## About

This project was created for Sparkify, a music streaming provider. To improve the added value for the
customer, the analytics team needs to be able to query easily the song play activity.
The system design does not allow easy analytics of the song play activity and therefore requires
an ETL process in combination with database.

## ETL Pipline and Database Design

### The Raw Data

The raw data consists of

- song_data json files
- log data files

**song_data**
Song data contains the metadata about song AND the artist

**log_data**
Log data contains metadata about the user activity (e.g. When, Where, What song etc. )

### Database Design

The analytics team is primarily interessted in the song play activity. Therefore the design is centered
around the songplay data, leading to the following setup:

Central the songplay table (fact table ***songplays**).
Connected to 4 dimension tables:

- users
- songs
- time
- artist

### ETL Design

The ETL pipline has 5 essential components.

1. ETL of song data into songs from song_data.
2. ETL of artist_data into artist from song_data.
3. ETL of time_data from logs
4. ETL of user_data from logs
5. ETL of songplay_data from logs (and referencing to the other tables)

## Tech/Framework used

- Docker
- Jupyter Notebook
- Postgres

## Deployment

Please install docker.
[Docker](https://www.docker.com/)

## Getting Started

### Build the containers

```cmd

docker-compose build

```

### Start the containers

```cmd

$ docker-compose up

...
...
 or http://127.0.0.1:8888/?token=xxxxxxxxxxxxxxxxxxxxx

```

Docker compose will spin up two containers:

- sparkifydb (the postgres database)
- jupyter (jupyter notebook container)

Jupyter displays multiple links to start working in the browser. Use the last one with "http://127.0.0.1:8888/?token=xxxxx

### Create the Database Schema

Run create_tables.py

```cmd

docker-compose run --rm jupyter_notebook python create_tables.py

```

### Extract, transfer load the data into the Database

Run etl.py via the terminal

```cmd

docker-compose run --rm jupyter_notebook python etl.py

```

### Check Data

Open test.ipynb and run the queries.

>Make sure to restart the kernel.

### Example Queries

Open test.ipynb, run the queries.

Only return songplay activity from Tampa-St. Petersburg-Clearwater, FL:

```sql

%sql SELECT * FROM songplays WHERE songplays.location='Tampa-St. Petersburg-Clearwater, FL';

```

Show how much total songplay activity by user id:

```sql

%sql SELECT user_id, COUNT(songplay_id) FROM songplays GROUP BY user_id;

```

## Files

- The Raw data of the Project can be found in **workspace/data** folder.
- **workspace/create_table.py**
  - Creates the sparkify database (drops the previous if one exists already).
  - Drops all existing tables.
  - Creates all tables.
  - Closes the connection.
- **workspace/sql_queries.py**
  - Drop table sql statement
  - Create table DDL for all tables
  - Insert sql statements for all tables
  - Select statement for ETL pipline
  - Query lists (Lists of Drop, Create, Insert statements)
- **workspace/etl.ipynb** is a helper tool for the desgin & development of the prototype of the ETL pipline.
- **workspace/test.ipynb** is a helper tool to quickly check all tables.
- **worksapce/etl.py** final ETL pipline of the project.
- **workspace/Sample.ipynb** helper tool to get quickly experiment.
- **README.md** documentation of the project.
- **requirements.txt** packages installed in jupyter_notebook container
- **docker-compose.yml** docker-compose file defining the services
- **compose/jupyter-notebook/Dockerfile** Dockerfile for jupyter_notebook
- **compose/postgres/Dockerfile** Dockerfile for postgres
- **.envs/.postgres** Environment file containing user, password etc. !! Do not share this in your repository !!
- **.gitignore** Python gitignore provided by GitHub

## Credits and Acknowledgments

This project is part of the [Udacity Data Engineering nanodegree](https://www.udacity.com/course/data-engineer-nanodegree--nd027).

Helpfull documentation and resources:

- [Docker Documentation](https://docs.docker.com/)
- [Jupyter Docker Stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/using/recipes.html)
- [Markdown Guide](https://www.markdownguide.org/basic-syntax/)
- [A Beginners Guide to writing a Kickass README](https://meakaakka.medium.com/a-beginners-guide-to-writing-a-kickass-readme-7ac01da88ab3)
- [How to write a good README for your GitHub project?](https://bulldogjob.com/news/449-how-to-write-a-good-readme-for-your-github-project)
- [Awesome README](https://github.com/matiassingers/awesome-readme)
- [SQL Style Guide](https://www.sqlstyle.guide/)
- [Kickstarter sql style guide](https://gist.github.com/fredbenenson/7bb92718e19138c20591)
