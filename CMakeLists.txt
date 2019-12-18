# Remmina - The GTK+ Remote Desktop Client
#
# Copyright (C) 2011 Marc-Andre Moreau
# Copyright (C) 2014-2015 Antenore Gatta, Fabio Castelli, Giovanni Panozzo
# Copyright (C) 2016-2019 Fabio Castelli (Muflone) <muflone@muflone.com>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin Street, Fifth Floor,
# Boston, MA  02110-1301, USA.
#
# In addition, as a special exception, the copyright holders give
# permission to link the code of portions of this program with the
# OpenSSL library under certain conditions as described in each
# individual source file, and distribute linked combinations
# including the two.
# You must obey the GNU General Public License in all respects
# for all of the code used other than OpenSSL. If you modify
# file(s) with this exception, you may extend this exception to your
# version of the file(s), but you are not obligated to do so. If you
# do not wish to do so, delete this exception statement from your
# version. If you delete this exception statement from all source
# files in the program, then also delete it here.


cmake_minimum_required(VERSION 2.8)

set(CMAKE_COLOR_MAKEFILE ON)

project(Remmina C)

include(CheckIncludeFiles)
include(CheckLibraryExists)
include(FindPkgConfig)
include(CheckCCompilerFlag)
include(GNUInstallDirs)

set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/)

include(FindOptionalPackage)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE "Release")
endif()

if(NOT BUILD_SHARED_LIBS)
  set(BUILD_SHARED_LIBS ON)
endif()

if(CMAKE_COMPILER_IS_GNUCC)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
  if(WITH_SSE2)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -msse2")
  endif()
  CHECK_C_COMPILER_FLAG(-Wno-unused-result Wno-unused-result)
  if(Wno-unused-result)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-result")
  endif()
  CHECK_C_COMPILER_FLAG(-Wno-unused-but-set-variable Wno-unused-but-set-variable)
  if(Wno-unused-but-set-variable)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-but-set-variable")
  endif()
  CHECK_C_COMPILER_FLAG(-Wno-unused-function Wno-unused-function)
  if(Wno-unused-function)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-unused-function")
  endif()
endif()

check_include_files(sys/param.h HAVE_SYS_PARAM_H)
check_include_files(fcntl.h HAVE_FCNTL_H)
check_include_files(unistd.h HAVE_UNISTD_H)
check_include_files(sys/un.h HAVE_SYS_UN_H)
check_include_files(errno.h HAVE_ERRNO_H)

include_directories(.)

set(REMMINA_DATADIR "${CMAKE_INSTALL_PREFIX}/share")
set(REMMINA_LOCALEDIR "${CMAKE_INSTALL_PREFIX}/share/locale")
set(REMMINA_PLUGINDIR "${CMAKE_INSTALL_PREFIX}/lib/remmina/plugins")
set(REMMINA_UIDIR "${CMAKE_INSTALL_PREFIX}/share/remmina/ui")

set(GTK_VERSION 3)
find_required_package(GTK)
find_required_package(PTHREAD)

set(REMMINA_COMMON_INCLUDE_DIRS ${GTK_INCLUDE_DIRS})
set(REMMINA_COMMON_LIBRARIES ${GTK_LIBRARIES} ${PTHREAD_LIBRARY})

include_directories(${REMMINA_COMMON_INCLUDE_DIRS})

if(WITH_TRACE_CALLS)
	message(STATUS "Enabling trace calls.")
	add_definitions(-DWITH_TRACE_CALLS)
endif()

if(REMMINA_VER_1_1)
	message(STATUS "Setup for Remmina 1.1 and older.")
	add_definitions(-DREMMINA_VER_1_1)
endif()

add_subdirectory(remmina-plugin-to-build)

configure_file(${CMAKE_CURRENT_SOURCE_DIR}/config.h.in ${CMAKE_CURRENT_SOURCE_DIR}/config.h)
