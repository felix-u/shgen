# Alacritty

Dynamically generate your alacritty colour scheme:
```yaml
colors:
  # Default colors
  primary:
    background: '0x$(xquerystrip background)'
    foreground: '0x$(xquerystrip foreground)'

  # Normal colors
  normal:
    black:   '0x$(xquerystrip color0)'
    red:     '0x$(xquerystrip color1)'
    green:   '0x$(xquerystrip color2)'
    yellow:  '0x$(xquerystrip color3)'
    blue:    '0x$(xquerystrip color4)'
    magenta: '0x$(xquerystrip color5)'
    cyan:    '0x$(xquerystrip color6)'
    white:   '0x$(xquerystrip color7)'

  # Bright colors
  bright:
    black:   '0x$(xquerystrip color8)'
    red:     '0x$(xquerystrip color9)'
    green:   '0x$(xquerystrip color10)'
    yellow:  '0x$(xquerystrip color11)'
    blue:    '0x$(xquerystrip color12)'
    magenta: '0x$(xquerystrip color13)'
    cyan:    '0x$(xquerystrip color14)'
    white:   '0x$(xquerystrip color15)'
```
Output on my system:
```yaml
colors:
  # Default colors
  primary:
    background: '0x1a1b26'
    foreground: '0xdce0ef'

  # Normal colors
  normal:
    black:   '0x2d2f43'
    red:     '0xf7768e'
    green:   '0x9ece6a'
    yellow:  '0xe0af68'
    blue:    '0x7aa2f7'
    magenta: '0x9a7ecc'
    cyan:    '0x3ec0d5'
    white:   '0x6b7394'

  # Bright colors
  bright:
    black:   '0x4e5173'
    red:     '0xfaa3b4'
    green:   '0xb9f27c'
    yellow:  '0xe8c28d'
    blue:    '0xa4bffa'
    magenta: '0xb5a0d9'
    cyan:    '0x64cddd'
    white:   '0xacb0d0'
```

# Polybar

Grab your network interface name:
```sh
[module/network]
type = internal/network
interface = $(ip -o route show to default | awk '{print $5}')

```
Output on my system:
```
[module/network]
type = internal/network
interface = eno2
```
