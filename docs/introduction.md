# Introduction to Hyli

[Hyli](https://hyli.org/) is the new proof-powered L1 to build the next generation of apps onchain.

On Hyli, instead of executing transactions onchain, you run your app logic anywhere off-chain, in Rust, Noir, or even multiple languages at once. You only need to send a proof for onchain settlement. With this model, your app benefits from the Web2 user experience and the security of Web3.

## How Hyli works

Here’s what happens when you use Hyli’s next-generation Layer 1:

1. **Write your app**: Write a contract and define how state changes should happen. You'll run this logic wherever you want. Write in Rust or Noir and use any proving scheme we support. You can also leverage our native digital signature verifiers.
1. **Send a blob transaction**: Send a provable blob to Hyli, stating what final state you expect after the transaction. Hyli will sequence the transaction immediately: you get a timestamp and a guaranteed place in the block. [Read more about pipelined proving](./concepts/pipelined-proving.md).
1. **Generate and submit the proof**: Generate proofs wherever you want: client-side or in a browser, with or without a partner proving network. When the proofs for your transaction are ready, send them to Hyli in a proof transaction that references the previous blob transaction.
1. **Finality**: Hyli validators receive the transaction and verify the proofs natively, without the limitations of a bulky virtual machine. If the proofs are valid, Hyli finalizes the transaction and updates your contract's onchain state root. You're good to go!

![Sequence diagram explaining the steps going from sequencing to proving to verification and consensus.](./assets/img/hyle-main-diagram.jpg)

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
