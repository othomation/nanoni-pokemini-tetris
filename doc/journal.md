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
The CPU was found to be from the S1C88 family of microcontroller by Epson.
It has its own official compiler you can find on [archive.org](https://web.archive.org/web/20190411141705/www.epsondevice.com/products_and_drivers/semicon/products/micro_controller/zip/s5u1c88000c16.zip).
Sadly it is only available natively on Windows. If you’re on Linux or Mac, you
have to make a Windows VM or install wine. The two links I posted above provide
a Makefile already configured to be able to use the compiler through wine.

It has a `doc/` directory which I encourage you to explore. The documentation has
a pdf for how the CPU behave and another one on the compiler itself and the
tools around it.

#### Compiling and running a hello world
To make sure everything is setup properly I’m going to build a minimal project.
I’m going with [tup] as my build system instead of `make` as I want to learn
more about it. I also don’t want to spend too much time writing a Makefile.

[tup]: http://gittup.org/tup
