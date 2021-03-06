AC_PREREQ(2.64)
AC_INIT([dnstable],
        [0.9.1],
        [https://github.com/farsightsec/dnstable/issues],
        [dnstable],
        [https://github.com/farsightsec/dnstable])
AC_CONFIG_SRCDIR([dnstable/dnstable.h])
AC_CONFIG_AUX_DIR([build-aux])
AM_INIT_AUTOMAKE([foreign 1.11 -Wall -Wno-portability silent-rules subdir-objects color-tests])
AC_PROG_CC_STDC
AC_USE_SYSTEM_EXTENSIONS
AC_SYS_LARGEFILE
AC_CONFIG_MACRO_DIR([m4])
AM_SILENT_RULES([yes])
LT_INIT

my_CFLAGS="-Wall \
-Wmissing-declarations -Wmissing-prototypes \
-Wnested-externs -Wpointer-arith \
-Wpointer-arith -Wsign-compare -Wchar-subscripts \
-Wstrict-prototypes -Wshadow \
-Wformat-security"
AC_SUBST([my_CFLAGS])

AC_CONFIG_HEADERS(config.h)
AC_CONFIG_FILES([Makefile dnstable/libdnstable.pc])

PKG_PROG_PKG_CONFIG
if test -n "$PKG_CONFIG"; then
    # Horrible hack for systems where the pkg-config install directory is simply wrong!
    if $PKG_CONFIG --variable=pc_path pkg-config 2>/dev/null | grep -q /libdata/; then
        PKG_INSTALLDIR(['${prefix}/libdata/pkgconfig'])
    else
        PKG_INSTALLDIR
    fi
else
    AC_MSG_ERROR([pkg-config is required!])
fi

AC_SEARCH_LIBS([clock_gettime], [rt])
AC_CHECK_FUNCS([clock_gettime])

PKG_CHECK_MODULES([libmtbl], [libmtbl >= 0.8.0])
PKG_CHECK_MODULES([libnmsg], [libnmsg])
PKG_CHECK_MODULES([libwdns], [libwdns >= 0.6.0])
PKG_CHECK_MODULES([libyajl], [yajl >= 2], [], [MY_CHECK_YAJL_1])

AC_CHECK_HEADERS([google/protobuf-c/protobuf-c.h],
                 [],
                 [AC_MSG_ERROR([required header file not found])])

AC_CHECK_HEADERS([nmsg/sie/dnsdedupe.pb-c.h],
                 [],
                 [AC_MSG_ERROR([required header file not found])])

AX_PTHREAD([
    LIBS="$PTHREAD_LIBS $LIBS"
    CFLAGS="$CFLAGS $PTHREAD_CFLAGS"
    CC="$PTHREAD_CC"
])

AC_PATH_PROG([ASCIIDOC], [a2x])
AM_CONDITIONAL([BUILD_MAN], [test -n "$ASCIIDOC"])
if test -n "$ASCIIDOC"; then
    DOC_MAN_MSG="yes (asciidoc available)"
else
    DOC_MAN_MSG="no (asciidoc not available)"
fi

gl_LD_VERSION_SCRIPT

AC_OUTPUT
AC_MSG_RESULT([
    $PACKAGE $VERSION

        compiler:               ${CC}
        cflags:                 ${CFLAGS}
        ldflags:                ${LDFLAGS}
        libs:                   ${LIBS}

        prefix:                 ${prefix}
        sysconfdir:             ${sysconfdir}
        libdir:                 ${libdir}
        includedir:             ${includedir}
        pkgconfigdir:           ${pkgconfigdir}

        building manpage docs:  ${DOC_MAN_MSG}
])
