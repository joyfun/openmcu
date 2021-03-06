VC_PLUGIN_DIR=codecs/video
prefix=/opt/openmcu-ru/lib/ptlib
exec_prefix=${prefix}
target_os=linux-gnu

SRCDIR	= ./vic
OBJDIR	= ./obj

BASENAME=h261-vic
SONAME	=$(BASENAME)
COMMONDIR       =opal
PLUGINDIR       =.

CC	 	=gcc
CXX	 	=g++
LDSO	 	=-shared -Wl,-soname,$(SONAME)
PLUGINEXT	=so
CFLAGS	       +=-g  -Os
STDCCFLAGS     +=-fPIC -D__STDC_CONSTANT_MACROS
LDFLAGS		=

EXTRACCFLAGS    += -I$(COMMONDIR) -I$(PLUGINDIR)

vpath   %.cxx $(COMMONDIR)
vpath	%.cxx $(SRCDIR)
vpath	%.c   $(SRCDIR)
vpath	%.o   $(OBJDIR)

SRCS	+= h261vic.cxx \
	   $(COMMONDIR)/trace.cxx \
           $(SRCDIR)/encoder-h261.cxx \
	   $(SRCDIR)/transmitter.cxx \
           $(SRCDIR)/vid_coder.cxx \
	   $(SRCDIR)/p64encoder.cxx \
           $(SRCDIR)/p64.cxx \
           $(SRCDIR)/dct.cxx \
           $(SRCDIR)/huffcode.c \
           $(SRCDIR)/bv.c \


$(OBJDIR)/%.o : %.cxx
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(Q_CC)$(CXX) -I../../../include $(EXTRACCFLAGS) $(STDCCFLAGS) $(OPTCCFLAGS) $(CFLAGS) -c $< -o $@

$(OBJDIR)/%.o : %.c
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(Q_CC)$(CC) -I../../../include $(EXTRACCFLAGS) $(STDCCFLAGS) $(OPTCCFLAGS) $(CFLAGS) -c $< -o $@

PLUGIN	= ./$(BASENAME)_video_pwplugin.$(PLUGINEXT)
STATIC	= ./lib$(BASENAME)_video_s.a

OBJECTS = $(addprefix $(OBJDIR)/,$(patsubst %.c,%.o,$(patsubst %.cxx,%.o,$(notdir $(SRCS)))))

$(PLUGIN): $(OBJECTS)
ifeq (,$(findstring solaris,$(target_os)))
	$(Q_LD)$(CXX) $(LDSO) -o $@ $^
else	
	$(Q_LD)$(CXX) $(LDSO) $@ -o $@ $^
endif

install:
	mkdir -p $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)
	install $(PLUGIN) $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)

uninstall:
	rm -f $(DESTDIR)$(prefix)/$(VC_PLUGIN_DIR)/$(PLUGIN)

clean:
	rm -f $(OBJECTS) $(PLUGIN)

###########################################
