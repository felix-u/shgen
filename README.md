# xgen
A POSIX tool for dynamically generating configs with colours, dpi, and fonts from .Xresources.

## Installation
The following must be installed on your system: `awk`, `sed`, `bc`, and `xorg-xrdb`.
To "install" `xgen`, clone this repo in a directory of your choosing, make `xgen` executable, and symlink it to `/usr/bin/` or another directory in your `$PATH`.
```sh
git clone https://github.com/felix-u/xgen && cd xgen
chmod +x ./xgen
sudo ln -s ./xgen /usr/bin
```

## Usage
Firstly, set up an `.Xresources` file and make sure you understand what's in it. You can learn about X Resources [here](https://wiki.debian.org/Xresources) or [here](https://wiki.archlinux.org/title/x_resources).
`xgen` also looks for a `fontmono` X Resource defining your monospace font in one word, but you can modify the script to find a different resource instead.

Inside a text file such as a configuration file, you can now call the `xquery` function to get colours, dpi, or your font from X Resources like so:
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

### Why this syntax?
At the moment, `xgen` uses `sed` to run global substitutions on a limited number of supported strings. In the future, `xgen` is planned to use your system shell to evaluate any commands called in your file as `$(command input)`. In short, shell syntax is used for compatibility with future versions of `xgen` eventually allowing you to use actual shell scripting in any config file. Help out with a pull request! :)
