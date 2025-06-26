# Apps on Hyli

Hyli is a fully programmable blockchain designed for efficient data storage. Hyli only keeps the essential information needed to validate app proofs, ensuring that apps are lightweight and performant.

[Unlike traditional blockchains](./hyli-vs-vintage-blockchains.md), which store all app data onchain, Hyli separates execution data (managed offchain by each application) from onchain data, which can be retrieved via our ABI.

## Onchain app data

Hyli apps store the following data onchain:

- **Name**: the unique identifier for the contract
- **Verifier**: the proof system used (e.g., "risc0")
- **Program ID**: the unique identifier for the program within that proof system
- **State digest**: current state commitment of the contract

### Explanation of the contract fields

#### Name

The contract name must be unique.

#### Verifier and program ID

Apps on Hyli rely on a pairing of a zero-knowledge proof scheme (verifier) and a program identifier. Together, these components validate contract proofs.

See the verifier scheme for each supported proving scheme in [this reference page](../reference/supported-proving-schemes.md).

#### State digest

The state digest represents the contract's current state commitment. It allows Hyli to guarantee its integrity. It can take any form as long as it fulfills this purpose.

The state digest can be expressed as a hash or even a serialization of the state if it's small enough.

## Smart contract ABI

All inputs in Hyli apps must be known at proof generation time. This includes elements like the `tx_hash` which are typically available only at execution time on other blockchains.

Here is the Rust structure specifying the output of a Hyli app:

```rust
pub struct HyleOutput {
    pub version: u32,
    pub initial_state: StateDigest,
    pub next_state: StateDigest,
    pub identity: Identity,
    pub index: BlobIndex,
    pub blobs: Vec<u8>,
    pub tx_hash: TxHash,
    pub success: bool,
    pub tx_ctx: Option<TxContext>,
    pub registered_contracts: Vec<RegisterContractEffect>,
    pub program_outputs: Vec<u8>, 
}
```

### Version

For now, `version` should always be set to 1.

### Initial state and next state

These fields define state transitions.

- `initial_state`: must match the onchain `state_digest` before the transaction. If they don't match, the state transition is invalid.
- `next_state`: Represents the new onchain `state_digest` after the transaction.

In the future, `state_digest` size will be limited and fees will depend on proof size (which is affected by the digest's size). Keep them small!

### Identity

!!! info
    Read [our identity documentation](./identity.md) for details.

Identity consists of:

1. An identifier;
1. The name of the contract that the proof was generated for.

!!! example
    A contract verifying Ethereum EOAs might have an identity like `0x1234...5678@eth_eoa` where the first part matches a regular Ethereum address and `eth_eoa` is the name of the contract.

For simplicity, we recommend using the pre-built [Hyli wallet](../tooling/wallet.md).

### Blob index and blobs

Each blob transaction includes multiple blobs:

- `index` uniquely identifies a blob within a transaction.
- `blobs` is a list of all blobs included in the transaction.

### TX Hash

`tx_hash` is the blob Transaction's hash.

The protocol does not validate this field and `tx_hash` may be deprecated in later versions.

### Success

This boolean field indicates whether the proof is for a successful or failed transactions. It can be used to prove that a transaction is invalid.

If a proof returns `success = false`, the whole blob transaction will fail. In that case, there is no need to generate proofs for other blobs.

### Transaction context

Transaction context allows the contract to know in which block (hash, height, timestamp) the blob transaction has been sequenced.

This field is optional. If left empty, it will not be validated by Hyli or usable by the program.

### Registered contracts

A list of new contracts to register, which can be used to self-upgrade a contract.

### Other program-specific outputs

Apps can generate additional outputs as proof data. These outputs ensure data availability.

## Events

Hyli does not use traditional events. Instead, it relies on blobs, which serve as containers for offchain data.
