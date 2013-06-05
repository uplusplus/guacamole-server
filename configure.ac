# ***** BEGIN LICENSE BLOCK *****
# Version: MPL 1.1/GPL 2.0/LGPL 2.1
#
# The contents of this file are subject to the Mozilla Public License Version
# 1.1 (the "License"); you may not use this file except in compliance with
# the License. You may obtain a copy of the License at
# http://www.mozilla.org/MPL/
#
# Software distributed under the License is distributed on an "AS IS" basis,
# WITHOUT WARRANTY OF ANY KIND, either express or implied. See the License
# for the specific language governing rights and limitations under the
# License.
#
# The Original Code is guacamole-server.
#
# The Initial Developer of the Original Code is
# Michael Jumper.
# Portions created by the Initial Developer are Copyright (C) 2010
# the Initial Developer. All Rights Reserved.
#
# Contributor(s):
#
# Alternatively, the contents of this file may be used under the terms of
# either the GNU General Public License Version 2 or later (the "GPL"), or
# the GNU Lesser General Public License Version 2.1 or later (the "LGPL"),
# in which case the provisions of the GPL or the LGPL are applicable instead
# of those above. If you wish to allow use of your version of this file only
# under the terms of either the GPL or the LGPL, and not to allow others to
# use your version of this file under the terms of the MPL, indicate your
# decision by deleting the provisions above and replace them with the notice
# and other provisions required by the GPL or the LGPL. If you do not delete
# the provisions above, a recipient may use your version of this file under
# the terms of any one of the MPL, the GPL or the LGPL.
#
# ***** END LICENSE BLOCK *****


AC_PREREQ([2.61])
AC_INIT([guacamole-server], [0.8.0])
AM_INIT_AUTOMAKE([-Wall -Werror foreign])

LT_PREREQ([2.2])
LT_INIT([dlopen])

AC_CONFIG_MACRO_DIR([m4])

# Programs
AC_PROG_CC
AC_PROG_CC_C99
AC_PROG_LIBTOOL

# Headers
AC_CHECK_HEADERS([fcntl.h stdlib.h string.h sys/socket.h time.h sys/time.h syslog.h unistd.h cairo/cairo.h pngstruct.h])

# Source characteristics
AC_DEFINE([_POSIX_C_SOURCE], [199309L], [Uses POSIX APIs])
AC_DEFINE([_BSD_SOURCE],     [],        [Uses BSD APIs])

# Libraries
AC_CHECK_LIB([dl],      [dlopen],,        AC_MSG_ERROR("libdl is required for loading client plugins"))
AC_CHECK_LIB([png],     [png_write_png],, AC_MSG_ERROR("libpng is required for writing png messages"))
AC_CHECK_LIB([cairo],   [cairo_create],,  AC_MSG_ERROR("cairo is required for drawing instructions"))
AC_CHECK_LIB([pthread], [pthread_create],
             [PTHREAD_LIBS=-lpthread
              AC_DEFINE([HAVE_LIBPTHREAD])])
AC_CHECK_LIB([cunit],   [CU_run_test],    [CUNIT_LIBS=-lcunit])
AC_CHECK_LIB([wsock32], [main])

AC_SUBST(PTHREAD_LIBS)
AC_SUBST(CUNIT_LIBS)

# Library functions
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_CHECK_FUNCS([clock_gettime gettimeofday memmove memset select strdup png_get_io_ptr nanosleep])

# Typedefs
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T

# Bundled libguac
AC_SUBST([LIBGUAC_LTLIB],   '$(top_builddir)/src/libguac/libguac.la')
AC_SUBST([LIBGUAC_INCLUDE], '-I$(top_srcdir)/src/libguac')

#
# Ogg Vorbis
#

have_vorbisenc=yes
VORBIS_LIBS=

AC_CHECK_HEADER(vorbis/vorbisenc.h,, [have_vorbisenc=no])
AC_CHECK_LIB([vorbisenc], [vorbis_encode_init], [VORBIS_LIBS="$VORBIS_LIBS -lvorbisenc"], [have_vorbisenc=no])
AM_CONDITIONAL([ENABLE_OGG], [test "x${have_vorbisenc}" = "xyes"])

#
# libVNCServer
#

have_libvncserver=yes
VNC_LIBS=

AC_CHECK_LIB([vncclient], [rfbInitClient], [VNC_LIBS="$VNC_LIBS -lvncclient"], [have_libvncserver=no])
AM_CONDITIONAL([ENABLE_VNC], [test "x${have_libvncserver}" = "xyes"])

#
# FreeRDP
#

have_freerdp=yes
RDP_LIBS=

AC_CHECK_LIB([freerdp-cache],    [glyph_cache_register_callbacks], [RDP_LIBS="$RDP_LIBS -lfreerdp-cache"], [have_freerdp=no])
AC_CHECK_LIB([freerdp-core],     [freerdp_new], [RDP_LIBS="$RDP_LIBS -lfreerdp-core"], [have_freerdp=no])
AC_CHECK_LIB([freerdp-channels], [freerdp_channels_new], [RDP_LIBS="$RDP_LIBS -lfreerdp-channels"], [have_freerdp=no])
AC_CHECK_LIB([freerdp-utils],    [xzalloc], [RDP_LIBS="$RDP_LIBS -lfreerdp-utils"], [have_freerdp=no])
AC_CHECK_LIB([freerdp-codec],    [freerdp_image_convert], [RDP_LIBS="$RDP_LIBS -lfreerdp-codec"], [have_freerdp=no])
AM_CONDITIONAL([ENABLE_RDP], [test "x${have_freerdp}" = "xyes"])

#
# libssh
#

have_libssh=yes
SSH_LIBS=

AC_CHECK_LIB([ssh], [ssh_new], [SSH_LIBS="$SSH_LIBS -lssh"], [have_libssh=no])
PKG_CHECK_MODULES([PANGO], pango);
PKG_CHECK_MODULES([PANGOCAIRO], pangocairo);
AM_CONDITIONAL([ENABLE_SSH], [test "x${have_libssh}" = "xyes"])

#
# SPICE
#

have_spice=yes
PKG_CHECK_MODULES([SPICE_CLIENT_GLIB], [spice-client-glib-2.0],, [have_spice=no])
PKG_CHECK_MODULES([GLIB], [glib-2.0],,                           [have_spice=no])
AM_CONDITIONAL([ENABLE_SPICE], [test "x${have_spice}" = "xyes"])

echo "
$PACKAGE_NAME version $PACKAGE_VERSION:

   Protocol support:
      RDP....... ${have_freerdp}
      SPICE..... ${have_spice}
      SSH....... ${have_libssh}
      VNC....... ${have_libvncserver}

"

AC_CONFIG_FILES([Makefile
                 tests/Makefile
                 src/libguac/Makefile
                 src/guacd/Makefile
                 src/protocols/rdp/Makefile
                 src/protocols/spice/Makefile
                 src/protocols/ssh/Makefile
                 src/protocols/vnc/Makefile])
AC_OUTPUT
