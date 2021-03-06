# This is _NOT_ the library release version, it's an API version.
# Please read chapter "Library interface versions" of the libtool documentation
# before making any modifications: https://www.gnu.org/software/libtool/manual/html_node/Versioning.html
LIBVERSION=8:0:0

AM_CPPFLAGS = -I$(top_srcdir)/include -I$(top_builddir)/include
AM_CFLAGS = -Wall $(TALLOC_CFLAGS)

if ENABLE_PSEUDOTALLOC
AM_CPPFLAGS += -I$(top_srcdir)/src/pseudotalloc
endif

lib_LTLIBRARIES = libosmocore.la

libosmocore_la_LIBADD = $(BACKTRACE_LIB) $(TALLOC_LIBS)
libosmocore_la_SOURCES = timer.c timer_gettimeofday.c select.c signal.c msgb.c bits.c \
			 bitvec.c bitcomp.c statistics.c fsm.c \
			 write_queue.c utils.c socket.c \
			 logging.c logging_syslog.c logging_gsmtap.c rate_ctr.c \
			 gsmtap_util.c crc16.c panic.c backtrace.c \
			 conv.c application.c rbtree.c strrb.c \
			 loggingrb.c crc8gen.c crc16gen.c crc32gen.c crc64gen.c \
			 macaddr.c stat_item.c stats.c stats_statsd.c prim.c \
			 viterbi.c viterbi_generic.c sercomm.c

if HAVE_SSE3
libosmocore_la_SOURCES += viterbi_sse.c
if HAVE_SSE4_1
viterbi_sse.lo : CFLAGS += -msse3 -msse4.1
else
viterbi_sse.lo : CFLAGS += -msse3
endif

if HAVE_AVX2
libosmocore_la_SOURCES += viterbi_sse_avx.c
if HAVE_SSE4_1
viterbi_sse_avx.lo : CFLAGS += -msse3 -mavx2 -msse4.1
else
viterbi_sse_avx.lo : CFLAGS += -msse3 -mavx2
endif
endif
endif

BUILT_SOURCES = crc8gen.c crc16gen.c crc32gen.c crc64gen.c
EXTRA_DIST = viterbi_sse_common.h

if ENABLE_PLUGIN
libosmocore_la_SOURCES += plugin.c
libosmocore_la_LDFLAGS = -version-info $(LIBVERSION) $(TALLOC_LIBS) $(LIBRARY_DL) -no-undefined
else
libosmocore_la_LDFLAGS = -version-info $(LIBVERSION) $(TALLOC_LIBS) -no-undefined
endif

if ENABLE_MSGFILE
libosmocore_la_SOURCES += msgfile.c
endif

if ENABLE_SERIAL
libosmocore_la_SOURCES += serial.c
endif

crc%gen.c: crcXXgen.c.tpl
	$(AM_V_GEN)sed -e's/XX/$*/g' $< > $@
