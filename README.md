# A wrapper on node-gpg with error handling for bad passphrase and wrong secret key
[![npm][npm-image]][npm-url]
[![downloads][downloads-image]][downloads-url]


[npm-image]: https://img.shields.io/npm/v/gpg.svg?style=flat
[npm-url]: https://www.npmjs.com/package/gpg-with-err-handling

[downloads-image]: https://img.shields.io/npm/dm/gpg.svg?style=flat
[downloads-url]: https://www.npmjs.com/package/gpg-with-err-handling

This module is a wrapper around `Node-GPG` for use within Node. It takes care of spawning `gpg`, passing it
the correct arguments, and piping input to stdin. It can also pipe input in from files and output out to files.
It also helps you identified if the passphrase or secret key is wrong.

Use Node-GPG if you are considering calling `gpg` directly from your application.

## Requirements

In order to use gpg-with-err-handling, you'll need to have the `gpg` binary in your $PATH.

## Installation

    npm install gpg-with-err-handling

## Usage

Gpg-With-Err-Handling supports both direct calls to GPG with string arguments, and streaming calls for piping input and output
from/to files.
```
    gpg.importKey(privateKey, [], (err, success) => {
      if(!err){
          const args = [
            '--pinentry-mode',
            'loopback',
            '--passphrase',
            passphrase,
          ];

        gpg.callStreaming(<encrypted file stream>, <output file name>, args, async (error, data) => {
            if(success) {
              // post success logic
            }
        });
      }
    });
```

See [the source](lib/gpg.js) for more details.

If a function you need is not implemented, you can call gpg directly with arguments of your choice by
calling `gpg.call(stdinStr, argsArray, cb)`, or `gpg.callStreaming(inputFileName, outputFileName, argsArray, cb)`.

## Notes

Existing implementations of PGP in Javascript are blocking and unfeasibly slow for server use.
In casual testing, encrypting a simple 400-character email to an El-Gamal key took upwards of 11 seconds using
[openpgpjs](https://github.com/openpgpjs/openpgpjs) and 14 seconds with [kbpgp](https://github.com/keybase/kbpgp),
but takes less than 0.1 seconds with `gpg` directly.

 
## Contributors

The following are the major contributors of `gpg-with-err-handling` (in no specific order).

  * Vivek Mehra ([vivmehra](http://github.com/vivmehra))
  * Hemant Singh [singhkumarhemant](https://github.com/singhkumarhemant)