# (C) Copyleft 2011 2012 2013 2014 2015 2016 2017
# Late Lee(latelee@latelee.com) from http://www.latelee.org
# 
# A simple Makefile for *ONE* project(c or/and cpp file) in *ONE* or *MORE* directory
#
# note: 
# you can put head file(s) in 'include' directory, so it looks 
# a little neat.
#
# usage: 
#        $ make
#        $ make V=1     # verbose ouput
#        $ make CROSS_COMPILE=arm-arago-linux-gnueabi-  # cross compile for ARM, etc.
#        $ make debug=y # debug
#        $ make SRC_DIR1=foo SRC_DIR2=bar SRC_DIR3=crc
#
# log
#       2013-05-14 sth about debug...
#       2016-02-29 sth for c/c++ multi diretory
#       2017-04-17 -s for .a/.so if no debug
#       2017-05-05 Add V for verbose ouput
###############################################################################

# !!!=== cross compile...
CROSS_COMPILE ?= /home/fangy/arm-linux-gcc-3.4.1/usr/local/arm/3.4.1/bin/arm-linux-

CC  = $(CROSS_COMPILE)gcc
CXX = $(CROSS_COMPILE)g++
AR  = $(CROSS_COMPILE)ar

ARFLAGS = -cr
RM     = -rm -rf
MAKE   = make

CFLAGS  = 
LDFLAGS = 
DEFS    =
LIBS    =

# !!!===
# target executable file or .a or .so
target = a.out

# !!!===
# compile flags
CFLAGS += -Wall -g

#****************************************************************************
# debug can be set to y to include debugging info, or n otherwise
debug  = y

#****************************************************************************

ifeq ($(debug), y)
    CFLAGS += -ggdb -rdynamic
else
    CFLAGS += -O2 -s
endif

# !!!===
DEFS    += -DJIMKENT

CFLAGS  += $(DEFS)
CXXFLAGS = $(CFLAGS)

LIBS    += -L/home/vk/libev/src/out/lib -lev

LDFLAGS += $(LIBS)

# !!!===
INC1 = ./
INC2 = ./inc
INC3 = 
INCDIRS := -I$(INC1) -I$(INC2) -I/home/vk/libev/src

# !!!===
CFLAGS += $(INCDIRS)
CXXFLAGS += 

# !!!===
LDFLAGS += -lpthread -lrt

DYNC_FLAGS += -fpic -shared

# !!!===
# source file(s), including c file(s) or cpp file(s)
# you can also use $(wildcard *.c), etc.
SRC_DIR = .
SRC_DIR1 = foo
SRC_DIR2 = bar
SRC_DIR3 = crc

# ok for c/c++
SRC = $(wildcard $(SRC_DIR)/*.c $(SRC_DIR)/*.cpp)
SRC+=$(wildcard $(SRC_DIR1)/*.c $(SRC_DIR1)/*.cpp)
SRC+=$(wildcard $(SRC_DIR2)/*.c $(SRC_DIR2)/*.cpp)
SRC+=$(wildcard $(SRC_DIR3)/*.c $(SRC_DIR3)/*.cpp)

# ok for c/c++
OBJ = $(patsubst %.c,%.o, $(patsubst %.cpp,%.o, $(SRC))) 


# !!!===
# in case all .c/.cpp need g++...
# CC = $(CXX)

ifeq ($(V),1)
Q=
NQ=true
else
Q=@
NQ=echo
endif

###############################################################################

all: $(target)

$(target): $(OBJ)

ifeq ($(suffix $(target)), .so)
	@$(NQ) "Generating dynamic lib file..." $(notdir $(target))
	$(Q)$(CXX) $(CXXFLAGS) $^ -o $(target) $(LDFLAGS) $(DYNC_FLAGS)
else ifeq ($(suffix $(target)), .a)
	@$(NQ) "Generating static lib file..." $(notdir $(target))
	$(Q)$(AR) $(ARFLAGS) -o $(target) $^
else
	@$(NQ) "Generating executable file..." $(notdir $(target))
	$(Q)$(CXX) $(CXXFLAGS) $^ -o $(target) $(LDFLAGS)
endif


objDir = obj
# make all .c or .cpp
#将c文件编译为o文件，并放在指定放置目标文件的目录中即 obj
$(OBJ):%.o:%.c
	@$(NQ) "Compiling: " $(addsuffix .c, $(basename $(notdir $@)))
	$(Q)$(CC) $(CFLAGS) -c $< -o $@

%.o: %.cpp
	@$(NQ) "Compiling: " $(addsuffix .cpp, $(basename $(notdir $@)))
	$(Q)$(CXX) $(CXXFLAGS) -c $< -o $@

clean:
	@$(NQ) "Cleaning..."
	$(Q)$(RM) $(target)
	@find . -iname '*.o' -o -iname '*.bak' -o -iname '*.d' | xargs rm -f
style:
	find ./ -name "*.h" -or -name "*.c" -or -name "*.cpp"|egrep -v 'xxx' \
	| xargs astyle -p --suffix=none --style=java

.PHONY: all clean