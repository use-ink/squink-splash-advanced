# Squink Splash: Advanced

## Helpful Resources

* [ink! Documenation Portal](https://use.ink/)
* [ink! Examples](https://github.com/paritytech/ink/tree/master/examples)

### Cross-contract Calls

```
use ink::env::{
    DefaultEnvironment,
    call::{build_call, Selector, ExecutionInput},
};

let board: (u32, u32) = build_call::<DefaultEnvironment>()
     .call(game_addr)
     .exec_input(
         ExecutionInput::new(Selector::new(0xf10dee95_u32.to_be_bytes()))
     )
     .returns::<(u32, u32)>()
     .invoke();
```

The [documentation of `build_call`](https://paritytech.github.io/ink/ink_env/call/fn.build_call.html)
contains more details.

## `cargo-contract` Commands

### Registering a player

```bash
export CONTRACT=5E123… # your player contract address

cargo contract call\
    --url wss://contracts.theissen.io\
    --suri "$SURI"\
    --skip-dry-run\
    --gas 300000000000\
    --proof-size 512000\
    --contract "$GAME"\
    --manifest-path=../game/Cargo.toml\
    --message register_player\
    --args "$CONTRACT" \"playername\"\
    --skip-confirm
```

### Starting the game

```bash
export SURI="your twelve words"
export GAME=5E123… # your game contract address

cargo contract call\
    --url wss://rococo-contracts-rpc.polkadot.io\
    --suri "$SURI"\
    --contract $GAME\
    --manifest-path=../game/Cargo.toml\
    --message "start_game"\
    --skip-confirm
```

### Submitting a turn

```bash
export SURI="your twelve words"
export GAME=5E123… # your game contract address

cargo contract call\
    --url wss://rococo-contracts-rpc.polkadot.io\
    --suri "$SURI"\
    --contract $GAME\
    --manifest-path=../game/Cargo.toml\
    --message "submit_turn"\
    --skip-dry-run\
    --gas 300000000000\
    --proof-size 912000\
    --skip-confirm
```

### Reading information 

```bash
export SURI="your twelve words"
export GAME=5E123… # your game contract address

cargo contract call\
    --url wss://rococo-contracts-rpc.polkadot.io\
    --contract $GAME
    --message "gas_limit"
    --dry-run game-metadata.json 
```

