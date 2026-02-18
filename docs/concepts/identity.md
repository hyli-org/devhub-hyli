# Identity management

!!! info
    See also [Wallet](../tooling/wallet.md) for wallet usage.

Identity [in traditional blockchains](./hyli-vs-vintage-blockchains.md) is typically tied to a single wallet address. This approach limits flexibility and compromises privacy.

On Hyli, **any app can be a proof of identity**. This enables you to register your preferred identity source as an app for authentication. The [Hyli wallet](../tooling/wallet.md) is a tool to aggregate all these identities; we recommend using it for simplicity.

## Identity for regulated finance

Financial institutions building on Hyli can integrate identity management with existing KYC/AML frameworks.

Licensed stablecoin issuers handle KYC at onboarding (as required by MiCA). Onchain identity provides cryptographic integrity: users prove they control their identity without exposing personal information publicly.

- **Privacy**: User identities remain confidential onchain.
- **Compliance**: Issuers maintain full KYC/AML records off-chain.
- **Cryptographic guarantees**: Identity proofs ensure only authorized users can transact.
- **Regulatory transparency**: Selective disclosure enables regulators to verify identity compliance without public exposure.

### Account abstraction for institutions

Hyli supports account abstraction natively, giving institutions full flexibility over wallet design and user experience.

Financial institutions can design identity experiences that feel familiar to end users while maintaining the cryptographic guarantees of blockchain settlement.

## How Hyli processes identity proofs

A blob transaction on Hyli includes multiple blobs. One of these blobs must contain an identity claim. (If this isn't clear, read more on [our transactions concept page](./transaction.md).)

- Each blob transaction has a single identity blob.
- All provable blobs within a transaction share the same identity.
- Identity proofs include a nonce to prevent replay attacks.
- The proof verification process ensures the identity was correctly provided.

![Each blob has a proof. Blob0 being a hydentity contract action for « verify password » on the account bob.hydentity is verified by the contract and by the node. Blob1 is an USDC transfer contract. Another graph under this one shows that each blob is proven and that all proofs are verified by the node; they all have bob.hydentity as their output identity. The identity fields are coherent between all blobs and proofs](../assets/img/identity/how-nodes-ensure-identity.jpg)

## Choosing an identity source

When selecting an identity contract, remember:

- Identity contracts define how identity proofs are verified.
- Applications decide which identity they support. Some may enforce a specific identity type (e.g., Google accounts), while others allow multiple sources.
- Transactions can transfer Hyli tokens between different identity types, such as from a Metamask wallet to an email/password-based account.
- Users authenticate with any proof supported by their application. There are no "Hyli wallets".

The identity provider should sign the entire blob transaction to ensure that all the included blobs have been approved by the user. One approach is to make the user sign the blobs they agree to execute. The identity contract then verifies that all blobs in the transaction are properly signed.

If you don't need to create a custom identity source for your app, or don't want to do it in early development, use [the Hyli wallet](../tooling/wallet.md). It's integrated by default in [our app scaffold](../quickstart/index.md).

## Custom identity contracts

Applications on Hyli can implement custom identity verification rules through apps. A typical identity contract includes two core functions:

- **Register**: Users submit an initial proof of identity.
- **Verify**: The contract validates the proof against predefined rules.

Applications can use this structure or define their own identity workflows as needed.

## Identity verification methods examples

Hyli supports multiple identity verification methods, each with unique characteristics. Here are some of the most common.

### Private password

A user authenticates using a private password known only to them.

- Password: private input.
- Proof: only those who know the password can generate a valid proof for these blobs.
- The proof is valid only for the blobs in this blob transaction.

![In this blob transaction, bob.hydentity's blob0 has verify password as its action. The proof for Blob1 has the password as private input and compares it with the stored hash. Only those who know the password can generate a valid proof of the identity blob. This proof is valid only for the blobs in this blob transaction.](../assets/img/identity/identity-password.jpg)

### Public signature

A user signs a message with their private key to prove identity.

- Signature: public input. No private input is required, since only the owner of the private key can generate a valid signature.
- Proof: anyone can generate a proof based on the public signature.

![In this graph, the blob transaction has 0xcafe.ecdsa as an identity. The ECDSA contract in blob0 has the action verify signature. The signature is `sign(hash([blob1])`. There is no private input required, anyone can generate a proof but only the owner of the private key can generate a valid signature.)](../assets/img/identity/identity-public-signature.jpg)
