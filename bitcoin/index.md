---
title: "Bitcoin"
---

## Taproot eltoo

I have compiled below a list of tasks and related notes that might be helpful for anyone that wants to learn more about the status and implementation details of BIP-118 (Anyprevout) and eltoo. Please get in touch if you have any questions or can help work on one of these tasks. Open an issue or PR if you think of additional tasks, notes or other changes for this page.

### Anyprevout (BIP-118)

Tasks:
 - [X] Basic unit tests, [legacy](https://github.com/ajtowns/bitcoin/blob/57cb1249a20d2e09952040693eb62d04fe1f1399/src/test/sighash_tests.cpp#L247) and [taproot](https://github.com/ajtowns/bitcoin/blob/57cb1249a20d2e09952040693eb62d04fe1f1399/src/test/sighash_tests.cpp#L404)
 - [ ] Review and comment on [BIP-118](https://github.com/bitcoin/bips/blob/master/bip-0118.mediawiki)
 - [ ] run a signet node
 - [ ] run a signet faucet

Notes:

* BIP
* Signet

Faucet idea: can signet miners create a single signature and prototype transaction that can spend any coinbase output using APOAS? As long as any change goes back to the same address then the signature can be used again by others.

### Replace-by-fee (RBF) changes

Tasks:

- [ ] Review and comment on instagibbs [BIP 125 nVersion=3 proposal](https://gist.github.com/instagibbs/b3095752d6289ab52166c04df55c1c19) for smart contracts like eltoo

Notes:

 - instagibbs [full_rbf branch](https://github.com/instagibbs/bitcoin/tree/full_rbf_2022)

### eltoo

Tasks:

 - [X] [eltoo](https://blockstream.com/eltoo.pdf): Basic transaction tests, [simulate_eltoo.py: test_tapscript_eltoo()](https://github.com/remyers/bitcoin/blob/eltoo-anyprevout/test/functional/simulate_eltoo.py#L1623)
 - [X] [Blog post](https://yakshaver.org/2021/07/26/first.html) about basic transaction tests
 - [ ] review/comment on [eltoo BOLT draft](https://github.com/instagibbs/bolts/tree/eltoo_draft)
 - [ ] Blog post about tx fees with eltoo using package relay and instagibbs nVersion=3 proposal
 - [ ] [PTLCs](https://suredbits.com/payment-points-part-1/) transaction tests
 - [ ] [Layered Commitments](https://lists.linuxfoundation.org/pipermail/lightning-dev/2020-January/002448.html) transaction tests
 - [ ] [Multiparty Channels](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6124062/) transaction tests
 - [ ] update simulation with APO transactions
 - [ ] add PTLCs to simulation
 - [ ] add Layered Commitments to simulation
 - [ ] add Multiparty Channels to simulation
 - [ ] add optimize fees to simulation (eg. [fee bumping](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-May/019031.html), GROUP sighash)
 - [ ] add MuSig2 to simulation

Notes:
* [##eltoo](https://matrix.to/#/##eltoo:libera.chat) discussion on matrix/irc

Segwit eltoo
* [functional simulation](https://github.com/remyers/bitcoin/blob/anyprevout/test/functional/simulate_eltoo.py)

Taproot eltoo
* [sketch by AJ Towns](https://lists.linuxfoundation.org/pipermail/lightning-dev/2019-May/001996.html)

### PTLCs

Tasks:

 - [X] Schnorr X-only pubkey [adaptor signature](https://github.com/bitcoin/bips/blob/master/bip-0340.mediawiki#Adaptor_Signatures) support in the functional test framework
 - [ ] create an adapter signatures test script to demonstrate how to exchange PTLC nonces
 - [ ] update transactions so `claim_tx` uses key spend with adapter signature and `refund_tx` uses script spend with timeout
 - [ ] create a new test derived from the [`test_tapscript_eltoo`](https://github.com/remyers/bitcoin/blob/37a7490dc3b2128c0f7e34a463531f1123682d42/test/functional/simulate_eltoo.py#L1623) script that replaces HTLCs with PTLCs

Notes:

* Privacy
* Smaller Transactions
* No Stuck Payments
* Adaptor Signatures: ["How DLC-Bets Work?"](https://satoshispritz.com/presentazioni/210318-how_dlc-bets_work.pdf) by  Leonardo Comandini + [adaptor-py](https://github.com/LeoComandini/adaptor-py), a (toy) pure python adapter signatures implementation for ECDSA and Schnorr
* Workflow and scripts: Bitcoin Optech ["The PTLC solution"](https://bitcoinops.org/en/preparing-for-taproot/#ptlcs) and ["PTLCs Over LN"](https://bitcoinops.org/en/newsletters/2021/09/01/#ptlcs-over-ln) by [ZmnSCPxj](https://zmnscpxj.github.io/about.html)
* Rust Schnorr adaptor signature implementation module [schnorr_fun::adaptor](https://docs.rs/schnorr_fun/0.6.2/schnorr_fun/adaptor/index.html) h/t [LLFOURN](https://twitter.com/LLFOURN)
* Python Schnorr adaptor signature using x-only pubkeys and SECP256K1 in functional test framework file [key.py](https://github.com/elichai/bitcoin/blob/b8252af349143346b5881ddb4c627e460557c3ae/test/functional/test_framework/key.py#L636) h/t [Elichai2](https://twitter.com/Elichai2)
* ZmnSCPxj post ["[Lightning-dev] Lightning in a Taproot future"](https://lists.linuxfoundation.org/pipermail/lightning-dev/2019-December/002375.html)
* AJTowns [feature_ln_ptlc.py](https://github.com/ajtowns/bitcoin/blob/202109-ptlc-lnpenalty/test/functional/feature_ln_ptlc.py)

### Layered Commitments

Notes:

* CLTV deltas
* Subject to [Siphon attacks](https://github.com/lightningnetwork/lightning-rfc/issues/845#issuecomment-779285607) if remote party signature does not commit amount of local party's HTLC outputs?
* lnpenalty taproot [diagram](https://gist.github.com/ajtowns/12f58fa8a4dc9f136ed04ca2584816a2/)
* Needs SIGHASH/OP_GROUP to regather update outputs to newer state outputs

### Multiparty Channels

Notes:

* Channel Factories

  shared liquidity, overlapping routing nodes, can update nested channels faster, can settle MP channel and continue to use nested channels

* Generalized Off-chain Transactions
  
  cache and aggregateve onchain transactions off-chain, timeouts must account for settlement of multiparty channel

### Anyprevout Covenants

Tasks:
 - [ ] Finish [covenant vault example tests](https://github.com/remyers/bitcoin/blob/covenant-anyprevout/test/functional/feature_apocovenant.py) in functional test framework off of APO branch 
 - [X] [Blog post](https://yakshaver.org/2021/11/18/covenants.html) about how to use APO based covenants for a simple vault system
 - [ ] Blog post on non-interactive Lightning channel open
 - [ ] Blog post on congestion control (delayed payments)
 - [ ] Blog post on pre-planned vesting transactions

Notes:
  * AJ Towns [bitcoin-dev post](https://www.mail-archive.com/bitcoin-dev@lists.linuxfoundation.org/msg08075.html) (June 2019)
  * instead of script ```<sig> <P> CHECKSIG``` can we use ```<sig> 0x01 CHECKSIG``` to use internal pubkey and save 32 bytes?
  * Kanzure’s [2019 mailing list post](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2019-August/017229.html)
  * Revault [vault scheme](https://github.com/revault/practical-revault/blob/master/revault.pdf)
  * [Bitcoin Covenants: Three Ways to Control the Future](https://arxiv.org/pdf/2006.16714.pdf) (2020)
  * Twitter [thread](https://twitter.com/remyers_/status/1461371410023133184) about APO Vault scheme

### GROUP
Tasks:
 - [ ] Blog post summarizing state of OP_GROUP discussions, including OP vs. SIGHASH, lexical ordering ([BIP-69](https://en.bitcoin.it/wiki/BIP_0069)) vs. tx cut-through/incremental batching

### DLCs
Tasks:
 - [ ] Blog post discussing how APO can enable DLCs over Lightning

 Notes:
 * [\[Lightning-dev\] Claiming the 1BTC Strike/HRF "Stable Channel" bounty](https://www.mail-archive.com/lightning-dev@lists.linuxfoundation.org/msg02690.html)
 * [How to claim the 1BTC “Stable Channel” Bounty from HRF and Strike](https://suredbits.com/how-to-claim-the-1btc-stable-channel-bounty-from-hrf-and-strike/)
 * [OP_CTV and DLCs](https://gist.github.com/LLFourn/aa1327117f2c8cc7c2ac3fc9d0c4a9b9)