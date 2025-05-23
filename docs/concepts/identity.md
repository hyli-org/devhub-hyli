# Identity management

Identity [in traditional blockchains](./hyli-vs-vintage-blockchains.md) is typically tied to a single wallet address. This approach limits flexibility and can compromise privacy.

On Hyli, **any smart contract can be a proof of identity**. This allows you to register the identity source that makes sense for your use case and generate session keys through the **Hyli wallet**.

## The Hyli wallet

The Hyli Wallet enables users to register multiple authentication methods under one identity layer.

Once registered, users can generate **session keys** (temporary credentials scoped to specific applications) that allow apps to verify identity seamlessly.

This simplifies interactions without requiring sensitive private inputs at runtime.

### Identity aggregation

You can register one or more identity methods for a single wallet.

<!--Add table of supported & roadmap items? pass+login supported, OIDC / Metamask / etc. coming soon, zkTLS coming later… -->

### Session keys

Session keys are generated in the user's browser.

They are signed by the Hyli wallet and have limited validity: a specific app, for a specific time.

They are registered onchain using their public key. Users can then sign requests using these session keys without needing to reveal private inputs.






## How Hyli processes identity

A blob transaction on Hyli includes multiple blobs. One of these blobs must contain an identity claim. (If this isn't clear, read more on [our transactions concept page](./transaction.md).)

- Each blob transaction has a single identity blob.
- All provable blobs within a transaction share the same identity.
- Identity proofs include a nonce to prevent replay attacks.
- The proof verification process ensures the identity was correctly provided.

![Each blob has a proof. Blob0 being a hydentity contract action for « verify password » on the account bob.hydentity is verified by the contract and by the node. Blob1 is an USDC transfer contract. Another graph under this one shows that each blob is proven and that all proofs are verified by the node; they all have bob.hydentity as their output identity. The identity fields are coherent between all blobs and proofs](../assets/img/how-nodes-ensure-identity.jpg)

## Choosing an identity source

When selecting an identity contract, remember:

- Identity contracts define how identity proofs are verified.
- Applications decide which identity they support. Some may enforce a specific identity type (e.g., Google accounts), while others allow multiple sources.
- Transactions can transfer Hyli tokens between different identity types, such as from a Metamask wallet to an email/password-based account.
- Users authenticate with any proof supported by their application. There are no "Hyli wallets".

The identity provider should sign the entire blob transaction to ensure that all the included blobs have been approved by the user. One approach is to make the user sign the blobs they agree to execute. The identity contract then verifies that all blobs in the transaction are properly signed.

If you don't want to create a custom identity source for early development, Hyli provides [a native `hydentity` contract](https://github.com/hyli-org/hyli/tree/main/crates/contracts/hydentity). This contract is not secure and must not be used in production.

## Identity verification methods

Hyli supports multiple identity verification methods, each with unique characteristics. Here are some of the most common.

### Private password

A user authenticates using a private password known only to them.

- Password: private input.
- Proof: only those who know the password can generate a valid proof for these blobs.
- The proof is valid only for the blobs in this blob transaction.

![In this blob transaction, bob.hydentity's blob0 has verify password as its action. The proof for Blob1 has the password as private input and compares it with the stored hash. Only those who know the password can generate a valid proof of the identity blob. This proof is valid only for the blobs in this blob transaction.](../assets/img/identity-password.jpg)

### Public signature

A user signs a message with their private key to prove identity.

- Signature: public input. No private input is required, since only the owner of the private key can generate a valid signature.
- Proof: anyone can generate a proof based on the public signature.

![In this graph, the blob transaction has 0xcafe.ecdsa as an identity. The ECDSA contract in blob0 has the action verify signature. The signature is `sign(hash([blob1])`. There is no private input required, anyone can generate a proof but only the owner of the private key can generate a valid signature.)](../assets/img/identity-public-signature.jpg)

### Private OpenID verification

This works just like it does with a private password, except it generally can't be done client-side.

The OpenID provider knows your secret key, so it could be able to generate transactions on  your behalf.

![A blob transaction where identity is bob@gmail.com.oidc. Blob0 is to verify openID with an OIDC contract; the proof for blob0 has the openID as private input and verify its validity. The other blob is an USDC contract to transfer money. ](../assets/img/identity-openid.jpg)

## Custom identity contracts

Applications on Hyli can implement custom identity verification rules through smart contracts. A typical identity contract includes two core functions, as shown in [our identity quickstart](../quickstart/example/custom-identity-contract.md):

- **Register**: Users submit an initial proof of identity.
- **Verify**: The contract validates the proof against predefined rules.

Applications can use this structure or define their own identity workflows as needed.
