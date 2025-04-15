# comfile
This is a fixed version (0.4, based on `comfile-0.3.tar.gz`) of http://www.muppetlabs.com/~breadbox/txt/mopb-app.html to build
and run on recent Linux kernel (as of 2025).

It also contains some improvements:
- produce smaller binary `COM` files (Eg: `tinier.asm` produces 3 bytes and `tiny.c` 6 bytes of `COM` binary returning 42)
- no need for any metadata (eg: like perusing the file name as data).

All due credits to Brian Raiter for the original work (0.3).
See `README` for the original project description.


Copyright 2025 by Nicolas Sauzede <nicolas.sauzede@gmail.com>.
Copyright 2019 by Brian Raiter <breadbox@muppetlabs.com>.

The files contained in this distribution are free software. The
comfile kernel module is licensed under the GNU General Public
License; either version 2 of the License, or (at your option) any
later version. The remaining files of this distribution can be copied
and distributed, with or without modification, in any medium without
royalty provided the copyright notice and this notice are preserved.
All files in this distribution are offered as-is, without any
warranty.

# Usage

Build the whole thing:
```shell
comfile $ make clean all
<enter sudo passwd to install the comfile kernel module>
comfile $ ls -l src/*.com
-rwxr-xr-x 1 nico nico 8644 Apr 15 11:35 src/cat.com
-rwxr-xr-x 1 nico nico  204 Apr 15 11:35 src/echo.com
-rwxr-xr-x 1 nico nico  164 Apr 15 11:35 src/env.com
-rwxr-xr-x 1 nico nico 1032 Apr 15 11:35 src/factor.com
-rwxr-xr-x 1 nico nico   80 Apr 15 11:35 src/hello.com
-rwxr-xr-x 1 nico nico  772 Apr 15 11:35 src/tac.com
-rwxr-xr-x 1 nico nico    3 Apr 15 11:35 src/tinier.com
-rwxr-xr-x 1 nico nico    6 Apr 15 11:35 src/tiny.com
```

Assembly:
```shell
comfile $ cat src/tinier.asm
BITS 64
    mov al, 42
    ret
comfile $ ls -l src/tinier.com
-rwxr-xr-x 1 nico nico 3 Apr 15 11:35 tinier.com
comfile $ src/tinier.com ; echo $?
42
```

C:
```shell
comfile $ cat src/tiny.c
int main() {
    return 42;
}
comfile $ ls -l src/tiny.com
-rwxr-xr-x 1 nico nico 6 Apr 15 11:35 tiny.com
comfile $ src/tiny.com ; echo $?
42
```

Other programs using arguments/environ still work as intended:
```shell
comfile $ src/hello.com
hello, world
comfile $ src/echo.com 1 2 3
1 2 3
comfile $ src/env.com
SHELL=/usr/bin/bash
COLORTERM=truecolor
HISTCONTROL=ignoreboth
XDG_MENU_PREFIX=gnome-
TERM_PROGRAM_VERSION=3.5a
...
comfile $ src/cat.com /etc/mailcap | cmp - /etc/mailcap
comfile $
```
