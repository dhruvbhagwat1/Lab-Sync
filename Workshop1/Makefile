#
#  Linux and MS Windows Makefile for Workshop 1
#  Computer Graphics course
#  Dept Computing Science, Umea University
#  Stefan Johansson, stefanj@cs.umu.se
#  Modified by Filip Henningsson, floop@cs.umu.se
#
SRC = ./
TARGET = workshop1
BUILD_DIR = ./

# All source files (they can be listed explicitly if needed)
CPPS =  $(SRC)/main.cpp $(wildcard $(SRC)/*.cpp) #$(wildcard $(SRC)/dir1/*.cpp) to add onther subdirectory
# All .o files are pu in the build directory
OBJS = $(CPPS:%.cpp=$(BUILD_DIR)/%.o)
# gcc/clang put the dependencies in the .d files
DEP = $(OBJS:%.o=%.d)

CXX = g++

DBFLAGS = -O0 -g3 -ggdb3 -fno-inline
#DBFLAGS = -O2
WFLAGS  = -Wall -std=c++11

# Uncomment if you have local libraries or headers in subfolders lib and include
IFLAGS = #-Iinclude
LFLAGS = #-Llib 

ifeq ($(OS), Windows_NT)
# -DWINDOWS_BUILD needed to deal with Windows use 0f \ instead of / in path
# Unless it's completely unnecessary and handled by the compiler.
DEFS      = -DWINDOWS_BUILD
GLFLAGS   = -DGLEW_STATIC
OSLDFLAGS = -static -lglew32 -lglfw3 -lopengl32 -lgdi32 -luser32 -lkernel32
else
DEFS     = 
GLFLAGS  = `pkg-config --cflags glfw3` 
LGLFLAGS = `pkg-config --libs glew glfw3 gl`
ELDFLAGS = -export-dynamic -lXext -lX11 
endif

CXXFLAGS = $(WFLAGS) $(DFLAGS) $(GLFLAGS)

CXXFLAGS = $(DBFLAGS) $(DEFS) $(WFLAGS) $(IFLAGS) $(DFLAGS) $(GLFLAGS)
LDFLAGS  = $(ELDFLAGS) $(LGLFLAGS) $(OSLDFLAGS)


all: $(BUILD_DIR)/$(TARGET)

# Binary target, depends on all .o files
$(BUILD_DIR)/$(TARGET) : $(OBJS)
	mkdir -p $(@D)
	$(CXX) $(LFLAGS) $^ -o $@ $(LDFLAGS) 

# Object files, depends on all .cpp files
# Headers are included by the next line
-include $(DEP)
$(BUILD_DIR)/%.o : %.cpp
	mkdir -p $(@D)
	$(CXX) $(CXXFLAGS) -MMD -c $< $(INCPATH) -o $@

$(BUILD_DIR)/%.o : %.c
	mkdir -p $(@D)
	$(CXX) $(CXXFLAGS) -MMD -c $<

clean:
ifeq ($(OS), Windows_NT)
	del /Q /S *.o *.d
else
	rm -f $(OBJS) $(DEP) $(TARGET)
endif
