# Proof composition

## The problem: clunky interactions

In zero-knowledge systems, coordinating multiple proofs is complex. Cross-contract calls often rely on recursive verification, where Program A verifies a proof of the correct execution of Program B. This is inefficient and creates overhead at the proof generation and verification stages.

The situation worsens when different proving schemes are involved. Most zero-knowledge systems force you to write your proofs in a unified scheme. By doing that, you lose all the advantages of specialization.

Different proving schemes answer different needs. [Some proof systems are best](./proof-generation.md) for client-side proving; others allow developers to use general-purpose programming languages.

## The solution: proof composition

Hyli introduces **native proof composition**, allowing proofs to interact while remaining independent. Each proof can use the most suitable language and proving scheme, all within a single transaction.

You benefit from composition when your transaction:

- Involves multiple applications
- Includes proofs in different languages or proving schemes
- Combines heterogeneous logic where different tools are ideal

If everything fits cleanly within one proof, there is no composition.

### Cross-contract calls with proof composition

Instead of recursion, Hyli lets a program declare: "This only applies if all referenced blobs are valid." During settlement, all proofs are included in one transaction. Hyli verifies them together. If any proof fails, the entire transaction fails.

This model:

- removes the need for embedded recursion;
- improves developer experience;
- reduces gas costs;
- enables parallel proving.

### Mixing proof schemes

Since proofs in Hyli remain independent, each proof in a composed transaction can use its own proving scheme. Proofs are verified separately, eliminating the need to compromise for compatibility. This also enables cross-contract calls between applications using different proof systems.

You can:

- batch heterogeneous proofs;
- call between contracts using different systems;
- choose the best proving scheme for each proof.

## How Hyli settles multiple proofs

![A ticket purchase process with four key steps. First, a user requests a ticket through the TicketApp, which in turn requests a transaction blob from MoneyApp. The blob includes the transfer details and is sent back to TicketApp for verification. Second, the TicketApp composes a transaction by combining its own blob and the MoneyApp blob, detailing the operation's validity. Third, the composed transaction is sent to Hyli for verification, where the state transition and assertions are confirmed. Finally, after verification, the user pays $10 and receives their ticket.](../assets/img/proof-composition-flow.jpg)

When a transaction includes multiple proofs, Hyli begins verifying each proof as soon as it's ready. If one fails or times out, the entire transaction is rejected.

Proof generation is parallelized as all proofs are independent. Verification is asynchronous thanks to [pipelined proving](./pipelined-proving.md).

## Writing a cross-contract call

Your program doesn't need to verify another program’s execution directly. Instead, your contract declares claims about other apps:

```md
MoneyApp::transfer(10, A, B) == true
TicketApp::get(A) == ticket
```

![An example of a blob with two transactions. For App A, the function called is get for a parameter A, and leads to a ticket. For App B, the function is a transfer of 10 from A to B and has the result true.](../assets/img/proof-composition-blob.jpg)

Each claim consists of:

- The application (MoneyApp, TicketApp)
- The function (transfer, get)
- Parameters
- A result assertion (== true, == ticket)

See [the source code from our example](https://github.com/hyli-org/examples/blob/492501ebe6caad8a0fbe3f286f0f51f0ddca537c/ticket-app/contract/src/lib.rs#L44-L66).

## Delegating identity

Each transaction in Hyli is signed by a single identity blob. By default, this identity authorizes all blobs in the transaction.

For cross-contract composition, Hyli supports **callees**, blobs that run under a different identity.

This lets contracts trigger delegated actions without needing nested calls or recursion.

Consider an AMM:

1. Alice signs and submits a swap
1. The AMM blob executes `swap()`
1. `swap()` lists `transfer()` and `transferFrom()` as callees
1. These callees run as if the AMM signed them
 
![Diagram showing a composed transaction in Hyli for an AMM swap. BlobIdentity verifies the user's identity. Blob1: The user approves the AMM to spend 5 TokenX (approve call to the TokenX contract). Blob2: The AMM initiates a swap for the X/Y pair, listing Blob3 and Blob4 as callees. It checks that the swap is valid and callees are correctly structured. Blob3: The AMM transfers 0.02 TokenY to the user via the TokenY contract. It verifies that the caller matches the from field. Blob4: The AMM calls transferFrom to pull 5 TokenX from the user. It checks that the caller is allowed to move the tokens or has an approval.Each blob runs independently but within a shared transaction structure, with the AMM blob delegating authority to callees.](../assets/img/amm.jpg)

### Caller

The **caller** is the identity under which a blob executes. By default, it’s the transaction signer (e.g., `Alice.hydentity`).

If a blob is a callee, its caller becomes the blob that declared it.

## Callee

A **callee** is a blob that runs on behalf of another blob. This lets contracts perform delegated actions without initiating their own transactions.

For example:

- Alice signs and submits a transaction that includes a blob for an AMM swap
- The AMM swap blob declares two callees: `transfer` and `transferFrom`
- These execute with the AMM as the caller

## Execution and validation

Each callee:

- Verifies that the caller blob explicitly listed it as a callee
- Checks authorization logic (e.g., `transferFrom` checks for prior `approve`)

This approach lets users delegate logic to contracts without nested transactions, maintaining clarity and flatness in Hyli’s execution model.
