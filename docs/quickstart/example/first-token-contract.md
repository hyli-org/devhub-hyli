# Your first token contract

!!! failure
   This documentation is not up-to-date. We'll rewrite it soon. In the meantime, we recommend using our [testnet faucet](https://github.com/hyli-org/faucet) or [testnet wallet](https://github.com/hyli-org/wallet) as examples.

This guide will walk you through creating and deploying your first token transfer contract using Hyli's tools and infrastructure. We'll use [our simple-token transfer example](https://github.com/hyli-org/examples/tree/main/simple-token) as the basis for this tutorial.

For an alternative implementation, check out [the same example built with SP1](https://github.com/hyli-org/examples/tree/main/simple-token-sp1).

If you’re new to apps on Hyli, read the [anatomy of an app](../../concepts/apps).

## Example

!!! warning
    Our examples work on Hyli v0.12.1. Later versions may introduce breaking changes that are not yet reflected in our examples.

### Prerequisites

- [Install Rust](https://www.rust-lang.org/tools/install) (you'll need `rustup` and Cargo).
- Install openssl-dev (e.g. `apt install openssl-dev` or `cargo add openssl`).
- For our example, [install RISC Zero](https://dev.risczero.com/api/zkvm/install).
- [Run a single-node devnet](../devnet.md). We recommend using [dev-mode](https://dev.risczero.com/api/generating-proofs/dev-mode) with `-e RISC0_DEV_MODE=1` for faster iterations.

## Quickstart

### Build and register the contract

To build all methods and register the app [from the source](https://github.com/hyli-org/examples/blob/main/simple-token/host/src/main.rs), navigate to your cloned Examples folder and run:

```bash
cargo run -- register 1000
```

The expected output is `📝 Registering new contract simple_token`.

### Transfer tokens

Transfer 2 tokens from the Hyli `faucet` to `Bob`:

```bash
cargo run -- transfer faucet@simple_token bob@simple_token 2
```

This command:

1. Sends a blob transaction to transfer 2 tokens from `faucet@simple_token` to `bob@simple_token`. To better understand what a blob transaction is, [read more about transactions on Hyli](../../concepts/transaction.md). 
2. Generates a ZK proof for the transfer.
3. Sends the proof to the devnet for verification. (Scroll down to see what that looks like in code.)

### Verify settled state

Upon reception of the proof, the node will:

1. Verify the proof
1. Settle the blob transaction
1. Update the contract's state

Expected log output:

```bash
INFO hyle::data_availability::node_state::verifiers: ✅ Risc0 proof verified.
INFO hyle::data_availability::node_state::verifiers: 🔎 Program outputs: Transferred 2 to bob.simple_token
```

On the following slot:

```bash
INFO hyle::data_availability::node_state: Settle tx TxHash("[..]")
```

#### Check onchain balance

Verify onchain balances:

```bash
cargo run -- balance faucet@simple_token
cargo run -- balance bob@simple_token
```

!!! note
    In this example, we do not verify the identity of the person who initiates the transaction. We use `@simple_token` as a suffix for the "from" and "to" transfer fields, but the correct identity scheme should be used in production. To better understand how Hyli manages identity, [read more about identity on Hyli](../../concepts/identity.md).

See your contract's state digest at: `https://explorer.hyli.org/contract/$CONTRACT_NAME`.

See your transaction on Hyli's explorer: `https://explorer.hyli.org/tx/$TX_HASH`.

## Development mode

For faster iteration during development, enable [dev-mode](https://dev.risczero.com/api/generating-proofs/dev-mode): `-e RISC0_DEV_MODE=1`.

To get execution statistics, set: `RUST_LOG="[executor]=info"` as an environment variable.

To run your project in development mode with logs:

```bash
RUST_LOG="[executor]=info" RISC0_DEV_MODE=1 cargo run
```

## Annotated code snippets

Find the full annotated code in [our examples repository](https://github.com/hyli-org/examples/blob/main/simple-token/host/src/main.rs).

Below are key snippets explaining contract registration, blob transactions, and proofs.

### Registering the contract

Set up information about your contract. To register the contract, you'll need:

- `owner`: we put "examples" as the `owner`, but you can put anything you like. This field is currently not leveraged; it will be in future versions.
- `verifier`: for this example, the verifier is `risc0`
- `program_id`: RISC Zero programs are identified by their image ID, without a prefix.
- `state_digest`: usually a MerkleRootHash of the contract's initial state. For this example, we use a hexadecimal representation of the state encoded in binary format. The state digest cannot be empty, even if your app is stateless.
- `contract_name` as set up above.

```rs
// Build initial state of contract
let initial_state = Token::new(supply, format!("faucet.{}", contract_name).into());
println!("Initial state: {:?}", initial_state);

// Send the transaction to register the contract
let register_tx = RegisterContractTransaction {
    owner: "examples".to_string(),
    verifier: "risc0".into(),
    program_id: sdk::ProgramId(sdk::to_u8_array(&GUEST_ID).to_vec()),
    state_digest: initial_state.as_digest(),
    contract_name: contract_name.clone().into(),
};
let res = client
    .send_tx_register_contract(&register_tx)
    .await
    .unwrap()
    .text()
    .await
    .unwrap();

println!("✅ Register contract tx sent. Tx hash: {}", res);
```

In [the explorer](https://explorer.hyli.org/), this will look like this:

```json
{
    "tx_hash": "321b7a4b2228904fc92979117e7c2aa6740648e339c97986141e53d967e08097",
    "owner": "examples",
    "verifier": "risc0",
    "program_id":"e085fa46f2e62d69897fc77f379c0ba1d252d7285f84dbcc017957567d1e812f",
    "state_digest": "fd00e876481700000001106661756365742e687964656e74697479fd00e876481700000000",
    "contract_name": "simple_token"
}
```

#### Create blob transaction

```rs
// The action to execute, that will be proved later
let action = sdk::erc20::ERC20Action::Transfer {
    recipient: to.clone(),
    amount,
};
// Into a BlobTransaction to be sent on chain
let blobs = vec![sdk::Blob {
    contract_name: contract_name.clone().into(),
    data: sdk::BlobData(
        bincode::encode_to_vec(action, bincode::config::standard())
            .expect("failed to encode BlobData"),
    ),
}];
let blob_tx = BlobTransaction {
    identity: from.into(),
    blobs,
};

// Send the blob transaction
let blob_tx_hash = client.send_tx_blob(&blob_tx).await.unwrap();
println!("✅ Blob tx sent. Tx hash: {}", blob_tx_hash);
```

#### Prove the transaction

Hyli transactions are settled in two steps, following [pipelined proving principles](../../concepts/pipelined-proving.md). After this step, your transaction is sequenced, but not settled.

For the transaction to be settled, it needs to be proven. You'll start with building the contract input, specifying:

- the initial state as set above
- the identity of the transaction initiator
- the transaction hash, which can be found in the explorer after sequencing
- information about the blobs.
  - private input for proof generation in `private_blob`
  - `blobs`: full list of blobs in the transaction (must match the blob transaction)
  - `index`: each blob of a transaction must be proven separately for now, so you need to specify the index of the blob you're proving.

```rs
// Build the contract input
let inputs = ContractInput::<Token> {
    initial_state,
    identity: from.clone().into(),
    tx_hash: "".into(),
    private_blob: sdk::BlobData(vec![]),
    blobs: blobs.clone(),
    index: sdk::BlobIndex(0),
};

// Generate the zk proof
let receipt = prove(cli.reproducible, inputs).unwrap();

let proof_tx = ProofTransaction {
    blob_tx_hash,
    proof: ProofData::Bytes(borsh::to_vec(&receipt).expect("Unable to encode receipt")),
    contract_name: contract_name.clone().into(),
};

// Send the proof transaction
let proof_tx_hash = client
    .send_tx_proof(&proof_tx)
    .await
    .unwrap()
    .text()
    .await
    .unwrap();
println!("✅ Proof tx sent. Tx hash: {}", proof_tx_hash);
```

Check the full annotated code in [our GitHub example](https://github.com/hyli-org/examples/blob/main/simple-token/host/src/main.rs).
