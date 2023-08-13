# remmina-plugin-builder

[![Travis CI Build Status](https://img.shields.io/travis/com/muflone/remmina-plugin-builder/master.svg)](https://www.travis-ci.com/github/muflone/remmina-plugin-builder)
[![CircleCI Build Status](https://img.shields.io/circleci/project/github/muflone/remmina-plugin-builder/master.svg)](https://circleci.com/gh/muflone/remmina-plugin-builder)

**Description:** Remmina minimal build environment for plugins.

**Copyright:** 2013-2023 Fabio Castelli (Muflone) <muflone(at)muflone.com>

**License:** GPL-2+

**Source code:** https://github.com/muflone/remmina-plugin-builder/

**Documentation:** http://www.muflone.com/remmina-plugin-builder/

# Description

**Remmina Plugin Builder** is used to ease the development of plugins for
Remmina as it includes all the header files, structs and enums needed to
build a new plugin for Remmina.

# Usage

To build a plugin for Remmina simply put it into the `remmina-plugin-to-build`
directory and compile it using:

    cmake -DCMAKE_INSTALL_PREFIX=/usr .
    make
    make DESTDIR=/destination_path install
