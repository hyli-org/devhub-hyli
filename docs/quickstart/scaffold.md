# Set up your application

## IMPORTANT: Editing a contract

If you make changes to the contracts, you need to execute this command in the node, the wallet, and the scaffold to restart them:

```sh
rm -rf data_node && RISC0_DEV_MODE=true SP1_PROVER=mock cargo run -- --pg
```

If you do not do this, you will see an error about a program id mismatch in the server.

## How the scaffold is built

The scaffold comes with a built-in autoprover and server implementation, so you can focus on your contracts and frontend.

The scaffold includes the following folders:

- `front/`, a basic frontend with a wallet integration;
- `contracts/`, two Risc0 contracts for a default app;
- `server/`, the [autoprover logic](../concepts/proof-generation.md).

The application follows a client-server model:

- The frontend sends operation requests to the server.
- The server handles transaction creation, proving, and submission. 
- All interactions go through the Hyli network.

## Add your contracts

Place your `.rs` app files in the `contracts/` directory.

For examples of contracts, you can look at examples from [our supported proving schemes](../reference/supported-proving-schemes.md).

## Add your frontend

Put your frontend code in the `front/` directory. By default, we've implemented the [Hyli wallet](../concepts/identity.md).

Make sure the frontend connects to the backend at the expected route (`/prove`, `/submit`, etc.), or adapt accordingly.

## Start the server

In the root of the scaffold, start the backend server:

```sh
RISC0_DEV_MODE=1 cargo run -p server
```

This starts the backend service, which handles contract interactions and proofs.

## Open the frontend interface

From the `front/` directory, install dependencies and run the dev server:

```sh
cd front
bun install
bun run dev
```

This starts the local frontend interface to interact with the Hyli network.
