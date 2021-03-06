AC_PREREQ(2.57)

m4_define([plugins_main_version], [esyscmd(. ./VERSION;echo -n $VERSION)])

AC_INIT([compiz-plugins-main], [plugins_main_version], [maniac@opencompositing.org])

#AC_CONFIG_AUX_DIR(config)

AM_INIT_AUTOMAKE([1.9 dist-bzip2])
AC_CONFIG_HEADERS([config.h])
AM_MAINTAINER_MODE

AC_ISC_POSIX
AC_PROG_CC
AC_PROG_CPP
AC_PROG_LIBTOOL
AC_HEADER_STDC
AC_CHECK_HEADERS([stdlib.h sys/time.h unistd.h])
IT_PROG_INTLTOOL([0.35.0])
AC_SUBST(ALL_LINGUAS)
AM_GLIB_GNU_GETTEXT
GETTEXT_PACKAGE=compiz-plugins-main
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE,"$GETTEXT_PACKAGE", [Gettext package.])
AC_SUBST(GETTEXT_PACKAGE)

if test "x$GCC" = "xyes"; then
  case " $CFLAGS " in
  *[[\ \	]]-Wall[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wall" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-Wpointer-arith[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wpointer-arith" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-Wstrict-prototypes[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wstrict-prototypes" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-Wmissing-prototypes[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wmissing-prototypes" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-Wmissing-declarations[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wmissing-declarations" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-Wnested-externs[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -Wnested-externs" ;;
  esac

  case " $CFLAGS " in
  *[[\ \	]]-fno-strict-aliasing[[\ \	]]*) ;;
  *) CFLAGS="$CFLAGS -fno-strict-aliasing" ;;
  esac

  if test "x$enable_ansi" = "xyes"; then
    case " $CFLAGS " in
    *[[\ \	]]-ansi[[\ \	]]*) ;;
    *) CFLAGS="$CFLAGS -ansi" ;;
    esac

    case " $CFLAGS " in
    *[[\ \	]]-pedantic[[\ \	]]*) ;;
    *) CFLAGS="$CFLAGS -pedantic" ;;
    esac
  fi
fi

AC_C_BIGENDIAN

plugindir=$libdir/compiz
AC_SUBST(plugindir)

imagedir=$datadir/compiz
AC_SUBST(imagedir)

metadatadir=$datadir/compiz
AC_SUBST(metadatadir)

compdatadir=$datadir/compiz
AC_SUBST(compdatadir)

dnl ============================================================
dnl Check for the pkg-config path.
if test x"$PKG_CONFIG_PATH" = x; then
    PKG_CONFIG_PATH=${prefix}/lib/pkgconfig:${prefix}/share/pkgconfig
        else
    PKG_CONFIG_PATH=${prefix}/lib/pkgconfig:${prefix}/share/pkgconfig:${PKG_CONFIG_PATH}
fi
export PKG_CONFIG_PATH
AC_SUBST(PKG_CONFIG_PATH)
AC_MSG_NOTICE([Using PKG_CONFIG_PATH=$PKG_CONFIG_PATH])

PKG_CHECK_MODULES(COMPIZ, compiz)
PKG_CHECK_MODULES(BCOP, bcop >= 0.7.3, [bcop_found=yes])
if test "$bcop_found" = yes; then
  if test -z "$PKG_CONFIG"; then
      AC_PATH_PROG(PKG_CONFIG, pkg-config, no)
  fi  
  if test "$PKG_CONFIG" != "no" ; then
      BCOP_BIN=`$PKG_CONFIG --variable=bin bcop`
      AC_SUBST(BCOP_BIN)
  fi
fi

AC_MSG_CHECKING(for GL_CFLAGS)
AC_ARG_WITH(gl-cflags, [  --with-gl-cflags=CFLAGS ],
		       [GL_CFLAGS="$withval"],
		       [GL_CFLAGS=""])

AC_MSG_RESULT($GL_CFLAGS)
AC_MSG_CHECKING(for GL_LIBS)
AC_ARG_WITH(gl-libs, [  --with-gl-libs=LIBS ],
		     [GL_LIBS="$withval"],
		     [GL_LIBS="-lGL"])
AC_MSG_RESULT($GL_LIBS)

AC_SUBST(GL_CFLAGS)
AC_SUBST(GL_LIBS)

PKG_CHECK_MODULES(SCALE, compiz-scale, [have_compiz_scale=yes], [have_compiz_scale=no])
PKG_CHECK_MODULES(CAIRO, cairo-xlib-xrender cairo >= 1.0, [have_cairo=yes], [have_cairo=no])
PKG_CHECK_MODULES(PANGO, cairo-xlib-xrender pangocairo cairo >= 1.0, [have_pango=yes], [have_pango=no])

AM_CONDITIONAL(TEXT_PLUGIN, test "x$have_pango" = "xyes")
if test "x$have_pango" = "xyes"; then
  AC_DEFINE(USE_TEXT, 1, [Build text plugin])
fi

AM_CONDITIONAL(RESIZEINFO_PLUGIN, test "x$have_pango" = "xyes")
if test "x$have_pango" = "xyes"; then
  AC_DEFINE(USE_RESIZEINFO, 1, [Build resizeinfo plugin])
fi

AM_CONDITIONAL(WALL_PLUGIN, test "x$have_cairo" = "xyes")
if test "x$have_cairo" = "xyes"; then
  AC_DEFINE(USE_WALL, 1, [Build wall plugin])
fi

AM_CONDITIONAL(SCALEADDON_PLUGIN, test "x$have_compiz_scale" = "xyes")
if test "x$have_compiz_scale" = "xyes"; then
  AC_DEFINE(USE_SCALEADDON, 1, [Build scaleaddon plugin])
fi

AC_ARG_ENABLE(jpeg,
  [  --disable-jpeg      Disable jpeg plugin],
  [use_jpeg=$enableval], [use_jpeg=yes])

AM_CONDITIONAL(JPEG_PLUGIN, test "x$use_jpeg" = "xyes")
if test "$use_jpeg" = yes; then
  AC_DEFINE(USE_JPEG, 1, [Build jpeg plugin])
fi

AC_OUTPUT([
compiz-text.pc
compiz-mousepoll.pc
compiz-animation.pc
data/Makefile
data/filters/Makefile
images/Makefile
images/Oxygen/Makefile
images/Default/Makefile
Makefile
metadata/Makefile
include/Makefile
src/Makefile
src/animation/Makefile
src/colorfilter/Makefile
src/expo/Makefile
src/ezoom/Makefile
src/jpeg/Makefile
src/kdecompat/Makefile
src/mag/Makefile
src/mousepoll/Makefile
src/neg/Makefile
src/opacify/Makefile
src/put/Makefile
src/resizeinfo/Makefile
src/ring/Makefile
src/scaleaddon/Makefile
src/session/Makefile
src/shift/Makefile
src/snap/Makefile
src/staticswitcher/Makefile
src/text/Makefile
src/thumbnail/Makefile
src/titleinfo/Makefile
src/vpswitch/Makefile
src/wall/Makefile
src/winrules/Makefile
src/workarounds/Makefile
po/Makefile.in
])
