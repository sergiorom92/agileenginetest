# Sergio Romero - Agile Engine Test

## Setup

Create a `.env` file containing the following variables in the root of this repository:

``` env
DEV_PORT=3000
PORT=80
```

## Run dev environment

To run the development environment, run the following commnand:

``` console
docker compose -f docker-compose-dev.yml up
```

> This will start the development environment under `http://localhost:3000`

## Run prod environment

To run the production environment, run the following commnand:

``` console
docker compose up
```

> This will start the production environment under `http://localhost`
