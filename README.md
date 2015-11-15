remmina-plugin-builder [![Build Status](https://travis-ci.org/muflone/remmina-plugin-builder.svg?branch=master)](https://travis-ci.org/muflone/remmina-plugin-builder)
======================

Remmina minimal build environment for plugins.

To build a plugin for Remmina simply put it into the remmina-plugin-to-build directory
and compile it using:

    cmake -DCMAKE_INSTALL_PREFIX=/usr .
    make
    make DESTDIR=/destination_path install

To build a plugin for Remmina 1.1 and older versions use this:

    cmake -DCMAKE_INSTALL_PREFIX=/usr -DREMMINA_VER_1_1=ON .
    make
    make DESTDIR=/destination_path install
