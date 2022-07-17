The Starknet Burner helps builders to build dapps on mobile applications during
hackathons without having to deal with all the abstract account interactions.

## Content (Sofar...)

The project contains a set of hardcoded methods that allow to
transfer an ERC20 named Stark Pills from the mobile application.

## Milestone v0.2.0

For now the project contains a working burner wallet and as well as its
working drone. To make it work, we had to provide a new compliant account
contract to the argent-x repository as well as a custom plugin that
implements an EIP-712 like signature for the wallet. To make the example
fully work, you might need some Starkpills. Do not hesitate to contact us
if you need more details.

## What to do next?

There is a number of things we can added to the project to make it better:

1. make the burner a library that can be embedded in other projects (target
   v0.3.0)
2. make the contract upgradable from drone (target v0.4.0) as for now, the
   procedure requires you interact with your argent-x wallet on voyager.
3. develop offchain collaborations that could be sealed with a set of
   signatures. It could be a fraud detection system, a 2FA service or even
   some third party authentication. The goal would be to rely on 3rd parties
   to prevents fraud. (target v0.5.0)

## Expected technical evolution

A number of evolutions are planned or proposed that might impact the project.
Below is a list of identified evolutions and technical work that we could rely
on.

### Validate and execute separation

Starknet 0.10 that is due in August and should split the execution of a
transaction in 2 parts. The validation of the signature, the nonce and fees
should be removed from the execute and be managed by a function called
`validate`. This feature might not full impact the project, except that the
validation plugin would also have to be updated in the contract definition.

You will find more details about v0.10.0
[here](https://starkware.notion.site/StarkNet-0-10-0-4ac978234c384a30a195ce4070461257)

### Payment delegation

Once the previous change is done done, the roadmap would also include the
ability to delegate paiements to a `PaymentContract`. A third party contract
would be able to pay the fees for your transactions based on a previous
commitment. This could be a security improvement for the plugin that could
only access funds from that `PaymentContract`.

### Additions to OpenZeppelin account

There are also a number of additions to the OpenZeppelin account that could be
of some use. In particular, it supports a ETH signature as you can see from PR
[#361](https://github.com/OpenZeppelin/cairo-contracts/pull/361). It might rely
on [the secp implementations](https://community.starknet.io/t/is-it-possible-to-use-verify-ecdsa-signature-in-cairo-to-verify-a-web3-js-wallet-ecdsa-signature/338).

To check the changes associated with OpenZeppelin, we should review:
[release notes](https://github.com/OpenZeppelin/cairo-contracts/releases)

### A more advanced plugin

There are a few ideas that we could implement to make the plugin more
advanced. In particular:

- limiting the plugin to interact with only one contract is a must. This is
  an hard requirement to make the solution usable in games or other
  applications.
- there is an open question about how we could limit the cost managed by the
  plugin. Can we embed something on the PaymentContract to track the actual
  signer? Could we have several Nonce (i.e. 1 per signer), etc
- there is an interesting branch in called
  [explore/pluginsv2](https://github.com/CremaFR/argent-contracts-starknet/tree/explore/pluginsv2)
  that should be reviewed to get some additional ideas of what could be done.
  There is also another plugin called `ArgentSecurity.cairo` with the current
  `explore/plugins` branch.

### More secure signing on mobiles

We can find some work here and there to support hashing/signing with other
schemes than [sn_keccak and pederson](https://docs.starknet.io/docs/Hashing/hash-functions/).

For instance, you can find these resources:
- [A cairo implementation of NIST P-256](https://github.com/spartucus/nistp256-cairo) 
- [Cairo examples](https://github.com/starkware-libs/cairo-examples/tree/master/secp)

This area has still to be research, both to change the validation scheme in Cairo and to
rely on the secure enclave, including with webauthn. the following documentations might
be interesting to read in detail:

- [Storing Keys in the Secure Enclave](https://developer.apple.com/documentation/security/certificate_key_and_trust_services/keys/storing_keys_in_the_secure_enclave)
- [iOS Keychain: using Secure Enclave-stored keys](https://medium.com/@alx.gridnev/ios-keychain-using-secure-enclave-stored-keys-8f7c81227f4)

### Even more resources

Then there are a number of additional features that we could target. The list
below is provided without any specific order or priority:

- enrich the interface to make the wallet perform more actions, like minting
  token or claiming a reward.
- providing an interface that is close to the one from the other wallets so
  that we can use starknet.js or a subset of it
- addition a sign/autosign feature so that the user does not even see he is
  minting tokens.
- Building demos to help people boostrap their projects.
- Have `drone` hosted or running on-demand to make it easier to use with
  hackathons. The project would "just implement the burner side" not the
  validation side to begin.
- Setting up the same feature with other accounts, including OpenZeppelin, Braavos
  and Metamask.
- Integrate an Indexer to check the assets minted, transferred and burned with the
  Starknet Burner.
- Improve the interaction to refresh the components automatically.
- Provide various ways to exchanges between the mobile and the application. It
  could be:
  - we send an SMS with a key;
  - we scan a QR code from the session public key;
  - we use some pre-signed printed QR code;
  - we use the NFC tokens and a phone;
  - ... other ideas are welcome... open an issue if you have any.
- It can be used to automate tests on the Goerli network without risking to
  loose the funds by requesting a token based on an NFT or something else.
- Checkout [Web3Auth/sign-in-with-starkware](https://github.com/Web3Auth/sign-in-with-starkware)
  to see if that can somehow be leverage
- Checkout [abdelhamidbakhta/starkvest](https://github.com/abdelhamidbakhta/starkvest)
  to see if it can make sense to use it for the project.