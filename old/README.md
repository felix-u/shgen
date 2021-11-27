Install `sed`.

This version of `xgen` looks for a `fontmono` X Resource defining your monospace font in one word, but you can modify the script to find a different resource instead.

You can query colours and dpi as normal.

Only `xquery` and `xquerystrip` will work with this version of `xgen`. It does not actually run the shell commands in your files, but looks for specific strings and runs global substitutions using `sed`. You can modify the script to support more "commands".
