# This Makefile.am is in the public domain
SUBDIRS  = .

if USE_COVERAGE
  AM_CFLAGS = -fprofile-arcs -ftest-coverage
endif

if ENABLE_HTTPS
  SUBDIRS += https
endif

AM_CPPFLAGS = \
-DCPU_COUNT=$(CPU_COUNT) \
-I$(top_srcdir) \
-I$(top_srcdir)/src/microhttpd \
-I$(top_srcdir)/src/include \
$(LIBCURL_CPPFLAGS)


THREAD_ONLY_TESTS = \
  test_urlparse \
  test_long_header \
  test_long_header11 \
  test_iplimit11 \
  test_termination \
  test_timeout

if HAVE_POSIX_THREADS
THREAD_ONLY_TESTS += \
  test_quiesce \
  test_concurrent_stop \
  perf_get_concurrent
if ENABLE_HTTP2
THREAD_ONLY_TESTS += \
  test_quiesce_http2 \
  test_concurrent_stop_http2 \
  perf_get_concurrent_http2
endif

if HAVE_CURL_BINARY
THREAD_ONLY_TESTS += \
  test_quiesce_stream
if ENABLE_HTTP2
THREAD_ONLY_TESTS += \
  test_quiesce_stream_http2
endif
endif
endif

if ENABLE_DAUTH
  THREAD_ONLY_TESTS += \
    test_digestauth \
    test_digestauth_sha256 \
    test_digestauth_with_arguments
if ENABLE_HTTP2
  THREAD_ONLY_TESTS += \
    test_digestauth_http2 test_digestauth_http2_direct \
    test_digestauth_with_arguments_http2 test_digestauth_with_arguments_http2_direct
endif
endif

if HAVE_CURL
check_PROGRAMS = \
  test_get \
  test_get_sendfile \
  test_delete \
  test_put \
  test_process_headers \
  test_process_arguments \
  test_parse_cookies \
  test_large_put \
  test_get11 \
  test_get_sendfile11 \
  test_put11 \
  test_large_put11 \
  test_large_put_inc11 \
  test_get_chunked \
  test_put_chunked \
  test_callback \
  perf_get

if ENABLE_HTTP2
check_PROGRAMS += \
  test_get_http2 test_get_http2_direct \
  test_get_sendfile_http2 test_get_sendfile_http2_direct \
  test_urlparse_http2 test_urlparse_http2_direct \
  test_delete_http2 test_delete_http2_direct \
  test_put_http2 test_put_http2_direct \
  test_process_headers_http2 test_process_headers_http2_direct \
  test_process_arguments_http2 test_process_arguments_http2_direct \
  test_parse_cookies_http2 test_parse_cookies_http2_direct \
  test_large_put_http2 test_large_put_http2_direct \
  test_large_put_inc_http2 test_large_put_inc_http2_direct \
  test_long_header_http2 test_long_header_http2_direct \
  test_get_chunked_http2 test_get_chunked_http2_direct \
  test_iplimit_http2 test_iplimit_http2_direct \
  test_termination_http2 test_termination_http2_direct \
  test_timeout_http2 test_timeout_http2_direct \
  test_callback_http2 test_callback_http2_direct \
  perf_get_http2 perf_get_http2_direct

endif

if HAVE_FORK_WAITPID
if HAVE_CURL_BINARY
check_PROGRAMS += test_get_response_cleanup
if ENABLE_HTTP2
check_PROGRAMS += \
  test_get_response_cleanup_http2 test_get_response_cleanup_http2_direct
endif
endif
endif

if HAVE_POSIX_THREADS
check_PROGRAMS += \
  test_quiesce \
  test_concurrent_stop \
  perf_get_concurrent
if ENABLE_HTTP2
check_PROGRAMS += \
  test_quiesce_http2 test_quiesce_http2_direct \
  test_concurrent_stop_http2 test_concurrent_stop_http2_direct \
  perf_get_concurrent_http2 perf_get_concurrent_http2_direct
endif
endif

if HAVE_POSTPROCESSOR
 check_PROGRAMS += \
  test_post \
  test_postform \
  test_post_loop \
  test_post11 \
  test_postform11 \
  test_post_loop11
if ENABLE_HTTP2
 check_PROGRAMS += \
  test_post_http2 test_post_http2_direct \
  test_postform_http2 test_postform_http2_direct \
  test_post_loop_http2 test_post_loop_http2_direct
endif
endif

if USE_POSIX_THREADS
check_PROGRAMS += \
  $(THREAD_ONLY_TESTS)
