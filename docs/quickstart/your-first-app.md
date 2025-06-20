# Your first app

!!! tip
    Check our [example walkthrough](./example/index.md) for more!

!!! failure
    This documentation is not up-to-date. We'll rewrite it soon. In the meantime, we recommend using our [testnet faucet](https://github.com/hyli-org/faucet) or [testnet wallet](https://github.com/hyli-org/wallet) as examples.

## Step 1: Clone a template

We support [several proving schemes](../reference/supported-proving-schemes.md):

| Proving scheme | Verifier | Program ID | Language | Template |
|----------------|----------|---------------------------------------------------|---|---|
| [Noir](https://noir-lang.org/docs/)     | noir     | Verification key. | Noir | |
| [Risc0](https://risc0.com/docs/)    | risc0    | Image ID without a prefix. ex. 0x123 becomes 123. | Rust | [Template](https://github.com/hyli-org/template-risc0)|
| [SP1](https://docs.succinct.xyz/docs/introduction)        | sp1   | Verification key.       | Rust | [Template](https://github.com/hyli-org/template-sp1)|

Clone the template or, for proving schemes without templates, use the templates as inspiration for writing your contract. You can also check our [app concept page](../concepts/apps.md) for more information!

## Step 2: Edit your app

Navigate to the `contract/` folder and edit your contract as necessary.

For identity management, we strongly recommend you rely on the [Hyli wallet](../concepts/identity.md).

In our templates, the application backend that generates the proof is a CLI. You can change this to your favorite architecture, for instance an http server.

Use any architecture you like for your [proof generation and submission](../concepts/proof-generation.md): the only thing we need is a valid `HyleOutput`. If you're not 100% sure how to do that, you might enjoy the autoprover [in our scaffold](./scaffold.md).
