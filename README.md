### This repo is undergoing refactoring and until that's complete, consider this README outdated.

# xgen
**A POSIX tool for using shell script in non-shell files.**

## Requirements
By itself, `xgen` has no dependencies. If you would like to use the supplied functions for querying Xresources, install `awk` and `xorg-xrdb`.
Clone this repo in a directory of your choosing and audit `xgen` to make sure you understand what it is doing - it is a short and simple script.
Then, make it executable.
```sh
git clone https://github.com/felix-u/xgen && cd xgen
chmod +x ./xgen
```
For convenience, you can alias `xgen` to the location of the script on your disc in your shell profile:
```sh
alias xgen=/path/to/xgen
```

## Usage

#### Working with plaintext

In a shell script, you'd use `$(command [arguments])` to include the output of a command. For instance:
```sh
$ echo "There are $(ls ~ | wc -l) lines in my home directory"
```
output: `There are 74 lines in my home directory`

`xgen` allows you to do the same in any plaintext file. This means you can call shell commands in lua, yaml, javascript, or any other plaintext file, such as configuration files. To get an idea of what is possible, view [some examples](./examples.md).

#### Querying X Resources
`xgen` comes with two functions allowing you to grab colours, dpi, and fonts from .Xresources: `xquery` and `xquerystrip`.

Firstly, set up an `.Xresources` file and make sure you understand what's in it. You can learn about X Resources [here](https://wiki.debian.org/Xresources) or [here](https://wiki.archlinux.org/title/x_resources).

Inside a text file such as a configuration file, you can now call the `xquery` function to get colours, dpi, or your font from X Resources:
```
set font                        "$(xquery fontmono) 12"
set notification-error-bg       "$(xquery background)" #background
set notification-error-fg       "$(xquery color1)" #red
```
output:
```
set font                        "Iosevka 12"
set notification-error-bg       "#1a1b26" #background
set notification-error-fg       "#f7768e" #red
```

`xquery` will include the hash symbol at the beginning of the colour hex. In situations where this is undesirable, such as when you need to work with the `#aaRRGGBB` format, you can use `xquerystrip` instead:
```
xbackground = #e5$(xquerystrip background)
```
output:
```
xbackground = #e51a1b26
```

Finally, generate the text and write it to a file as follows:
```sh
xgen /path/to/input /path/to/output
```

#### Safety
`xgen` works by generating a script in `/tmp/` and running it, so make certain you know exactly what commands you're calling in your configs. I take no responsibility for a ruined system due to careless use.
If this is a concern, use the [old version](./old/) of `xgen`, which searches for specific strings (involving `xquery` and `xquerystrip` only) and runs global substitutions on them. This is far more limited functionality, but if you only plan to use `xquery` and `xquerystrip` to grab your font, dpi, and colour scheme from X Resources, the syntax is unchanged.
