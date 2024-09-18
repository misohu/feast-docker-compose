
# Feast Docker Demo

This repository is an extension of the [Feast Feature Store](https://docs.feast.dev/getting-started/quickstart) demo with several key differences:

- **PostgreSQL** is used as the **offline store** to store historical feature data.
- **Redis** is used as the **online store** to store features that are available in real-time.
- **PostgreSQL** is also used to store the **Feature Registry** data, which keeps track of all the feature definitions and metadata.
- A **custom Docker container** is used to run the Feast UI, which allows you to interact with your feature store via a browser.

## Project Structure

```
.
├── data
│   ├── driver_stats.parquet         # Parquet file with feature data for offline store
│   └── load-data.ipynb              # Notebook to load data into the offline store
├── demo.ipynb                       # Example notebook to interact with the Feast feature store
├── docker-compose.yaml              # Docker Compose file to set up the services
├── feast_ui                         # Folder containing Dockerfile and requirements for the custom Feast UI container
│   ├── Dockerfile                   # Dockerfile to build the Feast UI container
│   └── requirements.txt             # Required Python packages for the Feast UI container
├── feature_store_compose.yaml       # Feature store configuration used by the Docker Compose setup
├── feature_store.yaml               # Feature store configuration for local development (e.g., notebooks)
├── README.md                        # This README file
├── specs
│   └── feature-repo.py              # Python script defining the feature repository
└── test_workflow.py                 # Example test workflow for the project
```

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed on your machine.
- Basic knowledge of [Feast](https://docs.feast.dev/getting-started/quickstart), as this demo builds on top of the official Quickstart guide.

### Setup

This project uses Docker Compose to spin up the following services:

- **PostgreSQL** as the offline store and feature registry store.
- **Redis** as the online store.
- **Feast UI** in a custom container, exposed on port `8889`.

To start the services, run:

```bash
docker-compose up --build
```

This will:

1. Spin up a PostgreSQL container for the offline store.
2. Spin up a Redis container for the online store.
3. Spin up a PostgreSQL container to store the Feast registry data.
4. Build and start a custom Feast UI container, which can be accessed at `http://localhost:8889`.

### Feature Store Configuration

#### `feature_store.yaml`

The `feature_store.yaml` file is used when working with Feast locally, for example, if you're running a notebook like `demo.ipynb` and want to interact with Feast from your local environment. The configuration specifies PostgreSQL as the offline store and Redis as the online store.

#### `feature_store_compose.yaml`

The `feature_store_compose.yaml` file is used by the Docker Compose setup and is mounted into the Feast UI container. This configuration also uses PostgreSQL for the offline store, Redis for the online store, and PostgreSQL to store the registry data.

### Working with Feast UI

After running `docker-compose up`, you can access the Feast UI at:

```
http://localhost:8889
```

From the UI, you can interact with your feature store, including viewing feature definitions, feature sets, and more.

### Example Notebooks

- **`demo.ipynb`**: This notebook demonstrates how to interact with the Feast feature store from a Jupyter notebook. It uses the `feature_store.yaml` configuration, so you can run it locally after the services are running.
  
- **`load-data.ipynb`**: This notebook loads sample data (`driver_stats.parquet`) into the PostgreSQL offline store. You can use this notebook to prepare your data for the feature store.

### Offline Store: PostgreSQL

PostgreSQL is used as the offline store to hold historical data, which is then used to compute features. The `offline-store` service in `docker-compose.yaml` runs the PostgreSQL container for the offline store.

### Online Store: Redis

Redis is used as the online store to store features that need to be served in real-time. The `online-store` service in `docker-compose.yaml` runs the Redis container for this purpose.

### Feature Registry: PostgreSQL

A separate PostgreSQL instance is used to store the Feast registry, which tracks feature definitions and metadata. The `registry` service in `docker-compose.yaml` runs this PostgreSQL container.

### Custom Feast UI

The Feast UI is built from the `feast_ui` directory using a custom Dockerfile. It installs the necessary Feast dependencies and runs the UI on port `8889`.

To modify the Feast UI or its dependencies, you can edit the following files:

- **Dockerfile**: The Dockerfile used to build the Feast UI container.
- **requirements.txt**: The list of Python packages installed in the UI container.

### Accessing Feast Locally

You can interact with Feast locally using the `feature_store.yaml` configuration. This can be useful for running Jupyter notebooks or scripts that access the feature store. For example, you can use the `demo.ipynb` notebook to access features defined in the store.

### Cleaning Up

To stop the running containers, press `Ctrl+C` or run:

```bash
docker-compose down
```

This will stop and remove the containers.

If you want to remove all associated volumes (including stored data), run:

```bash
docker-compose down -v
```
