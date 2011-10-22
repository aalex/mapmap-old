# Process this file with autoconf to produce a configure script.

# mapmap package version number,
# An odd micro number indicates in-progress development from Git
# An even micro number indicates a released version
#
# Making a point release:
# - increase mapmap_micro_version to the next even number
# After the release:
# - increase mapmap_micro_version to the next odd number
m4_define([mapmap_major_version], [0])
m4_define([mapmap_minor_version], [1])
m4_define([mapmap_micro_version], [3])

m4_define([mapmap_release_status],
          [m4_if(m4_eval(mapmap_micro_version % 2), [1], [git],
                 [m4_if(m4_eval(mapmap_minor_version % 2), [1], [snapshot],
                                                                 [release])])])

m4_define([mapmap_version], [mapmap_major_version.mapmap_minor_version.mapmap_micro_version])

AC_PREREQ([2.63])
AC_INIT([mapmap],
        [mapmap_version],
        [http://github.com/aalex/mapmap/issues],
        [mapmap],
        [http://github.com/aalex/mapmap])
AC_CONFIG_MACRO_DIR([m4])
AC_MSG_NOTICE([Configuring mapmap])
AM_INIT_AUTOMAKE([subdir-objects -Wno-portability])
AC_CONFIG_HEADERS([src/config.h])
AC_CANONICAL_HOST # fills $host variable

AC_SUBST([TEMPI_MAJOR_VERSION],  [mapmap_major_version])
AC_SUBST([TEMPI_MINOR_VERSION],  [mapmap_minor_version])
AC_SUBST([TEMPI_MICRO_VERSION],  [mapmap_micro_version])
AC_SUBST([TEMPI_VERSION],        [mapmap_version])
AC_SUBST([TEMPI_RELEASE_STATUS], [mapmap_release_status])

# Checks for programs.
AC_PROG_CC
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_LIBTOOL

AC_CHECK_PROGS([DOXYGEN], [doxygen])
if test -z "$DOXYGEN";
    then AC_MSG_WARN([Doxygen not found - continuing without Doxygen support])
fi
AM_CONDITIONAL([HAVE_DOXYGEN], [test -n "$DOXYGEN"])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_INLINE
AC_TYPE_SIZE_T
AC_CHECK_TYPES([ptrdiff_t])

# Checks for libraries
AC_CHECK_LIB([m],[sqrt])

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h stddef.h stdlib.h string.h sys/time.h])

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_CHECK_FUNCS([gettimeofday memmove mkdir setenv sqrt strchr strpbrk uname])

AX_PTHREAD

# Glib
PKG_CHECK_MODULES([GLIB], [glib-2.0 >= 2.10.2], have_glib=true, have_glib=false)
if test "x${have_glib}" = "xfalse" ; then
    AC_MSG_ERROR([missing package: libglib-2.0-dev])
fi
AC_SUBST([GLIB_LIBS])
AC_SUBST([GLIB_CFLAGS])


# GST
PKG_CHECK_MODULES([GST], [gstreamer-0.10 >= 0.10.0 gstreamer-interfaces-0.10 >= 0.10.19])
AC_SUBST([GST_LIBS])
AC_SUBST([GST_CFLAGS])

# Clutter
PKG_CHECK_MODULES([CLUTTER], [clutter-1.0 >= 1.0], have_clutter=true, have_clutter=false)
if test "x${have_clutter}" = "xfalse" ; then
    AC_MSG_ERROR([missing package: libclutter-1.0-dev])
fi
AC_SUBST([CLUTTER_LIBS])
AC_SUBST([CLUTTER_CFLAGS])

# Clutter-GST
PKG_CHECK_MODULES([CLUTTERGST], [clutter-gst-1.0 >= 1.0], have_clutter_gst=true, 
    PKG_CHECK_MODULES([CLUTTERGST], [clutter-gst-0.10 >= 0.10], have_clutter_gst=true, have_clutter_gst=false))
if test "x${have_clutter_gst}" = "xfalse" ; then
    AC_MSG_ERROR([missing package: libclutter-gst-dev])
fi
AC_SUBST([CLUTTERGST_LIBS])
AC_SUBST([CLUTTERGST_CFLAGS])

# libxml2
PKG_CHECK_MODULES([LIBXML], [libxml-2.0], have_libxml=true, have_libxml=false)
if test "x${have_libxml}" = "xfalse" ; then
    AC_MSG_ERROR([missing package: libxml2-dev])
fi
AC_SUBST([LIBXML_LIBS])
AC_SUBST([LIBXML_CFLAGS])

# BOOST
AX_BOOST_BASE([1.35])

AX_BOOST_SIGNALS
if test "x${ax_cv_boost_signals}" = "xno" ; then
	AC_MSG_ERROR([missing package: libboost-signals-dev])
fi

#Set LIBLO with pkg-config
PKG_CHECK_MODULES(LIBLO, liblo, have_liblo=true, have_liblo=false)
if test "x${have_liblo}" = "xfalse" ; then
    AC_MSG_ERROR([Missing liblo])
fi

AX_BOOST_PROGRAM_OPTIONS
if test "x${ax_cv_boost_program_options}" = "xno" ; then
	AC_MSG_ERROR([missing package: libboost-program-options-dev])
fi

AX_BOOST_FILESYSTEM
if test "x${ax_cv_boost_filesystem}" = "xno" ; then
	AC_MSG_ERROR([missing package: libboost-filesystem-dev])
fi

CFLAGS+=" -Wall -Wextra -Wfatal-errors -Werror "

# GNU help2man creates man pages from --help output; in many cases, this
# is sufficient, and obviates the need to maintain man pages separately.
# However, this means invoking executables, which we generally cannot do
# when cross-compiling, so we test to avoid that (the variable
# "cross_compiling" is set by AC_PROG_CC).
if test $cross_compiling = no; then
    AM_MISSING_PROG(HELP2MAN, help2man)
else
    HELP2MAN=:
fi

AC_CONFIG_FILES([
        Makefile 
        src/Makefile
        tests/Makefile
        doxyfile 
        data/Makefile
        ])

AC_OUTPUT

