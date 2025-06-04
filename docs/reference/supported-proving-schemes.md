# Supported proving schemes

Clone a template or write your own program to [get started with sapp writing](../quickstart/your-first-app.md).

| Proving scheme | Verifier | Program ID | Template |
|----------------|----------|---------------------------------------------------|---|
| [Noir](https://noir-lang.org/docs/)     | noir     | Verification key. | |
| [Risc0](https://risc0.com/docs/)    | risc0    | Image ID without a prefix. ex. 0x123 becomes 123. | [Template](https://github.com/hyli-org/template-risc0)|
| [SP1](https://docs.succinct.xyz/docs/introduction)        | sp1   | Verification key.       | [Template](https://github.com/hyli-org/template-sp1)|

<!--- **Cairo**: Cairo smart contracts will be identified by their Class Hash in the future.
- **Groth16**: Groth16 programs require a trusted ceremony. As such, their identifier is the verifying key corresponding to the matching private key, which will be unique for each program & ceremony.-->