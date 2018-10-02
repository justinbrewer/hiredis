ACLOCAL_AMFLAGS = -I m4
ARFLAGS = rcs

AM_CPPFLAGS = -I$(srcdir)
AM_CFLAGS = -Wall -W -Wstrict-prototypes -Wwrite-strings
AM_LDFLAGS =

if SOLARIS
AM_LDFLAGS += -ldl -lnsl -lsocket
endif

if CYGWIN
AM_LDFLAGS += -no-undefined
endif

pkgconfig_DATA = hiredis.pc

lib_LTLIBRARIES = libhiredis.la

libhiredis_la_SOURCES = net.c hiredis.c sds.c async.c read.c
libhiredis_la_LDFLAGS = $(AM_LDFLAGS) -version-number $(HIREDIS_VNUM)

pkginclude_HEADERS = hiredis.h async.h read.h sds.h hiredis-version.h
noinst_HEADERS = fmacros.h net.h sdsalloc.h dict.h dict.c

adaptersincludedir = $(pkgincludedir)/adapters
adaptersinclude_HEADERS = adapters/ae.h \
			  adapters/glib.h \
			  adapters/ivykis.h \
			  adapters/libev.h \
			  adapters/libevent.h \
			  adapters/libuv.h \
			  adapters/macosx.h \
			  adapters/qt.h

check_PROGRAMS = hiredis-test
hiredis_test_SOURCES = test.c
hiredis_test_LDADD = libhiredis.la

dist_check_SCRIPTS = test.sh
TESTS = test.sh
test.sh: hiredis-test

EXAMPLE_CFLAGS = $(AM_CFLAGS) -I$(srcdir)/adapters

noinst_PROGRAMS = hiredis-example
hiredis_example_SOURCES = examples/example.c
hiredis_example_LDADD = libhiredis.la

if LIBEVENT
noinst_PROGRAMS += hiredis-example-libevent
hiredis_example_libevent_SOURCES = examples/example-libevent.c
hiredis_example_libevent_CFLAGS = $(EXAMPLE_CFLAGS) $(LIBEVENT_CFLAGS)
hiredis_example_libevent_LDFLAGS = $(AM_LDFLAGS) $(LIBEVENT_LIBS)
hiredis_example_libevent_LDADD = libhiredis.la
endif

if LIBEV
noinst_PROGRAMS += hiredis-example-libev
hiredis_example_libev_SOURCES = examples/example-libev.c
hiredis_example_libev_CFLAGS = $(EXAMPLE_CFLAGS) $(LIBEV_CFLAGS)
hiredis_example_libev_LDFLAGS = $(AM_LDFLAGS) $(LIBEV_LIBS)
hiredis_example_libev_LDADD = libhiredis.la
endif

if GLIB
noinst_PROGRAMS += hiredis-example-glib
hiredis_example_glib_SOURCES = examples/example-glib.c
hiredis_example_glib_CFLAGS = $(EXAMPLE_CFLAGS) $(GLIB_CFLAGS)
hiredis_example_glib_LDFLAGS = $(AM_LDFLAGS) $(GLIB_LIBS)
hiredis_example_glib_LDADD = libhiredis.la
endif

if IVYKIS
noinst_PROGRAMS += hiredis-example-ivykis
hiredis_example_ivykis_SOURCES = examples/example-ivykis.c
hiredis_example_ivykis_CFLAGS = $(EXAMPLE_CFLAGS) $(IVYKIS_CFLAGS)
hiredis_example_ivykis_LDFLAGS = $(AM_LDFLAGS) $(IVYKIS_LIBS)
hiredis_example_ivykis_LDADD = libhiredis.la
endif

if MACOSX_EXAMPLE
noinst_PROGRAMS += hiredis-example-macosx
hiredis_example_macosx_SOURCES = examples/example-macosx.c
hiredis_example_macosx_CFLAGS = $(EXAMPLE_CFLAGS)
hiredis_example_macosx_LDFLAGS = $(AM_LDFLAGS) -framework CoreFoundation
hiredis_example_macosx_LDADD = libhiredis.la
endif

if AE
noinst_PROGRAMS += hiredis-example-ae
hiredis_example_ae_SOURCES = examples/example-ae.c
hiredis_example_ae_CFLAGS = $(EXAMPLE_CFLAGS) $(AE_CFLAGS)
hiredis_example_ae_LDFLAGS = $(AM_LDFLAGS) $(AE_LIBS)
hiredis_example_ae_LDADD = libhiredis.la
endif

if LIBUV
noinst_PROGRAMS += hiredis-example-libuv
hiredis_example_libuv_SOURCES = examples/example-libuv.c
hiredis_example_libuv_CFLAGS = $(EXAMPLE_CFLAGS) $(LIBUV_CFLAGS)
hiredis_example_libuv_LDFLAGS = $(AM_LDFLAGS) $(LIBUV_LIBS)
hiredis_example_libuv_LDADD = libhiredis.la
endif

if QT
noinst_PROGRAMS += hiredis-example-qt
hiredis_example_qt_SOURCES = examples/example-qt.cpp
nodist_hiredis_example_qt_SOURCES = examples/example-qt_moc.cpp \
				    adapters/qt_moc.cpp
hiredis_example_qt_CXXFLAGS = $(EXAMPLE_CFLAGS) -std=c++11 $(QT_CFLAGS)
hiredis_example_qt_LDFLAGS = $(AM_LDFLAGS) -std=c++11 $(QT_LIBS)
hiredis_example_qt_LDADD = libhiredis.la

.h_moc.cpp:
	$(MKDIR_P) `dirname $@`
	$(QT_MOC) -o $@ $(AM_CPPFLAGS) $(QT_CFLAGS) $<

SUFFIXES = .h _moc.cpp
endif

VALGRIND_FLAGS = --gen-suppressions=all
VALGRIND_memcheck_FLAGS = --track-origins=yes --leak-check=full --show-reachable=no
VALGRIND_SUPPRESSIONS_FILES = $(srcdir)/.valgrind.supp
@VALGRIND_CHECK_RULES@
