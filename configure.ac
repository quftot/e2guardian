# Autoconf script


# AC_DEFINE([__SSLMITM],[""],[Define to enable SSL MITM])

AC_PREREQ(2.63)
AC_INIT([e2guardian],[5.5.5])
AM_INIT_AUTOMAKE([subdir-objects])
AC_CONFIG_HEADERS([e2config.h])
AC_CONFIG_MACRO_DIR([m4])

AC_CACHE_LOAD

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET

AC_LANG(C++)

AC_CACHE_SAVE

#########################################################################
# Get GCC version
#########################################################################
GCC_VERSION=$(gcc --version | head -1 | awk '{print $NF}')

GCC_VERSION_MAJOR=$(echo $GCC_VERSION |cut -d'.' -f1 2>/dev/null)
GCC_VERSION_MINOR=$(echo $GCC_VERSION |cut -d'.' -f2 2>/dev/null)
GCC_VERSION_PATCH=$(echo $GCC_VERSION |cut -d'.' -f3 2>/dev/null)

# Checks for header files.
AC_CHECK_HEADERS([arpa/inet.h fcntl.h limits.h netdb.h netinet/in.h stdlib.h])
AC_CHECK_HEADERS([string.h sys/socket.h sys/time.h syslog.h unistd.h locale.h])
AC_CHECK_HEADERS([sys/types.h sys/un.h sys/poll.h sys/epoll.h sys/resource.h])
AC_CHECK_HEADERS([pwd.h grp.h])
AC_CHECK_HEADERS([byteswap.h])

# Check system endianness
AC_C_BIGENDIAN

# check for zlib
AC_MSG_CHECKING(if zlib should be statically linked)
AC_ARG_ENABLE(static-zlib,
[AS_HELP_STRING([--enable-static-zlib@<:@=no@:>@],[Enable static linking of zlib])],
[ if test "x$enableval" = "xno"; then
staticzlib=false
AC_MSG_RESULT(no)
else
staticzlib=true
AC_MSG_RESULT(yes)
fi
],
[
staticzlib=false
AC_MSG_RESULT(no)
])

AC_MSG_CHECKING(for zlib)
AC_ARG_WITH(zlib,
[AS_HELP_STRING([--with-zlib@<:@=NONE@:>@],[non-standard search path for zlib library])],
[ # check for header & func (in library) in given prefix
CPPFLAGS="${CPPFLAGS} -I${withval}/include"
if test "x$staticzlib" = "xtrue"; then
LIBS="-Bstatic -L${withval} -lz -Bdynamic ${LIBS}"
else
LIBS="-L${withval}/lib -lz ${LIBS}"
fi
],
[ # if --with-zlib is not supplied, we still need -lz in LIBS.
if test "x$staticzlib" = "xtrue"; then
LIBS="-Bstatic -lz -Bdynamic ${LIBS}"
else
LIBS="-lz ${LIBS}"
fi
])
AC_CHECK_HEADERS([zlib.h])
if ! test "x$staticzlib" = "xtrue"; then
AC_CHECK_LIB(z, gzdopen, [AC_MSG_RESULT(yes)], [AC_MSG_ERROR([no zlib!])])
fi

AC_CACHE_SAVE

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_TYPE_OFF_T
AC_TYPE_UID_T

# Checks for library functions.
AC_FUNC_FORK
AC_CHECK_FUNCS([dup2 gettimeofday memset select])
AC_CHECK_FUNCS([strerror strstr strtol])
AC_CHECK_FUNCS([setuid setgid umask seteuid setreuid setlocale])
AC_SEARCH_LIBS([floor], [m])
AC_SEARCH_LIBS([gethostbyname], [nsl])
AC_SEARCH_LIBS([socket], [socket], [], [
AC_CHECK_LIB(
[socket], [socket], [LIBS="-lsocket -lnsl $LIBS"],
[], [-lnsl]
)
])
AC_SEARCH_LIBS([inet_aton], [resolv])

