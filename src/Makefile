CHAINPREFIX=/opt/mipsel-linux-uclibc/usr
CROSS_COMPILE=$(CHAINPREFIX)/bin/mipsel-linux-

CC= $(CROSS_COMPILE)gcc
CXX = $(CROSS_COMPILE)g++
STRIP = $(CROSS_COMPILE)strip

CFLAGS = -I. -I"$(CHAINPREFIX)/include" `$(CHAINPREFIX)/bin/sdl-config --cflags` -DTARGET_GP2X -DTIXML_USE_STL -O3 -msoft-float -fomit-frame-pointer -ffast-math -funroll-loops -Wall -Wno-unknown-pragmas -Wno-format
CXXFLAGS = $(CFLAGS)
LDFLAGS = -L$(CHAINPREFIX)/lib `$(CHAINPREFIX)/bin/sdl-config --libs` -lfreetype -lSDL_image -lSDL_ttf -lSDL_gfx -ljpeg -lpng12 -lSDL -lz -ldl # -lpnd

TARGET = ../build/gmenu2x
OBJDIR = ../objs/gp2x

SOURCES := $(wildcard *.cpp)
OBJS := $(patsubst %.cpp, $(OBJDIR)/%.o, $(SOURCES))
#TinyXml
TXML_SOURCES := $(wildcard tinyxml/*.cpp)
TXML_OBJS := $(patsubst %.cpp, $(OBJDIR)/%.o, $(TXML_SOURCES))

#File types rules
$(OBJDIR)/tinyxml/%.o: tinyxml/%.cpp
	$(CXX) $(CFLAGS) -o $@ -c $<
$(OBJDIR)/tinyxml/%.o: tinyxml/%.c
	$(CXX) $(CFLAGS) -o $@ -c $<
$(OBJDIR)/%.o: %.cpp %.h
	$(CXX) $(CFLAGS) -o $@ -c $<

all: dir static

dir:
	@if [ ! -d $(OBJDIR)/tinyxml ]; then mkdir -p $(OBJDIR)/tinyxml; fi

static: $(TARGET)

$(TARGET): $(OBJS) $(TXML_OBJS) 
	@echo "Linking $(TARGET)..."
	$(CXX) -o $(TARGET) $(LDFLAGS) $(OBJS) $(TXML_OBJS)
	$(STRIP) $(TARGET)

shared: $(OBJS) $(TXML_OBJS) 
	@echo "Linking $(TARGET)..."
	$(CXX) -o $(TARGET) $(LDFLAGS) $(OBJS) $(TXML_OBJS)
	$(STRIP) $(TARGET)

clean:
	-rm -f $(OBJDIR)/*.o $(OBJDIR)/tinyxml/*.o $(TARGET)
