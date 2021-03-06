# Copyright © 2017, Che-Wei Hsu <cwxhsu@gmail.com>
# This file is part of the MintCM.
# Some rights reserved. See README.

CC_FLAGS = -Wall -Wno-unused-result -O2
LD_FLAGS =

INCLUDE_PATH =
LIBRARY_PATH = -L../mcm_lib/mcm_lulib

USE_STATIC_MCM_LIB = YES
ifeq (${USE_STATIC_MCM_LIB}, YES)
LIBRARY_FILE = ../mcm_lib/mcm_lulib/mcm_lulib_api.a
else
LIBRARY_FILE = -lmcm_lulib_api
endif

SOURCE_FILE = $(wildcard *.c)

CGI_CONFIG_OBJECT_FILE = mcm_cgi_config.o mcm_cgi_common.o
CGI_CONFIG_TARGET_FILE = mcm_cgi_config.cgi

CGI_UPLOAD_OBJECT_FILE = mcm_cgi_upload_handle.o mcm_cgi_upload_custom.o mcm_cgi_common.o
CGI_UPLOAD_TARGET_FILE = mcm_cgi_upload.cgi


binary : $(CGI_CONFIG_TARGET_FILE) $(CGI_UPLOAD_TARGET_FILE)


$(CGI_CONFIG_TARGET_FILE) : $(CGI_CONFIG_OBJECT_FILE)
	$(CC) $(LD_FLAGS) $(LIBRARY_PATH) $^ $(LIBRARY_FILE) -o $@
	$(STRIP) $@

$(CGI_UPLOAD_TARGET_FILE) : $(CGI_UPLOAD_OBJECT_FILE)
	$(CC) $(LD_FLAGS) $(LIBRARY_PATH) $^ $(LIBRARY_FILE) -o $@
	$(STRIP) $@

%.o : %.c
	$(CC) $(CC_FLAGS) $(INCLUDE_PATH) -c $< -o $@

%.d : %.c
	@set -e; rm -f $@; \
	$(CC) $(CC_FLAGS) $(INCLUDE_PATH) -MM $< > $@.$$$$; 2>/dev/null; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$

-include $(SOURCE_FILE:.c=.d)

install :
	cp -f $(CGI_CONFIG_TARGET_FILE) $(CGI_UPLOAD_TARGET_FILE) $(MCM_RUN_CGI_PATH)

clean :
	rm -f *.d *.d* *.o *.cgi
	cd $(MCM_RUN_CGI_PATH); rm -f $(CGI_CONFIG_TARGET_FILE) $(CGI_UPLOAD_TARGET_FILE)
