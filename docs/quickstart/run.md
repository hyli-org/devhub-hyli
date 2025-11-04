# Run the scaffold locally

## Prerequisites

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- Install openssl-dev (e.g. `apt install openssl-dev` or `cargo add openssl`).
- [Bun](https://bun.sh/) (or npm/yarn)
- Install provers as needed:
  - [RISC Zero](https://dev.risczero.com/api/zkvm/install) (used for the scaffold example)
  - [SP1](https://docs.succinct.xyz/docs/sp1/introduction)
  - [Noir](https://noir-lang.org/docs/getting_started/quick_start)
  - [Cairo M](https://github.com/kkrt-labs/cairo-m)

## Run a node locally

Clone [the Hyli node](https://github.com/hyli-org/hyli).

Run:

```sh
git checkout v0.13.1 # 0.13.1 is the latest stable version
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
```

Variants:

- Without indexer, for a faster development loop: `HYLE_RUN_INDEXER=false cargo run`
- With SP1 verifier: `cargo run -F sp1`

Open [the Hyli explorer](https://explorer.hyli.org) and select `localhost` in the upper-right corner.

For alternative setups and custom configurations, check out [the local node reference page](../reference/local-node.md).

## Run the wallet

Clone [the Wallet repository](https://github.com/hyli-org/wallet/).

Before taking any action, wait until the node has successfully launched.

Run:

```sh
git checkout v0.1.2 # 0.1.2 is the latest stable version
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release -- -m -a -w
```

## Run the app scaffold

Clone [the Hyli app scaffold](https://github.com/hyli-org/app-scaffold/).

Start the server:

```sh
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release
```

Start the scaffold's UI:

```sh
cd front
bun install && bun run dev
```

Everything is now functional. We can now explore the scaffold!
