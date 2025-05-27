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

On the Hyli testnet, you have one **wallet** to rule them all and climb the leaderboard. This wallet operates with every single testnet app without needing bridges or complex UX − it’s just a unified account that you can use with all the apps that settle on Hyli.

Here’s a deep dive into the technical choices we made for this application.

## A wallet to centralize your identities on Hyli

On Hyli, any smart contract can be a proof of identity. This enables you to register your preferred identity source as a smart contract for authentication. It’s great and flexible… but it also creates a lot of uncertainty, and we want to **make building on Hyli as simple as possible**.

So, while Hyli users don’t technically need a wallet, we decided to create one that will serve as an « identity hub.» Suppose one app works with your Google account, another requires a Ledger signature, and a third prefers your passport. Your Hyli wallet allows them to work hand in hand, knowing that you are the same person even though you’re using three different identities.

More importantly, once you’ve created your wallet, you don’t need private inputs for your transactions anymore: session keys include signatures that are natively verified by Hyli nodes. This means that you can submit blobs requiring authentication without needing private inputs, which removes many privacy constraints from app development.

## How the Hyli wallet works

There are four main steps to using the wallet:

1. Wallet creation
2. Session key creation (outside of the wallet)
3. Session key registration
4. Session key usage

Let’s discuss each in detail and then see why we chose to do things this way. If you prefer, you can check out the source code in [the Hyli Wallet GitHub repository](https://github.com/hyli-org/wallet).

In the flow below, Max signs up to Hyli using a password. Then, he creates a session key to interact with a USDC contract. From this point forward, his transactions don’t depend on proving his password again: they’re signed with the session key and verified onchain by the Hyli Wallet.

### Create a Hyli wallet

On classic Layer 1 blockchains, every piece of data is onchain and public. With zero-knowledge proofs and leveraging the Noir language, we can create private inputs at the base layer level on Hyli and prove them locally without needing to trust a server or proving service.

This is useful for password checks in the Hyli wallet. The user can hash the password locally and then prove that the hash has been performed correctly. When the proof is verified on Hyli, the password is a private input that never leaves the user’s device, and the hash is public inside the proof.

![image.png](attachment:e070dfc4-99be-42e9-9dea-edf05b34d15e:image.png)

Creating the Hyli wallet is done, as with everything else on Hyli, by **sending a transaction**. The transaction includes two blobs:

1. A `CheckSecret` blob takes Max’s password as private input and asserts that the hash is correct;
2. A `registerAccount` blob verifies and creates Max’s Hyli identity, `max@wallet`.

Other systems could be used for [other identification methods](https://docs.hyli.org/concepts/identity/#identity-verification-methods), but for our testnet, we chose to keep users in their comfort zone and use a simple login and password method.

The CheckSecret contract is written in Noir; see the [source code on GitHub](https://github.com/hyli-org/examples/tree/main/check-secret-noir/contract).

### Create a session key

The user can create a session key on any app. A session key includes a public key and a private key.

In our example flow, Max wants to use a USDC contract, so his session key comes from that contract.

### Register the session key

![image.png](attachment:d2eae297-2811-422b-bb5c-52b21ebcaa8a:image.png)

Max registers his session key in a new transaction, which, again, includes two blobs:

1. A `CheckSecret` blob makes sure that the password is correct, meaning that Max is correctly authenticated;
2. A `registerSessionKey` blob registers the public key as a session key associated with Max’s account and adds the USDC contract to the allowlist.

Now, the session key is linked to Max’s wallet. This means that Max can use his wallet to interact with his USDC contract.

### Use the session key for a future action

![image.png](attachment:ace40569-fb64-42a2-b0d9-b646f524707b:image.png)

Max wants to transfer money using the USDC contract. His new transaction will include three blobs:

1. A blob to *verify his signature* using his private key. The blob signs a timestamp and is used to update the session key’s nonce.
2. A blob to *verify the session key*, asserting that the key used for the signature in the first blob is associated with Max’s account.
3. A blob for the *transfer* itself.

## Why Hyli relies on session keys

With this architecture, the transfer **does not require any private input**. The signature is verified natively by blobs without requiring the generation of a zero-knowledge proof.

![image.png](attachment:52e8a8f3-c8cc-46b0-859c-84ebe2292ada:image.png)

The two other blobs don’t require authentication. Thanks to Hyli’s native proof composition, if the signature blob fails to verify, the other blobs fail, and the transaction doesn’t happen.

This has the added benefit of avoiding timeouts. Since there are no private inputs, anyone can prove every blob in the transaction without needing to worry too much about privacy. The USDC app can offload the work to a specialized prover instead of building complex circuits.

## Build with the Hyli wallet

You can use any contract as a source of identity on the Hyli wallet: play around and devise fun, hyper-secure, or very easy ways to use it!

When building your app, remember to leverage the Hyli wallet to avoid timeouts and complex circuits. Focus on what matters when you’re developing, and let us handle the rest! 

*The Hyli testnet is currently live! You can see the wallet in action at https://hyli.fun or [start building your own apps](https://docs.hyli.org/quickstart/).*





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
