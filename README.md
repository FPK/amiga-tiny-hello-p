# Tiny "Hello, World!" in Free Pascal for Amiga

This is a silly size coding experiment, to figure out what's the smallest
executable size which is possible with the Amiga/m68k version of Free Pascal.

I got bored by some assembly fans pointing out that Free Pascal generates
"huge" executables, and I wanted to prove that if you know what you're doing,
and stick to the AmigaOS API (much like you'd do in assembly), you can produce
really tiny executables with Free Pascal, which can compete with any compiler
for the same platform.

The result I've got is actually ~348~ ~308~ ~304~ ~296~ ~252~ 244 bytes for a
"Hello, world!".

The size could be reduced further by using the V36-only PutStr() call, and
doing some even messier hacks, but wanted something which works on every Amiga,
and it's clean enough as an example.

## So, what kind of magic is this?

Well, just what's needed. It provides a dummy version of the startup code
(`si_prc.pp`) used by Free Pascal, to bypass the entire System unit
initialization, and implements a custom user `_startup` function. Then - as
none of the original unit infrastructure is being referred to by active
code - the linker just optimizes all of it away and none of it lands in
the final executable.

## Files

* `si_prc.pp` - the dummy Startup code, to make the linker happy
* `hello.pas` - a "Hello, World!" in Pascal, using direct AmigaOS calls
* `hello`     - the example Amiga binary output
* `build.sh`  - cross-build script

## Requirements

Needs (means: it was only tested with)  Free Pascal current SVN trunk. Not
tested with the release version. You also need `vasm` and `vlink` for this
to work.

The resulting executable should run on any Amiga.

## Disclaimer

I actually don't encourage this kind of programming. Free Pascal provides a
convenient infrastructure for high level programming needs, starting from
stack management, fast heap routines preventing system level fragmentation,
advanced file handling, threading, and much much more. None of that is
available when you write programs this way. If you try to create more
complex apps, you'll also likely to encounter various issues, which are
difficult to understand and resolve, unless you're intimately familiar with
both AmigaOS and Free Pascal internals.

But it hopefully makes a point. A similar technique can be used with Free
Pascal on most operating systems, not only Amiga actually.
