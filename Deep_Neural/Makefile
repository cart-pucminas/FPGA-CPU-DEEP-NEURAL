TARGET := rnnkdd

# Where is the Altera SDK for OpenCL software?
ifeq ($(wildcard $(ALTERAOCLSDKROOT)),)
$(error Set ALTERAOCLSDKROOT to the root directory of the Altera SDK for OpenCL software installation)
endif
ifeq ($(wildcard $(ALTERAOCLSDKROOT)/host/include/CL/opencl.h),)
$(error Set ALTERAOCLSDKROOT to the root directory of the Altera SDK for OpenCL software installation.)
endif

# Directories.
HOST_DIR := host
TARGET_DIR := bin
INC_DIRS := host/inc ../common/inc ../extlibs/inc 

# All include files.
INC_FILES := $(foreach D,$(INC_DIRS),$(wildcard $D/*.h))

# Source files.
SRCS := $(wildcard ../common/src/AOCLUtils/*.cpp) $(wildcard host/src/*.cpp)

# Libraries.
LIBS :=

# Compiler.
CXX := g++

# OpenCL compile and link flags.
AOCL_COMPILE_CONFIG := $(shell aocl compile-config)
#AOCL_LINK_CONFIG := $(shell aocl link-config)
AOCL_LINK_CONFIG := -L/home/guillaume/altera/16.0/hld/host/linux64/lib -Wl,--no-as-needed -lalteracl -lstdc++ -lelf

CXXFLAGS += -O2 -D__USE_XOPEN2K8 -fopenmp -O3
CXXFLAGS += -Wall -I$AALSDK/include -I/usr/local/include -DHAVE_CONFIG_H -DTESTB -g -L/opt/aalsdk/lib  -L/usr/local/lib 

# Make it all!
all : $(TARGET_DIR)/$(TARGET)

# Host executable target.
$(TARGET_DIR)/$(TARGET) : Makefile $(SRCS) $(INC_FILES)
	@[ -d $(TARGET_DIR) ] || mkdir $(TARGET_DIR)
	$(ECHO)$(CXX) $(CPPFLAGS) $(CXXFLAGS) -fPIC $(foreach D,$(INC_DIRS),-I$D) \
			$(AOCL_COMPILE_CONFIG) $(SRCS) $(AOCL_LINK_CONFIG) \
			$(foreach D,$(LIB_DIRS),-L$D) \
			$(foreach L,$(LIBS),-l$L) \
			-o $(TARGET_DIR)/$(TARGET) 

# TODO: improve this entry to be better
emukernel:
	aoc -march=emulator -g -v device/calcul.cl -o bin/calcul.aocx --board de10_standard_sharedonly

kernel: 
	aoc -g -v device/calcul.cl -o bin/calcul.aocx --board de10_standard_sharedonly


# Standard make targets
clean :
	$(ECHO)rm -f $(TARGET_DIR)/$(TARGET)

.PHONY : all clean

