# cobol-playground

This is a place where I experiment with cobol (GnuCOBOL 3.1.2). Find it [here](https://gnucobol.sourceforge.io).

## Setting up an WSL2 environment

The WSL2 should be set up with, for example, ubuntu for the next steps.

More on that from microsoft: [Install Linux on Windows with WSL](https://docs.microsoft.com/en-us/windows/wsl/install).

```bash
mkdir -p "~/software"
cd "~/software"

wget "https://sourceforge.net/projects/gnucobol/files/gnucobol/3.1/gnucobol-3.1.2.tar.gz/download" "gnucobol-3.1.2.tar.gz"
tar -xvf "gnucobol-3.1.2.tar.gz"

cd "gnucobol-3.1.2"
```

## System library dependencies

```bash
sudo apt-get install build-essential make git
sudo apt-get install libdb-dev libgmp3-dev libncurses5-dev libncursesw5-dev libxml2-dev libcjson-dev
```

## Build configuration

```bash
./configure
```

```
configure: GnuCOBOL Configuration:
configure:  CC                gcc
configure:  CFLAGS            -O2 -pipe -finline-functions -fsigned-char -Wall -Wwrite-strings -Wmissing-prototypes -Wno-format-y2k
configure:  LDFLAGS            -Wl,-z,relro,-z,now,-O1
configure:  COB_CC            gcc
configure:  COB_CFLAGS         -pipe -I/usr/local/include -Wno-unused -fsigned-char -Wno-pointer-sign
configure:  COB_LDFLAGS
configure:  COB_DEBUG_FLAGS   -ggdb3 -fasynchronous-unwind-tables
configure:  COB_LIBS          -L${exec_prefix}/lib -lcob -lm
configure:  COB_CONFIG_DIR    ${datarootdir}/gnucobol/config
configure:  COB_COPY_DIR      ${datarootdir}/gnucobol/copy
configure:  COB_LIBRARY_PATH  ${exec_prefix}/lib/gnucobol
configure:  COB_OBJECT_EXT    o
configure:  COB_MODULE_EXT    so
configure:  COB_EXE_EXT
configure:  COB_SHARED_OPT    -shared
configure:  COB_PIC_FLAGS     -fPIC -DPIC
configure:  COB_EXPORT_DYN    -Wl,--export-dynamic
configure:  COB_STRIP_CMD     strip --strip-unneeded
configure:  Dynamic loading:                             System
configure:  Use gettext for international messages:      yes
configure:  Use fcntl for file locking:                  yes
configure:  Use math multiple precision library:         gmp
configure:  Use curses library for screen I/O:           ncursesw
configure:  Use Berkeley DB for INDEXED I/O:             yes
configure:  Used for XML I/O:                            libxml2
configure:  Used for JSON I/O:                           cjson
```

## Next Steps

```bash
make

# very important to make sure everything works fine, around 700 internal tests
make check
# run against a NIST COBOL-85 test suite
make test

sudo make install
# this will likely print some warning about a symbolic link
sudo ldconfig
```

Also edit `~/.profile`:

```bash
# ...

# set up cobol custom build
SW_GNUCOBOL_PATH="$HOME/software/gnucobol-3.1.2"
LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$SW_GNUCOBOL_PATH/lib"
PATH="$PATH:$SW_GNUCOBOL_PATH/bin"
```

## Reference

* [GnuCOBOL Wikipedia](https://en.wikipedia.org/wiki/GnuCOBOL)
* [GnuCOBOL FAQ](https://gnucobol.sourceforge.io/faq/index.html#building-gnucobol-3-0-release-candidates)
* Blog Post by Michael Hönnig ["Try Cobol in 2020 on your PC"](https://michael.hoennig.de/blog/2020/2020-10-30-cobol-in2020-try-it-on-your-PC.html)
* There are PDF hard-copies attached to this repository, taken from the official documentation.
  * [GnuCOBOL 3.2 Programmers Guide](gnucobpg-a4.pdf)
  * [GnuCOBOL 3.2 Quick Reference](gnucobqr-a4.pdf)
  * [GnuCOBOL 3.2 Sample Programs](gnucobsp-a4.pdf)

## Code editor

There are many editors out there (Notepad++, Vim to name some). The extensions for cobol supported available for Visual Studio Code provide sufficient highlighting and linting. Use the "COBOL" by "BitLang".

## A simple program

A simple `Hello world` program may look like this:

```cobol
       >> SOURCE FORMAT IS FREE
      *> GnuCOBOL - "Hello world" example
       IDENTIFICATION DIVISION.
       PROGRAM-ID.     hello-world.
       AUTHOR.         machinateur.
       PROCEDURE DIVISION.
           display "Hello world".
           STOP RUN.

```

See also [`hello-world.cob`](hello-world.cob).

## License

It's MIT.

The GnuCOBOL software itself is licensed under GNU GPLv3.
