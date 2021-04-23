Journal de Bord
=================================
This is were I keep track of notes throughout the project so that I can later
make more concise documentations and articles.

## 2021/04/09
The Pokemon Mini scene is quite small and niche. There are some work done to provide a
better development experience on the three major desktop OSes,
like [here](https://github.com/pokemon-mini/c88-pokemini)
or [there](https://github.com/notyourav/c88toolchain/tree/52c981103cd4791bcd6f566dd79b844f87d1a2b3).
However I found the informations around the dev env and CPU quite scarce. To
understand better what I’m doing, I would like to build my environment from the
ground up, and this required a hunt for data.

I hope that by doing this work, I can provide better informations
for the people who need it.

### The Tools
The CPU was found to be from the S1C88 family of microcontroller by Epson. The S1C88
comes in different models. The one used in the pokemini is a MODEL3 maximum mode.
It has its own official compiler you can find on [archive.org](https://web.archive.org/web/20190411141705/www.epsondevice.com/products_and_drivers/semicon/products/micro_controller/zip/s5u1c88000c16.zip).
Sadly it is only available natively on Windows. If you’re on Linux or Mac, you
have to make a Windows VM or install wine. The two links I posted above provide
a Makefile already configured to be able to use the compiler through wine.

It has a `doc/` directory which I encourage you to explore. The documentation has
`manual1E.pdf` for the C compiler, assembler and linker. `manual2E.pdf` focus on
the rest of the tools provided. You can find the documentation for the
inner functioning of the core itself [here](http://www.rayslogic.com/Software/TimexUSB/Docs/s1c88%20core%20cpu%20manual.pdf).

### Compiling and running a hello world
To make sure everything is setup properly I’m going to build a minimal project.
I’m going with [tup] as my build system instead of `make` as I want to learn
more about it. I also don’t want to spend too much time writing a solid Makefile.

## 2021/04/15
Still going on with the tup setup. It’s a bit hard to learn two tools at once.
I can’t use a typical C compiler as it wouldn’t be able to target the S1C88 as
a backend, without myself being involved in describing such backend.
The S1C88 official compiler seems to not follow flag conventions we see in
gcc or clang.
Plus, we don’t target a desktop executable but rather the binary blob that will
be sent either in a emulator or the real hardware.

## 2021/04/17
I couldn’t do a lot the 15th. Today, I would like to focus on actually being
able to compile a simple hello world, and learn the tools around the compiler
to debug and test the executable.

## 2021/04/18
Couldn’t do a lot yesterday too. Who knew??
I need to make the first step, I’m learning too much things at once.
Anxiety doesn’t help.

### building steps
Let’s start with a simple hello world like [this one](https://github.com/pokemon-mini/c88-pokemini/tree/master/examples/helloworld/src)

## 2021/04/20
I think I got the hang on tup. Today should be to configure VS Code and it’s
C/C++ extension to get a more integrated experience, and be able to run
a hello world. Afterwards, I think it would be good to take a deep dive in the
inner functioning of the mini and its tooling, not just to have a good idea
of how everything works, but to also write a tutorial for advanced beginners
that would cover how to build up everything from scratch.
I think that would be a better approach than just pointing to a repo with nearly
no documentation and a big Makefile.

### Configuring VS Code
Microsoft provides an official [VS Code extension for C and C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools).
It allows for syntax highlighting, error checking, debugging within the editor
itself, and what Microsoft calls [Intellisense](https://code.visualstudio.com/docs/editor/intellisense).
 (basically code completion with extra perks)

With such a niche environment though, we have to configure the extension so
it knows where to find the tooling and how it should process the code.
{Explain process to get to the config json, and what each option does}
[good stuff here](https://code.visualstudio.com/docs/cpp/c-cpp-properties-schema-reference)
and [here](https://code.visualstudio.com/docs/languages/cpp)
[here too](https://vscode.readthedocs.io/en/latest/languages/cpp/)
compiler comply to ANSI C, so C89

```json
{
    "configurations": [
        {
            "name": "PokeMini",
            "includePath": [
                "${workspaceFolder}/**",
                "C:/dev/c88-pokemini/include/**",
                "C:/dev/c88-pokemini/c88tools/include/**"
            ],
            "defines": ["VSCODE"],
            "compilerPath": "C:/dev/c88-pokemini/c88tools/bin/c88.exe",
            "cStandard": "c89",
            "intelliSenseMode": "gcc-x64"
        }
    ],
    "version": 4
}
```

## 2021/04/21
uuuuugh ADHD. Let’s do this one more time.
`windowsSdkversion` doesn’t seem to be needed.
c/c++ extension unable to resolve compiler path set to c88.exe
Sounds like it can’t use custom compilers for Intelisense. A bummer but nothing
too important. (see [this issue(https://github.com/microsoft/vscode-cpptools/issues/7146)])

## 2021/04/23
adhd orz
I opened a discussion on the C/C++ extension repo for VS Code and [I got a quick answer](https://github.com/microsoft/vscode-cpptools/discussions/7402#discussioncomment-642145).
Basically, adding support for a custom compiler is definitely not trivial, and
requires to pay the licence for a proprietary C/C++ front-end. However, not
having a compiler set up on `compilerPath` isn’t really a problem, as Intellisense
will have enough informations with the included files. So in the end, we can
set `compilerPath` to an empty tring, `""`.

There is two things we need to include for Intellisense to pick up:
* the `include/` folder from the S1C88 toolchain
* the [`include/pm.h`](https://github.com/pokemon-mini/c88-pokemini/tree/master/include/pm.h)
from the c88-pokemini repository, that defines registers and stuff with names, so
we don’t have to deal with hardcoded registers addresses.

Optionally, you can also use the [`include/stdint.h`](https://github.com/pokemon-mini/c88-pokemini/tree/master/include/stdint.h)
that defines the variable types we get from C99, since the compiler supports only
ANSI C (C89), so that you get more explicit sizes.

there is a `forcedInclude` field, using it to force the include of C88.H seem
to solve any kind of error/warning.


[tup]: http://gittup.org/tup
