# Btop Git Package
## Arch Linux Build
To build:

    $ makepkg

## Termux Build
To build:

    $ cd build_env/
    $ ./build.nu ../packages/btop-git/

In *Docker*:

    $ TERMUX=1 CARCH=aarch64 makepkg --nodeps
