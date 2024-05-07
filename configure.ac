AC_INIT([turbofec], [0.1], [tom@tsou.cc])
AC_CONFIG_MACRO_DIR([m4])
AM_INIT_AUTOMAKE([foreign -Wall -Werror])
AM_PROG_AR
LT_INIT

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

AM_PROG_AS
AC_PROG_MAKE_SET
AC_PROG_CC
AC_PROG_INSTALL

# Checks for header files.
AC_CHECK_HEADERS([stdlib.h, stdio.h, unistd.h])

# NEON support check
AC_MSG_CHECKING([for NEON support])
AC_COMPILE_IFELSE([AC_LANG_SOURCE([[
#include <arm_neon.h>
float32x4_t test_neon(float32x4_t a) { return vaddq_f32(a, a); }
int main() {
    float32x4_t v = vdupq_n_f32(1.0);
    v = test_neon(v);
    return 0;
}]])],
[neon_found=yes],
[neon_found=no])
AC_MSG_RESULT([$neon_found])


if test "x$neon_found" = "xyes"; then
    AC_DEFINE([HAVE_NEON], [1], [Define to 1 if NEON is supported.])
    AC_MSG_CHECKING([for architecture type])
    case "$(uname -m)" in
        aarch64) # On ARMv8-A 64-bit targets
            AC_DEFINE([ARCH_AARCH64], [1], [Define if architecture is AARCH64])
            # neon_cflags=""
            neon_cflags="-march=armv8-a+fp+simd+crypto+crc"
            ;;
        
        armv8*) # On ARMv8-A 32-bit targets
            AC_DEFINE([ARCH_ARMV8], [1], [Define if architecture is ARMV8])
            neon_cflags="-mfpu=neon-fp-armv8"
            ;;

        armv7*) # On ARMv7-A targets
            AC_DEFINE([ARCH_ARMV7], [1], [Define if architecture is ARMV7])
            neon_cflags="-mfpu=neon"
            ;;
        *)
            neon_cflags=""
            ;;
    esac
    AC_SUBST([neon_cflags])
    AC_MSG_RESULT([$(uname -m)])
fi


AM_CONDITIONAL([HAVE_NEON], [test "x$neon_found" = "xyes"])



AC_CONFIG_FILES([Makefile
                 include/Makefile
                 src/Makefile
                 tests/Makefile
                 turbofec.pc])
AC_OUTPUT

