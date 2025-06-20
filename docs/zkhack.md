# ZKHack Berlin Quickstart

Welcome to Berlin! Want hack to on Hyli during ZKHack? You're on the best place to get started.

In few minutes, you'll be running Hyli locally and everything else you need to start hacking.

You can check our [introduction](./introduction.md) to understand better what's Hyli and what to expect.

But TLDR: Build your contract in Rust or Noir, write provable programs, on a high-TPS blockchain with [state of the art consensus](https://arxiv.org/abs/2401.10369)

!!! tip
    We are always super happy to help, either in our [Builder Group](https://t.me/hyli_builders) or IRL.

    We're here to help you debug your weird errors :D

## Prerequisites
To get started you need to install:

- [Docker](https://docs.docker.com/get-started/) Note: we need both the docker engine and docker-compose.
- [Risc Zero zkVM](https://dev.risczero.com/api/zkvm/install), if you want to use SP1, come ask us; we will help!
- [Rust](https://www.rust-lang.org/tools/install)
- [Noir](https://noir-lang.org/docs/getting_started/quick_start). 
- [Bun](https://bun.sh/) (or npm/yarn)

For Noir, we use version `nargo version = 1.0.0-beta.3` and `bb = 0.82.2`. 

You can install these versions using:
```sh
$ noirup -v 1.0.0-beta.3
$ bbup -v 0.82.2
```

## Spinning up the scaffold

To get started quickly we would advice you to use the [app-scaffold](https://github.com/Hyli-org/app-scaffold/)

```sh
$ git clone git@github.com:hyli-org/app-scaffold.git
```

It contains three parts:

- `front/` containing a basic frontend with a wallet integration; that you could get some inspo from
- `contracts/` containing the two contracts of this default app; that's what R0 will compile
- `server/` containing the autoprover logic: each time a tx is sent on the front, the server sends the tx onchain and prove this transaction to make it settled

!!! information
    It's not hard to send the transaction from the frontend instead of sending it via an API, come see us for help

### Starting the scaffold

First run the docker-compose:
```sh
$ cd app-scaffold/
$ docker-compose up -d
```
It might take some time to download/build, but you'll have a Hyli node alongside a wallet server running!

Then you can run the UI of the scaffold:
```sh
$ cd front/
$ bun install
$ bun run dev
```

And for the server, we need to pass an env variable for the dev mode of risc0
```sh
$ RISC0_DEV_MODE=true cargo run -p server
```

!!! information
    When you are making some changes and want to clean the state, you have to remove the `data/` folder

    You can use the command `rm -rf data && RISC0_DEV_MODE=true cargo run -p server` to avoid getting stuck

## Making changes to the contracts

You can modify the in `contracts/` and have fun hacking whatever you have in mind.

Whenever you change the contract, you might see an error about a program id missmatch in the server.

To avoid this, you have to restart the node

```sh
$ rm -rf ./data # we have to clean the state of the server as well
$ docker-compose down --volumes --remove-orphan
$ docker-compose up -d
```

### Writing a Noir contract
In our current infrastructure, you can write your smart contracts using Noir, but so far, we only used Noir for state-less contracts

Because Noir is a circuit-based zk-language (compared to risc0 or sp1 VM architecture); you have to define the maximum size of the state at the contract creation. It's not really practical for us, so what we usually do is having two contracts: 

- A stateless contract in Noir for the sensitive informations/private inputs and the logic linked to it.
- A stateful in Rust using risc0/sp1 to store the private state onchain

For example, in the wallet, we use a Noir contract called `check_secret` whose purpose is to take some private input, hash it with a specified salt and output an hash.

This hash will then be used in the risc0 contract, using [proof composition](https://docs.hyli.org/concepts/proof-composition/). We leverage the privacy of Noir and ease of use of zkVMs!

If you want to write your entire contract in Noir, it's totally possible and we would be happy to help you!

### Examples

For examples of contracts, you can look:

- [Wallet](https://github.com/hyli-org/wallet) Our wallet implementation using Noir
- [ezcasino](https://github.com/hyli-org/ezcasino) A blackjack game

For more complex examples, come see us!

## Frontend 
In the current setup of the scaffold, every tx passes through a backend; but you need to pass some `wallet_blobs` to information

To do so, you have to use the `hyli-wallet` package

```sh
$ bun install hyli-wallet
```

And then use the `WalletProvider` in your app, to expose an instance of the wallet

```typescript
function App() {
    return (
        <WalletProvider
            config={{
                // These env variable are already exposed in the scaffold.
                nodeBaseUrl: import.meta.env.VITE_NODE_BASE_URL, 
                walletServerBaseUrl: import.meta.env.VITE_WALLET_SERVER_BASE_URL,
                applicationWsUrl: import.meta.env.VITE_WALLET_WS_URL,
            }}
            sessionKeyConfig={{
                duration: 24 * 60 * 60 * 1000, // Session key duration in ms (default: 72h)
                whitelist: ["contract1", "contract2"], // Required: contracts allowed for session key; Make sure your contract is here!
            }}
            forceSessionKeyCreation={true} 
        >
            <AppContent />
        </WalletProvider>
    )
}
```

You can call `useWallet()` hook to get what you need! 

```typescript
const { logout, wallet, createIdentityBlobs } = useWallet();
```

The `wallet` variable will be empty if the user isn't connected.

To create the `wallet_blobs` to pass to the server, you should use `createIdentityBlobs`.
```typescript
const [blob0, blob1] = createIdentityBlobs(); // Pass these blobs to the request body
```

You can then make the call to the backend using the right headers (don't forget to pass `wallet.address`) as `x-user` 

```typescript
const headers = new Headers();

headers.append('content-type', 'application/json');
headers.append('x-user', wallet.address);
headers.append('x-session-key', 'test-session');
headers.append('x-request-signature', 'test-signature');

const response = await fetch(`${import.meta.env.VITE_SERVER_BASE_URL}/api/increment`, {
    method: 'POST',
    headers: headers,
    body: JSON.stringify({
      wallet_blobs: [blob0, blob1]
    })
});
```

By default the headers are not used (like the session key or signature).

But you can access them in the server!

## Server
The `server/src/app.rs` is pretty straightforward to read to understand how to add new routes for your contract.

If you need another example, you can check [ezcasino's server](https://github.com/hyli-org/ezcasino/tree/main/server) to have a view of an actual app.


## Explorer
You can use the official Hyli [explorer](https://explorer.hyli.org/) and change the network to localhost to have access to local state of the chain.



