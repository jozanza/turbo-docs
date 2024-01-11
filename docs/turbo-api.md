# Turbo API

Turbo aims to be as simple as possible to help you learn and make 2D games fast.
For more information, visit the [rust SDK](https://docs.rs/turbo-genesis-sdk/).

## Frame Rate Control

The Turbo crate provides the following constants for managing frames rates:

- `canvas::fps::FAST (10)`: Represents a fast frame rate.
- `canvas::fps::MEDIUM (4)`: Represents a medium frame rate.
- `canvas::fps::SLOW (2)` Represents a slow frame rate.
-  `canvas::fps::REALLY_SLOW (1)`: Represents a really slow frame rate.
-  `canvas::fps::SUPER_FAST (20)`: Represents a super fast frame rate.

#### Sample

To control the frame rate of a sprite animation, you can use the fps parameter in the sprite! macro:

```rust
// Display a sprite named "cat" at specified position with a fast frame rate.
sprite!("cat", x = 120, y = 50, fps = canvas::fps::FAST);
```
?> You can also manually adjust frame rates by specifying values between 1 and 60. This flexibility allows you to precisely control the speed of animations in your game.

 ## Fonts


The `turbo_genesis_sdk::canvas::Font`
 enum provides different font styles for rendering text in Turbo. The available font variants are:

- Font::S: Small-sized font.
- Font::M: Medium-sized font.
- Font::L: Large-sized font.

```rust
// Using a small-sized font
text!("Hello, world!", font = Font::S);
```
## Setting Colours

For all methods that accept a color argument, it must be passed in RGBA format.

Examples:

- 0xffffffff is white
- 0x000000ff is black
- 0x0000ffff is blue
- 0xffff00ff is yellow
- 0xff0000ff is red
- 0x00ff00ff is green

At the moment, colors must be full opaque Semi-transparent colors will be rendered at full opacity.


## Configuration Settings

Turbo allows you to configure various settings for your game. Use the `turbo::cfg!` macro to define your game's configuration. Below is an example configuration:

```rust
// Define the game configuration using the turbo::cfg! macro
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
?>The Solana configuration is optional, except for those who want to interact with Solana.

## Adding shapes.

In Turbo, incorporating shapes into your game is a straightforward process. The provided macros simplify the task, allowing you to focus on the creative aspects of your design. Below are examples demonstrating how to draw filled circles and rectangles:


For Circle, we have the `circ!` macro
```rust 
// Draw a filled circle at specified position with specified diameter and color.
circ!(x = 128, y = 128, d = 7, fill = 0x000000ff);
```

For Rectangles, we have the `rect!` and `rectv!` macro
```rust
 // Draw a filled rectangle at specified position with specified width, height,
 // and color.
 rect!(w = 20, h = 40, x = 70, y = 70, fill = 0x0000ffff);

 // turbo provides a convenient way to create and 
 // manipulate rectangles with various properties. like rotated regtangles
 let border = Border {
        radius: 0,
        size: 0,
        color: 0xffffffff,
    };

 rectv!(
        x  = 70,
        y = 70,
        w = 20,
        h  = 40,
        fill = 0xffffffff,
        rotation_deg = 100,
        rotation_origin = (0, 0),
        border = border
    );
```

## Getting texts
 
 To display texts using Turbo, you can use the text macro.

The Text macro:
```rust
// Display customized text with specified position, color, and font size.
text!("super turbo society", x = 30, y = 40, color = 0x00ff00ff, font = Font::S);
```

## Adding Sprites
Adding sprites to your Turbo game is a straightforward process that involves creating a `sprites` folder within your project directory. Follow these simple steps to integrate sprites into your Turbo game:

1. **Create a `sprites` Folder:**
 - Inside your project directory, create a folder named `sprites`. This folder will contain all your game sprites.

2. **Place Sprite Images:**
- Place your sprite images (e.g., PNG or JPEG files) inside the `sprites` folder. These images will be the visual elements of your game.

3. **Use the Sprite Macro:**
- To use a sprite in your game, employ the `sprite!` macro provided by Turbo. This macro takes various parameters, such as the sprite name, position (`x` and `y`), frame rate (`fps`), and degree of rotation.

```rust
//Display a sprite named "cat" at a specific position with a fast frame rate.
sprite!("cat", x = 120, y = 50, fps = canvas::fps::FAST, deg = 0);
```

- Replace `"cat"` with the actual filename of your sprite image (excluding the file extension).
- Adjust the `x` and `y` parameters to position the sprite where you want it.
- Set the `fps` parameter to control the frame rate of the sprite animation.


## Initializing the Game State

Turbo provides a straightforward way to initialize the standard game state using the `turbo::init!` block. This block is essential for defining and setting up the game state before entering the main game loop. Let's take a look at an example using the standard `GameState` struct

```rust
turbo::init! {
    // Define the GameState struct with x and y positions.
    struct GameState {
        x_position: i32,
        y_position: i32,
    } = {
        // Initialize the struct with specific initial values.
        Self {
            x_position: 30,
            y_position: 40,
        }
    }
}
```

## Background Colour

The `clear` function in Turbo allows you to set the background color of the canvas. This function takes a single argument, `color`, which represents the color to be used for clearing the canvas.

```rust
// Set the background color to black (0x000000FF)
clear(0x000000FF);
```

## Debugging
In Turbo, you can use the `turbo::println!` or `turbo::prelude::println!`  macro to log messages to the console for debugging purposes.

```rust
// Log a message
turbo::print!("Debugging message: gooddd code");
```
