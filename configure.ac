#  Copyright 2005 Adam Jackson.
# 
#  Permission is hereby granted, free of charge, to any person obtaining a
#  copy of this software and associated documentation files (the "Software"),
#  to deal in the Software without restriction, including without limitation
#  on the rights to use, copy, modify, merge, publish, distribute, sub
#  license, and/or sell copies of the Software, and to permit persons to whom
#  the Software is furnished to do so, subject to the following conditions:
#
#  The above copyright notice and this permission notice (including the next
#  paragraph) shall be included in all copies or substantial portions of the
#  Software.
#
#  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
#  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
#  FITNESS FOR A PARTICULAR PURPOSE AND NON-INFRINGEMENT.  IN NO EVENT SHALL
#  ADAM JACKSON BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
#  IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
#  CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
#
# Process this file with autoconf to produce a configure script

AC_PREREQ(2.57)
AC_INIT([xlibre-xf86-video-freedreno],
        [25.0.0],
        [https://github.com/X11Libre/xf86-video-freedreno/issues],
        [xlibre-xf86-video-freedreno])

AC_CONFIG_SRCDIR([Makefile.am])
AM_CONFIG_HEADER([config.h])
AC_CONFIG_AUX_DIR(.)

AM_INIT_AUTOMAKE([dist-bzip2])

AM_MAINTAINER_MODE

# Require xorg-macros: XORG_DEFAULT_OPTIONS
m4_ifndef([XORG_MACROS_VERSION],
          [m4_fatal([must install xorg-macros 1.4 or later before running autoconf/autogen])])
XORG_MACROS_VERSION(1.4)
XORG_DEFAULT_OPTIONS

# Checks for programs.
AC_DISABLE_STATIC
AC_PROG_LIBTOOL
AC_PROG_CC
AM_PROG_AS

AH_TOP([#include "xorg-server.h"])

PKG_PROG_PKG_CONFIG([0.25])
AC_ARG_WITH(xorg-module-dir,
            AS_HELP_STRING([--with-xorg-module-dir=DIR],
                           [Default xorg module directory]),
            [moduledir="$withval"],
            [moduledir=`$PKG_CONFIG --variable=moduledir xorg-server`])

# Checks for pkg-config packages
PKG_CHECK_MODULES(XORG, [libdrm >= 2.4.54 libdrm_freedreno xorg-server >= 25.0.0 xproto libudev $REQUIRED_MODULES])
PKG_CHECK_MODULES(XEXT, [xextproto >= 7.1])

PKG_CHECK_MODULES([XATRACKER], [xatracker >= 2.2.0],
	BUILD_XA=yes; AC_DEFINE(HAVE_XA, 1, [xa state tracker available]),
	BUILD_XA=no)
AM_CONDITIONAL(BUILD_XA, [test "$BUILD_XA" = "yes"])

PKG_CHECK_MODULES(LIBUDEV, [libudev], [LIBUDEV=yes], [LIBUDEV=no])
if test "x$LIBUDEV" = xyes; then
	AC_DEFINE(HAVE_LIBUDEV, 1, [libudev support])
fi
AM_CONDITIONAL(LIBUDEV, [ test "x$LIBUDEV" = "xyes" ] )


# Define a configure option for an alternate X Server configuration directory
configdir=`$PKG_CONFIG --variable=sysconfigdir xorg-server`
AC_SUBST(configdir)

# Checks for header files.
AC_HEADER_STDC

AC_SUBST([moduledir])
AC_SUBST([CFLAGS])
AC_SUBST([CCASFLAGS])

#XORG_MANPAGE_SECTIONS
XORG_RELEASE_VERSION

AC_OUTPUT([Makefile src/Makefile man/Makefile conf/Makefile])
