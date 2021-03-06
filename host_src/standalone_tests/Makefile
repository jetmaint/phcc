#set this to 1 if you want to build under Windows:
WIN32=0
# set this to 1 if you are under unix but want windows binaries
CROSSCOMPILE=0
#


USE_CCACHE=0
USE_DEBUG=0

CXX=g++
CC=gcc

SUBDIRS=../lib
VPATH+=$(SUBDIRS)

INCLUDES=$(SUBDIRS:%=-I%)

CFLAGS=-I. -g -pedantic -Wall $(INCLUDES)
LDFLAGS=
EXEC_EXT=
LIB_EXT=.o
COMPILERPREFIX=

ifeq ($(CROSSCOMPILE),1)
  COMPILERPREFIX=/opt/xmingw/bin/i386-mingw32msvc-
endif

ifeq ($(USE_CCACHE),1)
  CXX:=ccache $(CXX)
  CC:=ccache $(CC)
endif


ifeq ($(WIN32),1)
  CFLAGS+=-D__WIN32__
  EXEC_EXT=.exe
  LIB_EXT=.lib
endif

ifeq ($(USE_DEBUG),1)
  CFLAGS+=-DDEBUG
endif


#ifeq ($(origin COMPILERPREFIX), undefined)
ifdef COMPILERPREFIX
  CXX:=$(COMPILERPREFIX)$(CXX)
  CC:=$(COMPILERPREFIX)$(CC)
endif

CXX_PROGS=phcc_reset phcc_doa_raw phcc_getkeymap phcc_getanalogmap phcc_watch_talking phcc_lcd_demo parport_doa_raw phcc_watch_talking_raw
C_PROGS=
PROGS=$(C_PROGS) $(CXX_PROGS)

HEADERS=PHCC_Serial.h MB_Opt.h


phcc_reset_OBJS=phcc_reset PHCC_Serial MB_Opt
phcc_reset_LIBS=
phcc_reset_CFLAGS=

phcc_doa_raw_OBJS=phcc_doa_raw PHCC_Serial MB_Opt
phcc_doa_raw_LIBS=
phcc_doa_raw_CFLAGS=

phcc_getkeymap_OBJS=phcc_getkeymap PHCC_Serial MB_Opt
phcc_getkeymap_LIBS=
phcc_getkeymap_CFLAGS=

phcc_getanalogmap_OBJS=phcc_getanalogmap PHCC_Serial MB_Opt
phcc_getanalogap_LIBS=
phcc_getanalogap_CFLAGS=

phcc_watch_talking_OBJS=phcc_watch_talking PHCC_Serial MB_Opt
phcc_watch_talking_LIBS=
phcc_watch_talking_CFLAGS=

phcc_watch_talking_raw_OBJS=phcc_watch_talking_raw PHCC_Serial MB_Opt
phcc_watch_talking_raw_LIBS=
phcc_watch_talking_raw_CFLAGS=

phcc_lcd_demo_OBJS=phcc_lcd_demo PHCC_Serial MB_Opt
phcc_lcd_demo_LIBS=
phcc_lcd_demo_CFLAGS=

ifneq ($(WIN32),1)
parport_doa_raw_OBJS=parport_doa_raw MB_Opt
parport_doa_raw_LIBS=
parport_doa_raw_CFLAGS=
endif

##########################################################################
all build: $(PROGS)

define CXX_PROGRAM_template
$(1): $$($(1)_OBJ) $$($(1)_OBJS:%=%$$(LIB_EXT))
	$$(CXX) $$(LDFLAGS) -o $$@$$(EXEC_EXT) $$^
ALL_OBJS += $$($(1)_OBJS:%=%$$(LIB_EXT))
endef

define C_PROGRAM_template
$(1): $$($(1)_OBJ) $$($(1)_OBJS:%=%$$(LIB_EXT))
	$$(CC) $$(LDFLAGS) -o $$@$$(EXEC_EXT) $$^
ALL_OBJS += $$($(1)_OBJS:%=%$$(LIB_EXT))
endef

$(foreach prog,$(CXX_PROGS),$(eval $(call CXX_PROGRAM_template,$(prog))))
$(foreach prog,$(C_PROGS),$(eval $(call C_PROGRAM_template,$(prog))))


.SUFFIXES:

%.lib : %.cpp $(HEADERS)
	$(CXX) $(CFLAGS) -c -o $@ $<

%.lib : %.c $(HEADERS)
	$(CC) $(CFLAGS) -c -o $@ $<

%.o : %.cpp $(HEADERS)
	$(CXX) $(CFLAGS) -c -o $@ $<

%.o : %.c $(HEADERS)
	$(CC) $(CFLAGS) -c -o $@ $<

%.h:
	@#echo "header file dependency: $<"

clean:
	rm -f core* $(PROGS) $(PROGS:%=%.exe) *.o *.lib $(ALL_OBJS)

doc-clean:
	rm -f *.pdf *.ps *.dvi *.aux *.log

dep depend install:
