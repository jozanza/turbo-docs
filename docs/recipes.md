## Deploying

`turbo` doesn't have a built-in commands for bundling/deploying your game yet.

However, you can follow these instructions to turn your Turbo game into a PWA:

&rarr; Template repo: https://github.com/jozanza/turbo-web-template

There's also a Codesandbox for those who want to see a live example:

&rarr; Codesandbox: https://codesandbox.io/p/devbox/bonk-runner-srn4pf

## Solana


When the `solana` feature is enabled, Turbo's `solana` module is made available. This module allows for you to read and write data to/from the Solana blockchain 🚀

> Turbo re-exports `solana_sdk` via `turbo::solana::solana_sdk`, allowing you use existing Solana development techniques you typically use when writing Solana programs (PDA derivation, transaction conconstruction, etc).

?> If you pull in an Anchor program as a dependency, there is also a useful `anchor` module in `turbo::solana::anchor` that provides utils for decoding Anchor account data and creating Anchor program transactions.

!> Note that `anchor-client` does not work in Turbo since their networking layers are incompatible. If it's in your game's dependency tree, ngmi.

### Getting Started

The setup requires 3 small steps:

#### 1. Update your project's `Cargo.toml`

```toml
# Enable the solana feature
turbo = { version = ">=0.3.8", package = "turbo-genesis-sdk", features = [
"solana",
] }

# If you add an anchor program as a dependency, you'll need this patch
[patch.crates-io]
cc = { git = "https://github.com/jozanza/cc-rs.git", branch = "wasm-patch" }
```

#### 2. Set your Solana RPC urls

In `src/lib.rs`, add or modify your `turbo::cfg!` with a `[solana]` section that says which Solana RPC urls to use. If you have other settings in the config, you can leave them as-is 👍🏽.

```rust
turbo::cfg!{r#"
    [solana]
    http-rpc-url = "http://localhost:8899"
    ws-rpc-url = "ws://localhost:8900"
"#}
```

#### 3. Set the user

When running `turbo`, you will need to specify the private key of the user via a `TURBO_SOL_USER` env var:

```bash
TURBO_SOL_USER=<insert base58 private key> turbo run -w path/to/my/project
```

?> You may also set a `TURBO_SOL_PAYER` private key as well if you wish for the payer to be different than the user.

### Get the User's Public Key

```rust
use turbo::solana;

let user_pubkey = solana::user_pubkey();
```

### Derive a PDA

```rust
use turbo::solana::solana_sdk;
use solana_sdk::pubkey::Pubkey;
use std::str::FromStr;

let program_id_str = "insert_id_here";
    let program_id: Pubkey = Pubkey::from_str(program_id_str)
    .expect("Error parsing program ID");

let (pda_pubkey, bump_seed) = Pubkey::find_program_address(
    &[...],
    &program_id,
);
```

### Submit a Transaction

#### Anchor

This approach is preferred when interacting with an Anchor program.

```rust
use turbo::solana::{anchor, solana_sdk};
use anchor::Program;
use solana_sdk::{instruction::AccountMeta, pubkey::Pubkey};
use my_anchor_program::instructions::DoSomething;
use std::str::FromStr;

let program_id_str = "insert_id_here";
let program_id: Pubkey = Pubkey::from_str(program_id_str)
    .expect("Error parsing ID");

let instruction_name = "name_of_anchor_instruction";
let accounts: Vec<AccountMeta> = vec![...];
let args = DoSomething { ... };
let tx_hash = Program::new(program_id)
    .instruction(instruction_name)
    .accounts(accounts)
    .args(args)
    .send();  
```

#### Solana SDK

This approach is preferred when interacting with a non-Anchor program.

```rust
use turbo::solana;
use solana::solana_sdk;
use solana_sdk::{
    instruction::{AccountMeta, Instruction},
    pubkey::Pubkey,
    transaction::Transaction
};
use std::str::FromStr;

 let program_id_str = "insert_id_here";
 let program_id: Pubkey = Pubkey::from_str(program_id_str)
 .expect("Error parsing program ID");

let accounts: Vec<AccountMeta> = vec![...];
let data = vec![...];
let instruction = Instruction {
    program_id,
    accounts,
    data
};
let tx = Transaction::new_with_payer(&[instruction], None);
let tx_hash = solana::sign_and_send_transaction(&tx);
```

### Fetch an Account

```rust
use turbo::solana;
use solana::{anchor, rpc, solana_sdk};
use solana_sdk::pubkey::Pubkey;

let pubkey = Pubkey::new(...);
let res = rpc::get_account(pubkey);
if !res.is_fetched() {
    // The account isn't fetched yet.
    // Here's where you can handle the loading state.
} else if let Some(ref account) = res.value {
    // The account is loaded.
    // Here is where you would deserialize its data.
} else {
    // The account loaded, but it has no data.
    // This typically means the account has been deleted.
}
```

### Deserialize Account Data

#### Borsh

Use this approach if one of the following is true:

- You are interacting with an Anchor program
- You are interacting with a non-Anchor program that borsh-encodes account data

```rust
use turbo::solana;
use solana::rpc::AccountInfo;
use my_anchor_program::MyAnchorAccount;

let account: AccountInfo = ...;
// You will need to skip the first 8 bytes when decoding an Anchor struct.
// Those bytes are reserved for a "discriminator" – basically, the struct's IDL schema identifier.
match MyAnchorAccount::deserialize(&mut account.data.get(8..).unwrap_or(&[])) {
    Ok(data) => {
        // Do stuff with deserialized data.
    }
    Err(err) => {
        // Uh oh deserialization error.
    }
}
```

?> When slicing off the discriminator, it's a good idea to check that the account data is >= 8. Slicing a smaller Vec will panic. If you are interacting with a non-Anchor program that borsh-encodes account data, don't skip the first 8 bytes.