endif
if USE_W32_THREADS
check_PROGRAMS += \
  $(THREAD_ONLY_TESTS)
endif

TESTS = $(check_PROGRAMS)

noinst_LIBRARIES = libcurl_version_check.a
endif

libcurl_version_check_a_SOURCES = \
  curl_version_check.c

test_concurrent_stop_SOURCES = \
  test_concurrent_stop.c
test_concurrent_stop_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_concurrent_stop_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_get_SOURCES = \
  test_get.c
test_get_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_quiesce_SOURCES = \
  test_quiesce.c
test_quiesce_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_quiesce_stream_SOURCES = \
  test_quiesce_stream.c
test_quiesce_stream_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_stream_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_callback_SOURCES = \
  test_callback.c
test_callback_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_SOURCES = \
  perf_get.c \
  gauger.h
perf_get_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_concurrent_SOURCES = \
  perf_get_concurrent.c \
  gauger.h
perf_get_concurrent_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
perf_get_concurrent_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_digestauth_SOURCES = \
  test_digestauth.c
test_digestauth_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_digestauth_sha256_SOURCES = \
  test_digestauth_sha256.c
test_digestauth_sha256_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_digestauth_with_arguments_SOURCES = \
  test_digestauth_with_arguments.c
test_digestauth_with_arguments_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_get_sendfile_SOURCES = \
  test_get_sendfile.c
test_get_sendfile_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_urlparse_SOURCES = \
  test_urlparse.c
test_urlparse_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_get_response_cleanup_SOURCES = \
  test_get_response_cleanup.c
test_get_response_cleanup_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la

test_get_chunked_SOURCES = \
  test_get_chunked.c
test_get_chunked_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_post_SOURCES = \
  test_post.c
test_post_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_headers_SOURCES = \
  test_process_headers.c
test_process_headers_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_parse_cookies_SOURCES = \
  test_parse_cookies.c
test_parse_cookies_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_arguments_SOURCES = \
  test_process_arguments.c
test_process_arguments_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_postform_SOURCES = \
  test_postform.c
test_postform_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_post_loop_SOURCES = \
  test_post_loop.c
test_post_loop_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_delete_SOURCES = \
  test_delete.c
test_delete_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_put_SOURCES = \
  test_put.c
test_put_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_put_chunked_SOURCES = \
  test_put_chunked.c
test_put_chunked_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_get11_SOURCES = \
  test_get.c
test_get11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_get_sendfile11_SOURCES = \
  test_get_sendfile.c
test_get_sendfile11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_post11_SOURCES = \
  test_post.c
test_post11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_postform11_SOURCES = \
  test_postform.c
test_postform11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_post_loop11_SOURCES = \
  test_post_loop.c
test_post_loop11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_put11_SOURCES = \
  test_put.c
test_put11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_SOURCES = \
  test_large_put.c
test_large_put_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put11_SOURCES = \
  test_large_put.c
test_large_put11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_inc11_SOURCES = \
  test_large_put.c
test_large_put_inc11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_long_header_SOURCES = \
  test_long_header.c
test_long_header_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_long_header11_SOURCES = \
  test_long_header.c
test_long_header11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_iplimit11_SOURCES = \
  test_iplimit.c
test_iplimit11_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_termination_SOURCES = \
  test_termination.c
test_termination_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_timeout_SOURCES = \
  test_timeout.c
test_timeout_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_concurrent_stop_http2_SOURCES = \
  test_concurrent_stop.c
test_concurrent_stop_http2_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_concurrent_stop_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_get_http2_SOURCES = \
  test_get.c
test_get_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_quiesce_http2_SOURCES = \
  test_quiesce.c
test_quiesce_http2_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_quiesce_stream_http2_SOURCES = \
  test_quiesce_stream.c
test_quiesce_stream_http2_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_stream_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_callback_http2_SOURCES = \
  test_callback.c
test_callback_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_http2_SOURCES = \
  perf_get.c \
  gauger.h
perf_get_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_concurrent_http2_SOURCES = \
  perf_get_concurrent.c \
  gauger.h
perf_get_concurrent_http2_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
perf_get_concurrent_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_digestauth_http2_SOURCES = \
  test_digestauth.c
test_digestauth_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_digestauth_with_arguments_http2_SOURCES = \
  test_digestauth_with_arguments.c
test_digestauth_with_arguments_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_get_sendfile_http2_SOURCES = \
  test_get_sendfile.c
test_get_sendfile_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_urlparse_http2_SOURCES = \
  test_urlparse.c
