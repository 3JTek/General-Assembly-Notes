# The Terminal

The terminal (or shell) is an application like Chrome, Slack or Atom. It allows a user to execute commands on the operating system, like navigating the hard-drive, making and modifying files and running programs.

## Basic syntax

Almost all terminal commands have the following syntax:

```sh
[command] [options] [file]
```

A typical example might be:

```
rm -r images
```

Here the command is `rm` which means *remove*, it will delete files. The `-r` is an option. Options always being with a `-`. In this instance `-r` means *recursively*, and is a requirement when deleting folders. Finally we have the folder to be deleted `images`.

As developers we spend a lot of time typing, and installing and configuring software and tools to help us do our jobs. The more comfortable you are with the terminal the more efficient you can be and the quicker you can solve problems.

## Navigating the file structure

In order to move around the hard drive via the terminal you need to be able to move into a directory, and out again. To do that you need to understand how _paths_ work.

Consider the following file structure:

```sh
mysite
├── css
│   └── style.css
├── index.html
└── js
    └── app.js
```

To navigate into the `css` folder, you can type `cd css`, or you can type `cd ./css`. The `.` means the _current directory_.

Once in the `css` directory, to navigate out, you would type `cd ..`. The `..` mean the _parent directory_.

To go to the `js` directory from the `css` directory you would type `cd ../js`.

The `~` character means you home directory, which on a Mac is `/Users/username`, so if your username was mickyginger, your home directory would be `/Users/mickyginger`. All of your files and folders live here. Things like `Documents` and `Downloads` etc. Notice that `/Users` starts with a slash. That's known as a _leading slash_. That indicates the very root of your hard drive. There is no parent folder for `/`.

To check which directory you are in you can type `pwd`.

## Useful commands

| **Command** | **Action** | **Example** |
|-------------|------------|-------------|
| `pwd` | Print current directory | `pwd`
| `cd` | Change directory | `cd ..`
| `mkdir` | Make a directory | `mkdir images`
| `rm` | Remove a file or folder | `rm -r images`
| `touch` | create an empty file | `touch index.html`
| `ls` | List directory contents | `ls .`
| `cp` | Copy a file or folder | `cp index.html contact.html`
| `mv` | Move or rename a file | `mv contact.html about.html`
| `open` | Open a file or folder | `open index.html`
| `atom` | Open a file or folder with Atom | `open .`
| `history` | Get a list of all the commands typed in the last session | `history`
| <kbd>TAB</kbd> | Autofill the current command or filename | &nbsp;  |

## Useful shortcuts

| **Shortcut**    | **Action** |
|-----------------|-------------|
| <kbd>Ctrl</kbd> + <kbd>A<kbd> |  Go to the beginning of the line you are currently typing on
| <kbd>Ctrl</kbd> + <kbd>E<kbd> |  Go to the end of the line you are currently typing on
| <kbd>Ctrl</kbd> + <kbd>L<kbd> |  Clear the screen, similar to the clear command
| <kbd>Ctrl</kbd> + <kbd>U<kbd> |  Clear the line before the cursor position. If you are at the end of the line, clears the entire line.
| <kbd>Ctrl</kbd> + <kbd>H</kbd> |  Same as backspace
| <kbd>Ctrl</kbd> + <kbd>R</kbd> |  Let’s you search through previously used commands
| <kbd>Ctrl</kbd> + <kbd>C</kbd> |  Kill whatever you are running
| <kbd>Ctrl</kbd> + <kbd>D</kbd> |  Exit the current shell
| <kbd>Ctrl</kbd> + <kbd>W</kbd> |  Delete the word before the cursor
| <kbd>Ctrl</kbd> + <kbd>K</kbd> |  Clear the line after the cursor
| <kbd>Ctrl</kbd> + <kbd>T</kbd> |  Swap the last two characters before the cursor
| <kbd>TAB</kbd> |   Auto-complete files and folder names
| <kbd>⬆︎</kbd> | Get the last command

## A word of warning

When using the terminal there is no undo, so **be careful!**

For example if you typed: `rm -rf /` (the `f` means force) it would delete your entire hard drive!

## Further reading
- [Useful \*NIX Shell Commands for Web Developers](https://code.tutsplus.com/articles/useful-nix-shell-commands-for-web-developers--cms-26161)
- [Terminal Mac Cheatsheet](https://github.com/0nn0/terminal-mac-cheatsheet)
