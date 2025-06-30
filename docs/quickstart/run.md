# Run the scaffold locally

## Prerequisites

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- Install openssl-dev (e.g. `apt install openssl-dev` or `cargo add openssl`).
- [Bun](https://bun.sh/) (or npm/yarn)

- For the scaffold example, [install RISC Zero](https://dev.risczero.com/api/zkvm/install) and [Noir](https://noir-lang.org/docs/getting_started/quick_start). You can also use [SP1](https://docs.succinct.xyz/docs/sp1/introduction).

## Run a node locally

Clone [the Hyli node](https://github.com/hyli-org/hyli).

Run:

```sh
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
git checkout v0.13.1
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
```

You can now use [the Hyli explorer](https://explorer.hyli.org). Select `localhost` in the upper-right corner.

For alternative setups, optional features, and advanced configurations, check out [the local node reference page](../reference/run.md).

## Run the wallet

Clone [the Wallet repository](https://github.com/hyli-org/wallet/).

Wait until the node has successfully launched.

Run:

```sh
git checkout v0.1.2
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release -- -m -a -w
```

## Run the app scaffold

Clone [the Hyli app scaffold](https://github.com/hyli-org/app-scaffold/).

Launch the server:

```sh
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release
```

Start the scaffold's UI:

```sh
cd front
bun install && bun run dev
```

Everything is now functional. We can now explore the scaffold!