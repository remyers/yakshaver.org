---
title: "APO Vault"
subtitle: "Covenants For Dummies"
---

It has been [suggested](https://www.mail-archive.com/bitcoin-dev@lists.linuxfoundation.org/msg08075.html) that a form of [covenants](https://bitcoinops.org/en/topics/covenants/) could be possible using the proposed ANYPREVOUT sighash described in [BIP-118](https://github.com/bitcoin/bips/blob/master/bip-0118.mediawiki). 

I've created a brief [presentation](https://yakshaver.org/assets/presentations/APO_Vault_Covenants_v1.pdf) of how I think it would work, and begun to create some simple [tests](https://github.com/remyers/bitcoin/blob/covenant-anyprevout/test/functional/feature_apocovenant.py) using AJ Town's anyprevout branch of Bitcon to see if this might work.

I'd love to get feedback from the covenant / vault experts out there. Is this covenant scheme useful? How does it compare to existing covenant proposals like the CHECKTEMPLATEVERIFY [BIP-119](https://github.com/bitcoin/bips/blob/master/bip-0119.mediawiki#feature-redundancy)?

Kanzure's [2019 mailing list post](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2019-August/017229.html) describes vaults that do not require, but would benefit from, SIGHASH_NOINPUT. The Revault team [describes](https://github.com/revault/practical-revault/blob/master/revault.pdf) describes a similar scheme. To investigate the use of covenants with APO I built a vault system loosly based on the Kanzure/Revault vaults, but using the current APO branch from AJ Towns to implement them with covenants.

You can read more about covenants in the paper, [Bitcoin Covenants: Three Ways to Control the
Future](https://arxiv.org/pdf/2006.16714.pdf).

