# Transactions on Hyli

Hyli introduces a novel transaction model that separates intent from proof, optimizing for scalability and privacy.

Unlike [traditional blockchains](./hyli-vs-vintage-blockchains.md), where transactions are executed and proven in a single step, Hyli employs a two-step process called [pipelined proving](./pipelined-proving.md):

1. **Blob-transaction**: outlines a state change for sequencing.
2. **Proof-transaction**: provides a proof of the state change for settlement.

Each proof transaction verifies a single blob, unless you use recursion. If a blob transaction contains multiple blobs, each requires a separate proof.

Once all blobs are proven, the blob transaction is settled, and the referenced contract states are updated.

## Blob transaction structure

Blobs are not your proof: they're your proof's public wrapper. Their job is to describe what should happen (e.g. a token transfer, a vote cast) and provide enough information for the blockchain to verify the proof.

A **blob transaction** consists of:

- An **identity** string. See [identity](./identity.md).
- A list of **blobs**, each containing:
    - A **contract name** (string).
    - A **data** field (binary), which the contract parses.

A blob should clearly encode the expected onchain effects (e.g. the token amount and destination, in the case of a token transfer). It should not duplicate all proof inputs just to "explain" what the proof does.

Blobs should never include private inputs. If there is secret data, include hashes or commitments to secret data but do not include it raw.

!!! example
    In a proof that checks whether a password is correct:

    - ✅ Blob: {"username": "bob", "password_hash": "0xabc123"}
    - ❌ Blob: {"username": "bob", "password": "hunter2"}

## Proof transaction structure

A **proof transaction** includes:

- A **contract name** (string).
- **Proof data** (binary), containing:
    - A zero-knowledge proof.
    - The app output.

For Risc0 and SP1, the proof data's app output follows `HyleOutput` as defined in the [smart contract ABI](./apps.md#smart-contract-abi).
