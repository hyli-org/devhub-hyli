# Using a custom identity contract

!!! failure
    This documentation is not up-to-date. We'll rewrite it soon. In the meantime, we recommend using our [testnet faucet](https://github.com/hyli-org/faucet) or [testnet wallet](https://github.com/hyli-org/wallet) as examples.

## When to use identity contracts on Hyli

On Hyli, **any app can be a proof of identity**. This flexibility enables you to register your preferred identity source as an app for account identification. If you don't want to use a custom identity source, Hyli ships [a native `hydentity` contract](https://github.com/hyli-org/hyli/tree/main/crates/contracts/hydentity).

This guide walks you through creating and deploying your first simple identity contract using Hyli and RISC Zero. We'll use [our simple identity example](https://github.com/hyli-org/examples/tree/main/simple-identity), which mirrors our [simple token transfer example](./first-token-contract.md).

If you’re new to identity management on Hyli, read the [identity management concept page](../../concepts/identity.md).

## Run the example

!!! warning
    Our examples work on Hyli v0.12.1. Later versions may introduce breaking changes which have not yet been reflected in our examples.

!!! info
    Jump to [Code Snippets](#code-snippets) for an in-depth look at the contract.

### Prerequisites

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- Install openssl-dev (e.g. `apt install openssl-dev` or `cargo add openssl`).
- For our example, [install RISC Zero](https://dev.risczero.com/api/zkvm/install).
- [Start a single-node devnet](../devnet.md). We recommend using [dev-mode](https://dev.risczero.com/api/generating-proofs/dev-mode) with `-e RISC0_DEV_MODE=1` for faster iterations during development.

### Build and register the identity contract

To build all methods and register the app on the local node [from the source](https://github.com/hyli-org/examples/blob/main/simple-identity/host/src/main.rs), navigate to your cloned Examples folder and run:

```bash
cargo run -- register-contract
```

The expected output is `📝 Registering new contract simple_identity`.

### Register an account (sign up)

To register an account with a username (`alice`) and password (`abc123`), execute:

```sh
cargo run -- register-identity alice.simple_identity abc123
```

Expected log output:

```bash
INFO hyle::data_availability::node_state::verifiers: ✅ Risc0 proof verified.
INFO hyle::data_availability::node_state::verifiers: 🔎 Program outputs: Successfully registered identity for account: alice.simple_identity
```

### Verify identity / Login

To verify `alice`'s identity:

```bash
cargo run -- verify-identity alice.simple_identity abc123 0
```

This command:

1. Sends a blob transaction to verify `alice`'s identity.
1. Generates a ZK proof of that identity. It will only be valid once, thus the inclusion of a nonce.
1. Sends the proof to the devnet.

Upon reception of the proof, the node will:

1. Verify the proof.
1. Settle the blob transaction.
1. Update the contract's state.

Expected log output:

```bash
INFO hyle::data_availability::node_state::verifiers: ✅ Risc0 proof verified.
INFO hyle::data_availability::node_state::verifiers: 🔎 Program outputs: Identity verified for account: alice.simple_identity
```

## Development mode

We recommend activating [dev-mode](https://dev.risczero.com/api/generating-proofs/dev-mode) during your early development phase for faster iteration upon code changes with `-e RISC0_DEV_MODE=1`.

You may also want to get insights into the execution statistics of your project: add the environment variable `RUST_LOG="[executor]=info"` before running your project.

The full command to run your project in development mode while getting execution statistics is:

```bash
RUST_LOG="[executor]=info" RISC0_DEV_MODE=1 cargo run
```

## Code snippets

Find the full annotated code in [our examples repository](https://github.com/hyli-org/examples/blob/main/simple-identity/host/src/main.rs).

### Registering the contract

This part is the same as for [Your first app](./first-token-contract.md).

### Register an identity

#### Build the blob transaction

This is a simplified code snippet:

```rs
let action = RegisterIdentity {
    account: identity,
};
let blob = sdk::Blob {
    contract_name: contract_name,
    data: action.as_blob_data(), // Note: This function does not exist. Used here for clarity
};
let blob_tx = BlobTransaction {
    identity: identity,
    blobs: vec![blob],
};
```

You can compare these to the fields described in the [Transactions on Hyli](../../concepts/transaction.md) page.

#### Prove the registration

##### On the backend (host) side

Hyli transactions follow a two-step settlement process based on [pipelined proving](../../concepts/pipelined-proving.md) principles:

1. Sending the blob transaction: The transaction is sequenced but not yet settled.
1. Proving the transaction: The transaction is settled after a proof is generated.

To generate the proof, we build the contract input, specifying:

- initial state
- identity of the transaction initiator
- transaction hash (found in the explorer after sequencing)
- blob information
  - The password as a private input for proof generation in `private_blob`
  - `blobs`: full list of blobs in the transaction (must match the blob transaction)
  - `index`: each blob must be proven separately, so they need an index

```rs
// Build the contract input
let inputs = ContractInput {
    initial_state: initial_state.as_digest(),
    identity: blob_tx.identity,
    tx_hash: blob_tx_hash,
    private_blob: sdk::BlobData(password),
    blobs: blobs,
    index: sdk::BlobIndex(0),
};

```

##### On the contract (guest) side

These inputs are then processed by the [SDK](../../tooling/sdk.md) to [initialize the contract](https://github.com/hyli-org/examples/blob/main/simple-identity/methods/guest/src/main.rs#L8) :

```rust
    // Parse contract inputs
    let (input, action) = sdk::guest::init_raw::<IdentityAction>();
```

- The **input** variable is the **ContractInput** defined earlier
- The **action** contains the `let action RegisterIdentity { account: identity };` from the blob.

The password is extracted as a private input:

```rust
    // Extract private information
    let password = from_utf8(&input.private_blob.0).unwrap();
```

The contract processes the identity registration:

```rust
    // We clone the initial state to be updated
    let mut next_state: Identity = input.initial_state.clone();

    // Execute the given action
    let res = sdk::identity_provider::execute_action(&mut next_state, action, password);
```

Finally, the contract commits the updated state:

```rust
    sdk::guest::commit(input, next_state, res);
```

The `guest::commit` function includes the `HyleOutput`, as explained in [Transactions on Hyli](../../concepts/transaction.md).

### Verify an identity

The identity verification process is identical to identity registration, with a different action:

```rs

let action = VerifyIdentity {
    account: identity,
    nonce,
};
```

Check the full annotated code in [our GitHub example](https://github.com/hyli-org/examples/blob/main/simple-identity/host/src/main.rs).