test_urlparse_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_get_response_cleanup_http2_SOURCES = \
  test_get_response_cleanup.c
test_get_response_cleanup_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la

test_get_chunked_http2_SOURCES = \
  test_get_chunked.c
test_get_chunked_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_post_http2_SOURCES = \
  test_post.c
test_post_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_headers_http2_SOURCES = \
  test_process_headers.c
test_process_headers_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_parse_cookies_http2_SOURCES = \
  test_parse_cookies.c
test_parse_cookies_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_arguments_http2_SOURCES = \
  test_process_arguments.c
test_process_arguments_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_postform_http2_SOURCES = \
  test_postform.c
test_postform_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_post_loop_http2_SOURCES = \
  test_post_loop.c
test_post_loop_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_delete_http2_SOURCES = \
  test_delete.c
test_delete_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_put_http2_SOURCES = \
  test_put.c
test_put_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_http2_SOURCES = \
  test_large_put.c
test_large_put_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_inc_http2_SOURCES = \
  test_large_put.c
test_large_put_inc_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_long_header_http2_SOURCES = \
  test_long_header.c
test_long_header_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_iplimit_http2_SOURCES = \
  test_iplimit.c
test_iplimit_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_termination_http2_SOURCES = \
  test_termination.c
test_termination_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_timeout_http2_SOURCES = \
  test_timeout.c
test_timeout_http2_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_concurrent_stop_http2_direct_SOURCES = \
  test_concurrent_stop.c
test_concurrent_stop_http2_direct_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_concurrent_stop_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_get_http2_direct_SOURCES = \
  test_get.c
test_get_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_quiesce_http2_direct_SOURCES = \
  test_quiesce.c
test_quiesce_http2_direct_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_quiesce_stream_http2_direct_SOURCES = \
  test_quiesce_stream.c
test_quiesce_stream_http2_direct_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
test_quiesce_stream_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_callback_http2_direct_SOURCES = \
  test_callback.c
test_callback_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_http2_direct_SOURCES = \
  perf_get.c \
  gauger.h
perf_get_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

perf_get_concurrent_http2_direct_SOURCES = \
  perf_get_concurrent.c \
  gauger.h
perf_get_concurrent_http2_direct_CFLAGS = \
  $(PTHREAD_CFLAGS) $(AM_CFLAGS)
perf_get_concurrent_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  $(PTHREAD_LIBS) @LIBCURL@

test_digestauth_http2_direct_SOURCES = \
  test_digestauth.c
test_digestauth_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_digestauth_with_arguments_http2_direct_SOURCES = \
  test_digestauth_with_arguments.c
test_digestauth_with_arguments_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_get_sendfile_http2_direct_SOURCES = \
  test_get_sendfile.c
test_get_sendfile_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_urlparse_http2_direct_SOURCES = \
  test_urlparse.c
test_urlparse_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_get_response_cleanup_http2_direct_SOURCES = \
  test_get_response_cleanup.c
test_get_response_cleanup_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la

test_get_chunked_http2_direct_SOURCES = \
  test_get_chunked.c
test_get_chunked_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_post_http2_direct_SOURCES = \
  test_post.c
test_post_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_headers_http2_direct_SOURCES = \
  test_process_headers.c
test_process_headers_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_parse_cookies_http2_direct_SOURCES = \
  test_parse_cookies.c
test_parse_cookies_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_process_arguments_http2_direct_SOURCES = \
  test_process_arguments.c
test_process_arguments_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_postform_http2_direct_SOURCES = \
  test_postform.c
test_postform_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBGCRYPT_LIBS@ @LIBCURL@

test_post_loop_http2_direct_SOURCES = \
  test_post_loop.c
test_post_loop_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_delete_http2_direct_SOURCES = \
  test_delete.c
test_delete_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_put_http2_direct_SOURCES = \
  test_put.c
test_put_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_http2_direct_SOURCES = \
  test_large_put.c
test_large_put_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_large_put_inc_http2_direct_SOURCES = \
  test_large_put.c
test_large_put_inc_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_long_header_http2_direct_SOURCES = \
  test_long_header.c
test_long_header_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_iplimit_http2_direct_SOURCES = \
  test_iplimit.c
test_iplimit_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_termination_http2_direct_SOURCES = \
  test_termination.c
test_termination_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@

test_timeout_http2_direct_SOURCES = \
  test_timeout.c
test_timeout_http2_direct_LDADD = \
  $(top_builddir)/src/microhttpd/libmicrohttpd.la \
  @LIBCURL@
