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
See also: [Pipelined proving](../concepts/pipelined-proving.md)

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
See also: [Pipelined proving](../concepts/pipelined-proving.md)

## Transaction

A **transaction** is a unit of communication sent to Hyli.
There are two kinds: blob transactions and proof transactions.
See also: [Transactions](../concepts/transaction.md)

## Scalability

Hyli achieves high throughput and low latency through off-chain execution and proof-based settlement, allowing Web2-like performance without sacrificing security.
See also: [Consensus](../concepts/consensus.md)

## Privacy proofs

Proofs can preserve privacy by verifying correctness without revealing the underlying data.
This enables compliance-friendly and privacy-aware applications.
See also: [Proof generation](../concepts/proof-generation.md)

## Pipelined proving

A proving model that separates proof generation from application logic.
This allows proof creation to run asynchronously while maintaining deterministic outcomes.
See also: [Proof composition](../concepts/proof-composition.md)

## Zero-knowledge proof (ZKP)

A cryptographic proof that shows a statement is true without revealing the data behind it.
Hyli supports multiple proof schemes, including Groth16, RISC Zero, Noir over Barretenberg, and SP1.
See also: [Proof generation](../concepts/proof-generation.md)

## State settlement

The process of finalizing verified operations onchain.
This guarantees that off-chain computations are securely reflected on the blockchain.
See also: [Transactions](../concepts/transaction.md)

## Ecosystem

The network of tools, projects, and partners building with or integrating Hyli — from wallet providers to proof systems and SDKs.
See also: [Find us](../resources/find-us.md)

## Developer quickstart

Resources for building your first app with Hyli, including SDKs, example repositories, and starter templates.
See also: [Quickstart](../quickstart/index.md)

## Financial institution

A regulated entity such as a bank, payment service provider, asset manager, or licensed e-money institution that operates under financial regulatory frameworks. Hyli enables financial institutions to build private, compliant stablecoins and tokenized assets.
See also: [Partnerships](../resources/partnerships.md)

## MiCA compliance

Markets in Crypto-Assets Regulation (MiCA) is the EU regulatory framework for digital assets, including stablecoins. Hyli's architecture enables MiCA compliance through selective disclosure and cryptographic privacy guarantees.
See also: [Identity for regulated finance](../concepts/identity.md)

## Private stablecoin

A digital currency pegged to a fiat currency (e.g., EUR, GBP) that provides transaction-level privacy while maintaining regulatory compliance through selective disclosure. Hyli enables licensed issuers to deploy private stablecoins with cryptographic guarantees.
See also: [Financial applications](../concepts/apps.md)

## Selective disclosure

A privacy model where different stakeholders see different levels of information. On Hyli, transaction parties see full details, the public sees only cryptographic proofs, and regulators can access complete information on request without public exposure. This solves the privacy-compliance dilemma.
See also: [Hyli vs. traditional blockchains](../concepts/hyli-vs-vintage-blockchains.md)

## Tokenized asset

Real-world assets (such as private credit, securities, or real estate) represented as digital tokens on the blockchain. Hyli enables private tokenization with selective disclosure for institutional investors and regulators.
See also: [Financial applications](../concepts/apps.md)

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
{"@type": "DefinedTerm", "name": "Zero-knowledge proof", "description": "A cryptographic method to prove correctness without revealing data."},
{"@type": "DefinedTerm", "name": "Financial institution", "description": "A regulated entity such as a bank or payment service provider."},
{"@type": "DefinedTerm", "name": "MiCA compliance", "description": "EU regulatory framework for digital assets."},
{"@type": "DefinedTerm", "name": "Private stablecoin", "description": "A fiat-pegged digital currency with transaction-level privacy and regulatory compliance."},
{"@type": "DefinedTerm", "name": "Selective disclosure", "description": "Privacy model where different stakeholders see different information levels based on their role."},
{"@type": "DefinedTerm", "name": "Tokenized asset", "description": "Real-world assets represented as digital tokens with privacy and compliance features."}
]
}
</script>
