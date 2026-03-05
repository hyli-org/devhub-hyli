# Why Hyli?

Financial institutions building digital asset infrastructure face a fundamental problem: public blockchains expose everything.

Every transaction amount, counterparty, and timestamp is permanently visible to anyone. For regulated entities, this is incompatible with competitive confidentiality, GDPR obligations, and fiduciary duties to clients.

## The problem with public blockchains

Public blockchains achieve trustlessness by making all transaction data publicly auditable. This works for permissionless consumer applications, but creates serious problems for institutional use:

- Competitors can monitor payment flows and trading activity.
- Client transaction histories are permanently exposed.
- GDPR and data minimisation obligations cannot be met when data is written to a public ledger.

## The problem with permissioned chains

Private or permissioned blockchains restrict data access. But this means settlement guarantees rest on trusting the operator, not on cryptographic proof. This reintroduces the counterparty risk that blockchain infrastructure is meant to eliminate.

## The privacy-compliance dilemma

Regulators and compliance frameworks require transparency. MiCA requires stablecoin issuers to maintain auditable records. AML/CTF frameworks require institutions to demonstrate the legitimacy of transactions on demand.

Public chains provide too much transparency: all data is permanently public. Permissioned chains provide too little: there is no cryptographic proof of correct behaviour.

No existing infrastructure offered both privacy by default and verifiable compliance on demand.

## How Hyli addresses this

Hyli is a privacy layer where applications execute offchain and settle outcomes through cryptographic proof verification.

The proof is public. The underlying transaction data is not.

This architecture delivers:

- **Privacy by default**: transaction details never touch the public ledger.
- **Selective disclosure**: issuers, regulators, and counterparties can access what they need — nothing more.
- **Cryptographic settlement guarantees**: outcomes are proven, not trusted.
- **Regulatory alignment**: designed for entities subject to MiCA and AML/CTF requirements.

## Who builds on Hyli

Hyli is designed for institutional digital asset infrastructure:

- Licensed stablecoin issuers deploying EUR or GBP tokens under MiCA
- Tokenization platforms handling private investor positions and secondary settlement
- Payment infrastructure providers building confidential B2B settlement networks
- Banks, custodians, and asset managers exploring private digital asset issuance

Hyli is strongest where public transparency blocks adoption.

Read more about [how Hyli compares to traditional blockchains](../concepts/hyli-vs-vintage-blockchains.md) and [the core components that make this possible](./components.md).
