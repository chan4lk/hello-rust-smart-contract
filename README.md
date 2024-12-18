# Soroban Project

## Project Structure

This repository uses the recommended structure for a Soroban project:
```text
.
├── contracts
│   └── hello_world
│       ├── src
│       │   ├── lib.rs
│       │   └── test.rs
│       └── Cargo.toml
├── Cargo.toml
└── README.md
```

- New Soroban contracts can be put in `contracts`, each in their own directory. There is already a `hello_world` contract in there to get you started.
- If you initialized this project with any other example contracts via `--with-example`, those contracts will be in the `contracts` directory as well.
- Contracts should have their own `Cargo.toml` files that rely on the top-level `Cargo.toml` workspace for their dependencies.
- Frontend libraries can be added to the top-level directory as well. If you initialized this project with a frontend template via `--frontend-template` you will have those files already included.

## Configuring the CLI for Testnet

### Configure an Identity
When you deploy a smart contract to a network, you need to specify an identity that will be used to sign the transactions.

Let's configure an identity called alice. You can use any name you want, but it might be nice to have some named identities that you can use for testing, such as alice, bob, and carol. Notice that the account will be funded using Friendbot.

```bash
stellar keys generate --global alice --network testnet --fund
```
You can see the public key of alice with:

```bash
stellar keys address alice
```

## Deployment

To deploy the contract to the Stellar testnet, use the following command:

```bash
stellar contract deploy \
  --wasm target/wasm32-unknown-unknown/release/hello_world.wasm \
  --source alice \
  --network testnet
```

Make sure you have:
1. Built the contract using `cargo build --target wasm32-unknown-unknown --release`
2. Set up your Stellar account credentials
3. Have sufficient testnet funds in your account

## Invoking the Contract

After deployment, you can invoke the contract's `hello` function using:

```bash
stellar contract invoke \
  --id CACDYF3CYMJEJTIVFESQYZTN67GO2R5D5IUABTCUG3HXQSRXCSOROBAN \
  --source alice \
  --network testnet \
  -- \
  hello \
  --to RPC
```

Replace the contract ID with the one you received during deployment.