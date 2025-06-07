#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

OUT_FILE = c1m2.out

# Platform Override
PLATFORM = MSP432

# ARM specific details
LINKER_FILE = msp432p401r.lds
CPU         = cortex-m4
ARCH        = armv7e-m
SPECS       = nosys.specs

# Common compiler flags
CFLAGS = -Wall -Werror -g -O0 -std=c99 

# == Conditional defines ==
ifeq ($(PLATFORM), HOST)
	# HOST
	CC        = gcc
	SRC      := $(COMMON_SRC)
	OBJS     := $(COMMON_SRC:.c=.o)
	INCLUDES := $(COMMON_INC)
else
	# ARM
	CC         = arm-none-eabi-gcc
	LDFLAGS    = -T ../$(LINKER_FILE)
	ARCH_FLAGS = -mcpu=$(CPU) -march=$(ARCH) -specs=$(SPECS) -mthumb -mfloat-abi=hard -mfpu=fpv4-sp-d16
	SRC       := $(TARGET_SRC) $(COMMON_SRC)
	OBJS      := $(TARGET_SRC:.c=.o) $(COMMON_SRC:.c=.o)
	INCLUDES  := $(TARGET_INC) $(COMMON_INC)
endif

# Targets

.PHONY: build
build: $(OBJS)
	$(CC) $(ARCH_FLAGS) $(CFLAGS) $(LDFLAGS) $(INCLUDES) -D$(PLATFORM) -o $(OUT_FILE) $(SRC)


.PHONY: compile-all
compile-all: $(OBJS)


.PHONY: clean
clean: 
	rm -f $(OBJS) $(OUT_FILE)


# %.i: #prereqs
#   #command

# %.asm: #prereqs
#   #command


%.o: %.c
	$(CC) $(ARCH_FLAGS) $(CFLAGS) $(INCLUDES) -D$(PLATFORM) -c $< -o $@


