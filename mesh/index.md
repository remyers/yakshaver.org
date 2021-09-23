---
title: "Mesh"
---

### Low Bandwidth Wallet

Tasks:

 - [ ] test `electrs`, branch: `cbor_replace_compiles` branch using `client2.py` test client
 - [ ] update `electrs` to use proposed compressed-block-header scheme
 - [ ] add support for compressed block headers to BDK electrs block source
 - [ ] test BDK cli client using electrs with compressed block headers
 
Notes:

Compressed Block Headers
* [how to test compressed block headers in electrs](https://gist.github.com/willcl-ark/89bd4731c6d074f5e98ac3332286926a)
  * first step is to get bitcoind and electrs running on Signet (or testnet, but Signet much smaller)
  * replace `json.dumps` with `cbor.dumps` in `client2.py`
* should use [electrs, branch: cbor_replace_compiles](https://github.com/remyers/electrs/tree/cbor_replace_compiles) electrs fork
* CBH [Proposal](https://github.com/willcl-ark/compressed-block-headers)
* [Mailing list discussion](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2020-May/017881.html)
* [current compressed block headers implementation](https://github.com/willcl-ark/compressed-block-headers/commits/modules)

Bitcoin Dev Kit (BDK)

### Lightning over Email
- [ ] c-lightning plugin based on lntenna-python to intercept and output channel traffic as text messages for email
- [ ] c-lightning plugin based on lntenna-python to ingest and process channel traffic from text messages from email
- [ ] disable timeouts

Notes:

* [LNTenna](https://github.com/willcl-ark/lntenna-python/tree/master)

### Lot49 App

- [ ] Add Settings Screen
- [ ] Add support for WiFi Direct
- [ ] BDK Layer 1 support using electrs
- [ ] LDK Layer 2 support

Notes:

Lot49
* [Lot49-android](https://github.com/remyers/lot49-android) source
* [Lot49 whitepaper](https://global-mesh-labs.gitbook.io/lot49/)
* [Lot49 simulation](https://github.com/remyers/lot49)

WiFi Direct and Nearby APIs
* ask about WiFi Direct and Nearby support in [LightSpot](https://play.google.com/store/apps/details?id=com.pseudozach.lightspot) by [pseudozach](https://github.com/pseudozach)

### Kaios Phone

- [ ] compile BDK to target KaiOS phone targeting asm.js
- [ ] test BDK cli wallet targeted for asm.js
- [ ] test on a KaiOS phone (eg. [Nokia 8110 4G](https://www.nokia.com/phones/en_int/nokia-8110-4g)]
- [ ] test simple KaiOS app wallet UI
- [ ] test simple KaiOS app wallet UI with BDK functionality

Notes:

KaiOS
* tutorial on [Programming Rust for KaiOS](http://ianrrees.github.io/2019/11/04/programming-for-kaios.html)

BDK
* Bitcoin Dev Kit [cli wallet](https://github.com/bitcoindevkit/bdk-cli)

Notes:
