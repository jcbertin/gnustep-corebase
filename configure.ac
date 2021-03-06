#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.60])
AC_INIT([libgnustep-corebase], [0.2], [bug-gnustep@gnu.org])
AC_CONFIG_SRCDIR([Source/CFBase.c])
AC_CONFIG_HEADERS([Source/config.h])

# Checks for programs.
AC_PROG_CC

# Checks for header files.
AC_CHECK_HEADERS([fcntl.h float.h limits.h stddef.h stdint.h stdlib.h string.h sys/time.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_INLINE
AC_STDC_HEADERS
AC_TYPE_INT16_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_TYPE_INT8_T
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T
AC_CHECK_TYPES([ptrdiff_t])
AC_C_BIGENDIAN
AC_CHECK_SIZEOF([void *])

# Checks for library functions.
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_CHECK_FUNCS([floor getcwd gettimeofday memmove memset mkdir modf pow rmdir strncasecmp])

#---
# Check for the Objective-C runtime
#---
AC_ARG_ENABLE([objc-bridge],
  [AS_HELP_STRING([--disable-objc-bridge],
    [Disables the Objective-C toll-free bridge mechanism.])],
  [enable_objc_bridge=$enableval], [enable_objc_bridge=yes])
if test "$enable_objc_bridge" = "yes"; then
  AC_PATH_PROG(GNUSTEP_CONFIG, [gnustep-config], [no])
  if test "$GNUSTEP_CONFIG" != "no"; then
    OBJC_LIBS=`$GNUSTEP_CONFIG --base-libs`
    LIBS="$LIBS $OBJC_LIBS"
    AC_DEFINE(HAVE_LIBOBJC, 1)
  else
    AC_CHECK_HEADER(objc/runtime.h)
    if test "$ac_cv_header_objc_runtime_h" = "no"; then
      AC_MSG_ERROR([Could not find the Objective-C runtime header.])
    fi
    
    AC_CHECK_LIB(objc, objc_getClass)
    if test "$ac_cv_lib_objc_objc_getClass" = "no"; then
      AC_CHECK_LIB(objc2, objc_getClass)
      if test "$ac_cv_lib_objc2_objc_getClass" = "no"; then
        AC_MSG_ERROR([Objective-C library not found!  Use --disable-objc-bridge to explicitly disable the toll-free bridge or install the Objective-C library.])
      fi
    fi
  fi
fi

#---
# Check for ICU
#---
ICU_MIN_VERSION='4.0'
#AC_ARG_WITH([icu],
#  [AS_HELP_STRING([--without-icu],
#    [Do not build with International Components for Unicode (ICU) support.])],
#  [], [with_icu=yes])
#if test "$with_icu" = "yes"; then
  # Adapted from icu.m4 in GNUstep-base
  AC_PATH_PROG(ICU_CONFIG, icu-config, [no])
  if test "$ICU_CONFIG" = "no"; then
    AC_MSG_ERROR([Could not find 'icu-config'.  Please install ICU or specify --without-icu.])
  fi
  
  AC_MSG_CHECKING(for ICU $ICU_MIN_VERSION or later)
  ICU_VERSION=`$ICU_CONFIG --version`
  found=`expr $ICU_VERSION \>= $ICU_MIN_VERSION`
  if test "$found" = "1"; then
    AC_MSG_RESULT(yes)
    ICU_LIBS=`$ICU_CONFIG --ldflags-libsonly`
    LIBS="$LIBS $ICU_LIBS"
  else
    AC_MSG_RESULT(no)
    AC_MSG_ERROR([Could not find ICU $ICU_MIN_VERSION or later])
  fi
  
  AC_CHECK_HEADERS([unicode/uloc.h unicode/ulocdata.h unicode/ucol.h unicode/ucurr.h unicode/ucal.h unicode/unum.h unicode/udat.h unicode/udatpg.h unicode/usearch.h])
  AC_DEFINE(HAVE_ICU, 1,
            [Define to 1 if you have International Components for Unicode.])
#fi

#---
# Check for zoneinfo directory
#---
AC_ARG_WITH([zoneinfo-dir],
  [AS_HELP_STRING([--with-zoneinfo-dir=DIR],
    [Directory of the Time zone object files.])],
  [], [with_zoneinfo_dir=undef])
AC_MSG_CHECKING([zoneinfo directory])
if test "$with_zoneinfo_dir" = "undef"; then
  # Try to find the a time zone database directory
  if test -d "/usr/share/zoneinfo"; then
    with_zoneinfo_dir='/usr/share/zoneinfo'
  elif test -d "/usr/lib/zoneinfo"; then
    with_zoneinfo_dir='/usr/lib/zoneinfo'
  elif test -d "/usr/local/share/zoneinfo"; then
    with_zoneinfo_dir='/usr/local/share/zoneinfo'
  elif test -d "/usr/local/lib/zoneinfo"; then
    with_zoneinfo_dir='/usr/local/lib/zoneinfo'
  elif test -d "/etc/zoneinfo"; then
    with_zoneinfo_dir='/etc/zoneinfo'
  elif test -d "/usr/local/etc/zoneinfo"; then
    with_zoneinfo_dir='/usr/local/etc/zoneinfo'
  else
    with_zoneinfo_dir='no'
  fi
fi
if test "$with_zoneinfo_dir" = "no" || test ! -d "$with_zoneinfo_dir"; then
  AC_MSG_ERROR([Please specify a valid zoneinfo directory.])
fi
AC_MSG_RESULT($with_zoneinfo_dir)
AC_DEFINE_UNQUOTED(TZDIR, ["$with_zoneinfo_dir"],
                   [Define to the directory contain time zone object files.])

AC_CONFIG_FILES([Source/GNUmakefile])

AC_OUTPUT

