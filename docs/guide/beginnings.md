# A (very short) Hyli history

The Hyli founders, [Sylve](https://x.com/sylvechv) and [Lancelot](https://x.com/wraitii), developed various consumer and developer-facing applications and frameworks within the Starknet ecosystem. Building chronically onchain applications led them to discover the limitations of current blockchains.

The first problem was that **all applications shared the same execution resource**, resulting in an unpredictable user experience. During periods of high congestion, the user experience of the apps would slow down until the congestion was resolved.

Another limiting factor in terms of the scale that applications can achieve is **onchain states**. Onchain states are expensive, and providing a whole state for each transaction is impractical. This state grows over time, making it very expensive for users to use the apps.

They encountered multiple other problems, including the inability to use one’s preferred language and tooling, the inability to customize aspects like block times, and restrictions due to the amount of block gas on the base chain. We will dive into these problems in a later section of this guide.

Sylve and Lancelot discussed **how to create a new Layer 1 with a future-proof architecture that can scale and solve these limitations**.

Zero-knowledge proofs had become very practical, and Starknet was proving that they are the best way to scale execution. This made them wonder: Why does the L1 even handle execution? Why not just sequence the transaction and verify the proofs? And if something other than zero-knowledge proofs appeared, how would that L1 support it?

The idea crystallized during a trip to Crete at the end of 2023, where they concluded that **they could build such an L1 themselves**. A Layer 1 that can horizontally scale thanks to validity proofs, handle millions of TPS, and never get burdened with the ever-growing onchain state.

In a few months, the team [successfully raised their seed round](https://blog.hyli.org/hyle-raises-3-4m-to-build-the-future-of-zk-proof-verification/), led by Framework VC, demonstrating support for the vision that the Hyli team is building. They assembled a team in 2024 to implement this vision, beginning with a Tendermint-based prototype to validate their thesis and identify what they needed and what they could omit.

The rest is history!

The team has since shipped a new [blockchain client](https://github.com/hyli-org/hyli) in Rust, featuring an implementation of the cutting-edge [Autobahn consensus](../concepts/consensus.md), and has demonstrated various applications built on top of it in its first public testnet, all with a team size of less than 10.

The team continues to build its vision of the future-proof Layer 1 blockchain from its office in Paris.
