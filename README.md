# TPM-Crypt

A secure file encryption tool that uses TPM 2.0 hardware for key protection
combined with AES-256 for file encryption.

## Use Case

I use this script to store API keys. If my machine failed and the keys were
lost that wouldn't be a big deal - I could just create new ones.
The primary motivation is to avoid accidentally leaking these keys by
committing them to a Git repository.

## Features

- Hardware-backed key protection using TPM 2.0
- AES-256-CBC encryption for file contents
- Streaming support (reads from stdin, writes to stdout)

## Prerequisites

- TPM 2.0 hardware
- tpm2-tools
- openssl
- bash

## Installation

Clone this repository

## Usage

Encrypt a file (reads from stdin):
```sh
./tpm-crypt encrypt output.enc < input.txt
```

Decrypt a file (writes to stdout):
```sh
./tpm-crypt decrypt input.enc > output.txt
```

## How it Works

1. The script maintains TPM-protected keys in `~/.config/tpm-crypto/`
2. For encryption:
   - Generates a random AES-256 key
   - Encrypts the AES key using TPM RSA key
   - Encrypts the file content using AES-256-CBC
   - Combines the encrypted key and file into one output
3. For decryption:
   - Splits the input into encrypted key and file portions
   - Decrypts the AES key using TPM
   - Decrypts the file content using the recovered AES key

## Security

- Keys never leave the TPM hardware in unencrypted form
- Uses strong encryption (AES-256-CBC) for file contents
- Implements PBKDF2 key derivation
- TPM binding ensures keys can only be used on the original machine

## License

MIT License

(C) Copyright Corrected Cloud Pty Ltd 2025

