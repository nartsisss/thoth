# Thoth

[![CI](https://github.com/jooaf/thoth/actions/workflows/rust-ci.yml/badge.svg)](https://github.com/jooaf/thoth/actions/workflows/rust-ci.yml)
[![Release](https://img.shields.io/github/v/release/jooaf/thoth)](https://github.com/jooaf/thoth/releases)
[![Auto Tag and Update Version](https://github.com/jooaf/thoth/actions/workflows/cut-tag.yml/badge.svg)](https://github.com/jooaf/thoth/actions/workflows/cut-tag.yml)
[![Crates.io](https://img.shields.io/crates/v/thoth-cli.svg)](https://crates.io/crates/thoth-cli)
[![Docs.rs](https://docs.rs/thoth-cli/badge.svg)](https://docs.rs/thoth-cli)

![thoth](https://github.com/user-attachments/assets/b5954aac-b20f-4c24-af7c-67dc6224df89)


A terminal scratchpad inspired by [Heynote](https://github.com/heyman/heynote). 

## Inspiration
As mentioned on Heynote's GitHub page, it is "a dedicated scratchpad for developers. It functions as a large persistent text buffer where you can write down anything you like."
This app is truly great, and I've quite enjoyed using it as dedicated scratchpad. However, I work in the terminal, and I was finding several workflows 
in which having a terminal based scratchpad would be nice. Enter Thoth! Thoth follows the same design philosophy as Heynote in terms of its simplicity, but with some key differences. 

### Differences Compared to Heynote 
- The persistent buffer in Thoth gets saved as a markdown file in your home directory. I did this since I use [Obsidian](https://obsidian.md) for all of my notes, and I wanted my scratchpad 
to also be displayable in the app.
- Blocks are titled and can be selected by title.
- The ability to edit a block using your favorite terminal editor such as Neovim, Helix, Vim, and others via setting the `$EDITOR` or `$VISUAL` environment variables.
- A CLI that allows one to pipe information via STDIN into a new block, or use STDOUT to get text out of a block. 


## Why the name Thoth? 

<img width="160" alt="image" src="https://github.com/user-attachments/assets/e24b51f1-1e07-45c1-973e-321d17b87041">

In Egyptology, Thoth is the god of writing, wisdom, and magic. Thoth was in charge of messenging and recording knowledge and events for the Egyptian deities. 
Seems like a fitting name for a persistent scratchpad :). 

## Main tools used to build Thoth
- [tui-textarea](https://github.com/rhysd/tui-textarea)
- [ratatui](https://github.com/ratatui-org/ratatui)
- [crossterm](https://github.com/crossterm-rs/crossterm)
- [pulldown-cmark](https://github.com/pulldown-cmark/pulldown-cmark)
- [clap](https://github.com/clap-rs/clap) 

## Installation 
### Via Cargo
If you have `cargo` installed on your machine, you can download directly from crates.io 

```bash 
cargo install thoth-cli
```

### From binaries via Release section
Go to the release page, and download the binary associated with your OS (Note: currently supports Linux and MacOS). 
Once you have downloaded the binary, go to the location where you downloaded the binary, rename the binary to `thoth`, and give the binary executable permissions by doing the following:
```bash
cd /path/to/binary
mv current_binary_name thoth
chmod +x thoth
```
You can then move binary if you choose so. Finally, add the path of the binary to your `$PATH`. 

### From Source 
To build from source, please make sure that you have the Rust compiler installed (version 1.70.0 or higher), and the Cargo package manager. You can install both by installing [rustup](https://rustup.rs/).

Once you have installed Rust, please do the following:
```bash
git clone https://github.com/jooaf/thoth.git
cd thoth
cargo build --release
```
The binary lives in `thoth/target/release/thoth`. You can add the binary to your `$PATH`.

## Usage 
This will show how to use the scratchpad via the CLI or the TUI. 

### TUI 
To start the TUI, simple type `thoth`. Since it is a persistent buffer, thoth will save when you hit quit using `q`.

#### Commands for main mode 
```
q: Quit 
<ctrl-n>: Add block in focus
<ctrl-d>: Delete block in focus
<ctrl-y>: Copy the complete block
Enter: Edit block 
<ctrl-f>: Full Screen 
Esc: Exit 
<ctrl-t>: Change title of block 
<ctrl-s>: Select block by title
<ctrl-j>: Format json
<ctrl-k>: Format markdown 
```
#### Commands for edit mode
```
Esc: Exit edit mode
<ctrl-g>: Move cursor top 
<ctrl-b>: Copy highlighted selection 
Shift-Up Arrow or Down: Make highlighted selection by line
<ctrl-y>: Copy the entire block
<ctrl-t>: Change title of block 
<ctrl-s>: Select block by title 
<ctrl-e>: Use external editor 
<ctrl-h>: Bring up other commands
```

The other commands are based off of the default keybindings 
for editing in `tui-textarea`.

| MAPPINGS | DESCRIPTIONS |
|----------|--------------|
| Ctrl+H, Backspace | Delete one character before cursor |
| Ctrl+K | Delete from cursor until the end of line |
| Ctrl+W, Alt+Backspace | Delete one word before cursor |
| Alt+D, Alt+Delete | Delete one word next to cursor |
| Ctrl+U | Undo |
| Ctrl+R | Redo |
| Ctrl+C, Copy | Copy selected text |
| Ctrl+X, Cut | Cut selected text |
| Ctrl+P, ↑ | Move cursor up by one line |
| Ctrl+→ | Move cursor forward by word |
| Ctrl+← | Move cursor backward by word |
| Ctrl+↑ | Move cursor up by paragraph |
| Ctrl+↓ | Move cursor down by paragraph |
| Ctrl+E, End, Ctrl+Alt+F, Ctrl+Alt+→ | Move cursor to the end of line |
| Ctrl+A, Home, Ctrl+Alt+B, Ctrl+Alt+← | Move cursor to the head of line |
| Ctrl+K | Format markdown block |
| Ctrl+J | Format JSON |

If you would like to use your external editor -- such as NeoVim, Helix, etc. -- Thoth offers that functionality.

### CLI 
For accessing the CLI, one can use `thoth` followed by a command.
```
A terminal scratchpad akin to Heynote

Usage: thoth [COMMAND]

Commands:
  add     Add a new block to the scratchpad
  list    List all of the blocks within your thoth scratchpad
  delete  Delete a block by name
  view    View (STDOUT) the contents of the block by name
  copy    Copy the contents of a block to the system clipboard
  help    Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version  
```

#### Examples 
```nu
# For adding new blocks 
thoth add hello_world "Hello, World!";
# For adding new blocks with content from STDIN 
echo "Hello, World (from STDIN)" | thoth add hello_world_stdin;
# Using view to pipe contents into another command
thoth view hello_world_stdin | cat
```
