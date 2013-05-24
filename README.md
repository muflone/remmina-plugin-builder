remmina-plugin-builder
======================

Remmina minimal build environment for plugins.

To build a plugin for Remmina simply put it into the remmina-plugin-to-build directory
and compile it using:

cmake -DCMAKE_INSTALL_PREFIX=/usr .
make
make DESTDIR=/destination_path install