AC_CACHE_SAVE

AC_MSG_CHECKING(for backside with both hands)
AC_MSG_RESULT(no)
AC_MSG_CHECKING(build os)
AC_CANONICAL_BUILD
AC_MSG_RESULT($build_os)

AC_MSG_CHECKING(for debug_high option)
AC_ARG_WITH(debug_high,
[AS_HELP_STRING([--with-debug_high@<:@=on@:>@],[switch on high level debug build mode])],
[if test "x${withval}" = "xoff" ; then
  AC_MSG_RESULT(no)
else
  AC_MSG_RESULT(yes)
  debug_high=${withval}
  AC_DEFINE(DEBUG_HIGH, 1, [Define to enable high level debug build mode])
fi], [
  # enable by default
  AC_MSG_RESULT(yes)
  AC_DEFINE(DEBUG_HIGH, 1, [Define to enable high level debug build mode])
])

AC_MSG_CHECKING(for debug_low option)
AC_ARG_WITH(debug_low,
[AS_HELP_STRING([--with-debug_low@<:@=off@:>@],[switch on low level debug build mode])],
[if test "x${withval}" = "xoff" ; then
  AC_MSG_RESULT(no)
else
  AC_MSG_RESULT(yes)
  debug_low=${withval}
  AC_DEFINE(DEBUG_LOW, 1, [Define to enable low level debug build mode])
fi], [
  # disable by default
  AC_MSG_RESULT(no)
])

AC_MSG_CHECKING(for proxy user)
AC_ARG_WITH(proxyuser,
[AS_HELP_STRING([--with-proxyuser@<:@=nobody@:>@],[name of proxy user])],
[if test "x${withval}" != "x" ; then
  AC_MSG_RESULT(yes)
  proxyuser=${withval}
else
  AC_MSG_RESULT(no)
  proxyuser=nobody
fi], [
  # disable by default
  AC_MSG_RESULT(no)
  proxyuser=nobody
])
AC_SUBST(E2PROXYUSER, "$proxyuser")
#AC_DEFINE_UNQUOTED(__PROXYUSER,"$proxyuser","User to run as")

AC_MSG_CHECKING(for proxy group)
AC_ARG_WITH(proxygroup,
[AS_HELP_STRING([--with-proxygroup@<:@=nobody@:>@],[name of proxy group])],
[if test "x${withval}" != "x" ; then
  AC_MSG_RESULT(yes)
  proxygroup=${withval}
else
  AC_MSG_RESULT(no)
  proxygroup=nobody
fi], [
  # disable by default
  AC_MSG_RESULT(no)
  proxygroup=nobody
])
AC_SUBST(E2PROXYGROUP, "$proxygroup")
#AC_DEFINE_UNQUOTED(__PROXYGROUP,"$proxygroup","Group to run as")

AC_MSG_CHECKING(for piddir)
AC_ARG_WITH(piddir,
[AS_HELP_STRING([--with-piddir@<:@=${localstatedir}/run@:>@],[path for pid file])],
[if test "x${withval}" != "x" ; then
  AC_MSG_RESULT(yes)
  piddir=${withval}
else
  AC_MSG_RESULT(no)
  piddir="${localstatedir}/run"
fi], [
  # disable by default
  AC_MSG_RESULT(no)
  piddir="${localstatedir}/run"
])
AC_FINALIZE_VAR(E2PIDDIR,"$piddir")
AC_SUBST(E2PIDDIR)
#AC_DEFINE_UNQUOTED([__PIDDIR],[${E2PIDDIR}], "path of pid directory")


