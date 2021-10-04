---
title: "Adventures in Adaptor Signatures"
subtitle: "How fun was porting Schnorr-fun adaptor signatures from Rust to Python?"
---

## Motivation

The next update of the scripts for my eltoo simulation will replace the HTLCs with [PTLCs](https://suredbits.com/payment-points-part-1/). To implement PTLCs I need support for [Adaptor Signatures](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki#Adaptor_Signatures). Although you can implement adaptor signatures using ECDSA today, eltoo will rely on Taproot which uses Schnorr signatures. For my simulation I needed an adaptor signature implementation that satisfied three criteria:
* Python (using SECP256K1 functions)
* Schnorr based
* Support for x-only pubkeys
  
I do not need something fast, secure or robustly tested because this is just a prototyping experiment and ease of understanding was most important.

## Implementations

The first useful example implementation I found was from [@LeoComandini](https://twitter.com/LeoComandini). His [adaptor-py](https://github.com/LeoComandini/adaptor-py) Python scripts were written to go along with a [Satoshi Spritz](https://satoshispritz.com) presentation he gave, ["How DLC-Bets Work?"](https://satoshispritz.com/presentazioni/210318-how_dlc-bets_work.pdf). If you want to learn about adaptor signatures, this (toy) pure python implementation for ECDSA and Schnorr is a great place to start. Unfortunately it relied on it's own eliptical curve functions and did not take into account x-only public keys. I wanted to use something that added a minimal amount of code to the functional test framework and also would be compatible with the x-only pubkeys used for tapscript.

The next solution that was recommended to me was to look at [@LLFourn](https://twitter.com/LLFOURN)'s [Schnorr-fun](https://docs.rs/schnorr_fun/0.6.2/schnorr_fun/) Rust implementation of BIP-340 compatible [adaptor signatures](https://docs.rs/schnorr_fun/0.6.2/schnorr_fun/adaptor/index.html). This was exactly what I needed, but in the wrong language to use for my eltoo simulation. 

I decided that I should try to port the schnorr_fun adaptor signature scheme to Python, but quickly got lost in how to translate the all important eliptical curve computations from the elegant Rust [secp256k1-fun](https://docs.rs/secp256kfun/0.6.2/secp256kfun/) system to the Python based [SECP256K1](https://github.com/bitcoin/bitcoin/blob/cdb4dfcbf1c82ef4c4eb7bb982b35219d5dbf827/test/functional/test_framework/key.py#L215) eliptical curve functions used in bitcoin-core's functional test framework. I reached out to my friend [@Elichai2](https://twitter.com/Elichai2) for help. Elichai is very smart about all things Bitcoin and elipitcal curves and authored the [Bitcoin Optech](https://bitcoinops.org/) [Schnorr signature tuturoial](https://youtu.be/wybiVFdknhg). Elichai whipped up an example Python [adaptor signature scheme](https://github.com/elichai/bitcoin/blob/b8252af349143346b5881ddb4c627e460557c3ae/test/functional/test_framework/key.py#L636) in a couple of hours (!) that demonstrated how to use SECP256K1 for what I wanted to do in the functional test framework. Thanks again Elichai!

I spent the next couple of weeks debugging my translation of Lloyd's Rust adaptor functions to Python, using Elichai's implementation and tips to guide me along the way. I won't go into all of the things I did wrong, but will mention the final break-through was that I was using an x-only pubkey for the [encryption_key](https://github.com/remyers/bitcoin/blob/c7e68f209105d91583ef1ed4c8968a7a09b0e351/test/functional/feature_adaptorsig.py#L44) parameter. This caused my adaptor signatures functions to work perfectly, BUT the recovered schnorr signatures did not verify as valid Schnorr signatures about half the time. More closely looking at the schnorr-fun version revealed my error and then everything magically worked.

## Testing

The file test/functional/feature_adaptorsig.py contains a simple test (translated from schnorr-fun) that demonstrates how to use the adaptor signature functions:

```python
        # Alice knows: signing_key
        # Bob knows: decryption_key
        # Both know: verification_key, encryption_key
        signing_key = generate_privkey() # x
        verification_key = compute_xonly_pubkey(signing_key)[0] # X
        decryption_key = generate_privkey() # y
        encryption_key = encryption_key_for(int.from_bytes(decryption_key, 'big')) # Y
        msg = hashlib.sha256(b"give 100 coins to Bob").digest()

        # Alice creates an encrypted signature for msg and sends it to Bob
        encrypted_signature, needs_negation = schnorr_adaptor_encrypt(signing_key, verification_key, encryption_key, msg)

        # Bob verifies the encrypted signature and decrypts it
        assert schnorr_adaptor_verify(verification_key, encryption_key, msg, encrypted_signature, needs_negation)
        signature = schnorr_adaptor_decrypt(decryption_key, encrypted_signature, needs_negation)

        # Verify schnorr signature using standard method
        assert verify_schnorr(verification_key, signature, msg)

        # Bob then broadcasts the signature to the public.
        # Once Alice sees it she can recover Bob's secret decryption key
        recovered_decryption_key = schnorr_adaptor_recover(encryption_key, encrypted_signature, signature, needs_negation)
        assert recovered_decryption_key != None

        # Alice got the decryption key, otherwise the signature is not the decryption of our original encrypted signature
        assert recovered_decryption_key == int.from_bytes(decryption_key,'big')
```

Ideally we would loop over different random and edge case key combinations to test this code, but for now the goal is more education than correctness.

## Next Steps

I should also take adantage of the fact that this adaptor signature implementation works in the functional test framework to test bitcoin transactions signed with Schnorr signatures recovered from adaptor signatures. I can then modify my eltoo `claim` transactions to use a key-path spend, and the `refund` transactions be script-path spends. I would also like to help AJ Towns test his [new scheme](https://twitter.com/ajtowns/status/1442737473142943747) for poon-drya LN PTLCs.


