# Proof generation and submission

Hyli allows you to build unchained applications by leveraging zero-knowledge proofs. They allow you to avoid onchain execution, guarantee privacy, and customize your application while maintaining composability with other apps.

With Hyli, generate your proof wherever you prefer, then send it for native onchain verification and settlement. This process enables scalable, modular applications with customizable proving schemes.

If you're a complete beginner with zero-knowledge proofs, [our no-code introduction](https://blog.hyli.org/a-simple-introduction-to-zero-knowledge-proofs-zkp/) might help.

## Zero-knowledge proofs on Hyli

### Choose what you prove

Each application defines its proof logic. This means that each application developer can decide **what information** gets proven: for Hyli, proof settlement is a Success or a Failure. You define what that means for your app.

Each app developer also defines what the **public and private inputs** of their app will be: what information should remain private and what should go onchain?

### Choose how you prove

We support as many proving schemes we can, giving you the flexibility to choose the most suitable protocol for your specific use case.

We've written templates for some of [our supported programs](../reference/supported-proving-schemes.md). Clone them to [get started](../quickstart/your-first-app.md) with app writing.

We also verify these natively, without the need for a ZK proof.

- sha3_256
- BLST signatures
- Secp256k1 signatures

There are [many ZK languages](https://github.com/microbecode/zk-languages). Hyli aims to verify as many as possible.

DSLs, like Circom, are specific languages that usually compile down to a specific circuit. They're good, but they're complex and may have a high learning curve.

zkVMs prove the correct execution of arbitrary code. They allow you to build ZK applications in a certain language without having to build a circuit around it. There are two main types of zkVMs: Cairo and RISC-V. You can benchmark your Rust code and find the best zkVM for your needs with [the any-zkvm template](https://github.com/MatteoMer/any-zkvm).

We will support more types, including Cairo-based zkVMs and DSLs, in the future, and plan to support all major proving schemes eventually. The next proving schemes we're aiming to support are [Cairo](https://www.cairo-lang.org/docs/) via Stwo and [Groth16](https://github.com/arkworks-rs/groth16).

## How to generate proofs

### Choose where you prove

Each application can generate its proof in whichever place fits best.

|                                   | Pros                                                                           | Cons                                                    | When to use                                         |
|-----------------------------------|--------------------------------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|
| Client-side (browser, mobile app) | Maximum privacy<br>Data ownership                                              | Requires robust client-side hardware                    | Personal data that should remain private            |
| External prover or proving market | No client-side costs or constraints<br>Offload proof generation to the experts | Requires trusting the external prover with your inputs  | Resource-intensive and not privacy-sensitive proofs |
| By the application itself         | Simple UX<br>No dependencies<br>Code can be private                            | Higher infrastructure needs<br>Potential liveness issue | Confidential or centralized applications            |

### Autoproving through our scaffold

[Hyli’s scaffold](https://github.com/hyli-org/app-scaffold) includes a built-in AutoProver service that automatically detects transactions related to your contract. It generates the corresponding proof and submits the associated proof transaction to Hyli.

With autoproving in place, you don’t need to manage custom proving flows. Just write your contract and connect a frontend—the scaffold handles everything else.
This setup is ideal for projects where you want minimal backend setup and an easier onboarding experience.

Visit the [scaffold repository](https://github.com/hyli-org/app-scaffold) or follow [the quickstart instructions](../quickstart/scaffold.md).

### Our proof generation partners

If you choose to work with an external prover or proving market, you can choose from one of our partners in that area and benefit from a better Hyli integration.

We recommend checking out [Electron](https://electron.dev/), [Fermah](https://www.fermah.xyz/), [Kakarot](https://www.kakarot.org/), [Marlin](https://www.marlin.org/), [Nexus](https://nexus.xyz/), [Nodekit](https://www.nodekit.xyz/), [ZEROBASE](http://zerobase.pro/), or [zkCloud](https://zkcloud.com/).

## Submitting a proof to Hyli

Read more about the transaction lifecycle on our [transactions overview](./transaction.md).

First, your application sends a blob transaction to Hyli.

Thanks to [pipelined proving](./pipelined-proving.md), once a transaction is submitted, it is sequenced.

You can then start generating your proof using the sequenced virtual base state as the base state for your operation. [Read our quickstart](../quickstart/example/first-token-contract.md#prove-the-transaction) for a concrete example of how to prove a transaction.

Once sent, the proof goes through Hyli’s native verification, removing the need for verifier contracts.

Once verified, the proof is settled onchain. Use this settled state to update your app accordingly outside of Hyli.

## External resources

- [Zero-knowledge proofs explained at 5 levels of difficulty](https://www.youtube.com/watch?v=fOGdb1CTu5c) (22')
- [awesome-zk](https://github.com/ventali/awesome-zk?tab=readme-ov-file) link repository on GitHub
- Hyli's [very simple introduction to zero-knowledge proofs](https://blog.hyli.org/a-simple-introduction-to-zero-knowledge-proofs-zkp/)
- Lauri Peltonen's [blog series on ZK](https://medium.com/@laurippeltonen)
