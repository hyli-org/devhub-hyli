# :checkered_flag: Quickstart

Welcome to the Hyli **Quickstart** guide.

In this guide, you’ll set up your local environment, run a node, launch the wallet, start the scaffold app, and begin building.

🧭 Estimated time: 15-30 minutes  
🧰 Requirements: basic command-line and Rust experience

## Prerequisites

Make sure you have the following installed:

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- Install libssl-dev
- [Bun](https://bun.sh/) (or npm/yarn)
- Install provers as needed (see [supported proving schemes](../reference/supported-proving-schemes.md))

## Run a local node

Clone [the Hyli node](https://github.com/hyli-org/hyli).

Run:

```sh
git checkout v0.13.1 # latest stable version
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
```

Variants:

- Without indexer (faster development loop): `HYLE_RUN_INDEXER=false cargo run`
- With SP1 verifier: `cargo run -F sp1`

Once it's running, open [the Hyli explorer](https://explorer.hyli.org) and select `localhost` in the upper-right corner.

For alternative setups and custom configurations, check out [the local node reference page](../reference/local-node.md).

## Run the wallet

Before taking any action, wait until the node has successfully launched.

Clone [the Wallet repository](https://github.com/hyli-org/wallet/) and run it:

```sh
git checkout v0.1.2 # 0.1.2 is the latest stable version
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release -- -m -a -w
```

The wallet aggregates identities and allows local authentication and signing during development.

Learn more: [Wallet documentation](../tooling/wallet.md)

## Run the app scaffold

Clone [the Hyli app scaffold](https://github.com/hyli-org/app-scaffold/) and start both the backend and frontend.

Start the backend:

```sh
rm -rf data 2>/dev/null || true && clear && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run --bin server --release
```

Start the frontend:

```sh
cd front
bun install && bun run dev
```

Everything is now live locally!

## Build your own app

### Restarting the infra when you edit a contract

When you edit a contract, you need to restart the node, the wallet, and the scaffold by executing this command in all three:

```sh
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
```

If you do not do this, you will see an error about a program id mismatch in the server.

### What to edit

The scaffold includes three components:

| Folder       | Purpose                                     |
| ------------ | ------------------------------------------- |
| `front/`     | Basic frontend with wallet integration      |
| `contracts/` | Example RISC Zero contracts                 |
| `server/`    | Autoprover and transaction submission logic |

To customize their content:

- Edit or add contracts in `contracts/`, referring to our [supported proving schemes](../reference/supported-proving-schemes.md)
- Update `front/`
- Adapt server routes

## Next steps

You now have a complete local setup for your Hyli application. You can now:

- Dive into [concepts](../concepts/index.md) as needed
- Explore [API and SDK](../tooling/api.md) for integrations
- Join the [Hyli community](https://t.me/hyli_org) to share what you build

## Troubleshooting

### Program ID mismatch

You likely modified a contract but didn’t restart all components. See [the restart instructions](#restarting-the-infra-when-you-edit-a-contract).

### Ports already in use

Stop any previous node/wallet/scaffold processes or change the ports in their configs.

### Proof timeout

Check that your chosen prover is running correctly.
