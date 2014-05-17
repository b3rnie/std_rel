###############################################################################
# The std_rel Makefile
###############################################################################

#
# Globals
#
SYSTEM         = std_rel
VERSION        = $(shell git describe --tags 2>/dev/null)
COMPILE_FLAGS  = -DS2_USE_ESTATSD -DS2_USE_LAGER
TARGET         = $(PWD)/target
TEST_SKIP_APPS = "foo,bar,baz"
REL           := prod

#
# Build
#
.PHONY: deps update-deps compile rel dist

all: deps compile

deps:
	./rebar get-deps skip_deps=true

update-deps:
	./rebar update-deps skip_deps=true

compile:
	./rebar compile $(COMPILE_FLAGS)

rel: all
	./rebar generate target_dir=$(TARGET)/$(SYSTEM)
	cp -R $(TARGET)/$(SYSTEM) $(TARGET)/$(SYSTEM)-$(VERSION)

dist: rel
	tar -C $(TARGET) -czf $(TARGET)/$(SYSTEM)-$(VERSION).tar.gz $(SYSTEM)

#
# Test
#
.PHONY: xref test eunit

xref:
	./rebar xref

test: eunit ct

eunit:
	./rebar eunit skip_apps=$(TEST_SKIP_APPS)

ct:
	./rebar ct skip_apps=$(TEST_SKIP_APPS)

#
# Cleanup
#
.PHONY: clean distclean

clean:
	./rebar clean

distclean: clean
	./rebar delete-deps
	rm -rf $(TARGET)

# eof
