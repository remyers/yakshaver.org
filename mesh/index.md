---
title: "Mesh"
---

### TxTenna

Tasks:

 - [ ] Merge [PRs](https://github.com/MuleTools/txTenna/pulls) to upstream Samourai repo
 - [ ] Get updated version on the Google Play store
 - [ ] Better documentation on how to use TxTenna App
 - [ ] Better documentation on how to setup a TxTenna gateway and use with TxTenna App

Notes:

Usage
* MarketsByLili [twitter post](https://twitter.com/Marketsbylili/status/1413185925128069121) on how to use TxTenna.

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
 * [Electrs connection over SOCKS5 proxy](https://github.com/remyers/BDWallet/commits/mesh)

### Lightning over Email
- [ ] c-lightning plugin based socks proxy to intercept and send channel traffic as email
- [ ] c-lightning plugin based socks proxy to ingest and process channel traffic from email
- [ ] disable timeouts

Notes:

* [LNTenna](https://github.com/willcl-ark/lntenna-python/tree/master)
* Mesh Only Signal App with [SOCKS5 proxy server](https://github.com/remyers/Signal-Android/commit/ee018e3fcc991ed20eac85859b12604c3a2e6507)
* Look at [Bluewallet/rn-ldk](https://github.com/BlueWallet/rn-ldk)

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
* [KaiOS 3.0 overview](https://developer.kaiostech.com/docs/sfp-3.0/introduction/overview) - version 84 includes WebAssembly.
* [Nokia N1374 (TA-1374)](https://nokiamob.net/2021/09/30/nokia-n139dl-with-kaios-3-0-and-4g-support-gets-wi-fi-certified/) will run KaiOS 3.0 out of the box.

BDK
* Bitcoin Dev Kit [cli wallet](https://github.com/bitcoindevkit/bdk-cli)

Notes:
