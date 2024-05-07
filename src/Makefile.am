
AM_CFLAGS = -Wall -fvisibility=hidden -I$(top_srcdir)/include

# Conditional flags for NEON support, based on architecture determined in configure.ac
if HAVE_NEON
AM_CFLAGS += $(neon_cflags)
endif

AM_CFLAGS += -O3

lib_LTLIBRARIES = libturbofec.la
libturbofec_la_SOURCES = \
    conv_dec.c \
    conv_enc.c \
    conv_rate_match.c \
    turbo_dec.c \
    turbo_enc.c \
    turbo_rate_match.c

libturbofec_la_CFLAGS = $(AM_CFLAGS)

noinst_HEADERS = \
    conv_gen.h \
    conv_sse.h \
    turbo_int.h \
    turbo_sse.h
