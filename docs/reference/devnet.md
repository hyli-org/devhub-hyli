# Devnet configuration

## Recommended: Run from source

For a single-node devnet (consensus disabled) with an indexer, clone the [hyle repository](https://github.com/hyli-org/hyli) and run:

```sh
cargo run -- --pg
```

This command starts a temporary PostgreSQL server and erases its data when you stop the node.

### Optional: Persistent storage

For persistent storage, start a standalone PostgreSQL instance:

```bash
# Start PostgreSQL with default configuration:
docker run -d --rm --name pg_hyle -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
```

Then, navigate to the Hyli root and run:

```bash
cargo run
```

## Alternative: Start with Docker

Use Docker to run a local node. Note that the devnet is unstable and may break with future updates.

### Pull the Docker image

```bash
docker pull ghcr.io/hyli-org/hyli:v0.12.1
```

### Run the Docker container

```bash
docker run -v ./data:/hyle/data -p 4321:4321 ghcr.io/hyli-org/hyli:v0.12.1
```

If you run into an error, try adding the `--privileged` flag:

```bash
docker run --privileged -v ./data:/hyle/data -p 4321:4321 ghcr.io/hyli-org/hyli:v0.12.1
```

To run with an indexer, add the parameter `-e HYLE_RUN_INDEXER=true` and set up a running PostgreSQL server with Docker:

```bash
docker run -d --rm --name pg_hyle -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
```

And the node linked to it:

```bash
docker run -v ./data:/hyle/data \
    -e HYLE_RUN_INDEXER=true \
    -e HYLE_DATABASE_URL=postgres://postgres:postgres@pg_hyle:5432/postgres \
    --link pg_hyle \
    -p 4321:4321 \
    ghcr.io/hyli-org/hyli:v0.7.2
```

You can now [create your first app](../quickstart/example/first-token-contract.md).

!!! tip
To reset your devnet, delete the ./data folder and restart from Step 1. Otherwise, you risk re-registering a contract that still exists.

## Alternative: Build the Docker image locally

If you prefer to build the image from source, run:

```bash
docker build -t hyli-org/hyli . && docker run -dit hyli-org/hyli
```

## Configuration

<!--Put on docs.rs when we'll be ready.-->

You can configure your setup using environment variables or by editing a configuration file.

### Using a configuration file

To load settings from a file, place `config.toml` in your node's working directory. It will be detected automatically at startup.

For documentation, see the defaults at [src/utils/conf_defaults.toml](https://github.com/hyli-org/hyli/blob/main/src/utils/conf_defaults.ron).

For Docker users, mount the config file when running the container:

```bash
docker run -v ./data:/hyle/data -v ./config.run:/hyle/config.toml -e HYLE_RUN_INDEXER=false -p 4321:4321 -p 1234:1234 ghcr.io/hyli-org/hyli:v0.12.1
cp ./src/utils/conf_defaults.toml config.toml
```

For source users, copy the default config template:

```bash
cp ./src/utils/conf_defaults.toml config.toml
```

### Using environment variables

All variables can be customized on your single-node instance.
The mapping uses 'HYLE\_' as a prefix, then '\_\_' where a '.' would be in the config file.

e.g.

`id` is set with `HYLE_ID="your_id"`.
`run_indexer` is set with `HYLE_RUN_INDEXER="true"`.
`p2p.address` is set with `HYLE_P2P__ADDRESS="127.0.0.1:4321"` (note the double \_\_ for the dot).
