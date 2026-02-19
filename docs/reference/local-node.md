# Local node configuration

## Recommended: Run from source

For a single local node (consensus disabled), you can run **with** or **without** the indexer:

```sh
# Without indexer (faster dev loop; ephemeral)
HYLE_RUN_INDEXER=false cargo run

# With indexer (starts a temporary PostgreSQL; data cleared on stop)
cargo run -- --pg

# With SP1 verifier
cargo run -F sp1
```

### Optional: Persistent storage

For a persistent PostgreSQL:

```bash
docker run -d --rm --name pg_hyle -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres
```

Then, from the Hyli root:

```bash
HYLE_RUN_INDEXER=true \
HYLE_DATABASE_URL=postgres://postgres:postgres@localhost:5432/postgres \
cargo run
```

!!! tip
    To reset your local node, delete the ./data folder and restart from Step 1. Otherwise, you risk re-registering a contract that still exists.

## Alternative: Start with Docker

Use Docker to run a local node.

### Pull the Docker image

```bash
docker pull ghcr.io/hyli-org/hyli:v0.12.1
```

### Run the Docker container

Run without indexer:

```bash
docker run -v ./data:/hyle/data -p 4321:4321 ghcr.io/hyli-org/hyli:v0.12.1
```

If you run into an error, try adding the `--privileged` flag:

```bash
docker run --privileged -v ./data:/hyle/data -p 4321:4321 ghcr.io/hyli-org/hyli:v0.12.1
```

Run with indexer (requires a PostgreSQL container):

```bash
docker run -d --rm --name pg_hyle -p 5432:5432 -e POSTGRES_PASSWORD=postgres postgres

docker run -v ./data:/hyle/data \
    -e HYLE_RUN_INDEXER=true \
    -e HYLE_DATABASE_URL=postgres://postgres:postgres@pg_hyle:5432/postgres \
    --link pg_hyle \
    -p 4321:4321 \
    ghcr.io/hyli-org/hyli:v0.7.2
```

Your local node is now running. For application development, refer to the [scaffold repository](https://github.com/hyli-org/app-scaffold).

## Alternative: Build the Docker image locally

If you prefer to build the image from source, run:

```bash
docker build -t hyli-org/hyli . && docker run -dit hyli-org/hyli
```

## Configuration

<!--Put on docs.rs when we'll be ready.-->

You can configure your setup using environment variables or by editing a configuration file.

### Using a configuration file

Place a `config.toml` in the node’s working directory to load it automatically at startup.

See the defaults at [src/utils/conf_defaults.toml](https://github.com/hyli-org/hyli/blob/main/src/utils/conf_defaults.toml).

Docker users can mount the file:

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

Examples:

```sh
HYLE_RUN_INDEXER="true"
HYLE_P2P__ADDRESS="127.0.0.1:4321" # (note the double \_\_ for the dot)
```