AC_MSG_CHECKING(for logdir)
AC_ARG_WITH(logdir,
[AS_HELP_STRING([--with-logdir@<:@=${localstatedir}/log/${PACKAGE_NAME}@:>@],[path for log files])],
[if test "x${withval}" != "x" ; then
  AC_MSG_RESULT(yes)
  logdir=${withval}
else
  AC_MSG_RESULT(no)
  logdir="${localstatedir}/log/${PACKAGE_NAME}"
fi], [
  # disable by default
  AC_MSG_RESULT(no)
  logdir="${localstatedir}/log/${PACKAGE_NAME}"
])
AC_FINALIZE_VAR(E2LOGLOCATION,"$logdir")
AC_SUBST(E2LOGLOCATION)
# AC_DEFINE_UNQUOTED([__LOGLOCATION],[${E2LOGLOCATION}], "path of log directory")

PKG_PROG_PKG_CONFIG

# asking user if they want PCRE support
AC_MSG_CHECKING(for PCRE support)
AC_ARG_ENABLE(
pcre,
[AS_HELP_STRING([--enable-pcre@<:@=yes@:>@],[Enable support for the PCRE library])],
[ if test "x$enableval" = "xno"; then
pcre=false
AC_MSG_RESULT(no)
else
pcre=true
AC_MSG_RESULT(yes)
fi
],
[ # enable by default
pcre=true
AC_MSG_RESULT(yes)
]
)

if test "x$pcre" = "xtrue"; then
PKG_CHECK_MODULES([PCRE],[libpcre2-posix >= 10.42])
AC_DEFINE([HAVE_PCRE],[],[Define to enable PCRE support])
else
AC_CHECK_FUNCS([regcomp])
fi
AM_CONDITIONAL(HAVE_PCRE, test "x$pcre" = "xtrue")

AC_CHECK_HEADERS([pthread.h], [ LIBS="-lpthread ${LIBS}"], [
echo "pthread.h required, failing"
exit -1
])
#AC_CHECK_HEADERS([event2/thread.h], [
#LIBS="-levent_pthreads ${LIBS}"
#], [
#echo "libevent_pthreads required, failing"
#exit -1
#])

[if test "$GCC_VERSION_MAJOR" -eq 4; then
	if test "$GCC_VERSION_MINOR" -ge 9; then
		if test "$GCC_VERSION_PATCH" -ge 3; then
                        echo "Version: $GCC_VERSION"
                        CXXFLAGS="${CXXFLAGS} --std=c++11"
                fi
        fi
	echo "Version: $GCC_VERSION "
        CXXFLAGS="${CXXFLAGS} --std=c++1y"
else
        if test "$GCC_VERSION_MAJOR" -ge 5; then
        echo "Version: $GCC_VERSION "
        CXXFLAGS="${CXXFLAGS} --std=c++11"
	else
	echo "Version: $GCC_VERSION "
        CXXFLAGS="${CXXFLAGS} --std=c++1y"
	fi
fi
]
  
# ask user if they want large file support on 32 bit systems
AC_MSG_CHECKING([for large file support])
AC_ARG_ENABLE(
lfs,
[AS_HELP_STRING([--enable-lfs@<:@=yes@:>@],[Enable large file support on 32 bit systems])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
else
lfsflags=`getconf LFS_CFLAGS`
lfsld=`getconf LFS_LDFLAGS`
lfslib=`getconf LFS_LIBS`
CXXFLAGS="${CXXFLAGS} $lfsflags"
LDFLAGS="${LDFLAGS} $lfsld"
LIBS="${LIBS} $lfslib"
AC_MSG_RESULT(yes)
fi
],
[ # enable by default
lfsflags=`getconf LFS_CFLAGS`
lfsld=`getconf LFS_LDFLAGS`
lfslib=`getconf LFS_LIBS`
CXXFLAGS="${CXXFLAGS} $lfsflags "
LDFLAGS="${LDFLAGS} $lfsld"
LIBS="${LIBS} $lfslib"
AC_MSG_RESULT(yes)
])

