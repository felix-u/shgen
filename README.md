### shgen
`shgen` is a POSIX shell script to evaluate shell expansions within any file.
It is fast and lightweight with no external dependencies.


## Usage
Make an alias or copy `shgen` to somewhere in your `$PATH`.
Then run `shgen`, providing as arguments an existing input file and an output file
location. If the output file already exists, it will be overwritten.
```sh
shgen /path/to/inputfile /path/to/outputfile
```


## Examples
1. One very handy use of `shgen` is to "hack in" support for your Xresources file
in any configuration file, including on Wayland. Put this script somewhere in
your `$PATH`. In this scenario the script is named `query`, and `$XRESOURCES`
is the path to your Xresources file.
```sh
grep "$1:" "$XRESOURCES" | awk '{print $2}'
```
Alternatively, if you use X11, you might like to take advantage of `xrdb`,
though in my experience it comes with no performance benefits and is
generally more hassle than the option above.
```sh
xrdb -query | grep "$1" | awk '{print $NF; exit}'
```
Now run `shgen` on any type of file. Here's a snippet of my `zathura` config:
```sh
set notification-error-fg       "$(query color1)" #red
set notification-warning-fg     "$(query color3)" #yellow
set notification-fg             "$(query color2)" #green
set inputbar-fg                 "$(query color12)" #bright-blue
set highlight-color             "$(query color3)" #yellow
set highlight-active-color      "$(query color4)" #blue
```
and the result:
```sh
set notification-error-fg       "#dc322f" #red
set notification-warning-fg     "#b58900" #yellow
set notification-fg             "#859900" #green
set inputbar-fg                 "#268bd2" #bright-blue
set highlight-color             "#b58900" #yellow
set highlight-active-color      "#268bd2" #blue
```

2. For the poor X11 users who don't benefit from Wayland's flawless fractional
scaling, managing configurations for multiple devices can be frustrating.
In this situation, `shgen` can be used to multiply any pixel values by the
device's scale factor. Have the following function saved as `dpi` somewhere
in your `$PATH`:
```bash
#!/usr/bin/env bash
xquery () {
    xrdb -query | grep "$1" | awk '{print $NF; exit}'
}

XDPI=$(bc <<< "scale=2; $(xquery dpi)/96")

dpi () {
    VAL=$(bc <<< "scale=2; $1 * $XDPI"); # multiply input by DPI
    # return value rounded to nearest integer
    printf "%.0f\n" "$(bc <<< "scale=2; $VAL + 0.01")"
            # add 0.01 so that 2.5 rounds to 3, not 2
}
```
Then:
```dosini
[bar/laptop]
height = $(dpi 50)
```
The height will evaluate to `100` on my laptop at 2x scaling, and `63` on my
desktop at 1.25x scale.

3. Specifying your network interface in your status bar's wifi module is
sometimes needed. With `shgen`, you can write
```sh
interface: $(ip -o route show to default | awk '{print $5}')
```
which in my case will evaluate to `interface: wlan0`.


## Limitations
You will have to escape shell-sensitive characters such as `$` and `^` in input
files, which is a problem far less often than you might expect.
