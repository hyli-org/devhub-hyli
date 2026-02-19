---
description: Hyli is private, compliant infrastructure for stablecoins and tokenized assets.
---

# Home

[Hyli](https://hyli.org/) is the settlement layer for private, compliant stablecoins and tokenized assets.

Hyli enables financial institutions and enterprises to build private stablecoins and tokenize real-world assets with cryptographic privacy guarantees and regulation-grade compliance through selective disclosure.

By separating execution from verification and leveraging cryptographic proofs, Hyli combines Web2 scalability with Web3 trustworthiness.

## Navigation

<div class="grid cards" markdown>

-   :material-home:{ .lg .middle } __Hyli guide__

    ---

    Your entry point to understanding Hyli.

    [:octicons-arrow-right-24: Guide](./guide/index.md)

-   :material-lightbulb:{ .lg .middle } __Concepts__

    ---

    Hyli-specific concepts and technical architecture explained in detail.

    [:octicons-arrow-right-24: Concepts](./concepts/index.md)

-   :material-tools:{ .lg .middle } __Tooling__

    ---

    Explore Hyli's core infrastructure and tools.

    [:octicons-arrow-right-24: Tooling](./tooling/index.md)

</div>

[Reach out on Telegram](https://t.me/hyli_org) or [open an issue](https://github.com/hyli-org/devhub-hyli) if you need assistance or wish to provide feedback on the documentation: we're always looking to improve.

## How Hyli works

Here's what happens on Hyli's next-generation Layer 1:

1. __Offchain execution__: Application logic runs offchain in Rust, Noir, or any supported language, with flexible proving scheme selection and native signature verification.
1. __Blob transaction sequencing__: A provable blob containing the expected post-transaction state is submitted to Hyli and instantly sequenced with a timestamp and guaranteed block position. [Read more about pipelined proving](./concepts/pipelined-proving.md).
1. __Proof submission__: Cryptographic proofs are generated and submitted in a proof transaction referencing the original blob.
1. __Native verification and finality__: Validators verify proofs natively without VM overhead. Valid proofs are finalized and settled onchain.

![Sequence diagram explaining the steps going from sequencing to proving to verification and consensus.](./assets/img/hyli-main-diagram.jpg)

## What you get with Hyli

Hyli delivers privacy, compliance, and performance for financial institutions and enterprises.

- __Transaction-level confidentiality__: Sensitive data never touches the public ledger.
- __MiCA-compliant selective disclosure__: Built-in regulatory compliance through controlled data access.
- __Flexible development__: Leverage familiar programming languages and tools without blockchain-specific expertise.
- __Identity abstraction__: Enterprise-grade identity management with the [Hyli wallet](./concepts/identity.md).
- __Proof composition__: [Composable proofs](./concepts/proof-composition.md) across contracts and [multiple proving systems](./reference/supported-proving-schemes.md).
- __Fast finality__: State-of-the-art [Autobahn consensus](./concepts/consensus.md) for institutional-grade performance.

Read more about [how Hyli compares to legacy blockchains](./concepts/hyli-vs-vintage-blockchains.md).

## Useful links

<div class="grid cards" markdown>

- :fontawesome-solid-circle-nodes: [Rust node](http://github.com/hyli-org/hyli)
- :material-home: [Website](https://hyli.org)
- :material-rss: [Hyli blog](https://blog.hyli.org)

</div>

## Let's talk

Reach out to the team for more information:

| :fontawesome-brands-github: Github | :fontawesome-brands-twitter: Twitter | :fontawesome-solid-archway: Farcaster | :fontawesome-brands-linkedin: LinkedIn | :fontawesome-brands-youtube: Youtube |:fontawesome-brands-telegram: Telegram|
|-------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|
| [Hyli](https://github.com/hyli-org) | [@hyli_org](https://x.com/hyli_org)  | [@Hyli](https://farcaster.xyz/hyli-org) | [Hyli](https://www.linkedin.com/company/hyli-org/) | [@Hyli](https://www.youtube.com/@hyli-org) | [Hyli](https://t.me/hyli_org)|
