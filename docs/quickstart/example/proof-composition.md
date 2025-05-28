# Proof composition with Ticket App

Hyli enables [proof composition](../../concepts/proof-composition.md), allowing different proving systems to work within a single operation. This removes constraints on provers and significantly improves interoperability and efficiency.

In this guide, we’ll build a Ticket App that demonstrates proof composition. Users can buy a ticket using [simple-token](./first-token-contract.md), and Hyli will verify multiple proofs in a single transaction.

Find the source code for all contracts here:

- [ticket-app](https://github.com/hyli-org/examples/tree/feat/ticket-app/ticket-app)
- [simple-identity](https://github.com/hyli-org/examples/tree/main/simple-identity)
- [simple-token](https://github.com/hyli-org/examples/tree/feat/ticket-app/simple-token)

Traditional verification systems often require all proofs to be generated using the same proving system. Hyli removes this limitation, allowing:

- Interoperability: use different proof systems within a single transaction.
- Atomicity: either all proofs verify, or none do. This ensures fail-safe execution.
- Efficiency: parallel processing of proofs without requiring a single proving standard.

## How this example works

In this example, `Alice` and `Bob` both want to buy a ticket from Ticket App for 15 `simple-tokens`. Bob has enough tokens to complete the transaction, while Alice does not.

### Step 1: Create the blob transaction

The Ticket App backend creates and sends a blob transaction to Hyli, including three blobs:

- an *identity blob* (see our [custom identity contract quickstart](./custom-identity-contract.md)) confirming that Bob (`bob.id`) is initiating the transaction;
- a *simple-token blob* transferring 15 simple-tokens from `bob.id`'s balance;
- a *ticket-app blob* sending `bob.id` a ticket if conditions are met.

At this stage, Hyli sequences the transaction, but it’s not yet settled. [Read more about pipelined proving.](../../concepts/pipelined-proving.md)

### Step 2: Prove the blobs

The Ticket App backend generates ZK proofs of each blob.

The `ticket-app` contract is executed, checking whether:

- A `simple-token` blob exists;
- The blob attempts to transfer 15 simple-tokens from `bob.id` to `ticket-app`.

At this step, the contract does not verify whether Bob actually has enough tokens: if the token transfer fails in Step 3, the entire transaction fails.

Check out the [source code](https://github.com/hyli-org/examples/blob/492501ebe6caad8a0fbe3f286f0f51f0ddca537c/ticket-app/contract/src/lib.rs#L44-L66).

### Step 3: Settlement

Once TicketApp has sent the proofs for the previously sequenced blobs, Hyli verifies:

- identity proof: verifies that `bob.id` has initiated the transaction.
- simple-token proof: verifies that `bob.id` paid the correct amount for his ticket.
- ticket-app proof: verifies that `bob.id` has received the ticket.

If all proofs are valid, the simple-token balance and ticket-app ticket balance are updated simultaneously at transaction settlement: `bob.id` sends 15 simple-tokens and gains one ticket.

If any proof fails, the entire transaction fails. Neither state is updated: Bob's token balance does not change and he doesn't get a ticket.

## Run the example

!!! warning
    Our examples work on Hyli v0.12.1. Later versions introduce breaking changes which have not yet been reflected in our examples.

### Prerequisites

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- For our example, [install RISC Zero](https://dev.risczero.com/api/zkvm/install).
- [Start a single-node devnet](../devnet.md).

This quickstart guide will take you through the following steps:

- [Simple-identity preparation](#simple-identity-preparation): register an identity contract & two identities.
- [Simple-token preparation](#simple-token-preparation): register a token contract and faucet the users with this token.
- [Register the ticket-app contract](#register-ticket-app)
- [Buy a ticket on ticket-app](#buy-a-ticket)

### Simple-identity preparation

Let's start with registering an identity contract and two identities.

Go to the `./simple-identity` folder and run:

```sh
cargo run -- --contract-name id register-contract
```

Now we have an identity contract called `id`. We can use it to declare our users:

```sh
cargo run -- --contract-name id register-identity bob.id pass
cargo run -- --contract-name id register-identity alice.id pass
```

Let's verify it quickly with:

```sh
cargo run -- --contract-name id verify bob.id pass 0
```

0 is a nonce: every time we verify successfully bob's identity, it increments. Now if we want to verify it again, we should use 1 as nonce. (We also use "pass" as our default password.)

We now do the same for alice:

```sh
cargo run -- --contract-name id verify alice.id pass 0
```

`bob.id` is bob's identity on the simple-identity contract. Check out our [Identity management](../../concepts/identity.md) and [custom identity contract](./custom-identity-contract.md) pages to know more.

### Simple-token preparation

#### Register simple-token

Go to `./simple-token` folder and run:

```bash
cargo run -- --contract-name simple-token register 1000
```

On the node's logs, you will see:

> 📝 Registering new contract simple_token

You just registered a token contract named simple-token with an initial supply of 1000.

#### Transfer tokens

Now let's transfer some tokens to our user `bob`.

To send 50 tokens to `bob` and 10 tokens to `alice`, run:

```bash
cargo run -- -contract-name simple-token transfer faucet.simple-token bob.id 50
cargo run -- -contract-name simple-token transfer faucet.simple-token alice.id 10
```

The node's log will show:

> INFO hyle::data_availability::node_state::verifiers: ✅ Risc0 proof verified.
>
> INFO hyle::data_availability::node_state::verifiers: 🔎 Program outputs: Transferred 50 to bob.ticket_app
> INFO hyle::data_availability::node_state::verifiers: 🔎 Program outputs: Transferred 10 to alice.ticket_app

#### Check onchain balance

Check onchain balance:

```bash
cargo run -- --contract-name simple-token balance faucet.simple-token

cargo run -- --contract-name simple-token balance bob.id
cargo run -- --contract-name simple-token balance alice.id
```

You should see that `bob` has a balance of 50 and `alice` has a balance of 10.

### Using ticket-app

Now that `bob` has some tokens, let's buy him a ticket.

#### Register ticket-app

Register the ticket app by going to `./ticket-app` folder and running:

```bash
cargo run -- --contract-name ticket-app register simple-token 15
```

ticket-app sells bob a ticket for 15 simple-token.

#### Buy a ticket

Let's buy a ticket for `bob`:

```bash
cargo run -- --contract-name ticket-app --user bob.id buy-ticket
```

Let's try with `alice`:

```bash
cargo run -- --contract-name ticket-app --user alice.id buy-ticket
```

You will get an error while executing the TicketApp program: `Execution failed ! Program output: Insufficient balance`. This is because Alice has a balance of 10 and the ticket costs 15.

#### Check ticket and token balance

Check that `bob` has a ticket:

```bash
cargo run -- --contract-name ticket-app --user bob.id has-ticket
```

You can also check `bob`'s balance and see he now has 35 tokens.

With proof composition, Hyli empowers you to leverage multiple proving systems in a single transaction, making advanced functionality like the Ticket App easier than ever.
