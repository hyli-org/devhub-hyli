# Hyli vs. traditional blockchains for financial institutions

If you're used to traditional blockchains, keep these Hyli characteristics in mind, especially if you're building financial applications that require both privacy and regulatory compliance.

## Why traditional blockchains don't work for regulated finance

Public blockchains expose all transaction data: every amount, every counterparty, every timestamp is permanently visible to everyone. For financial institutions, this is incompatible with competitive requirements, GDPR obligations, and basic fiduciary duties to clients.

Traditional blockchains force a choice: either sacrifice privacy (public chains) or sacrifice verifiability (private or permissioned chains). Hyli solves this through **selective disclosure**: cryptographic privacy with regulatory transparency.

## Privacy-first architecture

Hyli does not include a Virtual Machine.

There is no dedicated execution engine or specific programming language (like Solidity) you should use.

Financial applications need confidentiality. By executing off-chain with zero-knowledge proofs, Hyli provides transaction-level privacy, something that was impossible on public smart contract platforms. Your sensitive financial data never touches the public ledger.

Our approach is simple: onchain, we verify zero-knowledge proofs natively. Offchain, you do everything else the way you prefer. This gives you higher throughput, faster finality, lower gas fees, and most importantly, privacy by default.

## Minimal onchain state

The network maintains proofs of state transitions rather than the entire onchain state.

Transactions on Hyli verify and settle transitions without storing full intermediary states onchain.

This architecture reduces storage overhead and promotes scalability while maintaining trustlessness.

## No wallets

Stop asking yourself, "Which wallet do I use? How do I bridge?". You don’t need to worry about that with Hyli: any identity that can produce a valid proof can be integrated in your [Hyli wallet](./identity.md).

## Every app is a rollup

On Hyli, there is one general-purpose blockchain, and every app is its own based ZK-rollup, removing the problems associated with fragmentation.

An app’s transactions are sequenced directly on the Hyli Layer 1. They are split into a blob transaction, which allows for [pipelined proving](../concepts/pipelined-proving.md), and a proof transaction to store the state commitment onchain.

## GDPR-aligned Privacy + MiCA compliance

Unlike public chains where privacy solutions must be bolted on top (and rarely work for financial use cases), Hyli integrates privacy features natively through its off-chain execution model.

The proof is public, but your inputs don't need to be, as execution happens offchain.

### Selective disclosure for different stakeholders

Hyli's architecture enables selective disclosure: different parties see different levels of information based on their role:

| Stakeholder | What They See |
|-------------|---------------|
| **Transaction parties** | Full transaction details |
| **Stablecoin issuer** | Full visibility into their application |
| **MiCA regulators** | Selective disclosure on request for AML/CTF |
| **General public** | Proof that a transaction occurred, no details |

This solves the fundamental privacy-compliance dilemma: users get GDPR-aligned privacy, regulators get the transparency they need for AML/CTF enforcement, and the system remains cryptographically verifiable—not trust-based.
