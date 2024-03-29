Turbo aims to be as simple as possible to help you learn and make 2D games fast.
The following section provides an overview of [Turbo's Rust SDK](https://docs.rs/turbo-genesis-sdk/).

## Gameplay Controls

Turbo provides convenient ways to handle gamepad and mouse input from players, allowing you to make your games interactive.

Both gamepad and mouse controls use the `Button` enum to represent the input state of their buttons:

- `Button::JustPressed` - The button was just pressed this frame.
- `Button::Pressed` - The button has been held for more than one frame.
- `Button::JustReleased` - The button was just released this frame.
- `Button::Released` - The button has been released for more than one frame.

By default, all buttons are in the `Released` state when no input is detected.

### Gamepad

Turbo's "gamepad" is similar to a SNES controller and maps up to 2 player's gamepad input to the keyboard:

| Gamepad | Keyboard (P1)       | Keyboard (P2)       | 
| ------- | ------------------- | ------------------- |
| Up      | `W` or `UpArrow`    | `I`                 |
| Down    | `A` or `DownArrow`  | `J`                 |
| Left    | `S` or `LeftArrow`  | `K`                 |
| Right   | `D` or `RightArrow` | `L`                 |
| A       | `Z`                 | `M`                 |
| B       | `X`                 | `,`                 |
| X       | `C`                 | `.`                 |
| Y       | `V`                 | `/`                 |
| Start   | `Space`             | `[`                 |
| Select  | `Enter`             | `]`                 |

?> Turbo will support actual gamepad input in a future update.

To retrieve the gamepad state of a player, use the `gamepad` function. The player is 0-indexed, so P1 is 0, P2 is 1, etc.

```rust
// Get the gamepad state for player 1
let p1_gamepad = gamepad(0);

// Get the gamepad state for player 2
let p2_gamepad = gamepad(1);
```

To check the button states for players, utilize the following methods after obtaining the gamepad state using the `gamepad` function:

```rust
// Get the gamepad state for player 1
let gp = gamepad(0);

// Checks if the A button is JustPressed
if gp.a.just_pressed() {
    // Handle JustPressed state
}

// Checks if the A button is JustPressed or Pressed
if gp.a.pressed() {
    // Handle JustPressed and Pressed states
}

// Checks if the A button is JustReleased
if gp.a.just_released() {
    // Handle JustReleased state
}

// Checks if the A button is JustReleased or Released
if gp.a.released() {
    // Handle JustReleased and Released states
}
```

You can also match on the `Button` enum directly:

```rust
// Get the gamepad state for player 1
let gp = gamepad(0);

// Check the input state of the Start button
match gp.start {
    Button::JustPressed => {
        // Handle the JustPressed state
    },
    Button::Pressed => {
        // Handle the Pressed state
    },
    Button::JustReleased => {
        // Handle the JustReleased state
    },
    Button::JustReleased => {
        // Handle the Released state
    },
}
```

### Mouse

?> In Turbo, mouse input automatically converts to touch input on mobile devices.

To retrieve the mouse state of a player, use the `mouse` function. The player is 0-indexed, so P1 is 0, P2 is 1, etc. Right now, only P1 is supported.

```rust
// Get the mouse state for player 1
let p1_mouse = mouse(0);
```

The mouse input is similar to a gamepad except there are only two buttons – left and right.

```rust
// Get the mouse state for player 1
let m = mouse(0);

// Checks if the left mouse button is JustPressed (similar to a click)
if m.left.just_pressed() {
    // Handle JustPressed state
}

// Checks if the left mouse button is JustPressed or Pressed
if m.left.pressed() {
    // Handle JustPressed and Pressed states
}

// Checks if the left mouse button is JustReleased
if m.left.just_released() {
    // Handle JustReleased
}

// Checks if the left mouse button is JustReleased or Released
if m.left.released() {
    // Handle JustReleased and Released states
}
```

The mouse's position is also readily available. You can draw a small circular cursor at the mouse's position using the following snippet:

```rust
// Get the mouse state for player 1
let m = mouse(0);

// Get the mouse's x and y positions
let [mx, my] = m.position;

// Draw a circular cursor
circ!(d = 8, x = mx - 4, y = my - 4, fill = 0xff00ffff);
```

Mouse input is great for small games. Here's an example:

![mousepad](/_media/mousepad.gif)

Check out the source code [here](https://gist.github.com/tajerdev/4a18cf63972b5fec9b3de3a4261a1da2)

## Drawing Sprites

### Setup

Adding sprites to your Turbo game is a straightforward process. Follow these simple steps to integrate sprites into your Turbo game:

1. **Create a `sprites` Folder:**
 - Inside your project directory, create a folder named `sprites`. This folder will contain all your game sprites.

2. **Put Images in the Folder:**
- Place your sprite images (e.g., PNG or JPEG files) inside the `sprites` folder. These images will be the visual elements of your game.

Now you're ready to draw some sprites!

### Static Sprites

To use a sprite in your game, use the `sprite!` macro. This macro takes various parameters, such as the sprite name, position (`x` and `y`), frame rate (`fps`), and degrees of rotation:

```rust
// Display a sprite named "goblin" near the center of the screen with 180 degrees of rotation
sprite!("goblin", x = 120, y = 50, deg = 180);
```

- The first parameter (`"goblin"`) is the filename of your sprite excluding the file extension.
- Adjust the `x` and `y` parameters to position the sprite where you want it.
- Adjust the `deg` parameter to control the sprite's degrees of rotation.

?> All labelled parameters are optional and may be omitted. By default, their values are `0`.

And with that, we have ourselves an upside-down goblin:

![Goblin Sprite Screenshot](/_media/goblin_sprite_screenshot.png)

### Animated Sprites

Turbo treats sprites with a landscape aspect ratio like a "film strip". So a sprite like this...

![Doge animation strip](/_media/doge.png)

...will be rendered like this:

![Doge Sprite Screenshot](/_media/doge_animation_screencap.gif)

```rust
// Display a sprite named "doge" at specified position with a fast frame rate.
sprite!("doge", x = 112, y = 50, fps = fps::FAST);
```

Turbo automatically exports the following framerate constants from its canvas module:

- `fps::REALLY_SLOW`: Represents a really slow frame rate.
- `fps::SLOW` Represents a slow frame rate.
- `fps::MEDIUM`: Represents a medium frame rate.
- `fps::FAST`: Represents a fast frame rate.
- `fps::SUPER_FAST`: Represents a super fast frame rate.

?> You can also manually adjust frame rates by specifying values between 1 and 60. This flexibility allows you to precisely control the speed of animations in your game.

## Colors

For all methods that accept a color argument, it must be passed in RGBA format. For example:

- `0xffffffff` is white
- `0x000000ff` is black
- `0x0000ffff` is blue
- `0xffff00ff` is yellow
- `0xff0000ff` is red
- `0x00ff00ff` is green
- `0x00000000` is transparent

?> If you're familiar with CSS, it's basically the same with 2 differences:<br />1. Replace the leading `#` of your hex with `0x`<br/>2. Since it's RGBA instead of RGB, we need 8 chars instead of 6. The extra 2 chars at the end represent the color's alpha channel (the "A" in RGBA) -- typically `ff` to make your color fully opaque.

## Drawing Text

You can use the `text!` macro to draw ASCII characters in Turbo:

```rust
text!("Hello, world!");
```

![Hello, World Screenshot](/_media/hello_world_screenshot.png)

Here's a more advanced example using more parameters:

```rust
// Display customized text with specified position, color, and font size.
text!("Greetings, earthlings >:3", x = 30, y = 40, color = 0x00ff00ff, font = Font::S);
```

![Greetings, Earthlings Screenshot](/_media/greetings_earthlings_screenshot.png)


The `turbo::canvas` module exports a `Font` enum that provides different size and styling options for rendering text in Turbo. The available font variants are:

- `Font::S`: Small-sized font (5x5 characters).
- `Font::M`: Medium-sized font (5x8 characters).
- `Font::L`: Large-sized font (8x8 characters).

## Drawing Circles

To draw a circle, use the `circ!` macro:

```rust 
// Draw a filled circle at specified position with specified diameter and color.
circ!(d = 8, x = 128, y = 128, fill = 0x000000ff);
```

![Circle Screenshot](/_media/circle_screenshot.png)

## Drawing Rectangles

For basic rectangles, we have the `rect!` macro:

```rust
// Draw a filled rectangle at specified position with specified width, height, and color.
rect!(w = 20, h = 40, x = 70, y = 70, fill = 0x0000ffff);
```

![Rect Screenshot](/_media/rect_screenshot.png)

For more advanced rectangles with verbose options, we have the `rectv!` macro:

```rust
rectv!(
    x = 112,
    y = 56,
    w = 32,
    h = 32,
    fill = 0x0000ffff,
    rotation_deg = 45,
    rotation_origin = (0, 0),
    border = Border {
        radius: 4,
        size: 2,
        color: 0xffffffff,
    }
);
```

![Rectv Screenshot](/_media/rectv_screenshot.png)

?> The `border` parameter is currently experimental.

## Background Color

The `clear` function in Turbo allows you to set the background color of the canvas. This function takes a single argument, `color`, which represents the color to be used for clearing the canvas.

```rust
// Set the background color to white (0xffffffff)
clear(0xffffffff);
```

?> By default, the screen will clear to black (0x000000ff) each frame if `clear` is not called.

## Configuration

Turbo allows you to configure various settings for your game. Use the `turbo::cfg!` macro to define your game's configuration in TOML format:

```rust
turbo::cfg! {r#"
    name = "Game name"
    version = "1.0.0"
    author = "game author"
    description = "Game description"
    [settings]
    resolution = [256, 144]
    [solana]
    http-rpc-url = "http://127.0.0.1:8899"
    ws-rpc-url = "ws://127.0.0.1:8900"
"#}
```

?> The Solana configuration is required for those who want to interact with Solana in their games. Otherwise, the remaining config fields are optional.

## Game State

Turbo provides a straightforward way to initialize the standard game state using the `turbo::init!` block. This block is essential for defining and setting up the game state before entering the main game loop:

```rust
turbo::init! {
    // Define the GameState struct.
    struct GameState {
        screen: enum Screen {
            Title,
            Level,
        },
        x_position: i32,
        y_position: i32,
    } = {
        // Set the struct's initial value.
        Self {
            screen: Screen::Title,
            x_position: 30,
            y_position: 40,
        }
    }
}
```

Two main things to note about `turbo::init!`:

1. For convenience, it allows nested struct and enum definitions via the [structstruck](https://docs.rs/structstruck/latest/structstruck/) crate.
2. It will derive the following traits for type each defined in the macro: `BorshSerialize`, `BorshDeserialize`, `PartialEq`, `Debug`, and `Clone`.

?> In development, you can reset the state of the game to its initial state anytime by using a simple keyboard shortcut `Cmd+R` on MacOS/Linux and `Ctrl+R` on Windows.

### Saving Game State

The `save()` function is responsible for storing and saving the current state of the game. It captures essential information needed for the game to resume from the same point after a restart or reload.

```rust
// Load the game state at the beginning of the frame
let mut state = GameState::load();

// Example usage of the save() function
state.save();  // Save game state for the next frame
```
This function ensures that the game state is persisted, allowing for a seamless transition between frames and preserving the player's progress.

## Resolution

In Turbo, you may access the canvas's dimension via the `resolution()` function. This function returns an `[u32; 2]` representing the game canvas's dimensions. This information can be used to create adaptive layouts or even factor into gameplay.

To demonstrate, in the following example, when `state.circle_y` goes out of the top of the screen, it smoothly loops back to the bottom.

```rust
// If the circle moves off the screen upwards, reset its position to the bottom of the screen
if state.circle_y < 0.0 {
    let [_w, h] = resolution();
    state.circle_y = h as f32;
}

```

A simple moving rectangle and upward-moving circle.  Check out the source code [here](https://gist.github.com/tajerdev/d27b9b1b406c97b3b0972ee6d57980e5).


![A simple moving rectangle and upward-moving circle](/_media/gamesave.gif)


## Debugging

In Turbo, you can use the `turbo::println!` macro to log messages to the console for debugging purposes.

```rust
// Log a message
turbo::println!("Debugging message: gooddd code");
```
