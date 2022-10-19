# musl gcc vs. musl clang example

This repository is a complete and simple example of some unexpected behavior
from the `musl-clang` wrapper that the `musl-gcc` wrapper does not display. In
particular, the behavior is a number of warnings when compiling object files.
As a complete example:

    $ docker-compose build
    $ docker-compose run gcc -c hello.c
    $ docker-compose run clang -c hello.c
    clang: warning: argument unused during compilation: '-fuse-ld=musl-clang' [-Wunused-command-line-argument]
    clang: warning: argument unused during compilation: '-static-libgcc' [-Wunused-command-line-argument]
    clang: warning: argument unused during compilation: '-L-user-start' [-Wunused-command-line-argument]
    clang: warning: argument unused during compilation: '-L/usr/local/lib' [-Wunused-command-line-argument]
    clang: warning: argument unused during compilation: '-L-user-end' [-Wunused-command-line-argument]

The relevant unused arguments are all contained in the `musl-clang` wrapper
script. These *can* be suppressed via `-Wno-unused-command-line-argument`,
but given this occurs on such a simple use-case, I wanted to suggest that
perhaps it's not entirely intended behavior.
