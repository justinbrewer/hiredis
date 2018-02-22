m4_define([_MAJOR], [0])
m4_define([_MINOR], [13])
m4_define([_PATCH], [3])

# version-info a:b:c = soname (a-c).c.b
m4_define([_VINFO],[13:0:13])

AC_PREREQ([2.69])
AC_INIT([hiredis], [_MAJOR._MINOR._PATCH],
        [https://github.com/redis/hiredis])
AM_INIT_AUTOMAKE([-Wall -Werror foreign subdir-objects])
PKG_PROG_PKG_CONFIG
AC_CONFIG_MACRO_DIR([m4])

PKG_INSTALLDIR

AC_CONFIG_SRCDIR([hiredis.c])

# Platform checks.
AC_CANONICAL_HOST

case "${host_os}" in
  darwin*)
    build_macosx=yes
    ;;
  solaris*)
    build_solaris=yes
    ;;
  *cygwin*)
    build_cygwin=yes
    ;;
  *mingw*)
    AC_MSG_ERROR([mingw is not a supported platform])
    ;;
esac

AM_CONDITIONAL([MACOSX],[test "x$build_macosx" = "xyes"])
AM_CONDITIONAL([SOLARIS],[test "x$build_solaris" = "xyes"])
AM_CONDITIONAL([CYGWIN],[test "x$build_cygwin" = "xyes"])

# Checks for programs.
AC_PROG_AWK
AC_PROG_CXX
AC_PROG_CC
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AM_PROG_AR
AC_PROG_SED

LT_INIT([win32-dll])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h fcntl.h limits.h netdb.h netinet/in.h stdint.h stdlib.h string.h strings.h sys/socket.h sys/time.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_FUNC_STRERROR_R
AC_CHECK_FUNCS([gettimeofday memmove memset socket strcasecmp strchr strdup strerror strncasecmp strstr strtol])

# Checks for dependencies
m4_define([AVAILABLE_EXAMPLES],[libevent,libev,glib2,ivykis,ae,libuv,qt,macosx])

AC_ARG_ENABLE([examples],
  AS_HELP_STRING([--enable-examples=LIST],[Comma-separated list of examples to build. Defaults to none.
    Available examples: ]m4_translit(m4_defn([AVAILABLE_EXAMPLES]),[,],[ ])))

AC_MSG_CHECKING([examples to build])
if test "x$enable_examples" != "xno" && test "x$enable_examples" != "x"; then
  enable_examples=`echo "$enable_examples" | "$SED" 's/,/ /g'`
  for example in $enable_examples; do
    case $example in m4_foreach([NAME],[AVAILABLE_EXAMPLES], [
      NAME)
        enable_[]NAME=yes
        ;;])
      *)
        AC_MSG_RESULT([])
        AC_MSG_ERROR([unknown example: $example])
        ;;
    esac
  done
  AC_MSG_RESULT([$enable_examples])
else
  AC_MSG_RESULT([(none)])
fi

AM_CONDITIONAL([LIBEVENT], [test "x$enable_libevent" = "xyes"])
if test "x$enable_libevent" = "xyes"; then
  PKG_CHECK_MODULES([LIBEVENT], [libevent])
fi

AC_ARG_WITH([libev],
    AS_HELP_STRING([--with-libev=DIR],[Specify custom libev prefix]),
    [], [with_libev=no])
AM_CONDITIONAL([LIBEV], [test "x$enable_libev" = "xyes"])
if test "x$enable_libev" = "xyes"; then
  if test "x$with_libev" != "xno"; then
    AC_SUBST([LIBEV_CFLAGS],[-I$with_ev/include])
    AC_SUBST([LIBEV_LIBS],[-L$with_ev/lib -lev])
  else
    AC_SUBST([LIBEV_CFLAGS],[])
    AC_SUBST([LIBEV_LIBS],[-lev])
  fi
fi

AM_CONDITIONAL([GLIB], [test "x$enable_glib2" = "xyes"])
if test "x$enable_glib2" = "xyes"; then
  PKG_CHECK_MODULES([GLIB], [glib-2.0])
fi

AC_ARG_WITH([ivykis],
    AS_HELP_STRING([--with-ivykis=DIR],[Specify custom ivykis prefix]),
    [], [with_ivykis=no])
AM_CONDITIONAL([IVYKIS], [test "x$enable_ivykis" = "xyes"])
if test "x$enable_ivykis" = "xyes"; then
  if test "x$with_ivykis" != "xno"; then
    AC_SUBST([IVYKIS_CFLAGS],[-I$with_ivykis/include])
    AC_SUBST([IVYKIS_LIBS],[-L$with_ivykis/lib -livykis])
  else
    AC_SUBST([IVYKIS_CFLAGS],[])
    AC_SUBST([IVYKIS_LIBS],[-livykis])
  fi
fi

AC_ARG_WITH([ae],
    AS_HELP_STRING([--with-ae=DIR],[Specify custom ae prefix]),
    [], [with_ae=no])
AM_CONDITIONAL([AE], [test "x$enable_ae" = "xyes"])
if test "x$enable_ae" = "xyes"; then
  if test "x$with_ae" != "xno"; then
    AC_SUBST([AE_CFLAGS],[-I$with_ae])
    AC_SUBST([AE_LIBS],[$with_ae/ae.o $with_ae/zmalloc.o $with_ae/../deps/jemalloc/lib/libjemalloc.a -pthread])
  else
    AC_MSG_ERROR([--enable-examples=ae requires --with-ae=PATH])
  fi
fi

AM_CONDITIONAL([LIBUV], [test "x$enable_libuv" = "xyes"])
if test "x$enable_libuv" = "xyes"; then
  PKG_CHECK_MODULES([LIBUV], [libuv])
fi

AM_CONDITIONAL([QT], [test "x$enable_qt" = "xyes"])
if test "x$enable_qt" = "xyes"; then
  PKG_CHECK_MODULES([QT], [QtCore])
  PKG_CHECK_VAR([QT_MOC], [QtCore], [moc_location],
                [], [AC_MSG_ERROR([Unable to locate QT moc executable])])
fi

if test "x$enable_macosx" = "xyes" && test "x$build_macosx" != "xyes"; then
  AC_MSG_ERROR([macosx example can only be built on macosx])
fi
AM_CONDITIONAL([MACOSX_EXAMPLE], [test "x$enable_macosx" = "xyes"])

AC_SUBST([HIREDIS_MAJOR],[_MAJOR])
AC_SUBST([HIREDIS_MINOR],[_MINOR])
AC_SUBST([HIREDIS_PATCH],[_PATCH])
AC_SUBST([HIREDIS_VINFO],[_VINFO])

AC_CONFIG_FILES([Makefile hiredis.pc])
AC_OUTPUT
