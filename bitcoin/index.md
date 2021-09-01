---
title: "Bitcoin"
---

## Taproot eltoo

I have compiled below a list of tasks and related notes that might be helpful for anyone that wants to learn more about the status and implementation details of BIP-118 (Anyprevout) and eltoo. Please get in touch if you have any questions or can help work on one of these tasks. Open an issue or PR if you think of additional tasks, notes or other changes for this page.

### Anyprevout

Tasks:
 - [X] Basic unit tests, [legacy](https://github.com/ajtowns/bitcoin/blob/57cb1249a20d2e09952040693eb62d04fe1f1399/src/test/sighash_tests.cpp#L247) and [taproot](https://github.com/ajtowns/bitcoin/blob/57cb1249a20d2e09952040693eb62d04fe1f1399/src/test/sighash_tests.cpp#L404)
 - [ ] Review and comment on [BIP-118](https://github.com/bitcoin/bips/blob/master/bip-0118.mediawiki)
 - [ ] run a signet node
 - [ ] run a signet faucet

Notes:

* BIP
* Signet

Faucet idea: can signet miners create a single signature and prototype transaction that can spend any coinbase output using APOAS? As long as any change goes back to the same address then the signature can be used again by others.

### eltoo

Tasks:

 - [X] [eltoo](https://blockstream.com/eltoo.pdf): Basic transaction tests, [simulate_eltoo.py: test_tapscript_eltoo()](https://github.com/remyers/bitcoin/blob/eltoo-anyprevout/test/functional/simulate_eltoo.py#L1623)
 - [X] [Blog post](https://yakshaver.org/2021/07/26/first.html) about basic transaction tests
 - [ ] [PTLCs](https://suredbits.com/schnorr-applications-scriptless-scripts) transaction tests
 - [ ] [Layered Commitments](https://lists.linuxfoundation.org/pipermail/lightning-dev/2020-January/002448.html) transaction tests
 - [ ] [Multiparty Channels](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6124062/) transaction tests
 - [ ] update simulation with APO transactions
 - [ ] add PTLCs to simulation
 - [ ] add Layered Commitments to simulation
 - [ ] add Multiparty Channels to simulation
 - [ ] add optimize fees to simulation (eg. [fee bumping](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2021-May/019031.html), GROUP sighash)
 - [ ] add MuSig2 to simulation

Notes:

Segwit eltoo
* [functional simulation](https://github.com/remyers/bitcoin/blob/anyprevout/test/functional/simulate_eltoo.py)

Taproot eltoo
* [sketch by AJ Towns](https://lists.linuxfoundation.org/pipermail/lightning-dev/2019-May/001996.html)

### PTLCs

Tasks:

 - [ ] test/update [adapter-py](https://github.com/LeoComandini/adaptor-py) for use in the functional test framework
 - [ ] create an adapter signatures test script to demonstrate how to exchange PTLC nonces
 - [ ] update transactions so `claim_tx` uses key spend with adapter signature and `refund_tx` uses script spend with timeout
 - [ ] create a new test derived from the [`test_tapscript_eltoo`](https://github.com/remyers/bitcoin/blob/37a7490dc3b2128c0f7e34a463531f1123682d42/test/functional/simulate_eltoo.py#L1623) script that replaces HTLCs with PTLCs

Notes:

* Privacy
* Smaller Transactions
* No Stuck Payments
* Adapter Signatures: ["How DLC-Bets Work?"](https://satoshispritz.com/presentazioni/210318-how_dlc-bets_work.pdf) by  Leonardo Comandini + [adapter-py](https://github.com/LeoComandini/adaptor-py), a (toy) pure python adapter signatures implementation for ECDSA and Schnorr
* Workflow and scripts: Bitcoin Optech ["The PTLC solution"](https://bitcoinops.org/en/preparing-for-taproot/#ptlcs) and ["PTLCs Over LN"](https://bitcoinops.org/en/newsletters/2021/09/01/#ptlcs-over-ln) by [ZmnSCPxj](https://zmnscpxj.github.io/about.html)

### Layered Commitments

Notes:

* CLTV deltas

### Multiparty Channels

Notes:

* Channel Factories

  shared liquidity, overlapping routing nodes, can update nested channels faster, can settle MP channel and continue to use nested channels

* Generalized Off-chain Transactions
  
  cache and aggregateve onchain transactions off-chain, timeouts must account for settlement of multiparty channel 
