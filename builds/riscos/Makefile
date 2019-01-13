
# Host toolchain directory and extra flags
TARGET = Player
HOST ?= $(GCCSDK_INSTALL_CROSSBIN)/arm-unknown-riscos-
CXXFLAGS = -g0 -Wall -O3 -ffast-math -fdata-sections -ffunction-sections -std=gnu++11
CXXFLAGS += -DNDEBUG -DWANT_FASTWAV -D_GLIBCXX_USE_C99=1
LDFLAGS = -static
PKG_CONFIG ?= $(GCCSDK_INSTALL_ENV)/ro-pkg-config

SDL_CONFIG ?= $(GCCSDK_INSTALL_ENV)/bin/sdl-config
ifneq ($(SDL_CONFIG), )
	SDL_TARGET = $(shell $(SDL_CONFIG) --version | cut -f1 -d.)
	SDL_CFLAGS = `$(SDL_CONFIG) --cflags` -DUSE_SDL=$(SDL_TARGET) -DSUPPORT_AUDIO
	SDL_LIBS = `$(SDL_CONFIG) --libs`
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists SDL_mixer), )
	SDL_MIXER_CFLAGS := $(shell $(PKG_CONFIG) --cflags SDL_mixer) -DHAVE_SDL_MIXER -DWANT_FMMIDI=2
	SDL_MIXER_LIBS := $(shell $(PKG_CONFIG) --static --libs SDL_mixer)
	SDL_MIXER_LIBS += -lmikmod
else
	CXXFLAGS += -DWANT_FMMIDI=1
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists liblcf), )
	LCF_CFLAGS := $(shell $(PKG_CONFIG) --cflags liblcf)
	LCF_LIBS := $(shell $(PKG_CONFIG) --static --libs liblcf)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists pixman-1), )
	PIXMAN_CFLAGS := $(shell $(PKG_CONFIG) --static --cflags pixman-1)
	PIXMAN_LIBS := $(shell $(PKG_CONFIG) --static --libs pixman-1)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists libpng), )
	PNG_CFLAGS := $(shell $(PKG_CONFIG) --cflags libpng)
	PNG_LIBS := $(shell $(PKG_CONFIG) --static --libs libpng)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists zlib), )
	ZLIB_CFLAGS := $(shell $(PKG_CONFIG) --cflags zlib)
	ZLIB_LIBS := $(shell $(PKG_CONFIG) --static --libs zlib)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists freetype2), )
	FREETYPE_CFLAGS := $(shell $(PKG_CONFIG) --cflags freetype2) -DHAVE_FREETYPE
	FREETYPE_LIBS := $(shell $(PKG_CONFIG) --static --libs freetype2)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists libmpg123), )
	LIBMPG123_CFLAGS := $(shell $(PKG_CONFIG) --cflags libmpg123) -DHAVE_MPG123
	LIBMPG123_LIBS :=  $(shell $(PKG_CONFIG) --static --libs libmpg123)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists vorbisidec), )
	VORBIS_CFLAGS := $(shell $(PKG_CONFIG) --cflags vorbisidec) -DHAVE_TREMOR
	VORBIS_LIBS := $(shell $(PKG_CONFIG) --static --libs vorbisidec)
else
ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists vorbis), )
	VORBIS_CFLAGS := $(shell $(PKG_CONFIG) --cflags vorbis) -DHAVE_OGGVORBIS
	VORBIS_LIBS := $(shell $(PKG_CONFIG) --static --libs vorbis)
endif
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists opusfile), )
	OPUS_CFLAGS := $(shell $(PKG_CONFIG) --cflags opusfile) -DHAVE_OPUS
	OPUS_LIBS := $(shell $(PKG_CONFIG) --static --libs opusfile)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists sndfile), )
	LIBSNDFILE_CFLAGS := $(shell $(PKG_CONFIG) --cflags sndfile) -DHAVE_LIBSNDFILE
	LIBSNDFILE_LIBS := $(shell $(PKG_CONFIG) --static --libs sndfile)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists wildmidi), )
	WILDMIDI_CFLAGS := $(shell $(PKG_CONFIG) --cflags wildmidi) -DHAVE_WILDMIDI
	WILDMIDI_LIBS := $(shell $(PKG_CONFIG) --static --libs wildmidi)
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists libxmp-lite), )
	LIBXMP_CFLAGS := $(shell $(PKG_CONFIG) --cflags libxmp-lite) -DHAVE_XMP
	LIBXMP_LIBS := $(shell $(PKG_CONFIG) --static --libs libxmp-lite)
else
ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists libxmp), )
	LIBXMP_CFLAGS := $(shell $(PKG_CONFIG) --cflags libxmp) -DHAVE_XMP
	LIBXMP_LIBS := $(shell $(PKG_CONFIG) --static --libs libxmp)
endif
endif

ifeq ($(shell $(PKG_CONFIG) --print-errors --errors-to-stdout --exists speexdsp), )
	SPEEXDSP_CFLAGS := $(shell $(PKG_CONFIG) --cflags speexdsp) -DHAVE_SPEEXDSP
	SPEEXDSP_LIBS := $(shell $(PKG_CONFIG) --static --libs speexdsp)
endif

# Compiler flags
CXXFLAGS += $(LCF_CFLAGS) $(SDL_MIXER_CFLAGS) $(SDL_CFLAGS) $(PIXMAN_CFLAGS) $(FREETYPE_CFLAGS) \
	$(PNG_CFLAGS) $(ZLIB_CFLAGS) $(LIBSNDFILE_CFLAGS) $(VORBIS_CFLAGS) $(OPUS_CFLAGS) \
	$(LIBMPG123_CFLAGS) $(WILDMIDI_CFLAGS) $(LIBXMP_CFLAGS) $(SPEEXDSP_CFLAGS)
LDFLAGS += $(LCF_LIBS) $(SDL_MIXER_LIBS) $(SDL_LIBS) $(PIXMAN_LIBS) $(FREETYPE_LIBS) \
	$(PNG_LIBS) $(ZLIB_LIBS) $(LIBSNDFILE_LIBS) $(VORBIS_LIBS) $(OPUS_LIBS) $(LIBMPG123_LIBS) \
	$(WILDMIDI_LIBS) $(LIBXMP_LIBS) $(SPEEXDSP_LIBS)

# Compiler headers
INCLUDES = ../../src
INCDIR = $(foreach dir, $(INCLUDES), -I$(wildcard $(dir)))

# Compiler source
SOURCES =  ../../src
CPPFILES = $(foreach dir, $(SOURCES), $(wildcard $(dir)/*.cpp))
BINFILES = $(foreach dir, $(DATA), $(wildcard $(dir)/*.*))
OBJS = $(addsuffix .o, $(BINFILES)) $(CPPFILES:.cpp=.o)

all: !EasyRPG/$(TARGET),ff8

# Start compile
$(TARGET),e1f: $(OBJS)
	$(HOST)g++ $(OBJS) -o $@ $(LDFLAGS)

!EasyRPG/%,ff8: %,e1f
	elf2aif $< $@

.cpp.o:
	$(HOST)g++ $(INCDIR) $(CXXFLAGS) -c $< -o $@

# Clean Project
clean:
	rm -f $(OBJS) $(TARGET),e1f !EasyRPG/$(TARGET),ff8
