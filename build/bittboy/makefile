CXX       = arm-linux-g++
BINBASE = /opt/miyoo/bin/
SYSROOT = $(shell $(CC) --print-sysroot)

PROJECT_NAME = spartak-chess

SRC_PATH = ../..
OBJ_BASE = Release
DIST_PATH = $(OBJ_BASE)
OBJ_PATH = $(OBJ_BASE)
RES_PATH = res

SRCFOLDERS = . stockfish ui platform

STD_OPTS  = -O3 -Wall -c -fmessage-length=0 -DNDEBUG -DNO_PREFETCH -DNO_THREADS -DUSE_SDL -DSDL_USE_JOYSTICK -DSDL_KEYS_COMMON -D_MOBILE -DWITH_HOME_DIR -DBITTBOY $(shell $(BINBASE)pkg-config --cflags sdl)
CPP_OPTS  = $(STD_OPTS) -fno-rtti -fno-threadsafe-statics
C_OPTS    = $(STD_OPTS)
LD_OPTS   = $(shell $(BINBASE)pkg-config --libs sdl) -lpng -lz

CXXSRCS = $(foreach dir, $(SRCFOLDERS), $(wildcard $(SRC_PATH)/$(dir)/*.cpp))
CSRCS = $(foreach dir, $(SRCFOLDERS), $(wildcard $(SRC_PATH)/$(dir)/*.c))

CXX_OBJS = $(patsubst $(SRC_PATH)/%.cpp,$(OBJ_PATH)/%.o,$(CXXSRCS))
CXX_DEPS = $(patsubst $(SRC_PATH)/%.cpp,$(OBJ_PATH)/%.d,$(CXXSRCS))
C_OBJS = $(patsubst $(SRC_PATH)/%.c,$(OBJ_PATH)/%.o,$(CSRCS))
C_DEPS = $(patsubst $(SRC_PATH)/%.c,$(OBJ_PATH)/%.d,$(CSRCS))

BIN_TARGET = $(DIST_PATH)/$(PROJECT_NAME)

build: mkdirs $(BIN_TARGET)

mkdirs:
	mkdir -p $(DIST_PATH)
	mkdir -p $(foreach dir, $(SRCFOLDERS), $(OBJ_PATH)/$(dir))

$(CXX_OBJS): $(OBJ_PATH)/%.o : $(SRC_PATH)/%.cpp
	$(CXX) $(CPP_OPTS) -o $@ $<

$(C_OBJS): $(OBJ_PATH)/%.o : $(SRC_PATH)/%.c
	$(C) $(C_OPTS) -o $@ $<

$(BIN_TARGET): $(CXX_OBJS) $(C_OBJS)
	$(CXX) $(CXX_OBJS) $(C_OBJS) $(LD_OPTS) -o $(BIN_TARGET)

clean:
	rm -rf $(OBJ_BASE)
	rm -rf $(DIST_PATH)

.PHONY: mkdirs clean build
