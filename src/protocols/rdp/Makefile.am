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
# The Original Code is libguac-client-rdp.
#
# The Initial Developer of the Original Code is
# Michael Jumper.
# Portions created by the Initial Developer are Copyright (C) 2011
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

AUTOMAKE_OPTIONS = foreign

ACLOCAL_AMFLAGS = -I m4
AM_CFLAGS = -Werror -Wall -Iinclude @LIBGUAC_INCLUDE@

lib_LTLIBRARIES = libguac-client-rdp.la
freerdp_LTLIBRARIES = guac_rdpsnd.la guac_rdpdr.la

libguac_client_rdp_la_SOURCES = \
	audio.c                     \
	client.c                    \
	default_pointer.c           \
	guac_handlers.c             \
	rdp_bitmap.c                \
	rdp_cliprdr.c               \
	rdp_gdi.c                   \
	rdp_glyph.c                 \
	rdp_keymap_base.c           \
	rdp_keymap.c                \
	rdp_keymap_de_de.c          \
	rdp_keymap_failsafe.c       \
	rdp_keymap_fr_fr.c          \
	rdp_keymap_en_us.c          \
	rdp_pointer.c               \
	wav_encoder.c

guac_rdpsnd_la_SOURCES =          \
	guac_rdpsnd/rdpsnd_messages.c \
	guac_rdpsnd/rdpsnd_service.c  \
	audio.c

guac_rdpdr_la_SOURCES =         \
	guac_rdpdr/rdpdr_messages.c \
	guac_rdpdr/rdpdr_printer.c  \
	guac_rdpdr/rdpdr_service.c

noinst_HEADERS =                     \
	guac_rdpdr/rdpdr_messages.h      \
	guac_rdpdr/rdpdr_printer.h       \
	guac_rdpdr/rdpdr_service.h       \
	guac_rdpsnd/rdpsnd_messages.h    \
	guac_rdpsnd/rdpsnd_service.h     \
	audio.h                          \
	client.h                         \
	config.h                         \
	default_pointer.h                \
	guac_handlers.h                  \
	rdp_bitmap.h                     \
	rdp_cliprdr.h                    \
	rdp_gdi.h                        \
	rdp_glyph.h                      \
	rdp_keymap.h                     \
	rdp_pointer.h                    \
	wav_encoder.h

# Compile OGG support if available
if ENABLE_OGG
    libguac_client_rdp_la_SOURCES += ogg_encoder.c
    noinst_HEADERS += ogg_encoder.h
endif

libguac_client_rdp_la_LDFLAGS = -version-info 0:0:0 @RDP_LIBS@ @VORBIS_LIBS@ @PTHREAD_LIBS@ @CAIRO_LIBS@
guac_rdpsnd_la_LDFLAGS = -module -avoid-version -shared @RDP_LIBS@ @VORBIS_LIBS@ @PTHREAD_LIBS@
guac_rdpdr_la_LDFLAGS = -module -avoid-version -shared @RDP_LIBS@ @PTHREAD_LIBS@

libguac_client_rdp_la_LIBADD = @LIBGUAC_LTLIB@
guac_rdpsnd_la_LIBADD = @LIBGUAC_LTLIB@
guac_rdpdr_la_LIBADD = @LIBGUAC_LTLIB@

freerdpdir = ${libdir}/freerdp

