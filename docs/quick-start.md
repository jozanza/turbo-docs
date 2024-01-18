*Start Your Turbo Game Development Adventure with 'Hello, World!' 🚀** (~5-10 mins)

![Quick Start Banner](_media/quick-start-banner.webp)

## Installation

### Dependencies   

First, install Rust if it's not already on your system:

<!-- tabs:start -->

#### **MacOS / Linux**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Or follow the instructions at https://www.rust-lang.org/tools/install

#### **Windows**

**MSVC**

For a typical setup, follow the **Recommended** instructions using `rustup` at https://www.rust-lang.org/tools/install

**WSL**

For a Windows Linux Subsystem environment, follow the instructions for MacOS / Linux.

<!-- tabs:end -->

Next, add the necessary WebAssembly targets:

```bash
rustup target add wasm32-unknown-unknown wasm32-wasi
```

Also, install `cargo-watch` to streamline our workflow:

```bash
cargo install cargo-watch
```

### Turbo CLI

Now that our dependencies installed, let's also install `turbo`:

<!-- tabs:start -->

#### **MacOS / Linux**

Install `turbo` by running this script:

```bash
sh -c "$(curl -sSfL https://turbo.computer/install.sh)"
```

?> The installer will ask for your password to place the command in `/usr/local/bin`. If you don't want to do this, you can manually download the 64-bit releases and move them to a directory in your `PATH`: [Mac OS ARM](https://turbo.computer/bin/turbo-0.2.0-aarch64-apple-darwin/turbo), [Mac OS x86](https://turbo.computer/bin/turbo-0.2.0-x86_64-apple-darwin/turbo), [Linux (GNU)](https://turbo.computer/bin/turbo-0.2.0-x86_64-unknown-linux-gnu/turbo), [Linux (MUSL)](https://turbo.computer/bin/turbo-0.2.0-x86_64-unknown-linux-musl/turbo)
<br /><br />
If you need a binary for another platform, contact [Turbo](https://twitter.com/makegamesfast) on Twitter.

Verify the installation with:

```bash
turbo -h
```

If successful, you'll see `turbo`'s help documentation:

```bash
Run Turbo games natively on desktop

Usage: turbo <COMMAND>

Commands:
  init  Initializes a new Turbo project in Rust
  run   Runs a Rust Turbo project
  help  Print this message or the help of the given subcommand(s)

Options:
  -v, --version  Print version
  -h, --help     Print help
  -V, --version  Print version
```

#### **Windows**

Follow these steps to install `turbo` on Windows:

1. Download the 64-bit release for [Windows MSVC](https://turbo.computer/bin/turbo-0.2.0-x86_64-pc-windows-msvc/turbo.exe.zip).
2. If it doesn't already exist, create a folder at `C:\Users\YOUR_USERNAME\bin`.
3. Unzip the file and move `turbo.exe` into that folder.

Make sure you have [Git for Windows](https://git-scm.com/download/win) installed.

**Open Git Bash**. Verify the installation with:

```bash
turbo -h
```

If successful, you'll see `turbo`'s help documentation:

```bash
Run Turbo games natively on desktop

Usage: turbo.exe <COMMAND>

Commands:
  init  Initializes a new Turbo project in Rust
  run   Runs a Rust Turbo project
  help  Print this message or the help of the given subcommand(s)

Options:
  -v, --version  Print version
  -h, --help     Print help
  -V, --version  Print version
```

#### **Windows (WSL)**

Follow these steps to install `turbo` on Windows Linux Subsystem (WSL):

1. Download the 64-bit release for [Windows Linux Subsystem](https://turbo.computer/bin/turbo-0.2.0-x86_64-pc-windows-gnu/turbo.exe.zip).
2. Unzip the file and move `turbo.exe` into that folder.
3. Copy the `turbo.exe` command to `/usr/local/bin`.

In your WSL shell, verify the installation with:

```bash
turbo -h
```

If successful, you'll see `turbo`'s help documentation:

```bash
Run Turbo games natively on desktop

Usage: turbo.exe <COMMAND>

Commands:
  init  Initializes a new Turbo project in Rust
  run   Runs a Rust Turbo project
  help  Print this message or the help of the given subcommand(s)

Options:
  -v, --version  Print version
  -h, --help     Print help
  -V, --version  Print version
```

<!-- tabs:end -->



## Development

?> Windows users should always run `turbo` in Git Bash.

### Creating a Game

Begin by creating a new project called "hello-world":

```
turbo init hello-world
```

This initializes a rust crate in a `hello-world` directory. Open it in your preferred editor.


### Editing a Game

To view your game, run the following command at the root of the project directory:

```
turbo run -w .
```
?> The `-w` flag auto-refreshes your game window as you code. Just be sure to watch the console for compiler errors.


![Turbo game window with the text "Hello, world!!!"](_media/hello-world.png)

Now, open `src/lib.rs`. You should see something like this:

```rust
// This is where your main game loop code goes
// The stuff in this block will run ~60x per sec
turbo::go! {
  text!("Hello, world!!!");
}
```

Time for your first update. Modify the text and check out your game window:

```rust
text!("Yuuurrr!");
```

![Turbo game window with the text "yuuurrr!!!"](_media/yuuurrr.png)


And for the purposes of this guide, that's about all there is to it! If you want to keep playing around, the `text!` macro has several optional parameters you can experiment with:

```rust
text!("Let's gooo!", x = 32, y = 48, color = 0xff00ffff, font = Font::L);
```

## Next Steps

Congratulations on starting your game development journey! 🎉

Ready to level up your skills?

Dive deeper into exciting techniques like:

- Drawing shapes
- Animating sprites
- Handling gamepad input ️
- Implementing game settings
- Modeling game states
- Unlocking Web3 integration

#### [Explore the possibilities through interactive demos &rarr;](/examples)

<br />