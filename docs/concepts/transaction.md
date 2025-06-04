# Transactions on Hyli

Hyli introduces a novel transaction model that separates intent from proof, optimizing for scalability and privacy.

Unlike [traditional blockchains](./hyli-vs-vintage-blockchains.md), where transactions are executed and proven in a single step, Hyli employs a two-step process called [pipelined proving](./pipelined-proving.md):

1. **Blob-transaction**: outlines a state change for sequencing.
2. **Proof-transaction**: provides a proof of the state change for settlement.

Each proof transaction verifies a single blob, unless you use recursion. If a blob transaction contains multiple blobs, each requires a separate proof.

Once all blobs are proven, the blob transaction is settled, and the referenced contract states are updated.

## Blob transaction structure

A **blob transaction** consists of:

- An **identity** string. See [identity](./identity.md).
- A list of **blobs**, each containing:
  - A **contract name** (string).
  - A **data** field (binary), which the contract parses.

## Proof transaction structure

A **proof transaction** includes:

- A **contract name** (string).
- **Proof data** (binary), containing:
  - A zero-knowledge proof.
  - The app output.

For Risc0 and SP1, the proof data's app output follows `HyleOutput` as defined in the [smart contract ABI](./apps.md.md).

## Example: token transfer

A token transfer involves two blobs in a blob transaction:

- **Identity blob**: Verifies the sender’s identity and authorizes the transfer.
- **Transfer blob**: Executes the token transfer.

Each blob requires a corresponding proof transaction.

### Blob transaction

```json
{
    "identity": "bob.hydentity",
    "blobs": [
        {
            "contract_name": "hydentity",
             // Binary data for the operation of hydentity contract
             // VerifyIdentity { account: "bob.hydentity", nonce: "2" }
            "data": "[...]" 
        },
        {
            "contract_name": "hyllar",
             // Binary data for the operation of hyllar contract
             // Transfer { recipient: "alice.hydentity", ammount: "20" }
            "data": "[...]"
        }
    ]
}
```

### Proof transactions

#### Identity proof

```json
{
    "contract_name": "hydentity",
    "proof": "[...]"
}
```

The binary proof's output includes:

- Initial state: `bob.hydentity` nonce = 1.
- Next state: `bob.hydentity` nonce = 2.
- Index: 0 (first blob in the transaction).

and

```json
{
    "contract_name": "hyllar",
    "proof": "[...]"
}
```

The binary proof's output includes:

- Initial state: `bob.hydentity` balance = 100, `alice.hydentity` balance = 0.
- Next state: `bob.hydentity` balance = 80, `alice.hydentity` balance = 20.
- Index: 1 (second blob in the transaction).
