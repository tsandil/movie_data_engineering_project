# Movie Data Engineering Project
This repository demonstrates a complete **Data Engineering Pipeline** that **Extracts, Loads and Transforms (ELT)** movie data from **The TMDB API** into **PostgreSQL** database and then models the data using **dbt (Data Build Tool)** for analysis and visualizes using **Tableau Public**.

## Project Overview

The project is divided into three main parts:

1. **ETL Pipeline**:

    - **Extract**: Movie data is fetched from the **TMDB API** from different endpoints.

    - **Load**: The raw data from the API is directly loaded into **PostgreSQL** database handling the schema changes.

    - **Transform**: The raw data is then cleaned, transformed and modeled using **dbt(Data Build Tool)** for analytical purposes.

2. **Data Modeling using dbt**:

    - The raw data stored in PostgreSQL undergoes structured transformation using **dbt**, following the **Medallion Architecture**, which organizes data into three layers(Bronze, Silver and Gold Layers) for data modeling.

    - **ELT with Medallion Architecture** : Data extracted from TMDB API from different endpoints passes through three Layers. 
        
        1. **Bronze Layer** : Raw data is ingested into the system with inconsistencies, missing values, and stored in PostgreSQL without any transformation.

        2. **Silver Layer** : Data in the Bronze Layer is cleaned and then standarized by handling the missing values and generating surrogate keys through different staging layers.

        3. **Gold Layer** : In this Layer, the data from silver layer is structured into **facts** and **dimension** following **Kimball's Data Modeling Principle**.

            - **Fact Tables**: These focus on quantitative data, such as vote counts.
            - **Dimension Tables**: These contain all the descriptive data such as movie language, description, release dates and so on.
    
    - **Star Schema for Analytics** : The final transformed data is organized into fact and dimension tables, making it easy to use for BI tools.

3. **Data Visualization**:
    - The transformed data is visualized using **Tableau  Public** for better insights and analysis.

### Data Visualization

![dashboard image](dashboard.png)

### Data Architecture

![architecture image](movie_data_architecture.png)

#### Why This Architecture?

I chose this architecture because it follows best practices for building scalable and efficient data pipelines. Instead of traditional **ETL** approach, I implemented **ELT** paradigm, using Apache Airflow for orchestration, PostgreSQL as database and **dbt (Data Build Tool)** for transformation and Tableau Public for visualization.

- Apache Airflow because it has strong scheduling, dependency management, and integrates well with PostgreSQL and dbt. It also supports task parallelization and monitoring, making it an ideal choice.
- PostgreSQL because it is a powerful, open-source relational database with strong performance and scalability and reliable for managing large datasets.
- For transformation, dbt(Data Build Tool) was the perfect choice because it enables modular SQL-based transformations, integrates well with PostgreSQL and has an excellent documentation. It promotes version control, testing and documentation.
- Tableau Public because its free and easy to use.

This architecture ensures scalability and high performance and follows best practices used in real-world data engineering.

## Prerequisites

Before running the project, ensure that you have the following setup.
1. Clone both the ```the_movies-db``` and ```dbt_movie_db``` repositories into your local machine.

    ```bash
    git clone https://github.com/tsandil/the_movies_db.git
    git clone https://github.com/tsandil/dbt_movie_db.git
    cd the_movies_db
    ```
1. **PostgreSQL Database** : Ensure PostgreSQL is installed and running.
2. **Airflow** : Apache Airflow is used for Orchestration.
    - Install Airflow with [PYPI](https://airflow.apache.org/docs/apache-airflow/stable/installation/installing-from-pypi.html)

3. **Docker** : Docker is required to run Airflow in a containerized environment.
    - Install Docker from [here](https://www.docker.com/get-started).

4. **Python Libraries** : The following libraries has been used
- ```requests```
- ```pandas```
- ```sqlalchemy```
- ```json```
- ```airflow```
- ```PostgresHook``` from ```airflow.providers.postgres```
- **Install Dependencies** : ```pip install -r requirements.txt```

5. **dbt (Data Build Tool)** : Install dbt to run data transformation models:

    ```bash
    pip install dbt-core dbt-postgres
    ```

## How to Run This Project

1. Configuration

    1. **API Key** : Add the TheMovieDB API key as an Airflow variable named ```the_moviedb_auth_key``` in Airflow's Admin > Variables.

    2. **PostgreSQL Connection** : Set up a PostgreSQL connection in Airflow with the connection ID ```themovies_con```.
        - host: `host.docker.internal` if running from local machine
        - database: `your_database`

    3. Start Airflow

        1. **Initialize the Project**:

            ```bash
            astro dev init
            ```

        2. **Start the Project**:

            ```bash
            # Start the Apache Airflow locally using the Astro CLI
            astro dev start
            ```

        3. **Access the Airflow Web UI**:

            Go to `http://localhost:8080` and use default credentials:
            - **Username**: `admin`
            - **Password**: `admin`
        
        4. **Trigger the DAG** : Once Airflow is running, trigger the DAG from the Airflow UI to begin the ETL process.

        5. **Switch to ```dbt_movie_db``` Project** : 
            
            ```bash
            cd the_movies_db
            ```
        
        6. **Run dbt Models** : After the DAG completes, run the DBT models to transform the data.

            ```bash
            dbt run
            ```
## Lessons Learned

- Gained a deeper understanding of the **ETL** vs **ELT** paradigm.
- Understood how **Data Modeling** and **Transformations** play a crucial role in analytics. 
- Implemented **Medallion Architecture** and **Kimball's Principles** for **Data Modeling** to structure data effectively. 
- Learned to manage and orchestrate tasks with **Apache Airflow**, using parallel processing to handle multiple mapped tasks efficiently.
- Handling data with **PostgreSQL** and **dbt (Data Build Tool)** gave me valuable experience in managing and transforming large datasets.
- Realized the inportance of  **data visualization** for making insights more accessible.
- If I had more time or resources, I would have focused on following:
    - Improving the ETL pipeline to handle larger datasets more efficiently.
    - Enhancing the data transformation process with dbt for deeper analytics. 
    - Consider using **Apache Spark** for better scalability. 
    - The tools I used, like **PostgreSQL, dbt, and Airflow**, were a great fit for the project, but with more time/resources, I'd like to explore the alternatives like **Prefect** for orchestration and **Snowflake** instead of **PostgreSQL**
    - Refining the visualizations and make the reports even more interactive.

## Contact

Please feel free to contact me if you have any questions at:
- [LinkedIn](https://www.linkedin.com/in/sandil-tandukar-5b1259299)
- Email - tan.sandil44@gmail.com