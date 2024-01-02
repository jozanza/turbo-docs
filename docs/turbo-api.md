# Turbo API

Turbo aims to be as simple as possible to help you learn and make 2D games fast.

## Overview

The API presented in this document is specific to Rust; however, Turbo is capable of running game code in any language that compiles to WebAssembly.

For creating full-featured Turbo games outside of the browser, refer to [this guide](https://gist.github.com/jozanza/24847baebc33e55ffe5e5a0e34570b64).

## Specs

- **Resolution:** 256x144
- **Spritesheet:** 256x1024 (up to 1024 16x16 sprites)
- **Colors:** 0x000000ff - 0xffffffff
- **FPS Range:** 1 to 60
- **Inputs:** Virtual Gamepad (aka keyboard)

## Sprites

turbo games store their sprites in individual image files stored in a folder named 'sprite' within the project directory.

## Colors

For all methods that accept a color argument, it must be passed in RGBA format.

Examples:

- `0xffffffff` is white
- `0x000000ff` is black
- `0x0000ffff` is blue
- `0x00ff00ff` is green
- `0xff0000ff` is red

At the moment, Turbo enforces a limitation -- colors must be fully opaque or transparent. Semi-transparent colors will be rendered at full opacity.

## Getting User Input

In order to make games interactive, you'll need to react to user input. The primary method of user input is the gamepad.

Turbo's "gamepad" is keyboard-only at the moment. The controls are as follows:
- **Up**    : W or ArrowUp
- **Down**  : S or ArrowDown
- **Left**  : A or ArrowLeft
- **Right** : D or ArrowRight
- **A**     : Z
- **B**     : X
- **X**     : C
- **Y**     : V
- **Start** : Spacebar
- **Select**: Enter/Return

```rust
// Gets the gamepad state of the player
// The player is 0-indexed so P1 is 0, P2 is 1, etc
// Only one player is supported at the moment
fn gamepad(player: u32) -> Gamepad;

struct Gamepad {
    pub up: Button,
    pub down: Button,
    pub left: Button,
    pub right: Button,
    pub a: Button,
    pub b: Button,
    pub x: Button,
    pub y: Button,
    pub start: Button,
    pub select: Button,
}

enum Button {
    Released = 0,
    JustPressed = 1,
    Pressed = 2,
    JustReleased = 3,
}
