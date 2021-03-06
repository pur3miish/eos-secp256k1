![eos ecc logo](https://raw.githubusercontent.com/pur3miish/eos-ecc/main/static/eos-ecc.svg)

# EOS-ECC

[![NPM Package](https://img.shields.io/npm/v/eos-ecc.svg)](https://www.npmjs.org/package/eos-ecc) [![CI status](https://github.com/pur3miish/eos-ecc/workflows/CI/badge.svg)](https://github.com/pur3miish/eos-ecc/actions) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/pur3miish/eos-ecc/blob/main/LICENSE)

A [universal JavaScript](https://en.wikipedia.org/wiki/Isomorphic_JavaScript) package for [elliptic curve cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography), operations for the EOSIO blockchain.

**Features**

- Signature generation & validation
- Public key from private key
- Generate cryptographic random key pair

Built from [isomorphic-secp256k1](https://github.com/pur3miish/isomorphic-secp256k1) which has developed strategies to mitigate various [side channel attacks](https://en.wikipedia.org/wiki/Side-channel_attack).

# Support

We support all browsers that can handle [WebAssembly](https://caniuse.com/wasm).

- Node.js `^12.20.1 || >= 13.2`
- Browser `defaults, no IE 11`

**NB**
For testing purposes you will need [webcrypto](https://nodejs.org/api/webcrypto.html#webcrypto_class_subtlecrypto) a Node.js v15 feature.

# API

## Table of contents

- [function generate_eos_signature](#function-generate_eos_signature)
- [function new_eos_keys](#function-new_eos_keys)
- [function public_key_from_private](#function-public_key_from_private)
- [function verify_eos_signature](#function-verify_eos_signature)
- [type KeyPair](#type-keypair)

## function generate_eos_signature

Generate an EOS encoded signature.

| Parameter             | Type   | Description                              |
| :-------------------- | :----- | :--------------------------------------- |
| `arg`                 | object | Argument.                                |
| `arg.hex`             | string | Message digest sha256 to sign.           |
| `arg.wif_private_key` | string | An EOS wallet import format private key. |

**Returns:** string — EOS encoded signature.

### Examples

_Ways to `import`._

> ```js
> import { generate_eos_signature } from 'eos-ecc'
> ```
>
> ```js
> import generate_eos_signature from 'eos-ecc/public/generate_eos_signature.js'
> ```

_Ways to `require`._

> ```js
> const { generate_eos_signature } = require('eos-ecc')
> ```
>
> ```js
> const generate_eos_signature = require('eos-ecc/public/generate_eos_signature.js')
> ```

_Usage of `generate_eos_signature`._

> ```js
> import crypto from 'crypto'
>
> const message = 'hello'
> const hex = new Uint8Array(
>   crypto.createHash('sha256').update(message).digest()
> )
> generate_eos_signature({
>   hex,
>   wif_private_key: '5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3'
> }).then(console.log)
> ```
>
> The logged output will be SIG_K1_JxMN(…)NJ.

---

## function new_eos_keys

Generate a new cryptographically random EOS key pair.

**Returns:** [KeyPair](#type-keypair) — Key pair.

### Examples

_Ways to `import`._

> ```js
> import { new_eos_keys } from 'eos-ecc'
> ```
>
> ```js
> import new_eos_keys from 'eos-ecc/public/new_eos_keys.js'
> ```

_Ways to `require`._

> ```js
> const { new_eos_keys } = require('eos-ecc')
> ```
>
> ```js
> const new_eos_keys = require('eos-ecc/public/new_eos_keys.js')
> ```

_Usage `new_eos_keys`._

> ```js
> new_eos_keys().then(console.log)
> ```
>
> The logged output will be an object containing EOS wif public & private keys.

---

## function public_key_from_private

Convert an EOS WIF private key to a WIF public key.

| Parameter         | Type   | Description                   |
| :---------------- | :----- | :---------------------------- |
| `wif_private_key` | string | EOS wallet import format key. |

**Returns:** string — EOS wallet import format public key.

### Examples

_Ways to `import`._

> ```js
> import { public_key_from_private } from 'eos-ecc'
> ```
>
> ```js
> import public_key_from_private from 'eos-ecc/public/public_key_from_private.js'
> ```

_Ways to `require`._

> ```js
> const { public_key_from_private } = require('eos-ecc')
> ```
>
> ```js
> const public_key_from_private = require('eos-ecc/public/public_key_from_private.js')
> ```

_Usage `public_key_from_private`._

> ```js
> public_key_from_private(
>   '5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3'
> ).then(console.log)
> ```
>
> The logged output will be EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV.

---

## function verify_eos_signature

Validates and EOS signature from the message digest and public key.

| Parameter            | Type   | Description                  |
| :------------------- | :----- | :--------------------------- |
| `arg`                | object | Argument.                    |
| `arg.wif_public_key` | string | EOS public key.              |
| `arg.signature`      | string | EOS encoded signature.       |
| `arg.hash`           | string | The `sha256` message digest. |

**Returns:** boolena — Will be `true` & `false` for valid & invalid signatures respectively.

### Examples

_Ways to `import`._

> ```js
> import { verify_eos_signature } from 'eos-ecc'
> ```
>
> ```js
> import verify_eos_signature from 'eos-ecc/public/verify_eos_signature.js'
> ```

_Ways to `require`._

> ```js
> const { verify_eos_signature } = require('eos-ecc')
> ```
>
> ```js
> const verify_eos_signature = require('eos-ecc/public/verify_eos_signature.js')
> ```

_Usage `verify_eos_signature`._

> ```js
> const signature = 'SIG_K1_JxMNpqjt…yNJ'
> const wif_public_key = 'EOS6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV'
> const hash = new Uint8Array(
>   crypto.createHash('sha256').update('hello').digest()
> )
>
> verify_eos_signature({ wif_public_key, signature, hash })
> ```
>
> The logged output will return `true`.

---

## type KeyPair

An EOS wallet import formatted (WIF) public & private key pair.

| Property      | Type   | Description          |
| :------------ | :----- | :------------------- |
| `public_key`  | string | EOS WIF public key.  |
| `private_key` | string | EOS WIF private key. |
