# Set up your backend

Use the [Hyli scaffold repository](https://github.com/hyli-org/app-scaffold) to set up your backend service. It comes with a built-in autoprover and server implementation, so you can focus on your contracts and frontend.

The application follows a client-server model:

- The frontend sends operation requests to the server.
- The server handles transaction creation, proving, and submission. 
- All interactions go through the Hyli network.

For a deeper look at the backend's role, see [proof generation](../concepts/proof-generation.md).

## Clone the scaffold

Clone [Hyli scaffold repository](https://github.com/hyli-org/app-scaffold).

From here, you’ll add your contracts and frontend.

## Add your contracts

Place your `.rs` app files in the `contracts/` directory.

If you haven't written a contract yet, follow [how to write your first app](./your-first-app.md).

## Add your frontend

Put your frontend code in the `front/` directory.

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
