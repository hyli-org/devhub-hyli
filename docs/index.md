---
description: Hyli is the new proof-powered L1 to build the next generation of apps. This is your developer documentation.
---

# Home

[Hyli](https://hyli.org/) is the new proof-powered L1 to build the next generation of apps.

With Hyli, developers can build fast, composable, and verifiable apps without dealing with the usual pains of blockchain.

On Hyli, instead of executing transactions onchain, you run your app logic anywhere off-chain, in Rust, Noir, or even multiple languages at once. You only need to send a proof for onchain settlement. With this model, your app benefits from the Web2 user experience and the security of Web3.

## Navigation

<div class="grid cards" markdown>

-   :material-home:{ .lg .middle } __Hyli guide__

    ---

    Your entry point to understanding Hyli, with minimal code.

    [:octicons-arrow-right-24: Primer](./guide/index.md)

-   :material-clock-fast:{ .lg .middle } __Quickstart__

    ---

    Get started with Hyli in just a few minutes with a step-by-step annotated quickstart.

    [:octicons-arrow-right-24: Quickstart](./quickstart/index.md)

-   :material-lightbulb:{ .lg .middle } __Concepts__

    ---

    Hyli-specific concepts and Hyli's spin on industry classics explained in detail.

    [:octicons-arrow-right-24: Concepts](./concepts/index.md)

-   :material-tools:{ .lg .middle } __Tooling__

    ---

    Hyli tooling to improve your building experience.

    [:octicons-arrow-right-24: Tooling](./tooling/index.md)

</div>

[Reach out on Telegram](https://t.me/hyli_org) or [open an issue](https://github.com/hyli-org/devhub-hyli) if you need assistance or wish to provide feedback on the documentation: we're always looking to improve.

## How Hyli works

Here’s what happens when you use Hyli’s next-generation Layer 1:

1. __Write your app__: Write a contract and define how state changes should happen. You'll run this logic wherever you want. Write in Rust or Noir and use any proving scheme we support. You can also leverage our native digital signature verifiers.
1. __Send a blob transaction__: Send a provable blob to Hyli, stating what final state you expect after the transaction. Hyli will sequence the transaction immediately: you get a timestamp and a guaranteed place in the block. [Read more about pipelined proving](./concepts/pipelined-proving.md).
1. __Generate and submit the proof__: Generate proofs wherever you want: client-side or in a browser, with or without a partner proving network. When the proofs for your transaction are ready, send them to Hyli in a proof transaction that references the previous blob transaction.
1. __Finality__: Hyli validators receive the transaction and verify the proofs natively, without the limitations of a bulky virtual machine. If the proofs are valid, Hyli finalizes the transaction and updates your contract's onchain state root. You're good to go!

![Sequence diagram explaining the steps going from sequencing to proving to verification and consensus.](./assets/img/hyli-main-diagram.jpg)

## What you get with Hyli

Hyli is built for speed, flexibility, and seamless blockchain integration.

With our model, you get:

- You skip onchain execution entirely.
- You can use the Hyli wallet for [identity abstraction](./concepts/identity.md).
- You choose your proving system (Noir, RISC0, SP1…).
- You generate your proofs wherever you'd like (client-side, in a server, through a proving network…).
- You can [compose proofs](./concepts/proof-composition.md) across contracts and languages.
- You benefit from the state-of-the-art Autobahn consensus.
- You get Web2-like speed with Web3 security.

Read more about [how Hyli compares to legacy blockchains](./concepts/hyli-vs-vintage-blockchains.md).

## Useful links

<div class="grid cards" markdown>

- :fontawesome-solid-circle-nodes: [Rust node](http://github.com/hyli-org/hyli)
- :material-hexagon-multiple-outline: [Example contracts](http://github.com/hyli-org/examples)
- :material-home: [Website](https://hyli.org)
- :material-rss: [Hyli blog](https://blog.hyli.org)

</div>

Vibe coders, use our [LLMs.txt file](./llms.txt)!

## Let's talk

Reach out to the team for more information:

| :fontawesome-brands-github: Github | :fontawesome-brands-twitter: Twitter | :fontawesome-solid-archway: Farcaster | :fontawesome-brands-linkedin: LinkedIn | :fontawesome-brands-youtube: Youtube |:fontawesome-brands-telegram: Telegram|
|-------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|--------------------------------------|
| [Hyli](https://github.com/hyli-org) | [@hyli_org](https://x.com/hyli_org)  | [@Hyli](https://farcaster.xyz/hyli-org) | [Hyli](https://www.linkedin.com/company/hyli-org/) | [@Hyli](https://www.youtube.com/@hyli-org) | [Hyli](https://t.me/hyli_org)|
