# Hyli vs. vintage blockchains

If you're used to traditional blockchains such as Ethereum or Solana, keep these Hyli characteristics in mind.

## No EVM or execution layer

Hyli does not include a Virtual Machine.

There is no dedicated execution engine or specific programming language (like Solidity) you should use.

Our approach is simple: onchain, we verify zero-knowledge proofs natively. Offchain, you do everything else the way you prefer. This gives you higher throughput, faster finality, and lower gas fees.

## Minimal onchain state

The network maintains proofs of state transitions rather than the entire onchain state.

Transactions on Hyli verify and settle transitions without storing full intermediary states onchain.

This architecture reduces storage overhead and promotes scalability while maintaining trustlessness.

## No wallets

Stop asking yourself, "Which wallet do I use? How do I bridge?". You don’t need to worry about that with Hyli: any identity that can produce a valid proof can be integrated in your [Hyli wallet](./identity.md).

## Every app is a rollup

On Hyli, there is one general-purpose blockchain, and every app is its own based ZK-rollup, removing the problems associated with fragmentation.

An app’s transactions are sequenced directly on the Hyli Layer 1. They are split into a blob transaction, which allows for [pipelined proving](../concepts/pipelined-proving.md), and a proof transaction to store the state commitment onchain.

## Privacy is built-in

Unlike Ethereum, where privacy solutions must be implemented on top of the platform, Hyli integrates privacy features natively.

The proof is public, but your inputs don't need to be, as execution happens offchain.
