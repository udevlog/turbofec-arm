AUTOMAKE_OPTIONS = serial-tests

AM_CFLAGS = -Wall $(LIBOSMOCORE_CFLAGS) -I$(top_builddir)/include

# Conditional flags for NEON support, based on architecture determined in configure.ac
if HAVE_NEON
AM_CFLAGS += $(neon_cflags)
endif

AM_CFLAGS += -O3

check_PROGRAMS = conv_test turbo_test
conv_test_SOURCES = conv_test.c noise.c codes.c
conv_test_LDADD = $(top_builddir)/src/libturbofec.la $(LIBOSMOCORE_LIBS) -lpthread -lm
conv_test_CFLAGS = $(AM_CFLAGS)

turbo_test_SOURCES = turbo_test.c noise.c
turbo_test_LDADD = $(top_builddir)/src/libturbofec.la $(LIBOSMOCORE_LIBS) -lpthread -lm
turbo_test_CFLAGS = $(AM_CFLAGS)

noinst_HEADERS = \
	noise.h \
	codes.h

TESTS = $(check_PROGRAMS)
