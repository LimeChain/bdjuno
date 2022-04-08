# BDJuno
[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/forbole/bdjuno/Tests)](https://github.com/forbole/bdjuno/actions?query=workflow%3ATests)
[![Go Report Card](https://goreportcard.com/badge/github.com/forbole/bdjuno)](https://goreportcard.com/report/github.com/forbole/bdjuno)
![Codecov branch](https://img.shields.io/codecov/c/github/forbole/bdjuno/cosmos/v0.40.x)

BDJuno (shorthand for BigDipper Juno) is the [Juno](https://github.com/forbole/juno) implementation
for [BigDipper](https://github.com/forbole/big-dipper).

It extends the custom Juno behavior by adding different handlers and custom operations to make it easier for BigDipper
showing the data inside the UI.

All the chains' data that are queried from the RPC and gRPC endpoints are stored inside
a [PostgreSQL](https://www.postgresql.org/) database on top of which [GraphQL](https://graphql.org/) APIs can then be
created using [Hasura](https://hasura.io/).

## Usage
To know how to setup and run BDJuno, please refer to
the [docs website](https://docs.bigdipper.live/cosmos-based/parser/overview/).

### Build the binary: `./build/bdjuno`
```
make build
```

### Spin up Hasura and Postgres containers
```
docker-compose -f docker-compose-dev.yml up --build
```

### Import the database schemas
```
docker cp ./database/schema your_postgres_container_id:/schema
docker exec -it your_postgres_container_id bash
cd schema
psql -U postgres -d mantrachain -f ...
```


### Parse our dev genesis
```
./build/bdjuno parse genesis-file --home ./configuration
```

### Install Hasura cli
Use the described method for your OS

https://hasura.io/docs/latest/graphql/core/hasura-cli/install-hasura-cli/#install-hasura-cli

### Import the metadata to the running Hasura instance
```
cd ./hasura
hasura metadata apply --endpoint http://localhost:8080
```
Change `--endpoint` according to your current running Hasura instance

### Start bdjuno Hasura actions
```
./build/bdjuno hasura-actions --home ./configuration
```

### Start bdjuno
```
./build/bdjuno start --home ./configuration
```