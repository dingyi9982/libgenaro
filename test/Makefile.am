noinst_PROGRAMS = tests tests_rs
tests_SOURCES = mockbridge.c mockfarmer.c tests.c genarotests.h $(top_builddir)/src/genaro.h mockbridge.json.h mockbridgeinfo.json.h
tests_LDADD = $(top_builddir)/src/libgenaro.la
tests_LDFLAGS = -Wall -g

if BUILD_GENARO_DLL
tests_LDFLAGS += -lmicrohttpd $(top_builddir)/src/.libs/rs.o $(top_builddir)/src/.libs/bip39.o $(top_builddir)/src/.libs/crypto.o $(top_builddir)/src/.libs/utils.o
else
tests_LDFLAGS += -static -lmicrohttpd
endif

tests_rs_SOURCES = tests_rs.c
tests_rs_LDFLAGS = -Wall -g
TESTS = tests tests_rs

CLEANFILES = mockbridge.json.h mockbridgeinfo.json.h

mockbridge.c: mockbridge.json.h mockbridgeinfo.json.h

%.json.h: %.json
	@$(MKDIR_P) $(@D)
	@{ \
	 echo "static const char $(*F)_json[] = {" && \
	 $(HEXDUMP) -v -e '8/1 "0x%02x, "' -e '"\n"' $< | $(SED) -e 's/0x  ,//g' && \
	 echo "};"; \
	} > "$@.new" && mv -f "$@.new" "$@"
	@echo "Generated $@"
