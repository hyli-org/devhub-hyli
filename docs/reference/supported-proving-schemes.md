# Supported proving schemes

Clone a template or write your own program to [get started with app writing](../quickstart/index.md).

| Proving scheme | Verifier | Program ID | Example app |
|----------------|----------|---------------------------------------------------|---|
| [Noir](https://noir-lang.org/docs/)     | noir     | Verification key. | [check_secret](https://github.com/Hyle-org/examples/tree/main/check-secret-noir) |
| [Risc0](https://risc0.com/docs/)    | risc0    | Image ID without a prefix. ex. 0x123 becomes 123. | [eZKasino](https://github.com/hyli-org/ezcasino/tree/main/contracts)|
| [SP1](https://docs.succinct.xyz/docs/introduction)        | sp1   | Verification key.       | [Faucet](https://github.com/hyli-org/faucet/tree/main/contracts)|

## Noir

Because Noir is a circuit-based ZK-language, you have to define the maximum size of the state at the contract creation. For simplicity, we recommend keeping Noir for stateless contracts:

- A stateless contract in Noir for private inputs and the logic linked to them.
- A stateful contract in Rust using Risc0 or SP1 to store the private state onchain.

[Proof composition](https://docs.hyli.org/concepts/proof-composition/) allows you to leverage the privacy of Noir and ease of use of zkVMs.
