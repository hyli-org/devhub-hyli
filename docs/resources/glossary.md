# Glossary

This glossary explains key concepts of the **Hyli blockchain**, a high-performance blockchain with built-in privacy.

It helps developers and curious readers understand the key terms that describe how proofs, transactions, and operations work in Hyli.

Each definition links to a related concept page for deeper technical details.

## Base state

The **base state** is the state of an app before any operation occurs.
See also: [Transactions](../concepts/transaction.md)

## Blob

A **blob** is a piece of provable information sent to Hyli for sequencing. It represents off-chain data that will later be proven and settled onchain.
See also: [Apps](../concepts/apps.md)

## Blob transaction

A transaction that includes a provable blob and is used for sequencing within Hyli’s pipeline.
See also: [Pipelined proving](../concepts/proof-composition.md)

## Operation

An **operation** is an event that happens on an app.
Each operation consists of two transactions: a blob transaction and a proof transaction.
See also: [Transactions](../concepts/transaction.md)

## Proof transaction

A **proof transaction** contains a proof of a previously-submitted blob and is used for verification and settlement.
See also: [Transactions](../concepts/transaction.md)

## Proof composition

The process of combining multiple proofs (e.g., from different proving systems) into a single proof.
This enables **interoperability** and efficient **verification** across diverse proof frameworks.
See also: [Proof composition](../concepts/proof-composition.md)

## Proof verification

The step where Hyli verifies submitted proofs before final settlement.
This guarantees correctness and ensures that only valid operations update the blockchain state.
See also: [Transactions](../concepts/transaction.md)

## Cross-contract composition

The ability to use and verify a contract’s proofs within other contracts, enabling modular and interoperable applications.
See also: [Proof composition](../concepts/proof-composition.md)

## Timeout

A **timeout** is a defined window of time after which, if no proof transaction has been submitted, an operation fails.
See also: [Pipelined proving](../concepts/proof-composition.md)

## Transaction

A **transaction** is a unit of communication sent to Hyli.
There are two kinds: blob transactions and proof transactions.
See also: [Transactions](../concepts/transaction.md)

## Scalability

Hyli achieves high throughput and low latency through off-chain execution and proof-based settlement, allowing Web2-like performance without sacrificing security.
See also: [Performance](../concepts/performance.md)

## Privacy proofs

Proofs can preserve privacy by verifying correctness without revealing the underlying data.
This enables compliance-friendly and privacy-aware applications.
See also: [Proofs overview](../concepts/proofs.md)

## Pipelined proving

A proving model that separates proof generation from application logic.
This allows proof creation to run asynchronously while maintaining deterministic outcomes.
See also: [Proof composition](../concepts/proof-composition.md)

## Zero-knowledge proof (ZKP)

A cryptographic proof that shows a statement is true without revealing the data behind it.
Hyli supports multiple proof schemes, including Groth16, RISC Zero, Noir over Barretenberg, and SP1.
See also: [Proof systems](../concepts/proofs.md)

## State settlement

The process of finalizing verified operations onchain.
This guarantees that off-chain computations are securely reflected on the blockchain.
See also: [Settlement](../concepts/settlement.md)

## Ecosystem

The network of tools, projects, and partners building with or integrating Hyli — from wallet providers to proof systems and SDKs.
See also: [Ecosystem](../resources/ecosystem.md)

## Developer quickstart

Resources for building your first app with Hyli, including SDKs, example repositories, and starter templates.
See also: [Getting started](../getting-started.md)

<script type="application/ld+json">
{
"@context": "https://schema.org",
"@type": "DefinedTermSet",
"name": "Hyli Blockchain Glossary",
"url": "https://docs.hyli.org/resources/glossary",
"hasDefinedTerm": [
{"@type": "DefinedTerm", "name": "Blob", "description": "A piece of provable information sent to Hyli for sequencing."},
{"@type": "DefinedTerm", "name": "Proof composition", "description": "Combining multiple proofs into one for interoperability and efficiency."},
{"@type": "DefinedTerm", "name": "Proof verification", "description": "The process of verifying submitted proofs before settlement on Hyli."},
{"@type": "DefinedTerm", "name": "Pipelined proving", "description": "A model separating proof generation from application logic to boost throughput."},
{"@type": "DefinedTerm", "name": "Zero-knowledge proof", "description": "A cryptographic method to prove correctness without revealing data."}
]
}
</script>