# determine whether or not "off_t" is simply a typedef of
# "int", "unsigned int", "long", etc. - if it is, the String
# class won't compile if its off_t constructor is defined,
# as that would represent a function redefinition.
AC_MSG_CHECKING([for type collision with off_t])
AC_COMPILE_IFELSE(
[
 AC_LANG_PROGRAM(
 [[#include <sys/types.h>]],[[
 struct Foo{
  void f(const int);
void f(const long);
void f(const long unsigned);
void f(const unsigned int);
void f(const off_t);
 };]])
],[
   AC_MSG_RESULT([no])
],[
   AC_MSG_RESULT([yes])
   AC_DEFINE([OFFT_COLLISION],[],[Define if type "off_t" is a typedef of another type for which String already has a constructor])
])

# by default, we do not need the content scanner list or config directories, nor the download manager list directory
cslists=false
csconfigs=false
dmlists=false

# asking user if they want clamd support
AC_MSG_CHECKING(for clamd support)
AC_ARG_ENABLE(
clamd,
[AS_HELP_STRING([--enable-clamd@<:@=no@:>@],[Enable support for the ClamD content scanner])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
clamd=false
CLAMDSUPPORT="!! Not compiled !! "
else
AC_MSG_RESULT(yes)
clamd=true
cslists=true
csconfigs=true
CLAMDSUPPORT=""
AC_DEFINE([ENABLE_CLAMD],[],[Define to enable ClamD content scanner])
fi],
[
AC_MSG_RESULT(no)
clamd=false
CLAMDSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(ENABLE_CLAMD, test "x$clamd" = "xtrue")
AC_SUBST(CLAMDSUPPORT)

# asking user if they want avastd support
AC_MSG_CHECKING(for avastd support)
AC_ARG_ENABLE(
avastd,
[AS_HELP_STRING([--enable-avastd@<:@=no@:>@],[Enable support for the AvastD content scanner])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
avastd=false
AVASTDSUPPORT="!! Not compiled !! "
else
AC_MSG_RESULT(yes)
avastd=true
cslists=true
csconfigs=true
AVASTDSUPPORT=""
AC_DEFINE([ENABLE_AVASTD],[],[Define to enable AvastD content scanner])
fi],
[
AC_MSG_RESULT(no)
avastd=false
AVASTDSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(ENABLE_AVASTD, test "x$avastd" = "xtrue")
AC_SUBST(AVASTDSUPPORT)

# asking user if they want icap support
AC_MSG_CHECKING(for icap support)
AC_ARG_ENABLE(
icap,
[AS_HELP_STRING([--enable-icap@<:@=no@:>@],[Enable support for ICAP AV server content scanner])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
icap=false
ICAPSUPPORT="!! Not compiled !! "
else
AC_MSG_RESULT(yes)
icap=true
cslists=true
csconfigs=true
AC_DEFINE([ENABLE_ICAP],[],[Define to enable ICAP content scanner])
ICAPSUPPORT=""
fi],
[
AC_MSG_RESULT(no)
icap=false
ICAPSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(ENABLE_ICAP, test "x$icap" = "xtrue")
AC_SUBST(ICAPSUPPORT)

# asking user if they want kavd support
AC_MSG_CHECKING(for kavd support)
AC_ARG_ENABLE(
kavd,
[AS_HELP_STRING([--enable-kavd@<:@=no@:>@],[Enable support for the Kaspersky AV daemon content scanner])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
kavd=false
KAVDSUPPORT="!! Not compiled !! "
else
AC_MSG_RESULT(yes)
kavd=true
cslists=true
csconfigs=true
KAVDSUPPORT=""
AC_DEFINE([ENABLE_KAVD],[],[Define to enable KAVD content scanner])
fi],
[
AC_MSG_RESULT(no)
kavd=false
KAVDSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(ENABLE_KAVD, test "x$kavd" = "xtrue")
AC_SUBST(KAVDSUPPORT)

# asking user if they want command-line content scanner support
AC_MSG_CHECKING(for command-line content scanner support)
AC_ARG_ENABLE(
commandline,
[AS_HELP_STRING([--enable-commandline@<:@=no@:>@],[Enable support for command-line content scanners])],
[ if test "x$enableval" = "xno"; then
AC_MSG_RESULT(no)
commandline=false
COMMANDLINESUPPORT="!! Not compiled !! "
else
AC_MSG_RESULT(yes)
commandline=true
cslists=true
csconfigs=true
COMMANDLINESUPPORT=""
AC_DEFINE([ENABLE_COMMANDLINE],[],[Define to enable command-line content scanner])
fi],
[
AC_MSG_RESULT(no)
commandline=false
COMMANDLINESUPPORT="!! Not compiled !! "
])
AM_CONDITIONAL(ENABLE_COMMANDLINE, test "x$commandline" = "xtrue")
AC_SUBST(COMMANDLINESUPPORT)

# install CS configs and lists if necessary
AM_CONDITIONAL(NEED_CSLISTS, test "x$cslists" = "xtrue")
AM_CONDITIONAL(NEED_CSCONFIGS, test "x$csconfigs" = "xtrue")

# install DM lists if necessary
AM_CONDITIONAL(NEED_DMLISTS, test "x$dmlists" = "xtrue")


# asking user if they want SSL MITM support
  AC_DEFINE(DEBUG_HIGH, 1, [Define to enable high level debug build mode])
AC_MSG_CHECKING(for SSL MITM support)
AC_ARG_ENABLE(
sslmitm,
[AS_HELP_STRING([--enable-sslmitm@<:@=yes@:>@],[Enable support for the SSL MITM ])],
[
	AC_MSG_RESULT(yes)
	sslmitm=true
	SSLMITMSUPPORT=""
	AC_DEFINE([__SSLMITM],[],[Define to enable SSL MITM ])
	PKG_CHECK_MODULES([OPENSSL],[ openssl >= 1.0.1])
	CXXFLAGS="${CXXFLAGS} ${OPENSSL_CFLAGS}"
	LIBS="${LIBS} ${OPENSSL_LIBS}"
],
[
	AC_MSG_RESULT(no)
	sslmitm=false
	SSLMITMSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(__SSLMITM, test "x$sslmitm" = "xtrue")
AC_SUBST(SSLMITMSUPPORT)

# asking user if they want DNS auth support
AC_MSG_CHECKING(for DNS support)
AC_ARG_ENABLE(
dnsauth,
[AS_HELP_STRING([--enable-dnsauth@<:@=no@:>@],[Enable support for the DNS auth plugin])],
[ if test "x$enableval" = "xno"; then
	AC_MSG_RESULT(no)
	dnsauth=false
	DNSAUTHSUPPORT="!! Not compiled !! "
else
	AC_MSG_RESULT(yes)
	dnsauth=true
	DNSAUTHSUPPORT=""
	LIBS="${LIBS} -lresolv"
	AC_DEFINE([PRT_DNSAUTH],[],[Define to enable DNS auth plugin])
fi],
[
	AC_MSG_RESULT(no)
	dnsauth=false
	DNSAUTHSUPPORT="!! Not compiled !! "
]
)
AM_CONDITIONAL(PRT_DNSAUTH, test "x$dnsauth" = "xtrue")
AC_SUBST(DNSAUTHSUPPORT)


# asking the user if they want email notifications
AC_MSG_CHECKING(for email notification support)
AC_ARG_ENABLE(
email,
[AS_HELP_STRING([--enable-email@<:@=no@:>@],[Enable support for email reporting functionality])],
[ if test "x$enableval" = "xyes"; then
email=true
EMAILSUPPORT=""
AC_MSG_RESULT(yes)
AC_DEFINE([ENABLE_EMAIL],[],[Define to enable email reporting])
else
email=false
EMAILSUPPORT="#!! Not compiled !!"
AC_MSG_RESULT(no)
fi],
[
email=false
EMAILSUPPORT="#!! Not compiled !!"
AC_MSG_RESULT(no)
])
AC_SUBST(EMAILSUPPORT)

AC_DEFINE_UNQUOTED([E2_CONFIGURE_OPTIONS], ["$ac_configure_args"], [Record configure-time options])

libdir="${libdir}/${PACKAGE_NAME}"

AC_ARG_WITH(sysconfsubdir,
[AS_HELP_STRING([--with-sysconfsubdir@<:@=e2guardian@:>@],[subdirectory under sysconfdir in which to place config files])],
[if test "x$withval" != "x"; then
e2sysconfdir="${sysconfdir}/${withval}"
else
e2sysconfdir="${sysconfdir}"
fi],
[e2sysconfdir="${sysconfdir}/${PACKAGE_NAME}"])

AC_FINALIZE_VAR(E2LIBDIR,"${libdir}")
AC_SUBST(E2LIBDIR)
AC_FINALIZE_VAR(E2BINDIR,"${sbindir}")
AC_SUBST(E2BINDIR)
AC_FINALIZE_VAR(E2CONFDIR,"${e2sysconfdir}")
AC_SUBST(E2CONFDIR)
AC_FINALIZE_VAR(E2DATADIR,"${datadir}/${PACKAGE_NAME}")
AC_SUBST(E2DATADIR)
AC_FINALIZE_VAR(E2CONFFILE,"${e2sysconfdir}/${PACKAGE_NAME}.conf")
AC_SUBST(E2CONFFILE)
#AC_DEFINE_UNQUOTED(__CONFDIR,"${e2sysconfdir}","")
AC_DEFINE_UNQUOTED([__DATADIR],["${E2DATADIR}"],["Data directory path"])
AC_DEFINE_UNQUOTED(__CONFFILE,"${e2sysconfdir}/${PACKAGE_NAME}.conf","Configure file path")

AC_CHECK_SIZEOF([void *])
if test $ac_cv_sizeof_void_p = 8; then
    httpworkers=500
else
    httpworkers=300
fi
AC_SUBST([HTTP_WORKERS], [$httpworkers])

AC_CONFIG_FILES([Makefile
data/Makefile
data/languages/Makefile
data/scripts/Makefile
data/scripts/bsd-init
data/scripts/e2guardian
data/scripts/logrotation
data/scripts/solaris-init
data/scripts/systemv-init
data/scripts/e2guardian.service
doc/Makefile
configs/e2guardian.conf
configs/e2guardianf1.conf
configs/examplef1.story
configs/Makefile
configs/lists/Makefile
configs/lists/common/Makefile
configs/lists/example.group/Makefile
configs/lists/phraselists/Makefile
configs/lists/oldphraselists/Makefile
configs/lists/rooms/Makefile
configs/lists/contentscanners/Makefile
configs/lists/example.group/bannedphraselist
configs/lists/example.group/oldbannedphraselist
configs/lists/example.group/bannedurllist
configs/lists/example.group/bannedsitelist
configs/lists/example.group/domainsnobypass
configs/lists/example.group/urlnobypass
configs/lists/example.group/exceptionphraselist
configs/lists/example.group/weightedphraselist
configs/lists/example.group/oldexceptionphraselist
configs/lists/example.group/oldweightedphraselist
configs/downloadmanagers/Makefile
configs/downloadmanagers/default.conf
configs/contentscanners/Makefile
configs/contentscanners/clamdscan.conf
configs/contentscanners/avastdscan.conf
configs/contentscanners/icapscan.conf
configs/contentscanners/kavdscan.conf
configs/contentscanners/commandlinescan.conf
configs/authplugins/Makefile
configs/lists/authplugins/Makefile
configs/lists/downloadmanagers/Makefile
src/Makefile
])

AC_OUTPUT
